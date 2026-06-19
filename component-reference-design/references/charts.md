# Charts — Beautiful, Modern, Clean Statistical Visualization

Companion to `../SKILL.md`. The reference for building **beautiful** charts (Recharts-first)
that read as Linear/Vercel/Stripe/Tremor quality — not "basic HTML chart" and not AI-slop.
Synthesized from web research (2025–2026): Recharts 3.x docs, Tremor, shadcn/ui charts, Nivo,
Apache ECharts, Observable, IBM Carbon Data-Viz, ColorBrewer, Okabe-Ito, Vercel/Linear/Stripe
dashboards, Cole Nussbaumer (SWD), Tufte, NNG, animations.dev / Emil Kowalski.

> Quality bar: one brand hue (teal, NOT purple) · subtle dashed horizontal grid · gradient
> area fills · hover-only dots · custom tooltip card · `tabular-nums` · `prefers-reduced-motion`
> gated · `role="img"`+`aria-label` · real data only. If a chart could be swapped into any
> generic dashboard and nobody notices, it's slop.

---

## 0. THE 15-POINT ANTI-SLOP CHART SCAN (run before shipping any chart)

- [ ] Primary color is brand teal `#005D4C` — **NOT** purple/indigo (`#6366f1`/`#8b5cf6`)
- [ ] ≤4 colors in any single chart (no rainbow-per-series)
- [ ] Gridlines: **horizontal only**, dashed `"3 3"`, `#e5e7eb`/`#eef2f6`
- [ ] One focal series visually dominant (others muted / lower opacity)
- [ ] All numbers use `tabular-nums`
- [ ] `isAnimationActive` gated by `prefers-reduced-motion` (or `"auto"` in Recharts 3.x)
- [ ] Container has `role="img"` + descriptive `aria-label`
- [ ] KPI numbers rounded (12.4K not 12,398); every KPI shows a delta + comparison
- [ ] Empty state present (never a blank chart area) — see §11
- [ ] `<ResponsiveContainer>` (never fixed px width)
- [ ] No 3D, no glow, no drop-shadow on data marks (shadows only on card wrappers)
- [ ] No pie/donut with >5 slices (use bar instead)
- [ ] Axis lines + tick lines removed (`axisLine={false} tickLine={false}`)
- [ ] Hidden numeric axis ALWAYS has explicit `domain` (see §3 PITFALL)
- [ ] Data is real (or clearly marked "data contoh")

---

## 1. COLOR SYSTEM (the #1 thing that makes charts beautiful or slop)

### 1a. Brand + semantic tokens (JagaSekolah)
```ts
const CHART = {
  brand:   "#005D4C",  // teal — primary/focal series
  brandDk: "#004D40",
  merah:   "#ef4444",  // risk high (semantic only)
  kuning:  "#f59e0b",  // risk medium
  hijau:   "#10b981",  // risk low / success
  grid:    "#eef2f6",  // slate-100/200 — dashed gridlines
  axis:    "#94a3b8",  // slate-400 — tick text
  label:   "#475569",  // slate-600 — category labels
} as const;
```

### 1b. Single-hue SEQUENTIAL ramp (heatmaps, intensity, multi-series same metric)
Fixed hue ~170°, vary lightness only. Use 4–5 steps, never rainbow.
```
#E0F5F0 → #B3E8DD → #7AD4C4 → #45BBA8 → #1A9E8B → #008272 → #005D4C → #004438 → #002E25
```
(IBM Carbon Teal monochromatic equivalent. For multi-series "same metric across Kelas A/B/C"
use `#005D4C`, `#0d9488`, `#5eead4` — one hue, descending lightness.)

### 1c. CATEGORICAL palette (max ~5–7, colorblind-safe, brand-anchored)
Modified Okabe-Ito, teal first, **no purple as primary**:
```ts
const CATEGORICAL = ["#005D4C","#0072B2","#D55E00","#009E73","#CC79A7","#E69F00","#6B7280"];
```

