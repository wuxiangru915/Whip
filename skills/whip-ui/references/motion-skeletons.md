<!-- Reference: Motion Skeletons & Pattern Vocabulary - migrated from taste-skill Sections 5 + 10 -->

# Motion Skeletons & Pattern Vocabulary

## Context-Aware Proactivity

These are tools, not defaults. Use them when the design read calls for them. **None of these fire automatically.**

- **Liquid Glass / Glassmorphism:** Appropriate for premium consumer, Apple-adjacent, luxury brand, or media-overlay vibes. Inappropriate for dashboards, public-sector, or "boring B2B." When used, go beyond `backdrop-blur`: add a 1px inner border (`border-white/10`) and a subtle inner shadow (`shadow-[inset_0_1px_0_rgba(255,255,255,0.1)]`) for physical edge refraction. Provide a solid-fill fallback under `prefers-reduced-transparency`.
- **Magnetic Micro-physics:** Use when `MOTION_INTENSITY > 5` AND the brief reads premium / playful / agency. Implement EXCLUSIVELY with Motion's `useMotionValue` / `useTransform` outside the React render cycle. Never `useState`.
- **Perpetual Micro-Interactions** (Pulse, Typewriter, Float, Shimmer, Carousel): Use when `MOTION_INTENSITY > 5` AND the section actively benefits from motion. **Not every card needs an infinite loop.** If a section is informational, leave it still. Apply Spring Physics (`type: "spring", stiffness: 100, damping: 20`) - no linear easing.
- **"Motion claimed, motion shown."** If `MOTION_INTENSITY > 4`, the page must actually move: entry transitions on hero, scroll-reveal on key sections, hover physics on CTAs, at minimum. A static page that claims `MOTION_INTENSITY: 7` is broken. Conversely, if you cannot ship working motion in the available scope, drop the dial to 3 and ship a clean static page. Never half-build motion that breaks.
- **MOTION MUST BE MOTIVATED.** Before adding any animation, ask: "what does this animation communicate?" Valid answers: hierarchy, storytelling, feedback, state transition. Invalid answer: "it looked cool". Each ScrollTrigger, each marquee, each pinned section needs a reason. If you cannot articulate the reason in one sentence, drop the animation.
- **MARQUEE MAX-ONE-PER-PAGE.** Horizontal scrolling text marquees are appropriate at most ONCE per page. Two or more reads as lazy filler.

---

## Sticky-Stack - Canonical Skeleton

```tsx
"use client";
import { useRef, useEffect } from "react";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { useReducedMotion } from "motion/react";

gsap.registerPlugin(ScrollTrigger);

export function StickyStack({ cards }: { cards: React.ReactNode[] }) {
  const ref = useRef<HTMLDivElement>(null);
  const reduce = useReducedMotion();

  useEffect(() => {
    if (reduce || !ref.current) return;
    const ctx = gsap.context(() => {
      const cardEls = gsap.utils.toArray<HTMLElement>(".stack-card");
      cardEls.forEach((card, i) => {
        if (i === cardEls.length - 1) return;
        ScrollTrigger.create({
          trigger: card,
          start: "top top",
          endTrigger: cardEls[cardEls.length - 1],
          end: "top top",
          pin: true,
          pinSpacing: false,
        });
        gsap.to(card, {
          scale: 0.92,
          opacity: 0.55,
          ease: "none",
          scrollTrigger: {
            trigger: cardEls[i + 1],
            start: "top bottom",
            end: "top top",
            scrub: true,
          },
        });
      });
    }, ref);
    return () => ctx.revert();
  }, [reduce]);

  return (
    <div ref={ref} className="relative">
      {cards.map((card, i) => (
        <div
          key={i}
          className="stack-card sticky top-0 min-h-[100dvh] flex items-center justify-center"
        >
          {card}
        </div>
      ))}
    </div>
  );
}
```

Critical points: `start: "top top"`, `pin: true`, every card except the last is pinned, the scale/opacity transform is driven by the NEXT card's scroll trigger (so previous card shrinks as next one arrives).

---

## Horizontal-Pan - Canonical Skeleton

