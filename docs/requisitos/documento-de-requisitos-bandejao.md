# Documento de Requisitos de Software — Bandejão

**Universidade de Brasília — Faculdade de Ciências e Tecnologias em Engenharia (FCTE)**
**Disciplina:** Métodos de Desenvolvimento de Software (MDS)
**Grupo 6** · **Orientadora:** Profa. Carla Silva Rocha Aguiar
**Brasília, 2026**

> Este documento tem como base o Documento de Visão do projeto Bandejão (Grupo 6, 2026) e detalha, a partir dele, os requisitos funcionais, não funcionais e inversos do sistema, organizados por épico e com rastreabilidade até as necessidades das partes interessadas.

---

## 1. Introdução

### 1.1 Propósito

Este documento especifica, de forma detalhada e verificável, os requisitos funcionais (RF), não funcionais (RNF) e inversos (RI) do sistema Bandejão, servindo como referência para o desenvolvimento, teste e avaliação do produto ao longo do projeto.

### 1.2 Escopo

O documento cobre a totalidade das funcionalidades identificadas no Documento de Visão (Seção 5), independentemente da release em que estão alocadas — MVP, Release 2 ou Backlog do produto —, permitindo o planejamento e a rastreabilidade completa do sistema desde a primeira entrega até a visão de longo prazo do produto.

### 1.3 Convenções deste documento

| Elemento | Convenção |
|---|---|
| ID de requisito funcional | `RF` + dois dígitos sequenciais, únicos em todo o documento (ex.: RF01, RF02, ...) |
| ID de requisito não funcional | `RNF` + dois dígitos sequenciais (ex.: RNF01, RNF02, ...) |
| ID de requisito inverso | `RI` + dois dígitos sequenciais (ex.: RI01, RI02, ...) |
| Release | MVP, Release 2 ou Backlog, conforme Seção 8 do Documento de Visão |
| Prioridade | Derivada diretamente da Matriz de Impacto x Esforço do Documento de Visão: **Essencial** (MVP), **Importante** (Release 2), **Desejável** (Backlog) |

### 1.4 Documentos de referência

- Documento de Visão — Bandejão, Grupo 6, FCTE/UnB, 2026.
- FGA-EPS-MDS. *Documento de Visão — Projeto 2018.2-Lino*. Disponível em: https://github.com/fga-eps-mds/2018.2-Lino/blob/master/docs/documento-de-visao.md.

---

## 2. Requisitos Funcionais

Cada requisito funcional é descrito com um identificador único, sua descrição, ao menos um critério de aceite objetivo e testável, a release em que está alocado, a prioridade e a origem (épico e necessidade da parte interessada, conforme Seções 3.7 e 5 do Documento de Visão).

### Épico 1 — Login

#### RF01 — Cadastro de usuário

**Descrição:** O sistema deve permitir que um usuário (estudante, professor ou servidor) crie uma conta informando, no mínimo, nome, e-mail e senha.

**Critério de aceite:**
- O sistema rejeita cadastro com e-mail já existente na base, exibindo mensagem de erro específica.
- O sistema rejeita senhas com menos de 8 caracteres.
- Após cadastro bem-sucedido, o usuário é redirecionado para a tela de login ou autenticado automaticamente.

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Épico Login · Pré-requisito para avaliação de refeições (necessidade "Saber a qualidade da comida antes de decidir comer no RU")

---

#### RF02 — Login de usuário cadastrado

**Descrição:** O sistema deve permitir que um usuário previamente cadastrado se autentique informando e-mail e senha, para acessar funcionalidades restritas (avaliação de refeições e, na Release 2, votação de fila).

**Critério de aceite:**
- Credenciais corretas resultam em sessão autenticada e redirecionamento à página inicial.
- Credenciais incorretas exibem mensagem de erro genérica, sem indicar se o e-mail existe ou não na base (proteção contra enumeração de contas).
- Após 5 tentativas malsucedidas consecutivas para o mesmo e-mail em um intervalo de 10 minutos, o sistema bloqueia novas tentativas por 10 minutos.

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Épico Login · Pré-requisito para avaliação de refeições

---

#### RF03 — Login validado por matrícula/SIAPE

**Descrição:** O sistema deve oferecer um mecanismo de login robusto que valide a identidade do usuário por meio da matrícula (estudantes) ou do SIAPE/matrícula funcional (servidores e professores), necessário para validar votos no sistema de fila em tempo real.

