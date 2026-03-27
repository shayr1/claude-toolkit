## Nuxt UI - MANDATORY RULES

- **CRITICAL: Every interactive element MUST have `cursor-pointer`.** Nuxt UI does not set `cursor-pointer` by default on buttons, tabs, accordion triggers, checkboxes, radios, switches, dropdown items, pagination, or navigation links. Always configure `cursor-pointer` globally via `app.config.ts` theme slots for all interactive components. When adding a new Nuxt UI interactive component to a project, verify it has `cursor-pointer` in the global config.

## Nuxt - MANDATORY RULES

### File Creation

**NEVER CREATE NUXT FILES MANUALLY.** Always use `npx nuxt add`. Never use Write tool, touch, or mkdir for Nuxt files.

```bash
npx nuxt add component ComponentName          # Components
npx nuxt add component ComponentName --client  # Client-only component
npx nuxt add composable useMyComposable        # Composables
npx nuxt add page about                        # Pages
npx nuxt add page "users/[id]"                 # Dynamic routes (use quotes)
npx nuxt add layout default                    # Layouts
npx nuxt add middleware auth                    # Middleware
npx nuxt add middleware auth --global           # Global middleware
npx nuxt add plugin analytics                  # Plugins
npx nuxt add plugin analytics --client          # Client-only plugin
npx nuxt add api users --get                    # API routes
npx nuxt add api "users/[id]" --get             # Dynamic API routes
npx nuxt add server-route health                # Server routes (no /api prefix)
npx nuxt add server-util myUtil                 # Server utils
npx nuxt add server-plugin logger               # Server plugins
npx nuxt add server-middleware cors              # Server middleware
```

### Modules

Use `npx nuxt module add` only for modules listed on [nuxt.com/modules](https://nuxt.com/modules). For other packages, install manually with pnpm and add to `nuxt.config.ts`:
```bash
npx nuxt module add @pinia/nuxt               # Modules from nuxt.com/modules
```

### State Management

- Pinia for cross-component/cross-page stores
- Store files in `app/stores/` directory
- One store per domain: `useAuthStore.ts`, `useCartStore.ts`

## Code Style (Nuxt-specific)

### Vue Components
- Always `<script setup lang="ts">`. Never Options API.
- Section order: `<script setup>` -> `<template>` -> `<style>` (when needed)
- Prefer Tailwind utility classes in templates over `<style>` blocks
- Inline `defineProps<{ ... }>()` for simple props without defaults
- `withDefaults(defineProps<Props>(), { ... })` when defaults are needed - use inline generic for simple cases, named `interface Props` for complex ones
- Typed emits: `const emit = defineEmits<{ confirm: [], submit: [data: FormData] }>()`
- `defineModel<T>()` for two-way bindings
- Event handlers use `handle` prefix: `handleSubmit`, `handleCancel`, `handleConfirm`. Also acceptable: `toggle*`, `close*`, `open*` for their specific actions.
- Boolean refs prefixed with `is`: `isOpen`, `isSubmitting`, `isLoading`. Active processes can use gerund: `generatingAI`, `uploadingAvatar`.

### Template Patterns
- `v-if` exclusively (no `v-show`)
- Shorthand `:prop` binding (never `v-bind:prop`)
- Shorthand `@event` binding (never `v-on:event`)
- Boolean attributes without binding when static: `required`, `autofocus`, `block`
- `@click="handleX"` (method reference, no parens) for handlers
- `@click="expression"` for inline toggles: `@click="isOpen = true"`
- Named slots via `<template #slotName>`
- PascalCase for all components in templates
- All user-facing strings through i18n `t()` function

### Naming
- **Components:** PascalCase files and template usage
- **Composables:** `useXxx.ts` with camelCase
- **Server routes:** `{resource}.{method}.ts` or `{resource}/{action}.{method}.ts`
- **Middleware:** kebab-case: `auth.global.ts`
- **Plugins:** kebab-case with suffix: `auth.server.ts`, `paddle.client.ts`
- **Client-only components:** `.client.vue` suffix
- **Pages:** kebab-case: `reset-password.vue`, `book-demo.vue`

### Imports
- Rely on Nuxt auto-imports for Vue APIs, Nuxt composables, and project composables/utils
- Explicit imports only for external packages and type-only imports
- Use `import type` for type-only imports
- Order when explicit: external packages -> `import type` -> internal alias (`~/`, `~~/`) -> relative (`./`)

### Data Fetching
- `useFetch` for SSR-compatible data fetching with reactive queries
- `$fetch` for imperative calls inside event handlers

### i18n (@nuxtjs/i18n)
- When using Nuxt 4 `app/` directory structure, **must** set `restructureDir: 'app'` in the i18n config in `nuxt.config.ts` so i18n can find locale files under `app/locales/`
- `lazy` option does not exist in @nuxtjs/i18n v9+ - lazy loading is implicit when `file` or `files` is set on each locale
- CSS `@import url(...)` for Google Fonts must come **before** `@import "tailwindcss"` and `@import "@nuxt/ui"` to avoid PostCSS errors

### General
- All app code lives under `app/` directory (Nuxt 4 convention)
- JSDoc on composables and server utilities when the function signature isn't self-explanatory
