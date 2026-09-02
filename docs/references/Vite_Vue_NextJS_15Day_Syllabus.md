# Vite + Vue + Next.js — 15-Day Supplementary Syllabus
### A Continuity Track for After Golden Canon v8 Slim

**Position:** Begins only after Slim 30 (S01–E7) is complete. A standalone 15-day track — not a mega-course, not a full-stack capstone, a focused zero-to-hero pass on three specific tools.
**Validated against:** roadmap.sh's Vue Developer roadmap, and current (2026) release documentation for Vite 8 and Next.js 16 — both of which changed significantly enough this year that older tutorials are actively misleading; see the Currency Note at the end.
**Continuity commitment:** every day below explicitly connects back to something Slim 30 already taught, and every new gate/standing rule established across S01–S04's revisions applies here too — the fast-reference-survey-before-mechanism principle, Diamond Framework structure, Odin-style "what's genuinely new vs. reinforcement" framing, and full QA validation when each day is actually written.

---

## Why these three, in this order

You already know **Vite** a little — Session S12 used it to scaffold the React SPA (`npm create vite`, the dev server, the `/api` proxy to the Express BFF). This track's Vite days don't re-teach that; they go underneath it. You know **React** deeply by the time you arrive here, which makes **Vue** the ideal next stop — not because Vue is hard, but because *learning it by contrast against something you already know cold* is the fastest way to actually understand it, rather than just accumulate a second syntax. **Next.js** comes last because it's the one piece of this track with the most direct bearing on your actual engineering judgment going forward: Slim 30 deliberately built the BIA UI as a client-rendered SPA behind an Express BFF and explicitly skipped Next.js as unnecessary for that specific product. Finishing this track means you'll know *exactly* why that choice was right for BIA, and exactly when the opposite choice — reaching for Next.js — would be right instead.

---

## Honest Scope Math

| Part | Days | Builds on |
|---|---|---|
| Part 1 — Vite, Beyond the Scaffold | 3 | Session S12's basic Vite setup |
| Part 2 — Vue 3, Zero to Hero by Contrast | 6 | Sessions S13–S19 (React) |
| Part 3 — Next.js 16, Why and How | 6 | Part 1 + Sessions S13–S19 (React) |
| **TOTAL** | **15** | |

---

## PART 1 — Vite, Beyond the Scaffold (3 days)

### Day 1 — Why Vite Is Fast: Native ESM, Rolldown, and What Changed in 2026

**Core concept.** Session S12 had you run `npm create vite@latest` and move on. This day opens the box. Before Vite, dev servers worked by bundling your *entire* application before serving a single byte — on a large app, that could mean a multi-second wait just to see your first change. Vite's original insight: browsers now support ES modules natively, so instead of bundling everything up front, Vite can serve your source files directly over native `import` statements and let the browser request only what a given page actually needs, transforming each file on the fly as it's requested. This is why Vite's dev server starts in milliseconds regardless of project size — it was never bundling the whole app to begin with.

**What changed, and why it matters that you learn the current version, not an older one.** For years, Vite ran on *two* separate tools: esbuild (a fast Go-based tool) pre-bundled your `node_modules` dependencies for dev, while Rollup (a JavaScript-based bundler) handled production builds — two different pipelines, two different plugin systems, and a real, documented class of bugs where something worked in `vite dev` and broke in `vite build` because the two tools didn't behave identically. **As of Vite 8 (shipped March 2026), this split is gone.** A new Rust-based bundler called **Rolldown** now handles *both* dev-time dependency pre-bundling and production builds through one unified pipeline, with one plugin API — eliminating the entire "works in dev, breaks in prod" bug class by construction, not by more careful testing. If you read a Vite tutorial written before 2026, it will describe the old two-tool split as current — it no longer is.

**Dependency pre-bundling, explained properly.** When Vite's dev server starts, it scans your source code for "bare imports" (`import React from 'react'` — anything resolved from `node_modules`, as opposed to your own relative-path files) and pre-bundles just those into single, cacheable files, converting any CommonJS/UMD packages to ESM in the process. This cache lives in `node_modules/.vite` and is invalidated automatically when your lockfile, `vite.config`, or `NODE_ENV` changes — not on every request.

**Production build, explained properly.** `vite build` runs Rolldown in full-bundle mode: tree-shaking (removing code you never actually use), code-splitting (breaking your app into smaller chunks loaded on demand), and minification, producing the static files you'd actually deploy.

**Mini-exercise.** Run `npm create vite@latest` fresh, then run `npm run dev` and note the startup time reported in the terminal. Add a deliberately large dependency (e.g., `lodash` imported in full, not tree-shaken). Restart the dev server and observe the one-time re-pre-bundling step Vite performs, logged to the terminal. This is the mechanism from the paragraph above, not just a fact about it.

