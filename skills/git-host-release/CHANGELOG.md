# CHANGELOG Template

> See the actual effect of this template should be in [release-note-preview.md](release-note-preview.md).

Here are the changelogs of plugin versions, and used for CI also. Some old version's changelogs is not included. For AI agents, add missing commit short-hash (replace `hash123`) before release action starts. Version sections are ordered newest first; CI locates each section by its heading, so the order is for readers only.

For CI, Add `Header` or `Endfix` section to every version's release note, do not include the title(as `Header` or `Endfix`). Header content add to the first line, and Endfix content in the last line. Especially, `**full_changelog!**` is a tag, need to convert to real `Full Changelog` link.

For example, the content of `**full_changelog!**` is:

```markdown
**Full Changelog**: https://github.com/Username/repository/compare/v0.1.0...v0.2.0
```

> Do not copy this! Caculate it with scripts automatically in CI actions.

And you can create H3 level titles in a version section to override the default Header or Endfix content.

Notice that CI is not responsible for editing the old version's release note, it only generates new version's release note.

Write latest version in the front, so people can easily see them if reading this documentation.

## Header

> [!NOTE]
> Methods to install the software/plugin.

## Endfix

**full_changelog!**

## 0.2.0

### Header

> [!NOTE]
> In this version, you have to remove old configurations before running the new exe.

### Features

- Add new features

## 0.1.0

### Fixes

- Fix some bugs