**Critério de aceite:**
- O sistema recusa cadastro/login com número de matrícula ou SIAPE em formato inválido.
- Um usuário validado por matrícula/SIAPE é habilitado a votar no nível de fila (RF11); um usuário sem essa validação não é.

**Release:** Release 2 · **Prioridade:** Importante
**Origem:** Épico Login · Necessidade "Evitar longas filas no horário de pico" (pré-requisito de confiabilidade da votação)

---

#### RF04 — Navegação sem autenticação

**Descrição:** O sistema deve permitir que qualquer visitante, sem necessidade de cadastro ou login, consulte o cardápio diário/semanal dos campi.

**Critério de aceite:**
- A página de cardápio é acessível a partir da URL raiz do sistema sem exigir autenticação.
- Funcionalidades que exigem autenticação (avaliar refeição, votar em fila) exibem um convite ao cadastro/login ao serem acionadas por um visitante, em vez de erro genérico.

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Transversal (Seção 4.1 do Documento de Visão — sistema como "porta") · Necessidade "Planejar a refeição com informação antecipada"

---

### Épico 2 — Visualização do Cardápio

#### RF05 — Leitura automatizada do PDF do cardápio

**Descrição:** O sistema deve extrair automaticamente as informações de cardápio a partir do arquivo PDF publicado periodicamente no site oficial do RU, sem intervenção manual da equipe em condições normais de publicação.

**Critério de aceite:**
- Diante de uma nova publicação do PDF no site do RU, o cardápio exibido no sistema é atualizado em até 24 horas.
- Caso a leitura automatizada falhe (ex.: mudança no formato do arquivo), o sistema mantém em exibição o último cardápio lido com sucesso e registra um alerta para a equipe, em vez de exibir uma página vazia ou quebrada.

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Épico Cardápio · Necessidade "Planejar a refeição com informação antecipada"

---

#### RF06 — Exibição do cardápio por dia e por campus

**Descrição:** O sistema deve exibir o cardápio segmentado por campus (ex.: Darcy Ribeiro, Gama) e por dia da semana, permitindo ao usuário selecionar qual campus e qual dia deseja consultar.

**Critério de aceite:**
- O usuário consegue alternar entre ao menos dois campi distintos e visualizar cardápios diferentes para cada um.
- O usuário consegue alternar entre os dias da semana corrente sem recarregar a página inteira (navegação em abas ou equivalente).

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Épico Cardápio · Necessidade "Planejar a refeição com informação antecipada"

---

#### RF07 — Filtro de restrições alimentares e alergias

**Descrição:** O sistema deve permitir que o usuário aplique filtros ao cardápio exibido, ocultando ou sinalizando pratos que contenham alérgenos ou ingredientes previamente selecionados como restrição (ex.: lactose, glúten, frutos do mar).

**Critério de aceite:**
- Ao ativar um filtro de restrição, pratos correspondentes deixam de aparecer na listagem padrão ou são exibidos com marcação visual explícita de alerta.
- O filtro selecionado permanece ativo ao navegar entre dias/campi na mesma sessão.

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Épico Cardápio · Necessidade "Saber a qualidade da comida antes de decidir comer no RU" (Seção 2.2 — dificuldade de quem possui restrição alimentar)

---

#### RF08 — Ícones de indicação de alérgenos

**Descrição:** O sistema deve exibir, ao lado de cada prato do cardápio, ícones que indiquem visualmente a presença de alérgenos comuns (ex.: cogumelo, leite e derivados, mel).

**Critério de aceite:**
- Cada prato identificado como contendo um alérgeno cadastrado exibe o ícone correspondente, visível sem necessidade de interação adicional (hover ou clique).

**Release:** Backlog · **Prioridade:** Desejável
**Origem:** Épico Cardápio

---

#### RF09 — Visualização refinada do cardápio

**Descrição:** O sistema deve converter a leitura bruta do PDF em uma apresentação visual próxima de um cardápio tradicional, com design refinado, em vez de uma listagem simples de texto extraído.

**Critério de aceite:**
- O cardápio exibido segue um layout com hierarquia visual clara entre categorias de prato (ex.: prato principal, acompanhamento, sobremesa), validado por revisão de design da equipe.

**Release:** Release 2 · **Prioridade:** Importante
**Origem:** Épico Cardápio

---

### Épico 3 — Visualização da Fila e Previsão de Pico

