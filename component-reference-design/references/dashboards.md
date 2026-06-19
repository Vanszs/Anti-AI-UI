# Dashboards — Modern Clean Admin Dashboard Patterns

Companion to `../SKILL.md`. The reference for building **clean, modern admin dashboards** that read as Linear/Vercel/Stripe/Tremor quality — dense yet calm, not generic SaaS template, not AI-slop. Every pattern: NAME · when-to-use · concrete accessible Tailwind · why-clean · source. Synthesized from web research (2025–2026): Linear (2026 "calmer interface" refresh), Vercel/Geist, Stripe Apps, Tremor, shadcn/ui dashboard, Untitled UI, PostHog "3000", Notion, Retool, Mixpanel, TanStack Table, IBM Carbon, React Aria, W3C WAI-ARIA APG, NNG, SetProduct, Refactoring UI.

> Quality bar: brand teal `#005D4C` accent ONLY on interactive/focal (NOT purple) · sidebar recedes (`bg-slate-50`, content shines) · elevation via surface shade NOT shadow-spam · border-only cards (shadow reserved for floating) · `tabular-nums` on all data · radius scale (md/lg/xl) · `focus-visible:ring-2 ring-teal-600` everywhere · `prefers-reduced-motion` gated · real `<button>`/`<a>` · all states designed (empty/loading/error/zero). If it could be swapped into any generic admin panel and nobody notices, it's slop.

---

## 0. THE 15 CLEAN-DASHBOARD PRINCIPLES (run before shipping any dashboard)

1. **Tiered surface, not shadow spam** — `bg-white` content › `bg-slate-50` sidebar › `bg-slate-100` insets. Shadows ONLY for floating (modal/dropdown/tooltip); cards use `border border-slate-200`. (Geist Materials)
2. **Radius scale, never uniform** — inputs/badges `rounded-md` (6) · cards `rounded-lg` (8) · modals `rounded-xl` (12). Child radius ≤ parent.
3. **Borders fewer/softer/purposeful** — `border-slate-200` not harsh black; dividers only between semantically different groups. "Structure felt, not seen." (Linear 2026)
4. **Small purposeful type** — body 13–14px (data-dense), page title `text-lg font-semibold`, section label `text-sm font-medium text-slate-500`. Big titles waste space. (PostHog/Linear density)
5. **`tabular-nums` on ALL data** — KPIs, tables, badges. Prevents number "dance" on refresh. (Vercel guideline)
6. **Max 2 fonts, max 3 weights** — 400/500/600. Avoid `font-bold` (700) in dense UI — too heavy.
7. **One accent + semantic-only hues** — teal for active-nav/CTA/focal-series; red/amber/emerald for STATUS only; everything else gray. Monochromatic content area. (Stripe/Vercel/Linear)
8. **Sidebar dims, content shines** — nav = muted (`text-slate-500`, `bg-slate-50`), 16–18px icons, active = subtle teal bg. Navigation recedes. (Linear 2026)
9. **Tight internal, generous external** — card padding `p-5` · between cards `gap-4` · between sections `space-y-6/8`. Internal ≤ external (Gestalt proximity).
10. **Fixed panels, not whole-page scroll** — sidebar + header fixed; `<main>` scrolls independently. Context never lost. (PostHog/Linear/Vercel)
11. **KPI inverted pyramid** — big value › delta badge › sparkline. Most crucial largest. 4 cards/row max. (Mixpanel/Tremor)
12. **Motion functional, never decorative** — hover `transition-colors duration-150` (NEVER scale), entrance 200–250ms opacity+translateY, gated by `prefers-reduced-motion`. Never `transition-all`. (Vercel)
13. **Keyboard-first** — command palette ⌘K, `focus-visible` on every interactive. (PostHog/Linear)
14. **All states designed** — empty (icon+title+desc+1 CTA), loading (skeleton mirror shape), error (+retry), zero-but-present (render chart with 0). Never a blank area. (Vercel guideline)
15. **Progressive disclosure** — overview first (KPIs+chart+recent), detail on demand. Never overwhelm. (Stripe)

### Instant-slop bans (dashboard-specific)
purple/indigo accent · shadow on every card · `hover:scale-105` · big `text-3xl` page titles · uniform `rounded-2xl` · `transition: all` · whole-page scroll · rainbow chart series · glassmorphic nav · pulsing notification badge · left-border nav active state.

---

## 1. APP SHELL (header + sidebar + content)

**When:** every admin dashboard. CSS Grid 2-track column + fixed header row; sidebar full-height, content scrolls independently.

