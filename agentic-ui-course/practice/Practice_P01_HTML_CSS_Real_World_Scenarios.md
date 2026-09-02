# Practice & Application Guide — HTML5 + CSS Fundamentals
### Seven Real-World Scenarios, Built From Session S01 + S02, Zero → Hero

**Builds on:** [Session S01 — HTML5 Essentials](../s1/Session_S01_HTML5_Essentials.md) · [Session S02 — CSS Fundamentals](../s2/Session_S02_CSS_Fundamentals.md)
**Prerequisites:** You've read S01 and S02 at least once. This doc doesn't re-teach the mechanisms — it makes you *use* them until they're reflexes.
**Time budget:** ~4–5 hours, spread across sessions. Do the scenarios in order — each one reuses and compounds the previous one's code, exactly like S01→S02 did.

---

## 0. My Role, and the Contract for This Document

**My role here is instructor, not code-generator.** Every scenario below exists to make you *predict, build, break, and fix* — not to hand you a finished snippet to paste. That's why every scenario ends with a "Do This Yourself" step you must actually perform before moving on, mirroring the exact pedagogy S01/S02 already used.

**The plan I followed to build this doc** (stated so you can audit it, per the "a plan is a contract" principle):
1. Re-read S01 and S02 in full, section by section.
2. Extracted every *mechanism* taught (not every tag) — semantics, forms, ARIA, cascade, box model, units, custom properties, selectors.
3. Chose **one continuous fictional product** — the same "BIA Unified Intelligence Platform" chat app S01/S02 already build toward — so scenarios feel like a real job, not disconnected katas.
4. Ordered scenarios by real-world frequency: profile pages and forms exist in nearly every product; theming systems and accessible live regions are rarer but define "production-grade" work.
5. Validated every code block mentally against the same rules S01/S02's own QA appendix uses: no hallucinated tags/properties, every concept traceable to a specific section, every scenario solvable using *only* S01+S02 content (nothing from S03 Flexbox/Grid — that's next session's job).

**Why this matters industry-wide, restated once, up front:** the scenarios below are not academic exercises. Semantic HTML + accessible forms + a working theme system are graded, contractual requirements on real engineering teams (WCAG 2.1 AA / Section 508 compliance, design-system consistency, SEO). You are practicing the exact muscle a hiring manager tests for in a take-home assignment.

---

## 1. Concept Coverage Map

Use this table to jump straight to the scenario that drills a concept you're shaky on.

| Concept | Session · Section | Drilled in Scenario |
|---|---|---|
| Document skeleton, semantic landmarks | S01 §4, §6 | 1, 7 |
| Lists, links, images, `alt` text | S01 §7 | 1 |
| `<button>` vs `<div>`, `type="button"` | S01 §7 | 5, 7 |
| Semantic tables (`thead`, `scope`, `caption`) | S01 §7 | 3 |
| Forms, input types, native validation | S01 §8 | 2, 7 |
| ARIA (`aria-label`, `aria-hidden`, `aria-live`) | S01 §9 | 2, 5, 7 |
| DevTools + `data-*` attributes | S01 §5 | 5, 6 |
| The cascade, `!important`, source order | S02 §3.1 | 4, 5 |
| Specificity `(A, B, C)` scoring | S02 §3.2 | 4, 5 |
| Box model, `box-sizing: border-box` | S02 §4 | 3, 4 |
| Units (`rem`, `em`, `%`, `ch`) | S02 §5 | 3, 4 |
| CSS custom properties + theming | S02 §6 | 6, 7 |
| Selectors, `:hover`, `:focus-visible`, `::before/after` | S02 §7 | 4, 5, 6 |

---

## 2. How to Run Each Scenario

Every scenario gives you two files: `index.html` and `styles.css`, linked exactly the way S02 §1 taught (Method 3 — external stylesheet, never inline styles). For every scenario:

1. Create a folder, e.g. `~/agentic-ui-course/practice/scenario-01/`.
2. Type the code yourself — don't paste. (S01 §1 already told you why; it's still true here.)
3. Open `index.html` in a browser. Reload after every change.
4. Open DevTools (`F12` / `Cmd+Option+I`) and do the "Do This Yourself" step before reading the Explain section.

---

## 3. Scenario 1 — The Semantic Profile Page

### Real-world context
Every product has an "About" or public profile page — a support agent's bio, a doctor's profile on a health portal, your own resume page. This is the single most common "first real page" a junior dev ships.

### Objective
Build a fully semantic, zero-CSS-yet profile page and confirm its structure is machine-readable, not just visually correct.

### Why this matters (industry)
Recruiters, screen readers, and search engine crawlers all read *structure* before *style*. A resume page built from `<div>` soup is invisible to a screen reader and ranks worse in search — this is the exact "div soup" problem S01 §6 names directly.

### What (concepts, with reference)
- Document skeleton — S01 §4
- Landmarks (`header`, `main`, `footer`, `section`) — S01 §6
- Heading hierarchy (one `<h1>`, ordered `<h2>`s) — S01 §6
- Lists for skills — S01 §7
- `alt` text discipline — S01 §7

### How
1. Skeleton first: `<!DOCTYPE html>` → `<html lang="en">` → `<head>` (charset, viewport, title) → `<body>`.
2. One `<header>` with your name as `<h1>`.
3. `<main>` containing two `<section>`s, each with its own `<h2>`.
4. An unordered list of 3 skills inside one section.
5. `<footer>` with a copyright line.

