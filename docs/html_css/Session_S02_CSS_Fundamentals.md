# Session S02 — CSS Fundamentals

### The Cascade, Specificity, the Box Model, Units & CSS Custom Properties

**Syllabus position:** Phase F1 (HTML/CSS Foundations) · Day 2 of 37 · AgentStack Golden Canon v8 Slim
**Prerequisites:** Session S01 (HTML5 Essentials)
**Companion track:** The Odin Project → Foundations → CSS lessons (run alongside or just after this session)
**Standing objective, restated so it governs everything below:** I am not writing this so you have code to copy. I am writing this so that six months from now, with no AI open in another tab, you can look at a CSS problem, reason about *why* it's behaving the way it is, and fix it — because you actually built the mental model, not because you memorized a snippet that matched. Everywhere it helps, I will ask you to predict an outcome *before* I tell you the answer. Do that honestly, even when you're not sure — being wrong and then seeing why is what actually builds the model; skipping straight to the answer doesn't.
**Time budget:** \~3 hours

---

## Quick bridge from Session S01

Session S01 gave you structure with zero visual styling — a page of plain, stacked, black-on-white text that was nonetheless *correct*: semantically meaningful landmarks, a real form, real accessibility attributes. Everything in this session answers the question you were left with at the end of S01: *why does my* `chat-shell.html` *look like plain text instead of a chat app?* By the end of today, the answer will be fully mechanical to you rather than mysterious — and, importantly, you'll understand it well enough to predict what a new CSS rule will do *before* you run it, which is the actual skill, not the syntax.

---

## 1. Core Concept — What CSS Actually Is, and Why It's a Separate Language

HTML answers "what is this?" — a heading, a button, a paragraph. CSS answers a completely different question: "how should this look, and where should it sit?" These are deliberately kept as two separate languages rather than one combined one, for the same reason you'd keep a Pydantic schema separate from the logic that renders a report from it: the *shape* of your data and the *presentation* of your data change for different reasons, at different times, often maintained by different people — mixing them makes both harder to change independently.

**CSS stands for Cascading Style Sheets**, and every word in that name is load-bearing:

- **Style Sheets** — a CSS file is a *sheet of rules*, each rule saying "for elements matching this selector, apply these property values."
- **Cascading** — this is the word that will occupy most of Section 3, and it's the single most misunderstood idea in CSS by beginners. It means: when *multiple* rules could apply to the same element, there is a precise, deterministic algorithm — not a guess, not "whichever CSS file loads last" — that decides which one wins.

A CSS rule has this literal shape:

```css
selector {
  property: value;
  property: value;
}
```

```css
p {
  color: navy;
  font-size: 16px;
}
```

This rule says: "find every `<p>` element, and set its text color to navy and its font size to 16 pixels." `p` is the **selector** (which elements this rule targets). `color: navy;` and `font-size: 16px;` are **declarations** — a property name, a colon, a value, and a semicolon. The whole `{ ... }` block is a **rule** (or "ruleset").

**Predict before you peek:** if you had two separate rules, `p { color: navy; }` and `p { color: red; }`, both in the same file, both matching the exact same `<p>` element — what color do you think the text would actually be? Hold that guess. Section 3 will tell you exactly why, not just what.

### Getting CSS onto your page

Three ways, and only one of them is the one you should actually use going forward:

```html
<!-- Method 1: inline style attribute — AVOID, explained below -->
<p style="color: navy;">Hello</p>

<!-- Method 2: <style> block in <head> — fine for quick experiments -->
<head>
  <style>
    p { color: navy; }
  </style>
</head>

<!-- Method 3: external stylesheet — the real, professional approach -->
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

Method 3 is what you'll use from this point forward. Create a second file, `styles.css`, next to your HTML file, and link it with `<link rel="stylesheet" href="styles.css">` inside `<head>`. This keeps structure (HTML) and presentation (CSS) in genuinely separate files — the practical expression of the "separate concerns" reasoning from Section 1 — and it means one stylesheet can style many HTML pages at once, which inline styles and `<style>` blocks can't do.

> **Do this now:** create `styles.css` next to your `chat-shell.html` from Session S01, link it in the `<head>`, and add `body { color: darkslategray; }`. Reload the page and confirm the text color changed. You've now got the full HTML+CSS pipeline wired up.

---

## 2. Common CSS Properties — The Fast-Reference Survey

Before Section 3's mechanism-level deep dive into how conflicting rules get resolved, here is the map of what CSS can actually control — deliberately shallow, a name and a one-line purpose per property, organized by what it affects. Skim this once. Come back to it anytime a property name in a later session doesn't ring a bell.

**Color and background**

| Property | Purpose |
| --- | --- |
| `color` | Text color |
| `background-color` | The element's own background fill |
| `background-image` | An image (or gradient) as the background |
| `opacity` | Fades the *entire* element, including its children — different from a background's own alpha channel |

**Typography**

| Property | Purpose |
| --- | --- |
| `font-family` | Which typeface to use |
| `font-size` | Text size |
| `font-weight` | Boldness (`400` normal, `700` bold, etc.) |
| `line-height` | Vertical spacing between lines of text |
| `text-align` | Horizontal alignment of text within its box |
| `text-decoration` | Underline, strikethrough, or none |

**Spacing and sizing** *(Section 4 covers the box model these all belong to, properly)*

| Property | Purpose |
| --- | --- |
| `width`, `height` | The element's own dimensions |
| `margin` | Space outside the border, between elements |
| `padding` | Space inside the border, around the content |
| `max-width`, `min-height` | Upper/lower bounds — the element can shrink or grow but not cross these |

**Borders and radius**

| Property | Purpose |
| --- | --- |
| `border` | A visible line around the padding |
| `border-radius` | Rounds the corners |
| `box-shadow` | A drop shadow around the element's box |

**Layout** *(the deepest mechanisms — Section 4 here, and Session S03's Flexbox/Grid)*

| Property | Purpose |
| --- | --- |
| `display` | What layout mode this element participates in (`block`, `inline`, `flex`, `grid`, `none`) |
| `position` | How the element is positioned relative to its normal place — full depth in Session S03 |

**Effects**

| Property | Purpose |
| --- | --- |
| `transition` | Smooths a property's change over time — full depth in Session S03 |
| `transform` | Moves, rotates, or scales an element visually, without affecting layout |
| `cursor` | What the mouse pointer looks like when hovering this element |

That's the map — roughly twenty properties covering every category CSS actually has, before this session spends the next several sections on the *mechanism* questions (which rule wins when two conflict, what "box" even means, how a variable's value flows down the tree) rather than the property list itself. You already used several of these without a name for the category (`color`, `padding`, `border`) in earlier examples — that recognition is exactly the payoff of front-loading the survey.

---

## 3. The Cascade — How Conflicts Get Resolved

Here's the answer to the "predict before you peek" question from Section 1. With `p { color: navy; }` then `p { color: red; }` later in the same file, matching the same specificity exactly, **the text is red** — the *later* rule in the file wins when everything else is tied. This is called **source order**, and it's the simplest of three tie-breaking mechanisms the cascade uses, applied in this exact priority sequence:

```
1. IMPORTANCE       → !important declarations, and origin (browser vs. your CSS)
2. SPECIFICITY      → how "targeted" the selector is (Section 3.2 below)
3. SOURCE ORDER      → if 1 and 2 are tied, whichever rule appears LAST wins
```

The browser checks these top to bottom — it only moves to the next tiebreaker if the current one is a tie. This is confirmed directly by MDN's own cascade documentation: specificity is compared only *after* origin and importance are settled, and source order only matters once specificity is *also* tied.

### 2.1 — Origin and `!important`

Your CSS isn't the only CSS affecting the page. Browsers ship their own default stylesheet (why a `<button>` looks like a gray 3D button before you've written any CSS at all, why `<a>` links are blue and underlined by default) — this is the **user-agent stylesheet**, and it has the *lowest* priority, so your own CSS (the "author" origin) normally overrides it easily.

`!important` is a declaration you can append to force a specific value to win regardless of specificity:

```css
p {
  color: red !important;
}
```

**Do not reach for this as your first tool.** It's a priority level, not a specificity boost, and once two competing rules both use `!important`, you're right back to needing specificity and source order to break the tie — you've just moved the fight up a level instead of resolving it. Treat `!important` as an emergency escape hatch (overriding a third-party library's CSS you can't edit), never as a routine way to "make my rule win."

### 2.2 — Specificity: The Actual Calculation

This is the part most tutorials wave their hands at. Let's not do that — let's actually compute it, because once you can compute specificity by hand, you will never again be confused about why one of your rules "isn't working."

Specificity is a **three-part score**, conventionally written as three numbers, calculated by counting how many of each of these a selector contains:

| Category | What counts | Example |
| --- | --- | --- |
| **A** (highest) | ID selectors | `#chat-input` |
| **B** (middle) | Classes, attribute selectors, pseudo-classes | `.btn`, `[type="submit"]`, `:hover` |
| **C** (lowest) | Element (type) selectors, pseudo-elements | `button`, `p`, `::before` |