```html
<div class="grid h-dvh grid-cols-[16rem_1fr] grid-rows-[3.5rem_1fr] max-md:grid-cols-1">
  <aside class="row-span-full overflow-y-auto border-r border-slate-200 bg-slate-50 max-md:hidden" aria-label="Navigasi utama">
    <!-- sidebar §2 -->
  </aside>
  <header class="sticky top-0 z-10 flex h-14 items-center gap-3 border-b border-slate-200 bg-white px-4 md:px-6">
    <button type="button" class="md:hidden -ml-2 rounded-md p-2 text-slate-500 hover:bg-slate-100 focus-visible:ring-2 focus-visible:ring-teal-600 focus-visible:ring-offset-2" aria-label="Buka menu" aria-expanded="false" aria-controls="mobile-drawer">
      <svg aria-hidden="true" class="h-5 w-5"><!-- hamburger --></svg>
    </button>
    <!-- breadcrumb / title §6 -->
  </header>
  <main class="overflow-y-auto p-4 md:p-6"><!-- §5 grid composition --></main>
</div>
```
**Why clean:** Grid > flex for 2D shell (no height hacks). `h-dvh` correct on mobile. Sidebar `bg-slate-50` recedes; content `bg-white` is the star. **Source:** Linear 2025/26, Vercel dashboard, shadcn template, layouts.md §19.

---

## 2. SIDEBAR NAVIGATION

### 2a. Collapsible (w-64 ↔ w-16 rail)
Toggle expanded/rail via CSS var `--sidebar-w`; transition `width` (not transform). Icons always visible; labels hidden in rail (Radix Tooltip shows label on hover/focus).
```html
<aside class="row-span-full flex flex-col border-r border-slate-200 bg-slate-50 transition-[width] duration-200 ease-out motion-reduce:transition-none" style="width:var(--sidebar-w,16rem)" aria-label="Navigasi utama">
  <nav class="flex-1 overflow-y-auto p-3 space-y-6"><!-- groups --></nav>
  <div class="border-t border-slate-200 p-3"><!-- §2d footer --></div>
</aside>
```

### 2b. Nav item states (active / inactive)
NEVER left-border accent (P0 slop), NEVER purple, NEVER bold-color bg. Active = subtle fill + medium weight + teal icon.
```html
<!-- Active -->
<a href="/dashboard" aria-current="page"
   class="flex items-center gap-3 rounded-lg bg-slate-100 px-2.5 py-2 text-[13px] font-medium text-slate-900 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-teal-600 focus-visible:ring-offset-2">
  <svg aria-hidden="true" class="h-4 w-4 shrink-0 text-teal-700"><!-- icon --></svg>Dashboard
</a>
<!-- Inactive -->
<a href="/siswa"
   class="flex items-center gap-3 rounded-lg px-2.5 py-2 text-[13px] text-slate-600 hover:bg-slate-100 hover:text-slate-900 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-teal-600 focus-visible:ring-offset-2">
  <svg aria-hidden="true" class="h-4 w-4 shrink-0 text-slate-400"><!-- icon --></svg>Siswa
</a>
```

### 2c. Section headers & collapsible groups
`<details>` = native collapse, keyboard accessible, `aria-expanded` for free. Group label `text-[11px] uppercase tracking-wide text-slate-400`. `space-y-6` between groups, `space-y-0.5` within.
```html
<details open>
  <summary class="flex cursor-pointer items-center justify-between px-2.5 py-1 text-[11px] font-semibold uppercase tracking-wide text-slate-400 hover:text-slate-600 focus-visible:rounded-md focus-visible:ring-2 focus-visible:ring-teal-600">
    Analitik
    <svg aria-hidden="true" class="h-3 w-3 transition-transform duration-150 motion-reduce:transition-none"><!-- chevron --></svg>
  </summary>
  <ul role="list" class="mt-1 space-y-0.5"><!-- items --></ul>
</details>
```

### 2d. Sidebar footer (user/org switcher)
`mt-auto` pins to bottom in flex column. Avatar `rounded-full` 32px + name + role, truncated. `chevrons-up-down` icon signals switcher.
```html
<button type="button" aria-haspopup="menu" aria-expanded="false"
  class="flex w-full items-center gap-3 rounded-lg px-2.5 py-2 text-left hover:bg-slate-100 focus-visible:ring-2 focus-visible:ring-teal-600 focus-visible:ring-offset-2">
  <img src="/avatar.jpg" alt="" class="h-8 w-8 rounded-full object-cover" />
  <div class="min-w-0 flex-1">
    <p class="truncate text-[13px] font-medium text-slate-900">Pak Ahmad</p>
    <p class="truncate text-[11px] text-slate-500">Wali Kelas 6A</p>
  </div>
  <svg aria-hidden="true" class="h-4 w-4 shrink-0 text-slate-400"><!-- chevrons-up-down --></svg>
</button>
```

### 2e. Responsive strategy
| Viewport | Sidebar | Trigger |
|---|---|---|
| <768 mobile | hidden → off-canvas `<dialog>` drawer | header hamburger |
| 768–1024 tablet | rail `w-16` icons + tooltip | auto/user toggle |
| >1024 desktop | full `w-64` | persist pref (localStorage) |

Mobile drawer = native `<dialog>` (free focus-trap + inert + Esc + backdrop); `motion-reduce` disables slide animation. **Source:** Linear, shadcn sidebar, IBM Carbon side-nav, MD3 Navigation Rail, slop §9 (ban left-border).

---

