# Anti-AI-Slop Code Patterns — React/TSX/Next.js

Companion to `slop-uiux-patterns.md` (visual tells). This file catalogs
**code-level** tells that reveal LLM-generated frontend code.

Format: **TELL → WHY-SLOP → FIX → SEVERITY**.

Severity: **P0** = security/a11y failure or instant "AI wrote this" signal ·
**HIGH** = strong code-review tell · **MEDIUM** = trained eyes notice ·
**LOW** = subtle, cumulative.

Sources: react-japan.dev (GAIA/Claude anti-patterns), Frontend Masters (AI a11y),
vibecoder.me (AI memory leaks), freeCodeCamp (refactoring case study),
react.dev docs, Kent C. Dodds, TkDodo, Josh Comeau, Theo/t3, ESLint/biome
discourse, WCAG/WebAIM, Next.js docs, HN, Reddit r/reactjs, dev.to,
arXiv:2604.06373 (CMU design issues in AI IDE-generated projects),
arXiv:2511.04427 (CMU speed vs quality study).

---

## 0. ROOT CAUSE

LLMs generate code by predicting the most probable token sequence from training
data dominated by: tutorials (happy-path only), StackOverflow answers (isolated
snippets), abandoned repos (unchanged defaults), and beginner blog posts.

**Result:** code that compiles and renders once but lacks cleanup, error handling,
decomposition, type safety, accessibility, and production hardening. The model
optimizes for "runs correctly once" not "maintains correctly over time."

CMU arXiv:2604.06373 found 4,498 design issues across 10 Cursor-generated projects
(avg 16,965 LoC). Most prevalent: Code Duplication (28.4%), Complex Methods (27.9%),
Framework Best-Practice Violations (35.3%), Exception-Handling Issues (10.4%),
Accessibility Issues (6.1%). All violate SRP, SoC, DRY.

---

## 1. STATE MANAGEMENT & HOOKS

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| S1 | Derived state stored in `useState` + synced via `useEffect` | #1 documented AI anti-pattern (react-japan.dev, react.dev). Extra render cycle, infinite-loop risk, obscures intent | Compute during render: `const item = items.find(i => i.id === selectedId)` | **HIGH** |
| S2 | 5+ `useState` declarations in one component ("state soup") | AI generates one state per data field; makes re-render tracking impossible | Group into single object, or extract to custom hook, or use reducer, or data-fetching lib (TanStack Query) | **HIGH** |
| S3 | `useCallback` on every handler with `[]` deps | AI cargo-cults memoization; empty deps freeze stale closures; wrapper allocates dep array every render for nothing | Plain inline function unless: (a) passed to `memo`-wrapped child, (b) used as hook dependency, (c) passed to child's hook dep array. That's it. | **HIGH** |
| S4 | `useMemo` on trivial computations (`useMemo(() => a + b, [a, b])`) | "Performance optimization ALWAYS comes with a cost but does NOT always come with a benefit" (Kent C. Dodds). AI wraps everything defensively | Remove unless computation is genuinely expensive (sort/filter 1000+ items) or stabilizing reference for memo'd child | **MEDIUM** |
| S5 | Neither `useMemo` nor `useCallback` anywhere (opposite extreme) | AI in "minimal" mode; expensive list transforms recalculate on every keystroke | Profile first; memoize only measured bottlenecks | **LOW** |
| S6 | `useState` for data that never changes after init | AI pattern: `const [config] = useState(initialConfig)`. Wastes a state slot | `useRef` or module-level constant | **LOW** |

### Slop snippet (S1):
```tsx
// SLOP — derived state via effect
const [fullName, setFullName] = useState('');
useEffect(() => {
  setFullName(`${firstName} ${lastName}`);
}, [firstName, lastName]);

// FIX — compute during render
const fullName = `${firstName} ${lastName}`;
```

---