```tsx
"use client";
import { useRef, useEffect } from "react";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { useReducedMotion } from "motion/react";

gsap.registerPlugin(ScrollTrigger);

export function HorizontalPan({ children }: { children: React.ReactNode }) {
  const wrap = useRef<HTMLDivElement>(null);
  const track = useRef<HTMLDivElement>(null);
  const reduce = useReducedMotion();

  useEffect(() => {
    if (reduce || !wrap.current || !track.current) return;
    const ctx = gsap.context(() => {
      const distance = track.current!.scrollWidth - window.innerWidth;
      gsap.to(track.current, {
        x: -distance,
        ease: "none",
        scrollTrigger: {
          trigger: wrap.current,
          start: "top top",
          end: () => `+=${distance}`,
          pin: true,
          scrub: 1,
          invalidateOnRefresh: true,
        },
      });
    }, wrap);
    return () => ctx.revert();
  }, [reduce]);

  return (
    <section ref={wrap} className="relative overflow-hidden">
      <div ref={track} className="flex h-[100dvh] items-center">
        {children}
      </div>
    </section>
  );
}
```

Critical points: `start: "top top"`, `pin: true`, `end: "+=${distance}"` (scroll length = horizontal travel needed), `scrub: 1`. The wrapper is pinned, the inner track slides horizontally as the user scrolls vertically.

---

## Scroll-Reveal Stagger - Canonical Skeleton (lighter alternative)

For simple "items appear as they enter viewport" (no pinning), prefer Motion's `whileInView` over GSAP - lighter, no ScrollTrigger needed:

```tsx
"use client";
import { motion, useReducedMotion } from "motion/react";

export function RevealStagger({ items }: { items: string[] }) {
  const reduce = useReducedMotion();
  return (
    <ul className="grid gap-6">
      {items.map((item, i) => (
        <motion.li
          key={item}
          initial={reduce ? false : { opacity: 0, y: 24 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true, amount: 0.3 }}
          transition={{
            duration: 0.6,
            delay: i * 0.06,
            ease: [0.16, 1, 0.3, 1],
          }}
        >
          {item}
        </motion.li>
      ))}
    </ul>
  );
}
```

Use this for: feature lists, testimonial grids, logo walls, anything that just needs "enter on scroll." Save GSAP for actual pin/scrub work.

---

## Forbidden Animation Patterns

- **`window.addEventListener("scroll", ...)`** is banned. It runs on every scroll frame, jank-prone, no batching. Use Motion's `useScroll()`, GSAP's `ScrollTrigger`, IntersectionObserver, or CSS `scroll-driven animations` (`animation-timeline: view()`).
- **Custom scroll progress calculations using `window.scrollY`** in React state. Same reason. Re-renders on every frame.
- **`requestAnimationFrame` loops that touch React state.** Use motion values (`useMotionValue` + `useTransform`) instead.
- **Layout Transitions:** Use Motion's `layout` and `layoutId` props for visible state changes (re-ordering lists, expanding modals, shared elements between routes). Do not wrap static content in `layout` props "for safety" - it costs measurement work.
- **Staggered Orchestration:** Use `staggerChildren` (Motion) or CSS cascade (`animation-delay: calc(var(--index) * 100ms)`) for reveal moments where sequence matters. For `staggerChildren`, parent (`variants`) and children MUST share the same Client Component tree.

---

## Reference Vocabulary (Pattern Names)

This is a vocabulary, not a library. Know these pattern names to communicate about them, design with them in mind, and reach for them when the design read calls for them.

### Hero Paradigms
- **Asymmetric Split Hero** - Text on one side, asset on the other, generous white space.
- **Editorial Manifesto Hero** - Large type, no asset, almost-poster.
- **Video / Media Mask Hero** - Type cut out as mask over video background.
- **Kinetic-Type Hero** - Animated typography as the primary visual.
- **Curtain-Reveal Hero** - Hero parts on scroll like a curtain.
- **Scroll-Pinned Hero** - Hero stays pinned while content scrolls behind.

### Navigation & Menus
- **Mac OS Dock Magnification** - Edge nav, icons scale fluidly on hover.
- **Magnetic Button** - Pulls toward cursor.
- **Gooey Menu** - Sub-items detach like viscous liquid.
- **Dynamic Island** - Morphing pill for status / alerts.
- **Contextual Radial Menu** - Circular menu expanding at click point.
- **Floating Speed Dial** - FAB springing into curved secondary actions.
- **Mega Menu Reveal** - Full-screen dropdown, stagger-fade content.

### Layout & Grids
- **Bento Grid** - Asymmetric tile grouping (Apple Control Center).
- **Masonry Layout** - Staggered grid, no fixed row height.
- **Chroma Grid** - Borders / tiles with subtle animating gradients.
- **Split-Screen Scroll** - Two halves sliding in opposite directions.
- **Sticky-Stack Sections** - Sections that pin and stack on scroll.

