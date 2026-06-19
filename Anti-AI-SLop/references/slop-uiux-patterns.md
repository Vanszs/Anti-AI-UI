# Anti-AI-Slop UI/UX — Exhaustive Pattern Catalog

Companion to `../SKILL.md`. Format for every entry: **TELL → WHY-SLOP → FIX → SEVERITY**.
Synthesized from deep research across slop-detect.com, Adrian Krebs' Show HN study
(1,590 sites scored), Thomas Wiegold, Noqta, The Fountain Institute, NNG, Refactoring
UI, WIRED, Built In, and arXiv:2603.13036 (design homogenization in vibe coding).

Severity: **P0** = single-handedly screams AI / a11y failure · **HIGH** = strong tell ·
**MEDIUM** = trained eyes notice · **LOW** = subtle, cumulative.

---

## 0. THE ROOT CAUSE & THE GESTALT

LLMs generate UI by computing a weighted average across their training distribution
(Tailwind docs + shadcn/ui + abandoned-default repos + 2020–2024 Dribbble SaaS shots).
Output **collapses toward the visual mean**. Every unspecified decision is filled with
the single most probable token. This is structural math, not a bug.

**The full gestalt formula — any 5+ = unmistakable AI slop:**
```
purple-gradient + Inter + 3-card grid + centered hero + rounded-2xl + pill CTA
+ "trusted by X teams" + "unlock/supercharge" copy + gradient blob background
+ shadcn dashboard grid + generic Lucide icons + letter-in-rounded-box logo
```

**The cure (one rule to rule them all):** every spacing value, color, font, grid,
and animation must be *chosen for a content-specific reason*. If you can't explain
WHY, it's slop.

---

## 1. HERO / ABOVE-THE-FOLD

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| H1 | Centered headline + subhead + symmetric dual CTA (filled primary + ghost secondary) | Statistically safest layout in training data; every 2020–2023 SaaS template | Left-align text; asymmetric split (text left / real product shot right); ONE dominant CTA + text-link secondary | HIGH |
| H2 | Gradient `bg-clip-text` headline (purple→pink/blue) | #1 most-cited visual AI tell; the visual "delve"; fails contrast on partial stops | Solid high-contrast headline; emphasis via weight/size/typeface | P0 |
| H3 | Eyebrow pill badge above H1 (`✨ Now with AI`, `🚀 New`) | slop-detect scores +5; decoration cosplaying as information | Remove unless genuinely time-bound; if kept, plain text, no emoji, no pill | HIGH |
| H4 | Aurora/gradient blobs (`blur-3xl` purple/blue circles) behind hero | "Radial light bloom for no particular reason"; saturates 2020–22 Dribbble data | Solid bg, subtle noise/grain, or single-tone radial ≤5% opacity | HIGH |
| H5 | Perspective-tilted dashboard mockup + glow (`rotateY(-5deg) shadow-2xl`) | Canonical "AI product shot"; nobody screenshotted real software | Flat, straight-on real product screenshot; minimal flat device frame | MEDIUM |
| H6 | Greyed fake "Trusted by 5,000+ teams" logo strip | Logos often hallucinated/unlicensed; domain registered 3 weeks ago | Only verified customer logos w/ permission; else omit; use named case studies | HIGH |
| H7 | `h-screen` hero with only text + button | PowerPoint-slide emptiness; pushes content below fold | `py-24 sm:py-32 lg:py-40`; full-height only with rich visual filling ≥60% | LOW |
| H8 | Standard skeleton hero→features→pricing→FAQ→CTA→footer | "Default WordPress theme" structure; most-probable page order | Introduce ≥1 unconventional section; break the template rhythm | MEDIUM |

---

