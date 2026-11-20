# Kids Educational Apps & YouTube Directory

Live site: <https://f1mahesh.github.io/kids-app>

A static, dependency-free web page that helps parents find educational apps and YouTube channels for kids. Visitors can filter by category, age group and pricing, or search by keyword. Every card links to the official website or channel.

All content lives in JSON files, so you can add or change entries without touching any HTML or JavaScript.

## Project structure

```
kids-app/
├── index.html          # Page layout, styles and rendering logic
└── data/
    ├── apps.json       # Educational apps (categories + items)
    └── youtube.json    # YouTube channels (categories + items)
```

## Run locally

The page loads the JSON with `fetch()`, so it must be served over HTTP. Opening `index.html` directly from disk will not work.

```bash
cd kids-app
python -m http.server 8000
```

Then open <http://localhost:8000>. Any static server works (e.g. `npx serve`, VS Code Live Server).

## Update content

Edit the JSON files and refresh the page. Counts, filter buttons and cards update automatically.

### Add an app (`data/apps.json` → `items`)

```json
{
  "name": "Khan Academy Kids",
  "icon": "🐻",
  "category": "early",
  "ageLabel": "Ages 2–8",
  "ageGroups": ["2-5", "6-9"],
  "pricing": "free",
  "description": "Short description shown on the card.",
  "url": "https://learn.khanacademy.org/khan-academy-kids/"
}
```

| Field | Notes |
| --- | --- |
| `category` | Must match a category `id` in the same file. |
| `ageLabel` | Text shown on the card. |
| `ageGroups` | Used by the age filter. Allowed values: `2-5`, `6-9`, `10+`. |
| `pricing` | `free`, `freemium` or `paid`. |
| `url` | Must start with `http://` or `https://`. |

### Add a YouTube channel (`data/youtube.json` → `items`)

```json
{
  "name": "SciShow Kids",
  "icon": "🔬",
  "category": "science",
  "ageLabel": "Ages 4–10",
  "handle": "@SciShowKids",
  "description": "Short description shown on the card.",
  "url": "https://www.youtube.com/@SciShowKids"
}
```

### Add or rename a category (`categories` in either file)

```json
{
  "id": "early",
  "pillLabel": "🧸 Early Learning",
  "label": "Early Learning",
  "badgeClass": "badge-cat-early"
}
```

- `id` is referenced by items via `category`.
- `pillLabel` is the text on the filter button; `label` is the badge text on cards.
- `badgeClass` is a CSS class defined in `index.html`. Reuse an existing one (`badge-cat-*` for apps, `badge-yt-*` for channels), or add a new class in the `<style>` block.
- Button order follows the order of the `categories` array.

### Remove an entry

Delete its object from `items`. Keep the JSON valid (no trailing commas).

## Maintenance tips

- Validate JSON after editing, e.g. `python -m json.tool data/apps.json` (or rely on your editor's JSON validation).
- Periodically check that links still work, especially YouTube handles and app store pages.
- Keep descriptions to one or two sentences so cards stay evenly sized.
- If the page shows "Could not load data", check the browser console for a JSON syntax error or a wrong file path.

## Deploy on GitHub Pages

1. Push the repository to GitHub.
2. Go to **Settings → Pages**, set **Source** to *Deploy from a branch*, choose `main` and the `/ (root)` folder, then save.
3. The site is published at `https://<your-username>.github.io/<repo-name>/`.

To update the live site, edit the JSON, commit and push. The page picks up the changes after Pages redeploys (usually within a minute or two).