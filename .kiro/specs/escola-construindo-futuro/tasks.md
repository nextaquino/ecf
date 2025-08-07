# Lista de Tarefas Detalhada - Sistema Escola Construindo o Futuro

## Fase 1: Setup e Configuração Base

### ✅ Completadas
- [x] **Configuração inicial do projeto Next.js 15**
  - Criação do projeto com `create-next-app`
  - Configuração do App Router
  - Setup inicial de estrutura de pastas
  - Instalação de dependências base

- [x] **Setup do TypeScript e configurações base**
  - Configuração do tsconfig.json com strict mode
  - Setup de tipos para Next.js
  - Configuração de paths absolutos (@/)
  - Integração com ESLint para TypeScript

- [x] **Instalação e configuração do Tailwind CSS**
  - Instalação do Tailwind CSS v4+
  - Configuração do tailwind.config.js
  - Setup de classes utilitárias customizadas
  - Integração com PostCSS

- [x] **Setup do shadcn/ui**
  - Instalação e configuração do shadcn/ui
  - Setup do components.json
  - Instalação de componentes base (Button, Input, etc.)
  - Configuração de temas e variáveis CSS

- [x] **Configuração do Prisma ORM**
  - Instalação do Prisma
  - Setup do schema.prisma inicial
  - Configuração de conexão com banco
  - Setup de scripts de migration

- [x] **Configuração do banco Neon PostgreSQL**
  - Criação da conta no Neon
  - Setup da database
  - Configuração de connection strings
  - Teste de conectividade

- [x] **Setup do Stack Auth para autenticação**
  - Instalação do Stack Auth
  - Configuração inicial de providers
  - Setup de variáveis de ambiente
  - Integração com Next.js

- [x] **Configuração de variáveis de ambiente**
  - Criação dos arquivos .env
  - Setup de variáveis de desenvolvimento
  - Configuração de variáveis de produção
  - Documentação no .env.example

- [x] **Setup do ESLint e Prettier**
  - Configuração do ESLint para Next.js e TypeScript
  - Setup do Prettier com regras customizadas
  - Integração entre ESLint e Prettier
  - Configuração de scripts npm

- [x] **Configuração do Husky para git hooks**
  - Instalação e configuração do Husky
  - Setup de pre-commit hooks
  - Configuração do lint-staged
  - Validação de commits

### 🚧 Em Progresso
- [ ] **Implementação dos componentes base de UI**
  - Criação de componentes customizados baseados no shadcn/ui
  - Setup de componentes de layout (Header, Sidebar, Footer)
  - Implementação de componentes de formulário
  - Criação de componentes de tabela com paginação

- [ ] **Criação do layout principal do dashboard**
  - Implementação do DashboardLayout component
  - Setup de navegação responsiva
  - Criação de breadcrumbs
  - Implementação de sistema de notificações

- [ ] **Setup da estrutura de roteamento**
  - Organização de rotas do App Router
  - Criação de grupos de rotas (auth), (dashboard)
  - Setup de rotas protegidas
  - Implementação de middleware de roteamento

## Fase 2: Autenticação e Autorização

### 📋 Pendentes
- [ ] **Implementação da página de login**
  - Criação da página /login com formulário responsivo
  - Implementação de validação client-side e server-side
  - Integração com Stack Auth providers
  - Tratamento de erros de autenticação
  - Redirect após login bem-sucedido

- [ ] **Configuração completa do Stack Auth**
  - Setup de providers de autenticação (email, Google, etc.)
  - Configuração de callbacks e redirects
  - Setup de sessões e tokens JWT
  - Configuração de refresh tokens
  - Implementação de logout

- [ ] **Sistema de roles e permissões (RBAC)**
  - Definição de roles: ADMIN, TEACHER, GUARDIAN
  - Criação de sistema de permissões granulares
  - Middleware para verificação de permissões
  - HOCs para proteção de componentes
  - Database schema para roles e permissions

- [ ] **Middleware de autenticação**
  - Implementação de middleware.ts para Next.js
  - Proteção de rotas baseada em roles
  - Redirecionamento para login quando não autenticado
  - Verificação de tokens e sessões
  - Rate limiting por usuário

- [ ] **Páginas de registro e recuperação de senha**
  - Página de registro com validação de dados
  - Processo de verificação de email
  - Página de recuperação de senha
  - Implementação de reset de senha
  - Templates de email customizados

- [ ] **Proteção de rotas sensíveis**
  - Implementação de route guards
  - Proteção de API routes
  - Middleware para verificação de permissões
  - Error boundaries para erros de autorização
  - Audit log para tentativas de acesso

## Fase 3: Gestão de Alunos

