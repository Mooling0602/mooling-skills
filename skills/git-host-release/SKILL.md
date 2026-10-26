---
name: git-host-release
description: Set up CI/CD and publish releases on a Git hosting provider (GitHub, GitLab, and similar). Use when configuring pipelines, cutting version tags, or generating release notes from a changelog.
metadata:
  source: https://github.com/Mooling0602/mooling-skills/tree/main/skills/git-host-release
---

# git-host-release

CI/CD and version release on Git hosting providers.

## Scope

Covers running CI/CD and publishing releases on a Git hosting provider.

This skill deliberately provides **no** platform-specific implementation details, documentation, or code. Collect that information from the provider's own docs and integrate it into your actual workflow.

## Providers

Any Git repository hosting provider qualifies, as long as it:

- supports CI/CD pipelines,
- supports Releases,
- exposes a usable, stable, standard API.

Examples: GitHub, Gitea (including self-hosted instances), Gitee, GitLab.

## CI configuration

Put CI configuration and scripts under a `.<host>/` directory at the repository root, such as `.github/` or `.gitea/`.

That layout belongs to GitHub and Gitea. Other providers dictate their own places: GitLab reads `.gitlab-ci.yml` from the repository root and cannot be given a `.gitlab/` directory in this shape. Check the provider's documentation before assuming the skeleton below carries over.

Taking GitHub as the example, a complete CI skeleton looks like this:

```sh
.
└── .github
    ├── scripts
    │   └── tool.sh
    └── workflows
        ├── ci.yml
        ├── deploy.yml
        └── release.yml
```

- `scripts/` holds the scripts and tooling the pipeline needs.
- `workflows/` holds the CI configuration in YAML.

Each workflow has one job:

| File | Purpose |
| --- | --- |
| `ci.yml` | Check syntax, run tests, and build artifacts. |
| `deploy.yml` | Optional. Deploy live services such as websites. |
| `release.yml` | Publish new versions of packages or plugin bundles, attaching release notes and the artifacts built automatically. Generate the release note from the repository's `CHANGELOG.md` (by default at the repository root, or wherever you choose to keep it). |

You may add a separate `publish.yml` to publish packages to registries such as PyPI or npm. Prefer integrating that into `release.yml` instead, so the version stays consistent.

Put helper scripts under a directory like `.github/scripts/`. This covers anything that generates or computes content, such as assembling release notes.

On GitHub you can also enable "Immutable releases" in the repository settings to raise the trustworthiness of release artifacts.

Extend this skeleton with whatever else your situation needs.

## Versioning and tags

### Commit messages

Write commits that are concise but still carry enough change information, following [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/). This skill assumes you work to that specification.

### Cutting a release

Make the version number the last thing changed, and give it a commit of its own:

```plaintext
release: v0.1.0
```

Keep the `v` prefix everywhere the version appears: the release commit message, the tag, and the `CHANGELOG.md` heading. With one spelling throughout, scripts never have to add or strip it.

Record the seven-character short hash of that commit, shown here as `hash123`, and create the version tag with `commit <hash>` as its message:

```sh
git tag -a v0.1.0 -m "commit hash123"
```

Push the branch and the tag together, naming the tag by its full ref so it cannot be confused with a branch of the same name:

```sh
git push origin main refs/tags/v0.1.0
```

Create the release commit and the tag on the main branch (usually `main`) so the version history stays linear.

## Release notes

`release.yml` generates the release note from the repository's `CHANGELOG.md`.

See the [CHANGELOG template](CHANGELOG.md) for the format. The [release note preview](release-note-preview.md) shows what the generated notes should look like after `release.yml` runs.

### Structure

- Version sections are ordered newest first. CI locates each section by its heading, so the order only matters for readers.
- CI only generates the new version's release note. It never edits older ones.
- Besides the per-version sections, the file carries two shared sections, `Header` and `Endfix`.

### Header and Endfix

For CI, every version's release note is assembled from `Header` and `Endfix`, excluding their titles: `Header` content goes on the first line and `Endfix` content on the last.

A version section may override the defaults by declaring H3-level `Header` or `Endfix` subsections of its own.

### Full changelog link

`**full_changelog!**` is a placeholder tag, not final content. Convert it into a real `Full Changelog` link, for example:

```markdown
**Full Changelog**: https://github.com/Username/repository/compare/v0.1.0...v0.2.0
```

Do not copy that URL. Calculate it with a script in the CI action.

The first release has no previous version to compare against, so it gets no compare link. A single-ref form such as `compare/v0.1.0` resolves to "from that tag to the default branch", which quietly changes as the repository moves on. Point the first version's entry at the tag or release page instead:

```markdown
**Full Changelog**: https://github.com/Username/repository/releases/tag/v0.1.0
```

### Commit hashes

Some old versions' entries lack a commit short hash. Before a release starts, add any missing short hash, replacing the `hash123` placeholder.

## Release assets

Building, attaching, and verifying release artifacts are platform-specific, and so is confirming that a release succeeded. This skill does not cover them. Consult your provider's own documentation and adapt it to your workflow.
