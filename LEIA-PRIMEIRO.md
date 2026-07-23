# Power BI + Copilot — Como usar este kit

Este kit organiza um fluxo de trabalho de Power BI em cima de duas fontes
oficiais/comunitárias de skills, para você gastar o mínimo de tokens/tempo
"reexplicando" o processo a cada conversa:

- **`microsoft/skills-for-fabric`** (oficial Microsoft) — skills de planejamento, design,
  autoria (PBIR/PBIP) e gestão (publicação) de relatórios Power BI, além de skills de
  modelagem semântica e de todos os workloads do Fabric (Warehouse, Lakehouse, Spark,
  Dataflows, Eventhouse etc.).
- **`data-goblin/power-bi-agentic-development`** (comunidade) — plugins complementares:
  Tabular Editor (incl. Best Practice Analyzer), PBIP CLI, admin do Fabric, visuais
  customizados, relatórios paginados.

Este kit **não reimplementa** essas skills — ele te dá (1) o passo a passo ordenado de
quando usar cada uma, (2) os pedaços que elas explicitamente não cobrem (RLS via lista
SharePoint, solicitação de acesso, handoff para a IA de design visual, documentação em
dois formatos) e (3) atalhos prontos para as tarefas pontuais do dia a dia.

## Dois jeitos de rodar tudo isto — e por quê existem os dois

Existem **dois mecanismos diferentes** dentro do VS Code, e todos os documentos deste
kit (`GUIA-FLUXO-COMPLETO.md`, `MENU-ATIVIDADES-PONTUAIS.md`) marcam, em cada passo, o
que fazer nos dois. Não são concorrentes — dá para usar os dois no mesmo repositório,
cada um quando fizer mais sentido:

| | **Modo CLI** (GitHub Copilot CLI) | **Modo Chat** (extensão GitHub Copilot Chat) |
|---|---|---|
| O que é | `copilot`, rodado no terminal (inclusive o terminal integrado do VS Code) | O painel de chat do VS Code, ícone de chat, modo *Agent* |
| Como sabe qual skill usar | **Sozinho** — lê a descrição de cada `SKILL.md` instalada e escolhe pela sua frase | **Você escolhe** — seleciona um *custom agent* (`.github/agents/*.agent.md`) no dropdown do chat, ou um prompt file já escolhe por você |
| Como instala as skills | `/plugin install ...` a partir do marketplace oficial (repos acima) | Não instala nada — os agentes deste kit já têm o essencial de cada skill embutido no próprio arquivo |
| Melhor para | Fluxos longos/orquestrados (fase 4–7 do `GUIA-FLUXO-COMPLETO.md`: planejar → construir um relatório inteiro, com transição automática entre skills) | Edições pontuais, revisão de código, quem não quer instalar/manter o Copilot CLI |
| Setup | Seção 1–2 abaixo | Seção 3 abaixo |

Se você só vai fazer correções pontuais (`MENU-ATIVIDADES-PONTUAIS.md`), o **Modo Chat
sozinho já é suficiente** — pule a instalação do Copilot CLI. Se você vai construir
relatórios do zero com frequência, vale instalar os dois: comece pelo Modo CLI para o
fluxo completo e use o Modo Chat para ajustes rápidos no meio do caminho.

---

## Modo CLI — setup

### 1. Pré-requisitos

- **GitHub Copilot CLI** (`npm install -g @github/copilot`) — roda no terminal, inclusive
  o terminal integrado do VS Code, então na prática você continua "no VS Code".
- **PowerShell 7** (exigido pelo Copilot CLI) — `pwsh --version`.
- **Power BI Desktop**, **Azure CLI** (`az login`), **Node.js 20+** (exigido pelas CLIs de
  autoria de relatório PBIR) — valem para os dois modos, ver seção 3 também.

### 2. Instalação das skills (uma vez por máquina)

No terminal:

```bash
copilot
```

Dentro da sessão do Copilot CLI:

```
/plugin marketplace add microsoft/skills-for-fabric
/plugin install powerbi-authoring@fabric-collection
/plugin install fabric-consumption@fabric-collection

/plugin marketplace add data-goblin/power-bi-agentic-development
/plugin install pbip@power-bi-agentic-development
/plugin install semantic-models@power-bi-agentic-development
/plugin install tabular-editor@power-bi-agentic-development
/plugin install fabric-admin@power-bi-agentic-development
/plugin install pbi-desktop@power-bi-agentic-development

/quit
```

Reabra o Copilot CLI e confirme com `/skills`.

Atualizar depois: `/plugin update fabric-skills@fabric-collection` (repita para os
outros pacotes) ou `copilot plugin update --all`.

Uso: dentro do `copilot`, descreva o que quer em português — ele escolhe a skill certa
sozinho (ex.: "quero criar um relatório novo a partir do modelo X" aciona
`powerbi-report-planning`). Para forçar uma skill específica:
`/powerbi-authoring:powerbi-report-authoring`.

---

## Modo Chat — setup

### 3. Pré-requisitos

- **VS Code** com a extensão **GitHub Copilot Chat**, modo *Agent*.
- **Power BI Desktop**, **Azure CLI** (`az login`), **Node.js 20+** — os comandos de
  validação de PBIR e reload do Desktop ainda são CLIs de linha de comando (`powerbi-report-author`,
  `powerbi-desktop`), que o agente `powerbi-autoria` roda pelo terminal integrado mesmo
  neste modo.

