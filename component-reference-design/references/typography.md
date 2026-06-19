# Typography — Modern Systems & Font Pairings

Companion to `../SKILL.md`. Sources: Refactoring UI, Material Design 3, Apple HIG, IBM Carbon, MUI, Utopia, Typewolf, Fontshare, Google Fonts, Smashing.

> Rule of thumb: distinctive display font (NOT Inter-everywhere) + neutral body; max 2 families; max 3 weights (400/500/700); modular scale; `max-w-prose` (~65ch); left-aligned body.

---

## (A) PROVEN FONT PAIRINGS (display + body)

| # | Display | Body | Vibe | Source / availability |
|---|---------|------|------|------------------------|
| 1 | **Bricolage Grotesque** | Source Sans 3 / DM Sans | Expressive editorial, organic | Google Fonts |
| 2 | **Fraunces** (variable, "wonky" serif) | Inter | Warm/literary meets tech; D2C, wellness | Google Fonts |
| 3 | **Instrument Serif** | DM Sans | Refined luxury, elevated minimal | Google Fonts |
| 4 | **Space Grotesk** | Newsreader / Work Sans | Technical authority + literary | Google Fonts |
| 5 | **Libre Franklin** | Libre Baskerville | Institutional trust; gov/edu/journalism | Google Fonts |
| 6 | **Outfit** | Lora / Nunito Sans | Geometric warmth; e-commerce, edu | Google Fonts |
| 7 | **Cabinet Grotesk** | Satoshi / Switzer | Geometric display + humanist body; SaaS | Fontshare (free, commercial OK) |
| 8 | **Satoshi** | General Sans | Premium geometric neutrality | Fontshare |
| 9 | **Newsreader** | Source Sans 3 | Classic editorial; publishing | Google Fonts |
| 10 | **Plus Jakarta Sans** | Libre Franklin | Friendly geometric; fintech/health | Google Fonts |

```css
/* Example token layer */
:root{
  --font-display:'Fraunces', serif;
  --font-body:'Inter', system-ui, sans-serif;
  --font-mono:'JetBrains Mono', ui-monospace, monospace;
}
```
> For JagaSekolah (gov/edu, trustworthy): pairing #5 Libre Franklin + Libre Baskerville, or #2 Fraunces + Inter.

---

## (B) MODULAR SCALE RECIPES

### Major Third (1.25) — recommended general UI (Refactoring UI / Tailwind-aligned)
```
10 → 13 → 16(base) → 20 → 25 → 31 → 39 → 49 → 61
```

### Minor Third (1.2) — compact / data-dense (dashboards)
```
12 → 14(base) → 17 → 20 → 24 → 29 → 35 → 42
```

### Perfect Fourth (1.333) — editorial / marketing
```
12 → 16(base) → 21 → 28 → 38 → 50 → 67
```

Rules: 4–6 distinct sizes; each level visibly distinct without relying on weight alone.

---

## (C) PER-ROLE RULES (leading / measure / tracking / weight)

| Role | Size | Line-height | Tracking | Weight |
|------|------|-------------|----------|--------|
| Display (>48px) | hero | 1.0–1.1 | −0.5 to −1.5px (tight) | 300–700 (pick one philosophy) |
| Heading (24–48) | h1/h2 | 1.15–1.3 | −0.3 to −0.8px | 600–700 |
| Title (16–24) | h3/h4 | 1.3–1.5 | 0 to +0.15px | 500–600 |
| Body (14–18) | p | 1.5–1.75 | 0 to +0.5px | 400 |
| Label/Button | sm | 1.3–1.45 | +0.1 to +0.5px | 500 |
| Caption (11–14) | meta | 1.3–1.45 | +0.1 to +0.5px | 400 |
| ALL-CAPS label | <14 | 1.4 | +0.5 to +1.5px | 500 |

**Universal laws:** line-height *decreases* as size *increases*; tracking *tightens* as size *increases*; body measure ~65ch (never full-width prose); body always left-aligned.

---