### Cards & Containers
- **Parallax Tilt Card** - 3D tilt tracking mouse coordinates.
- **Spotlight Border Card** - Borders illuminate under cursor.
- **Glassmorphism Panel** - Frosted glass with inner refraction.
- **Holographic Foil Card** - Iridescent rainbow shift on hover.
- **Tinder Swipe Stack** - Physical card stack, swipe-away.
- **Morphing Modal** - Button expands into its own dialog.

### Scroll Animations
- **Sticky Scroll Stack** - Cards stick and physically stack.
- **Horizontal Scroll Hijack** - Vertical scroll -> horizontal pan.
- **Locomotive / Sequence Scroll** - Video / 3D sequence tied to scrollbar.
- **Zoom Parallax** - Central background image zooming on scroll.
- **Scroll Progress Path** - SVG line drawing along scroll.
- **Liquid Swipe Transition** - Page transition like viscous liquid.

### Galleries & Media
- **Dome Gallery** - 3D panoramic gallery.
- **Coverflow Carousel** - 3D carousel with angled edges.
- **Drag-to-Pan Grid** - Boundless draggable canvas.
- **Accordion Image Slider** - Narrow strips expanding on hover.
- **Hover Image Trail** - Mouse leaves popping image trail.
- **Glitch Effect Image** - RGB-channel shift on hover.

### Typography & Text
- **Kinetic Marquee** - Endless text bands reversing on scroll.
- **Text Mask Reveal** - Massive type as transparent window to video.
- **Text Scramble Effect** - Matrix-style decoding on load / hover.
- **Circular Text Path** - Text curving along spinning circle.
- **Gradient Stroke Animation** - Outlined text with running gradient.
- **Kinetic Typography Grid** - Letters dodging the cursor.

### Micro-Interactions & Effects
- **Particle Explosion Button** - CTA shatters into particles on success.
- **Liquid Pull-to-Refresh** - Reload indicator like detaching droplets.
- **Skeleton Shimmer** - Shifting light reflection across placeholders.
- **Directional Hover-Aware Button** - Fill enters from cursor's exact side.
- **Ripple Click Effect** - Wave from click coordinates.
- **Animated SVG Line Drawing** - Vectors drawing themselves in real time.
- **Mesh Gradient Background** - Organic lava-lamp blobs.
- **Lens Blur Depth** - Background UI blurred to focus foreground action.

### Animation Library Choice
- **Motion (`motion/react`)** - default for UI / Bento / state-change motion.
- **GSAP + ScrollTrigger** - for full-page scrolltelling and scroll hijacks. Isolate in dedicated leaf components with `useEffect` cleanup.
- **Three.js / WebGL** - for canvas backgrounds and 3D scenes. Same isolation rule.
- **NEVER mix GSAP / Three.js with Motion in the same component tree.** They fight over the same frames.

---

## Dial Definitions (Technical Reference)

### DESIGN_VARIANCE (Level 1-10)
- **1-3 (Predictable):** Symmetrical CSS Grid (12-col, equal fr-units), equal paddings, centered alignment.
- **4-7 (Offset):** `margin-top: -2rem` overlaps, varied image aspect ratios (4:3 next to 16:9), left-aligned headers over center-aligned data.
- **8-10 (Asymmetric):** Masonry layouts, CSS Grid with fractional units (`grid-template-columns: 2fr 1fr 1fr`), massive empty zones (`padding-left: 20vw`).
- **MOBILE OVERRIDE:** For levels 4-10, asymmetric layouts above `md:` MUST collapse to strict single-column (`w-full`, `px-4`, `py-8`) on viewports `< 768px`.

### MOTION_INTENSITY (Level 1-10)
- **1-3 (Static):** No automatic animations. CSS `:hover` and `:active` states only. `prefers-reduced-motion` is the default mode anyway.
- **4-7 (Fluid CSS):** `transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1)`. `animation-delay` cascades for load-ins. Focus on `transform` and `opacity`.
- **8-10 (Advanced Choreography):** Complex scroll-triggered reveals, parallax, scroll-driven animation (CSS `animation-timeline` or GSAP ScrollTrigger). Use Motion hooks. **NEVER use `window.addEventListener('scroll')`** - it is a hard ban.

### VISUAL_DENSITY (Level 1-10)
- **1-3 (Art Gallery):** Lots of white space. Huge section gaps (`py-32` to `py-48`). Expensive, clean.
- **4-7 (Daily App):** Standard web app spacing (`py-16` to `py-24`).
- **8-10 (Cockpit):** Tight paddings. No card boxes; 1px lines separate data. Mandatory: `font-mono` for all numbers.
