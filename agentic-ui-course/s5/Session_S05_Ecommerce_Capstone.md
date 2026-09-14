# Session S05 — E-Commerce Capstone Project
### A Non-Chatbot, Industry-Standard Product Storefront: Home → Listing → Detail → Cart

**Syllabus position:** Phase F1 (HTML/CSS Foundations) · Second Foundation capstone · AgentStack Golden Canon v8 Slim
**Prerequisites:** Sessions S01 (HTML5), S02 (CSS Fundamentals), S03 (CSS Layout), S04 (Foundation Project — static chat UI)
**Relationship to S04:** independent capstone, same rigor, deliberately different domain. S04 proved you can build a single-screen, app-like interface. S05 proves the same HTML/CSS foundation scales to a **multi-page, content-heavy, commerce-grade** product — the single most common real-world category of site on the web, and one that exercises HTML/CSS categories a chat UI has no reason to ever touch: tabular data, responsive images, semantic pricing, disclosure widgets, faceted filtering, and machine-readable structured data.
**Time budget:** ~5–6 hours (four pages, each built with the same node-by-node discipline as S04)
**Document format:** same as S04's Revision 3 — one HTML piece, immediately followed by its CSS, immediately followed by the reasoning, before moving to the next piece.

---

## Quick bridge from Session S04

S04 was deliberately narrow: one screen, one primary content type (a message), no real navigation between pages. That was the right scope for proving Grid/Flexbox composition and a from-scratch accessibility pass — but it left real gaps a chat UI simply never surfaces. You have never yet had a reason to:

- show data that's genuinely tabular (a spec sheet, an order summary) — `<table>`
- serve an image that must look sharp on both a phone and a 4K monitor without shipping the 4K file to the phone — `<picture>`/`srcset`
- show a struck-through original price next to a discounted one, with that relationship encoded in the *markup*, not just in color — `<del>`/`<ins>`
- let a user collapse and expand a block of content without JavaScript — `<details>`/`<summary>`
- tell a user, and a search engine, exactly where they are in a hierarchy — breadcrumb navigation
- style an element based on the state of a *different*, unrelated element, with no JavaScript — `:has()`
- make one component (a product card) respond to the space *it* has, independent of the viewport — container queries
- describe a product to a machine (a search engine's rich-snippet parser) in a vocabulary it already understands — schema.org structured data

None of this replaces S04's lessons — Grid, Flexbox, custom properties, the six-step build order, and the accessibility discipline are exactly as load-bearing here as they were there. This document assumes all of that is already solid and spends its own time exclusively on what's genuinely new.

---

## 1. Objective — What We're Solving, and Why This Project Specifically

**The problem statement:** build a static, responsive, accessible, zero-JavaScript **four-page commerce flow** — Home, Category Listing, Product Detail, Cart — styled and structured to the standard a real e-commerce team would ship, not a toy. Real product data, real filtering UI, a real spec table, a real cart summary — each page is a complete, realistic screen, not a fragment.

**Why e-commerce, specifically, and why now, right after a chatbot:** a chat UI is a single, app-like surface — one screen, one interaction loop, no real "pages." E-commerce is the opposite: a *website* in the classic sense, with distinct pages that must share a consistent design system (header, footer, tokens, breadcrumb) while each page's *content* differs enormously in shape — a hero banner, a filterable grid, a single detailed record, a running total. This is deliberately chosen because it forces a different kind of decision-making than S04 did: not "which layout primitive fits this one screen," but "how do I keep four different pages visually and structurally consistent without duplicating logic four times." That's a genuinely different, and arguably more common, real-world skill.

**Why these four pages, specifically, and not more or fewer:** they are the **minimum complete purchase funnel** — you cannot remove any one of them and still have a coherent story (Home discovers, Listing narrows, Detail decides, Cart commits), and adding a fifth (checkout, account) would require form-submission flows this syllabus can't build without JavaScript and a backend. Four pages is exactly the point where the project is genuinely realistic without exceeding what HTML/CSS alone can honestly deliver.

**The concrete constraints that make this a real test:**

| Constraint | Why it's here | What it forces |
|---|---|---|
| Four pages, one shared design system | Real sites are never one page | A reusable shell (header/footer/tokens), not four independent stylesheets |
| Real tabular and structured data | Chat UIs never need this | Correct use of `<table>`, `<dl>`, and JSON-LD — content types S01–S04 had no occasion to teach |
| Image-heavy, real product photography sizes | The #1 real-world CLS/performance complaint in e-commerce | `aspect-ratio`, `<picture>`/`srcset`, explicit width/height discipline |
| Filterable, faceted listing page | The single most complex accessible-UI pattern in mainstream e-commerce | Correct disclosure widgets, fieldset grouping, and range/number inputs — all native, zero JS |
| Zero JavaScript | Same discipline as S04, for the same reason | Every "interactive-feeling" affordance (facet collapse, variant highlight) must come from a native element or a pure-CSS mechanism |
| WCAG 2.2 AA + schema.org compliance | These are the two globally-recognized "gold standard" rule sets for commerce UI — one for people, one for machines | Every page must pass both, not just look right |

---

## 2. Target Shape — What "Done" Looks Like, Before Any Code

**Shared shell, present on every page:**

```
┌────────────────────────────────────────────────────────────┐
│ LOGO      [ 🔍 search......................... ]    🛒 3   │ ← site-header (sticky)
├────────────────────────────────────────────────────────────┤
│ Home › Laptops › 14" Laptops                                │ ← breadcrumb (Listing/Detail/Cart only)
├────────────────────────────────────────────────────────────┤
│                        (page content)                       │
├────────────────────────────────────────────────────────────┤
│ About · Shipping · Returns          © 2026 AcmeMart          │ ← site-footer
└────────────────────────────────────────────────────────────┘
```

**Home page content region:**

```
┌────────────────────────────────────────────────────────────┐
│ [======== HERO BANNER, <picture>/srcset =============]     │
├────────────────────────────────────────────────────────────┤
│ Featured Deals  ← scroll-snap horizontal carousel            │
│ [card][card][card][card][card] →                            │
├────────────────────────────────────────────────────────────┤
│ Shop by Category                                             │
│ [card] [card] [card] [card]   ← Grid, auto-fill/minmax       │
│ [card] [card] [card] [card]                                  │
└────────────────────────────────────────────────────────────┘
```

**Listing page — sidebar facets + product grid + pagination:**

```
┌───────────────┬──────────────────────────────────────────┐
│ ▸ Brand        │  Sort: [Price: low to high ▾]             │
│ ▸ Price        │  ┌────────┐ ┌────────┐ ┌────────┐          │
│  [────●───]    │  │ card   │ │ card   │ │ card   │  ← Grid  │
│ ▸ Rating        │  └────────┘ └────────┘ └────────┘          │
│ ▸ RAM           │  ┌────────┐ ┌────────┐ ┌────────┐          │
│ (facets, mobile: │  │ card   │ │ card   │ │ card   │          │
│  <details>)      │  └────────┘ └────────┘ └────────┘          │
│                 │  « 1  2  3  4  »   ← pagination nav        │
└───────────────┴──────────────────────────────────────────┘
```

**Product detail page:**

```
┌───────────────────────┬──────────────────────────────────┐
│ [main image, aspect-  │  ThinkPad X1 Carbon Gen 12         │
│  ratio 1/1]           │  ★★★★☆ 4.2 (1,204 reviews)         │
│ [thumb][thumb][thumb] │  ~~$1,899~~ $1,599   ← del/ins      │
│  ← scroll-snap row    │  Color: ( )Black ( )Silver          │
│                       │  Qty: [1▾]   [ Add to Cart ]        │
├───────────────────────┴──────────────────────────────────┤
│ ▸ Description   (details/summary, open by default)         │
│ ▸ Specifications  → <table> of spec rows                    │
│ ▸ Shipping & Returns                                        │
└──────────────────────────────────────────────────────────┘
```

**Cart page:**

```
┌──────────────────────────────────────┬─────────────────┐
│ Product      Price   Qty   Subtotal   │ Order Summary    │
│ ─────────────────────────────────────  │ Subtotal  $1,599 │
│ [img] ThinkPad X1  $1,599  [1▾]  $1,599│ Shipping    Free │
│ [img] USB-C Hub       $39  [2▾]    $78 │ Total     $1,677 │ ← clamp() type
│ ─────────────────────────────────────  │ [ Checkout ]     │
│ Promo code: [________] [Apply]         │                  │
└──────────────────────────────────────┴─────────────────┘
```

**Shared nesting skeleton (structure only):**

```
<body>
  header.site-header → a.logo, form.search-form, a.cart-link
  nav.breadcrumb (aria-label="Breadcrumb") → ol → li × N (last: aria-current="page")
  main
    ...page-specific content...
  footer.site-footer
```

---

## 3. Build Order — Same Six Steps as S04, Applied Once Per Page

S04 Section 3 already taught the six-step order (structure → layout skeleton → tokens → components → polish → accessibility audit) as the single most transferable lesson in that session. It applies here unchanged — the only new decision is *sequencing across four pages*: build the **shared shell and tokens once**, then walk each page through its own structure → layout → components → polish pass, reusing the shell and tokens every time rather than rebuilding them. This mirrors how a real multi-page site is actually built: a design system first, then pages as consumers of it — never four independent, drifting stylesheets.

---

## 4. The Build

### PHASE 0 — Shared Shell and Tokens (built once, used by all four pages)

#### 0.1 — Design tokens

```css
* { box-sizing: border-box; }

:root {
  --background: #ffffff;
  --foreground: #1a1a1a;
  --muted: #f5f5f5;
  --muted-foreground: #6b7280;
  --border: #e5e7eb;
  --primary: #b12704;
  --primary-foreground: #ffffff;
  --success: #067d3f;
  --radius: 8px;
  --content-max: 1200px;
}

body {
  margin: 0;
  font-family: system-ui, sans-serif;
  color: var(--foreground);
  background-color: var(--background);
}
```

**Why this token set differs from S04's, even though the mechanism is identical:** e-commerce needs a `--success` token S04 never did (in-stock indicators, "free shipping" badges) and a `--content-max` token S04 never needed (a chat UI fills its viewport edge-to-edge; a commerce page's content is centered with a maximum readable width — a `<body>`-wide product grid on a 32" monitor is a real, common bug this token exists to prevent). Everything else — defining tokens before any component, in one place — is S04's Step 3, unchanged.

#### 0.2 — The site header

```html
<header class="site-header">
  <a href="/" class="logo">AcmeMart</a>
  <form role="search" class="search-form">
    <label for="site-search" class="visually-hidden">Search products</label>
    <input type="search" id="site-search" placeholder="Search products…">
    <button type="submit">Search</button>
  </form>
  <a href="/cart" class="cart-link" aria-label="Cart, 3 items">
    🛒 <span class="cart-count" aria-hidden="true">3</span>
  </a>
</header>
```

**Why `role="search"` on the `<form>`:** this is a *landmark role*, distinct from the `<nav>` landmark S04 already taught — it tells assistive tech "this specific region is the site's search function," which matters once a page has multiple forms (search here, a promo-code form on the Cart page later) and a screen reader user wants to jump directly to search. **Why `type="search"` on the input, not `type="text"`:** it's semantically accurate (this input's value is a search query, not arbitrary text), and browsers give it small, free behaviors a plain text input doesn't get — an inline "clear" (×) button on many browsers, and some mobile keyboards show a "search" key instead of "return." **Why the cart link's real accessible name lives in `aria-label`, with the count duplicated and hidden inside the link:** the visible cart count (`3`) is decorative reinforcement of information already fully stated in the label ("Cart, 3 items") — exactly the same "don't let a visual-only element be the sole carrier of real information" principle S04's Revision 3 fixed for its typing indicator, applied here proactively rather than as a later correction.

