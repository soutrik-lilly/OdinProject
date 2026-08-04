# Post-Slim-30 Roadmap: Two Mega Courses + Fusion Capstone

**Sequencing (confirmed):** Complete Golden Canon v8 Slim — all of S01 through E7 — before starting anything below. This document is the *plan*; full session-by-session content for Mega Course A and B gets written the same way Slim 30 did, one session at a time, only once you actually arrive there.

**Capstone (confirmed):** A social / networking application — working name **"the Social Platform Capstone"** below; rename it to whatever you like when you get there, the same way "Now & Not Now" became Apopax.

---

## Why this document exists before either course is written

You asked for two brand-new mega-courses. If built from scratch, they'd re-teach JavaScript, TypeScript, React, Node/Express, auth concepts, and ORM basics — all of which Slim 30 already covers in depth. That duplication is exactly the "shallow learning" risk you flagged. So both courses below are designed **additive-only**: every phase either covers ground Slim 30 never touches, or goes meaningfully deeper than Slim 30 went on purpose (Slim 30 was scoped tightly to what one BFF-pattern chat UI needs; these two courses exist to give you the *general* range a senior engineer has beyond that one pattern).

---

## Total Timeline Math (honest, not rounded up)

| Track | Days | At 1 session/day | At 2 sessions/day |
|---|---|---|---|
| Slim 30 (S01–E7) | 37 | ~5.5 weeks | ~3 weeks |
| Mega Course A (Node.js + TypeScript) | 35 | ~5 weeks | ~2.5 weeks |
| Mega Course B (React + Next.js) | 28 | ~4 weeks | ~2 weeks |
| **Total** | **100** | **~14.5 weeks (~3.3 months)** | **~7.5 weeks (~1.7 months)** |

Pace is your call session by session — you can run 1/day some weeks and 2/day others. This table exists so the scope is visible up front rather than discovered halfway through.

---

## MEGA COURSE A — Node.js + TypeScript: Scalable Systems Engineering

**Starts only after Slim 30's E7. Assumes:** JS async/event loop (Slim 30 S05–S09), TypeScript core (S10–S11), Express BFF patterns (S20, S23), JWT/OAuth concepts (S19), Drizzle ORM (S24), npm workspaces (S25), Docker/K8s ops (S28), Redis as a stream cache (S26).

**Does NOT re-teach:** any of the above. Every phase below is either genuinely new or goes past where Slim 30 deliberately stopped.

### Phase A0 — Orientation & Recap Bridge (1 day)
Quick diagnostic pass confirming the Slim 30 prerequisites above are solid before building on them; explicit map of "here's exactly what carries over, here's exactly what's new" so nothing feels like unexplained repetition.

### Phase A1 — TypeScript Type System Mastery (4 days)
- **A1.1** Generics deep dive: constraints, defaults, generic classes, variance, inference edge cases
- **A1.2** Conditional & mapped types: `infer`, template literal type gymnastics, recursive types, building your own `DeepPartial<T>`/`DeepReadonly<T>`
- **A1.3** Decorators & metadata: experimental decorators, `reflect-metadata`, the pattern NestJS-style dependency injection is built on
- **A1.4** Type-level testing: writing your own utility types from scratch (a mini `Pick`/`Omit`), `tsd`/`expect-type` for testing types themselves

### Phase A2 — Node.js Runtime Internals (5 days)
- **A2.1** The event loop's actual phases in Node specifically (timers → pending callbacks → poll → check → close callbacks) and libuv — one level deeper than Slim 30's JS-level event loop
- **A2.2** Streams: Readable/Writable/Transform/Duplex, backpressure, piping — the mechanism SSE parsing in Slim 30 quietly depended on, now explained from first principles
- **A2.3** Buffers and binary data
- **A2.4** `worker_threads` for CPU-bound parallelism (image processing, heavy computation off the main thread)
- **A2.5** `cluster` and `child_process` for multi-core scaling and process management

### Phase A3 — API Architecture Beyond One BFF (4 days)
- **A3.1** REST API design at scale: versioning strategies, HATEOAS, idempotency keys, cursor pagination generalized beyond the one case Slim 30 used it for
- **A3.2** GraphQL with Apollo Server: schema design, resolvers, the N+1 problem and DataLoader
- **A3.3** gRPC and Protocol Buffers: when and why, service definitions, streaming RPCs
- **A3.4** API gateway patterns: BFF vs. API gateway vs. service mesh — generalizing the one-BFF pattern you already know into the wider landscape

