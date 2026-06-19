# Components — Cards, Buttons, Forms, Navigation, Data Display

Companion to `../SKILL.md`. Every entry: NAME · when-to-use · concrete accessible Tailwind. Sources: MUI, shadcn/ui, Radix, Headless UI, Ant Design, Material Design 3, IBM Carbon, Tailwind UI, Inclusive Components, TanStack Table, Tremor, W3C WAI-ARIA APG.

> Quality bar: real `<button>`/`<a>` (never `<div onClick>`), `focus-visible` ring on every interactive, labels wired, radius is a scale, hover = shadow/color not `scale-105`.

---

# 1. CARDS (17 variants)

| Variant | When | Key classes |
|---------|------|-------------|
| Elevated | default content grouping, primary emphasis | `rounded-lg bg-white p-6 shadow-md` |
| Outlined | secondary; dense UIs (avoids shadow noise) | `rounded-lg border border-gray-200 p-6` |
| Filled | lowest emphasis; nested | `rounded-lg bg-gray-50 p-6` |
| Media-top | blog/product preview | `overflow-hidden rounded-lg border` + `<img class="aspect-video w-full object-cover">` + `p-5` |
| Horizontal | search results, list items | `flex gap-4 rounded-lg border p-4` (image `w-32 shrink-0`) |
| Product | e-commerce grid | media + price + rating + add-to-cart |
| Pricing | SaaS tiers | header + price + ≤5 features + CTA; recommended = inverted fill not ring |
| Stat/KPI | dashboards | label(muted) + value(`text-2xl tabular-nums`) + trend(arrow + %) |
| Profile | team, author byline | avatar + name + role + bio |
| Testimonial | social proof | `<figure><blockquote>…</blockquote><figcaption>` real name + company |
| Blog/article | content index | media + tag + linked heading + excerpt + author/date |
| Interactive (link) | nav cards | pseudo-content trick (below) |
| Actions-footer | settings, CRUD | content + `border-t` footer with buttons |
| Overlay-text-on-image | featured/hero | image + `bg-gradient-to-t from-gray-900/80` + bottom text (≥70% gradient for AA) |
| Glass (tasteful) | ONE per viewport over imagery | `bg-white/80 backdrop-blur-sm border-white/20` + `@supports` fallback |
| Skeleton | loading | mirror real shape, `animate-pulse motion-reduce:animate-none`, `aria-busy` |
| Bento cell | feature showcase | grid with `col-span-2 row-span-2` dominant cell |

### Elevated (baseline)
```html
<article class="rounded-lg bg-white p-6 shadow-md dark:bg-zinc-900">
  <h3 class="text-base font-semibold text-gray-900 dark:text-gray-100">Title</h3>
  <p class="mt-2 text-sm text-gray-600 dark:text-gray-400">Real description content.</p>
</article>
```

### Interactive link card (Inclusive Components pseudo-content)
```html
<article class="relative rounded-lg border border-gray-200 p-5 transition-all duration-150
                hover:border-blue-300 hover:shadow-md focus-within:ring-2 focus-within:ring-blue-500">
  <h3 class="text-sm font-semibold text-gray-900">
    <a href="/dest" class="after:absolute after:inset-0 focus:outline-none">Card title</a>
  </h3>
  <p class="mt-1 text-sm text-gray-600">Description.</p>
</article>
```
Entire card clickable, heading stays the semantic link, `:focus-within` mirrors hover for keyboard.

### Stat/KPI
```html
<div class="rounded-lg border border-gray-200 p-5">
  <p class="text-sm font-medium text-gray-500">Siswa Berisiko Tinggi</p>
  <p class="mt-1 text-2xl font-semibold tabular-nums text-gray-900">24</p>
  <p class="mt-2 flex items-center gap-1 text-xs">
    <svg aria-hidden="true" class="h-3.5 w-3.5 text-red-500"><!-- arrow-up --></svg>
    <span class="font-medium text-red-600">+12%</span>
    <span class="text-gray-400">vs bulan lalu</span>
  </p>
</div>
```

### Radius & shadow scale (never uniform)
| | inputs/badges | cards | modals | pills/avatars |
|--|--|--|--|--|
| radius | `rounded-md` | `rounded-lg` | `rounded-xl` | `rounded-full` |
| shadow | none | `shadow-sm` rest / `shadow-md` hover | `shadow-lg` | — |

