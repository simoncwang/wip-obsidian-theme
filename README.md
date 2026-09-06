# WIP Obsidian theme

A custom Obsidian theme currently under development.

## Architecture

Theme styles are organized as small source files and compiled into the single `theme.css` file that Obsidian loads.

```text
src/
├── index.css
├── foundations/
│   ├── colors.css
│   ├── spacing.css
│   ├── typography.css
│   └── radii.css
└── components/
    ├── editor.css
    ├── icons.css
    ├── menus.css
    ├── navigation.css
    └── tabs.css

theme.css
```

`src/index.css` is the build entry point and controls the order in which the source files are imported. Files under `src/` are the authored source; the root-level `theme.css` is generated output. It remains committed so the repository can be cloned directly into an Obsidian vault and used without requiring Node.js or a build step.

## Development

Install the development dependencies once:

```bash
npm install
```

Start the development watcher:

```bash
npm run dev
```

Make style changes in `src/`, not directly in `theme.css`. While the watcher is running, changes to the source files automatically rebuild `theme.css`, allowing Obsidian to load the latest result.

Before committing, build and validate the theme:

```bash
npm run check
```

Commit both the source changes and the generated `theme.css`:

```bash
git add src theme.css
git commit -m "Describe the theme change"
git push
```

## Releases

The GitHub Actions release workflow installs dependencies, lints the source, builds `theme.css`, and packages the generated file together with `manifest.json` in a draft GitHub release.

The workflow will also verify that the committed `theme.css` matches the build output. This keeps direct clones, repository source, and published release artifacts synchronized.

The release workflow runs when a Git tag is pushed. To create a patch release, first commit all intended theme changes, then run:

```bash
npm version patch --tag-version-prefix=""
git push --follow-tags
```

Use `minor` or `major` in place of `patch` when appropriate. The version command updates `package.json`, `manifest.json`, and `versions.json`, creates a release commit, and tags it without a `v` prefix. The tag must exactly match the version in `manifest.json`.

After the tag is pushed, GitHub Actions creates a draft release. Review its generated notes and attached `manifest.json` and `theme.css`, then publish it from GitHub when it is ready.
