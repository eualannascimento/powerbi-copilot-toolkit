# Documentação técnica — [Nome do relatório]

> Público: desenvolvedores/analistas de BI que vão manter este relatório. Peça ao
> Copilot para preencher a partir do TMDL, PBIR e `report-spec.md` do relatório.

## 1. Fontes de dados

| Fonte | Modo de armazenamento | Frequência de refresh | Observações |
|---|---|---|---|

## 2. Modelo semântico

- **Tabelas** (fato/dimensão, grão de cada uma):
- **Relacionamentos** (origem → destino, cardinalidade, direção de filtro, ativo/inativo):
- **Medidas principais** (nome, DAX, dependências):
- **Colunas calculadas** (nome, DAX, motivo de existir em vez de vir da fonte):
- **Hierarquias**:

## 3. Segurança (RLS/OLS)

- **Roles definidas** e expressão DAX de filtro de cada uma.
- Se usa o padrão "lista SharePoint" (`templates/rls-sharepoint-pattern.md`): local da
  lista, estrutura de colunas, quem mantém.
- Como testar localmente (`Ver como` no Desktop).

## 4. Estrutura do relatório (PBIR)

- Páginas e o que cada uma contém (visuais principais, filtros de página).
- Tema (`theme.json`): nome, paleta base, onde fica o arquivo.
- Bookmarks e navegação, se houver.
- Drillthrough/tooltips customizados, se houver.

## 5. Publicação

- Workspace de destino, nome do relatório e do modelo semântico no Fabric.
- Como publicar (skill `powerbi-report-management` / comando usado).
- Credencial usada (usuário pessoal vs. service principal) e onde está documentada a
  solicitação de acesso correspondente (`templates/solicitacao-acesso.md`).

## 6. Decisões de modelagem e pendências conhecidas

- Decisões não óbvias e o porquê (ex. "medida X usa `ALL` porque...").
- Limitações conhecidas / dívida técnica.
- Itens auditados e não corrigidos (link para a última auditoria, se houver).
