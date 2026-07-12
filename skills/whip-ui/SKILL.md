---
name: whip-ui
description: Use when building or redesigning frontend interfaces - landing pages, portfolios, web apps - to ensure non-templated, production-grade UI quality with anti-slop design standards
---

# whip-ui: Anti-Slop Frontend Design

> Landing pages, portfolios, and redesigns. Not dashboards, not data tables, not multi-step product UI.
> Every rule below is **contextual**. First read the brief, then pull only what fits.

## When to Use

- Building a landing page, portfolio, or marketing site
- Redesigning an existing website or web app UI
- User asks for "modern", "premium", "clean", or specific aesthetic direction
- Frontend output looks generic, templated, or "AI-generated"
- Choosing a design system or component library

## When NOT to Use

- Dashboards / dense product UI / admin panels (use Fluent, Carbon, Polaris)
- Data tables (use TanStack Table or AG Grid)
- Multi-step forms / wizards
- Code editors (use Monaco / CodeMirror)
- Native mobile apps (use Apple HIG / Material directly)

---

## 1. Brief Inference (Read the Room)

Before touching code, **infer what the user actually wants**. Most LLM design output is bad because the model jumps to a default aesthetic instead of reading the room.

### Read these signals first

1. **Page kind** - landing (SaaS / consumer / agency / event), portfolio (dev / designer / studio), redesign (preserve vs overhaul), editorial / blog.
2. **Vibe words** - "minimalist", "calm", "Linear-style", "Awwwards", "brutalist", "premium consumer", "Apple-y", "playful", "serious B2B", "editorial", "glassy", "dark tech".
3. **Reference signals** - URLs linked, screenshots pasted, products named.
4. **Audience** - B2B procurement panel vs. design-conscious consumer vs. recruiter scanning a portfolio.
5. **Brand assets that already exist** - logo, color, type, photography. For redesigns, these are starting material (see `references/redesign-protocol.md`).
6. **Quiet constraints** - accessibility-first, public-sector, regulated industries, trust-first commerce, kids' products. These OVERRIDE aesthetic preference.

### Output a "Design Read" before generating

State in one line: **"Reading this as: \<page kind> for \<audience>, with a \<vibe> language, leaning toward \<design system or aesthetic family>."**

If the brief is ambiguous, ask **one** clarifying question, never a multi-question dump. If you can confidently infer, do not ask.

### Anti-Default Discipline

Do NOT default to: AI-purple gradients, centered hero over dark mesh, three equal feature cards, generic glassmorphism on everything, infinite-loop micro-animations everywhere, Inter + slate-900. These are the LLM defaults. Reach past them deliberately.

---

## 2. The Three Dials

After the design read, set three dials. Every layout, motion, and density decision is gated by these.

| Dial | Default | Range |
|------|---------|-------|
| **DESIGN_VARIANCE** | `8` | 1 = Perfect Symmetry, 10 = Artsy Chaos |
| **MOTION_INTENSITY** | `6` | 1 = Static, 10 = Cinematic / Physics |
| **VISUAL_DENSITY** | `4` | 1 = Art Gallery / Airy, 10 = Cockpit / Packed Data |

### Dial Inference

| Signal | VARIANCE | MOTION | DENSITY |
|--------|----------|--------|---------|
| "minimalist / clean / calm / editorial / Linear-style" | 5-6 | 3-4 | 2-3 |
| "premium consumer / Apple-y / luxury / brand" | 7-8 | 5-7 | 3-4 |
| "playful / wild / Dribbble / Awwwards / experimental / agency" | 9-10 | 8-10 | 3-4 |
| "landing page / portfolio / marketing site (default)" | 7-9 | 6-8 | 3-5 |
| "trust-first / public-sector / regulated / accessibility-critical" | 3-4 | 2-3 | 4-5 |
| "redesign - preserve" | match existing | +1 | match existing |
| "redesign - overhaul" | +2 | +2 | match existing |

---

## 3. Style Routing

Based on the design read, load the matching style reference for detailed rules:

