# Gemini Watermark Remover

A free, browser-based tool that removes the visible 4-point star watermark Google Gemini adds to AI-generated images. No server, no uploads — everything runs locally in your browser.

**Live site:** https://watergone.vercel.app

---

## What It Does

Google Gemini automatically adds a small semi-transparent star (✦) watermark to every AI-generated image in the bottom-right corner. This tool mathematically reverses the blending operation to recover the original pixels — pixel-perfect, zero quality loss.

---

## How It Works

The Gemini watermark is applied as a semi-transparent white overlay using alpha compositing:

```
watermarked = alpha × 255 + (1 - alpha) × original
```

By capturing the watermark's alpha map (the exact transparency pattern), we can reverse this equation:

```
original = (watermarked - alpha × 255) / (1 - alpha)
```

This is applied per pixel, per color channel, only in the watermark region (bottom-right corner).

---

## Features

| Feature | Detail |
|---|---|
| 100% client-side | Images never leave your device |
| Batch processing | Drop multiple files at once |
| Auto-detect size | Handles 48×48 (standard) and 96×96 (large images >1024px) watermarks |
| Lossless output | Saves as PNG — no compression artifacts |
| EXIF stripped | Canvas processing removes all metadata automatically |
| Dark / Light / System | Theme toggle with localStorage persistence |
| Before/After slider | Interactive comparison on processed image |

---

## File Structure

```
watergone/
│
├── index.html              ← Full app (HTML + CSS + JS in one file)
│
├── assets/
│   ├── bg_48.png           ← Alpha map for 48×48 watermark (standard images)
│   ├── bg_96.png           ← Alpha map for 96×96 watermark (large images >1024px)
│   ├── before.png          ← Demo image with watermark (for slider example)
│   ├── after.png           ← Demo image cleaned (for slider example)
│   └── seo-image.jpg       ← Open Graph image for social sharing previews
│
├── .gitignore              ← Ignores OS files (.DS_Store, Thumbs.db)
├── robots.txt              ← Allows all crawlers, points to sitemap
├── sitemap.xml             ← Sitemap for SEO
└── README.md               ← This file
```

---

## Run Locally

This is a plain HTML project — no build step, no npm, no dependencies to install.

### Option 1 — VS Code Live Server (recommended)

1. Open VS Code → **File → Open Folder** → select your project folder
2. Install the **Live Server** extension (`Ctrl + Shift + X` → search "Live Server")
3. Right-click `index.html` → **Open with Live Server**
4. Opens automatically at `http://127.0.0.1:5500`

### Option 2 — Python

```bash
python -m http.server 8080
```
Then open `http://localhost:8080`

### Option 3 — Node.js

```bash
npx serve .
```

> ⚠️ **Do not open `index.html` by double-clicking.** The browser blocks local file requests (`file://`), which means `bg_48.png` and `bg_96.png` alpha maps won't load and the tool won't work. Always use a local server.

---

## Deploy to GitHub Pages

### Step 1 — Create GitHub repo

1. Go to https://github.com/new
2. Name your repo (e.g. `watermark-remover`)
3. Set to **Public**
4. Click **Create repository**

### Step 2 — Upload files

Upload all files keeping the same structure:
```
index.html               → root
assets/bg_48.png         → assets folder
assets/bg_96.png         → assets folder
assets/before.png        → assets folder
assets/after.png         → assets folder
assets/seo-image.jpg     → assets folder
README.md                → root
.gitignore               → root
```

### Step 3 — Enable GitHub Pages

1. Go to repo → **Settings → Pages**
2. Source: **Deploy from branch**
3. Branch: **main** → folder: **/ (root)**
4. Click **Save**

Your site will be live at:
```
https://watergone.vercel.app
```

---

## Deploy to Vercel (optional)

1. Go to https://vercel.com → sign in with GitHub
2. Click **Add New → Project**
3. Import your repo
4. Framework preset: **Other** (plain HTML)
5. Click **Deploy**

Vercel auto-redeploys on every GitHub push. ✅

---

## Customize

### Change the accent color

In `index.html`, find the `:root` CSS block and update `--accent`:

```css
:root {
  --accent: #e84d1c;   /* change this to any color */
}
```

### Update your GitHub link

Search for `devmelife` in `index.html` and replace with your GitHub username.

### Update the canonical URL

If you're deploying to a custom domain, update these in `<head>`:

```html
<link rel="canonical" href="https://watergone.vercel.app/">
<meta property="og:url" content="https://watergone.vercel.app/">
<meta property="og:image" content="https://watergone.vercel.app/assets/seo-image.jpg">
<meta name="twitter:image" content="https://watergone.vercel.app/assets/seo-image.jpg">
```

And in `sitemap.xml` (if keeping it):
```xml
<loc>https://watergone.vercel.app/</loc>
```

---

## Tech Stack

| Layer | Detail |
|---|---|
| Language | HTML + CSS + Vanilla JavaScript |
| Icons | Lucide Icons v0.468.0 (via unpkg CDN) |
| Fonts | Plus Jakarta Sans + JetBrains Mono (Google Fonts) |
| Image processing | Browser Canvas API |
| Hosting | GitHub Pages or Vercel |
| Build tool | None — single HTML file |

---

## Credits

Based on the original work by:
- [GargantuaX](https://github.com/GargantuaX/gemini-watermark-remover)
- [allenk](https://github.com/allenk/GeminiWatermarkTool)

Built and redesigned by [devmelife](https://github.com/devmelife)

---

## License

MIT — free to use, modify, and distribute.