## 2. COLOR & GRADIENT

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| C1 | `indigo-500`/`violet-500`/`purple-500` (`#6366f1 #8b5cf6 #a855f7`) as primary accent | Tailwind's 5-yr default button color; >50% of YC Spring'25 AI cos used it | ONE brand hue that is NOT purple/indigo; define via CSS var / config | P0 |
| C2 | `from-purple-500 to-pink-500` / `from-indigo-500 to-purple-600` / `from-blue-500 to-cyan-500` | Exact gradient combos dominating 2020–22 tutorials; "safest choice AI can make" | Single-hue gradient (brand-500→700) max, or flat color; gradient must draw the eye to ONE focal point | P0 |
| C3 | Gradient on text AND button AND border AND background at once | Each fine alone; all together = converged fingerprint | Budget ONE gradient element per section | HIGH |
| C4 | Neon glow `box-shadow: 0 0 40px rgba(purple)` on cards/buttons/text | SaaS WordArt; when everything glows nothing is emphasized | Shadows = elevation only; neutral `shadow-sm/md`; glow only on focus ring | MEDIUM |
| C5 | Aurora blobs / mesh gradient washes (`blur(100px)` saturated circles) | Named slop-detect pattern; every vibe-coder default for "modern" bg | Noise texture / single-tone radial ≤10%; never animate (paint storms) | MEDIUM |
| C6 | `bg-gradient-to-br from-X to-Y` on every surface | Most common gradient direction in tutorials; kills hierarchy | Flat solids for 90% of surfaces; `to-b` if any; ONE gradient bg per page | MEDIUM |
| C7 | Low-contrast `text-gray-400`/`slate-400` body on white/dark | 83.9% of homepages fail WCAG contrast (WebAIM 2026); "elegant to you, invisible to millions" | Body ≥ `gray-700` (light) / `gray-200` (dark); verify 4.5:1 | P0 (a11y) |
| C8 | "Just slate-900 + indigo accent" default dark mode | Every Tailwind dark-mode tutorial; zero brand intent | Warmer dark surface (`zinc-900`); brand-matched accent; 2–3 surface elevations | HIGH |
| C9 | Oversaturated "vibrant" palette (every accent at -500) | AI stacks every high-performing color at once; busy & artificial | 60-30-10; desaturate to -700/-800 text, -50/-100 bg; max 2 saturated/viewport | MEDIUM |
| C10 | No brand color system (random hex, interchangeable identity) | "Nobody made a decision here"; meta-tell enabling all other color slop | Define 1 primary + ramp, 1–2 accents, 8–10 greys, semantic colors; tokens in config | HIGH |

---

## 3. TYPOGRAPHY

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| T1 | Inter/Geist for display AND body, no second face | Default in Figma/Tailwind/v0/shadcn; "safety creates invisibility" | Distinctive display ≠ Inter (Fraunces, Bricolage Grotesque, Satoshi, Cabinet Grotesk); Inter OK for body only if paired | HIGH |
| T2 | Gradient `bg-clip-text text-transparent` headline | Every v0/Lovable/Bolt output; +6 on slop-detect | Solid color, high contrast | P0 |
| T3 | `text-5xl font-extrabold tracking-tighter` on every heading | "Crushed display tracking"; max-impact formula = sameness | Modular scale (1.25); bold(700) max weight; `tracking-tight` not tighter | HIGH |
| T4 | Flat/arbitrary type scale (8–12 random sizes, or H1≈H2) | No ratio, no rhythm; hierarchy breaks | Modular scale, 4–6 sizes: 12/14/16/20/25/31/39 | HIGH |
| T5 | 5–6 font weights on one page | Each component generated in isolation; eye can't anchor | Max 3 weights (400/500/700); never Light body | MEDIUM-HIGH |
| T6 | Centered multi-line body paragraphs | "Safe" symmetry; ragged left edge kills readability (WebAIM) | Body always `text-left`; center only single-line headings/taglines | MEDIUM-HIGH |
| T7 | Uniform `leading` on everything (1.5 on headings too) | One value applied universally; no proportional adjustment | Headings `leading-tight` (1.25); body `leading-relaxed` (1.625) | MEDIUM |
| T8 | Full-width body text (80–120 chars/line) | AI ignores measure; tracking fails >75 chars | `max-w-prose` (65ch) on every text block | MEDIUM-HIGH |
| T9 | `tracking-widest uppercase text-xs` eyebrow above every section | When every section has one, none stand out; auto-applied everywhere | 1–2 per page; `tracking-wide` max; `text-muted-foreground` | MEDIUM |
| T10 | Emoji as heading/section icons (`🚀 Features`) | Renders per-OS; zero semantics; demo-quality | SVG icons (Lucide/Heroicons/Phosphor), `currentColor`, fixed viewBox | HIGH |
| T11 | Playfair Display + Inter "tasteslop" pairing | WIRED-coined; most-suggested serif; "premium mediocre" new default | Non-Playfair serif (Fraunces/Instrument Serif/Newsreader); shared x-height | MEDIUM-HIGH |
| T12 | Positive `tracking-wide` on body copy | "Wide body tracking" slop-detect tell; loses word-shape | `tracking-normal`/`tight` on body; wide only on <14px caps labels | MEDIUM |
| T13 | Monospace for non-code "tech vibes" | Decoration; reduces prose readability | Mono ONLY for `<code>/<pre>/<kbd>` | MEDIUM |
| T14 | Inconsistent type across components (nav Inter, hero Poppins…) | Token drift; "isolated statistical events" | Define typography once in config; max 2 families site-wide | MEDIUM-HIGH |

