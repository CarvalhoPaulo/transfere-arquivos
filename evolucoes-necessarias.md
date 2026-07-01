# Evoluções Necessárias

Este documento consolida as evoluções necessárias para a próxima versão da fábrica de software SDD, ampliando o fluxo atual de criação de histórias de usuário e revisão de artefatos até a geração da Specification (Spec).

## 1. Nova esteira de agentes entre o chat e a Spec

Hoje o fluxo principal sai do chat de criação da história de usuário e segue diretamente para a geração da Spec. A evolução proposta insere novas etapas intermediárias, com artefatos próprios, validações específicas e maior automação.

### 1.1 Etapas propostas

O fluxo passa a considerar a seguinte sequência lógica **executada sequencialmente**:

**Novas Etapas (Evolução):**

1. Chat de criação da História de Usuário.
2. Agente de Requisitos.
3. Agente de Acessibilidade.
4. Agente de Prototipação.
5. Modelo de Dados Conceitual.
6. Power Design.
7. Modelo de Dados Físico.
8. Estruturador de Testes.
9. Modelador de Testes de Qualidade.
10. Modelador de Testes de Desenvolvimento.
11. Geração da Spec.

**Etapas Existentes (Consolidação):**

12. Geração do Plan.
13. Geração de Tasks.
14. Codificação.

Essa sequência deve ser entendida como uma esteira de produção de artefatos sequencial, onde cada agente recebe um insumo específico e devolve um resultado estruturado para a próxima etapa. **Todas as 11 novas etapas devem ser implementadas na próxima versão, e as etapas existentes (Plan, Tasks, Codificação) serão adaptadas para consumir os artefatos refinados gerados pela nova esteira.**

### 1.2 Descrição das novas etapas

#### 1.2.1 Agente de Requisitos

Responsável por gerar um arquivo de requisitos com estrutura definida no prompt, a partir do artefato produzido na conversação.

Importante:

- Não confundir o Agente de Requisitos com o cadastro de requisitos existente no sistema.
- Embora ambos usem o mesmo nome no domínio, eles representam processos diferentes.
- O Agente de Requisitos é uma etapa automatizada da esteira, enquanto o cadastro de requisitos é uma funcionalidade de gestão.

Resultado esperado:

- Um artefato de requisitos organizado e reutilizável pelas próximas etapas do fluxo.

#### 1.2.2 Agente de Acessibilidade

Agente especializado em analisar Histórias de Usuário e gerar Critérios de Acessibilidade que deverão ser atendidos na prototipação.

Características esperadas:

- Base de conhecimento em Markdown.
- Leitura orientada por requisitos funcionais e jornada do usuário.
- Produção de critérios objetivos e verificáveis.

Resultado esperado:

- Lista de critérios de acessibilidade aplicáveis à jornada da solução.

#### 1.2.3 Agente de Prototipação

Recebe a História de Usuário, os Critérios de Acessibilidade e o dispositivo alvo da solução.

Objetivo:

- Analisar os requisitos com base em boas práticas internas e externas da Caixa Econômica Federal.
- Responder com um Fluxo de Navegação contendo caminho feliz, erros e exceções.
- Entregar uma lista detalhada de telas com comportamento esperado, componentes sugeridos e critérios de acessibilidade por etapa.

Resultado esperado:

- Um protótipo textual estruturado, pronto para apoiar design e validação de jornada.

#### 1.2.4 Modelo de Dados Conceitual

Agente que recebe Histórias de Usuário, transcrições de reuniões e regras de negócio, podendo ainda usar glossário, exemplos de dados e restrições.

Objetivo:

- Extrair requisitos e semântica do domínio.
- Identificar entidades, atributos, relacionamentos, cardinalidades e regras de integridade.
- Produzir um modelo conceitual de dados claro e consistente em Markdown.

Resultado esperado:

- DER conceitual em Markdown.
- Registro de premissas e dúvidas para eventual ajuste.

#### 1.2.5 Power Design

Agente que gera, a partir do modelo conceitual, um arquivo XML com a modelagem de banco de dados para Power Design seguindo os padrões da Caixa.

Resultado esperado:

- Arquivo XML compatível com o padrão esperado para modelagem.

#### 1.2.6 Modelo de Dados Físico

Agente que recebe o modelo conceitual, regras de negócio, Histórias de Usuário, transcrições de reuniões e a indicação do SGBD alvo.

Objetivo:

- Validar coerência entre os insumos recebidos.
- Derivar um modelo físico aderente ao SGBD escolhido.
- Produzir script DDL completo com tabelas, colunas, tipos, chaves, índices e constraints.