## 2. DATA FETCHING

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| D1 | `fetch` inside `useEffect` in Next.js App Router | Ignores server components entirely; ships unnecessary JS; kills SEO; forces loading spinners; no cache/retry/dedup | Async Server Component with `fetch()` at top level; or TanStack Query for client-interactive data | **P0** (Next.js) |
| D2 | `useEffect` fetch with no `AbortController` / cancelled flag | AI "optimizes for runs correctly once, not cleanup over time." Memory leak + race condition on fast navigation | Return cleanup: `const ctrl = new AbortController(); ... return () => ctrl.abort();` | **HIGH** |
| D3 | No error state, no loading state, no empty state (happy-path only) | AI skips multi-state thinking; functionally incomplete for production | Handle all 4 states: loading, error, empty, success. Or use Suspense + ErrorBoundary | **HIGH** |
| D4 | Fetch-in-useEffect re-fetches on every mount (no cache) | User navigates away and back → full reload; no stale-while-revalidate | TanStack Query / SWR / Next.js `fetch` with `cache: 'force-cache'` | **MEDIUM** |
| D5 | `'use client'` on every file / at page level | Defeats RSC model; ships entire React runtime for static content; "all pages client-side" | `'use client'` only on leaf components needing hooks/browser APIs; keep pages/layouts as Server Components | **HIGH** |
| D6 | `async function` inside `useEffect` without IIFE or extracted fn | `useEffect` callback cannot be async (returns cleanup, not Promise). AI writes `useEffect(async () => {...})` | Extract: `const fetchData = async () => {...}; fetchData();` inside the effect, or use data lib | **MEDIUM** |

### Slop snippet (D1 + D2):
```tsx
// SLOP — client-side fetch in Next.js, no cleanup
'use client';
export default function UsersPage() {
  const [users, setUsers] = useState([]);
  useEffect(() => {
    fetch('/api/users').then(r => r.json()).then(setUsers);
  }, []);
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}

// FIX — Server Component (zero client JS)
export default async function UsersPage() {
  const users = await prisma.user.findMany();
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

---

## 3. MEMORY LEAKS (AI-specific)

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| ML1 | `useEffect` with subscription, no cleanup return | AI never returns unsubscribe. Every mount adds another subscription | `return () => subscription.unsubscribe()` | **HIGH** |
| ML2 | `addEventListener` on `window`/`document`, no `removeEventListener` | Listener persists after unmount; stale closure updates dead state | Cleanup: `return () => window.removeEventListener(...)` | **HIGH** |
| ML3 | `setInterval`/`setTimeout` without `clearInterval`/`clearTimeout` | Timer fires after unmount; "can't perform state update on unmounted component" | `return () => clearInterval(id)` | **HIGH** |
| ML4 | Fetch without cancelled flag or AbortController | setState on unmounted component; race conditions on fast nav | `let cancelled = false; ... return () => { cancelled = true; };` or AbortController | **HIGH** |
| ML5 | Closures capturing large objects passed to external libs | Old closures pile up in registry, each holding prev data array | Use ref for large data; deregister old handler in cleanup | **MEDIUM** |

---

## 4. COMPONENT ARCHITECTURE

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| A1 | God Component (500+ lines, manages state + fetches + renders everything) | AI generates in one shot; violates SRP; can't reuse, can't test, can't scale. CMU found this as top structural issue | Decompose: presentational components + custom hooks + container | **HIGH** |
| A2 | Prop drilling 4+ levels deep | AI passes state top-down through every intermediate; intermediates don't use the prop | Composition (children), Context for truly global, or component inversion | **HIGH** |
| A3 | `index` as key in dynamic lists | AI uses `key={index}` by default; breaks reconciliation on reorder/insert/delete; subtle bugs | Use stable unique ID (`key={item.id}`) | **HIGH** |
| A4 | Missing keys entirely | React warns; AI sometimes omits; list won't reconcile correctly | Always provide `key` with stable identity | **HIGH** |
| A5 | Everything in one file, no separation | AI produces single-file solutions; no feature/concern separation | Feature-based folders: `components/`, `hooks/`, `types/`, `lib/` | **MEDIUM** |
| A6 | Inline arrow handlers recreated each render in hot paths | `<Item onClick={() => handleClick(id)} />` inside `.map()` of 1000 items forces re-render of all children | Extract to named component with memo, or useCallback with stable deps | **MEDIUM** |
| A7 | No composition — everything via props, never `children` | AI over-configures; Button takes 15 props instead of `children` | Prefer `children` for content; compound components for complex patterns | **MEDIUM** |

---

## 5. TYPESCRIPT QUALITY

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| TS1 | `any` type everywhere / `as any` casts | AI escapes type errors instead of solving them; defeats TS purpose | Proper typing; `unknown` + type guards for truly dynamic data | **HIGH** |
| TS2 | `// @ts-ignore` / `@ts-expect-error` without explanation | Suppresses errors the AI couldn't resolve; hides real bugs | Fix the type error; if truly unavoidable, use `@ts-expect-error` with comment explaining why | **HIGH** |
| TS3 | No types on props (plain JS in .tsx files) | AI defaults to JS unless prompted; no autocomplete, no safety | `interface Props { ... }` on every component | **HIGH** |
| TS4 | Overly permissive types (`string` for enums, `object` for complex shapes) | AI picks broadest type that compiles | Narrow: union literals, discriminated unions, branded types | **MEDIUM** |
| TS5 | Type assertions (`as Type`) instead of type guards | AI bypasses validation; runtime crash when shape doesn't match | Runtime validation (Zod/valibot) at boundaries; type guards internally | **MEDIUM** |
| TS6 | `!` non-null assertion sprinkled everywhere | AI suppresses "possibly null" instead of handling null | Optional chaining + nullish coalescing + early returns | **MEDIUM** |

