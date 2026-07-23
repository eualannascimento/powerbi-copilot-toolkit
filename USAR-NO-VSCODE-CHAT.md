# Aprofundamento do Modo Chat (extensão GitHub Copilot Chat, sem Copilot CLI)

`LEIA-PRIMEIRO.md` já cobre o setup básico dos dois modos (seções 3–5). Este documento
aprofunda só o que é específico do **Modo Chat**: por que ele funciona diferente do Modo
CLI por baixo dos panos, como montar a referência local completa das skills, e o que
fica manual em comparação ao Modo CLI.

A extensão **GitHub Copilot Chat** (a que você abre pelo ícone de chat do VS Code) **não
lê o formato de plugin** (`SKILL.md`) que o Copilot CLI instala, e **não roteia skills
automaticamente pela descrição** — ela tem seu próprio mecanismo, mais manual, baseado em
*custom agents*.

## O que muda

| Mecanismo do Copilot CLI | Equivalente na extensão do VS Code |
|---|---|
| `/plugin install ...` (skills roteadas automaticamente pela descrição) | **Custom agents** (`.github/agents/*.agent.md`) — você escolhe manualmente no dropdown do chat, ou o prompt file escolhe por você |
| Skills chamam MCP servers já configurados pelo pacote | **Você mesmo registra** os MCP servers em `.vscode/mcp.json` |
| Roteamento automático entre skills relacionadas (ex. planning → design → authoring) | Cada custom agent te diz, no próprio texto, quando trocar de agente (`handoffs`) — mas a troca ainda é manual |
| Auto-update dos pacotes de skill | Você atualiza puxando (`git pull`) o clone de referência |

Resumindo: você perde o roteamento automático "a IA decide sozinha qual skill usar", mas
ganha simplicidade — não depende do Copilot CLI nem do PowerShell 7, só da extensão.

## Passo 1 — Clonar as skills originais como referência local

Os `.agent.md` que vou te dar abaixo já têm o essencial de cada skill embutido, mas
apontam também para os arquivos de referência completos (exemplos JSON, catálogos de
tema, checklists) que só existem nos repositórios originais. Clone os dois como
**submódulo git** dentro de cada repositório de domínio Power BI (assim todo
desenvolvedor do time recebe a mesma referência ao clonar o repo, e você controla a
versão/pin com `git submodule update`):

```bash
cd powerbi-<dominio>          # raiz do seu repositório real de Power BI
git submodule add https://github.com/microsoft/skills-for-fabric .reference/skills-for-fabric
git submodule add https://github.com/data-goblin/power-bi-agentic-development .reference/power-bi-agentic-development
git add .gitmodules .reference
git commit -m "Adiciona skills de Power BI como referência local"
```

Quem clonar o repositório depois roda `git submodule update --init` uma vez.

> Se preferir não usar submódulo (ex. repositório pessoal, sem outros devs), um
> `git clone` simples em `.reference/` e adicionar a pasta ao `.gitignore` funciona igual
> para uso individual — só não fica versionado/compartilhado pelo repo.

## Passo 2 — Registrar os MCP servers (o pulo do gato)

Já resumido em `LEIA-PRIMEIRO.md` seção 4. Complemento: se sua organização também expõe
um MCP de modelagem (ex. `powerbi-modeling-mcp`) ou um MCP do Tabular Editor, adicione da
mesma forma no `.vscode/mcp.json` — pergunte ao time de plataforma de dados se algum já
existe antes de tentar subir um novo. O MCP é o que dá **acesso vivo** aos dados do
Fabric sem depender de nenhum agente — útil inclusive dentro de um agente custom, que
pode chamar as ferramentas do MCP registrado.

## Passo 3 — Os custom agents (equivalente às 5 skills principais)

Já criados em `.github/agents/`, um por responsabilidade — mesma divisão de
`GUIA-FLUXO-COMPLETO.md`:

| Agente | Quando escolher no dropdown do chat |
|---|---|
| `powerbi-planejamento` | Começar um relatório novo ou redesign amplo |
| `powerbi-design` | Decidir tom, arquétipo, paleta, tipografia — antes de mexer em arquivo |
| `powerbi-modelo-semantico` | Medidas, colunas, relacionamentos, RLS |
| `powerbi-autoria` | Editar PBIR: páginas, visuais, filtros, tema |
| `powerbi-gestao` | Publicar/baixar/listar relatórios no Fabric |

Para usar: abra o chat, clique no seletor de modo/agente (canto do painel de chat) e
escolha o agente. Ele carrega as instruções específicas daquela responsabilidade.

## Passo 4 — Prompt files já escolhem o agente certo por você

Os prompt files de `MENU-ATIVIDADES-PONTUAIS.md` (`.github/prompts/*.prompt.md`) foram
atualizados: cada um tem `mode:` apontando para o agente certo. Ao rodar `/dax-criar-corrigir`,
por exemplo, o VS Code já entra automaticamente no agente `powerbi-modelo-semantico` — você
não precisa trocar de agente manualmente antes.

## Passo 5 — `.github/copilot-instructions.md` continua valendo

A extensão do VS Code lê `.github/copilot-instructions.md` automaticamente em toda
conversa (não precisa selecionar nada) — as regras gerais do kit (nunca editar PBIR à
mão, sempre confirmar workspace antes de publicar, padrão de RLS via SharePoint) continuam
ativas sem mudança nenhuma.

## O que fica manual de verdade nesse modo (sem CLI)

- **Trocar de agente** quando o trabalho muda de fase (ex. planejamento → autoria) — o
  Copilot CLI faria essa transição sozinho lendo a descrição da próxima skill; aqui você
  lê a sugestão de `handoffs` no fim da resposta do agente e troca manualmente.
- **Atualizar a referência**: `git submodule update --remote` de vez em quando, não tem
  aviso automático de versão nova como o `check-updates` do Copilot CLI.
- **Validação de PBIR** (`powerbi-report-author validate`) e o CLI de Desktop
  (`powerbi-desktop`) continuam sendo ferramentas de linha de comando — a extensão de
  chat pode rodá-las pelo terminal integrado (o agente `powerbi-autoria` já instrui isso),
  mas exige Node.js instalado, igual ao caminho com CLI.