Hover (never `scale-105`): `hover:shadow-md hover:-translate-y-0.5 transition-all duration-150 ease-out`.

---

# 2. BUTTONS (10 variants)

| Variant | When | Classes (base `px-4 py-2 rounded-md text-sm font-medium transition-colors focus-visible:ring-2 focus-visible:ring-offset-2`) |
|---------|------|--------|
| Filled/Primary | one principal CTA per area | `bg-blue-700 text-white hover:bg-blue-800 focus-visible:ring-blue-500` |
| Tonal/Secondary | secondary action | `bg-blue-100 text-blue-900 hover:bg-blue-200` |
| Outlined | cancel/alt | `border border-gray-300 bg-white text-gray-700 hover:bg-gray-50` |
| Text/Link | tertiary, inline | `text-blue-700 hover:underline underline-offset-4` |
| Ghost | toolbars, dense | `text-gray-700 hover:bg-gray-100` |
| Destructive | delete/revoke | `bg-red-600 text-white hover:bg-red-700 focus-visible:ring-red-500` |
| Icon-only | toolbar, close | `p-2` + **`aria-label`** + `<svg aria-hidden="true">` |
| FAB / Extended FAB | single screen action (mobile) | `fixed bottom-6 right-6 rounded-2xl shadow-lg` |
| Split | one default + dropdown | button group + `aria-haspopup` chevron |
| Loading | async in progress | `disabled` + spinner + `<span aria-live="polite">Saving…</span>` |

```html
<button type="button" class="inline-flex items-center gap-2 rounded-md bg-blue-700 px-4 py-2
  text-sm font-medium text-white hover:bg-blue-800 active:bg-blue-900
  focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2
  disabled:opacity-50 disabled:pointer-events-none transition-colors duration-150">
  Create your first report
</button>
```
**Sizes:** xs `px-2.5 py-1 text-xs` · sm `px-3 py-1.5 text-sm` · md `px-4 py-2 text-sm` · lg `px-5 py-2.5 text-base`.
**States:** hover (darken 5–10%) · active (15–20%) · focus-visible (ring, never bare `outline-none`) · disabled (`opacity-50 pointer-events-none`) · loading (disabled + spinner + aria-live).

---

# 3. FORM CONTROLS (13)

> Top-aligned visible label (NOT floating) — fastest completion, best a11y (NNG, Baymard). Color is never the sole error signal: pair `border-red-500` + icon + message + `aria-invalid` + `aria-describedby`.

### Text input — default / error / success
```html
<div class="space-y-1.5">
  <label for="email" class="block text-sm font-medium text-gray-900">Email</label>
  <input type="email" id="email" autocomplete="email"
    class="block w-full rounded-md border border-gray-300 px-3 py-2 text-sm
           focus:border-blue-500 focus:ring-1 focus:ring-blue-500 focus:outline-none
           disabled:bg-gray-50 transition-colors duration-150" />
  <p class="text-xs text-gray-500">Helper text.</p>
</div>
<!-- error: border-red-500 + aria-invalid="true" aria-describedby="email-error" -->
<p id="email-error" role="alert" class="text-xs text-red-600">Please enter a valid email.</p>
```

| Control | When | Source |
|---------|------|--------|
| Text input | single-line text | Carbon, Tailwind UI |
| Textarea | multi-line | shadcn, Carbon |
| Select (native) | 5–15 fixed options | Tailwind UI |
| Combobox/autocomplete | >15 options, searchable | Headless UI, React Aria, W3C APG |
| Checkbox | multiple independent | Radix, shadcn |
| Radio group | mutually exclusive 2–7 | Radix, Headless UI (`<fieldset><legend>`) |
| Switch | instant-apply binary | Radix (`role="switch" aria-checked`) |
| Slider/range | numeric range | Radix, React Aria (`aria-valuenow/min/max`) |
| Date picker | dates | React Aria, shadcn calendar |
| File upload/drop | files | Carbon, React Aria DropZone |
| Input group | prefix/suffix (Rp, %, search) | Tailwind UI |
| Number input | quantities | Carbon, Ant |
| Validation states | every field | Carbon, WCAG |

