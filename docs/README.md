# **Accessway — Roteiro de Estudos e Documentação**

> **Nota de atualização (Ata de 13/11/2026):** Este roteiro foi migrado de `Estudos/accessway/docs` para o repositório público `accessway` e está em processo de **validação** e **adaptação para Kotlin Multiplatform (KMP)**, além de incorporar validações com **ESP32** e estudo de viabilidade de um **terceiro ponto de acesso (NVIDIA Sionna)**.

A ideia central deste roteiro de estudos é conduzir uma base de aprendizagem-execução para cada fase do projeto **Accessway**.

---

### **Fase 0:** Fundamentos de Kotlin e Desenvolvimento Android Nativo

*Antes de iniciar qualquer mapeamento, precisamos dominar a linguagem base e o ambiente de desenvolvimento.*

1. **Sintaxe do Kotlin:** Variáveis (`val` e `var`), tipos de dados, controle de fluxo (`if/else`, `when`), funções e expressões lambda.
2. **Segurança de Nulos (*Null Safety*):** Entender o sistema de tipos do Kotlin (`Type?`, `?.`, `?:`, `!!`) para evitar travamentos da aplicação.
3. **Programação Orientada a Objetos (POO) em Kotlin:** Classes, `data class` (essencial para modelar dados), herança, interfaces e enums.
4. **Ambiente Android Studio:** Estrutura de pastas de um projeto Android, arquivo `AndroidManifest.xml`, `build.gradle` e o ciclo de vida de uma *Activity*.

> **Exercício de Aquecimento:** 
> Criar um app simples com uma única tela contendo um botão e um campo de texto: ao clicar no botão, o app lê um nome digitado e exibe uma mensagem de boas-vindas na tela.

---

### **Fase 1:** Mapeamento do Ambiente e Modelagem de Dados

Antes de escrever o código do aplicativo, é preciso digitalizar o espaço físico onde o testes serão realizados.

1. **Planta Digital e Grade de Coordenadas:** Converter a planta baixa do prédio em um sistema de coordenadas cartesiano bidimensional ($X, Y$).
2. **Mapeamento dos Pontos de Acesso (APs):** Identificar a localização exata de cada roteador Wi-Fi do prédio e registrar seus respectivos endereços MAC (BSSID) e posições ($X, Y$).
3. **Criação do Grafo de Navegação:** Traduzir os caminhos navegáveis (corredores, portas, halls) em um grafo composto por **nós** (pontos de decisão) e **arestas** (trechos do corredor com distâncias conhecidas).

---

### **Fase 2:** Motor de Posicionamento (*Indoor Positioning System - IPS*)

Fase focada em ler o ambiente físico por meio dos sensores do celular e converter essas leituras na posição do usuário.

1. **Serviço de Varredura Wi-Fi:** Criar um *Foreground Service* em Kotlin utilizando a API `WifiManager` para escutar continuamente as redes Wi-Fi e extrair os valores de RSSI em tempo real.
2. **Tratamento e Filtragem do Sinal:** Aplicar um algoritmo de filtragem (como Média Móvel ou Filtro de Kalman) para eliminar o "ruído" e as oscilações naturais do sinal Wi-Fi.
3. **Cálculo de Trilateração:** Implementar a lógica matemática que estima a distância do celular até pelo menos 3 APs com base no sinal RSSI e calcula o ponto $(X, Y)$ interseção.
4. **Integração com Odometria Pediátrica (PDR):** Usar o `SensorManager` para capturar acelerômetro e giroscópio, contando os passos e a orientação da bússola para atualizar a posição entre os ciclos de varredura do Wi-Fi.

---

### **Fase 3:** Algoritmos de Roteamento e Busca

Fase onde o aplicativo aprende a traçar caminhos do ponto onde a pessoa está até onde ela deseja ir.

1. **Implementação do Algoritmo de Rota:** Escrever a lógica de navegação sobre o grafo estruturado na Fase 1 utilizando algoritmos de menor caminho como **A* (A-Star)** ou **Dijkstra**.
2. **Mapeamento Rota-Instrução:** Criar a camada que converte o vetor da rota em instruções em linguagem natural (ex.: se o próximo nó exige uma mudança de vetor de 90° à esquerda, a instrução vira "Em 5 metros, vire à esquerda").

---

### **Fase 4:** Interface de Voz e Acessibilidade (UX Assistiva)

Desenvolvimento da camada que interage diretamente com o usuário com deficiência visual.

1. **Módulo de Síntese de Voz (TTS):** Configurar o componente `TextToSpeech` nativo do Android para converter as instruções do motor de roteamento em comandos em áudio claros.
2. **Lógica de Orientação Relativa:** Mapear a orientação da bússola do celular com a direção do usuário para emitir comandos baseados no seu corpo ("às suas 3 horas", "siga em frente por 8 passos").
3. **Interface Visual Minimalista:** Desenvolver telas acessíveis usando as diretrizes do **TalkBack** do Android (alto contraste, áreas de toque grandes e leituras de tela por gestos simples).

---

### **Fase 5:** Arquitetura de Software e Armazenamento

Organização do código-fonte e garantia de que tudo roda sem travamentos.

1. **Banco de Dados Local:** Implementar a biblioteca **Room** para salvar o mapa do prédio, coordenadas dos APs e pontos de interesse (POIs) diretamente no aparelho, garantindo operação offline.
2. **Gestão de Tarefas Assíncronas:** Estruturar o fluxo de dados usando **Kotlin Coroutines e Flow** para manter a leitura de sensores, varredura Wi-Fi, cálculos matemáticos e geração de áudio rodando simultaneamente sem travar a interface.

---

### **Fase 6:** Testes, Calibração e Validação em Campo

Ajuste fino da solução no ambiente real antes da apresentação final.

1. **Calibração de Sinal:** Realizar testes em campo no prédio escolhido para ajustar as variáveis da equação do sinal Wi-Fi (medir a atenuação de paredes reais).
2. **Medição da Margem de Erro:** Avaliar a precisão da trilateração em metros (ex.: determinar se o erro médio é de 2m, 3m ou 5m) e implementar margens de segurança.
3. **Testes de Usabilidade Assistiva:** Simular a navegação de olhos vendados ou com apoio de pessoas com baixa visão para ajustar a frequência do áudio e a clareza das instruções.
