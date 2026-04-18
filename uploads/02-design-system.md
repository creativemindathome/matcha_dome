# 02 · Design System

## Design principles

1. **Instrument, not app.** Every surface should feel like it was milled, not rendered. Sharp corners. Honest materials. Type that reveals, not decorates.
2. **Ceremony, not convenience.** Slow, deliberate pace. One idea per viewport. Nothing hurried. Pauses between sections.
3. **Light is the primary material.** The site feels like a north-facing studio, not a lit showroom. Warm daylight tones. Ink appears only to mark gravity — never as ambient mood.
4. **Data is decoration.** Measurements, wavelengths, spectra, grades — numbers in mono type carry aesthetic weight, so expose them.
5. **One voice at a time.** At any scroll position, either the photography speaks or the type speaks. Never both competing.
6. **Asymmetry with intent.** Grid as a skeleton to break, not obey.
7. **The product demonstrates itself.** Wherever possible, show the dome *doing* its job, not just sitting on a backdrop.

---

## Color tokens

Drop in `app/globals.css` under `@theme` (Tailwind v4 syntax):

```css
@theme {
  /* Primary surfaces — paper-dominant */
  --color-paper:      #EDE8DF;
  --color-paper-soft: #F5F1E8;

  /* Ink — punctuation only */
  --color-ink:        #0C0B09;
  --color-ink-soft:   #1A1815;

  /* Stone — warm neutrals for product photography contexts */
  --color-stone:      #AEA69D;
  --color-stone-deep: #6D6962;

  /* Accents */
  --color-gold:       #C9A45C;
  --color-gold-soft:  #A68848;

  /* Matcha — used sparingly; photography carries most green */
  --color-koicha:     #4A6B3A;
  --color-usucha:     #8FA654;
  --color-matcha-bg:  #2F3D26;

  /* Signals */
  --color-signal:     #C9A45C;
  --color-warn:       #D97757;

  /* Derived neutrals */
  --color-ink-60:     #0C0B0999;
  --color-paper-60:   #EDE8DF99;
  --color-hairline:   #0C0B0918;     /* ink at 9%, dividers on paper */
  --color-hairline-gold: #C9A45C33;  /* gold at 20%, dividers on ink */

  /* Quality gradient — 5 stops for the Koku Score bar */
  --grad-quality-0:   #6B5E3A;   /* oxidized — dull olive-brown */
  --grad-quality-25:  #8A8146;   /* culinary — muted green */
  --grad-quality-50:  #8FA654;   /* usucha — bright thin-brewed */
  --grad-quality-75:  #6B8A3E;   /* ceremonial — rich green */
  --grad-quality-100: #4A6B3A;   /* koicha — deep thick-brewed */
}
```

### Dominant-surface rule

Across the full landing page, sections use backgrounds in this ratio:
- **Paper (or paper-soft): 70%** of scroll-length.
- **Ink: 20%** of scroll-length (concentrated in Hero, Koku Score, and Preorder CTA — the three dramatic moments).
- **Stone / stone-deep: 10%** (used inside product photography compositions — as backdrops in image shots, not as full section backgrounds).

The Koku Score section is the one **matcha-green background** moment. No other section uses green as a surface; the photography carries the green.

### Per-viewport rule

Any given viewport uses at most **3 of these colors** (excluding photography). If you need a 4th, you're overdesigning.

---

## Typography

### Font stack

```css
@theme {
  --font-display: "Fraunces", ui-serif, Georgia, serif;
  --font-sans:    "Instrument Sans", ui-sans-serif, system-ui, sans-serif;
  --font-mono:    "JetBrains Mono", ui-monospace, monospace;
}
```

Loaded via `next/font`:

```typescript
// app/layout.tsx
import { Fraunces, Instrument_Sans, JetBrains_Mono } from "next/font/google";

const fraunces = Fraunces({
  subsets: ["latin"],
  variable: "--font-display",
  axes: ["SOFT", "WONK", "opsz"],
});
const instrument = Instrument_Sans({
  subsets: ["latin"], variable: "--font-sans"
});
const jetbrains = JetBrains_Mono({
  subsets: ["latin"], variable: "--font-mono"
});
```

