---
name: frontend-design
description: Build distinctive, production-grade frontend UIs with high design quality. Trigger when user asks to create, style, or beautify web pages, components, dashboards, landing pages, posters, or any HTML/CSS/JS/React/Vue interface.
license: Complete terms in LICENSE.txt
---

This skill guides creation of distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. Implement real working code with exceptional attention to aesthetic details and creative choices.

## Step 1: Understand the Brief

Before writing any code, answer these four questions (silently -- do not print them):

1. **Purpose**: What does this interface do and who is the audience?
2. **Tone**: Select ONE specific aesthetic direction from the palette below (or invent one). Do not blend more than two.
3. **Constraints**: Framework, browser support, accessibility level, performance budget.
4. **Signature detail**: Identify one memorable element that makes this design unforgettable (e.g., a dramatic scroll animation, an unusual grid structure, a textured background, a bold typographic hierarchy).

### Aesthetic Palette (pick one or combine two max)

- Brutally minimal -- near-monochrome, extreme whitespace, single typeface, no decoration
- Maximalist chaos -- layered elements, clashing colors, overlapping text, controlled disorder
- Retro-futuristic -- CRT glow, scanlines, monospace type, neon on dark
- Organic/natural -- earth tones, hand-drawn textures, rounded shapes, warmth
- Luxury/refined -- high contrast, serif headlines, restrained gold/cream accents, ample spacing
- Playful/toy-like -- rounded sans-serif, saturated primaries, bouncy animations, chunky borders
- Editorial/magazine -- strong grid, dramatic type scale, pull quotes, black-and-white photography feel
- Brutalist/raw -- system fonts used ironically, exposed structure, harsh borders, no polish
- Art deco/geometric -- symmetry, metallic gradients, chevrons, ornamental lines
- Soft/pastel -- muted palette, gentle shadows, rounded corners, light motion
- Industrial/utilitarian -- monospace, muted greens/ambers, dense information, no fluff

## Step 2: Select Typography

Choose fonts from Google Fonts (or Bunny Fonts for GDPR). Load via `<link>` or `@import`.

**Rules:**
- NEVER use: Inter, Roboto, Arial, Helvetica, Open Sans, Lato, system-ui, or any default system font stack.
- NEVER reuse Space Grotesk, Poppins, or Montserrat across generations -- rotate choices.
- Pair exactly two fonts: one display/headline font and one body font.

**Curated starting points (vary from these -- do not always pick the same one):**

| Category | Display options | Body options |
|---|---|---|
| Geometric | Clash Display, Satoshi, General Sans | Outfit, Plus Jakarta Sans, Manrope |
| Serif | Playfair Display, Fraunces, Libre Caslon | Source Serif 4, Lora, Literata |
| Mono/Tech | JetBrains Mono, Space Mono, IBM Plex Mono | IBM Plex Sans, Overpass, Barlow |
| Humanist | Bricolage Grotesque, Gabarito, Instrument Sans | Nunito Sans, Karla, Work Sans |
| Expressive | Syne, Cabinet Grotesk, Instrument Serif | DM Sans, Figtree, Geist |

## Step 3: Build a Color System

Define a palette using CSS custom properties on `:root`. Every design must declare at minimum:

```css
:root {
  --color-bg: /* dominant background */;
  --color-surface: /* cards, panels */;
  --color-text: /* primary text */;
  --color-text-muted: /* secondary text */;
  --color-accent: /* primary action/highlight */;
  --color-accent-hover: /* accent interaction state */;
}
```

**Rules:**
- NEVER default to purple-gradient-on-white. Rotate hue families across generations.
- Use a dominant color with one sharp accent. Evenly-distributed multi-color palettes look weak.
- For dark themes: avoid pure #000 backgrounds -- use tinted darks (e.g., `#0a0a0f`, `#1a1a2e`).
- For light themes: avoid pure #fff -- use warm or cool off-whites (e.g., `#faf9f6`, `#f0f4f8`).
- Ensure WCAG AA contrast (4.5:1) for body text. Use a contrast checker mentally.

## Step 4: Implement Layout and Spatial Composition

**Rules:**
- Avoid cookie-cutter centered-card layouts. Consider asymmetry, overlapping elements, diagonal flow, grid-breaking hero sections, or edge-to-edge compositions.
- Use CSS Grid or Flexbox. For complex layouts, use named grid areas.
- Be intentional with spacing: either generous negative space (minimalist) or controlled density (utilitarian). Never middling.
- Set a spacing scale using CSS variables: `--space-xs` through `--space-3xl`.

## Step 5: Add Motion and Interaction

Use CSS animations and transitions by default. Only reach for JS animation libraries when CSS cannot achieve the effect.

**High-impact patterns (pick 1-3, not all):**
- Staggered entrance reveals on page load using `animation-delay`
- Scroll-triggered fade/slide using `IntersectionObserver`
- Hover state transforms (scale, color shift, shadow elevation)
- Smooth page/section transitions
- Micro-interactions on buttons and form elements (ripple, glow, press)

**Rules:**
- Respect `prefers-reduced-motion` -- wrap animations in `@media (prefers-reduced-motion: no-preference)`.
- Keep durations between 200ms-600ms for UI transitions; up to 1200ms for dramatic reveals.
- Use `ease-out` or custom cubic-bezier for entrances; `ease-in` for exits.
- For React projects, use Motion (framer-motion) if available in the project.

## Step 6: Add Depth and Atmosphere

Do not default to flat solid-color backgrounds. Layer visual texture appropriate to the chosen aesthetic:

- **Gradient meshes**: Multi-stop radial/conic gradients for organic backgrounds
- **Noise/grain**: CSS `background-image` with a tiny noise SVG or base64 PNG overlay at low opacity
- **Geometric patterns**: Repeating SVG patterns for structure
- **Shadows**: Use layered box-shadows (2-3 layers) for realistic depth, not single-layer flat shadows
- **Glassmorphism**: `backdrop-filter: blur()` with semi-transparent backgrounds (use sparingly)
- **Borders/dividers**: Subtle gradient borders, dashed lines, or ornamental SVG dividers

## Step 7: Deliver Production-Grade Code

**Structure:**
- For single-file deliverables: one self-contained HTML file with embedded `<style>` and `<script>`.
- For component deliverables: framework-appropriate file structure (`.jsx`/`.vue`/`.svelte`).
- All CSS must use custom properties for colors, spacing, and font families -- no hardcoded hex values scattered through the stylesheet.

**Quality checks before delivering:**
- Responsive: test mentally at 320px, 768px, 1440px widths. Add breakpoints as needed.
- Accessible: semantic HTML elements, sufficient color contrast, focus-visible styles, alt text.
- Performant: no layout thrashing, images lazy-loaded if present, animations on compositor-friendly properties (transform, opacity).
- No dead code, no commented-out blocks, no placeholder "lorem ipsum" unless the user asked for placeholder content.

## Anti-Patterns to Avoid

- Generic card grids with rounded corners and light shadows (the "SaaS template" look)
- Purple/blue gradient CTAs on white backgrounds
- Identical spacing and sizing throughout (creates visual monotony)
- Over-reliance on icon libraries without typographic or compositional interest
- Using the same aesthetic, fonts, or color scheme across different generations

Remember: Claude is capable of extraordinary creative work. Commit fully to the chosen direction and execute every detail with intention.