SGBDs previstos:

- SQL Server.
- Oracle.
- DB2.
- PostgreSQL.
- Sybase em manutenção.

Resultado esperado:

- Script DDL completo em `.sql` ou `.txt`.
- Sinalização de lacunas ou inconsistências encontradas.

#### 1.2.7 Estruturador de Testes

Agente de IA especializado em análise de requisitos funcionais e design de testes, com foco em qualidade de software.

Objetivo:

- Transformar a documentação de entrada em artefatos estruturados de teste.
- Gerar casos de teste funcionais e negociais com cobertura positiva, negativa e de exceção.
- Organizar os casos em suítes coerentes por funcionalidade e por fluxo crítico.

Regra de operação na primeira interação:

- O agente deve solicitar o conteúdo a ser tratado em texto claro e completo, seja fornecido diretamente ou por arquivo anexado.
- Se o conteúdo estiver fragmentado ou incompleto, deve sinalizar e pedir complementação.
- Se houver mais de um documento, deve questionar se eles devem ser tratados individualmente, em qual ordem, ou em conjunto.

Resultado esperado:

- Suítes de teste estruturadas com casos de teste completos.

#### 1.2.8 Modelador de Testes de Qualidade

Agente especializado em detalhamento de testes.

Objetivo:

- Receber suítes de teste já definidas pelo Estruturador.
- Transformar os casos em roteiro de testes com scripts consistentes.
- Garantir alinhamento entre artefato de requisito e cenário de teste.

Resultado esperado:

- Roteiro de testes detalhado, com coerência entre requisito, suíte e caso.

#### 1.2.9 Modelador de Testes de Desenvolvimento

O objetivo funcional deste agente ainda não foi definido.

Diretriz inicial:

- Reservar a etapa para um futuro papel voltado a testes técnicos, automatizados ou de apoio ao desenvolvimento.
- A definição final deve ser feita antes da implementação, para evitar sobreposição com os demais agentes de teste.

### 1.3 Descrição das etapas existentes

As seguintes etapas já existem no sistema e deverão ser adaptadas para consumir os artefatos refinados gerados pela nova esteira:

#### 1.3.1 Geração do Plan

Etapa que recebe a Spec gerada pela etapa anterior e produz um plano de implementação estruturado.

Objetivo:

- Analisar a Spec e decompor a solução em componentes lógicos.
- Definir sequência de implementação, dependências e marcos (milestones).
- Produzir um documento de plan com estimativas e priorização.

Resultado esperado:

- Documento de plan com arquitetura proposta, componentes e cronograma de implementação.

Impacto da Evolução:

- A qualidade e detalhe dos artefatos anteriores (requisitos estruturados, modelos de dados, protótipos, testes) melhorarão significativamente a precisão do plan.

#### 1.3.2 Geração de Tasks

Etapa que recebe o Plan e decompõe em tarefas atômicas.

Objetivo:

- Transformar o plan em tarefas granulares e executáveis.
- Definir responsabilidades, critérios de aceite, dependências entre tarefas.
- Organizar tarefas por épica, feature, ou sprint.

Resultado esperado:

- Lista estruturada de tasks com rastreabilidade completa para o plan e spec.

Impacto da Evolução:

- Tasks melhor definidas graças aos artefatos de teste estruturados e requisitos refinados.

#### 1.3.3 Codificação

Etapa de desenvolvimento que executa as tasks e produz código de produção.

Objetivo:

- Implementar as funcionalidades descritas nas tasks.
- Seguir padrões de código, arquitetura e boas práticas.
- Produzir código testável, documentado e pronto para produção.

Resultado esperado:

- Código-fonte implementado e integrado aos repositórios de código.
- Artefatos de construção (build, deployment) funcionais.

Impacto da Evolução:

- Desenvolvimento mais preciso e acelerado, com menor necessidade de refinamento, graças aos artefatos de prototipagem, dados e testes já estruturados nas etapas anteriores.

### 1.4 Pontos de Atenção - Ajustes Necessários nas Etapas Existentes

⚠️ **As etapas existentes (Plan, Tasks, Codificação) deverão ser ajustadas para:**

#### Geração do Plan

- **Ações:** Substituir ações existentes pelas 3 ações padrão: Aprovar, Reprovar, Solicitar Alteração.
- **Consumo de Artefatos:** Receber como entrada a Spec aprovada, o Modelo de Dados Conceitual, o Protótipo e os Testes estruturados gerados pelas novas etapas.
- **Saída:** Documento de Plan estruturado baseado nos artefatos refinados.

#### Geração de Tasks

