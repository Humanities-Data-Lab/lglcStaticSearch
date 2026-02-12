# Dev Build and Release Automation

Pushes to `dev` now run an automated GitHub Actions workflow (`.github/workflows/dev-release.yml`) which:

1. Reads `EDITION`, parses semantic versioning (`X.Y.Z`), and increments the patch number (`Z + 1`).
1. Updates `EDITION` on `dev` with the new version and commits that change automatically.
1. Runs the project build and tests (`ant -f build.xml test`).
1. Builds release archives (`ant -f buildRelease.xml all`).
1. Creates and pushes a Git tag in the form `vX.Y.Z`.
1. Bundles the generated test pages (`test/search.html`, `test/search-debug.html`, `test/ssTest/`) into `dist/staticSearch_test_pages_<version>.tar.gz`.
1. Creates a GitHub prerelease with generated notes and attached `dist/*.zip` and `dist/*.tar.gz` artifacts.

For workflow testing outside `dev`, run the workflow manually with `workflow_dispatch` on the target branch. Non-`dev` runs execute the same build/package steps, but skip the `dev`-only publish actions (commit `EDITION`, push tag, create prerelease).

## Notes

- Automatic release runs happen on pushes to `dev`; manual runs are available through `workflow_dispatch`.
- `EDITION` should remain semantic-version compatible (`major.minor.patch`).
- Legacy values like `2.0.1beta` are accepted as input for bumping, but output is normalized to strict semver (`2.0.2`).
- GitHub Pages deployment will be handled in a separate workflow step later.
