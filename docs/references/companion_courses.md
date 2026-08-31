# Free Course Companion — AgentStack Golden Canon v8 Slim (56 Days)
### Revision 2 — Updated for the Vite + Vue 3 + Next.js 16 + Testing Mastery Senior Track

Verified August 2026. Every course below is genuinely free (not a disguised trial), has real reviews or credible institutional/community backing, isn't a 100+ hour mega-course, and maps directly onto a phase of your syllabus. Use these to **supplement**, never replace, the syllabus — read the theory, do the interactive reps, then come back and build the actual BIA UI pattern or its Vue/Next.js counterpart.

**What changed in this revision.** The bible grew from 38 to 56 days: a JavaScript "Ground Zero" day was added to Phase F2, and — the reason for this update — a full 18-day Senior Track was added (Phases G1–G4: Vite, Vue 3, Next.js 16, and Testing Mastery), closing the Vitest/Playwright deferral and adding a second major framework plus the meta-framework layer Slim 30's core deliberately skipped. Every day-number below has been re-checked against the bible's *current* numbering — several phases shifted by one or two days when Ground Zero was inserted, and the old version of this document would have pointed you at the wrong session for several phases. New sections for Vite, Vue, Next.js, and desktop (Electron + its lightweight alternative, Tauri) are added at the end.

**How to use this document.** Work through the courses in the order they appear — they mirror your S01→S49→E1→E7 sequence. Each entry says exactly which syllabus days it supports.

---

## Phase F1 — HTML/CSS Foundations (S01–S04)

### Scrimba — Learn HTML and CSS
**https://scrimba.com/learn-html-and-css-c0p**
5.7 hours · Free · Certificate on completion · Built with Mozilla MDN
Interactive "scrim" format — pause the instructor's screencast and edit the actual code in your browser, no local setup. Covers semantic HTML, forms, the box model, Flexbox, and Grid — the fastest way to get muscle memory for S01–S03 before S04's static chat UI build.

### The Odin Project — Foundations (HTML/CSS/Git module)
**https://www.theodinproject.com/paths/foundations/courses/foundations**
~50+ hours self-paced · Free · No certificate · Open-source curriculum
Project-based: read short lessons, then build. Pairs with Scrimba for a second explanation in different words, and its projects (a calculator, an Etch-a-Sketch clone) are additional practice reps beyond S01–S04's own exercises.

### MDN Web Docs — Learn Web Development
**https://developer.mozilla.org/en-US/docs/Learn**
Self-paced reference · Free · No certificate · Official Mozilla reference
Use this specifically when S01's ARIA/rendering-pipeline content or S02's cascade mechanics need a deeper "why does this work this way" than the syllabus's topic-detail gives room for.

---

## Phase F2 — JavaScript, Ground Zero Through Streaming (S05–S10)

*Note the day-number correction: this phase now runs S05–S10, not S05–S09 as in the original bible — Session S05 ("JavaScript Basics, History & Programming Fundamentals — Ground Zero") was added after this document's first draft.*

### Scrimba — Learn JavaScript
**https://scrimba.com/learn-javascript-c0v**
9.4 hours · Free · Certificate · Taught by Per Borgen, MDN partnership
140+ in-browser coding challenges and four build-along projects. Covers `let`/`const`, destructuring, arrow functions, the DOM, `fetch`, and `async`/`await` — directly supporting S06 through half of S08.

### javascript.info — The Modern JavaScript Tutorial
**https://javascript.info/**
Self-paced reference · Free · No certificate · Community-maintained
Keep this permanently bookmarked for S08–S09. Its event-loop and async-iteration chapters are the clearest free explanation at the conceptual level of what those two sessions cover — read the relevant chapter right before that day's exercise.