> **Type slop fingerprint (slop-detect):** Geist/Inter default + crushed display tracking + wide body tracking + flat hierarchy = "Heavy" on type axis alone.

---

## 4. CARDS

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| K1 | Identical 3-col feature grid (icon-in-rounded-square + title + blurb) | THE #1 AI layout fingerprint; statistical mean of every SaaS features section | Vary card sizes (one `col-span-2`); remove some containers; use list/table for uniform items | HIGH |
| K2 | Uniform `rounded-2xl shadow-lg border` on every card | One safe shadow + radius applied everywhere; flattens hierarchy | Shadow hierarchy (`shadow-sm` cards, `shadow-xl` modals); drop border OR shadow | HIGH |
| K3 | `hover:scale-105` in a grid | Tutorial-grade; overlaps neighbors; CLS | `hover:-translate-y-0.5 hover:shadow-md` or `hover:border-primary/50`, ≤200ms | HIGH |
| K4 | Glassmorphism cards (`bg-white/10 backdrop-blur-lg`) | Peak-Dribbble 2021–22; fails contrast; GPU cost; invisible in light mode | Solid `bg-card`; glass on ≤1 element/viewport, min `bg-white/80`, `backdrop-blur-sm` | HIGH |
| K5 | Gradient-border cards (purple→blue wrapper hack) | "Safest choice AI can make"; pure decoration | Solid border conveying state; `ring-1 ring-primary/20` for emphasis | MEDIUM-HIGH |
| K6 | `border-l-4 border-purple-500` left accent | "Almost as reliable a sign as em-dashes for text" (Krebs); lazy accent | Remove; left-border only for semantic alert/callout state | P0 |
| K7 | Emoji icons in cards | Inconsistent cross-platform; can't theme; amateur | Consistent SVG set, monochrome, `w-5 h-5` | HIGH |
| K8 | Identical pricing cards + middle "Most Popular" `ring-2` | Most-templated SaaS section; lists 15+ features (people compare 3–5) | 5 features max; differentiate by bg fill not ring; full table below | MEDIUM |
| K9 | Testimonial cards w/ fake avatars ("Sarah M., CEO", 5/5 stars) | AI invents personas; FTC fines $51k per undisclosed fake | Real quotes + company logos > faces; mark placeholders explicitly; mixed ratings | MEDIUM-HIGH |
| K10 | Over-nested card-in-card (padding bloat) | AI wraps everything; flattens hierarchy | Max 1 nesting level; use `space-y` + dividers, not nested containers | MEDIUM |
| K11 | Dramatic hover lift (`-translate-y-2 shadow-xl duration-500`) | "Impressive" in isolation; sluggish; shifts row | ≤2px translate, shadow-only, ≤200ms, no bounce easing | MEDIUM |
| K12 | Meaningless "Live"/"New" pulsing pills | "Decoration cosplaying as information" | Badges only for real status; kill `animate-pulse` on static data | MEDIUM |

---

