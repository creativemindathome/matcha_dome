# 04 · Image & Video Manifest

All visual assets on the site, with generation prompts tuned for **Google Veo 3** (video hero), **Midjourney v6.1**, and **Flux 1.1 Pro** (stills). Each asset has:

- A unique key (referenced from `03-landing-page.md`).
- The physical subject specification.
- A generation prompt.
- A negative prompt.
- Target aspect ratio and resolution.
- Iteration guidance.

---

## Canonical: the Matcha Dome physical spec

**Fix this in mind before generating anything. Every dome render in every asset must match these proportions and material cues. Deviations will make the site incoherent.**

### Physical specification

- **Overall dimensions**: 170mm diameter × 180mm tall (about the size of a large cookie jar).
- **Weight**: 1.42 kg.
- **Two-piece construction**: aluminum base + ceramic dome. Dome lifts off cloche-style.

**Base (bottom 50mm)**:
- Material: bead-blasted anodized aluminum, matte black (`#1A1815`).
- Profile: slight taper — 170mm at bottom, 165mm at the top edge where it meets the dome.
- Seam detail: a single **1mm polished PVD gold pinstripe** (`#C9A45C`) running the full circumference. **Hairline, not a chunky band.**
- Front face: laser-etched lowercase `platôme` in JetBrains Mono, 4mm tall, slightly recessed. Only external branding.
- Back: recessed USB-C port, 6 narrow vent slits.
- Bottom: 4 matte-black silicone feet, barely visible.
- Top surface inside the dome: a 120mm concentric recess (load cell platform), matte white, 1mm lower than surrounding.

**Dome (upper 130mm)**:
- Material: zero-gloss ceramic composite, color warm off-white `#EDE8DF`, unglazed-porcelain feel.
- **Profile**: catenary curve — **wider at the base, tapered flatter toward the crown**. Reference silhouette: inverted tea bowl, Kamakura Daibutsu. **NOT a hemisphere. NOT cylindrical.**
- Wall thickness: 5mm.
- **No branding, no openings, no windows anywhere on the dome's side surfaces. The exterior is unbroken matte ceramic.**
- Lifts straight off the base. No hinges, threads, or latches.

**Crown (top 40mm flat disc of the dome)**:
- A 50mm-diameter disc **recessed 2mm into the dome's flat top**. Flush with the dome surface, not a protruding knob.
- Material: charcoal ceramic `#2C2A27` — subtly darker than the dome body, matches the base color family.
- Dead center: a 20mm-diameter **sensor window** — obsidian-black glass (matte, not reflective), with a 1mm polished gold inner bezel.
- **No buttons, no lights, no markings on the crown surface.**

**LED ring** (at the gold pinstripe seam): during scanning, a 1mm-wide warm-gold LED glow line appears just above the gold bevel; invisible at rest.

**Interior (when dome lifted)**: matte white ceramic lining, 16 warm-white LEDs recessed around the inner rim ~30mm above the platform, sensor window looking down with gold bezel visible.

### Non-negotiable visual rules — violate any of these and re-generate

- **No screens. No visible buttons. No branding on the dome.** Only the 4mm wordmark on the base front.
- **The gold at the seam is a 1mm hairline pinstripe, not a thick band.** If the render shows a 5mm+ gold ring, regenerate.
- **No windows, viewfinders, or rectangular openings on the dome's sides.** The dome is an unbroken matte ceramic surface.
- **No knob, button, or protrusion on the crown.** The crown is a flush disc with a flat 20mm sensor window in the center.
- **Catenary profile, not a hemisphere.** Widest at the bottom third, tapering soft to a flat crown. If the render looks like a perfect dome, regenerate.
- **Zero-gloss matte ceramic, never reflective.** If the dome shows highlights or environmental reflections, regenerate.

### Reference sentence (paste verbatim into every dome prompt)

