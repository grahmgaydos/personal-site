# Deployment Walkthrough

This walks you through getting the site live on GitHub Pages with a custom domain — no command line required. Total time: about 90 minutes (most of which is waiting for DNS propagation).

---

## Phase 1 — Set up the GitHub repository

### 1.1 Create the repo

1. Go to [github.com/new](https://github.com/new).
2. Name it something simple — `grahmgaydos.com`, `personal-site`, or `portfolio` all work. The name doesn't show up publicly.
3. Set it to **Private** initially. (You can flip it to public later if you want, but private is fine for personal sites — GitHub Pages works on private repos for free.)
4. **Check the box** for "Add a README file" — this initialises the repo so you can immediately upload files to it through the web interface. (You'll replace this README in the next step.)
5. Click **Create repository**.

### 1.2 Show hidden files on your computer

The site bundle includes two files that start with a dot (`.gitignore`) or have no extension (`CNAME`). macOS hides dotfiles by default, so before uploading:

- **Mac:** open Finder, press `Cmd + Shift + .` (period). Hidden files will appear greyed-out. Press the same combo again to hide them later.
- **Windows:** open File Explorer → View → Show → check "Hidden items".

You need both `.gitignore` and `CNAME` to upload — they're real files, not noise.

### 1.3 Upload the site files

1. On your new repo's page, click **Add file → Upload files** (the button is near the top right of the file list).
2. Open the `grahmgaydos-site` folder on your computer in another window.
3. **Select the files inside the folder** (not the folder itself):
   - `index.html`
   - `thesis.html`
   - `404.html`
   - `CNAME`
   - `README.md`
   - `DEPLOYMENT.md`
   - `.gitignore`
4. Drag them all onto the GitHub upload area. (You should see all seven files listed.)
5. At the bottom, write a commit message like "Initial site upload" and click **Commit changes**.

You should now see the seven files at the root of your repo.

> **Important:** GitHub Pages requires `index.html` at the repository *root* — not nested inside a folder. If you accidentally upload the `grahmgaydos-site` folder itself rather than its contents, the site won't render. If this happens: click each file inside the folder, click the trash icon to delete it, then re-upload correctly.

### 1.4 Enable GitHub Pages

1. Go to your repo → **Settings** (tab at the top of the repo) → **Pages** (in the left sidebar, scroll down to find it).
2. Under **Source**, select **Deploy from a branch**.
3. Under **Branch**, pick `main` and `/(root)`. Save.
4. Wait ~30 seconds and refresh the page. You should see a banner: "Your site is live at `https://YOUR_USERNAME.github.io/YOUR_REPO_NAME/`".

Click that URL to verify the site is working before moving on. The Thesis link in the nav should also work.

---

## Phase 2 — Register a custom domain

You need to pick and buy a domain first. Recommendations:

**Where to buy:**

- **[Cloudflare Registrar](https://www.cloudflare.com/products/registrar/)** — sells domains at cost (no markup), free WHOIS privacy, easy DNS management. Best choice if you're comfortable with their dashboard.
- **[Namecheap](https://www.namecheap.com)** — slightly more expensive but very simple. Free WHOIS privacy.
- **[Porkbun](https://porkbun.com)** — also at-cost pricing, decent UI.

Avoid GoDaddy and Google Domains (sold off — not a great experience).

**What to buy:** A `.com` is the safest. Some good candidates given your name:

- `grahmgaydos.com`
- `grahmtg.com` (matches your initials, shorter)
- `gaydos.io` (modern, tech-flavoured — slightly more expensive)

Buy it. The domain costs around $10–15/year for a `.com`. Don't pay for hosting, email, "website builder," or any other add-on the registrar tries to upsell — you don't need them.

### 2.1 After buying

Once registered, log into your registrar's dashboard. You'll need to find the DNS settings page (sometimes called "DNS records," "Manage DNS," or just "DNS"). Don't change anything yet.

---

## Phase 3 — Connect the domain to GitHub Pages

This is a two-sided configuration: tell GitHub the domain, and tell DNS where the domain points.

### 3.1 Update the CNAME file in your repo

The `CNAME` file is currently a placeholder. To update it through GitHub's web interface:

1. In your repo, click the `CNAME` file.
2. Click the pencil icon (top right of the file view) to edit.
3. Replace the entire content with your actual domain — just the bare domain, no `https://`, no slash. For example:

```
grahmtg.com
```

4. Scroll down, write a commit message ("Set custom domain") and click **Commit changes**.

### 3.2 Configure DNS at your registrar

GitHub Pages requires a specific set of DNS records. You have two cases depending on whether you want the apex domain (`grahmtg.com`) or the `www` subdomain (`www.grahmtg.com`) as primary.

**Recommended: apex domain as primary.** This means `grahmtg.com` (no `www`) is the canonical URL.

In your registrar's DNS panel, **delete any existing A, AAAA, or CNAME records for `@` and `www`**, then add:

**For the apex domain (`@` or empty host):** four `A` records, each pointing at one of GitHub's IPs:

```
Type: A    Host: @    Value: 185.199.108.153
Type: A    Host: @    Value: 185.199.109.153
Type: A    Host: @    Value: 185.199.110.153
Type: A    Host: @    Value: 185.199.111.153
```

**For the `www` subdomain (so `www.grahmtg.com` redirects to `grahmtg.com`):** one `CNAME` record:

```
Type: CNAME    Host: www    Value: YOUR_USERNAME.github.io
```

(Replace `YOUR_USERNAME` with your GitHub username — note this points at `username.github.io`, not at the repo specifically. The trailing dot may or may not be required by your registrar.)

**TTL:** leave at default (usually 3600 seconds / 1 hour) for now.

### 3.3 Verify the domain on GitHub

1. Back in the repo, go to **Settings → Pages**.
2. Under **Custom domain**, type your domain (e.g. `grahmtg.com`) and click **Save**.
3. Wait. GitHub will check the DNS records (this takes a few minutes after DNS has propagated, which itself takes 10 min – 24 hr).
4. Once verified, you'll see a green checkmark and a tickbox labelled **Enforce HTTPS**. Tick it. (You may need to wait another 10 minutes for HTTPS to be available — GitHub provisions a Let's Encrypt cert automatically.)

### 3.4 Verify the site is live

Visit `https://grahmtg.com` (or whatever your domain is). The site should load with a valid HTTPS certificate.

If you see "404," DNS hasn't propagated yet — wait 30 minutes and try again. Use [whatsmydns.net](https://whatsmydns.net) to check propagation status globally.

---

## Phase 4 — Migrate from Wix (the actual cutover)

You're not migrating *to* Wix — you're migrating *off* it. This phase ensures visitors who follow old Wix URLs end up at the right place.

### 4.1 Inventory your old URLs

From the original Wix site (`gpt172.wixsite.com/grahamtg`), the live pages were:

```
/                          → New: /
/about-8                   → New: / (anchor #about)
/research-and-projects     → New: /#work
/media-coverage            → New: / (or remove entirely)
```

If your old Wix URLs are linked to from anywhere meaningful (LinkedIn bio, published reports, etc.), you'll want either:

- **Option A:** update those links manually to the new domain. Simplest. Recommended for low-volume cases.
- **Option B:** set up automatic redirects from the Wix domain. Tricky because Wix doesn't natively support redirects, and you'll be cancelling Wix anyway.

For your situation (Wix site is on a free `wixsite.com` subdomain, no real SEO weight), **Option A is the right answer.** Update your LinkedIn, your CV, the contact info on any published reports, and call it done.

### 4.2 Cancel the Wix site

**Wait until you've verified the new site is live and you've updated all external links** before doing this. Wix is the fallback — keep it for at least two weeks after going live just in case something with the new site needs fixing.

When ready: log into Wix → Settings → cancel the site / let the free trial expire / disable publishing. The `gpt172.wixsite.com/grahamtg` URL will eventually return a "this site doesn't exist" page, which is fine.

### 4.3 Update your LinkedIn

Replace the old Wix URL in your LinkedIn bio with the new custom domain. Same for Twitter/X bio, Instagram, anywhere else your portfolio link appears.

---

## Phase 5 — Day-2 maintenance (web upload workflow)

You don't need git on the command line for any of this. Everything happens through the GitHub web interface.

### Editing copy

For small text changes (a single word, a sentence, fixing a typo):

1. In your repo, click the file you want to edit (e.g. `index.html`).
2. Click the pencil icon (top right of the file view) to edit in-browser.
3. Use `Cmd+F` (Mac) or `Ctrl+F` (Windows) to find the text you want to change.
4. Edit. Scroll to the bottom. Write a commit message describing what you changed. Click **Commit changes**.
5. The site redeploys automatically within ~30 seconds.

For larger edits (several paragraphs, multiple sections at once), the web editor gets clunky. Better to:

1. Click the file → click the **download icon** (the small arrow pointing down on the right side of the file view) or click **Raw** then `Cmd+S` to save.
2. Open the downloaded file in a text editor — [VS Code](https://code.visualstudio.com) is excellent and free, but TextEdit or Notepad work too.
3. Edit and save the file.
4. Back in GitHub, navigate to the file in the repo, click **Add file → Upload files**, drag your edited file in. GitHub will recognise it as an update to the existing file. Commit.

### Adding a new work entry

In `index.html`, find the `<div class="work-list" id="workList">` block. Each entry is a complete `<article class="work-item">` block. Copy an existing entry, paste it in the right position (entries are listed chronologically newest-first), and update:

- `data-cat` attribute: `applied`, `published`, or `research`
- `id` attribute: a unique slug like `work-newpiece` (so the Featured Research links can target it)
- The work-num, work-year, work-title, work-venue, work-cat values
- The detail body paragraphs
- The impact panel rows (dt/dd pairs)
- The tags

That's it. Commit, and the new entry shows up.

### Adding photos

1. In the repo, click **Add file → Upload files**.
2. Drag your photo file(s) in. To put them in a subfolder (recommended for organisation), type `images/` (with the slash) at the top of the upload area before dragging — GitHub will create the folder.
3. Optimise photos before uploading — keep each under 500KB. Use [squoosh.app](https://squoosh.app) for compression in your browser.
4. In the HTML, replace the placeholder slots with actual `<img>` tags or `background-image` styles pointing to `images/your-file.jpg`.

The placeholder slots are:

- `.hero-photo` — the big right-side hero photo
- `.about-portrait` — the secondary portrait in the About section
- `.photo-strip-item` — the six horizontally-scrolling thumbnails (currently labelled `[ Speaking · UN Geneva ]`, etc.)

### Reverting changes

If you break something:

1. Go to the **Commits** view of your repo (click on "commits" near the top of the file list, or visit `https://github.com/YOUR_USERNAME/REPO/commits/main`).
2. Find the bad commit. Click on it.
3. Click the **`...` menu** (three dots, top right of the commit page) → **Revert**.
4. GitHub creates a new commit that undoes the bad one. Confirm.

You can revert any commit, even ones from months ago. Git keeps the full history.

---

## Phase 6 — Optional polish later

Things you can add over time, none of which are urgent:

- **Real photos** in the placeholder slots (the single biggest visual upgrade).
- **Real thesis data** in the chart once your numbers are final.
- **Open Graph / Twitter Card meta tags** for nice link previews when shared.
- **Favicon** (a `favicon.svg` or `favicon.ico` in the repo root).
- **Analytics** — [Plausible](https://plausible.io) or [Cabin](https://withcabin.com) are privacy-respecting and easy to add. Avoid Google Analytics if you can.
- **A blog** — add a `posts/` folder with `.html` files for each post and a `posts/index.html` listing them. No CMS needed.

---

## Help and gotchas

**The site looks broken on the domain but fine on the GitHub Pages URL.**
DNS hasn't propagated, or the CNAME file is wrong. Wait an hour. Check `whatsmydns.net`.

**HTTPS isn't available on the custom domain.**
GitHub provisions Let's Encrypt automatically, but it takes ~15 min after the domain is verified. If it's been more than an hour, go to **Settings → Pages**, untick **Enforce HTTPS**, save, then re-tick it.

**The site loads but the fonts look wrong.**
Google Fonts is failing to load. Check the network tab in browser DevTools. Usually a temporary issue.

**I want to roll back to a previous version.**
See "Reverting changes" above. The commits view shows everything that's ever happened to the repo.

**I can't see my CNAME or .gitignore files in Finder.**
macOS hides dotfiles by default. Press `Cmd + Shift + .` (period) in Finder to toggle their visibility.

**I uploaded files but the site shows the README instead of my homepage.**
Either you uploaded the folder rather than its contents (so `index.html` is at `grahmgaydos-site/index.html` instead of the root), or GitHub Pages hasn't deployed yet. Check Settings → Pages to see the deployment status.

---

## Appendix — Using git on the command line (optional)

If you'd rather use git locally, the workflow is faster for large or frequent edits but requires one-time setup. Skip this if you're comfortable with the web upload approach.

### One-time setup

1. Install git: [git-scm.com/downloads](https://git-scm.com/downloads).
2. Set up SSH or use a personal access token for authentication ([GitHub's docs](https://docs.github.com/en/authentication)).
3. Clone your repo:

```bash
git clone git@github.com:YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME
```

### Editing workflow

```bash
# Edit files in your favourite editor
# Then:
git add .
git commit -m "Description of what you changed"
git push
```

That's the entire flow. The site redeploys within ~30 seconds of pushing.

### Reverting

```bash
git log                          # see recent commits
git revert <commit-hash>         # undo a specific commit (creates a new commit reversing it)
git push
```