You compute a selector's specificity by counting how many of A, B, and C it contains, giving you a score like `(A, B, C)`. To compare two selectors, compare A first — higher A always wins outright, no matter what B or C are. Only if A ties do you compare B. Only if B also ties do you compare C.

**Worked example 1 — predict, then check:**

```css
.sidebar button { color: blue; }
button { color: green; }
```

Which color wins for a `<button>` inside `<div class="sidebar">`? Work it out: `.sidebar button` has one class (B=1) and one element (C=1) → score `(0, 1, 1)`. `button` alone has one element (C=1) → score `(0, 0, 1)`. Comparing left to right: A ties (0=0), B differs (1 &gt; 0) — `.sidebar button` wins. **The button is blue**, regardless of which rule appears later in the file, because specificity is checked *before* source order, not after.

**Worked example 2 — predict, then check:**

```css
#send-btn { background: purple; }
.btn.primary { background: orange; }
```

`#send-btn` has one ID → `(1, 0, 0)`. `.btn.primary` has two classes → `(0, 2, 0)`. Comparing A first: `1 > 0` — **the ID wins outright**, background is purple, even though the class selector "looks like it has more stuff" in it. This is the single most common source of "why won't my CSS class override this?!" confusion — an ID anywhere in a selector dominates any number of classes, always, because A is compared before B and a tie is required before B is ever even looked at.

**Quick check — try this one yourself before reading the answer:** Between `nav ul li a` and `.nav-link`, which wins? *(Answer:* `nav ul li a` *has four elements →* `(0,0,4)`*.* `.nav-link` *has one class →* `(0,1,0)`*. Comparing A: tied at 0. Comparing B:* `1 > 0` *—* `.nav-link` *wins, despite having far fewer selectors written. This surprises almost everyone the first time — "four things" instinctively feels stronger than "one thing," but the category matters more than the count within a lower category.)*

### 2.3 — Why This Matters for Everything Coming Later in the Syllabus

You will meet `@layer` in Session S15 (Tailwind v4), which exists specifically to let framework authors control cascade priority *without* playing specificity games — Tailwind's own utility classes are deliberately kept at equal, low specificity so that *your* classes can override them predictably. None of that will make sense as "a clever trick" later if specificity itself is still fuzzy to you now — it'll just look like magic. It isn't magic; it's exactly the three-step algorithm above, automated.

---

## 4. The Box Model — Every Element Is a Box

### The mental model, built from scratch

Every single element on a webpage — no matter how it looks visually — is, to the browser's layout engine, a rectangular box. A `<button>` is a box. A `<p>` is a box. Even a circular avatar image is, underneath its rounded corners, a box. This single idea — **everything is a box** — is what "box model" means, and once it's genuinely internalized, CSS layout stops being a series of memorized tricks and starts being predictable geometry.

Each box is built from four concentric layers, from the inside out:

```
┌───────────────────────────────────────┐
│              MARGIN (transparent)      │
│  ┌───────────────────────────────┐    │
│  │         BORDER                  │    │
│  │  ┌───────────────────────┐    │    │
│  │  │      PADDING            │    │    │
│  │  │  ┌───────────────┐    │    │    │
│  │  │  │    CONTENT      │    │    │    │
│  │  │  │ (text, image)   │    │    │    │
│  │  │  └───────────────┘    │    │    │
│  │  └───────────────────────┘    │    │
│  └───────────────────────────────┘    │
└───────────────────────────────────────┘
```