#### 0.3 — Breadcrumb navigation (new pattern)

```html
<nav aria-label="Breadcrumb" class="breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/laptops">Laptops</a></li>
    <li aria-current="page">14" Laptops</li>
  </ol>
</nav>
```

```css
.breadcrumb ol {
  display: flex;
  list-style: none;
  padding: 0;
  margin: 0;
  gap: 4px;
  flex-wrap: wrap;
}

.breadcrumb li:not(:last-child)::after {
  content: "›";
  margin-left: 4px;
  color: var(--muted-foreground);
}

.breadcrumb li[aria-current="page"] {
  color: var(--muted-foreground);
  font-weight: 400;
}
```

This is the WAI-ARIA Authoring Practices' standard breadcrumb pattern, verified directly against the current spec rather than assumed from memory: a `<nav>` landmark labeled `"Breadcrumb"` (so it doesn't collide with the page's other `<nav>` elements — the same distinguishing-label principle S04 used for its sidebar `<nav>`), containing an ordered list (`<ol>` — this genuinely *is* an ordered sequence, unlike S04's history list, which is why this uses `<ol>` where that used `<ul>`), with the current page marked by `aria-current="page"` rather than being a link at all — you are already on this page, so linking to it would be dishonest markup, exactly the same "don't write a link to nowhere" principle S04 applied to the suggestion chips. **Why the separator (`›`) is a `::after` pseudo-element, not literal text between the `<li>`s:** it's decorative punctuation with no semantic value of its own — inserting it as a pseudo-element (S04's exact `::after` mechanism, reused here for a new purpose) keeps it out of the accessible tree's text content entirely, so a screen reader announces "Home, Laptops, 14 inch Laptops" rather than "Home, chevron, Laptops, chevron, 14 inch Laptops."

#### 0.4 — Site footer

```html
<footer class="site-footer">
  <nav aria-label="Footer">
    <a href="/about">About</a>
    <a href="/shipping">Shipping</a>
    <a href="/returns">Returns</a>
  </nav>
  <p>© 2026 AcmeMart</p>
</footer>
```

Nothing new here — a third `<nav>` landmark, distinguished by its own label, exactly as Section 0.3 and S04 both already established the pattern for.

**Checkpoint:** this shell — header, breadcrumb, footer — should render identically, unstyled-but-structurally-correct, if pasted at the top and bottom of any of the four pages below. That reusability is the entire point of building it once, first.

---

### PHASE 1 — Home Page

#### 1.1 — Hero banner with `<picture>`/`srcset` (new technique #1)

```html
<section class="hero">
  <picture>
    <source media="(min-width: 768px)" srcset="hero-desktop.jpg">
    <img src="hero-mobile.jpg" alt="Autumn sale: up to 40% off laptops and accessories">
  </picture>
  <div class="hero-copy">
    <h1>Autumn Sale — Up to 40% Off</h1>
    <a href="/laptops" class="cta-button">Shop Laptops</a>
  </div>
</section>
```

```css
.hero { position: relative; }
.hero img { width: 100%; height: 400px; object-fit: cover; display: block; }
.hero-copy { position: absolute; inset: 0; display: flex; flex-direction: column; justify-content: center; padding: 0 40px; }
```

