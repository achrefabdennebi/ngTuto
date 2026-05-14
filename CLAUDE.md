# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm start          # dev server at http://localhost:4200
ng serve           # same as above
ng build           # production build → dist/
ng build --watch --configuration development  # watch mode
ng test            # run all unit tests with Vitest
ng generate component <name>  # scaffold a new component
```

To run a single test file, use Vitest directly:

```bash
npx vitest run src/app/app.spec.ts
```

Format code with Prettier:

```bash
npx prettier --write .
```

## Architecture

This is a **standalone Angular 21** app — there are no NgModules. Everything is wired through `ApplicationConfig` in `src/app/app.config.ts`.

**Bootstrap flow:** `src/main.ts` → `bootstrapApplication(App, appConfig)` → providers from `app.config.ts` → router from `app.routes.ts`

**Key patterns:**
- Components use the `imports: []` array directly instead of an NgModule
- Reactive state uses Angular **signals** (`signal()`, `computed()`, `effect()`) rather than RxJS subjects where possible
- Routing is configured via `provideRouter(routes)` in `app.config.ts`; add routes to `src/app/app.routes.ts`
- Static assets go in `public/` (served at root `/`)
- Global styles go in `src/styles.scss`; component-scoped styles live alongside each component as `.scss` files

## Style & Formatting

Prettier is configured (`.prettierrc`):
- `printWidth: 100`
- `singleQuote: true`
- HTML files use the `angular` parser

TypeScript strict mode is fully enabled including `strictTemplates` and `strictInjectionParameters`.

## Testing

Tests use **Vitest** (not Jasmine/Karma). Test files are `*.spec.ts` alongside the source file. `vitest/globals` types are configured in `tsconfig.spec.json`, so `describe`/`it`/`expect` are available without imports.
