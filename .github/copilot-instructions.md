# UI Style Museum - Copilot Instructions

## Project Overview
A Vue 3 + TypeScript starter template with file-based routing, auto-imports, and Tailwind CSS v4. This is a minimal SPA showcasing modern Vue development patterns with heavy automation via Vite plugins.

## Architecture & Key Patterns

### File-Based Routing (vite-plugin-pages)
- Pages live in `src/pages/` and auto-generate routes via `~pages` import in `main.ts`
- `index.vue` → `/`, `about.vue` → `/about`, `users/[id].vue` → `/users/:id`
- No manual route registration needed - just create `.vue` files in `pages/`

### Auto-Imports (unplugin-auto-import)
- Vue APIs (`ref`, `computed`, `watch`, etc.) and composables are globally available - **no imports needed**
- All VueUse composables from `@vueuse/core` auto-imported (e.g., `useMouse`, `useLocalStorage`, `useToggle`)
- Custom functions from `src/composables/`, `src/utils/` are auto-imported
- Type definitions generated in `src/shims/auto-imports.d.ts`
- When adding new composables/utils, they become immediately available without import statements

### Component Auto-Registration (unplugin-vue-components)
- Components in `src/components/` are globally registered - use directly in templates without imports
- Icons via `unplugin-icons` with Lucide icons (`@iconify-json/lucide`): use `<LucideIconName />` directly (e.g., `<LucideHome />`, `<LucideArrowRight />`)
- Icon resolver configured with no prefix, so Lucide icons work with `Lucide` prefix only
- Type definitions in `src/shims/components.d.ts`

### Styling with Tailwind CSS v4
- Imported via Vite plugin (`@tailwindcss/vite`) - no config file needed for basic usage
- Global styles in `src/styles/index.css` with single `@import "tailwindcss"` statement
- Use `cn()` utility from `src/utils/className.ts` for conditional class merging (combines clsx + tailwind-merge)

### TypeScript Configuration
- Project uses TypeScript references with separate configs: `tsconfig.app.json` (src), `tsconfig.node.json` (vite config)
- Alias `@/` points to `src/` directory (configured in `vite.config.ts`)

### Vue Component Patterns
- **Always use Composition API**
- **Use `<script setup>` syntax** for all components
- **Define props inline** - use `defineProps<{ propName: Type }>()` with inline type definitions, not separate interfaces
- Example:
  ```vue
  <script setup lang="ts">
  const props = defineProps<{
    title: string
    count?: number
  }>()
  </script>
  ```

## Development Workflow

### Commands
```bash
yarn dev           # Start dev server (Vite + Vue DevTools)
yarn build         # Type-check + production build
yarn preview       # Preview production build locally
yarn type-check    # Run Vue TSC type checking
```

### Adding New Features
1. **New page**: Create `src/pages/foo.vue` - route auto-registers as `/foo`
2. **New component**: Create `src/components/Button.vue` - use as `<Button />` anywhere
3. **New composable/utility**: Create in `src/composables/` or `src/utils/` - auto-imported everywhere
4. **Icons**: Install iconify set (e.g., `@iconify-json/lucide`), update resolver, use as `<LucideHome />`, `<LucideArrowRight />`

### Important Notes
- **No import statements** for Vue APIs, composables, utils, or components in `src/components/`
- Router composables (`useRoute`, `useRouter`) are auto-imported
- When writing new files, leverage auto-imports - avoid manual `import` unless external libraries
- Vercel deployment configured with clean URLs and no trailing slashes (`vercel.json`)

## Project Structure
```
src/
├── pages/          # File-based routes (auto-generated)
├── components/     # Auto-registered components
├── composables/    # Auto-imported composables
├── utils/          # Auto-imported utilities (e.g., cn() for classes)
├── styles/         # Global CSS (Tailwind v4 import)
└── shims/          # Generated TypeScript declarations
```
