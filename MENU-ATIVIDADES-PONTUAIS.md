# Menu de atividades pontuais

Correções e consultas do dia a dia — **não** rode o fluxo completo (`GUIA-FLUXO-COMPLETO.md`)
para nada disso. Cada item tem um prompt file pronto em `.github/prompts/`, que funciona
igual nos dois modos (ver `LEIA-PRIMEIRO.md`): digite `/nome-do-arquivo` no chat.

| # | Atividade | Skill (Modo CLI) | Agente (Modo Chat) | Prompt file (os dois modos) |
|---|---|---|---|---|
| 1 | Gerar/corrigir uma medida DAX | `semantic-model-authoring` | `powerbi-modelo-semantico` | `/dax-criar-corrigir` |
| 2 | Alterar página(s): bookmark, gráfico, alinhamento, comportamento, cor, plano de fundo | `powerbi-report-authoring` | `powerbi-autoria` | `/editar-pagina` |
| 3 | Alterar RLS (role, filtro DAX, ou a lista SharePoint que controla o acesso) | `semantic-model-authoring` + `templates/rls-sharepoint-pattern.md` | `powerbi-modelo-semantico` | `/rls-sharepoint` |
| 4 | Buscar inconsistências/erros | Leitura de TMDL/PBIR + checklist de anti-padrões | Agente genérico (abrange modelo + PBIR, sem agente único) | `/auditoria-inconsistencias` |
| 5 | Buscar pontos de melhoria (design/UX) | `powerbi-report-design` (referências) | `powerbi-design` | `/auditoria-melhorias` |
| 6 | Buscar pontos de eficiência/modelagem | `semantic-model-authoring` + skill `bpa-rules` (Tabular Editor, se instalada) | `powerbi-modelo-semantico` | `/auditoria-eficiencia-modelagem` |
| 7 | Identificar medidas/colunas sem uso | Análise de dependências DAX (`INFO.CALCDEPENDENCY`) | `powerbi-modelo-semantico` | `/auditoria-itens-nao-usados` |

## Como usar — Modo CLI

1. Rode `copilot` no terminal.
2. Digite `/` + nome do prompt file (autocomplete mostra os disponíveis), ou descreva a
   tarefa em português — a skill certa é escolhida automaticamente.
3. Preencha o que for pedido (nome do relatório, tabela, medida, etc.).

## Como usar — Modo Chat

1. Abra o chat do Copilot no VS Code (modo *Agent*) com o repositório do domínio Power BI
   aberto.
2. Digite `/` e o nome do prompt file — ele já troca para o agente certo sozinho (campo
   `mode:` no arquivo). Sem prompt file, selecione o agente manualmente no dropdown do chat.
3. Preencha os `${input}` que o VS Code pedir — abre uma caixa de texto para cada um.

## Vale para os dois modos

Revise o diff antes de aceitar — nenhum prompt deste kit publica ou sobrescreve nada em
Fabric automaticamente; publicação é sempre um passo separado e explícito (fase 8 do
`GUIA-FLUXO-COMPLETO.md`).

## Quando usar o fluxo completo em vez do menu

Se a "correção pontual" na verdade exige nova fonte de dados, novo acesso, ou uma página
inteiramente nova com identidade visual própria — pare e volte para
`GUIA-FLUXO-COMPLETO.md`. O menu pressupõe que fonte, acesso e identidade visual já
existem.