> "a ceremonial matcha spectrometer: a warm-off-white zero-gloss ceramic matte dome with a catenary tapered-inverted-tea-bowl profile (wider at base, flat crown), 170mm wide by 180mm tall, with a perfectly unbroken matte ceramic surface on all sides, seated on a bead-blasted matte-black anodized aluminum base separated by a single 1mm polished gold hairline pinstripe ring, a flush recessed charcoal-ceramic disc on the flat top housing a 20mm obsidian-glass sensor window with a 1mm gold bezel, no screens no buttons no logos no front windows no knobs"

---

## HERO-VIDEO-01 · The cinematic hero video (Veo 3)

**Purpose**: full-bleed background video for Section 2 of the landing.
**Aspect ratio**: 16:9, 1920×1080 source (upscale to 3840×2160 if Veo Pro is available).
**Duration**: 8-second loop, seamless.
**Audio**: stripped (muted).
**Output format**: master as H.264 MP4, then encode to H.265/AV1/WebM for web.

### The shot

A cinematic slow-motion 8-second establishing shot. The matcha dome sits on a dark brushed-stone slab at the right-third of the frame. Warm golden volumetric god-rays stream in from the upper-right, catching airborne matcha dust particles like stardust. Cherry blossom branches hang defocused in the upper-left corner. The lower-left quadrant stays dark and empty for the headline overlay. Around the base of the dome, vivid bright-green matcha powder is spilled across the stone like fresh snow.

Over 8 seconds: a single syrupy vivid-green drop of matcha forms in the air above the center of the dome, falls in slow motion, lands on the flush crown, and at the moment of impact the matcha powder around the base blasts outward in a soft explosion mingling with the god-rays.

### Veo prompt

```
Cinematic slow-motion 8-second shot, 16:9 ultrawide, static camera. 
A ceremonial matcha spectrometer sits on a dark brushed-stone slab 
at the right-third of the frame: a warm-off-white zero-gloss matte 
ceramic dome with a catenary inverted-tea-bowl profile — wider at 
its base than at its flat crown — with a perfectly unbroken matte 
ceramic surface on all sides (no openings, no windows, no viewfinder). 
A single 1mm-thin polished gold hairline pinstripe marks the seam 
where the dome meets its matte-black anodized aluminum base. The 
flat crown of the dome has a small flush recessed charcoal-ceramic 
disc in its center with a 20mm obsidian-glass sensor window; there 
is no knob, button, or protrusion on top.

Background: warm near-black atmospheric void fading to depth. From 
the upper-right, warm golden volumetric god-rays stream down, 
catching airborne matcha dust particles suspended in the light beam 
like green stardust. Cherry blossom branches with three small 
blossoms hang gently defocused in the upper-left corner. The 
lower-left quadrant is dark, empty, and quiet. Around the base of 
the dome, vivid bright-green matcha powder is spilled across the 
dark stone like fresh snow.

Action over 8 seconds: a single syrupy vivid-green drop of liquid 
matcha materializes in the air directly above the center of the 
dome at 2 seconds, falls in beautiful slow-motion rotation with a 
fine trailing smear, and at 6.5 seconds lands precisely on the flush 
center of the crown. At the moment of impact, the matcha powder 
surrounding the base blasts outward softly in a slow-motion 
explosion, mingling with the golden light rays and the airborne 
dust.

Mood: reverent, cinematic, ceremonial. Kinfolk magazine meets Denis 
Villeneuve cinematography meets Rinko Kawauchi stillness. Arri 
Alexa LF, 50mm, f/2.8, 120fps for slow-motion capture feel. Warm 
black and deep matcha green color grade with single gold highlight 
accent. Film grain. No text, no logos, no UI, no captions.
```

### Veo negative cues (include in the prompt or separately if Veo supports)

- **No front window on the dome. No viewfinder. No rectangular opening on the side.**
- **No knob, button, or protrusion on top of the dome.**
- **No thick gold band. The gold pinstripe must be a 1mm hairline only.**
- **No hemispherical dome. The profile must be wider at the base than at the top, tapering flat.**
- **No glossy reflections on the ceramic.**
- **No text, logos, watermarks, UI overlays, or captions.**
- **No harsh studio lighting — only the single volumetric god-ray from upper-right.**

