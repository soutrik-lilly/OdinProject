# Plan: JavaScript Sessions (S05–S12) + Post-JavaScript Capstone Phase
### Researched, Gap-Checked Against The Odin Project, and Designed Before Writing a Single Session

**Status:** This is a plan. Nothing below is session content — per your own instruction, no session gets written until this plan is confirmed and until I've re-read the immediately prior session right before writing the next one.

---

## Part 1 — What Changed After Checking Odin's Actual Curriculum

You told me you're going through Odin's "JavaScript Basics" (inside Foundations) and then the separate "JavaScript" course (inside Full Stack JavaScript, what you're calling "Intermediate") before coming to each session here. I pulled both lesson lists directly from Odin's own site rather than assume, and found two real, substantive gaps in the current bible's S05–S10 plan — not stylistic gaps, missing *topics* Odin treats as core.

**Odin's "JavaScript Basics" (Foundations) — confirmed lesson list:** Variables and Operators → Data Types and Conditionals → JavaScript Developer Tools → Function Basics → Problem Solving → Understanding Errors → *Project: Rock Paper Scissors* → Clean Code → Loops and Arrays → DOM Manipulation and Events → *Project: Etch-a-Sketch* → Object Basics → *Project: Calculator*.

**Odin's "JavaScript" course (Full Stack JavaScript path) — confirmed lesson list:** Organizing Code with Objects → Object Constructors → *Project: Library* → Factory Functions and the Module Pattern → *Project: Tic Tac Toe* → Classes → ES6 Modules → npm → Webpack → *Project: Restaurant Page* → JSON → OOP Principles → *Project: Todo List* → Form Validation with JavaScript → Asynchronous Code → Working with APIs → Async and Await → *Project: Weather App* → Testing Basics → *Project: Testing Practice* → Recursive Methods → *Project: Recursion*. (Advanced/optional continuation: Linked Lists, HashMaps, Binary Search Trees, Knight's Travails, Battleship.)

**The two real gaps, checked against the current bible's S05–S10 topic list directly:**

1. **Object-oriented JavaScript is entirely absent.** Object constructors, factory functions, the module pattern, ES6 classes, prototypes — Odin gives this enough weight to build three separate named projects around it (Library, Tic Tac Toe, Todo List). The current S05–S10 plan has objects appearing only incidentally (destructuring in S06), never as its own topic.
2. **Recursion is entirely absent.** Odin treats it as substantial enough for its own lesson and dedicated project. Nothing in S05–S10 currently covers it.

A smaller, worth-noting gap: **JSON** as its own named concept doesn't appear explicitly — it would currently be picked up only incidentally when S09's Fetch API work needs `response.json()`. Odin gives it a dedicated lesson. This one is small enough to fold into an existing day rather than needing its own.

**The fix:** two new days inserted into Phase F2, expanding it from six days (S05–S10) to eight (S05–S12). Consistent with the standing "don't compress content to hit a day count" principle already established for this whole syllabus, and consistent with your own "I don't care if it takes more days" instruction from the mega-course planning conversation.

---

## Part 2 — The Revised Phase F2 Topic Map

| Day | Title | Status |
|---|---|---|
| S05 | JavaScript Basics, History & Programming Fundamentals — Ground Zero | Unchanged, already built |
| S06 | Types, Destructuring, Spread, Array Methods & ES Modules | Unchanged, already built |
| **S07** | **Object-Oriented JavaScript — Object Literals, Factory Functions, Constructor Functions, Classes & Prototypes** | **NEW** |
| **S08** | **Recursion — Thinking Recursively, Base Cases, the Call Stack & Practical Patterns** | **NEW** |
| S09 | DOM + Events + IntersectionObserver + Scroll APIs | Shifts from S07, content unchanged |
| S10 | Async Mastery — Event Loop, Promises, async/await & Promise Combinators | Shifts from S08, content unchanged |
| S11 | Generators + Async Generators + SSE Parsing + Fetch + AbortController | Shifts from S09, content unchanged |
| S12 | Utilities + Streaming Chat Deliverable — Vanilla JS End-to-End | Shifts from S10, content unchanged |

**Why OOP goes right after array methods, not before:** factory functions and constructors both lean on destructuring (`function createUser({name, email})`) and the spread operator (`{...defaults, ...overrides}`) that S06 just taught — placing OOP before S06 would mean teaching those two mechanisms twice, once shallow and once properly. Why OOP goes *before* DOM manipulation: organizing DOM-manipulating code into objects (a `MessageList` object with methods, rather than a scattered pile of functions) is a real pattern you'll use starting the moment DOM work gets non-trivial, so the organizing principle should exist in your head before the DOM day, not after.

