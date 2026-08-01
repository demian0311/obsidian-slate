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

**Already done.** The theme is in `community-css-themes.json` as **Diagrammo
Slate** and installs from Settings → Appearance → Themes → Manage. The name in
that entry is the name, permanently — see *Renaming* below. The steps below are kept for
reference; they do not need to be repeated. Subsequent versions ship by cutting
a release (above) — the directory picks up the new tag automatically, no
resubmission.

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

## Renaming the theme

**Do not. A listed theme cannot be renamed.** Settled by trying it: 1.1.0
shortened the manifest name to "Slate", and on 2026-08-01 the portal entry
carried a red banner — *"Your manifest.json was updated to the name 'Slate',
however theme names cannot be changed. Your theme has been hidden from the
directory until the manifest is reverted to its original name"* — alongside a
failed review, `The theme name in manifest.json (Slate) does not match the
expected name (Diagrammo Slate)`. The listing was pulled for the two days it
took to notice. 1.1.2 reverts the name.

The name is fixed at first submission. What is still true, for reference:

- The Style Settings `id` in `theme.css` (`diagrammo-slate`) keys every user's
  saved toggles. **Do not change it either.** It is not the manifest name and
  the portal does not check it, but changing it resets everyone's configuration
- Obsidian installs a theme into `<vault>/.obsidian/themes/<manifest name>/`
  and records `cssTheme: "<name>"` in `appearance.json`, so even without the
  portal rule a rename strands every existing install with no migration path
- A genuinely different name means a new repo and a new submission, carrying no
  users, no stars and no review history across

## What the automated review checks

Every release triggers a review at
[community.obsidian.md](https://community.obsidian.md) → Themes → the entry.
Errors hide the theme; warnings do not. As of 1.1.2 the review is clean except
for one accepted warning:

- **`!important` (~50, all in the mermaid block)** — accepted, unavoidable.
  Mermaid scopes its generated stylesheet with the diagram's `#mermaid-<id>`,
  and no class-only selector can outrank an id no matter how specific. The
  alternative the warning suggests — more specificity — cannot work here
- **`:has`** — removed in 1.1.2. The gap above a live-preview heading is a
  transparent top border on the heading line with `background-clip: padding-box`
  instead of bottom padding on the preceding line
- **`text-decoration`** — removed in 1.1.2. The red-tinted strikethrough is now
  plain faint ink

## Before submitting

- [x] `screenshot.png` at repo root
- [x] Release tag matches `manifest.json` version exactly (1.1.2)
- [x] `manifest.json` name is still `Diagrammo Slate` — it can never change
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