## 5. LAYOUT & SPACING

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| L1 | Uniform `py-20` on every section | Metronomic rhythm; no semantic weight | Vary: hero `py-40` → features `py-16` → CTA `pt-12 pb-16`; no two adjacent equal | HIGH |
| L2 | Everything in `max-w-7xl mx-auto` | Single centered column; "average of all design ever fed in" | Vary by content: text `max-w-3xl`, grids `max-w-6xl`, ≥1 full-bleed/page | HIGH |
| L3 | Symmetric `grid-cols-3` for every section | Most over-represented layout in training data | Mix cols (2/4/asymmetric spans); one hero cell `md:col-span-2`; never repeat consecutively | HIGH |
| L4 | Endless alternating image-left/text-right | Z-pattern to absurd repetition; stale after 2 | Max 2 alternations → break w/ full-width / grid / stats bar; vary split ratios | HIGH |
| L5 | Equal visual weight everywhere | No focal point; eye bounces & leaves | 3-size system; ONE dominant element ≥1.5× weight per section | HIGH |
| L6 | Bento grid misuse (uniform cells called "bento") | Aesthetic copied without size=hierarchy logic | Must have 2×2 dominant + mixed 1×1/2×1; else it's a card grid | MEDIUM |
| L7 | Identical section anatomy ×7 (heading→subtext→3col→CTA) | Convergence effect; banner-blindness on whole sections | Vary anatomy: left-aligned / centered / split / full-bleed / stats bar | HIGH |
| L8 | No spacing intentionality (all `gap-2` cramped or all `py-32` drowning) | Inherits template defaults or runs out of context | Internal ≤ external rule; 8px scale; articulate every gap | MEDIUM |
| L9 | Inconsistent gutters/padding (random `p-4`/`p-6`/`px-5`) | No base unit; "off" without knowing why | One scale (4px multiples); one gutter per context; audit w/ grid overlay | MEDIUM |
| L10 | Mechanical Z/F-pattern, never broken | Predictable = forgettable | Pattern-interrupt every 2–3 sections (full-bleed, offset, color break) | MEDIUM |
| L11 | Perfect bilateral symmetry, zero tension | "Static and boring" (Smashing); centered templates are "safe" | ≥2 asymmetric moments/page; left-align headings; unequal column widths | MEDIUM |
| L12 | One `gap-6` token for nav, cards, forms, sections | Breaks Gestalt proximity signaling | Contextual: 4–8px inline, 12–16px forms, 16–24px cards, 48–64px sections | MEDIUM |

> **Meta:** internal spacing ≤ external spacing (proximity = grouping). 8px base scale: 4/8/12/16/24/32/48/64/96.

---

## 6. ICONS, ILLUSTRATION & IMAGERY

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| I1 | Emoji as functional icons (🚀⚡✨🎨💡) | "Em-dash of AI UI"; frequency not intention | SVG set only; emoji only in chat/notification content | HIGH |
| I2 | Icons in gradient rounded squares (feature grid) | Most-recognized AI pattern; equal weight + decorative bg | Icons w/o bg containers; vary hierarchy; kill left-border accent | HIGH |
| I3 | Mismatched icon sets (Heroicons + Lucide + FA mixed) | No visual memory across components; name-matched per icon | ONE library + ONE variant; consistent `w-5/w-6` | HIGH |
| I4 | Generic uncustomized Lucide/Heroicons (`Rocket Sparkles Zap`) | Names frequent in feature-section data; "vibes" not meaning | Match stroke to type weight; tint w/ brand; choose for meaning | MEDIUM |
| I5 | Corporate-Memphis / Alegria blob-people (unDraw/Humaaans) | Overrepresented 2017–22; reads generic, insincere, dated | Custom illustration, photography, or none; product screenshots | MEDIUM |
| I6 | AI-generated hero images w/ artifacts; stock "team-at-laptop" | Converges on stock conventions; polished but hollow; 63% bounce | Real product screenshots/recordings; bold typographic hero | HIGH |
| I7 | Undifferentiated icon "spray" (6–12 equal icons) | Flat feature list; no IA | Prioritize top 2–3; vary sizes; group; drop unneeded | MEDIUM |
| I8 | Oversized decorative icons (>48px line icons) | "Big icon = impressive" Dribbble logic; strokes break | UI 16–24px, hero features ≤48px; >48px needs real illustration | MEDIUM |
| I9 | Glow behind icons (`drop-shadow 0 0 12px`) | "Glow = premium" gaming/crypto data; competes w/ content | No glow; subtle bg circle for emphasis | HIGH |
| I10 | 3D claymorphism icons | 2021–22 Dribbble trend; uncanny in production, low contrast | Flat monochrome icons, legible at 16px | LOW |
| I11 | Illustration-pack sameness (same pack every section) | Shared resource = shared identity = zero differentiation | Custom > paid > free packs; recolor to brand; or none | MEDIUM |
| I12 | Status dots on everything (colors map to nothing) | "Colored dot = status" extracted & over-applied | Every dot = documented state; else text label or remove; 3 states max | MEDIUM |
| I13 | Rainbow icon coloring (each icon different hue) | Per-icon semantic color, no system | 1–2 brand colors max; monochrome reads professional | HIGH |
| I14 | Letter-in-rounded-square logo placeholder | Visual "lorem ipsum" for branding; every AI tool default | Real logo mark; never ship monogram-in-box as identity | MEDIUM |