### What We're Building — the Component List

| # | Component | Element | Traces to |
|---|---|---|---|
| 1 | Name banner | `header` > `h1` + `p` | S01 §6 |
| 2 | Background section | `main` > `section` > `h2` + `p` | S01 §6 |
| 3 | Skills section | `main` > `section` > `h2` + `ul` | S01 §6, §7 |
| 4 | Copyright line | `footer` > `p` | S01 §6 |

### How It Should Look

No CSS layout tool exists yet at this point in the syllabus — every block-level element simply stacks top-to-bottom in normal document flow, in source order.

```text
┌──────────────────────────────┐
│ Soutrik                        │  ← header
│ Backend engineer moving into   │
│ full-stack UI work.            │
├──────────────────────────────┤
│ Background                     │  ← section 1
│ Python/FastAPI engineer...     │
├──────────────────────────────┤
│ Skills                         │  ← section 2
│ • Python & FastAPI              │
│ • Semantic HTML5                │
│ • CSS custom properties...      │
├──────────────────────────────┤
│ © 2026 Soutrik. All rights...  │  ← footer
└──────────────────────────────┘
```

### Step-by-Step Build Order

1. Write the skeleton: `<!DOCTYPE html>` → `<html lang="en">` → `<head>` → `<body>`.
2. Add `<header>` with your `<h1>` name and a one-line tagline.
3. Add `<main>` with the first `<section>` (`<h2>Background</h2>` + a paragraph).
4. Add the second `<section>` inside the same `<main>` (`<h2>Skills</h2>` + a 3-item `<ul>`).
5. Add `<footer>` with the copyright paragraph.
6. Link `styles.css` and add only the two rules shown below — nothing more yet.

### Code

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Soutrik — Profile</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>

  <header>
    <h1>Soutrik</h1>
    <p>Backend engineer moving into full-stack UI work.</p>
  </header>

  <main>
    <section>
      <h2>Background</h2>
      <p>Python/FastAPI engineer, currently building the BIA chat UI front end.</p>
    </section>

    <section>
      <h2>Skills</h2>
      <ul>
        <li>Python &amp; FastAPI</li>
        <li>Semantic HTML5</li>
        <li>CSS custom properties &amp; theming</li>
      </ul>
    </section>
  </main>

  <footer>
    <p>&copy; 2026 Soutrik. All rights reserved.</p>
  </footer>

</body>
</html>
```

```css
/* styles.css */
* { box-sizing: border-box; }

body {
  font-family: system-ui, sans-serif;
  max-width: 60ch;      /* readable line length — S02 §5 */
  margin: 0 auto;        /* margin: auto trick — S02 §4 */
  padding: 2rem;
}
```

### Explain
- `max-width: 60ch` uses the `ch` unit specifically for **readable line length** (S02 §5) — not an arbitrary pixel guess.
- `margin: 0 auto` only centers because `body` effectively has a bounded width via `max-width` — the exact "one specific way it works" caveat from S02 §4.
- `box-sizing: border-box` is on line one, before anything else, as S02 §4 insists every professional stylesheet does.

### Do This Yourself
Open DevTools' Elements panel. Confirm exactly one `<h1>` exists and that `<section>` elements are direct children of `<main>`, not nested inside each other.

### Reference
S01 §4, §6, §7 · S02 §4, §5

---

## 4. Scenario 2 — The Accessible Sign-Up Form

### Real-world context
Every SaaS product gates access behind a sign-up form. This is also the #1 place accessibility lawsuits target (unlabeled inputs, no keyboard path, no validation feedback).

### Objective
Build a sign-up form that is fully keyboard-navigable and validates natively, with zero JavaScript.

### Why this matters (industry)
S01 §12 names this directly: enterprise software under Section 508 / WCAG 2.1 AA obligations treats broken forms as a **compliance failure**, not a UX nitpick. Native HTML validation (S01 §8) is also simply less code to ship and maintain than a hand-rolled JS validator.

### What
- `<label for>` ↔ `id` pairing — S01 §8
- Input types (`email`, `password`) — S01 §8
- Native validation (`required`, `minlength`) — S01 §8
- Icon-only button + `aria-label` — S01 §9
- Box model spacing — S02 §4

### How
1. One `<form>`, three fields: name, email, password.
2. Every input gets a paired `<label for>`.
3. `required` on all three; `type="email"` on email; `minlength="8"` on password.
4. Add an icon-only "clear form" button using `aria-label`.

### What We're Building — the Component List

| # | Component | Element | Traces to |
|---|---|---|---|
| 1 | Page title | `main` > `h1` | S01 §6 |
| 2 | Name field | `.field` > `label for` + `input type="text"` | S01 §8 |
| 3 | Email field | `.field` > `label for` + `input type="email"` | S01 §8 |
| 4 | Password field | `.field` > `label for` + `input type="password"` | S01 §8 |
| 5 | Submit button | `button type="submit"` | S01 §7 |
| 6 | Icon-only clear button | `button type="button" aria-label` | S01 §9 |

### How It Should Look

Each `.field` div is `display: block`, so the three fields stack vertically. The two `<button>` elements are the form's last two children — with no layout tool applied yet, they render side-by-side inline, in source order.

```text
┌──────────────────────────────┐
│ Create your BIA account        │  ← h1
├──────────────────────────────┤
│ Full name                      │  ← label
│ [______________________]       │  ← input, width:100%
├──────────────────────────────┤
│ Email                          │
│ [______________________]       │
├──────────────────────────────┤
│ Password                       │
│ [______________________]       │
├──────────────────────────────┤
│ [Create account]  🗑           │  ← two buttons, inline, same line
└──────────────────────────────┘
```

### Step-by-Step Build Order

1. Add `<main>` with an `<h1>` and an empty `<form>`.
2. Build the name field first: a `.field` div containing `<label for="full-name">` + `<input id="full-name" required minlength="2">`. Confirm the `for`/`id` pair matches exactly.
3. Copy the pattern for email (`type="email"`) and password (`type="password" minlength="8"`).
4. Add the submit button (`<button type="submit">Create account</button>`).
5. Add the icon-only clear button last, with `type="button"` (never `submit`) and `aria-label="Clear form"`.
6. Add the CSS: box-sizing reset, `.field` spacing, `input` sizing, and the `:focus-visible` ring — in that order.

### Code

```html
<!-- index.html body content -->
<main>
  <h1>Create your BIA account</h1>
  <form>
    <div class="field">
      <label for="full-name">Full name</label>
      <input type="text" id="full-name" name="fullName" required minlength="2">
    </div>

    <div class="field">
      <label for="email">Email</label>
      <input type="email" id="email" name="email" required>
    </div>

    <div class="field">
      <label for="password">Password</label>
      <input type="password" id="password" name="password" required minlength="8">
    </div>

    <button type="submit">Create account</button>
    <button type="button" aria-label="Clear form">🗑</button>
  </form>
