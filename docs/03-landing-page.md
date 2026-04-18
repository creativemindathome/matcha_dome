# 03 · Landing Page Spec

One page, long scroll, 11 content sections + announcement + header. Each section below has precise layout, copy, motion, and image/asset dependencies.

Asset keys (`IMG-XX`, `HERO-VIDEO-01`) reference prompts in `04-image-manifest.md`.

---

## Section 0 · Announcement bar

**Height**: 36px · **Background**: `--color-ink` · **Text**: `--color-paper` · **Accent**: `--color-gold`

Content:

```
[gold dot] FIRST 500 PREORDERS INCLUDE 12 MONTHS OF CEREMONIAL-GRADE MATCHA → reserve yours
```

- JetBrains Mono 12px uppercase, 0.08em tracking.
- Gold dot is the chawan-bullet motif.
- Entire bar clickable, scrolls to preorder section.
- Does not stick on scroll.

---

## Section 1 · Header / Nav

**Height**: 80px · **Background**: transparent over hero → paper-soft with 1px hairline bottom border after hero exits · **Position**: sticky

```
[platôme ◦ matcha]                  [the instrument]  [the tea]  [science]  [reserve]
```

- Brand mark left: lowercase `platôme ◦ matcha` in JetBrains Mono 14px. Gold dot separator between wordmark and sub-brand lockup.
- Right nav: 4 links, Instrument Sans 14px uppercase, 0.08em tracking.
- "reserve" link emphasized with a gold underline.
- No search, no cart, no login.

---

## Section 2 · Hero (cinematic animated)

**Full-bleed, 100vh min 720px, background `--color-ink`**

A single cinematic video hero occupying the full viewport, with an animated product-demo sequence overlaid in the center and three feature cards floating at the bottom third.

### Visual layer stack (bottom to top)

**Layer 0 — Hero media**: a looped video background (`HERO-VIDEO-01`) showing the atmospheric scene — the dome, cherry blossoms, golden god-rays, airborne matcha dust, and the matcha drop falling in slow-motion. 8-second loop, autoplay muted playsinline, H.265 + AV1 + WebM sources with a poster frame fallback for initial load and reduced-motion. If the video hasn't yet been generated, the layer falls back to the 8K still poster (`HERO-POSTER-01`).

```html
<video autoplay muted loop playsinline poster="/images/hero/hero-poster.webp">
  <source src="/images/hero/hero.av1.mp4" type="video/mp4; codecs=av01" />
  <source src="/images/hero/hero.hevc.mp4" type="video/mp4; codecs=hvc1" />
  <source src="/images/hero/hero.webm" type="video/webm" />
</video>
```

**Layer 1 — Subtle dark vignette**: a CSS radial gradient at 0–40% opacity, darkening the corners slightly, keeping overlaid type readable. Applied in CSS, not baked into the video.

**Layer 2 — Headline block** (top-left, positioned 10% from left, 25% from top):
- Preamble label: `— PLATÔME MATCHA` · Instrument Sans 12px uppercase tracked 0.2em, gold.
- Headline: `The light reads` (line 1) `what the label doesn't.` (line 2) — Fraunces italic, weight 300, ~96px / line-height 0.95, paper color, max-width 640px.
- Subhead (body-l, paper-60, max 480px): `A ceremonial spectrometer for matcha. Thirty-two compounds, five dimensions, one score — measured at the point of the bowl.`

**Layer 3 — UI animation overlay**: DOM/SVG elements layered on top of the video, tied to the video's `currentTime` event. See §Animation Sequence below.

**Layer 4 — Feature cards** (floating at bottom, 3-column row, 40px from bottom edge). See §Feature Cards below.

**Layer 5 — Scroll indicator** (bottom-center, 1px × 40px gold vertical line, slow downward pulse). Fades in at the end of the hero animation sequence.

### Animation sequence (video + CSS overlay)

The hero video (`HERO-VIDEO-01`) covers phases 0–3 (rest, drop appears, drop falls, impact). The CSS overlay covers phases 4–9 (spectral flash, gradient bar reveal, score count, hold, loop reset). Both loop on the same 8-second cycle, synchronized via `timeupdate` listener on the video element.