### Iteration protocol

Expect 3–6 Veo generations to get the dome geometry correct while keeping the atmosphere. The atmosphere (god-rays, blossoms, dust, drop) generates reliably; the dome drifts toward generic appliance/teapot/hemisphere forms. Explicit negatives ("no front window", "no knob", "no hemisphere", "wider at base than at top") are the critical leverage.

If Veo cannot lock the dome geometry after 6 attempts, fall back to:
- Keep the atmospheric video for the backdrop; in post (After Effects or DaVinci), matte-replace the dome region with a 3D-rendered or AI-generated still dome at the correct geometry, composited back into the scene with matching light and shadow.
- Or commission a short 3D render just for the dome and composite into the Veo atmosphere.

### Post-generation pipeline

1. Receive the ~1080p clip from Veo.
2. Upscale to 4K using Topaz Video AI (Artemis or Iris model).
3. Color-grade in DaVinci Resolve: lift shadows +0.03, warm-shift highlights, saturate the matcha-green region locally.
4. Trim to seamless 8-second loop. Apply a 500ms crossfade dissolve at the join if the return-to-start isn't perfectly seamless.
5. Export:
   - **Master**: ProRes 422 HQ, 3840×2160, 30fps.
   - **Web H.265**: 3840×2160, 30fps, CRF 23, target ~8Mbps.
   - **Web AV1**: 3840×2160, 30fps, CRF 28, target ~4Mbps.
   - **Web WebM/VP9**: 1920×1080, 30fps, target ~5Mbps.
6. Extract a single frame at the "hold" moment (~6.5s, post-impact) as `HERO-POSTER-01` — used as `<video poster>`, LCP image, and reduced-motion fallback.

### File outputs

```
/public/images/hero/hero.av1.mp4
/public/images/hero/hero.hevc.mp4
/public/images/hero/hero.webm
/public/images/hero/hero-poster.webp
/public/images/hero/hero-poster.avif
```

---

## HERO-VIDEO-01-MOBILE · Mobile portrait hero

**Purpose**: portrait variant of the hero video for mobile viewports ≤768px.
**Aspect ratio**: 4:5, 1080×1350.
**Duration**: same 8-second loop.

Generate using the same Veo prompt with two modifications:
- "4:5 portrait aspect ratio"
- "dome centered horizontally at 50%, vertically at 65%, with negative space in the upper third for headline overlay"

Extract poster frame as `HERO-POSTER-01-MOBILE`.

---

## HERO-POSTER-01 · 8K still hero (also serves as video poster)

**Purpose**: used as the `<video poster>` attribute for initial paint before the video loads, as the LCP-optimized image, and as the static frame displayed under `prefers-reduced-motion`.
**Aspect ratio**: 16:9, 8192×4608 (true 8K).
**Format**: AVIF primary (quality 92), JPEG fallback.

If you extract from the Veo video, the frame at ~6.5s (post-impact hold) is ideal. If you generate separately (for higher resolution than the video supports), use the Midjourney prompt below.

### Midjourney prompt (only if generating separately)

```
cinematic hero photograph, 16:9 ultrawide frame, warm near-black 
atmospheric depth fading to void in the background, [REFERENCE 
SENTENCE] positioned at the right third of the composition (65% 
horizontal, 55% vertical) sitting on a dark brushed stone slab, 
drifts of vivid brilliant-green matcha powder spilled around the 
base of the dome like fresh green snow, fine airborne matcha 
particles suspended in a warm golden shaft of light entering from 
upper right like volumetric god-rays, cherry blossom branch with 
three small blossoms hanging defocused from upper left background, 
deliberate empty dark negative space in the lower-left quadrant, 
gold bevel ring on the dome catches a single thin highlight, 
obsidian sensor window remains dark, zero reflections on the 
ceramic dome, film grain, cinematic Arri Alexa LF 65mm, f/2.8, 
Denis Villeneuve crossed with Rinko Kawauchi stillness, warm black 
and deep green color grade with single gold highlight accent, no 
text, no watermarks, 8k detail --ar 16:9 --s 300 --style raw 
--v 6.1
```

