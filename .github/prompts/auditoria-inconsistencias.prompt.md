---
mode: agent
description: Audita um relatório/modelo Power BI em busca de inconsistências e erros técnicos.
---
Relatório/modelo a auditar: ${input:alvo:caminho em reports/ ou nome no Fabric}

Faça uma auditoria técnica, sem alterar nada ainda — apenas relate. Verifique:

1. **Modelo (TMDL)**: relacionamentos inativos não utilizados, relacionamentos
   bidirecionais desnecessários, colunas/medidas com erro de sintaxe ou referência
   quebrada, tipos de dado incoerentes com o uso (ex. coluna numérica formatada como
   texto), medidas com `CALCULATE`/`FILTER` que ignoram contexto de filtro de forma não
   intencional.
2. **RLS**: roles cuja expressão DAX não filtra nenhuma linha (RLS "vazando" dados),
   roles referenciando colunas/tabelas que não existem mais.
3. **PBIR (páginas/visuais)**: visuais sem binding de campo (quebrados), filtros
   apontando para campo removido, sobreposição de visuais, textos com nome técnico cru
   (ex. "Sum of qtd_venda" em vez de um nome legível), formatos de número incoerentes
   (taxas mostrando 0,53 em vez de 53%).
4. **Fontes**: fontes de dados que falharam no último refresh (se houver log/histórico
   acessível), parâmetros de conexão hardcoded que deveriam ser parametrizados.

Entregue uma lista priorizada (crítico / importante / cosmético), cada item com o
arquivo/local exato e uma sugestão de correção — mas não aplique a correção sem eu pedir.
