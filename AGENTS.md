# Repository Guidelines

## Project Structure & Module Organization
This is a Nuxt 4 blog project. App code lives in `app/` (`components/`, `pages/`, `stores/`, `composables/`, `utils/`). Content is stored in `content/`, with published posts under `content/posts/`, previews under `content/previews/`, and standalone pages such as `content/theme.md` and `content/link.md`. Server endpoints are in `server/`, custom Nuxt modules in `modules/`, and reusable scripts in `scripts/`.

## Build, Test, and Development Commands
- `pnpm i` — install dependencies.
- `pnpm dev` — start the local dev server.
- `pnpm dev:host` — start dev server on the local network.
- `pnpm build` — build the app.
- `pnpm generate` — generate the static site into `dist/`.
- `pnpm preview` — preview the production build.
- `pnpm lint` — run ESLint and Stylelint.
- `pnpm lint:fix` — auto-fix lint issues.
- `pnpm new` — create a new blog post.
- `pnpm init-project` — initialize project-specific configuration.

## Coding Style & Naming Conventions
Use tabs for code and spaces for `*.md`, `*.json`, and `*.yaml` per `.editorconfig`. TypeScript is the default for scripts and Vue `<script>` blocks; SCSS is used for styles. Follow the existing naming style: PascalCase for Vue components (for example `BlogFooter.vue`), camelCase for utilities, and kebab-case for content filenames (for example `dark-mode-guide.md`). Lint rules come from `@antfu/eslint-config` plus Stylelint.

## Testing Guidelines
There is no dedicated automated test suite in this repository yet. At minimum, run `pnpm lint` before submitting changes. For content or routing changes, also verify with `pnpm dev` or `pnpm generate && pnpm preview`. If you add tests later, place them near the feature or in a clearly named `*.test.*` or `*.spec.*` file.

## Commit & Pull Request Guidelines
Recent history follows concise Conventional Commit style such as `feat(icon): ...` and `docs: ...`. Keep commits scoped and descriptive. Pull requests should explain the change, list affected areas (for example `app/pages`, `content`, `blog.config.ts`), link related issues, and include screenshots for UI changes.

## Configuration Tips
Before deploying a fork, replace the original site identity and services in `blog.config.ts` and related app config files, and remove or replace sample content in `content/`.