---
mode: powerbi-design
description: Audita um relatório Power BI em busca de pontos de melhoria de design/UX (sem quebra técnica).
---
Relatório a revisar: ${input:relatorio:caminho em reports/}

Faça uma auditoria de design/UX, sem alterar nada ainda — apenas relate. Use a skill
`powerbi-report-design` como referência de critério (arquétipo de página, acessibilidade,
seleção de gráfico, anti-padrões) e avalie cada página quanto a:

1. **Escolha de gráfico**: o tipo de visual respeita a hierarquia de codificação
   (posição > comprimento > ângulo > área > cor)? Há gráfico de linha com linha reta
   demais ou barra com só 2 categorias (sinal de visual errado para o dado)?
2. **Cor**: paleta consistente entre páginas, contraste adequado (WCAG AA), mesma medida
   sempre na mesma cor entre visuais.
3. **Layout**: título de página presente, slicers num local prático e consistente,
   tabelas detalhadas no rodapé em vez de dominarem a página, espaço vazio desbalanceado.
4. **Callouts/cards**: algum cartão de KPI repete um valor absoluto já mostrado no
   gráfico ao lado sem agregar contexto (variação, meta, ranking)?
5. **Acessibilidade**: alt text em gráficos, contraste, slicers de campo com muitas
   categorias usando busca/dropdown em vez de lista longa.

Entregue uma lista priorizada por página, com sugestão concreta (não "melhore o visual",
e sim "troque X por Y porque Z"). Não aplique nada sem eu confirmar.