| Phase | Video time (s) | Source | What happens |
|---|---|---|---|
| 0 · Rest | 0.0 → 0.3 | Video | Atmospheric establishing frame. Dome sits, god-rays stream, matcha dust drifts. |
| 1 · Drop appears | 0.3 → 0.5 | Video | Single matcha droplet fades in above the dome's sensor window. |
| 2 · Drop falls | 0.5 → 1.7 | Video | Droplet falls in slow-motion with gentle trail and squash. |
| 3 · Impact | 1.7 → 1.85 | Video | Droplet lands on the crown. Surrounding matcha powder blasts outward in a soft explosion. |
| 4 · Spectral flash | 1.85 → 2.1 | Overlay | CSS-driven: 250ms radial gradient burst at the sensor window position, cycling violet → blue → green → yellow → red → gold. Simultaneously, the gold bevel ring pulses via CSS box-shadow glow (`0 0 0` → `0 0 40px` gold → `0 0 0` over 600ms). |
| 5 · Bar reveal | 2.5 → 3.1 | Overlay | Quality Gradient Bar component slides in from the right edge, 600ms translate-from-right + fade. |
| 6 · Marker travels | 3.1 → 4.3 | Overlay | Gold marker slides along the bar from 0 to position 87. Score counts up from 00 to 87 using tabular JetBrains Mono digits, synchronized via `requestAnimationFrame`. |
| 7 · Score snap | 4.3 → 4.6 | Overlay | Score crossfades from JetBrains Mono tabular to Fraunces italic final frame (design token `score-hero`). Tiny gold dot flashes at marker position. |
| 8 · Hold | 4.6 → 7.0 | Both | Everything holds. Dome settled, bar visible, "87" displayed, headline still visible. This is the poster frame and the reduced-motion static state. |
| 9 · Loop | 7.0 → 8.0 | Both | Overlay elements fade out. Video loops back to 0.0. Next cycle begins. |

**Headline motion** (runs once on page load, not on loop):
1. `t=0`: video playing, paused at 0s until load.
2. `t=600ms`: preamble label fades in, 400ms.
3. `t=800ms`: headline fades up from 16px offset, 1000ms, Fraunces italic tracks in from 0.02em to -0.02em.
4. `t=1800ms`: subhead fades in, 600ms.
5. `t=2400ms`: three feature cards animate up from 40px below their final position, staggered 120ms, 800ms each.
6. `t=3200ms`: video starts playing from 0s and first overlay sequence begins.

### Feature cards (3 cards floating at bottom)

Positioned `position: absolute; bottom: 40px; left: 40px; right: 40px`, arranged as a horizontal 3-column grid with 24px gaps. On viewport <1024px, stack to 1 column below the hero video (pushing hero to its natural height).

Each card uses the Feature Card component from `02-design-system.md`.

#### Card 1 · Read the leaf
- Icon: **Scan icon** (spectral bracket, gold stroke)
- Tags: `32 COMPOUNDS` · `0.8 S` · `900–1650 NM`
- Title: *Read the leaf* (Fraunces italic)
- Body: `The NIR sensor finds what the label doesn't. Moisture, theanine, oxidation, origin signature — captured in under a second.`

#### Card 2 · Score the ceremony *(emphasized — 1px full-gold border)*
- Icon: **Gauge icon**
- Tags: `5 DIMENSIONS` · `0–100` · `FARM SIGNATURE`
- Title: *Score the ceremony*
- Body: `Every bowl earns a Koku Score. Freshness, theanine, grade, farm signature, ceremony readiness — five honest dimensions, one unified number.`

#### Card 3 · Pair with the tea
- Icon: **Tin icon**
- Tags: `4 GRADES` · `SINGLE-ESTATE` · `PRE-REGISTERED`
- Title: *Pair with the tea*
- Body: `Monthly tins from Uji, Kagoshima, and Wazuka arrive with their spectral signature pre-registered. Your dome already knows what's inside.`

### Composition alignment (for CSS positioning)

The hero media is composed so the dome sits at approximately `x:65%, y:55%` of the frame and the sensor window (crown center) sits at `x:65%, y:38%`. The lower-left quadrant (`x:8–45%, y:25–55%`) is deliberate negative space for the headline overlay.

All animation overlay elements (impact flash, spectral burst, bevel pulse, gradient bar) are positioned in percentages of the hero container so they align with the media composition regardless of viewport size.

### Mobile adaptation

