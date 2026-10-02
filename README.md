# johnapaz.com — staging

[Preview URL](https://staging.johnapaz.com) · [Deployment runs](https://github.com/johnapaz/johnapaz-staging/actions/workflows/staging.yml) · [Website source](https://github.com/johnapaz/johnapaz.github.io/tree/v2) · [Project story and design decisions](https://github.com/johnapaz/johnapaz.github.io#readme)

This repository provides a separate GitHub Pages environment for reviewing changes to the launched V2 site of my personal website. It lets me iterate on layout, navigation, content, and responsive behavior while the production site remains available at [johnapaz.com](https://johnapaz.com).

The website's content and implementation live in **[johnapaz/johnapaz.github.io](https://github.com/johnapaz/johnapaz.github.io)**. This repository holds the staging deployment workflow; generated pages are published as a Pages artifact.

## Why a separate repository?

I wanted the source, planning, and hosting to stay inside GitHub, with an independent preview of a substantial redesign. A separate Pages repository gives staging its own deployment and domain while the main repository retains the production source and the `v2` integration branch.

I chose the GitHub-only approach, separate staging, and review before production promotion. AI assisted with proposing and implementing the workflow mechanics, build configuration, checks, and documentation. The main repository's README explains the wider design decisions and division of work.

This is a shared staging environment. It displays the ref selected in `staging-source.txt` (normally `v2`). A feature branch can replace the shared preview; each branch does not get its own URL. Manual dispatch can also override the ref for one run.

## Technology and deployment

The executable configuration is [`.github/workflows/staging.yml`](.github/workflows/staging.yml).

| Component | Purpose |
| --- | --- |
| **GitHub Actions** | Check out the website source, build it, validate the output, and deploy |
| **Jekyll, Liquid, Markdown, YAML, and Sass/CSS** | Generate the website using the main repository's content, templates, configuration, and styles |
| **Ruby 2.7 and Bundler 2.4.22** | Match the current legacy build dependencies; modernization is future work |
| **Python staging checker** | Validate the generated output using `scripts/check-staging.py` from the source repository |
| **GitHub Pages** | Serve the generated artifact at the staging domain |

Each run:

1. Checks out `johnapaz/johnapaz.github.io` at the selected branch or commit and records its exact commit.
2. Builds in Jekyll safe mode with `_config.yml` and `_config.staging.yml`.
3. Writes the staging domain, crawler instructions, and `build-info.json`.
4. Runs the source repository's staging privacy and internal-link/asset checkers.
5. Uploads the build artifact and publishes it through GitHub Pages if the build succeeds.

The deployment uses GitHub's temporary workflow credentials and OIDC, with Pages write permissions limited to the deployment job. No persistent cross-repository deploy key is required because the source is public.

## When staging updates

The workflow runs on pushes to this repository's `main` branch, on manual dispatch, and on a **30-minute schedule**. GitHub may delay scheduled runs.

A push to the website's `v2` branch runs source-side validation but does not directly trigger this repository's deployment. For an immediate refresh, open [Publish branch preview to staging](https://github.com/johnapaz/johnapaz-staging/actions/workflows/staging.yml), choose **Run workflow**, and run it on `main`.

Edit website content and code in the source repository. Use `staging-source.txt` to select a persistent branch preview; restore it to `v2` after review.

## Reviewing a build

Check the latest workflow run, then compare [`build-info.json`](https://staging.johnapaz.com/build-info.json) with the source commit you intend to review. It records the source branch, commit SHA, and workflow run ID.

Review navigation, downloads, links, images, desktop scrolling, mobile and foldable layouts, keyboard operation, and hover/focus behavior. A successful automated build is one check; it does not establish browser or physical-device review.

Staging configuration disables analytics and includes indexing discouragement. **The preview is public:** `noindex` and `robots.txt` are not access controls. The preview link identifies the configured destination, rather than certifying current availability, DNS, or HTTPS status. Follow [deployment runs](https://github.com/johnapaz/johnapaz-staging/actions/workflows/staging.yml) and the [project backlog](https://github.com/johnapaz/johnapaz.github.io/issues?q=is%3Aissue%20state%3Aopen%20sort%3Aupdated-desc) for current evidence.

## Promotion and recovery

Production promotion happens in the source repository: record the reviewed commit, open a pull request from `v2` into `main`, the production/default branch, and merge after my approval. Production uses its own build configuration.

To recover staging, revert the problematic source change on `v2` and run this workflow again. Verify the resulting commit and deployment. The [staging guide](https://github.com/johnapaz/johnapaz.github.io/blob/v2/docs/staging.md) documents the full review and recovery procedure.

## Follow the project

- [Main README](https://github.com/johnapaz/johnapaz.github.io#readme): purpose, architecture, design authorship, and AI collaboration
- [Project wiki](https://github.com/johnapaz/johnapaz.github.io/wiki): requirements, decisions, roadmap, and development history
- [Live backlog](https://github.com/johnapaz/johnapaz.github.io/issues?q=is%3Aissue%20state%3Aopen%20sort%3Aupdated-desc): bugs, planned work, and release readiness
- [Source credits and license](https://github.com/johnapaz/johnapaz.github.io/blob/v2/LICENSE.md): inherited theme and repository licensing

## Launch record

V2 launch includes site-wide search, an article layout, AI assistance disclosures, social metadata, and internal link/asset validation. [Launch story](https://johnapaz.com/blog/website-v2-launch/) · [Release guide](https://github.com/johnapaz/johnapaz.github.io/blob/main/docs/releasing.md). Production publishes from main; staging remains independent.

![Staging deployment](https://github.com/johnapaz/johnapaz-staging/actions/workflows/staging.yml/badge.svg)
