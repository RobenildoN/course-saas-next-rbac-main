# SaaS Multi-tenant com Next.js + RBAC 🚀

Um boilerplate completo para construir um SaaS multi-tenant com autenticação, autorização baseada em papéis (RBAC) e um conjunto de features essenciais para organizações, projetos, membros, convites e billing — tudo pronto para evoluir em produção. 🧭

## O que é este projeto? 🧩

- Monorepo com front-end em Next.js e back-end em Fastify, compartilhando pacotes para autenticação/autorização e gerenciamento de variáveis de ambiente.
- Foco em multi-tenancy: cada organização possui seus próprios membros, projetos e regras de acesso.
- Autorização com RBAC usando CASL para definir permissões claras por papel.

## Tecnologias ⚙️

- Front-end: Next.js (App Router) • React • Tailwind CSS • Radix UI • TanStack Query • Ky • Zod
- Back-end: Fastify • Prisma • PostgreSQL • JWT • Zod • Swagger/OpenAPI
- Compartilhado: CASL (@saas/auth) • @t3-oss/env-nextjs (@saas/env) • ESLint/Prettier/TSConfig compartilhados

## Estrutura do Código 🗂️

Monorepo organizado por apps e packages:

```
apps/
  api/            # API Fastify + Prisma + Swagger
  web/            # Next.js (App Router) + Tailwind + Radix

packages/
  auth/           # RBAC com CASL (roles, subjects, permissions)
  env/            # Validação e carga de variáveis (.env)
```

- API
  - Rotas e plugins Fastify, JWT, CORS e documentação via Swagger.
  - Prisma com PostgreSQL e modelos para usuários, organizações, membros, convites e projetos.
- Web
  - App Router moderno (src/app) com páginas e layouts por segmento.
  - Componentes UI (Radix + Tailwind) e data-fetching com TanStack Query.
- Packages
  - @saas/auth: Abstrações de RBAC usando CASL (roles, subjects, permissions).
  - @saas/env: Tipagem e validação de variáveis de ambiente para server e client.

## RBAC (Autorização) 🛡️

Papéis disponíveis:

- ADMIN
- MEMBER
- BILLING

Regras principais:

- ADMIN: pode gerenciar quase tudo; transferir propriedade/atualizar organização apenas se for o owner.
- MEMBER: pode criar/listar projetos e gerenciar seus próprios projetos.
- BILLING: pode gerenciar Billing da organização.

As regras são definidas com CASL e aplicadas conforme o papel do usuário na organização.

## Principais Features ✨

- Autenticação por e-mail/senha e via GitHub
- Recuperação de senha, criação de conta e perfil
- Organizações: criar, listar, atualizar, encerrar, transferir propriedade
- Convites: criar, listar, aceitar, rejeitar, revogar
- Membros: listar, atualizar papel, remover
- Projetos: criar, listar, atualizar, deletar
- Billing: consulta por organização

---

Feito com ❤️ para acelerar o desenvolvimento de SaaS multi-tenant com uma base sólida e opinada, pronta para personalização.

## Como executar localmente 🧪

### 1) Pré-requisitos

- Node.js 18+ e pnpm
- PostgreSQL rodando localmente

### 2) Variáveis de ambiente

- Copie `.env.example` para `.env` na raiz e ajuste valores:
  - DATABASE_URL, JWT_SECRET, GITHUB_OAUTH_CLIENT_ID, GITHUB_OAUTH_CLIENT_SECRET, GITHUB_OAUTH_CLIENT_REDIRECT_URI, NEXT_PUBLIC_API_URL

### 3) Instalar dependências

```bash
pnpm install -r
```

### 4) Banco de dados (com Docker) 🐳

```bash
docker compose up -d
pnpm db:migrate
pnpm db:studio
```

Se preferir usar um Postgres local, garanta que a `DATABASE_URL` aponte para seu servidor.

### 5) Subir API e Web juntos

```bash
pnpm dev:stack
```

- API: http://localhost:3333 (docs em `/docs`)
- Web: http://localhost:3000 (ou 3001/3002 se portas ocupadas)

### 6) Comandos úteis

- Subir apenas API: `pnpm dev:api`
- Subir apenas Web: `pnpm dev:web`
- Build Web: `pnpm build:web`
- Build API: `pnpm build:api`

### Troubleshooting 🛠️

- Erro de módulo não encontrado (Next.js): reinstale dependências e confirme caminhos de import.
- Variáveis de ambiente ausentes: use `pnpm --filter @saas/web env:load next build` para validar e ajuste `.env`.
- Docker no Windows: inicie o Docker Desktop antes do `docker compose up -d`.
- Prisma CLI falhando com Node muito novo: use Node 20 LTS para evitar incompatibilidades.
