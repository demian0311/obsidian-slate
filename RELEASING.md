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

- **16:9 is required**, not a suggestion — the submission template states it,
  and 512x288 is the recommended size for fast loading. Anything else will be
  bounced
- Show what makes this theme itself: the heading ladder, a tinted file
  explorer with a few nesting levels, colored callouts, and a mermaid diagram
- Either mode is fine; a light/dark pairing uses the single slot better when
  the two modes are genuinely different designs
- Save as `screenshot.png` at the repo root

The current one is a 1600x900 composite: light and dark captured at identical
framing, each scaled to *cover* its half of the canvas and cropped from the
right. Fitting whole windows inside a 16:9 frame leaves the card mostly empty —
the captures are 1.30:1 while each half is 0.89:1 — and cropping from the right
keeps the rail and the start of every line, dropping only the empty end of the
measure. Sources are in `docs/`; the compositing script is in the commit that
added it.

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

**This is not a pull request.** `obsidianmd/obsidian-releases` is now a mirror —
pull requests and issues are both disabled on it, and `community-css-themes.json`
only receives automated `chore: Mirror community plugins and themes` commits.
Older guides describing a fork-and-PR flow are out of date.

Submit through the web portal instead:

1. Go to [community.obsidian.md](https://community.obsidian.md) and sign in with
   your Obsidian account
2. Link your GitHub account, which verifies you own the repo
3. **Themes** > **New theme** in the sidebar
4. Enter `https://github.com/demian0311/obsidian-slate`
5. Review the developer policies and confirm ongoing support
6. **Submit**

The directory reads `manifest.json` from your default branch's HEAD, so commit
any version bump before submitting.

## Before submitting

- [x] `screenshot.png` at repo root
- [x] Release tag matches `manifest.json` version exactly (1.0.0)
- [x] `theme.css` and `manifest.json` attached to the release
- [ ] Both light and dark checked against a real vault, not just the preview
      note — nested folders, long note lists, a populated graph
- [ ] Mermaid verified: flowchart, sequence, class, mindmap
- [ ] Checked with Style Settings both installed and absent — every toggle is
      an opt-*out*, so the theme must look identical without the plugin
- [x] Checked with no CSS snippets enabled

## Installing a pre-release with BRAT

Once a release exists, `demian0311/obsidian-slate` can be added as a beta theme
in BRAT without being in the community list.