### 📋 Pendentes
- [ ] **Schema do banco para entidade Student**
  - Criação do model Student no Prisma
  - Definição de relacionamentos (Guardian, Class, Payment)
  - Índices para otimização de queries
  - Campos obrigatórios e opcionais
  - Validações a nível de banco

- [ ] **Server Actions para CRUD de alunos**
  - createStudent: Validação e criação de novo aluno
  - updateStudent: Atualização de dados do aluno
  - deleteStudent: Soft delete com audit trail
  - getStudent: Busca de aluno por ID
  - searchStudents: Busca com filtros e paginação

- [ ] **Formulário de cadastro de alunos**
  - Formulário multi-step para dados completos
  - Validação em tempo real com Zod
  - Campos para dados pessoais, responsáveis, emergência
  - Upload de foto com preview
  - Auto-save de progresso

- [ ] **Página de listagem de alunos com filtros**
  - Tabela responsiva com paginação server-side
  - Filtros por nome, turma, série, status
  - Ordenação por diferentes campos
  - Busca instantânea (debounced)
  - Ações em lote (ativar/desativar múltiplos)

- [ ] **Página de detalhes do aluno**
  - Visualização completa de dados do aluno
  - Histórico acadêmico e ocorrências
  - Timeline de atividades
  - Documentos anexados
  - Botões de ação (editar, imprimir, etc.)

- [ ] **Funcionalidade de upload de foto**
  - Upload com drag & drop
  - Validação de tipo e tamanho de arquivo
  - Redimensionamento automático de imagens
  - Armazenamento seguro (S3 ou similar)
  - Fallback para avatar padrão

- [ ] **Sistema de busca avançada**
  - Busca por múltiplos critérios simultaneamente
  - Filtros salvos e reutilizáveis
  - Exportação de resultados
  - Busca por responsáveis e contatos
  - Histórico de buscas realizadas

- [ ] **Validações com Zod**
  - Schemas de validação para todos os campos
  - Validações customizadas (CPF, telefone, etc.)
  - Mensagens de erro user-friendly
  - Validação server-side em Server Actions
  - Type safety com TypeScript

## Fase 4: Gestão de Turmas

### 📋 Pendentes
- [ ] **Schema do banco para entidade Class**
  - Model Class com campos grade, name, maxCapacity
  - Relacionamento com Teacher (Staff) e Students
  - Enum para séries (MATERNAL, PRE_1, etc.)
  - Campo academicYear para controle anual
  - Índices para queries de performance

- [ ] **Server Actions para CRUD de turmas**
  - createClass: Criação com validação de capacidade
  - updateClass: Atualização com verificação de conflitos
  - deleteClass: Verificação de alunos matriculados
  - getClasses: Listagem com filtros e paginação
  - assignTeacher: Atribuição de professor à turma

- [ ] **Formulário de criação de turmas**
  - Seleção de série/nível educacional
  - Definição de capacidade máxima
  - Atribuição de professor responsável
  - Configuração de horários e sala
  - Validação de conflitos de horário

- [ ] **Página de listagem de turmas**
  - Visualização em cards ou tabela
  - Filtros por série, professor, ano letivo
  - Indicadores de ocupação (% de capacidade)
  - Status das turmas (ativa, inativa, completa)
  - Ações rápidas (editar, ver alunos, relatórios)

- [ ] **Sistema de atribuição de professores**
  - Interface para atribuir/trocar professores
  - Validação de disponibilidade do professor
  - Histórico de professores por turma
  - Notificações automáticas de mudanças
  - Controle de conflitos de horário

- [ ] **Controle de capacidade das turmas**
  - Validação automática na matrícula de alunos
  - Alertas quando próximo da capacidade máxima
  - Lista de espera para turmas lotadas
  - Relatórios de ocupação por turma
  - Sugestões de redistribuição de alunos

- [ ] **Relacionamento aluno-turma**
  - Sistema de matrícula de alunos em turmas
  - Histórico de turmas por aluno
  - Transferência entre turmas
  - Controle de período de matrícula
  - Relatórios de movimentação de alunos

## Fase 5: Gestão de Funcionários

### 📋 Pendentes
- [ ] **Schema do banco para entidade Staff**
  - Model Staff com dados pessoais e profissionais
  - Enum para positions (TEACHER, ADMIN, COORDINATOR)
  - Relacionamento com User para autenticação
  - Campos de salário, data de contratação
  - Sistema de hierarquia organizacional

- [ ] **Server Actions para CRUD de funcionários**
  - createStaff: Cadastro com validação de CPF único
  - updateStaff: Atualização com audit trail
  - deactivateStaff: Desativação em vez de exclusão
  - getStaffMembers: Listagem com filtros
  - getStaffHierarchy: Estrutura organizacional

- [ ] **Formulário de cadastro de funcionários**
  - Dados pessoais (nome, CPF, email, telefone)
  - Informações profissionais (cargo, salário, supervisor)
  - Data de contratação e documentos
  - Foto funcional e assinatura digital
  - Permissões e acessos no sistema

