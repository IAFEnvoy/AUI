# t01-ore

`t01-ore` is a rewrite of the Ore theme: the same MC pixel look, but the components
ask for as little markup as possible. Anything that can be drawn with
`::before` / `::after` is drawn with a pseudo element, so one element is one
component.

It is a standalone theme, not a drop-in replacement for Ore: no `--ore-*`
compatibility variables, no legacy sub-elements.

## Usage

```html
<link rel="stylesheet" href="t01-ore.css">
<body class="t01-ore-theme">
  <button class="button button-primary">Create world</button>
</body>
```

Two local fonts are bundled (`OreRegular` body, `OreDisplay` headings and
controls); nothing is fetched from the network. The stylesheet is MPL-2.0, see
`license.txt`. Open `example.html` for the seven-page showcase.

## What no longer needs markup

| Component | Ore asked for | t01-ore asks for |
| --- | --- | --- |
| `.switch` | `.switch-control` > `.switch-status` + `.switch-button` | the `.switch` element alone (track is `::before`, thumb `::after`, the icon variant is extra background layers) |
| `.slider` / `.slider-2` | `.slider-process` + `.slider-thumb` | the element alone (`::before` fill, `::after` thumb) |
| `.progress` / `.progress-2` | a `.progress-bar` child | the element alone (`::before` fill) |
| `.scrollbar` / `.scrollbar-2` | `-track` + `-thumb` | the element alone (`::before` track, `::after` thumb) |
| `.loading-mask` | a nested `.spinner` | the mask draws its own spinner in `::before`; it can be empty |
| `.list-group` | `:last-child` to drop the last divider | the divider is drawn between adjacent items |
| `.breadcrumb` | `<li>` items | any child elements |

Values that used to be inline widths are now custom properties on the host:

| Component | Write |
| --- | --- |
| `.progress` / `.progress-2` | `style="--value:82%"` |
| `.slider` / `.slider-2` | `style="--value:40%"` (fill and thumb together) |
| `.scrollbar` / `.scrollbar-2` | `style="--thumb:60px"` |

## What still needs markup

CSS cannot invent content or a separate scrim layer, so these keep their
structure: `card` / `panel` headers, bodies and footers; `dropdown` label,
options and option rows; `.tooltip-content` (the engine has no
`content: attr()`); the `modal` backdrop plus its `.modal-dismiss` layer, which
is the real `<label for=…>` that makes a click on the scrim close the dialog;
the `drawer` / `sidebar` masks and their items; and `table` (it must be a real
`<table>`, but cells share each row equally, so any column count works); the
items of `tabs` and `pagination`; and the optional `.slider-segment` ticks,
which are individually placed decorations.

A component's own scrim cannot become a pseudo element either: a `z-index: -1`
pseudo would still paint above its host's background and cover the panel.

## Added components

Beyond the shared core set above, t01-ore also ships the shapes an MC settings
window needs, all in the same language (hard corners, 3px `--edge` outlines,
inset bevels, greens for the active state, `--disabled-*` for disabled):

- **Settings page**: `.settings-row` / `.settings-list` / `.settings-label` /
  `.settings-help` / `.settings-control`
- **Menu**: `.menu` > `.menu-item`, plus `.menu-label`, `.menu-separator`,
  `.menu-shortcut`
- **Segmented control**: `.segmented` > `.segmented-item` (the aliases
  `.toggle-group` / `.toggle-item` are the same component)
- **Content blocks**: `.accordion` (+ `-item` / `-header` / `-body`), `.kv`
  (+ `-row` / `-key` / `-value`), `.hotbar` (+ `.slot-count`), `.empty-state`
  (+ `-icon` / `-title` / `-text` / `-actions`), `.skeleton` (+ `-title` /
  `-text` / `-block` / `-circle`), `.code-block`
- **Odds and ends**: `.divider-label`, `.stepper` (+ `-button` / `-value`),
  `.tree` (+ `-item` / `-group` / `-toggle`), `.stat` (+ `-value` / `-label`),
  `.button-group`, `.tabs-vertical`, `.tag-close`, and the size tiers
  `.modal-sm` / `-lg` and `.drawer-sm` / `-lg` / `-short` / `-tall`
- **What a page cannot avoid**: real scroll containers get `scrollbar-width` /
  `scrollbar-color` / `::-webkit-scrollbar*`; placeholders are coloured through
  `:placeholder-shown` (there is no `::placeholder`); `selection-color` and the
  `[contenteditable]` skin are covered as well. Native checkbox / radio / range
  are deliberately left alone: `accent-color` is the only property the engine
  reads for them and the parts that define their look are hardcoded, so a page
  that wants the MC look should use the components.

`example.html` previews all of it, and `../t00-example.html` is the
cross-theme preview harness for the same set.

## Naming

The scope class is `.t01-ore-theme`, a plain spec-valid class name, so the
stylesheet behaves the same in AUI and in a browser. Everything else follows
the shared theme conventions: `--*` tokens without a theme prefix, state hooks
(`:checked`, `[disabled]`, `[aria-checked="true"]`, `[data-state="on"]`, …)
with the class equivalents (`.on`, `.active`, `.open`, `.show`, `.disabled`,
`.hidden`), and disabled always winning over hover and active.
