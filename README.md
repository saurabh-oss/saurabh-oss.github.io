# saurabh-oss.github.io

Personal portfolio site for **Saurabh Srivastava** — Senior IT Architect, AI Innovation Lead, builder of open-source things.

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
                     │   GitHub REST API     │  ← live repo data
                     │ /users/saurabh-oss/   │     (no auth needed,
                     │       repos           │      60 req/hr/IP)
                     └──────────────────────┘
```

**Stack — fully open source:**
- HTML5 + modern CSS (CSS variables, grid, container queries)
- Vanilla JS (no framework, no build step)
- Google Fonts (Fraunces serif + DM Sans + JetBrains Mono)
- GitHub Pages (free static hosting)
- GitHub REST API (live project data, no key required)

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
| **Hero name** | `<h1>` block — line ~ in `<section class="hero">` |
| **One-line positioning** | `<p class="hero-sub">` |
| **Bio paragraphs** | `<div class="about-text">` |
| **Stats numbers** | `<div class="stat-num">` (3 of them) |
| **Expertise / domains** | `<div class="expertise-grid">` — six `.exp-card` blocks |
| **Manifesto items** | `<div class="manifesto">` — five `.manifesto-item` blocks |
| **Contact links** | `<div class="contact-grid">` — three `.contact-card` blocks |
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

1. **Sorts** — non-forks first → most stars → most recently updated.
2. **Renders** — name, description, primary language, stars, forks.
3. **Animates in** — each card uses `IntersectionObserver` for scroll-reveal.

**Rate limit:** unauthenticated GitHub API allows 60 requests/hour per IP. For a personal portfolio that's far more than enough.

### To highlight specific projects

If you want to feature only certain repos, edit the `loadRepos()` function in `index.html`:

```js
// Pin specific repos by name
const PINNED = ['my-flagship-poc', 'ai-architecture-patterns'];
repos.sort((a, b) => {
  const ai = PINNED.indexOf(a.name);
  const bi = PINNED.indexOf(b.name);
  if (ai !== -1 || bi !== -1) {
    return (ai === -1 ? 999 : ai) - (bi === -1 ? 999 : bi);
  }
  // ... existing sort
});
```

Or filter to only show repos with descriptions:
```js
const filtered = repos.filter(r => r.description && !r.fork);
```

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