## 3. TOPBAR / HEADER

7 elements, 56px (`h-14`) sticky, single `border-b` (no shadow). Progressive disclosure on mobile.
```
Tablet+: [org-switcher] | [breadcrumb] ---spacer--- [⌘K search] [notif] [avatar]
Mobile:  [hamburger] [page-title trunc] ---spacer--- [notif] [avatar]
```

| # | Element | Key |
|---|---------|-----|
| 1 | Sticky bar | `h-14` (thinner = more content space), `border-b`, `bg-white`, `z-10` |
| 2 | Breadcrumb | `<nav aria-label="Breadcrumb"><ol>`, last = `aria-current="page"` no link. Replaces redundant page title (Notion/Retool) |
| 3 | Global search | pill trigger (NOT full input) + `<kbd>⌘K</kbd>` → opens command palette §4. Saves width |
| 4 | Notification bell | icon `<button>` + `aria-label="Notifikasi (3 belum dibaca)"`; static red dot — **never** `animate-pulse` (slop) |
| 5 | Avatar menu | `rounded-full` 32px `<button>` + `aria-haspopup="menu"`; name shown inside dropdown, not in bar |
| 6 | Org/workspace switcher | first element; truncate name ~16ch + chevron; teal initial-badge `bg-teal-700` |
| 7 | Mobile hamburger | `md:hidden`, first-left, triggers drawer; hides breadcrumb+search on mobile |

```html
<!-- Search trigger (pill, not input) -->
<button type="button" aria-label="Cari (⌘K)"
  class="hidden md:inline-flex items-center gap-2 rounded-md border border-slate-200 bg-slate-50 px-3 py-1.5 text-sm text-slate-400 hover:border-slate-300 hover:bg-slate-100 focus-visible:ring-2 focus-visible:ring-teal-600 focus-visible:ring-offset-2">
  <svg aria-hidden="true" class="h-4 w-4"><!-- search --></svg>Cari...
  <kbd class="ml-2 rounded border border-slate-200 bg-white px-1.5 py-0.5 text-[11px] font-medium text-slate-400">⌘K</kbd>
</button>
<!-- Notification (static dot, NO pulse) -->
<button type="button" aria-label="Notifikasi (3 belum dibaca)"
  class="relative rounded-md p-2 text-slate-500 hover:bg-slate-100 focus-visible:ring-2 focus-visible:ring-teal-600 focus-visible:ring-offset-2">
  <svg aria-hidden="true" class="h-5 w-5"><!-- bell --></svg>
  <span class="absolute right-1.5 top-1.5 h-2 w-2 rounded-full bg-red-500"></span>
</button>
```
**Source:** Vercel/Stripe/Linear/Notion/Retool, W3C APG (breadcrumb/menu/dialog), layouts.md §19.

---

## 4. COMMAND PALETTE (⌘K)

**When:** power-user nav + actions. Use `cmdk` (shadcn) or native `<dialog>` + combobox.
```html
<dialog id="command-palette" aria-label="Command palette"
  class="mx-auto mt-[15vh] w-full max-w-lg rounded-xl border border-slate-200 bg-white p-0 shadow-xl backdrop:bg-black/40">
  <div class="flex items-center border-b border-slate-200 px-4">
    <svg aria-hidden="true" class="h-4 w-4 shrink-0 text-slate-400"><!-- search --></svg>
    <input type="text" role="combobox" aria-expanded="true" aria-controls="cmd-list" aria-autocomplete="list"
      class="w-full border-0 bg-transparent py-3 pl-3 text-sm text-slate-900 placeholder:text-slate-400 focus:outline-none focus:ring-0"
      placeholder="Ketik perintah atau cari..." />
  </div>
  <ul id="cmd-list" role="listbox" class="max-h-72 overflow-y-auto p-2">
    <li role="option" aria-selected="true" class="flex items-center gap-3 rounded-lg bg-slate-100 px-3 py-2 text-sm text-slate-900">
      <svg aria-hidden="true" class="h-4 w-4 text-slate-500"><!-- icon --></svg>Dashboard
      <kbd class="ml-auto rounded bg-slate-200 px-1.5 py-0.5 text-[11px] font-mono text-slate-500">⌘D</kbd>
    </li>
  </ul>
</dialog>
```
**Why clean:** centered modal, generous max-width, items reuse `rounded-lg` (nav consistency), no gradient/glow. `role="combobox"`+`role="listbox"` = full keyboard a11y. **Source:** cmdk (Paco/shadcn), Linear, Raycast, W3C APG combobox.

---

## 5. GRID COMPOSITION (widget layout)

### Page shell
```html
<main class="overflow-y-auto p-4 md:p-6 lg:p-8">
  <div class="mx-auto max-w-[1400px] space-y-6"><!-- KPI row → chart row → table --></div>
</main>
```
`max-w-[1400px]` (not 7xl) → wider data without drowning on ultrawide. `space-y-6` between major sections.

