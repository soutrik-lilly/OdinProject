# Practice & Application Guide — Flexbox vs Grid: The Decision Framework
### Nine Globally-Used Layout Patterns, With the Reasoning For Why Each One Chose Its Tool

**Builds on:** [Session S03 — CSS Layout](../s3/Session_S03_CSS_Layout.md) (Sections 2–4)
**Prerequisites:** S01, S02, and S03 read at least once.
**Grounded against, live, this session** (not recalled from memory — fetched and cross-checked while building this doc):
- MDN — [*Relationship of Grid Layout with Other Layout Methods*](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Relationship_of_grid_layout_with_other_layout_methods) — the official decision framework this whole doc is organized around
- CSS-Tricks — [*A Complete Guide to Grid*](https://css-tricks.com/snippets/css/complete-guide-grid/) — the industry's most-referenced Grid reference, source of two of the patterns below
- [Every Layout](https://every-layout.dev/) — the naming convention (Stack, Cluster, Sidebar, Center, Cover, Reel) that real component libraries (Chakra UI, Braid, and others) actually ship as primitives

**Time budget:** ~3–4 hours.

---

## 0. My Role and the Plan

**My role:** teach you to *decide*, not just to *write* Flexbox or Grid syntax. Anyone can look up `justify-content: center`. The actual industry skill — the thing a senior engineer does in a code review that a junior doesn't — is looking at a UI problem for two seconds and knowing which tool it is, *before* writing a single property. That's this document's only real goal.

**The plan I followed:**
1. Pulled MDN's own stated decision criteria (not folklore) for when Grid beats Flexbox and vice versa.
2. Cross-checked against CSS-Tricks' Grid guide for the two most industry-cited concrete patterns (responsive card grids, named-area dashboards).
3. Cross-checked against Every Layout's pattern vocabulary — because "build me a Cluster" or "this needs a Sidebar pattern" is genuinely how layout gets discussed in real PR reviews and design-system docs, and knowing the *name* alongside the *mechanism* is part of being fluent, not decoration.
4. Selected nine patterns that, between them, cover essentially every layout ticket you'll be handed in a real job: navigation, page shells, card grids, dashboards, comment/chat rows, multi-column "holy grail" layouts, horizontally-scrolling rails, asymmetric editorial grids, and form alignment.
5. Every pattern states, explicitly, **why** it picked Flexbox or Grid — not just "here's the code that works."

---

## 1. The Decision Framework — Learn This Section Cold Before Anything Else

This is the single highest-leverage five minutes in this entire document. Everything below is an *application* of this framework, not a new one per scenario.

### Question 1 — One dimension, or two?

> MDN, verbatim: *"Do we only need to control the layout by row or column? If yes, use flexbox. Do we need to control the layout by row and column? If yes, use grid layout."*

If you can describe the arrangement as **"a line of things"** (even if that line wraps) → Flexbox.
If you can only describe it as **"a grid of things"** — rows *and* columns need to be planned together — → Grid.

### Question 2 — Does the content size drive the layout, or does the layout drive where content goes?

> MDN: *"Flexbox works from the content out... Grid works from the layout in — you create a layout and then place items into it."*

- **Content-out (Flexbox):** you have a variable, unpredictable number of things (chat messages, nav links, tags) and want them spaced out sensibly. The content decides the sizing.
- **Layout-in (Grid):** you already know the *shape* you want (a 3-column dashboard, a 12-column form grid) before you know what goes in each cell.

### Question 3 — The tie-breaker MDN states as a literal warning sign

> MDN: *"If you are using flexbox and find yourself disabling some of the flexibility, you probably need to use CSS grid layout."*

The moment you catch yourself writing `flex-shrink: 0` and explicit pixel widths on every flex child just to force them into neat columns — that's Flexbox fighting a job it was never built for. Stop, and reach for Grid instead.

```
┌───────────────────────────────────────────────────────────┐
│  Q1: One line, or a grid?         Line → FLEXBOX           │
│                                    Grid → CSS GRID           │
│                                                               │
│  Q2: Content-out, or layout-in?    Content-out → FLEXBOX     │
│                                     Layout-in   → CSS GRID    │
│                                                               │
│  Q3: Fighting Flexbox's flex?      Yes → switch to CSS GRID  │
└───────────────────────────────────────────────────────────┘
```

### The Every Layout vocabulary — the names your team will actually use

| Pattern name | Mechanism | One-line description |
|---|---|---|
| **Stack** | Flexbox column + `gap` | Vertical rhythm — consistent spacing between stacked elements |
| **Cluster** | Flexbox row + `flex-wrap: wrap` + `gap` | An unpredictable number of same-ish items that should wrap, not overflow (tags, pills, nav links) |
| **Sidebar** | Flexbox row, one fixed-width child, one `flex: 1` child | A fixed-width column next to a fluid one |
| **Center** | `margin-inline: auto` (single item) or `place-items: center` (Grid) | Constrain width and center |
| **Cover** | Grid or Flexbox column with `min-height: 100dvh` | A hero/full-height section with content pinned top/center/bottom |
| **Reel** | Flexbox row + `overflow-x: auto` + scroll-snap | Horizontally scrolling rail (carousels, story bars) |
| **Grid** (as a named pattern, distinct from the CSS feature) | `repeat(auto-fit, minmax(...))` | Responsive card/tile layout with no media query |

You'll recognize every mechanism in that table — you already learned all of them in S03. What's new here is the **name**, because "let's use a Cluster for the filter chips" is a sentence you will actually hear in a real standup.

---

## 2. Concept Coverage Map

| Pattern | Tool | S03 Section | Every Layout Name |
|---|---|---|---|
| 1. Navbar | Flexbox | §2 | Cluster + Sidebar |
| 2. Sticky-Footer Page Shell | Flexbox | §2 | Stack |
| 3. Responsive Card Grid | Grid | §3 | "Grid" |
| 4. Named-Area Dashboard | Grid | §3, §4 | Cover + Sidebar |
| 5. Media Object (Chat Row) | Flexbox | §2 | Sidebar |
| 6. Holy Grail 3-Column Layout | Grid (outer) + Flexbox (inner) | §3, §4 | — |
| 7. Horizontal Scroll Rail | Flexbox | §2, §5 | Reel |
| 8. Asymmetric "Bento" Grid | Grid | §3 | "Grid" |
| 9. Aligned Form Rows | Grid | §3 | — |

---

## 3. Pattern 1 — The Navigation Bar (Flexbox)

### Real-world context
Every product on the internet has one. It's the single most-repeated layout job in front-end work.

### Objective
Logo pinned left, nav links grouped in the middle, a CTA button pinned right — all in one flex container, no Grid.

### Decision reasoning
**One dimension** (a single horizontal line) and **content-out** (the number of nav links is small and known, but their combined width isn't fixed) → Flexbox, per Question 1 and 2. This is a textbook Cluster/Sidebar hybrid: three distinct groups distributed along one axis.

### Code

```html
<nav class="navbar">
  <div class="logo">BIA</div>
  <ul class="nav-links">
    <li><a href="#">Product</a></li>
    <li><a href="#">Pricing</a></li>
    <li><a href="#">Docs</a></li>
  </ul>
  <button type="button" class="cta">Sign in</button>
</nav>
```

```css
.navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.75rem 1.5rem;
}

.nav-links {
  display: flex;
  gap: 1.5rem;
  list-style: none;
  margin: 0;
  padding: 0;
}
```

### Explain
- `justify-content: space-between` on the *outer* flex container does all the heavy lifting — three children (logo, list, button), pushed to the two ends with the middle one absorbing whatever's between. (S03 §2)
- The nav links are their *own nested flex container* — a Cluster inside a Sidebar-style outer arrangement. Nesting flex containers, not reaching for Grid, is correct here because both layers are genuinely one-dimensional.

### Do This Yourself
Resize the browser narrower until the three links plus the button plus the logo can no longer fit on one line at their natural size. Watch what happens (they shrink, per `flex-wrap`'s `nowrap` default from S03 §2) — then decide, as a real engineer would, whether that's acceptable here or whether this specific navbar needs a mobile hamburger pattern instead (a decision for S04+, not this doc — the point is recognizing the moment you'd need it).

---

## 4. Pattern 2 — The Sticky-Footer Page Shell (Flexbox)

### Real-world context
A page whose content is sometimes short and sometimes long, but the footer must always sit at the bottom of the *viewport* on short pages, and at the bottom of *content* on long pages — never floating mid-screen. One of the most commonly mis-solved layout problems on the web before Flexbox existed.

### Decision reasoning
This is a **column of exactly three regions, stacked** — one dimension. Content-out: `main` should absorb however much space is left, whatever its content height turns out to be. Textbook Stack, with one child told to grow.

### Code

```css
.page {
  display: flex;
  flex-direction: column;
  min-height: 100dvh;   /* dvh, not vh — S03 §5 */
}

main {
  flex: 1;    /* absorbs all leftover vertical space */
}
```

### Explain
- `min-height: 100dvh` guarantees the column is *at least* one full screen tall — so on short pages, the footer is still pushed to the visual bottom of the viewport (S03 §5's `dvh` payoff, applied here for a different reason than the chat shell used it).
- `main { flex: 1; }` is the entire trick: on a short page, `main` stretches to fill the gap; on a long page, `main` simply grows past 100dvh and the whole page scrolls normally, footer following naturally at the true end of content. No media query, no JavaScript, no magic numbers.

### Do This Yourself
Build this with three regions containing one line of placeholder text each. Confirm the footer sits at the bottom of the screen. Then paste 40 paragraphs into `main` and confirm the footer moves to the bottom of *that* content instead, with the whole page now scrolling.

---

## 5. Pattern 3 — The Responsive Card Grid (Grid)

### Real-world context
Product listings, blog post grids, a dashboard's metric tiles — "N cards, however many fit per row, responsive, with zero media queries." CSS-Tricks calls the mechanism below "one of the all-time great CSS tricks," and it is genuinely everywhere in production.

### Decision reasoning
**Two dimensions** — rows *and* columns need to reflow together as the viewport changes — and the number of items is unpredictable, which is exactly the case Grid's `auto-fit`/`auto-fill` machinery exists for. This is also a case where reaching for Flexbox would immediately trigger Question 3's warning sign: you'd need explicit widths and `flex-wrap` fighting to fake what Grid does natively.

### Code

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1rem;
}
```

### Explain
- `repeat(auto-fit, minmax(240px, 1fr))` reads as: "fit as many columns as will hold at least 240px each; whatever's left over, split evenly (`1fr`) among however many columns fit." No breakpoint, no `@media` query, no JavaScript resize listener.
- `auto-fit` vs `auto-fill` — the one subtlety worth knowing by name: `auto-fit` collapses empty leftover tracks so existing cards stretch to fill the row; `auto-fill` leaves them as empty invisible tracks, so cards stay at their `minmax` size even with room to spare. For a card gallery, you almost always want `auto-fit`.

### Do This Yourself
Build a grid of 7 placeholder cards with this rule. Resize the browser from very narrow to very wide and confirm the column count changes on its own — 1 column, then 2, then 3+ — with zero breakpoints written.

---

## 6. Pattern 4 — The Named-Area Dashboard Shell (Grid)

### Real-world context
Nearly every admin panel, analytics tool, and the BIA app itself: header across the top, sidebar down one side, main content, footer across the bottom. CSS-Tricks' own guide uses almost this exact four-region template as its flagship Grid example.

### Decision reasoning
Rows *and* columns, planned together, with a known shape decided before content exists — the purest possible case of MDN's "layout-in" description. This is also the layout where naming areas (instead of counting grid lines) becomes a real maintainability win, not a novelty.

### Code

```css
.dashboard {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 220px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100dvh;
}

.dashboard > header  { grid-area: header; }
.dashboard > nav     { grid-area: sidebar; }
.dashboard > main    { grid-area: main; }
.dashboard > footer  { grid-area: footer; }
```

### Explain
- `grid-template-areas` is a literal ASCII map of the layout — you can *see* the shape in the CSS itself, which is exactly why CSS-Tricks singles it out: it's self-documenting in a way `grid-column: 2 / 4` never is.
- Each area string must form a valid rectangle — you cannot describe an L-shape with `grid-template-areas` (that requires manual line-based placement instead). Knowing this boundary is part of knowing the tool, not a gotcha to memorize separately.
- Compare this directly to S03 §3's `.app-shell` (`grid-template-columns: 260px 1fr` with only 2 explicit regions) — this pattern is the *same mechanism*, extended to four named regions instead of two anonymous ones. Named areas become worth the extra syntax once you're past 2–3 regions.

### Do This Yourself
Delete the `sidebar` row entirely (change the area string's first two rows to `"header header"` / `"main main"`) and confirm the sidebar column collapses out of the layout without touching `grid-template-columns` at all — Grid recalculates around the areas you actually declared.

---

## 7. Pattern 5 — The Media Object / Chat Row (Flexbox)

### Real-world context
An avatar next to a name and message, a comment with a profile picture, a product thumbnail next to its description — this exact shape appears dozens of times in almost every real UI you've ever used, under the classic name "the media object" (coined by Nicole Sullivan, one of the most-cited layout patterns in CSS history).

### Decision reasoning
One dimension (a fixed-width image, an flexible-width text block, side by side) — a Sidebar pattern by another name. Content-out: the text block's height is whatever the message needs; the image never changes size regardless.

### Code

```html
<div class="message-row">
  <img src="/avatar.png" alt="" class="avatar">
  <div class="message-content">
    <strong>Soutrik</strong>
    <p>How do I center this without Grid?</p>
  </div>
</div>
```

```css
.message-row {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
}

.avatar {
  flex-shrink: 0;   /* never let the browser shrink the image */
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

.message-content {
  flex: 1;   /* absorb all remaining width */
}
```

### Explain
- `flex-shrink: 0` on the avatar is doing real work here, not decoration: without it, a long unbroken message could visually compress the avatar under space pressure — exactly the scenario S03 §2 names it for.
- `align-items: flex-start` (not the `stretch` default) is the correct choice specifically because a multi-line message shouldn't force the avatar to stretch tall to match — it should stay pinned to the top, its natural size.

### Do This Yourself
Paste a very long, multi-line message into `.message-content`. Confirm the avatar stays exactly 40×40px at the top of the row, rather than stretching or shrinking.

---

## 8. Pattern 6 — The "Holy Grail" Three-Column Layout (Grid outer, Flexbox inner)

### Real-world context
Header, footer, and three middle columns (left nav, main content, right rail) — the layout that was infamous for requiring float/table hacks before Grid existed, hence the "holy grail" nickname developers gave it.

### Decision reasoning
The outer shape (header/middle-row/footer, with the middle row split into three columns) is genuinely two-dimensional → Grid. Each individual column's *internal* content, though, is usually a simple vertical stack → Flexbox nested inside, exactly the composition principle S03 §3 names directly ("Grid for the shell, Flexbox for what's inside a cell").

### Code

```css
.holy-grail {
  display: grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header header header"
    "nav main aside"
    "footer footer footer";
  min-height: 100dvh;
}

.holy-grail nav    { grid-area: nav; }
.holy-grail main   { grid-area: main; }
.holy-grail aside  { grid-area: aside; }

.holy-grail nav {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
```

### Explain
- One `grid-template-areas` declaration replaces what used to take an entire blog post's worth of float/clearfix workarounds in pre-2017 CSS — worth knowing as production-relevant history, not trivia: if you ever inherit a legacy codebase using floats for this, you're looking at a Grid refactor opportunity, not a design problem.
- The nested `display: flex; flex-direction: column;` inside `nav` is a Stack — the internal nav links only need one-dimensional stacking, so nesting Grid-inside-Grid here would be reaching for more power than the sub-problem needs (the same discipline S03 §3 explicitly warns against: "using Grid everywhere because it's more powerful is not the right instinct").

### Do This Yourself
Swap `grid-template-columns: 200px 1fr 200px` for `grid-template-columns: 1fr 3fr 1fr` and predict, before reloading, whether the middle column gets wider or narrower relative to the sides on a 1000px-wide viewport. *(Wider — proportionally 600px vs 200px each, versus the fixed 200px sides plus flexible middle in the original. This is the `fr`-unit arithmetic from S03 §3, applied to three tracks instead of two.)*

---

## 9. Pattern 7 — The Horizontal Scroll Rail (Flexbox — the "Reel")

### Real-world context
Netflix's row of thumbnails, an e-commerce "related products" carousel, Instagram/WhatsApp-style story bars. All the same underlying pattern, and all built the same simple way in modern CSS — no JavaScript carousel library required for the base case.

### Decision reasoning
One dimension, explicitly *not* wrapping — this is a case where you actively want `flex-wrap`'s default `nowrap` behavior (S03 §2) instead of fighting it, combined with horizontal scrolling instead of shrinking.

### Code

```css
.reel {
  display: flex;
  gap: 1rem;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  padding-bottom: 0.5rem;
}

.reel > * {
  flex-shrink: 0;
  width: 200px;
  scroll-snap-align: start;
}
```

### Explain
- `overflow-x: auto` plus the default `nowrap` is the entire mechanism — items are never allowed to wrap or shrink, so once they exceed the container's width, the container simply becomes scrollable instead.
- `scroll-snap-type` + `scroll-snap-align` are new vocabulary (not covered in S03) but built on nothing except what you already know: they make the browser "snap" the scroll position to align with an item's edge after a swipe/scroll gesture, entirely native, no JS.
- Watch the `overflow` interaction: if this `.reel` ever needs to live inside a `position: sticky` ancestor, S03 §4's central gotcha applies directly — `overflow-x: auto` here creates a new scrolling context, so don't nest a sticky element that expects to stick to the *page* inside this rail.

### Do This Yourself
Build this with 10 placeholder 200px-wide boxes. Confirm the row scrolls horizontally instead of wrapping to new lines or shrinking every box — the direct, deliberate opposite of Pattern 1's navbar, where shrinking-under-pressure was the (undesirable) default you were warned about.

---

## 10. Pattern 8 — The Asymmetric "Bento" Grid (Grid, with spanning)

### Real-world context
Apple's product marketing pages, Pinterest-style boards, a "featured + 4 smaller" hero section on a homepage. Genuinely one of the most requested modern layout styles in 2024–2026 design briefs — irregular tile sizes in an otherwise organized grid.

### Decision reasoning
Two dimensions, and specifically **irregular spans** — some cells are bigger than others by design, which only Grid can express (`repeat(auto-fit, minmax())` alone gives you *uniform* responsive tiles; a Bento layout needs individual cells to deliberately break that uniformity).

### Code

```css
.bento {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(2, 200px);
  gap: 1rem;
}

.bento .featured {
  grid-column: span 2;
  grid-row: span 2;
}

.bento .wide {
  grid-column: span 2;
}
```

### Explain
- `grid-column: span 2` tells one specific child "occupy two columns starting from wherever the auto-placement algorithm puts you" — this is the mechanism, and it composes with the *implicit* placement Grid already does for every other, non-spanning child.
- This is the direct, practical answer to "how do sites build those uneven hero-plus-thumbnails sections" — it's not a special layout mode, it's this one property applied selectively.

### Do This Yourself
Build a 6-item bento grid with one `.featured` (2×2) item and confirm the remaining 5 items auto-place themselves around it without you manually specifying their positions — only the "special" cell needed an explicit span.

---

## 11. Pattern 9 — Aligned Form Rows (Grid) — the Case Flexbox Genuinely Can't Do Well

### Real-world context
A settings form with several label/input pairs stacked vertically, where every label column should line up to the same width — a genuinely common design request ("make the labels align") that trips people up specifically because they reach for Flexbox first.

### Decision reasoning
This is the sharpest possible illustration of Question 3's warning sign. With Flexbox, each label/input pair is its *own separate flex container* — so there is no way for one row's label width to influence another row's label width; you'd have to measure the longest label manually and hard-code a `width`. Grid solves this natively: **columns are shared across every row of the grid**, by definition.

### Code

```html
<form class="aligned-form">
  <label for="name">Name</label>
  <input id="name" type="text">

  <label for="email-addr">Email address</label>
  <input id="email-addr" type="email">

  <label for="msg">Message</label>
  <textarea id="msg"></textarea>
</form>
```

```css
.aligned-form {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 0.75rem 1rem;
  align-items: center;
}
```

### Explain
- Because `.aligned-form` is **one single grid**, not three separate rows, `grid-template-columns: auto 1fr` means "the first column is exactly as wide as its widest label, shared across every row" — "Email address" and "Name" both belong to the *same* column, so the column width is computed once, from the widest occupant, and every row respects it.
- This is the exact case where Flexbox would force you to either manually set a `width` on every label (fragile — breaks the moment copy changes) or restructure into a Grid anyway. Recognizing this *before* writing the Flexbox version that later needs a rewrite is the actual point of Question 3.

### Do This Yourself
Change "Email address" to something much longer ("Preferred contact email address"). Reload and confirm the *entire first column* — including "Name" and "Message"'s labels — widens together, automatically, with zero CSS changes required.

---

## 12. Quick-Reference: Signal → Tool

| If the ticket says... | Reach for | Why |
|---|---|---|
| "row of buttons/links/tags" | Flexbox | 1D, content-out |
| "space these evenly" | Flexbox `justify-content` | MDN's own stated ideal Flexbox use case |
| "N cards, responsive, no breakpoints" | Grid `auto-fit`/`minmax` | 2D + unpredictable count |
| "header/sidebar/main/footer shell" | Grid `grid-template-areas` | 2D, layout-in, known shape |
| "avatar next to text" | Flexbox | 1D Sidebar/media object |
| "scrolling row of items, no wrap" | Flexbox + `overflow-x` | 1D, deliberately non-wrapping |
| "uneven tile sizes, one big + several small" | Grid + `span` | 2D + irregular spans |
| "align these labels/columns across rows" | Grid | Shared column tracks across siblings — Flexbox structurally can't do this |
| "I keep fighting flex-wrap/flex-shrink to force a grid shape" | **Switch to Grid** | MDN's literal warning sign (Question 3) |

---

## 13. Common Pitfalls (Decision-Level, Not Syntax-Level)

### Pitfall 1 — Reaching for Grid everywhere "because it's more powerful"

**SYMPTOM:** Simple, genuinely one-dimensional rows (a button group, a tag list) get built with `display: grid; grid-auto-flow: column;` instead of Flexbox — it works, but every teammate now has to mentally translate Grid's heavier vocabulary for a job Flexbox states more directly.

**WHY:** S03 §3 names this directly: dimensionality should match the tool, not developer preference or habit.

**FIX:** Ask Question 1 first, every time, before writing `display:`.

### Pitfall 2 — Fighting Flexbox instead of switching (MDN's named anti-pattern)

**SYMPTOM:** A "card row" built in Flexbox needs `flex-basis` percentage math, `flex-wrap`, and per-breakpoint width overrides just to approximate what `repeat(auto-fit, minmax())` does in one line.

**WHY:** This is MDN's Question 3, verbatim — disabling Flexbox's own flexibility to force a grid shape is the signal, not a valid final approach.

**FIX:** The moment you write a second `flex-basis` override for "how many fit per row," stop and rewrite as Grid.

### Pitfall 3 — Using `auto-fill` when you meant `auto-fit` (or vice versa)

**SYMPTOM:** A card grid with only 3 items, on a wide screen, leaves large empty gaps to the right instead of the 3 cards stretching to fill the row.

**WHY:** `auto-fill` preserves empty tracks at their `minmax` size even when there's nothing to put in them; `auto-fit` collapses those empty tracks, letting existing items grow into the freed space.

**FIX:** For a "cards that should fill available width" gallery, use `auto-fit`. Reserve `auto-fill` for the rarer case where you specifically want consistent tile sizing regardless of item count (e.g., a fixed-size swatch palette).

---

## 14. Zero-to-Hero Checklist — Decision Fluency

- [ ] Given any layout ticket, I can answer "one dimension or two?" in under five seconds
- [ ] I can state MDN's content-out vs layout-in distinction from memory
- [ ] I recognize the "fighting Flexbox's flexibility" warning sign in my own code, live, as I write it — not after a code review flags it
- [ ] I can name at least four Every Layout patterns (Stack, Cluster, Sidebar, Reel) and their mechanism without looking
- [ ] I know why form-label alignment is a genuinely Grid-only problem, not a preference
- [ ] I can explain `auto-fit` vs `auto-fill` and pick correctly between them from a one-sentence design request
- [ ] I default to Grid-outer/Flexbox-inner composition for any app shell, and can say why in terms of dimensionality per sub-problem

If any box is unchecked, rebuild that pattern from this doc's section, from memory, before moving on — this is the exact "explain it back to yourself" discipline S01–S03 already trained you to use.

---

## 15. Further Reading

- MDN — [Relationship of Grid Layout with Other Layout Methods](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Relationship_of_grid_layout_with_other_layout_methods) — the decision framework Section 1 is built on
- CSS-Tricks — [A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/) — source of Patterns 3 and 4
- CSS-Tricks — [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) — already bookmarked in S02/S03
- Every Layout — [every-layout.dev](https://every-layout.dev/) — the pattern-naming vocabulary used throughout Section 1
- MDN — [`repeat()`](https://developer.mozilla.org/en-US/docs/Web/CSS/repeat) and [`minmax()`](https://developer.mozilla.org/en-US/docs/Web/CSS/minmax) — full syntax reference for Pattern 3
- MDN — [`scroll-snap-type`](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-type) — full reference for Pattern 7's Reel mechanism

## 16. Bridge Forward

Session S04 is pure integration — no new theory. Every one of these nine patterns is a direct candidate for pieces of that build: the BIA chat UI's outer shell is Pattern 6 (Holy Grail) simplified to two columns instead of three, its sidebar history list is Pattern 2's Stack, and its suggested-action chips are Pattern 7's Reel or Pattern 1's Cluster depending on whether they should scroll or wrap. Walking into S04 already able to *name* which pattern each piece of that UI is, before writing the CSS, is the entire payoff of this document.
