# Dev notes (not published)

This repo is a Jekyll [just-the-docs](https://just-the-docs.com/) site published
via **GitHub Pages**. It hosts a company-agnostic prompt library for building a
secure Databricks AI deliverable workflow with Genie Code.

## Local preview

```bash
bundle install
bundle exec jekyll serve   # http://localhost:4000
```

## Publish on GitHub Pages

1. Create the repo and push.
2. In **Settings → Pages**, set the source to **GitHub Actions** (recommended for
   `remote_theme`) or to the branch root if using the classic Pages build.
3. Update `OWNER/REPO` in `_config.yml` (`aux_links`) after the repo exists.

## Structure

```
_config.yml                 site + theme + olive color scheme selection
_sass/color_schemes/olive.scss   brand palette
index.md                    home / overview
steps/step-00..07-*.md      one page per build step (each holds its prompt)
```

## Conventions

- One step = one page. Each page holds the **company-agnostic prompt(s)** for that step.
- Prompts use `[UPPERCASE_BRACKETS]` placeholders. No real company details in the repo.
- Step pages are drafted one at a time, in build order.