**Why Recursion sits right after OOP rather than at the very end (unlike Odin's own placement):** Odin places it near the end of their course; I'm placing it right after OOP specifically because both days are teaching *a way of structuring a solution* (composing behavior via objects, versus composing a solution via self-reference) rather than a new browser API — grouping the two "thinking pattern" days together, before moving into DOM/Async which are more mechanism-heavy, keeps the conceptual arc cleaner. This is a deliberate, reasoned deviation from Odin's order, not an oversight — worth you knowing the reasoning rather than just the reordering.

**Cascading impact on everything after Phase F2:** TypeScript (currently S11–S12) becomes S13–S14. React (currently S13–S19) becomes S15–S21. Everything through the Capstone and the Vite/Vue/Next.js/Testing senior track shifts by +2. Electron (E1–E7) is unaffected — it's a separate letter-prefixed track. I will do this renumbering, with the full mechanical cross-reference re-verification this project now always applies, before S07 is written — not as an afterthought after content exists.

---

## Part 3 — The Continuity Spine: What "Same Story, Same Examples" Actually Means Here

You asked for two things that are in tension if handled carelessly: strict session-to-session continuity (S06 refers to S05, S07 refers to S06+S05, etc.), *and* JavaScript taught both through the BIA chat UI *and* through standalone, non-BIA applications. Here's how both get satisfied without contradiction.

**Two parallel threads, both present in every session from S05 onward, never merged into one:**

- **Thread A — the BIA chat UI.** Every session's examples that connect to *this specific product* (parsing SSE, debouncing a search box, formatting a timestamp for a message) use the same running BIA domain vocabulary already established across S01–S04: `chat-input`, `message-list`, `sidebar`, `history-item`. This is the thread that keeps this JavaScript track feeling like one continuous project with the HTML/CSS work already done, not a context-switch into generic examples.

- **Thread B — a second, standalone running example, introduced in S05 and carried forward deliberately.** Rather than inventing a new throwaway example per session (which is what breaks continuity), I'll pick **one small, realistic, non-BIA domain** and build it up incrementally alongside Thread A across all eight JS sessions — the same way S01–S04 built up one HTML/CSS artifact incrementally. My proposal: a **personal task tracker** (`Task`, `Project`, add/complete/delete) — genuinely useful as a standalone app, directly reusable as the objects-and-arrays domain for S06's array methods, S07's OOP day (a `Task` factory function or class is a natural, non-contrived teaching example), and S08's recursion day (recursively counting completed subtasks in a nested project tree is a real, non-contrived recursion example, not the usual factorial/fibonacci toy). This is also, not coincidentally, extremely close to Odin's own "Todo List" project — meaning Thread B doubles as direct preparation for something you'll also encounter in Odin's own course, reinforcing rather than duplicating it.

**The rule going forward, stated explicitly so it's checkable:** every session from S05 onward must show at least one BIA-domain example and at least one task-tracker-domain example for its core new concept — not as two disconnected demos, but explicitly cross-referenced ("notice this is the same `.filter()` call you just used on `tasks`, now applied to `messages`"). A session that only ever uses one domain fails this plan's own continuity requirement.

---

## Part 4 — The Standing Guidelines, Made Explicit and Checkable

You asked me to state clearly what I follow as tutor/mentor versus what I follow as "code dispenser," and to apply this to every future track (TypeScript, React, Node, Express, Vue, Vite). Here is that checklist, and it is the actual review criteria I will hold each session against before delivering it — not aspirational language.

**As mentor/teacher (the reasoning layer):**
- [ ] Every new concept gets the "how would a senior developer start thinking about this, before writing code" framing established in S03/S04's revisions — not retrofitted at the end, built in from the first draft.
- [ ] Every topic gets **at least two** real, named industry examples (a company, a well-known open-source project, or a widely-recognized pattern) — not "many companies do this," a specific, checkable one.
- [ ] Explanation is clear, human, and lucid first; technical precision is never sacrificed for brevity, but brevity is used wherever a concept doesn't need three paragraphs to land. Verbosity is a tool applied where a concept is genuinely hard or commonly misunderstood (async/await's mental model, recursion's call stack), not a default setting.
- [ ] Where a visual, table, or diagram genuinely communicates faster than prose (the call stack during recursion, the event loop's phases, a prototype chain), it gets one — my judgment call each time, not prose-only by default.

**As "code dispenser" (the correctness layer — non-negotiable, zero exceptions):**
- [ ] Every code example is researched and verified against current, live documentation before being written — not recalled from training data and assumed current, exactly the standard already applied to `field-sizing`, `backdrop-filter`, and the Vite/Next.js currency notes earlier in this project.
- [ ] Every code block is syntactically complete and independently runnable, with every import/dependency shown.
- [ ] Every exercise is solvable using only what that session and its predecessors actually taught — checked explicitly, not assumed.
- [ ] Every session is validated in an independent Pass 2 after writing, using the tutorial-qa-framework skill, before being called done — never self-certified in the same breath as the draft.

**As the person responsible for continuity across this entire course (the standing practice this project has built up the hard way, through real caught bugs):**
- [ ] Before writing session N, I re-read session N-1 in full — not a summary of it — specifically to pull forward its exact terminology, its exact running examples, and any forward-references it made that session N is now responsible for paying off.
- [ ] Every session's forward-references and backward-references are mechanically re-verified against the actual current header/section numbers of the documents they point to — not assumed correct because they were correct when written, exactly the lesson from the "S01, Section 8" bug this project already found and fixed once.
- [ ] The master bible's topic list is treated as the source of truth for *what* gets covered; Odin's curriculum is treated as a completeness check and a source of well-tested project ideas, not as a second, competing bible.
- [ ] This entire checklist applies identically to every future track — TypeScript, React, Node, Express, Vue, Vite — not just JavaScript. I am writing it once, here, specifically so it doesn't need restating for each subsequent track.

---

## Part 5 — The Post-J12 Capstone Phase: Multiple Standalone JavaScript Projects

After S12, before moving into TypeScript, a dedicated capstone phase — mirroring exactly the practice-material structure already built for HTML/CSS, and directly informed by Odin's own project list (validating that these are genuinely the industry-standard practice projects, not my invention).

**Proposed structure — 5 capstone projects plus a standing practice-exercise bank, each pulling from a different combination of S05–S12's concepts:**

| Capstone | Concepts it forces you to combine | Odin's equivalent (validates the choice) |
|---|---|---|
| **1. Rock Paper Scissors, properly built** | Functions, control flow, basic DOM events (S05, S09) | Odin's own first JS project — but built here to a genuinely production-quality standard, not the beginner version |
| **2. Tic-Tac-Toe with a module-pattern architecture** | Factory functions/module pattern, arrays, DOM events, win-condition logic (S07, S06, S09) | Odin's Tic Tac Toe project, explicitly — same brief, your own implementation |
| **3. The Task Tracker, finished** | The Thread B running example, completed: classes, array methods, recursion (nested subtask counting), localStorage persistence (S07, S06, S08, S12) | Odin's Todo List project — this is Thread B's actual payoff, not a new project from zero |
| **4. A Weather App using a real public API** | Fetch, async/await, error handling, DOM rendering of async data (S10, S11, S09) | Odin's Weather App project, explicitly |
| **5. BIA Mini-Feature: Client-Side Search + Debounce Over Chat History** | Debounce (S12), array `.filter()` (S06), DOM updates (S09) — the one capstone deliberately in the BIA domain, not standalone, closing Thread A's arc for this phase | No Odin equivalent — this is where the BIA-specific thread gets its own dedicated capstone, so Thread A isn't shortchanged relative to Thread B |

**Practice exercise bank, mirroring the CSS phase's structure:** a standing set of smaller, focused drills — recursion warm-ups (sum an array recursively, flatten a nested array, count nodes in a nested object), array-method chains on realistic data (filter-then-map-then-reduce on a list of orders, messages, or tasks), and DOM manipulation katas (build a live character counter, a dynamic list with add/remove, a simple modal) — assembled the same way the HTML/CSS practice material was, as a growing reference bank rather than a one-time exercise set.

---

## What Happens Next

Once you confirm this plan, the execution order is: (1) update the master bible HTML with S07/S08 inserted and the full S09–onward renumbering, mechanically re-verified; (2) write S07 (OOP), reading S06 in full first; (3) write S08 (Recursion), reading S07 in full first; (4) continue S09–S12 with the same read-prior-session discipline, updating only what needs to change given the new day numbers; (5) build the five-capstone-plus-practice-bank phase. Nothing gets written out of this order, and nothing gets written without the Part 4 checklist applied and independently validated.
