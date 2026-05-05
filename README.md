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