</main>
```

```css
* { box-sizing: border-box; }

.field {
  margin-bottom: 1rem;
  display: block;      /* preview — full layout control arrives in S03 */
}

label {
  display: block;
  margin-bottom: 0.25rem;
  font-weight: 600;
}

input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #cbd5e1;
  border-radius: 0.375rem;
}

input:focus-visible {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}
```

### Explain
- `input:focus-visible` (S02 §7) gives keyboard users a visible ring **without** annoying mouse-click outlines — the exact distinction S02 §7 draws between `:focus` and `:focus-visible`.
- `outline`, not `border`, for the focus ring — because outline doesn't participate in the box model (S02 §4) and can't shift layout when it appears.
- The trash-can button has **no visible text**, so `aria-label="Clear form"` (S01 §9) is not optional — without it, a screen reader announces nothing useful.

### Do This Yourself
Tab through the entire form using only the keyboard. Then submit with the password field at 3 characters and read the browser's own validation message — no JavaScript involved.

### Reference
S01 §8, §9 · S02 §4, §7

---

## 5. Scenario 3 — The Pricing Comparison Table

### Real-world context
Every SaaS marketing site has a pricing table: Free / Pro / Enterprise, rows of feature comparisons. This is genuinely tabular data — S01 §7's rule for when `<table>` is the *correct* tool, not a layout hack.

### Objective
Build an accessible pricing table where a screen reader can announce "42 messages, column: Messages, row: Aug 18" style context for every cell.

### Why this matters (industry)
S01 §7 is explicit: tables misused for layout produce a "confusing, meaningless reading experience" for screen reader users — this was a widespread pre-Flexbox anti-pattern. Doing tables *correctly* the few times you genuinely need them is a real, tested skill.

### What
- `<caption>`, `<thead>`, `<tbody>`, `scope` — S01 §7
- Borders, padding, `rem` spacing — S02 §4, §5
- `:hover` row highlighting — S02 §7

### How
1. `<table>` with a `<caption>` naming it.
2. `<thead>` with one `<tr>` of `<th scope="col">`.
3. `<tbody>` rows, each with a `<th scope="row">` for the plan name.
4. CSS: border on cells, `rem` padding, hover highlight.

### What We're Building — the Component List

| # | Component | Element | Traces to |
|---|---|---|---|
| 1 | Table name | `caption` | S01 §7 |
| 2 | Column headers | `thead` > `tr` > `th scope="col"` × 3 | S01 §7 |
| 3 | Plan rows | `tbody` > `tr` > `th scope="row"` + `td` × 2 | S01 §7 |

### How It Should Look

A `<table>` lays out its own rows and columns natively — this is the one scenario in the syllabus so far where the browser's built-in layout algorithm, not CSS you write, does the column alignment.

```text
┌──────────┬────────────────────┬──────────┐
│ Plan     │ Messages / month   │ Price    │  ← thead (bold, bordered)
├──────────┼────────────────────┼──────────┤
│ Free     │ 100                │ $0       │  ← tbody row 1
├──────────┼────────────────────┼──────────┤
│ Pro      │ 10,000             │ $29/mo   │  ← tbody row 2, hover → light gray
└──────────┴────────────────────┴──────────┘
```

### Step-by-Step Build Order

1. Start with `<table>` and its `<caption>BIA Plan Comparison</caption>` — caption comes first, immediately inside the table.
2. Add `<thead>` with one `<tr>` of three `<th scope="col">` cells.
3. Add `<tbody>` with one `<tr>` per plan, `<th scope="row">` for the plan name, `<td>` for the two data cells.
4. Add the CSS: `border-collapse: collapse`, bordered/padded cells, then the `tbody tr:hover` rule last.

### Code

```html
<table>
  <caption>BIA Plan Comparison</caption>
  <thead>
    <tr>
      <th scope="col">Plan</th>
      <th scope="col">Messages / month</th>
      <th scope="col">Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Free</th>
      <td>100</td>
      <td>$0</td>
    </tr>
    <tr>
      <th scope="row">Pro</th>
      <td>10,000</td>
      <td>$29/mo</td>
    </tr>
  </tbody>