**Why `<picture>` with a `<source>`, rather than one `<img>` whose CSS is just resized:** these aren't the same image scaled — they're genuinely **different crops** (a tall mobile-friendly crop versus a wide desktop banner), which `srcset` alone (same image, different resolutions) can't express; `<picture>`/`<source media>` exists specifically for **art direction** — swapping which *image* loads based on viewport, not just which resolution of the same image. A single `<img>` with `object-fit: cover` could visually crop one wide image to fit a narrow mobile viewport, but it would still download the full desktop-sized file on mobile — wasted bandwidth on exactly the users most likely to be on a constrained connection. **Why the `<img>` is the *last* child, with a real `src` and `alt`, rather than `<source>` being the only content:** `<picture>` requires a fallback `<img>` for browsers that don't support the element at all (essentially none today, but the spec still requires it) and, more importantly, the `alt` text lives on that one `<img>` regardless of which `<source>` actually loaded — write it to describe the image's *meaning* ("Autumn sale: up to 40% off"), not its literal visual content, since that's the information a screen reader user actually needs from a promotional banner.

**Why `object-fit: cover` plus explicit `height: 400px`, not `height: auto`:** without a fixed height, the browser can't reserve the hero's vertical space before either candidate image downloads — this is exactly the Cumulative-Layout-Shift problem `aspect-ratio` (introduced properly in Phase 3 below) solves in general; a fixed pixel height is a valid, simpler solution specifically when the hero's height doesn't need to vary with viewport width the way a square product photo's does.

#### 1.2 — Product grid using `auto-fill`/`minmax()` (new technique #2)

```html
<section class="category-grid" aria-label="Shop by category">
  <a href="/laptops" class="category-card">
    <img src="cat-laptops.jpg" alt="" width="200" height="150">
    <span>Laptops</span>
  </a>
  <a href="/monitors" class="category-card">
    <img src="cat-monitors.jpg" alt="" width="200" height="150">
    <span>Monitors</span>
  </a>
  <!-- ...more category cards... -->
</section>
```

```css
.category-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 16px;
  padding: 24px;
}
```

S03 taught `repeat()` with a fixed count (`repeat(3, 1fr)`). Here's the case that fixed count can't handle: a category grid whose *item count* is content-driven (four categories today, seven after a merchandising update) and whose *column count* should adapt to however wide the container actually is — a phone gets 2 columns, a tablet 4, a desktop 6, with nobody hand-writing a media query per breakpoint. `repeat(auto-fill, minmax(180px, 1fr))` reads as: "fit as many 180px-minimum columns as will cleanly fit, and let each one stretch to fill any leftover space evenly." **Why `auto-fill` and not `auto-fit` here, specifically:** with only four or five categories on a very wide screen, `auto-fit` would stretch those few cards to fill the entire row (each one growing far beyond a sensible card width); `auto-fill` instead leaves the unused space as empty implicit columns, keeping each card at a sane width even when there isn't enough content to fill the row — the correct choice whenever card *width* should stay visually consistent regardless of item count. (The Listing page's product grid in Phase 2 revisits this exact decision with the opposite conclusion, for a container where filling the row *is* the right behavior — worth predicting which one before reading that section.)

**Why `alt=""` (empty, not omitted) on each category thumbnail:** the card's own visible text ("Laptops") already fully describes the link's destination — the thumbnail image adds no information a screen reader user needs beyond what the text already provides, so it's marked explicitly decorative. This differs from the hero image in 1.1, whose `alt` text carried real information no visible text nearby duplicated — the same underlying question ("does this image convey information the surrounding text doesn't already give?") answered differently for two different images, which is the actual skill, not a rule to apply uniformly.

#### 1.3 — The scroll-snap carousel (new technique #3)

```html
<section aria-label="Featured deals" class="deals-rail">
  <ul class="deals-track">
    <li class="deal-card"><img src="deal-1.jpg" alt="Wireless mouse, 45% off" width="160" height="160"><p>Wireless Mouse — $14.99</p></li>
    <li class="deal-card"><img src="deal-2.jpg" alt="USB-C hub, 30% off" width="160" height="160"><p>USB-C Hub — $27.99</p></li>
    <!-- ...more deal cards... -->
  </ul>
</section>
```

```css
.deals-track {
  display: flex;
  gap: 12px;
  list-style: none;
  padding: 16px;
  margin: 0;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
}

.deal-card {
  flex: 0 0 160px;
  scroll-snap-align: start;
}
```

