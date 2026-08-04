# Session S01 — HTML5 Essentials: Semantic Structure, Forms, ARIA & the Browser Rendering Pipeline

**Phase F1 · Day S01 of 37 · AgentStack Golden Canon v8 Slim**
**Roadmap position:** Day 1 of 30 core web days (plus 7 Electron days at the end). This is the very first session.

---

## Before You Start

**Who this session assumes you are:** You know Python well. You know FastAPI. You have written functions, classes, dictionaries, and probably some SQL. You have never written a line of HTML, CSS, or JavaScript, and you have never opened a browser's developer tools on purpose.

**What you need installed:** A text editor (VS Code is the standard — if you don't have it, install it before continuing) and any modern browser (Chrome, Firefox, or Edge — this session uses Chrome's DevTools terminology, but the concepts are identical everywhere).

**Prerequisites from earlier sessions:** None. This is session 1.

**What "done" looks like by the end of this session:** You can open a blank text file, write a complete, valid HTML5 document from memory, explain what every line does and why it's there, open your browser's DevTools and inspect any element on any webpage, and you understand *why* a chat application's HTML has to be structured a specific way for screen reader users and keyboard users to be able to use it at all — not just for sighted mouse users.

**Why this session exists at all:** Every single thing you build for the rest of this 37-day roadmap — the React components in Phase F4, the streaming chat UI in Phase F5, even the Electron desktop windows in the final 7 days — is HTML underneath. React doesn't invent a new way to describe a button; it generates the exact same `<button>` tag you're about to learn to write by hand. If you get the mental model right today, every later session gets easier. If you skip today and go straight to "just show me React," you will spend the next 36 days confused about why your components don't behave the way you expect.

---

## Topic 1 — Document Skeleton + Semantic Elements

### What HTML actually is (the concept you're missing)

Here's the one-sentence version, and then we'll unpack every word of it: **HTML is a text format for describing the *structure* of a document — what each piece of content *is* — not what it looks like.**

Compare this to something you already know. In FastAPI, when you write:

```python
class ChatMessage(BaseModel):
    role: str
    content: str
```

You are not describing how this data should be *displayed*. You're describing what it *is*: a chat message has a role and content. HTML works the same way. When you write `<h1>Welcome</h1>`, you are not saying "make this text big and bold." You are saying "this text is the main heading of the document." What it *looks like* (big, bold, a certain font) is a completely separate job, done by CSS, which you'll learn in Session S02. Today, forget about appearance entirely. Today is 100% about *meaning*.

This separation — structure in HTML, appearance in CSS, behavior in JavaScript — is the single most important mental model in all of frontend development. Every session for the next month builds on top of this split. Keep it in your head: **HTML = what it is. CSS = what it looks like. JavaScript = what it does.**

### The anatomy of a tag

An HTML "tag" is the basic unit of HTML. It looks like this:

```
<tagname attribute="value">content goes here</tagname>
```

Break that down:
- `<tagname>` is the **opening tag**. The angle brackets `<` and `>` are not part of the name — they're punctuation that says "a tag starts here."
- `</tagname>` is the **closing tag** — identical to the opening tag but with a `/` before the name. It says "the thing that started at the opening tag ends here."
- Everything between the opening and closing tag is the **content** — and that content can itself contain more tags, nested inside. This is exactly like nesting Python dictionaries or nesting function calls: a tag can contain other tags, which can contain other tags, as deep as you need.
- `attribute="value"` is an **attribute** — extra information *about* the tag, written inside the opening tag. If you've ever called a Python function with keyword arguments, like `requests.get(url, timeout=5)`, an HTML attribute is doing the exact same job: it's a named parameter that modifies how this specific tag behaves.

Some tags don't have separate content and don't need a closing tag at all — they're called **void elements** because they represent a single, self-contained thing with nothing "inside" them. `<input>` is one; you'll meet it in Topic 2. These are written as `<input type="text">` with no `</input>` anywhere — there's nothing to close because there's no content slot.

### The complete document skeleton, line by line

Every HTML document you will ever write in your entire career starts with the same skeleton. Here it is in full, and then we'll go through every single line:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Chat</title>
  </head>
  <body>
    <!-- Your visible page content goes here -->
  </body>
</html>
```

- **`<!DOCTYPE html>`** — This is not actually an HTML tag. It's a one-line instruction to the browser that says "parse everything that follows using the modern HTML5 rules, not some ancient 1998 rule set." Think of it like a Python file's `#!/usr/bin/env python3` shebang line — it's a declaration about *how to interpret what follows*, not content itself. Every HTML file needs exactly one, as the very first line, with nothing before it.

- **`<html lang="en">`** — This is the root tag. Every other tag in the document lives inside this one — it's the outermost container, exactly like how every key in a Python dict lives inside that one dict. The `lang="en"` attribute tells browsers and screen readers "this document's text is in English" — screen readers use this to pick the correct pronunciation engine, and browsers use it for things like spell-check and translation prompts. If you're writing a page in French, this would be `lang="fr"`.

- **`<head>`** — This section holds *metadata*: information *about* the page that isn't shown as visible content on the page itself. Nothing inside `<head>` appears in the visible chat window — it configures the document.

- **`<meta charset="UTF-8" />`** — Declares the character encoding. UTF-8 supports every character in every language plus emoji — this is the encoding you want in 100% of cases in 2026. Without this line, some browsers will guess the encoding wrong and your page will show garbled text ("mojibake") the moment a user's message contains an emoji or a non-English character — extremely relevant for a chat app.

- **`<meta name="viewport" content="width=device-width, initial-scale=1.0" />`** — This one line is what makes your page work on a phone at all. Without it, mobile browsers assume your page was designed for a desktop-width screen (usually 980px) and shrink the entire page to fit, making everything tiny and requiring the user to pinch-zoom. `width=device-width` says "use the actual width of this device's screen." `initial-scale=1.0` says "start at 100% zoom, don't shrink." You will put this exact line in every single HTML page you ever write.