</table>
```

```css
* { box-sizing: border-box; }

table {
  border-collapse: collapse;
  width: 100%;
}

th, td {
  border: 1px solid #cbd5e1;
  padding: 0.75rem 1rem;   /* rem, not px — respects user font-size prefs */
  text-align: left;
}

tbody tr:hover {
  background-color: #f1f5f9;
}
```

### Explain
- `padding: 0.75rem 1rem` uses `rem` deliberately (S02 §5) — table padding should scale if the user bumps their browser's base font size for readability.
- `border-collapse: collapse` merges adjacent cell borders into one line instead of doubling them — a table-specific detail, mentioned here because it's the single most common "why are my table borders doubled" question.
- `tbody tr:hover` only targets body rows, not the header — because `:hover` (S02 §7) is a pseudo-*class* (a state), scoped precisely by the selector it's attached to.

### Do This Yourself
Open your browser's Accessibility inspector (part of DevTools) on the "10,000" cell and confirm its accessible description references both "Pro" (row) and "Messages / month" (column) — exactly the mechanism `scope` exists for.

### Reference
S01 §7 · S02 §5, §7

---

## 6. Scenario 4 — The Feature Card Grid (Box Model + Specificity Debugging)

### Real-world context
Nearly every landing page has a row of "feature cards" — icon, title, description, in a bordered box. This scenario is deliberately designed to **break**, so you practice the debugging skill, not just the happy path.

### Objective
Build three feature cards, then deliberately introduce and fix a specificity bug and a box-model width bug — the two most common real-world "why isn't my CSS working" tickets.

### Why this matters (industry)
S02 §9 states this plainly: specificity confusion is one of the most common sources of "CSS isn't working" reports from *working developers*, not just beginners — because most people never learn to compute `(A, B, C)` by hand. This scenario forces you to compute it, not guess it.

### What
- Box model arithmetic — S02 §4
- Specificity scoring — S02 §3.2
- `border-radius`, `box-shadow` — S02 §2

### How
1. Build three `.card` elements inside a wrapper.
2. Give the wrapper a class `.grid` with a *conflicting* rule below it (the bug).
3. Compute the specificity by hand before checking in DevTools.
4. Fix the width bug caused by missing `box-sizing`.

### What We're Building — the Component List

| # | Component | Element | Traces to |
|---|---|---|---|
| 1 | Wrapper | `.grid` (a plain `div` — no layout property set on it yet) | S02 §3 |
| 2 | Featured card | `.card#card-featured` | S02 §3.2, §4 |
| 3 | Two plain cards | `.card` × 2 | S02 §4 |

### How It Should Look

The wrapper is named `.grid`, but nothing sets `display: grid` or `display: flex` on it yet — that's deliberate, this is S01/S02 content only. So, despite the name, the three `.card` divs are plain block elements and stack **vertically**, full width, one under another.

```text
┌──────────────────────────────┐
│ Streaming Responses            │  ← .card#card-featured
│ See tokens arrive in real time.│     (background color = the bug)
├──────────────────────────────┤
│ Dark Mode                      │  ← .card
│ One class toggles the entire.. │
├──────────────────────────────┤
│ Accessible by Default          │  ← .card
│ Built on semantic HTML...      │
└──────────────────────────────┘
   ↑ "  .grid " in the name is aspirational — S03 is what
     actually makes these sit side by side.
```

### Step-by-Step Build Order

1. Build the three `.card` divs inside one `.grid` wrapper — give only the first card `id="card-featured"`.
2. Write the base `.card` rule: fixed width, padding, border, radius, shadow.
3. Add the three conflicting background-color rules exactly as shown, in the order shown — order matters for the lesson.
4. **Stop. Predict** which color wins by computing each selector's `(A, B, C)` by hand (see below) *before* reloading the browser.
5. Reload, confirm your prediction in DevTools, then do the two-part debugging exercise in "Do This Yourself."

### Code (the buggy version — build this first)

```html
<div class="grid">
  <div class="card" id="card-featured">
    <h3>Streaming Responses</h3>
    <p>See tokens arrive in real time.</p>
  </div>
  <div class="card">
    <h3>Dark Mode</h3>
    <p>One class toggles the entire theme.</p>
  </div>
  <div class="card">
    <h3>Accessible by Default</h3>
    <p>Built on semantic HTML, not ARIA patches.</p>
  </div>
</div>
```

```css
* { box-sizing: border-box; }

.card {
  width: 300px;
  padding: 24px;
  border: 2px solid #cbd5e1;
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

/* Bug 1: this rule "should" turn the featured card blue, but doesn't */
.grid .card { background-color: white; }
#card-featured { background-color: #dbeafe; }
.card.featured { background-color: yellow; }
```

### Predict, then check
Which background color does `#card-featured` actually render? **Compute the three selectors' `(A, B, C)` scores yourself before reading on.**

