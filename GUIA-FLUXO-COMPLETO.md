# Fluxo completo — criação ou recriação de um relatório Power BI

Ordem semântica recomendada. Cada fase diz **o que fazer**, com **os dois modos** (ver
`LEIA-PRIMEIRO.md` para a diferença entre eles) lado a lado, e **o que é manual**
(nenhum dos dois cobre — é decisão/ação humana, geralmente em portal).

Os seus 11 itens originais foram reordenados assim porque acesso lógico e técnico
(pedir → conceder) precisa vir **antes** de decifrar/usar os dados, e design **depois**
de saber o que vai ser mostrado (senão você desenha em cima de dado que ainda não existe).

```
0. Descoberta e escopo do pedido
1. Obter bases de dados + identificar fontes
2. Pedir acesso (usuário + serviço)
3. Decifrar dados e extrair regras de negócio
4. Planejar o relatório (audiência, páginas, escopo) — trava a spec
5. Design: identidade visual, mockup, layout/plano de fundo
6. Autoria: modelo semântico (medidas, RLS) + páginas/visuais PBIR
7. Validar e conferir no Desktop
8. Publicar + conceder acesso ao relatório
9. Documentar (dev + analista de negócio)
```

---

## 0. Descoberta e escopo do pedido

Antes de qualquer skill/agente, deixe claro, em uma frase, o que está pedindo: "criar um
relatório novo" (fase 4) vs. "corrigir/alterar algo que já existe" (vai direto para
`MENU-ATIVIDADES-PONTUAIS.md` — não repita este fluxo inteiro para uma correção pontual).

---

## 1. Obter bases de dados + identificar fontes/origens

**Não é uma skill/agente — é levantamento.** Preencha `templates/inventario-fontes-dados.md`
para cada fonte: nome, tipo (Fabric nativo vs. externo), local, responsável, frequência
de atualização.

Classifique cada fonte:

| Categoria | Exemplos | Modo CLI | Modo Chat |
|---|---|---|---|
| **Fabric nativo** | Lakehouse, Warehouse, SQL DB no Fabric, Dataflow Gen2, Eventhouse/KQL | Skills `*-consumption-cli` do `fabric-skills` (ex.: "liste as tabelas do warehouse X") | MCP **FabricIQ** (`DiscoverArtifacts`, `GetSemanticModelSchema`) direto no chat, sem skill nenhuma |
| **Fora do Fabric, com migração conhecida** | Databricks, Synapse, HDInsight | Skills `databricks-migration`, `synapse-migration`, `hdinsight-migration` | Sem agente dedicado neste kit — peça no chat genérico descrevendo a origem; considere instalar o Modo CLI só para essa etapa |
| **Fora do Fabric, sem skill dedicada** | SAS (extratos/tabelas), Teradata, arquivos avulsos, SharePoint (listas/planilhas), input manual | Documente manualmente; peça o script/notebook de ingestão (Spark/Python) via `spark-authoring-cli` | Mesma coisa, peça ao agente `powerbi-modelo-semantico` (ou chat genérico) para gerar o script de ingestão depois que o dado estiver acessível |

---

## 2. Pedir acesso (usuário + usuário de serviço)

**Manual — decisão/aprovação humana**, nos dois modos. Use `templates/solicitacao-acesso.md`
para formalizar: local (workspace/lakehouse/tabela), papel necessário
(Viewer/Contributor/Admin), motivo, prazo.

- **Usuário pessoal**: acesso via portal do Fabric/Azure AD — workspace role concedida
  pelo dono do workspace.
- **Usuário de serviço** (service principal / managed identity): necessário para
  automação (publicação via `az rest`, refresh agendado). Peça ao time de
  plataforma/segurança o registro do app e a concessão do mesmo papel de workspace.
- Validar depois de obter as credenciais (igual nos dois modos):
  ```bash
  az login
  az account get-access-token --resource https://api.fabric.microsoft.com
  ```

---

## 3. Decifrar dados e extrair regras de negócio/técnicas

| Modo CLI | Modo Chat |
|---|---|
| Skills de `fabric-consumption` (read-only): amostrar linhas, checar cardinalidade/nulos. Se já existir modelo publicado, `semantic-model-consumption` inspeciona sem recriar do zero. Prompt equivalente ao exemplo oficial: *"Documente o workspace X: liste itens, papel de cada um, linhagem, e o que cada notebook/pipeline/view faz. Salve em docs/workspace/."* | MCP **FabricIQ** (`GetSemanticModelSchema`, `ExecuteQuery`, `ValueSearch`) direto no chat, sem instalar nada. Para workspace inteiro (itens além do modelo semântico), o MCP é mais limitado — se precisar de inventário completo do workspace, vale abrir o Modo CLI só para essa consulta. |

Isso te dá as regras técnicas (transformações, joins, filtros aplicados a montante). As
regras de **negócio** normalmente só vêm de conversa com o dono do processo — registre-as
você mesmo no inventário de fontes ou direto no `report-spec.md` da fase 4.

---

## 4. Planejar o relatório (trava a spec antes de construir)

| Modo CLI | Modo Chat |
|---|---|
| Skill **`powerbi-report-planning`** (pacote `powerbi-authoring`), acionada automaticamente ao pedir "quero criar um relatório novo a partir do modelo X" | Agente **`powerbi-planejamento`** (`.github/agents/`) — selecione no dropdown do chat, ou rode o prompt file `/novo-relatorio` (já entra nesse agente) |

