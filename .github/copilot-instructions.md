# Instruções do projeto — Power BI

Este repositório contém projetos Power BI no formato PBIP (`reports/<Nome>/*.pbip`,
`.Report/`, `.SemanticModel/` em TMDL).

## Skills disponíveis

Se o GitHub Copilot CLI estiver rodando com os plugins deste kit instalados
(`powerbi-authoring`, `fabric-consumption`, `pbip`, `semantic-models`, `tabular-editor`,
`fabric-admin`, `pbi-desktop` — ver `LEIA-PRIMEIRO.md`), prefira sempre a skill
especializada em vez de editar JSON/TMDL "na mão":

- **Planejar relatório novo ou redesign amplo** → skill `powerbi-report-planning`.
- **Design visual, tema, paleta, arquétipo de página** → skill `powerbi-report-design`.
- **Editar páginas/visuais/filtros/tema em PBIR já existente** → skill `powerbi-report-authoring`.
- **Publicar, listar, baixar relatórios do Fabric** → skill `powerbi-report-management`.
- **Medidas, colunas calculadas, relacionamentos, RLS (role + filtro DAX)** → skill
  `semantic-model-authoring`.
- **Inspecionar modelo/dados existentes (somente leitura)** → skill `semantic-model-consumption`
  ou `fabric-consumption`.

## Regras deste repositório

- Nunca edite manualmente os arquivos dentro de `*.Report/` ou `*.SemanticModel/` sem
  rodar a validação da skill depois (`powerbi-report-author validate` para PBIR).
- Cada relatório vive isolado em `reports/<Nome>/` — nunca misture arquivos de relatórios
  diferentes na mesma pasta.
- Antes de publicar em um workspace do Fabric, confirme o nome do workspace com o usuário
  — nunca assuma. Publicação é sempre uma ação explicitamente pedida, nunca automática.
- RLS deste repositório usa o padrão "lista SharePoint" por padrão — ver
  `templates/rls-sharepoint-pattern.md` antes de criar ou alterar qualquer role de
  segurança. Não proponha atribuição de usuários/grupos a roles via API/TMDL — isso é
  fora de escopo das skills e deve ser feito manualmente no portal do Power BI (ou,
  quando o padrão SharePoint estiver em uso, editando a lista, não o portal).
- Para o passo de design visual (mockup, capa, plano de fundo, materiais de divulgação),
  não gere imagens você mesmo — monte o prompt para a ferramenta externa de IA de design
  usando `.github/prompts/design-brief-para-ia-visual.prompt.md`.
- Documentação de relatório sempre em dois arquivos separados: um técnico
  (`templates/doc-desenvolvedor.md`) e um de negócio (`templates/doc-analista-negocio.md`).

## Referências rápidas deste kit

- `GUIA-FLUXO-COMPLETO.md` — fluxo ordenado para criar/recriar um relatório do zero.
- `MENU-ATIVIDADES-PONTUAIS.md` — atalhos para correções pontuais.
- `templates/` — modelos de inventário de fontes, solicitação de acesso, RLS via
  SharePoint, e os dois formatos de documentação.
