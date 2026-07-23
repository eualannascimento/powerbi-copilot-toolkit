---
mode: powerbi-planejamento
description: Inicia o fluxo completo de planejamento de um novo relatório Power BI (ou redesign amplo de um existente).
---
Quero criar (ou redesenhar) um relatório Power BI.

- Modelo semântico / dataset de origem: ${input:modelo:nome do modelo semântico ou caminho do .SemanticModel}
- Assunto / objetivo do relatório: ${input:assunto:ex. desempenho comercial mensal}
- É criação nova ou redesign de um relatório existente?: ${input:tipo:novo ou redesign}

Use o workflow guiado de planejamento de relatório (skill `powerbi-report-planning`):
inspecione o modelo, conduza as rodadas de perguntas (audiência, escopo, páginas,
identidade visual, destino de publicação) uma pergunta por vez, e produza
`_brief/report-spec.md` com o `Design Brief:` embutido. Não construa nada antes de eu
aprovar explicitamente a spec.

Depois de eu aprovar, siga a sequência: modelo semântico → PBIR → validação → Desktop →
screenshot. Não publique no Fabric a menos que eu confirme o workspace de destino.
