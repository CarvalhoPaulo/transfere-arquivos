# Funcionalidades Desenvolvidas

## Visão geral atual da aplicação

A aplicação implementa uma fábrica de software orientada por workflow SDD (Story, Spec, Plan, Task e Implementação), com:

- Backend em Spring Boot (APIs em `http://localhost:8080`)
- Frontend Angular com PrimeNG (no repositório atual, gerado com Angular 20)

O frontend concentra a operação da fábrica em três áreas principais:

- Gestão de projetos e repositórios de código
- Gestão de requisitos e acompanhamento do status por etapa
- Execução manual das etapas do workflow com suporte a prompts

## Navegação principal

O menu principal possui as entradas:

- Projetos
- Requisitos
- Prompt

A rota inicial redireciona para a lista de projetos (`/lista-projeto`).

## Funcionalidades de Projetos

### 1. Lista de projetos

Tela: `lista-projeto`

- Carrega projetos via API (`GET /api/projects`)
- Exibe ID, sigla e nome em tabela
- Clique em uma linha abre o cadastro do projeto

### 2. Cadastro de projeto

Tela: `cadastro-projeto/:id`

Permite:

- Cadastrar e editar projeto (sigla, nome e constituição)
- Salvar projeto (`POST /api/projects` para novo, `PUT /api/projects` para edição)
- Cadastrar e editar repositórios vinculados ao projeto
- Remover repositório da tela
- Atualizar constituição do projeto por processamento automático (`POST /api/projects/update-constitution-structure/{projetoId}`)
- Criar novo requisito para o projeto (navega para `chat-requisito/novo/:projetoId`)
- Fazer pergunta sobre o projeto para endpoint de discovery (`POST /discovery/ask`) e visualizar resposta

No grid de repositórios do projeto, há ações para:

- Gerar/atualizar estrutura do repositório (`PATCH /api/coderepos/update-structure/{repoId}`)
- Abrir cadastro detalhado do code repo

### 3. Cadastro de code repo

Tela: `cadastro-code-repo/:id`

Permite:

- Visualizar nome, caminho, branch e tipo do repositório
- Editar constituição e estrutura do repositório
- Salvar atualização (`PATCH /api/coderepos/update-constituctions/{repoId}`)

## Funcionalidades de Requisitos e Workflow

### 1. Lista de requisitos (painel de acompanhamento)

Tela: `lista-requisitos`

- Carrega resumo das conversas/requisitos (`GET /conversations/summary`)
- Exibe colunas de status por etapa: Story, Spec, Plan, Task e Impl
- Cada etapa possui botão de acesso à tela correspondente
- Exibe status visual (ex.: `IN_PROGRESS`, `APPROVED`)
- Permite abrir auditoria do resultado por requisito

### 2. Etapa Story (Chat de Requisito)

Tela: `chat-requisito/:id` ou `chat-requisito/novo/:projetoId`

Permite:

- Criar requisito/conversa inicial (quando entra por `novo`)
- Editar nome e mensagem base do requisito
- Refinar conteúdo enviando para LLM (`POST /message`)
- Gerar história de usuário da conversa (`POST /chat/aprove?sessionId=...`)
- Excluir conversa/requisito (`DELETE /conversations/{id}`)
- Alterar prompt da etapa (abre prompt `CREATE_USER_STORY`)

### 3. Etapa User Story

Tela: `detalhe-user-story/:id`

Permite:

- Visualizar metadados da história e da sessão
- Editar e salvar conteúdo da User Story (`PATCH /user-stories/{id}`)
- Excluir User Story (`DELETE /user-stories/{id}`)
- Gerar SDD Specification (`POST /sdd/{userStoryId}/spec`)
- Alterar prompt da etapa (abre prompt `CREATE_SSD_SPEC`)

### 4. Etapa Spec (SDD Specification)

Tela: `cadastro-spec-sdd/:id`

Permite:

- Visualizar e editar conteúdo da Spec
- Salvar (`PATCH /spec-sdds/{id}`)
- Aprovar (`PATCH /spec-sdds/{id}/approve`)
- Excluir (`DELETE /spec-sdds/{id}`)
- Quando aprovada, gerar Plan (`POST /sdd/{userStoryId}/plan`)
- Alterar prompt da etapa (abre prompt `CREATE_SSD_PLAN`)

### 5. Etapa Plan (SDD Plan)

Tela: `cadastro-plan-sdd/:id`

Permite:

- Visualizar e editar conteúdo do Plan
- Salvar (`PATCH /plan-sdds/{id}`)
- Aprovar (`PATCH /plan-sdds/{id}/approve`)
- Excluir (`DELETE /plan-sdds/{id}`)
- Quando aprovado, gerar Task (`POST /sdd/{userStoryId}/task`)
- Alterar prompt da etapa (abre prompt `CREATE_SSD_TASK`)

### 6. Etapa Task (SDD Task)

Tela: `cadastro-task-sdd/:id`

Permite:

- Visualizar e editar conteúdo da Task
- Salvar (`PATCH /task-sdds/{id}`)
- Aprovar (`PATCH /task-sdds/{id}/approve`)
- Excluir (`DELETE /task-sdds/{id}`)
- Gerar preview técnico (`GET /sdd-executor/preview/{id}`)
- Quando aprovada, gerar implementação (`POST /sdd/{userStoryId}/impl`)

### 7. Etapa Implementação (SDD Impl)

Tela: `cadastro-impl-sdd/:id`

Permite:

- Visualizar e editar conteúdo da implementação
- Salvar (`PATCH /impl-sdds/{id}`)
- Aprovar (`PATCH /impl-sdds/{id}/approve`)
- Excluir (`DELETE /impl-sdds/{id}`)
- Gerar código efetivo via executor (`POST /sdd-executor/execute-impl/{implId}`)
- Executar teste docker (`POST /sdd-executor/execute-docker/1`)

## Funcionalidades de Prompt

### 1. Lista de prompts

Tela: `lista-prompt`

- Lista prompts cadastrados (`GET /prompts`)
- Clique na linha abre o cadastro do prompt

### 2. Cadastro de prompt

Tela: `cadastro-prompt/:id` ou `cadastro-prompt/key/:key`

Permite:

- Carregar prompt por ID ou por chave
- Editar conteúdo do prompt
- Salvar (`POST /prompts`)
- Excluir por chave (`DELETE /prompts/key/{key}`)

Essa funcionalidade é usada no workflow para ajustar prompts específicos de cada etapa.

## Auditoria de resultado

Tela: `auditar-resultado/:id`

Permite:

- Enviar uma pergunta de auditoria para uma User Story
- Executar auditoria via prompt (`POST /sdd/{userStoryId}/prompt-audit`)
- Visualizar retorno estruturado (fonte, confiança, justificativa, trecho, resposta bruta) ou resposta textual

## Comportamento de execução e loading

O comportamento atual no frontend é:

- Transição entre etapas feita manualmente pelo usuário
- Cada etapa abre tela própria com conteúdo em área editável (`textarea`)
- Ações de salvar, aprovar, gerar próxima etapa, excluir e alterar prompt ficam na própria tela da etapa
- As chamadas HTTP usam loading global com overlay e contador de segundos
- A execução é síncrona do ponto de vista da UI (sem processamento em background controlado pelo frontend)

## Fluxo funcional atual (resumo)

1. Usuário cria/seleciona projeto e configura repositórios.
2. Usuário cria requisito no chat e refina conteúdo.
3. Usuário gera User Story.
4. Usuário revisa e salva User Story.
5. Usuário gera Spec, aprova e gera Plan.
6. Usuário aprova Plan e gera Task.
7. Usuário aprova Task e gera Implementação.
8. Usuário gera código na etapa de Implementação e pode rodar teste Docker.
9. Usuário audita resultado quando necessário.