**Critical composition**: dome at x:65%, y:55%. Sensor window at x:65%, y:38%. Lower-left (x:8–45%, y:25–55%) is empty negative space for the headline overlay.

### Upscale pipeline

Midjourney max output → Magnific or Gigapixel → 8K master → AVIF + JPEG exports.

### File outputs

```
/public/images/hero/hero-poster.avif
/public/images/hero/hero-poster.jpg
/public/images/hero/hero-poster-1920.webp
```

---

## IMG-01 · matcha_macro_froth

**Purpose**: Section 4.5 Moments gallery, left tile.
**Aspect ratio**: 1:1, 2048×2048.

```
ultra macro top-down photograph of freshly whisked usucha matcha 
in a traditional black-glazed chawan, vivid bright green surface 
with a dense field of fine microfoam, shallow depth of field 
focused on the foam bubbles at dead center, natural daylight from 
a north-facing window, subtle steam rising, editorial food 
photography Kinfolk style, Hasselblad 100mm macro, f/4.5, vivid 
matcha green tones (#7A9F3A to #4A6B3A), dark chawan edges just 
visible at frame corners, warm golden light glint at bottom right 
catching one bubble, tight composition filling the frame, no text 
in frame, no hands in frame --ar 1:1 --s 250 --v 6.1
```

**Negative**: `no text, no hands, no whisk, no wooden surfaces, no saturated phone-photo look`.

---

## IMG-03 · matcha_chawan_pour

**Purpose**: Section 4.5 Moments gallery, center tile.
**Aspect ratio**: 1:1, 2048×2048.

```
high-speed editorial food photograph of vivid bright green matcha 
powder falling in a fine stream from a bamboo chashaku scoop held 
at the top right of frame, descending into the deep black interior 
of a traditional iron-glazed chawan at the bottom left, diagonal 
composition, tack-sharp focus on the powder stream, shallow depth 
of field blurring the chashaku, dramatic side lighting from camera 
left creating volumetric texture in the falling powder, rich 
contrast between brilliant green powder and dark bowl interior, 
studio seamless backdrop in warm off-white (#EDE8DF), Hasselblad 
H6D-100c, 100mm macro, 1/2000 shutter, David Loftus style, no 
hands in frame, no text --ar 1:1 --s 200 --v 6.1
```

**Negative**: `no hands, no whisk, no finished drink, no liquid, no steam, no motion blur on stream`.

---

## IMG-04 · dome_sensor_macro

**Purpose**: Section 4.5 Moments gallery, right tile.
**Aspect ratio**: 1:1, 2048×2048.

```
extreme macro product photograph of the crown detail of [REFERENCE 
SENTENCE], framed tightly to show only the top 60mm: the recessed 
flush charcoal ceramic disc set into the warm off-white ceramic 
dome body, a 20mm obsidian-black glass sensor window dead center 
with a 1mm polished gold circular bezel, raking light from upper 
left revealing the subtle unglazed ceramic tooth texture, crisp 
detail on the gold bezel catching light, scientific instrument 
photography, Leica aesthetic, no reflections in the obsidian glass, 
composition is slightly-tilted top-down (about 20 degrees off 
vertical), Phase One medium format, 120mm macro, f/11, stacked 
focus, warm off-white backdrop (#EDE8DF) just visible at frame 
edges, no text, no knob, no button --ar 1:1 --s 150 --v 6.1
```

**Negative**: `no reflections in glass, no text, no glossy surfaces, no lens flare, no knob, no button, no protrusion`.

---

## IMG-05 · ritual_place_chawan

**Purpose**: Section 4 How-it-works, step 01.
**Aspect ratio**: 4:5, 1600×2000.