---

## 7. MICROCOPY & CONTENT

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| M1 | Hype verbs: Supercharge/Unlock/Revolutionize/Elevate/Empower/Unleash | Training saturated w/ SaaS marketing; generic | Concrete action verb + metric: "reduces load 40%" | HIGH |
| M2 | "Seamless/Effortless/Frictionless" | Vague intensifiers, no mechanism | State the mechanism ("one API call") | HIGH |
| M3 | "in seconds/instantly/blazing fast" | Unmeasured time claims = filler | Real benchmark ("avg 2.3s cold start") | HIGH |
| M4 | "Robust/Scalable/Powerful/Cutting-edge/World-class" | Hollow superlatives; 90%+ of AI copy | Quantify ("10K concurrent connections") | HIGH/MED |
| M5 | "for modern teams" | 97% AI match (shitfa.st); could be any product | Name who & what specifically | HIGH |
| M6 | "X plays a crucial role in shaping Y" / "rather than" / "ensuring that" | Strongest corpus-proven AI trigrams (WriteHuman) | Direct causation; active verbs; cut hedges | HIGH |
| M7 | "In today's ever-evolving landscape…" openers | Template scaffolding | Start with the specific thing; no throat-clearing | HIGH |
| M8 | "Get Started" / "Learn More" on every button | Statistical mode of all SaaS CTAs; zero info | Action-specific: "Import your data", "See pricing for teams" | HIGH |
| M9 | "The Future of X is Here" / "Transform Your Workflow" headlines | Most clichéd AI headline patterns | State what it does in one line; lead w/ pain or metric | HIGH |
| M10 | Headline + subhead saying the same thing | Redundant pair | Headline=WHAT, subhead=HOW/FOR-WHOM | HIGH |
| M11 | Generic testimonials ("Sarah M.", "transformed our workflow!") | Invented archetypes, no specifics | Real name+company+photo+specific outcome, or none | HIGH |
| M12 | Generic feature names ("Advanced Analytics", "Powerful Integrations") | Adjective padding, no differentiator | Name the thing ("Funnel drop-off heatmap") | HIGH |
| M13 | Zero concrete numbers anywhere | AI can't invent real metrics so avoids them | ≥1 real number/limit/benchmark per feature section | HIGH |
| M14 | Emoji bullets (🚀✨💡⚡) | Decorative; vibe-coded | Plain bullets or SVG icons | HIGH |
| M15 | Leftover Lorem ipsum / `[placeholder]` / TODO | Generated skeleton never filled | grep "lorem/placeholder/[/TODO" pre-delivery | HIGH |
| M16 | Em-dash overuse (2+/paragraph) | 18.5% AI text vs 7.1% human (WriteHuman) | Max 1 per 3 paragraphs; commas/periods/parens | MEDIUM |
| M17 | Uniform ~3-sentence paragraphs | Robotic cadence | Vary: 1-sentence punches, 5-sentence dives | MEDIUM |
| M18 | Rule-of-three triplets every sentence | AI produces 3 examples obsessively | Vary list lengths (2, 4); break the pattern | MEDIUM |
| M19 | "In short…/Ultimately…/At the end of the day…" closers | LLM termination convention | End w/ action item or link, or just stop | MEDIUM |
| M20 | Artificially cheerful / sanitized tone | RLHF positivity bias; 35% of new sites (Imperial/Stanford) | Specific human voice w/ edge; name concrete problems | MEDIUM |

> **Meta-rule:** the #1 AI content tell is *absence of specificity*. If you can swap the product name and the copy still works → slop.
> **Corpus red-flag words (overrepresentation):** ensuring 4.3× · highlights 4.3× · broader 6.1× · reflects 3.6× · significantly/effectively/reducing 3.1× · essential 2.6× · leveraging/utilizing/delve/landscape/tapestry/multifaceted (high). Replace with plain verbs.

---