### 1d. Emphasis hierarchy for multi-series (Linear/Stripe pattern)
Primary series full strength; context series muted/dashed:
```tsx
<Line dataKey="tahunIni"  stroke="#005D4C" strokeWidth={2.5} />
<Line dataKey="tahunLalu" stroke="#9CA3AF" strokeWidth={1.5} strokeDasharray="4 4" strokeOpacity={0.7} />
```

### 1e. OKLCH for perceptual uniformity (advanced)
`oklch(L C H)` — L maps to *perceived* brightness, so equal-L colors look equally bright.
Teal brand ≈ `oklch(0.38 0.08 170)`. Generate sequential by varying only L. Always ship a hex
fallback. Sources: evilmartians.com/opensource/oklch-color-picker, ColorBrewer, Okabe-Ito.

### Color BANS (instant slop)
purple/indigo accent · `from-purple to-pink/cyan` gradients · rainbow-per-series ·
neon glow (`box-shadow:0 0 40px`) · amber `#f59e0b` as TEXT on white (use `#B45309`) ·
color as the ONLY signal (always pair with label/shape/position).

---

## 2. THE "PREMIUM MINIMAL" CHROME (shared presets — apply to every chart)

Extract once, reuse everywhere. This is what separates Tremor/Linear from a default chart.

```tsx
// theme.tsx
export const axisProps = { tick:{fill:"#94a3b8",fontSize:11}, axisLine:false as const, tickLine:false as const };
export const yAxisProps = { ...axisProps, width:30, allowDecimals:false as const };
export const gridProps  = { stroke:"#eef2f6", strokeDasharray:"4 4", vertical:false as const };
export const ANIM_MS = 600;
```

### 2a. Custom tooltip card (the single biggest "premium" upgrade)
```tsx
function ChartTooltip({ active, payload, label, unit="" }) {
  if (!active || !payload?.length) return null;
  return (
    <div className="rounded-xl border border-slate-200 bg-white/95 px-3 py-2 shadow-[0_4px_16px_rgb(15_23_42_/0.08)] backdrop-blur-sm">
      {label != null && <p className="mb-1 text-xs font-medium text-slate-500">{label}</p>}
      <ul className="space-y-0.5">
        {payload.map((p,i)=>(
          <li key={i} className="flex items-center gap-2 text-sm">
            <span className="h-2 w-2 shrink-0 rounded-full" style={{backgroundColor:p.color}} aria-hidden="true" />
            <span className="text-slate-500">{p.name}</span>
            <span className="ml-auto pl-3 font-semibold tabular-nums text-slate-900">
              {Number(p.value).toLocaleString("id-ID")}{unit}
            </span>
          </li>
        ))}
      </ul>
    </div>
  );
}
```
Usage: `<Tooltip content={<ChartTooltip unit=" siswa" />} cursor={{stroke:"#94a3b8",strokeWidth:1,strokeDasharray:"4 4"}} />`
Dark variant: bg `#1f2937`, border `#374151`, text `#f3f4f6`.

### 2b. Cursor styles
| Chart | cursor |
|---|---|
| Line/Area | `{ stroke:"#94a3b8", strokeWidth:1, strokeDasharray:"4 4" }` (dashed crosshair) |
| Bar | `{ fill:"#005D4C", fillOpacity:0.05 }` (subtle highlight band) |
| Scatter | `false` (dot-only) |

### 2c. Legend — circle icons, not squares/lines
`<Legend iconType="circle" iconSize={8} wrapperStyle={{fontSize:12,paddingTop:10}} />`
For interactive toggle, render a custom legend of `<button>`s with `aria-pressed` that set a
`hidden` Set and pass `hide={hidden.has(key)}` to each series.

### 2d. Minimal axes
No axis line, no tick line, sparse ticks (5–7), gray-400 text, `tabular-nums`, unit suffix,
abbreviated month labels (`"Jan"`), `minTickGap`/`interval="preserveStartEnd"` to thin ticks.
Bar charts start Y at 0. Never rotate labels on desktop (truncate/abbreviate instead).

