# Design conversion log – k0rdent AI docs

This document records changes made to apply the **new design** (from k0rdent-enterprise-docs / ke-new-design) to this repo, using this repo’s own graphic elements (logo, favicon, etc.). The API specification special index page was left unchanged.

## Summary

- **Design source:** New-style layout and palette from k0rdent-enterprise-docs (hero + cards, dark slate default, k0rdent AI accent colors, no right TOC, wider content).
- **Assets:** All graphics (logo `img/k0rdent-ai-logo-horizontal-inverted.svg`, favicon `img/favicon-32x32.png`) are from this repo; no assets were copied from the enterprise repo.

## Files changed

| File | Change |
|------|--------|
| `docs/stylesheets/newstyle.css` | **Added.** New-style overrides: k0rdent AI palette (yellow/pink accents), dark slate as default, header/sidebar/link styling, right TOC hidden, content width increased, hero and card classes for the index. |
| `docs/index.md` | **Replaced.** Traditional long-form index replaced with hero + card landing: tagline, “Get started” (API specification, What k0rdent AI solves, Key capabilities), “Join the community” (GitHub, X, LinkedIn). Section headings use `<h3>` inside the HTML block so they render correctly, and anchor headings below are implemented with HTML `<h2 id=\"...\">` to avoid macro syntax issues. Original “What k0rdent AI solves”, “Key capabilities”, and “Platform components” content kept below. |
| `mkdocs.yml` | **Updated.** Theme palette: slate first with `primary: black`, `accent: amber`; default scheme is dark. `extra_css` extended with `stylesheets/newstyle.css`. First nav item set to `Home: index.md`. `site_description` set to “Documentation for k0rdent AI.” `extra.docsVersionInfo.k0rdentName` set to “k0rdent AI”. Logo and favicon paths unchanged. **API formula:** nav under “API” with API overview (discursive), Reference (ReDoc), Reference (Scalar), Reference (Swagger UI). |
| `docs/stylesheets/extra.css` | **Updated.** Added `.md-logo img` width (225px) so the header logo displays at a size consistent with the new design. |
| `docs/api-specification/index.md` | **Replaced.** Now a normal Material page (no custom template): narrative API overview (overview, docs variants, what k0rdent AI solves, key capabilities, platform components, endpoint reference tables, future/TBD, lifecycle & contracts). Links to Reference (ReDoc/Scalar/Swagger) pages. |
| `docs/api-reference/redoc.md` | **Added.** Full-screen ReDoc embed shell; loads `openapi/openapi.bundled.yaml`. Sidebars and footer hidden. |
| `docs/api-reference/scalar.md` | **Added.** Scalar API reference shell; loads same OpenAPI spec. |
| `docs/api-reference/swagger.md` | **Added.** Swagger UI shell; loads same OpenAPI spec. |
| `docs/stylesheets/newstyle.css` | **Updated.** Removed the API section layout block (`.api-main`, `.api-layout`, `.api-hero`) that was used by the removed `api.html` template. |
| `docs/stylesheets/newstyle.css` | **Updated.** Day/night palette: show only one visible toggle (the “other” scheme’s label) so it behaves as a single switch; added transitions for subtle animation. Uses Material theme palette only, no custom toggle. |
| `docs/custom_theme/partials/palette.html` | **Added.** Override of Material palette partial: labels rendered without the `hidden` attribute so custom CSS can control which toggle is visible (one per scheme). Theme palette JS unchanged. |
| `docs/custom_theme/api.html` | **Removed.** Custom API template deleted; API overview uses default Material layout. |
| `CONVERSION.md` | **Updated.** This log. |

## Not changed

- **Logo / favicon:** Still `img/k0rdent-ai-logo-horizontal-inverted.svg` and `img/favicon-32x32.png` from this repo.
- **Custom theme:** `docs/custom_theme/main.html` remains a thin extension of Material’s `base.html`.
- **Site design:** Hero index, newstyle palette, and main TOC are unchanged; only the API strategy was reverted and the formula (discursive overview + reference shells) applied.

## How to revert or toggle the new design

- **Revert everything:** Restore `docs/index.md` and `mkdocs.yml` from git; remove `docs/stylesheets/newstyle.css` and this `CONVERSION.md`.
- **Turn off new style only:** In `mkdocs.yml`, remove or comment out the `- stylesheets/newstyle.css` line under `extra_css`; the index will still use the new HTML structure but without the new-style colors/layout (you may want to revert `index.md` as well if you prefer the old content).