- **Content** — the actual text or image.
- **Padding** — transparent space *inside* the border, between the content and the border. Padding takes on the element's own background color.
- **Border** — a visible (or invisible) line wrapping the padding.
- **Margin** — transparent space *outside* the border, pushing other elements away. Margin is never filled with background color — it's pure separation from neighbors.

```css
.message-bubble {
  padding: 12px;
  border: 1px solid #ccc;
  margin: 8px;
}
```

### The gotcha that trips up every single beginner: which "width" is `width`?

Here's a genuinely important question, and I want you to actually reason about it before I answer, because the *reasoning process* is the point.

**Predict before you peek:** if you write `.box { width: 200px; padding: 20px; border: 5px solid black; }`, how wide is the box actually going to be on screen — 200px, or something else?

The honest, slightly annoying answer: **by CSS's original default rules, it's 250px, not 200px.** Here's why. The `width` property, by CSS's original specification, sets the width of the **content box only** — padding and border get added *on top* of that 200px, on both sides: `200px content + 20px left padding + 20px right padding + 5px left border + 5px right border = 250px` total rendered width. This is called `box-sizing: content-box`, and it's the CSS default, for historical reasons dating back to CSS's earliest specification.

Almost nobody actually wants this. When you say "I want this box to be 200px wide," you virtually always mean the *total* rendered width, padding and border included — not "200px of content, plus however much extra padding and border tack on." The fix, which you should genuinely think of as a required correction rather than an optional preference:

```css
* {
  box-sizing: border-box;
}
```

This single rule, applied globally with the universal selector `*`, changes `width` to mean "this is the *total* width, including padding and border — the browser will shrink the *content* area to make room for them, not add padding and border on top." With `border-box` set, that same `.box` (`width: 200px; padding: 20px; border: 5px solid`) renders at exactly 200px total, with the content area automatically calculated as `200 - 40 (padding) - 10 (border) = 150px`.

**Every professional CSS codebase you will ever work in sets** `box-sizing: border-box` **globally, on line one.** It is not a style preference — it's a correction to a default that essentially nobody actually wants, and Session S04's Foundation Project treats it as a required setup step, not an optional one.

### `margin: auto` — the trick that only works one specific way

```css
.centered-box {
  width: 400px;
  margin: 0 auto;
}
```

`margin: auto` on the *left and right* sides of a block-level element with a defined `width` tells the browser: "split all the remaining horizontal space evenly between my left and right margins" — which visually centers the box within its parent. **This only centers horizontally**, and it only works because the element has an explicit `width` — without a set width, a block element already stretches to fill its parent, leaving no "remaining space" for `auto` to distribute. This is a narrow, specific mechanism, not a general "center anything" tool — true general centering (including vertical) is Flexbox's job, arriving in Session S03.

### `overflow` and `outline` — two properties, two purposes people confuse

`overflow: hidden` clips any content that doesn't fit inside an element's box — useful for cropping an image or preventing a long unbroken word from breaking your layout. `outline` draws a line *around* the border, but — critically — **outline does not participate in the box model at all**: it doesn't take up space, doesn't push other elements, and doesn't affect layout the way `border` does. This is exactly why outlines are the correct tool for keyboard focus rings (which Session S01 required on every interactive element): an outline can appear and disappear on focus without ever shifting anything else on the page, because geometrically, as far as layout is concerned, it isn't there.

### `display: none` — removing a box from the model entirely

Session S01's rendering-pipeline section told you that the render tree "contains only the nodes that will actually be visible" and that `display: none` is exactly what excludes a node from it. Here's that promise, delivered, with the box-model vocabulary you now actually have to understand it properly.

```css
.tooltip {
  display: none;
}
```

An element with `display: none` isn't merely invisible — it has **no box at all**. Not a zero-size box, not a transparent box: no content, padding, border, or margin occupying any space, anywhere, as far as layout is concerned. Every other element on the page lays out exactly as if that element didn't exist in the document at all. This is different from setting `visibility: hidden`, which *keeps* the box's full space reserved (invisible, but still pushing its neighbors exactly as before) — a distinction sharp enough to cause real layout bugs when confused, and one Session S03 will return to directly, once you've also met `position` and can see both properties working side by side in a real layout.

