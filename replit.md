# Attendance Management System

A full-stack attendance tracking web app with role-based access, group scheduling, monthly grids, charts, and CSV export.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080, served at /api)
- `pnpm --filter @workspace/attendance-app run dev` — run the React frontend (port 19377, served at /)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string, `SESSION_SECRET` — JWT signing key

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite + Tailwind CSS + shadcn/ui + recharts + wouter
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Auth: JWT (bcryptjs + jsonwebtoken)
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — OpenAPI contract (source of truth)
- `lib/db/src/schema/` — DB schema: `users.ts`, `attendance.ts`
- `lib/api-client-react/src/generated/` — generated React Query hooks
- `lib/api-zod/src/generated/` — generated Zod validators for server
- `artifacts/api-server/src/routes/` — Express routes: auth, members, attendance, dashboard
- `artifacts/api-server/src/middlewares/auth.ts` — JWT middleware
- `artifacts/attendance-app/src/` — React frontend
  - `pages/` — login, dashboard, attendance, members, export
  - `hooks/use-auth.tsx` — auth context
  - `components/layout.tsx` — sidebar layout

## Architecture decisions

- JWT stored in localStorage; `setAuthTokenGetter` wires it into every generated hook's fetch call
- Group 1 meets Mon/Wed/Sat; Group 2 meets Tue/Thu/Sun — enforced in attendance grid and stats
- Attendance uses `ON CONFLICT DO UPDATE` for idempotent mark/update operations
- Admin seeded as `ADMIN-001` / `admin123`; members seeded with password `Attend@2024`
- `lib/api-spec/package.json` codegen script overwrites `api-zod/src/index.ts` to avoid duplicate export conflicts from orval's barrel generation

## Product

- Admin: manage members, mark/edit attendance in an Excel-like monthly grid, view dashboard stats with trend charts, export CSV
- Members: log in to view personal attendance, present/absent counts, percentage, and monthly bar chart
- Group system: each member belongs to Group 1 or 2, attending on their scheduled days only

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Always run `pnpm --filter @workspace/api-spec run codegen` after OpenAPI changes — it also fixes the api-zod index.ts barrel
- The api-server must be rebuilt (`pnpm run build`) before changes take effect — `dev` script does this automatically
- `SESSION_SECRET` env var must be set for stable JWT signing in production

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