- **`<title>Chat</title>`** — The text shown in the browser tab and in bookmarks. Not visible inside the page itself.

- **`<body>`** — Everything the user actually *sees* goes here. Every visible element of your chat application — the sidebar, the messages, the input box — lives inside `<body>`.

### Landmark elements — giving structure to the body

Inside `<body>`, HTML5 gives you a set of **semantic landmark elements** — tags whose *name* tells you and the browser what role that section of the page plays, instead of using a generic, meaningless container everywhere. Here they are:

| Tag | What it means | Chat UI example |
|---|---|---|
| `<header>` | Introductory content, usually at the top | The bar with the chat title and sidebar toggle |
| `<nav>` | A block of navigation links | The sidebar's list of past conversations |
| `<main>` | The primary content of the page — **only one per page** | The message list and input area |
| `<aside>` | Content tangentially related to the main content | A "related topics" panel, if you had one |
| `<footer>` | Closing content, usually at the bottom | A copyright line or footer links |
| `<section>` | A generic, thematic grouping of content with its own heading | A distinct grouping within a longer page |
| `<article>` | Self-contained content that would make sense on its own, syndicated elsewhere | A single blog post; less common in a chat app |

Compare this to the "before" state, which is how HTML used to be written and how a lot of bad code out there still is: `<div class="header">`, `<div class="nav">`, `<div class="main">`. A `<div>` (short for "division") is a completely generic, meaningless container — it tells you nothing except "some content is grouped here." The problem: a screen reader reading a page built entirely from `<div>`s has *no way to know* which div is the navigation and which is the main content. It just hears "group, group, group" with no map of the page. A screen reader reading a page built with `<header>`, `<nav>`, and `<main>` can literally say to the user: "3 landmarks on this page: banner, navigation, main" — and let the user jump straight to the one they want, the same way you `Ctrl+F` to jump to a spot in a long document.

### Heading hierarchy

`<h1>` through `<h6>` are heading tags, in decreasing importance. The rule that trips up almost every beginner: **there should be exactly one `<h1>` per page** — it's the single most important heading, like the title of a book. `<h2>` are the next level down (chapter titles), `<h3>` below that (section titles within a chapter), and so on. You should never skip a level going *down* (don't jump from `<h1>` straight to `<h3>` — go through `<h2>` first), though you can jump back *up* any number of levels. Screen reader users very commonly navigate a page by jumping between headings only — this is the equivalent of skimming a table of contents — so a broken hierarchy (or a page using `<div class="heading">` styled to *look* like a heading but not actually being one) makes that navigation impossible.

### `button` vs `div` for clicks — the concept, not just the gotcha

You'll see this again as this session's main Pitfall, but the *concept* belongs here in Topic 1: HTML gives you a real `<button>` element specifically for "the user clicks this to trigger an action." A `<div>` is a generic container with *no* built-in behavior at all — no keyboard focus, no "activated" state, nothing. If you want something clickable, `<button>` is not a suggestion, it's the correct semantic tag — we'll go deeper on exactly why in the Pitfalls section.

### `fieldset` + `legend` for groups

When a set of form fields belong together conceptually — say, three radio buttons all answering "which AI model do you want to use?" — wrap them in a `<fieldset>` with a `<legend>` as its first child. The `<legend>` becomes the group's accessible name, announced once by a screen reader before it reads each individual option, instead of the user having to guess why these three radio buttons are grouped.

```html
<fieldset>
  <legend>Choose a model</legend>
  <label><input type="radio" name="model" value="fast" /> Fast</label>
  <label><input type="radio" name="model" value="accurate" /> Accurate</label>
</fieldset>
```

---

## Topic 2 — Forms + Input Types + Validation

### Why forms matter even in a "just a chat box" app

You might think: "My chat app just has one text box and a send button — do I really need to know about `<form>`?" Yes — and understanding *why* will save you from a very confusing bug three sessions from now (Session S12, when you build this in React) where pressing Enter unexpectedly reloads the whole page. The `<form>` element is HTML's built-in mechanism for "the user is submitting a batch of input to somewhere." Even a single text box and a send button is a form, semantically — one field, one submission action.

### The core building blocks

```html
<form>
  <label for="chat-input">Your message</label>
  <input type="text" id="chat-input" name="message" required />
  <button type="submit">Send</button>
</form>
```

- **`<label for="chat-input">`** — This is the single most important accessibility pattern in all of HTML forms, and it is also just good practice for every user, not only screen reader users. The `for` attribute must exactly match the `id` attribute of the input it labels. This does two things: (1) it gives the input an accessible name that a screen reader announces ("Your message, edit text" instead of just "edit text"), and (2) clicking anywhere on the label text moves keyboard focus to the input — try clicking directly on a form label on any real website and watch the cursor jump into the box next to it. That's this pattern, working.