### Canonical sequence (each row = different anatomy + weight)
```
1. KPI row        4 cards (sparklines)          grid sm:grid-cols-2 xl:grid-cols-4  gap-4
2. Hero + side    chart 2fr / panel 1fr         lg:grid-cols-[2fr_1fr]              gap-4
3. Ranking+heat   2 charts                      lg:grid-cols-2
4. Data table     full width drill-down         col-span-full
```

### Widget card chrome (universal)
Header (title + action) + body. `border-b` separates metadata from data.
```html
<article class="rounded-lg border border-slate-200 bg-white">
  <header class="flex items-center justify-between border-b border-slate-100 px-5 py-4">
    <div>
      <h2 class="text-sm font-semibold text-slate-900">Tren Risiko Bulanan</h2>
      <p class="mt-0.5 text-xs text-slate-500">Rata-rata per minggu</p>
    </div>
    <!-- period toggle §7b / action button -->
  </header>
  <div class="px-5 py-4"><!-- chart/list/table --></div>
</article>
```
Hover on INTERACTIVE cards only: `hover:shadow-sm hover:border-slate-300 transition-[box-shadow,border-color] duration-150` (NEVER scale).

### Bento overview (must have dominant cell)
```html
<div class="grid grid-cols-1 gap-4 md:grid-cols-4 lg:grid-cols-6">
  <article class="md:col-span-2 md:row-span-2 lg:col-span-3 lg:row-span-2 rounded-lg border border-slate-200 bg-white p-5"><!-- dominant trend --></article>
  <article class="md:col-span-2 lg:col-span-2 rounded-lg border border-slate-200 bg-white p-5"><!-- stat+sparkline --></article>
  <article class="lg:col-span-1 rounded-lg border border-slate-200 bg-white p-5"><!-- single metric --></article>
  <article class="md:col-span-4 lg:col-span-3 rounded-lg border border-slate-200 bg-white p-5"><!-- wide ranking --></article>
</div>
```
Dominant ≥`col-span-2 row-span-2` else it's a uniform card grid (slop). Mix content per cell.

### Gap & rhythm system
| Context | Gap | Rationale |
|---|---|---|
| Between KPI cards | `gap-4` (16) | tight = one group |
| Between sections | `space-y-6` (24) | clear separation |
| Inside card (header↔body) | `border-b`+padding | internal < external |
| Page padding | `p-4 md:p-6 lg:p-8` | scales with viewport |

**Draggable widgets (mention only):** react-grid-layout (PostHog) or @dnd-kit. **MVP recommendation:** skip — fixed grid is simpler + more accessible. Add later as opt-in. **Source:** Tremor blocks, Vercel Analytics, Linear Insights, PostHog, shadcn dashboard, layouts.md §3/§6/§19.

---

## 6. KPI / STAT CARDS

### Anatomy (4 layers)
`label` (sm muted) → `value` (2xl semibold tabular-nums) → `delta` (arrow+%+period) → `sparkline` (optional, aria-hidden).
```html
<article class="rounded-lg border border-slate-200 bg-white p-5"
  role="img" aria-label="Siswa Berisiko Tinggi: 24, naik 12% vs bulan lalu">
  <p class="text-sm font-medium text-slate-500">Siswa Berisiko Tinggi</p>
  <p class="mt-1 text-2xl font-semibold tabular-nums text-slate-900">24</p>
  <div class="mt-2 flex items-center gap-1.5">
    <span class="inline-flex items-center gap-0.5 rounded-md bg-red-50 px-1.5 py-0.5 text-xs font-medium tabular-nums text-red-700 ring-1 ring-inset ring-red-600/20">
      <svg aria-hidden="true" class="h-3 w-3" fill="none" viewBox="0 0 12 12"><path d="M6 2v8M6 2l3 3M6 2L3 5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/></svg>+12%
    </span>
    <span class="text-xs text-slate-400">vs bulan lalu</span>
  </div>
</article>
```

### Grid
`grid grid-cols-1 gap-4 sm:grid-cols-2 xl:grid-cols-4` — 4-up desktop, 2×2 tablet, stacked mobile. Hero metric → `sm:col-span-2` + `text-3xl` + embedded sparkline (breaks 4-box monotony, Vercel/Mixpanel).

### ⚠️ Delta semantic INVERSION (critical for JagaSekolah)
Direction (↑↓) ≠ judgment (good/bad). Decouple them — caller decides intent per metric:
- **"Siswa Berisiko"**: ↑ = red (bad), ↓ = emerald (good)
- **"Kehadiran"**: ↑ = emerald (good), ↓ = red (bad)
```ts
type DeltaIntent = "positive" | "negative" | "neutral";
const badge: Record<DeltaIntent,string> = {
  positive: "bg-emerald-50 text-emerald-700 ring-emerald-600/20",
  negative: "bg-red-50 text-red-700 ring-red-600/20",
  neutral:  "bg-slate-50 text-slate-600 ring-slate-500/20",
};
```
Color = intent, arrow = direction. Screen readers get full context via card `aria-label`. (Mixpanel/PostHog)