### 4. Registrar o MCP server (dá acesso vivo ao Fabric, sem depender de skill nenhuma)

Copie `.vscode/mcp.json` (já incluído neste kit) para o seu repositório real. Ele registra
o **FabricIQ**, MCP remoto da Microsoft para Power BI: descoberta de artefatos, schema de
modelo semântico, geração/execução de DAX. No VS Code: **Command Palette → MCP: List
Servers** para conferir, autentique quando pedido (usa o `az login` acima).

### 5. Custom agents (equivalente às 5 skills principais)

Já incluídos em `.github/agents/`. Selecione no dropdown do chat, ou deixe um prompt file
escolher por você (seção 7). Detalhe completo, inclusive como clonar as skills originais
como referência local (`git submodule`) para os agentes citarem: `USAR-NO-VSCODE-CHAT.md`.

---

## 6. Onde colocar os projetos Power BI (.pbip) — vale para os dois modos

Estrutura recomendada — um repositório por **domínio/área de negócio** (ex.: um repo
`powerbi-financeiro`, outro `powerbi-comercial`), não um repo gigante com tudo:

```
powerbi-<dominio>/                        ← raiz do repositório, abra o VS Code aqui
├── .github/
│   ├── copilot-instructions.md           ← copiado deste kit (lido automaticamente nos dois modos)
│   ├── agents/                           ← copiado deste kit (só usado no Modo Chat)
│   └── prompts/                          ← copiado deste kit (funciona nos dois modos)
├── .vscode/
│   └── mcp.json                          ← copiado deste kit (só usado no Modo Chat)
├── .reference/                           ← opcional, só Modo Chat — submódulo com as skills originais (ver USAR-NO-VSCODE-CHAT.md)
├── _brief/                               ← gerado automaticamente pela skill/agente de planejamento
│   └── report-spec.md                    ← 1 arquivo por relatório em construção (sobrescreve entre relatórios; renomeie se quiser manter histórico)
├── docs/
│   ├── <NomeRelatorio>/
│   │   ├── doc-desenvolvedor.md
│   │   └── doc-analista-negocio.md
├── fontes-de-dados/
│   └── inventario-fontes-dados.md        ← 1 por relatório/projeto, ver templates/
├── reports/
│   ├── <NomeRelatorio>/
│   │   ├── <NomeRelatorio>.pbip          ← Power BI Desktop abre ESTE arquivo
│   │   ├── <NomeRelatorio>.Report/       ← PBIR (páginas, visuais, tema)
│   │   └── <NomeRelatorio>.SemanticModel/ ← TMDL (tabelas, medidas, relacionamentos, RLS)
│   └── <OutroRelatorio>/
│       └── ...
└── docs-fontes-externas/                 ← notas manuais sobre SAS/Teradata/Databricks/SharePoint (ver GUIA-FLUXO-COMPLETO.md, fase 1)
```

Regras práticas (independem do modo):

- **Abra o VS Code na raiz do repositório** (`powerbi-<dominio>/`), não dentro de
  `reports/<NomeRelatorio>/` — assim `.github/copilot-instructions.md`, os agentes e os
  prompt files valem para todos os relatórios do domínio.
- **Abra o Power BI Desktop diretamente no arquivo `.pbip`** dentro de `reports/<NomeRelatorio>/`.
  Nunca edite manualmente os `.json` do `.Report/` fora do Copilot — sempre valide o PBIR
  depois de cada lote de mudanças (`powerbi-report-author validate`); edição manual fora
  desse ciclo tende a gerar JSON inválido.
- Cada relatório é uma pasta isolada em `reports/`. Isso deixa `git diff`/PR por relatório
  limpo e permite dar acesso de repositório por domínio sem misturar relatórios não
  relacionados.
- `_brief/report-spec.md` é reescrito a cada novo planejamento — se quiser manter o
  histórico de specs aprovadas, copie para `docs/<NomeRelatorio>/report-spec-aprovado.md`
  depois da aprovação.

## 7. Copiar o kit para o seu projeto real

Este kit foi montado em `~/Development/general/powerbi-copilot-toolkit/` como um
**modelo/starter kit** — ele não é, em si, o seu repositório de Power BI. Copie o
conteúdo para dentro de cada repositório `powerbi-<dominio>/` real:

```bash
cp -r powerbi-copilot-toolkit/.github  powerbi-<dominio>/
cp -r powerbi-copilot-toolkit/.vscode  powerbi-<dominio>/
cp -r powerbi-copilot-toolkit/templates powerbi-<dominio>/templates
```

Os arquivos em `.github/prompts/*.prompt.md` aparecem digitando `/nome-do-arquivo` — nos
dois modos (Copilot CLI e chat do VS Code).

## 8. Os documentos principais deste kit

- **`GUIA-FLUXO-COMPLETO.md`** — os passos do fluxo completo de criação/recriação de
  relatório, na ordem certa, cada um dizendo qual skill (Modo CLI) **e** qual agente
  (Modo Chat) usar, além do que fazer quando nenhum dos dois cobre (acesso, fontes
  externas, RLS via SharePoint, documentação, handoff de design).
- **`MENU-ATIVIDADES-PONTUAIS.md`** — cardápio de correções pontuais (DAX, páginas, RLS,
  auditorias), com o comando/prompt de cada uma nos dois modos.
- **`USAR-NO-VSCODE-CHAT.md`** — aprofundamento do Modo Chat: submódulo de referência,
  MCP, limitações em relação ao Modo CLI (o que fica manual).