---

## 6. HTML SEMANTICS & ACCESSIBILITY

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| A11Y1 | `<div onClick>` instead of `<button>` | No keyboard access, no role, no focus, no activation via Enter/Space. W3C F59 failure. 73% of AI React apps fail WCAG AA on first generation (Frontend Masters). In CMU study, flagged as "Accessibility Issue" category (6.1% of all SonarQube issues) | `<button type="button" onClick={...}>` | **P0** |
| A11Y2 | `<div>` with `cursor-pointer` for links | Not focusable, not announced as link, can't open in new tab | `<a href="...">` or `<Link>` | **P0** |
| A11Y3 | No landmarks (`<nav>`, `<main>`, `<aside>`, `<header>`, `<footer>`) — pure div soup | Screen readers can't jump to sections; NVDA users must listen to every element. WebAIM 2026: 94.8% of top 1M sites have WCAG failures | Use semantic HTML elements for page regions | **P0** |
| A11Y4 | Icon-only buttons with no accessible name | `role: generic`, empty name in a11y tree; invisible to AT. Frontend Masters: "icons shipped without text alternatives more often than not" | `<button aria-label="Close"><svg aria-hidden="true">...</svg></button>` | **P0** |
| A11Y5 | Modal as positioned `<div>` (no dialog role, no focus trap, no Escape) | Keyboard users can't interact; focus leaks behind overlay | `<dialog>` with `showModal()`, or Radix/Headless UI Dialog | **P0** |
| A11Y6 | Custom select/dropdown: div soup with no `listbox`/`option` roles | No keyboard nav, no aria-expanded, no aria-selected | Use Headless UI Listbox / Radix Select / React Aria | **P0** |
| A11Y7 | Missing `<label>` on form inputs | Screen readers announce nothing for the input. WebAIM: "missing form input labels" top-5 most common failure | `<label htmlFor="x">` or `aria-label` / `aria-labelledby` | **HIGH** |
| A11Y8 | No `aria-expanded` / `aria-controls` on disclosure toggles | State change is visual-only (chevron rotates but AT sees nothing) | Add `aria-expanded={isOpen}` + `aria-controls="panel-id"` | **HIGH** |
| A11Y9 | Color as only indicator (error = red border, no text) | Colorblind users miss the signal entirely | Error text + icon + `aria-describedby` linking input to error msg | **HIGH** |
| A11Y10 | Images/SVG with no `alt` / no `aria-hidden` | Decorative icons announced as "image" with no name; meaningful images unidentified. WebAIM 2026: 18.5% of homepage images lack alt | Decorative: `aria-hidden="true"`. Meaningful: `alt="description"` | **HIGH** |
| A11Y11 | Heading hierarchy skipped (`<h1>` → `<h3>`, no `<h2>`) | Screen reader heading navigation breaks; broken document outline | Sequential heading levels; one `<h1>` per page | **MEDIUM** |
| A11Y12 | Lists rendered as sibling `<div>`s instead of `<ul>/<li>` | No "list, 5 items" context for screen readers | Use `<ul>` + `<li>` for any repeated group | **MEDIUM** |
| A11Y13 | Tabular data in `<div>` grid instead of `<table>` | No row/column association for AT | `<table>` + `<thead>` + `<th scope="col">` for data tables | **MEDIUM** |
| A11Y14 | No `prefers-reduced-motion` in CSS/Tailwind animations | Physical harm to ~5% of users (vestibular disorders) | Gate all spatial animations with `motion-safe:` or `@media (prefers-reduced-motion: no-preference)` | **P0** |
| A11Y15 | No visible focus styles (or `outline-none` without replacement) | Keyboard users can't see where they are. UGA: "Without a visible focus indicator, keyboard users are lost" | `focus-visible:ring-2 focus-visible:ring-blue-500` on all interactives | **HIGH** |
| A11Y16 | Missing `lang` attribute on `<html>` | Screen readers mispronounce all text; WCAG 3.1.1 Level A failure. 94.8% of top 1M sites have detectable failures (WebAIM 2026) | `<html lang="id">` (or appropriate BCP-47 code) | **P0** |
| A11Y17 | Placeholder text as only label | Disappears on input; fails WCAG 1.3.1 + 3.3.2; low contrast; AT may not announce | Always pair with visible `<label>`; placeholder is supplementary hint only | **HIGH** |
| A11Y18 | No focus management on modal open / SPA route change | Focus stays behind overlay or on previous page; AT user lost | Move focus to modal heading on open; restore on close. On route change: focus `<main>` or `<h1>` | **HIGH** |
| A11Y19 | No `aria-live` for dynamic content (toast, form error, status) | Screen reader never announces content added after page load | `<div role="status" aria-live="polite">` for non-urgent; `aria-live="assertive"` for critical | **HIGH** |
| A11Y20 | `tabindex` > 0 (positive tabindex) | Overrides natural DOM order; creates unpredictable navigation; maintenance nightmare | `tabindex="0"` to add to flow; `tabindex="-1"` for programmatic focus; never positive values | **HIGH** |
| A11Y21 | `autofocus` on non-first interactive / in non-modal context | Scrolls page unexpectedly; disorienting for SR users; skips preceding content | Only use in modals (focus first interactive); avoid on page-level forms | **MEDIUM** |
| A11Y22 | `onMouseOver`/`onMouseOut` without `onFocus`/`onBlur` equivalents | Keyboard/touch users never trigger the interaction. SonarQube flags this as accessibility issue | Always pair mouse events with keyboard equivalents | **HIGH** |
| A11Y23 | Missing `fieldset`/`legend` for related form groups (radio buttons, address fields) | AT can't convey group relationship; user doesn't know which options belong together | `<fieldset><legend>Payment method</legend>...radios...</fieldset>` | **MEDIUM** |