```
editorial photograph, portrait orientation, two elegant hands 
gently placing an empty iron-glazed traditional Japanese chawan 
onto a warm walnut wooden countertop, the aluminum base ring of a 
matte-black ceremonial spectrometer device visible to the right of 
frame with its lifted ceramic dome sitting beside it out of focus, 
natural morning light from a kitchen window, warm neutral color 
grade, shallow depth of field, shot on Kodak Portra 400 film look, 
Kinfolk aesthetic, quiet composition with generous negative space 
at top, no faces, hands only from wrist forward, no jewelry, no 
text --ar 4:5 --s 100 --v 6.1
```

**Negative**: `no faces, no logos, no jewelry, no nail polish, no distracting background, no app screens`.

---

## IMG-06 · ritual_whisk_matcha

**Purpose**: Section 4 How-it-works, step 02.
**Aspect ratio**: 4:5, 1600×2000.

```
editorial photograph portrait orientation, hands holding a bamboo 
chasen in classic M-pattern whisking motion over a chawan of thick 
bright-green usucha matcha, foam beginning to form, close overhead 
angle showing whisk bristles and foam surface in sharp focus, the 
dome aluminum base visible blurred in background, warm natural 
light, film grain, Kodak Portra, wooden countertop matching 
previous image for consistency, no faces, hands from wrist, shallow 
depth of field on whisk tip, no text --ar 4:5 --s 100 --v 6.1
```

**Negative**: `no faces, no jewelry, no watches, no finished drink, no text`.

---

## IMG-07 · ritual_lower_dome

**Purpose**: Section 4 How-it-works, step 03.
**Aspect ratio**: 4:5, 1600×2000.

```
editorial photograph portrait orientation, two hands gently 
lowering the ceramic dome portion of [REFERENCE SENTENCE] down 
onto its aluminum base, dome approximately 5cm above the base mid 
motion, through the gap a glimpse of a vivid green usucha matcha 
bowl inside, warm morning window light from camera left, moment of 
ritual reverence, very subtle faint warm-gold glow emerging from 
the seam LED ring, hands from forearm down only, no faces, Phase 
One with Schneider 80mm, slight motion blur only on the descending 
dome (sharp on base and bowl), Kinfolk aesthetic, no text --ar 4:5 
--s 150 --v 6.1
```

**Negative**: `no faces, no screens, no app visible, no wires, no bright LEDs, no text`.

**Iteration note**: LED glow must be restrained — "sunrise edge", not "gadget light".

---

## IMG-08 · dome_hero_studio

**Purpose**: Section 5 The Instrument, left column hero shot.
**Aspect ratio**: 3:4, 1500×2000.

```
monumental studio product photograph of [REFERENCE SENTENCE], 
centered in frame on a seamless warm charcoal-taupe backdrop 
(#6D6962), single large rectangular softbox from upper right 
creating a dramatic wrap light with deep soft shadow on the left, 
three-quarter angle with a slight upward tilt giving the object 
monumental presence, object occupies center 40% of frame (generous 
negative space top and bottom), Phase One IQ4 150MP, 120mm lens at 
f/11, Leica campaign aesthetic, absolutely matte ceramic dome with 
no reflections, warm soft shadow beneath base, gold pinstripe bevel 
catches a single thin highlight, no text, no knob on top, no front 
window --ar 3:4 --s 100 --v 6.1
```

**Negative**: `no glossy finish, no multiple light sources, no reflections, no props, no text, no additional objects, no knob, no front window, no thick gold band`.

---

## IMG-09 · dome_interior_open

**Purpose**: optional secondary product shot.
**Aspect ratio**: 3:4, 1500×2000.

```
studio product photograph showing the ceramic dome of [REFERENCE 
SENTENCE] lifted approximately 100mm above its aluminum base, held 
in frame by an invisible hand (focus on the objects only), 
revealing the matte white interior of the dome and a ring of 16 
warm-white LEDs recessed around the inner rim visible but not 
glowing, the load cell platform surface of the base visible in the 
lower half of the frame, three-quarter angle, seamless warm 
off-white backdrop, single softbox lighting from above, editorial 
product photography, clean composition, no text --ar 3:4 --s 100 
--v 6.1
```

