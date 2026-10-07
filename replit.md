# Optivive.app — SEO / GEO Platform

Prywatna platforma do tworzenia, audytowania i publikowania dopasowanych treści SEO oraz GEO.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/api-server` — Express API, logika publikacji i integracje serwerowe
- `artifacts/seo-studio` — jedyne kanoniczne źródło panelu Optivive.app i jedyny aktywny podgląd aplikacji (`/`)
- Historyczny artefakt Autoamer został wycofany i usunięty; nie ma osobnego artefaktu ani workflow
- `artifacts/seo-studio-deck` — prezentacja produktu
- `artifacts/mockup-sandbox` — podgląd komponentów na canvasie
- `lib/db/src/schema` — źródło schematu PostgreSQL
- `lib/api-spec/openapi.yaml` — źródło kontraktu API
- `lib/article-style` — wspólny styl artykułów dla podglądu i publikacji WordPress
- `docs/replit-account-transfer.md` — instrukcja importu do innego konta Replit

## Architecture decisions

- Optivive.app is a private SEO/GEO editorial platform: the whole SPA sits behind Clerk sign-in (managed Clerk, keys auto-provisioned), and the WordPress-mutating endpoints (`POST /articles/:id/publish`, `POST`/`DELETE /articles/:id/wp-sync`) return 401 without a Clerk session, since the server holds WordPress credentials.
- Web auth is cookie-based (Clerk session cookie) — no bearer tokens in the browser client.

## Product

Studio obsługuje tworzenie artykułów SEO, analizę i regenerację treści, edycję HTML, media,
słowa kluczowe, profile marki oraz publikację do WordPressa i kanałów społecznościowych.

## User preferences

- Przy pracy nad treściami/SEO (nowe artykuły, frazy, audyty) używaj skilli `deep-research` (pogłębiony research tematu) i `seo-auditor` (audyt SEO stron/treści) — prośba użytkownika z 12.08.2026.
- Komunikacja po polsku.
- Design artykułów: sekcja „Przeczytaj również" (linki wewnętrzne) bez bulletów/znaczników przed linkami; przyciski „Kontakt telefoniczny" i „Wycena po VIN" zawsze czerwone z białym napisem.

## Gotchas

- Projekt używa pnpm workspace; po imporcie uruchom `pnpm install`, nie `npm install`.
- Sekrety, baza PostgreSQL, Clerk, Object Storage i integracje Replit nie przenoszą się z ZIP-em.
- Przed publikacją trzeba odtworzyć konfigurację opisaną w `docs/replit-account-transfer.md`.
- `POST /articles/:id/wp-sync` aktualizuje istniejący wpis WordPressa przez multipart POST i publikuje go.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
- Granica między silnikiem SEO Studio a konfiguracją Workspace: `docs/workspace-configuration-boundary.md`
