# Backlog de Tarefas - Evoluções da Fábrica SDD

Este backlog organiza as evoluções necessárias da próxima versão da fábrica SDD em épicos e tarefas implementáveis. A priorização considera dependências entre etapas e o impacto no fluxo principal do produto.

## Objetivo da evolução

Evoluir a esteira atual de `Story -> Spec -> Plan -> Task -> Impl` para um fluxo ampliado e sequencial com novos agentes intermediários, novos artefatos persistidos, adaptação das etapas existentes e suporte a workspace/repositórios persistentes.

## Épico 1 - Nova esteira de agentes entre Chat e Spec

### 1.1 Estruturar o novo fluxo sequencial de artefatos
- Definir o fluxo oficial com as etapas: Chat, Requisitos, Acessibilidade, Prototipação, Modelo Conceitual, Power Design, Modelo Físico, Estruturador de Testes, Modelador de Testes de Qualidade, Modelador de Testes de Desenvolvimento e Spec.
- Definir o contrato de entrada e saída de cada etapa.
- Garantir que cada etapa produza um artefato persistente e reutilizável pela próxima.

### 1.2 Implementar o Agente de Requisitos
- Criar o prompt e a estrutura do artefato de requisitos.
- Consumir a saída do chat de história de usuário.
- Persistir o documento de requisitos em formato padronizado.
- Expor a etapa na interface e no backend.

### 1.3 Implementar o Agente de Acessibilidade
- Criar base de conhecimento em Markdown para critérios de acessibilidade.
- Gerar critérios objetivos a partir da história de usuário e requisitos funcionais.
- Persistir os critérios gerados para consumo da prototipação.
- Permitir revisão e aprovação do artefato.

### 1.4 Implementar o Agente de Prototipação
- Criar o prompt para gerar fluxo de navegação textual.
- Receber história de usuário, acessibilidade e dispositivo alvo.
- Produzir caminho feliz, erros, exceções, telas, componentes e critérios por etapa.
- Persistir o protótipo textual e disponibilizá-lo no workflow.

### 1.5 Implementar o Modelo de Dados Conceitual
- Criar o prompt para extração de entidades, atributos, relacionamentos e cardinalidades.
- Suportar insumos como história de usuário, transcrições, glossário e regras de negócio.
- Gerar DER conceitual em Markdown com premissas e dúvidas.
- Persistir o artefato para uso posterior em modelagem e planificação.

### 1.6 Implementar o Power Design
- Criar a etapa para gerar XML de modelagem compatível com o padrão da Caixa.
- Consumir o modelo conceitual como entrada principal.
- Persistir o XML gerado como artefato de projeto.

### 1.7 Implementar o Modelo de Dados Físico
- Criar o prompt e a estrutura para derivar modelo físico por SGBD.
- Suportar SQL Server, Oracle, DB2, PostgreSQL e Sybase em manutenção.
- Gerar script DDL com tabelas, colunas, chaves, índices e constraints.
- Registrar inconsistências e lacunas encontradas na conversão.

### 1.8 Implementar o Estruturador de Testes
- Criar fluxo para receber conteúdo textual completo ou arquivo anexado.
- Gerar suítes de teste funcionais e negociais com cenários positivos, negativos e de exceção.
- Organizar os testes por funcionalidade e fluxo crítico.
- Definir comportamento de solicitação de complementação quando o conteúdo estiver incompleto.

### 1.9 Implementar o Modelador de Testes de Qualidade
- Receber suítes de teste já estruturadas.
- Converter casos em roteiro de testes detalhado e consistente.
- Garantir rastreabilidade entre requisito, suíte e caso.

### 1.10 Definir o Modelador de Testes de Desenvolvimento
- Levantar a finalidade funcional da etapa com stakeholders.
- Decidir se a etapa será voltada a testes automatizados, técnicos ou de apoio ao desenvolvimento.
- Definir escopo para evitar sobreposição com as demais etapas de teste.

## Épico 2 - Adaptação das etapas existentes

### 2.1 Ajustar a etapa de Spec
- Adaptar o consumo de artefatos refinados vindos da nova esteira.
- Padronizar ações para `Aprovar`, `Reprovar` e `Solicitar Alteração`.
- Atualizar backend e frontend para refletir o novo contrato de entrada e saída.

### 2.2 Ajustar a etapa de Plan
- Fazer a etapa receber Spec aprovada, modelo conceitual, protótipo e testes estruturados.
- Padronizar ações para `Aprovar`, `Reprovar` e `Solicitar Alteração`.
- Atualizar geração do documento de plan com base nos novos artefatos.

### 2.3 Ajustar a etapa de Tasks
- Fazer a etapa consumir Plan aprovado, Spec, modelo de dados e testes.
- Padronizar ações para `Aprovar`, `Reprovar` e `Solicitar Alteração`.
- Garantir rastreabilidade entre tarefas e artefatos de origem.

### 2.4 Ajustar a etapa de Codificação
- Alterar o fluxo para aprovação via Pull Request no GitHub.
- Fazer a etapa consumir Tasks, Spec, protótipo, modelo de dados e testes.
- Ajustar a interface e o backend para refletir a aprovação externa por PR.

## Épico 3 - Repositório de conhecimento e organização por agentes

