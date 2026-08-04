# Mega Course B — React + Next.js: Full Frontend Mastery
### Full Comprehensive Syllabus (Maximal Coverage) — Revision 2

**Position:** Starts after Golden Canon v8 Slim (S01–E7) is complete. Runs in parallel with Mega Course A.
**Validated against:** roadmap.sh's React roadmap (77 topics, 3–6 months by its own estimate), Coursera's 2026 React roadmap, and multiple 2026 industry Next.js/React syllabi — see the Validation Note and Audit Report at the end.

---

## Why this course exists, and what I owe you as your guide through it

By the time you reach this course you'll already be able to build a working React interface — Slim 30 will have taken you there. What this course is actually for is the gap between "I can build a UI that works" and "I can reason about *why* a given rendering strategy, state tool, or component pattern is the right one for this specific situation" — which is the difference that shows up the moment you're not just following a tutorial anymore. Every phase below is built around that gap, and every References entry is chosen the same way as in Mega Course A: one, at most two sources, picked because they're what a working frontend engineer actually reaches for, not a link dump for its own sake.

The zero-to-hero arc here, stated once: you start this course already able to build a chat UI in a specific, well-understood pattern (Slim 30's SPA + Express BFF). You end it able to look at a *different* kind of product — one that needs public SEO-friendly pages, or fast anonymous-visitor load times, or server-rendered content — and correctly recognize "this is a Next.js problem, not a SPA problem," explain exactly why in the vocabulary of Phase B7, and build it. That specific moment of recognition — knowing which tool a job actually calls for, rather than reaching for the one you already know — is what separates a developer who's completed a course from an engineer.

---

## Honest Scope Math

| Phase | Days |
|---|---|
| B1 — React Advanced Component Patterns | 5 |
| B2 — React Performance Mastery | 3 |
| B3 — State Management Ecosystem | 4 |
| B4 — Forms & Validation Mastery | 2 |
| B5 — Testing React | 3 |
| B6 — Design Systems & Accessibility at Scale | 2 |
| B7 — What Next.js Is and Why | 1 |
| B8 — Next.js App Router Core | 5 |
| B9 — Next.js Server Actions & Mutations | 3 |
| B10 — Next.js Platform Features | 3 |
| B11 — Next.js Authentication | 2 |
| B12 — Real-Time Data Integration in Next.js | 2 |
| B13 — Performance & SEO at Production Scale | 2 |
| B14 — Deployment & Observability | 2 |
| B15 — Capstone Frontend Sprint | 2 |
| **TOTAL** | **41 days** |

At 1 session/day, ~5.9 weeks; at 2 sessions/day, ~3 weeks.

---

## PHASE B1 — React Advanced Component Patterns (5 days)

Slim 30 (S12–S14) gave you hooks, JSX, and props. This phase is where you stop writing components one at a time and start designing *systems* of components — the shift that makes a codebase maintainable at the size the Fusion Capstone will eventually reach.

**Day B1.1 — Compound Components**
The pattern behind `<Select><Select.Option/></Select>`-style APIs; sharing implicit state between a parent and its children via context, without prop drilling.

**Day B1.2 — Render Props & Higher-Order Components**
Older patterns still found throughout production codebases and libraries; when they're still the right tool versus when a custom hook replaces them entirely.

**Day B1.3 — Context Architecture at Scale**
Multiple contexts in one app without re-render storms; the selector pattern (subscribing to a slice of context rather than the whole object) — a direct extension of the single-context pattern Slim 30's `DataStreamProvider` used.

**Day B1.4 — Custom Hook Design as an API**
Designing hooks as a small composable library rather than one-off extractions; naming conventions, return-value shape conventions (object vs. tuple), and when a hook should expose loading/error states versus throw.

**Day B1.5 — Headless UI Patterns**
Building fully accessible, unstyled interactive components (the pattern Radix UI itself is built on) — understanding the layer beneath the shadcn/UI components Slim 30 already taught you to consume.

**References for Phase B1:**
- **react.dev** (react.dev/learn) — the official React documentation; its "Passing Data Deeply with Context" and "Reusing Logic with Custom Hooks" pages are the direct source for B1.3 and B1.4.
- **Kent C. Dodds — "Compound Components"** (kentcdodds.com/blog/compound-components-with-react-hooks) — written by one of the most widely trusted React educators; the canonical explanation of the Day B1.1 pattern, still the one most other tutorials are paraphrasing.

---

## PHASE B2 — React Performance Mastery (3 days)

**Day B2.1 — Profiler-Driven Optimization, Extended**
Going past Slim 30 S27's memo-comparator basics: reading flame charts to find the *actual* bottleneck rather than guessing; the "why did this render" investigation workflow.

**Day B2.2 — List Virtualization**
`@tanstack/react-virtual` for rendering only the visible slice of a long list — directly needed for the capstone's social feed, which will have far more items than the BIA chat history sidebar ever did.

**Day B2.3 — Concurrent React**
`useTransition` and `useDeferredValue` for marking updates as non-urgent; code-splitting and lazy loading with `React.lazy` and `Suspense` at the route level, not just the component level.

**References for Phase B2:**
- **react.dev — Profiler reference** (react.dev/reference/react/Profiler) — the official API and interpretation guide for Day B2.1's flame charts.
- **TanStack Virtual docs** (tanstack.com/virtual) — the library used in Day B2.2, documented by its maintainer.

---

## PHASE B3 — State Management Ecosystem (4 days)

**Day B3.1 — Zustand**
A lightweight global-state library — the alternative to the "SWR as a state bus" trick Slim 30 taught, compared directly.

**Day B3.2 — TanStack Query, In Depth**
Since Slim 30 used SWR exclusively, this gives you the other major data-fetching ecosystem: mutations, optimistic updates, and — critically for the capstone — infinite queries for the feed.

**Day B3.3 — Redux Toolkit & Jotai, Conceptually**
Enough of each to recognize the patterns in an existing codebase and know when a team's choice of Redux Toolkit (structured, boilerplate-reduced) or Jotai (atomic, bottom-up) makes sense, without building a full app in either.

**Day B3.4 — The Decision Framework**
SWR vs. TanStack Query vs. Zustand vs. Context vs. Redux Toolkit — a real, honest decision tree for which one wins in which situation, not "always use the newest one."

**References for Phase B3:**
- **Zustand docs** (github.com/pmndrs/zustand) — the official repository and docs, maintained by Poimandres (pmndrs).
- **TanStack Query docs** (tanstack.com/query) — mutations, optimistic updates, and infinite queries, all documented directly by the maintainer, Tanner Linsley.

---

## PHASE B4 — Forms & Validation Mastery (2 days)

**Day B4.1 — React Hook Form**
Uncontrolled-by-default form state for performance; field arrays for dynamic form sections.

**Day B4.2 — Zod Schema Validation**
Sharing one validation schema between client-side form checks and server-side request validation (a preview of the type-sharing theme that comes back hard in the Fusion Project); complex multi-step form patterns.

**References for Phase B4:**
- **React Hook Form docs** (react-hook-form.com/docs) — official documentation, including the field-array API used in Day B4.1.
- **Zod docs** (zod.dev) — official documentation for the schema library shared with Mega Course A's validation layer.

---

## PHASE B5 — Testing React (3 days)

**Day B5.1 — Vitest + React Testing Library**
Testing behavior, not implementation details — the philosophy, not just the API.

**Day B5.2 — Mocking with MSW (Mock Service Worker)**
Intercepting network requests at the service-worker level so component tests never need a real backend.

**Day B5.3 — Playwright End-to-End Testing**
Real browser automation for full user-flow tests — login, post creation, the kind of test that catches integration bugs unit tests can't.

**References for Phase B5:**
- **Testing Library docs** (testing-library.com/docs/react-testing-library/intro) — the official docs, whose stated guiding principle ("test your software the way your users use it") is the exact philosophy Day B5.1 teaches.
- **Playwright docs** (playwright.dev) — official documentation for Day B5.3, consistent with the Playwright reference already in Slim 30's optional-track material.

---

## PHASE B6 — Design Systems & Accessibility at Scale (2 days)

**Day B6.1 — Building a Design System**
Design tokens, component variant systems (extending the `cva()` pattern from Slim 30 S17), Storybook for isolated component development and documentation.

**Day B6.2 — Accessibility Auditing at Scale**
Automated auditing (axe-core) integrated into CI, not just manual keyboard-Tab checks; auditing an entire component library rather than one page at a time.

**References for Phase B6:**
- **Storybook docs** (storybook.js.org/docs) — official documentation for Day B6.1.
- **W3C WAI-ARIA Authoring Practices Guide** (w3.org/WAI/ARIA/apg/) — the same authoritative source Slim 30's ARIA sessions already pointed you to, now applied at the scale of a full component library rather than one page.

---

## PHASE B7 — What Next.js Is and Why (1 day)

**Day B7.1 — The Rendering Spectrum, and an Honest Comparison**
CSR vs. SSR vs. SSG vs. ISR vs. streaming, laid out in full, followed by a direct head-to-head table against the SPA + Express BFF pattern you already know cold from Slim 30. The honest takeaway: Next.js isn't "better than React" — it *is* React, with a rendering and routing layer added. It wins specifically for SEO, fast first paint for anonymous public visitors, and edge rendering — exactly the profile the capstone's public profile and post pages need, and exactly the profile the BIA agent-chat UI deliberately didn't need (correctly, which is why Slim 30 skipped it).

**References for Phase B7:**
- **Next.js docs — Rendering** (nextjs.org/docs/app/building-your-application/rendering) — the official, current explanation of the full CSR/SSR/SSG/ISR spectrum, from the framework's own maintainers.
- **web.dev — "Rendering on the Web"** (web.dev/articles/rendering-on-the-web) — Google's vendor-neutral explainer of the same spectrum, useful specifically because it isn't written by a framework vendor and won't frame the comparison as one-sided.

---

## PHASE B8 — Next.js App Router Core (5 days)

**Day B8.1 — File-Based Routing & Layouts**
The `app/` directory conventions; nested layouts; loading and error UI conventions per route segment.

**Day B8.2 — Server Components vs. Client Components**
The mental model in full; the `"use client"` boundary and what crosses it; why this is a genuinely new concept with no Slim 30 equivalent (the SPA pattern has no server-rendering boundary at all).

**Day B8.3 — Data Fetching in Server Components**
Async components; Next.js's extended `fetch` caching model; when to fetch in a Server Component versus a Client Component with TanStack Query.

**Day B8.4 — Streaming with Suspense**
`loading.tsx` conventions; streaming SSR piece by piece rather than waiting for the whole page.

**Day B8.5 — Dynamic, Parallel & Intercepting Routes**
Dynamic segments (`[id]`); parallel routes for rendering multiple independent sections; intercepting routes for modal-over-page patterns (e.g., viewing a post in a modal from the feed, but as its own shareable URL if visited directly) — a pattern the capstone's post-detail modal needs.

**References for Phase B8:**
- **Next.js docs — App Router** (nextjs.org/docs/app) — the primary, current documentation for every convention in this phase; App Router is a fast-moving part of the framework, so this is the one phase where the official docs matter more than any third-party tutorial, which can go stale within months.
- **react.dev — Server Components** (react.dev/reference/rsc/server-components) — the React-level (not Next.js-specific) explanation of the Server/Client Component boundary itself, since the concept originates in React, not Next.js.

---

## PHASE B9 — Next.js Server Actions & Mutations (3 days)

**Day B9.1 — Server Actions Fundamentals**
Mutating data without a separate API route; form actions bound directly to a server function.

**Day B9.2 — Revalidation & Optimistic UI**
`revalidatePath`/`revalidateTag`; `useOptimistic` for instant-feeling UI before the server confirms — directly used for the capstone's like/comment interactions.

**Day B9.3 — Error Handling in Server Actions**
`error.tsx` boundaries at the route level; returning structured validation errors from a Server Action back to the form (tying back to B4's Zod schemas).

**References for Phase B9:**
- **Next.js docs — Server Actions and Mutations** (nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations) — official, current documentation for this entire phase.

---

## PHASE B10 — Next.js Platform Features (3 days)

**Day B10.1 — Middleware**
Auth gating, redirects, A/B testing at the edge.

**Day B10.2 — Image & Font Optimization**
`next/image` automatic optimization and lazy loading; `next/font` for zero-layout-shift font loading — directly relevant to the capstone's image-heavy feed.

**Day B10.3 — Route Handlers & the Edge Runtime**
Building API routes inside the Next.js app itself; the Edge Runtime's constraints versus the Node.js runtime, and when each is the right choice.

**References for Phase B10:**
- **Next.js docs — Middleware** (nextjs.org/docs/app/building-your-application/routing/middleware) — official documentation for Day B10.1.
- **Next.js docs — next/image and next/font** (nextjs.org/docs/app/api-reference/components) — official API reference for both optimization primitives in Day B10.2.

---

## PHASE B11 — Next.js Authentication (2 days)

**Day B11.1 — Auth.js (NextAuth) Integration**
Session strategy, OAuth providers, connecting to the auth system Mega Course A's Phase A7 builds on the backend.

**Day B11.2 — Middleware-Gated Routes & Role-Based UI**
Protecting entire route trees; conditionally rendering UI based on the RBAC/ABAC model from Mega Course A A7.4.

**References for Phase B11:**
- **Auth.js docs** (authjs.dev) — official documentation, covering both session strategies and the Next.js-specific integration used in this phase.

---

## PHASE B12 — Real-Time Data Integration in Next.js (2 days)

**Day B12.1 — WebSockets Inside a Next.js App**
Connecting a Client Component to the Socket.io server Mega Course A's Phase A8 builds — the exact wiring the capstone's live notifications and DMs need.

**Day B12.2 — Combining Server Components with Live Client Data**
The pattern for a page that's mostly server-rendered (a profile page) but has one small, genuinely live island (an online/offline indicator) — a common real-world composition problem with no single "correct" pattern until you've seen it done properly.

**References for Phase B12:**
- **Socket.io client docs** (socket.io/docs/v4/client-api/) — the client-side API this phase wires into a Client Component; paired directly with Mega Course A's server-side Socket.io reference (Phase A8).

---

## PHASE B13 — Performance & SEO at Production Scale (2 days)

**Day B13.1 — Core Web Vitals**
LCP, INP, CLS — what each measures and the specific Next.js features (streaming, `next/image`, font optimization) that improve each one.

**Day B13.2 — ISR Strategy & Metadata API**
Incremental Static Regeneration timing decisions for the capstone's profile pages; the Metadata API for dynamic Open Graph tags (what makes a shared post link show a rich preview).

**References for Phase B13:**
- **web.dev — Core Web Vitals** (web.dev/articles/vitals) — Google's official, current definition of LCP/INP/CLS and how each is measured; this is the vendor-neutral source of truth these metrics originate from.
- **Next.js docs — Metadata API** (nextjs.org/docs/app/building-your-application/optimizing/metadata) — official documentation for Day B13.2's Open Graph tag generation.

---

## PHASE B14 — Deployment & Observability (2 days)

**Day B14.1 — Vercel Deep Dive**
Preview deployments, edge config, environment variable scoping across environments.

**Day B14.2 — Self-Hosting Next.js & Frontend Analytics**
Running Next.js outside Vercel (Docker, a standalone Node server) for teams that need that; basic real-user-monitoring/analytics wiring.

**References for Phase B14:**
- **Vercel docs** (vercel.com/docs) — official documentation for Day B14.1.
- **Next.js docs — Self-Hosting** (nextjs.org/docs/app/building-your-application/deploying) — the official guidance for Day B14.2's non-Vercel deployment path, directly from the framework maintainers rather than a third-party writeup.

---

## PHASE B15 — Capstone Frontend Sprint (2 days)

Build the real Next.js frontend for the Fusion Capstone: SSR'd public profile and post pages (B8, B13), the live feed with streaming and infinite scroll (B8.4, B3.2), auth-gated dashboard via middleware (B10.1, B11), post creation via Server Actions with optimistic updates (B9), and the real-time notification/DM UI wired to Mega Course A's WebSocket layer (B12).

---

## Validation Note

Cross-checked in August 2026 against roadmap.sh's React roadmap (77 topics spanning ~3–6 months by its own estimate, confirming the depth here is appropriately scoped rather than superficial), Coursera's 2026 React learning roadmap (which explicitly names Server Components, Suspense, and meta-frameworks as the current frontier — matching Phases B7 onward here), and several independent 2026 "React + Next.js in one path" syllabi, which converge on the same App Router → Server/Client Components → Server Actions → middleware progression used in Phases B8–B10.

---
---

# Appendix — Mode 2 QA Audit

*Mode 2 applies here, not the Diamond Framework — this is a syllabus/roadmap document, not tutorial content. Independent Pass 2 read.*

```
AUDIT REPORT: Mega Course B — React + Next.js Syllabus (Revision 2)
DATE: 2026-08-04
PASS: 2

GATE 1 — COMPLETENESS: PASS
  All 15 phases (B1–B15) claimed in the summary table are present in full below it,
  each with day-by-day content and a References subsection. Direct correspondence
  checked: 15 table rows, 15 "## PHASE" headers.

GATE 2 — STRUCTURAL INTEGRITY: PASS, one formatting inconsistency disclosed
  Mechanical numbering check: B1 through B15, sequential, no gaps, no repeats,
  verified by script. For Phases B1–B14, the table's day count matches the actual
  "**Day B" sub-heading count exactly (14 of 15 phases, zero mismatches). Phase
  B15 (Capstone Frontend Sprint) is the same kind of exception found in Mega
  Course A's Phase A18: it's written as integration prose, not discrete
  sub-headings, so a mechanical count finds 0 explicit "Day B15.X" headings
  against a table entry of 2 days. Same defensible reasoning as A18 — a capstone
  sprint doesn't split into independent topics the way a content phase does —
  but naming the exception here rather than claiming a blanket "zero mismatches"
  is the more honest statement. Every phase has topics + References + table
  inclusion — checked for all 15, zero missing.

GATE 3 — CLAIM VERIFICATION: PASS (9/15 phases' references sampled and re-confirmed)
  Sampled roughly 60% of phases, weighted toward the ones with the most specific
  factual claims: Phase B7's claim that Next.js "isn't better than React, it is
  React plus a rendering/routing layer" is checked against Next.js's own docs
  framing (nextjs.org describes itself as "The React Framework," consistent with
  this characterization, not a rival to React). Phase B8's flag that App Router is
  "fast-moving" and third-party tutorials go stale "within months" is a reasonable,
  appropriately hedged claim rather than a specific unverifiable number. The 41-day
  total was independently re-summed from the 15 phase figures
  (5+3+4+2+3+2+1+5+3+3+2+2+2+2+2 = 41) and confirmed correct.

GATE 4 — OVERCLAIM AUDIT: PASS
  No unqualified "always/never/only" claims found on re-read. The one place an
  absolute-sounding claim appears — "Next.js isn't better than React" — is itself
  the corrective to a potential overclaim (the person's own original question
  assumed a hierarchy), and is immediately qualified with the specific conditions
  under which Next.js does win (SEO, first paint, edge rendering), not left as an
  unqualified reversal.

GATE 5 — INTERNAL CONSISTENCY: PASS
  The 41-day total matches the figure used for Mega Course B in the companion
  Fusion Capstone Project Plan's phase-mapping table. Terminology matches Mega
  Course A's document where the two intersect (Phase B11's reference to "the
  RBAC/ABAC model from Mega Course A A7.4" was checked against Mega Course A's
  actual Phase A7.4 heading and confirmed to name the same model, not a
  contradictory or renamed one).

HONEST SCOPE STATEMENT: All 15 phases read in full for structural/consistency
  checks. References spot-checked at 9 of 15 phases (60% sample) against live
  search/fetch results gathered this session; the remainder draw from official-docs
  domains (react.dev, nextjs.org, etc.) already confirmed live during this
  session's research and were not independently re-fetched a second time in this
  specific audit pass.

VERDICT: APPROVED
```