#### RF10 — Previsão estática de horário de pico

**Descrição:** O sistema deve apresentar ao usuário uma previsão simples do horário de maior movimento do RU, baseada em padrões conhecidos de uso (ex.: pico por volta das 11h40), sem depender do sistema de votação em tempo real.

**Critério de aceite:**
- A tela de fila exibe ao menos uma faixa de horário identificada como "horário de pico esperado" para o dia corrente, mesmo sem nenhum voto registrado.

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Épico Fila · Necessidade "Evitar longas filas no horário de pico"

---

#### RF11 — Visualização do nível de fila em tempo real

**Descrição:** O sistema deve calcular e exibir o nível atual da fila do RU em quatro categorias (vazia, curta, moderada, longa), com base na votação colaborativa de usuários autenticados e validados (RF03).

**Critério de aceite:**
- Um voto registrado por um usuário validado é refletido no nível de fila exibido em até 1 minuto.
- Cada voto tem validade de 15 minutos, deixando de contar para o cálculo do nível de fila após esse período.
- O mesmo usuário só pode registrar um novo voto após um cooldown de 15 minutos a partir do voto anterior; uma tentativa de voto antes disso é rejeitada com mensagem informando o tempo restante.

**Release:** Release 2 · **Prioridade:** Importante
**Origem:** Épico Fila · Necessidade "Evitar longas filas no horário de pico"

---

#### RF12 — Sistema de monitoramento/informativo da fila

**Descrição:** O sistema deve consolidar os votos de fila recebidos em um painel informativo acessível ao usuário, mostrando a evolução do nível de fila ao longo do período de funcionamento do RU.

**Critério de aceite:**
- O usuário consegue visualizar, em uma mesma tela, o nível de fila atual e um histórico simplificado (ex.: gráfico ou lista) do nível registrado nas últimas horas do dia corrente.

**Release:** Release 2 · **Prioridade:** Importante
**Origem:** Épico Fila

---

#### RF13 — Check-in no RU

**Descrição:** O sistema deve permitir que um usuário autenticado registre sua presença no RU (check-in), associando essa ação ao sistema de fila.

**Critério de aceite:**
- Um usuário autenticado consegue registrar check-in a partir da interface do sistema, e essa ação fica associada ao seu histórico de uso.

**Release:** Release 2 · **Prioridade:** Importante
**Origem:** Épico Fila

---

#### RF14 — Acesso ao GPS para confirmar voto de fila

**Descrição:** O sistema deve utilizar a localização do dispositivo do usuário (GPS) para confirmar que o voto de nível de fila foi feito a partir das imediações do RU correspondente, aumentando a confiabilidade da votação colaborativa.

**Critério de aceite:**
- Um voto de fila só é aceito quando a localização do dispositivo está dentro de um raio pré-definido do RU votado; fora desse raio, o sistema informa que o voto não pôde ser confirmado por localização.
- O usuário é informado, antes da primeira votação, sobre a necessidade de conceder permissão de localização ao navegador.

**Release:** Release 2 · **Prioridade:** Importante
**Origem:** Épico Fila

---

#### RF15 — Confirmação de check-in por GPS

**Descrição:** O sistema deve utilizar a localização do dispositivo do usuário para confirmar automaticamente um check-in realizado nas imediações do RU.

**Critério de aceite:**
- Um check-in realizado dentro do raio pré-definido do RU é marcado como "confirmado por localização"; fora desse raio, é marcado como "não confirmado".

**Release:** Backlog · **Prioridade:** Desejável
**Origem:** Épico Fila

---

### Épico 4 — Avaliação das Refeições

#### RF16 — Avaliação de refeições por estrelas e comentários

**Descrição:** O sistema deve permitir que um usuário autenticado avalie uma refeição específica do cardápio do dia atribuindo uma nota de 1 a 5 estrelas e, opcionalmente, um comentário em texto livre, associados ao seu nome de usuário.

**Critério de aceite:**
- Um usuário autenticado consegue registrar uma avaliação (nota obrigatória, comentário opcional) para um prato do cardápio do dia corrente.
- A avaliação registrada é exibida publicamente, associada ao nome do usuário avaliador, para qualquer visitante que consulte aquele prato.
- Um mesmo usuário só pode registrar uma avaliação por prato por dia; uma segunda tentativa no mesmo dia edita a avaliação anterior em vez de criar uma nova.

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Épico Avaliação · Necessidade "Saber a qualidade da comida antes de decidir comer no RU"