### Slop snippet (A11Y1 + A11Y4):
```tsx
// SLOP — div buttons, no a11y
<div className="cursor-pointer rounded-full p-2 hover:bg-gray-100"
     onClick={onClose}>
  <svg className="h-5 w-5" viewBox="0 0 24 24">
    <path d="M6 18L18 6M6 6l12 12" />
  </svg>
</div>

// FIX
<button type="button" aria-label="Close dialog"
        className="rounded-full p-2 hover:bg-gray-100
                   focus-visible:ring-2 focus-visible:ring-blue-500"
        onClick={onClose}>
  <svg aria-hidden="true" className="h-5 w-5" viewBox="0 0 24 24">
    <path d="M6 18L18 6M6 6l12 12" />
  </svg>
</button>
```

### Slop snippet (A11Y5 — invisible modal):
```tsx
// SLOP — div modal, no semantics
{isOpen && (
  <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/50">
    <div className="rounded-xl bg-white p-6 shadow-xl">
      <div className="text-lg font-bold">Confirm</div>
      <div className="mt-4 flex gap-2">
        <div className="cursor-pointer rounded bg-red-500 px-4 py-2 text-white" onClick={onConfirm}>Yes</div>
        <div className="cursor-pointer rounded bg-gray-300 px-4 py-2" onClick={onClose}>No</div>
      </div>
    </div>
  </div>
)}

// FIX — native dialog
<dialog ref={dialogRef} aria-labelledby="dialog-title"
        className="rounded-xl bg-white p-6 shadow-xl backdrop:bg-black/50"
        onClose={onClose}>
  <h2 id="dialog-title" className="text-lg font-bold">Confirm</h2>
  <p className="mt-2">Are you sure?</p>
  <div className="mt-4 flex gap-2">
    <button type="button" onClick={onConfirm}
            className="rounded bg-red-500 px-4 py-2 text-white hover:bg-red-600
                       focus-visible:ring-2 focus-visible:ring-red-400">Yes</button>
    <button type="button" onClick={onClose}
            className="rounded bg-gray-300 px-4 py-2 hover:bg-gray-400
                       focus-visible:ring-2 focus-visible:ring-gray-500">No</button>
  </div>
</dialog>
```

