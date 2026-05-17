# Recipe Book — hostable site

A static version of the recipe book, ready to deploy to GitHub Pages (or any static host). On load, the app fetches `recipes.json` and renders the grid. To update the recipes anyone sees, edit `recipes.json` and re-deploy.

## Files

- `index.html` — the app shell (filters, grid, recipe modal, shop mode).
- `recipes.json` — the recipe database. Edit this to add/change recipes, then re-deploy.
- `.nojekyll` — tells GitHub Pages not to run Jekyll over the folder.

## Deploy to GitHub Pages (unlisted)

1. **Create a private repo** on GitHub — call it something like `recipe-book` (the repo can be private; the published Pages URL is still public, just not linked anywhere or indexed).
2. Add this `site/` folder's contents to the repo root (or to a `docs/` folder, your choice).
3. In the repo: **Settings → Pages**.
   - Source: **Deploy from a branch**.
   - Branch: `main`, folder: `/ (root)` (or `/docs` if you put files there).
4. Wait ~30 seconds. GitHub will give you a URL like `https://<your-username>.github.io/recipe-book/`.

That URL is technically public but not linked from anywhere, won't show up in search results immediately, and isn't guessable unless someone knows the repo name. That's the "unlisted" model you asked for.

### Want a less guessable URL?

Rename the repo to something opaque (e.g. `rb-x7k2`) — the URL becomes `https://<username>.github.io/rb-x7k2/`. Or use a custom subdomain like `recipes.your-domain.com` via a `CNAME` file.

### Quick deploy via `gh` CLI

```bash
cd "Recipe App/site"
git init && git add -A && git commit -m "Initial recipe site"
gh repo create recipe-book --private --source=. --push
# Then enable Pages in the GitHub UI (one-time).
```

## Updating recipes

The simplest workflow:

1. Edit `recipes.json` (add a new recipe object to the array, or tweak existing ones).
2. Commit and push. GitHub Pages re-deploys in ~30 seconds.
3. Reload the URL — fresh data.

Each recipe object should match the existing shape: `id`, `name`, `source`, `cuisine`, `protein`, `timeMinutes`, `baseServings`, `calories`, `desc`, `time`, `tags`, `ingredients` (array of `[name, qty]`), `pantry`, `allergens`, `method` (array of step strings).

## Run locally (before deploying)

Because the app fetches `recipes.json`, opening `index.html` directly via `file://` won't work — browsers block `fetch` to local files. Serve the folder over HTTP:

```bash
cd "Recipe App/site"
python3 -m http.server 8000
```

Then visit `http://localhost:8000/`.

## What's different from `recipe-cards.html`

The original `recipe-cards.html` (in the parent folder) bundles all recipe data inline as a JS array — handy as a single-file app. This hosted version splits that data into `recipes.json` so you can update recipes without re-encoding the whole HTML, and so re-deploys are tiny diffs.

The `customRecipes` localStorage feature still works: anyone visiting the site can add recipes that persist in *their* browser, but those won't be shared across devices or visitors. To make a recipe canonical, add it to `recipes.json` and re-deploy.
