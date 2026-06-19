---
name: Anti-AI-Slop UI/UX
description: >-
  Overkill negative-prompt skill that stops AI-generated UI from looking like
  "AI slop" in a single glance. Detects and bans the statistical-mean design
  defaults LLMs emit (purple gradients, Inter, 3-card grids, glassmorphism,
  pill CTAs, hype copy) across EVERY component — hero, color, typography, cards,
  layout, icons, imagery, microcopy, motion, buttons, forms, nav, footer.
  Use when designing, building, reviewing, or refactoring any UI.
metadata:
  author: b4r7x
  version: 2.0.0
  references:
    - references/slop-uiux-patterns.md
    - references/slop-code-patterns.md
---

# Anti-AI-Slop UI/UX — Negative Prompt Skill

## Purpose
Produce UI that a discerning designer **cannot** instantly flag as
machine-generated. This skill is a hard filter against "AI slop": the
homogenized, brand-agnostic, statistically-average aesthetic that LLMs default
to because they predict the most probable token, not the most *intentional* one.

> If you could swap the logo with a competitor and nobody would notice — it's slop.

## Root Cause (why this happens)
LLMs generate UI by averaging across training data dominated by Tailwind docs,
shadcn/ui examples, abandoned repos with unchanged defaults, and 2020–2024
Dribbble SaaS shots. The output **collapses toward the visual mean**. This is
structural, not a bug (arXiv:2603.13036, "Interrogating Design Homogenization in
Web Vibe Coding"). Every decision left unspecified gets filled with the single
most frequent pattern. **The cure is deliberate, content-specific variation.**

---

## ⛔ THE INSTANT-DEATH SIGNATURE
Any **3+ of these together = reads as "AI made this" in under 2 seconds.** Treat
each as a hard block, not a suggestion.

```
purple/indigo gradient  +  Inter everywhere  +  centered hero + dual CTA
+ 3-card feature grid   +  rounded-2xl on everything  +  gradient pill button
+ "Trusted by 5,000+ teams"  +  "Supercharge your workflow" copy
+ gradient/aurora blobs  +  glassmorphism  +  emoji icons  +  left-border cards
```

If your output contains this combo, you have generated the canonical AI slop
landing page. **Stop and regenerate.**

---

## How To Use This Skill

### When designing/building UI
1. Read the **Per-Component Negative Prompts** below for every component you create.
2. Make a deliberate, stated choice for each of the **5 Slop Axes** (color, type,
   layout, copy, motion) — never accept the default.
3. Run the **Pre-Delivery Slop Audit** before handing off.

### When reviewing/refactoring UI
1. Scan against the **Slop Severity Table** — fix all P0/HIGH first.
2. Use `references/slop-uiux-patterns.md` for the exhaustive tell→fix catalog.
3. Report findings as: `tell → why-slop → fix → severity`.

---

## The 5 Slop Axes (decide each one explicitly)

| Axis | Slop default (BANNED) | Required action |
|------|----------------------|-----------------|
| **Color** | `indigo-500`/`violet`/`purple` accent, purple→pink/blue→cyan gradient | Pick ONE brand color (NOT purple/indigo). Define 60-30-10 palette. |
| **Type** | Inter/Geist for everything, gradient headline, `font-extrabold tracking-tighter` | Distinctive display font ≠ Inter. Solid headline. 2–3 weights, modular scale. |
| **Layout** | Centered everything, `max-w-7xl`, uniform `py-20`, symmetric 3-col grid | Vary container widths, section padding, grid structure. ≥2 asymmetric moments. |
| **Copy** | "Supercharge/Unlock/Seamless", "Get Started", "Trusted by X+ teams" | Concrete verbs + real numbers + specific CTAs. No buzzwords, no fake proof. |
| **Motion** | `hover:scale-105`, universal fade-in-up, 500ms, no reduced-motion | ≤200ms, transform/opacity only, `prefers-reduced-motion` mandatory. |

---

## PER-COMPONENT NEGATIVE PROMPTS

### 🦸 HERO — never generate
- ❌ Centered headline + subhead + symmetric dual buttons (filled primary + ghost secondary)
- ❌ Gradient `bg-clip-text` headline (purple→pink/blue)
- ❌ Eyebrow pill badge above H1 (`✨ Now with AI`, `🚀 New`)
- ❌ Aurora/gradient blobs (`blur-3xl` purple/blue circles) behind content
- ❌ Perspective-tilted dashboard mockup with glow (`rotateY(-5deg) shadow-2xl`)
- ❌ Greyed fake "Trusted by 5,000+ teams" logo strip
- ❌ `h-screen` hero with only text + button (PowerPoint-slide emptiness)
- ✅ **Do instead:** left-aligned headline, asymmetric split (text left / real product screenshot right), one dominant CTA + a text-link secondary, solid background, specific value-prop copy.

### 🎨 COLOR & GRADIENT — never generate
- ❌ `indigo-500`/`violet-500`/`purple-500`/`#6366f1`/`#8b5cf6`/`#a855f7` as primary accent
- ❌ `from-purple-500 to-pink-500`, `from-indigo-500 to-purple-600`, `from-blue-500 to-cyan-500`
- ❌ Gradient on text AND button AND border AND background simultaneously
- ❌ Neon glow `box-shadow: 0 0 40px rgba(purple)` on cards/buttons/text
- ❌ Slate-900 + indigo-accent "default dark mode"
- ❌ Low-contrast `text-gray-400` body on white/dark (fails WCAG 4.5:1)
- ✅ **Do instead:** one intentional brand hue, 60-30-10 distribution, flat solid surfaces, functional shadows (`shadow-sm/md`, neutral), body text ≥ `gray-700` (light) / `gray-200` (dark).

### 🔤 TYPOGRAPHY — never generate
- ❌ Inter/Geist for both display and body with no second face
- ❌ Gradient `bg-clip-text text-transparent` headlines
- ❌ `text-5xl font-extrabold tracking-tighter` formula on every heading
- ❌ 5–6 font weights; arbitrary/flat type scale; centered multi-line body text
- ❌ `tracking-widest uppercase` eyebrow above every section
- ❌ Playfair Display + Inter "tasteslop" pairing; monospace for non-code "tech vibes"
- ✅ **Do instead:** distinctive display font (Fraunces, Bricolage Grotesque, Satoshi, Cabinet Grotesk), modular scale (1.25), max 2–3 weights (400/500/700), `leading-tight` headings / `leading-relaxed` body, `max-w-prose` (65ch), left-aligned paragraphs.

### 🃏 CARDS — never generate
- ❌ Identical 3-column feature grid (icon-in-rounded-square + title + blurb)
- ❌ Uniform `rounded-2xl shadow-lg border` on every card
- ❌ `hover:scale-105` (shifts layout) or dramatic `-translate-y-2` lifts
- ❌ Glassmorphism cards (`bg-white/10 backdrop-blur-lg`)
- ❌ Gradient-border cards; `border-l-4 border-purple-500` left accent
- ❌ Emoji icons; identical pricing cards with "Most Popular" ring; fake-avatar testimonials
- ✅ **Do instead:** vary card size/weight (one larger, one image-led), elevation hierarchy, `hover:shadow-md`/`hover:border-primary` color transitions only, solid surfaces, real logos over fake faces, max 5 pricing features.

### 📐 LAYOUT & SPACING — never generate
- ❌ Everything centered in `max-w-7xl mx-auto`
- ❌ Uniform `py-20` section rhythm (no spatial hierarchy)
- ❌ Symmetric `grid-cols-3` for every section
- ❌ Endless alternating image-left/text-right
- ❌ Identical section anatomy repeated (heading → subtext → 3-col → CTA ×7)
- ❌ One `gap-6` token for nav, cards, forms, and sections alike
- ✅ **Do instead:** vary container widths by content (`max-w-2xl` text / `max-w-6xl` grids / full-bleed breaks), vary section padding, mix grid structures (2/4/asymmetric spans), one dominant element per section, contextual gap scale (4px inline → 16px cards → 48px sections), ≥2 deliberate asymmetric moments per page.

### 🎯 ICONS & IMAGERY — never generate
- ❌ Emoji as functional icons (🚀⚡✨🎨💡)
- ❌ Icons in gradient rounded squares; rainbow per-icon coloring
- ❌ Mixed icon sets/variants/stroke widths
- ❌ Corporate-Memphis / Alegria blob-people illustrations (unDraw/Humaaans)
- ❌ AI-generated hero images with artifacts; generic stock "diverse-team-at-laptop"
- ❌ Glow behind icons; oversized (>48px) decorative line icons; status dots that map to nothing
- ✅ **Do instead:** ONE icon library + ONE variant, monochrome/2-color max, sized 16–24px UI, real product screenshots over illustration, every status dot maps to a documented state.

### ✍️ MICROCOPY & CONTENT — never generate
- ❌ Hype verbs: "Supercharge, Unlock, Revolutionize, Elevate, Seamless, Effortless, in seconds"
- ❌ Filler: "ensuring", "leveraging", "robust/powerful/scalable", "cutting-edge", "for modern teams"
- ❌ Generic CTAs: "Get Started", "Learn More" everywhere
- ❌ Fake testimonials ("Sarah M., CEO"), invented metrics, "Trusted by X,000+"
- ❌ Em-dash overuse, uniform paragraph length, "In today's landscape…" openers, emoji bullets, leftover Lorem ipsum
- ✅ **Do instead:** say what it does for whom with concrete numbers, specific CTAs ("Import your data"), real or clearly-placeholdered proof, varied sentence rhythm, a human voice with edge.

### 🎬 MOTION — never generate
- ❌ Universal fade-in-up on scroll (AOS on everything)
- ❌ `hover:scale-105` on every card; gratuitous Framer-Motion wrappers
- ❌ Infinite floating/bobbing blobs; marquee logo scrollers; animated gradient backgrounds
- ❌ Spring bounce on everything; typewriter effects; 500ms+ transitions
- ❌ **NO `prefers-reduced-motion` support** (this is an a11y FAILURE, not just slop)
- ✅ **Do instead:** animate ≤30% of elements, 100–150ms hovers / 200–300ms entrances / faster exits, `transform`+`opacity` only, `ease-out` in / `ease-in` out, **mandatory** `prefers-reduced-motion: reduce`, max 0–1 ambient infinite animation (must pause on hover).

### 🔘 BUTTONS / FORMS / NAV / FOOTER — never generate
- ❌ Gradient primary + ghost secondary pair; emoji-in-button; `rounded-full` pills everywhere; oversized `px-8 py-4 text-lg`
- ❌ Forms with no validation/error/success states; heavy `rounded-xl shadow` glass inputs; toggle overuse; generic gradient "newsletter" band
- ❌ Floating glassmorphic `backdrop-blur` navbar; shadcn icon+label sidebar clone for non-dashboards; premature sticky CTA bar
- ❌ 4-column mega-footer of dead `#` links + social icons you don't use; dark-pattern cookie banner
- ✅ **Do instead:** one solid-color primary per section + text-link secondary, `rounded-md/lg`, `px-4 py-2 text-sm`; inputs `border border-gray-300 rounded-md` with real error/success states; `fixed top-0 border-b` solid nav; minimal footer with only real links; equal-weight cookie choices (if needed at all).

---

## Style Guardrails (positive defaults)
- 8px spacing system; internal spacing ≤ external spacing (proximity = grouping).
- One dominant focal element per section; 60-30-10 color distribution.
- One primary action per section. Restrained emphasis.
- Radius **scale**, not one value: inputs/badges `rounded-md`, cards `rounded-lg`, modals `rounded-xl`, `rounded-full` only when semantically a pill/avatar.
- Real product UI, real data, real screenshots — never demo/placeholder UI.
- WCAG AA contrast minimum (4.5:1 body, 3:1 large/UI). Test light AND dark.

---

## Slop Severity Table

| Severity | Meaning | Examples |
|----------|---------|----------|
| **P0 (auto-block)** | Single-handedly screams AI; never ship | purple/indigo gradient, gradient-text headline, emoji icons, left-border cards, invented metrics, **no reduced-motion** |
| **HIGH** | Strong tell; fix before delivery | Inter-everywhere, centered dual-CTA hero, 3-card grid, `rounded-2xl` uniformity, pill gradient CTA, fake "Trusted by", glassmorphic nav, no form validation states, dead-link mega-footer, hype copy, `hover:scale-105` |
| **MEDIUM** | Trained eyes notice | perspective mockup+glow, bento misuse, uniform `py-20`, em-dash overuse, toggle overuse, oversized buttons, illustration-pack sameness |
| **LOW** | Subtle but cumulative | 0.1-opacity shadows, Space Grotesk fallback, wide letter-spacing, count-up numbers |

---

## Pre-Delivery Slop Audit (run every time)

**Color & Type**
- [ ] No `indigo`/`violet`/`purple` accent unless it is the real brand
- [ ] No gradient text; ≤1 gradient element per section
- [ ] Display font is NOT Inter/Geist; ≤3 font weights; modular scale
- [ ] Body text passes 4.5:1 contrast (light + dark)

**Layout & Cards**
- [ ] Container widths vary by content; section padding varies
- [ ] No identical 3-col grid repeated; ≥1 dominant element per section
- [ ] Radius is a differentiated scale, not one uniform value
- [ ] No `border-l-4` accent; no uniform glassmorphism; no emoji icons

**Copy**
- [ ] No "Supercharge/Unlock/Seamless/for modern teams"; CTAs are specific
- [ ] Every metric is real or clearly marked placeholder; no fake testimonials
- [ ] No leftover Lorem ipsum / `[placeholder]` / `TODO`

**Motion & A11y**
- [ ] `prefers-reduced-motion: reduce` handled everywhere
- [ ] Transitions ≤200ms, `transform`/`opacity` only, no `hover:scale-105` in grids
- [ ] Forms have label + error + success states; focus states visible
- [ ] Images have alt text; color is not the only indicator

**Gestalt check**
- [ ] Could I swap the logo with a competitor and nobody notices? → if yes, FAIL
- [ ] Does the Instant-Death Signature appear (3+ tells)? → if yes, regenerate
- [ ] Would this pass a senior designer in a 2-second glance? → if no, fix

---

## Output Expectations
The final UI must read as: **intentional, branded, content-aware, accessible,
production-ready — and unmistakably NOT machine-averaged.** Every spacing value,
color, font, grid, and animation must be defensible with a content-specific
reason. If you cannot explain *why* a choice was made, it is slop — remove or
replace it.

> For the exhaustive tell → why → fix → severity catalog across all components,
> see `references/slop-uiux-patterns.md`.