Mobile (≤768px):
- Hero shrinks to 100svh (safe viewport height).
- The video/poster swaps to a portrait-oriented 4:5 crop (`HERO-VIDEO-01-MOBILE` / `HERO-POSTER-01-MOBILE`) that re-centers the dome as the central subject.
- Headline moves to top-center, smaller scale (`display-m` at 56px).
- Animation sequence still runs at reduced scale.
- Feature cards move below the hero into a full-width vertical stack.

---

## Section 3 · Editorial opener

**Background**: `--color-paper` · **Padding**: 160px vertical, centered

Pure-type section. One centered editorial paragraph.

### Content

- Preamble label: `— ON MATCHA` · Instrument Sans 12px uppercase tracked 0.2em, gold, 40px above the paragraph.
- Main editorial body (`body-editorial` token, centered, max 560px, ink):

> Platôme Matcha sources ceremonial-grade tea from small single-estate farms in Uji, Kagoshima, and Wazuka — and pairs it with a *spectrometer* that reads every bowl you brew. Designed for people who care what the light finds in their tea.

- Italic on *spectrometer* (one emphasized word).
- 80px breathing room, then the animated spectral curve motif (gold, 1.5px, 2.4s draw-in on scroll-into-view, full content width).

### Motion

Paragraph fades up 16px over 800ms on scroll-into-view. Spectral curve draws afterward.

---

## Section 4 · How it works (the ritual)

**Background**: `--color-paper-soft` · **Padding**: 160px vertical

Horizontal 3-column grid (desktop). Each step has a number, a portrait image with caption pill, a title, and a spec line.

| # | Image key | Caption pill | Title | Spec line |
|---|---|---|---|---|
| 01 | `IMG-05 ritual_place_chawan` | PLACE | Place the chawan. | weight detected · 0.2s |
| 02 | `IMG-06 ritual_whisk_matcha` | WHISK | Whisk with a chasen. | no change to tradition |
| 03 | `IMG-07 ritual_lower_dome` | LOWER | Lower the dome. | 32 compounds · 0.8s scan |

Each step:
- Vertical number (01, 02, 03) in Fraunces italic 72px, `--color-koicha`.
- 4:5 portrait image with caption pill (`label-overlay` token) in top-left.
- Below image: title (Fraunces 32px, ink) + spec (JetBrains Mono 14px, ink-60).

### Motion

Each step fades up 800ms on scroll-into-view, staggered 200ms. Number counts from 00 to final in 600ms synchronized with image fade.

---

## Section 4.5 · Moments

**Background**: `--color-paper` · **Padding**: 120px vertical

A photographic breathing interlude. Three 1:1 images in a horizontal row, 24px gap.

Above the row, a small quiet label: `— IN DETAIL` (gold, tracked, centered).

| Position | Image key | Caption pill |
|---|---|---|
| Left | `IMG-01 matcha_macro_froth` | MATCHA |
| Center | `IMG-03 matcha_chawan_pour` | POUR |
| Right | `IMG-04 dome_sensor_macro` | SENSOR |

Each tile uses the Image Tile with Caption component. No titles, no additional copy. 1.02× zoom on hover (desktop).

### Motion

Images fade up on scroll with 120ms stagger.

---

## Section 5 · The device

**Background**: `--color-paper` · **Layout**: 2-column asymmetric, 7/5 split

**Left column (60%)**: hero product shot on a warm stone-deep seamless (`IMG-08 dome_hero_studio`). Image has ~40% negative space around the product.

**Right column (40%)**: vertically centered content block.

### Content block

```
───
THE INSTRUMENT

A spectrometer
the size of
a tea bowl.

Seventeen centimeters wide. Eighteen tall.
One kilogram four hundred grams in the hand.
No buttons. No screen. Seat the dome on its
base and the light begins.

───────────────────────── SPECIFICATIONS

  DIAMETER          170 mm
  HEIGHT            180 mm
  WEIGHT            1.42 kg
  DOME              zero-gloss ceramic composite
  BASE              bead-blasted anodized aluminum
  BEVEL             PVD 18k gold
  SENSOR            TriEye TES200 Ge-on-Si
  RANGE             900–1650 nm
  RESOLUTION        10 nm
  SCAN TIME         0.8 s
  CONNECTIVITY      BLE 5.3
  POWER             USB-C, 5V 2A

───
```

### Precise treatment

