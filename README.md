# ANIYUME 構築の道

> **Tracker de anime, mangá, séries e doramas.**  
> Full-stack com NestJS + Next.js 14 + PostgreSQL + Redis.

---

## Stack

| Camada | Tecnologia |
|---|---|
| Backend | NestJS · TypeORM · PostgreSQL · Redis · JWT |
| Frontend | Next.js 14 (App Router) · TypeScript · Tailwind CSS · shadcn/ui |
| Infra | Docker · Docker Compose |
| State | TanStack React Query |
| Auth | @nestjs/passport · bcrypt · next-auth |

---

## Estrutura do Projeto

```
aniyume/
├── backend/          # NestJS API (porta 3001)
├── frontend/         # Next.js App (porta 3000)
├── docker-compose.yml
├── .gitignore
└── README.md
```

---

## Pré-requisitos

- Node.js 18+
- Docker & Docker Compose
- npm ou yarn

---

## Instalação e Setup

### 1. Clone e configure o ambiente

```bash
git clone https://github.com/seu-usuario/aniyume.git
cd aniyume
```

### 2. Suba os serviços de infra

```bash
docker compose up -d
```

Serviços disponíveis após o comando:
- PostgreSQL → `localhost:5432`
- Redis → `localhost:6379`

### 3. Configure as variáveis de ambiente

**Backend** — crie `backend/.env`:

```env
DATABASE_URL=postgresql://aniyume:secret123@localhost:5432/aniyume_dev
JWT_SECRET=sua_chave_secreta_aqui
REDIS_URL=redis://localhost:6379
```

**Frontend** — crie `frontend/.env.local`:

```env
NEXTAUTH_SECRET=sua_chave_secreta_aqui
NEXTAUTH_URL=http://localhost:3000
NEXT_PUBLIC_API_URL=http://localhost:3001
```

### 4. Backend

```bash
cd backend
npm install
npm run migration:run
npm run start:dev
```

### 5. Frontend

```bash
cd frontend
npm install
npm run dev
```

**Tudo funcionando quando:**
- Frontend → `http://localhost:3000` ✓
- Backend → `http://localhost:3001` ✓
- Postgres → `:5432` ✓
- Redis → `:6379` ✓

---

## Scripts úteis

```bash
# Backend
npm run start:dev          # dev com hot reload
npm run migration:generate -- src/migrations/NomeDaMigration
npm run migration:run
npm run migration:revert
npm run build

# Frontend
npm run dev
npm run build
npm run lint

# Infra
docker compose up -d       # sobe tudo em background
docker compose down        # derruba tudo
docker compose logs -f     # acompanha logs
```

---

## Roadmap de Construção

O projeto foi construído em fases sequenciais — cada fase desbloqueia a próxima.

| Fase | Descrição | Tempo estimado |
|---|---|---|
| 01 | Fundação & Ambiente | 1–2 semanas |
| 02 | Modelagem & Banco de Dados | 1–2 semanas |
| 03 | Autenticação & Usuários | 1 semana |
| 04 | Catálogo & Tracking | em andamento |
| 05 | Social (follows, feed) | planejado |
| 06 | APIs Externas | planejado |

> **Regra de ouro:** não pule fases. A dor de voltar atrás é 10× maior que a dor de respeitar a ordem.

---

## Modelo de Dados (visão geral)

```
User ──< UserMedia >── Media (anime/manga/serie/dorama)
User ──< Review >── Media
Media ──< MediaGenre >── Genre
User ──< Follow >── User
```

---

## Convenções do Projeto

- **Migrations**: toda alteração de schema vai via migration, nunca `synchronize: true` em produção
- **DTOs**: todos os inputs são validados com `class-validator` antes de chegar nos services
- **Guards**: rotas protegidas usam `@UseGuards(AuthGuard('jwt'))` — sem exceção
- **Variáveis de ambiente**: nunca commitar `.env` — use `.env.example` como referência
- **Commits**: mensagens em inglês, imperative mood (`Add auth module`, não `Added auth module`)

---

## Contribuindo

1. Crie uma branch a partir de `main`: `git checkout -b feature/nome-da-feature`
2. Implemente e teste localmente
3. Abra um PR com descrição do que foi feito e por quê

---

## Licença

MIT
# 📁 Frontend Architecture — Scalable Structure (Next.js)

Estrutura recomendada para projetos **Next.js + TypeScript + Tailwind + React Query**, focada em **escalabilidade, organização e manutenção**.

