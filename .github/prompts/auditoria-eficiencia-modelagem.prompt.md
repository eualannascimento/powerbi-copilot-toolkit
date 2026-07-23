---
mode: powerbi-modelo-semantico
description: Audita a eficiência e a modelagem (star schema, cardinalidade, DAX) de um modelo semântico Power BI.
---
Modelo semântico a revisar: ${input:modelo:caminho do .SemanticModel ou nome no Fabric}

Faça uma auditoria de eficiência/modelagem, sem alterar nada ainda — apenas relate:

1. **Formato do esquema**: o modelo segue star schema (fatos vs. dimensões) ou há
   relacionamentos "floco de neve"/tabelas híbridas fato-dimensão que poderiam ser
   separadas?
2. **Cardinalidade e tamanho**: colunas de alta cardinalidade importadas sem necessidade
   (ex. um GUID ou timestamp completo quando só a data importa), colunas numéricas que
   poderiam ser inteiras e estão como decimal/texto, colunas totalmente não usadas em
   nenhuma medida/visual/relacionamento (para o inventário completo de "sem uso", use
   `/auditoria-itens-nao-usados`).
3. **Relacionamentos**: bidirecionais desnecessários (custo de performance e risco de
   ambiguidade), relacionamentos inativos que forçam `USERELATIONSHIP` em toda medida.
4. **DAX**: medidas recalculando a mesma sub-expressão várias vezes em vez de usar uma
   medida base reaproveitada, uso de `FILTER(ALL(...))` onde `CALCULATE` com filtro direto
   resolveria com menos custo, iteradores (`SUMX`/`FILTER`) sobre tabelas grandes que
   poderiam ser pré-agregadas.
5. **Modo de armazenamento**: tabelas em Import que talvez devessem ser DirectQuery/Direct
   Lake (ou vice-versa) dado o volume e a frequência de atualização necessária.

Se o plugin `tabular-editor` (skill `bpa-rules`) estiver instalado, ofereça também gerar
um conjunto de regras de Best Practice Analyzer para automatizar essa checagem em
modelos futuros.

Entregue uma lista priorizada por impacto (tamanho do modelo / velocidade de
consulta / manutenibilidade), com a mudança concreta sugerida. Não aplique nada sem eu
confirmar.
