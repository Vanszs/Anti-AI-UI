# Design Systems Index + Tokens + Marketing Sections

Companion to `../SKILL.md`. Which library to reach for, how to structure tokens, and varied marketing-section anatomies. Sources: official docs of each system, W3C DTCG, Evil Martians (100 dev-tool landing pages study), Tailwind UI.

---

# PART A — TRUSTED DESIGN SYSTEM INDEX (18)

| System | Ecosystem | Styling | A11y | Theming | Best for | License | URL |
|--------|-----------|---------|------|---------|----------|---------|-----|
| **MUI** | React | Emotion CSS-in-JS / `sx` / Pigment | strong | `createTheme()` palette/typography/components | enterprise dashboards, 70+ components fast | MIT | mui.com |
| **shadcn/ui** | React/Next | Tailwind + CSS vars | inherits Radix | CSS vars in globals.css; you own the code | full ownership, no lock-in, unique identity | MIT | ui.shadcn.com |
| **Radix Primitives** | React | unstyled | gold standard (WAI-ARIA) | none (`data-state` hooks) | custom design systems on bulletproof behavior | MIT | radix-ui.com |
| **Radix Themes** | React | pre-styled + CSS vars | inherits Radix | `<Theme accentColor radius scaling>` | quick accessible prototypes w/o Tailwind | MIT | radix-ui.com/themes |
| **Headless UI** | React/Vue | unstyled (Tailwind-paired) | full WAI-ARIA | none | ~10 complex interactive components + Tailwind | MIT | headlessui.com |
| **Ant Design** | React/Vue/Angular | CSS-in-JS (cssinjs) | good (gaps in complex widgets) | 3-tier tokens (global/map/component) | enterprise/admin, 60+ components, tables | MIT | ant.design |
| **Chakra UI v3** | React | Panda CSS (zero-runtime) | high | DTCG-aligned core + semantic tokens | style-props DX, accessible defaults | MIT | chakra-ui.com |
| **Mantine** | React | CSS Modules (no runtime) | good | `createTheme()` + Styles API | batteries-included (hooks/forms/dates/charts) | MIT | mantine.dev |
| **Tailwind UI** | HTML/React/Vue | Tailwind | accessible (uses Headless UI) | Tailwind config | polished marketing + app templates | Commercial | tailwindui.com |
| **Material Design 3** | Android/Flutter/Web | token-driven | color roles ≥3:1 by default | Dynamic Color (HCT) + tokens | cross-platform Material feel | Apache 2.0 | m3.material.io |
| **Apple HIG** | Apple platforms | platform-native | first-class | semantic system colors + Dynamic Type | clarity/deference/depth reference | proprietary (free) | developer.apple.com/design |
| **IBM Carbon** | React/Angular/Vue/Svelte/WC | Sass / CSS vars | rigorous (WCAG AA + 508) | 4 themes, role tokens | enterprise data, strict a11y | Apache 2.0 | carbondesignsystem.com |
| **Atlassian DS** | React 18 + TS | CSS vars + Compiled | WCAG AA | semantic tokens; theme auto-switch | Forge/Jira/Confluence apps | Apache 2.0 (tokens) | atlassian.design |
| **Polaris (Shopify)** | React → Web Components | CSS vars | "build once" a11y (4.5:1/7:1) | role tokens, 12 palettes×16 shades | commerce/merchant tools | MIT | polaris.shopify.com |
| **Fluent UI** | React v9 / WC / Blazor | Griffel (zero-runtime CSS-in-JS) | high (MS-mandated) | tokens as CSS vars; DTCG-aligned | M365/Teams/Azure apps | MIT | react.fluentui.dev |
| **Base Web (Uber)** | React | Styletron (atomic) | good | `createTheme()` + deep `overrides` API | Uber-scale, deep per-part customization | MIT | baseweb.design |
| **Park UI (Ark UI)** | React/Solid/Vue/Svelte | Panda or Tailwind | full WAI-ARIA (Ark UI) | Panda preset | multi-framework one design system | MIT | park-ui.com |
| **daisyUI** | framework-agnostic | Tailwind plugin (semantic classes) | basic (add ARIA for complex) | 35+ themes via CSS vars (`data-theme`) | rapid prototyping, no-JS, non-React | MIT | daisyui.com |

**Companions:** Tremor (dashboards/KPI), TanStack Table (headless tables), Recharts (charts), Sonner (toasts), Vaul (drawers/sheets), cmdk (command palette), Every-Layout (CSS layout primitives), Framer Motion / Motion (React animation).

---

# PART B — W3C DESIGN TOKENS (DTCG)

First stable release Oct 2025. Tool-agnostic JSON interchange format.
```json
{
  "color": { "brand": { "$value": "#0969da", "$type": "color" } },
  "spacing": { "md": { "$value": "16px", "$type": "dimension" } },
  "color": { "action": { "primary": { "$value": "{color.brand}", "$type": "color" } } }
}
```
Types: color, dimension, fontFamily, fontWeight, duration, cubicBezier, number, strokeStyle, border, transition, shadow, gradient, typography. Aliases via `{group.token}`.

