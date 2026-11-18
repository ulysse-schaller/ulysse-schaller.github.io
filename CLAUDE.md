# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Ulysse Schaller's personal website: one static page (`index.html` + `style.css` + `assets/`), deployed as a GitHub Pages user site at https://ulysse-schaller.github.io from the root of `main`. There is no build step, package manager, linter or test suite. `.nojekyll` makes Pages serve every tracked file as-is, so anything committed here (this file included) is publicly reachable on the site.

## Commands

Preview locally, then open http://localhost:8765:

```bash
python3 -m http.server 8765
```

In the Claude desktop app, `preview_start` with name `site` runs the same server (config in `.claude/launch.json`, which is gitignored).

Deploy by pushing to `main`; GitHub Pages republishes within a minute or two.

Check that the portrait is untouched (must print `9216d8a378fd85611aa544ba88b290f8fff8742fd3d41e8c8dbf6100150da1d8`):

```bash
shasum -a 256 assets/ulysse-schaller.jpg
```

## Rules Ulysse set

- **`assets/ulysse-schaller.jpg` must stay byte-identical** to the original (704×704, 52095 bytes). Never re-encode, resize, crop or strip its metadata; do any sizing or rounding in CSS.
- **The "about" text in `.about` is verbatim**, written by Ulysse. Don't reword, re-punctuate or "fix" it (straight apostrophes included). Project titles and link labels ("LessWrong post", "Paper", "Article in Die Zeit") are Ulysse's wording too.
- **Bump the footer date on every change to the page** (index.html, style.css or assets): both the attribute and the visible text of `<p class="updated">Last updated on <time datetime="2026-11-16">16 November 2026</time></p>`.
- Settled choices, not to be undone without asking: Roboto (sans-serif), justified body text, projects stacked vertically as cards with all of a project's links on one line, centred footer icon buttons, LessWrong's compass logo as its icon, ☸️ as the tab icon, and "AI Safety researcher" as the meta and link-preview description.

## How the pieces fit together

- **Theme tokens.** Every colour is a custom property on `:root` in `style.css`, and a `prefers-color-scheme: dark` block redefines the same tokens. `--body-weight` also differs between modes (350 light, 300 dark), which works because Roboto is a variable font. The accent (`--accent`, a burgundy-brown) was picked from the photo. Add new colours as tokens in both blocks.
- **No third-party requests.** Roboto is self-hosted (`assets/fonts/roboto-latin-wght-normal.woff2`: latin subset, weights 100–900, OFL licence next to it) and preloaded from `index.html`. The preload needs `crossorigin`, or the font downloads twice. Don't add externally hosted fonts, CDNs, analytics or embeds. Characters outside the latin subset fall back to the system font.
- **Footer icons** are inline SVGs with `fill: currentColor`, so they follow the theme and hover colours. Envelope, LinkedIn and Google Scholar come from Font Awesome Free 6.7.2; keep the CC BY 4.0 attribution comment in the footer. The LessWrong compass is LessWrong's own logo, drawn larger (`svg.compass`, 25px against 19px) so it looks the same size.
- **One-line project links.** `.project-links` is `flex-wrap: nowrap` on purpose. The 720px and 400px breakpoints shrink the link chips (the 400px one with `clamp()`) so three links still fit on a 320px screen. After adding a project or a long link label, check at 320px that the links stay on one line and the page doesn't scroll sideways.
- **Justified text** uses `hyphens: auto` on `.about p`, which relies on `lang="en"` on `<html>`. Links get `hyphens: manual` so a hyphen never splits a link.
- **Link previews** come from the Open Graph and Twitter meta tags in `<head>`. `og:image` is `assets/og-image.jpg`, a separate 1200×630 card (name, "AI Safety researcher", site URL and a copy of the photo, drawn on a canvas) so platforms never crop the portrait itself. If the name, tagline, photo or accent colour change, regenerate it at the same size and keep `og:image:width`/`og:image:height` in sync. `og:image` and `og:url` must be absolute URLs.
- **Favicons.** `assets/favicon.svg` draws ☸️ as SVG text, so it shows the visitor's own system emoji. `assets/apple-touch-icon.png` (180×180) is a pre-rendered bitmap of the Apple emoji on a purple gradient, because iOS home-screen icons must be PNG.

## Checking a change

Look at the page at about 1280px, 375px and 320px wide, and in dark mode. At 720px and below the layout becomes one column with the portrait above the name.