- **`<input>`** — The workhorse tag for a single line of user-entered data. Note it has no closing tag (it's a void element, from Topic 1) and its behavior is entirely controlled by its `type` attribute.

- **`name="message"`** — This is the key that this field's value will be submitted under. If you don't know backend form handling terminology yet: think of `name` as the dictionary key, and whatever the user types as the dictionary value. When this form is submitted, the browser bundles it conceptually as `{"message": "whatever the user typed"}`.

### The input types that matter

The `type` attribute on `<input>` is doing a lot of work — it changes the on-screen keyboard on mobile, changes what the browser validates automatically, and sometimes changes the rendered widget entirely.

| `type` value | What it does |
|---|---|
| `text` | Plain single-line text, no special validation |
| `email` | Shows an `@`-friendly keyboard on mobile; browser validates it looks like an email address before allowing submit |
| `password` | Masks the characters as dots on screen |
| `number` | Shows a numeric keypad on mobile; browser only accepts numbers |
| `search` | Semantically marks this as a search box; some browsers show a small "×" clear button |
| `checkbox` | An on/off toggle; multiple checkboxes with the same `name` let a user select several |
| `radio` | One selection out of a group — all radios sharing the same `name` become mutually exclusive |
| `file` | Opens the OS file picker |

### Native validation — the browser will do work for you, for free

```html
<input type="email" required minlength="5" />
```

- **`required`** — the browser refuses to submit the form (and shows a small tooltip pointing at the field) if it's empty. No JavaScript needed.
- **`min` / `max`** — for numeric or date inputs, sets the allowed range.
- **`pattern`** — a regular expression the value must match (you already know regex from Python — the syntax is nearly identical).

This is called **native form validation**, and the reason it matters: it works *before any JavaScript has even loaded*. If your page's JavaScript bundle is still downloading (very common on a slow connection), a user trying to submit an empty required field still gets instant feedback, because the browser itself — not your code — is enforcing the rule.

### `FormData` — a preview of what's coming

`FormData` is a browser-native JavaScript object that reads every named field out of a `<form>` and packages it up, ready to send to a server — conceptually similar to how you'd build a `dict` from `request.form` in a Flask/FastAPI-style backend, except it's happening in the browser, on the way *out*. You won't write any JavaScript today — this is just so the name doesn't come as a surprise when you meet it for real in Session S08 (Fetch API + streaming), where the chat input's `<form>` gets read and sent to your backend.

---

## Topic 3 — ARIA Fundamentals

### The problem ARIA solves (verified against the current W3C spec)

Everything in Topics 1 and 2 — `<header>`, `<nav>`, `<button>`, `<label for="">` — gives you accessibility *for free*, automatically, because the browser and screen readers already understand what these tags mean. ARIA (**Accessible Rich Internet Applications**) exists for the cases native HTML *can't* cover on its own: highly dynamic content that changes after the page has loaded, and custom interactive widgets that don't map cleanly onto any single native tag.

The current stable specification is **WAI-ARIA 1.2**, published as a W3C Recommendation in June 2023; a newer 1.3 version is still in Working Draft status as of 2026 and not yet something you need to track. The single most important rule in the entire spec, stated directly by the W3C's own guidance: **"You should only use WAI-ARIA when you need to!"** — always prefer a real HTML element over adding ARIA to a `<div>`, and only reach for ARIA when there is genuinely no native element that does the job.

### The three things ARIA defines

1. **Roles** — what an element *is*. Example: `role="alert"`.
2. **Properties** — extra information that mostly doesn't change. Example: `aria-label="Send message"`.
3. **States** — information that *does* change, usually via JavaScript, as the user interacts with the page. Example: `aria-disabled="true"`.

### `aria-label` vs `aria-labelledby`

Both give an element an **accessible name** — the text a screen reader announces for it. `aria-label` supplies that text directly, inline:

```html
<button aria-label="Send message">➤</button>
```

Without the `aria-label`, a screen reader would announce this button as just "button" — useless, because the visible content is an arrow icon, not readable text. `aria-labelledby`, in contrast, *points at* the `id` of some other element already on the page and reuses *its* text as the label — useful when the label text is already visible somewhere and you don't want to duplicate it. When both are present on the same element, `aria-labelledby` wins.

### `aria-hidden="true"` — and its one dangerous mistake

`aria-hidden="true"` removes an element, and everything inside it, from the accessibility tree entirely — screen readers skip it as if it doesn't exist. This is the correct tag for purely decorative content: an icon that's next to a text label that already says the same thing, so the icon would just be noise if announced.

```html
<button>
  <span aria-hidden="true">🗑️</span>
  Delete conversation
</button>
```

**The one mistake to never make:** never put `aria-hidden="true"` on an element that can still receive keyboard focus (like a `<button>` or a link). If you do, a keyboard user can still Tab their way onto it — the cursor visibly lands there — but a screen reader announces nothing at all, because you told it the element doesn't exist. The user is stuck on an invisible, silent stop with no idea what they've tabbed onto. `aria-hidden` is only for content that is *also* removed from the tab order (or was never in it to begin with).

### `aria-live` — announcing content that changes on its own

This is the property your chat application's message list depends on completely, and it's worth understanding precisely, not just as a magic incantation. A **live region** is any part of the page marked so that when its content changes — without the user doing anything to trigger it directly, like new tokens streaming in from an AI response — a screen reader announces the change automatically. `aria-live` takes exactly three values:

- **`off`** (the default for ordinary content) — changes are not announced at all.
- **`polite`** — the change is announced, but only once the screen reader finishes whatever it's currently reading. This is what you want for a chat message list: new messages get read out, but never *interrupt* a user who is mid-sentence listening to an earlier message.
- **`assertive`** — the change is announced immediately, interrupting whatever is currently being read. Reserve this for genuinely urgent alerts only (a failed payment, a security warning) — using it for routine content is jarring and is considered a misuse of the attribute.

```html
<div id="message-list" aria-live="polite">
  <!-- New chat messages get appended here as they arrive -->
</div>
```

There are two companion properties worth knowing exist, even if you won't use them today: `aria-atomic="true"` tells the screen reader to re-read the *entire* live region on any change, not just the new bit that was added — useful when surrounding context matters. `aria-relevant` controls *which kinds* of changes get announced (additions, removals, or both).

### `role="alert"` and `role="status"` — the shortcut versions

Two roles exist specifically because "make a live region" is such a common need that ARIA gives you a one-word shortcut: `role="alert"` is equivalent to `aria-live="assertive"` (plus it also semantically marks the element as an alert message), and `role="status"` is equivalent to `aria-live="polite"`. This is directly confirmed by the current W3C `Using ARIA` guidance. You'll see `role="alert"` used for form validation error messages that appear dynamically after a failed submission.

### `aria-required="true"` — reinforcing what `required` already does

You already met the HTML `required` attribute in Topic 2. Some older or less common screen reader/browser combinations don't announce the native `required` attribute reliably, so `aria-required="true"` exists as a belt-and-suspenders reinforcement:

```html
<input type="text" required aria-required="true" />
```

### The `role="button"` escape hatch — and its unavoidable cost

Earlier in Topic 1, you learned to always prefer a real `<button>`. But suppose you've inherited a codebase where someone already built a clickable element out of a `<div>`, and you need to make it accessible without a full rewrite today. ARIA gives you an escape hatch:

```html
<div tabindex="0" role="button">Click me!</div>
```

`role="button"` tells a screen reader to announce this element as a button. `tabindex="0"` inserts it into the normal keyboard Tab order at its natural position in the page (a value of `-1` would make it focusable only via JavaScript, not via Tab). But — and this is the critical, easy-to-miss catch, confirmed directly in the current MDN guidance — **adding the role and the tabindex does not give you any of the actual keyboard *behavior* a real button has**. A real `<button>` fires its click handler when the user presses Enter *or* Space, automatically, with zero extra code. This fake `<div>` button does neither — you would have to write your own JavaScript to listen for Enter and Space key presses and trigger the same action a click would. This is exactly why the Pitfall for this session — and the reason it's rule #1 for the rest of this roadmap — is: **use `role="button"` on a div only as a last resort, and know that you are taking on real extra work by doing so.**

---

## Topic 4 — The Browser Rendering Pipeline

### The concept: what happens between typing a URL and seeing a page

You've been writing HTML for three topics now without ever asking: how does this text file become pixels on a screen? Understanding this pipeline is what will let you debug a "why isn't my page showing what I wrote" problem quickly instead of guessing randomly — and it directly explains a rule you'll follow in every session from here forward (where `<script>` tags go).

### The mental model — the full pipeline as a diagram

```
 You type a URL
       │
       ▼
┌─────────────┐
│     DNS     │  "What server IP does this domain name point to?"
└──────┬──────┘
       │
       ▼
┌─────────────┐
│     TCP     │  Opens a reliable connection to that server
└──────┬──────┘
       │
       ▼
┌─────────────┐
│    HTTP     │  "GET me the HTML document at this path"
└──────┬──────┘
       │
       ▼  the raw HTML text arrives
┌─────────────┐
│ HTML PARSE  │  Browser reads the text top to bottom, tag by tag
└──────┬──────┘
       │
       ▼
┌─────────────────────────┐
│   DOM   +   CSSOM        │  DOM = tree of every HTML tag as an object.
│                          │  CSSOM = tree of every CSS rule that applies.
└──────────┬───────────────┘
           │
           ▼
     ┌─────────────┐
     │ RENDER TREE │  DOM + CSSOM merged: only the VISIBLE nodes, with styles attached
     └──────┬──────┘
            │
            ▼
      ┌───────────┐
      │  LAYOUT   │  Calculates the exact X/Y position and size of every visible node
      └─────┬─────┘
            │
            ▼
      ┌───────────┐
      │   PAINT   │  Actually draws pixels to the screen
      └───────────┘
```

### Why this pipeline matters to you as a beginner, right now

Two concrete, practical rules fall directly out of this diagram, and they explain behavior you will otherwise find mysterious:

**Rule 1 — CSS blocks rendering.** The browser cannot build the "Render Tree" step until it has the *complete* CSSOM — because it needs to know every rule that might apply before it can compute final styles. This is why CSS `<link>` tags belong in `<head>`, loaded before `<body>` starts parsing: if CSS arrived late, the user would see a flash of completely unstyled, ugly HTML before styles suddenly snapped into place. You want the CSSOM ready *before* the page starts becoming visible.

**Rule 2 — JavaScript blocks HTML parsing, by default.** When the HTML parser reaches a `<script>` tag with no special attributes, it *stops parsing the rest of the page*, downloads the script (if it's an external file), runs it completely, and only then resumes reading the HTML that comes after it. This is because JavaScript is allowed to modify the page — it might delete or rewrite content the parser hasn't reached yet — so the browser has to pause and let it finish first, to be safe. Two attributes exist specifically to fix this: `defer` (download the script in the background, but wait to *run* it until HTML parsing is completely finished) and `async` (download in the background and run it the instant it's ready, potentially interrupting parsing). For almost every real script tag you'll write for the rest of this roadmap, `defer` is the one you want.

```html
<script src="app.js" defer></script>
```

### DOM and CSSOM — connecting back to what you already know

If it helps: think of the **DOM** as the browser's live, in-memory, queryable version of your HTML file — structurally, it's a tree of nested objects, the same shape as a deeply nested Python dict or a parsed JSON document, where every HTML tag becomes one node with a type, attributes, and children. The **CSSOM** is the equivalent tree, but built from every CSS rule instead of every HTML tag. Both are built independently, in parallel where possible, and only combined at the "Render Tree" step.

---

## Topic 5 — Accessibility for Chat UIs (bringing Topics 1–4 together)

This topic doesn't introduce new tags — it's where the previous four topics get applied specifically to the thing you're actually building over the next 36 days: a production LLM chat interface. This is the organic connective tissue the syllabus asks for.

**Every interactive element must be a real `<button>`, never a `<div>` with a click handler.** This is Topic 1's rule, applied concretely: the Send button, the Stop-generation button, the delete-conversation button, and every dropdown-menu trigger in your eventual chat sidebar all must be genuine `<button>` tags. You'll see exactly why in this session's Pitfalls section below.

**Tab order follows DOM order, unless `tabindex` overrides it.** When a user presses Tab repeatedly, focus moves through interactive elements *in the exact order they appear in your HTML* — top to bottom, as written. This means the *order you write your tags in* is not a cosmetic choice — it directly determines the keyboard navigation experience. If your sidebar's HTML appears before your message list in the document, a keyboard user tabs through the entire sidebar before ever reaching the chat input, even if the input is visually first on screen (visual position, controlled by CSS, and DOM/tab order are two separate things — you'll see this distinction sharpen further once you learn CSS positioning in Session S03).

**Focus rings must never be removed without a replacement.** A "focus ring" is the visible outline the browser draws around whatever element currently has keyboard focus — it's how a keyboard-only user (or anyone who's ever accidentally clicked and lost their mouse, or anyone using switch-access hardware) can *see* where they currently are on the page. `outline: none` in CSS (coming in Session S02) removes this ring. Removing it without providing a different visible replacement makes your entire application unusable for keyboard navigation — the user has no way to know which element is about to activate when they press Enter. Keep this rule in your head now, three sessions before you'll be tempted to write that exact line of CSS because a plain focus ring "doesn't match the design."

**The message list needs `aria-live="polite"`.** This is Topic 3's live region concept, applied directly: as new assistant tokens stream in (which you'll build for real starting in Session S08), a screen reader user needs to be told new content has arrived, without every single token interrupting their reading of the previous message. `polite`, not `assertive` — a wall of interruptions on every token would make the experience worse, not better.

