# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Project

Arcade Vault: an online platform to play games and compete for the highest score. Currently a fresh Create Next App scaffold (`app/page.tsx` is still the template page).

Development follows Spec Driven Design via the `/spec` and `/spec-impl` skills from https://github.com/Klerith/fernando-skills (install: `npx skills@latest add Klerith/fernando-skills`). Write a spec before implementing features.

## Commands

Package manager is pnpm (`packageManager: pnpm@12.18.0`).

- `pnpm dev` — dev server (Turbopack)
- `pnpm build` — production build
- `pnpm start` — serve production build
- `pnpm lint` — ESLint (flat config: `eslint-config-next` core-web-vitals + typescript)

No test runner is configured yet.

## Stack and non-default config

- Next.js 16.4 (App Router, `app/`), React 19.3, TypeScript, Tailwind CSS v4.
- Next.js APIs differ from training data. Check `node_modules/next/dist/docs/` (`01-app`, `03-architecture`, ...) before using any Next API.
- `next.config.ts` enables `cacheComponents`, `partialPrefetching`, and `experimental.agentFeedback`. `cacheComponents` changes data-fetching/caching semantics (read the docs on `"use cache"` and Suspense boundaries before writing data-fetching code).
- Tailwind v4 is wired through a Turbopack loader rule (`@tailwindcss/turbopack` on `*.css`), not PostCSS. There is no `tailwind.config`; theme tokens live in `app/globals.css` via `@import "tailwindcss"` and `@theme inline`.
- Layouts use the global typed helper `LayoutProps<"/">` (generated route types), not hand-written props types.
- Path alias `@/*` maps to the repo root.
- `pnpm-workspace.yaml` disables build scripts for `sharp` and `unrs-resolver`.
