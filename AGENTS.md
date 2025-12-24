# Repository Guidelines

## About the codebase

- This repo is a Jekyll based static website generator for https://diliprajbaral.com.
- Uses Minmial Mistakes Jekyll theme;
    - Docs: https://mmistakes.github.io/minimal-mistakes/
    - Source: https://github.com/mmistakes/minimal-mistakes

## Project Structure & Module Organization
- `_config.yml` holds site metadata, URL settings, plugins, and defaults.
- Content lives in `index.md`, `about.md`, `blog.md`, and `_posts/` (use `YYYY-MM-DD-title.md`).
- Layout and includes are in `_layouts/` (theme-provided) and `_includes/` (local overrides).
- Styling is in `assets/css/` (entrypoints) and `_sass/` (custom overrides).
- Generated output is in `_site/` and should not be edited.
- Post images live in `assets/images/posts/`.

## Build, Test, and Development Commands
- `bundle install` — install Ruby dependencies from `Gemfile`.
- `bundle exec jekyll serve` — build and serve locally at `http://localhost:4000`.
- `bundle exec jekyll build` — generate the static site into `_site/`.

## Coding Style & Naming Conventions
- Use Markdown with YAML front matter for pages/posts.
- Post filenames follow `YYYY-MM-DD-title.md`.
- Keep HTML indentation at 2 spaces in `_includes/`.
- Prefer ASCII in content unless a non-ASCII character is required.

## Testing Guidelines
- No automated test framework is configured.
- Validate changes by running `bundle exec jekyll serve` and checking key pages manually.

## Commit & Pull Request Guidelines
- No strict commit convention; prefer short, imperative subjects (e.g., “Update hero copy”).
- Include description to detail the changes made
- Cite Codex CLI as co-author
- PRs should include a brief summary and note local preview status.
- For visual changes, include a screenshot or describe the manual check performed.

## Security & Configuration Tips
- Do not commit generated output (`_site/`) or Bundler artifacts (`vendor/`, `.bundle/`).
- Keep `_config.yml` aligned with production URL and permalink structure.