**References:** Vite official docs — Features (`vite.dev/guide/features`) and Dependency Pre-Bundling (`vite.dev/guide/dep-pre-bundling`); the Vite 8 announcement (`vite.dev/blog/announcing-vite8`) for the Rolldown unification specifically.

---

### Day 2 — `vite.config` in Full, and the Plugin System

**Core concept.** Session S12 used Vite's defaults. This day is the actual configuration surface: `resolve.alias` (the `@/` import shortcuts you've seen in shadcn/UI code without knowing where they came from), `server.proxy` (the exact mechanism behind S12's `/api` proxy to the Express BFF, now explained rather than copied), `define` (compile-time constant replacement), and `build.rollupOptions` (now Rolldown options under the hood, kept backward-compatible in naming).

**The plugin system.** Vite's plugin API is a superset of Rollup's, meaning most Rollup plugins work in Vite with no changes, plus Vite-specific hooks for dev-server-only behavior. A plugin is fundamentally an object with named hook functions (`transform`, `resolveId`, `configureServer`) that Vite calls at specific points in its pipeline. You will not write a production plugin today — you'll read and correctly explain what three official plugins (`@vitejs/plugin-react`, `@vitejs/plugin-vue`, `vite-plugin-svgr`) each actually do at the hook level, then write one deliberately trivial custom plugin (a `transform` hook that uppercases a specific comment marker in any `.txt` file) purely to prove the mental model, not because it's useful.

**Environment variables, properly.** `import.meta.env.VITE_*` — only variables prefixed `VITE_` are exposed to client code; everything else stays server/build-time only, a deliberate security boundary preventing accidental secret leakage into a shipped bundle. `.env`, `.env.local`, `.env.production` — Vite's file-based environment layering, and which of these belongs in `.gitignore` (all the `.local` ones, always).

**Mini-exercise.** Add a `resolve.alias` for `@` pointing at `./src`, then rewrite one relative import (`../../components/Button`) to use it (`@/components/Button`). Add a `VITE_API_URL` variable across `.env` and `.env.local`, log both from client code, and confirm `.env.local` correctly overrides `.env` — the same override-priority mental model as CSS's cascade from Session S02, applied to a completely different system.

**References:** Vite docs — Config (`vite.dev/config`), Plugin API (`vite.dev/guide/api-plugin`), Env Variables and Modes (`vite.dev/guide/env-and-mode`).

---

### Day 3 — Vitest, Library Mode, and Vite vs. the Alternatives

**Core concept.** Vitest — the test runner referenced throughout Slim 30's optional testing track — is built directly on Vite's transform pipeline, which is *why* it shares Vite's config file and requires zero separate bundler setup for tests. Today closes that loop explicitly: run a real Vitest suite against a small Vite project and see the shared-config mechanism, rather than treating Vitest as an unrelated tool that happens to have a similar name.

**Library mode.** So far, every Vite project has been an *application* (something you deploy and run). `build.lib` configures Vite to instead produce a *reusable package* — an npm library other projects import — with proper ESM/CJS dual output and externalized peer dependencies (React itself, for instance, shouldn't be bundled *into* a component library; it should be declared as something the consumer provides).

**Vite vs. the alternatives, compared honestly.** Create React App (CRA) is deprecated and no longer recommended by the React team itself — knowing this matters because a large fraction of existing React tutorials online still teach it. Webpack remains extremely common in older, larger codebases but is slower in dev and requires far more configuration for equivalent results. Turbopack (Next.js's own bundler, covered properly in Part 3) is a *different* Rust-based tool built specifically for Next.js's needs, not a Vite competitor in the general sense — the two solve overlapping but not identical problems.

**Mini-exercise (also this part's capstone).** Take the actual `vite.config.ts` from your Session S12 React SPA, annotate every single line with a one-sentence explanation of what it does and why — using nothing from memory, only what Days 1–3 covered. If any line can't be explained this way, that's the signal to revisit that specific day before moving to Vue.

**References:** Vitest docs (`vitest.dev`, specifically the "Why Vitest" page for the shared-config mechanism); Vite docs — Library Mode (`vite.dev/guide/build.html#library-mode`).

---

## PART 2 — Vue 3, Zero to Hero by Contrast (6 days)

### Day 4 — Vue's Core Philosophy and the Reactivity System

**Core concept, taught by direct contrast with what you already know.** React's model: state changes trigger a re-render of the component function, and the framework diffs the resulting virtual DOM to figure out what actually changed on screen. Vue's model is different at the root: Vue wraps your data in a **reactive Proxy** (JavaScript's native `Proxy` object, intercepting property reads and writes), and when a reactive property is *read* inside a component's render, Vue records that dependency; when it's later *written to*, Vue already knows precisely which parts of the DOM depend on it and updates only those, directly — no virtual-DOM diff of the whole component required. This is a genuinely different mental model, not just different syntax for the same idea, and naming that difference explicitly up front is what will keep you from unconsciously trying to write "React with Vue syntax" for the next five days.

**`ref` and `reactive` — the two primitives.**
```javascript
import { ref, reactive } from 'vue'

const count = ref(0)          // wraps a single primitive value
count.value++                  // must access/mutate via .value

const user = reactive({ name: 'Soutrik', age: 30 })  // wraps an object
user.age++                     // direct property access, no .value needed
```
`ref` exists specifically because a Proxy can't intercept reads/writes to a bare primitive (a number has no properties to intercept) — wrapping it in an object with a `.value` property gives the Proxy something to observe. `reactive` wraps an object directly and needs no `.value` because object property access is exactly what a Proxy *can* intercept natively. **The single most common beginner mistake in Vue, worth flagging now rather than after you've made it:** destructuring a `reactive` object (`const { age } = user`) breaks reactivity immediately — the destructured `age` is a plain, disconnected number the moment it's extracted, no longer tracked by the Proxy at all.

**`computed` and `watch`.**
```javascript
const doubled = computed(() => count.value * 2)   // derived, cached, auto-updates

watch(count, (newVal, oldVal) => {
  console.log(`changed from ${oldVal} to ${newVal}`)
})
```
`computed` is Vue's direct equivalent of React's `useMemo` — a derived value, recalculated only when its dependencies actually change. `watch` has no precise one-to-one React equivalent; it's closer to a `useEffect` that runs specifically in response to one named reactive source changing, rather than after every render where a dependency array entry differs.

**Mini-exercise.** Build a counter using `ref`, then deliberately reproduce the destructuring bug described above with a `reactive` object, observe it break, and fix it using `toRefs` (which converts a reactive object's properties into individually reactive `ref`s that survive destructuring).

**References:** Vue.js official guide — Reactivity Fundamentals (`vuejs.org/guide/essentials/reactivity-fundamentals`); Vue.js — Reactivity in Depth (`vuejs.org/guide/extras/reactivity-in-depth`) for the Proxy mechanism specifically.

---

### Day 5 — Single-File Components and Template Directives

**Core concept.** A Vue Single-File Component (`.vue` file) bundles template, script, and scoped styles in one file — structurally similar to how a React component file mixes JSX and logic, but with genuinely separate, clearly delimited sections rather than JSX's "it's all JavaScript" approach:
```vue
<script setup>
import { ref } from 'vue'
const message = ref('Hello')
</script>

<template>
  <p>{{ message }}</p>
</template>

<style scoped>
p { color: var(--primary); }
</style>
```
`<script setup>` is the modern, current-recommended syntax (as opposed to the older Options API, which organizes code by option type — `data`, `methods`, `computed` — rather than by logical feature; new Vue code should use `<script setup>`, and most Vue code you'll encounter in current tutorials and job postings will too). `<style scoped>` automatically scopes CSS to only this component, via an attribute-selector mechanism the browser enforces — a built-in answer to the "how do I stop my CSS from leaking" question Session S02's specificity lessons never fully solved for you in plain CSS.

**Directives — Vue's template mini-language, mapped against JSX's plain-JavaScript approach.**

| Vue directive | Purpose | React/JSX equivalent |
|---|---|---|
| `{{ expression }}` | Text interpolation | `{expression}` |
| `v-bind:href="url"` (or `:href="url"`) | Bind an attribute to a value | `href={url}` |
| `v-on:click="handler"` (or `@click="handler"`) | Event listener | `onClick={handler}` |
| `v-if` / `v-else-if` / `v-else` | Conditional rendering | `{condition && <X/>}` or a ternary |
| `v-show` | Toggles CSS `display`, element stays in DOM | Manual `style={{display: ...}}` |
| `v-for="item in items"` | List rendering | `.map()` |
| `v-model="value"` | Two-way input binding | Manually paired `value`+`onChange` |

**`v-if` vs. `v-show`, explained with the same mechanism you already know.** `v-if` fully adds/removes the element from the DOM — this is Session S02's `display: none` mechanism, exactly. `v-show` only toggles CSS visibility while the element stays mounted — this is closer to `visibility: hidden`, except it specifically manipulates `display: none` under the hood rather than `visibility`, so it's really "cheap re-toggling of the same display:none mechanism," not a different property. Use `v-show` for something toggled frequently (a tab panel); use `v-if` for something rarely shown at all (an error state) — the same "which mechanism actually fits this job" reasoning Session S03 already trained.

**`v-model` — the one genuinely new idea here.** Two-way binding: `<input v-model="text">` is shorthand for binding the input's value to `text` *and* wiring an event listener that updates `text` on every keystroke — React deliberately has no equivalent built-in, because React's philosophy insists you wire the value and the change handler explicitly and separately, precisely to make data flow traceable. Vue's philosophy trades a little of that explicitness for less boilerplate on the extremely common case of "a form field bound to a variable."

**Mini-exercise.** Rebuild the BIA chat UI's message input (from Session S04's capstone) as a Vue SFC: a `v-model`-bound textarea, a `v-for` loop rendering a static array of three sample messages with `v-if`/`v-else` distinguishing user vs. assistant styling, and `v-on:click` wiring a Send button. Do not use React or JSX anywhere in this file — the goal is feeling the directive-based template model directly, not translating JSX mentally line by line.

**References:** Vue.js guide — Template Syntax (`vuejs.org/guide/essentials/template-syntax`) and Conditional Rendering / List Rendering (adjacent pages in the same guide section).

---

### Day 6 — The Composition API in Depth: Composables

**Core concept.** A **composable** is Vue's direct equivalent of a React custom hook: a function using `ref`/`reactive`/`computed`/`watch` internally, extracted so multiple components can reuse the same stateful logic.
```javascript
// useCounter.js
import { ref } from 'vue'

export function useCounter(initial = 0) {
  const count = ref(initial)
  function increment() { count.value++ }
  return { count, increment }
}
```
```vue
<script setup>
import { useCounter } from './useCounter'
const { count, increment } = useCounter(10)
</script>
```
The naming convention (`useX`), the "just a function" implementation, and the composition-over-inheritance philosophy are close enough to React hooks that most of Session S14's custom-hook-design lessons transfer directly — the syntax differs, the design thinking doesn't.

**Lifecycle hooks — Vue's `useEffect` equivalent, but split by moment instead of unified.** Where React collapses "run on mount," "run on update," and "run on unmount" into one `useEffect` with a dependency array, Vue gives each moment its own named function: `onMounted`, `onUpdated`, `onUnmounted`, `onBeforeMount`, and others. This is a real design difference worth sitting with rather than memorizing as a 1:1 mapping — Vue's approach is more explicit about *which* moment you mean; React's approach unifies them at the cost of the dependency-array reasoning Session S14 spent real effort teaching you to get right.

**`provide`/`inject` — Vue's Context.** Structurally identical in purpose to React's Context (Session S15): a value made available to an entire subtree without prop-drilling, provided by an ancestor via `provide(key, value)` and read by any descendant via `inject(key)`.

**Mini-exercise.** Extract a `useLocalStorage` composable (mirroring the `usehooks-ts` pattern from Session S14) that persists a `ref`'s value to `localStorage` and rehydrates it on mount using `onMounted`. Use it in two different components simultaneously and confirm both share the same persisted value — the same "composable as a genuinely reusable unit" test Session S14 applied to custom hooks.

**References:** Vue.js guide — Composables (`vuejs.org/guide/reusability/composables`); Lifecycle Hooks (`vuejs.org/guide/essentials/lifecycle`); Provide/Inject (`vuejs.org/guide/components/provide-inject`).

---

### Day 7 — Component Communication: Props, Emits, and Slots

**Core concept.** Props flow down, exactly as in React — with one syntactic difference worth naming: Vue distinguishes *emitting an event upward* from *calling a passed-down function*, whereas React uses the same mechanism (a function prop) for both.
```vue
<!-- Child.vue -->
<script setup>
defineProps(['label'])
const emit = defineEmits(['submit'])
function handleClick() { emit('submit', 'some payload') }
</script>
<template>
  <button @click="handleClick">{{ label }}</button>
</template>
```
```vue
<!-- Parent.vue -->
<Child label="Send" @submit="onSubmit" />
```
`defineEmits` explicitly declares which events a component can emit — a small but real discoverability win over React's "any prop might secretly be a callback" convention; reading a Vue component's `defineEmits` list tells you its complete public event API without reading the implementation.

**Slots — content projection, with no direct one-line React equivalent.** React's `children` prop lets a parent pass arbitrary JSX into a component's render output at one designated spot. Vue's slots generalize this to *multiple, named* insertion points:
```vue
<!-- Card.vue -->
<template>
  <div class="card">
    <header><slot name="header"></slot></header>
    <main><slot></slot></main>
  </div>
</template>
```
```vue
<Card>
  <template #header><h2>Title</h2></template>
  <p>Default slot content goes here.</p>
</Card>
```
This is genuinely more capable than a single `children` prop for building flexible UI-library-style components — the closest React analogue is a component accepting several distinct named-JSX props (`headerSlot`, `footerSlot`), which is a workaround for something Vue's template syntax handles natively.

**Mini-exercise.** Build a `ChatBubble.vue` component with a default slot for the message text and a named `actions` slot for hover-revealed action icons (copy, edit) — directly mirroring the structure of the BIA chat UI's `MessageActions` pattern from Slim 30's React sessions, translated into Vue's slot model rather than React's children/prop model.

**References:** Vue.js guide — Props (`vuejs.org/guide/components/props`), Component Events (`vuejs.org/guide/components/events`), Slots (`vuejs.org/guide/components/slots`).

---

### Day 8 — Vue Router and Pinia

**Core concept.** Vue Router is the direct equivalent of Session S16's React Router: file-agnostic, config-based client-side routing.
```javascript
const routes = [
  { path: '/', component: Home },
  { path: '/chat/:id', component: Chat, props: true },
]
```
`useRoute()` and `useRouter()` are Vue's equivalents of `useParams`/`useLocation` and `useNavigate` respectively — the concepts (dynamic segments, programmatic navigation, route params) transfer directly from Session S16; only the specific hook names differ.

**Pinia — the current official state-management library, replacing the older Vuex.** Pinia is closer in spirit to Zustand than to Redux: minimal boilerplate, TypeScript-friendly by default, no action-type-constants ceremony.
```javascript
// store/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({ count: 0 }),
  actions: {
    increment() { this.count++ }
  }
})
```
```vue
<script setup>
import { useCounterStore } from './store/counter'
const counter = useCounterStore()
</script>
<template>
  <button @click="counter.increment">{{ counter.count }}</button>
</template>
```
A Pinia store is conceptually a global composable — same underlying `ref`/`reactive` mechanism from Day 4, just declared once and shared app-wide rather than scoped to one component.

**Mini-exercise (Part 2's near-capstone).** Build a two-route Vue app (a chat list page and a chat detail page, using dynamic route params) with a Pinia store holding the list of chats, and a `Card.vue` component (Day 7's slots) rendering each chat preview.

**References:** Vue Router docs (`router.vuejs.org`); Pinia docs (`pinia.vuejs.org`), specifically its own "Vuex vs. Pinia" comparison page for the historical context.

---

### Day 9 — Vue Capstone: The BIA Chat List, Rebuilt

**The build.** Using only Days 4–8, rebuild a scoped-down piece of the BIA chat UI in Vue: a chat history sidebar (Pinia store + `v-for`), a chat detail view (Vue Router dynamic route + `ref`-based message list), a `ChatBubble.vue` component (Day 7's slots), and a `useLocalStorage` composable (Day 6) persisting the draft input text. This deliberately mirrors a slice of what Sessions S13–S19 already built in React — not as busywork, but so the final reflection below has two concrete, comparable implementations to actually compare rather than comparing memory against abstraction.

**Closing reflection — write this down, don't just think it.** In your own words: which felt more natural, Vue's template-directive model or JSX's "it's all just JavaScript" model, and why? Where did Vue's explicit `defineEmits` API feel like a real discoverability win over React's implicit callback-prop convention? Where did React's unified `useEffect` feel simpler than juggling `onMounted`/`onUpdated`/`onUnmounted` separately? There's no correct answer being tested here — the value is in having a informed, specific opinion instead of a vague "they're similar" impression, which is exactly the kind of engineering judgment a senior developer is expected to have about the tools in their ecosystem.

**Production relevance, stated honestly.** React has substantially more job listings and library ecosystem breadth in the US market specifically; Vue holds a genuinely strong, sometimes dominant position in parts of Europe and Asia, and among teams that specifically prefer its template syntax over JSX. Neither framework is "winning" in any global sense as of 2026 — this is a real, current market split, not a decline narrative for either side.

---

## PART 3 — Next.js 16, Why and How (6 days)

### Day 10 — Why Next.js Exists: The Rendering Spectrum, Direct Comparison to Slim 30's Choice

**Core concept.** Every one of Sessions S13–S19's React work produced a **client-side rendered (CSR) single-page application**: the browser downloads a mostly-empty HTML shell, then JavaScript takes over entirely, fetching data and rendering everything client-side. This is the right choice for an authenticated, always-logged-in internal tool like the BIA chat UI — nobody needs the chat interface to be indexed by Google, and every user is already authenticated before they see a single pixel of it. Next.js exists because *not every product fits that profile.*

**The full spectrum, defined precisely, not just named:**
- **CSR** (what Slim 30 built): nothing meaningful in the initial HTML; everything renders after JS loads and runs.
- **SSR (Server-Side Rendering):** the server renders full HTML *per request*, sent to the browser already populated — better for SEO and first-paint speed, at the cost of a server round-trip on every navigation.
- **SSG (Static Site Generation):** HTML is rendered once, at *build* time, and served as a static file forever after — fastest possible delivery, but the content can only change by rebuilding.
- **ISR (Incremental Static Regeneration):** SSG's speed, with a background revalidation window — the static file is regenerated periodically or on-demand, without a full rebuild.

**The direct, honest comparison you were promised back in the mega-course planning conversation, now delivered for real:** Next.js is not "better than React" — Next.js *is* React, with a rendering and routing layer built on top. It earns its complexity specifically when a product needs SEO (public, unauthenticated, search-indexable pages), fast first paint for anonymous visitors who haven't downloaded your JS bundle yet, or content that changes but not on every single request (ISR's exact use case). The BIA chat UI has none of these properties — every user is authenticated, nothing is search-indexed, and the interface is genuinely more interactive than "mostly static content that occasionally updates." Slim 30's SPA choice was correct *for that product*, not correct in general — and now you have the vocabulary to make that same judgment call on a different product without anyone telling you the answer.

**Mini-exercise.** For each of these three hypothetical products, decide CSR, SSR, SSG, or ISR, and justify it in one sentence using the definitions above, not vibes: (1) a public marketing landing page for a SaaS product; (2) an internal admin dashboard behind SSO; (3) a blog that publishes a new post roughly once a week.

**References:** Next.js docs — Rendering (`nextjs.org/docs/app/building-your-application/rendering`); web.dev — Rendering on the Web (`web.dev/articles/rendering-on-the-web`) for the vendor-neutral version of the same spectrum.

---

### Day 11 — App Router, Turbopack, and Server vs. Client Components

**Core concept, and a currency note that matters.** As of Next.js 16 (stable since October 2025, current release 16.x as of this writing), **Turbopack — a Rust-based bundler built specifically for Next.js — is the default bundler for both `next dev` and `next build`**, replacing Webpack entirely as the default. If you read a Next.js tutorial that configures Webpack manually or treats Turbopack as an opt-in experimental flag, it's describing an older version; today's default requires no bundler configuration at all for the common case.

**File-based routing.** The `app/` directory's folder structure *is* your route structure: `app/chat/[id]/page.tsx` produces the route `/chat/:id`, with `[id]` as a dynamic segment. `layout.tsx` files wrap every page beneath them in shared UI (persisting across navigations, unlike a `page.tsx` which fully remounts). `loading.tsx` and `error.tsx` are conventions for automatic loading and error states per route segment — no manual Suspense/ErrorBoundary wiring required for the common case, though both remain available for finer control.

**Server Components vs. Client Components — the one genuinely new architectural idea in this entire 15-day track.** Every component you wrote in Sessions S13–S19 was, structurally, a Client Component — it runs in the browser. In the Next.js App Router, **components are Server Components by default**: they run *only* on the server, are never shipped to the browser as JavaScript at all, and can directly perform server-side work (reading a database, reading a file, using secrets) with no API layer in between. Adding `"use client"` at the top of a file opts that component — and everything it imports — back into the familiar browser-side model, with `useState`, `onClick`, and everything else from Slim 30 available exactly as before.

```tsx
// app/chat/[id]/page.tsx — Server Component by default, no "use client"
export default async function ChatPage({ params }: { params: { id: string } }) {
  const chat = await getChatById(params.id)   // runs on the server, directly
  return <ChatView chat={chat} />
}
```
```tsx
// ChatView.tsx
"use client"
export function ChatView({ chat }) {
  const [input, setInput] = useState("")       // useState requires the client
  return <div>...</div>
}
```

**Why this matters architecturally, connected back to Slim 30 directly.** The BFF pattern (Sessions S20–S23) exists specifically to let a client-rendered app safely talk to a database and inject auth headers the browser can't produce itself. A Server Component can do a meaningful slice of that same job *without* a separate BFF process at all — it's genuinely a different way of drawing the same architectural boundary, not a strictly better or worse one. Recognizing that these are two different solutions to a related problem, rather than treating Server Components as "how you'd rebuild the BFF in Next.js," is the actual point of this paragraph.

**Mini-exercise.** Build a two-file App Router page: a Server Component `page.tsx` that awaits a fake async `getChatById` (a `setTimeout`-wrapped Promise standing in for a real database call) and passes the result as a prop to a `"use client"` child component that renders an interactive `useState`-driven input. Confirm in the browser's Network tab that the Server Component's own code never appears in any downloaded JS bundle.

**References:** Next.js docs — App Router (`nextjs.org/docs/app`); Next.js docs — Turbopack (`nextjs.org/docs/app/api-reference/turbopack`); React docs — Server Components (`react.dev/reference/rsc/server-components`), since the concept itself originates in React, not Next.js specifically.

---

### Day 12 — Data Fetching, Caching, and Server Actions

**Core concept, with a currency note.** Next.js extends the native `fetch` API with its own caching layer, but the *specific caching model* changed meaningfully in Next.js 16: the newer **Cache Components** model (built on Partial Pre-Rendering) uses an explicit `use cache` directive to mark what should be cached, replacing the older implicit-by-default caching behavior most Next.js 13/14 tutorials describe. If a resource you read online treats fetch caching as "on by default everywhere unless opted out," it predates this shift — the current model is closer to "explicit opt-in for what you actually want cached."

```tsx
async function getChats() {
  'use cache'
  const res = await fetch('https://api.example.com/chats')
  return res.json()
}
```

**Server Actions — mutating data without a separate API route, the single most genuinely novel idea in this day.** A Server Action is a function, marked `'use server'`, that a Client Component can call directly as if it were a normal function call — Next.js handles the network round-trip transparently.
```tsx
// actions.ts
'use server'
export async function createPost(formData: FormData) {
  const title = formData.get('title')
  await db.insert(posts).values({ title })
  revalidatePath('/posts')
}
```
```tsx
"use client"
import { createPost } from './actions'
export function PostForm() {
  return <form action={createPost}><input name="title" /><button>Post</button></form>
}
```
Compare this directly against Session S23's Express BFF routes: both exist to let client-side code safely trigger a server-side mutation, but a Server Action collapses "define an API route, write a client-side fetch call to it, handle the response" into one function with no separate route to maintain — a genuinely different division of labor for the same underlying job.

**`revalidatePath`/`revalidateTag`.** After a mutation, these tell Next.js "the cached data at this path/tag is now stale, refetch it" — the App Router's answer to the same "how do I know my UI reflects the latest data after a write" problem Session S17's SWR `mutate()` already solved for the SPA pattern, solved here through server-driven cache invalidation instead of client-driven refetching.

**Mini-exercise.** Build a Server Action that appends a message to an in-memory array (a stand-in for a real database) and calls `revalidatePath`, wired to a `<form>` in a Client Component. Confirm the page shows the new message after submission without you writing any client-side `fetch` call at all.

**References:** Next.js docs — Server Actions and Mutations (`nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations`); Next.js docs — Caching (search the current docs directly for "Cache Components," since this is one of the fastest-moving parts of the framework and the canonical page location has shifted across recent releases).

---

### Day 13 — Middleware (Now `proxy.ts`), `next/image`, `next/font`, and Route Handlers

**Core concept, with a naming currency note that will confuse you if you don't know it up front.** What earlier Next.js versions called `middleware.ts` — a function intercepting every matching request before it reaches a route, used for auth gating and redirects — **was renamed to `proxy.ts` in Next.js 16**, specifically to clarify that this code runs at the network boundary rather than implying it's a general-purpose request pipeline. The underlying concept and API are the same; only the filename and framing changed. Expect to see `middleware.ts` in most existing tutorials and codebases for a while yet — know both names refer to the same mechanism.

```tsx
// proxy.ts (formerly middleware.ts)
import { NextResponse } from 'next/server'

export function proxy(request) {
  const isAuthed = request.cookies.has('session')
  if (!isAuthed && request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.redirect(new URL('/login', request.url))
  }
}
```
This is the App Router's answer to Session S19's `requireAuth` guard pattern — same job (gate a route based on auth state), implemented at a different layer (before the route even renders, rather than inside the BFF's Express middleware chain).

**`next/image` and `next/font`.** Both solve real, specific problems you've already met the hard way: `next/image` automatically serves correctly-sized, lazy-loaded images and prevents the layout-shift problem Session S04's skeleton-loader lesson covered in detail — this time solved by the framework, not by manually matching skeleton dimensions to real content. `next/font` loads fonts with zero layout shift by inlining font-loading CSS at build time, rather than a font swapping in visibly after the page has already rendered with a fallback.

**Route Handlers — when you do want a traditional API route.** Not every server-side operation fits the Server Action model (webhooks from an external service, for instance, need a real HTTP endpoint with a stable URL). `app/api/webhook/route.ts` exporting a `POST` function is Next.js's direct equivalent of an Express route handler, for exactly the cases where that pattern is still the right one.

**Mini-exercise.** Build a `proxy.ts` gating `/dashboard/*` behind a cookie check, a Route Handler at `/api/health` returning a JSON status object (directly comparable to Session S23's BFF health-check route), and a page using both `next/image` (with an intentionally large source image, confirming the served size is optimized) and `next/font`.

**References:** Next.js docs — Proxy (search current docs for "proxy," since this page's URL and exact name are recent as of the Next.js 16 rename); `next/image` and `next/font` API reference (`nextjs.org/docs/app/api-reference/components`); Route Handlers (`nextjs.org/docs/app/building-your-application/routing/route-handlers`).

---

### Day 14 — Auth.js and Deployment

**Core concept.** Auth.js (formerly NextAuth.js) is the standard authentication library for Next.js — session strategy configuration, OAuth provider setup (Google, GitHub), and its own middleware/proxy integration for gating routes based on session state, directly comparable in purpose to Slim 30's Bouncer-issued session cookie, implemented instead as something *you* configure rather than infrastructure someone else runs for you.

**Deployment to Vercel.** Push to a connected Git repository; Vercel builds and deploys automatically, with a unique preview URL for every branch/PR before anything reaches production — a meaningfully different deployment model from Slim 30's Docker-plus-Kubernetes pipeline (Session S28), optimized specifically for the App Router's Server Components and edge-function model rather than a generic containerized service.

**Self-hosting, briefly, and why you might still need Slim 30's Docker knowledge.** Next.js can run outside Vercel entirely — as a standalone Node.js server, or in Docker — for teams with infrastructure requirements Vercel doesn't fit. This is precisely where Session S28's Docker multi-stage build knowledge transfers directly: a self-hosted Next.js app is, at the container level, not meaningfully different from any other Node.js service you've already learned to containerize.

**Mini-exercise.** Deploy the Day 13 mini-project to Vercel from a fresh Git repository, confirm the preview-URL-per-branch behavior by pushing to a second branch, and separately write (without necessarily running) a Dockerfile for the same app using Slim 30 S28's multi-stage pattern, explicitly noting which stages differ from a plain Express service and why (the build stage needs to run `next build`, not just copy source files).

**References:** Auth.js docs (`authjs.dev`); Next.js docs — Deploying (`nextjs.org/docs/app/building-your-application/deploying`).

---

### Day 15 — Capstone: The BIA Public Share Page, Rebuilt in Next.js

**The build, and why this specific feature.** The BIA chat UI, as built in Slim 30, has no public-facing page at all — by design, every screen requires authentication. Today's capstone asks: what would a *public, shareable* piece of this product look like, and why would you reach for Next.js specifically to build it? Build a single feature: a public "shared conversation" page (`app/share/[id]/page.tsx`) that Server-Component-fetches a (mocked) read-only conversation transcript, renders it with real Open Graph metadata (so a shared link shows a rich preview when pasted into Slack or iMessage — the Metadata API doing real, visible work here, not an abstract feature), uses `next/image` for any avatar images, and includes one interactive island — a "Copy link" button — as a small `"use client"` component embedded in an otherwise fully server-rendered page.

**Closing reflection for the entire 15-day track, write this down.** In one paragraph each: (1) what specifically makes Vite fast, in your own words, without looking back at Day 1; (2) the one thing about Vue's reactivity model that felt genuinely different from React, not just differently spelled; (3) the specific product property (not "it's more powerful") that would make you reach for Next.js over Slim 30's SPA+BFF pattern on a real future project. If you can write all three without opening this document, the track did its job.

---

## Currency Note

This syllabus was written in August 2026 and deliberately calls out three places where the *current* correct answer differs from what an older tutorial, or a language model's own training-data-era default, would say: **Vite 8's unification of esbuild and Rollup into a single Rolldown bundler** (shipped March 2026 — the biggest architectural change in Vite's history), **Next.js 16 making Turbopack the default bundler for both dev and build** (previously an opt-in flag), and **the `middleware.ts` → `proxy.ts` rename**, alongside the shift toward explicit `use cache`-based Cache Components over the older implicit fetch-caching model. All three were verified against official current documentation and 2026-dated release notes during this syllabus's research, not recalled from memory — and all three are exactly the kind of fact worth re-verifying again yourself when you actually sit down to build each day, since both Vite and Next.js are shipping fast enough that "current as of August 2026" has a real expiration date.
