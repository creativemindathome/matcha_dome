# 05 · Build Script for Claude Code

Two parts:
1. **Copy-pasteable master prompt** for Claude Code to execute the build.
2. **Manual checklist** for what you do before, during, and after.

---

## Part 1 · Master prompt

Scaffold the repo first:

```bash
npx create-next-app@latest platome-matcha \
  --typescript --tailwind --app --eslint --src-dir=false
cd platome-matcha
npx shadcn@latest init --defaults
npm i motion
git init && git add -A && git commit -m "scaffold"
```

Drop all five MD files into `/docs`. Then launch Claude Code:

```bash
npm install -g @anthropic-ai/claude-code
claude
```

Paste this into the Claude Code session:

---

```
You are building the Platôme Matcha landing site. The full specification 
lives in /docs across five markdown files. Read them in order before 
writing any code:

  /docs/01-brief.md           — project context, brand, scope
  /docs/02-design-system.md   — tokens, type, motion, component primitives
  /docs/03-landing-page.md    — section-by-section layout spec
  /docs/04-image-manifest.md  — image and video asset specs and paths
  /docs/05-build-script.md    — this file

## Stack (already scaffolded)

- Next.js 15 App Router, TypeScript, Tailwind v4
- shadcn/ui initialized (use sparingly — only Accordion for the FAQ and 
  Dialog for preorder; do NOT default to shadcn for primary UI, the 
  design system has custom primitives)
- next/font for Fraunces (with variable axes SOFT, WONK, opsz),
  Instrument Sans, JetBrains Mono
- motion (imports from "motion/react", NOT framer-motion)

## Hero asset strategy

The hero uses a looping video (HERO-VIDEO-01) + a CSS/DOM overlay layer 
for the UI-side animations (impact flash, bevel pulse, spectral flash, 
gradient bar, score count-up). The video handles phases 0–3 of the 
animation; the overlay handles phases 4–9. Both synchronize via a 
timeupdate listener on the video element.

If the video file isn't yet in /public/images/hero/, the Hero component 
must gracefully fall back to the 8K poster still (HERO-POSTER-01) and 
run the FULL animation sequence (phases 0–9) as CSS/DOM overlay on top 
of the still. So the Hero component's animation code must be able to 
either:
  (a) listen to the video's currentTime and trigger overlay phases 
      at the specified timestamps, OR
  (b) run the full animation sequence on its own timeline when no 
      video is present.

Design the component with a `mediaType: "video" | "image"` prop or 
similar abstraction.

## Build order

Execute in this sequence. After each step, stop and show me what you 
built so I can approve before continuing.

### Step 1 · Design tokens

- Update app/globals.css with the @theme block from 02-design-system.md 
  (all color, font, spacing, and quality-gradient tokens)
- Load all three fonts via next/font in app/layout.tsx with the variable 
  axes specified
- Configure Tailwind v4 to consume the custom tokens
- Create /test page showing every token visually (color swatches with 
  hex codes, every type token, the quality-gradient strip rendered). We 
  use this to verify tokens before touching any real section.

### Step 2 · Layout chrome

- app/layout.tsx with font loading and base body styles (background 
  paper, color ink, antialiased)
- components/site/AnnouncementBar.tsx (Section 0)
- components/site/Header.tsx (Section 1 — absolute over hero, sticky 
  with paper-soft fill + hairline border after hero exits)
- components/site/Footer.tsx (Section 11 — newsletter zone, four-column 
  nav, big-type platôme lockup with paper→ink clip-path transition)

### Step 3 · Component primitives

Before the hero, build the reusable components the hero depends on:

- components/ui/FeatureCard.tsx per spec (icon slot, tags slot, title, 
  body, variant="default"|"emphasized", backdrop-blur ink background)
- components/ui/QualityGradientBar.tsx per spec (props: score, max, 
  size="hero"|"section", animate boolean; uses Motion for the bar 
  slide-in and marker slide; uses requestAnimationFrame for tabular 
  score count)
- components/ui/Icon.tsx with three SVG icons (Scan, Gauge, Tin) per 
  spec — single-color 1.5px stroke, round caps

### Step 3.5 · The cinematic animated hero

This is the hardest section. Follow the animation sequence in Section 2 
of /docs/03-landing-page.md exactly. Structure:

components/sections/Hero.tsx
  - Full-bleed, 100vh min 720px, ink background
  - Layer 0 · Media:
      - If HERO-VIDEO-01 exists in /public/images/hero/, render a 
        <video autoplay muted loop playsinline poster=...> with 3 
        <source> tags (AV1, H.265, WebM) in that order.
      - Otherwise render next/image with priority, 
        src="/images/hero/hero-poster.avif" and jpg fallback via picture.
      - Responsive: use the -mobile variant for viewports ≤768px.
  - Layer 1 · Dark vignette (CSS radial gradient, 0–40% opacity corners)
  - Layer 2 · Headline block (preamble, h1, subhead) positioned 
    absolute, runs entrance sequence on mount
  - Layer 3 · Animation overlay (see below)
  - Layer 4 · Feature cards (3 × FeatureCard, entrance with stagger)
  - Layer 5 · Scroll indicator (1px × 40px gold vertical line, slow 
    pulse)

components/sections/HeroAnimation.tsx

Orchestrates the overlay animation. When mediaType="video", attaches a 
timeupdate listener on the video element passed by ref and triggers 
overlay phases at the timestamps specified in the landing-page spec 
(phase 4 at 1.85s, phase 5 at 2.5s, etc.). When mediaType="image", runs 
the entire 0–9 phase sequence on its own timeline via Motion's 
useAnimate or a custom state machine.

Implementation sketch:

  const { scope, animate } = useAnimate();
  const runPhase4_SpectralFlash = () => { 
    animate("#spectral", { opacity: [0, 1, 0], scale: [0, 2] }, 
            { duration: 0.25 });
    animate("#bevel-pulse", { boxShadow: [...] }, { duration: 0.6 });
  };
  const runPhase5_BarReveal = () => { 
    animate("#quality-bar", { x: [100, 0], opacity: [0, 1] }, 
            { duration: 0.6 });
  };
  // ...etc.

  if (mediaType === "video") {
    videoRef.current.addEventListener("timeupdate", (e) => {
      const t = videoRef.current.currentTime;
      if (t >= 1.85 && !state.phase4Triggered) runPhase4_SpectralFlash();
      if (t >= 2.5  && !state.phase5Triggered) runPhase5_BarReveal();
      // ...etc.
      if (t >= 7.0) resetOverlayState();  // prepare for next loop
    });
  } else {
    // run full timeline
    const runLoop = async () => {
      await wait(300); showDrop(); 
      await wait(200); fallDrop(); 
      await wait(1200); impactFlash(); 
      await wait(150); runPhase4_SpectralFlash(); 
      // ...etc, then
      setTimeout(runLoop, 8000);
    };
    useEffect(() => { runLoop(); }, []);
  }

Position all overlay elements in percentages of the hero container so 
they align with the media composition regardless of viewport:
  - Drop start: x:65%, y:0%
  - Drop impact (sensor window): x:65%, y:38%
  - Bevel ring: centered on dome at x:65%, y:55%, ~180px wide
  - Gradient bar: centered-bottom, above feature cards, ~480px wide

Do NOT hardcode pixel positions. Everything is percentage-based.

Implement prefers-reduced-motion: skip the loop and render only the 
final-frame composition statically (drop absent, bar visible, score 87 
displayed, bevel softly lit). Also if mediaType="video", add the video 
attribute autoplay only if reduced motion is NOT preferred; otherwise 
just show the poster image.

Stop after implementing Step 3.5 and show me the hero running for ~20 
seconds. We iterate on timing, easing, and positioning here before 
moving on.

### Step 4 · Remaining sections

Build these one at a time, stopping after each for review:

  Section 3  — Editorial opener
  Section 4  — How it works
  Section 4.5 — Moments gallery
  Section 5  — The instrument
  Section 6  — The Koku Score
  Section 7  — The Collection
  Section 8  — The matcha (subscription tiers)
  Section 9  — Preorder CTA
  Section 10 — FAQ (shadcn Accordion)

Insert the specified hairline dividers between sections.

### Step 5 · The spectral curve motif

components/motifs/SpectralCurve.tsx — reusable SVG component, animated 
stroke-dashoffset draw-in on scroll-into-view via Motion's whileInView. 
Used in Section 3 and as the backbone of Section 6.

### Step 6 · Preorder flow

- components/preorder/ReserveButton.tsx — opens shadcn Dialog
- Dialog contains two options:
    1. "Reserve with €149 deposit" → links to 
       process.env.NEXT_PUBLIC_STRIPE_PAYMENT_LINK (user provides URL)
    2. "Join waitlist without paying" → inline email form POSTing 
       to /api/waitlist
- app/api/waitlist/route.ts — accepts email, logs, returns 200 (wire 
  to real backend later)
- app/thank-you/page.tsx — minimal confirmation page

### Step 7 · Copy

All body copy is inline in /docs/03-landing-page.md. Do not invent 
copy. Flag anything unclear.

### Step 8 · Polish

- prefers-reduced-motion disables all entrance animations AND the 
  hero loop (static final frame only)
- All interactive targets ≥44×44px
- Contrast: paper-on-ink ≥14:1, gold-on-ink ≥4.8:1, ink-on-paper 
  ≥14:1 — verify with contrast checker
- Lighthouse desktop: perf ≥90 (hero media is LCP candidate, use 
  priority loading), accessibility ≥100
- Verify CLS = 0 (reserve hero aspect ratio so it doesn't shift layout 
  when the video loads)
- No console errors or warnings
- `next build` builds clean

### Step 9 · Deploy prep

- README with setup, env vars needed, asset expectations
- Verify `vercel --prod` would deploy cleanly

## Principles while you build

- Implement exactly what the spec says. If ambiguous, ask concisely; 
  do not invent. For judgment calls, make the smallest reasonable 
  choice and flag it.
- Do not default to shadcn/ui for primary UI. Buttons, forms, cards, 
  dividers are specified custom.
- Motion library is "motion" (imports from "motion/react"), NOT 
  framer-motion.
- No drop shadows except specified (feature cards use backdrop-blur, 
  not shadow).
- No rounded corners beyond 2px unless specified.
- No gradients unless specified (the quality bar is explicit; the 
  footer paper→ink transition is explicit).
- All spec measurements are desktop; halve or clamp() for mobile.
- CSS Grid for section layouts, Flexbox for component internals.
- Semantic HTML — SEO matters.
- The hero animation depends on the dome sitting at x:65%, y:55% in 
  the media, with sensor at x:65%, y:38%. If the media you receive 
  has the dome in a different position, TELL me — don't silently 
  adjust animation coordinates.

## What to ignore from your training

- Inter, Roboto, Space Grotesk — we use Fraunces + Instrument Sans + 
  JetBrains Mono.
- Pastel gradients, rounded cards, playful illustration.
- Cheerful DTC marketing voice — we write like Kinfolk.
- Framer Motion naming — the library rebranded to Motion.

Begin with Step 1. Read all five docs first, then confirm understanding 
before writing code.
```