## (D) FLUID TYPE — clamp() RECIPES (Utopia method, 360→1240px)
```css
:root{
  --step--1: clamp(0.833rem, 0.775rem + 0.29vw, 1rem);      /* 13→16 */
  --step-0:  clamp(1rem, 0.909rem + 0.45vw, 1.25rem);       /* 16→20 base */
  --step-1:  clamp(1.2rem, 1.055rem + 0.73vw, 1.563rem);    /* 19→25 */
  --step-2:  clamp(1.44rem, 1.211rem + 1.15vw, 1.953rem);   /* 23→31 */
  --step-3:  clamp(1.728rem, 1.376rem + 1.76vw, 2.441rem);  /* 28→39 */
  --step-4:  clamp(2.074rem, 1.547rem + 2.63vw, 3.052rem);  /* 33→49 */
}
/* simple cases */
h1{font-size:clamp(2rem,1.64rem+1.82vw,3rem)}    /* 32→48 */
h2{font-size:clamp(1.5rem,1.23rem+1.36vw,2.25rem)} /* 24→36 */
body{font-size:clamp(1rem,0.95rem+0.23vw,1.125rem)} /* 16→18 */
```
**A11y warning (Adrian Roselli):** raw `vw` breaks zoom — always clamp with a rem component; test at 200% zoom.

---

## (E) TAILWIND CONFIG

### v4 (CSS-first `@theme`)
```css
@theme{
  --font-display:'Fraunces', serif;
  --font-body:'Inter', sans-serif;
  --text-base:1rem; --text-lg:1.25rem; --text-xl:1.563rem;
  --text-2xl:1.953rem; --text-3xl:2.441rem; --text-4xl:3.052rem;
}
```

### v3 (`fontSize` with line-height + tracking)
```js
fontSize:{
  xs:  ['0.75rem',  {lineHeight:'1rem',   letterSpacing:'0.025em'}],
  sm:  ['0.875rem', {lineHeight:'1.25rem',letterSpacing:'0.01em'}],
  base:['1rem',     {lineHeight:'1.5rem', letterSpacing:'0'}],
  lg:  ['1.25rem',  {lineHeight:'1.75rem',letterSpacing:'0'}],
  xl:  ['1.563rem', {lineHeight:'2rem',   letterSpacing:'-0.01em'}],
  '2xl':['1.953rem',{lineHeight:'2.25rem',letterSpacing:'-0.02em'}],
  '3xl':['2.441rem',{lineHeight:'1.2',    letterSpacing:'-0.025em'}],
  '4xl':['3.052rem',{lineHeight:'1.1',    letterSpacing:'-0.03em'}],
}
```

---

## (F) DESIGN-SYSTEM SCALES COMPARED (reference values)

### Material Design 3 (15 role tokens)
```
displayLarge 57/64 w400 −0.25 · displayMedium 45/52 · displaySmall 36/44
headlineLarge 32/40 · headlineMedium 28/36 · headlineSmall 24/32
titleLarge 22/28 · titleMedium 16/24 w500 +0.15 · titleSmall 14/20 w500 +0.1
bodyLarge 16/24 +0.5 · bodyMedium 14/20 +0.25 · bodySmall 12/16 +0.4
labelLarge 14/20 w500 · labelMedium 12/16 w500 +0.5 · labelSmall 11/16 w500 +0.5
```

### Apple HIG (iOS Dynamic Type, default)
```
LargeTitle 34 Bold −1.05 · Title1 28 Bold −0.8 · Title2 22 Bold −0.7 · Title3 20 Semibold
Headline 17 Semibold −0.43 · Body 17 Regular −0.43 · Callout 16 · Subhead 15
Footnote 13 +0.03 · Caption1 12 +0.12 · Caption2 11 +0.15
```
Apple uses optical sizing: SF Pro Text <20pt vs SF Pro Display ≥20pt.

### IBM Carbon — Productive (base 14)
```
label-01 12/16 · body-01 14/20 w400 · heading-01 14/20 w600
heading-03 20/28 · heading-04 28/36 · heading-05 32/40 · heading-06 42/50 w300 · heading-07 54/64 w300
```
Carbon uses an additive formula (not a ratio) + Light (300) for display.

### MUI default
```
h1 96 w300 · h2 60 w300 · h3 48 · h4 34 · h5 24 · h6 20 w500
body1 16/1.5 · body2 14/1.43 · button 14 w500 UPPERCASE · caption 12 · overline 12 UPPERCASE
```

---

## KEY PRINCIPLES
1. Two font families max (display + body); mono free for code.
2. Max 3 weights; never Light body on screen.
3. Line-height inverse to size; tracking tighter at large sizes.
4. Body measure ~65ch; left-aligned; `max-w-prose`.
5. Fluid type needs min AND max (clamp), test zoom.
6. MD3's 5-role × 3-size = 15 tokens is the most economical semantic naming to copy.

Sources: Refactoring UI, m3.material.io, Apple HIG, carbondesignsystem.com, mui.com, utopia.fyi, Typewolf, Fontshare, Smashing ("Modern Fluid Typography"), Adrian Roselli (zoom a11y).
