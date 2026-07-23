# Inventário de fontes de dados — [Nome do relatório/projeto]

Uma linha por fonte. Preencha ao longo da fase 1 do `GUIA-FLUXO-COMPLETO.md`.

| Fonte | Categoria | Local exato | Responsável/dono | Frequência de atualização | Acesso pessoal (status) | Acesso serviço (status) | Regras de negócio conhecidas |
|---|---|---|---|---|---|---|---|
| Ex.: Warehouse Vendas | Fabric nativo | Workspace X / Warehouse Vendas | Time BI Comercial | Diária, 06h | Concedido (Viewer) | Pendente | Cancelamento zera a venda do mês, não do trimestre |
| Ex.: Extrato SAS Crédito | Externo — sem skill de ingestão | Compartilhado por e-mail mensal pelo time de Risco | Time Risco | Mensal | Pendente | N/A | — |
| Ex.: Lista SharePoint "Acesso Regional" | Externo — usado para RLS | site.sharepoint.com/sites/BI/Lists/AcessoRegional | Analista X | Sob demanda | Concedido | N/A | Fonte da tabela de segurança RLS |

## Categorias possíveis

- **Fabric nativo**: Lakehouse, Warehouse, SQL DB, Dataflow Gen2, Eventhouse/KQL.
- **Migração conhecida**: Databricks, Synapse, HDInsight (existe skill de migração).
- **Externo sem skill de ingestão**: SAS, Teradata, arquivo avulso, input manual.
- **Externo usado para RLS**: lista/planilha SharePoint que alimenta segurança.

## Checklist por fonte antes de seguir para a fase 4 (planejamento)

- [ ] Local exato documentado (não "no servidor de sempre").
- [ ] Acesso pessoal concedido e testado.
- [ ] Acesso de serviço concedido (se a fonte fará parte de refresh automatizado).
- [ ] Regras de negócio e técnicas relevantes anotadas (mesmo que informalmente).
- [ ] Se externa sem skill de ingestão: decidido o caminho de entrada no Fabric
      (Dataflow, notebook Spark, import direto no Desktop).