### Type scale (desktop — halves or clamps on mobile)

| Token | Font | Size / Line | Weight | Tracking | Use |
|---|---|---|---|---|---|
| `display-xxl` | Fraunces | 320px / 0.85 | 300 | -0.04em | Footer big-type lockup |
| `display-xl` | Fraunces | 220px / 0.85 | 300 | -0.04em | Hero center word (if used) |
| `display-l`  | Fraunces | 120px / 0.9  | 300 | -0.03em | Section openers, big editorial |
| `display-m`  | Fraunces | 72px / 0.95  | 400 | -0.02em | Subsection titles |
| `display-s`  | Fraunces | 48px / 1.0   | 400 | -0.02em | Card titles |
| `stat-xl`    | Fraunces | 140px / 0.85 | 300 | -0.04em | Oversized numerals |
| `score-hero` | Fraunces | 180px / 0.85 | 300 | -0.04em | Hero score "87" |
| `body-editorial` | Fraunces | 22px / 1.5 | 400 | -0.01em | Centered serif editorial body |
| `body-l`     | Instrument Sans | 20px / 1.5 | 400 | 0 | Lead paragraphs |
| `body-m`     | Instrument Sans | 16px / 1.55 | 400 | 0 | Default body |
| `body-s`     | Instrument Sans | 14px / 1.45 | 400 | 0 | Captions |
| `label`      | Instrument Sans | 12px / 1.2 | 500 | 0.08em | UPPERCASE micro-labels |
| `label-overlay` | Instrument Sans | 11px / 1.2 | 500 | 0.12em | Photo tile caption pills |
| `pill`       | Instrument Sans | 11px / 1.0 | 500 | 0.06em | Feature card pill tags |
| `mono-l`     | JetBrains Mono | 16px / 1.4 | 400 | -0.01em | Spec values, wavelengths |
| `mono-s`     | JetBrains Mono | 12px / 1.3 | 400 | 0 | Footer, meta, timestamps |

### Typographic rules

- Set Fraunces `SOFT` axis to `100` (max soft) for warm editorial display; `0` for spec contexts.
- Numerals in `stat-xl` and `score-hero` are Fraunces regular italic, lining figures (`font-feature-settings: "lnum", "ss01"`).
- Any measurement (`900–1650 nm`, `0.8 s`, `1.4 kg`) is JetBrains Mono, lowercase unit, never italicized.
- Labels (`label` token) are always uppercase, 0.08em tracked.
- Caption labels (`label-overlay`) appear as small uppercase pills in the top-left of photographic tiles: 8px horizontal × 6px vertical padding, 1px hairline border, transparent fill with backdrop-blur.
- Editorial body (`body-editorial`) is centered, max-width 560px, on paper backgrounds. Italic reserved for one emphasized word per paragraph.
- No underlined links by default. Links get a 1px ink underline offset 4px below baseline, fade to gold on hover.
- Do not hyphenate display type.

---

## Spacing

Based on an 8px grid:

```
4   → 4px    (micro)
8   → 8px    (default gap)
16  → 16px   (tight)
24  → 24px   (default)
40  → 40px   (section internal)
80  → 80px   (between subsections)
160 → 160px  (between sections, desktop)
240 → 240px  (before footer big-type)
```

Section rhythm: 160px top/bottom desktop, 80px mobile. Before the big-type footer, pad 240px for extra pause.

---

## Layout

- **Max content width**: 1440px.
- **Gutter**: 40px desktop, 20px mobile.
- **Grid**: 12-column with 24px gutter.
- **Editorial block width**: 640px max (for centered serif body copy).

---

## Motion

Use the Motion library (imports from `motion/react`). Prefer CSS where possible. All motion is slow and deliberate.

```typescript
// Timing tokens
const timing = {
  instant: 120,   // hover states
  gentle:  400,   // defaults, fades
  slow:    800,   // entrances, reveals
  ritual:  1600,  // one-off orchestrations
};

// Easing
const easing = {
  default: [0.2, 0.8, 0.2, 1],
  sharp:   [0.6, 0, 0.3, 1],
  soft:    [0.4, 0.0, 0.2, 1],
};

// Hero animation sequence (seconds)
const heroSequence = {
  dropFall:    1.2,
  impact:      0.15,
  scanPulse:   0.8,
  barReveal:   0.6,
  scoreCount:  1.2,
  hold:        2.5,
  loopReset:   0.5,
};
```