- Label `THE INSTRUMENT` — Instrument Sans 12px uppercase, tracked 0.2em, gold, preceded by a 24px gold hairline.
- Headline — Fraunces 72px, weight 400, ink, italic on the word *spectrometer*.
- Paragraph — Instrument Sans 18px, ink-60, max-width 400px.
- Spec table — JetBrains Mono. Left label 14px ink-60, right value 14px ink. 1px `--color-hairline` divider between rows. Row height 40px.
- Label `SPECIFICATIONS` — same treatment as `THE INSTRUMENT`.
- Closing 24px gold hairline at the bottom.

### Motion

Subtle parallax on product image (0.9× page speed, max 40px displacement). Spec table rows reveal with 80ms stagger on scroll-into-view.

---

## Section 6 · The Koku Score

**Background**: `--color-matcha-bg` · **Padding**: 160px vertical

The one intentionally dramatic green section. Centered composition, max 1200px.

### Content

1. Label `— SCORING` (gold, tracked).
2. Headline: `Five dimensions. One number.` — Fraunces 96px, paper.
3. Lead paragraph (body-l, paper-60, max 640px):
   > Every scan produces a full NIR reflectance spectrum. We decompose it into five dimensions — Freshness, Theanine, Grade, Farm Signature, Ceremony Readiness — and surface a single Koku Score from 0 to 100.
4. **Composite visual**, approx 480px tall:
   - Top: spectral curve (gold 2px), dashed hairlines at 900/1200/1450/1650nm.
   - Middle: Quality Gradient Bar (section size, 600px wide), with score `87` in Fraunces italic 140px (`stat-xl`) to the left.
   - Bottom: 5-dimension breakdown, compact table in JetBrains Mono (label paper-60, value paper, hairline-gold dividers, 2×5 two-column layout):
     ```
     FRESHNESS           92
     THEANINE            78
     GRADE               91
     FARM SIGNATURE      85
     CEREMONY READINESS  89
     ```
5. Bottom line: `Sample: Uji-origin usucha, scanned 17/04/2026` · JetBrains Mono 14px, paper-40, lowercase.

### Motion

Spectral curve draws 2.4s → gradient bar reveals from right 600ms → marker slides to 87 over 1200ms synchronized with score count → 5-dimension rows reveal 80ms stagger.

---

## Section 7 · The Collection

**Background**: `--color-paper` · **Padding**: 160px vertical

Three-image category gallery. Equal-width cards, horizontal on desktop, stacked on mobile.

### Section intro (centered)

- Label: `— THE COLLECTION`
- Title: `Three parts of one ritual.` — Fraunces 72px, ink, centered.
- Sub (body-editorial, max 520px): *The instrument, the tea, and the implements — each considered, each available on its own or together in a starter kit.*

### Cards

| Card | Image key | Label pill | Name | Sub-line |
|---|---|---|---|---|
| 1 | `IMG-12 collection_instrument` | THE INSTRUMENT | The Dome | Spectrometer · €649 |
| 2 | `IMG-13 collection_tea` | THE TEA | Monthly Matcha | From €24/month |
| 3 | `IMG-14 collection_ritual` | THE RITUAL | Kit & Implements | Chawan, chasen, chashaku |

Each card:
- 4:5 portrait image with caption pill top-left.
- Bottom footer strip (~80px tall, paper background, hairline top border):
  - Name: Fraunces 28px italic, ink.
  - Sub-line: Instrument Sans 14px, ink-60.
  - Ghost link: `explore →` in gold.

### Motion

Cards fade up 120ms stagger. Hover: image 1.02× over 800ms.

---

## Section 8 · The matcha (subscription)

**Background**: `--color-paper-soft` · **Padding**: 160px vertical

Four subscription tier cards.

### Section intro

- Label: `— THE TEA`
- Title: `Paired matcha, monthly.` — Fraunces 72px, ink.
- Sub (body-l, ink-60, max 560px): `Each tin arrives with a spectral signature pre-registered to your device. Open, scan, verify.`

### Tier cards

| Tier | Farm | Grade | Monthly grams | Price/month |
|---|---|---|---|---|
| APPRENTICE | Kagoshima blend | Culinary+ | 40g | €24 |
| ARTISAN | Uji single-estate | Ceremonial | 30g | €42 |
| CEREMONIAL | Wazuka (Kyoto) | Competition | 20g | €68 |
| MASTER | Rotating competition lots | Koicha-grade | 20g | €98 |

