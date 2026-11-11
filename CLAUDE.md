# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Static marketing website for **Skill Engineering** (engineering, training, video and technical writing). Content is in French (`<html lang="fr">`). Plain HTML/CSS/jQuery, with no package manager, build step, tests or linter.

## Running locally

Open the HTML files directly, or serve the repo root with any static server. The project is set up for the VS Code Live Server extension on port 5501 (`.vscode/settings.json`).

## Architecture

- **Four standalone pages**: `index.html`, `a-propos.html`, `services.html` and `contact.html`. There are no templates or includes, so the header/nav (including the "Nos formations" dropdown) and the footer are **identical copies in every page**. A nav or footer change has to be made in all four files. The only difference between pages is which `<li>` has `class="current_page_item"` and `aria-current="page"`.
- **Single stylesheet**: `assets/css/main.css`. There is no Sass or preprocessor, and pages have no inline `<style>` blocks. The file has two parts:
  1. The base theme (reset, grid, typography, nav, wrappers, and the responsive rules at 1280/980/736px).
  2. The **"Skill Engineering — Custom styles"** section at the end: CSS variables in `:root`, cards (`.box.*`, `.card`), the home sector grid (`.sector`), the services grid (`.services-grid` / `.service-card`), the contact form (`.contact-card`) and the mobile `#titleBar` overrides.

  Put new styles in the custom section.
- **Grid**: `.row` + `.col-N`, with responsive variants `col-N-large|medium|small` and `imp-medium` (moves a column first at medium width).
- **Responsive breakpoints** (xlarge 1281–1680, large 981–1280, medium 737–980, small ≤736) are defined in both `main.css` and `assets/js/main.js`, so keep them in sync.
- **Navigation on narrow screens**: at ≤980px the CSS hides `#header-wrapper`. `assets/js/main.js` then builds `#titleBar` and the off-canvas `#navPanel` from `#nav` at runtime, so mobile nav comes from the same markup. Page content must sit inside `#main-wrapper`, whose top padding keeps it clear of the fixed title bar.
- **JS**: the other files in `assets/js/` are vendored minified libraries plus `util.js` (the `navList()`/`panel()` helpers). Do not strip their MIT license headers. Scripts load at the end of `<body>` in a fixed order: jquery → dropotron → browser → breakpoints → util → main.
- **Contact form** posts to FormSubmit (`https://formsubmit.co/sez@skillengineering.fr`). There is no backend.
- **Images** live in `images/`. Some filenames contain spaces and accented characters, so URL-encode them in `src` (e.g. `Supply-chain%20attacks.webp`).

## Conventions

- Source files (HTML, CSS, JS) start with a header comment crediting developer Wajih Jabri and "Copyright (c) 2026 Skill Engineering. All rights reserved." Keep it when editing files, and add it to new source files.
