# t02-claude

A Claude / Anthropic-styled theme for ApricityUI. Warm parchment surfaces,
terracotta accents, serif headlines, hairline borders and ring shadows - the
opposite end of the room from t01's dark MC pixel look, built on exactly the
same class contract.

## Where it comes from

The design language is taken from the Claude design system: parchment
(`#f5f4ed`) and ivory (`#faf9f5`) surfaces, near-black text (`#141413`), the
terracotta brand accent (`#c96442`), warm neutrals only (**never a cool
blue-gray**), 1px hairline borders over a ring-shadow depth system, a radius
scale of 8 / 12 / 16 / 24 / 32px, serif headlines pinned to weight 500, and a
generous 1.60 body line-height.

Per `../theme-spec.md` §8 it was built the documented way: copy t01's
stylesheet, swap the token values, then adapt the shapes. The generator that
did it lives in the session scratch, so the resulting file carries t01's rules
plus a "Claude shape layer" and a "light-surface corrections" section at the
end - both commented and safe to read as part of the theme.

## Contract token mapping

The token *names* are the shared contract; here is what each one carries in
this theme, which is the part worth copying into the next theme:

| Contract token | Claude role |
| --- | --- |
| `--canvas` | Parchment `#f5f4ed` |
| `--surface` | Ivory `#faf9f5` (cards, panels, inputs' neighbours) |
| `--surface-deep` | Warm Sand `#e8e6dc` (recessed: inputs, table heads, code) |
| `--surface-soft` | Pure White `#ffffff` (the lightest surface only) |
| `--edge` / `--edge-light` | Border Cream `#f0eee6` / Ring Warm `#d1cfc5` |
| `--ink` / `--ink-muted` | Near Black `#141413` / Olive Gray `#5e5d59` |
| `--green` (+hover/shadow) | **Terracotta Brand `#c96442`** - the primary action |
| `--purple` (+hover/shadow) | Coral `#b55536` - the secondary action |
| `--focus` | Focus Blue `#3898ec`, the only cool colour in the system |
| `--gold`, `--red`, `--blue`, `--success`, `--warning`, `--danger`, `--info` | Muted warm tones darkened enough to read as text on parchment |
| `--gray-10..100` | The ten warm tones straight from the design system, light to dark |
| `--overlay` / `--overlay-soft` | Warm black scrims, `rgba(20,20,19,.55)` / `.35` |
| `--radius-sm` / `--radius-md` | 6px / 12px (the design system forbids sharp corners) |
| `--motion-fast` | 150ms |
| extras: `--ink-on-dark`, `--deep-dark`, `--dark-surface`, `--ring`, `--elev-ring`, `--elev-whisper`, `--font-display`, `--font-body`, `--font-mono` | Theme-private helpers, generic names, no theme prefix |

## Shape language

- **1px hairlines everywhere.** Every 2-4px border in t01 became 1px; thick
  borders are simply not part of this language.
- **Depth is a ring, not a bevel.** All inset bevels were dropped; the depth
  language is `0 0 0 1px` rings, plus a whisper shadow
  (`rgba(0,0,0,.05) 0 4px 24px`) on elevated surfaces, and the switch, slider,
  progress bar and loading bar became pills.
- **Serif for authority, sans for utility.** Headings and other display text
  use `--font-display` (Georgia and friends) at weight 500; everything else
  uses `--font-body`.
- **The slider is a thin pill track with a white knob ringed in terracotta**,
  optionally filled from the left edge to the knob. `.slider-value` is the
  capsule readout, `.slider-scale` the two end labels, and `.slider-plain`
  keeps the knob without the fill.
- **Containers Claude keeps dark stay dark** - drawer, sidebar, toast,
  tooltip, code block - so their ivory text still holds; everything else is a
  light surface with ink text.

## Known limitations

- **CJK headlines fall back to a sans font unless a serif CJK face is
  installed.** Georgia has no Chinese glyphs; the stack lists Noto Serif SC /
  Source Han Serif SC / Songti SC / SimSun / STSong, and on a machine with
  none of them (this one) Chinese headings render in the system CJK sans.
  Verified with Chrome's platform-font query: Latin headings really are
  Georgia, Chinese ones are not.
- The `-2` and `-3` numbered generations keep their original geometry; in this
  theme they read as further variants of the same flat language rather than a
  distinct second design.
- The two `-3` "unit bevel" buttons lost their top highlight band, which is the
  intended consequence of flattening every inset shadow.

## Preview

`../t00-example.html` already lists this theme: pick `t02-claude` in the
toolbar. Everything in the harness is rendered by this stylesheet alone.
