# Frontend Development Learning Path

A comprehensive guide to becoming a professional frontend developer using this project as foundation.

## Prerequisites

### Required Knowledge
- HTML5 fundamentals (semantic markup, forms, accessibility)
- CSS basics (box model, flexbox, grid, responsive design)
- JavaScript ES6+ (modules, destructuring, async/await, arrow functions)
- Git version control basics

### Recommended Tools
- VS Code with Vue/Vite extensions
- Node.js 18+ and pnpm
- Browser DevTools (Chrome/Firefox)
- Terminal familiarity

---

## Learning Modules

### Module 1: Modern Build Tools (Vite)

**Concepts**:
- ES modules vs CommonJS
- Hot Module Replacement (HMR)
- Build optimization & tree shaking
- Development vs production builds

**Practice**:
```bash
# Explore Vite features
npm run dev        # Watch HMR in action
npm run build      # Inspect dist/ output
npm run preview    # Test production build
```

**Deep Dive**:
- [vite.config.js](../vite.config.js) — Plugin system, build configuration
- Rolldown bundler (Rust-based Vite alternative)
- Source maps and debugging

**Resources**:
- [Vite Guide](https://vitejs.dev/guide/)
- [Why Vite?](https://vitejs.dev/guide/why.html)

---

### Module 2: Vue 3 Composition API

**Concepts**:
- Reactivity system (ref, reactive, computed)
- Component lifecycle (onMounted, onUnmounted, watch)
- `<script setup>` syntax sugar
- Props, emits, and component communication
- Composables (reusable logic)

**Practice**:
```vue
<!-- Create your first composable -->
<script setup>
import { ref, computed, onMounted } from 'vue'

// Reactive state
const count = ref(0)
const doubled = computed(() => count.value * 2)

// Lifecycle hook
onMounted(() => {
  console.log('Component mounted')
})
</script>
```

**Study Files**:
- [App.vue](../src/App.vue) — Root component structure
- [HelloWorld.vue](../src/components/HelloWorld.vue) — Props & reactivity

**Advanced Topics**:
- Provide/inject for dependency injection
- Teleport for portal-style rendering
- Suspense for async components
- Custom directives

**Resources**:
- [Vue 3 Composition API](https://vuejs.org/guide/extras/composition-api-faq.html)
- [Script Setup RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0040-script-setup.md)

---

### Module 3: Tailwind CSS v4 (Utility-First CSS)

**Concepts**:
- Utility-first methodology vs component CSS
- Design tokens and consistency
- Responsive modifiers (sm:, md:, lg:)
- State variants (hover:, focus:, active:)
- CSS-based configuration (Tailwind v4 feature)

**Setup** (Not yet configured in project):
```js
// vite.config.js
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [vue(), tailwindcss()]
})
```

```css
/* style.css */
@import "tailwindcss";
```

**Practice Examples**:
```vue
<!-- Responsive card component -->
<div class="p-4 sm:p-6 md:p-8 bg-white rounded-lg shadow-md hover:shadow-xl transition-shadow">
  <h2 class="text-xl sm:text-2xl md:text-3xl font-bold text-gray-900">
    Responsive Heading
  </h2>
  <p class="mt-2 text-sm sm:text-base text-gray-600">
    Content adapts to screen size
  </p>
</div>
```

**Exercises**:
1. Build a navigation bar (sticky, responsive menu)
2. Create a hero section with gradient background
3. Design a card grid (auto-responsive with grid)
4. Implement dark mode with `dark:` variants

**Advanced**:
- Custom CSS variables in Tailwind v4
- `@layer` for custom utilities
- Performance optimization (PurgeCSS automatic)

**Resources**:
- [Tailwind CSS v4 Docs](https://tailwindcss.com/docs)
- [Utility-First Fundamentals](https://tailwindcss.com/docs/utility-first)

---

### Module 4: Component Architecture

**Patterns**:

#### 1. Container/Presenter Pattern
```vue
<!-- Container (logic) -->
<script setup>
import { ref } from 'vue'
import UserList from './UserList.vue'

const users = ref([])
const loading = ref(true)

async function fetchUsers() {
  const res = await fetch('/api/users')
  users.value = await res.json()
  loading.value = false
}
</script>

<template>
  <UserList :users="users" :loading="loading" />
</template>

<!-- Presenter (UI only) -->
<script setup>
defineProps({
  users: Array,
  loading: Boolean
})
</script>

<template>
  <div v-if="loading">Loading...</div>
  <ul v-else>
    <li v-for="user in users" :key="user.id">{{ user.name }}</li>
  </ul>
</template>
```

#### 2. Composables for Reusable Logic
```js
// composables/useCounter.js
import { ref, computed } from 'vue'

export function useCounter(initial = 0) {
  const count = ref(initial)
  const doubled = computed(() => count.value * 2)

  function increment() {
    count.value++
  }

  return { count, doubled, increment }
}
```

**Best Practices**:
- Single Responsibility Principle (one job per component)
- Props down, events up (unidirectional data flow)
- Prop validation with TypeScript or PropTypes
- Scoped styles to prevent leakage

---

### Module 5: State Management

**When to Use**:
- Global state (user auth, theme)
- Shared state across routes
- Complex data flows

**Options**:

#### Pinia (Recommended for Vue 3)
```bash
npm install pinia
```

```js
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({ count: 0 }),
  actions: {
    increment() {
      this.count++
    }
  }
})
```

#### Provide/Inject (Built-in)
```vue
<!-- Parent -->
<script setup>
import { provide, ref } from 'vue'
const theme = ref('dark')
provide('theme', theme)
</script>

<!-- Child (any depth) -->
<script setup>
import { inject } from 'vue'
const theme = inject('theme')
</script>
```

---

### Module 6: Routing (Vue Router)

**Installation**:
```bash
npm install vue-router@4
```

**Setup**:
```js
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'

export default createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/', component: Home },
    { path: '/about', component: () => import('../views/About.vue') }
  ]
})
```

**Features to Master**:
- Dynamic routes (`/user/:id`)
- Nested routes
- Navigation guards (authentication)
- Lazy loading routes

---

### Module 7: API Integration

**Fetch Patterns**:
```js
// composables/useApi.js
import { ref } from 'vue'

export function useApi(url) {
  const data = ref(null)
  const error = ref(null)
  const loading = ref(false)

  async function fetch() {
    loading.value = true
    try {
      const res = await fetch(url)
      data.value = await res.json()
    } catch (e) {
      error.value = e
    } finally {
      loading.value = false
    }
  }

  return { data, error, loading, fetch }
}
```

**Advanced**:
- Axios for interceptors/retry logic
- TanStack Query (Vue Query) for caching
- WebSocket integration
- Error boundary patterns

---

### Module 8: Testing

**Unit Testing (Vitest)**:
```bash
npm install -D vitest @vue/test-utils
```

```js
// HelloWorld.test.js
import { mount } from '@vue/test-utils'
import HelloWorld from './HelloWorld.vue'

test('increments count on click', async () => {
  const wrapper = mount(HelloWorld, {
    props: { msg: 'Test' }
  })

  await wrapper.find('button').trigger('click')
  expect(wrapper.text()).toContain('count is 1')
})
```

**E2E Testing (Playwright)**:
```bash
npm install -D @playwright/test
```

**Component Testing**:
- Storybook for isolated development
- Visual regression with Percy/Chromatic

---

### Module 9: Performance Optimization

**Techniques**:
1. **Code Splitting**
   ```js
   const Heavy = defineAsyncComponent(() => import('./Heavy.vue'))
   ```

2. **Virtual Scrolling**
   ```bash
   npm install vue-virtual-scroller
   ```

3. **Lazy Images**
   ```vue
   <img loading="lazy" src="image.jpg" />
   ```

4. **Memoization**
   ```vue
   <script setup>
   import { computed } from 'vue'
   const expensive = computed(() => heavyCalculation())
   </script>
   ```

**Monitoring**:
- Lighthouse audits
- Vue DevTools performance tab
- Bundle analyzer

---

### Module 10: TypeScript Integration

**Setup**:
```bash
npm install -D typescript vue-tsc
```

**Migration**:
```vue
<script setup lang="ts">
interface Props {
  msg: string
  count?: number
}

const props = withDefaults(defineProps<Props>(), {
  count: 0
})

const emit = defineEmits<{
  update: [value: number]
}>()
</script>
```

**Benefits**:
- Type safety for props/events
- Better IDE autocomplete
- Catch errors at compile time

---

### Module 11: Accessibility (A11Y)

**Checklist**:
- ✅ Semantic HTML (`<nav>`, `<main>`, `<article>`)
- ✅ ARIA labels for interactive elements
- ✅ Keyboard navigation (Tab, Enter, Escape)
- ✅ Focus management
- ✅ Color contrast (WCAG AA minimum 4.5:1)
- ✅ Screen reader testing (NVDA, VoiceOver)

**Tools**:
- axe DevTools extension
- WAVE browser extension
- Lighthouse accessibility audit

---

### Module 12: Production Readiness

**Checklist**:
- [ ] Environment variables (.env, .env.production)
- [ ] Error tracking (Sentry, Rollbar)
- [ ] Analytics (Google Analytics, Plausible)
- [ ] SEO meta tags (vue-meta, vueuse/head)
- [ ] PWA setup (vite-plugin-pwa)
- [ ] Security headers (CSP, HSTS)
- [ ] Performance budgets
- [ ] CI/CD pipeline (GitHub Actions, GitLab CI)

**Deployment**:
```bash
# Build
npm run build

# Deploy to Netlify/Vercel/Cloudflare Pages
# (drag dist/ folder or connect git repo)
```

---

## Project-Based Learning Path

### Beginner Projects
1. **Todo App** — CRUD operations, local storage
2. **Weather Dashboard** — API integration, error handling
3. **Portfolio Site** — Responsive design, animations

### Intermediate Projects
4. **E-commerce Product Page** — State management, cart logic
5. **Blog with CMS** — Routing, markdown rendering
6. **Dashboard with Charts** — Data visualization, performance

### Advanced Projects
7. **Real-time Chat** — WebSockets, Pinia store
8. **Admin Panel** — Authentication, role-based access
9. **Component Library** — Documentation, testing, publishing

---

## Daily Practice Routine

**Week 1-2**: Vite + Vue fundamentals
- Build 3 small components daily
- Read Vue docs (1 section/day)

**Week 3-4**: Tailwind CSS mastery
- Recreate 1 UI design from Dribbble
- Practice responsive layouts

**Week 5-6**: State management + routing
- Build multi-page app
- Implement authentication flow

**Week 7-8**: Testing + performance
- Write tests for existing components
- Optimize bundle size

**Week 9-10**: TypeScript migration
- Convert project to TypeScript
- Add type definitions

**Week 11-12**: Production deployment
- Deploy to hosting platform
- Set up monitoring

---

## Resources

### Official Documentation
- [Vue 3](https://vuejs.org/)
- [Vite](https://vitejs.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Pinia](https://pinia.vuejs.org/)

### Courses
- [Vue Mastery](https://www.vuemastery.com/) (premium)
- [Frontend Masters](https://frontendmasters.com/) (Vue & performance)
- [Egghead.io](https://egghead.io/) (composables deep dive)

### Communities
- [Vue Discord](https://discord.com/invite/vue)
- [r/vuejs](https://reddit.com/r/vuejs)
- [Vue Forum](https://forum.vuejs.org/)

### Blogs
- [Vue.js Blog](https://blog.vuejs.org/)
- [CSS-Tricks](https://css-tricks.com/) (Tailwind tutorials)
- [web.dev](https://web.dev/) (performance, a11y)

---

## Next Steps

1. **Fix Tailwind Setup** — Follow code review recommendations
2. **Complete Module 1-3** — Build foundation
3. **Clone Real Designs** — Practice Tailwind + Vue together
4. **Contribute to Open Source** — Find Vue projects on GitHub
5. **Build Portfolio** — Showcase your projects

---

*Last updated: 2025-10-03 | Based on Vue 3.5, Vite 7, Tailwind CSS v4*
