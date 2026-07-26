# Diagrammo Slate

An Obsidian theme built on the **Slate** palette from
[diagrammo.app/palette](https://diagrammo.app/palette/) — a cool-gray ground, a
confident corporate blue, a muted teal accent, and a nine-hue categorical
spectrum tuned to a single perceived weight.

Slate was designed so no series shouts. This theme takes that even-weight
spectrum and spends all of it: headings, callouts, tags, folders, syntax, task
states, graph nodes and canvas all carry hue, and none of it fights.

Light and dark are both first-class — the palette ships distinct values for
each, not one set with opacity tricks.

## Palette

| Role | Light | Dark |
| --- | --- | --- |
| Background | `#ffffff` | `#161b22` |
| Surface | `#f3f5f8` | `#202833` |
| Overlay | `#eaeef3` | `#29323e` |
| Border | `#d4dae1` | `#38424f` |
| Text | `#1f2933` | `#e6eaef` |
| Muted | `#5b6672` | `#9aa5b1` |
| Primary | `#3b6ea5` | `#5b9bd5` |
| Accent | `#3a9188` | `#45b3a3` |

| Spectrum | Light | Dark |
| --- | --- | --- |
| Red | `#c0504d` | `#e07b6e` |
| Orange | `#cc7a33` | `#e0975a` |
| Yellow | `#c9a227` | `#d9bd5a` |
| Green | `#5b9357` | `#74b56e` |
| Blue | `#3b6ea5` | `#5b9bd5` |
| Purple | `#7d5ba6` | `#a585c9` |
| Teal | `#3a9188` | `#45b3a3` |
| Cyan | `#4f96c4` | `#62b0d9` |
| Gray | `#7e8a97` | `#95a1ae` |

In light mode the gold is too light to read as body-weight ink, so text roles
use a darkened `#8f7418` while fills and washes keep the true `#c9a227`.

## What is colored

- **Headings** — H1–H6 walk red → orange → gold → green → blue → purple. H1
  gets a red underline, H2 a orange left bar. The outline pane mirrors the
  ladder.
- **Callouts** — every built-in type maps to a spectrum hue: note/info blue,
  tip/todo teal, success green, question gold, warning orange, failure/danger
  red, example purple, quote gray.
- **Tags** — hued by first character across nine buckets (`a`/`j`/`s` → red,
  `b`/`k`/`t` → orange, and so on). CSS can't hash a string, so this is the
  closest deterministic approximation. Live preview has no tag name in the DOM,
  so tags there take the teal accent pill.
- **Folders** — file-explorer folders tint by nesting depth, six levels deep,
  with a matching left rule on the children container. Files stay neutral so
  the hues carry structure, not noise.
- **Syntax** — keywords red, strings green, functions purple, properties cyan,
  values teal, operators orange. Inline code is purple ink on a purple wash;
  fenced blocks sit on the neutral panel with a purple spine.
- **Tasks** — `[>]` cyan, `[!]` orange, `[?]` gold, `[*]` purple, `[-]` red and
  struck through. Only a real `[x]` dims.
- **Graph & canvas** — nodes blue, focused orange, tags teal, attachments
  purple, unresolved red. Canvas colors 1–6 map to the spectrum.

## Typography

System fonts only — nothing is downloaded.

- Interface: Inter → system sans
- Body: Iowan Old Style → Charter → Palatino → Georgia
- Code: JetBrains Mono → SF Mono → IBM Plex Mono → Menlo

Measure is `42rem`, body line height `1.7`, headings tracked at `-0.018em`.

> If you have set **Font** under Appearance in Obsidian, your setting wins over
> the theme's. Clear the Text font field to get the serif reading face.

## Install

### From the community list

Settings → Appearance → Themes → Manage → search *Diagrammo Slate*.

### With BRAT

Add `demian0311/obsidian-slate` as a beta theme in BRAT.

### Manually

Copy `theme.css` and `manifest.json` into
`<vault>/.obsidian/themes/Diagrammo Slate/`, then pick the theme under
Settings → Appearance.

## Customizing

The theme ships a [Style Settings](https://github.com/mgmeyers/obsidian-style-settings)
block. Everything colorful is **on by default** and the toggles turn things
*off*, so the theme looks the same with or without the plugin installed:

- Plain headings — drop the H1–H6 spectrum ladder
- No heading rules — drop the H1 underline and H2 bar
- Plain folders — neutral file explorer
- Plain tags — all tags take the teal accent
- Plain horizontal rules — hairline instead of the spectrum gradient
- Plain bold & italic — uncolored emphasis
- Sans-serif body — interface sans for note text
- Line width — the readable measure, default `42rem`

## Development

`theme.css` is hand-written, no build step.

For live editing, the working copy has to live where Obsidian's file watcher
can see it. Obsidian does not follow a symlink out of `.obsidian/themes/`, so
symlinking the repo *into* a vault gives you a theme that loads once and never
hot-reloads. Put the real files in the vault and symlink back out instead:

```sh
git clone <repo> "<vault>/.obsidian/themes/Diagrammo Slate"
ln -s "<vault>/.obsidian/themes/Diagrammo Slate" ~/code/obsidian-slate
```

Now saving `theme.css` reloads the theme immediately. `Cmd+R` forces a reload
if the watcher misses a change.

## Credits

Palette: the `slate` palette from [Diagrammo](https://diagrammo.app).

## License

MIT
