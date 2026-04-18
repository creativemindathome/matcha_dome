# Platôme Matcha (static export)

This repository contains the **`matcha dome`** export: the single-page HTML, `images/`, and `uploads/`.

Open `index.html` or `Platôme Matcha.html` in a browser (double-click or serve the folder locally). `index.html` is a copy of the same page so `/` works on hosts like Vercel.

**Vercel:** the project must use the **Other** framework (see `vercel.json`). If an old deployment still runs `next build`, open the project on Vercel → Settings → General → clear **Build Cache**, then redeploy.

There is no Node.js app in this version—the previous Next.js scaffold was removed in favor of this folder layout.

Older commits still contain the full `/docs` spec set (including `03-landing-page.md` and `04-image-manifest.md`) if you need them: `git log --oneline` then `git show <commit>:docs/03-landing-page.md`.