Nos dois casos: roteiro guiado — Definir → Inspecionar → Especificar → Aprovar —
perguntando uma coisa de cada vez (audiência, páginas, escopo, identidade visual,
destino de publicação), gerando `_brief/report-spec.md` como contrato único das fases
seguintes. Nada é construído até você aprovar essa spec explicitamente.

> Use esta fase também para **recriação** de um relatório existente (modo "brownfield" —
> preserva ou troca a identidade visual atual).

---

## 5. Design: identidade visual, mockup, layout/plano de fundo

| Modo CLI | Modo Chat |
|---|---|
| Skill **`powerbi-report-design`**, acionada internamente pela skill de planejamento (fase 4) | Agente **`powerbi-design`** — o agente de planejamento sugere trocar para ele nas decisões de tom/arquétipo; ou selecione manualmente |

Produz um `Design Brief:` (YAML) embutido no `report-spec.md`: tom, arquétipo de página,
paleta, tipografia. Esse Design Brief é a base do handoff para a sua IA de design externa:

1. Depois do `report-spec.md` aprovado, rode `/design-brief-para-ia-visual` (funciona
   nos dois modos) — lê o `Design Brief:` YAML e converte em um prompt pronto para colar
   na sua IA de imagem/design (mockup, capa, plano de fundo, divulgação).
2. Leve os arquivos gerados de volta como assets — entram no relatório na fase 6.

---

## 6. Autoria: modelo semântico + páginas/visuais PBIR

Duas frentes, nesta ordem (modelo antes de relatório, porque as páginas referenciam
medidas/colunas):

**6a. Modelo semântico**

| Modo CLI | Modo Chat |
|---|---|
| Skill `semantic-model-authoring` | Agente `powerbi-modelo-semantico`, ou prompt files `/dax-criar-corrigir` e `/rls-sharepoint` |

Cria/edita medidas, colunas calculadas, relacionamentos, hierarquias, roles de RLS e a
expressão DAX de cada role (ver `templates/rls-sharepoint-pattern.md` para o padrão "RLS
via lista SharePoint" — item 7 original). **Não cobre**, em nenhum dos dois modos,
atribuir usuários/grupos a uma role — isso é manual no portal (fase 8).

**6b. Páginas e visuais (PBIR)**

| Modo CLI | Modo Chat |
|---|---|
| Skill `powerbi-report-authoring` | Agente `powerbi-autoria`, ou prompt file `/editar-pagina` |

Implementa o `Design Brief:` aprovado: páginas, visuais, filtros, slicers, bookmarks,
formatação, tema, imagens de plano de fundo da fase 5. Valida a cada lote de mudanças
(`powerbi-report-author validate` — CLI de linha de comando, roda pelo terminal
integrado nos dois modos).

No Modo CLI, isso roda **automaticamente** dentro do fluxo de planejamento depois que
você aprova a spec. No Modo Chat, você troca manualmente do agente `powerbi-planejamento`
para `powerbi-modelo-semantico` e depois `powerbi-autoria` — cada um sugere o próximo
passo ao final da resposta.

---

## 7. Validar e conferir no Power BI Desktop

Igual nos dois modos: recarregar o `.pbip` no Desktop (`powerbi-desktop` CLI), tirar
screenshot de cada página nova/alterada, revisar contra o checklist de
acessibilidade/anti-padrões (contraste, nomes de campo legíveis, rótulos em %,
sobreposição de visuais). No Modo CLI a skill `powerbi-report-authoring` faz esse ciclo
sozinha quando pedida; no Modo Chat o agente `powerbi-autoria` faz o mesmo, chamando as
CLIs pelo terminal integrado — confirme só que Power BI Desktop e Node.js estão
instalados.

---

## 8. Publicar + conceder acesso ao relatório

| Modo CLI | Modo Chat |
|---|---|
| Skill **`powerbi-report-management`** | Agente **`powerbi-gestao`** |

Publica o `.pbip` local (modelo + relatório) para o workspace do Fabric via API REST
(`az rest`), resolvendo workspace/relatório por nome. Prompt (nos dois modos):
> "Publique o relatório em `reports/<Nome>/` no workspace [X]."

**Conceder acesso** (item 6 do seu pedido original) é manual, feito no portal do
Fabric/Power BI Service, igual nos dois modos: adicionar desenvolvedores como
`Contributor`/`Member` no workspace, clientes finais como `Viewer` do app/relatório
publicado. Se o RLS usa o padrão "lista SharePoint" (fase 6a), a única atribuição de
role necessária aqui é **uma vez**: colocar todos os usuários finais numa única role de
RLS — quem vê o quê depois é 100% controlado pela lista SharePoint, sem voltar ao portal.

---

## 9. Documentar (formato dev + formato analista de negócio)

Não existe skill/agente dedicado — é um prompt sobre o que já foi construído (TMDL +
PBIR + `report-spec.md` já contêm quase tudo). Funciona igual nos dois modos, sem
prompt file dedicado (peça direto no chat, com o agente/skill de modelo ou autoria ativo,
ou no CLI genérico):

- `templates/doc-desenvolvedor.md` — grão técnico: tabelas/colunas/medidas, DAX,
  relações, RLS (roles + filtro), fontes e frequência de refresh, decisões de modelagem.
- `templates/doc-analista-negocio.md` — grão de negócio: o que cada página responde, como
  ler cada visual, definição de cada métrica em linguagem de negócio, quem vê o quê (RLS
  em termos de "por que", não de DAX).

Prompt:
> "Gere a documentação do relatório em `reports/<Nome>/` nos dois formatos, usando
> `templates/doc-desenvolvedor.md` e `templates/doc-analista-negocio.md`, e salve em
> `docs/<Nome>/`."