---

## Part 2 · Manual checklist

### Before you run Claude Code

- [ ] Register the domain (platome.com / platomematcha.com).
- [ ] Create a Stripe account, generate a Payment Link for €149 deposit. Save the URL for `.env.local` as `NEXT_PUBLIC_STRIPE_PAYMENT_LINK`.
- [ ] Set up a Resend account (free tier) for later transactional email. Save API key.
- [ ] Generate **HERO-VIDEO-01** in Veo using the prompt in `04-image-manifest.md`. Iterate 3–6 times until the dome geometry is correct (catenary profile, no front window, no knob, thin gold pinstripe). Upscale in Topaz. Grade in DaVinci. Encode to AV1 + H.265 + WebM.
- [ ] Extract the poster frame from the video at ~6.5s as `HERO-POSTER-01`.
- [ ] Generate **HERO-VIDEO-01-MOBILE** in Veo with the 4:5 portrait variant.
- [ ] Generate **IMG-08** (`dome_hero_studio`) in Midjourney first. Iterate 20+ times until you have a definitive canonical dome. Upscale to 4K. Save as IP reference.
- [ ] Generate the remaining stills using IMG-08 as IP reference where a dome appears.
- [ ] Place all assets at the paths specified in `04-image-manifest.md` under `/public/images/`.