| Design Read | Reference File | Vibe |
|-------------|---------------|------|
| Minimalist, editorial, Linear-style, Notion-like | `references/styles/minimalist.md` | Warm monochrome, typography contrast, flat bento, soft pastel |
| Brutalist, industrial, tactical, terminal, Swiss print | `references/styles/brutalist.md` | Raw mechanical, extreme type contrast, utilitarian color, CRT texture |
| Premium, luxury, agency, $150k+ feel, Apple-adjacent | `references/styles/soft-premium.md` | Haptic depth, cinematic spacing, double-bezel, spring motion |
| GPT/Codex strict, higher variance, aggressive anti-slop | `references/styles/gpt-strict.md` | Stricter layout variance, stronger GSAP direction |

For **redesigns** of existing projects: load `references/redesign-protocol.md` for the audit-first workflow.

If none of the above fits, use the core directives below with the default stack.

---

## 4. Design System Selection

When the brief reads as a specific platform, use the **official** package. Do not recreate its CSS by hand.

| Brief reads as... | Use | Why |
|-------------------|-----|-----|
| Microsoft / enterprise SaaS / dashboards | `@fluentui/react-components` | Official Fluent UI |
| Google-ish UI, Material-flavored | `@material/web` + Material 3 tokens | Official, theme-able |
| IBM-style B2B / enterprise analytics | `@carbon/react` | Official Carbon |
| Shopify app surfaces | `polaris.js` | Required for Shopify admin |
| Atlassian / Jira-style product | `@atlaskit/*` | Official Atlassian DS |
| GitHub-style devtool | `@primer/css` | Official Primer |
| Public-sector UK service | `govuk-frontend` | Legally expected |
| US public-sector / trust-first | `uswds` | Same |
| Modern accessible React foundation | `@radix-ui/themes` | Primitives + polished theme |
| Modern SaaS where you own components | `shadcn/ui` (customize, never ship default state) | You own the code |
| Tailwind-based modern SaaS | Tailwind v4 utilities + `dark:` variant | Default for indie builds |

**Rules:** One system per project. Do not mix systems in the same tree. See `references/design-systems.md` for install commands and canonical doc links.

For aesthetic directions (glassmorphism, bento, brutalism, editorial, kinetic typography, Apple Liquid Glass) with no official package: build with native CSS + Tailwind. See `references/design-systems.md` for implementation guidance.

---

## 5. Default Stack

Unless the design read picks a real design system:

- **Framework:** React or Next.js. Default to Server Components (RSC). Wrap interactive components in `"use client"`.
- **Styling:** Tailwind v4. Use `@tailwindcss/postcss`, not the old `tailwindcss` plugin.
- **Animation:** Motion (formerly Framer Motion). Import from `motion/react`.
- **Fonts:** `next/font` or self-host with `@font-face` + `font-display: swap`. Never `<link>` Google Fonts in production.
- **State:** Local `useState` / `useReducer`. Global state only for prop-drilling avoidance (Zustand, Jotai, context). NEVER use `useState` for continuous values (mouse, scroll) - use Motion's `useMotionValue`.
- **Icons:** `@phosphor-icons/react` (priority), `hugeicons-react`, `@radix-ui/react-icons`, `@tabler/icons-react`. Discouraged: `lucide-react`. NEVER hand-roll SVG icons. One family per project. Standardize `strokeWidth`.
- **Emoji:** Discouraged by default. Override only when user explicitly asks for playful / chat-style vibe.
- **Responsiveness:** Standard breakpoints (sm 640, md 768, lg 1024, xl 1280, 2xl 1536). `max-w-[1400px] mx-auto`. NEVER `h-screen` - use `min-h-[100dvh]`. Grid over flex-math.
- **Dependency check:** Before importing ANY 3rd-party library, check `package.json`.

---

## 6. Core Directives

LLMs default to cliches. Override these proactively. Each rule has a context-aware override path.

### Typography
- Display: `text-4xl md:text-6xl tracking-tighter leading-none`. Body: `text-base text-gray-600 leading-relaxed max-w-[65ch]`.
- **Discouraged as default:** `Inter`. Pick `Geist`, `Outfit`, `Cabinet Grotesk`, `Satoshi` first. Override: Inter OK for neutral / Linear-style / public-sector.
- **SERIF DISCIPLINE:** Very discouraged as default. Only when brand brief names a serif, or aesthetic is genuinely editorial / luxury / publication. **BANNED as defaults:** `Fraunces` and `Instrument_Serif`.
- **ITALIC DESCENDER CLEARANCE:** When italic used in display with descender letters (y g j p q), use `leading-[1.1]` minimum + `pb-1`.

