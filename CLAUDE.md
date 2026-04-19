# CLAUDE.md
## Stack & Commands
- **Framework:** Svelte 5 + SvelteKit
- **Language:** TypeScript
- **Dev:** `bun run dev` | **Build:** `bun run build` | **Test:** `bun run test` | **Lint:** `bun run check`

## Conventions
- Use Svelte 5 runes (`$state`, `$derived`, `$effect`); never use `$:` or `export let`
- Components: `.svelte`; Shared logic: `.ts` or `.svelte.ts`
- Routing: `+page.svelte`, `+layout.svelte`, `+server.ts`, `+page.ts`
- Keep components <150 lines; extract complex logic to TS files
- Use `@/` alias for `src/`; prefer relative imports for sibling files
- Styling: scoped `<style>` or Tailwind utility classes
- Make Responsive as soon as possible if user need to create or modify pages

## AI Rules
- Output TypeScript-first, complete, and directly pasteable code
- Default to accessibility, SSR/CSR safety, and performance best practices
- Never mock env vars or API routes; use explicit placeholders
- Ask one clarifying question if context is missing; otherwise proceed