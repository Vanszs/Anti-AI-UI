# Layouts — Modern Page & App Skeletons

Companion to `../SKILL.md`. Every pattern: NAME · when-to-use · concrete CSS/Tailwind · responsive · motion. Sources: Every-Layout.dev, Josh Comeau, web.dev, Tailwind UI, MDN, NNG, Refactoring UI, Smashing.

> Anti-monotony rule of thumb: never repeat the same skeleton in consecutive sections; vary container width and section padding; ≥1 full-bleed break per page.

---

## 1. HOLY GRAIL (header / nav / main / aside / footer)
**When:** docs, content sites, admin with TOC sidebars.
```css
.holy-grail{
  display:grid;
  grid-template-areas:"header header header" "nav main aside" "footer footer footer";
  grid-template-columns:16rem 1fr 14rem;
  grid-template-rows:auto 1fr auto;
  min-height:100dvh;
}
.header{grid-area:header}.nav{grid-area:nav}.main{grid-area:main}.aside{grid-area:aside}.footer{grid-area:footer}
@media(max-width:768px){
  .holy-grail{grid-template-areas:"header" "main" "nav" "aside" "footer";grid-template-columns:1fr}
}
```
**Responsive:** collapses to single column. **Motion:** none (structural).

---

## 2. SIDEBAR + CONTENT (app shell) — Every-Layout "Sidebar"
**When:** dashboards, settings, docs. Most common app layout.
```html
<div class="flex h-dvh">
  <aside class="w-64 shrink-0 overflow-y-auto border-r bg-white">…nav…</aside>
  <main class="flex-1 overflow-y-auto p-6">…content…</main>
</div>
```
Intrinsic version (no breakpoints) — wraps when sidebar can't fit:
```css
.with-sidebar{display:flex;flex-wrap:wrap;gap:1rem}
.sidebar{flex-basis:16rem;flex-grow:1}
.not-sidebar{flex-basis:0;flex-grow:999;min-inline-size:50%}
```
**Responsive:** sidebar → off-canvas drawer on mobile. **Motion:** drawer slide-in `translate-x` 200ms ease-out.

---

## 3. DASHBOARD SHELL (header + sidebar + content grid)
**When:** analytics, admin with KPI cards + tables + charts.
```html
<div class="grid h-dvh grid-cols-[16rem_1fr] grid-rows-[4rem_1fr]">
  <aside class="row-span-full border-r overflow-y-auto bg-slate-50">…</aside>
  <header class="sticky top-0 z-10 border-b bg-white px-6 flex items-center">…</header>
  <main class="overflow-y-auto p-6">
    <div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-4">…KPI cards…</div>
  </main>
</div>
```
**Responsive:** sidebar → icon-rail (`w-16`) on tablet → off-canvas on mobile.

---

## 4. SPLIT SCREEN (50/50 or asymmetric)
**When:** login/signup, comparison, product showcase, hero.
```html
<div class="grid min-h-dvh lg:grid-cols-2 max-lg:grid-cols-1">
  <div class="flex items-center justify-center bg-slate-900 p-12 max-lg:hidden">…branding…</div>
  <div class="flex items-center justify-center p-8">…form…</div>
</div>
```
Asymmetric: `lg:grid-cols-[2fr_3fr]` (40/60). **Responsive:** stacks; branding panel may hide.

---

## 5. FULL-BLEED LAYOUT (Josh Comeau)
**When:** long-form content where some elements span the viewport.
```css
.wrapper{
  --pad:1rem;
  display:grid;
  grid-template-columns:1fr min(42rem,100%) 1fr;
  padding-inline:var(--pad);
}
.wrapper>*{grid-column:2}
.full-bleed{grid-column:1 / -1;width:100%}
.breakout{grid-column:1 / -1;max-width:60rem;margin-inline:auto}
```
Tailwind: `grid grid-cols-[1fr_min(42rem,100%)_1fr] px-4` → children `col-start-2`, breaks `col-span-full`.
**Motion:** scroll-reveal on full-bleed media (opacity 0→1 + translateY 8px, 250ms).

---