### 2e. Soft dashed gridlines
Horizontal only (`vertical={false}`), `strokeDasharray="3 3"` or `"4 4"`, `#e5e7eb`/`#eef2f6`.
NO grid at all for: pie/donut, sparklines, single big-number KPIs, value-labelled bars.

---

## 3. BAR & COLUMN

Beauty = rounded caps + breathing room + value labels + ONE focal color.

```tsx
<BarChart data={data} layout="vertical" barCategoryGap="28%" margin={{top:4,right:36,bottom:4,left:8}}>
  <CartesianGrid {...gridProps} horizontal={false} />
  <XAxis type="number" hide domain={[0,"dataMax"]} />        {/* ← see PITFALL */}
  <YAxis type="category" dataKey="label" tick={{fill:CHART.label,fontSize:12}} axisLine={false} tickLine={false} width={140} />
  <Tooltip content={<ChartTooltip />} cursor={{fill:"rgba(0,93,76,0.05)"}} />
  <Bar dataKey="value" radius={[0,6,6,0]} isAnimationActive={!reduced} animationDuration={ANIM_MS} barSize={20}>
    {data.map((d,i)=><Cell key={i} fill={d.color ?? CHART.brand} />)}
    <LabelList dataKey="value" position="right" className="fill-slate-500"
      style={{fontSize:12, fontVariantNumeric:"tabular-nums"}} />
  </Bar>
</BarChart>
```

| Technique | Value |
|---|---|
| Rounded caps | vertical `radius={[4,4,0,0]}` · horizontal `[0,6,6,0]` · pill `radius={6}` |
| Gap | `barCategoryGap="20–28%"`, `barGap={2–4}` |
| Value labels | `<LabelList position="top"|"right">` + `tabular-nums`; hide tiny values via formatter |
| Stacked | shared `stackId`; only TOP segment rounded; `stackOffset="expand"` for 100% |
| Diverging | per-`Cell` color (emerald ≥0 / red <0) + `ReferenceLine y={0}` + `stackOffset="sign"` |
| Track/progress | `background={{ fill:"#f1f5f9", radius:4 }}` |
| Emphasis | dim others to `fillOpacity 0.3` on hover, OR `<Cell>` highlight one |
| Gradient (single-series hero) | vertical `linearGradient` brand 100%→60% |

### 3·PITFALL (this is the bug that made JagaSekolah bars look "empty")
> Setting `<XAxis type="number" hide />` (or `<YAxis hide />`) makes Recharts **drop the
> domain → bars render 0-width / invisible** (labels still show). **ALWAYS** add an explicit
> domain when hiding a numeric axis: `domain={[0, "dataMax"]}`. Affects ranking bars,
> sparklines, any "hidden axis" chart. (Verified fix; Tremor computes domain explicitly too.)

---

## 4. LINE & AREA

Beauty = smooth monotone curve + gradient fill + hidden dots (hover ring) + dashed crosshair.