---

# 🧠 Visão Geral

* Arquitetura baseada em **features (domínios)**
* Separação clara entre:

  * UI
  * lógica de negócio
  * infraestrutura
* Código desacoplado e reutilizável

---

# 📦 Estrutura de Pastas

```bash
/frontend
 ├── /app                    # Rotas (Next.js App Router)
 │   ├── /(auth)
 │   │   ├── login/
 │   │   └── register/
 │   │
 │   ├── /(dashboard)
 │   │   ├── page.tsx
 │   │   └── layout.tsx
 │   │
 │   ├── /media
 │   │   └── [id]/
 │   │
 │   ├── layout.tsx
 │   └── globals.css
 │
 ├── /features              # 🔥 Domínios do sistema
 │   ├── /auth
 │   │   ├── components/
 │   │   ├── hooks/
 │   │   ├── services/
 │   │   ├── types.ts
 │   │   ├── store.ts
 │   │   └── index.ts
 │   │
 │   ├── /user
 │   ├── /media
 │   ├── /review
 │   ├── /list
 │
 ├── /components            # UI global reutilizável
 │   ├── /ui                # (shadcn/ui)
 │   ├── /layout
 │   └── /shared
 │
 ├── /lib                   # Infraestrutura
 │   ├── api.ts             # Config API (axios/fetch)
 │   ├── auth.ts
 │   ├── utils.ts
 │
 ├── /hooks                 # Hooks globais
 ├── /types                 # Tipos globais
 ├── /styles                # Estilos globais
 ├── /constants             # Constantes
 └── /config                # Configurações gerais
```

---

# 🔥 Arquitetura por Feature

Cada domínio do sistema deve ser isolado dentro de `/features`.

## Exemplo: `media`

```bash
/features/media
 ├── components/
 │   ├── MediaCard.tsx
 │   ├── MediaList.tsx
 │
 ├── hooks/
 │   ├── useMedia.ts
 │
 ├── services/
 │   ├── media.service.ts
 │
 ├── types.ts
 └── index.ts
```

---

# 🧩 Responsabilidades por Camada

## 📍 `/app`

* Define rotas e páginas
* NÃO contém lógica de negócio

---

## 🔥 `/features`

* Regra de negócio
* Integração com API
* Hooks específicos
* Estado da feature

---

## 🎨 `/components`

* Componentes reutilizáveis
* Sem lógica de negócio

---

## ⚙️ `/lib`

* Configurações globais
* Helpers
* Cliente de API

---

## 🧠 `/hooks`

* Hooks reutilizáveis globais

---

# ⚡ Padrões Obrigatórios

## ✔ Service Layer

```ts
// features/media/services/media.service.ts
import { api } from "@/lib/api"

export const getMedia = async () => {
  const { data } = await api.get("/media")
  return data
}
```

---

## ✔ Hooks como Interface

```ts
// features/media/hooks/useMedia.ts
import { useQuery } from "@tanstack/react-query"
import { getMedia } from "../services/media.service"

export const useMedia = () => {
  return useQuery({
    queryKey: ["media"],
    queryFn: getMedia
  })
}
```

---

## ✔ Barrel Pattern (index.ts)

```ts
// features/media/index.ts
export * from "./components/MediaCard"
export * from "./hooks/useMedia"
```

Uso:

```ts
import { MediaCard, useMedia } from "@/features/media"
```

---

## ✔ Tipagem

```ts
// features/media/types.ts
export interface Media {
  id: string
  title: string
  cover: string
}
```

---

# 🚫 O que EVITAR

❌ Estrutura baseada só em tipo:

```bash
/components
/hooks
/services
```

❌ Chamar API direto no componente
❌ Misturar lógica de negócio com UI
❌ Arquivos gigantes

---

# 🚀 Evolução (Projetos grandes)

```bash
/features/media
 ├── api/
 ├── domain/
 ├── ui/
 ├── store/
```

---

# 📊 Resumo

| Camada        | Função       |
| ------------- | ------------ |
| `app/`        | Rotas        |
| `features/`   | Lógica       |
| `components/` | UI           |
| `lib/`        | Infra        |
| `hooks/`      | Reutilização |

---

# 🧠 Filosofia

* Organização por domínio > organização por tipo
* Código próximo de onde é usado
* Baixo acoplamento
* Alta coesão

---

# 🏁 Resultado

✔ Projeto organizado
✔ Escalável para times grandes
✔ Fácil manutenção
✔ Código previsível

---