---

## Topic 6 — DevTools Fundamentals + `data-*` Attributes

### Opening DevTools (verified current shortcuts)

Every modern browser ships a full set of developer tools, free, built in. To open them:

- **Windows / Linux:** `F12`, or `Ctrl+Shift+I`
- **macOS:** `Cmd+Option+I`

Two more targeted shortcuts worth knowing immediately: `Ctrl+Shift+C` (`Cmd+Shift+C` on Mac) opens DevTools *and* immediately switches your cursor into "inspect element" mode — click anything on the page and DevTools jumps straight to that element in the Elements panel. `Ctrl+Shift+J` (`Cmd+Option+J` on Mac) opens DevTools with the Console tab already focused.

### The panels you'll use constantly

- **Elements panel** — shows the live DOM tree (from Topic 4) as a browsable, editable tree. You can click any node, see its current styles, and even edit HTML or CSS directly in the browser to experiment — changes here are temporary and gone on refresh, which makes it a safe playground.
- **Console panel** — lets you run JavaScript directly, and shows error messages your page produces. Two shortcuts specific to this panel matter a lot: after clicking an element in the Elements panel, typing `$0` in the Console refers to that exact element — you can then run things like `$0.textContent` to read its text. `$$('selector')` is shorthand for `document.querySelectorAll('selector')` — it returns every matching element on the page as a list, instantly, without writing a full line of JavaScript.
- **Network panel** — shows every request the page makes to any server, including headers and response bodies. You will live in this panel from Session S08 onward, when you start debugging streaming responses and authentication headers.
- **Performance panel** — records exactly what the browser was doing, frame by frame (scripting, rendering, painting) over a span of time. Relevant much later, in Session S27, for diagnosing why a chat UI re-renders too often.
- **Sources panel** — lets you set breakpoints in your JavaScript and step through it line by line, similar to using `pdb` or a debugger in Python, but for the browser.

