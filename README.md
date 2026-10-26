# saurabh-oss.github.io

Personal portfolio site for **Saurabh Srivastava** — Chief Architect, AI Innovation Lead, builder of open-source things.

🌐 **Live:** https://saurabh-oss.github.io

---

## 🎯 Business Objective

Build a personal "publishing surface" that turns **16 years of architecture experience** into a visible, compounding asset:

- **Positioning** — A clear, premium statement of who you are and what you stand for.
- **Trust** — A manifesto of architectural POV that converts visitors into followers and collaborators.
- **Distribution** — Auto-syncs your GitHub work, so every PoC you ship strengthens the page.
- **Zero ops cost** — Static site on GitHub Pages: free hosting, zero infra to maintain.

---

## 🏗️ Architecture

```
                     ┌──────────────────────┐
                     │    Visitor browser    │
                     └──────────┬───────────┘
                                │ HTTPS
                                ▼
                     ┌──────────────────────┐
                     │   GitHub Pages CDN    │  ← static hosting (free)
                     │   (static HTML/CSS)   │
                     └──────────┬───────────┘
                                │ fetch on load
                                ▼
                     ┌──────────────────────┐
                     │   GitHub REST API     │  ← live repo data + ForgeAI's
                     │ /users/saurabh-oss/   │     latest release (no auth,
                     │       repos           │      60 req/hr/IP)
                     └──────────────────────┘
                     ┌──────────────────────┐
                     │   PyPI JSON API       │  ← ArchiLens version and
                     │ /pypi/archilens/json  │     release count
                     └──────────────────────┘
```

**Stack — fully open source:**
- HTML5 + modern CSS (CSS variables, grid, container queries)
- Vanilla JS (no framework, no build step)
- Google Fonts (Fraunces serif + DM Sans + JetBrains Mono)
- GitHub Pages (free static hosting)
- GitHub REST API and PyPI JSON API (live project data and versions, no key required)

**Why no framework?**
- Zero build step → push and ship.
- Faster page loads (no JS framework runtime).
- Easier for anyone to fork and customize.
- The site itself demonstrates the "right tool for the job" architectural principle.

---

## 📁 File Structure

```
saurabh-oss.github.io/
├── index.html      # Single-page site (HTML + inline CSS + inline JS)
├── 404.html        # Custom 404 page in matching aesthetic
├── favicon.svg     # Browser tab icon
├── portrait.jpg    # Profile photo (800×1000, shown in the Profile section)
├── portrait-avatar.jpg  # Square face crop (192×192, shown in the hero)
├── og-card.jpg     # Link-preview image (1200×630 photo, face centred for square thumbnails)
├── robots.txt      # Crawler rules
├── sitemap.xml     # Sitemap for search engines
├── repos.json      # Snapshot of the public repo list, used when GitHub's API limit is hit
├── .nojekyll       # Tell GitHub Pages to skip Jekyll processing
└── README.md       # This file
```

---

## 🚀 Deployment — Step by Step

### Option A — User site (recommended): `saurabh-oss.github.io`

This gives you the clean URL **`https://saurabh-oss.github.io`** with no `/repo-name/` suffix.

#### 1. Create the repository

On GitHub, create a new public repo named **exactly**:

```
saurabh-oss.github.io
```

> ⚠️ The repo name must match `<your-username>.github.io` exactly for the user-site URL to work.

#### 2. Push the files

From your local machine:

```bash
# Initialize a fresh repo
mkdir saurabh-oss.github.io
cd saurabh-oss.github.io

# Copy the three files (index.html, 404.html, .nojekyll, README.md) into this folder

git init
git add .
git commit -m "Initial commit: portfolio site"

# Add remote (replace if you already have one)
git branch -M main
git remote add origin https://github.com/saurabh-oss/saurabh-oss.github.io.git
git push -u origin main
```

#### 3. Enable GitHub Pages

