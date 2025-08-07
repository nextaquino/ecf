# Documento de Design - Sistema Escola Construindo o Futuro

## Visão Geral

O sistema "Escola Construindo o Futuro" será desenvolvido como uma aplicação web moderna utilizando Next.js 15+ com App Router, seguindo princípios de Clean Architecture. A aplicação será responsiva, acessível e otimizada para performance, atendendo instituições de ensino do maternal ao ensino fundamental 1.

### Stack Tecnológica

- **Frontend**: Next.js 15+ (App Router), TypeScript, Tailwind CSS
- **Componentes**: shadcn/ui como biblioteca base
- **Backend**: Server Actions e Server Components
- **Banco de Dados**: Neon PostgreSQL com Prisma ORM
- **Autenticação**: Stack Auth
- **Validação**: Zod
- **Ícones**: Lucide React

## Arquitetura

### Estrutura de Diretórios

```
src/
├── app/                          # Next.js App Router
│   ├── (auth)/                   # Grupo de rotas de autenticação
│   ├── dashboard/                # Rotas do dashboard
│   ├── api/                      # API Routes (quando necessário)
│   ├── layout.tsx
│   └── page.tsx
├── components/                   # Componentes reutilizáveis
│   ├── ui/                      # shadcn/ui components
│   ├── forms/                   # Formulários específicos
│   └── tables/                  # Tabelas de dados
├── lib/                         # Utilitários e configurações
│   ├── db/                      # Configuração do banco
│   ├── auth/                    # Configuração de autenticação
│   └── validations/             # Schemas Zod
├── types/                       # Definições de tipos TypeScript
└── constants/                   # Constantes da aplicação
```

### Server Actions

O sistema utilizará Server Actions como principal método de interação com o servidor:

```typescript
'use server'

import { revalidatePath } from 'next/cache'
import { z } from 'zod'

export async function createStudent(formData: FormData) {
  // Validação com Zod
  const validatedFields = createStudentSchema.safeParse({
    name: formData.get('name'),
    birthDate: formData.get('birthDate'),
  })

  if (!validatedFields.success) {
    return { errors: validatedFields.error.flatten().fieldErrors }
  }

  // Lógica de negócio
  const student = await studentService.create(validatedFields.data)
  revalidatePath('/dashboard/alunos')
  
  return { success: true, student }
}
```

## Modelos de Dados

### Schema do Banco (Prisma)

```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String
  role      UserRole
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Student {
  id              String    @id @default(cuid())
  name            String
  birthDate       DateTime
  cpf             String?   @unique
  photoUrl        String?
  enrollmentDate  DateTime  @default(now())
  status          StudentStatus @default(ACTIVE)
  
  guardians       Guardian[]
  emergencyContacts EmergencyContact[]
  classEnrollments ClassEnrollment[]
  payments        Payment[]
  
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Class {
  id          String @id @default(cuid())
  name        String
  grade       Grade
  maxCapacity Int
  academicYear Int
  
  teacher     Staff?  @relation(fields: [teacherId], references: [id])
  teacherId   String?
  enrollments ClassEnrollment[]
  
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

enum UserRole {
  ADMIN
  TEACHER
  GUARDIAN
}

enum StudentStatus {
  ACTIVE
  INACTIVE
  TRANSFERRED
}

enum Grade {
  MATERNAL
  PRE_1
  PRE_2
  FIRST_GRADE
  SECOND_GRADE
  THIRD_GRADE
  FOURTH_GRADE
  FIFTH_GRADE
}
```

## Componentes e Interfaces

### Layout Principal

- **DashboardLayout**: Layout base com sidebar, header e área de conteúdo
- **AuthLayout**: Layout para páginas de autenticação
- **Sidebar**: Navegação lateral com menu hierárquico
- **Header**: Barra superior com informações do usuário

### Formulários

- `StudentForm`: Cadastro/edição de alunos
- `ClassForm`: Criação/edição de turmas
- `StaffForm`: Formulário de funcionários
- `ActivityForm`: Formulário de atividades

### Tabelas de Dados

- `StudentsTable`: Tabela de alunos com filtros e paginação
- `ClassesTable`: Tabela de turmas
- `PaymentsTable`: Tabela de pagamentos

## Segurança e Conformidade

### Medidas de Segurança

1. **Autenticação**: Stack Auth para autenticação robusta
2. **Autorização**: Role-based access control (RBAC)
3. **Validação**: Validação rigorosa com Zod
4. **Conformidade LGPD**: Consentimento e auditoria completa

### Auditoria

```typescript
interface AuditLog {
  id: string
  userId: string
  action: string
  resource: string
  timestamp: DateTime
  ipAddress: string
}
```

## Performance e Otimização

### Estratégias

1. **Server Components**: Renderização no servidor por padrão
2. **Server Actions**: Redução de JavaScript no cliente
3. **Code Splitting**: Carregamento lazy de componentes
4. **Database Optimization**: Índices apropriados

### Responsividade

- **Mobile-first**: Design otimizado para dispositivos móveis
- **Breakpoints**: sm (640px), md (768px), lg (1024px), xl (1280px)
- **Progressive Web App**: Service workers para funcionalidade offline

## Acessibilidade

- **Navegação por teclado**: Todos os elementos acessíveis
- **Screen readers**: Semântica HTML apropriada
- **Contraste**: Cores com contraste adequado
- **WCAG 2.1**: Compliance completo