- **Ações:** Substituir ações existentes pelas 3 ações padrão: Aprovar, Reprovar, Solicitar Alteração.
- **Consumo de Artefatos:** Receber como entrada o Plan aprovado, a Spec, o Modelo de Dados e os Testes para criar Tasks com rastreabilidade.
- **Saída:** Lista de Tasks estruturada com referência aos artefatos de origem.

#### Codificação

- **Aprovação:** Através de Pull Request (PR) no GitHub, não pela aplicação.
- **Consumo de Artefatos:** Usar como referência as Tasks, a Spec, o Protótipo, o Modelo de Dados e os Testes para guiar a implementação.
- **Saída:** Código implementado conforme especificação e testes estruturados.

## 2. Repositório de conhecimento e organização por agentes

Os arquivos de conhecimento dos agentes ficarão em um repositório GitHub organizado por pastas por agente.

### Necessidades principais

- Cada agente deverá possuir sua própria pasta de conhecimento.
- Os prompts, bases em Markdown, exemplos e instruções devem ficar organizados por contexto de uso.
- A estrutura deve facilitar manutenção, versionamento e evolução independente dos agentes.
- **Cada agente pode ter um modelo LLM diferente**, selecionável via properties do sistema.
- **Definições específicas** de escopo (escopo de SGBDs, tipo de testes, critérios de acessibilidade) ficam configuráveis no prompt do agente, não no código da aplicação.

## 3. Tools para acesso a artefatos e repositórios

Será necessário criar tools para permitir que os agentes:

- Acessem, criem e alterem artefatos no repositório de documentação do projeto.
- Acessem repositórios de código para entender o funcionamento atual da aplicação.
- Façam consultas por expressões como Glob e Grep.

### Requisitos para essas tools

- Ler arquivos e diretórios por padrão flexível de busca.
- Buscar conteúdo por texto e expressão regular.
- Navegar em múltiplos repositórios vinculados ao projeto (**por enquanto, apenas Git**).
- Permitir criação e atualização controlada de artefatos documentais.
- **Implementar rate limiting e paginação** para performance em repositórios grandes.
- **Suportar caching de busca** para padrões frequentes.
- **Incluir auditoria de operações** (quem acessou/modificou qual arquivo).

## 4. Workspace persistente para repositórios e artefatos

Atualmente somente o agente de codificação acessa arquivos locais, clonando repositórios em pastas temporárias e apagando após o processamento. Esse modelo precisa ser substituído.

### Novo modelo de workspace

- Será criado um workspace persistente para armazenar clones dos repositórios.
- O workspace não deve ser temporário e não deve ser apagado automaticamente ao final do trabalho do agente.
- Deve haver organização por projetos dentro desse workspace.
- **Retenção:** Por tempo (com possibilidade de funcionalidade de limpeza manual).
- **Isolamento:** Cada requisito em sua própria branch nos repositórios que sofrerão alterações.
- **Armazenamento de Artefatos:** No repositório Git, organizados em pastas por requisito/feature.

### Fluxo esperado dentro da pasta do projeto

Antes do início do trabalho de um agente:

1. Clonar os repositórios caso ainda não existam.
2. Fazer checkout para a branch de origem, quando necessário.
3. Fazer pull/fetch para atualizar os arquivos locais.
4. Para repositórios que poderão ser alterados, criar worktree vinculada a uma branch de feature associada ao requisito.

Ao final do trabalho:

- Implementar funcionalidade para commit, push e criação de pull request.
- Encerrar a worktree após a finalização do fluxo.

### Considerações de persistência

- Controle de permissões e isolamento entre projetos.
- Versionamento de artefatos gerados (backup e histórico).
- Sincronização eficiente entre repositórios remotos e cópias locais (fetch/pull).

## 5. Reestruturação do frontend

O frontend deverá ser reestruturado a partir do novo protótipo.

### Necessidades funcionais de interface

- Adequar telas e navegação ao novo fluxo com mais etapas.
- Expor os artefatos intermediários gerados por cada agente.
- Adaptar o acompanhamento do status para refletir execução assíncrona.
- Simplificar a interação do usuário com ações padronizadas de decisão.
- Permitir navegação livre do usuário enquanto processamento ocorre em background.
- Exibir histórico e rastreabilidade de versões de artefatos intermediários.

### Padronização dos botões por etapa

O conjunto atual de ações manuais deve ser substituído por três ações padrão:

- **Aprovar:** Automaticamente inicia a próxima etapa.
- **Reprovar:** Interrompe a esteira naquele ponto.
- **Solicitar alteração:** Usuário fornece feedback textual; agente reprocessa com novo prompt.

