# Free Course Companion — AgentStack Golden Canon v8 Slim (37 Days)

Verified July 2026. Every course below is genuinely free (not a disguised trial), has real reviews or a credible institutional/community backing, isn't a 100+ hour mega-course, and maps directly onto a phase of your Slim syllabus. Use these to **supplement** the syllabus — read the theory, do the interactive reps, then come back and build the actual BIA UI pattern.

**How to use this document:** work through the courses in the order they appear — they mirror your S01→S30→E1→E7 sequence. Each entry says exactly which syllabus days it supports and what it does that the syllabus's written explanations don't (mainly: guided, hands-on typing practice with instant feedback, which is hard to get from prose alone).

---

## Phase F1 — HTML/CSS Foundations (S01–S04)

### Scrimba — Learn HTML and CSS
**https://scrimba.com/learn-html-and-css-c0p**
5.7 hours · Free · Certificate on completion · Built with Mozilla MDN
Interactive "scrim" format — you pause the instructor's screencast and edit the actual code in your browser, no local setup. Covers semantic HTML, forms, the box model, Flexbox, and Grid. This is the fastest way to get the muscle memory for S01–S03 before you build the static chat UI in S04.

### The Odin Project — Foundations (HTML/CSS/Git module)
**https://www.theodinproject.com/paths/foundations/courses/foundations**
~50+ hours self-paced · Free · No certificate · Open-source curriculum
Project-based, not video-based — you read short lessons then build (a calculator, an Etch-a-Sketch clone). Pairs well with Scrimba: use Scrimba for the interactive first pass, then Odin's projects to prove you actually retained it. Also introduces Git/GitHub, which the syllabus assumes you already have.

### MDN Web Docs — Learn Web Development
**https://developer.mozilla.org/en-US/docs/Learn**
Self-paced · Free · No certificate · Official Mozilla reference
Not a course — a reference. Use this specifically when S01's gotcha section (button types, ARIA) or S02's CSS custom properties topic needs a deeper "why does this work this way" explanation than the syllabus topic-detail gives you room for.

---

## Phase F2 — JavaScript + Async + Streaming (S05–S09)

### Scrimba — Learn JavaScript
**https://scrimba.com/learn-javascript-c0v**
9.4 hours · Free · Certificate on completion · Taught by Per Borgen (Scrimba CEO), MDN partnership
The single best free interactive JavaScript course available in 2026 per independent review comparisons. 140+ in-browser coding challenges, four build-along projects (including a Chrome extension — directly relevant to your E7 day). Covers `let`/`const`, destructuring, arrow functions, the DOM, `fetch`, and `async`/`await` — this is S05, S06, and half of S07 in one interactive pass.

### javascript.info — The Modern JavaScript Tutorial
**https://javascript.info/**
Self-paced reference · Free · No certificate · Community-maintained, translated into 20+ languages
This is the one resource to keep permanently bookmarked for S07–S08. Its sections on the event loop, generators, and async iteration are the clearest free explanation of exactly what your syllabus's S07 (Event Loop, Promises) and S08 (Generators, Async Generators, SSE) cover at a conceptual level — read the relevant chapter right before doing that day's exercise.