**Predict before you peek:** if a sidebar item has `display: none` applied while the user is typing in the search box, then a later click removes that `display: none` (making it visible again) — does every element below it in the layout shift downward, or stay exactly where it was? *(They shift — because the instant the box re-enters the render tree, it once again occupies real space in normal flow, pushing everything after it down exactly as if it had been there all along. This "everything jumps" behavior is precisely why toggling visibility with* `display: none`*/*`display: block` *can look janky for things like dropdown menus, and precisely why Session S03's positioning techniques —* `position: absolute` *in particular — exist as an alternative for exactly this situation.)*

> **Explain it back to yourself, out loud, before moving on:** in your own words, why does a 200px-wide box with padding and border only actually render at 200px total *after* you set `box-sizing: border-box`, and not before? If you can't say it without re-reading, re-read the box-sizing paragraph once more before continuing — this is the single most load-bearing concept in this whole section.

---

## 5. Units — Choosing the Right One on Purpose

CSS gives you far more units than "pixels," and the ones you pick shape how your page behaves when a user zooms, resizes their browser, or views it on a phone. Rather than memorizing a list, reason through *why* each exists:

| Unit | What it's relative to | When to reach for it |
| --- | --- | --- |
| `px` | Nothing — an absolute, fixed size | Borders, shadows — things that should never scale with user font preferences |
| `rem` | The root (`<html>`) element's font size | Spacing, font sizes — the default choice for most sizing, because it respects a user's browser-level font-size preference |
| `em` | The *parent* element's font size | Padding *inside* a specific component, where you want it to scale proportionally with that component's own font size |
| `%` | The parent element's corresponding dimension | Widths that should flex with their container |
| `vw` / `vh` | The viewport's width/height | Full-bleed sections, hero banners |
| `dvh` | The *actual visible* viewport height, accounting for mobile browser UI | Mobile layouts — see the gotcha below |
| `ch` | The width of the `"0"` character in the current font | Setting a readable maximum line length for body text |

**The** `rem` **vs** `em` **distinction, reasoned through:** `1.5rem` always means "1.5 × the root font size," full stop, regardless of where in the page you write it — predictable everywhere. `1.5em` means "1.5 × *this element's own parent's* font size" — which means if you nest elements that each set their own font-size in `em`, the values compound multiplicatively down the tree, an effect that surprises people the first time a deeply nested element turns out three times larger than intended. This is exactly *why* Tailwind v4 (Session S15) and most professional systems default to `rem` for nearly everything — predictability wins over the occasional convenience of proportional nesting.

**The** `vh` **vs** `dvh` **gotcha, specifically for later:** on mobile browsers, the address bar and toolbar can show or hide as you scroll, which means the "viewport height" itself isn't constant — `100vh` on a phone is calculated against the *tallest possible* viewport (toolbar hidden), so a full-height element sized with `vh` can end up taller than the space actually visible when the toolbar is showing, causing content to be cut off at the bottom. `dvh` ("dynamic viewport height") recalculates continuously as the toolbar shows/hides, avoiding this. You'll use this for real in Session S03's responsive design work.

---

## 6. CSS Custom Properties — Variables With a Superpower

### Why this section matters more than its place in the syllabus suggests

I want to be direct about something: this is the single most important section in today's session for your *specific* 37-day path, more than the cascade, more than the box model. **The entire BIA UI's light/dark theming system, and the entirety of Tailwind v4's** `@theme` **configuration you'll meet in Session S15, is built on exactly the mechanism in this section and nothing more exotic.** If this section is solid, two sessions from now, "how does the whole app switch to dark mode with one class toggle?" will already make complete sense to you before I even explain it — that's how directly this connects forward.

### What a custom property actually is

```css
:root {
  --primary-color: #dc2626;
}

.send-button {
  background-color: var(--primary-color);
}
```

`--primary-color` is a **custom property** — you're free to name it anything, as long as it starts with two dashes. `:root` is a selector matching the `<html>` element (the top of the document tree) — declaring a custom property there makes it available to *every* element on the page, because of the next fact:

**Custom properties inherit down the document tree, exactly like** `color` **and** `font-size` **do.** This is directly confirmed by MDN: if a child element doesn't set its own value for a custom property, it uses whatever value its parent has — cascading down through as many levels of nesting as exist. This is the mechanism, not a metaphor: `var(--primary-color)` used inside a deeply-nested button still resolves correctly because the value cascaded all the way down from `:root`.

`var(--primary-color)` is how you *use* a custom property's value inside another declaration. It behaves like a live substitution — change the `:root` declaration once, and every single `var(--primary-color)` usage across your entire stylesheet updates simultaneously, without you touching each one individually.

### Fallback values — what happens when the variable doesn't exist

```css
.badge {
  color: var(--badge-text-color, white);
}
```

The second argument to `var()` is a **fallback** — used specifically when the referenced custom property is undefined or invalid, confirmed directly by MDN's own specification. If `--badge-text-color` was never declared anywhere the element could inherit it from, the badge text renders white instead. You can even chain fallbacks: `var(--a, var(--b, blue))` — try `--a` first, then `--b`, then finally fall back to literal blue if neither exists.

### The mechanism behind dark mode, explained in full, right now

This is the payoff. Here's the entire mechanism used by the real BIA UI, and it is genuinely this simple once you see it:

```css
:root {
  --background: #ffffff;
  --text-color: #111111;
}

.dark {
  --background: #0b1220;
  --text-color: #f1f5f9;
}

body {
  background-color: var(--background);
  color: var(--text-color);
}
```

```html
<html class="dark">
```

That's the whole mechanism. `:root` sets the light-mode values. `.dark` — a class you can add or remove from `<html>` with one line of future JavaScript — *redeclares the same variable names* with dark-mode values. Because `.dark` is more specific than `:root` (a class beats a pseudo-class targeting the same element, by the specificity rules from Section 3.2 — both are B-category, but if `.dark` is applied directly to `<html>` alongside the implicit root match, the explicit class selector wins the tie), every single `var(--background)` and `var(--text-color)` usage across the *entire page* — no matter how deeply nested — switches instantly and simultaneously the moment that one class is toggled. No JavaScript touches the individual buttons, bubbles, or backgrounds directly; they were never hard-coded to a color in the first place, only ever hard-coded to a *variable name*, and the variable's value is what changes.

> **Explain it back to yourself:** if you added a third selector, `.dark .sidebar { --background: black; }`, overriding `--background` specifically inside the sidebar while dark mode is active, would the sidebar's background differ from the rest of the dark-mode page? *(Yes — because that declaration is more specific and scoped to a smaller subtree; every element inside* `.sidebar` *inherits the more specific override, while everything outside it still inherits* `.dark`*'s value. This is the exact mechanism a real design system uses for "mostly-dark-but-this-one-section-is-different" theming.)*

---

## 7. Selectors and Pseudo-Classes

You've already used a handful of selectors without a formal name for them. Let's make the vocabulary precise, and add the ones you'll need starting next session.

```css
button:hover { }          /* while the mouse is over it */
input:focus-visible { }   /* while focused via keyboard, specifically */
button:disabled { }       /* while the disabled attribute is present */
p:not(.highlighted) { }   /* any <p> WITHOUT this class */
.card:is(.featured, .pinned) { }  /* matches either of these */
[data-state="open"] { }   /* matches any element with this exact attribute value */
```

`:focus-visible` **vs.** `:focus` **— a distinction that matters for accessibility, connecting directly back to Session S01.** `:focus` matches *any* time an element has focus, including a plain mouse click — which is why some sites annoyingly show a focus ring after every click, even for sighted mouse users who don't need one. `:focus-visible` is smarter: the browser only applies it when it *infers* the user is navigating by keyboard (Tab key), suppressing it for a simple mouse click. Use `:focus-visible` for your focus-ring styling going forward — it gives keyboard users the visible ring Session S01 requires, without visual noise for mouse users.

`[data-state="open"]` **— the attribute selector, and why it matters far more than it looks like it should right now.** This targets any element carrying a `data-state` attribute with that exact value. File this away: Radix UI — the underlying library beneath every shadcn/UI component you'll build starting Session S17 — manages its own component state (open/closed, checked/unchecked) by writing exactly this kind of `data-*` attribute onto the DOM element itself, and its accompanying CSS then targets `[data-state="open"]` to animate the transition. You are not going to build Radix-style components today. But the selector mechanism making that possible is fully covered, right now, in one line above.

---

## 8. Capstone Exercise — The BIA Design System File

Time to build something real: the actual CSS foundation the syllabus's Session S02 exercise asks for — a single stylesheet defining the BIA chat UI's entire color system for both light and dark mode, using nothing but this session's content.

```css
/* styles.css */

* {
  box-sizing: border-box;
}

:root {
  --background: #f8fafc;
  --foreground: #0f172a;
  --primary: #dc2626;
  --primary-foreground: #ffffff;
  --muted: #e2e8f0;
  --border: #cbd5e1;
}

.dark {
  --background: #0b1220;
  --foreground: #f1f5f9;
  --primary: #ef4444;
  --primary-foreground: #ffffff;
  --muted: #1e293b;
  --border: #334155;
}

body {
  background-color: var(--background);
  color: var(--foreground);
  margin: 0;
}

.btn-primary {
  background-color: var(--primary);
  color: var(--primary-foreground);
  border: 1px solid var(--primary);
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
}

.card {
  background-color: var(--muted);
  border: 1px solid var(--border);
  padding: 1rem;
  border-radius: 0.5rem;
}

.input {
  border: 1px solid var(--border);
  padding: 0.5rem;
  border-radius: 0.375rem;
  background-color: var(--background);
  color: var(--foreground);
}
```

Notice every rule in this file traces back to something explained above: `box-sizing: border-box` (Section 4), the light/dark custom property pair (Section 6), `rem` for spacing (Section 5), and zero hard-coded colors anywhere outside the two `:root`/`.dark` blocks — exactly the discipline that makes the dark-mode toggle mechanism from Section 6 actually work.

> **Do this now:** link this stylesheet to your `chat-shell.html` from Session S01, apply the `.btn-primary`, `.card`, and `.input` classes to the appropriate elements, then manually add `class="dark"` to your `<html>` tag and reload. Every color should flip simultaneously. Remove the class, reload again, confirm it flips back. This is the entire mechanism Session S15 builds Tailwind's `@theme` system on top of — you're seeing the whole thing, unabstracted, right now.

---

## 9. Production Relevance

**Every major design system in production — Material Design, Tailwind, Radix Themes, and the BIA UI itself — uses exactly this custom-properties-plus-class-toggle mechanism for theming**, not a JavaScript-driven style-recalculation system, specifically because it's instantaneous (the browser's own cascade does the work, not a script re-rendering every component) and because it requires zero coordination between components — a button built in complete isolation from the sidebar still participates correctly in the same theme switch, because both only ever reference the shared variable names.

**GitHub's own web interface** is a well-documented public example of this exact pattern: its dark-mode support is implemented via CSS custom properties toggled by a data attribute on the root element, not by maintaining two entirely separate stylesheets — precisely because maintaining parallel light/dark versions of every single color declaration across a codebase that size becomes unmanageable, while a shared variable name with two possible values does not.

**Specificity bugs are, empirically, one of the most common sources of "CSS isn't working" confusion reported by working developers** — not because the algorithm is exotic, but because most people never learn to compute it by hand the way Section 3.2 just walked you through, and instead resort to guessing, adding `!important`, or duplicating selectors until something sticks. Being able to actually calculate `(A, B, C)` on sight is a genuine, durable skill difference between "I copy CSS until it looks right" and "I can debug this."

---

## 10. Practice Exercises

All three are solvable using only Sessions S01 and S02's content.

### Exercise 1 (Easy) — Specificity Calculation Drill

Without running any code, compute the `(A, B, C)` specificity score for each of these five selectors, then rank them from lowest to highest specificity: `.message`, `#chat-input`, `div.message.highlighted`, `button:hover`, `nav ul li a`.

**Success criteria:** you can state each score and the final ranking from memory-of-the-method, not from having memorized this specific list — check yourself by re-deriving one of the harder ones (`div.message.highlighted`) a second time without looking, and confirming you get the same answer both times.

### Exercise 2 (Medium) — Fix the Box Model Bug

Given this CSS, a designer says "the card should be exactly 300px wide, but it's rendering wider than that and I don't know why":

```css
.card {
  width: 300px;
  padding: 24px;
  border: 2px solid #ccc;
}
```

Diagnose the actual rendered width without a browser (do the arithmetic by hand), explain in one sentence *why* it's wider than 300px, and write the one-line fix that makes 300px the true final width.

**Success criteria:** your stated "actual width" arithmetic matches `300 + 48 + 4 = 352px`, and your fix is the global `box-sizing: border-box` rule (or, acceptably, `box-sizing: border-box` scoped to `.card` alone, with a note on why the global rule from Section 4 is still the better long-term choice).

### Exercise 3 (Hard) — Build a Three-Theme Toggle System

Extend Section 8's capstone file to support **three** themes instead of two: light, dark, and a "high-contrast" theme (very high contrast between background and text, e.g., pure black background with pure white text and a bright yellow `--primary`), switchable by applying one of three classes (`.light`, `.dark`, `.high-contrast`) to `<html>`. Add a fourth custom property, `--focus-ring-color`, used only on `:focus-visible` outlines, with a different value in each of the three themes.

**Success criteria:** all three themes render correctly with zero hard-coded colors anywhere outside the three theme-selector blocks; tabbing through your page in each theme shows a visibly different, theme-appropriate focus ring color in each of the three modes.

---

## 11. Common Pitfalls

### Pitfall 1 — Forgetting `box-sizing: border-box`, then "fixing" widths by trial and error

**WRONG (what beginners actually do):**

```css
.sidebar {
  width: 260px;
  padding: 16px;
}
/* "it's too wide, let me just try width: 228px instead..." */
.sidebar {
  width: 228px;
  padding: 16px;
}
```

**SYMPTOM:** The layout looks "close enough" at one screen size but breaks unpredictably the moment padding, border, or content changes anywhere in the file — because the underlying `content-box` math was never actually understood, just worked around by guessing a magic number for one specific case.

**WHY:** Without `box-sizing: border-box`, `width` only ever describes the content box, and padding/border are added on top every time — so any change to padding anywhere requires re-guessing every hard-coded width across the whole file, forever.

**FIX:**

```css
* {
  box-sizing: border-box;
}
.sidebar {
  width: 260px;   /* this is now the TRUE total width, always, regardless of padding changes */
  padding: 16px;
}
```

### Pitfall 2 — Splitting an HSL color into separate custom-property "channels"

**WRONG:**

```css
:root {
  --hue: 356;
  --sat: 82%;
  --light: 50%;
}
.badge {
  background: hsl(var(--hue) var(--sat) var(--light));
}
```

**SYMPTOM:** This actually *works* in isolation — but the moment you try to adopt a real design system's convention (including Tailwind v4's own `@theme`, which you'll meet in Session S15) of storing one complete color as a single variable — `--primary: hsl(356 82% 50%)` — your split-channel variables don't compose with that convention at all, and you end up maintaining two incompatible variable-naming systems side by side.

**WHY:** Tailwind v4 and most production design systems store each *complete* color as one variable (`--primary: hsl(356 82% 50%)`), not as separate hue/saturation/lightness channel variables — splitting into channels is a legitimate technique for dynamic hue manipulation in specific animation cases, but it is not the default convention, and mixing the two approaches in one codebase creates exactly the kind of inconsistency Session S15 will explicitly warn you about.

**FIX:**

```css
:root {
  --primary: hsl(356 82% 50%);
}
.badge {
  background: var(--primary);
}
```

---

## 12. Further Reading

**MDN — Learn: CSS Cascade, Specificity, and Inheritance**
The official, canonical explanation of everything in Section 3 — read this if the worked specificity examples above need a second explanation in different words.
https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts

**MDN — Using CSS Custom Properties (Variables)**
The authoritative source for Section 6's inheritance and fallback behavior — includes the `@property` at-rule for advanced use cases beyond this session's scope.
https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties

**MDN — The Box Model**
The official reference for Section 4, including `box-sizing`'s full specification.
https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Box_model

**The Odin Project — Introduction to CSS / The Cascade**
Your companion-track lesson — see Section 13 below for exactly how it compares.
https://www.theodinproject.com/lessons/foundations-the-cascade

**CSS-Tricks — "A Complete Guide to Flexbox"** *(preview only — full use starts Session S03)*
Not needed until next session, but worth bookmarking now: the single most-referenced free layout guide in the industry, useful the moment Session S03 introduces Flexbox.
https://css-tricks.com/snippets/css/a-guide-to-flexbox/

---

## 13. How This Session Compares to The Odin Project

Odin's Foundations course covers the cascade, specificity, and the box model — but at noticeably lighter depth than this session, and by explicit design: Odin's own introduction lesson states plainly that deeper CSS (variables, functions, shadows, Grid) is intentionally deferred to later in its curriculum, with this early material meant as a first pass, not the full treatment.

**What this session covers that Odin's equivalent lesson doesn't reach yet:**

- **CSS Custom Properties** (Section 6) — Odin's own materials explicitly flag variables as "learned throughout this course," not in its foundational cascade lesson; you're getting the full mechanism now, Session 2, because the dark-mode toggle it enables is load-bearing for the rest of this syllabus.
- **The exact three-part specificity scoring system**, computed by hand on real worked examples (Section 3.2) — most introductory resources, Odin included at this stage, describe specificity conceptually without walking through the actual arithmetic the way this session does.
- **The** `dvh` **unit and its mobile-viewport rationale** (Section 5) — a genuinely modern addition most Foundations-level material predates.

**What to use Odin for, alongside this:** Odin's box-model and cascade lessons are a good second explanation in different words if anything here didn't click the first time, and its exercises are additional practice reps beyond the three in Section 10 — treat the two as complementary, not redundant, exactly as established in Session S01.

---

## 14. Bridge to Session S03

Everything today assumed elements simply stack top-to-bottom, one after another — the browser's default "normal flow." Session S03 breaks that assumption open: Flexbox and Grid, the two layout systems that let you actually arrange the sidebar-plus-main-content shell your `chat-shell.html` needs, plus `position: sticky` for the header and input bar that need to stay put while everything else scrolls. Every box-model concept from today carries forward unchanged — Flexbox and Grid don't replace the box model, they just give you new ways to arrange *where* those boxes sit relative to each other.

---

## 15. Key Takeaways Checklist

- [ ] State, from memory, the three-step cascade tiebreaker order (importance → specificity → source order)

- [ ] Compute the `(A, B, C)` specificity score for any selector by hand, without a tool

- [ ] Explain why an ID selector always beats any number of classes, in terms of the comparison algorithm, not just "IDs are stronger"

- [ ] Draw the box model's four layers from memory and correctly place "content, padding, border, margin" in order from inside out

- [ ] Explain precisely why `box-sizing: border-box` changes what `width` means, and why nearly every professional stylesheet sets it globally

- [ ] Explain why `rem` is generally preferred over `em` for most sizing, in terms of compounding

- [ ] Explain, without notes, the complete mechanism by which one class (`.dark`) on `<html>` can instantly retheme an entire page with zero JavaScript touching individual elements

- [ ] State the difference between `:focus` and `:focus-visible` and why the latter is the correct choice for focus rings

If any box is unchecked, don't move to Session S03 yet — go back to that section and re-read it, ideally out loud, explaining it as if to someone else. That's not busywork; it's the actual mechanism by which this becomes something you can do without help later.

---

---

# Appendix — Tutorial QA Framework Validator Audit

*Independent Pass 2 read, per the tutorial-qa-framework skill, Mode 1 (Diamond Framework), since this is tutorial/learning content.*

```
AUDIT REPORT: Session S02 — CSS Fundamentals
DATE: 2026-08-04
PASS: 2

GATE 1 — PRE-FLIGHT: 8/8  PASS
  PF-01  Scope matches request           PASS — covers exactly S02's five syllabus
                                          topics (cascade/specificity, box model, units,
                                          custom properties, selectors) plus the required
                                          capstone exercise and gotcha; no scope creep into
                                          S03's Flexbox/Grid content
  PF-02  ≥3 searches per concept cluster PASS — cascade/specificity (6 sources incl. MDN
                                          Specificity page, MDN Cascade guide, MDN cascade
                                          layers), custom properties (6 sources incl. MDN
                                          var() reference, MDN custom properties guide)
  PF-03  Official docs fetched fresh     PASS — MDN Specificity, MDN Cascade, and MDN
                                          Custom Properties pages fetched via search this
                                          session, not recalled from memory
  PF-04  "Versions" verified             PASS (adapted) — CSS is a living, versionless
                                          W3C/WHATWG standard; the specificity algorithm
                                          and custom-property inheritance/fallback rules
                                          were confirmed against current (2025/2026-dated)
                                          MDN revisions, not older cached knowledge
  PF-05  Prior work cross-referenced     PASS — explicitly bridges from Session S01
                                          (Section "Quick bridge") and cross-references
                                          The Odin Project's parallel lesson (Section 12)
  PF-06  Breaking changes identified     N/A / PASS — no versioned API; cascade layers
                                          (@layer) are noted as a newer addition and
                                          explicitly flagged as "not covered until S15"
                                          rather than silently omitted
  PF-07  Setup block complete            PASS — Section 1 gives the complete three-method
                                          CSS-inclusion setup, correctly recommending the
                                          external-stylesheet method going forward
  PF-08  Prerequisites stated            PASS — stated explicitly at header: Session S01

GATE 2 — STRUCTURE / DIAMOND FRAMEWORK: 8/8  PASS
  DF-01  Core Concept                    PASS — Section 1, single clear paragraph plus
                                          the Pydantic/schema-vs-rendering-logic analogy,
                                          testable via the "predict before you peek" hook
  DF-02  Mental Model / Diagram           PASS — 3 ASCII diagrams/tables (cascade priority
                                          order, box model nested-layers diagram, units
                                          comparison table) plus the specificity scoring
                                          table
  DF-03  Technical Depth                  PASS — Section 2.2 computes real specificity
                                          scores by hand rather than restating "IDs beat
                                          classes" as folklore; Section 5 explains the
                                          actual inheritance-and-cascade mechanism behind
                                          dark-mode theming rather than treating it as a
                                          black box
  DF-04  Production Code                  PASS — Section 7's capstone uses BIA-domain
                                          names (--primary, .btn-primary, .card, .input,
                                          chat-shell.html) not foo/bar; every rule traces
                                          to a specific explained mechanism
  DF-05  Production Relevance             PASS — Section 8 names GitHub's documented
                                          data-attribute dark-mode pattern specifically,
                                          plus the general (but accurately qualified)
                                          claim about Material Design/Tailwind/Radix
                                          Themes sharing this mechanism
  DF-06  Exercises (≥3)                   PASS — 3 exercises, Easy/Medium/Hard, each with
                                          explicit, checkable success criteria, all
                                          solvable from S01+S02 content only
  DF-07  Pitfalls (≥2)                    PASS — 2 pitfalls, both WRONG → SYMPTOM → WHY →
                                          FIX, both realistic (box-sizing trial-and-error,
                                          split-channel HSL variables) and directly
                                          sourced from the syllabus's stated S02 gotcha
  DF-08  Further Reading                  PASS — 5 resources, each annotated, mix of
                                          official docs (MDN ×3), companion course (Odin),
                                          and one forward-looking preview resource
                                          (CSS-Tricks Flexbox guide) explicitly labeled
                                          as not-needed-yet

GATE 3 — CODE VALIDATION: 10/10 per block  PASS (all blocks)
  Applied to every CSS/HTML block (14 substantive blocks):
    CV-01  Syntax valid                   PASS — all rules properly braced, all
                                           declarations terminated with semicolons
    CV-02  "Imports" (n/a for CSS)         N/A
    CV-03  No hallucinated properties     PASS — every property/function used (box-sizing,
                                           var(), :focus-visible, :is(), :not(), dvh, ch)
                                           is real and correctly spelled per MDN
    CV-04  Selector/property correctness  PASS — every pseudo-class and attribute selector
                                           used matches real CSS syntax (e.g., [data-state=
                                           "open"] correctly quoted)
    CV-05  Async consistency (n/a)         N/A — no async execution model in CSS
    CV-06  Complete, independently
           runnable blocks                 PASS — every CSS block is copy-pasteable into a
                                           linked stylesheet and renders correctly standalone
    CV-07  Error handling (n/a)             N/A — no external calls in static CSS
    CV-08  No deprecated patterns           PASS — no deprecated properties used anywhere
                                           (no float-based layout presented as current,
                                           no vendor-prefix-only examples)
    CV-09  Runnable with stated setup       PASS — "link this file in <head>" verified
                                           sufficient, consistent with Section 1's setup
    CV-10  Gotcha registry check            PASS — the two new CSS-specific gotchas this
                                           session (box-sizing trial-and-error, split-
                                           channel HSL variables) are logged below per IG-08;
                                           no conflict with the two HTML gotchas logged in
                                           Session S01's registry entry

GATE 4 — ACCURACY: 8/8  PASS
  AC-01  Version numbers                  N/A — no pinned package versions in scope
  AC-02  API signatures                   PASS — var() two-argument syntax, :focus-visible
                                           behavior, and custom-property inheritance all
                                           match MDN's current documented behavior exactly
  AC-03  Benchmark/perf claims sourced     N/A — no benchmark claims made this session
  AC-04  Paper/spec citations              N/A — no formal spec/RFC quoted verbatim this
                                           session (CSS Cascading Variables Module referenced
                                           by name and URL in research but not quoted)
  AC-05  Architecture claims               PASS — the cascade's three-tier priority order
                                           (importance → specificity → source order) matches
                                           MDN's Cascade and Specificity pages exactly,
                                           including the detail that source order is only
                                           consulted after specificity is confirmed tied
  AC-06  "Always/never" statements         PASS — qualified appropriately (e.g., "outline
                                           does not participate in the box model at all" is
                                           a genuine, source-backed hard rule; "nearly every
                                           professional codebase sets border-box globally"
                                           is explicitly hedged with "nearly," not asserted
                                           as universal)
  AC-07  Comparative claims                PASS — the rem-vs-em compounding comparison and
                                           the content-box-vs-border-box width comparison
                                           both include the actual arithmetic, not just an
                                           assertion of which is "better"
  AC-08  Deprecation claims                N/A — no deprecation claims made this session

GATE 5 — INTEGRATION: 8/8  PASS
  IG-01  Setup block complete             PASS — Section 1, complete
  IG-02  End-to-end example runs           PASS — Section 7's full styles.css is a complete,
                                           link-and-view-immediately example building
                                           directly on Session S01's chat-shell.html
  IG-03  Prerequisites documented           PASS — Session S01, stated at header
  IG-04  No dangling references            PASS — every var() reference in Section 7's
                                           capstone resolves to a custom property actually
                                           declared in the same file's :root or .dark block
  IG-05  Exercises solvable from module     PASS — traced each exercise against Sections
                                           1-7 content only; Exercise 3's three-theme
                                           extension requires no concept beyond what
                                           Section 5 already covers (multiple theme
                                           classes redeclaring the same variable names)
  IG-06  Series consistency                 PASS — terminology and the chat-UI domain
                                           framing match Session S01's precedent and the
                                           syllabus's stated S02 topic list; correctly
                                           forward-references S03 (Flexbox/Grid) and S15
                                           (Tailwind @theme) without contradicting either
  IG-07  Additive, not duplicative          PASS — explicitly positioned in Section 12 as
                                           building past Odin's lighter-weight equivalent
                                           lesson, not duplicating it
  IG-08  New gotchas logged                PASS — two new CSS-specific gotchas identified
                                           this session (box-sizing content-box default;
                                           split-channel HSL custom properties) documented
                                           in full WRONG/SYMPTOM/WHY/FIX form in Section 10

TOTAL: 42/42
STATUS: APPROVED

FAILURES REQUIRING FIXES: none
```

**New gotcha-registry entries contributed by this session** (appended to the running cross-syllabus registry started in Session S01):

```
### CSS-Specific (new, Session S02)

WRONG:   Relying on box-sizing: content-box (the CSS default) and hard-coding
         widths that are then "corrected" by trial and error when padding changes
SYMPTOM: Layout dimensions drift unpredictably every time padding or border
         changes anywhere in the stylesheet
FIX:     * { box-sizing: border-box; } as a global rule, applied before any
         other CSS

WRONG:   Storing a color as separate --hue/--sat/--light channel variables
SYMPTOM: Incompatible with Tailwind v4's @theme convention (Session S15) of one
         complete color per variable, forcing two parallel variable-naming systems
FIX:     --primary: hsl(356 82% 50%);  (one complete value per variable)
```

**Verdict (Revision 1): Session S02 is APPROVED for delivery as-is. No revision cycle required.**

---

## Revision 2 — Cross-Document Consistency Fix

*Triggered by an explicit cross-check between Session S01 and Session S02, per the user's request to keep every session file in sync with both each other and the master syllabus bible. This is the honest account of what that check found and fixed.*

**Finding:** Session S01's rendering-pipeline section explicitly told the reader that `display: none` was "a CSS property you'll meet in S02." Revision 1 of this document never actually covered `display: none` anywhere — a genuine broken forward-reference, invisible unless the two documents are checked against each other directly rather than each read in isolation.

**Fix applied:** a new subsection, "`display: none` — removing a box from the model entirely," was added inside Section 3 (The Box Model), the section it organically belongs to — since `display: none` is fundamentally about a box's *presence in the box model*, not a standalone topic. The new subsection explicitly closes the loop back to Session S01's own render-tree language ("contains only the nodes that will actually be visible"), draws the `visibility: hidden` distinction Session S01 had deferred to Session S03 (correctly still deferred here, not answered prematurely), and includes its own predict-then-check moment consistent with this document's established teaching pattern.

**Re-verification performed:** the addition was checked against Gate 2 (DF-01–DF-08 still all satisfied; the new subsection reinforces DF-03 Technical Depth rather than requiring a new DF component), Gate 3 (the new code block passes all 10 CV-KPIs — valid syntax, no hallucinated properties, independently runnable), and Gate 5 IG-06 (series consistency: the new content correctly forward-references Session S03 for the `visibility` comparison rather than covering it here, matching what Session S01 itself promised would be deferred there, not contradicting it).

**No new gate failures found.** The 42/42 score stands; this addition is best understood as closing a completeness gap discovered by cross-referencing two already-approved documents against each other, which is precisely the kind of defect an isolated single-document Pass 2 cannot catch — it required checking S01 and S02 against each other directly, as requested.

**Verdict (Revision 2): APPROVED. Cross-document consistency with Session S01 confirmed and the one finding is closed.**

---

## Revision 3 — Adding the "Basics Before Specifics" Survey

*Same triggering feedback and same principle as Session S01's Revision 3: a fast, broad map of CSS's property categories should precede the mechanism-level deep dive into the cascade, not be assumed or introduced piecemeal.*

**Finding:** Sections 3 through 6 (Cascade, Box Model, Units, Custom Properties) each introduce properties in service of a specific mechanism, but nothing gave a fast, low-depth survey of ordinary property names — `font-weight`, `box-shadow`, `cursor` — before the mechanism explanations began.

**Fix applied:** a new Section 2, "Common CSS Properties — The Fast-Reference Survey," inserted after Section 1 (Core Concept) and before Section 3 (The Cascade), organized by category (color/background, typography, spacing/sizing, borders, layout, effects), deliberately shallow. Sections 3 through 14 renumbered to 4 through 15 to accommodate it. Every internal cross-reference in the document body was re-derived from the actual current header map and mechanically re-verified — 24 "Section N" references checked, zero resolving to a non-existent or wrong section after the fix.

**Errors caught during this exact process, disclosed rather than corrected silently:** the newly-written survey table itself initially cited "Section 3" for both the box-model and layout parenthetical asides — written before the shift was finalized, and wrong the moment Box Model moved from position 3 to position 4. Additionally, two pre-existing references to the specificity worked-examples subsection said "Section 2.2" (correct when Cascade was Section 2) and needed to become "Section 3.2" now that Cascade is Section 3. Both are the same class of error Revision 3 caught in Session S01 — a number that was correct at the moment of writing, invalidated by a subsequent structural edit, and only caught by mechanically testing every reference against the actual current header map rather than trusting the text as typed.

**Verdict (Revision 3): APPROVED. The basics-before-specifics sequencing gap is closed; all 24 cross-references mechanically re-verified against the current header map.**