- [ ] **Página de listagem de funcionários**
  - Tabela com informações principais
  - Filtros por cargo, departamento, status
  - Busca por nome, CPF ou email
  - Ordenação por nome, cargo, data de contratação
  - Ações (editar, desativar, visualizar perfil)

- [ ] **Sistema de hierarquia organizacional**
  - Organograma visual interativo
  - Definição de supervisores e subordinados
  - Níveis hierárquicos configuráveis
  - Relatórios por departamento
  - Fluxos de aprovação baseados na hierarquia

- [ ] **Controle de permissões por cargo**
  - Matriz de permissões por role/cargo
  - Interface para gerenciar permissões
  - Herança de permissões por hierarquia
  - Audit log de mudanças de permissão
  - Validação de acesso em tempo real

## Fase 6: Dashboard e Métricas

### 📋 Pendentes
- [ ] **Implementação do dashboard principal**
  - Layout responsivo com widgets configuráveis
  - Resumo executivo com KPIs principais
  - Gráficos de tendências e comparativos
  - Filtros por período (mês, trimestre, ano)
  - Personalização por role do usuário

- [ ] **Gráficos e estatísticas com Recharts**
  - Gráficos de barras para distribuição por turmas
  - Gráficos de linha para evolução temporal
  - Gráficos de pizza para proporções
  - Dashboards interativos com drill-down
  - Exportação de gráficos para relatórios

- [ ] **Métricas de alunos matriculados**
  - Total de alunos ativos por período
  - Distribuição por série/faixa etária
  - Taxa de crescimento de matrículas
  - Análise de sazonalidade
  - Comparativo com anos anteriores

- [ ] **Métricas de turmas e ocupação**
  - Taxa de ocupação por turma e série
  - Número médio de alunos por turma
  - Turmas com vagas disponíveis
  - Eficiência de uso de salas
  - Projeções de demanda futura

- [ ] **Indicadores financeiros básicos**
  - Receita total e por período
  - Taxa de inadimplência
  - Previsão de recebimentos
  - Comparativo orçado vs realizado
  - Principais fontes de receita

- [ ] **Widgets de alertas e notificações**
  - Alertas de pagamentos em atraso
  - Notificações de eventos próximos
  - Lembretes de tarefas pendentes
  - Alertas de capacidade de turmas
  - Dashboard de status do sistema

## Fase 7: Sistema Financeiro

### 📋 Pendentes
- [ ] Schema do banco para entidade Payment
- [ ] Server Actions para gestão financeira
- [ ] Formulário de registro de pagamentos
- [ ] Página de controle de mensalidades
- [ ] Sistema de alertas de inadimplência
- [ ] Geração de recibos
- [ ] Relatórios financeiros

## Fase 8: Calendário e Atividades

### 📋 Pendentes
- [ ] Schema do banco para entidades Event e Activity
- [ ] Implementação do calendário interativo
- [ ] Server Actions para eventos e atividades
- [ ] Formulário de criação de eventos
- [ ] Sistema de notificações
- [ ] Gestão de atividades acadêmicas

## Fase 9: Relatórios e Análises

### 📋 Pendentes
- [ ] Sistema de geração de relatórios
- [ ] Relatórios de alunos e turmas
- [ ] Relatórios financeiros detalhados
- [ ] Análises de desempenho escolar
- [ ] Exportação para PDF/Excel
- [ ] Agendamento de relatórios

## Fase 10: Melhorias e Otimizações

### 📋 Pendentes
- [ ] Implementação de testes unitários
- [ ] Testes de integração
- [ ] Otimizações de performance
- [ ] Implementação de cache
- [ ] SEO e meta tags
- [ ] Progressive Web App (PWA)
- [ ] Acessibilidade completa (WCAG 2.1)

## Fase 11: Segurança e Conformidade

### 📋 Pendentes
- [ ] Auditoria de segurança
- [ ] Sistema de logs de auditoria
- [ ] Implementação LGPD
- [ ] Backup e recuperação
- [ ] Monitoramento e alertas
- [ ] Rate limiting

## Fase 12: Deploy e Produção

### 📋 Pendentes
- [ ] Configuração do ambiente de produção
- [ ] Setup do CI/CD
- [ ] Deploy na Vercel/AWS
- [ ] Configuração de domínio
- [ ] Monitoramento de aplicação
- [ ] Documentação final

## Notas Importantes

- **Prioridade**: Focar nas Fases 1-6 para MVP
- **Testing**: Implementar testes desde o início
- **Performance**: Monitorar Core Web Vitals
- **Security**: Validação rigorosa em todas as camadas
- **UX/UI**: Design responsivo e acessível
- **Documentation**: Manter documentação atualizada