Scroll-triggered reveals: fade up 16px over 800ms, stagger 120ms per element, fire once. Never bounce. The spectral curve motif draws itself in 2.4s once on section entry via stroke-dashoffset animation.

The big-type footer wordmark tracks in from 0.1em to -0.04em over 1800ms when the footer enters viewport.

`prefers-reduced-motion` disables all entrance animations and the hero loop; replaces with the static final frame.

---

## Elevation / surfaces

No drop shadows on primary surfaces.
- Cards on paper: `--color-paper-soft` with 1px hairline border `--color-hairline`.
- Cards on ink: `--color-ink-soft` with 1px hairline border `--color-hairline-gold`.
- Cards on stone: `--color-stone` with a 1px paper-15 inset highlight at top.
- Only the preorder CTA button gets any lift: a 1px gold inset line on hover, not a shadow.

---

## Component primitives

### Button
- **Primary on paper**: ink fill, paper text, 1px gold inset on hover, 48px tall, 24px horizontal padding, JetBrains Mono uppercase 14px, 0.08em tracking. 2px radius max.
- **Primary on ink**: paper fill, ink text, same sizing.
- **Secondary**: transparent, 1px border matching text color. Same sizing.
- **Ghost**: text-only, underline on hover.

### Form fields
- 48px tall, transparent background, 1px ink hairline bottom border only (no side/top borders).
- Label floats above: Instrument Sans 12px uppercase tracked.
- Focus: bottom border becomes gold, no glow.

### Spec table
- Two-column.
- Left: Instrument Sans label, 14px, uppercase tracked.
- Right: JetBrains Mono value, 16px.
- Hairline divider between rows.
- No zebra striping.

### Image tile with caption
- Used in Section 4.5 Moments gallery and Collection cards.
- Image fills container, `object-fit: cover`.
- Caption pill (`label-overlay` token) pinned to top-left, 16px offset.
- On hover (desktop only): 1.02× scale over 800ms ease-out.

### Feature Card

Floating card overlaying the hero at its bottom third. Three cards in a horizontal row on desktop.

**Dimensions**: ~360px wide × ~240px tall desktop, fluid on mobile.

**Visual treatment**:
- Background: `rgba(12, 11, 9, 0.55)` (ink at 55%) with `backdrop-filter: blur(24px) saturate(140%)`.
- Border: 1px `--color-hairline-gold` inset.
- Border radius: 2px.
- Padding: 24px.
- Text color: `--color-paper`.

**Structure (top to bottom)**:
1. Top row — icon (40×40 rounded-square, 2px radius, 1px gold hairline, transparent fill, 24×24 outline icon inside in gold) on the left, and 2–3 pill tags on the right (pill token: gold text at 8% gold fill, 1px gold hairline, 2px radius, 4×10 padding).
2. Breathing space.
3. Title: Fraunces italic, `display-s` token, paper color.
4. Body: Instrument Sans 14px, paper-60, line-height 1.5, 2–3 lines max.

**Hover (desktop)**: background opacity 55% → 70% ink; border shifts to `--color-gold-soft`; subtle 4px upward translate over 400ms.

**Middle-card emphasis**: the middle of three cards uses a 1px full-gold border (not hairline-gold) to signal primary feature.

### Quality Gradient Bar

The color gradient visualization for the Koku Score. Used in the hero animation and again in the Koku Score section.

**Dimensions**: 480×56px (hero); 320×48px (Koku Score section); 100% × 40px (mobile).

**Structure**:
- **Score** on the left: Fraunces italic, `score-hero` token in hero (180px), `stat-xl` in Koku Score (140px), gold.
- **"KOKU SCORE"** label right of the score: Instrument Sans 12px uppercase, 0.1em tracked, gold.
- **Gradient strip** (16px tall hero, 24px section):
  ```css
  background: linear-gradient(90deg,
    var(--grad-quality-0) 0%,
    var(--grad-quality-25) 25%,
    var(--grad-quality-50) 50%,
    var(--grad-quality-75) 75%,
    var(--grad-quality-100) 100%
  );
  ```
  1px inset gold border.
