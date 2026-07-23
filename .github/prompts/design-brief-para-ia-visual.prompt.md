---
mode: powerbi-design
description: Converte o Design Brief (YAML) de um report-spec.md aprovado em um prompt pronto para a IA de design visual externa gerar mockup, capa, plano de fundo ou material de divulgação.
---
Report spec aprovado: ${input:reportSpec:caminho, ex. _brief/report-spec.md ou docs/<Nome>/report-spec-aprovado.md}
Peça específica: ${input:peca:ex. mockup da página inicial, capa do relatório, imagem de plano de fundo, arte de divulgação}

Leia o bloco `Design Brief:` (YAML) dentro do arquivo indicado e monte um prompt em
português, pronto para colar em uma ferramenta externa de geração de imagem/design,
contendo:

1. **Tom e assinatura visual** (`design_identity.tone` / `signature`) traduzidos em
   linguagem de briefing de design (não cole o YAML cru — traduza para descrição visual).
2. **Paleta de cores** e **tipografia** (se definidas no brief ou no tema), descritas por
   nome/uso (cor primária, cor de destaque, cor de fundo) em vez de só hex codes, mas
   inclua os hex codes também entre parênteses.
3. **Arquétipo e propósito da página** relevante ao pedido (`pages[].archetype`,
   `pages[].role`) — isso define o tom do mockup (executivo/operacional/analítico/
   narrativo/comparativo).
4. **Formato e dimensão de saída** adequados ao pedido: mockup de página = 1920x1080
   (mesma proporção do canvas do relatório); capa = proporção vertical ou quadrada
   conforme uso; plano de fundo = 1920x1080 com área central "respirável" para não
   competir com os visuais que serão sobrepostos.
5. **O que NÃO deve conter**: nenhum texto/dado real do relatório (a peça é decorativa/
   estrutural, os dados entram depois via Power BI), nenhum logotipo que não seja o da
   empresa (pergunte se precisa incluir logo).

Ao final, mostre o prompt pronto em um bloco de texto separado, fácil de copiar, e
pergunte se quer ajustar tom, cor ou formato antes de eu levar para a ferramenta externa.
Não gere a imagem você mesmo.
