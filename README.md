# grahmtg.com

Personal portfolio site for Grahm Gaydos. Static HTML/CSS/JS, hosted on GitHub Pages.

## Structure

```
.
├── index.html          # Homepage: hero, featured research, work table, about, now, contact
├── thesis.html         # Standalone thesis page with interactive chart
├── 404.html            # Custom 404 page
├── CNAME               # Custom domain configuration (edit when domain is ready)
└── README.md
```

That's it. No build step, no dependencies, no node_modules, no framework. Open the HTML files in any browser to preview.

## Tech notes

- **Fonts** are loaded from Google Fonts: Geist, Geist Mono, Instrument Serif.
- **No JavaScript libraries.** All interactivity (work filter, auto-cycle toggle, thesis chart, marquee, status bar clock) is hand-written vanilla JS.
- **Thesis chart** is custom SVG generated in JS from a data array. To update with real data, replace the `data` array in `thesis.html` (line ~470).
- **Mobile breakpoint** at 900px.

## Editing copy

Most edits are in plain HTML. The structure is:

- Hero copy: search for `class="hero-title"` and `class="hero-bio"` in `index.html`.
- Stats: search for `class="hero-meta-strip"`.
- Featured Research links: search for `class="featured-line"`.
- Work entries: each is an `<article class="work-item">` block with a body, an impact panel, and tags. To add a new entry, copy an existing one and update the six fields (title, year, venue, body paragraphs, impact rows, tags).
- About copy: search for `class="about-text"`.
- Now panel: search for `class="now-items"`.
- Contact: search for `class="contact-links"`.

## Editing photos

The site uses placeholder slots in dashed grey for photos. To add real images:

1. Drop image files into a new `images/` folder at the repo root.
2. In `index.html`, find each `.hero-photo`, `.about-portrait`, and `.photo-strip-item` block.
3. Replace the placeholder pseudo-element styling with `<img src="images/your-photo.jpg" alt="..." />` inside the div, or set a `background-image: url('images/your-photo.jpg')` on the container.
4. Optimise photos before committing — keep each under 500KB. Use [squoosh.app](https://squoosh.app) for compression.

## Updating the thesis chart with real data

The chart in `thesis.html` currently uses synthetic data calibrated to the preliminary findings. When the real numbers are ready:

1. Open `thesis.html` and find the `data = months.map((d, i) => { ... })` block (~line 470).
2. Replace it with a hardcoded array of `{date, label, actions, spend}` objects, one per month.
3. Update the intervention indices (`m1 = 74`, `m2 = 80`) if they shift.
4. Update the y-axis range (`yMin`, `yMax`) if the real values fall outside 50–250.

## Local development

No build step needed. To preview locally:

```bash
# Option 1: open the file directly
open index.html

# Option 2: serve via Python for proper relative paths
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deployment

The site is served via GitHub Pages from the `main` branch. Every push to `main` triggers a redeploy (typically live within 30 seconds). No CI/CD config needed.

To set up Pages on a fresh repo: Settings → Pages → Source: "Deploy from a branch" → Branch: `main` / `/(root)` → Save.

## Custom domain

The `CNAME` file contains the apex domain. To change domains, edit that file and update the DNS records at the registrar.
