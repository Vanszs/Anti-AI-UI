# Animation & Overlays — Tasteful Motion + Feedback Components

Companion to `../SKILL.md`. Overlay/feedback components + accessible motion. Sources: Radix, Headless UI, shadcn/ui, MUI, Ant Design, Material Design 3, Framer Motion / Motion, Emil Kowalski (Sonner, Vaul, animations.dev), Josh Comeau, NNG, web.dev, W3C WCAG/APG.

> Motion budget: animate ≤30% of elements · `transform`+`opacity` ONLY (GPU) · ease-out in / ease-in out · exits 25–35% faster · `prefers-reduced-motion` MANDATORY.

---

# PART A — MOTION FOUNDATIONS

## Duration & easing reference
| Context | Duration |
|---------|----------|
| Micro-feedback (toggle/check) | 100ms |
| Hover | 100–150ms |
| Tooltip/fade | 150–200ms |
| Dropdown/card/popover | 200–250ms |
| Modal/drawer enter | 250–300ms |
| Exits | 25–35% faster than enter |
| **Max (never exceed)** | 400ms |

| Easing | cubic-bezier | Use |
|--------|--------------|-----|
| Deceleration (ease-out) | `cubic-bezier(0,0,0.2,1)` | entering |
| Acceleration (ease-in) | `cubic-bezier(0.4,0,1,1)` | leaving |
| Standard (ease-in-out) | `cubic-bezier(0.4,0,0.2,1)` | on-screen move |
| Energetic (Emil) | `cubic-bezier(0.16,1,0.3,1)` | snappy reveals |

## prefers-reduced-motion (MANDATORY — WCAG 2.3.3)
```css
/* additive: only add motion when allowed */
@media (prefers-reduced-motion: no-preference){
  .reveal{opacity:0;transform:translateY(8px);transition:opacity 250ms ease-out,transform 250ms ease-out}
  .reveal.visible{opacity:1;transform:none}
}
/* subtractive safety net */
@media (prefers-reduced-motion: reduce){
  *,*::before,*::after{animation-duration:.01ms!important;transition-duration:.01ms!important;scroll-behavior:auto!important}
}
```
```tsx
// Framer Motion / Motion
import { useReducedMotion } from "motion/react";
const reduce = useReducedMotion();
<motion.div initial={reduce ? false : {opacity:0,y:8}} animate={{opacity:1,y:0}} transition={{duration:0.25,ease:[0,0,0.2,1]}} />
```

## Motion design tokens
```css
:root{
  --dur-fast:150ms; --dur-normal:250ms; --dur-slow:350ms;
  --ease-out:cubic-bezier(0,0,0.2,1); --ease-in:cubic-bezier(0.4,0,1,1);
  --ease-energetic:cubic-bezier(0.16,1,0.3,1);
}
/* never use transition-all */
button,a,.interactive{transition:color var(--dur-fast) var(--ease-out),background-color var(--dur-fast) var(--ease-out),box-shadow var(--dur-fast) var(--ease-out)}
```

---

# PART B — MOTION RECIPES (13)

| # | Pattern | When | Spec |
|---|---------|------|------|
| 1 | Entrance fade+slide | content first appears | opacity 0→1 + translateY 8px, 250ms ease-out |
| 2 | Staggered list | grids, nav, results | 30–50ms/child, ≤300ms total |
| 3 | Page transition | route/tab change | View Transitions API or `AnimatePresence` crossfade 200ms |
| 4 | Layout / shared element | reorder, card→modal | Motion `layout` / `layoutId` (transform-based) |
| 5 | Scroll reveal | long pages | animate **section containers**, fire once, `IntersectionObserver`/`whileInView once` |
| 6 | Hover micro | buttons, cards | shadow + ≤1px translate, 150ms (never `scale-105`) |
| 7 | Skeleton/shimmer | loading | gated `motion-safe`, `aria-busy`, 3:1 contrast |
| 8 | Accordion expand | FAQ, panels | height 0↔auto (`interpolate-size`/Radix var), 200ms |
| 9 | Modal/drawer | dialogs, sheets | scale 0.95→1 + fade enter 250ms, exit 180ms |
| 10 | Toast slide-in | feedback | y 14→0 + fade 200ms, `role="status"` |
| 11 | Subtle parallax | hero only | factor 0.1–0.2, never on text, reduced-motion off |
| 12 | Number count-up | KPIs entering view | 1–1.5s ease-out, static for reduced-motion |
| 13 | Accessible marquee | logo strips | duplicate `aria-hidden`, pause on hover/focus, >30s, reduced-motion off |

### Entrance + stagger (Motion)
```tsx
const container={hidden:{},show:{transition:{staggerChildren:0.04}}};
const item={hidden:{opacity:0,y:6},show:{opacity:1,y:0,transition:{duration:0.25,ease:[0,0,0.2,1]}}};
<motion.ul variants={container} initial="hidden" whileInView="show" viewport={{once:true,margin:"-100px"}}>
  {items.map(i=><motion.li key={i.id} variants={item}>{i.name}</motion.li>)}
</motion.ul>
```
### Hover micro (CSS, NOT scale-105)
```css
.card{transition:box-shadow 150ms ease-out,border-color 150ms ease-out}
.card:hover{box-shadow:0 8px 24px rgb(0 0 0 /0.08);border-color:var(--color-primary-200)}
```

