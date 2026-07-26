# Releasing

## What Obsidian requires

A theme in the community list is a GitHub repo with, at the root:

- `theme.css`
- `manifest.json` — `name`, `version`, `minAppVersion`, `author`, `authorUrl`
- A GitHub **release** whose tag exactly matches `manifest.json`'s `version`,
  with `theme.css` and `manifest.json` attached as release assets
- `screenshot.png` in the repo (referenced by the submission PR, not the
  manifest)

The theme name must be unique across the community list, and must not contain
"Obsidian".

## Screenshot

This is the one step that cannot be automated — it has to be taken from a
running Obsidian.

- Obsidian's guidance is a wide screenshot, roughly 2:1, showing the theme in
  actual use rather than a swatch grid
- Show what makes this theme itself: the heading ladder, a tinted file
  explorer with a few nesting levels, colored callouts, and a mermaid diagram
- Take it in **light** mode — the community browser renders cards on a light
  ground — but consider a second dark shot in the README
- Save as `screenshot.png` at the repo root

## Cutting a release

```sh
# 1. bump the version in manifest.json, commit
git add manifest.json && git commit -m "chore: 1.0.0"

# 2. tag with the exact version string — no leading v
git tag 1.0.0
git push origin main --tags

# 3. create the release with both files attached
gh release create 1.0.0 theme.css manifest.json \
  --title "1.0.0" \
  --notes "First release."
```

The tag must be `1.0.0`, not `v1.0.0`. Obsidian matches the manifest version
string literally.

## Submitting to the community list

1. Fork [obsidianmd/obsidian-releases](https://github.com/obsidianmd/obsidian-releases)
2. Append to `community-css-themes.json`:

```json
{
  "name": "Diagrammo Slate",
  "author": "Demian Neidetcher",
  "repo": "demian0311/obsidian-slate",
  "screenshot": "screenshot.png",
  "modes": ["dark", "light"]
}
```

3. Open a PR. The checklist in the PR template asks you to confirm the release
   exists and the screenshot renders.

Review is done by a bot first, then a human. Expect a wait, and expect at
least one round of comments.

## Before submitting

- [ ] `screenshot.png` at repo root
- [ ] Release tag matches `manifest.json` version exactly
- [ ] `theme.css` and `manifest.json` attached to the release
- [ ] Both light and dark checked against a real vault, not just the preview
      note — nested folders, long note lists, a populated graph
- [ ] Mermaid verified: flowchart, sequence, class, mindmap
- [ ] Checked with Style Settings both installed and absent — every toggle is
      an opt-*out*, so the theme must look identical without the plugin
- [ ] Checked with no CSS snippets enabled

## Installing a pre-release with BRAT

Once a release exists, `demian0311/obsidian-slate` can be added as a beta theme
in BRAT without being in the community list.