1. On GitHub, open the repo → **Settings** → **Pages** (left sidebar).
2. Under **Build and deployment**:
   - **Source**: `Deploy from a branch`
   - **Branch**: `main` / `(root)`
3. Click **Save**.
4. Wait ~30–60 seconds. Refresh the Pages settings — you'll see:
   > ✅ Your site is live at https://saurabh-oss.github.io

#### 4. (Optional) Custom domain

If you own a domain (e.g. `saurabhsrivastava.dev`):

1. In repo **Settings → Pages → Custom domain**, enter your domain.
2. At your DNS provider, add:
   - `A` record → `185.199.108.153`
   - `A` record → `185.199.109.153`
   - `A` record → `185.199.110.153`
   - `A` record → `185.199.111.153`
   - `CNAME` (`www`) → `saurabh-oss.github.io`
3. Tick **Enforce HTTPS** in Pages settings (after DNS propagates, ~10 min to a few hours).

### Option B — Project site (alternative)

If you'd rather keep a different repo name (e.g. `portfolio`):

1. Create repo `portfolio`, push the files.
2. Enable Pages from `main` / `(root)`.
3. Site lives at `https://saurabh-oss.github.io/portfolio/`.

---

## 🧪 Local Preview

You don't *need* a server — just open `index.html` in a browser. But for a production-faithful preview (especially because the GitHub API call uses `fetch` which behaves better over HTTP):

```bash
# Python 3 (built-in)
cd saurabh-oss.github.io
python3 -m http.server 8000
# → http://localhost:8000

# OR Node.js (if installed)
npx serve .
```

---

## ✏️ Customizing Content

All content lives in `index.html`. Open it and search for these markers:

