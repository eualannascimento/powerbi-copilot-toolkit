---
mode: powerbi-modelo-semantico
description: Cria ou corrige uma medida/coluna calculada DAX em um modelo semântico existente.
---
Preciso de uma medida/coluna DAX no modelo semântico em: ${input:caminhoModelo:caminho do .SemanticModel ou nome no Fabric}

- Nome do campo: ${input:nomeCampo:nome da medida ou coluna}
- O que ela deve calcular (em português claro, com regra de negócio se houver): ${input:regra:descreva o cálculo}
- É criação nova ou correção de uma medida existente? Se for correção, qual o problema observado?: ${input:situacao:novo ou descreva o bug}

Use a skill `semantic-model-authoring`. Antes de escrever a expressão:
1. Inspecione as tabelas/colunas/medidas relacionadas para usar nomes reais (não invente
   nomes de campo).
2. Verifique relacionamentos e direção de filtro envolvidos no cálculo.
3. Depois de criar/editar, valide o resultado com uma consulta DAX de amostra e mostre o
   resultado antes/depois se for correção.
4. Se a medida for usada por alguma página do relatório, avise quais páginas podem
   precisar de reload/refresh visual.

Não altere nenhuma role de RLS neste prompt — se a mudança envolve segurança, use
`/rls-sharepoint`.
