---
name: style-tokens
description: Apply the user's personal visual style (colors, fonts, sizes, layout, components, charts) when building dashboards, GUIs, web apps, data visualizations, paper figures, or terminal output. Use only when the user invokes it or asks for "my style".
disable-model-invocation: true
---

Apply this style to whatever you build. When a case is not covered, extrapolate from the principles; do not fall back to framework defaults.

## Contexts

| Output | Style |
|---|---|
| Dashboards, GUIs, web apps, charts inside them | Dark UI |
| Static plots and figures (matplotlib, seaborn, exported plotly) | Paper (light) |
| CLI output, logs, TUIs | Terminal (dark) |

## Principles

- Quiet and dense. Hierarchy comes from color, size, and spacing, not from boxes, borders, or headings.
- No page titles. Panels and sections get a small uppercase label, never a heading.
- No borders on containers. Separate panels from the page by surface shade.
- The accent marks the one thing that matters: the focal series, the active item, the primary action. Everything else is neutral.
- Numbers are monospaced and tabular, right-aligned in tables.
- Nothing decorative: no shadows (except floating elements), gradients, icons without meaning, or emoji.

## Dark UI

### Color

```css
:root {
  --bg: #181818;            /* page */
  --surface: #1f1f1f;       /* panels, cards */
  --text: #fafafa;
  --text-2: #b0b0b0;        /* secondary text */
  --text-3: #7a7a7a;        /* labels, table headers, axis ticks */
  --line: #2e2e2e;          /* internal dividers and gridlines only */
  --ctl-border: #3a3a3a;    /* checkboxes, kbd, chart axes */
  --ctl-fill: rgba(255,255,255,.06);   /* inputs, secondary buttons */
  --hover: rgba(255,255,255,.035);
  --zebra: rgba(255,255,255,.025);
  --track: rgba(255,255,255,.07);      /* progress/meter tracks */
  --seg-bg: rgba(255,255,255,.05);     /* segmented-control container */
  --accent: #6600cc;        /* fills only: toggles, checkboxes, brand mark */
  --accent-fg: #a970ff;     /* text, lines, strokes, icons on dark */
  --accent-soft: rgba(169,112,255,.16);
  --ok: #4fb286;
  --warn: #d4a54a;
  --err: #e06c6c;
}
```

`#6600cc` on `#1f1f1f` is 1.9:1, so it is unreadable as text or thin lines. Never use `--accent` for text or strokes; use `--accent-fg`.

### Typography

- Font: JetBrains Mono for everything, UI and numbers. Fallback `ui-monospace, monospace`.
- Sizes: base 13px, sm 12px, xs 11px, lg 15px, KPI value 27px.
- Weights: 400 body and KPI values, 500 for emphasis and labels. Nothing bolder.
- Line height 1.45. `font-variant-numeric: tabular-nums` on all numbers.
- Section/panel label ("eyebrow"): xs, uppercase, `letter-spacing: .08em`, `--text-3`, weight 500.

### Layout

- Chrome: one top bar (height 50px): brand mark (12px `--accent` square, radius 3) and name, horizontal nav, then search and avatar on the right. No sidebar. No border below; the bar sits on `--bg`.
- Page: padding 24px, then a toolbar row (time-range tabs, filters left; actions right), then a 12-column grid.
- Spacing: gap 12px, panel padding 16px, row/control height 34px (buttons and inputs 30px).
- Panels: `--surface` on `--bg`, no border, radius 8px. Separate cards, not a joined grid.
- Panel header: eyebrow label left, legend or a ghost action right, 12px below.
- Typical dashboard: KPI row (4 × span 3), main chart (span 8) + breakdown (span 4), secondary row, full-width table.
- KPI tile: eyebrow label, value (27px/400) with a unit in sm `--text-2`, delta in xs status color with "vs prev" in `--text-3`, optional 28px sparkline.

### Components

- Radius 8px; small elements (checkbox, kbd, bar ends) 3px; elements nested in a padded container 6px; switches and avatars fully round.
- Primary button: soft tint: `--accent-soft` background, `--accent-fg` text, no border. Secondary: `--ctl-fill` background, `--text`. Ghost: no background, `--text-2`, `--text` on hover.
- Inputs: `--ctl-fill` background, no border, placeholder `--text-3`. Focus: 1px `--accent-fg` border plus `0 0 0 3px var(--accent-soft)` ring.
- Active state (nav item, tab): `--accent-soft` background, `--accent-fg` text. Links: `--accent-fg`, no underline.
- Tabs: segmented: `--seg-bg` container with 2px padding, items in sm, active item uses the active state.
- Tables: zebra rows (`--zebra`), no row borders, header in sm `--text-3` weight 500, 34px rows, 8px horizontal padding at the edges, numbers right-aligned, `--hover` on row hover.
- Status: 6px dot in the status color followed by the label in `--text`. Running → `--accent-fg`, done → `--ok`, failed → `--err`, queued → `--text-3`.
- Meters and progress bars: 4–6px tall, `--track` track, `--accent-fg` fill, `--warn` above 90%.
- Tooltips and popovers: `#2a2a2a` background, no border, radius 8px, `0 4px 16px rgba(0,0,0,.4)` shadow, xs text.