Radio group skeleton:
```html
<fieldset>
  <legend class="text-sm font-medium text-gray-900">Notification</legend>
  <div class="mt-2 space-y-2">
    <div class="flex items-center gap-3">
      <input type="radio" id="n-email" name="notif" value="email"
        class="h-4 w-4 border-gray-300 text-blue-600 focus:ring-2 focus:ring-blue-500" checked />
      <label for="n-email" class="text-sm text-gray-900">Email</label>
    </div>
  </div>
</fieldset>
```

---

# 4. NAVIGATION (13) — pick by IA size × device

| Pattern | When | Key |
|---------|------|-----|
| Top navbar | ≤7 items, marketing/app | `fixed top-0 inset-x-0 border-b bg-white` + `<nav aria-label>` |
| Sticky/condensing | long pages | shrink height on scroll (`h-16`→`h-12`) |
| Sidebar (collapsible) | dashboards 5–20 items | `w-64`↔`w-16`; `aria-current="page"` on active |
| Navigation rail | tablet, 3–7 items | `w-20` icon + label stack (MD3) |
| Bottom nav | mobile, 3–5 items | `fixed bottom-0` (`md:hidden`) |
| Tabs (underline/pill) | same-page sections | `role="tablist"/"tab"/"tabpanel"`, arrow-key nav (W3C APG) |
| Breadcrumbs | hierarchy 3+ levels | `<nav aria-label="Breadcrumb"><ol>`, last = `aria-current="page"` |
| Mega menu | 50+ pages | click-activated (not hover), ≤full-width panel, columns |
| Command palette ⌘K | power users | dialog + combobox + listbox (cmdk) |
| Pagination | long lists | `<nav aria-label="Pagination">`, `aria-current="page"` |
| Stepper/wizard | 3–7 sequential steps | states: complete/active/upcoming/error |
| Drawer | mobile menu / side panel | focus trap + Esc + `aria-modal` |
| Radix NavigationMenu | header dropdown nav | `data-motion` directional anim |

Top navbar (decision matrix: compact→bottom nav/hamburger, medium→rail, expanded→top nav or sidebar):
```html
<header class="fixed top-0 inset-x-0 z-50 border-b border-gray-200 bg-white">
  <nav aria-label="Global" class="mx-auto flex h-16 max-w-7xl items-center justify-between px-4 sm:px-6">
    <a href="/"><img class="h-8 w-auto" src="/logo.svg" alt="JagaSekolah home" /></a>
    <ul role="list" class="hidden md:flex md:gap-x-8">
      <li><a href="/dashboard" aria-current="page" class="text-sm font-medium text-gray-900">Dashboard</a></li>
      <li><a href="/siswa" class="text-sm font-medium text-gray-600 hover:text-gray-900">Siswa</a></li>
    </ul>
    <button type="button" class="md:hidden p-2" aria-label="Open menu" aria-expanded="false" aria-controls="m-menu">
      <svg aria-hidden="true" class="h-6 w-6"><!-- hamburger --></svg>
    </button>
  </nav>
</header>
```
Tabs (W3C APG keyboard: arrows move, Home/End, Enter/Space activate):
```html
<div role="tablist" aria-label="Account" class="flex border-b border-gray-200">
  <button role="tab" aria-selected="true" aria-controls="p1" id="t1"
    class="relative px-4 py-3 text-sm font-medium text-blue-700 after:absolute after:inset-x-0 after:bottom-0 after:h-0.5 after:bg-blue-700">General</button>
  <button role="tab" aria-selected="false" aria-controls="p2" id="t2" tabindex="-1"
    class="px-4 py-3 text-sm font-medium text-gray-500 hover:text-gray-700">Security</button>
</div>
<div role="tabpanel" id="p1" aria-labelledby="t1" tabindex="0" class="py-4">…</div>
<div role="tabpanel" id="p2" aria-labelledby="t2" tabindex="0" class="hidden py-4">…</div>
```

---

# 5. DATA DISPLAY (16)

