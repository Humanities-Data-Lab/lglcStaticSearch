# staticSearch

### A codebase to support a pure JSON search engine requiring no backend for any XHTML5 document collection

This codebase, developed by Joey Takeda and Martin Holmes, provides a configurable, customizable tool which you can point at an XHTML5 document collection and have it generate a search page which requires no backend server-side component. It creates stemmed indexes of all document text, along with an HTML search page including faceted search features based on `<meta>` tags in the document collection. The search page uses pure JavaScript to query the index, which is a large collection of small JSON files, to provide a rapid and sophisticated search for any small-to-medium website. The search does not require any server-side code at all.

The generation code uses XSLT3 and the search functionality is JavaScript. Implementations of the Porter2 stemmer in XSLT and JavaScript are part of the package. Live search pages based on this code are already in use in the sites [_Mapping Keats's Progress_](https://johnkeats.uvic.ca/search.html), [_The Map of Early Modern London_](https://mapoflondon.uvic.ca/search.htm) and [_The Winnifred Eaton Archive_](https://www.winnifredeatonarchive.org/search.html).

The default branch of this repo is the dev branch; the main branch is used for releases. Formal releases started in early 2020, and major releases will always have their own release branch, so you can pin your own project to a release branch, or to a specific release tag if you want to avoid unexpected changes in behaviour due to codebase changes. For testing to prepare for upcoming changes, you can use the dev branch. Releases are also archived on Zenodo:

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.6329800.svg)](https://doi.org/10.5281/zenodo.6329800)

Full documentation can be found in the file docs/staticSearch.html. Live searchable documentation (built using staticSearch) for the latest release can be found at the [Project Endings site](https://endings.uvic.ca/staticSearch/docs/).

### GitHub Actions overview

CI runs three workflows:

- **Test (test.yml)** — On push to any branch except `dev`: builds the project with Ant, runs Playwright browser tests against the built test site, and, if the branch has an open pull request, deploys a **feature preview** to GitHub Pages at `…/previews/branch-<branch>/search.html`. When a PR is closed or a branch is deleted, that branch’s preview is removed and any orphaned previews are pruned. All preview content lives on a separate `pages-content` branch and is served as one GitHub Pages site.

- **Dev Build and Release (dev-release.yml)** — On push to `dev` (or when run manually): builds with Ant, bumps the `EDITION` file (semver patch), creates a release tag and a GitHub **prerelease** with distribution archives (zip/tar.gz), updates the `pages-content` branch with the new `test/` output, and deploys the **test site** to GitHub Pages at `…/test/search.html`. Runs triggered by the bot (e.g. from CI commits) are skipped to avoid loops.

- **PR Preview Pages (pr-preview.yml)** — When a pull request from the same repo is **merged**: promotes that PR’s branch preview from `previews/branch-<branch>` to a permanent **staging** URL at `…/staging/pr-<number>/search.html`. A follow-up job removes staging folders for PRs that are no longer open (e.g. closed without merge).

Please report all issues you encounter as tickets on the repo.

The code is licensed under both MPL and BSD. 