### freeCodeCamp — JavaScript Algorithms and Data Structures
**https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures-v8/**
~300 hrs full curriculum (use as a drill bank, not end to end) · Free · Certificate
The array-method and destructuring challenges are strong supplementary reps for S06.

---

## Phase F3 — TypeScript (S11–S12)

### Scrimba — Learn TypeScript
**https://scrimba.com/learn-typescript-c03c**
4.2 hours · Free · Certificate · Taught by Rachel Johnson & Bob Ziroll
Core types, interfaces, generics, and type narrowing — a close match for S11's topic list. Do this in parallel with S11, cross-referencing the syllabus's BIA-specific examples.

### TypeScript Official Handbook
**https://www.typescriptlang.org/docs/**
Self-paced reference · Free · Maintained by Microsoft's TypeScript team
The authoritative reference for `satisfies`, assertion functions, and `tsconfig.json`'s `moduleResolution: bundler` — S12 territory most beginner courses skip entirely.

---

## Phase F4 — React + Tailwind v4 + shadcn/UI (S13–S19)

### Scrimba — Learn React
**https://scrimba.com/learn-react-c0e**
15.1 hours · Free · Certificate · Taught by Bob Ziroll
The longest free interactive React course available — JSX, components, props, hooks, custom hooks — supporting S13–S15.

### Full Stack Open — Parts 1–2 (React)
**https://fullstackopen.com/en/**
University of Helsinki · Free, optional certificate + ECTS credits
Real university course covering modern React, Redux, Node.js, Express, and TypeScript in one continuous curriculum. Run one part per week in parallel with S13–S19, not instead of them.

### Tailwind CSS — Official Docs
**https://tailwindcss.com/docs**
Self-paced reference · Free
Tailwind v4's own docs are the primary reference for S17's `@theme` and CSS-first setup — most third-party tutorials still describe v3's `tailwind.config.js` model and will actively mislead you here.

### shadcn/ui — Official Docs
**https://ui.shadcn.com/docs**
Self-paced reference · Free
Read the relevant component's doc page before building each piece of S18's sidebar exercise.

---

## Phase F5 — Auth + Integration + Agent UI Stack (S20–S27)

### Full Stack Open — Part 3 (Node.js + Express) and Part 9 (TypeScript)
**https://fullstackopen.com/en/part3** · **https://fullstackopen.com/en/part9**
Free · University of Helsinki
Part 3 builds a real Express + MongoDB REST API — the closest free equivalent to S23's hand-built BFF. Part 9 supports S24's typed query layer conceptually.

### Scrimba — Learn Node.js + Learn Express.js
**https://scrimba.com/learn-nodejs-c00ho9qqh6** · **https://scrimba.com/learn-expressjs-c062las154**
3.5 hrs + 4 hrs · Free · Certificates
A faster on-ramp than Full Stack Open's Part 3 before S23's full BFF build.

### Vercel Academy — Builders Guide to the AI SDK
**https://vercel.com/academy/ai-sdk**
Free · Self-paced · Official Vercel course
The vendor's own free course on the exact SDK version S21–S22 use — streaming, tool calling, `useChat`. Do the streaming-chatbot module immediately before S21.

### Vercel AI SDK — Official Docs + Cookbook
**https://ai-sdk.dev/docs/introduction** · **https://ai-sdk.dev/cookbook**
Free · Reference + copy-ready recipes
Authoritative source for exact function signatures — verify anything in S21–S22 you're not fully certain of here.

### FastAPI — Official Tutorial
**https://fastapi.tiangolo.com/tutorial/**
Free · Written by FastAPI's creator
Directly supports S20's CORS and `StreamingResponse` sections.

---

## Phase F6 — Production + Streaming UX (S28–S30)

### freeCodeCamp — Docker and Kubernetes Full Course for Beginners
**https://www.youtube.com/results?search_query=freeCodeCamp+Docker+Kubernetes+full+course** *(search — link rotates)*
~6 hours · Free · YouTube, KodeKloud-affiliated labs
Standard free entry point before S30's K8s manifest-writing day.

### freeCodeCamp — Kubernetes Course (CKA-track, 2026 update)
**https://www.freecodecamp.org/news/prepare-for-the-kubernetes-administrator-certification-and-pass-2026-update/**
~2 hours · Free · Linux Foundation grant-funded
Denser `kubectl`/Deployments/Services depth before S30.

### React DevTools — Official Documentation
**https://react.dev/learn/react-developer-tools**
Free · Official React team documentation
The primary source for actually driving the Profiler tab S29's exercise assumes you already know.

---

## Phase G1 — Vite, Beyond the Scaffold (S32–S34) — NEW

### Vite — Official Documentation
**https://vite.dev/guide/**
Free · Official, maintained by the Vite team
The primary source for S32–S34, and the *only* place guaranteed to reflect Vite 8's March 2026 Rolldown unification correctly — most third-party Vite content published before 2026 describes the older two-bundler architecture as current, which it no longer is. Read "Features," "Dependency Pre-Bundling," and the Vite 8 blog announcement specifically.

### freeCodeCamp — Vite Crash Course
**https://www.freecodecamp.org/news/** *(search "freeCodeCamp Vite crash course" — YouTube, ~40 min)*
Free · YouTube
A fast practical companion to S32 — scaffolding, HMR, env variables, and a production build, in under an hour. Treat the official docs as the source of truth on anything this video's publish date might predate.

### Vitest — Official Documentation
**https://vitest.dev/**
Free · Official docs
The direct source for S34's shared-config mechanism — read "Why Vitest" specifically for the explanation of *why* it needs no separate bundler setup.

---

## Phase G2 — Vue 3, Zero to Hero by Contrast (S35–S40) — NEW

### Scrimba — Learn Vue
**https://scrimba.com/learn-vuejs-c020**
Free · Certificate · Interactive scrim format
The best free interactive starting point for Vue per independent course-comparison reviews — same build-alongside-the-instructor format as the React and JS courses you've already used, so there's no new format to adjust to.

### Vue.js Official Guide
**https://vuejs.org/guide/introduction.html**
Free · Official documentation, exceptionally well-written
Vue's own docs are consistently rated among the best-written official framework documentation in the industry — use this as the primary reference for S35's reactivity system and S37's Composition API specifically, since it's written by the people who designed the mechanism.

### Vue School — Free Courses
**https://vueschool.io/courses**
Free tier available (paid tier for the deeper catalog) · Official Vue training partner
Filter to the free courses for a second explanation of Composition API and Pinia in different words than Scrimba's, if S36–S39 need it.

---

## Phase G3 — Next.js 16, Why and How (S41–S46) — NEW

### Scrimba — Learn Next.js
**https://scrimba.com/learn-next-js-c026**
4.4 hours · Free · Certificate · Interactive
The best free interactive option per multiple independent 2026 course comparisons — you build a real Next.js app inside the lesson rather than watching one get built. Confirms App Router coverage (not the legacy Pages Router), which is essential — roughly half of older Next.js tutorials online still teach Pages Router content that's actively misleading for new projects in 2026.

### Next.js — Official Learn Course
**https://nextjs.org/learn**
~2 hours · Free · Official, written by the Next.js/Vercel team
Rewritten from scratch to teach the App Router paradigm specifically. The fastest way to build a correct mental model before S42's Server/Client Components day — and, being official, the most likely to already reflect Next.js 16's Turbopack-default and `proxy.ts` changes correctly.

### Next.js — Official Documentation
**https://nextjs.org/docs**
Free · Official reference
App Router is the fastest-moving part of the framework — for S43's Cache Components and S44's `proxy.ts` rename specifically, the official docs matter more here than for almost any other topic in this whole companion guide, since third-party tutorials can go stale within months.

---

## Phase G4 — Testing Mastery (S47–S49) — NEW

### Testing Library — Official Documentation
**https://testing-library.com/docs/react-testing-library/intro/**
Free · Official docs
The source of the "test the way your users use it" principle S47 is built on.

### Mock Service Worker (MSW) — Official Documentation
**https://mswjs.io/docs/**
Free · Official docs
Primary reference for S48's network-level interception approach — read the "Getting Started" and "Mocking Responses" pages specifically.

### Playwright — Official Documentation
**https://playwright.dev/docs/intro**
Free · Official, maintained by Microsoft
The primary reference for S49's end-to-end capstone, including the official "Best Practices" page — worth reading in full once, since it directly addresses the "when NOT to reach for E2E" judgment call S49's gotcha covers.

---

## Phase E — Electron Desktop + Lightweight Alternatives (E1–E7)

### Electron — Official Quick Start Tutorial
**https://www.electronjs.org/docs/latest/tutorial/quick-start**
Free · Self-paced · Official Electron documentation
The canonical starting point for E1 — main/renderer process split, `contextBridge`, `ipcMain`/`ipcRenderer`. Actively maintained and reflects current security defaults, unlike many stale third-party Electron tutorials that still teach `nodeIntegration: true`.

### Electron — Official Tutorials: IPC, Security, Packaging
**https://www.electronjs.org/docs/latest/tutorial/ipc** · **https://www.electronjs.org/docs/latest/tutorial/security** · **https://www.electronjs.org/docs/latest/tutorial/tutorial-packaging**
Free · Official docs
Read the Security page in full before E1's exercise — the primary source for the `nodeIntegration off / contextIsolation on` gotcha.

### electron-vite — Official Documentation
**https://electron-vite.org/**
Free · Official project docs
Directly supports E1's dev-cycle topic — this tool isn't covered in Electron's own docs since it's a separate project.

### Tauri — Official Documentation ("electron-lite")
**https://v2.tauri.app/start/**
Free · Official documentation, current stable v2.10.1 (March 2026)
If you want to see the lightweight alternative to Electron the desktop-framework landscape has been shifting toward — Tauri bundles a Rust binary and uses the OS's native WebView instead of shipping a full Chromium runtime, producing apps reported as roughly 20x smaller than an equivalent Electron build. Not a required part of the syllabus (E1–E7 is built on Electron deliberately, matching the Now & Not Now / Sage architecture), but worth reading the official "What is Tauri?" page and Quick Start once you finish E1–E7, specifically to understand the real trade-off: Tauri's smaller footprint and stronger default security model come at the cost of Rust in your stack and WebView rendering inconsistencies across operating systems that Electron's bundled Chromium avoids. For a background-running productivity app like Apopax, this is a genuine, non-obvious architectural fork worth knowing about even if you don't switch to it.

### Frontend Masters — Electron: Build Cross-Platform Desktop Apps (Steve Kinney)
**https://frontendmasters.com/courses/electron-v3/**
Free with a GitHub Student Developer Pack; otherwise paid subscription
The best-reviewed structured Electron course available, covering the main/renderer split, IPC, security, tray/menubar apps, and packaging — essentially E1, E2, and E5 in one course, if you have or can get student access.

---

## Suggested Pacing

| Syllabus Phase | Companion course(s) | When to use it |
|---|---|---|
| S01–S04 | Scrimba HTML/CSS, Odin Foundations | Before S01; MDN open throughout |
| S05–S10 | Scrimba JavaScript, javascript.info | Scrimba before S06; javascript.info chapters right before S08–S09 |
| S11–S12 | Scrimba TypeScript, TS Handbook | Scrimba before S11; Handbook as ongoing reference |
| S13–S19 | Scrimba React, Full Stack Open 1–2, Tailwind/shadcn docs | Scrimba before S13; Full Stack Open in parallel; docs per-day |
| S20–S27 | Full Stack Open 3/9, Vercel Academy AI SDK, FastAPI tutorial | Vercel Academy immediately before S21; FastAPI before S20 |
| S28–S30 | freeCodeCamp Docker/K8s, React DevTools docs | Docker/K8s before S30; DevTools doc before S29 |
| **S32–S34 (Vite)** | Vite official docs, freeCodeCamp Vite crash course | Official docs before S32 — currency matters most here |
| **S35–S40 (Vue)** | Scrimba Vue, Vue.js official guide | Scrimba before S35; official guide as ongoing reference through S40 |
| **S41–S46 (Next.js)** | Scrimba Next.js, Next.js official Learn course | Either before S41; official docs for S43–S44's fast-moving specifics |
| **S47–S49 (Testing)** | Testing Library, MSW, Playwright official docs | One doc set per day, immediately before that day's session |
| E1–E7 | Electron official docs, electron-vite docs | Official Quick Start before E1; Security page before E1's exercise; Tauri docs as post-E7 context |

**One caution, unchanged from the previous revision:** these are third-party resources on their own release schedules. Cross-check any code sample against official docs before treating it as current — Vite, Next.js, and the Vercel AI SDK are all shipping fast enough in 2026 that a course recorded even six months ago can already describe a superseded default.
