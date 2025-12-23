# Repository Guidelines

## Project Structure & Module Organization
- `_config.yml` holds site metadata, URL settings, plugins, and permalinks.
- `_layouts/` contains HTML layouts (currently `default.html`).
- `_posts/` stores published blog posts in `YYYY-MM-DD-title.md` format.
- `_drafts/` holds unfinished posts that should not publish.
- `index.md`, `about.md`, and `blog.md` are top-level pages.
- `_site/` is the generated output from `jekyll build` and should not be edited.

## Build, Test, and Development Commands
- `bundle install` installs Ruby dependencies from `Gemfile`.
- `bundle exec jekyll serve` builds and serves the site locally at `http://localhost:4000`.
- `bundle exec jekyll build` generates the static site into `_site/`.

## Coding Style & Naming Conventions
- Use Markdown for content files and YAML front matter at the top of pages/posts.
- Follow the post filename convention: `_posts/2025-12-23-my-title.md`.
- Keep HTML indentation consistent (2 spaces) in `_layouts/`.
- Prefer ASCII in content; avoid special characters unless necessary.

## Testing Guidelines
- No automated test framework is configured.
- Validate changes by running `bundle exec jekyll serve` and checking rendering locally.

## Commit & Pull Request Guidelines
- Git history only shows “Initial commit”; no established commit convention yet.
- Suggested approach: short, imperative subject lines (e.g., “Add about page”).
- PRs should include a brief description of changes and a note about local preview.
- For visual changes, include a screenshot or mention a quick manual check.

## Security & Configuration Tips
- Do not commit generated output (`_site/`) or Bundler artifacts (`vendor/`, `.bundle/`).
- Keep `_config.yml` aligned with the production URL and permalink structure.