---

#### RF17 — Consulta de avaliações de pratos anteriores

**Descrição:** O sistema deve disponibilizar um menu separado com as avaliações salvas de pratos já servidos anteriormente, permitindo ao usuário consultar a reputação histórica de um prato mesmo fora do dia em que foi avaliado.

**Critério de aceite:**
- O usuário consegue localizar, em uma tela dedicada, a nota média e os comentários de um prato específico já servido em datas passadas.

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Épico Avaliação · Necessidade "Saber a qualidade da comida antes de decidir comer no RU"

---

### Transversal

#### RF18 — Design e identidade visual do site

**Descrição:** O sistema deve seguir uma distribuição de informações e paleta de cores definidas e consistentes em todas as telas, garantindo identidade visual coesa ao produto.

**Critério de aceite:**
- Todas as telas do sistema utilizam a mesma paleta de cores e os mesmos padrões de tipografia e espaçamento definidos pelo guia de estilo da equipe, verificável por inspeção visual comparativa entre telas.

**Release:** MVP · **Prioridade:** Essencial
**Origem:** Transversal (Seção 5.16 do Documento de Visão)

---

## 3. Requisitos Não Funcionais

Os requisitos não funcionais abaixo propõem metas mensuráveis para categorias não quantificadas no Documento de Visão (Seção 7 — Faixas de Qualidade). Os valores foram calibrados para serem **defensáveis e verificáveis em operação**, e ao mesmo tempo **alcançáveis por uma equipe iniciante em projetos de maior porte**, evitando metas irreais que comprometeriam a entrega do MVP.

#### RNF01 — Desempenho

O sistema deve responder a requisições de visualização de cardápio em até **3 segundos**, em condições normais de operação (até 300 usuários simultâneos), medido do lado do servidor em pelo menos 95% das requisições.

**Justificativa:** cobre a "Métrica de Eficiência" citada na Seção 7 do Documento de Visão sem exigir infraestrutura de alta escala incompatível com um projeto acadêmico.

---

#### RNF02 — Disponibilidade e confiabilidade

O sistema deve estar disponível pelo menos **95% do tempo** durante o horário de funcionamento dos RUs em dias letivos (aproximadamente 7h às 19h). Em caso de falha na leitura automatizada do cardápio (RF05), o sistema deve continuar exibindo o último cardápio lido com sucesso, em vez de ficar indisponível.

**Justificativa:** meta realista para um serviço mantido por uma equipe estudantil, ainda assim protegendo a experiência do usuário contra a principal fonte de instabilidade identificada (dependência do PDF externo).

---

#### RNF03 — Usabilidade

Um usuário não autenticado deve conseguir visualizar o cardápio do dia do seu campus em **no máximo 3 interações** (cliques/toques) a partir da página inicial. A interface deve ser funcional em telas a partir de **360px de largura**, compatível com o caráter PWA/mobile do produto.

**Justificativa:** o perfil de usuário descrito na Seção 3.4 do Documento de Visão é de pessoas com pouco tempo disponível; a usabilidade precisa refletir isso objetivamente.

---

#### RNF04 — Segurança

Senhas de usuário devem ser armazenadas de forma **criptografada (hash)**, nunca em texto plano. Toda comunicação entre cliente e servidor deve utilizar **HTTPS**. Entradas de usuário (ex.: comentários de avaliação) devem ser tratadas para prevenir injeção de código (XSS) e injeção de consultas (SQL Injection), usando consultas parametrizadas ou ORM.

**Justificativa:** conjunto mínimo de boas práticas de segurança viável para uma equipe em seu primeiro projeto de maior porte, sem exigir maturidade de segurança de nível corporativo.

---

#### RNF05 — Portabilidade e compatibilidade

O sistema, como PWA, deve funcionar corretamente nas **duas versões mais recentes** dos navegadores Chrome, Firefox, Edge e Safari, tanto em desktop quanto em dispositivos móveis, e deve ser instalável na tela inicial do dispositivo conforme os critérios básicos de instalabilidade de um PWA (manifest válido e service worker registrado).

**Justificativa:** decorre diretamente do requisito de sistema definido na Seção 9.2 do Documento de Visão (entrega como PWA).

---

#### RNF06 — Manutenibilidade