Each card:
- 4:5 portrait, paper background, 1px hairline border.
- Top: grade name in Fraunces italic 40px ink, followed by Instrument Sans 14px ink-60 paragraph on farm + harvest.
- Bottom: sub-table (mono 13px) — farm / cultivar / harvest / stone-milled.
- Price: Fraunces italic 48px, koicha green, bottom-right.
- Select button: ghost style, `choose this tier →`.

Cards fade up with 120ms stagger on scroll.

---

## Section 9 · Preorder CTA

**Background**: full-bleed `--color-ink` with a large background image at 15% opacity (`IMG-11 hands_over_dome_ambient`) · **Padding**: 200px vertical

Single centered block.

- Label: `— RESERVE` (gold, tracked).
- Headline: `The first five hundred.` — Fraunces 120px italic, paper, centered.
- Sub (body-l, paper-60, max 640px centered): `A €149 deposit reserves your dome and your first twelve months of matcha. Full €649 balance due when we ship, Q4 2026.`
- Three-icon row (mono 12px labels): `FULL REFUND ANY TIME · LIMITED TO 500 · DELIVERED Q4 2026`.
- Primary button: `RESERVE YOUR DOME · €149` — 64px tall (oversized for this CTA), paper fill on ink, ink text.
- Ghost link: `or join the waitlist without paying →`.

### Motion

Headline tracks in from 0.08em to -0.03em over 1400ms on scroll-into-view.

---

## Section 10 · FAQ

**Background**: `--color-paper` · **Padding**: 160px vertical

Accordion, single column, max 800px centered. 8 questions.

1. How accurate is it?
2. Do I have to use Platôme matcha, or does it work with any matcha?
3. Why does it take 0.8 seconds to scan?
4. What happens to my data?
5. Can I cancel the subscription and keep the dome?
6. When do you ship?
7. What does it cost at full retail?
8. I'm a chef / tea house / retailer — can I get wholesale?

Closed state: Fraunces 28px ink, gold `+` marker on the right. Hairline below.
Open state: `+` rotates 45° to `×`. Answer reveals with height animation 400ms. Answer is Instrument Sans 16px ink-60, max 600px.

Full answers drafted inline with the developer during build, or pulled from a separate `/docs/faq-answers.md`.

---

## Section 11 · Footer

**Background**: transitions `--color-paper-soft` → `--color-ink` · **Padding**: 240px top, 80px bottom

Two stacked zones.

### Zone A — Newsletter + navigation (on paper-soft)

Horizontal 4-column layout:

```
SIGN UP FOR JOURNAL           SHOP              PLATÔME         STAY IN TOUCH
(occasional dispatches        The Instrument    About            Instagram
 from the lab)                The Tea           Press            Substack
                              The Kit           FAQ              
[ email                 →]    All                                
```

- Column 1: label `SIGN UP FOR JOURNAL`, body-s sub-line, minimal email input (bottom-border only, `→` submit in gold).
- Columns 2–4: labels in `label` token, links in Instrument Sans 14px ink.
- Hairline divider below the four-column block.

### Zone B — Big-type lockup (crops paper → ink)

Full-width editorial gesture. The word **`platôme`** in `display-xxl` (Fraunces 320px, weight 300, SOFT 100), aligned to the left gutter. The top ~60% of each letter sits on paper-soft; the bottom ~40% extends into an ink band at the viewport bottom. Two-tone effect via clip-path mask or two stacked copies.

Below the big-type, inside the ink band:
- `© 2026 platôme ltd · made between london, munich, and uji · all measurements honest` — JetBrains Mono 12px paper-40.

### Motion

`platôme` tracks in from 0.1em to -0.04em over 1800ms when footer enters viewport. Fires once.

---

## Cross-section rules

- Dividers on paper backgrounds: 1px `--color-hairline`, spanning content width (not viewport edge-to-edge), with ≥80px space above and below.
- Dividers on ink backgrounds: 1px `--color-hairline-gold`.
- Every section opener label is preceded by a 40px-wide horizontal line matching the label color (`— LABEL` pattern).
- No section uses more than one font weight per token size.
- Photography is the only source of green outside the Koku Score section.
- Stone colors appear only inside image compositions, never as section backgrounds.
- The Quality Gradient Bar is the canonical quality-score visualization; do not invent alternatives elsewhere.
