# Anti-AI-UI

![Kiro](https://img.shields.io/badge/Kiro-Steering-6366f1?style=for-the-badge&logo=google-gemini)
![Version](https://img.shields.io/badge/version-2.0.0-blue?style=for-the-badge)
![3 Skills](https://img.shields.io/badge/skills-3-green?style=for-the-badge)
![44 Files](https://img.shields.io/badge/files-44-orange?style=for-the-badge)
![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)

A comprehensive steering suite for Kiro that eliminates AI slop and produces intentional, branded, accessible UI/UX — not the statistical-mean design defaults that make AI-generated interfaces instantly recognizable.

## The Problem

LLMs generate UI by averaging across training data dominated by Tailwind docs, shadcn/ui examples, abandoned repos with unchanged defaults, and 2020–2024 Dribbble SaaS shots. The output **collapses toward the visual mean** (arXiv:2603.13036, "Interrogating Design Homogenization in Web Vibe Coding").

**The result:** Every AI-generated landing page has purple/indigo gradients, Inter everywhere, centered dual-CTA heroes, 3-card feature grids, glassmorphism navbars, and hype copy like "Supercharge your workflow."

> If you could swap the logo with a competitor and nobody would notice — it's slop.

## The Solution

**Anti-AI-UI** provides three complementary steering skills that work together:

```
┌─────────────────────────────────────────────────────────────────┐
│  FILTER: Anti-AI-Slop                                          │
│  What to AVOID — hard bans on purple gradients, Inter-everywhere,│
│  glassmorphism, emoji icons, hype copy, no reduced-motion       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  PALETTE: Component Reference Design                           │
│  What to BUILD — curated patterns from MUI, shadcn, Radix,     │
│  MD3, Apple HIG, IBM Carbon, NNG, Refactoring UI               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  RESEARCH: UI/UX Pro Max                                       │
│  Design intelligence — 161 reasoning rules, 67 UI styles,      │
│  57 font pairings, industry-specific design system generator   │
└─────────────────────────────────────────────────────────────────┘
```

**Together:** Filter out slop → Build from proven patterns → Research industry-specific solutions.

## Features

### 🛡️ Anti-AI-Slop — The Filter

**Detect and ban the statistical-mean design defaults LLMs emit.**

- **Instant-Death Signature Detection** — Any 3+ tells (purple gradient + Inter + centered hero + dual CTA + 3-card grid + rounded-2xl + gradient pill button + "Trusted by" + "Supercharge" + aurora blobs + glassmorphism + emoji icons + left-border cards) = reads as AI in under 2 seconds
- **5 Slop Axes** — Hard decisions required for color, type, layout, copy, motion
- **Per-Component Negative Prompts** — Hero, color, typography, cards, layout, icons, microcopy, motion, buttons, forms, nav, footer
- **Slop Severity Table** — P0 (auto-block) → HIGH → MEDIUM → LOW
- **Pre-Delivery Slop Audit** — Color/type, layout/cards, copy, motion/a11y, gestalt check
- **Code-Level Patterns** — React/TSX/a11y slop detection (A11Y1–23)

**Example bans:**
```
❌ purple/indigo/violet accent
❌ Inter-everywhere (display + body)
❌ gradient bg-clip-text headlines
❌ glassmorphism backdrop-blur nav
❌ rounded-2xl uniformity
❌ hover:scale-105 on cards
❌ emoji icons (🚀⚡✨)
❌ "Supercharge/Unlock/Seamless" copy
❌ no prefers-reduced-motion
```

### 🎨 Component Reference Design — The Palette

**Curated, copy-pasteable, accessible patterns from trusted design authorities.**

**7 Reference Categories:**

| Category | File | Contains |
|----------|------|----------|
| **Layouts** | `references/layouts.md` | Holy-grail, sidebar shell, dashboard, split-screen, full-bleed, bento, masonry, magazine, asymmetric, sticky-aside, container-query, Z/F patterns + 2025–2026 page skeletons (Linear, Vercel, Stripe, Raycast, Clerk, Resend) |
| **Typography** | `references/typography.md` | 8+ display+body pairings, modular scales (1.2/1.25/1.333), `clamp()` recipes, per-role leading/measure/tracking, MUI/MD3/Apple/Carbon scales |
| **Components** | `references/components.md` | 17 card variants, 10 buttons, 13 form controls, 13 navigation, 16 data-display |
| **Animation** | `references/animation.md` | Modal, drawer, toast, tooltip, popover + 13 motion recipes (timing, easing, reduced-motion) |
| **Design Systems** | `references/design-systems.md` | 18 design-system index + W3C DTCG tokens + decision matrix |
| **Charts** | `references/charts.md` | Recharts patterns: color systems (teal/Okabe-Ito/OKLCH), premium chrome, bar/line/area/donut/radial, KPI+sparkline, specialty (heatmap/treemap/funnel/radar/scatter/bullet/waffle), 15-point anti-slop scan |
| **Dashboards** | `references/dashboards.md` | Modern clean admin: 15 principles, app shell, sidebar nav, topbar, ⌘K palette, grid composition, KPI cards, chart placement, filters, data table (TanStack), all states, 3-tier color tokens |

**Anti-Monotony Rules:**
1. Never repeat the same section anatomy consecutively
2. Vary container widths by content (text `max-w-3xl`, grids `max-w-6xl`, ≥1 full-bleed break)
3. Vary section padding (hero `py-32`, features `py-16`, CTA `py-12`)
4. One dominant element per section (size/weight ≥1.5× neighbours)
5. Break rhythm every 2–3 sections
6. Mix grid structures (2-col, 4-col, asymmetric spans)
7. ≥2 deliberate asymmetric moments per page

**Non-Negotiable Quality Bar:**
- Semantic HTML first (`<table>`, `<ul>`, `<dl>`, `<dialog>`, `<nav>`, `<button>`, `<a>`)
- Accessible by default (labels wired, `aria-*` present, focus-visible rings, keyboard reachable)
- `prefers-reduced-motion` gates every spatial animation
- Radius is a scale (inputs `rounded-md`, cards `rounded-lg`, modals `rounded-xl`, pills `rounded-full`)
- Distinctive display font (NOT Inter-everywhere), modular scale, `max-w-prose` body
- Motion budget: hovers 100–150ms, entrances 200–300ms, exits faster
- WCAG AA contrast: 4.5:1 body, 3:1 large/UI

### 🔍 UI/UX Pro Max — The Research Tool

**Design intelligence for building professional UI/UX across multiple platforms.**

**Database:**
- **161 Reasoning Rules** — Industry-specific design system generation (SaaS, Fintech, Healthcare, E-commerce, Services, Creative, Lifestyle, Emerging Tech)
- **67 UI Styles** — Glassmorphism, Claymorphism, Minimalism, Brutalism, Neumorphism, Bento Grid, Dark Mode, AI-Native UI, and more
- **161 Color Palettes** — Industry-specific palettes aligned 1:1 with product types
- **57 Font Pairings** — Curated typography combinations with Google Fonts imports
- **25 Chart Types** — Recommendations for dashboards and analytics
- **99 UX Guidelines** — Best practices, anti-patterns, accessibility rules
- **15 Tech Stacks** — React, Next.js, Astro, Vue, Nuxt.js, Svelte, SwiftUI, React Native, Flutter, HTML+Tailwind, shadcn/ui, Jetpack Compose, Angular, Laravel

**Design System Generator:**
```bash
python3 ui-ux-pro-max/scripts/search.py "beauty spa wellness service" \
  --design-system -p "Serenity Spa"
```

**Output:** Complete design system with pattern, style, colors, typography, effects, anti-patterns to avoid, and pre-delivery checklist.

**Domain-Specific Search:**
```bash
python3 ui-ux-pro-max/scripts/search.py "glassmorphism" --domain style
python3 ui-ux-pro-max/scripts/search.py "elegant serif" --domain typography
python3 ui-ux-pro-max/scripts/search.py "dashboard" --domain chart
python3 ui-ux-pro-max/scripts/search.py "form validation" --stack react
```

## Installation

### For Kiro Projects

Copy the steering suite into your project's `.kiro/steering/` directory:

```bash
# Clone this repo
git clone https://github.com/Vanszs/Anti-AI-UI.git

# Copy all 3 skills to your project
cp -r Anti-AI-UI/Anti-AI-SLop your-project/.kiro/steering/
cp -r Anti-AI-UI/component-reference-design your-project/.kiro/steering/
cp -r Anti-AI-UI/ui-ux-pro-max your-project/.kiro/steering/
```

### For Individual Skills

Pick only what you need:

```bash
# Just the filter (ban slop)
cp -r Anti-AI-UI/Anti-AI-SLop your-project/.kiro/steering/

# Just the palette (build from proven patterns)
cp -r Anti-AI-UI/component-reference-design your-project/.kiro/steering/

# Just the research tool (design intelligence)
cp -r Anti-AI-UI/ui-ux-pro-max your-project/.kiro/steering/
```

### Prerequisites

**Python 3.x** is required for the UI/UX Pro Max search script:

```bash
# Check if Python is installed
python3 --version

# macOS
brew install python3

# Ubuntu/Debian
sudo apt update && sudo apt install python3

# Windows
winget install Python.Python.3.12
```

## Usage

### Automatic Activation

When you open a project with these steering skills in Kiro, they activate automatically. Kiro reads the `SKILL.md` files and applies the rules to your UI/UX work.

### Workflow

1. **You ask** — Request any UI/UX task (build, design, create, implement, review, fix)
2. **Filter runs** — Anti-AI-Slop bans statistical-mean defaults
3. **Palette applies** — Component Reference provides proven patterns
4. **Research supports** — UI/UX Pro Max generates industry-specific design systems
5. **Pre-delivery audit** — Validates against slop tells and accessibility requirements

### Example Prompts

```
Build a landing page for my SaaS product
→ Filter: no purple gradients, no Inter-everywhere, no 3-card grid
→ Palette: asymmetric split hero, distinctive display font, varied section padding
→ Research: SaaS reasoning rules, conversion-optimized pattern

Design a dashboard for healthcare analytics
→ Filter: no glassmorphism, no emoji icons, no left-border cards
→ Palette: dashboard app shell, TanStack Table, KPI cards with delta inversion
→ Research: healthcare reasoning rules, data-dense dashboard style

Create a portfolio website with dark mode
→ Filter: no centered dual-CTA hero, no hover:scale-105, no gradient text
→ Palette: asymmetric bento grid, distinctive typography, motion budget
→ Research: portfolio reasoning rules, dark mode (OLED) style
```

### Direct Commands

**Generate design system:**
```bash
python3 ui-ux-pro-max/scripts/search.py "SaaS dashboard" \
  --design-system -p "MyApp"
```

**Search by domain:**
```bash
python3 ui-ux-pro-max/scripts/search.py "glassmorphism" --domain style
python3 ui-ux-pro-max/scripts/search.py "Cormorant Garamond" --domain typography
python3 ui-ux-pro-max/scripts/search.py "heatmap" --domain chart
```

**Stack-specific guidelines:**
```bash
python3 ui-ux-pro-max/scripts/search.py "form validation" --stack react
python3 ui-ux-pro-max/scripts/search.py "responsive layout" --stack html-tailwind
python3 ui-ux-pro-max/scripts/search.py "navigation" --stack nextjs
```

## Architecture

### Directory Structure

```
Anti-AI-UI/
├── Anti-AI-SLop/                      # FILTER — what to AVOID
│   ├── SKILL.md                       # Instant-death signature, 5 slop axes, per-component bans
│   └── references/
│       ├── slop-uiux-patterns.md      # Exhaustive tell→why-slop→fix→severity catalog
│       └── slop-code-patterns.md      # Code-level slop (React/TSX/a11y A11Y1–23)
│
├── component-reference-design/        # PALETTE — what to BUILD
│   ├── SKILL.md                       # Anti-monotony rules, trusted sources, quality bar
│   └── references/
│       ├── layouts.md                 # 16 layouts + 2025–2026 page skeletons
│       ├── typography.md              # 8+ pairings, modular scales, clamp() recipes
│       ├── components.md              # 17 cards, 10 buttons, 13 forms, 13 nav, 16 data-display
│       ├── animation.md               # Overlays + 13 motion recipes
│       ├── design-systems.md          # 18 design-system index + W3C DTCG tokens
│       ├── charts.md                  # Recharts patterns + 15-point anti-slop scan
│       └── dashboards.md              # Modern clean admin (15 principles)
│
└── ui-ux-pro-max/                     # RESEARCH — design intelligence
    ├── SKILL.md                       # Usage guide, design system generator
    ├── data/                          # CSV databases (161 rules, 67 styles, 57 pairings, etc.)
    │   ├── charts.csv
    │   ├── colors.csv
    │   ├── icons.csv
    │   ├── landing.csv
    │   ├── products.csv
    │   ├── react-performance.csv
    │   ├── styles.csv
    │   ├── typography.csv
    │   ├── ui-reasoning.csv
    │   └── ux-guidelines.csv
    ├── data/stacks/                   # Stack-specific guidelines (15 stacks)
    │   ├── astro.csv
    │   ├── flutter.csv
    │   ├── html-tailwind.csv
    │   ├── jetpack-compose.csv
    │   ├── nextjs.csv
    │   ├── nuxtjs.csv
    │   ├── nuxt-ui.csv
    │   ├── react.csv
    │   ├── react-native.csv
    │   ├── shadcn.csv
    │   └── ...
    └── scripts/                       # Python search engine
        ├── core.py
        ├── design_system.py
        └── search.py
```

### How the Three Skills Interact

1. **Anti-AI-Slop** runs first — detects and bans statistical-mean defaults
2. **Component Reference** provides the constructive alternative — what to build instead
3. **UI/UX Pro Max** generates industry-specific design systems when you need tailored guidance

**Together:** Filter → Palette → Research.

## Contributing

Contributions welcome! Areas for improvement:

- **Expand slop patterns** — Add new AI slop tells as they emerge
- **Add reference patterns** — More layouts, components, chart types, dashboard examples
- **Grow reasoning rules** — Industry-specific rules for new product categories
- **Stack coverage** — Add guidelines for new frameworks/stacks
- **Translate documentation** — Make skills accessible in more languages
- **Improve search engine** — Better BM25 ranking, fuzzy matching, multi-domain queries

### Development Workflow

```bash
# 1. Fork and clone
git clone https://github.com/YOUR_USERNAME/Anti-AI-UI.git
cd Anti-AI-UI

# 2. Create feature branch
git checkout -b feat/your-feature

# 3. Make changes in the appropriate skill directory
# - Anti-AI-SLop/           → Add slop patterns, severity levels
# - component-reference-design/ → Add patterns, update references
# - ui-ux-pro-max/          → Add reasoning rules, improve search

# 4. Test the design system generator
python3 ui-ux-pro-max/scripts/search.py "test query" --design-system

# 5. Commit and push
git add .
git commit -m "feat: description"
git push -u origin feat/your-feature

# 6. Create PR
gh pr create
```

### Guidelines

- **Anti-AI-Slop** — Add patterns to `references/slop-uiux-patterns.md` or `references/slop-code-patterns.md` with tell, why-slop, fix, severity
- **Component Reference** — Add patterns to the appropriate `references/*.md` file with when-to-use, concrete Tailwind markup, source
- **UI/UX Pro Max** — Add reasoning rules to `data/ui-reasoning.csv`, styles to `data/styles.csv`, etc.

## Acknowledgments

**This project includes the excellent work of:**

- **[UI/UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)** by [NextLevelBuilder](https://github.com/nextlevelbuilder) — The `ui-ux-pro-max/` directory contains their design intelligence system (161 reasoning rules, 67 UI styles, 57 font pairings, search engine). We use it as-is with their permission. Consider [supporting their project](https://paypal.me/uiuxpromax) if you find it valuable.

**Inspired by:**
- [MUI](https://mui.com/), [shadcn/ui](https://ui.shadcn.com/), [Radix](https://www.radix-ui.com/), [Headless UI](https://headlessui.com/)
- [Material Design 3](https://m3.material.io/), [Apple HIG](https://developer.apple.com/design/human-interface-guidelines), [IBM Carbon](https://carbondesignsystem.com/)
- [NNG](https://www.nngroup.com/), [Refactoring UI](https://www.refactoringui.com/), [Every-Layout](https://every-layout.dev/)
- [Tailwind UI](https://tailwindui.com/), [Ant Design](https://ant.design/), [Chakra UI](https://chakra-ui.com/)
- [Josh Comeau](https://www.joshwcomeau.com/), [Emil Kowalski](https://emilkowal.ski/), [Adrian Krebs](https://slop-detect.com/)

## License

This project is licensed under the [MIT License](LICENSE).

**UI/UX Pro Max** is also MIT licensed (see their [LICENSE](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill/blob/main/LICENSE)).

## About

Anti-AI Slop UI/UX steering skills for Kiro — filter, palette, and research tool to build intentional, branded, accessible interfaces.

**Stats:**
- 3 steering skills
- 44 reference files
- 161 reasoning rules
- 67 UI styles
- 57 font pairings
- 15 tech stacks
