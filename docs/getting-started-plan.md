# Design Mockup System for Take-off

## Context

The Take-off project has detailed requirements but zero implementation so far. The goal is to rapidly experiment with visual design options — layouts, colour schemes, interactive element styles — and share them for feedback before writing production code.

## HTML Mockups vs PNG Images

**Recommendation: HTML mockups**, for these concrete reasons:

- Real CSS allows layout options to be evaluated properly — a static image lies about spacing and overflow
- Colour scheme experiments are a 2-line change in one CSS file when you use CSS custom properties; with PNGs you redraw everything
- Modals, hover states, and active/disabled button states can be shown interactively in HTML but require multiple images per state in PNG
- The files live in the repo, are version-controlled, and can be shared via GitHub Pages with no extra tooling
- The HTML becomes the visual specification the implementation will be built against

PNGs are only better for very rough initial sketches or when you need to annotate with arrows/notes — but for this stage, HTML is clearly stronger.

## Proposed Structure

Each concept gets its own self-contained subfolder with multiple colour themes. A top-level hub links to all of them.

```
design/
├── index.html                  # hub — links to concept-1, concept-2, etc.
├── shared/
│   └── reset.css               # minimal CSS reset shared by all concepts
├── concept-1/
│   ├── layout.css              # layout, spacing, component structure (theme-independent)
│   ├── themes/
│   │   ├── light.css           # colour scheme: light/neutral
│   │   ├── dark.css            # colour scheme: dark
│   │   └── brand.css           # colour scheme: branded/accent colour
│   ├── page-1.html              # links a theme via <link> tag; swap to change theme
│   ├── page-2.html
│   ├── page-3.html
│   ├── page-4.html
│   ├── page-5.html
├── concept-2/
│   └── ...                     # same structure, different layout approach + themes
└── concept-3/
    └── ...
```

**How theming works:** Each HTML file loads `layout.css` plus one theme file (e.g. `themes/light.css`). Theme files only override CSS custom properties (`--color-primary`, `--color-surface`, etc.) — no layout rules. Switching theme = changing one `<link>` line. A theme switcher widget in the HTML lets you toggle themes in the browser without editing files.

## Implementation Order

1. **`shared/reset.css`** — Minimal normalisation only.
2. **`concept-1/layout.css`** — Define CSS custom properties for layout/spacing and all component structure.
3. **`concept-1/themes/`** — Three colour schemes: `light.css`, `dark.css`, `brand.css`.
4. **All pages for concept-1** — Each page loads `layout.css` + a default theme, plus a small theme-switcher widget. Build main drawing display first, then add other capabilities.
5. **`design/index.html`** — Hub listing all concepts with brief layout descriptions.
6. **`concept-2/`** — Second layout approach with its own theme set.
7. **`concept-3/`** — Third approach if desired.

## Sharing for Feedback

Enable GitHub Pages on the repo (`main` branch, `/design` directory or a `gh-pages` branch). Anyone with the URL can click through all concepts without installing anything. Link to `design/index.html` for the full overview, or to `design/concept-N/page-x.html` for a specific page.
