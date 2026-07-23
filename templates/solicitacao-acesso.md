# Solicitação de acesso — [Nome do relatório/projeto]

Preencha uma solicitação por local/fonte e envie ao dono do recurso (workspace admin,
DBA, time de dados, etc.). Uma cópia por credencial (pessoal e de serviço).

## Modelo

- **Solicitante**: [seu nome]
- **Data**: [data]
- **Relatório/projeto associado**: [nome]
- **Recurso**: [workspace / lakehouse / warehouse / tabela / lista SharePoint / pasta]
- **Local exato**: [URL, caminho, nome do workspace]
- **Tipo de credencial**: [ ] Usuário pessoal &nbsp; [ ] Usuário de serviço (service principal)
- **Papel necessário**: [ ] Viewer &nbsp; [ ] Contributor &nbsp; [ ] Admin &nbsp; [ ] Outro: ___
- **Motivo**: [para que serve o acesso — ex. "leitura para modelagem do relatório X"]
- **Prazo desejado**: [permanente / até data X]
- **Aprovador**: [nome/cargo, se souber]

## Usuário de serviço — informações extras a levantar

- Nome do app registrado no Azure AD (se já existir) ou pedido de registro novo.
- Client ID / Tenant ID (depois de aprovado, guardar em local seguro, nunca no repositório).
- Escopo mínimo necessário (ex. `Workspace.ReadWrite.All` só se for publicar; `Workspace.Read.All`
  se for só consumir).
- Onde essa credencial será usada: publicação manual pontual vs. pipeline agendado —
  isso muda se a credencial deve ser de curta duração (token) ou permanente (secret/cert).

## Status de acompanhamento

| Recurso | Tipo credencial | Solicitado em | Status | Concedido em |
|---|---|---|---|---|
| | | | Pendente / Aprovado / Negado | |
