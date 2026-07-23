# Padrão de RLS via lista SharePoint

Objetivo: quem-vê-o-quê é controlado por uma **lista SharePoint** editável por um
analista de negócio, sem precisar tocar no portal do Power BI nem em DAX a cada mudança
de permissão. É o padrão a usar sempre que o pedido for "criar RLS usando lista SharePoint".

## 1. Estrutura da lista SharePoint (fonte)

Colunas mínimas:

| Coluna | Tipo | Exemplo |
|---|---|---|
| `UPN` (e-mail corporativo do usuário) | Texto | `maria.silva@empresa.com` |
| `ValorPermitido` | Texto | `Sudeste` (o valor da dimensão que essa pessoa pode ver) |

Regra prática para usuários com **acesso total**: crie uma linha por usuário para
**cada** valor válido da dimensão (denormalizado), ou uma linha especial com um valor
sentinela (ex. `TODOS`) tratada à parte — ver seção 4 para a variante avançada. Comece
pela abordagem denormalizada; é mais simples de manter por um analista sem DAX.

Cada linha nova = uma pessoa a mais vendo aquele recorte. Apagar a linha = revogar. Isso
é 100% controlado pela lista, sem mexer no relatório.

## 2. Carregar a lista no modelo semântico

No Power BI Desktop: **Obter dados → Lista do SharePoint**, aponte para o site/lista.
Nomeie a tabela `Seguranca RLS`. Normalize o UPN para minúsculas na Poder Query (evita
falha de comparação por caixa alta/baixa):

```m
= Table.TransformColumns(#"Fonte", {{"UPN", Text.Lower, type text}})
```

> **Refresh**: mudanças na lista só valem depois do próximo refresh agendado do modelo
> (a menos que o dataset esteja em modo DirectQuery/near-real-time, que a conectora de
> Lista SharePoint não suporta bem). Avise quem gerencia a lista que a revogação não é
> instantânea — alinhe a frequência de refresh com a criticidade do dado.

## 3. Relacionamento

Relacione `Seguranca RLS[ValorPermitido]` → `DimX[ValorPermitido]` (a dimensão que você
está protegendo, ex. `DimRegiao[Regiao]`), cardinalidade muitos-para-um,
**filtro em uma direção só** (de `Seguranca RLS` para `DimX`). Isso propaga o filtro de
`Seguranca RLS` para `DimX` e, por transitividade, para as tabelas fato relacionadas a
`DimX`.

## 4. Role e expressão DAX de filtro

Peça à skill `semantic-model-authoring` para criar a role (ex. `AcessoPorListaSharePoint`)
com o filtro de tabela em `Seguranca RLS`:

```dax
'Seguranca RLS'[UPN] = LOWER(USERPRINCIPALNAME())
```

Isso filtra a tabela `Seguranca RLS` só para as linhas do usuário logado — e, via o
relacionamento da seção 3, filtra `DimX` e as fatos junto.

### Variante avançada — flag de acesso total sem denormalizar

Se preferir uma linha só por usuário com uma coluna booleana `AcessoTotal`, o filtro
precisa reintroduzir todos os valores da dimensão quando a flag é verdadeira. Isso exige
uma medida auxiliar (RLS por medida) em vez de filtro puro de tabela — mais complexo de
manter; só vale a pena se o número de linhas denormalizadas ficar grande demais. Peça
ajuda pontual ao Copilot só se cair nesse caso; a abordagem denormalizada da seção 1
resolve a maioria dos casos reais.

## 5. Atribuição de role no portal (uma vez só)

Publique o relatório e, no portal do Power BI Service, atribua **todos os usuários
finais** a essa única role (`AcessoPorListaSharePoint`) — uma vez. Depois disso, nunca
mais volte ao portal para gerenciar quem vê o quê: tudo passa a ser editar a lista
SharePoint (seção 1).

> A skill `semantic-model-authoring` explicitamente **não** mexe em atribuição de
> usuários/grupos a roles (é considerado fora do escopo dela) — por isso esse passo é
> manual, mas só precisa acontecer uma vez por causa deste padrão.

## 6. Validação antes de considerar concluído

Peça ao Copilot para simular `USERPRINCIPALNAME()` com pelo menos dois UPNs da lista
(um com acesso restrito, um com acesso total) e confirmar que cada um vê exatamente o
recorte esperado — no Power BI Desktop isso é **Modelagem → Ver como → Esta role** com
um UPN específico.