| Component | When | Key |
|-----------|------|-----|
| Data table | tabular comparison/scan | `<table>` + `<th scope="col">` + `aria-sort` + sticky `<thead>` + zebra |
| List (1/2/3-line) | feeds, contacts, settings | `<ul role="list">` + leading/content/trailing |
| Description list | read-only key-value | `<dl><dt>/<dd>` |
| Stat/KPI tile | dashboards | see card §1 |
| Badge/Tag | status, category (read-only) | `inline-flex rounded-md px-2 py-0.5 text-xs ring-1 ring-inset` |
| Chip (removable) | filters | tag + close `<button aria-label>` |
| Notification badge | counts | absolute overlay `rounded-full bg-red-600` |
| Avatar + group | users | `rounded-full`; group `flex -space-x-2 ring-2 ring-white` |
| Tooltip | label icon-only/abbr | `role="tooltip"` + `aria-describedby`, hover+focus, 300–700ms |
| Popover | rich anchored content | `aria-expanded`, focus mgmt, Esc |
| Empty state | no data/first-use | icon + title + description + action (never blank) |
| Skeleton | loading | match shape, `aria-busy`, `motion-reduce` |
| Progress (linear/circular) | uploads, completion | `role="progressbar" aria-valuenow/min/max` |
| Timeline | chronological history | `<ol>` + node + content + `<time>` |
| Tree view | hierarchical data | `role="tree"/"treeitem"/"group"`, `aria-expanded` |
| Accordion | show/hide sections (FAQ) | `<button aria-expanded aria-controls>` + `role="region"` (W3C APG) |

### Accessible data table
```html
<div class="overflow-x-auto rounded-lg border border-gray-200" role="region" aria-label="Student data" tabindex="0">
  <table class="min-w-full divide-y divide-gray-200 text-sm">
    <thead class="sticky top-0 bg-gray-50">
      <tr>
        <th scope="col" aria-sort="ascending" class="px-4 py-3 text-left font-medium text-gray-700">Nama</th>
        <th scope="col" class="px-4 py-3 text-left font-medium text-gray-700">Status</th>
        <th scope="col" class="px-4 py-3 text-right font-medium text-gray-700">Skor</th>
      </tr>
    </thead>
    <tbody class="divide-y divide-gray-100">
      <tr class="odd:bg-white even:bg-gray-50/50 hover:bg-blue-50/50">
        <td class="px-4 py-3 font-medium text-gray-900">Ahmad Fauzi</td>
        <td class="px-4 py-3"><span class="inline-flex rounded-md bg-amber-50 px-2 py-0.5 text-xs font-medium text-amber-700 ring-1 ring-inset ring-amber-600/20">Waspada</span></td>
        <td class="px-4 py-3 text-right tabular-nums">72</td>
      </tr>
    </tbody>
  </table>
</div>
```
For complex tables (sort/filter/paginate/virtualize) use **TanStack Table** (headless) or **MUI DataGrid**. For charts use **Recharts** / **Tremor** with `role="img"` + `aria-label` + a hidden data-table alternative.

### Empty state
```html
<div class="flex flex-col items-center py-16 text-center">
  <div class="flex h-12 w-12 items-center justify-center rounded-full bg-gray-100">
    <svg aria-hidden="true" class="h-6 w-6 text-gray-400"><!-- icon --></svg>
  </div>
  <h3 class="mt-4 text-sm font-medium text-gray-900">Belum ada data siswa</h3>
  <p class="mt-1 text-sm text-gray-500">Impor data dari Dapodik untuk mulai memantau.</p>
  <button type="button" class="mt-4 rounded-md bg-blue-600 px-3 py-2 text-sm font-medium text-white hover:bg-blue-700 focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2">Impor data</button>
</div>
```

---

## CROSS-CUTTING PRINCIPLES
1. Semantic HTML first; ARIA only when HTML can't express it.
2. Every interactive: `<button>`/`<a>`, keyboard reachable, visible focus ring.
3. One primary button per visible area.
4. Labels always visible (top-aligned); error = border + icon + text + aria.
5. `tabular-nums` on all numeric data.
6. Loading + empty + error states for every async surface.
7. Radius/shadow are scales, not single values.
8. Dark surfaces `zinc-900` (not pure black); body `gray-200`, secondary `gray-400`.

Sources: MUI, shadcn/ui, Radix, Headless UI, Ant Design, Material Design 3, IBM Carbon, Tailwind UI, Inclusive Components (Heydon Pickering), TanStack Table, Tremor, Recharts, NNG, W3C WAI-ARIA APG.
