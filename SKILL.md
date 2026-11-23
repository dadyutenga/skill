---
name: opendashboards-design
description: >-
  Apply the OpenDashboards design system (dashboard tokens, app chrome, and
  marketing bevels) instead of inventing colors or type. Use when building or
  restyling a dashboard, KPI, chart card, filter chip, status delta, or
  landing button in the OpenDashboards look, or when the user mentions
  OpenDashboards design, series colors, .od tokens, or this skill.
---

# OpenDashboards design

Use the bundled tokens. The palette, type, radius, and component recipes already exist in this skill. Read them and apply them.

## Pick the layer

Match the surface, then stop. One root class per surface.

| Surface | Root class | What it styles |
| --- | --- | --- |
| Dashboard cards, KPIs, charts, filters | `od` | Ink, series, status, 16px cards. Add `data-mode="dark"` for dark. |
| Signed-in app chrome | `od-app` | Near-black buttons, 10px radius, gray chart ramp |
| Marketing page | `od-mkt` | Inter Display, pill bevel buttons, `#0a0a0a` / `#f7f7f8` |

Done when the surface root carries that class and `assets/tokens.css` is loaded on it.

## Load the files

Copy these into the project and load both stylesheets. Paths are relative to this skill.

- [assets/tokens.css](assets/tokens.css) — variables
- [assets/components.css](assets/components.css) — recipes that read those variables
- [assets/tokens.json](assets/tokens.json) — the same values as literals, for a chart library that cannot read CSS variables

```html
<link rel="stylesheet" href="tokens.css" />
<link rel="stylesheet" href="components.css" />
<div class="od">
  <article class="od-card">…</article>
</div>
```

Fonts: Inter for product UI, Inter Display for marketing (Inter is the fallback), Geist for the wordmark, Geist Mono for code.

Done when a rendered dashboard card uses `var(--od-series-1)` (or a recipe class), not a new hex.

## Recipes

Use these classes. Their rules live in `assets/components.css`.

| Class | Use |
| --- | --- |
| `od-card`, `od-card-title`, `od-card-subtitle` | Chart and metric card. Padding 20×24, radius 16, hairline border. |
| `od-kpi-label`, `od-kpi-value` | Headline number, 26px, tabular nums. |
| `od-delta`, `od-delta-good`, `od-delta-bad`, `od-delta-neutral` | Change vs previous. |
| `od-chips`, `od-chip` | Segmented control. Selected chip sets `aria-checked="true"`. |
| `od-page-title`, `od-section-title`, `od-caption` | Page, section, and muted caption. |
| `od-btn`, `od-btn-outline`, `od-btn-ghost` | App-shell buttons, 32px, 10px radius. Needs `od-app`. |
| `od-bevel`, `od-bevel-secondary`, `od-bevel-badge` | Landing pills, 48px. Needs `od-mkt`. |

## Keep the two palettes apart

Dashboard charts use `--od-series-1` through `--od-series-8`. Status text uses `--od-good`, `--od-bad`, `--od-warning`, `--od-info`. Strokes and dots use the matching `--od-*-mark` token.

`--od-app-chart-1` through `--od-app-chart-5` are a gray ramp for the app shell. Leave them off dashboard charts.

Inside the OpenDashboards app, theme `@openuidev/dashboard-library` with `ThemeProvider` `lightTheme` / `darkTheme`. Its keys are the camelCase form of these variables (`series1` → `--od-series-1`). Read `assets/tokens.css` for the value. Do not edit the library's private component CSS.