## 8. MOTION & ANIMATION

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| MO1 | Universal fade-in-up on scroll (AOS on everything) | Most common scroll pattern; uniformity kills hierarchy; delays content | Animate section containers not elements; ≤2–3/viewport; fire once; 200–300ms, 8–12px | HIGH |
| MO2 | `hover:scale-105` on every card | First Tailwind hover example; tutorial-grade; CLS | `hover:shadow-lg hover:-translate-y-0.5`, `scale-[1.02]` max, 150ms | HIGH |
| MO3 | Gratuitous Framer-Motion wrappers (default spring on all) | Same `y:20, opacity:0` copy-pasted; library defaults everywhere | 3–4 motion primitives; custom cubic-bezier; `useReducedMotion`; transform/opacity only | HIGH |
| MO4 | Infinite floating/bobbing blobs | Cheap "dynamic" feel; drains battery; vestibular trigger | Remove; max 1 ambient/page; must pause on hover + reduced-motion | HIGH |
| MO5 | Marquee logo scrollers | Template staple; unreadable moving targets; WCAG 2.2.2 | Static grid; if used: pause on hover/focus, control, >30s, reduced-motion fallback | MEDIUM-HIGH |
| MO6 | Animated gradient backgrounds (`background-size:400%`) | Signature of vibe-coded sites; NOT GPU-composited, constant repaint | Static gradient; if animated use `@property`, hero only, 20s+, reduced-motion | HIGH |
| MO7 | Shimmer/shine on non-loading content | Confuses skeleton-loader w/ decoration | Shimmer = loading state ONLY | MEDIUM |
| MO8 | Parallax overuse / scroll-jacking | NNG/WCAG 2.3.3: dizziness, nausea, migraines | Max 1 subtle hero parallax (0.1–0.2); never text; reduced-motion disables | HIGH |
| MO9 | Spring bounce on everything | Framer default `type:spring`; toylike for all UI | Tween + custom easing for standard UI; spring only for drag snap-back | MEDIUM-HIGH |
| MO10 | **No `prefers-reduced-motion` support** | Almost never in AI code; physical harm to ~5% of users | EVERY animation gated; additive (`no-preference`) or subtractive reset | **P0 (a11y)** |
| MO11 | 500ms+ transitions | "Safe" round number; feels sluggish (NNG) | Hovers 100–150ms, entrances 200–300ms, never >400ms | HIGH |
| MO12 | Typewriter text effects | 2015 novelty / "my first portfolio"; delays info | Remove; if kept ≤1/page, 30–50ms/char, static fallback | MEDIUM |
| MO13 | Uniform stagger 100ms on all lists | One pattern learned; page feels still-loading | 30–50ms stagger, ≤300ms total; long lists animate as group | MEDIUM |
| MO14 | Identical enter/exit timing (`ease-in-out` both) | No physics distinction | Exit 25–35% faster; `ease-out` in, `ease-in` out | MEDIUM |
| MO15 | `transition-all` globally | Safety blanket; transitions layout props, thrash | Explicit `transition-colors/shadow/opacity`; only interactive elements | MEDIUM-HIGH |

> **Golden rules:** animate ≤30% of elements · transform+opacity only (never width/height/top/left/filter) · ease-out in / ease-in out · reduced-motion mandatory · every animation must answer "what user task does this support?"

---

## 9. BUTTONS / FORMS / NAV / FOOTER

### Buttons
| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| B1 | Gradient primary + ghost secondary always paired ("Get Started"+"Learn More") | Statistical button pair from Tailwind UI/shadcn; uniform fingerprint | ONE primary/section + text-link secondary; specific copy | HIGH |
| B2 | Emoji inside buttons (`🚀 Launch`) | Laziest visual affordance; no design system | Icon library `w-4 h-4`, or no icon | HIGH |
| B3 | `rounded-full` pill buttons everywhere | "Modern" default; hierarchy collapses | `rounded-md/lg`; pill only when semantically a tag/avatar | HIGH |
| B4 | Oversized `px-8 py-4 text-lg` buttons | "Prominent" = "huge"; screams desperation | Standard `px-4 py-2 text-sm`; hero `px-6 py-3 text-base` | MEDIUM |
| B5 | Purple gradient CTA `from-violet-600 to-blue-500` + `hover:opacity-90` | THE most-cited tell; Wathan apologized for indigo default | Solid brand color; `hover:bg-[color]-700` | HIGH |
| B6 | Generic CTA copy ("Get Started"/"Join Now") | Highest-probability text; says nothing | Name the action/benefit; first-person lifts 30–40% | MEDIUM |