- `.grid .card` → two classes → `(0, 2, 0)`
- `#card-featured` → one ID → `(1, 0, 0)`
- `.card.featured` → two classes → `(0, 2, 0)` (and doesn't even match — the HTML never got `class="featured"` added)

**Answer:** `#card-featured` wins outright — `A=1` beats any number of classes, per S02 §3.2's worked example 2. This is deliberately the same lesson, applied to code you wrote yourself instead of a canned example.

### Explain the box-model half of this scenario
Without the global `* { box-sizing: border-box; }` line, each `.card` would render at `300 + 48 (padding) + 4 (border) = 352px`, not 300px — the exact arithmetic from S02 §10 Exercise 2. It's included here already fixed; delete that one line yourself and watch the cards visibly widen to confirm the bug is real, not theoretical.

### Do This Yourself
1. Delete the `* { box-sizing: border-box; }` line, reload, and measure a card in DevTools (element's computed width should now show ~352px).
2. Restore it. Then remove the `id="card-featured"` attribute entirely and confirm the card's background falls back to white — because now only the `(0,2,0)` rule matches at all.

### Reference
S02 §3.2, §4

---

## 7. Scenario 5 — The Dashboard Shell (Semantics + `data-*` + Buttons)

### Real-world context
Nearly every internal tool — an admin panel, a support dashboard, the BIA app itself — shares one shell: header, collapsible sidebar, main content. This is the layout you'll extend with real Flexbox in S03, but the *semantic skeleton* is 100% S01/S02 content already.

### Objective
Build a dashboard shell with a working (CSS-only, no JS yet) sidebar-item active-state, using `data-*` attributes as the single source of truth for both meaning and styling — exactly the pattern S01 §5 previewed.

### Why this matters (industry)
S01 §5 states this outright: this is the *exact* mechanism Radix UI (underneath every shadcn/ui component) uses in production — `data-state="open"` written onto the DOM, styled via an attribute selector. You are learning today, in plain CSS, the mechanism a widely-used component library is built on.

### What
- Landmarks + `<nav>` + lists of buttons — S01 §6, §7
- `<button>` vs `<div>`, `type="button"` — S01 §7
- `data-*` attributes — S01 §5
- Attribute selectors, `:hover` — S02 §7

### How
1. `<header>`, `<nav aria-label="Sidebar">`, `<main>`.
2. Sidebar items are real `<button type="button">`s, each carrying `data-active="true"` on exactly one.
3. CSS targets `[data-active="true"]` directly — no class needed, no JS needed yet.

### What We're Building — the Component List

| # | Component | Element | Traces to |
|---|---|---|---|
| 1 | Header bar | `header` > `h1` | S01 §6 |
| 2 | Sidebar | `nav aria-label="Sidebar"` > `ul` of `button` | S01 §5, §6, §7 |
| 3 | Main content | `main` > `h2` + `p` | S01 §6 |

### How It Should Look

`header`, `nav`, and `main` are all block-level landmarks with no layout tool applied — they stack vertically, in source order. The "sidebar" is a full-width band above the main content at this point in the syllabus, not a true side-by-side sidebar yet.

```text
┌──────────────────────────────┐
│ BIA Dashboard                  │  ← header
├──────────────────────────────┤
│ Conversations   (highlighted)  │  ← nav — full-width for now
│ Usage Analytics                │     (S03 moves it beside main)
│ Settings                       │
├──────────────────────────────┤
│ Conversations                  │  ← main
│ Select a conversation from...  │
└──────────────────────────────┘
```

### Step-by-Step Build Order

1. Add `<header>` with the `<h1>BIA Dashboard</h1>`.
2. Add `<nav aria-label="Sidebar">` with a `<ul>` of three `<li><button type="button">…</button></li>` items.
3. Put `data-active="true"` on exactly one button — "Conversations."
4. Add `<main>` with an `<h2>` and a placeholder paragraph.
5. Write the CSS in this order: box-sizing reset → base `nav button` styles → `:hover` → `[data-active="true"]` last, so it visibly overrides the base style.

### Code

```html
<header>
  <h1>BIA Dashboard</h1>
</header>

<nav aria-label="Sidebar">
  <ul>
    <li><button type="button" data-active="true">Conversations</button></li>
    <li><button type="button">Usage Analytics</button></li>
    <li><button type="button">Settings</button></li>
  </ul>
</nav>

<main>
  <h2>Conversations</h2>
  <p>Select a conversation from the sidebar.</p>
</main>
```

```css
* { box-sizing: border-box; }

nav button {
  display: block;
  width: 100%;
  text-align: left;
  padding: 0.5rem 1rem;
  border: none;
  background: none;
  cursor: pointer;
}

nav button:hover {
  background-color: #f1f5f9;
}

nav button[data-active="true"] {
  background-color: #dbeafe;
  font-weight: 600;
}
```

### Explain
- Every sidebar item is a real `<button>`, not a styled `<li>` or `<a>` — so Tab/Enter/Space work for free, per S01 §7's "single most important gotcha in this session."
- `[data-active="true"]` (S02 §7) is a **plain CSS attribute selector** — the same mechanism, unabstracted, that a design system like Radix builds entire component libraries on. You're not simulating the pattern; you're using it.
- `cursor: pointer` on a `<button>` is slightly redundant (buttons already show a pointer by default in most browsers) but is included because you'll need this exact property the moment you style a `<div>`-based custom control later in the syllabus.

### Do This Yourself
In DevTools, manually move `data-active="true"` from the "Conversations" button to "Settings" (double-click the attribute in the Elements panel). Confirm the highlight moves instantly — pure CSS, zero JavaScript, reacting to an attribute value.

### Reference
S01 §5, §6, §7 · S02 §7

---

## 8. Scenario 6 — The Three-Theme Design System (Custom Properties)

### Real-world context
Every mature product ships light mode, dark mode, and increasingly a high-contrast/accessibility mode. This is the single mechanism S02 §6 calls "more important than the cascade or the box model" for this specific syllabus — and it's true industry-wide: GitHub, Tailwind, Material Design, and Radix Themes all use exactly this.

### Objective
Extend Scenario 5's dashboard shell to support three themes, switchable by one class on `<html>`, with zero JavaScript and zero per-component color logic.

### Why this matters (industry)
S02 §9: this mechanism is instantaneous (browser cascade, not JS re-render) and requires zero coordination between components — a button built in isolation still theme-switches correctly because it only ever references a variable name, never a literal color.

### What
- `:root` custom properties + inheritance — S02 §6
- Class-based overrides (`.dark`, `.high-contrast`) — S02 §6
- `var()` with fallback — S02 §6

### How
1. Declare all colors as `--variables` in `:root` (light defaults).
2. Redeclare the *same names* inside `.dark` and `.high-contrast` blocks.
3. Every component (reuse Scenario 5's CSS) references `var(--name)`, never a literal hex code.
4. Add `--focus-ring-color`, themed independently in each mode.

### What We're Building — the Component List

| # | Component | Element | Traces to |
|---|---|---|---|
| 1 | Token declarations | `:root { --name: value; }` | S02 §6 |
| 2 | Dark-mode overrides | `.dark { --name: value; }` (same names) | S02 §6 |
| 3 | High-contrast overrides | `.high-contrast { --name: value; }` (same names) | S02 §6 |
| 4 | Themed components | Scenario 5's `body`, `nav button[data-active]`, `:focus-visible` — unchanged, now reading `var(--name)` | S02 §6 |

### How It Should Look

The *shape* on screen is identical to Scenario 5 — same header, sidebar, main. What changes is only color, driven by which class sits on `<html>`. Three color states of the same shell:

```text
Light (default)          Dark (.dark)              High-contrast (.high-contrast)
┌────────────┐           ┌────────────┐             ┌────────────┐
│ white bg    │           │ near-black  │             │ pure black  │
│ dark text   │           │ light text  │             │ white text  │
│ blue active │           │ slate active│             │ yellow ring │
└────────────┘           └────────────┘             └────────────┘
        same HTML structure, same component CSS — only the
        class on <html> and the :root/.dark/.high-contrast
        variable blocks differ.
```

### Step-by-Step Build Order

1. Take Scenario 5's HTML unchanged — do not touch `header`/`nav`/`main` markup at all.
2. Declare every color Scenario 5 used as a `--variable` inside `:root`, with light-mode values.
3. Copy the same variable *names* into a `.dark { }` block with dark values, then into a `.high-contrast { }` block with high-contrast values.
4. Go back through Scenario 5's CSS rules and replace every literal hex color with `var(--name)`.
5. Add `class="dark"` to `<html>` and confirm the whole shell re-colors with zero other edits.

### Code

```css
* { box-sizing: border-box; }

:root {
  --background: #ffffff;
  --foreground: #0f172a;
  --sidebar-active: #dbeafe;
  --focus-ring-color: #2563eb;
}

.dark {
  --background: #0b1220;
  --foreground: #f1f5f9;
  --sidebar-active: #1e293b;
  --focus-ring-color: #93c5fd;
}

.high-contrast {
  --background: #000000;
  --foreground: #ffffff;
  --sidebar-active: #000000;
  --focus-ring-color: #facc15;
}

body {
  background-color: var(--background);
  color: var(--foreground);
}

nav button[data-active="true"] {
  background-color: var(--sidebar-active);
}

nav button:focus-visible {
  outline: 2px solid var(--focus-ring-color);
  outline-offset: 2px;
}
```

```html
<html lang="en" class="dark">
```

### Explain
- Nothing in the component CSS (`nav button[data-active]`, `:focus-visible`) changed between Scenario 5 and this one — only the `:root`/`.dark`/`.high-contrast` blocks exist to be swapped. This is the actual point: **components never know which theme is active.**
- Swapping the `class` on `<html>` between `dark` and `high-contrast` is a one-line change with zero cascading edits elsewhere — the direct payoff of S02 §6's inheritance mechanism.
- `--focus-ring-color` themed per-mode matters specifically because a pure-black high-contrast theme with a blue focus ring would be nearly invisible — theming the ring color, not just backgrounds, is what a real accessibility-conscious design system does.

### Do This Yourself
Change `class="dark"` to `class="high-contrast"` on `<html>`, reload, then Tab to a sidebar button and confirm the focus ring is now yellow, not blue — proving the theme change reached every themed property, not just backgrounds.

### Reference
S02 §6, §10 (Exercise 3)

---

## 9. Scenario 7 — Capstone: The Accessible Streaming Chat Shell

### Real-world context
This is S01 §11's own capstone, extended with everything CSS gave you since. It is, deliberately, the actual shape of the BIA Unified Intelligence Platform this whole syllabus is building toward — the same file you'll keep extending in S03 (Flexbox/Grid layout) and S06+ (real JavaScript behavior).

### Objective
Merge every concept from Scenarios 1–6 into one file: semantic shell, accessible form, `data-*`-driven state, and a working theme system — and identify, out loud, which session/section each piece traces back to.

### Why this matters (industry)
This is the exact deliverable a take-home assignment or a first sprint ticket at a real company looks like: "build the accessible shell for the chat feature, theme-aware, before the backend is even wired up." Nothing here is invented busywork — S01 §12 names Eli Lilly's own Section 508/WCAG obligations, and S02 §9 names GitHub's public dark-mode architecture, as the direct real-world precedent for this exact file.

### What
Everything above, plus:
- `aria-live="polite"` on the message region — S01 §9
- Full theme system wired to a real component tree — S02 §6, §8

### What We're Building — the Component List

| # | Component | Traces back to | Key concept |
|---|---|---|---|
| 1 | Header bar (sidebar-toggle button + title) | Scenario 5's header/button pattern | `<button aria-label>`, S01 §7/§9 |
| 2 | Chat-history sidebar (`nav` + `data-active` buttons) | Scenario 5, verbatim | `data-*` attribute selectors, S01 §5 / S02 §7 |
| 3 | Conversation region (`#message-list`, `aria-live`) | New — S01 §9's live-region rule, applied for the first time | `aria-live="polite"` |
| 4 | Message form (label + textarea + Send + Stop) | Scenario 2's accessible-form pattern, adapted | `<label for>`, native validation, icon-only `aria-label` buttons |
| 5 | Footer copyright | Scenario 1, verbatim | Landmark semantics, S01 §6 |
| 6 | Theme variables (`:root` / `.dark`) | Scenario 6, extended with two more tokens (`--primary`, `--border`) | Custom properties, S02 §6 |

Six components, four of which are *reused outright* from earlier scenarios rather than rebuilt — that reuse is the point: a real capstone assembles existing, already-tested pieces, it doesn't start from a blank file.

### How It Should Look

No Flexbox or Grid exists yet in this syllabus (that's S03) — so, honestly, the regions stack top-to-bottom in normal document flow. This is deliberate, not a limitation to hide: it's exactly what the Bridge Forward section below calls out as the thing S03 will change next.

```text
┌────────────────────────────────────────┐
│ ☰   BIA Assistant                      │  ← header
├────────────────────────────────────────┤
│ Chat History                           │  ← nav (sidebar) — full-width,
│  • Kubernetes manifest question        │     stacked ABOVE main because
│  • Drizzle schema design               │     no layout tool exists yet
├────────────────────────────────────────┤
│ Conversation                           │  ← main
│ ┌─────────────────────────────────┐   │
│ │ You: How do I center a fixed-... │   │  ← #message-list
│ │ Assistant: margin: 0 auto ...    │   │     (aria-live="polite")
│ └─────────────────────────────────┘   │
│ [ Your message textarea           ]   │
│ [ Send ]   [ Stop ]                    │
├────────────────────────────────────────┤
│ © 2026 BIA Unified Intelligence Plat.  │  ← footer
└────────────────────────────────────────┘
```

### Step-by-Step Build Order

Build outside-in, exactly like every prior scenario: skeleton and landmarks first, content and interactivity second, theme last (theme is the one layer allowed to come last precisely *because* every component below only ever references a variable name, never a literal color — that's Scenario 6's entire payoff).

1. **Skeleton.** `<!DOCTYPE html>` → `<html lang="en" class="dark">` → `<head>` (charset, viewport, title, stylesheet link) → `<body>`.
2. **Header.** One `<header>` with the sidebar-toggle `<button aria-label="Toggle chat history sidebar">` and an `<h1>`.
3. **Sidebar.** `<nav aria-label="Chat history">`, an `<h2>`, and a `<ul>` of real `<button type="button">`s — reuse Scenario 5's markup exactly, mark exactly one with `data-active="true"`.
4. **Main region skeleton.** `<main>` with an `<h2>Conversation</h2>` and an empty `<div aria-live="polite" id="message-list">`.
5. **Seed the conversation.** Add two `<p>` lines inside `#message-list` — a user line and an assistant line — so there's something for `aria-live` to have already announced once, before any real streaming exists.
6. **Message form.** Reuse Scenario 2's label/input pattern, but swap the `<input>` for a `<textarea>`; add a submit `Send` button and an icon-only `Stop` button with `aria-label="Stop generating response"`, `type="button"` so it can never submit the form.
7. **Footer.** One `<footer>` with the copyright line, reused verbatim from Scenario 1.
8. **Wire the theme.** Copy Scenario 6's `:root`/`.dark` blocks in, add the two new tokens this capstone needs (`--primary`, `--border`), then point every component's CSS at `var(--name)` — never a literal hex value.
9. **Verify**, per the Do This Yourself steps below, before calling this done.

### Code

```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BIA Chat — Capstone Shell</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>

  <header>
    <button type="button" aria-label="Toggle chat history sidebar">☰</button>
    <h1>BIA Assistant</h1>
  </header>

  <nav aria-label="Chat history">
    <h2>Chat History</h2>
    <ul>
      <li><button type="button" data-active="true">Kubernetes manifest question</button></li>
      <li><button type="button">Drizzle schema design</button></li>
    </ul>
  </nav>

  <main>
    <h2>Conversation</h2>

    <div aria-live="polite" id="message-list">
      <p><strong>You:</strong> How do I center a fixed-width box?</p>
      <p><strong>Assistant:</strong> <code>margin: 0 auto</code> on a block element with an explicit width.</p>
    </div>

    <form>
      <label for="chat-input">Your message</label>
      <textarea id="chat-input" name="message" rows="1" required></textarea>
      <button type="submit">Send</button>
      <button type="button" aria-label="Stop generating response">Stop</button>
    </form>
  </main>

  <footer>
    <p>&copy; 2026 BIA Unified Intelligence Platform</p>
  </footer>

</body>
</html>
```

```css
/* styles.css */
* { box-sizing: border-box; }

:root {
  --background: #f8fafc;
  --foreground: #0f172a;
  --primary: #dc2626;
  --primary-foreground: #ffffff;
  --muted: #e2e8f0;
  --border: #cbd5e1;
  --focus-ring-color: #2563eb;
}

.dark {
  --background: #0b1220;
  --foreground: #f1f5f9;
  --primary: #ef4444;
  --primary-foreground: #ffffff;
  --muted: #1e293b;
  --border: #334155;
  --focus-ring-color: #93c5fd;
}

body {
  background-color: var(--background);
  color: var(--foreground);
  font-family: system-ui, sans-serif;
  margin: 0;
}

nav button {
  display: block;
  width: 100%;
  text-align: left;
  padding: 0.5rem 1rem;
  border: none;
  background: none;
  color: inherit;
  cursor: pointer;
}

nav button[data-active="true"] {
  background-color: var(--muted);
  font-weight: 600;
}

#message-list {
  border: 1px solid var(--border);
  border-radius: 0.5rem;
  padding: 1rem;
  margin-bottom: 1rem;
}

textarea, button {
  font: inherit;
}

textarea {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid var(--border);
  border-radius: 0.375rem;
  background-color: var(--background);
  color: var(--foreground);
}

button[type="submit"] {
  background-color: var(--primary);
  color: var(--primary-foreground);
  border: 1px solid var(--primary);
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
}

button:focus-visible, textarea:focus-visible {
  outline: 2px solid var(--focus-ring-color);
  outline-offset: 2px;
}
```

### Explain — trace every piece back to its origin, out loud
Before moving on, say each of these out loud (S01/S02's own "explain it back to yourself" pattern):
- Sidebar toggle and Stop button use `aria-label` because they carry no visible text — **S01 §9**.
- Every non-submit button is `type="button"` so it never accidentally submits the form and reloads the page — **S01 §7, Pitfall 2**.
- `aria-live="polite"` on `#message-list` is what will make real streamed tokens accessible, months from now, because it's wired correctly *today* — **S01 §9**.
- Zero hex colors exist outside the `:root`/`.dark` blocks — every component only ever references a variable name — **S02 §6, §8**.
- `* { box-sizing: border-box; }` on line one, as always — **S02 §4**.

### Do This Yourself
1. Tab through the entire page start to finish. Confirm every interactive element gets a visible focus ring, and that the Stop button never triggers a reload.
2. Remove `class="dark"` from `<html>`. Confirm every color flips to light mode simultaneously — sidebar, message box, button, focus ring — with zero other edits.
3. Open DevTools' accessibility inspector on `#message-list` and confirm it reports as a live region.

### Reference
S01 §5, §6, §7, §9, §11 · S02 §4, §6, §7, §8

---

## 10. Zero-to-Hero Mastery Checklist (Industry-Wide)

Confirm every line, without looking back at S01/S02 or this document:

**Structure & Semantics**
- [ ] I can write a correct document skeleton from memory
- [ ] I can explain why `<header>`/`<nav>`/`<main>`/`<footer>` beat `<div class="...">`, in accessibility terms
- [ ] I default to `<button>` for anything clickable, and I know why a clickable `<div>` is broken
- [ ] I know when a `<table>` is the right tool and when it's a layout anti-pattern

**Forms & Accessibility**
- [ ] Every `<input>`/`<textarea>` I write has a paired `<label for>`
- [ ] I reach for native validation (`required`, `type="email"`, `minlength`) before writing JS validation
- [ ] I know the "first rule of ARIA" from memory and can name the three attributes that matter most (`aria-label`, `aria-hidden`, `aria-live`)
- [ ] I know exactly why a streaming chat region needs `aria-live="polite"`

**CSS Mechanics**
- [ ] I can compute a selector's `(A, B, C)` specificity by hand, on sight
- [ ] I can state the three-step cascade tiebreaker order from memory
- [ ] I know why `box-sizing: border-box` goes on line one of every stylesheet I write
- [ ] I choose `rem` over `px` for spacing/type by default, and can say why in terms of compounding

**Design Systems (the production-grade skill)**
- [ ] I can build a full light/dark/high-contrast theme system using only custom properties and class toggles — no JavaScript touching individual components
- [ ] I understand this is the *exact* mechanism GitHub, Tailwind, Material Design, and Radix Themes use in production, not a toy simplification

If any box is unchecked, redo that scenario before starting Session S03 (Flexbox & Grid) — S03 assumes every one of these is now reflexive, not something you're still reasoning through step by step.

---

## 11. Further Reading (carried forward from S01/S02, still the primary sources)

- MDN — Learn: Getting Started with HTML — https://developer.mozilla.org/en-US/docs/Learn/HTML
- MDN — ARIA — https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA
- MDN — CSS Cascade, Specificity, and Inheritance — https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts
- MDN — The Box Model — https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Box_model
- MDN — Using CSS Custom Properties — https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties
- WebAIM — The WebAIM Million — https://webaim.org/projects/million/

## 12. Bridge Forward

Every scenario above assumed default top-to-bottom flow — no real layout control yet. Session S03 (Flexbox & Grid) is what turns Scenario 7's capstone shell from "stacked sections" into an actual sidebar-beside-main-content dashboard, and `position: sticky` is what keeps its header and input bar in place while the conversation scrolls. Nothing here gets discarded — S03 arranges these exact same semantic, accessible, themed boxes; it doesn't replace them.