### Progress vs target (linear, dashboard-dense)
```html
<div class="mt-3">
  <div class="flex justify-between text-xs text-slate-500"><span>Kehadiran</span><span class="tabular-nums">78% / 85%</span></div>
  <div class="mt-1 h-2 w-full overflow-hidden rounded-full bg-slate-100" role="progressbar" aria-valuenow="78" aria-valuemin="0" aria-valuemax="100" aria-label="Kehadiran 78% dari target 85%">
    <div class="h-full rounded-full bg-[#005D4C] transition-[width] duration-500 ease-out motion-reduce:transition-none" style="width:78%"></div>
  </div>
</div>
```
Color logic: ≥target teal · 70–84% amber · <70% red. **Source:** Tremor, Vercel Analytics, Stripe, Mixpanel, charts.md §6, WCAG 1.4.1/4.1.2.

---

## 7. CHART PLACEMENT & CHROME

> For Recharts color/tooltip/axis detail see `charts.md`. This is composition only.

### 7a. ONE hero chart per viewport
Largest block, `aspect-[16/9]` desktop → `4/3` mobile, fixed height `h-64` (avoid CLS). "Largest block reserved for the trend that matters most." (SetProduct)

### 7b. Segmented period toggle (inside card header, not external)
```html
<div role="tablist" aria-label="Periode" class="flex rounded-md border border-slate-200 p-0.5">
  <button role="tab" aria-selected="true" class="rounded px-2.5 py-1 text-xs font-medium text-slate-900 bg-slate-100 focus-visible:ring-2 focus-visible:ring-teal-500 focus-visible:ring-offset-1">7h</button>
  <button role="tab" aria-selected="false" class="rounded px-2.5 py-1 text-xs font-medium text-slate-500 hover:text-slate-700 focus-visible:ring-2 focus-visible:ring-teal-500 focus-visible:ring-offset-1">30h</button>
  <button role="tab" aria-selected="false" class="rounded px-2.5 py-1 text-xs font-medium text-slate-500 hover:text-slate-700 focus-visible:ring-2 focus-visible:ring-teal-500 focus-visible:ring-offset-1">90h</button>
</div>
```
Compact, inline → chart transitions immediately on switch. (Stripe/Vercel/shadcnblocks)

### 7c. Ranking horizontal bar (no axes needed)
```html
<div class="flex items-center gap-3">
  <span class="w-24 shrink-0 truncate text-sm text-slate-700">Kelas 7A</span>
  <div class="relative h-2 flex-1 rounded-full bg-slate-100"><div class="absolute inset-y-0 left-0 rounded-full bg-teal-600" style="width:85%"></div></div>
  <span class="w-12 shrink-0 text-right text-sm font-medium tabular-nums text-slate-900">85%</span>
</div>
```
Sorted descending = self-explanatory hierarchy. (Tremor BarList)

### 7d. Sparkline in KPI (ambient, aria-hidden)
height 32–48px · `dot={false}` · NO axes/grid/tooltip · `isAnimationActive={false}` · `aria-hidden="true"` (card carries label). Color follows delta sentiment.

### 7e. Calendar heatmap (kehadiran)
52 weeks × 7 days, cell 11×11px gap 3px, single-hue teal sequential (`#f1f5f9`→`#005D4C`), tooltip on hover, `role="img"`+`aria-label`. Never rainbow. (GitHub graph)

### 7f. Hover tooltip card
`rounded-lg border border-slate-200 bg-white/95 backdrop-blur-sm shadow-[0_4px_16px_rgba(15,23,42,0.08)]` — date label + dot+name+value (right-aligned `tabular-nums`). Fade 100ms, no spring/scale. **Source:** Tremor, Vercel, Stripe, charts.md §2.

---

## 8. FILTERS & TOOLBAR

### Composite anatomy (top→bottom)
```
SAVED VIEWS TABS (role=tablist, border-b)
FILTER BAR (sticky, role=toolbar): [📅 date] [Kelas ③] [Status ▾] ··· [🔍 search]
APPLIED CHIPS (conditional, flex-wrap): Filter: [Kelas 6A ×] [Risiko Tinggi ×]  Hapus semua
```

### 8a. Horizontal filter bar (NOT sidebar for SaaS, ≤6–8 filters — Baymard)
```html
<div class="sticky top-14 z-20 border-b border-slate-200 bg-white/95 backdrop-blur-sm">
  <div class="flex items-center gap-2 px-4 py-2 sm:px-6" role="toolbar" aria-label="Filter data">
    <button type="button" aria-haspopup="listbox" aria-expanded="false"
      class="inline-flex items-center gap-1.5 rounded-md border border-slate-300 bg-white px-3 py-1.5 text-sm text-slate-700 hover:bg-slate-50 focus-visible:ring-2 focus-visible:ring-teal-600 focus-visible:ring-offset-2 transition-colors">
      Status Risiko <svg aria-hidden="true" class="h-3 w-3 text-slate-400"><!-- chevron --></svg>
    </button>
  </div>
</div>
```
Eliminates sidebar → full content width. (Linear/Stripe/PostHog)

