# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A freshly generated Angular 21 application (Angular CLI 21.2). `src/app/app.html` is still the CLI placeholder template, and `app.routes.ts` is still empty.

## Commands

- `npm start` (`ng serve`): dev server at http://localhost:4200 using the `development` configuration
- `npm run build` (`ng build`): production build into `dist/`, which is the default configuration. Size budgets are enforced: initial bundle warns at 500kB and errors at 1MB, and per-component styles warn at 4kB and error at 8kB.
- `npm run watch`: development build in watch mode
- `npm test` (`ng test`): unit tests run by **Vitest** through the `@angular/build:unit-test` builder, with jsdom as the environment. This is not Karma or Jasmine.
  - Single file: `ng test --include src/app/app.spec.ts`
  - One run without watch mode (for CI): `ng test --watch=false`
- `npx prettier --write .` formats code. Prettier is configured in `.prettierrc` with print width 100, single quotes, and the Angular parser for `*.html`. No lint tool is set up.
- `ng generate component <name>` scaffolds a component. The selector prefix is `app` and styles default to SCSS.

## Architecture and conventions

- **Standalone and zoneless.** There are no NgModules, and `zone.js` is not a dependency. `src/main.ts` calls `bootstrapApplication(App, appConfig)`, and app-wide providers go in `src/app/app.config.ts` (currently `provideBrowserGlobalErrorListeners()` and `provideRouter(routes)`). Change detection depends on signals, so use `signal`/`computed`/`input()`/`output()` for state, not zone-triggered mutation. Tests should `await fixture.whenStable()` and not rely on zone-based auto-detection.
- **File naming follows the Angular v20+ style.** Files have no `.component` suffix: `app.ts`, `app.html`, `app.scss`, `app.spec.ts`. The class is named `App`, not `AppComponent`. New code should follow the same pattern.
- **TypeScript and the template compiler are strict.** `strict` is on, along with `strictTemplates`, `strictInjectionParameters`, `strictInputAccessModifiers`, `noPropertyAccessFromIndexSignature`, and `noImplicitOverride`.
- Static assets go in `public/` and are served from the site root. Global styles go in `src/styles.scss`.
- `.vscode/mcp.json` configures the Angular CLI MCP server (`npx @angular/cli mcp`).