Essa padronização deve reduzir o número de controles por tela e tornar o fluxo mais previsível.

### Considerações de UX

- Implementar Server-Sent Events (SSE) ou WebSocket para notificações de progresso.
- UX intuitiva para ações padronizadas.

## 6. Execução assíncrona e status de processamento

Atualmente o frontend aguarda o término do trabalho do agente em uma tela de loading, prendendo a requisição HTTP. Para os novos agentes, isso precisa mudar.

### Necessidades de comportamento

- O trabalho dos agentes deve ser apenas iniciado pela aplicação.
- A execução deve ocorrer em background, sem bloquear o uso da interface.
- O frontend deve acompanhar o status de execução de forma desacoplada da requisição HTTP.
- **Múltiplas iterações permitidas** em cada etapa com feedback do usuário.

### Estados mínimos sugeridos

- Não iniciado.
- Em execução.
- Aguardando aprovação.
- Aguardando alteração solicitada.
- Aprovado.
- Reprovado.
- Concluído com erro.

### Fluxo de Aprovação

- **Modelo:** Aprovação simples por um único usuário (por enquanto).
- **Recuperação:** Em caso de falha, mantém estado; permite intervenção manual ou pular etapa se necessário.
- **Rastreabilidade:** Histórico completo de versões com metadados (quem, quando, por quê).

## 7. Fluxo funcional futuro consolidado

**Novas Etapas (Evolução):**

1. Usuário cria o requisito no chat de história de usuário.
2. Sistema gera e organiza o arquivo de requisitos.
3. Sistema gera critérios de acessibilidade.
4. Sistema produz o fluxo de navegação e a lista de telas do protótipo.
5. Sistema gera o modelo de dados conceitual, quando aplicável.
6. Sistema gera o Power Design e o modelo físico, quando houver escopo de dados.
7. Sistema gera e detalha artefatos de teste.
8. Usuário aprova, reprova ou solicita alteração em cada etapa.
9. A aprovação de uma etapa inicia automaticamente a próxima.
10. O processamento ocorre em background com acompanhamento de status.

**Etapas Existentes (Consolidação):**

11. Sistema gera o Plan baseado na Spec aprovada.
12. Sistema gera as Tasks a partir do Plan.
13. Equipe de desenvolvimento implementa o código seguindo as Tasks.
14. O processamento de Plan, Tasks e Codificação também ocorre com acompanhamento de status.

**Resultado Consolidado:**

- Fluxo completo da ideia (história de usuário) até a criação do PR com código funcional.
- Rastreabilidade end-to-end desde requisito até implementação.
- Redução significativa de retrabalho graças aos artefatos intermediários estruturados.

## 8. Observações de implantação

- A definição final dos contratos de entrada e saída de cada agente deve ser documentada nos prompts e na estrutura de conhecimento do repositório.
- **Concorrência:** Múltiplos usuários podem trabalhar no mesmo projeto simultaneamente; isolamento garantido por branch separada para cada requisito.
- **Auditoria:** Rastreabilidade completa e retenção de logs para compliance.
- **Sucesso:** Código funcional + atendimento a todos os critérios de aceite pré-definidos.

## 9. Principais Desafios Técnicos

### 9.1 Orquestração e Coordenação de Agentes

**Desafios:**

- Implementar fluxo sequencial de múltiplos agentes sem criar dependências rígidas.
- Garantir que falhas parciais em um agente não bloqueiem o fluxo inteiro.
- Estabelecer contrato claro de entrada e saída entre etapas consecutivas.
- Implementar rollback e recuperação de etapas em caso de erro.

**Considerações:**

- Necessidade de estado persistente para rastrear progresso em cada etapa.
- Mecanismo de retry e idempotência para operações críticas.

### 9.2 Execução Assíncrona e Gerenciamento de Background Jobs

**Desafios:**

- Migrar de modelo síncrono (HTTP com loading) para assíncrono (background processing).
- Implementar fila de jobs com suporte a priorização e agendamento.
- Garantir escalabilidade para múltiplas requisições simultâneas.
- Manter rastreabilidade de execução (logging e auditoria).

**Considerações:**

- Escolher tecnologia apropriada (message queue: RabbitMQ, Kafka, ou similar).
- Implementar retry logic com backoff exponencial.
- Monitoramento e alertas para jobs que falham ou expiram.

### 9.3 Persistência e Gerenciamento de Workspace

**Desafios:**