- **Marker**: 2px gold vertical line, 24px tall (extending above and below the strip), with a 10px filled gold circle at the top. Positioned at the score percentage along the strip.
- **Tick labels** beneath the strip: `0 · 25 · 50 · 75 · 100`, Instrument Sans 10px uppercase tracked, paper-40 on ink / ink-40 on paper.
- **Stop category labels** (optional): `OXIDIZED · CULINARY · USUCHA · CEREMONIAL · KOICHA`. Pill token, paper-70 on the gradient strip.

**Animation**:
- Bar fades + translates in from right (40px offset) over 600ms ease-out.
- Marker slides from 0 to final position over 1200ms, ease-out.
- Score counts up from 00 using tabular JetBrains Mono during count, crossfades to Fraunces italic final frame at t+1200ms.

**Responsive**: on mobile, score stacks above the bar; bar becomes full-width.

### Icon system

Minimal, line-based, 1.5px stroke, round caps/joins. Single color. Never filled (exception: the 4px chawan-bullet dot).

Three hero icons needed:
- **Scan icon**: horizontal bracket with three stacked hairlines between — spectral readout. 24×24, gold stroke.
- **Gauge icon**: 3/4 arc with a needle — scoring. 24×24, gold stroke.
- **Tin icon**: cylinder outline with a lid notch — matcha tin. 24×24, gold stroke.

---

## Decorative / brand motifs

### NIR spectral curve

Single stroke, 1.5px, `--color-gold`. ViewBox `0 0 1600 200`. Plots a characteristic NIR reflectance with three broad absorption dips around x=400 (980nm, water), x=880 (1200nm, C-H), x=1320 (1450nm, water + O-H). Used as:
- Faint hero watermark (8% opacity, behind the headline block).
- Full-width divider between certain sections (animated draw-in).
- Visual backbone of the Koku Score section.

### Wavelength rail

A thin horizontal ruler showing 700nm → 1650nm with labeled ticks at 900, 1200, 1450, 1650. JetBrains Mono 11px. Appears once, above or below the spectral curve in the Koku Score section.

### Chawan-inspired punctuation

Small filled circle (4px, gold) used as bullet / list marker. Echoes the dome profile.

### Big-type wordmark

The word `platôme` at `display-xxl` (320px Fraunces, weight 300, SOFT 100), used once in the footer. Aligned to the left gutter. The top ~30% of each letter extends up into the preceding section's negative space, cropping across the paper/ink transition. Implemented with a clip-path mask or two stacked copies (ink-colored on paper, paper-colored on ink).

### Spectral flash (hero animation only)

At the moment of matcha-drop impact, the sensor window emits a brief spectral flash: a 250ms radial gradient burst cycling through a compressed visible-spectrum palette (violet → blue → green → yellow → red → gold). Absolutely-positioned SVG element over the hero media, tied to the sensor window position in the composition.

---

## Accessibility

- Minimum contrast: 4.5:1 body / 3:1 large display. Verified pairs: ink-on-paper 14.6:1, gold-on-ink 4.8:1, ink-on-stone 4.2:1 (display-size only, not body).
- All interactive targets ≥44×44px.
- `prefers-reduced-motion` disables entrance animations and the hero loop — shows only the static final composition.
- No autoplay video except the hero (muted, loop, playsinline). The hero must have a descriptive ARIA label: *"The Platôme matcha dome scans a bowl of matcha and displays a Koku Score of 87 out of 100."*
- No parallax deeper than 20px.

---

## What this design system is NOT

- Not "premium" black-and-gold luxury cliché.
- Not startup-minimalist (Inter, rounded corners, pastel gradients).
- Not Apple-derivative.
- Not wellness-DTC (warm pink, hand-drawn illustrations, affirmations).
- Not instrument-cold — the tone is explicitly warmth, restraint, and editorial calm.

It's a ceremonial instrument brand, executed like Kinfolk would execute it if Kinfolk made instruments.