| What to change | Where |
|---|---|
| **Name and role** | `<div class="hero-eyebrow">` |
| **Photo** | Replace `portrait.jpg` (4:5) and `portrait-avatar.jpg` (square face crop) — same filenames, no code change |
| **Hero headline** | `<h1 class="hero-title">` — one `<span class="w">` per word, numbered with `--i` for the staggered reveal |
| **One-line positioning** | `<p class="hero-sub">` |
| **Hero 3D stack** | `layers` array in `buildStack()` — layer names, sub-labels and node positions |
| **"Published on" ribbon** | `<div class="registry">` in the hero — one `<li>` per official listing (logos are [Simple Icons](https://simpleicons.org), CC0) |
| **Live versions** | Any element with `data-live="archilens-version"`, `"archilens-releases"` or `"forgeai-release"` is filled from the registries on load; the text in the HTML is the fallback |
| **Bio paragraphs** | `<div class="about-text">` |
| **Stats numbers** | `<div class="stats">` — set both the text and `data-count` (the repo count updates itself from GitHub) |
| **Expertise / domains** | `<div class="expertise-grid">` — four `.exp-card` blocks |
| **Manifesto items** | `<div class="manifesto">` — five `.manifesto-item` blocks (also update the `/ 05` total beside them) |
| **Flagship projects** | The three `.plugin-spotlight` blocks (ForgeAI, TraceGuard AI, ArchiLens); their animated panels are driven by the `runDemo(...)` calls |
| **Repo carousel** | Built from the live repo list by `carousel.load()`; `const FEATURED = [...]` names the repos tagged "Featured" |
| **Contact links** | The LinkedIn `.btn` and the `.social` row in `<section id="contact">` |
| **GitHub username** | `const GH_USER = 'saurabh-oss';` near the bottom |

### Color theme

All colors are CSS variables at the top of `<style>`. Swap these to re-skin the entire site:

```css
:root {
  --bg:        #0c0b09;   /* page background */
  --text:      #f1ede4;   /* primary text     */
  --accent:    #d4a574;   /* warm gold accent */
  --muted:     #a8a092;   /* secondary text   */
  /* ... */
}
```

### Fonts

The site loads three fonts from Google Fonts. To swap, change the `<link href="https://fonts.googleapis.com/...">` line and the `--serif`, `--sans`, `--mono` variables.

---

## 🧠 How the GitHub Repo Section Works

The site fetches your live repos from:

```
https://api.github.com/users/saurabh-oss/repos?sort=updated&per_page=100
```

It then:

1. **Filters** — leaves out forks of other people's projects; everything else goes in the carousel, most-starred first.
2. **Renders** — a 3D coverflow card per repo: language, name, description, stars, forks, last update. Repos in `FEATURED` get a "Featured" tag.
3. **Groups** — language filter chips (Python, TypeScript, JavaScript, Java, Other) with counts.
4. **Counts** — the total feeds the "Public Repos" stat and the "All N repositories on GitHub" link.

Visitors can drag or swipe (a flick carries on for a few cards), use a sideways trackpad swipe, the arrow buttons or arrow keys, or click a side card to bring it to the centre; clicking the centre card opens the repo. It advances on its own every few seconds while on screen, and pauses on hover, focus, or for 8 seconds after any interaction.

**Rate limit:** the unauthenticated GitHub API allows 60 requests per hour per IP address, and everyone behind one office network shares that allowance. So the page:

1. **Caches** each successful answer in the visitor's browser for 6 hours (`cachedJSON()`), so reloads and return visits make no API calls.
2. **Falls back** to `repos.json`, a snapshot published with the site, when GitHub refuses. The cards always render; only star counts can lag.

Refresh the snapshot now and then (needs the GitHub CLI, logged in):

```bash
gh api "users/saurabh-oss/repos?per_page=100&sort=updated" --paginate --jq '[.[] | {name, html_url, description, language, stargazers_count, forks_count, fork, updated_at}]' > repos.json
```

### To tag different projects as featured

Edit the `FEATURED` list near the bottom of `index.html`:

```js
// Highlighted in their own cards above; tagged in the carousel
const FEATURED = ['forgeai-pipeline-intelligence', 'traceguard-ai', 'archilens'];
```

---

## 🎞️ Motion

All animation is plain CSS (including the 3D, via `transform-style: preserve-3d`), SVG and a little vanilla JS — no libraries, no WebGL.

- **Hero 3D stack** — four real 3D layers built by `buildStack()`. They drop into place on load, sway slowly, turn toward the cursor, carry pulses between layers, and pull apart as you scroll past.
- **"Published on" ribbon** — the official listings loop as a marquee (paused on hover); each logo turns over in 3D on hover.
- **Manifesto** — pinned while you scroll; statements turn on a 3D drum, lit word by word (`updateManifesto()`).
- **Project highlights** — cards with `data-tilt` lean toward the cursor and the demo panel lifts off the card; `runDemo()` steps through each panel and lands a verdict, only while it is on screen.
- **Domain cards** — swing down into place in 3D as they reveal.
- **Small touches** — stat count-up, scroll-progress hairline, active nav link, cursor glow on cards.

Visitors with **"reduce motion"** enabled in their OS get the static version: no pinning, no pulses, no tilt, everything in its final state.

---

## 🔄 Roadmap / Ideas to Extend

- **Blog section** — Add a `/posts/` folder with markdown rendered via [marked.js](https://marked.js.org/) client-side, or migrate to Astro for SSG.
- **Live AI demos** — Embed Hugging Face Spaces or your own deployed PoCs as `<iframe>`s under each repo card.
- **Newsletter capture** — Replace one Contact card with a Buttondown / ConvertKit form.
- **OG image generator** — Use [satori](https://github.com/vercel/satori) in a GitHub Action to auto-generate a unique social card per project.
- **Analytics** — Add [Plausible](https://plausible.io) (privacy-first, lightweight) before the closing `</body>` tag.

---

## 📜 License

MIT — fork it, remix it, ship your own.

---

**Built in the open by Saurabh Srivastava.**
[LinkedIn](https://www.linkedin.com/in/saurabh-tcs/) · [X / Twitter](https://x.com/sauvast) · [GitHub](https://github.com/saurabh-oss)
