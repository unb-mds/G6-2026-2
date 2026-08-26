# CONTEXT.md — Sistema de Monitoramento do RU (UnB)

Glossário do domínio. Sem detalhes de implementação — apenas conceitos e suas relações.

## Termos

**Campus**
Unidade física da UnB. Campi com RU: Darcy Ribeiro, Ceilândia, Gama, Planaltina.

**RU (Restaurante Universitário)**
Unidade de restaurante universitário vinculada a um `Campus`. Cada RU possui seu próprio `Cardápio` e sua própria `Fila`. Cada RU tem seus próprios **dias de funcionamento** (ex.: Darcy Ribeiro funciona todos os dias da semana, incluindo sábado e domingo; os demais RUs funcionam apenas de segunda a sexta) — só existe `Cardápio` para os dias em que o RU opera.

**Cardápio**
Conjunto de `Refeição` oferecidas por um RU em uma data específica. O *padrão* de refeições (café da manhã, almoço, jantar) se repete toda semana, mas o *conteúdo* (os itens servidos) muda a cada dia. A UnB publica um cardápio semanal em PDF por campus.

**Refeição**
Um dos períodos de alimentação de um `Cardápio` (ex.: café da manhã, almoço, jantar). Contém uma lista de `ItemCardápio`.

**ItemCardápio**
Um alimento específico oferecido dentro de uma `Refeição`, classificado por uma `Categoria`.

**Categoria (de item)**
Classificação de um `ItemCardápio` dentro de uma `Refeição`. Lista fechada, igual em todos os RUs e datas: guarnição, salada, sobremesa, bebida/refresco, prato principal, prato vegetariano, prato ovolactovegetariano.

**Usuário**
Pessoa com conta no sistema, com um `Papel`: Aluno (login via matrícula) ou Servidor (login via matrícula funcional para professores, ou SIAPE para os demais. Só um `Usuário` autenticado pode criar um `Relato`. O campus visualizado na tela é livre — não é vinculado à conta, qualquer `Usuário` ou `Visitante` pode ver/reportar em qualquer RU.

**Visitante**
Pessoa que acessa o sistema sem login. Pode visualizar a `Fila` e o `Cardápio` semanal de qualquer campus, mas não pode criar `Relato`.

**Relato**
Reporte de um `Usuário` autenticado sobre o estado da fila de um RU em um instante específico. Contém um nível qualitativo (escala ordenada de 4 níveis, nomes ainda a definir) e um instante de criação. Válido por 15 minutos a partir da criação. Opcionalmente inclui a localização GPS do usuário no momento do relato — quando presente, o relato recebe um selo de **"verificado"**, que lhe dá peso extra na agregação (além do peso por recência).

**Fila**
Estado da fila física de entrada de um RU em um dado momento, calculado como a média ponderada dos níveis de todos os `Relato` válidos (dentro dos 15 min) daquele RU. O peso de cada relato combina dois fatores: recência (relatos mais recentes pesam mais) e verificação por GPS (relatos verificados pesam mais que não-verificados da mesma idade).