### During the build

- [ ] After Step 1: verify `/test` page shows tokens correctly. Fraunces must load with variable axes — if it looks generic, the axes aren't loading.
- [ ] After Step 3: test FeatureCard, QualityGradientBar, Icon on a throwaway page before integrating into Hero.
- [ ] After Step 3.5: watch the hero loop 10 times. Critique:
  - Does the matcha drop (in the video) feel slow-motion, cinematic, satisfying?
  - Does the CSS impact flash land exactly on the sensor window position, or is it drifting off?
  - Does the spectral burst feel like "reading the light", or like a generic glow?
  - Does the gradient bar slide in smoothly? Does the marker travel at the right speed?
  - Is the hold moment long enough to read the composition?
  - When the loop restarts, is the transition seamless or jarring?
- [ ] After Step 4: full scroll-through pass on desktop and mobile. Walk away 5 minutes, come back, scroll again. Iterate any section that feels generic.
- [ ] After Step 8: run Lighthouse on the deployed preview URL (not localhost — the LCP will differ).

### After deploy

- [ ] Deploy to Vercel (`vercel --prod` or GitHub integration).
- [ ] Point domain at Vercel deployment.
- [ ] Install Plausible or Simple Analytics (not Google Analytics).
- [ ] Set up a `robots.txt` and `sitemap.xml`.
- [ ] Submit to Google Search Console.
- [ ] Share the URL with 5 people fitting the buyer profile (premium matcha buyers, instrument-curious early adopters). Watch their scroll behavior. Listen for "what is this?" (failure) vs "how much?" (goal).