O código-fonte deve manter cobertura de testes automatizados de, no mínimo, **60% nos módulos críticos** (leitura de cardápio, autenticação, avaliação de refeições e, na Release 2, votação de fila), e a arquitetura deve separar claramente esses módulos, de modo que uma mudança no formato do PDF do cardápio (risco citado na Seção 6 do Documento de Visão) exija alteração apenas no módulo de leitura de cardápio.

**Justificativa:** meta de cobertura de testes compatível com uma equipe sem experiência prévia em projetos de grande porte, mas suficiente para reduzir o risco já identificado no Documento de Visão (inexperiência da equipe com as tecnologias escolhidas).

---

## 4. Requisitos Inversos

Requisitos inversos definem explicitamente o que o sistema **não deve** fazer, delimitando o escopo e prevenindo interpretações equivocadas durante o desenvolvimento.

| ID | Descrição |
|---|---|
| **RI01** | O sistema não deve permitir que usuários não autenticados registrem avaliações de refeições ou votos de nível de fila. |
| **RI02** | O sistema não deve exibir cardápios ou informações de restaurantes/cantinas que não sejam os RUs oficiais da UnB. |
| **RI03** | O sistema não deve computar mais de um voto de fila do mesmo usuário dentro da janela de cooldown de 15 minutos. |
| **RI04** | O sistema não deve armazenar senhas de usuário em texto plano, sob nenhuma circunstância. |
| **RI05** | O sistema não deve depender de sensores físicos (câmeras, contadores de pessoas) para estimar o nível de fila — a estimativa deve se basear exclusivamente na votação colaborativa dos usuários cadastrados. |
| **RI06** | O sistema não deve bloquear o acesso à visualização do cardápio para usuários que optem por não se cadastrar. |

---

## 5. Matriz de Rastreabilidade

| ID | Épico | Necessidade da Parte Interessada (Seção 3.7 do Documento de Visão) | Release |
|---|---|---|---|
| RF01 | Login | Pré-requisito de avaliação — "Saber a qualidade da comida antes de decidir comer no RU" | MVP |
| RF02 | Login | Pré-requisito de avaliação e votação de fila | MVP |
| RF03 | Login | "Evitar longas filas no horário de pico" (confiabilidade da votação) | Release 2 |
| RF04 | Login / Transversal | "Planejar a refeição com informação antecipada" | MVP |
| RF05 | Cardápio | "Planejar a refeição com informação antecipada" | MVP |
| RF06 | Cardápio | "Planejar a refeição com informação antecipada" | MVP |
| RF07 | Cardápio | "Saber a qualidade da comida antes de decidir comer no RU" | MVP |
| RF08 | Cardápio | "Saber a qualidade da comida antes de decidir comer no RU" | Backlog |
| RF09 | Cardápio | "Planejar a refeição com informação antecipada" | Release 2 |
| RF10 | Fila | "Evitar longas filas no horário de pico" | MVP |
| RF11 | Fila | "Evitar longas filas no horário de pico" | Release 2 |
| RF12 | Fila | "Evitar longas filas no horário de pico" | Release 2 |
| RF13 | Fila | "Evitar longas filas no horário de pico" | Release 2 |
| RF14 | Fila | "Evitar longas filas no horário de pico" (confiabilidade da votação) | Release 2 |
| RF15 | Fila | "Evitar longas filas no horário de pico" (confiabilidade do check-in) | Backlog |
| RF16 | Avaliação | "Saber a qualidade da comida antes de decidir comer no RU" | MVP |
| RF17 | Avaliação | "Saber a qualidade da comida antes de decidir comer no RU" | MVP |
| RF18 | Transversal | Experiência geral do usuário | MVP |
| RNF01–RNF06 | Transversal | Métrica de Eficiência (Seção 7) e requisitos de sistema (Seção 9.2) | MVP / Release 2 |

---

## 6. Resumo por Release

| Release | Requisitos Funcionais |
|---|---|
| **MVP** | RF01, RF02, RF04, RF05, RF06, RF07, RF10, RF16, RF17, RF18 |
| **Release 2** | RF03, RF09, RF11, RF12, RF13, RF14 |
| **Backlog** | RF08, RF15 |

Todos os requisitos não funcionais (RNF01–RNF06) e todos os requisitos inversos (RI01–RI06) aplicam-se transversalmente a partir do MVP, ainda que alguns (ex.: RNF06 quanto à votação de fila, RI03, RI05) só se manifestem completamente a partir da entrada em vigor das funcionalidades de fila na Release 2.