---

## 7. CSS / TAILWIND MARKUP QUALITY

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| TW1 | Conflicting utilities (`p-4 p-6`, `text-sm text-lg`) in same className | AI appends without checking existing; last one doesn't reliably win | Use `cn()` (clsx + tailwind-merge) for conditional overrides; never duplicate utility categories | **HIGH** |
| TW2 | Dynamic class construction (`text-${color}-500`) | Tailwind can't detect at build time; class won't be in output CSS | Use complete class names in conditional: `color === 'red' ? 'text-red-500' : 'text-blue-500'` | **HIGH** |
| TW3 | 20+ utilities in single className string, no extraction | Unreadable; impossible to maintain; AI dumps everything inline | Extract to `@apply` in CSS module for truly repeated patterns, or component abstraction | **MEDIUM** |
| TW4 | Inconsistent class ordering (no prettier-plugin-tailwindcss) | AI doesn't follow canonical order; makes diff noise; harder to scan | Install `prettier-plugin-tailwindcss`; enforce in CI | **MEDIUM** |
| TW5 | Hardcoded hex/rgb in className (`bg-[#6366f1]`) when design token exists | AI invents values; no system; can't theme; can't maintain | Use Tailwind config theme values: `bg-primary`, `text-brand-500` | **HIGH** |
| TW6 | `style={{ }}` mixed with Tailwind for no reason | AI falls back to inline styles for things Tailwind handles fine | Pure Tailwind; inline style only for truly dynamic computed values (e.g., `style={{ width: `${percent}%` }}`) | **MEDIUM** |
| TW7 | `!important` via `!` prefix sprinkled everywhere | AI resolves specificity battles with nuclear option | Fix the specificity; restructure component; use `cn()` for merging | **HIGH** |
| TW8 | No responsive design (`sm:` / `md:` / `lg:` absent) | AI generates desktop-only; breaks on mobile | Mobile-first: base = mobile, `sm:`/`md:`/`lg:` for larger | **HIGH** |

