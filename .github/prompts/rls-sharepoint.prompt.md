---
mode: powerbi-modelo-semantico
description: Cria ou altera segurança em nível de linha (RLS) usando o padrão de lista SharePoint como fonte de quem-vê-o-quê.
---
Modelo semântico: ${input:caminhoModelo:caminho do .SemanticModel ou nome no Fabric}
O que precisa: ${input:necessidade:ex. criar RLS nova para a tabela Regional, adicionar uma condição, trocar a coluna de comparação}

Leia primeiro `templates/rls-sharepoint-pattern.md` neste repositório — ele define o
padrão adotado aqui (tabela de segurança carregada de uma lista SharePoint, comparação
via `USERPRINCIPALNAME()`, uma única role de portal).

Use a skill `semantic-model-authoring` para:
1. Confirmar se a tabela de segurança (staging da lista SharePoint) já existe no modelo;
   se não existir, prepare a estrutura esperada (colunas mínimas: e-mail/UPN do usuário +
   dimensão(ões) liberada(s)) e avise que a fonte de dados (conector SharePoint List) e o
   agendamento de refresh são passos manuais no Power BI Desktop/Service.
2. Criar ou ajustar a role e a expressão DAX de filtro conforme `rls-sharepoint-pattern.md`.
3. Validar com `USERPRINCIPALNAME()` simulando pelo menos dois usuários diferentes da
   lista (um com acesso amplo, um restrito) antes de considerar concluído.

Não proponha chamadas de API/TMDL para adicionar usuários/grupos à role no portal — isso
é fora de escopo e deve continuar manual. Lembre o usuário de que, neste padrão, a
manutenção de quem-vê-o-quê deve ser feita editando a lista SharePoint, não o portal.