### 3.1 Organizar conhecimento por agente
- Criar estrutura de pastas por agente no repositório de conhecimento.
- Separar prompts, bases Markdown, exemplos e instruções por contexto de uso.
- Permitir evolução independente de cada agente.

### 3.2 Permitir configuração de modelo LLM por agente
- Expor configuração de modelo por agente via properties do sistema.
- Garantir que diferentes agentes possam usar LLMs distintos.
- Validar fallback e comportamento padrão quando a configuração estiver ausente.

## Épico 4 - Tools para acesso a artefatos e repositórios

Este épico é **fundacional** para a nova esteira: os agentes dependem dessas tools para ler contexto, criar artefatos, atualizar documentos e navegar em múltiplos repositórios com segurança e rastreabilidade.

### 4.1 Criar tools de leitura e navegação de arquivos
- Implementar leitura de arquivos por caminho e por padrão flexível de busca.
- Suportar navegação em diretórios e consulta de estrutura.
- Permitir uso pelos agentes para consultar artefatos de documentação.

### 4.2 Criar tools de busca textual e regex
- Implementar busca por texto e expressão regular.
- Suportar padrões Glob e Grep.
- Permitir busca em múltiplos repositórios vinculados ao projeto.

### 4.3 Criar tools de criação e atualização controlada
- Permitir criação e atualização de artefatos documentais.
- Restringir alterações ao contexto do agente e do projeto.
- Registrar alterações com rastreabilidade.

### 4.4 Implementar paginação e rate limiting
- Adicionar paginação para resultados volumosos.
- Implementar rate limiting para repositórios grandes.
- Garantir que a experiência de busca continue estável sob carga.

### 4.5 Implementar auditoria de operações
- Registrar quem acessou ou modificou cada arquivo.
- Tornar auditável a leitura, criação e atualização de artefatos.
- Expor logs compatíveis com análise operacional e rastreabilidade.

## Épico 5 - Workspace persistente para repositórios e artefatos

### 5.1 Criar workspace persistente
- Substituir o modelo atual de clones temporários por workspace persistente.
- Organizar os repositórios por projeto dentro do workspace.
- Garantir retenção por tempo e possibilidade de limpeza manual.

### 5.2 Implementar fluxo de atualização de repositórios
- Clonar repositórios quando ainda não existirem no workspace.
- Fazer checkout na branch de origem quando necessário.
- Executar pull/fetch para manter o conteúdo atualizado.

### 5.3 Criar isolamento por branch e worktree
- Criar branch por requisito para repositórios alteráveis.
- Associar worktrees às branches de feature.
- Garantir isolamento entre execuções paralelas ou independentes.

### 5.4 Persistir artefatos no Git
- Organizar artefatos em pastas por requisito ou feature.
- Definir convenção de nomes e estrutura de diretórios.
- Garantir persistência e versionamento dos artefatos gerados pelos agentes.

## Épico 6 - Ajustes de UI e jornada do usuário

### 6.1 Exibir novas etapas no painel de requisitos
- Adicionar colunas e status para as novas etapas intermediárias.
- Atualizar o acompanhamento do fluxo completo até a Spec.
- Garantir consistência visual com as etapas já existentes.

### 6.2 Criar telas para novos agentes
- Criar telas de cadastro/edição para os novos artefatos.
- Manter padrão de edição, salvamento, aprovação e exclusão quando aplicável.
- Disponibilizar acesso aos prompts específicos de cada etapa.

### 6.3 Atualizar navegação do fluxo
- Ajustar a navegação sequencial entre as etapas novas e existentes.
- Tratar dependências entre aprovação, geração e revisão.
- Reduzir ações manuais redundantes no caminho principal.

## Épico 7 - Infraestrutura, governança e evolução técnica

### 7.1 Definir modelo de persistência para novos artefatos
- Mapear entidades, estados e relacionamentos necessários.
- Atualizar o banco e os repositórios de dados da aplicação.
- Garantir compatibilidade com o workflow atual.

### 7.2 Revisar contratos de API
- Atualizar endpoints para suportar os novos artefatos e estados.
- Padronizar respostas e erros das novas etapas.
- Manter compatibilidade com o frontend durante a transição.

### 7.3 Criar estratégia de migração incremental
- Definir a ordem de implementação por dependência funcional.
- Separar entregas em fatias pequenas e validáveis.
- Planejar compatibilidade temporária entre fluxo antigo e fluxo novo.

## Prioridade sugerida

### Fase 1 - Fundacional
- Tools de acesso a artefatos e repositórios
- Paginação, rate limiting e auditoria das tools
- Workspace persistente
- Organização de conhecimento por agente
- Contratos básicos de artefatos
- Ajustes de persistência e APIs

### Fase 2 - Nova esteira
- Requisitos
- Acessibilidade
- Prototipação
- Modelo conceitual
- Power Design
- Modelo físico
- Estruturador de testes
- Modelador de testes

### Fase 3 - Adequações do fluxo existente
- Spec
- Plan
- Tasks
- Codificação via PR

### Fase 4 - Plataforma de suporte e otimização
- Cache
- Hardening de performance em produção
- Observabilidade e tuning operacional das tools

### Fase 5 - Evolução de UX
- Ajustes de telas
- Nova navegação
- Painéis de acompanhamento

## Observação

O item **Modelador de Testes de Desenvolvimento** permanece como backlog aberto de definição funcional e deve ser fechado antes do início da implementação dessa etapa.