### Phase A4 — Real-Time Systems From First Principles (4 days)
- **A4.1** Raw WebSockets: the `ws` library, the handshake, frames — the layer below Slim 30's SSE-only approach
- **A4.2** Socket.io: rooms, namespaces, transport fallback
- **A4.3** Redis pub/sub for multi-instance real-time — the actual mechanism that lets real-time features scale horizontally across multiple server processes
- **A4.4** Presence systems and ephemeral state: online/offline, typing indicators — **directly reused in the capstone's live notifications and DMs**

### Phase A5 — Data Layer Mastery (4 days)
- **A5.1** Prisma vs. Drizzle, compared directly since you already know Drizzle
- **A5.2** Transactions, isolation levels, and the N+1 problem in practice
- **A5.3** Indexing and query optimization: reading `EXPLAIN ANALYZE`, composite indexes
- **A5.4** NoSQL with MongoDB: document modeling, and *when* NoSQL beats relational — directly relevant to the capstone's activity-feed/timeline data, which is a classic case where the two approaches genuinely trade off

### Phase A6 — Auth & Security Built By Hand (3 days)
- **A6.1** JWTs from scratch: signing and verifying manually, understanding the actual cryptography instead of only consuming Bouncer's output as Slim 30 did
- **A6.2** A full OAuth2 authorization-code flow implemented end-to-end — building your *own* provider and consumer, not just reading someone else's token
- **A6.3** Session security, CSRF, rate limiting, Helmet, input sanitization

### Phase A7 — Scalability & Production Systems (5 days)
- **A7.1** Message queues (BullMQ + Redis): background jobs — image resizing, notification fan-out, **exactly what the capstone's "someone with 10,000 followers just posted" problem needs**
- **A7.2** Caching strategies: cache-aside vs. write-through, Redis as cache vs. Redis as store
- **A7.3** Horizontal scaling and load balancing — including the sticky-sessions problem for WebSockets specifically, which becomes real the moment your real-time features need more than one server process
- **A7.4** Circuit breakers and resilience patterns
- **A7.5** Observability: structured logging, distributed tracing basics, Prometheus-style metrics

### Phase A8 — Testing & Quality (3 days)
- **A8.1** Vitest/Jest unit testing
- **A8.2** Integration testing with test containers and in-memory databases
- **A8.3** Contract testing and CI integration

### Phase A9 — Capstone Backend Build (2 days)
Build the real backend for the Social Platform Capstone: auth, posts/feed API with cursor pagination, the follow graph, real-time notifications over WebSockets, and the background-job image-upload pipeline. This is where every phase above gets used together for the first time.

**Mega Course A total: 35 days**

---

## MEGA COURSE B — React + Next.js: Full Frontend Mastery

**Starts after Mega Course A. Assumes:** React fundamentals and hooks (Slim 30 S12–S14), Tailwind + shadcn/UI (S15, S17), React Router + SWR (S16), React performance basics (S27).

**Does NOT re-teach:** basic hooks, JSX, or the SWR pattern. Phase B1 is reinforcement-and-extend, not a restart; everything from B4 onward is genuinely new ground Slim 30 explicitly deferred.

### Phase B0 — Orientation & Recap Bridge (1 day)

### Phase B1 — React Advanced Patterns (5 days)
- **B1.1** Compound components and render props — patterns beyond hooks alone
- **B1.2** Context architecture at scale: multiple contexts, selector patterns, avoiding re-render storms in a large app
- **B1.3** Performance mastery: Profiler-driven optimization building on Slim 30 S27, plus list virtualization (`react-window`/`@tanstack/react-virtual`) for a long social feed specifically
- **B1.4** Suspense and error boundaries in depth: data-fetching Suspense, error boundary hierarchies
- **B1.5** Custom hook design patterns: building a small composable hooks library, not just one-off hooks