### Forms
| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| F1 | No validation/error/success states (happy-path only) | One-shot gen skips multi-state thinking; functionally incomplete | Every input: label + error (`border-red-500` + msg) + success + helper; never silent disabled submit | HIGH |
| F2 | Heavy `rounded-xl shadow glass` inputs | Card aesthetic on inputs; should be invisible infra | `rounded-md border border-gray-300 px-3 py-2`, focus ring only, no shadow | MEDIUM |
| F3 | Toggle/switch overuse | shadcn Switch is pretty; implies instant-apply | Toggles only for instant binary; checkboxes for form submit; ≤2–3 visible | MEDIUM |
| F4 | Generic gradient "newsletter signup" band | Cargo-cult; on 80%+ of training sites; no value prop | Remove or inline w/ specific value ("Weekly tips, 5-min read"); never full-width gradient | MEDIUM |

### Navigation
| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| N1 | Floating glassmorphic `backdrop-blur` navbar | slop-detect Glassmorphism axis; novel 2021, ubiquitous by 2024 | `fixed top-0 inset-x-0 bg-white border-b`; no blur/float/rounded | HIGH |
| N2 | Premature sticky CTA bar (on load) | Copies e-commerce; desperate before user understands product | Only after scroll past primary CTA on long pages; slide-in | MEDIUM |
| N3 | shadcn icon+label sidebar clone for non-dashboards | Most-copied AI dashboard layout | Choose nav by IA: <5 items→top nav; customize visual treatment | MEDIUM |
| N4 | Navbar logo-left / links-center / pill-CTA-right | Appears in every AI output; indistinguishable | Vary layout/spacing/type for personality | MEDIUM |

### Footer
| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| FO1 | 4-column mega-footer of dead `#` links + unused social icons | "Real companies have big footers" cargo-cult; fiction for MVP | Only real links; logo + 3–4 links + copyright until 16+ real pages | HIGH |
| FO2 | Cookie-banner cliché w/ dark-pattern asymmetry | Copies common banner incl. dark patterns; usually non-functional | No banner if no tracking; equal-weight Accept/Reject; must be functional | MEDIUM (HIGH if asymmetric) |

### Cross-cutting
| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| X1 | Uniform border radius on everything | "Identical corners → hierarchy disappears" (Krebs) | Differentiated scale: inputs/buttons `rounded-md`, cards `rounded-lg`, modals `rounded-xl` | HIGH |
| X2 | Shadows at exactly 0.1 opacity (Tailwind default) | Part of the fingerprint; communicates no depth hierarchy | Vary by importance; most elements need none | LOW |
| X3 | Left-border accent on cards | #1 single-component tell (Krebs/Grigorev) | Remove; state-only emphasis (alerts) | HIGH |
| X4 | Eyebrow pill badge above hero H1 | +5 slop-detect; decorative info on every AI page | Remove unless time-sensitive; no pill, no emoji | HIGH |

---

## 10. SLOP-vs-CORRECT TAILWIND SNIPPETS

### Button
```html
<!-- SLOP -->
<button class="bg-gradient-to-r from-purple-600 to-blue-500 text-white px-8 py-4 rounded-full text-lg font-bold shadow-lg hover:opacity-90">
  🚀 Get Started
</button>
<button class="border border-gray-300 text-gray-600 px-8 py-4 rounded-full text-lg hover:bg-gray-50">Learn More →</button>

<!-- CORRECT -->
<button class="bg-blue-700 text-white px-4 py-2 rounded-md text-sm font-medium hover:bg-blue-800 transition-colors">
  Create your first report
</button>
```

### Input
```html
<!-- SLOP -->
<input class="w-full rounded-xl shadow-md border-0 bg-white/80 backdrop-blur-sm px-6 py-4 text-lg focus:ring-4 focus:ring-purple-500/50" placeholder="Enter your email" />

<!-- CORRECT -->
<div>
  <label for="email" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
  <input id="email" type="email" class="w-full rounded-md border border-gray-300 px-3 py-2 text-sm focus:border-blue-500 focus:ring-1 focus:ring-blue-500" />
  <p class="mt-1 text-sm text-red-600 hidden" id="email-error">Please enter a valid email address</p>
</div>
```

