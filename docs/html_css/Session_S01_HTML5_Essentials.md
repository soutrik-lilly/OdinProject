# Session S01 — HTML5 Essentials
### Semantic Structure, Forms, ARIA & the Browser Rendering Pipeline

**Phase F1 — HTML/CSS Foundations · Day 1 of 30 (Day 1 of 37 overall) · Agentic UI Engineer Track**

---

## Before You Start

**Who this session assumes you are:** a working Python/FastAPI engineer who has never written a line of HTML, CSS, or JavaScript with intent. You know what a server is, what a request/response cycle is, and what it means for something to be "typed." You do not know what a browser does with the text you send it. This session starts exactly there.

**Prerequisites:** none, except a text editor (VS Code is fine) and any modern browser (Chrome, Firefox, or Edge).

**What "done" looks like by the end of today:** you can open a blank file, write a complete, valid, accessible HTML document from memory, explain to a colleague why every single line in it exists, and you understand — genuinely understand, not memorized — why a browser turns that text into a picture on a screen.

**How this session connects to the road map:** Session S01 is pure HTML — no CSS, no JavaScript. That is deliberate. The Slim 30 syllabus (your permanent reference for this entire 37-day plan) treats structure and styling and behavior as three separate skills, taught in that order, because that is also the order a browser processes them internally (you will see exactly why in Topic 4 below). S02–S03 will style the exact same document you build today. By S04, you will extend today's skeleton into the full static chat interface, still with zero JavaScript. Nothing you write today gets thrown away — it gets dressed up.

**Running in parallel with The Odin Project:** you mentioned you are working through Odin's Foundations course starting from HTML, in parallel with this syllabus. Good — here is exactly how the two relate, so you're not duplicating effort or wondering why this session covers more ground.

Odin's Foundations course teaches HTML across a few early lessons ("Introduction to HTML and CSS," "Elements and Tags," working with text/lists/links/images, then forms later once you've also covered some CSS). It is a good, community-vetted sequence — but by its own explicit design, it **defers two things you need today** to a *different, later* course called "Advanced HTML and CSS": accessibility and ARIA. Odin's own introduction lesson says so directly: *"You'll learn about animations, accessibility, and responsive design later in the curriculum, in the Advanced HTML and CSS course."* Odin's HTML lessons also do not cover the browser's DevTools (that shows up afterward, folded into the JavaScript section as "JavaScript Developer Tools") or the browser rendering pipeline as a topic in its own right.

This session covers all of that **today**, on day one, because you are building a real production interface (a streaming chat UI) from the start, not a personal portfolio site — and a chat UI without accessibility support is a chat UI that silently fails for a meaningful fraction of real users. So: everything Odin's Foundations HTML lessons cover, this session covers too, in more depth and with working code you'll reuse for the next 36 days. Where Odin defers a topic to "later," this session teaches it now, because your project needs it now. Do Odin's HTML lessons if you want a second explanation in different words — they're a fine supplement — but this document is self-contained and is the one your 37-day project depends on.

---

## Table of Contents

