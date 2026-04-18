# Platôme Matcha (static export)

The site lives under **`public/`** (that is what Vercel deploys when `outputDirectory` is set to `public`):

- `public/index.html` — open locally or at `/` on Vercel
- `public/Platôme Matcha.html` — same page as `index.html`
- `public/images/` — image assets referenced by the page
- `public/uploads/` — extra assets and markdown copies

**Local preview:** from the repo root run `npx serve public` (or open `public/index.html` in a browser).

**Vercel:** `vercel.json` sets `framework` to `null` (Other) and deploys the **`public`** directory. If the dashboard still shows **Next.js** as the framework, open **Project → Settings → General**, set **Framework Preset** to **Other**, clear **Build Cache**, and redeploy—otherwise Vercel may look for a `next` dependency that this repo does not use.

Older commits still contain the full `/docs` spec set if you need them: `git log --oneline` then `git show <commit>:docs/03-landing-page.md`.
