# Notes: Day/Night palette toggle CSS

This document explains how the header palette (day/night) toggle is styled so you can tweak it later.

## DOM structure

No extra wrapper exists in the theme. The hierarchy is:

```
header.md-header
  └── nav.md-header__inner
        └── form.md-header__option[data-md-component="palette"]   ← grandparent
              ├── input#__palette_0 (radio, scheme slate)
              ├── label[for="__palette_1"].md-header__button.md-icon   ← parent (visible in Night)
              │     └── svg (Material: toggle-switch-off-outline)
              ├── input#__palette_1 (radio, scheme default)
              └── label[for="__palette_0"].md-header__button.md-icon   ← parent (visible in Day)
                    └── svg (custom icon with .palette-track, .palette-thumb)
```

- **Grandparent** = the `form` with `data-md-component="palette"`.
- **Parent** = the visible `label` (one per scheme; the other is hidden with `visibility: hidden` + `clip`).
- **Toggle** = the `svg` inside that label (track/lozenge + thumb/circle).

The theme normally gives both labels the `hidden` attribute. We override that via a custom partial so we control visibility with CSS.

## Files involved

| File | Role |
|------|------|
| `mkdocs.yml` | `theme.palette`: two entries (slate, default). Day mode uses `icon: custom/toggle-switch-day`. |
| `docs/custom_theme/partials/palette.html` | Palette partial override: labels rendered **without** `hidden` so one label is shown per scheme via CSS. |
| `docs/custom_theme/.icons/custom/toggle-switch-day.svg` | Custom icon for Day mode only: two paths with `class="palette-track"` (lozenge) and `class="palette-thumb"` (circle) so we can style track and thumb separately. |
| `docs/stylesheets/newstyle.css` | All palette toggle CSS (loaded as `extra_css`). |

## How the CSS works

### Scheme detection

- **Night (dark):** `body[data-md-color-scheme="slate"]` or `[data-md-color-scheme="slate"]`.
- **Day (light):** `[data-md-color-scheme="default"]` (or `body[data-md-color-scheme="default"]`).

The theme sets `data-md-color-scheme` on `body` from the active palette.

### Overriding the theme

The theme has:

- `.md-header .md-header__option { max-width: 0; opacity: 0 }` (hides the palette form by default).
- Labels in the palette partial are output with the `hidden` attribute.

We override with more specific selectors and `!important` so the form and the correct label are visible. We hide the “other” label per scheme with `visibility: hidden`, `opacity: 0`, `position: absolute`, `width/height: 0`, and `clip: rect(0,0,0,0)`.

### Making form and label invisible

So only the **toggle graphic** is visible (no visible container):

- **Night:** Form and the visible label use `background-color: var(--md-default-bg-color, #111315)` to match the header (same as `[data-md-color-scheme="slate"] .md-header`).
- **Day:** Form and the visible label use `background-color: var(--md-primary-fg-color, #000)` to match the header. No border on the label.

### Which label is visible

- **Night:** Only `label[for="__palette_1"]` is shown (clicking it switches to Day / default).
- **Day:** Only `label[for="__palette_0"]` is shown (clicking it switches to Night / slate).

The “other” label is hidden with the rules above so only one control is visible.

### Day-mode icon (custom SVG)

Only in Day mode do we use the custom icon (from `icon: custom/toggle-switch-day`). Its paths have classes so we can style them in CSS:

- **`.palette-track`** (lozenge): `fill: #000`, `stroke: #c0c0c0`, `stroke-width: 1.5` (thicker light gray border).
- **`.palette-thumb`** (circle): `fill: #e0e0e0`, `stroke: #000`, `stroke-width: 0.5`.

Night mode uses the Material icon `toggle-switch-off-outline`; we don’t override its path styling, only the label (background, color).

### Key selectors (in order in newstyle.css)

