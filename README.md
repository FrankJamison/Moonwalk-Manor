# Moonwalk Manor

A single-page, static marketing site for **Moonwalk Manor** (a fictional “hotel on the moon” concept). This repo can be opened directly in a browser, or served by any simple static file server.

## Links

- **Live preview**: https://moonwalkmanor.fcjamison.com/
- **Local preview (VS Code task default)**: http://moonwalkmanor.localhost/

> Change note: the VS Code **Open in Browser** task now targets `http://moonwalkmanor.localhost/`.

## What’s in here

- **One-page layout**: All content lives in `index.html`.
- **Responsive images**: A small script generates `srcset` + `sizes` for every `<img>`.
- **FlexSlider carousels**:
  - A “hero” promo slider at the top.
  - A “Tales from the Moon” testimonial/story slider.
- **Multi-level navigation**: A dropdown toggle script enables the sub-menu on smaller screens.
- **Icon system (Grunticon)**: SVG/PNG icon CSS is loaded dynamically with a fallback.

## Quick start

### Option A: Open the file directly

1. Open `index.html` in a modern browser.
2. If images/icons don’t load when using the `file://` protocol, use Option B (a local static server) instead.

### Option B: Serve as a static site (recommended)

Use any static server you like. Examples:

- **Python**: `python -m http.server 8080`
- **Node**: `npx serve .`

Then open the server URL shown in your terminal.

If you want to keep using the `moonwalkmanor.localhost` hostname, you can typically access the same server via the same port, e.g. `http://moonwalkmanor.localhost:8080/`.

### VS Code task

This workspace includes a VS Code task named **“Open in Browser”** (see [.vscode/tasks.json](.vscode/tasks.json)).

- Default URL: `http://moonwalkmanor.localhost/`
- If your local server uses a different host/port, update the task’s URL (for example: `http://localhost:8080/`).

## Project structure

```
.
├─ index.html
├─ favicon.ico
├─ css/
│  └─ style-main.css
├─ JS/
│  ├─ script.js
│  ├─ flexslider.js
│  ├─ menu-toggle.js
│  └─ libs/
│     ├─ jquery-2.1.3.min.js
│     ├─ jquery.flexslider-min.js
│     └─ flexibility.js
├─ gfx/
│  ├─ grunticon.loader.js
│  ├─ icons.data.svg.css
│  ├─ icons.data.png.css
│  ├─ icons.fallback.css
│  ├─ preview.html
│  └─ png/
└─ images/
   ├─ experience/
   ├─ mainpromo/
   └─ testimonials/
```

## Key files and how they work

### HTML entry point

- `index.html`
  - Loads the main stylesheet: `css/style-main.css`
  - Loads scripts:
    - `JS/script.js` (deferred) – responsive image attributes
    - `JS/libs/flexibility.js` – flexbox polyfill
    - Footer scripts:
      - `JS/libs/jquery-2.1.3.min.js`
      - `JS/libs/jquery.flexslider-min.js`
      - `JS/menu-toggle.js`
      - `JS/flexslider.js`
  - Includes Grunticon loader inline, which loads:
    - `gfx/icons.data.svg.css` (preferred)
    - `gfx/icons.data.png.css`
    - `gfx/icons.fallback.css`

### Styling

- `css/style-main.css`
  - Includes a reset section at the top.
  - Contains layout and component styles for headers, sliders, content blocks, and footer.

### Responsive images (`JS/script.js`)

This script:

1. Selects **all** `<img>` elements on the page.
2. Assumes each image `src` ends with **`-800.jpg`**.
3. Removes that suffix to get a base path.
4. Generates a `srcset` with 5 widths:
   - `-400.jpg`, `-800.jpg`, `-1200.jpg`, `-1600.jpg`, `-2000.jpg`
5. Sets the `sizes` attribute based on `data-type`:

| data-type  | sizes value                       |
| ---------- | --------------------------------- |
| `showcase` | `100vw`                           |
| `reason`   | `(max-width: 799px) 100vw, 372px` |
| `feature`  | `(max-width: 799px) 100vw, 558px` |
| `story`    | `(max-width: 799px) 100vw, 670px` |

If you add new images, make sure you:

- Keep the naming convention (at minimum `*-800.jpg`, ideally also the other generated sizes).
- Add the correct `data-type` to match one of the entries above.

### Navigation dropdown toggle (`JS/menu-toggle.js`)

- Adds click behavior to `.dropdown-toggle`.
- Toggles CSS classes (`toggle-on`, `toggled-on`) and updates `aria-expanded`.

### Sliders (`JS/flexslider.js`)

Initializes FlexSlider instances:

- `.front-slider`
  - `animation: "slide"`
  - `controlNav: false`
  - `directionNav: false`

- `.story-slider`
  - `animation: "slide"`
  - `controlNav: true`
  - `directionNav: true`

## Dependencies (vendored)

This project vendors front-end libraries directly in `JS/libs/`:

- jQuery 2.1.3
- FlexSlider 2.6.1
- Flexibility (Flexbox polyfill)
- Grunticon loader + generated icon CSS in `gfx/`

No package manager (npm/yarn) is required for this repo.

## Editing content

Most edits are straightforward:

- Text and sections: edit `index.html`
- Layout/visual styling: edit `css/style-main.css`
- Slider behavior: edit `JS/flexslider.js`
- Menu behavior: edit `JS/menu-toggle.js`
- Responsive image behavior: edit `JS/script.js`

## Common issues

- **Broken images**: ensure each `src` ends with `-800.jpg` and the corresponding image files exist.
- **Icons not showing**: confirm the `gfx/icons.*.css` files are present and the Grunticon loader is running (or rely on the `<noscript>` fallback).
- **Dropdown not working**: make sure jQuery is loading and `JS/menu-toggle.js` is included.

## License / attribution

This repo contains vendored third-party libraries (jQuery, FlexSlider, Flexibility, Grunticon loader). Each library retains its own licensing and attribution as indicated in its header comments.
