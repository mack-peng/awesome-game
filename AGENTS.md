# AGENTS.md

## Project overview

Vue 2 browser-game collection (Flappy Bird, Snake, Minesweeper). No tests, no CI, no TypeScript.

## Commands

- `npm run dev` — dev server on **port 7777** (auto-opens browser)
- `npm run build` — production build to `dist/`
- `npm run d` — runs `./deploy.sh` (not in repo)
- No test/lint/typecheck scripts are configured. `eslint` is installed but has no `package.json` script.

## Architecture

- **Entry point**: `src/_config/main.js` (not `src/main.js`)
- **Webpack config**: `build/` + `config/` (vue-cli 2.x template, webpack 3)
- **Router**: lazy-loaded via `require([...], resolve)` in `src/router/index.js`
- **Vuex store**: `src/store/index.js` — single flat store, persisted to sessionStorage on `beforeunload`
- **Global installs**: `src/_config/build.js` mounts utility functions on `Vue.prototype` and registers custom directives

## Webpack aliases

Beyond `@` → `src/`:

| Alias | Target |
|-------|--------|
| `@style` | `src/assets/css` |
| `@image` | `src/assets/img` |
| `@constants` | `src/constants/index.js` |
| `@mixin-scss` | `src/utils/common.scss` |

## ESLint quirks

Extends `standard` preset but **requires semicolons** (`"semi": ["error", "always"]`) and disables several standard rules (`eqeqeq`, `spaced-comment`, `keyword-spacing`, `space-before-function-paren`, `quotes`). Run with `npx eslint src/` if needed.

## Other gotchas

- `node-sass` is aliased to the `sass` Dart package in `package.json` (`"node-sass": "npm:sass@1.49.9"`) — don't install the real `node-sass`.
- `supply/` is README image assets, not source code.
- `src/utils/index.js` imports the Vue instance from `src/_config/main.js`; be careful of circular dependencies when editing these files.
- `src/constants/` has per-game config files (`bird.js`, `snake.js`, `sweep.js`) plus `index.js` re-export.