---

# PART C — OVERLAY & FEEDBACK COMPONENTS

| Component | Modal? | Focus trap | Auto-dismiss | When |
|-----------|--------|-----------|--------------|------|
| Dialog/Modal | yes | yes | no | forms, decisions requiring focus |
| Alert Dialog | yes | yes | no | destructive confirms (no outside-click dismiss) |
| Sheet/Drawer | yes | yes | no | side panels, filters, mobile nav |
| Bottom sheet | yes | yes | swipe | mobile actions/selections |
| Toast/Snackbar | no | no | yes (≥5s) | brief success/info |
| Inline alert/banner | no | no | no | persistent contextual info |
| Popover | no | contained | no | anchored interactive content |
| Hover card | no | no | auto | preview on hover+focus (non-essential) |
| Tooltip | no | no | auto | label icon-only/abbr (no interactive content) |
| Loading overlay | blocks | — | on done | async blocking op |
| Command palette | yes | yes | no | ⌘K power-user search/actions |

### Modal — native `<dialog>` (free focus-trap + inert + Esc)
```html
<dialog id="dlg" aria-labelledby="dlg-title"
  class="rounded-xl bg-white p-6 shadow-xl backdrop:bg-black/40 max-w-md w-full">
  <h2 id="dlg-title" class="text-lg font-semibold">Confirm</h2>
  <p class="mt-2 text-sm text-gray-600">Are you sure?</p>
  <form method="dialog" class="mt-6 flex justify-end gap-3">
    <button class="rounded-md px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">Cancel</button>
    <button class="rounded-md bg-blue-700 px-4 py-2 text-sm text-white hover:bg-blue-800">Confirm</button>
  </form>
</dialog>
<!-- open: dlg.showModal(); css-tricks 2025: no manual focus-trap needed -->
<style>
  @media (prefers-reduced-motion:no-preference){ dialog[open]{animation:fade-in 200ms ease-out} }
  @keyframes fade-in{from{opacity:0;transform:translateY(8px)}}
</style>
```
For React: prefer **Radix Dialog** / **Headless UI Dialog** (auto trap, Esc, `aria-labelledby` via Title). Alert dialog = **Radix AlertDialog** (cannot dismiss by backdrop). Sheets/bottom-sheets = **Vaul** (Emil Kowalski). Toasts = **Sonner**. Command palette = **cmdk**.

### Toast (Sonner)
```tsx
import { Toaster, toast } from 'sonner';
<Toaster position="bottom-right" richColors closeButton />  // once at root
toast.success('Profile saved');
toast('Event created', { description:'Mon, 6PM', action:{label:'Undo', onClick:undo} });
```
Sonner auto-wires `aria-live`, stacking, swipe-dismiss, reduced-motion.

### Tooltip (Radix)
```tsx
<Tooltip.Provider delayDuration={400}>
  <Tooltip.Root>
    <Tooltip.Trigger asChild>
      <button aria-label="Save"><svg aria-hidden="true">…</svg></button>
    </Tooltip.Trigger>
    <Tooltip.Portal>
      <Tooltip.Content sideOffset={6} class="rounded bg-gray-900 px-2.5 py-1.5 text-xs text-white">
        Save (⌘S)<Tooltip.Arrow class="fill-gray-900"/>
      </Tooltip.Content>
    </Tooltip.Portal>
  </Tooltip.Root>
</Tooltip.Provider>
```
No interactive content inside tooltips — use a Popover for that.

### Loading overlay (accessible)
```html
<div role="status" aria-live="polite" aria-busy="true"
  class="absolute inset-0 z-10 flex items-center justify-center bg-white/70">
  <svg aria-hidden="true" class="h-6 w-6 animate-spin text-blue-600 motion-reduce:animate-none">…</svg>
  <span class="ml-2 text-sm text-gray-600">Loading…</span>
</div>
```

---

## OVERLAY A11Y RULES (all)
1. Native `<dialog>.showModal()` = free focus-trap + inert background + Esc + backdrop.
2. `aria-labelledby` → visible title always.
3. Focus returns to trigger on close.
4. Esc closes (except AlertDialog → explicit action).
5. Non-modal overlays (toast/popover/tooltip) do NOT trap focus.
6. Dynamic content (toast/alert) lives in an `aria-live` region.
7. Never `<div onClick>` for close/action — always `<button>`.
8. All enter/exit animations gated by `prefers-reduced-motion`.

## GOLDEN RULES (motion)
- Animate ≤30% of visible elements; if everything moves, nothing is emphasized.
- `transform`+`opacity` only (never width/height/top/left/filter).
- ease-out in, ease-in out; exits faster.
- Hovers 100–150ms, entrances 200–300ms, never >400ms.
- 0–1 ambient infinite animation per page; must pause on hover + respect reduced-motion.
- Every animation must answer "what user task does this support?" — else remove it.

Sources: Radix UI, Headless UI, shadcn/ui, MUI, Ant Design, Material Design 3, motion.dev, Emil Kowalski (Sonner/Vaul/animations.dev), Josh Comeau, NNG, web.dev, Adrian Roselli (skeletons), W3C WCAG 2.2.2/2.3.3 + WAI-ARIA APG.