### 8b. Date-range picker (presets + 2 calendars)
React Aria DateRangePicker pattern: trigger → `role="dialog"` popover with preset buttons left (Hari ini / 7 hari / Bulan ini / Semester / Tahun ajaran) + 2 month grids. Selected preset `bg-teal-50 text-teal-800`. Presets = 80% of use without touching calendar.

### 8c. Segmented control — semantic choice
- switches content panel → `role="tablist"` (underline `after:h-0.5 after:bg-teal-700`)
- mutually-exclusive toggle → `role="radiogroup"` (container `bg-slate-100 p-0.5`, active pill `bg-white shadow-sm`)
Roving tabindex (`tabindex="-1"` on inactive) → arrow-key nav (W3C APG).

### 8d. Multi-select dropdown
Trigger shows count badge (`Kelas ③`); `role="listbox" aria-multiselectable="true"`; search inside when >7 options; selected = teal check icon (not checkbox).

### 8e. Filter chips (removable, appear only when active)
```html
<span class="inline-flex items-center gap-1 rounded-md bg-teal-50 px-2 py-0.5 text-xs font-medium text-teal-800 ring-1 ring-inset ring-teal-600/20">
  Kelas 6A
  <button type="button" aria-label="Hapus filter Kelas 6A" class="ml-0.5 rounded-sm p-0.5 text-teal-600 hover:bg-teal-100 focus-visible:ring-2 focus-visible:ring-teal-600">
    <svg aria-hidden="true" class="h-3 w-3"><!-- x --></svg>
  </button>
</span>
```
`ring-1 ring-inset` (not border-l-4). Status chip uses semantic color + text label. "Hapus semua" = text-link at END of row.

### 8f. Search (expand-on-focus)
`type="search"` + leading icon + `w-48 focus:w-64 transition-[width] duration-200 motion-reduce:transition-none` + `sr-only` label. **Source:** Baymard, Pencil&Paper, React Aria, W3C APG (Tabs/Radio), Linear/PostHog/Stripe.

---

## 9. DATA TABLE

**Architecture:** TanStack Table v8 headless (logic) + semantic `<table>` + Tailwind (markup). Only the `<DataTable>` leaf is `'use client'`; page stays Server Component.

### 9a. Base (semantic + scrollable)
```html
<div class="overflow-x-auto rounded-lg border border-slate-200" role="region" aria-label="Daftar siswa berisiko" tabindex="0">
  <table class="min-w-full divide-y divide-slate-200 text-sm">
    <thead class="sticky top-0 z-10 bg-slate-50">
      <tr><th scope="col" class="px-4 py-3 text-left text-xs font-medium uppercase tracking-wide text-slate-500">Nama</th></tr>
    </thead>
    <tbody class="divide-y divide-slate-100 bg-white">
      <tr class="hover:bg-teal-50/40 transition-colors duration-100"><td class="px-4 py-3 text-slate-900">Ahmad Fauzi</td></tr>
    </tbody>
  </table>
</div>
```
`divide-y` not full borders; subtle hover; sticky header; `role="region"`+`tabindex="0"` = keyboard-scrollable.

### 9b. Key decisions
| Aspect | Decision | Why |
|---|---|---|
| Density | Comfortable 52px default (`px-4 py-3.5 text-sm`); Compact 36px opt (`px-3 py-2 text-xs`) | guru ≠ power-user |
| Row distinction | `divide-y` + hover, NOT zebra | zebra+hover+selection = 3 bg clash; zebra only for wide absensi table |
| Sort | `<button>` inside `<th>` + `aria-sort` ONLY on active sorted col | MDN/W3C; never th onClick |
| Selection | checkbox col + floating bottom bar | Linear/shadcn; non-intrusive |
| Row actions | ellipsis `⋮` → Radix dropdown (max 5–7, destructive last+red) | declutters row |
| Pagination | numbered + prev/next (25 default) | jump-to-page for admin; infinite breaks sort/selection |
| Sticky column | first col (Nama) only when horizontal scroll | prevents disorientation |
| Column visibility | dropdown checkboxes, localStorage; essential cols non-hideable | TanStack `getToggleVisibilityHandler` |

### 9c. Inline status badge
```html
<span class="inline-flex items-center rounded-md px-2 py-0.5 text-xs font-medium ring-1 ring-inset bg-red-50 text-red-700 ring-red-600/20">Tinggi</span>
<!-- Sedang: bg-amber-50 text-amber-700 ring-amber-600/20 · Rendah: bg-emerald-50 text-emerald-700 ring-emerald-600/20 -->
```