### Navbar
```html
<!-- SLOP -->
<nav class="fixed top-4 left-4 right-4 mx-auto max-w-7xl bg-white/10 backdrop-blur-xl border border-white/20 rounded-2xl shadow-lg z-50 px-6 py-3">

<!-- CORRECT -->
<nav class="fixed top-0 inset-x-0 bg-white border-b border-gray-200 z-50 px-4 sm:px-6 lg:px-8">
  <div class="mx-auto max-w-7xl flex items-center justify-between h-16">
```

### Card hover
```css
/* SLOP */ .card { @apply rounded-2xl shadow-lg border border-gray-200 hover:scale-105 transition-transform duration-300; }
/* CORRECT */ .card { @apply rounded-lg border border-border hover:shadow-md hover:border-primary/30 transition-all duration-150 ease-out; }
```

### Motion (reduced-motion)
```css
/* SLOP — no guard, layout props, slow */
* { @apply transition-all duration-500; }

/* CORRECT — additive + guarded */
@media (prefers-reduced-motion: no-preference) {
  .reveal { opacity: 0; transform: translateY(8px); transition: opacity 250ms ease-out, transform 250ms ease-out; }
}
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: .01ms !important; transition-duration: .01ms !important; scroll-behavior: auto !important; }
}
```

### Footer
```html
<!-- SLOP: grid-cols-4 of dead links + 5 social icons -->
<!-- CORRECT -->
<footer class="border-t border-gray-200 py-8 mt-16">
  <div class="max-w-5xl mx-auto px-4 flex flex-col sm:flex-row items-center justify-between gap-4">
    <p class="text-sm text-gray-600">© 2026 JagaSekolah</p>
    <nav class="flex gap-6 text-sm text-gray-600">
      <a href="/docs" class="hover:text-gray-900">Dokumentasi</a>
      <a href="/privacy" class="hover:text-gray-900">Privasi</a>
      <a href="https://github.com/..." class="hover:text-gray-900">GitHub</a>
    </nav>
  </div>
</footer>
```

---

## 11. SLOP-DETECT SCORING AXES (quick self-scan)
slop-detect.com scores 27 deterministic patterns across 4 axes. Trigger several → "Heavy":
- **Fonts:** Geist/Inter default · crushed display tracking · wide body tracking · flat hierarchy
- **Gradients:** gradient hero text · aurora blobs · colored glows · gradient-heavy bg
- **Layout:** centered dual-CTA hero · 3-card grid · eyebrow pill · uniform radius · glassmorphism
- **Copy:** hype verbs · "for modern teams" · invented metrics · generic CTA

Adrian Krebs scored 1,590 Show HN sites: **22% showed "Heavy" AI tells.** Most reliable
single signals he found: colored left-borders, gradient text, indigo accent, eyebrow pill.

---

## 12. SOURCES
- **arXiv:2603.13036** — Shin et al., "Interrogating Design Homogenization in Web Vibe Coding" (HCI)
- **Imperial College/Stanford/Internet Archive (2026)** — ~35% of new sites AI-generated (via WIRED, Fast Company)
- **slop-detect.com** — 27-pattern deterministic scoring engine (Adrian Krebs)
- **Adrian Krebs** — Show HN AI-design scoring study (1,590 sites, April 2026)
- **Thomas Wiegold** — "The AI slop fingerprint"; Adam Wathan indigo-default apology
- **Noqta.tn** — "Fix the 4 Overused AI UI Patterns" (Wes Bos "four horsemen" + 5th tell)
- **The Fountain Institute** — "7 signs of vibe-coded UI"
- **Built In (Tanya Donska)** — "The New Skeuomorphism: How AI Makes Bad Design Look Good Enough"
- **WIRED (Jun 2026)** — "AI Has Come for Serif Fonts" ("tasteslop")
- **NNG** — visual hierarchy, animation duration, scroll-triggered text, grids
- **Refactoring UI** (Wathan/Schoger) — palette, spacing, 3-size hierarchy
- **WebAIM Million 2026** — 83.9% of homepages fail contrast
- **WriteHuman (2026)** — 80K-pair corpus of AI text markers
- **W3C WCAG** — 2.2.2 Pause/Stop/Hide · 2.3.3 Animation from Interactions
- **Paco Valdez**, "Fifty Shades of Indigo" (YC S25) · **deeplearning.fr** "Hidden Purple Bias"
- **Smashing Magazine** — compositional balance, motion preferences · **shitfa.st** — scoring rubrics