- Manter workspace persistente sem consumir armazenamento excessivo.
- Implementar estratégia de cleanup para repositórios antigos ou obsoletos.
- Garantir acesso concorrente seguro a arquivos compartilhados.
- Sincronização eficiente entre repositórios remotos e cópias locais (fetch/pull).

**Considerações:**

- Uso de git worktrees para isolamento de branches por requisito.
- Controle de permissões e isolamento entre projetos.
- Versionamento de artefatos gerados (backup e histórico).

### 9.4 Geração de Artefatos Específicos de Domínio

**Desafios:**

- **Power Design XML:** Mapear modelo conceitual para XML válido segundo padrões internos da Caixa.
- **DDL Multi-SGBD:** Gerar scripts SQL corretos e otimizados para SQL Server, Oracle, DB2, PostgreSQL e Sybase.
- **Testes Estruturados:** Criar casos de teste bem formados com cobertura completa (positivo, negativo, exceção).
- **Acessibilidade:** Incorporar critérios WCAG e normas brasileiras de acessibilidade.

**Considerações:**

- Validação de artefatos gerados antes de persisti-los.
- Base de conhecimento específica e prompts calibrados para cada tipo de artefato.
- Testes de compatibilidade com ferramentas downstream (Power Design, geradores de DDL, etc.).

### 9.5 Integração com LLMs e Gerenciamento de Prompts

**Desafios:**

- Organizar e versionar prompts para cada agente de forma modular e reutilizável.
- Calibrar temperatura, tokens máximos e parâmetros de geração para cada tipo de tarefa.
- Implementar mecanismo de feedback para melhoria contínua de prompts.
- Gerenciar custos de chamadas de API e implementar rate limiting.

**Considerações:**

- Estrutura de diretório dedicada no repositório de conhecimento para prompts.
- Testes de regressão de qualidade para alterações em prompts.
- Observabilidade de chamadas (latência, custo, qualidade de output).

### 9.6 Tools para Acesso a Artefatos e Repositórios

**Desafios:**

- Implementar tools seguras para leitura/escrita de arquivos (Glob, Grep, File I/O).
- Validar acesso conforme permissões e escopo do projeto.
- Garantir performance em repositórios grandes (filtros eficientes).
- Suportar múltiplos formatos de busca (regex, glob, full-text search).

**Considerações:**

- Rate limiting e paginação de resultados para evitar overhead.
- Caching de busca para padrões frequentes.
- Auditoria de operações (quem acessou/modificou qual arquivo).

### 9.7 Interface de Usuário com Status em Tempo Real

**Desafios:**

- Implementar polling ou WebSocket para atualização de status sem bloquear a interface.
- Sincronizar múltiplos clientes com estado consistente do servidor.
- Permitir navegação livre do usuário enquanto processamento ocorre em background.
- Exibir histórico e rastreabilidade de versões de artefatos intermediários.

**Considerações:**

- Implementar Server-Sent Events (SSE) ou WebSocket para notificações de progresso.
- Cache local no frontend para melhorar responsividade.
- Ux intuitiva para ações padronizadas (Aprovar, Reprovar, Solicitar Alteração).

### 9.8 Controle de Versão e Artefatos

**Desafios:**

- Manter histórico completo de versões de cada artefato.
- Permitir rollback seguro a versões anteriores.
- Implementar diff e merge para comparação de iterações.
- Determinar quando criar nova versão vs. sobrescrever versão atual.

**Considerações:**

- Estratégia de versionamento semântico ou timestamp-based.
- Armazenamento eficiente (delta compression).
- Rastreabilidade de quem/quando/por quê uma versão foi criada.

### 9.9 Tratamento de Erros e Validação

**Desafios:**

- Implementar validação rigorosa de inputs para cada agente.
- Detectar e relatar inconsistências entre artefatos (ex: requisito vs. modelo de dados).
- Criar mensagens de erro clara e acionáveis para o usuário.
- Implementar recuperação graceful com sugestões de correção.

**Considerações:**

- Schemas de validação (JSON Schema, OpenAPI) para contrato de agentes.
- Testes de validação automatizados.
- Logging estruturado para debugging.

### 9.10 Escalabilidade e Performance

**Desafios:**

- Garantir que sistema escale com crescimento de usuários e projetos.
- Otimizar tempo de geração de artefatos complexos (modelos de dados, testes).
- Monitorar e alertar sobre degradação de performance.
- Implementar caching inteligente para artefatos reutilizáveis.

**Considerações:**

- Arquitetura horizontal com múltiplas instâncias de worker.
- Database indexing e query optimization.
- Profiling de operações críticas durante desenvolvimento.
- SLA (Service Level Agreement) para tempo de processamento por tipo de artefato.