```tsx
<AreaChart data={data} margin={{top:8,right:12,bottom:0,left:-10}}>
  <defs>
    <linearGradient id={`a-${gid}`} x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%"  stopColor="#005D4C" stopOpacity={0.28} />
      <stop offset="100%" stopColor="#005D4C" stopOpacity={0.02} />
    </linearGradient>
  </defs>
  <CartesianGrid {...gridProps} />
  <XAxis dataKey="label" {...axisProps} dy={4} minTickGap={16} />
  <YAxis {...yAxisProps} />
  <Tooltip content={<ChartTooltip />} cursor={{stroke:"#94a3b8",strokeWidth:1,strokeDasharray:"4 4"}} />
  <Area type="monotone" dataKey="value" stroke="#005D4C" strokeWidth={2.5}
    fill={`url(#a-${gid})`} dot={false}
    activeDot={{ r:5, strokeWidth:2, stroke:"#fff" }}
    isAnimationActive={!reduced} animationDuration={ANIM_MS} />
</AreaChart>
```
`gid` from `useId().replace(/:/g,"")` to make gradient ids unique per instance.

| Technique | Value / when |
|---|---|
| Curve | `type="monotone"` for bounded data (%, counts) — never overshoots. `natural` = aesthetic only |
| Gradient fill | top stop 0.20–0.35, bottom 0.01–0.03, vertical |
| Dots | `dot={false}` + `activeDot={{r:5,stroke:"#fff",strokeWidth:2}}` (white ring "lift") |
| Multi-line | smooth, hover dots, **no purple in palette**, circle legend |
| Stacked area | per-series gradient fills (`sa-${gid}-${key}`), `stackId="stack"` |
| Gaps | `connectNulls` true for attendance/trend; false when data genuinely missing |
| Thresholds | `<ReferenceLine y={85} stroke="#f59e0b" strokeDasharray="6 4" label="Batas 85%" />` |
| Danger zone | `<ReferenceArea y1={0} y2={40} fill="#ef4444" fillOpacity={0.06} stroke="none" />` |
| Dual-axis | **AVOID** — use two stacked charts sharing X, or indexed values, or scatter |

---

## 5. DONUT / PIE / RADIAL

Use ONLY for ≤5 part-of-whole segments. >5 → horizontal bar. Time-series → stacked area.

```tsx
<PieChart>
  <Pie data={data} dataKey="value" nameKey="name" innerRadius={66} outerRadius={92}
    paddingAngle={3} cornerRadius={6} stroke="none" startAngle={90} endAngle={-270}
    isAnimationActive={!reduced} animationDuration={ANIM_MS}>
    {data.map((d)=><Cell key={d.key} fill={COLOR[d.key]} />)}
  </Pie>
  <Tooltip content={<DonutTip />} />
</PieChart>
{/* center total via absolute-positioned overlay: big tabular-nums + caption */}
```

| Technique | Value |
|---|---|
| Rounded segments | `cornerRadius={6}` + `paddingAngle={3}` (min, to avoid overlap artifacts) |
| Thick ring | `innerRadius` 60–66% of outer; center reserved for total |
| Start at top | `startAngle={90} endAngle={-270}` (12 o'clock) |
| Active expand | `activeShape` → `outerRadius+8`; dim inactives to `opacity 0.3` |
| Tooltip | show absolute + `%` of total |
| Radial gauge (1 KPI %) | `RadialBarChart` `startAngle={90} endAngle={-270}` + `PolarAngleAxis domain={[0,100]} tick={false}` + `background={{fill:"#e5e7eb"}}` + `cornerRadius={6}` + center text |
| Semi-donut gauge | `startAngle={180} endAngle={0}`, color by threshold |
| Multi-ring | ≤4 rings, teal sequential, legend below with values |
| Legend | custom dots below (≤200px) or right (>200px); none for single-metric gauge |

---

## 6. KPI / STAT CARDS + SPARKLINES (the dashboard's first impression)

4 layers: **label** (`text-sm text-slate-500`) · **value** (`text-2xl font-semibold tabular-nums`) ·
**delta badge** (arrow + colored pill + period) · **sparkline** (mini area, no axes).

```tsx
<article className="rounded-lg border border-slate-200 bg-white p-5"
  role="img" aria-label="Siswa berisiko: 24, +12% vs bulan lalu">
  <div className="flex items-start justify-between">
    <div>
      <p className="text-sm font-medium text-slate-500">Siswa Berisiko</p>
      <p className="mt-2 text-2xl font-semibold tabular-nums text-slate-900">24</p>
      <p className="mt-2 flex items-center gap-1 text-xs">
        <span className="inline-flex items-center rounded bg-red-50 px-1.5 py-0.5 font-medium tabular-nums text-red-700">+12%</span>
        <span className="text-slate-400">vs bulan lalu</span>
      </p>
    </div>
    <div className="h-12 w-24 flex-none" aria-hidden="true">{/* Sparkline */}</div>
  </div>
</article>
```

### Sparkline (mini area — NO axes/grid/tooltip)
```tsx
<ResponsiveContainer width="100%" height={48}>
  <AreaChart data={data} margin={{top:4,right:0,bottom:4,left:0}}>
    <defs><linearGradient id="sp" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stopColor={color} stopOpacity={0.15} /><stop offset="100%" stopColor={color} stopOpacity={0.01} />
    </linearGradient></defs>
    <Area type="monotone" dataKey="v" stroke={color} strokeWidth={1.5} fill="url(#sp)" dot={false} isAnimationActive={false} />
  </AreaChart>
</ResponsiveContainer>
```
Rules: height 32–48px · `dot={false}` · no XAxis/YAxis/Grid/Tooltip · sparkline `aria-hidden`
(card carries the label) · sparkline color follows delta (teal neutral, red negative).
Delta beats absolute — always show both. Round aggressively (12.4K not 12,398).
KPI grid: 4 per row max (`sm:grid-cols-2 xl:grid-cols-4`); one hero may `sm:col-span-2`.
Progress vs target: `role="progressbar"` + colored fill (teal on-track, amber behind).

---

## 7. SPECIALTY (use when the data story needs it; always pair with a data table)

| Chart | When (JagaSekolah) | Library | Beauty notes | Data? |
|---|---|---|---|---|
| **Calendar heatmap** | 365-day attendance per siswa | `react-activity-calendar` | block radius 2, gap 3px, teal 5-step theme | ✅ Absensi |
| **Heatmap matrix** | day×week absence, subject×student | `@nivo/heatmap` or custom SVG | rect cells radius 2, single-hue sequential, empty=`#f8fafc` | ✅ |
| **Treemap** | risk distribution per kelas/sekolah | Recharts `Treemap` (`content` prop) | `rx={4}`, white 2px stroke, labels only if cell big | ✅ |
| **Funnel** | dropout/intervention pipeline | Recharts `FunnelChart` | semantic red→amber→teal→emerald, conversion-% labels | ✅ |
| **Radar** | per-student ABC+D profile vs class avg | Recharts `RadarChart` | polygon grid `"3 3"`, fill 0.15, ≤2 overlays, dashed reference | ✅ |
| **Scatter/bubble** | risk score vs attendance, outliers | Recharts `ScatterChart` | `ReferenceArea` danger quadrant, dashed threshold lines, ZAxis bubble | ✅ |
| **Bullet** | KPI vs target (kehadiran/intervensi) | `@nivo/bullet` | pastel ranges + teal measure + dark marker; beats gauges for multi-KPI | ✅ |
| **Waffle** | "24 of 200 at-risk" part-of-whole | `@nivo/waffle` | 10×10, cell radius 2, empty `#f1f5f9`, 3 risk colors | ✅ |
| **Geo / choropleth** | risk by kecamatan/coordinates | — | — | ⚠️ **NEEDS-DATA** (no lat/lng) |

---

## 8. MOTION (tasteful — guides the eye, never entertains)

- **Recharts 3.x default `isAnimationActive="auto"` respects `prefers-reduced-motion`** — leave it,
  or pass `!reduced` from a `usePrefersReducedMotion()` hook for explicit control. Never `={true}`.
- Durations: bar grow 400–600ms · line draw / pie sweep 600–800ms · hover 150ms · KPI count-up 1–1.5s.
- Easing: `ease-out` (entrances) / `cubic-bezier(0,0,0.2,1)` (premium deceleration). Exits faster.
- Stagger series via `animationBegin` (80–150ms offset, ≤5 items, total ≤1200ms).
- Hover: highlight active series, dim siblings to `opacity 0.3` via `transition: ... 150ms ease-out`.
- Scroll-reveal: `IntersectionObserver` `triggerOnce` → set `isAnimationActive` on enter.
- **NEVER:** >2s animations · spring/bounce on data · infinite pulse · re-animate on every update ·
  `hover:scale-105` (CLS) · animate gridlines/axes · `transition-all` on SVG · typewriter/parallax.

---

## 9. DARK MODE & RESPONSIVE

- **Tokens via CSS vars** (`--chart-1`… + `.dark` overrides). shadcn `ChartConfig` maps keys→`var(--color-key)`.
- Dark bg: **never pure black** — `#0f1419`/`zinc-900`. Brand teal **lightened** for dark
  (`#14b8a6`/`teal-500`). Risk colors → `-400` variants (avoid neon glare). Grid `rgba(255,255,255,0.06)`.
  Carbon principle: in dark, lightest color = highest value (invert the ramp).
- Responsive: `<ResponsiveContainer>` (or Recharts 3.3 `responsive` prop) inside a parent with
  height (`min-h-[200px]`/`aspect-[16/9]`). Aspect: line/area 16/9→4/3 mobile, donut 1/1, sparkline 3/1.
- Mobile: `interval={2}` to thin ticks, abbreviate labels, 10px font, swap bars to `layout="vertical"`,
  legend → bottom, wrap time-series in `overflow-x-auto` with `min-w-[600px]`.

---

## 10. ACCESSIBILITY (mandatory on every chart)

```tsx
<div role="img" aria-label="Tren risiko tinggi per bulan: 45 → 26 (Jul 2025–Jun 2026)">
  <ResponsiveContainer width="100%" height={264}>
    <LineChart data={data} accessibilityLayer> … </LineChart>
  </ResponsiveContainer>
  {/* optional sr-only data table for screen readers */}
  <table className="sr-only"><caption>…</caption>…</table>
</div>
```
- `role="img"` + `aria-label` with the **key insight** (not just "a chart").
- `accessibilityLayer` on the chart (Recharts 3.x keyboard nav).
- Color never the only signal (pair with label / dash pattern / position).
- `tabular-nums` on every numeric label/tooltip/axis.
- Recharts tooltips are visual-only → provide sr-only `<table>` or "Tampilkan tabel" toggle for dense charts.

---

## 11. EMPTY / LOADING / ERROR STATES (never a blank chart box)

- **Empty:** centered icon + "Belum ada data" + one CTA ("Impor data") — see components.md EmptyState.
- **Zero-but-present:** charts must still render axes/buckets with zero values (don't collapse). Guard
  every `pct` calc against divide-by-zero (`total>0 ? … : 0`, never `NaN`).
- **Loading:** skeleton matching the chart's height (`animate-pulse motion-reduce:animate-none`), not a spinner.
- **Sparse data:** ≤5 points → use `type="linear"` + visible dots (monotone curves look odd on few points).

---

## 12. DECISION MATRIX — which chart for which question

| Question | Chart |
|---|---|
| How did X change over time? | Line / Area (gradient) |
| Compare a metric across categories | Bar (vertical) / ranking (horizontal) |
| Part-of-whole, ≤5 parts | Donut (rounded, center total) |
| Part-of-whole, >5 parts | Horizontal bar (sorted) |
| Composition over time | Stacked area / stacked bar |
| One KPI vs target | Radial gauge / bullet / progress bar |
| Multi-factor profile of one entity | Radar |
| Relationship between 2 variables + outliers | Scatter / bubble |
| Pipeline drop-off | Funnel |
| Daily pattern over a year | Calendar heatmap |
| KPI glance (number + trend) | Stat card + sparkline |

---

## SOURCES (selected)
Recharts 3.x docs (recharts.github.io) · Tremor (tremor.so) · shadcn/ui charts (ui.shadcn.com/docs/components/radix/chart) ·
Nivo (nivo.rocks) · Apache ECharts · Observable Plot · IBM Carbon Data-Viz (carbondesignsystem.com/data-visualization) ·
ColorBrewer (colorbrewer2.org) · Okabe-Ito (siegal.bio.nyu.edu/color-palette) · Vercel Analytics curve-fitting blog ·
Linear design refresh · Stripe dashboard components · Cole Nussbaumer SWD (gridlines) · Tufte / chartjunk ·
NNG clutter-free charts · Emil Kowalski (emilkowal.ski/ui/great-animations) · animations.dev · Josh Comeau (reduced-motion) ·
evilmartians OKLCH picker · WCAG 1.1.1/1.4.1/1.4.11/2.3.3 · W3C SVG ARIA roles.
