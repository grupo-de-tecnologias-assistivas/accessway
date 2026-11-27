# [NOME DA ORGANIZAÇÃO]

[![Status](https://img.shields.io/badge/Status-Em_Desenvolvimento-yellow.svg)]()
[![Curso](https://img.shields.io/badge/Curso-Bacharelado_em_TI_IMD_UFRN-blue.svg)]()
[![Ciclo](https://img.shields.io/badge/Ciclo-Sprints_de_15_dias-green.svg)]()

Grupo de pesquisa e desenvolvimento formado por professores e estudantes do **Bacharelado em Tecnologia da Informação (BTI)** do **Instituto Metrópole Digital (IMD/UFRN)**, dedicado à criação de tecnologias assistivas voltadas à autonomia, mobilidade e inclusão de pessoas com deficiência visual.

## Objetivo Geral

Pesquisar, projetar e validar soluções tecnológicas assistivas acessíveis que promovam independência no dia a dia de pessoas com deficiência visual, unindo formação prática em engenharia de software, redes, algoritmos, hardware e inteligência artificial.

## Projetos e Estágio Atual

### 1. Projeto 1: Navegação Indoor Assistiva ([NOME DO PROJETO 1])

Aplicativo de orientação em ambientes fechados por Wi-Fi, trilateração, grafos e áudio com suporte previsto a fones de condução óssea (nome provisório no repositório: `Sonaris`, citado na documentação inicial como `Accessway`).

- **Já produzido:** detecção de redes Wi-Fi, identificação dos pontos de acesso e logs de auditoria para validar quais pontos pertencem ao andar.
- **Em andamento:** migração para Kotlin Multiplatform (KMP), validação com ESP32, estratégia de posicionamento para 2 pontos de acesso (com separação de sinais de outros andares), estudo de terceiro ponto com simulação e definição da arquitetura do projeto.

### 2. Projeto 2: Leitura com a Própria Voz ([NOME DO PROJETO 2])

Aplicativo para pessoas com deficiência visual ouvirem textos lidos com a própria voz a partir de modelos pré-treinados.

- **Em andamento:** pesquisa inicial sobre clonagem de voz, verificação automática do resultado e opções simples de conversão de texto em fala (TTS).

## Estrutura dos Repositórios

1. **`.github`**: página principal da organização, guia de contribuição e templates acessíveis compartilhados.
2. **`estudos`** (Repositório Estudos): trilha de aprendizagem individual por fases (Fase 0, Fase 1 e Fase 2), atas de reunião e materiais de Journal Club.
3. **`[repo-projeto-1]`**: código do aplicativo de navegação indoor, firmware de validação e documentação técnica.
4. **`[repo-projeto-2]`**: experimentos e aplicação de leitura com voz clonada.

## Fluxo de Trabalho da Equipe

Trabalhamos em **sprints de 15 dias** conectadas ao painel central do **GitHub Projects** (visualização em tabela com `Fase`, `Sprint` e `Status`).

1. **Estudos individuais:** cada membro abre sua issue de fase no repositório `estudos` usando o template correspondente (Fase 0, Fase 1 ou Fase 2), marca o checklist e fecha ao concluir.
2. **Implementação dos projetos:** cada tarefa técnica vira uma issue com um responsável definido e entra no fluxo de código: criar branch a partir da `main`, fazer commits padronizados, abrir Pull Request ligado à issue e passar por revisão de pelo menos um colega antes do merge.
3. **Fechamento da sprint:** revisão das entregas na reunião quinzenal e apresentação opcional de materiais no Journal Club.

Consulte todos os padrões de branches, commits, issues, modelos de template e acessibilidade para leitores de tela no arquivo `CONTRIBUTING.md` desta organização.