1. **Form (grandparent)**  
   `.md-header .md-header__inner [data-md-component="palette"].md-header__option`  
   Layout (flex, size, padding, radius). Then scheme-specific background so it matches the header.

2. **All palette labels (base)**  
   `.md-header__inner [data-md-component="palette"] label.md-header__button`  
   Size, flex, visibility, opacity (overrides theme).

3. **All palette label SVGs**  
   `.md-header__inner [data-md-component="palette"] label.md-header__button svg`  
   `opacity: 1`, `fill: currentColor` (for Night icon).

4. **Night – visible label**  
   `body[data-md-color-scheme="slate"] ... label[for="__palette_1"]`  
   Background = header; color for icon.

5. **Night – hidden label**  
   `body[data-md-color-scheme="slate"] ... label[for="__palette_0"]`  
   Hidden (visibility, clip, etc.).

6. **Day – visible label**  
   `[data-md-color-scheme="default"] .md-header ... label[for="__palette_0"]`  
   Background = header; no border.

7. **Day – icon parts**  
   `[data-md-color-scheme="default"] ... label[for="__palette_0"] svg .palette-track` and `... svg .palette-thumb`  
   Fill and stroke for lozenge and circle.

8. **Day – hidden label**  
   `[data-md-color-scheme="default"] ... label[for="__palette_1"]`  
   Hidden (visibility, clip, etc.).

## Tweaking

- **Border thickness (Day lozenge):** Change `stroke-width` on `.palette-track` (e.g. 1.5 → 2).
- **Colors (Day track/thumb):** Edit `fill` and `stroke` on `.palette-track` and `.palette-thumb`.
- **Night icon color:** Change `color` on `label[for="__palette_1"]` (and `:hover` for accent).
- **Form/label visible again:** Set a different `background-color` (e.g. a light gray) on the form or label for that scheme instead of the header variable.
- **Different icon in Day:** Add another SVG under `docs/custom_theme/.icons/` and point `theme.palette` (default scheme) at it; add classes to its paths if you need per-part styling.

---

## ReDoc: parameter section expand/collapse (carets)

The carets next to “Query parameters”, “Request body”, “Responses”, etc. in the operation panel **expand** on first click but **do not collapse** on second click. The same behavior occurs on [ReDoc’s official demo](https://redocly.github.io/redoc/), so it is not specific to our spec or CSS.

### Intended behavior (from ReDoc’s own design)

- **Accordion convention:** In [Redocly/redoc#975](https://github.com/Redocly/redoc/issues/975) the maintainer aligned the section icons with the left nav: **right arrow = collapsed**, **down arrow = expanded**. So the design intent is that these are **accordion toggles** — one click expands, another click collapses.
- **Expand/Collapse all:** [Issue #1286](https://github.com/Redocly/redoc/issues/1286) and [PR #1424](https://github.com/Redocly/redoc/pull/1424) add “Expand all” / “Collapse all” *buttons* for nested content; those are separate from the section-header carets. The section-level caret is meant to show/hide the whole block (Parameters, Request body, etc.).
- **Conclusion:** The **intended** behavior is that the section caret **toggles** (expand and collapse). The current **actual** behavior (expand only, both on the official demo and our docs) is either a long-standing bug or an unimplemented collapse path in ReDoc’s operation panel.

### What we’ve done

- **ReDoc bundle:** `https://cdn.redoc.ly/redoc/v2.5.2/bundles/redoc.standalone.js`.
- **OpenAPI:** Spec fixed so array `items` use `type: object` and `properties`; parameter definitions were already valid.
- **Workaround:** Use [Reference (Scalar)](../api-reference/scalar.md) or [Reference (Swagger UI)](../api-reference/swagger.md) for parameter sections that fully toggle.
- **Upstream:** Consider opening or linking an issue at [Redocly/redoc](https://github.com/Redocly/redoc/issues) with “section caret expand-only, no collapse” and a link to the [official demo](https://redocly.github.io/redoc/) to confirm it’s not spec-specific.