**Negative**: `no visible hands, no wires visible, no LEDs illuminated, no text`.

---

## IMG-10 · matcha_subscription_tin

**Purpose**: subscription card imagery.
**Aspect ratio**: 4:5, 1600×2000.

```
editorial product photograph of a matte black cylindrical 100g 
matcha tin, 80mm diameter, with a minimal gold lowercase "platôme" 
wordmark in JetBrains Mono etched on the front, the lid off and 
placed beside it at an angle, revealing brilliant vivid green 
matcha powder inside, warm morning light from camera left, seamless 
warm off-white backdrop, Hasselblad 100mm macro, f/5.6, editorial 
food + product aesthetic, Kinfolk meets Aesop, no other props, no 
loose powder outside the tin --ar 4:5 --s 100 --v 6.1
```

**Negative**: `no loose powder outside tin, no spoon, no chawan, no text besides the wordmark`.

---

## IMG-11 · hands_over_dome_ambient

**Purpose**: Section 9 Preorder CTA background (used at 15% opacity over ink).
**Aspect ratio**: 16:9, 2560×1440.

```
wide editorial photograph, kitchen scene, both hands resting 
lightly on the crown of [REFERENCE SENTENCE] which sits on a warm 
walnut counter, soft morning light from a large window at camera 
right casting a wide gentle shadow to the left, moody low-contrast 
tonality (this will be used at 15% opacity over black), warm Kodak 
Portra film look, no faces, hands from the forearm only, 
composition has the device centered with large negative space all 
around, Kinfolk lifestyle editorial style, no text --ar 16:9 
--s 150 --v 6.1
```

**Negative**: `no faces, no bright highlights, no harsh contrast, no extra people, no additional kitchen clutter`.

---

## IMG-12 · collection_instrument

**Purpose**: Section 7 Collection card 1.
**Aspect ratio**: 4:5, 1600×2000.

```
editorial product photograph of [REFERENCE SENTENCE] on a seamless 
warm taupe backdrop (#AEA69D), three-quarter angle slightly from 
below, single softbox from upper left, clean minimal composition 
with breathing space, Phase One medium format, Kinto Japan 
aesthetic, absolutely matte ceramic, subtle warm shadow, no text, 
no additional props --ar 4:5 --s 100 --v 6.1
```

**Negative**: `no glossy finish, no reflections, no text, no additional objects`.

---

## IMG-13 · collection_tea

**Purpose**: Section 7 Collection card 2.
**Aspect ratio**: 4:5, 1600×2000.

```
editorial still life photograph, a matte black 100g matcha tin 
standing upright with its lid slightly off-set on top exposing a 
glimpse of vivid green matcha inside, a small pile of loose matcha 
powder on the warm taupe backdrop (#AEA69D) beside the tin, a 
bamboo chashaku scoop resting in the pile, warm morning light from 
camera left, Hasselblad 100mm, f/5.6, editorial still life 
composition, generous negative space at top, no text besides the 
minimal gold wordmark on the tin --ar 4:5 --s 100 --v 6.1
```

**Negative**: `no chawan, no hands, no excess text, no busy backgrounds`.

---

## IMG-14 · collection_ritual

**Purpose**: Section 7 Collection card 3.
**Aspect ratio**: 4:5, 1600×2000.

```
editorial still life photograph of traditional matcha implements 
arranged on a dark brushed stone slab: a handcrafted iron-glazed 
chawan with one subtle drip of dark glaze down its side, a bamboo 
chasen whisk standing upright beside it, a long slender bamboo 
chashaku scoop laid across the rim of the chawan, warm directional 
light from camera right, warm taupe backdrop (#AEA69D), composition 
in lower third of frame leaving generous negative space above, 
Phase One medium format, Kinfolk meets Kinto aesthetic, quiet 
reverence, no text --ar 4:5 --s 100 --v 6.1
```

**Negative**: `no text, no dome, no matcha powder loose in frame, no additional props`.

---

## Generation workflow