### Slop snippet (TW2):
```tsx
// SLOP — dynamic class, won't build
<div className={`bg-${status}-100 text-${status}-800`}>

// FIX — complete literals
const statusStyles = {
  success: 'bg-green-100 text-green-800',
  error: 'bg-red-100 text-red-800',
  warning: 'bg-yellow-100 text-yellow-800',
} as const;
<div className={statusStyles[status]}>
```

---

## 8. SECURITY & HYGIENE

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| SEC1 | `dangerouslySetInnerHTML` without sanitization | AI uses it to render formatted content; direct XSS vector | Sanitize with DOMPurify: `dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(html) }}`. Or better: avoid entirely with structured rendering | **P0** |
| SEC2 | `console.log` / `console.error` left in production code | AI debugging artifacts ship; leaks internal state to devtools; noise | Strip via ESLint `no-console` rule (error in prod config); use proper logger | **HIGH** |
| SEC3 | API keys / secrets in client-side code | AI puts env vars in `'use client'` components; exposed in bundle | Server-only: `process.env` in Server Components / Route Handlers only. Client needs `NEXT_PUBLIC_` prefix (and must be safe to expose) | **P0** |
| SEC4 | No input validation/sanitization on forms | AI renders inputs, posts data; no Zod/yup schema; accepts anything | Validate client + server; Zod schema at API boundary | **HIGH** |
| SEC5 | Hardcoded strings not extracted for i18n | Every UI string inline; can't translate; can't reuse | Extract to constants file or i18n lib (next-intl, react-i18next) | **MEDIUM** |
| SEC6 | Magic numbers / unlabeled constants | `if (status === 3)`, `padding: 47`, `setTimeout(_, 1500)` | Named constants: `const STATUS_ACTIVE = 3; const DEBOUNCE_MS = 1500;` | **MEDIUM** |

---

## 9. PERFORMANCE & NEXT.JS PATTERNS

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| P1 | `'use client'` at page/layout level | Entire subtree becomes client; no streaming, no RSC benefits | Push `'use client'` to smallest leaf components that need interactivity | **HIGH** |
| P2 | Importing heavy libs in client components (date-fns, lodash full) | AI imports everything; bloats bundle | Tree-shake: `import { format } from 'date-fns'`; prefer native APIs where possible | **MEDIUM** |
| P3 | No `loading.tsx` / `error.tsx` / `not-found.tsx` in route segments | AI creates `page.tsx` only; no Suspense boundaries; white screen on error | Add Next.js route-segment files for all states | **HIGH** |
| P4 | Images with `<img>` instead of `next/image` | No lazy loading, no responsive sizing, no format optimization | `import Image from 'next/image'`; set `width`/`height` or `fill` | **MEDIUM** |
| P5 | Re-renders entire page on single input change | AI puts form state at page level; every keystroke re-renders all children | Isolate form state in dedicated client component; keep parent as Server Component | **HIGH** |
| P6 | No `Suspense` boundaries for async children | Entire page blocks until slowest data resolves | Wrap independent data sections in `<Suspense fallback={<Skeleton />}>` | **MEDIUM** |
| P7 | Waterfalls: sequential awaits that could be parallel | `const a = await getA(); const b = await getB();` — needlessly serial | `const [a, b] = await Promise.all([getA(), getB()]);` | **MEDIUM** |
| P8 | `useEffect(() => { router.push(...) }, [])` for redirect | Client-side redirect; flash of wrong content; SEO miss | Use `redirect()` in Server Component or middleware | **HIGH** |

---

## 10. TESTING & DX TELLS

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| T1 | Zero tests generated | AI produces features, never tests; untested = unverified | Test alongside: unit (Vitest) + component (Testing Library) + e2e (Playwright) | **HIGH** |
| T2 | Tests query by `data-testid` instead of role | Tests pass but prove nothing about accessibility; brittle | `getByRole('button', { name: /submit/i })` — if query fails, AT can't find it either | **HIGH** |
| T3 | Tests only cover happy path | AI skips error/empty/loading/edge cases | Test: error state, empty list, boundary values, loading skeleton | **MEDIUM** |
| T4 | Snapshot tests of entire components | AI's go-to; breaks on any change; proves nothing; noise | Behavioral assertions: "when clicked, shows X"; reserve snapshots for small stable markup | **MEDIUM** |
| T5 | `// TODO: add tests` comments | AI scaffolds intent but never delivers | If you write the TODO, write the test | **LOW** |