## 3-tier token architecture (recommended)
1. **Primitive** — `blue-500: #3b82f6`
2. **Semantic** — `color-action-primary: {blue-500}` (NEVER reference primitives directly in components)
3. **Component** — `button-bg: {color-action-primary}`
Theme = remap semantic layer (dark mode: `color-action-primary: {blue-300}`).

---

# PART C — HOW TO PICK

| Need | Pick |
|------|------|
| Full ownership + Tailwind | **shadcn/ui** |
| Accessible primitives, BYO styling | **Radix Primitives** / **Headless UI** |
| Multi-framework (React/Vue/Svelte/Solid) | **Park UI** (Ark UI) |
| Enterprise React, 50+ components fast | **MUI** / **Ant Design** / **Mantine** |
| Batteries-included (forms/dates/charts/hooks) | **Mantine** |
| Shopify / Microsoft / Atlassian / IBM ecosystem | **Polaris / Fluent / Atlassian DS / Carbon** |
| Android/Flutter native | **Material Design 3** |
| Apple native | **HIG** |
| Token-first, DTCG-aligned | **Chakra v3** |
| Rapid prototype, no JS deps | **daisyUI** |
| Deep per-part overrides at scale | **Base Web** |

**Anti-patterns:** picking MUI "because popular" (heavy bundle, Material look hard to shed) · picking shadcn when you need 50+ complex prebuilt components (you'll rebuild too much) · daisyUI for complex interactive widgets (no built-in a11y for modal/combobox) · mixing 3+ systems (token/CSS conflicts).

---

# PART D — MARKETING SECTION ANATOMIES (varied, anti-monotony)

> Storytelling sequence (Evil Martians): Hero → Trust → Features → Social proof → Supporting (FAQ/pricing/comparison) → Final CTA. Each section a DIFFERENT anatomy.

## Hero (4 variants — break the centered-dual-CTA mold)
| Variant | Structure |
|---------|-----------|
| Split | text left (left-aligned) + product screenshot right; one dominant CTA + text-link |
| Centered + screenshot below | centered headline + single CTA → full-width product shot below |
| Full-bleed | video/image bg + overlay text (≥AA contrast) |
| Editorial / text-only | large display headline, no image, code snippet or one text-link CTA |

## Features (5 variants)
| Variant | Structure |
|---------|-----------|
| Alternating | image-left/text-right, max 2 alternations then break; vary split ratios |
| Bento | mixed cell sizes, one 2×2 dominant |
| Tabbed | tab bar → swappable panel (Build/Deploy/Monitor) |
| Scroll-sticky | sticky visual one side + scrolling text panels |
| Icon list (non-uniform) | `<ul>` icon+text, varied card spans, no equal-weight grid |

## Social proof (4)
Logo wall (real logos only, ≤6–12, grayscale, no auto-scroll <12) · metrics bar (GitHub stars, usage, uptime) · case-study card (logo + quote + metric + named person) · inline quote next to the feature it validates.

## Testimonials (3)
Single big quote (early-stage, weight) · masonry grid (`columns-*` break-inside-avoid) · accessible carousel (pause/prev/next, `aria-live` slide count).

## Pricing (3)
Cards (2–3 tiers, ≤5 features each, recommended = inverted fill NOT ring) · monthly/annual toggle (`role="switch"`) · comparison table (`<table>` checkmarks, for "vs" pages).

## FAQ (2)
Accordion single-column (5–12 Qs, long answers — Radix/Headless Disclosure) · two-column grid (short answers, all visible).

## CTA (2)
Full-width contrast block ("Ready to start?" + specific button) · inline CTA with calendar embed (early-stage, high-touch).

## Footer (2)
Minimal (MVP, <16 pages: logo + 3–5 real links + copyright) · structured (16+ pages: link groups + legal + real social icons).

```html
<!-- Split hero (left-aligned, asymmetric, specific copy) -->
<section class="py-24 sm:py-32 px-6">
  <div class="mx-auto max-w-6xl grid gap-12 lg:grid-cols-2 lg:items-center">
    <div>
      <h1 class="text-4xl sm:text-5xl font-bold leading-tight tracking-tight text-gray-900">
        Identify at-risk students 6 weeks earlier
      </h1>
      <p class="mt-5 max-w-prose text-lg leading-relaxed text-gray-600">
        JagaSekolah scores attendance, grades, and economic data so wali kelas can intervene before a student drops out.
      </p>
      <a href="/demo" class="mt-8 inline-block rounded-md bg-blue-700 px-5 py-2.5 text-sm font-medium text-white hover:bg-blue-800 transition-colors">
        See demo with sample data
      </a>
    </div>
    <img src="/dashboard.png" alt="JagaSekolah risk-score dashboard" class="rounded-lg border border-gray-200 shadow-sm lg:justify-self-end" />
  </div>
</section>
```

Sources: official design-system docs; W3C DTCG (tr.designtokens.org); Evil Martians "We studied 100 dev tool landing pages" (2025); Tailwind UI marketing sections; Refactoring UI.
