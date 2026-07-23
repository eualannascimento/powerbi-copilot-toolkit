---
mode: powerbi-autoria
description: Altera algo pontual em uma ou mais páginas de um relatório Power BI já existente (bookmark, gráfico, alinhamento, comportamento, cor, plano de fundo).
---
Relatório: ${input:relatorio:caminho do .pbip/.Report em reports/}
Página(s) afetada(s): ${input:paginas:nome da(s) página(s), ou "todas"}
O que precisa mudar: ${input:mudanca:ex. alterar cor de um gráfico, mover um cartão, criar um bookmark, trocar plano de fundo}

Use a skill `powerbi-report-authoring`. Antes de editar:
1. Leia o PBIR atual da(s) página(s) afetada(s) — não assuma estrutura, confira o JSON real.
2. Se a mudança for de cor/tema, verifique se existe um tema (`theme.json`) — prefira
   alterar o tema a fazer override visual a visual, a menos que o pedido seja
   explicitamente pontual em um único visual.
3. Depois de editar, rode a validação de PBIR e recarregue no Power BI Desktop; tire
   screenshot da página alterada para conferência visual antes de finalizar.
4. Não altere páginas ou visuais fora do escopo pedido.
