# Hacking

Developer notes for maintainers.

## Releases

Releases use semantic versioning with a `v` prefix (e.g. `v0.2.0`). The
GitHub Actions that consume this repository reference floating major-version
tags (e.g. `v1`) that are updated automatically as part of this process.

### 1. Create a draft release

Run the release script to create a draft GitHub release with auto-generated
notes:

```sh
# Preview (dry-run, default)
nu .github/scripts/release.nu

# Bump minor version (default) and create the draft
nu .github/scripts/release.nu --dry-run=false

# Bump major or patch instead
nu .github/scripts/release.nu --bump major --dry-run=false
nu .github/scripts/release.nu --bump patch --dry-run=false
```

The script determines the next version by bumping the latest existing tag,
generates release notes from the git log, and creates a draft release via
`gh`.

### 2. Edit the draft in GitHub

Open the draft release on GitHub and revise the title, description, and
release notes as needed before publishing.

### 3. Publish the release

Publishing the release creates the version tag (e.g. `v0.3.0`). After
publishing, update the floating major-version tags so that consumers
referencing `v0` (or `v1`, etc.) pick up the new release:

```sh
# Preview
nu .github/scripts/update-floating-tags.nu

# Apply
nu .github/scripts/update-floating-tags.nu --dry-run=false
```

### 4. Verify tags

Confirm the floating tag points to the correct release:

```sh
git tag -l 'v*'
git rev-parse v0      # should match the new release tag
git rev-parse v0.3.0  # same commit
```