## 6. BENTO GRID
**When:** feature showcases, product overviews, Apple-style marketing. MUST have a dominant 2×2 cell + mixed small cells (else it's a card grid).
```html
<div class="grid grid-cols-4 grid-rows-3 gap-4 max-md:grid-cols-2">
  <div class="col-span-2 row-span-2 rounded-xl bg-slate-100 p-6">…dominant…</div>
  <div class="rounded-xl bg-slate-100 p-6">…small…</div>
  <div class="rounded-xl bg-slate-100 p-6">…small…</div>
  <div class="col-span-2 rounded-xl bg-slate-100 p-6">…wide…</div>
</div>
```
**Responsive:** 4-col → 2-col → 1-col. **Motion:** stagger reveal 30–50ms/cell.

---

## 7. MASONRY
**When:** image galleries, varied-height cards, Pinterest feeds.
```html
<!-- columns fallback (works everywhere) -->
<div class="columns-1 gap-4 sm:columns-2 lg:columns-3">
  <div class="mb-4 break-inside-avoid rounded-lg bg-white p-4 shadow-sm">…</div>
</div>
```
CSS-native (Chrome/FF 2025+): `grid-template-rows:masonry`. **Responsive:** column count reduces with viewport.

---

## 8. MAGAZINE / EDITORIAL
**When:** news, content homepages with featured + secondary articles.
```html
<div class="grid gap-6 lg:grid-cols-[2fr_1fr_1fr] lg:grid-rows-2 max-lg:grid-cols-1">
  <article class="lg:row-span-2">…featured, large image…</article>
  <article>…secondary…</article>
  <article>…secondary…</article>
  <article>…secondary…</article>
  <article>…secondary…</article>
</div>
```

---

## 9. ASYMMETRIC (alternating ratios)
**When:** portfolios, agencies, creative landing. Break monotony.
```html
<section class="grid gap-8 lg:grid-cols-[3fr_2fr] items-center">…text / image…</section>
<section class="grid gap-8 lg:grid-cols-[2fr_3fr] items-center">…image / text…</section>
```
**Key:** never repeat the same ratio consecutively (vary 60/40, 40/60, 70/30). Max 2 alternations → break with full-bleed/grid/stats.

---

## 10. STICKY ASIDE (TOC / contextual sidebar)
**When:** docs, long articles with table-of-contents.
```html
<div class="grid gap-8 lg:grid-cols-[1fr_16rem] items-start max-lg:grid-cols-1">
  <main>…long content…</main>
  <aside class="sticky top-20 max-h-[calc(100dvh-6rem)] overflow-y-auto max-lg:hidden">…TOC…</aside>
</div>
```
**Responsive:** aside → expandable disclosure on mobile.

---

## 11. MULTI-COLUMN (newspaper)
**When:** dense text, FAQs, changelogs.
```html
<div class="columns-1 gap-8 sm:columns-2 lg:columns-3">
  <p class="break-inside-avoid mb-4">…</p>
</div>
```

---

## 12. CARD GRID (responsive auto-fill — no breakpoints)
**When:** product listings, team pages, blog index.
```html
<div class="grid grid-cols-[repeat(auto-fill,minmax(18rem,1fr))] gap-6">
  <article class="rounded-lg border p-6">…</article>
</div>
```
**Motion:** stagger entrance 30ms/card.

---

## 13. CONTAINER QUERIES (component adapts to parent, not viewport)
**When:** reusable cards that live in both sidebar and main.
```html
<div class="@container">
  <div class="block @md:flex @md:gap-4">
    <img class="w-full @md:w-2/5 rounded-lg" alt="" />
    <div class="mt-3 @md:mt-0">…</div>
  </div>
</div>
```
Tailwind v4 native (`@container`, `@md:`). Source: web.dev, MDN.

---

## 14. Z-PATTERN
**When:** minimal landing, single-CTA pages. Logo top-left → CTA top-right → diagonal → final CTA bottom-right.
```html
<header class="flex items-center justify-between px-6 py-4">…logo / CTA…</header>
<section class="grid lg:grid-cols-2 gap-8 items-center px-6 py-16">…headline+CTA / hero…</section>
<section class="grid lg:grid-cols-2 gap-8 items-center px-6 py-16">…proof / benefit+CTA…</section>
```
Source: NNG eye-tracking.

---

## 15. F-PATTERN
**When:** content-heavy pages — blogs, search results, feeds.
**Rules:** strong horizontal top, left-aligned scannable headings, front-loaded keywords.
```html
<main class="max-w-3xl mx-auto px-4">
  <h1 class="text-2xl font-bold mb-2">Headline full width</h1>
  <section class="space-y-6">
    <article class="border-b pb-6">
      <h2 class="font-semibold text-lg">Left-aligned scannable heading</h2>
      <p class="text-gray-600 mt-1">Content below.</p>
    </article>
  </section>
</main>
```
Source: NNG F-pattern research.

---

## 16. COVER (Every-Layout) — vertically centered hero / login
```css
.cover{display:flex;flex-direction:column;min-height:100dvh;padding:1rem}
.cover>*{margin-block:1rem}
.cover>.centered{margin-block:auto}
```
Tailwind: `flex min-h-dvh flex-col items-center justify-center p-4`.

---

## EVERY-LAYOUT COMPOSITION PRIMITIVES
Building blocks — compose without `@media`. Source: Every-Layout.dev (Pickering & Bell).

| Primitive | Purpose | Core CSS |
|-----------|---------|----------|
| **Stack** | vertical sibling spacing | `* + *{margin-block-start:var(--space)}` |
| **Box** | padded bordered container | `padding:var(--s1);border:var(--bt) solid` |
| **Center** | intrinsic centering + measure | `max-inline-size:var(--measure);margin-inline:auto` |
| **Cluster** | wrapping inline groups (tags, nav) | `display:flex;flex-wrap:wrap;gap:var(--space)` |
| **Sidebar** | content + narrower aside, wraps | see §2 |
| **Switcher** | N items → row, else column | `flex-wrap:wrap; >*{flex-grow:1;flex-basis:calc((var(--threshold) - 100%)*999)}` |
| **Cover** | vertically centered content | see §16 |
| **Grid** | auto-fill responsive grid | `grid-template-columns:repeat(auto-fill,minmax(var(--min),1fr))` |
| **Frame** | aspect-ratio media box | `aspect-ratio:16/9;overflow:hidden;object-fit:cover` |
| **Reel** | horizontal scroll strip | `display:flex;overflow-x:auto; >*{flex:0 0 auto}` |
| **Imposter** | absolute overlay | `position:absolute;inset:0` (or centered) |

---

## RESPONSIVE CHEAT SHEET
| Width | Tailwind | Typical change |
|-------|----------|----------------|
| <640 | (base) | single column, stacked, hamburger |
| ≥640 | `sm:` | 2-col cards, inline fields |
| ≥768 | `md:` | sidebar appears (collapsible), 2–3 col |
| ≥1024 | `lg:` | full sidebar, 3–4 col, split layouts |
| ≥1280 | `xl:` | max-width containers, wider gutters |

Mobile-first: base = mobile; add complexity upward. Container queries override viewport for components in variable-width slots.

---

## ANTI-MONOTONY TECHNIQUES (recap)
1. Don't repeat the same grid in consecutive sections.
2. Vary section padding (hero `py-24/32` → features `py-16` → CTA `py-12`).
3. ≥2 asymmetric moments per page.
4. One dominant element per section (≥1.5× weight).
5. Break rhythm every 2–3 sections (full-bleed / color block / stats bar).
6. Vary container width (text `max-w-3xl`, grids `max-w-6xl`, bleed full).
7. Mix card sizes in grids (one `col-span-2`).

Sources: Every-Layout.dev, joshwcomeau.com (full-bleed, interactive grid guide), web.dev (container queries), Tailwind UI/Catalyst, NNG (F-pattern, menu checklist), Refactoring UI, MDN (grid-template-areas, masonry).

---
---

# PART 2 — MODERN WEBSITE SKELETONS (2025–2026)

Real page-level skeletons as built by best-in-class sites: Linear, Vercel, Stripe, Raycast, Clerk, Resend, Framer, Apple, Nike, NYT, Mintlify. Each: NAME · when · concrete Tailwind · responsive · source. All anti-slop compliant (no purple-gradient default, no centered-dual-CTA monotony, varied section anatomy, semantic HTML, `dvh`, focus-visible, reduced-motion).

> **The golden flow (Evil Martians, 100 dev-tool pages):** Hero → Trust → Features → Social proof → Supporting (FAQ/pricing/comparison) → Final CTA → Footer. **Each section a DIFFERENT anatomy.** Never repeat the same grid back-to-back.

---

## 17. HERO VARIANTS (8 — pick by product type)

| # | Variant | When | Real site |
|---|---------|------|-----------|
| 17a | Split (text-left / product-right) | SaaS with showable UI (the workhorse) | Linear, Clerk, Resend |
| 17b | Centered + product shot below | Product-led, single CTA, screenshot = proof | Vercel, Mintlify, Cal.com |
| 17c | Dark + single product shot | Dev/creative tools where dark UI is the brand | Raycast, Warp, Arc, Railway |
| 17d | Full-bleed video/image + overlay | Brand/lifestyle, cinematic | Apple, Nike |
| 17e | Editorial / text-only | Agencies, publishing, strong copy | NYT, Pentagram, Substack |
| 17f | Code-block hero | APIs, SDKs, CLIs (proves simplicity) | Resend, Stripe, Prisma |
| 17g | Bento preview hero | Multi-feature, no single screenshot captures it | Apple, Notion |
| 17h | Stats/metrics hero | Enterprise proving scale | Stripe, Cloudflare |

**Decision matrix:** SaaS-with-UI → 17a/b · dev tool/API → 17c/f · brand → 17d · agency → 17e · multi-feature → 17g · enterprise → 17h.

### 17a. Split hero (default, left-aligned, asymmetric)
```html
<section class="px-6 py-24 sm:py-32 lg:py-40">
  <div class="mx-auto max-w-6xl grid gap-12 lg:grid-cols-[1.2fr_1fr] lg:items-center">
    <div>
      <h1 class="text-3xl font-bold leading-[1.1] tracking-tight text-gray-900 sm:text-4xl lg:text-5xl text-balance">
        Identify at-risk students 6 weeks earlier
      </h1>
      <p class="mt-5 max-w-[52ch] text-lg leading-relaxed text-gray-600">
        JagaSekolah scores attendance, grades, and economic data so wali kelas can intervene before dropout.
      </p>
      <a href="/demo" class="mt-8 inline-flex items-center rounded-md bg-blue-700 px-5 py-2.5 text-sm font-medium text-white transition-colors hover:bg-blue-800 focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2">
        See demo with sample data
      </a>
    </div>
    <img src="/dashboard.png" alt="Risk-score dashboard" class="rounded-lg border border-gray-200 shadow-sm max-lg:order-first" />
  </div>
</section>
```
Mobile: stacks, image first via `order-first`. One dominant CTA + text-link secondary (never dual symmetric buttons).

### 17c. Dark hero + product shot (Linear/Raycast)
```html
<section class="relative overflow-hidden bg-gray-950 px-6 py-24 sm:py-32">
  <!-- blueprint grid bg, decorative -->
  <div class="absolute inset-0 bg-[linear-gradient(rgba(255,255,255,.03)_1px,transparent_1px),linear-gradient(90deg,rgba(255,255,255,.03)_1px,transparent_1px)] bg-[size:24px_24px]" aria-hidden="true"></div>
  <div class="relative mx-auto max-w-5xl text-center">
    <h1 class="text-3xl font-bold leading-[1.1] tracking-tight text-white sm:text-5xl text-balance">The keyboard-first launcher</h1>
    <p class="mx-auto mt-5 max-w-[50ch] text-lg leading-relaxed text-gray-400">Run scripts, control apps, manage your workflow in one place.</p>
    <a href="/download" class="mt-8 inline-flex rounded-md bg-white px-5 py-2.5 text-sm font-medium text-gray-900 hover:bg-gray-100 focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-offset-2 focus-visible:ring-offset-gray-950">Download</a>
  </div>
  <img src="/app-dark.png" alt="App command palette" class="relative mx-auto mt-16 max-w-4xl rounded-xl border border-white/10 shadow-2xl" />
</section>
```

### 17f. Code-block hero (dev tools)
```html
<section class="bg-gray-950 px-6 py-24 sm:py-32">
  <div class="mx-auto max-w-6xl grid gap-12 lg:grid-cols-2 lg:items-center">
    <div>
      <h1 class="text-3xl font-bold leading-[1.1] tracking-tight text-white sm:text-4xl">Email for developers</h1>
      <p class="mt-5 max-w-[48ch] text-base leading-relaxed text-gray-400">Send transactional emails with 3 lines. No templates, just an API.</p>
      <a href="/docs" class="mt-8 inline-flex rounded-md bg-white px-5 py-2.5 text-sm font-medium text-gray-900 hover:bg-gray-100">Read the docs</a>
    </div>
    <div class="overflow-x-auto rounded-lg border border-white/10 bg-gray-900 p-5 font-mono text-sm leading-relaxed" role="img" aria-label="Code: send email in 3 lines">
      <pre class="text-gray-300"><code>import { Resend } from 'resend';
const resend = new Resend('re_123');
await resend.emails.send({ to, subject, html });</code></pre>
    </div>
  </div>
</section>
```
Code-block always `role="img"` + `aria-label`; real runnable code, never pseudo.

---

## 18. SAAS LANDING — FULL SECTION FLOW

Anti-monotony reference sequence (each row a different anatomy + width + padding):
```
1. Hero            split / centered          py-32   max-w-6xl
2. Trust bar       logo strip OR metrics      py-8    max-w-5xl   border-y
3. Features        scroll-sticky (pin+scroll) py-24   max-w-6xl   2-col
4. Stats band      full-bleed rhythm break    py-12   bg-gray-50  border-y
5. Features        bento grid (varied cells)  py-24   max-w-6xl   4-col
6. Testimonial     single big quote           py-24   max-w-3xl   bg-gray-50
7. Integrations    logo grid                  py-24   max-w-5xl   6-col
8. Pricing         3-tier cards               py-24   max-w-5xl   3-col
9. FAQ             accordion                  py-24   max-w-3xl
10. CTA            full-width dark band       py-16   full-bleed  bg-gray-900
11. Footer         minimal or structured      py-8    border-t
```

### Scroll-sticky features (Linear signature) — §3 of this flow
```html
<section class="px-6 py-16 sm:py-24" aria-label="Features">
  <div class="mx-auto max-w-6xl grid gap-16 lg:grid-cols-2">
    <div class="hidden lg:block">
      <div class="sticky top-24 aspect-[4/3] overflow-hidden rounded-xl border border-gray-200 bg-gray-100">
        <img id="feature-visual" src="/feature-1.png" alt="" class="h-full w-full object-cover transition-opacity duration-300" />
      </div>
    </div>
    <div class="space-y-24 lg:space-y-48">
      <article class="scroll-mt-24" data-feature="1">
        <h3 class="text-xl font-semibold text-gray-900">Automated risk scoring</h3>
        <p class="mt-3 max-w-prose leading-relaxed text-gray-600">Rules engine, transparent and deterministic — no black box.</p>
        <img src="/feature-1.png" alt="Scoring view" class="mt-6 rounded-lg border lg:hidden" />
      </article>
      <!-- repeat per feature; JS IntersectionObserver swaps the sticky image -->
    </div>
  </div>
</section>
```
Mobile: sticky panel hidden; inline image per feature shows.

---

## 19. DASHBOARD / APP SHELL (Linear / Vercel 2026)

```html
<div class="grid h-dvh grid-cols-[16rem_1fr] grid-rows-[3.5rem_1fr] max-md:grid-cols-1">
  <aside class="row-span-full overflow-y-auto border-r bg-gray-50 max-md:hidden">
    <nav aria-label="Main" class="p-4 space-y-1 text-[13px]">
      <a href="/dashboard" aria-current="page" class="flex items-center gap-2 rounded-md bg-gray-100 px-2.5 py-1.5 font-medium text-gray-900">
        <svg aria-hidden="true" class="h-4 w-4 text-gray-500"><!-- icon --></svg>Dashboard</a>
      <a href="/siswa" class="flex items-center gap-2 rounded-md px-2.5 py-1.5 text-gray-600 hover:bg-gray-50">
        <svg aria-hidden="true" class="h-4 w-4 text-gray-400"><!-- icon --></svg>Siswa</a>
    </nav>
  </aside>
  <header class="sticky top-0 z-10 flex items-center gap-4 border-b bg-white px-4 md:px-6">
    <button type="button" class="md:hidden -ml-2 p-2" aria-label="Open menu" aria-expanded="false" aria-controls="sidebar">
      <svg aria-hidden="true" class="h-5 w-5"><!-- hamburger --></svg>
    </button>
    <h1 class="text-sm font-semibold">Dashboard</h1>
  </header>
  <main class="overflow-y-auto p-4 md:p-6">
    <div class="grid gap-4 sm:grid-cols-2 xl:grid-cols-4"><!-- KPI cards --></div>
    <div class="mt-6 grid gap-6 lg:grid-cols-[2fr_1fr]"><!-- chart + side panel --></div>
  </main>
</div>
```
2026 principle (Linear "calmer interface"): sidebar recedes (`bg-gray-50/80`, dimmed), content takes precedence. Tablet → icon rail `w-14`; mobile → off-canvas drawer.

---

## 20. BENTO GRID — CELL-SPAN RECIPES

A real bento MUST have a dominant cell (≥2×2) + mixed sizes + diverse content. Uniform cells = card grid, NOT bento.

### Apple 4×3 (dominant top-left)
```html
<div class="grid grid-cols-4 grid-rows-3 gap-4 max-md:grid-cols-2">
  <article class="col-span-2 row-span-2 rounded-xl bg-gray-900 p-8 text-white"><!-- dominant: product/video --></article>
  <article class="rounded-xl bg-gray-50 p-6"><!-- stat --></article>
  <article class="rounded-xl bg-gray-50 p-6"><!-- icon feature --></article>
  <article class="rounded-xl bg-gray-50 p-6"><!-- icon feature --></article>
  <article class="rounded-xl bg-gray-50 p-6"><!-- icon feature --></article>
  <article class="col-span-2 rounded-xl bg-gray-50 p-6"><!-- wide: code/testimonial --></article>
</div>
```

### Progressive reflow (2→4→6, Lexington method)
```html
<div class="grid grid-cols-1 gap-4 md:grid-cols-4 lg:grid-cols-6">
  <article class="col-span-full md:col-span-2 md:row-span-2 lg:col-span-3 lg:row-span-2"><!-- dominant --></article>
  <article class="md:col-span-2 lg:col-span-2"><!-- secondary --></article>
  <article class="md:col-span-2 lg:col-span-1"><!-- small --></article>
</div>
```

### Mixed-cell types (stat + media + quote + code)
Vary content per cell — never icon+title+blurb in all six. Use `grid-auto-flow:dense` to fill gaps, `auto-rows-fr` for equal row heights.

---

## 21. E-COMMERCE SKELETONS

### PLP — product grid + sidebar filters
```html
<div class="grid min-h-dvh grid-cols-1 lg:grid-cols-[16rem_1fr]">
  <aside class="sticky top-16 hidden h-[calc(100dvh-4rem)] overflow-y-auto border-r p-5 lg:block">
    <form aria-label="Product filters">
      <fieldset class="border-b pb-4 mb-4">
        <legend class="mb-3 text-sm font-semibold text-gray-900">Category</legend>
        <label class="flex items-center gap-2 text-sm text-gray-700">
          <input type="checkbox" class="h-4 w-4 rounded border-gray-300 text-blue-600 focus:ring-2 focus:ring-blue-500" />
          Sneakers <span class="ml-auto text-gray-400">(42)</span>
        </label>
      </fieldset>
    </form>
  </aside>
  <main class="p-4 sm:p-6">
    <div class="grid grid-cols-2 gap-4 sm:grid-cols-3 xl:grid-cols-4">
      <article class="group">
        <a href="/product/1">
          <div class="aspect-[3/4] overflow-hidden rounded-lg bg-gray-100">
            <img src="" alt="Product name" loading="lazy" class="h-full w-full object-cover transition-transform duration-300 group-hover:scale-[1.03]" />
          </div>
          <h3 class="mt-3 text-sm font-medium text-gray-900">Product Name</h3>
          <p class="text-sm font-semibold tabular-nums text-gray-900">Rp 299.000</p>
        </a>
      </article>
    </div>
  </main>
</div>
```
Mobile: sidebar → filter bottom-sheet/drawer. Note `scale-[1.03]` on image inside `overflow-hidden`, never `scale-105` on the card.

### PDP — gallery + sticky buy box
```html
<main class="mx-auto max-w-7xl px-4 py-8 sm:px-6">
  <div class="grid grid-cols-1 gap-8 lg:grid-cols-[1fr_24rem] lg:items-start">
    <section aria-label="Product images" class="grid grid-cols-1 gap-3 sm:grid-cols-2">
      <img src="" alt="Front view" class="aspect-square w-full rounded-lg object-cover sm:col-span-2" />
      <img src="" alt="Side view" class="aspect-square w-full rounded-lg object-cover" loading="lazy" />
    </section>
    <aside class="lg:sticky lg:top-20 space-y-6">
      <div><h1 class="text-2xl font-bold text-gray-900">Air Max 90</h1><p class="mt-3 text-xl font-semibold tabular-nums">Rp 1.649.000</p></div>
      <fieldset>
        <legend class="mb-2 text-sm font-medium text-gray-900">Size</legend>
        <div class="grid grid-cols-4 gap-2">
          <label class="cursor-pointer">
            <input type="radio" name="size" value="8" class="peer sr-only" />
            <span class="flex items-center justify-center rounded-md border border-gray-300 py-2 text-sm transition-colors peer-checked:border-gray-900 peer-checked:bg-gray-900 peer-checked:text-white peer-focus-visible:ring-2 peer-focus-visible:ring-blue-500 peer-focus-visible:ring-offset-2">8</span>
          </label>
        </div>
      </fieldset>
      <button type="button" class="w-full rounded-full bg-gray-900 px-6 py-4 text-base font-medium text-white hover:bg-gray-800 focus-visible:ring-2 focus-visible:ring-gray-900 focus-visible:ring-offset-2">Add to Bag</button>
    </aside>
  </div>
</main>
```
Size/color selectors = real `<input type="radio">` with `peer-checked:`, never div-click. Cart drawer + multi-step/single-page checkout: see source catalog; checkout = `grid lg:grid-cols-[1fr_28rem]` (form + sticky order summary), mobile = summary-first + sticky bottom pay button in thumb zone.

---

## 22. EDITORIAL / CONTENT / DOCS

### Docs three-column (nav + content + TOC) — Stripe/Mintlify/Nextra
```html
<div class="grid min-h-dvh grid-cols-1 lg:grid-cols-[16rem_1fr_14rem]">
  <aside class="sticky top-0 h-dvh overflow-y-auto border-r p-4 max-md:hidden">
    <nav aria-label="Documentation"><ul role="list" class="space-y-1 text-sm"><!-- tree --></ul></nav>
  </aside>
  <main class="overflow-y-auto">
    <article class="mx-auto max-w-3xl px-6 py-10 prose prose-gray"><!-- MDX --></article>
  </main>
  <aside class="sticky top-0 h-dvh overflow-y-auto py-10 pr-4 max-lg:hidden">
    <nav aria-label="On this page">
      <h2 class="text-xs font-semibold uppercase tracking-wide text-gray-500">On this page</h2>
      <ul role="list" class="mt-3 space-y-2 text-sm text-gray-600"><!-- headings, IntersectionObserver highlights active --></ul>
    </nav>
  </aside>
</div>
```

### Article reading — full-bleed breakouts (Josh Comeau / Ryan Mulligan named lines)
```html
<article class="grid grid-cols-[1fr_min(65ch,100%)_1fr] px-4">
  <div class="col-start-2 space-y-6"><h1 class="text-3xl font-bold tracking-tight">Title</h1><p class="leading-relaxed text-gray-700">Body at measure…</p></div>
  <figure class="col-span-full my-12"><img src="/wide.jpg" alt="" class="w-full rounded-xl" /><figcaption class="mx-auto mt-2 max-w-prose px-4 text-sm text-gray-500">Caption</figcaption></figure>
  <div class="col-start-2 space-y-6"><p class="leading-relaxed text-gray-700">Continues…</p></div>
</article>
```
For full named-line system (content/popout/feature/full) see §5 in core layouts. Changelog: left-date + right-content with vertical connector line, OR sticky month headers. Magazine homepage: `lg:grid-cols-[2fr_1fr]` featured spanning 2 rows + stacked secondaries. Scrollytelling: sticky graphic panel + scrolling text steps (gate with reduced-motion).

---

## 23. AUTH / ONBOARDING / PRICING / UTILITY PAGES

| Page | Skeleton | Key |
|------|----------|-----|
| Login (brand) | `grid min-h-dvh lg:grid-cols-2` — dark brand panel + form | brand panel `hidden lg:flex`, real testimonial |
| Login (minimal) | centered card `max-w-sm` on `bg-gray-50` | social-first OR progressive (email→password) |
| Onboarding wizard | header progress bar + centered step `max-w-md` + footer Back/Continue | `has-[:checked]` role cards, "Skip" text-link, time estimates |
| Empty-state dashboard | centered icon + title + ONE CTA + progressive checklist | human copy, "see sample data" secondary path |
| Pricing | toggle + 3 cards `lg:grid-cols-3` + comparison `<table>` below | recommended = inverted fill + badge (NOT ring-only), ≤5 features/card, `tabular-nums` |
| Settings | secondary nav sidebar + sectioned content `max-w-2xl` + sticky save bar | mobile nav → dropdown; danger zone separated |
| 404 / error | centered `min-h-dvh`: code + heading + home CTA + docs link | error.tsx adds retry button |
| Waitlist | centered, NO nav, single email capture + real count | distraction-free, one goal |
| Status page | overall banner + component uptime bars + incident timeline + subscribe | separate domain, green/yellow/red mapping |
| Search results | persistent search + horizontal filters above + result list + pagination | SaaS = filters horizontal (not sidebar), `<mark>` highlights |

### Onboarding wizard step (intent-based)
```html
<fieldset class="space-y-3">
  <legend class="sr-only">Pilih peran</legend>
  <label class="flex cursor-pointer items-center gap-4 rounded-lg border border-gray-200 p-4 transition-colors hover:border-blue-300 has-[:checked]:border-blue-500 has-[:checked]:bg-blue-50">
    <input type="radio" name="role" value="guru" class="h-4 w-4 border-gray-300 text-blue-600" />
    <div><p class="text-sm font-medium text-gray-900">Wali Kelas</p><p class="text-xs text-gray-500">Pantau siswa & terima saran intervensi</p></div>
  </label>
</fieldset>
```

---

## 24. MOBILE-FIRST / PWA / RESPONSIVE

### PWA app shell — bottom tab (flex child, NOT fixed)
```html
<div class="flex h-dvh flex-col overflow-hidden">
  <main class="min-h-0 flex-1 overflow-y-auto"><!-- active tab --></main>
  <nav aria-label="Main" class="shrink-0 border-t bg-white pb-[env(safe-area-inset-bottom)]">
    <ul role="list" class="flex items-center justify-around px-2 py-2">
      <li><a href="/home" aria-current="page" class="flex flex-col items-center gap-0.5 text-xs font-medium text-blue-700"><svg aria-hidden="true" class="h-6 w-6"><!-- icon --></svg>Home</a></li>
      <li><a href="/search" class="flex flex-col items-center gap-0.5 text-xs text-gray-500"><svg aria-hidden="true" class="h-6 w-6"><!-- icon --></svg>Search</a></li>
    </ul>
  </nav>
</div>
```
**Critical:** never `fixed bottom-0` for PWA bottom nav — iOS standalone places it above home indicator. Use flex child + `h-dvh` container + `pb-[env(safe-area-inset-bottom)]`. Requires `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">`.

### Responsive nav decision matrix
| Viewport | Pattern | Width |
|----------|---------|-------|
| <640 mobile | bottom tab bar (3–5) OR hamburger→drawer | full |
| 768 tablet | navigation rail (icon-only) | w-16 |
| 1024 desktop | collapsible sidebar | w-56 |
| >1280 | full sidebar + optional right panel | w-56 + content + w-64 |

Thumb-zone: primary mobile CTA bottom-center, full-width, ≥48px target. Sticky add-to-cart bar appears on scroll (IntersectionObserver) — bottom on mobile, top on desktop.

---

## 25. CUTTING-EDGE CSS TECHNIQUES (2025–2026)

Browser-support table (June 2026): ✅ ship = all evergreen · ⚠️ progressive = enhance with `@supports`.

| Technique | Status | Use |
|-----------|--------|-----|
| **Subgrid** | ✅ ship | align card internals (title/body/CTA) across a row |
| **Container queries** `@container` | ✅ ship | component adapts to parent width, not viewport |
| **`:has()`** | ✅ ship | parent-aware styling (form-with-invalid, grid by child count) |
| **`dvh`/`svh`/`lvh`** | ✅ ship | mobile-correct full-height (replace `100vh`) |
| **Scroll-snap** | ✅ ship | carousels, full-page sections |
| **`@starting-style`** | ✅ ship | animate elements entering DOM (modals/toasts), no JS lib |
| **`text-wrap: balance`/`pretty`** | ✅ ship | headings balance, body pretty — no orphans |
| **`light-dark()` / `color-mix()` / OKLCH** | ✅ ship | theme tokens + derived hover states |
| **View Transitions (SPA)** | ✅ ship | route/tab crossfade, shared element |
| **View Transitions (MPA)** | ⚠️ Chrome | one line `@view-transition{navigation:auto}` |
| **Scroll-driven animations** `view()`/`scroll()` | ⚠️ progressive | parallax, reveal, progress bar — no JS |
| **Anchor positioning** | ⚠️ progressive | tooltips/popovers tethered without JS positioning |

### Subgrid — cross-card alignment
```html
<div class="grid grid-cols-[repeat(auto-fill,minmax(18rem,1fr))] gap-6">
  <article class="grid grid-rows-subgrid row-span-3 gap-[inherit] rounded-lg border p-6">
    <h3 class="text-base font-semibold">Title</h3>
    <p class="text-sm text-gray-600">Variable-length body still aligns across the row.</p>
    <a href="#" class="self-end text-sm font-medium text-blue-700">Learn more →</a>
  </article>
</div>
```

### Scroll-driven reveal (no JS, gated)
```css
@supports (animation-timeline: view) {
  @media (prefers-reduced-motion: no-preference) {
    .reveal { animation: fade-up linear both; animation-timeline: view(); animation-range: entry 0% entry 40%; }
    @keyframes fade-up { from { opacity:0; transform:translateY(16px) } to { opacity:1; transform:none } }
  }
}
```

### Anchor positioning — tooltip without JS
```css
.trigger { anchor-name: --t; }
.tooltip { position: absolute; position-anchor: --t; position-area: top; margin-bottom: 8px; position-try-fallbacks: flip-block; }
```

### `@starting-style` — modal entrance
```css
dialog[open] { opacity:1; scale:1; transition: opacity 250ms, scale 250ms, display 250ms allow-discrete; }
@starting-style { dialog[open] { opacity:0; scale:.95; } }
```

### Sticky stacking cards (Linear/Stripe feature scroll)
```html
<div class="relative">
  <section class="sticky top-0 flex min-h-dvh items-center rounded-xl bg-white p-8 shadow-sm" style="z-index:1">…1…</section>
  <section class="sticky top-0 flex min-h-dvh items-center rounded-xl bg-gray-50 p-8 shadow-md" style="z-index:2">…2…</section>
  <section class="sticky top-0 flex min-h-dvh items-center rounded-xl bg-gray-100 p-8 shadow-lg" style="z-index:3">…3…</section>
</div>
```

### dvh decision
`100svh` → above-the-fold (no jump) · `100dvh` → immersive app shell/hero · `100lvh` → decorative bg. Always pair fallback `min-height:100vh` then `100dvh`.

---

## PART 2 SOURCES
Linear, Vercel (+ Geist DS / rauno.me), Stripe, Raycast, Clerk, Resend, Framer, Apple, Nike, NYT, Mintlify, Nextra · Evil Martians "100 dev-tool landing pages" (2025) · Josh Comeau (full-bleed, subgrid, scroll-driven) · Ryan Mulligan (layout breakouts, marquee) · web.dev / MDN / WebKit / chrome.dev (subgrid, container queries, scroll-driven, view transitions, anchor positioning, `@starting-style`) · SaaSFrame (bento, 2026 trends) · NNG · Refactoring UI · Tailwind v4 docs · utopia.fyi (fluid clamp).
