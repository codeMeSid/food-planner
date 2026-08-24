# Keto for GERD: 4 Month Plan (PWA)

Installable app with the daily meal plan, workout split, and weekly
grocery checklist with quantities. Works offline once installed.

## Deploy to GitHub Pages

1. Create a repo on GitHub (example: `keto-gerd-plan`).
2. Put these files at the **repo root** (not in a subfolder):

   `index.html`, `manifest.json`, `service-worker.js`, `.nojekyll`,
   `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`

   Drag-and-drop on the GitHub website works, or:

   ```
   git init
   git add .
   git commit -m "Keto GERD plan PWA"
   git branch -M main
   git remote add origin https://github.com/<your-username>/keto-gerd-plan.git
   git push -u origin main
   ```

3. Repo **Settings → Pages**: Source = `main` branch, folder `/ (root)`, save.
4. Open `https://<your-username>.github.io/keto-gerd-plan/` on your phone.
5. iPhone (Safari): Share → **Add to Home Screen**.
   Android (Chrome): menu → **Install app**, or the browser install prompt.

`.nojekyll` tells Pages not to run Jekyll, so every file at the root is served as-is.

If the site is at `https://<user>.github.io/keto-gerd-plan/`, keep `start_url` and `scope` as `./` (already set). Do not point them at `/` or the PWA will escape the project folder.

## Notes

- Grocery checks live in `localStorage` on that device. Two phones will not sync.
- Edit `PLAN`, `RECIPES`, or `WORKOUT` near the top of the `<script>` in `index.html`. Weekly grocery quantities rebuild from `PLAN` × recipe ingredients.
- After you edit files and push, bump `CACHE` in `service-worker.js` (example: `keto-gerd-v4`) so installed copies pick up the change.