### What to do if Claude Code gets stuck

- If it installs `framer-motion`, correct it: *"the library is `motion`, imports from `motion/react`"*.
- If the hero animation feels stiff: *"the easing should feel like gravity, not linear. Try cubic-bezier(0.4, 0, 0.6, 1) for the drop fall and ease-out for the bar reveal"*.
- If the hero media shows the dome in the wrong position: regenerate the media, do not bend the animation coordinates to match.
- If the bar reveal competes visually with the score count: slow the bar to 800ms and start the count 100ms after the bar is fully in.
- If Claude produces "clean modern SaaS" design: paste *"Re-read 02-design-system.md §'What this design system is NOT'. Regenerate."*

### Launch gate

Do not go public until:
- [ ] HERO-VIDEO-01 is real (not placeholder) and the dome geometry is correct.
- [ ] Hero animation loops smoothly for ≥3 minutes without jitter.
- [ ] `prefers-reduced-motion` correctly shows the static final frame.
- [ ] All 14 stills + 2 video assets are in place.
- [ ] Preorder charges €149 end-to-end in Stripe test mode.
- [ ] Waitlist endpoint stores emails somewhere you'll see them.
- [ ] A non-you human has read the copy and flagged jargon.
- [ ] You've sent a preorder from mobile Safari.

---

## Estimated timeline

| Day | Work |
|---|---|
| Day 1 | Scaffold, drop docs, Steps 1–2. Start generating HERO-VIDEO-01 in Veo in parallel. |
| Day 2 | Step 3 (component primitives). Continue Veo iterations, encode final video. Start IMG-08 iteration. |
| Day 3 | Step 3.5 (the hero animation). Iterate on timing. |
| Day 4 | Step 4 sections 3–5. Generate ritual images in parallel. |
| Day 5 | Step 4 sections 6–10. Generate remaining images. |
| Day 6 | Steps 5–7 (spectral curve, preorder flow, copy pass). |
| Day 7 | Step 8 (polish). Deploy staging. |
| Day 8 | Share with 5 people. Iterate feedback. Go live. |

Weekend-only pace: ~14 calendar days.