### Charts

- Series colors: if one series is focal ("ours", the current run), it is `--accent-fg` and the others are grays `#8a8a8a`, `#555555`. Otherwise use categorical colors `#a970ff`, `#2fb8a6`, `#e3a33b`, `#e0607e`, `#5b8def`. Bars: `#474747`, with the focal bar in `--accent-fg`.
- Lines 1.5px, round joins, no area fill, no markers.
- Gridlines: horizontal, `--line`, dotted (`1 3`). Axes: x baseline and y axis line in `--ctl-border`. Tick labels xs `--text-3`, monospaced.
- Bars: thin (about 42% of the band), radius 2px.
- Legend: on top, in the panel header: a 10×2px line swatch plus the name in xs `--text-2`.
- Hover: dashed vertical crosshair in `--text-3`, hollow 3px dots on each series, tooltip listing every series value.
- No chart titles inside the plot; the panel eyebrow names it.

## Paper plots

Match LaTeX papers: Times-style serif, closed black box, no grid, white background.

```python
plt.rcParams.update({
    "font.family": "serif",
    "font.serif": ["Times New Roman", "Times", "Nimbus Roman", "TeX Gyre Termes", "STIXGeneral"],
    "mathtext.fontset": "stix",
    "font.size": 10,
    "text.color": "black", "axes.labelcolor": "black", "axes.edgecolor": "black",
    "axes.linewidth": 1.0,
    "axes.grid": False,
    "axes.titlesize": 10, "axes.titlepad": 6,
    "axes.prop_cycle": plt.cycler(color=["#6600cc", "#1f8f81", "#c07f12", "#c43d5f", "#3a6fd8"]),
    "xtick.direction": "out", "ytick.direction": "out",
    "xtick.major.size": 4, "ytick.major.size": 4,
    "xtick.major.width": 1.0, "ytick.major.width": 1.0,
    "lines.linewidth": 1.5,
    "legend.frameon": False, "legend.fontsize": 10, "legend.handlelength": 2.0,
    "figure.facecolor": "white",
    "savefig.dpi": 200, "savefig.bbox": "tight",
    "pdf.fonttype": 42,
})
```

- Size figures to the paper column (3.25 in single, 6.75 in double) so 10pt text matches the body.
- Series colors: focal series `#6600cc`, others gray `#808080` told apart by line style (solid, dashed `--`, dotted `:` in `#b3b3b3`). No focal series: the categorical cycle above.
- Bars: categorical fills with a thin black edge (`edgecolor="black", linewidth=0.6`).
- Sequential colormaps: white → `#d3b5ff` → `#6600cc` → `#2e005c`.
- Legend: frameless, one row, centered above the axes; use one figure-level legend when panels share series.
- Subplot titles: short and centered (e.g. "10 Hz"), regular weight. Axis labels in sentence case with units in parentheses.
- Save as PDF for papers.

## Terminal

Assume a dark background. Use truecolor via `rich`:

```python
Theme({
    "text": "#fafafa", "muted": "#b0b0b0", "faint": "#7a7a7a",
    "accent": "#a970ff", "ok": "#4fb286", "warn": "#d4a54a", "err": "#e06c6c",
    "eyebrow": "bold #7a7a7a",
})
```

- Section headers: uppercase `eyebrow`, one blank line before.
- Log line: `HH:MM:SS` faint, level padded to 5 (INFO faint, WARN warn, ERROR err, DONE ok), scope padded in muted, message in text, then `key` faint and `value` text pairs separated by two spaces.
- Tables: `box=None`, header faint, zebra rows (`row_styles=["on #262626", ""]`), padding `(0, 2)`, ids in accent, numbers right-aligned, status as `●` in the status color plus the label.
- Progress: bar track `#333333`, complete accent, finished ok; description muted.
- Key-value summaries: key faint padded to a fixed width, value in text or a status color.
- No boxes, panels, or rules around output.