### Phase B2 — State Management at Scale (3 days)
- **B2.1** Zustand — the lightweight-global-state alternative to the SWR-as-bus trick you already know
- **B2.2** TanStack Query in depth — since Slim 30 used SWR, this gives you the other major data-fetching ecosystem: mutations, optimistic updates, infinite queries (**directly used for the capstone's infinite-scroll feed**)
- **B2.3** A real decision framework: SWR vs. TanStack Query vs. Zustand vs. Context — when each actually wins, not "just use the newest one"

### Phase B3 — Testing React (2 days)
- **B3.1** Vitest + React Testing Library — testing behavior, not implementation details
- **B3.2** Mocking API calls, testing hooks, testing async UI

### Phase B4 — What Next.js Is and Why (1 day)
This is the single day that directly answers your original question. The rendering spectrum in full — CSR vs. SSR vs. SSG vs. ISR vs. streaming — followed by a direct, head-to-head comparison table against the SPA + Express BFF pattern you already know cold from Slim 30. **The honest answer, previewed:** Next.js isn't "better than React" — Next.js *is* React, plus a rendering and routing layer bolted on. It wins specifically when you need SEO, fast first paint for anonymous/public visitors, or edge rendering — exactly the profile a social platform's public profile pages have, and exactly the profile the BIA agent-chat UI deliberately does *not* have (which is why Slim 30 correctly skipped it).

### Phase B5 — Next.js App Router Core (5 days)
- **B5.1** File-based routing and layouts
- **B5.2** Server Components vs. Client Components: the mental model, the `"use client"` boundary
- **B5.3** Data fetching inside Server Components: async components, fetch caching
- **B5.4** Streaming with Suspense: `loading.tsx`, streaming SSR
- **B5.5** Dynamic routes, parallel routes, route groups

### Phase B6 — Server Actions & Mutations (3 days)
- **B6.1** Server Actions fundamentals: mutating data without a separate API route
- **B6.2** Revalidation: `revalidatePath`, `revalidateTag`, optimistic UI with `useOptimistic`
- **B6.3** Error handling in Server Actions

### Phase B7 — Next.js Platform Features (3 days)
- **B7.1** Middleware: auth gating, redirects, edge middleware
- **B7.2** `next/image`, `next/font`, the Metadata API for SEO
- **B7.3** Route Handlers (API routes) and the Edge Runtime

### Phase B8 — Auth & Deployment (3 days)
- **B8.1** Auth.js (NextAuth) integration
- **B8.2** Environment config and secrets management
- **B8.3** Deploying to Vercel, ISR behavior in production

### Phase B9 — Capstone Frontend Build (2 days)
Build the real Next.js frontend for the Social Platform Capstone: SSR'd public profile pages (for shareability and SEO), the live feed with streaming + client interactivity, an auth-gated dashboard via middleware, and post creation via Server Actions.

**Mega Course B total: 28 days**

---

## THE FUSION CAPSTONE — Social Platform

The design goal: a single application where nearly every phase from both mega-courses gets used for a real reason, not as a checklist exercise. Here's the feature-to-phase map, so you can see the fusion is genuine rather than cosmetic:

| Capstone feature | Backend phases used | Frontend phases used |
|---|---|---|
| Auth (signup/login/OAuth) | A6 (JWT + OAuth2 from scratch) | B8 (Auth.js), B7 (middleware) |
| Public profile pages | A5 (relational modeling) | B5 (SSR for SEO), B7 (`next/image`, Metadata API) |
| Post creation + feed | A3 (API design, cursor pagination) | B6 (Server Actions), B2 (TanStack Query infinite queries) |
| Follow graph | A5 (relational/graph-ish queries) | B1 (Context for follow-state) |
| Real-time notifications | A4 (WebSockets, Redis pub/sub), A7 (BullMQ fan-out for popular accounts) | B1.3 (Profiler-driven perf under real-time load) |
| Direct messages | A4 (presence, typing indicators) | B2 (Zustand for live message state) |
| Image uploads | A7 (background job: resize/process) | B7 (`next/image` optimized delivery) |
| Infinite-scroll feed | A5 (indexing, query optimization) | B1.3 (virtualization), B2.2 (TanStack Query) |
| Search | A5 (indexing / full-text search) | B5 (dynamic routes for search results) |

Two build days are already reserved for this at the end of each mega-course (A9, B9) — those are where backend and frontend are built as two coherent halves. If you want a third, dedicated **integration week** after both are done — wiring the two halves together end-to-end, deploying the whole thing, and doing a final performance/security pass across the fused system — say so when we get there and I'll scope that as its own short module rather than assuming it's needed.

---

## What happens next

Nothing in this document gets built yet. We return to Slim 30 at Session S02 (CSS Fundamentals) and continue exactly as before — one session, full Diamond Framework treatment, tutorial-qa-framework validation, Odin cross-reference — all the way through E7. This roadmap sits here, unchanged, until you arrive at Phase A0.
