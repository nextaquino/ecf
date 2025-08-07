# Database Setup - Sistema Escola Construindo o Futuro

Este documento detalha a configuração e estrutura do banco de dados PostgreSQL hospedado no Neon para o sistema de gestão escolar.

## 🔧 Configuração do Banco

### Neon PostgreSQL
- **Provider**: Neon (Serverless PostgreSQL)
- **Região**: South America (São Paulo)
- **Versão**: PostgreSQL 15+
- **Connection**: Via CONNECTION_STRING no arquivo .env

### Variáveis de Ambiente
```bash
# .env
DATABASE_URL="postgresql://neondb_owner:npg_5uNEqp9SjDto@ep-fragrant-shadow-acvgwh6c-pooler.sa-east-1.aws.neon.tech/neondb?sslmode=require"
```

### Prisma Configuration
O projeto utiliza Prisma ORM para gerenciar o banco de dados:

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

## 🗄️ Estrutura do Banco

### Entidades Principais

#### 1. User (Usuários do Sistema)
- **Propósito**: Autenticação e autorização
- **Relacionamentos**: Staff (1:1), AuditLogs (1:N)
- **Roles**: ADMIN, TEACHER, GUARDIAN

```sql
CREATE TABLE "User" (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR UNIQUE NOT NULL,
  name VARCHAR NOT NULL,
  role UserRole NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

#### 2. Student (Alunos)
- **Propósito**: Gestão de alunos matriculados
- **Relacionamentos**: Guardian (N:N), Class (N:N), Payment (1:N)
- **Features**: Upload de foto, dados pessoais, histórico acadêmico

```sql
CREATE TABLE "Student" (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR NOT NULL,
  birth_date DATE NOT NULL,
  cpf VARCHAR UNIQUE,
  photo_url VARCHAR,
  enrollment_date TIMESTAMP DEFAULT NOW(),
  status StudentStatus DEFAULT 'ACTIVE',
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

#### 3. Class (Turmas)
- **Propósito**: Organização de turmas por série
- **Relacionamentos**: Staff/Teacher (N:1), Student (N:N)
- **Features**: Controle de capacidade, ano letivo

```sql
CREATE TABLE "Class" (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR NOT NULL,
  grade Grade NOT NULL,
  max_capacity INTEGER NOT NULL,
  academic_year INTEGER NOT NULL,
  teacher_id VARCHAR REFERENCES "Staff"(id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

#### 4. Staff (Funcionários)
- **Propósito**: Gestão de professores e funcionários
- **Relacionamentos**: User (1:1), Class (1:N)
- **Features**: Hierarquia organizacional, permissões

```sql
CREATE TABLE "Staff" (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR NOT NULL,
  cpf VARCHAR UNIQUE NOT NULL,
  email VARCHAR UNIQUE NOT NULL,
  phone VARCHAR NOT NULL,
  position StaffPosition NOT NULL,
  hire_date DATE NOT NULL,
  salary DECIMAL(10,2),
  user_id VARCHAR UNIQUE REFERENCES "User"(id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

#### 5. Payment (Pagamentos)
- **Propósito**: Controle financeiro de mensalidades
- **Relacionamentos**: Student (N:1)
- **Features**: Status de pagamento, histórico financeiro

```sql
CREATE TABLE "Payment" (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  amount DECIMAL(10,2) NOT NULL,
  due_date DATE NOT NULL,
  paid_date DATE,
  status PaymentStatus DEFAULT 'PENDING',
  description TEXT NOT NULL,
  student_id VARCHAR NOT NULL REFERENCES "Student"(id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### Enums e Tipos

```sql
CREATE TYPE "UserRole" AS ENUM ('ADMIN', 'TEACHER', 'GUARDIAN');
CREATE TYPE "StudentStatus" AS ENUM ('ACTIVE', 'INACTIVE', 'TRANSFERRED');
CREATE TYPE "Grade" AS ENUM ('MATERNAL', 'PRE_1', 'PRE_2', 'FIRST_GRADE', 'SECOND_GRADE', 'THIRD_GRADE', 'FOURTH_GRADE', 'FIFTH_GRADE');
CREATE TYPE "StaffPosition" AS ENUM ('TEACHER', 'COORDINATOR', 'DIRECTOR', 'ADMIN', 'SUPPORT');
CREATE TYPE "PaymentStatus" AS ENUM ('PENDING', 'PAID', 'OVERDUE', 'CANCELLED');
```

### Índices para Performance

```sql
-- Índices para otimização de queries frequentes
CREATE INDEX idx_student_name ON "Student"(name);
CREATE INDEX idx_student_status ON "Student"(status);
CREATE INDEX idx_student_enrollment_date ON "Student"(enrollment_date);
CREATE INDEX idx_payment_due_date ON "Payment"(due_date);
CREATE INDEX idx_payment_status ON "Payment"(status);
CREATE INDEX idx_class_grade ON "Class"(grade);
CREATE INDEX idx_class_academic_year ON "Class"(academic_year);
```

## 🚀 Comandos de Setup

### Instalação e Configuração

```bash
# 1. Instalar Prisma
npm install prisma @prisma/client

# 2. Inicializar Prisma (já feito)
npx prisma init

# 3. Configurar schema.prisma (já configurado)

# 4. Gerar migrations
npx prisma migrate dev --name init

# 5. Gerar client Prisma
npx prisma generate

# 6. Seed do banco (opcional)
npx prisma db seed
```

### Scripts NPM Configurados

```json
{
  "scripts": {
    "db:migrate": "prisma migrate dev",
    "db:generate": "prisma generate",
    "db:studio": "prisma studio",
    "db:reset": "prisma migrate reset",
    "db:seed": "prisma db seed"
  }
}
```

## 🔒 Segurança

### Medidas Implementadas
- **SSL/TLS**: Conexão criptografada obrigatória
- **Connection Pooling**: Gerenciado pelo Neon
- **Environment Variables**: Credenciais em .env
- **Prisma Client**: ORM com proteção contra SQL injection
- **Audit Logging**: Rastreamento de operações sensíveis

### Backup e Recuperação
- **Automated Backups**: Neon realiza backups automáticos
- **Point-in-time Recovery**: Disponível via dashboard Neon
- **Data Export**: Via `pg_dump` quando necessário

## 📊 Monitoramento

### Métricas Importantes
- **Connection Pool Usage**: Monitorar via Neon Dashboard
- **Query Performance**: Logs de queries lentas
- **Storage Usage**: Acompanhar crescimento do banco
- **Error Rates**: Monitorar falhas de conexão

### Ferramentas de Monitoramento
- **Neon Console**: Dashboard principal
- **Prisma Studio**: Interface visual para dados
- **Application Logs**: Logs da aplicação Next.js

## 🔧 Manutenção

### Rotinas Recomendadas
- **Migrations**: Sempre testar em ambiente de desenvolvimento
- **Schema Updates**: Usar migrations versionadas
- **Data Cleanup**: Rotinas de limpeza de dados antigos
- **Performance Review**: Análise periódica de queries

### Troubleshooting Comum
1. **Connection Issues**: Verificar DATABASE_URL e firewall
2. **Migration Errors**: Reset do banco em desenvolvimento
3. **Performance Issues**: Análise de queries e índices
4. **Schema Conflicts**: Resolver conflitos de migration

## 📝 Próximos Passos

1. **✅ Implementar schema completo do Prisma**
2. **🔄 Criar seeds para dados de teste**
3. **📋 Implementar audit logging**
4. **🚀 Setup de ambiente de produção**
5. **📊 Configurar monitoramento avançado**