---

## 11. PROJECT STRUCTURE TELLS

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| PS1 | Everything in `src/components/` flat (50+ files, no hierarchy) | AI generates one component per prompt; no feature grouping | Feature-based: `features/auth/`, `features/dashboard/` with colocated components/hooks/types | **MEDIUM** |
| PS2 | Mixed concerns in single files (API call + UI + business logic) | AI optimizes for single-file solution that "works" | Separate: `lib/` (business), `hooks/` (data), `components/` (UI) | **HIGH** |
| PS3 | No barrel exports, or barrel-exporting everything | AI either imports every file individually or re-exports the universe | Intentional public API per feature; avoid barrel files in App Router (tree-shaking issues) | **LOW** |
| PS4 | Utility functions duplicated across components | AI generates inline helpers per component; no shared lib. CMU: Code Duplication = 28.4% of all issues | Extract to `lib/utils.ts`; DRY shared logic | **MEDIUM** |
| PS5 | No error boundary anywhere in tree | Entire app crashes on single component error | `error.tsx` at route level + custom ErrorBoundary for client subtrees | **HIGH** |

---

## 12. FORM HANDLING

| # | Tell | Why-slop | Fix | Severity |
|---|------|----------|-----|----------|
| FM1 | Controlled inputs with `useState` per field (no form lib) | AI writes `const [email, setEmail] = useState('')` ×10; performance + boilerplate hell | React Hook Form or Conform (Next.js server actions) | **MEDIUM** |
| FM2 | No validation states (error messages, success feedback) | AI renders inputs + submit; no error display; user sees nothing on invalid input | Wire validation → per-field error display + `aria-describedby` + `aria-invalid` | **HIGH** |
| FM3 | Submit handler with no loading/disabled state | Double-submit; no feedback; AI skips UX states | `isSubmitting` state → disable button + show spinner | **HIGH** |
| FM4 | Form with no `<form>` element (just divs + button) | No native validation, no Enter-to-submit, no form semantics | Wrap in `<form onSubmit={handleSubmit}>` with `<button type="submit">` | **HIGH** |
| FM5 | Client-only form in Next.js (should be Server Action) | Ships validation JS to client; duplicates server-side check | `'use server'` action + progressive enhancement + Conform/useFormStatus | **MEDIUM** |

---

## 13. META-TELLS (GESTALT INDICATORS)

These aren't single-line bugs but structural patterns that together scream
"an LLM generated this entire codebase":

1. **Happy-path-only architecture** — works with good data, explodes on null/error/empty/edge
2. **Flat component tree** — one massive page, no composition, no reuse
3. **Copy-paste across components** — same fetch logic, same error handling, duplicated instead of abstracted
4. **No custom hooks** — everything inline; hooks are AI's weakest abstraction
5. **Framework-ignorant** — uses Next.js but writes CRA-era patterns (client fetch, no RSC, no route-segment files)
6. **Zero a11y** — entire app navigable only by mouse; keyboard/screen reader = broken
7. **No loading/error/empty states** — component either shows data or crashes
8. **Console artifacts** — `console.log`, `// TODO`, commented-out code blocks

If 4+ of these appear together → the codebase is vibe-coded without human review.

---

## 14. ENFORCEMENT (STOP THE SLOP)

### ESLint / Biome rules (minimal set that catches the worst)