### Color
- Max 1 accent color. Saturation < 80%.
- **LILA RULE:** AI Purple / Blue glow discouraged as default. Use neutral bases (Zinc / Slate / Stone) with high-contrast singular accents.
- **One palette per project.** No warm/cool gray mixing.
- **COLOR CONSISTENCY LOCK:** Once accent chosen, used on WHOLE page.
- **PREMIUM-CONSUMER PALETTE BAN:** Beige+brass+espresso default banned for premium-consumer briefs. Rotate alternatives (Cold Luxury, Forest, Black and Tan, Cobalt+Cream, Terracotta+Slate).

### Layout
- **ANTI-CENTER BIAS:** Centered Hero avoided when `DESIGN_VARIANCE > 4`. Force split screen or asymmetric layouts.
- **Hero MUST fit initial viewport:** headline max 2 lines, subtext max 20 words, CTAs visible without scroll.
- **HERO TOP PADDING CAP:** max `pt-24` at desktop.
- **HERO STACK:** max 4 text elements (eyebrow OR brand strip, headline, subtext, CTAs).
- **Navigation:** single line at desktop, height max 80px.
- **BENTO CELL COUNT:** exact as many cells as content items. No empty cells.
- **Section-Layout-Repetition Ban:** same layout family appears at most ONCE per page. 8 sections need 4+ different families.
- **ZIGZAG ALTERNATION CAP:** max 2 consecutive image+text-split sections.
- **EYEBROW RESTRAINT:** max 1 eyebrow per 3 sections. Hero counts as 1.
- **SPLIT-HEADER BAN:** no "left big headline + right small explainer" as section header.
- **BENTO BACKGROUND DIVERSITY:** at least 2-3 cells need real visual variation.

### Materiality
- Cards ONLY when elevation communicates real hierarchy. Otherwise use `border-t`, `divide-y`, or negative space.
- Shadows tinted to background hue. No pure-black drop shadows on light backgrounds.
- **SHAPE CONSISTENCY LOCK:** one corner-radius scale per page.

### Interactive States
- Full cycles: loading (skeletal), empty states, error states (inline, not `window.alert`).
- Tactile feedback: `:active` uses `-translate-y-[1px]` or `scale-[0.98]`.
- **BUTTON CONTRAST CHECK:** WCAG AA 4.5:1 for body, 3:1 for large text.
- **CTA BUTTON WRAP BAN:** button text fits one line at desktop. Max 3 words for primary CTAs.
- **NO DUPLICATE CTA INTENT:** one label per intent across the page.
- **FORM CONTRAST CHECK:** all form elements pass WCAG AA.

### Content
- Default per section: headline (max 8 words) + sub-paragraph (max 25 words) + one visual OR CTA.
- No data-dump sections. Long lists (>5 items) need different UI component.
- **COPY SELF-AUDIT:** re-read every visible string before shipping. Flag grammatically broken, unclear referents, AI hallucination, mock-poetic micro-meta.
- Fake-precise numbers banned unless from real data or explicitly labeled mock.

### Images
- Priority: image-gen tool first, real web images (picsum.photos/seed) second, tell user third.
- NO div-based fake screenshots. NO hand-rolled decorative SVGs.
- Logo walls: use real SVG logos (Simple Icons CDN). NO plain text wordmarks for invented brands. LOGO-ONLY (no category labels below logos).
- Hero needs a real visual. Text + gradient blob is not a hero.

### Page Theme Lock
- ONE theme (light, dark, or auto) for the whole page. Sections do not invert.
- Design for both modes from the start. WCAG AA contrast across both.
- No pure `#000000` and no pure `#ffffff`.

---

## 7. Motion Rules

### When to animate
- **MOTION MUST BE MOTIVATED:** every animation needs one-sentence reason (hierarchy / storytelling / feedback / state transition). "It looked cool" is invalid.
- **Motion claimed = motion shown:** if `MOTION_INTENSITY > 4`, page must actually animate. If you cannot ship working motion, drop the dial to 3.
- **MARQUEE MAX-ONE-PER-PAGE.**