1. [The Document Skeleton — DOCTYPE, head, and why every tag has a job](#topic-1)
2. [Forms, Input Types & Validation](#topic-2)
3. [ARIA Fundamentals](#topic-3)
4. [The Browser Rendering Pipeline](#topic-4)
5. [Accessibility for Chat UIs](#topic-5)
6. [DevTools Fundamentals & `data-*` Attributes](#topic-6)
7. [Session Exercises](#exercises)
8. [Pitfalls](#pitfalls)
9. [Further Reading](#further-reading)
10. [Session Recap & What's Next](#recap)

---

<a name="topic-1"></a>
## Topic 1 — The Document Skeleton: DOCTYPE, `<head>`, and Why Every Tag Has a Job

### What it is, and why it exists

An HTML document is not "a file that has some tags in it." It is a **contract between you and the browser**: a strict, agreed-upon shape that tells the browser "read me this way." Before the browser can draw a single pixel, it needs to know three things about the document you sent it: what *kind* of document this is (HTML5, and nothing else — no ambiguity), what language it's written in, and what character encoding the bytes are in. Without that agreement, the browser has to *guess*, and browsers guessing is exactly how you get broken layouts, garbled text, and — critically for later sessions — screen readers announcing content in the wrong language.

Think of it like an HTTP request in FastAPI. You wouldn't send a request body without a `Content-Type` header and expect the server to figure out on its own whether it's JSON or form data — it *might* guess right, but you don't build production systems on "might." The top of every HTML document is that same kind of explicit header, just for the browser instead of a server.

### The skeleton, piece by piece

```
┌──────────────────────────────────────────┐
│ <!DOCTYPE html>                           │  ← "Render me in Standards Mode, HTML5 rules"
├──────────────────────────────────────────┤
│ <html lang="en">                          │  ← the root; lang= tells screen readers
│   ┌────────────────────────────────────┐  │     and translators what language this is
│   │ <head>                             │  │
│   │   <meta charset="UTF-8">           │  │  ← how to decode the bytes into characters
│   │   <meta name="viewport" ...>       │  │  ← how to size the page on mobile screens
│   │   <title>...</title>               │  │  ← what shows in the browser tab
│   │ </head>                            │  │
│   └────────────────────────────────────┘  │
│   ┌────────────────────────────────────┐  │
│   │ <body>                             │  │  ← everything the user actually sees
│   │   (all your visible content lives  │  │
│   │    here — this is what the next    │  │
│   │    five topics are about)          │  │
│   │ </body>                            │  │
└──────────────────────────────────────────┘
```

Every piece in that diagram has a job, and only one job. Nothing is decorative.

- **`<!DOCTYPE html>`** — this is not a tag, and it is not optional. It is a signal to the browser's parser: "treat this as HTML5, use Standards Mode." Omit it, and older layout logic (Quirks Mode, inherited from how browsers handled broken HTML in the 1990s) can silently activate, changing how box sizing and dozens of other things behave. You will never see an error for a missing DOCTYPE — the page will just render subtly wrong, forever, with no warning. That's a **silent failure**, the worst kind, and it's why this line is the very first thing in every HTML file you will ever write.
- **`<html lang="en">`** — the `lang` attribute is read by screen readers to select the correct pronunciation engine and by browsers to offer translation. If you leave it off, a screen reader may mispronounce every word in the document, and this isn't a rare edge case — it's one of the most common accessibility failures in real, shipped products.
- **`<meta charset="UTF-8">`** — tells the browser how to turn the raw bytes of the file into text characters. UTF-8 covers essentially every character in every language plus emoji; it must be declared, and it must be one of the first things in `<head>`, because the browser needs to know the encoding before it can correctly parse anything that follows, including the `<title>` if it contains a non-ASCII character.
- **`<meta name="viewport" content="width=device-width, initial-scale=1.0">`** — without this, a mobile browser assumes your page was built for a desktop-width screen (usually 980px) and shrinks the whole thing down to fit, making text unreadably tiny. This single line is why some old websites look zoomed-out on a phone and modern ones don't.
- **`<title>`** — shown in the browser tab, in bookmarks, and read aloud first by screen readers when a page loads. It is not decorative text; for a screen reader user, it's the first thing they hear, so it needs to actually describe the page.

### Landmark elements: giving the document's regions a name

Inside `<body>`, HTML5 gives you a set of **semantic landmark elements** — tags whose *name* tells the browser (and any assistive technology) what kind of region of the page this is, rather than just how it should look:

```
<header>   — introductory content, usually branding/title/top navigation
<nav>      — a block of navigation links
<main>     — the one primary, unique content of THIS page (exactly one per page)
<aside>    — content tangential to the main content (a sidebar, related links)
<footer>   — closing content, usually copyright/secondary links
<section>  — a thematic grouping of content, usually with its own heading
<article>  — a self-contained piece of content that would make sense on its own
```

Contrast this with `<div>`, which means nothing at all — it is a pure grouping box with zero semantic meaning. A `<div>` is like a Python `dict` with no schema: it holds things, but nothing about it tells you what it holds. A `<nav>` is like a Pydantic model called `NavigationBar` — the name itself carries information that both humans and machines can use.

This matters concretely, today, for the chat UI you're building: a screen reader user can press a single key to jump directly between landmark regions ("skip to main," "jump to navigation") without having to tab through every single element on the page one at a time. If your entire chat app is built out of `<div>` soup, that jump-navigation feature simply does not exist for that user — not "works badly," *does not exist*.

### Heading hierarchy: the document's outline

`<h1>` through `<h6>` are not "big text" and "medium text" — they define the document's outline, the same way Markdown's `#`, `##`, `###` define this very document's outline. There must be exactly **one `<h1>` per page** (it's the title of *this page*, not "big text I want at the top"), and headings should not skip levels going down (an `<h3>` should not appear directly under an `<h1>` with no `<h2>` in between) — screen reader users frequently navigate a page by heading level alone, the same way you might use a table of contents, and a broken hierarchy breaks that navigation silently.

### The `button` vs `div` decision — made right, from the start

You will see this again as this session's central pitfall, but the concept belongs here, at the skeleton level, because it's a structural decision, not a styling one: **any element the user can click or activate must be a real interactive element** — `<button>` for actions, `<a href="...">` for navigation — never a `<div>` with a click handler bolted on. A `<div>` is invisible to a keyboard user (it can't receive focus) and invisible to a screen reader (it has no role). `<button>` gets keyboard focusability, Enter/Space activation, and an announced role ("button") for free, from the browser, with zero extra code. You will build this correctly in the exercise below and never need to "add accessibility later" — because you never subtracted it in the first place.

### Grouping form-related controls: `fieldset` and `legend`

One more skeleton-level element you'll use immediately: `<fieldset>` wraps a *group* of related form controls, and `<legend>` is that group's caption — read aloud by screen readers before any control inside the group. You won't build a form with radio buttons today, but the input area of your chat UI (label + textarea + button) is exactly the kind of small, related group that benefits from this pattern, and you'll use it explicitly in the exercise.

### Production relevance

The GOV.UK Design System — the UK government's official frontend framework, used across hundreds of government services handling millions of users — publishes its own accessibility statement citing direct compliance with the Public Sector Bodies (Websites and Mobile Applications) (No. 2) Accessibility Regulations 2018, which since a December 2022 update require UK public sector websites to conform to WCAG 2.2 AA. That regulation is enforced in law, not just recommended: it mandates exactly this session's skeleton discipline — one `<h1>`, semantic landmarks over `<div>`s, and a declared `lang` attribute — specifically because government services are legally required to be usable by screen reader and switch-access users. This isn't an academic best practice; it's the load-bearing structure of an accessibility-compliant product, backed by UK statute (S.I. 2018/952) and checked in real government service audits.

### Quick Check — Topic 1

Before moving on, you should be able to answer, out loud, without looking back:
1. What are the two failure modes of leaving out `<!DOCTYPE html>`, and why does neither one throw an error?
2. Why can a page have exactly one `<main>` but potentially several `<section>`s?
3. If a `<div>` and a `<button>` both look identical after styling, what is the actual, functional difference between them for a keyboard-only user?

---

<a name="topic-2"></a>
## Topic 2 — Forms, Input Types & Validation

### What it is, and why it exists

Every chat interface — including the BIA production UI this entire syllabus builds toward — is, structurally, a form: the user types text into an input, and submits it. HTML forms exist because the browser needs a standard way to (a) collect typed and structured input from the user, (b) validate some of it *before* any JavaScript even runs, and (c) associate every input with a human-readable label, both visually and for assistive technology. All three of those are things you get from the browser for free, if you use the actual form elements instead of styled `<div>`s pretending to be inputs.

### The label/input binding contract

```
<label for="chat-input">Message</label>
<input id="chat-input" type="text">
        ▲                    ▲
        │                    │
        └──── these two MUST match ────┘
```

This is not a styling convenience — `for` and `id` matching creates a real, browser-enforced relationship. Click the label text, and focus jumps to the input. A screen reader announces the label's text every time the input receives focus. Without this pairing, a sighted user might visually associate a label with a nearby input just by proximity — but a screen reader user has no "nearby," only "linked" or "not linked." This is the single most common accessibility bug in real forms: labels that *look* attached but aren't *programmatically* attached.

### Input types — each one is a contract, not a decoration

```
type="text"      — free text, one line
type="email"     — mobile keyboards show @ and .com shortcuts; basic format hint to browser
type="password"  — characters are masked as you type
type="number"    — mobile keyboards show a numeric pad; up/down spinners appear
type="search"    — semantically "this is a search box"; some browsers show a clear (x) button
type="checkbox"  — one or more options can be selected independently
type="radio"     — exactly one option from a named group can be selected
type="file"      — opens the OS file picker; produces a File object
```

Each `type` value changes real, functional browser behavior — not just appearance. Setting `type="email"` on a mobile device changes the on-screen keyboard layout to make `@` easy to reach. This is "free" functionality: you write one word in one attribute, and every browser and every mobile OS on the planet adjusts its behavior to match, with no JavaScript required.

### Native validation, before a single line of JavaScript exists

```html
<input type="email" required>
<input type="number" min="1" max="10">
<input type="text" pattern="[A-Za-z]{3,}" title="At least 3 letters">
```

`required`, `min`, `max`, and `pattern` are enforced by the browser itself, at the moment of form submission, before any JavaScript runs. Try to submit a form with a `required` field empty, and the browser blocks the submission and shows its own validation message — for free. This matters for the same reason input validation matters in a FastAPI route: you never trust that the client "probably" sent good data, and native HTML validation is your first, cheapest line of defense — one you get before you've written a single line of your own validation logic.

### `FormData` — reading the form programmatically

Later sessions (once JavaScript is introduced in S05–S09) will read form values using the `FormData` API, which walks the DOM and pulls every named input's current value into a single object — the browser's own equivalent of `request.form()` in a web framework, except it happens client-side, before anything is sent anywhere. You don't need to write this code yet; you need to understand that this is *why* every input in a form needs a `name` attribute (or, in a component-driven React world later, is read via its own state) — it's the key `FormData` uses to identify each field.

### Domain code — the chat input, done correctly

```html
<form>
  <fieldset>
    <legend>Send a message</legend>

    <label for="chat-message">Message</label>
    <textarea
      id="chat-message"
      name="message"
      required
      placeholder="Ask Socrates anything..."
    ></textarea>

    <button type="submit">Send</button>
  </fieldset>
</form>
```

Notice: `<textarea>` (not `<input>`) because chat messages are multi-line. `required` so the browser itself blocks an empty submission. `label`/`for` correctly paired. `fieldset`/`legend` grouping the one logical unit. And `type="submit"` is explicit and intentional here — this is the one button in this entire session that *should* submit the form, because that's its only job.

### Production relevance

Any payment or checkout form handling real money — the category Stripe's widely-embedded checkout UI belongs to — has a direct, measurable incentive to get input types right: the correct `type="email"` or `type="tel"` changes which on-screen keyboard a mobile user sees, and a wrong keyboard is friction at exactly the moment a company least wants to lose a paying customer. This is why "boring" HTML attributes are treated as a first-class concern by teams building checkout flows, not an afterthought — native browser validation via `required`/`pattern` is the cheapest possible first-pass filter, catching obviously-invalid input before any custom JavaScript validation logic even runs.

### Quick Check — Topic 2

1. Why does `type="email"` matter even though you could validate email format yourself later in JavaScript?
2. What breaks, specifically, if a `<label>`'s `for` attribute doesn't match any `id` on the page?
3. Why is `<textarea>` the correct element for a chat message box instead of `<input type="text">`?

---

<a name="topic-3"></a>
## Topic 3 — ARIA Fundamentals

### What it is, and why it exists

ARIA (Accessible Rich Internet Applications) is a set of HTML attributes that exist for exactly one purpose: to describe things to assistive technology that plain HTML cannot describe on its own — usually because you're building a custom, dynamic interface (like a chat app) that doesn't map cleanly onto the handful of native elements browsers ship with. ARIA does not change how anything *looks*. It does not change how anything *behaves* for a mouse or keyboard user. It exists purely in a second, parallel representation of your page called the **accessibility tree**, which is what screen readers actually read from — not your HTML, not your CSS, the accessibility tree the browser builds *from* your HTML.

The single most important rule in this entire topic, stated exactly the way the official W3C ARIA Authoring Practices state it: **"No ARIA is better than bad ARIA."** Native HTML elements (`<button>`, `<nav>`, `<input>`) come with correct accessibility behavior built in, verified by every browser vendor, for free. ARIA is a patch you reach for only when no native element does the job — and if you apply it incorrectly, you can actively make a page *less* accessible than doing nothing at all, because you can override a screen reader's default (correct) behavior with an incorrect one.

### The core vocabulary

```
role="..."          — declares WHAT an element is, when the tag itself doesn't say
                       (button, dialog, listbox, status, alert, navigation...)

aria-label="..."     — provides an accessible name when there's no visible text
                       (e.g. an icon-only button)

aria-labelledby="id" — points to ANOTHER element on the page whose text becomes
                       this element's accessible name

aria-hidden="true"   — hides an element from the accessibility tree entirely
                       (for purely decorative content — the screen reader
                        skips it completely, as if it doesn't exist)

aria-live="polite"   — marks a region as "dynamic content will appear here;
                       announce it, but wait until the user is idle"
```

### `aria-live="polite"` — the one you will use constantly, starting today

This is the ARIA attribute your entire chat UI depends on, so it earns a deeper look. When new assistant tokens stream into the message list, there is no click, no keypress, no form submission — the content just *appears*. A sighted user sees it happen. A screen reader user, by default, hears nothing, because nothing they did caused a screen reader announcement. `aria-live="polite"` on the container tells the screen reader: "watch this element; when its content changes, announce the change — but wait for a natural pause in what the user is currently doing, don't interrupt them mid-sentence."

Here is the piece that goes deeper than most tutorials, and that you will need working code for by S08 (streaming): **the live region must already exist in the DOM, with `aria-live` already set, *before* you insert content into it.** Screen readers register which regions are "live" once, at page load (or once, when an element is first created). If you create a fresh `<div>` and set both its content *and* its `aria-live` attribute in the same moment, several major screen readers will simply miss the announcement — not because the code is wrong, but because the *registration* happened too late. The fix, which you'll apply directly in this session's exercise: put an empty, permanent container with `aria-live="polite"` in your HTML from the very first render, and only ever change what's *inside* it afterward.

### `role`, and when you actually need it

```html
<!-- Decorative icon inside a button that already has visible text — hide the icon -->
<button type="button">
  <svg aria-hidden="true">...</svg>
  Send
</button>

<!-- Icon-only button with NO visible text — the icon needs a spoken name -->
<button type="button" aria-label="Delete conversation">
  <svg aria-hidden="true">...</svg>
</button>
```

Notice the icon itself always gets `aria-hidden="true"` — an SVG icon has no inherent meaning a screen reader can announce usefully, so you either hide it (when adjacent text already does the job) or replace it with an explicit `aria-label` on the parent button (when there's no adjacent text at all). Getting this backward — leaving the icon exposed to the accessibility tree with no label, or hiding the whole button — is one of the most common real-world ARIA bugs.

### Production relevance

GitHub's own UI — one of the most heavily used developer-facing products in the world — uses `aria-live="polite"` regions extensively for exactly the pattern you're learning today: PR status checks updating in place, notification counts changing, search results loading — all announced to screen reader users without a page reload and without interrupting whatever the user was doing. The registration-timing rule from this topic is not a rare edge case invented for this tutorial — it is documented directly in MDN's own ARIA live regions guide as a genuine, common source of missed announcements, which is exactly why this session insists on wiring `aria-live="polite"` into the HTML from the very first render rather than adding it later with JavaScript.

### Quick Check — Topic 3

1. What is the accessibility tree, and how is it different from the DOM?
2. Why would setting `aria-live="polite"` on a `<div>` you create *and* fill with content in the same JavaScript statement fail silently?
3. When should you use `aria-label` instead of `aria-hidden` on an icon inside a button?

---

<a name="topic-4"></a>
## Topic 4 — The Browser Rendering Pipeline

### What it is, and why it exists

Every time you load a web page, the browser runs a fixed, well-defined pipeline to turn the bytes you sent it into pixels on a screen. Understanding this pipeline is not academic trivia — it directly explains *why* the `<head>` is ordered the way it is, *why* your CSS file must be linked before your page's content, and *why* a slow or badly-placed `<script>` tag can make an entire page appear to freeze. As a backend engineer, think of this the way you'd think of understanding exactly how `uvicorn` handles a request through ASGI middleware before you write a FastAPI dependency that assumes a particular execution order.

### The pipeline, step by step

```
1. URL entered / link clicked
        │
2. DNS lookup           →  domain name resolved to an IP address
        │
3. TCP connection        →  (TLS handshake, if HTTPS)
        │
4. HTTP request/response →  browser sends GET, server sends back HTML bytes
        │
5. HTML PARSING          →  browser reads HTML top-to-bottom, building the DOM
        │                   (the Document Object Model — an in-memory tree
        │                    of every element in your HTML)
        │
6. CSS PARSING           →  in parallel, builds the CSSOM
        │                   (an in-memory tree of every style rule)
        │
7. RENDER TREE           →  DOM + CSSOM combined = what to actually draw
        │                   (elements with display:none are excluded here)
        │
8. LAYOUT (reflow)       →  browser calculates the exact size and position
        │                   of every element on the page
        │
9. PAINT                 →  pixels are actually drawn to the screen
```

### Why JavaScript blocks HTML parsing (and why this matters for `<head>` order)

Step 5, HTML parsing, is where things get interesting: when the browser's parser hits a `<script>` tag with no special attributes, it must **stop parsing the rest of the HTML**, fetch the script (if external), execute it fully, and only then resume parsing the document. This is because a script can use `document.write()` to inject more HTML — so the browser can't safely skip ahead without risking missing content the script is about to add. This is exactly why a `<script>` tag placed carelessly in `<head>`, before your content, can make an entire page appear to hang while a slow script loads — the user sees a blank white page, not because rendering is slow, but because *parsing itself* has stopped.

Two attributes fix this, and you'll use them by name in later sessions:

```
<script src="app.js"></script>          ← blocks parsing (default, avoid this)
<script src="app.js" defer></script>    ← downloads in parallel, runs AFTER parsing finishes
<script src="app.js" async></script>    ← downloads in parallel, runs AS SOON AS it's ready
                                            (may run before parsing finishes — order not guaranteed)
```

`defer` is what you want for almost everything: the browser keeps parsing your HTML immediately, downloads the script in the background, and only executes it once the entire document is parsed — the best of both worlds.

### Why CSS blocks rendering, and why `<link rel="stylesheet">` belongs in `<head>`

Step 6/7 explain something you'll feel directly in S02–S03: the browser will not paint anything to the screen until it has both the DOM *and* the CSSOM — because painting an element without knowing its final styles would mean painting it once, then immediately repainting it differently once the CSS arrives, causing a visible flash of unstyled content. This is why `<link rel="stylesheet">` always belongs in `<head>`, loaded as early as possible: the browser deliberately delays the very first paint until your CSS is available, specifically to avoid that flash.

### Production relevance

Google's own Core Web Vitals — the performance metrics that directly affect a site's search ranking, checked by real automated crawlers on billions of pages — includes a metric called Largest Contentful Paint (LCP), which measures exactly how long steps 1 through 9 above take for the largest visible element on a page. Improving LCP is largely a matter of respecting this exact pipeline: avoiding render-blocking scripts placed carelessly in `<head>`, and getting critical CSS to the browser as early as possible — this pipeline isn't background theory, it is the literal mechanism a search-ranking signal used across billions of pages is measuring.

### Quick Check — Topic 4

1. Put these in the correct pipeline order: Paint, DOM construction, Layout, HTTP response, Render Tree.
2. Why does a `<script>` tag with no `defer` or `async` block HTML parsing, specifically?
3. Why does the browser wait for the CSSOM before painting anything, even elements with very simple styling?

---

<a name="topic-5"></a>
## Topic 5 — Accessibility for Chat UIs

### What it is, and why it exists

This topic is where Topics 1 through 4 combine into the specific, non-negotiable pattern your production chat interface must follow. A chat UI is unusually demanding for accessibility, for one structural reason: almost everything in it is dynamic. Messages appear without a page reload. Text streams in token by token. There's no natural "page load" moment for a screen reader to announce anything — so if you get the accessibility wiring wrong here, the entire application is silently broken for a screen reader user, even though it looks and works perfectly for you.

### The four rules, and why each one is non-negotiable

**Rule 1 — every interactive element is a real `<button>` or `<a>`, never a styled `<div>`.** You met this in Topic 1; here is the concrete list of everywhere it applies in a chat UI specifically: the send button, the stop-generation button, the delete-conversation button, every dropdown menu trigger, every sidebar history item. If any of these is a `<div onClick={...}>` instead of a real `<button>`, a keyboard-only user cannot Tab to it and cannot activate it with Enter or Space — it is functionally invisible to them, full stop.

**Rule 2 — tab order follows DOM order, and you almost never override it.** By default, pressing Tab moves focus through the page in the exact order elements appear in your HTML — top to bottom, matching your visual layout if your layout is sensible. The `tabindex` attribute *can* override this order, but doing so is one of the most common ways to make a page's keyboard navigation feel broken and unpredictable, because it decouples "what you see" from "what order Tab visits." The correct move, almost always, is to write your HTML in the visual order you want and let the browser's default tab order do the work for free.

**Rule 3 — focus rings are never removed without a replacement.** `outline: none` (or Tailwind's `outline-none`) removes the browser's default visual indicator of "this is the currently focused element" — the one and only way a keyboard user can see where they are on the page. Removing it with nothing in its place is, functionally, the same class of bug as removing a cursor from a text editor: the user can still type, they just can't see where. You will always either keep the default outline or explicitly style a replacement (`focus-visible` styles, covered in S02), never simply delete it.

**Rule 4 — the message list carries `aria-live="polite"`, wired correctly from first render.** This is Topic 3's deep-dive, applied: the container that holds chat messages must have `aria-live="polite"` present in the very first HTML the browser receives — not added later by JavaScript once a message arrives. Get this timing wrong, and streaming text is invisible to a screen reader user for the entire session, with no error, no warning, nothing — it simply never gets announced.

### Domain code — the complete accessible chat shell

This is the actual exercise deliverable for this session, shown here fully worked so you can see all five topics combine into one real artifact:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Socrates — Agentic Chat</title>
</head>
<body>

  <header>
    <button type="button" aria-label="Toggle sidebar">☰</button>
    <h1>Socrates</h1>
  </header>

  <nav aria-label="Chat history">
    <ul>
      <li><a href="#">Load-test readiness analysis</a></li>
      <li><a href="#">FlowMedic pipeline design</a></li>
    </ul>
  </nav>

  <main role="main">
    <div id="message-list" aria-live="polite">
      <!-- messages will be inserted here, later, once JavaScript exists -->
      <!-- this container ALREADY has aria-live set, before any content arrives -->
    </div>

    <form>
      <fieldset>
        <legend>Send a message</legend>

        <label for="chat-message">Message</label>
        <textarea
          id="chat-message"
          name="message"
          required
          placeholder="Ask Socrates anything..."
        ></textarea>

        <button type="submit">Send</button>
        <button type="button" aria-label="Stop generation" hidden>Stop</button>
      </fieldset>
    </form>
  </main>

</body>
</html>
```

Walk through why every line is there: `lang="en"` from Topic 1, the viewport meta from Topic 1, `<header>`/`<nav>`/`<main>` landmarks from Topic 1, the toggle button using `aria-label` because it has no visible text (Topic 3), the `<nav aria-label="Chat history">` giving this specific navigation region a distinguishing name (useful the moment a page has more than one `<nav>`), `#message-list` with `aria-live="polite"` present from the very first render (Topics 3 and 5 combined), the form's `label`/`fieldset`/`legend` structure (Topic 2), `type="submit"` on Send and `type="button"` on Stop (today's central pitfall, covered fully below), and zero `<div>`s used for anything clickable.

### Production relevance

Slack's own web client — a chat product used by tens of millions of daily active users, including screen reader users in regulated industries with legal accessibility requirements — relies on exactly this `aria-live` pattern on its message pane to make incoming messages audible to screen reader users without requiring them to manually re-check the conversation. The four rules in this topic are not a checklist invented for this tutorial; they are the same baseline the W3C's own WAI-ARIA Authoring Practices Guide sets for any "live region" widget, which is the category every real-time chat interface — Slack, Teams, or the BIA UI you are building — falls into.

### Quick Check — Topic 5

1. Name all four interactive elements in a typical chat UI that must never be `<div>`s.
2. Why is overriding `tabindex` usually a worse choice than just reordering your HTML?
3. In the code block above, why does `#message-list` already have `aria-live="polite"` even though it's currently empty?

---

<a name="topic-6"></a>
## Topic 6 — DevTools Fundamentals & `data-*` Attributes

### What it is, and why it exists

Every browser ships a full inspection and debugging toolkit, built in, free, called DevTools (open it with `F12` on Windows/Linux, or `Cmd+Option+I` on Mac). For the rest of this 37-day plan, DevTools is your equivalent of a Python debugger plus `curl` plus `htop`, all in one panel — and today is the day you learn where each of those capabilities lives, because from S05 onward you will use this tool in nearly every single exercise.

### The panels that matter, and what each one is actually for

```
Elements    — the live DOM tree. Click any element on the page, see its exact HTML.
              Edit it live, in the browser, to experiment — changes vanish on reload,
              nothing is saved, so this is a completely safe sandbox for testing ideas.

Console     — a live JavaScript execution environment, running in the context of
              THIS page. $0 refers to whatever element you last selected in Elements.
              $$('selector') is a shortcut for document.querySelectorAll('selector').

Network     — every HTTP request the page has made: headers sent, headers received,
              response body, and a timing waterfall showing exactly how long DNS,
              connection, and download each took. This is where you will debug
              auth headers and streaming SSE responses starting in S08 and S20.

Performance — records a timeline of exactly what the browser did: scripting,
              rendering, painting — as a flame chart. This is where "why is my
              chat UI janky while streaming" gets answered with evidence, not guesses.

Sources     — set actual breakpoints in your JavaScript, step through execution
              line by line, inspect variables at each step — a real debugger,
              not console.log statements.
```

For today, specifically: open Elements, click on any part of a real webpage, and watch the HTML tree highlight in sync with your cursor. This single exercise — clicking around a real, live DOM — will teach you more about how the skeleton from Topic 1 actually behaves than any amount of reading.

### `data-*` attributes — a hook with no built-in meaning

```html
<div data-role="user" data-state="sent">...</div>
```

Any attribute you prefix with `data-` is guaranteed, by the HTML specification, to never conflict with any current or future standard HTML attribute — it exists purely for *you* to attach information for your own CSS or JavaScript to read later. This is the mechanism component libraries use to expose internal state to CSS without needing a class name for every possible state combination:

```css
[data-state="open"] { display: block; }
[data-state="closed"] { display: none; }
```

Radix UI — the accessible component primitives library your syllabus uses starting in S17 for the sidebar, dropdown menus, and dialogs — exposes every single piece of internal state this way: `data-state="open"`/`"closed"`, `data-disabled`, `data-orientation`. You will style Radix components almost entirely by targeting these attributes rather than adding your own class names, and understanding *why* this pattern exists (a stable, semantic hook, decoupled from CSS class naming) starts here, in plain HTML, before any library is involved.

In the BIA production UI this syllabus builds toward, this exact pattern shows up twice you'll recognize immediately once you get there: sidebar history items carry `data-active` to mark the currently open conversation, and message bubbles carry `data-role="user"`/`data-role="assistant"` so a single CSS rule can style both roles differently without needing two separate component implementations.

### Production relevance

The shadcn/ui component library — which this syllabus uses starting in S17, and which is one of the most widely adopted component collections in the current React ecosystem — is built entirely on top of Radix UI's `data-*` state exposure rather than inventing its own state-tracking mechanism. Every "it just works" animation and style variant you'll write starting in S17 depends on this exact, plain-HTML mechanism you're learning today with zero libraries involved.

### Quick Check — Topic 6

1. What does `$0` refer to in the DevTools Console, and how do you set it?
2. Why will a `data-role` attribute never collide with a future official HTML attribute?
3. Which DevTools panel would you open to see the exact headers sent on a chat request, and which panel would you open to see whether a script blocked rendering?

---

<a name="exercises"></a>
## Session Exercises

Every exercise below is solvable using only what this document has covered — nothing further ahead in the syllabus is required.

### Exercise 1 (Easy) — Build and validate the skeleton

Write a complete, empty HTML5 document from memory: `<!DOCTYPE html>`, `<html lang="en">`, `<head>` with `charset`, `viewport`, and a `<title>`, and an empty `<body>`. Open it in a browser. Open DevTools → Elements, and confirm the DOM tree matches what you wrote exactly. Then delete the `<!DOCTYPE html>` line, reload, and open DevTools Console — type `document.compatMode` and press Enter. You should see `"CSS1Compat"` with the DOCTYPE present, and `"BackCompat"` without it. This is Quirks Mode, made visible.

**Success criteria:** you can explain, in your own words, what `document.compatMode` just proved to you.

### Exercise 2 (Medium) — The full accessible chat shell

Build the complete semantic HTML shell from Topic 5's code block, but do not copy it — write it from the topic explanations, referring back only to check yourself afterward. It must include: a `<header>` with a sidebar-toggle `<button>` and an `<h1>`; a `<nav aria-label="Chat history">` with at least two links; a `<main role="main">` containing a `<div id="message-list" aria-live="polite">` and a `<form>` with a properly paired `<label>`/`<textarea>` inside a `<fieldset>`/`<legend>`, a `type="submit"` Send button, and a `type="button"` Stop button. Zero CSS. Zero `<div>`s used for anything clickable.

**Success criteria:** every single interactive element on the page — toggle, both nav links, textarea, Send, Stop — is reachable, in a sensible order, using only the Tab key, with no mouse. Confirm this yourself before moving on: unplug your mouse (or just don't touch it) and Tab through the entire page.

### Exercise 3 (Hard) — Break it, then explain why it's broken

Take your working Exercise 2 page and deliberately introduce three specific bugs, one at a time, reloading and re-testing after each: (a) change the Send button from `type="submit"` to no `type` at all — reload, and note what changes about the page's behavior when you press Enter inside the textarea; (b) change the `<label for="chat-message">` to `<label for="chatmessage">` (remove the hyphen) so it no longer matches the textarea's `id` — reload, and click directly on the label text, noting whether focus still jumps to the textarea; (c) move the `aria-live="polite"` attribute off `#message-list` entirely — this one you can't test by eye, but write down, in your own words, exactly what class of user would be affected and how, referencing Topic 3's registration-timing explanation.

**Success criteria:** for each of the three bugs, you can state, precisely, what actually broke and why — not "it feels different," but the specific mechanism (default form submission behavior, the `for`/`id` binding contract, or accessibility-tree registration timing).

---

<a name="pitfalls"></a>
## Pitfalls

### Pitfall 1 — The missing `type="button"` silent form submission

**WRONG (what almost every beginner writes):**
```html
<form>
  <textarea name="message"></textarea>
  <button>Send</button>
  <button onclick="stopGeneration()">Stop</button>
</form>
```

**SYMPTOM:** Click "Stop" while a message is streaming. Instead of stopping generation, the entire form submits, the page reloads (or, in a React SPA later, the app re-renders unexpectedly), and any in-progress state — the streaming message, the scroll position, everything — is lost. Nothing in the console explains why; there's no error, because nothing actually failed. The browser did exactly what you told it to do.

**WHY:** inside a `<form>`, any `<button>` with no explicit `type` attribute defaults to `type="submit"`. This is the HTML specification's behavior, not a bug — but it means *both* buttons in the WRONG example above submit the form when clicked, because neither one declares otherwise. The `onclick` handler on "Stop" does run, but the form submission happens too, and a full-page form submission is a far bigger, more destructive event than whatever `stopGeneration()` was supposed to do.

**FIX:**
```html
<form>
  <textarea name="message"></textarea>
  <button type="submit">Send</button>
  <button type="button" onclick="stopGeneration()">Stop</button>
</form>
```

Every button inside a `<form>` that is not the one intended action that submits the form must have `type="button"` explicitly. This is not optional, defensive styling — it is the single most common unexpected-form-submission bug in real production React and vanilla-JS applications, and it will bite you again the moment you build the real chat UI's Stop button in a later session if you don't build the habit now.

### Pitfall 2 — `<div>` with a click handler instead of a real button

**WRONG (looks completely fine, visually):**
```html
<div class="send-btn" onclick="sendMessage()">Send</div>
```

**SYMPTOM:** Visually indistinguishable from a real button, especially once CSS is applied in S02. But: Tab through the page, and focus skips straight over it — it simply cannot receive keyboard focus. A screen reader encountering it announces nothing useful; it has no role, so it's read as plain, non-interactive text. A keyboard-only user, or a screen reader user, has genuinely no way to activate this "button" — there is no error message anywhere telling you this, because from the browser's point of view, you never asked for interactive behavior; you asked for a `<div>` and got exactly that.

**WHY:** `<div>` carries no default ARIA role and is not part of the browser's native tab order. `onclick` makes it respond to a *mouse* click, and only a mouse click — it does nothing for Enter, Space, or any assistive technology, because none of that behavior is native to `<div>`; you'd have to manually add `tabindex="0"`, `role="button"`, and separate keyboard event handlers for Enter and Space to fully replicate what `<button>` gives you automatically. At that point you've written more code to badly reinvent something the browser already does correctly for free.

**FIX:**
```html
<button type="button" onclick="sendMessage()">Send</button>
```

Use the real element. You get focusability, Enter/Space activation, and an announced "button" role, all for zero additional code, from every browser, on every platform, with zero exceptions.

---

<a name="further-reading"></a>
## Further Reading

**MDN — Structuring the web with HTML** (official docs) — Mozilla's authoritative reference on every element covered in Topics 1, 2, and 5. Use this as your dictionary: whenever you forget an attribute's exact behavior, this is the source of truth, not a blog post repeating what MDN already says better.
https://developer.mozilla.org/en-US/docs/Learn/HTML

**MDN — ARIA Live Regions** (official docs) — the exact source for Topic 3's `aria-live` timing behavior, including the browser-support notes on `aria-atomic` and `aria-relevant` that this session didn't have room to cover in full.
https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Guides/Live_regions

**W3C WAI-ARIA Authoring Practices Guide** (official spec-adjacent guide) — the canonical source for "no ARIA is better than bad ARIA" and every correct pattern for common interactive widgets. This is what Radix UI and shadcn/ui (which you'll use starting S17) are themselves built against — reading it now means later sessions' library choices will make sense as implementations of patterns you already recognize.
https://www.w3.org/WAI/ARIA/apg/

**web.dev — Rendering Performance** (Google, official) — a deeper, illustrated walk through the rendering pipeline from Topic 4, written by the Chrome team, including the exact Core Web Vitals connection mentioned in this session's production-relevance note.
https://web.dev/articles/rendering-performance

**The Odin Project — Introduction to HTML and CSS** (community curriculum, cross-reference) — read this specifically to see the same HTML fundamentals taught in a different voice and sequence; useful as reinforcement, not as a replacement for this session, since (as noted at the top of this document) it defers accessibility and DevTools to a later Odin course this syllabus does not wait for.
https://www.theodinproject.com/lessons/foundations-introduction-to-html-and-css

---

<a name="recap"></a>
## Session Recap & What's Next

You now have a complete, valid, accessible HTML5 document skeleton, written from real understanding rather than a memorized boilerplate you copy-pasted once. You know why the DOCTYPE line exists, why forms bind labels to inputs the way they do, why ARIA is a last resort rather than a first choice, why the browser refuses to paint anything until it has both a DOM and a CSSOM, why a chat UI's message list needs `aria-live` wired correctly from the very first render, and where in DevTools to look when any of this misbehaves.

**Nothing you wrote today is throwaway code.** Session S02 takes this exact skeleton and gives it the CSS cascade, the box model, and CSS custom properties — the same `<header>`, `<nav>`, `<main>`, and form you built today, now with real visual design. By S04, this becomes the complete static chat UI, still with zero JavaScript. The accessibility discipline you practiced today — real buttons, correct label pairing, `aria-live` present from first render — does not get revisited as an afterthought; it's already load-bearing in the very next session's markup, because you built it correctly the first time.

Before you close this session: run through the Quick Check questions in all six topics one more time, out loud, without looking at the answers in the topic text. If any of them make you pause, reread that topic's Core Concept and Technical Depth sections before moving to S02 — this syllabus's day-to-day pace only works if each session's foundation is actually solid, not just completed.