### freeCodeCamp — JavaScript Algorithms and Data Structures
**https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures-v8/**
~300 hrs full curriculum (you don't need all of it) · Free · Certificate
Don't do the whole thing — use it as a **drill bank**. The array method and destructuring challenges are excellent supplementary reps for S05. Five required capstone projects if you want a portfolio piece alongside the syllabus's own exercises.

---

## Phase F3 — TypeScript (S10–S11)

### Scrimba — Learn TypeScript
**https://scrimba.com/learn-typescript-c03c**
4.2 hours · Free · Certificate · Taught by Rachel Johnson & Bob Ziroll
Covers core types, interfaces, generics, and type narrowing in the same interactive scrim format. This is close to a 1:1 match for S10's topic list (types, interfaces, generics, discriminated unions) — do this course in parallel with S10, pausing to cross-reference the syllabus's BIA-specific examples (`ChatMessage`, `ErrorCode` template literals).

### TypeScript Official Handbook
**https://www.typescriptlang.org/docs/**
Self-paced reference · Free · No certificate · Maintained by Microsoft's TypeScript team
The authoritative reference for anything the Scrimba course moves past too quickly — especially `satisfies`, assertion functions, and `tsconfig.json`'s `moduleResolution: bundler` setting, which S11 covers and most beginner courses skip entirely.

---

## Phase F4 — React + Tailwind v4 + shadcn/UI (S12–S18)

### Scrimba — Learn React
**https://scrimba.com/learn-react-c0e**
15.1 hours · Free · Certificate · Taught by Bob Ziroll
The longest free interactive React course available. Covers JSX, components, props, hooks (`useState`, `useEffect`, `useContext`), and custom hooks — directly supporting S12–S14. Same scrim format as the JS course, so there's zero context-switching cost if you did that one first.

### Full Stack Open — Parts 1–2 (React)
**https://fullstackopen.com/en/**
University of Helsinki · Free, optional certificate + ECTS credits · Parts 1–2 ≈ 40–60 hrs
This is the single strongest complement to your entire syllabus, not just React. It's a real university course (Dept. of Computer Science, Helsinki) that teaches **modern React, Redux, Node.js, Express, MongoDB, GraphQL, and TypeScript in one continuous curriculum** — almost exactly the tech progression your Slim 30 follows (S12→S18 React, S19→S25 backend/auth/DB). Parts 1–2 cover hooks-based React with real testing; Part 3 covers Express + REST APIs (maps to S20/S23); a later part covers TypeScript with Express (maps to S24). It assumes you already know basic JS — which by S12 you will. Submitted via GitHub, auto-graded. Reviewers consistently note it is "like a demanding university semester," so treat it as depth-add, not your primary path — run it alongside the syllabus, one part per week, rather than instead of it.

### Tailwind CSS — Official Docs
**https://tailwindcss.com/docs**
Self-paced reference · Free · No certificate
Tailwind v4's own documentation is unusually good and is the primary reference for S15's `@theme` and CSS-first setup — most third-party tutorials are still written for v3's `tailwind.config.js` model and will actively mislead you here. Confirm anything you read elsewhere against this page.

### shadcn/ui — Official Docs
**https://ui.shadcn.com/docs**
Self-paced reference · Free · No certificate
Not a course, but essential alongside S17: the docs explain the copy-not-install philosophy and have runnable examples for every component (Sidebar, Collapsible, AlertDialog) that S17 covers. Read the component's own doc page before building each piece of the sidebar exercise.

---

## Phase F5 — Auth + Integration + Agent UI Stack (S19–S26)

### Full Stack Open — Part 3 (Node.js + Express) and Part 9 (TypeScript)
**https://fullstackopen.com/en/part3** · **https://fullstackopen.com/en/part9**
Free · University of Helsinki
Part 3 builds a real Express + MongoDB REST API with validation and deployment — the closest free equivalent to what S20 and S23 ask you to build by hand. Part 9 introduces typing an Express app, which supports S24's Drizzle-typed query layer conceptually even though the ORM differs.

### Scrimba — Learn Node.js + Learn Express.js
**https://scrimba.com/learn-nodejs-c00ho9qqh6** · **https://scrimba.com/learn-expressjs-c062las154**
3.5 hrs + 4 hrs = 7.5 hrs total · Free · Certificates
Faster and more interactive than Full Stack Open's Part 3 if you want a quicker on-ramp before tackling S23's full BFF build. Covers routing, middleware, and REST fundamentals in the same scrim format.

### Vercel Academy — Builders Guide to the AI SDK
**https://vercel.com/academy/ai-sdk**
Free · Self-paced · Official Vercel course
This is the one entry on this list that is almost a direct substitute-teacher for S21–S22. It's Vercel's own free, structured course on the exact SDK version your syllabus uses — streaming, tool calling, structured outputs, `useChat`, and building a production chatbot with Next.js/React/TypeScript. Do this course's streaming-chatbot module immediately before S21; it will make `DefaultChatTransport` and `createUIMessageStream` click faster than reading topic-details alone.

### Vercel AI SDK — Official Docs + Cookbook
**https://ai-sdk.dev/docs/introduction** · **https://ai-sdk.dev/cookbook**
Free · Reference + copy-ready recipes
The docs are the authoritative source for exact function signatures (`useChat` options, `pipeUIMessageStreamToResponse`) — use these to verify anything in S21–S22 you're not 100% certain about, per your own zero-hallucination standing rule. The Cookbook has runnable recipes for streaming and tool calling that are faster to read than searching docs prose.

### FastAPI — Official Tutorial
**https://fastapi.tiangolo.com/tutorial/**
Free · Self-paced · Official, written by FastAPI's creator (Sebastián Ramírez)
The official tutorial is genuinely excellent and is what most paid FastAPI courses are ultimately teaching from. Directly supports S20 (CORS, typed clients, `StreamingResponse`) — read the "Bigger Applications" and "CORS" sections specifically.

### Full Stack Open — Part 4/5 (Testing + Auth) or freeCodeCamp Back End certification
**https://fullstackopen.com/en/part4** · **https://www.freecodecamp.org/learn/back-end-development-and-apis/**
Free
Both cover token-based auth patterns (JWT) conceptually, which underpins S19's OAuth/OIDC/JWT day even though the syllabus's specific X-EMAIL/OBO pattern is Lilly-internal and won't appear in any public course — treat these as "how auth generally works," then apply the syllabus's Bouncer-specific detail on top.

---

## Phase F6 — Production + Streaming UX (S27–S29)

### freeCodeCamp — Docker and Kubernetes Full Course for Beginners
**https://www.youtube.com/watch?v=RqTEHSBrYFw** (search "freeCodeCamp Docker Kubernetes full course" if link rotates)
~6 hours · Free · YouTube, KodeKloud-affiliated hands-on labs
The standard free entry point for container fundamentals — Docker CLI, images, Compose, and enough Kubernetes (Pods, Services, Deployments) to make S28's manifests legible. Not BIA-specific (no Bouncer/Flux content, obviously), but essential general-purpose grounding before S28's K8s day.

### freeCodeCamp — Kubernetes Course (CKA-track, 2026 update)
**https://www.freecodecamp.org/news/prepare-for-the-kubernetes-administrator-certification-and-pass-2026-update/**
~2 hours · Free · Grant-funded by Linux Foundation
Denser and more recent than the general Docker+K8s course above — go here specifically for `kubectl`, Deployments, and Services depth before S28's manifest-writing exercise.

### React DevTools — Official Documentation
**https://react.dev/learn/react-developer-tools**
Free · Official React team documentation
Short but necessary alongside S27 — this is the primary source for actually using the Profiler tab your syllabus's S27 exercise assumes you already know how to drive.

---

## Electron Desktop Module (E1–E7)

### Electron — Official Quick Start Tutorial
**https://www.electronjs.org/docs/latest/tutorial/quick-start**
Free · Self-paced · Official Electron documentation
The canonical free starting point — covers the main/renderer process split, `contextBridge`, and `ipcMain`/`ipcRenderer` that E1 is built around. Unlike most third-party Electron tutorials (many of which are stale and still teach `nodeIntegration: true`, a security anti-pattern), the official docs are actively maintained and reflect current security defaults.

### Electron — Official Tutorial: IPC, Security, Packaging
**https://www.electronjs.org/docs/latest/tutorial/ipc** · **https://www.electronjs.org/docs/latest/tutorial/security** · **https://www.electronjs.org/docs/latest/tutorial/tutorial-packaging**
Free · Official docs
Read the Security page in full before E1's exercise — it's the primary source your syllabus's `nodeIntegration off / contextIsolation on` gotcha is drawn from. The Packaging page supports E5 (electron-builder, code signing) at a general level; Apple notarization specifics in E5 go further than the official docs do, so treat E5's own gotcha content as the deeper source there.

### Frontend Masters — Electron: Build Cross-Platform Desktop Apps (Steve Kinney)
**https://frontendmasters.com/courses/electron-v3/**
Free with a GitHub Student Developer Pack; otherwise a paid Frontend Masters subscription
Not free by default, but flagged because it's the best-reviewed structured Electron course available and covers the main/renderer split, IPC, security, tray/menubar apps, and packaging — essentially E1, E2, and E5 in one course. If you have (or can get) student access, this is worth the time; otherwise the official docs above cover the same ground for zero cost, just less guided.

### electron-vite — Official Documentation
**https://electron-vite.org/**
Free · Official project docs
Directly supports E1's dev-cycle topic (electron-vite + HMR for renderer, unified config for main and renderer builds). The official Electron docs don't cover this tool in depth since it's a separate open-source project — this is the primary source.

---

## Suggested Pacing

Run these **alongside**, not instead of, your daily syllabus session — treat each as 20–40 extra minutes of guided practice before or after that day's exercise, not a separate track:

| Syllabus Phase | Companion course(s) | When to use it |
|---|---|---|
| S01–S04 | Scrimba HTML/CSS, Odin Foundations | Before S01; keep MDN open throughout |
| S05–S09 | Scrimba JavaScript, javascript.info | Scrimba before S05; javascript.info chapters right before S07–S08 |
| S10–S11 | Scrimba TypeScript, TS Handbook | Scrimba before S10; Handbook as ongoing reference |
| S12–S18 | Scrimba React, Full Stack Open Parts 1–2, Tailwind/shadcn docs | Scrimba before S12; Full Stack Open one part per week in parallel; docs referenced per-day |
| S19–S26 | Full Stack Open Part 3/9, Vercel Academy AI SDK course, FastAPI tutorial | Vercel Academy immediately before S21; FastAPI tutorial before S20 |
| S27–S29 | freeCodeCamp Docker/K8s, React DevTools docs | Docker/K8s course before S28; DevTools doc before S27 |
| E1–E7 | Electron official docs, electron-vite docs | Official Quick Start before E1; Security page before E1's exercise |

**One caution, consistent with your own standing rules:** these are third-party resources maintained on their own schedules. Cross-check any code sample against the official docs (TypeScript Handbook, Tailwind docs, Vercel AI SDK docs, Electron docs) before treating it as current — course platforms are sometimes slower to update than the underlying library, especially for fast-moving packages like the Vercel AI SDK and Tailwind v4.