### Allowed patterns
- Motion `whileInView` for scroll reveals (lighter than GSAP).
- GSAP ScrollTrigger for pin/scrub work only. See `references/motion-skeletons.md` for canonical skeletons.
- Spring physics (`type: "spring", stiffness: 100, damping: 20`). No linear easing.

### Forbidden patterns
- `window.addEventListener("scroll", ...)` - BANNED. Use Motion `useScroll()`, ScrollTrigger, IntersectionObserver, or CSS `animation-timeline: view()`.
- `requestAnimationFrame` loops touching React state. Use `useMotionValue` + `useTransform`.
- Animate only `transform` and `opacity`. NEVER `top`, `left`, `width`, `height`.
- NEVER mix GSAP / Three.js with Motion in the same component tree.

### Reduced Motion (mandatory)
Any motion with `MOTION_INTENSITY > 3` MUST honor `prefers-reduced-motion`. Wrap with `useReducedMotion()` and degrade to static. Non-negotiable.

---

## 8. Performance Guardrails

- **LCP** < 2.5s. Hero image `next/image priority` or preloaded.
- **INP** < 200ms. Heavy work off main thread.
- **CLS** < 0.1. Reserve space for images, fonts, embeds.
- Grain/noise filters ONLY on `fixed` + `pointer-events-none` pseudo-elements.
- Z-index for systemic layer contexts only (sticky navbars, modals, overlays, grain). Document the scale.
- `useEffect` animations MUST have strict cleanup functions.

---

## 9. Output Enforcement

Every task is production-critical. Partial output = broken output. Do not optimize for brevity, optimize for completeness.

### Banned Output Patterns (hard fail, never produce)

**In code blocks:**
- `// ...`, `// rest of code`, `// implement here`, `// TODO`, `/* ... */`
- `// similar to above`, `// continue pattern`, `// add more as needed`, bare `...`

**In prose:**
- "Let me know if you want me to continue"
- "I can provide more details if needed"
- "for brevity", "the rest follows the same pattern"
- "similarly for the remaining", "and so on"
- "I'll leave that as an exercise"

**Structural shortcuts:**
- Requested full implementation but given skeleton
- Showed first and last, skipped middle
- Used 1 example + description instead of repeated logic
- Described what code should do instead of writing code

### Handling Long Outputs
Do NOT compress remaining parts. Do NOT jump to conclusion. Write to a clean break point (function end / file end / section end), end with `[PAUSED - X of Y complete. Send "continue" to resume from: next section name]`. Resume exactly from breakpoint on "continue".

---

## 10. Pre-Flight Check (MANDATORY)

Run `references/pre-flight-check.md` before outputting code. This is the last filter. Every box must pass. If any box fails, the output is not done.

Also check `references/ai-tells.md` for the full forbidden patterns list. Key highlights:

- **ZERO em-dashes (`—`)** anywhere on the page. Headlines, eyebrows, pills, body, quotes, attribution, captions, buttons, alt text. Zero. Use regular hyphen (`-`). Non-negotiable.
- NO version labels in hero (`V0.6`, `BETA`) unless launch brief.
- NO section-number eyebrows (`00 / INDEX`, `001 · Capabilities`).
- NO scroll cues (`Scroll`, `Scroll to explore`).
- NO decorative status dots by default.
- NO `border-t` + `border-b` on every row of spec tables.
- NO locale / time / weather strips unless place-focused brief.
- NO pills/labels overlaid on images.
- NO fake product UI built from divs.

---

## Reference Files

| File | When to Load |
|------|-------------|
| `references/styles/minimalist.md` | Editorial / Linear / Notion-like clean UI |
| `references/styles/brutalist.md` | Industrial / Swiss / terminal aesthetic |
| `references/styles/soft-premium.md` | Luxury / agency / $150k+ haptic design |
| `references/styles/gpt-strict.md` | GPT/Codex stricter variance variant |
| `references/redesign-protocol.md` | Modifying an existing project (audit first) |
| `references/ai-tells.md` | Full forbidden patterns checklist (40+ items) |
| `references/pre-flight-check.md` | Mandatory pre-ship verification (40+ items) |
| `references/design-systems.md` | Install commands + canonical doc links per system |
| `references/motion-skeletons.md` | GSAP/Motion canonical code skeletons + pattern vocabulary |
