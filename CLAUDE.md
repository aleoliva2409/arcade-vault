# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Project

Arcade Vault: an online platform to play games and compete for the highest score. Global theme is in place (see Design system); `app/page.tsx` is still the Create Next App template page.

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
- ESLint ignores `references/**` (`globalIgnores` in `eslint.config.mjs`).

## Design system

- Visual source of truth: `references/templates/` (`Arcade Vault.html`, `styles.css`, and `.jsx` screen mockups: biblioteca, detalle, reproductor, salon, auth, nav). These are references, not app code; the UI must match them exactly.
- `app/globals.css` is a port of `references/templates/styles.css`. Theme variables (`--bg`, `--ink`, `--cyan`, `--magenta`, `--yellow`, `--line`, `--pixel`, `--mono`, ...) live in `:root`. Resets are in `@layer base`, template classes (`.av-nav`, `.btn`, `.card`, `.crt`, `.modal`, `.podium`, ...) in `@layer components`, so Tailwind utilities can override them. Keep new custom CSS in those layers.
- `@theme inline` exposes the tokens to Tailwind: `bg-bg-2`, `text-cyan`, `border-line`, `font-pixel`, `font-mono`, etc.
- Fonts load via `next/font/google` in `app/layout.tsx` (Press Start 2P, JetBrains Mono, Courier Prime) as `--font-press-start-2p`, `--font-jetbrains-mono`, `--font-courier-prime`; `--pixel` and `--mono` reference them. Do not add Google Fonts `<link>` tags.
- Root layout mirrors the template skeleton: `.av-bg` and `.av-noise` background layers, then `<div id="root">` wrapping the page. Some template CSS targets `#root`, so keep that id.
- UI copy is in Spanish (`<html lang="es">`).

## Skills
Always use /frontend-design to design the UI