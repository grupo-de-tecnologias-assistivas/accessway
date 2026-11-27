# Estratégia de Mapeamento com 2 Pontos de Acesso, Sinal de Outros Andares e ESP32

Documento direto para discussão na reunião sobre o cenário real do prédio: **apenas 2 pontos de acesso (APs) no andar**, **interferência de roteadores de andares acima ou abaixo** e **papel do ESP32**.

---

## 1. O Problema Real no Prédio

1. **Apenas 2 APs no mesmo andar:** a trilateração geométrica pura em 2D exige pelo menos **3 pontos de referência** no mesmo plano para fechar uma única posição `(X, Y)`. Com apenas 2 APs, duas circunferências se cruzam em **2 pontos possíveis** (ambiguidade de lado).
2. **Captura acidental de APs de outros andares:** o celular ou o ESP32 frequentemente enxerga roteadores do andar de cima ou de baixo.
   - **Por que isso quebra a trilateração:** a laje e o concreto atenuam muito mais o sinal do que o ar no mesmo corredor. Se o algoritmo tratar um AP do andar de cima como se estivesse no mesmo andar, ele calcula uma distância errada e desloca a posição do usuário (ou confunde o andar atual).

---

## 2. Como Evitar que o Sinal de Outro Andar Atrapalhe

A auditoria de pontos de acesso (que o grupo já iniciou) resolve isso com três regras diretas no código e no `map.json`:

1. **Classificação explícita por `BSSID` (endereço MAC):**
   - **Lista principal do andar (`floor_aps`):** apenas os 2 `BSSIDs` confirmados fisicamente naquele pavimento entram no cálculo principal de distância ou posição.
   - **Lista auxiliar de outros andares (`other_floor_aps`):** `BSSIDs` conhecidos de andares acima ou abaixo marcados com seu respectivo andar (`floor: 2`, `floor: 0`), para **nunca** entrarem na equação 2D do andar atual.
   - **Redes descartadas (`ignored`):** hotspots de celulares e redes externas sem vínculo com o prédio.
2. **Filtro de estabilidade por limiar de `RSSI` e presença:**
   - Sinais de outro andar tendem a chegar mais fracos e instáveis atrás da laje. Se o `RSSI` de um AP desconhecido for muito baixo ou intermitente, ele é descartado antes do cálculo.
3. **Uso positivo do AP de cima apenas como detector de andar:**
   - Em vez de misturar o sinal de cima na conta de `(X, Y)`, compare a média dos APs do andar 1 contra a média dos APs do andar 2: se os de cima ficarem consistentemente mais fortes do que os do andar atual, o sistema identifica que o usuário subiu de andar (por exemplo na escada ou elevador).

---

## 3. Como Resolver Usando Apenas os 2 APs do Andar (Sem Comprar 3º Roteador)

Existem duas estratégias práticas que funcionam mesmo com só **2 APs no andar** e ainda aproveitam melhor o cenário real do prédio:

### Estratégia A: Restrição pelo Grafo do Corredor + Odometria de Pedestre (PDR)

Se você mantém cálculo baseado em distância (`RSSI`) usando apenas os **2 APs do andar**:
- As duas distâncias estimadas geram **2 pontos candidatos** (um de cada lado da linha entre os 2 roteadores).
- Na prática de um prédio, o próprio **mapa/grafo** elimina o ponto falso: um dos pontos costuma cair fora da área navegável (atrás da parede ou fora do corredor).
- Se ambos caírem em corredores diferentes, o **histórico da rota (nó anterior no grafo)** mais a direção e passos da **Odometria de Pedestre (PDR)** indicam de qual lado a pessoa veio.
- **Papel do AP do andar de cima aqui:** fica **bloqueado** do cálculo geométrico 2D (usado só para detectar troca de andar).

### Estratégia B: Wi-Fi Fingerprinting (Rádio-mapa) — Mais Fácil e Preciso em Corredores

Em vez de converter `RSSI` em distância geométrica em metros (que sofre muito com paredes e exige 3+ APs no plano):
- **Como montar:** você caminha pelos nós do grafo no corredor (por exemplo a cada 2 ou 3 metros) e registra no `map.json` o vetor médio de `RSSI` observado em cada ponto.
- **Como usar os 2 APs + o ponto de cima:**
  - Os **2 APs do andar** dão a base principal ao longo do corredor.
  - O **AP do andar de cima** (se tiver sinal estável perto de escadas ou trechos específicos) pode ser gravado como **característica extra da assinatura daquele ponto** sem precisar saber a posição geométrica real dele nem calcular distância através da laje.
  - Se o sinal de cima estiver muito instável ou ausente em certos dias, basta deixá-lo fora da assinatura e usar apenas os 2 APs do andar junto com a restrição sequencial do grafo (só aceitar nós vizinhos ao ponto anterior).
- **Por que funciona melhor:** absorve naturalmente paredes, portas e móveis sem depender de fórmula ideal de propagação no ar.

---

## 4. Qual é o Melhor Papel do ESP32 Nesse Cenário?

| Uso do ESP32 | Quando vale a pena | Limitação principal |
| --- | --- | --- |
| **1. Auditoria e mapeamento em campo (`WIFI_STA`)** | **Sempre útil:** levantar todos os `BSSIDs` do corredor, identificar quais são do andar e quais vêm de cima/baixo, e montar as listas `floor_aps` e `other_floor_aps` antes de codar no app. | ESP32 comum lê apenas **2.4 GHz** (não enxerga roteadores que operam só em 5 GHz). |
| **2. Terceiro ponto Wi-Fi (`SoftAP` 2.4 GHz)** | **Bom para testes rápidos / PoC** quando quiser simular 3 APs na bancada ou validar a matemática da trilateração sem mexer na rede do prédio. | Antena da placa é pequena, sinal varia mais e o Android limita apps a **4 scans Wi-Fi a cada 2 minutos** (*scan throttling*). |
| **3. Beacon BLE (Bluetooth Low Energy)** | **Excelente apoio definitivo barato:** colocar um ESP32 como *iBeacon* em pontos críticos (escada, entrada, bifurcação) para zerar dúvidas entre nós próximos. | Exige alimentação (tomada ou power bank) e leitura BLE no app; não sofre o limite de 4 scans/2 min do Wi-Fi no Android. |
| **4. Escuta passiva do celular pelo ESP32** | **Não recomendado.** | Celulares randomizam o endereço MAC e enviam poucos pacotes contínuos, além de precisar devolver o resultado ao celular via rede. |

---

## 5. Resumo Prático para Decisão na Reunião

1. **Imediato (com os 2 APs atuais):** usar a auditoria para **separar `BSSIDs` do andar vs. `BSSIDs` de andares acima/abaixo**, impedindo que o sinal de cima entre em cálculos de distância 2D.
2. **Para resolver sem 3º roteador:** combinar **apenas os 2 APs do andar** com **restrição no grafo + PDR** (ou adotar **Wi-Fi Fingerprinting** nos nós do corredor). O AP de cima fica apenas como **referência de troca de andar** (ou pista auxiliar controlada no rádio-mapa).
3. **Para o ESP32:** usar primeiro para **auditoria de `BSSIDs` e calibração**; se precisarem de um 3º ponto físico no prédio definitivo para trilateração pura, um **roteador simples em modo Access Point** é mais estável que o ESP32 em `SoftAP`, enquanto o ESP32 brilha se for usado como **Beacon BLE** em pontos estratégicos.
