# Plate 78 (PWA)

Installable app with the daily meal plan (~1,500 kcal, ~150 g protein),
workout split, and weekly grocery checklist with quantities. Works offline
once installed.

## Deploy to GitHub Pages

1. Create a repo on GitHub (example: `plate-78`).
2. Put these files at the **repo root** (not in a subfolder):

   `index.html`, `manifest.json`, `service-worker.js`, `.nojekyll`,
   `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`

   Drag-and-drop on the GitHub website works, or:

   ```
   git init
   git add .
   git commit -m "Plate 78 PWA"
   git branch -M main
   git remote add origin https://github.com/<your-username>/plate-78.git
   git push -u origin main
   ```

3. Repo **Settings → Pages**: Source = `main` branch, folder `/ (root)`, save.
4. Open `https://<your-username>.github.io/plate-78/` on your phone.
5. iPhone (Safari): Share → **Add to Home Screen**.
   Android (Chrome): menu → **Install app**, or the browser install prompt.

`.nojekyll` tells Pages not to run Jekyll, so every file at the root is served as-is.

If the site is at `https://<user>.github.io/plate-78/`, keep `start_url` and `scope` as `./` (already set). Do not point them at `/` or the PWA will escape the project folder.

## Notes

- Grocery checks live in `localStorage` on that device (`plate78_grocery`). Two phones will not sync.
- Edit `PLAN`, `RECIPES`, or `WORKOUT` near the top of the `<script>` in `index.html`. Keep ingredient strings in the form `MACRO_RULES` understands (e.g. `Chicken breast, raw 150 g`, `Olive oil 1 tsp`) so day totals stay accurate. Weekly grocery rebuilds from `PLAN` × recipe ingredients.
- After you edit files and push, bump `CACHE` in `service-worker.js` (example: `plate78-v2`) so installed copies pick up the change.