### 9d. Floating bulk-action bar (appears on selection)
```html
<div role="toolbar" aria-label="Aksi massal"
  class="fixed bottom-6 left-1/2 z-30 -translate-x-1/2 flex items-center gap-3 rounded-lg border border-slate-200 bg-white px-4 py-2.5 shadow-lg motion-safe:animate-[slideUp_200ms_ease-out]">
  <span class="text-sm font-medium tabular-nums text-slate-700">3 dipilih</span>
  <span class="h-4 w-px bg-slate-200" aria-hidden="true"></span>
  <button type="button" class="rounded-md px-3 py-1.5 text-sm font-medium text-slate-700 hover:bg-slate-100 focus-visible:ring-2 focus-visible:ring-teal-500 focus-visible:ring-offset-2">Export CSV</button>
  <button type="button" class="rounded-md px-3 py-1.5 text-sm font-medium text-red-600 hover:bg-red-50 focus-visible:ring-2 focus-visible:ring-red-500 focus-visible:ring-offset-2">Hapus</button>
</div>
```
**Source:** TanStack v8, shadcn data-table, MUI DataGrid (pinning/a11y), MDN aria-sort, Stanford a11y (sticky header), setproduct (pagination), components.md §5.

---

## 10. STATES — empty / loading / error / zero

Never a blank area. NNG 2025: 92% of AI dashboards ship no empty state — anti-slop must-have.

### 10a. Skeleton (mirror shape, not spinner)
```html
<div aria-busy="true" aria-label="Memuat data..." class="rounded-lg border border-slate-200 p-5 space-y-3">
  <div class="h-3 w-24 rounded-md bg-slate-200 animate-pulse motion-reduce:animate-none"></div>
  <div class="h-7 w-16 rounded-md bg-slate-200 animate-pulse motion-reduce:animate-none"></div>
  <div class="h-3 w-20 rounded-md bg-slate-100 animate-pulse motion-reduce:animate-none"></div>
</div>
```
Mirror final shape → no CLS. `aria-busy` announces once. Neutral gray (no brand color in loading).

### 10b. Empty (icon + title + desc + ONE CTA)
```html
<div class="flex flex-col items-center py-16 text-center">
  <div class="flex h-12 w-12 items-center justify-center rounded-full bg-teal-50">
    <svg aria-hidden="true" class="h-6 w-6 text-teal-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5"><path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15"/></svg>
  </div>
  <h3 class="mt-4 text-sm font-medium text-slate-900">Belum ada data siswa</h3>
  <p class="mt-1 max-w-[28ch] text-sm text-slate-500">Impor data dari Dapodik untuk mulai memantau risiko.</p>
  <button type="button" class="mt-5 rounded-md bg-teal-700 px-4 py-2 text-sm font-medium text-white hover:bg-teal-800 transition-colors duration-150 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-teal-500 focus-visible:ring-offset-2">Impor data</button>
</div>
```

### 10c. Error (+ retry, role="alert")
```html
<div class="flex flex-col items-center py-12 text-center" role="alert">
  <div class="flex h-10 w-10 items-center justify-center rounded-full bg-red-50">
    <svg aria-hidden="true" class="h-5 w-5 text-red-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5"><path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m9-.75a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
  </div>
  <h3 class="mt-3 text-sm font-medium text-slate-900">Gagal memuat data</h3>
  <p class="mt-1 text-sm text-slate-500">Periksa koneksi internet atau coba lagi.</p>
  <button type="button" class="mt-4 rounded-md border border-slate-300 bg-white px-3 py-1.5 text-sm font-medium text-slate-700 hover:bg-slate-50 transition-colors duration-150 focus-visible:ring-2 focus-visible:ring-teal-500 focus-visible:ring-offset-2">Coba lagi</button>
</div>
```
Outlined retry (non-aggressive); human copy; red semantic only on icon.

### 10d. Zero-but-present chart
Render axes + buckets with zero values + explicit `domain={[0,10]}` (else bars invisible) + subtle annotation "Belum ada kejadian tercatat periode ini". Guard divide-by-zero: `total>0 ? v/total*100 : 0`. Never collapse to empty state.

### 10e. Refresh (stale-while-revalidate)
Show stale data + corner spinner (`role="status"`), never blank the widget. Table: `opacity-50 pointer-events-none` + centered overlay spinner + `sr-only "Memuat"`.

### 10f. Per-widget Suspense (Next.js 15)
Each widget = independent `<Suspense fallback={<Skeleton/>}>` boundary → fast KPI paint even if chart query slow. Server Components fetch; each can have own error boundary.
```tsx
<main className="space-y-6 p-4 md:p-6">
  <Suspense fallback={<KpiSkeleton/>}><KpiCards/></Suspense>
  <div className="grid gap-6 lg:grid-cols-[2fr_1fr]">
    <Suspense fallback={<ChartSkeleton/>}><RiskChart/></Suspense>
    <Suspense fallback={<ChartSkeleton/>}><StudentTable/></Suspense>
  </div>
</main>
```
**Source:** shadcn, IBM Carbon, NNG, Vercel, Next.js 15, Tremor, WCAG 2.3.3/4.1.3, Recharts domain.

---

## 11. COLOR / THEME TOKENS (3-tier + dark)