### `data-*` attributes — custom hooks for JavaScript and CSS

HTML lets you invent your own attributes, as long as they start with `data-`. These do absolutely nothing on their own — no built-in browser behavior, no accessibility meaning — they exist purely as a place to attach information that *your own* JavaScript or CSS can read later.

```html
<div data-role="user">Hey, can you help me debug this?</div>
<div data-role="assistant">Sure, paste the error message.</div>
```

Later, JavaScript can ask "give me every element where `data-role` equals `assistant`," or CSS can target `[data-role="assistant"]` to style user and assistant messages differently. This pattern is everywhere in real production component libraries — Radix UI (which you'll use directly in Session S17, months from now, when building your chat sidebar with shadcn/ui) represents every one of its component states this way: `data-state="open"`, `data-state="closed"`, `data-disabled`, `data-orientation`. The reason a library reaches for `data-*` instead of, say, a CSS class that changes: a `data-*` attribute can hold an actual value (`open` vs `closed`), not just be present-or-absent like a class toggle, and it keeps "is this open" cleanly separate from "what does it look like."

---

## Exercises

**Solvability check:** every exercise below can be completed using *only* what's in this session — no CSS, no JavaScript. If you find yourself wanting to add a `<style>` tag or an `onclick` handler, stop — that's next session's and the session after's job. Zero CSS, as the syllabus's own exercise instruction says.

### Exercise 1 (Easy) — A single well-formed page

Write a complete, valid HTML5 document (full skeleton: `<!DOCTYPE html>` through `</html>`) with the title "My First Chat Page." Inside `<body>`, include exactly one `<h1>` reading "Welcome to the Chat," and one `<p>` paragraph below it with any sentence of your choosing.

**Success criteria:** Open the file directly in your browser (double-click it, or drag it into a browser tab). The tab shows "My First Chat Page." The page shows your heading and paragraph. Open DevTools' Elements panel and confirm you can see the full tree: `html > head` and `html > body > h1, p`.

### Exercise 2 (Medium) — A labeled, validating login-style form

Build a `<form>` with: a `<label>`+`<input type="email">` pair for an email address (`required`), a `<label>`+`<input type="password">` pair for a password (`required`, `minlength="8"`), and a `<button type="submit">` reading "Log In." Every `<label>` must be correctly connected to its `<input>` via matching `for`/`id`.

**Success criteria:** Clicking directly on either label text moves the cursor into its corresponding input (this proves the `for`/`id` link works). Clicking "Log In" with the email field empty shows the browser's own native validation message, with zero JavaScript written. Typing a password under 8 characters and submitting shows a native validation message about the minimum length.

### Exercise 3 (Hard) — The chat UI semantic shell

This is the session's primary exercise, taken directly from the syllabus. Build the full semantic HTML shell for a chat application:

- A `<header>` containing a `<button>` (acting as a sidebar toggle — no functionality needed yet, just the tag) and an `<h1>` with the app's name.
- A `<main>` with `role="main"`.
- Inside `<main>`: a `<nav aria-label="Chat history">` containing a placeholder unordered list (`<ul>`/`<li>`) of 3 fake past conversation titles.
- Also inside `<main>`: a message list container with `aria-live="polite"`, containing 2–3 sample `<div>` messages (you can use `data-role="user"` and `data-role="assistant"` on them, per Topic 6).
- A `<form>` at the bottom containing a `<label>` (visually hidden is fine conceptually, we haven't learned CSS yet, so just include it) + `<textarea>` for the message, with an `aria-label`, and a `<button type="button">` reading "Send" (not `type="submit"` — you'll understand exactly why after reading this session's Pitfall below).

**Success criteria:** Every single interactive element (sidebar toggle button, send button) is reachable, in a sensible order, by repeatedly pressing Tab with your mouse untouched. Nothing on the page is clickable-looking that isn't a real `<button>` or link. Opening DevTools' Elements panel shows a clean, correctly nested tree with no `<div>` standing in for something that has a real semantic tag.

---

## Pitfalls

### Pitfall 1 — Using `<div onclick="...">` instead of `<button>`

**WRONG (what developers actually write, especially when copying a design from Figma):**
```html
<div class="send-button" onclick="sendMessage()">Send</div>
```

**SYMPTOM:** Visually, this looks identical to a real button and works perfectly for mouse users. But: press Tab repeatedly through the page — your cursor skips over it entirely, because a `<div>` has no built-in tab-stop. A screen reader user navigating the page hears nothing that identifies this as an interactive control at all — it's just announced as a generic block of text, "Send," with no indication it does anything. A user who navigates by keyboard alone (a substantial population, including many power users who simply prefer it, not only users with disabilities) cannot use your send button at all.

**WHY:** `<div>` is a purely generic container. It carries zero built-in semantics and zero built-in keyboard behavior. Giving it an `onclick` attribute only wires up the *mouse* click event — it does nothing for keyboard interaction, and it gives screen readers no role to announce.

**FIX:**
```html
<button type="button" class="send-button">Send</button>
```
A real `<button>` is automatically part of the Tab order, automatically fires its click handler on both Enter and Space, and is automatically announced by every screen reader as "Send, button" with zero extra code required.

### Pitfall 2 — The `<button>` inside a `<form>` that silently submits and reloads the page

**WRONG (this is the single most common surprising bug for developers new to forms, and it will bite you again in three months when you rebuild this exact chat input in React):**
```html
<form>
  <textarea name="message"></textarea>
  <button>Send</button>
</form>
```

**SYMPTOM:** The user clicks "Send," and the entire page visibly reloads/flashes, the URL bar might briefly show a `?message=...` query string, and any in-memory chat history (once you have JavaScript managing it, from Session S05 onward) is instantly wiped, because a full page reload restarts everything from scratch.

**WHY:** Inside a `<form>`, any `<button>` with no explicit `type` attribute *defaults to* `type="submit"`. A submit button's entire native job is to trigger the form's submission — which, with no other instructions, means "navigate the browser to this same URL, sending the form's data along as a query string." This is completely correct, expected behavior for a `<button>` in a form — it is not a bug in the browser. It only becomes a bug in *your* app because you didn't intend to trigger a full navigation; you intended to run some JavaScript that sends the message quietly in the background (which you'll build starting in Session S08).

**FIX:**
```html
<form>
  <textarea name="message"></textarea>
  <button type="button">Send</button>
</form>
```
Explicitly writing `type="button"` tells the browser "this button does not submit anything on its own — its behavior is entirely up to whatever JavaScript I attach to it later." Every send button, stop button, and action button you build for the rest of this roadmap that lives inside a `<form>` needs this explicit `type="button"` — it is one of the most frequently hit bugs in React single-page applications built by developers who learned React before they learned plain HTML forms, which is exactly the trap this session is designed to prevent you from ever falling into.

---

## Where This Is Used in Production

The BIA_CATS_AIA_TEMPLATE_UI — the real production chat interface this entire 37-day roadmap is reverse-engineered from — follows every rule in this session precisely. Its sidebar is a real `<nav aria-label="Chat history">`, not a styled `<div>`. Its message list carries `aria-live="polite"`, exactly as described in Topic 3, so that streaming assistant responses (which you'll build the actual streaming mechanics for in Sessions S08, S21, and S22) are announced to screen reader users token-group by token-group, without interrupting them mid-sentence. Every action in its sidebar dropdown menus — share, delete, rename — is built on Radix UI primitives, which internally represent every open/closed/disabled state using the exact `data-*` attribute pattern from Topic 6 (`data-state="open"`, `data-disabled`), not CSS classes. This is not a coincidence or a stylistic preference: it's the same reasoning covered in this session, applied at production scale, because a widely-used interface has to work for every user, including the ones using screen readers or keyboard-only navigation, or it fails an accessibility audit and cannot ship internally at a company like Eli Lilly.

---

## Further Reading

**MDN — WAI-ARIA basics** (official reference)
The single best free explanation of ARIA's three concepts (roles, properties, states), with working before/after code examples for live regions and fake buttons. Read this if Topic 3 above moved faster than you'd like.
https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics

**MDN — Structuring content with HTML** (official curriculum module)
The full, free MDN curriculum module this session's Topics 1 and 2 are drawn from, with additional practice challenges (a feedback form, a data table) beyond what's in this session.
https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content

**Scrimba — Learn HTML and CSS** (free, 5.7 hours, interactive)
An interactive companion where you edit real code in your browser alongside a video instructor, built in partnership with MDN. Use this alongside this session if you want more typing repetitions than the exercises above give you.
https://scrimba.com/learn-html-and-css-c0p

**W3C — Using ARIA** (official specification-adjacent guidance)
The authoritative source for exactly which native HTML element corresponds to which ARIA role, and the precise definition of `role="alert"` / `role="status"` as shortcuts for `aria-live` values, cited directly in Topic 3 above.
https://www.w3.org/TR/using-aria/

**web.dev — Open Chrome DevTools** (official Google documentation)
The canonical reference for every DevTools panel and keyboard shortcut mentioned in Topic 6, kept current by the Chrome team itself.
https://developer.chrome.com/docs/devtools/open

---

## Session Recap and What Connects Next

You now have the vocabulary and mental model for: the DOCTYPE/html/head/body skeleton, every landmark element, forms and native validation, the three ARIA concepts (roles, properties, states) and specifically `aria-live` for streaming content, the browser's rendering pipeline and why script placement matters, and how to open and navigate DevTools. Every one of these becomes load-bearing almost immediately: **Session S02 (CSS Fundamentals)** takes the exact HTML skeleton and landmark elements you just built and starts controlling their *appearance* — you'll finally get to make that chat shell from Exercise 3 look like an actual application instead of plain black text on a white background. The semantic tags you chose today (`<button>`, `<nav>`, `<header>`) are not just an accessibility nicety — CSS in Session S02 and S03 specifically targets and lays out these same tags, so getting the HTML right today directly determines how much friction you'll feel styling it tomorrow.

---
---

# QA AUDIT REPORT — Session S01

**MODULE:** Session S01 — HTML5 Essentials
**DATE:** 2026-08-02
**PASS:** 2 (Validator — independent read)
**AUDITOR NOTE:** Per the tutorial-qa-framework's cardinal rule, this audit was performed as an independent read of the finished draft, treating it as if written by someone else, checking every claim against the Gate 1 research collected before drafting began, not against memory of writing it.

## GATE 1 — PRE-FLIGHT: 8/8 — PASS

- **PF-01 Scope matches request:** PASS — Covers exactly the 6 topics, 1 exercise (expanded to 3 per DF-06), and 1 gotcha (expanded to 2 per DF-07) listed in the Slim 30 bible's S01 card, no more, no less.
- **PF-02 Research complete:** PASS — 3 web searches performed before drafting: (1) WAI-ARIA current spec status, (2) MDN WAI-ARIA basics full fetch, (3) Chrome DevTools shortcuts verification.
- **PF-03 Official docs fetched fresh:** PASS — MDN's WAI-ARIA basics page was fetched in full this session (not recalled from memory); W3C spec status confirmed via live search results dated 2026.
- **PF-04 Versions verified:** N/A-PASS — This topic (HTML5, ARIA 1.2) is a stable web standard, not a versioned package; the one version-like claim (WAI-ARIA 1.2 stable, 1.3 draft) was verified live against W3C's own overview page.
- **PF-05 Prior work cross-referenced:** PASS — This is Session 1; no prior session content exists to overlap with. Explicitly notes what is deferred to S02 (CSS) and S08 (Fetch/FormData) to avoid scope creep into those sessions.
- **PF-06 Breaking changes identified:** PASS — None applicable; HTML5/ARIA landmark elements and `aria-live` values have been stable for many years with no relevant breaking changes to flag.
- **PF-07 Setup block complete:** PASS — "Before You Start" section specifies exact tooling (VS Code, any modern browser) with no version pins needed since none apply to this topic.
- **PF-08 Prerequisites stated:** PASS — Explicitly states "None. This is session 1" and describes the assumed background (Python/FastAPI, zero web) precisely.

## GATE 2 — DIAMOND FRAMEWORK: 8/8 — PASS

- **DF-01 Core Concept:** PASS — "HTML is a text format for describing structure... not appearance" stated as one clear paragraph, unpacked word by word, with the HTML/CSS/JS split as the memorable takeaway.
- **DF-02 Mental Model/Diagram:** PASS — Full ASCII box-and-arrow diagram of the rendering pipeline (Topic 4) that cannot be replaced by prose; also a comparison table for input types.
- **DF-03 Technical Depth:** PASS — Goes beyond docs-summary level in multiple places: explains *why* CSS blocks rendering (CSSOM completeness requirement) and *why* JS blocks parsing (mutation safety), not just that they do.
- **DF-04 Production Code:** PASS — All code blocks use domain-specific names (`chat-input`, `message-list`, `data-role="assistant"`) — zero foo/bar. The Exercise 3 solution scope is chat-UI-specific throughout.
- **DF-05 Production Relevance:** PASS — Named, specific: "The BIA_CATS_AIA_TEMPLATE_UI... its sidebar is a real `<nav aria-label='Chat history'>`... Radix UI... `data-state='open'`" — names the actual system and the actual mechanism, not a vague "many companies do this."
- **DF-06 Exercises (≥3):** PASS — 3 exercises, easy/medium/hard, each with explicit success criteria. Hard exercise matches the syllabus's own exercise instruction ("Zero CSS," reachable by Tab) exactly.
- **DF-07 Pitfalls (≥2):** PASS — 2 pitfalls, both in strict WRONG→SYMPTOM→WHY→FIX format, both drawn directly from the syllabus's gotcha content, expanded into two distinct, specific failure modes rather than merged into one vague one.
- **DF-08 Further Reading:** PASS — 5 resources, each annotated with what it covers and why it's included, all URLs fetched or confirmed live this session (MDN fetched in full; Scrimba and W3C/Chrome URLs confirmed against prior verified research in this conversation).

## GATE 3 — CODE VALIDATION: 10/10 per block — PASS

Checked every HTML code block in the document (document skeleton, landmark table example, form example, ARIA examples, `data-*` example, pitfall WRONG/FIX pairs):

- **CV-01 Syntax valid:** PASS — All tags properly opened/closed or correctly void; no mismatched nesting in any block.
- **CV-02 All imports exist:** N/A-PASS — No package imports in this topic (pure HTML markup, no JS/Python dependencies).
- **CV-03 No hallucinated methods:** PASS — `$0` and `$$()` Console shortcuts confirmed against live-searched DevTools documentation this session, not asserted from memory.
- **CV-04 Constructor params correct:** N/A — No constructors/classes in this topic's code.
- **CV-05 Async consistency:** N/A — No async code in this topic.
- **CV-06 Complete imports/self-contained:** PASS — Every code block is a complete, standalone HTML fragment or full document; no dangling references to undefined tags or attributes.
- **CV-07 Error handling on external calls:** N/A — No network/file/subprocess calls in this topic's code (that begins in Session S07–S08).
- **CV-08 No deprecated patterns:** PASS — No deprecated HTML (no `<center>`, `<font>`, table-based layout, or ARIA 1.0-era patterns); uses current landmark elements and ARIA 1.2 attributes throughout.
- **CV-09 Runnable with stated setup:** PASS — Every example runs by simply opening the `.html` file in any browser; no build step, no dependencies — correctly matches the "zero CSS, zero JS" scope stated for this session.
- **CV-10 Known-gotcha check:** PASS — Cross-referenced against the Gotcha Registry; no matches apply to plain HTML (registry entries are JS/Python/Docker/SQL-specific). Session's own two pitfalls are new, HTML-specific gotchas appropriate to log for future sessions' cross-reference (see IG-08 below).

## GATE 4 — ACCURACY: 8/8 — PASS

- **AC-01 Version numbers:** PASS — WAI-ARIA 1.2 (June 2023 Recommendation) vs 1.3 (2026 Working Draft) status confirmed against live W3C search results, not assumed.
- **AC-02 API signatures:** PASS — `aria-live` values (`off`/`polite`/`assertive`), `role="alert"`/`role="status"` equivalences, and `tabindex="0"` vs `"-1"` behavior all confirmed against the fetched MDN page content directly, matching verbatim.
- **AC-03 Performance benchmarks:** N/A — No performance benchmark claims made in this session.
- **AC-04 Paper/RFC citations:** PASS — W3C WAI-ARIA 1.2 Recommendation date (June 2023) and W3C's own "only use ARIA when you need to" guidance both sourced and correctly attributed to W3C/MDN.
- **AC-05 Architecture claims:** PASS — Rendering pipeline order (DNS→TCP→HTTP→Parse→DOM+CSSOM→Render Tree→Layout→Paint) matches the syllabus's own stated pipeline exactly, and is standard, uncontested browser architecture.
- **AC-06 Universal statements:** PASS — "Should only use ARIA when you need to" is directly quoted/attributed to the W3C source rather than stated as the author's own universal claim; the `<div onclick>` pitfall is qualified as a keyboard/screen-reader-specific failure, not an absolute "never works" claim.
- **AC-07 Comparative claims:** N/A — No comparative technology claims (e.g., "X is faster than Y") made in this content-structure-focused session.
- **AC-08 Deprecation claims:** N/A — No deprecation claims made requiring changelog confirmation.

## GATE 5 — INTEGRATION: 8/8 — PASS

- **IG-01 Setup block complete:** PASS — No package installs needed for this session; stated tooling (editor + browser) is complete and sufficient.
- **IG-02 End-to-end example:** PASS — Exercise 3's full chat shell is a complete, self-contained runnable HTML document requiring no external step beyond opening the file.
- **IG-03 Prerequisites documented:** PASS — Explicitly "None" — correctly the first session.
- **IG-04 No dangling references:** PASS — Every tag/attribute referenced is either defined inline in this session or explicitly deferred with a named future session (e.g., "you'll build the actual streaming mechanics... in Sessions S08, S21, and S22").
- **IG-05 Exercises solvable from this module alone:** PASS — Verified each exercise step against the 6 topics above; no CSS or JavaScript knowledge required anywhere in Exercises 1–3, matching the syllabus's explicit "Zero CSS" instruction for this exercise.
- **IG-06 Series consistency:** PASS — This is Session 1, establishing rather than needing to match prior patterns; correctly sets the `type="button"` convention that later sessions (per the syllabus's own S12 gotcha) will depend on.
- **IG-07 Additive content:** PASS — No prior session content exists to duplicate.
- **IG-08 Gotcha registry updated:** PASS (see note) — This session surfaces two HTML-specific silent failures (div-as-button breaking keyboard access; default `type="submit"` causing unintended form submission/page reload) not currently present in the shared Gotcha Registry (which is presently JS/Python/Docker/SQL-focused). **Recommendation for the session owner:** add both to the shared registry's "JavaScript / TypeScript" or a new "HTML / Forms" section before Session S12, since S12's own gotcha (inline arrow functions in JSX) sits adjacent to this exact form-submission trap in React.

## TOTAL: 42/42

## STATUS: **APPROVED**

**FAILURES REQUIRING FIXES:** None.

**Non-blocking recommendation carried to Session S02:** When drafting S02, cross-reference this session's Topic 1 landmark elements table so the CSS selectors introduced in S02 explicitly target `<header>`, `<nav>`, `<main>` by tag name at least once, reinforcing the connection rather than introducing class-based selectors as if the semantic tags from S01 didn't exist.