1. **Generate HERO-VIDEO-01 first.** This is the single most important asset. Iterate Veo 3–6 times to lock the dome geometry. Save the Veo clip, upscale, grade, export formats. Extract HERO-POSTER-01 from the held frame.
2. **Generate HERO-VIDEO-01-MOBILE** with portrait 4:5 aspect ratio.
3. **Lock the canonical dome render**: generate IMG-08 (`dome_hero_studio`) in Midjourney, iterate 20+ times until the catenary profile, hairline gold, flush crown all read correctly. This becomes the IP reference (`--cref`) for all subsequent dome shots.
4. Generate IMG-12, IMG-14 using IMG-08 as IP reference for dome consistency.
5. Generate IMG-04 (sensor macro) with the cref.
6. Generate IMG-07 (dome lowering onto base) with the cref.
7. Generate IMG-09 (dome interior open) with the cref — this one is harder; expect 30+ attempts.
8. Generate IMG-05 + IMG-06 (ritual place, ritual whisk) as a matched pair — shared wooden countertop, matched lighting direction.
9. Generate IMG-01 + IMG-03 + IMG-13 (matcha-forward shots) in a batch — matched green saturation.
10. Generate IMG-10 (subscription tin) and IMG-11 (ambient hands) last.

Color-grade everything through a single-pass batch in Lightroom or Capture One to align white balance (4800K), shadow lift (+8), and per-category clarity adjustments.

Export all stills at 2048px long edge (Moments / Collection / Cards) or 1600×2000 (ritual portraits) or 1500×2000 (product portraits), sRGB, AVIF quality 92 primary, JPEG quality 85 fallback.

---

## File structure

```
/public/images/
  /hero/
    hero.av1.mp4                  ← HERO-VIDEO-01 AV1 encode
    hero.hevc.mp4                 ← HERO-VIDEO-01 H.265 encode
    hero.webm                     ← HERO-VIDEO-01 VP9 encode
    hero-mobile.av1.mp4           ← HERO-VIDEO-01-MOBILE
    hero-mobile.hevc.mp4
    hero-mobile.webm
    hero-poster.avif              ← HERO-POSTER-01 (8K LCP / reduced-motion fallback)
    hero-poster.jpg
    hero-poster-1920.webp         ← compressed poster for fast first paint
    hero-poster-mobile.avif       ← HERO-POSTER-01-MOBILE
    hero-poster-mobile.jpg
  /moments/
    img-01-matcha-macro-froth.{avif,jpg}
    img-03-matcha-chawan-pour.{avif,jpg}
    img-04-dome-sensor-macro.{avif,jpg}
  /ritual/
    img-05-ritual-place-chawan.{avif,jpg}
    img-06-ritual-whisk-matcha.{avif,jpg}
    img-07-ritual-lower-dome.{avif,jpg}
  /product/
    img-08-dome-hero-studio.{avif,jpg}
    img-09-dome-interior-open.{avif,jpg}
    img-10-matcha-subscription-tin.{avif,jpg}
  /ambient/
    img-11-hands-over-dome-ambient.{avif,jpg}
  /collection/
    img-12-collection-instrument.{avif,jpg}
    img-13-collection-tea.{avif,jpg}
    img-14-collection-ritual.{avif,jpg}
```

---

## When to replace AI-generated assets with real photography

AI-generated renders are fine for MVP launch. Replace in this priority order once physical prototypes exist:

1. **IMG-08** (`dome_hero_studio`) — the most prominent product shot.
2. **HERO-VIDEO-01** — commission a real physical prototype + tabletop slow-motion cinematography + practical god-ray lighting. Budget €4–8k for a half-day shoot with a product cinematographer.
3. **IMG-04, IMG-09, IMG-12** — product detail shots.
4. **IMG-05, IMG-06, IMG-07** — ritual shots (benefit from a real kitchen environment).

Matcha macros (IMG-01, IMG-03, IMG-13) and the ambient hands shot (IMG-11) can remain AI-generated indefinitely — they're forgiving and indistinguishable from stock.
