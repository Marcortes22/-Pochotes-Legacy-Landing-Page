# AGENTS.md

Static marketing landing page for "Pochote's Legacy" (Blue Zone Pochote real-estate project).
Astro 7 + React 19 + Tailwind v4. Still largely the `astro basics` starter — `README.md` is
unedited template boilerplate, treat it as noise.

## Commands

Package manager is **pnpm** (`pnpm-lock.yaml`). Node `>=22.23.4`.

| Task | Command |
| --- | --- |
| Install | `pnpm install` |
| Dev server | `astro dev --background` (then `astro dev stop` / `status` / `logs`) |
| Build | `pnpm build` → static output in `dist/` |
| Preview build | `pnpm preview` |
| Add shadcn component | `pnpm dlx shadcn@latest add <name>` |

There is **no lint, test, or format setup**. Don't invent `pnpm test` / `pnpm lint`.
`astro check` is the only verification step, but `@astrojs/check` is **not installed** — it will
prompt to install on first run. Prefer `pnpm build` as the cheap correctness gate.

## Stack quirks (easy to get wrong)

- **shadcn style is `base-nova`, built on `@base-ui/react` — NOT Radix.** Never hand-write a
  component with `@radix-ui/*` imports; add it via the shadcn CLI so the Base UI variant lands.
- **`cn` comes from the standalone `cn` package**, not `clsx` + `tailwind-merge`.
  `src/lib/utils.ts` is a one-line re-export. Import from `@/lib/utils` in app code.
- **Tailwind v4 via `@tailwindcss/vite`. There is no `tailwind.config.*`.** All theme tokens,
  `@theme inline`, and the `dark` variant live in `src/styles/global.css`.
- `global.css` is currently imported per-page (`src/pages/index.astro`), **not** in
  `src/layouts/Layout.astro`. Any new page must import it or ship unstyled.
- Path alias `@/*` → `src/*` (tsconfig). TS is `astro/tsconfigs/strict`.

## Conventions

- **Astro-first.** Build UI as `.astro` components. Reach for React (`.tsx`) only when a piece
  genuinely needs client interactivity, and add an explicit `client:*` directive.
- Icons: `@lucide/astro` in `.astro` files, `lucide-react` in `.tsx` files.
- Copy is **bilingual ES/EN** and lives in `src/i18n/ui.ts`. Don't hardcode one language into
  shared components without asking.
- Deploy target is **Vercel, static** (`vercel.json`, no adapter). Keep the build static; don't
  add `output: "server"` or an adapter unprompted. `PUBLIC_FORM_ENDPOINT` must be set in Vercel's
  environment variables (it is inlined at build time).
- Photos live in `src/assets/images/` (optimized by `astro:assets`). Everything in `public/` is
  published as-is — never put private material there.
- `docs/` is **gitignored on purpose**: it holds private source material (family, health and
  financial data, owner email). It exists only locally; never commit it or copy it into `public/`.

## Agent config

- `opencode.jsonc` wires the remote **Astro docs MCP server**; use it for Astro API questions.
- `.agents/skills/` is **vendored from GitHub and pinned by `skills-lock.json`** (hash-checked).
  Do not hand-edit skill files; they are not project source.
  Relevant skills here: `astro-best-practices`, `frontend-design`, `responsive-design`,
  `accessibility`, `seo-audit`.
