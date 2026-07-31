# powerbi-copilot-toolkit

Kit de referência e templates para trabalhar com **Power BI/Microsoft Fabric + GitHub
Copilot** (CLI ou extensão de chat do VS Code), construído em cima de
[`microsoft/skills-for-fabric`](https://github.com/microsoft/skills-for-fabric) e
[`data-goblin/power-bi-agentic-development`](https://github.com/data-goblin/power-bi-agentic-development).

Não reimplementa essas skills — organiza **quando usar cada uma** (fluxo completo de
criação de relatório e correções pontuais) e cobre o que nenhuma das duas resolve: RLS
via lista SharePoint, solicitação de acesso, handoff para IA de design visual,
documentação em dois formatos (dev + negócio).

## Comece aqui

👉 **[`LEIA-PRIMEIRO.md`](LEIA-PRIMEIRO.md)** — pré-requisitos, instalação e onde colocar
os projetos `.pbip`. Explica os dois modos de uso (GitHub Copilot CLI vs. extensão de
chat do VS Code) lado a lado.

## Mapa do repositório

| Arquivo/pasta | O que é |
|---|---|
| `LEIA-PRIMEIRO.md` | Setup, pré-requisitos, estrutura de pastas recomendada |
| `GUIA-FLUXO-COMPLETO.md` | Passo a passo ordenado para criar/recriar um relatório do zero |
| `MENU-ATIVIDADES-PONTUAIS.md` | Cardápio de correções pontuais (DAX, páginas, RLS, auditorias) |
| `USAR-NO-VSCODE-CHAT.md` | Aprofundamento do uso só com a extensão de chat (sem Copilot CLI) |
| `.github/agents/` | Custom agents do VS Code Copilot Chat, um por responsabilidade |
| `.github/prompts/` | Prompt files (`/nome`) para as tarefas pontuais |
| `.github/copilot-instructions.md` | Instruções gerais lidas automaticamente pelo Copilot |
| `.vscode/mcp.json` | Registro do MCP server FabricIQ (acesso vivo ao Fabric no chat) |
| `templates/` | Modelos de inventário de fontes, solicitação de acesso, RLS via SharePoint, documentação dev/negócio |

## Como usar num projeto Power BI real

Este repositório é um **modelo/starter kit** — copie `.github/`, `.vscode/` e
`templates/` para dentro do repositório real de cada domínio de Power BI. Detalhes em
`LEIA-PRIMEIRO.md`, seção "Copiar o kit para o seu projeto real".

## Validação e compatibilidade

Consulte `docs/COMPATIBILITY.md`, `docs/PBIX-VALIDATION.md` e `examples/pbip-starter/` antes de usar o toolkit em um projeto real.