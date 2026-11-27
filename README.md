# Accessway (Projeto 1 — Navegação Indoor Acessível)

[![Status](https://img.shields.io/badge/Status-Em_Desenvolvimento-yellow.svg)](#estado-atual)
[![GTA](https://img.shields.io/badge/Org-Grupo_de_Tecnologias_Assistivas-blue.svg)](https://github.com/grupo-de-tecnologias-assistivas)

> Aplicativo assistivo de **navegação indoor** para pessoas com cegueira ou baixa visão, utilizando pontos de acesso Wi-Fi (RSSI), mapeamento em grafo, trilateração e interface auditiva em tempo real.

*(O nome definitivo deste projeto também está em votação na enquete do grupo.)*

---

## O Que Já Foi Produzido

- Detecção de redes Wi-Fi.
- Identificação dos pontos de acesso (APs).
- Logs de acesso usados como auditoria para validar quais pontos de acesso pertencem ao andar.

## Próximos Passos e Direções Técnicas

- **Migração para Kotlin Multiplatform (KMP):** adaptação da base e dos estudos iniciais (antes focados apenas em Android nativo).
- **Hardware e Pontos de Acesso:**
  - Validação da solução com microcontrolador **ESP32** (Wi-Fi integrado).
  - Estudo de viabilidade de um terceiro ponto de acesso (simulação com referência ao [NVIDIA Sionna](https://developer.nvidia.com/sionna)).
  - Algoritmo futuro para selecionar os 3 melhores APs para maior resolução de posição.
- **Funcionalidades Previstas:**
  - Indicar onde a pessoa está e guiá-la até onde deseja ir.
  - Suporte a **fones de condução óssea**.

## Estudos e Documentação

- [`docs/`](docs/README.md): roteiro de estudos por fases (`Fase 0.ipynb` a `Fase 6.ipynb`) migrado de `Estudos/accessway/docs` e em processo de validação/adaptação para KMP.
- [`estudos/`](estudos/README.md): espaço público para novos estudos, apresentações (como trilateração), simulações e materiais de **Journal Club** do Projeto 1.

## Como Contribuir

Este repositório segue o fluxo oficial da organização (`main` + `develop` + `feature/...`, Conventional Commits e Pull Request com pelo menos 1 revisor aprovado). Consulte o [CONTRIBUTING.md da organização](https://github.com/grupo-de-tecnologias-assistivas/.github/blob/main/CONTRIBUTING.md).