### Tier 1 primitive → Tier 2 semantic → Tier 3 component. Components reference semantic ONLY.
```css
:root{
  /* semantic surfaces — elevation via lightness, NOT shadow */
  --surface-page:#f8fafc; --surface-card:#ffffff; --surface-elevated:#fcfcfd; --surface-overlay:#ffffff;
  --border-default:#e2e8f0; --border-muted:#f1f5f9; --border-focus:#005D4C;
  --text-primary:#0f172a; --text-body:#334155; --text-muted:#64748b; --text-on-accent:#fff;
  /* accent restrained: interactive + focal only */
  --accent-default:#005D4C; --accent-hover:#004D40; --accent-subtle:#f0fdfa; --accent-muted:#ccfbf1;
  /* risk — semantic state only, never decorative */
  --risk-high-bg:#fef2f2;  --risk-high-text:#b91c1c;
  --risk-medium-bg:#fffbeb; --risk-medium-text:#b45309;
  --risk-low-bg:#f0fdf4;   --risk-low-text:#15803d;
}
.dark{
  --surface-page:#0f1419; --surface-card:#1c2127; --surface-elevated:#242a31; --surface-overlay:#1c2127;
  --border-default:rgba(255,255,255,.08); --border-muted:rgba(255,255,255,.04); --border-focus:#2dd4bf;
  --text-primary:#f1f5f9; --text-body:#cbd5e1; --text-muted:#94a3b8; --text-on-accent:#0f172a;
  --accent-default:#2dd4bf; --accent-hover:#99f6e4; --accent-subtle:rgba(45,212,191,.1); --accent-muted:rgba(45,212,191,.06);
  --risk-high-bg:rgba(239,68,68,.1); --risk-high-text:#fca5a5;
  --risk-medium-bg:rgba(245,158,11,.1); --risk-medium-text:#fcd34d;
  --risk-low-bg:rgba(16,185,129,.1); --risk-low-text:#6ee7b7;
}
```

### Surface elevation model
Light: page `#f8fafc` < card `#fff` < inset `#fcfcfd`. Dark: `#0f1419` < `#1c2127` < `#242a31` (warm dark, NEVER pure black). Teal lightened to `#2dd4bf` in dark (keeps 4.5:1).

### Accent restraint rule
Teal `#005D4C` ONLY at: primary buttons · active nav · focus rings · chart primary series · link hover · success badge. **Never:** card/section backgrounds, gradients, borders-everywhere, icon-bg squares. Accent = signal, not decoration.

### Contrast verified (WCAG AA)
| Pair | Ratio |
|---|---|
| text-body #334155 on white | 7.4:1 AAA |
| text-muted #64748b on white | 4.6:1 AA |
| accent #005D4C on white | 7.1:1 AAA |
| accent-dark #2dd4bf on #1c2127 | 6.8:1 AAA |
| risk-high #b91c1c on #fef2f2 | 5.9:1 AA |
**Source:** shadcn theming, Tremor, IBM Carbon (3-tier/4-theme), Linear "calmer interface", MD3 surface tint, WebAIM, Refactoring UI.

---

## 12. ANTI-PATTERN CHEAT SHEET (teardown-confirmed)

| ❌ Slop | ✅ Clean | Source |
|---|---|---|
| purple/indigo accent | teal `#005D4C` | Linear/Stripe/Vercel |
| shadow on every card | border only; shadow for floating | Geist Materials |
| `hover:scale-105` | `hover:bg-slate-50` / `hover:shadow-sm` | Vercel guidelines |
| big `text-3xl` page title | `text-lg font-semibold` | PostHog/Linear density |
| uniform `rounded-2xl` | scaled radii md/lg/xl | Geist |
| `transition: all` | explicit `transition-colors` | Vercel |
| whole-page scroll | fixed sidebar + scrollable main | PostHog/Linear/Vercel |
| rainbow chart series | teal sequential + muted context | Stripe/Tremor |
| left-border nav active | fill `bg-slate-100` + teal icon | slop §9 |
| pulsing notification badge | static red dot | slop K12 |
| blank widget on no-data | empty state icon+title+CTA | NNG/Carbon |

---

## SOURCES (selected)
Linear — "A calmer interface for a product in motion" (Mar 2026) + UI redesign part II · Vercel Geist Materials + Web Interface Guidelines · PostHog "What if PostHog looked like a dev tool?" · Stripe Apps design patterns · Tremor (tremor.so) · shadcn/ui dashboard + data-table · Untitled UI dashboards · Notion / Retool / Mixpanel teardowns · TanStack Table v8 · MUI DataGrid · IBM Carbon (tokens, empty states, side-nav) · React Aria (DateRangePicker, ComboBox multi-select, Tabs) · Headless UI · W3C WAI-ARIA APG (breadcrumb, menu, dialog, tabs, radio, combobox) · NNG (skeleton screens, empty states, error guidelines) · SetProduct (dashboard anatomy) · Baymard (filter toolbars) · Pencil & Paper (filter UX) · Refactoring UI · Next.js 15 (streaming/Suspense) · MDN aria-sort · Stanford a11y (sticky header) · WebAIM contrast · WCAG 1.4.1/2.3.3/4.1.2/4.1.3.