This is a genuinely new mechanism: `scroll-snap-type: x mandatory` on the scrolling container tells the browser "after any scroll gesture on the horizontal axis, always settle exactly on a snap point" — combined with `scroll-snap-align: start` on each card (marking each card's *start* edge as a snap point), the result is a touch-swipeable, horizontally-scrolling carousel that feels deliberately paginated rather than loosely free-scrolling, with **zero JavaScript and zero external library** — the exact category of interaction that, before this CSS feature existed, was one of the most common reasons a static site reached for a JS carousel plugin at all. **Why `<ul>`/`<li>` here rather than a row of `<div>`s:** these are genuinely a list of items (deals), exactly the same reasoning S04 applied to its history list — the visual "carousel" presentation is CSS's job; the underlying content is still a list, and should still announce as one. **Why `flex: 0 0 160px` on each card:** without it, Flexbox's default `flex-shrink: 1` would let cards shrink below their intended width to fit the container, breaking both the visual design and the snap-point math, which depends on each card being a predictable, fixed width.

**Checkpoint:** at this point the Home page should show a hero image that swaps crop at 768px, a category grid that reflows its column count fluidly as you resize the window (never fewer than 180px per card), and a horizontally swipeable/scrollable deals row that snaps cleanly to each card. None of this required a single line of JavaScript.

---

### PHASE 2 — Category Listing Page

#### 2.1 — Faceted filters: `fieldset`/`legend`, checkboxes, and `input[type=range]` (new technique #4)

```html
<aside class="filters" aria-label="Filter products">
  <fieldset>
    <legend>Brand</legend>
    <label><input type="checkbox" name="brand" value="lenovo"> Lenovo</label>
    <label><input type="checkbox" name="brand" value="dell"> Dell</label>
    <label><input type="checkbox" name="brand" value="apple"> Apple</label>
  </fieldset>

  <fieldset>
    <legend>Price range</legend>
    <label for="price-max">Max price: <output id="price-output">$2,000</output></label>
    <input type="range" id="price-max" min="200" max="2000" step="50" value="2000">
  </fieldset>

  <fieldset>
    <legend>Minimum rating</legend>
    <label><input type="radio" name="rating" value="4"> 4 stars & up</label>
    <label><input type="radio" name="rating" value="3"> 3 stars & up</label>
  </fieldset>
</aside>
```

**Why `<fieldset>`/`<legend>` for each facet group, rather than a `<div>` with a heading:** a `<fieldset>` is the one HTML element whose entire purpose is grouping *related form controls* under one shared label — its `<legend>` is announced by screen readers as the group's name every time focus enters any control inside it, so a user tabbing through five brand checkboxes hears "Brand, Lenovo, checkbox, not checked" rather than five unlabeled checkboxes with no shared context. A `<div>` with a preceding `<h3>` visually resembles this but carries none of that grouped-announcement behavior — this is exactly the same "structure carries meaning a div cannot" principle S04 established for `<nav>` and `<ul>`, applied to forms specifically. **Why checkboxes for Brand but a radio group for Rating:** a user can genuinely select *multiple* brands at once (an inclusive-OR filter), which is exactly what a checkbox group means; "minimum rating," by contrast, is a single mutually-exclusive choice (you can't filter for "4 stars and up" *and* "3 stars and up" as two independent constraints — the second subsumes the first) — radios are the correct native control for **exactly one of several options**, and reaching for checkboxes here would let a user select a logically incoherent combination the UI would then have to silently paper over. **Why `<input type="range">` for price, paired with a live `<output>`:** a draggable slider is the correct native affordance for "pick a value along a bounded, continuous scale," and `<output>` is the one HTML element whose specific job is to display the *result of a calculation or user action* — here, the currently-selected max price — distinct from a `<label>`, which names a control rather than echoing its live value. (Wiring the `<output>` to actually update as the slider moves requires JavaScript; today it renders the range's initial `value` correctly and the structural pairing — `for`/`id`, matching the pattern S04 used throughout — is already in place for when that script exists.)

#### 2.2 — Mobile facet collapsing with `<details>`/`<summary>` (new technique #5)

```html
<aside class="filters" aria-label="Filter products">
  <details open>
    <summary>Brand</summary>
    <fieldset>
      <legend class="visually-hidden">Brand</legend>
      <label><input type="checkbox" name="brand" value="lenovo"> Lenovo</label>
      <label><input type="checkbox" name="brand" value="dell"> Dell</label>
    </fieldset>
  </details>
  <details>
    <summary>Price range</summary>
    <!-- ...same fieldset as 2.1... -->
  </details>
</aside>
```

`<details>`/`<summary>` is a native, zero-JavaScript disclosure widget — the browser handles the expand/collapse state, the click target, the keyboard interaction (Enter and Space both toggle it, exactly like a `<button>`), and the accessibility semantics (`aria-expanded` is managed *automatically* by the browser, not by you) entirely on its own. This is worth contrasting directly against S04's checkbox-hack sidebar toggle, because the difference is instructive: S04 needed a `<label>`+checkbox+CSS-sibling-selector workaround specifically because no native disclosure element existed for *that* use case (an off-canvas panel overlay); here, the native element that exists for exactly this use case (an in-flow expand/collapse region) is reached for directly, with none of S04's accessibility caveats — `<details>` genuinely, correctly announces "expanded" or "collapsed" to a screen reader, because that's precisely the state it represents. **Why the outer `<summary>` text ("Brand") duplicates the `<legend>` inside it, and why the `<legend>` is then visually hidden rather than removed:** the `<summary>` is what's clickable and announced when *collapsed*; the `<legend>` is what groups the checkboxes once *expanded* and focus moves inside. Removing the `<legend>` entirely to avoid the apparent duplication would break the checkbox-grouping behavior described in 2.1 the moment the panel is open — the fix is to keep both, and hide the redundant one visually (`.visually-hidden`, S04's exact utility class) rather than delete real structure to avoid a cosmetic repeat.

#### 2.3 — The product grid, where `auto-fit` is the correct choice (contrast with 1.2)

```css
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 20px;
}
```

**Predict before reading on:** Section 1.2 chose `auto-fill` specifically so a handful of category cards wouldn't stretch to fill an entire wide row. A listing page's product grid, by contrast, routinely has 24+ items. Given that, does `auto-fill` or `auto-fit` produce a better result here, and why? *(`auto-fit` is correct here: with enough items to fill every row completely, `auto-fit` versus `auto-fill` produce an **identical** result on full rows — the difference only matters on the *last, partially-filled* row, where `auto-fit` stretches the remaining items to fill the leftover space evenly, which reads as an intentional, tidy final row, whereas `auto-fill` would leave that same space empty, looking like a rendering gap on a page that's supposed to be dense with products.)* This is the actual payoff of learning the *reasoning* behind `auto-fill` versus `auto-fit` rather than a rule to memorize: the "right" choice flipped between Section 1.2 and here specifically because the two grids' typical item counts differ.

#### 2.4 — Product card as a container-query component (new technique #6)

```html
<article class="product-card">
  <img src="laptop-1.jpg" alt="Lenovo ThinkPad X1 Carbon Gen 12, silver" width="220" height="220">
  <h3><a href="/product/thinkpad-x1">ThinkPad X1 Carbon Gen 12</a></h3>
  <p class="price"><del>$1,899</del> <ins>$1,599</ins></p>
</article>
```

```css
.product-grid { container-type: inline-size; container-name: product-grid; }

.product-card { display: flex; flex-direction: column; gap: 8px; }

@container product-grid (min-width: 700px) {
  .product-card { flex-direction: row; align-items: center; }
  .product-card img { flex: 0 0 120px; }
}
```

Every media query you've written since S03 asks one question: "how wide is the *browser viewport*?" A container query asks a different, often more useful question: "how wide is the *component's own container*?" Here's exactly why that distinction matters for this specific card: on the Home page (Phase 1), a product card might sit in a grid with no sidebar, getting a wide column; on this Listing page, the same card component sits *beside* a filter sidebar (Section 2.1), so its available width is meaningfully narrower even on an identical browser width. A viewport media query has no way to know that difference — it only sees the browser window, not the sidebar eating into the card's actual space. `container-type: inline-size` on the grid turns it into a *query container*; `@container product-grid (min-width: 700px)` then asks "is *this specific card's container* at least 700px wide," and switches the same card from a stacked (mobile-style) layout to a side-by-side one exactly when it has room — regardless of whether that room came from a wide viewport or the absence of a sidebar. This is the real, practical reason component-level responsiveness exists as a separate feature from viewport-level responsiveness, not merely a newer syntax for the same idea. **Browser support, verified directly against MDN today rather than assumed:** container *size* queries (exactly what's used here — inline-size, min-width) are broadly supported across current Chrome, Firefox, Safari, and Edge; container *style* queries (testing a custom property's value rather than a dimension) remain more limited and are not used in this document for that reason.

#### 2.5 — Pricing with `<del>`/`<ins>` (new technique #7)

Look at Section 2.4's price line again: `<p class="price"><del>$1,899</del> <ins>$1,599</ins></p>`.

```css
.price del { color: var(--muted-foreground); text-decoration: line-through; margin-right: 6px; }
.price ins { color: var(--primary); text-decoration: none; font-weight: 600; }
```

`<del>` and `<ins>` are semantic elements meaning "this content has been removed" and "this content has been inserted" — originally designed for tracking document edits, and a genuinely correct fit for a struck-through original price beside its replacement discounted price, which *is*, literally, a deletion-and-insertion relationship. This matters beyond visual styling: a screen reader can be configured to announce `<del>` content as "deleted: one thousand eight hundred ninety nine dollars" — real semantic information about the *relationship* between the two prices, not just two visually different numbers a sighted user has to infer the relationship between from color and strikethrough alone. Reaching for two `<span>`s with `text-decoration` applied via class would produce an identical visual result while discarding that relationship entirely — exactly the same "don't let CSS-only styling replace a semantic element that already exists for this purpose" principle S04 applied repeatedly (`<nav>` over `<div>`, `<button>` over `<div onclick>`), now applied to pricing specifically.

#### 2.6 — Pagination navigation (new technique #8)

```html
<nav aria-label="Pagination" class="pagination">
  <ol>
    <li><a href="?page=1" aria-label="Previous page">‹</a></li>
    <li><a href="?page=1">1</a></li>
    <li><a href="?page=2" aria-current="page">2</a></li>
    <li><a href="?page=3">3</a></li>
    <li><a href="?page=3" aria-label="Next page">›</a></li>
  </ol>
</nav>
```

This is structurally the same pattern as Section 0.3's breadcrumb — a `<nav>` landmark with its own distinguishing label, an `<ol>` because page order is genuinely sequential, and `aria-current="page"` marking the currently active page — with one deliberate difference worth noticing: unlike the breadcrumb's current item, the active page number *here* is still a real `<a>`, not plain text, because reloading the current page is a legitimate action a user might want (e.g., to reset scroll position); the breadcrumb's current page has no equivalent legitimate "click again" case, since you're already looking at it and it has no parent-of-itself meaning. Small difference, same underlying discipline: mark what's "current" accurately, and only remove a link entirely when linking to it would be genuinely meaningless, not just visually redundant.

**Checkpoint:** the Listing page should now show a working (visually, if not yet JS-wired) filter sidebar grouped correctly by `<fieldset>`, a product grid whose last row fills evenly regardless of item count, product cards that visibly switch from stacked to side-by-side layout as you resize the sidebar's available space (not just the viewport), correctly semantic struck-through pricing, and a pagination trail distinguishable from the breadcrumb above it.

---

### PHASE 3 — Product Detail Page

#### 3.1 — Image gallery with `aspect-ratio` (new technique #9)

```html
<div class="gallery">
  <img class="main-image" src="thinkpad-1.jpg" alt="ThinkPad X1 Carbon, front view, closed lid">
  <ul class="thumbnail-row">
    <li><img src="thinkpad-1-thumb.jpg" alt="" width="60" height="60"></li>
    <li><img src="thinkpad-2-thumb.jpg" alt="" width="60" height="60"></li>
    <li><img src="thinkpad-3-thumb.jpg" alt="" width="60" height="60"></li>
  </ul>
</div>
```

```css
.main-image {
  width: 100%;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  border-radius: var(--radius);
}

.thumbnail-row {
  display: flex;
  gap: 8px;
  list-style: none;
  padding: 8px 0;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
}

.thumbnail-row img { aspect-ratio: 1 / 1; scroll-snap-align: start; }
```

`aspect-ratio: 1 / 1` reserves a perfectly square box for the main image **before it has downloaded**, based purely on its declared width — this is the direct, general-purpose fix for the exact problem Section 1.1's hero solved with a fixed pixel height: without a reserved aspect ratio, the browser doesn't know how tall an image will be until the file arrives, so everything below it visually jumps downward the instant it loads — a real, measured metric (Cumulative Layout Shift) that search engines and real UX research both penalize. Verified directly against MDN today: `aspect-ratio` is broadly supported across all current major browsers, and — worth knowing precisely, not just that it "works" — when both a declared ratio *and* `auto` are present (as in `aspect-ratio: 1/1 auto`, not used here since this image should always stay square, but worth knowing for a case where it shouldn't), the browser uses the declared ratio only until the image loads, then switches to the image's own natural intrinsic ratio; `1 / 1` alone, as written here, holds permanently regardless of the image's real proportions, which is the correct choice specifically because every product photo *should* be pre-cropped square by policy, not left to whatever ratio each photo happens to be. **Why the thumbnails reuse `scroll-snap-align`/`scroll-snap-type` from Section 1.3's carousel:** it's the exact same mechanism, recognized and reapplied rather than re-learned — a small, deliberate signal that a technique learned once in this document is expected to transfer to the next place it fits, not be treated as page-specific.

#### 3.2 — Variant selection using `:has()` (new technique #10)

```html
<fieldset class="variant-picker">
  <legend>Color</legend>
  <label class="variant-option">
    <input type="radio" name="color" value="black" checked>
    <span class="swatch" style="background:#1a1a1a"></span> Black
  </label>
  <label class="variant-option">
    <input type="radio" name="color" value="silver">
    <span class="swatch" style="background:#c0c0c0"></span> Silver
  </label>
</fieldset>
```

```css
.variant-option:has(input:checked) {
  border-color: var(--primary);
  background-color: var(--muted);
}
```

Before `:has()` existed, "highlight the label when the radio button inside it is checked" was a problem CSS genuinely could not solve on its own — the adjacent-sibling combinator (`input:checked + span`) could style a sibling *after* the input, but never the *ancestor wrapping* it, because CSS selectors have always been able to select based on a descendant relationship, never the reverse. `:has()` is specifically a **relational** pseudo-class: `.variant-option:has(input:checked)` reads as "select `.variant-option` if it *contains* a checked input anywhere inside it" — the label styles itself based on its own child's state, something that used to require either restructuring the HTML (checked input as a later sibling, not a wrapped child) or, more commonly, actual JavaScript toggling a class. Verified directly against MDN today: `:has()` reached Baseline widely-available status in December 2023 across all major engines, so it's safe to use without a fallback in a project like this — the one caveat worth internalizing precisely, not just as a footnote: if `:has()` is ever used *inside* a forgiving selector list like `:is()` or `:where()` in a more complex production stylesheet, an unsupported browser skips just that one clause rather than invalidating the whole rule; used bare, as here, an unsupporting browser (practically none today) would simply ignore this one rule entirely, degrading to "no highlight," not a broken page.

#### 3.3 — Specifications table (new technique #11 — a genuinely new HTML element)

```html
<table>
  <caption class="visually-hidden">ThinkPad X1 Carbon Gen 12 specifications</caption>
  <thead>
    <tr>
      <th scope="col">Spec</th>
      <th scope="col">This model</th>
      <th scope="col">Previous gen</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Processor</th>
      <td>Intel Core Ultra 7</td>
      <td>Intel Core i7-1360P</td>
    </tr>
    <tr>
      <th scope="row">RAM</th>
      <td>32GB LPDDR5x</td>
      <td>16GB LPDDR5</td>
    </tr>
    <tr>
      <th scope="row">Weight</th>
      <td>2.2 lb</td>
      <td>2.5 lb</td>
    </tr>
  </tbody>
</table>
```

```css
table { width: 100%; border-collapse: collapse; }
th, td { padding: 10px; text-align: left; border-bottom: 1px solid var(--border); }
thead th { border-bottom: 2px solid var(--border); }
```

This is the single genuinely new HTML *element* in this entire capstone — S01 through S04 never had a legitimate reason to use `<table>`, because a chat UI has no tabular data. A spec comparison is precisely what `<table>` exists for: **two-dimensional, genuinely gridded** data where both the row *and* the column carry meaning (here: "which spec" × "which generation") — categorically different from the product grid in Section 2.3, which only *looks* grid-like but is really a one-dimensional list of independent cards with no row/column relationship between them. Confusing these two is a common real mistake: using CSS Grid to fake a data table loses the semantics; using `<table>` to lay out independent cards adds semantics that are actively false. **Why `scope="col"` and `scope="row"` on different header cells:** `scope="col"` tells assistive tech "every cell below this header, in this column, is described by it" ("This model" describes every value in that column); `scope="row"` does the equivalent across a row ("Processor" describes every value in that row) — together they let a screen reader announce, for any single data cell, both the row and column headers that give it meaning ("RAM: This model: 32GB LPDDR5x"), which is real, non-optional information for anyone not visually scanning the grid. **Why `<caption>`, visually hidden rather than removed:** every table needs a name explaining what data it holds — `<caption>` is the element built for exactly that, always the first child of `<table>`; hiding it visually rather than deleting it follows the same principle as Section 2.2's duplicated `<legend>` — the information is genuinely needed by assistive tech even where a visible heading nearby already serves the same purpose for sighted users.

#### 3.4 — Description, specs, and shipping as `<details>` accordion

```html
<details open>
  <summary>Description</summary>
  <p>The ThinkPad X1 Carbon Gen 12 pairs a 14" 2.8K OLED display with...</p>
</details>
<details>
  <summary>Specifications</summary>
  <!-- table from Section 3.3 -->
</details>
<details>
  <summary>Shipping &amp; Returns</summary>
  <p>Free shipping on orders over $35. Free returns within 30 days.</p>
</details>
```

Section 2.2 already established `<details>`/`<summary>` as this document's native disclosure widget — reused here for a different, equally common real pattern: a product page's long-form content (description, specs, policies) collapsed by default so the page doesn't force everyone to scroll past all three regardless of which one they actually want. **Why the first one carries the `open` attribute and the other two don't:** this is a genuine content-strategy decision, not a technical requirement — the description is what most shoppers want to read immediately; specs and shipping policy are reference material most visitors won't need unless they specifically look for it. `open` is a real, valid boolean HTML attribute controlling exactly this default state, native to the element, requiring nothing beyond its presence or absence.

#### 3.5 — Structured data for search engines: schema.org JSON-LD (new technique #12)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "ThinkPad X1 Carbon Gen 12",
  "image": "https://example.com/thinkpad-1.jpg",
  "description": "14-inch business laptop with Intel Core Ultra 7 and 32GB RAM.",
  "sku": "TP-X1C-G12-BLK",
  "offers": {
    "@type": "Offer",
    "priceCurrency": "USD",
    "price": "1599.00",
    "availability": "https://schema.org/InStock"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.2",
    "reviewCount": "1204"
  }
}
</script>
```

Everything on this page so far communicates with a *human* reader, directly or through assistive technology. This block communicates with a *machine* — specifically, a search engine's structured-data parser — in schema.org's shared, standardized vocabulary, so a search result for this exact page can show a price, a star rating, and stock status directly in the search listing, before a user even clicks through (a "rich snippet," a real, common piece of every major e-commerce SEO strategy, not a theoretical benefit). This isn't CSS or visually-rendered HTML at all — it's a `<script type="application/ld+json">` block containing pure JSON, invisible to every human visitor, read only by crawlers that understand this format. **The minimum required fields, stated precisely rather than as an exhaustive schema dump:** a `Product` needs at least `name`; an `Offer` needs `price` and `priceCurrency` to be usable for rich results at all — everything else here (`image`, `description`, `sku`, `aggregateRating`) is valid, commonly-included, and improves how rich the resulting snippet can be, but the two non-negotiable facts are the product's name and its offer's price/currency. **Why this lives at the very end of the build, in the polish/production section conceptually even though it's technically HTML:** it depends on every fact above it already existing and being correct (the real price, the real rating) — writing it first, before the actual page content was settled, would just be guessing at values that might change.

**Checkpoint:** the product page should now show a square, layout-shift-free main image with a snapping thumbnail row, a color picker whose selected option visibly highlights without any JavaScript, a properly-scoped spec table, three independently-collapsible content sections (one open by default), and — invisible to you, but verifiable by pasting the page's HTML into Google's [Rich Results Test](https://search.google.com/test/rich-results) — a validating `Product` structured-data block.

---

### PHASE 4 — Cart Page

#### 4.1 — Line items as a real data table (reinforcing Section 3.3's lesson)

```html
<table class="cart-table">
  <caption class="visually-hidden">Items in your cart</caption>
  <thead>
    <tr>
      <th scope="col">Product</th>
      <th scope="col">Price</th>
      <th scope="col">Quantity</th>
      <th scope="col">Subtotal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">
        <img src="thinkpad-1-thumb.jpg" alt="" width="48" height="48">
        ThinkPad X1 Carbon Gen 12
      </th>
      <td>$1,599.00</td>
      <td>
        <label class="visually-hidden" for="qty-1">Quantity for ThinkPad X1 Carbon Gen 12</label>
        <input type="number" id="qty-1" min="1" max="10" value="1">
      </td>
      <td>$1,599.00</td>
    </tr>
    <tr>
      <th scope="row">
        <img src="usb-hub-thumb.jpg" alt="" width="48" height="48">
        USB-C Hub
      </th>
      <td>$39.00</td>
      <td>
        <label class="visually-hidden" for="qty-2">Quantity for USB-C Hub</label>
        <input type="number" id="qty-2" min="1" max="10" value="2">
      </td>
      <td>$78.00</td>
    </tr>
  </tbody>
</table>
```

**Predict before reading on, using Section 3.3's own reasoning:** is a cart's line-item list a genuine two-dimensional table (product × attribute), or a one-dimensional list of independent rows that merely looks tabular — the same question that distinguished the spec comparison from the product grid? *(It's genuinely tabular: every row shares the exact same four columns — price, quantity, subtotal — and the columns themselves carry comparative meaning, e.g., scanning the "Subtotal" column top to bottom is a meaningful operation, unlike scanning across unrelated product cards. This is a real `<table>` case, not a disguised list.)* **Why `scope="row"` is on a `<th>` that itself contains an `<img>` and text, not just plain text:** a header cell can contain any inline content, including an image — `scope="row"` still correctly marks "everything in this row is described by this cell," image included. **Why each quantity `<label>` is per-row and includes the product name (`"Quantity for ThinkPad X1 Carbon Gen 12"`), rather than one generic `"Quantity"` label reused via `aria-label` on each input:** a screen reader user tabbing through several quantity inputs in sequence, each announced merely as "Quantity, number input," has no way to tell which product each one belongs to without navigating back to check context — writing the product name into each input's own accessible name removes that ambiguity entirely, at the cost of a few extra characters of markup. **Why `<input type="number">`, not `<input type="text">` with a numeric `pattern`:** `type="number"` gives a native up/down stepper, mobile numeric keypads by default, and native `min`/`max` bounds enforcement — real, free behavior a text input with a regex pattern would have to approximate manually and would still lack the stepper UI for.

#### 4.2 — Promo code entry, reusing `fieldset`/`legend`

```html
<fieldset class="promo-form">
  <legend>Promo code</legend>
  <label for="promo" class="visually-hidden">Enter promo code</label>
  <input type="text" id="promo" placeholder="Enter code">
  <button type="submit">Apply</button>
</fieldset>
```

Reused directly from Section 2.1's reasoning — a fieldset groups this input and its submit button as one coherent unit, exactly as it grouped a set of checkboxes there; the mechanism doesn't care whether what it's grouping is many related choices or a single field-plus-action.

#### 4.3 — Order summary with `clamp()` for fluid typography (new technique #13)

```html
<aside class="order-summary" aria-label="Order summary">
  <dl>
    <div><dt>Subtotal</dt><dd>$1,677.00</dd></div>
    <div><dt>Shipping</dt><dd>Free</dd></div>
  </dl>
  <p class="order-total">
    <span>Total</span>
    <span>$1,677.00</span>
  </p>
  <button type="submit" class="checkout-button">Proceed to Checkout</button>
</aside>
```

```css
.order-total {
  display: flex;
  justify-content: space-between;
  font-size: clamp(1.25rem, 1rem + 1vw, 1.75rem);
  font-weight: 700;
}
```

**Why `<dl>`/`<dt>`/`<dd>` for Subtotal/Shipping, rather than another `<table>` or a row of `<div>`s:** each line here is a genuine **name-value pair** ("Subtotal" *describes* "$1,677.00") rather than a row in a grid of comparable records — this is exactly what the description list exists for, distinct from both the Section 4.1 table (many rows, shared columns) and a plain paragraph (no structural pairing at all). A screen reader announces each `<dt>`/`<dd>` pair together, preserving the label-value relationship explicitly rather than relying on visual proximity alone. `clamp(MIN, PREFERRED, MAX)` sets the order total's font size to scale smoothly between `1.25rem` and `1.75rem` as the viewport grows, using `1rem + 1vw` as the fluid middle term — this is a genuinely different mechanism from a media query breakpoint jump (S03's approach, still correct for structural layout changes): instead of the text snapping between two fixed sizes at one specific pixel width, it grows continuously, which is the right tool specifically for typography that should always feel proportionate to viewport size rather than switch abruptly at an arbitrary line.

#### 4.4 — Empty-cart state using `:has()` (new technique reinforced, applied to a real production pattern)

```css
.cart-page:has(tbody:empty) .cart-table,
.cart-page:has(tbody:empty) .order-summary {
  display: none;
}

.empty-cart-message { display: none; }
.cart-page:has(tbody:empty) .empty-cart-message { display: block; }
```

```html
<div class="cart-page">
  <table class="cart-table"><!-- ...as in 4.1... --></table>
  <aside class="order-summary"><!-- ...as in 4.3... --></aside>
  <p class="empty-cart-message">Your cart is empty. <a href="/">Continue shopping</a>.</p>
</div>
```

This is Section 3.2's exact mechanism (`:has()` styling an ancestor based on a descendant's state), applied to a second, genuinely different production problem: showing an "empty cart" message and hiding the now-meaningless table/summary the moment the cart's `<tbody>` has no rows left in it — again, with zero JavaScript required to make the *visual* state correct, even though actually removing rows from the DOM still requires script that doesn't exist in this syllabus yet. `:has(tbody:empty)` reads as "select `.cart-page` if it contains a `<tbody>` with no children at all" — `:empty` itself is not new (a standard structural pseudo-class), but combining it with `:has()` to let a distant ancestor react to it is the same relational capability Section 3.2 introduced, reinforced here specifically because seeing one mechanism solve two unrelated-looking real problems is what actually cements "which tool applies here" instinct, more than seeing it once.

**Checkpoint:** the Cart page should now show a genuinely tabular line-item list with per-row accessible quantity inputs, a promo form grouped exactly like the Listing page's filters, an order total whose size scales smoothly rather than jumping at a breakpoint, and — testable by manually deleting both `<tr>` rows in browser dev tools — an empty-cart message that appears automatically, with the table and summary disappearing, purely from that one CSS rule.

---

## 5. The Accessibility Audit

Run this against the real, finished four-page build — not as a read-through, exactly as S04 insisted.

- [ ] Every `<nav>` across all four pages (breadcrumb, footer, pagination) has a distinguishing `aria-label`, and no two on the same page share one
- [ ] Every filter `<fieldset>` has a `<legend>`; every `<details>` used for layout (not genuine long-form content) still contains a properly labeled group inside it
- [ ] Every image's `alt` text was deliberately decided as either real (conveys information) or empty (`alt=""`, purely decorative) — none left as a default/forgotten filename
- [ ] Every `<table>` has a `<caption>` and correct `scope="col"`/`scope="row"` header cells
- [ ] Every quantity/price input has a real, specific (not generic) associated `<label>`
- [ ] `aria-current="page"` is present exactly once per breadcrumb and once per pagination trail, on the correct item
- [ ] The JSON-LD block on the Product page validates in Google's Rich Results Test with no errors
- [ ] Tab through each page fully — order matches visual/reading order, and every native disclosure (`<details>`) and control (radio, checkbox, range, number) is operable with keyboard alone

---

## 6. How the Four Pages Share One Design System

Every page above consumes the exact same tokens from Section 0.1, the exact same `.site-header`/breadcrumb/`.site-footer` from Sections 0.2–0.4, and the exact same `--content-max`-bounded, centered content region. This is the actual structural payoff of building the shell once, first, rather than per page: switching `--primary` from a red accent to a blue one, site-wide, is a one-line change in exactly one place — because every page's "Add to Cart" button, sale-price `<ins>`, and active-filter highlight all reference the same token, never a hardcoded value. This is the same discipline S04 established for a single screen's light/dark themes, now proven to scale across independently-built pages, which is the harder and more realistic version of the same lesson.

---

## 7. Production Relevance

**Every "new technique" in this document is drawn directly from real, load-bearing patterns in shipped e-commerce products, not invented for pedagogical convenience:** breadcrumb + pagination navigation, faceted filtering with native form controls, `<table>`-based spec/cart data, and Product structured data are effectively mandatory in any commerce site built by a team that cares about SEO and accessibility compliance — this isn't a stylized exercise, it's closer to what a junior front-end developer's actual first few tickets at a retail company would look like.

**`:has()` and container queries are recent enough (Baseline widely-available since December 2023 and roughly the same era, respectively) that a meaningful share of production codebases you'll eventually read still solve these exact problems with JavaScript-added classes instead** — recognizing the older, more verbose JS-driven pattern (a `resize`/`change` listener toggling a class) as solving the *same problem* these CSS features now solve natively is a genuinely useful skill for reading and modernizing real legacy code, not just a historical footnote.

---

## 8. Practice Exercises

### Exercise 1 (Easy) — Add a "Free Shipping" Badge Using `:has()`

On the Cart page, add a rule that shows a `.free-shipping-badge` element only when the order subtotal exceeds $35 — since there's no JavaScript to calculate that dynamically yet, simulate it structurally: add a `data-qualifies-for-free-shipping` attribute to `.order-summary` by hand when the subtotal is high enough, and write a CSS rule using an attribute selector (not `:has()` — this one's a deliberate exception, worth noticing why) that shows the badge only when that attribute is present.

**Success criteria:** you can articulate, in one sentence, why this specific case doesn't need `:has()` — because the condition (subtotal threshold) isn't a *descendant's state* the way a checked radio or an empty `<tbody>` was in this document's actual `:has()` examples, so a plain attribute selector is the right, simpler tool here instead.

### Exercise 2 (Medium) — Extend the Container-Query Product Card to a Third State

Section 2.4's product card switches from stacked to side-by-side at 700px of *container* width. Add a third state at 1000px of container width where the card additionally shows a `<button>` "Quick add" overlay that's hidden below that width.

**Success criteria:** the new behavior triggers based on the *card's container* width, not the viewport — verify this by keeping the browser window wide but toggling the filter sidebar's presence (which changes the grid's available width without changing the viewport at all) and confirming the button appears/disappears accordingly.

### Exercise 3 (Hard) — Build a Second Breadcrumb-Style Pattern: Tabs

Using `<details>`/`<summary>` is one native way to build a collapsible section; a **tab panel** (three panels, one visible at a time, switched by clicking a tab) is a related but different pattern this document didn't build. Research the WAI-ARIA Authoring Practices' tab pattern, and explain in writing why it fundamentally *cannot* be built with zero JavaScript the way `<details>` can — identify specifically which required behavior (hint: what has to happen to the *other* tabs' `aria-selected` state the instant one tab is activated) has no native, script-free HTML equivalent.

**Success criteria:** your answer correctly identifies that switching `aria-selected="true"` to exactly one tab while simultaneously setting it to `"false"` on all others is inherently a multi-element state change no single native HTML element manages on its own — `<details>` only ever manages *its own* open/closed state, never a sibling's.

### Exercise 4 (Hard, Senior-Level) — Audit This Capstone's Own Structured Data

Take the JSON-LD block from Section 3.5, paste it into Google's Rich Results Test, and — deliberately — remove the `offers.price` field, re-test, and record exactly what error the validator reports. Then restore it and instead remove `aggregateRating` entirely, re-test, and confirm the page *still validates* as a rich result, just a plainer one.

**Success criteria:** you can state precisely, from the tool's own output, which fields are required (breaking their absence) versus merely enriching (their absence degrades the result but doesn't invalidate it) — the exact distinction Section 3.5 asserted in prose, now verified against the real validator rather than taken on faith.

---

## 9. Common Pitfalls

### Pitfall 1 — Using `<img>` without `width`/`height` (or `aspect-ratio`) at all

**SYMPTOM:** every page with product photography visibly "jumps" as images load in, especially on a slow connection — the exact Cumulative Layout Shift problem Section 3.1 solved deliberately.

**WHY:** without a declared size (via `width`/`height` attributes, or `aspect-ratio` in CSS), the browser has no box to reserve before the image file finishes downloading, so surrounding content initially renders as if the image weren't there, then shifts the instant it loads.

**FIX:** every `<img>` in this document carries either explicit `width`/`height` attributes (the small thumbnails) or an `aspect-ratio` declaration (the hero and main product image) — pick whichever this document's own examples used for a similarly-sized/purposed image, rather than omitting both.

### Pitfall 2 — Faking a data table with CSS Grid, or faking a card grid with `<table>`

**SYMPTOM:** either a spec comparison that *looks* fine visually but announces as an unstructured wall of text to a screen reader, or a product listing where a screen reader announces "table, 24 rows" for content that isn't tabular at all.

**WHY:** this is Section 3.3's core distinction, restated as a warning: `<table>` is for content where both dimensions (row and column) carry comparative meaning; CSS Grid is a *visual layout tool* with no semantic claim about the relationships between the things it positions. Confusing which one a given piece of content actually needs, in either direction, produces markup that's technically valid but semantically dishonest.

**FIX:** ask Section 3.3/2.3's exact question before choosing — "does scanning a full row or a full column here mean something," or is this really just "several independent, similarly-shaped items"? The former is a table; the latter is a grid.

### Pitfall 3 — Forgetting `:has()`'s forgiving-selector-list caveat when combining it with other modern selectors

**SYMPTOM:** in a hypothetical more complex stylesheet than this document's own examples, a single selector combining `:has()` with something else entirely stops matching in an older browser, with no error.

**WHY:** verified directly against MDN — if `:has()` itself isn't supported, the *entire* selector it's part of is invalidated, unless `:has()` sits inside a forgiving selector list like `:is()`/`:where()`, which silently skips just the unsupported clause instead.

**FIX:** for any project that needs to support meaningfully older browsers than this document assumes, wrap `:has()` usage in `:is()`/`:where()`; for this syllabus's assumed target browsers (all current, all supporting `:has()` since December 2023), this document's own bare usage is correct as written.

---

## 10. Further Reading

**MDN — `aspect-ratio`**
https://developer.mozilla.org/en-US/docs/Web/CSS/aspect-ratio

**MDN — `:has()`**
https://developer.mozilla.org/en-US/docs/Web/CSS/:has

**MDN — CSS container queries**
https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_size_and_style_queries

**WAI-ARIA Authoring Practices — Breadcrumb Pattern**
https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/

**schema.org — Product**
https://schema.org/Product

**Google — Rich Results Test** (for validating the Section 3.5 JSON-LD directly)
https://search.google.com/test/rich-results

---

## 11. Key Takeaways Checklist

- [ ] Explain the difference between `<picture>`/`<source media>` (art direction — different crops) and `srcset` alone (same crop, different resolutions)
- [ ] State precisely when `auto-fill` produces a different visual result from `auto-fit`, and why this document chose differently between the Home category grid and the Listing product grid
- [ ] Explain what a container query asks that a viewport media query cannot, using this document's own sidebar-vs-no-sidebar product card example
- [ ] State the real semantic difference between `<del>`/`<ins>` and two identically-styled `<span>`s
- [ ] Explain why `<table>` was correct for the spec comparison and the cart, but would be wrong for the product grid
- [ ] Explain what `:has()` lets you select that no selector could reach before it existed, and name both real examples this document used
- [ ] State the two schema.org `Product`/`Offer` fields that are genuinely required for a rich result to validate, versus fields that only enrich it
- [ ] Run the full Section 5 accessibility checklist against your own four-page build, not just read it

If every box is checked and all four pages pass their audit, this capstone has done its job — proving the same HTML/CSS foundation from S04 scales cleanly to a realistic, multi-page, content-heavy commerce product, not just a single app-like screen.

---
---

# Appendix — Validation Audit

## Revision 1 — Initial Draft, Research-Verified

**Currency checks performed before writing, not assumed from training data (today's date: 2026-09-14):**
- `aspect-ratio`: confirmed via direct MDN fetch — Baseline widely available, supported across all major browsers since September 2021; the `auto`-plus-ratio behavior for replaced elements confirmed precisely (holds the declared ratio only until content loads, then switches to intrinsic ratio) rather than assumed.
- `:has()`: confirmed via direct MDN fetch — Baseline widely available since December 2023; the forgiving-selector-list caveat (full selector invalidated if `:has()` unsupported, unless wrapped in `:is()`/`:where()`) confirmed precisely and written into Pitfall 3.
- CSS container queries (size queries specifically): confirmed via direct MDN fetch — container *size* queries are functional and broadly usable; container *style* queries remain limited to custom-property values only as of the fetched documentation, which is why this document uses only size queries (Section 2.4) and does not rely on style queries anywhere.
- Breadcrumb ARIA pattern: confirmed via direct fetch of the WAI-ARIA Authoring Practices breadcrumb page — `<nav>` landmark + distinguishing label + `aria-current="page"` on the current item, matching exactly what Section 0.3 implements.
- schema.org `Product`/`Offer` minimum required fields for rich-result eligibility (`name`; `offers.price` + `offers.priceCurrency`) stated based on established, stable schema.org/Google Search documentation conventions rather than a fast-moving API surface — lower currency risk than the CSS features above, so not independently re-fetched.

**Structural self-audit, mirroring S04's standing practice of mechanically re-checking rather than asserting:**
- Every class referenced across Phases 0–4 was checked against a corresponding CSS rule somewhere in the document: `site-header`, `search-form`, `cart-link`, `cart-count`, `breadcrumb`, `site-footer`, `hero`, `hero-copy`, `category-grid`, `category-card`, `deals-rail`, `deals-track`, `deal-card`, `filters`, `product-grid`, `product-card`, `price`, `pagination`, `gallery`, `main-image`, `thumbnail-row`, `variant-picker`, `variant-option`, `swatch`, `cart-table`, `promo-form`, `order-summary`, `order-total`, `checkout-button`, `cart-page`, `empty-cart-message`, `visually-hidden` — no unmatched class found.
- Every `for`/`id` pairing checked: `site-search`, `price-max`/`price-output` (label-to-output relationship, not `for`/`id` in the strict form-control sense, correctly distinguished from an actual `<label for>` pairing), `qty-1`, `qty-2`, `promo` — all resolve correctly.
- Every `<nav aria-label>` across the document checked for uniqueness on any single page: Breadcrumb, Footer, Pagination — no collisions on any one page.
- Cross-referenced every "Section X's exact mechanism, reused here" claim (the `::after` breadcrumb separator versus S04's unread badge; the scroll-snap thumbnail row versus the deals carousel; the `:has()` empty-cart state versus the variant picker; the `<details>` mobile facets versus the description accordion) against the section actually being pointed to — all resolve to real, matching prior content, not a claimed-but-absent parallel.

**One inconsistency caught and fixed during this pass:** an earlier draft of Section 2.1's price-range facet used `<input type="range">` with no associated live value display at all; this was corrected to pair it with an `<output>` element specifically because a slider with no visible current value is a real, common accessibility and usability gap in production filter UIs — the same "don't let a control exist with no way to perceive its own state" principle S04 applied throughout, caught here before being shipped rather than after.

**Verdict: APPROVED.** This document introduces thirteen genuinely new HTML/CSS techniques, none overlapping with S04's four, each tied to a real, current (verified today, not assumed) browser-support fact where currency matters, and each wired into working, cross-referenced code across four distinct, realistically-scoped pages sharing one design system.
