---
mode: powerbi-modelo-semantico
description: Identifica medidas, colunas e tabelas sem uso em nenhum lugar do modelo/relatório.
---
Modelo semântico / relatório a revisar: ${input:alvo:caminho do .SemanticModel/.Report ou nome no Fabric}

Identifique itens sem uso, sem alterar nada ainda:

1. Para cada medida/coluna calculada, verifique se ela é referenciada por:
   - outra medida (dependência DAX),
   - algum visual/filtro/slicer/tooltip em qualquer página do `.Report`,
   - alguma role de RLS,
   - algum relacionamento (no caso de colunas).
   Se disponível, use a DMV `INFO.CALCDEPENDENCY()` (via XMLA/MCP de modelagem) para
   dependências de medidas em vez de inferir só por leitura de texto.
2. Para colunas físicas (não calculadas), verifique também se estão ocultas
   (`isHidden`) e sem uso — candidatas a remover da importação (reduz tamanho do
   modelo).
3. Não conte como "sem uso" campos usados apenas como chave de relacionamento, mesmo que
   nenhum visual os mostre diretamente.

Entregue uma lista dividida em:
- **Seguro remover** (sem nenhuma referência encontrada),
- **Revisar com o time de negócio** (parece sem uso, mas pode ser consumido fora deste
  relatório — ex. por outro relatório no mesmo modelo, ou por export/Analyze in Excel).

Não delete nada sem eu confirmar item a item.