```json
{
  "rules": {
    "no-console": "error",
    "react-hooks/exhaustive-deps": "warn",
    "jsx-a11y/click-events-have-key-events": "error",
    "jsx-a11y/no-static-element-interactions": "error",
    "jsx-a11y/anchor-is-valid": "error",
    "jsx-a11y/alt-text": "error",
    "jsx-a11y/label-has-associated-control": "error",
    "jsx-a11y/interactive-supports-focus": "error",
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/no-non-null-assertion": "warn"
  }
}
```

### Pre-commit checklist (run every time)

**State & Hooks**
- [ ] No derived state in useState+useEffect; compute during render
- [ ] useCallback/useMemo justified (passed to memo child or hook dep)
- [ ] Every useEffect has cleanup if it subscribes/listens/timers/fetches

**Architecture**
- [ ] No God Components (>200 lines → split)
- [ ] No index-as-key in dynamic lists
- [ ] `'use client'` only on leaf components, never page/layout level

**Accessibility**
- [ ] Zero `<div onClick>` — all interactive elements are `<button>` or `<a>`
- [ ] All inputs have `<label>` or `aria-label`
- [ ] All icon-only buttons have `aria-label`; icons have `aria-hidden`
- [ ] Focus styles visible; keyboard navigation works
- [ ] `prefers-reduced-motion` respected
- [ ] `<html lang="...">` present
- [ ] Modals use `<dialog>` or headless lib with focus trap
- [ ] Dynamic content has `aria-live` region
- [ ] No positive `tabindex`; no `autofocus` outside modals

**Security & Hygiene**
- [ ] No `dangerouslySetInnerHTML` without DOMPurify (or none at all)
- [ ] No `console.log` in committed code
- [ ] No secrets in client code
- [ ] No `any` / `@ts-ignore` without documented justification

**Next.js Specific**
- [ ] Data fetching in Server Components (not useEffect+fetch)
- [ ] `loading.tsx` + `error.tsx` + `not-found.tsx` in route segments
- [ ] Images use `next/image`
- [ ] No client-side `router.push` for redirects (use `redirect()` or middleware)

---

## 15. SOURCES

- **arXiv:2604.06373** — Kashif et al., "Design Issues in AI IDE-Generated Large-Scale Projects" (2026)
- **arXiv:2511.04427** — He et al., "Speed at the Cost of Quality: How Cursor AI Increases Short-Term Velocity and Long-Term Complexity" (CMU 2025)
- **react-japan.dev** — "How I keep Claude from writing React anti-patterns" (Steven Sacks, GAIA, 2026)
- **react.dev** — "You Might Not Need an Effect" (official React docs)
- **Frontend Masters** — "AI-Generated UI Is Inaccessible by Default" (Durgesh Pawar, 2026)
- **vibecoder.me** — "Debugging Memory Leaks in AI-Generated React Code" (2026)
- **freeCodeCamp** — "Stop Trusting AI Code Blindly: A React Code Refactoring Case Study" (Tapas Adhikary, 2026)
- **htmlgenie.net** — "Your Div Soup Is Still Ruining the Web" (Sarah Chen, 2026)
- **W3C WCAG F59** — Failure of SC 4.1.2: div/span as UI control without role
- **WebAIM Million 2026** — 94.8% of homepages have WCAG failures; 83.9% low contrast; 18.5% images lack alt
- **TkDodo** — "The Useless useCallback" (Dominik Dorfmeister)
- **Kent C. Dodds** — "Before You memo()" / "useMemo and useCallback"
- **testparty.ai** — "When AI Writes Your Code: Accessibility Risks"
- **Tailwind Labs** — prettier-plugin-tailwindcss / class ordering docs
- **Next.js docs** — Composition Patterns, Rendering, Data Fetching
- **eslint-plugin-jsx-a11y** — accessibility linting rules
- **UGA DASH** — Accessibility Techniques: Visible Focus, Tab Order, ARIA, Popups
- **Polypane** — "The WebAIM Million 2025: Solving the most common issues"
- **Smashing Magazine** — "Building Self-Correcting Color Systems" (contrast stats, 2026)
