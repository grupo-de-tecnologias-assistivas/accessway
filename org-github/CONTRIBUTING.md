# Guia de Contribuição, Padrões e Fluxo de Trabalho

Este documento unifica o esquema de organização do GitHub definido pelo grupo com os padrões de **Sprints, Issues, Branches, Commits, Pull Requests, Nomenclatura e Acessibilidade**.

## 1. Organização dos Repositórios e Painel Central

A organização opera com quatro frentes de repositórios integradas a um painel central no **GitHub Projects** (visualização principal em **tabela**):

1. **`.github`**: página principal da organização, este guia e templates padrão herdados por todos os repositórios.
2. **`estudos`**: repositório para estudos individuais por fases (Fase 0, Fase 1, Fase 2), atas de reunião e Journal Club.
3. **Repositório do Projeto 1** (Navegação Indoor): código do aplicativo (KMP), validação em hardware (ESP32), simulação e documentação técnica.
4. **Repositório do Projeto 2** (Leitura com Própria Voz): experimentos de clonagem de voz, verificação e TTS.

### Configuração do GitHub Projects

- **Visualização principal:** Tabela (priorizada para navegação acessível com leitores de tela).
- **Campos:** `Fase` (`Fase 0` até `Fase 6`, acompanhando o roteiro completo do projeto), `Status` (`A fazer`, `Em andamento`, `Concluído`) e `Sprint` (ciclos de 15 dias).
- **Abas recomendadas:** `Onde cada pessoa está`, `Minhas tarefas` e `Implementação do app`.
- **Automações:** nova issue entra automaticamente no projeto; issue fechada muda o status para `Concluído`.

## 2. Acessibilidade Obrigatória em Textos e Issues

Como o grupo desenvolve soluções assistivas e utiliza leitores de tela, todo README, issue, template e documento deve seguir estas regras:

- **Proibido em textos e títulos:** não usar emojis, não usar setas decorativas no texto corrido e não usar ponto médio.
- **Títulos navegáveis pela tecla H:** manter sempre hierarquia clara de cabeçalhos em ordem fixa.
- **Pausas naturais na leitura:** usar vírgula e dois-pontos nos títulos das issues.
- **Imagens e fórmulas:** toda imagem precisa de texto alternativo descritivo; fórmulas matemáticas também devem ser descritas por extenso.

## 3. Padrão de Issues

### A) Issues de Estudo (repositório `estudos`)

Cada pessoa abre sua própria issue a partir do template da fase (inicialmente `Fase 0`, `Fase 1` e `Fase 2` para os notebooks de linguagem e base, podendo expandir até `Fase 6` conforme o roteiro técnico).

- **Formato do título:** `Fase 1, Mapeamento: SEU NOME`
- **Labels:** `estudos` e o número da fase (por exemplo: `fase-0` até `fase-6`)
- **Ordem obrigatória dos 5 títulos no corpo:**
  1. `Objetivo`: o que a fase ensina.
  2. `Material`: onde está o notebook ou referência.
  3. `Etapas`: checklist com uma caixa por seção, em uma única página.
  4. `Pronto quando`: critério para considerar a fase concluída.
  5. `Próximo passo`: fechar a issue e abrir a da fase seguinte.

### B) Issues de Implementação (repositórios dos projetos)

Issues compartilhadas pelo grupo, com **uma tarefa** e **uma pessoa responsável** (por exemplo: criar modelos de dados, carregar o `map.json`, implementar leitura RSSI).

- **Formato do título:** `Fase 1, Map json: NOME DO RESPONSAVEL` ou `[Modulo] Descricao curta da tarefa`
- **Labels:** `projeto-1` ou `projeto-2`, o tipo (`feat`, `fix`, `docs`, `research`, `refactor`) e a fase (`fase-0` até `fase-6`).
- **Estrutura do corpo:** usa os mesmos 5 títulos dos templates de estudo para manter consistência de navegação com a tecla H.

## 4. Fluxo de Código (GitHub Flow) e Sprints de 15 Dias

A branch `main` deve permanecer sempre estável. Todo trabalho de código ou documentação técnica é feito em uma branch própria criada a partir da `main`.

### Passo a passo de uma tarefa

1. Atualizar a `main` local (`git checkout main` e `git pull`).
2. Criar a branch da tarefa (`git checkout -b tipo/assunto-curto`).
3. Desenvolver a tarefa e registrar commits padronizados.
4. Enviar a branch (`git push -u origin tipo/assunto-curto`) e abrir um Pull Request ligando à issue (ex.: `Closes #12`).
5. Aguardar revisão e aprovação de pelo menos um colega antes de fazer merge na `main` e apagar a branch antiga.

## 5. Padrão de Nomes de Branches

Formato obrigatório: `<tipo>/<descricao-curta>` (apenas letras minúsculas, sem acentos, palavras separadas por hífen).

- `feat/...`: nova funcionalidade (ex.: `feat/esp32-scan-wifi`)
- `fix/...`: correção de erro (ex.: `fix/filtro-ap-andar`)
- `docs/...`: documentação e atas (ex.: `docs/fluxo-organizacao`)
- `research/...`: estudos, experimentos e provas de conceito (ex.: `research/voice-clone-xtts`)
- `refactor/...`: reorganização ou migração de código (ex.: `refactor/migrar-kmp`)
- `chore/...`: configurações, templates e organização de repositório (ex.: `chore/template-pr`)

## 6. Padrão de Mensagens de Commit

Formato adotado (Conventional Commits em português):

```text
<tipo>(<escopo-opcional>): <descricao curta no infinitivo>
```

Exemplos recomendados:

```text
feat(esp32): adicionar captura de BSSID e RSSI em JSON
fix(auditoria): filtrar pontos de acesso externos ao andar
docs(org): publicar fluxo de trabalho na pagina principal
research(voz): testar modelos leves de TTS em portugues
refactor(kmp): organizar camada compartilhada de posicionamento
chore(repo): adicionar templates acessiveis de fase
```

Evite mensagens genéricas como `update`, `ajustes` ou `commit final`, e evite misturar idiomas sem necessidade.

## 7. Padrão de Nomes de Repositórios, Pastas e Arquivos

- **Pastas e arquivos:** usar letras minúsculas e hífen ou sublinhado, evitando espaços e acentos (ex.: `ata-reuniao-01-2026-11-13.md`, `sonaris-logo-compact.jpg`).
- **Segredos e dados sensíveis:** nunca commitar senhas de Wi-Fi, tokens de API ou chaves pessoais. Usar arquivos de exemplo como `config.example.h` ou `.env.example`.
