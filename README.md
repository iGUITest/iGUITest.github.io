# iGUITest Research Group Website

This repository contains a static team homepage for GitHub Pages. The site is built with plain `HTML`, `CSS`, and small client-side scripts. There is no build step and no framework dependency.

## Structure

- `index.html`: page structure and client-side rendering logic
- `styles.css`: visual styles and layout
- `paper.csv`: publication dataset
- `members.json`: member dataset
- `support.json`: awards and funding dataset
- `teaching.json`: teaching dataset
- `YSC.jpg`, `award.gif`, `ETH.svg`, `TUM.png`: visual assets used by the page

## How the site works

The page loads structured data in the browser:

- publications are loaded from `paper.csv`
- members are loaded from `members.json`
- awards and funding are loaded from `support.json`
- teaching content is loaded from `teaching.json`

This means most content updates should be done by editing data files instead of changing `index.html`.

## Content maintenance

### 1. Update team members

Edit `members.json`.

Each item uses this shape:

```json
{
  "name": "Member Name",
  "role": "Role Title",
  "type": "featured",
  "image": "photo.jpg",
  "description": "Short profile text.",
  "highlights": [
    "Highlight 1",
    "Highlight 2"
  ]
}
```

Notes:

- `type` is optional. Use `"featured"` for the large first member card.
- `image` is only needed for a featured card.
- `highlights` is optional.

For a normal member card:

```json
{
  "name": "Member Name",
  "role": "Student Researcher",
  "description": "Short profile text."
}
```

### 2. Update awards and funding

Edit `support.json`.

Each card uses this shape:

```json
{
  "title": "Section Title",
  "image": "award.gif",
  "items": [
    "Line 1",
    "Line 2"
  ]
}
```

Notes:

- `image` is optional.
- `items` should be a short list of bullet points.

### 3. Update teaching content

Edit `teaching.json`.

Each item uses this shape:

```json
{
  "title": "Course or Activity",
  "description": "Short explanation.",
  "tag": "Category label"
}
```

Notes:

- `tag` is optional but recommended for clarity.

### 4. Update publications

Edit `paper.csv`.

Current columns:

```text
year,id,title,link,status,author,cofauthor,corauthor,level,venue,note
```

The current page uses these fields:

- `year`
- `id`
- `title`
- `link`
- `author`
- `level`
- `venue`
- `note`

Notes:

- publications are grouped by `year`
- within the same year, they are sorted by `id`
- `status` is currently not shown on the page
- venue abbreviations are supplemented in `index.html` by a small mapping function

If a new venue abbreviation is missing, update `getVenueLabel()` in `index.html`.

## Visual maintenance

### Change text content

Update:

- `index.html` for static sections such as hero, about, and research areas
- the data files for members, support, teaching, and publications

### Change colors or spacing

Edit `styles.css`.

The main design tokens are near the top of the file:

- `--bg`
- `--surface`
- `--surface-muted`
- `--text`
- `--brand`
- `--accent`
- `--shadow`

### Replace images

1. Add the new image file to the repository root.
2. Update the corresponding filename in `index.html` or JSON data.
3. Keep filenames simple and web-safe.

## Local preview

Because the page uses `fetch()` to load `CSV` and `JSON`, opening `index.html` directly with `file://` may fail in some browsers.

Recommended local preview options:

1. Use GitHub Pages after pushing changes.
2. Run a small local static server.

Example:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/
```

## GitHub Pages deployment

This site is compatible with GitHub Pages as a plain static website.

Typical workflow:

1. Edit the data files or page files.
2. Commit and push to the GitHub Pages branch.
3. Wait for GitHub Pages to publish the update.

## Recommended maintenance rules

- Keep all homepage copy in English unless bilingual content is intentionally introduced.
- Prefer updating data files over editing rendering logic.
- Keep descriptions concise. This page works better with short academic summaries than long paragraphs.
- When adding publications, keep `year`, `id`, and `venue` consistent.
- If content grows significantly, consider moving `about` and `research` into their own JSON files as well.

## Common issues

### The page shows “Unable to load ... data.”

Possible reasons:

- the site was opened with `file://`
- the data file name is wrong
- the JSON syntax is invalid
- the CSV format is broken

### A section is blank after editing JSON

Check:

- commas and quotes in the JSON file
- whether the file is valid JSON
- whether required fields such as `title` or `name` are present

### A publication venue has no abbreviation

Update the `getVenueLabel()` mapping in `index.html`.

## Suggested future cleanup

- move `about` content into `about.json`
- move `research areas` into `research.json`
- add explicit `abbr` column to `paper.csv` to avoid hardcoded venue mapping
