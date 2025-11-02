# Repository Guidelines

## Project Structure & Module Organization
- Root Markdown lessons like `pbx-for-dummies.md`, `Asterisk.md`, and weekly folders `w1/`, `w2/` hold daily modules (`day-00.md` etc.).
- Published assets live under `site/` (static HTML/CSS) and each week folder mirrors it for staged updates. Update Markdown first, regenerate HTML via your preferred exporter, then replace the matching `site/` files.
- Audio, video, and reference media belong in `resources/`; keep filenames descriptive and URL-safe (`kebab-case`).

## Build, Test, and Development Commands
- `open site/index.html` (macOS) or `xdg-open site/index.html` previews the published bundle.
- `python3 -m http.server 8000 --directory site` starts a lightweight local server at http://localhost:8000 for link testing.
- When adjusting week modules, run the same server inside `w1/site` or `w2/site` to validate staged content.

## Coding Style & Naming Conventions
- Author in Markdown using ATX headings (`#`), bold callouts, and collapsible `<details>` blocks to match existing tone; reuse emoji section markers when they clarify the learning flow.
- Wrap lines at natural sentence breaks, use hard line breaks (`two spaces + newline`) for poetically spaced lists as seen in the course outline.
- Asset and Markdown filenames use lowercase kebab-case (`pbx-routing.md`); align links relative to the repository root.
- Keep HTML edits minimal; prefer updating Markdown and re-exporting to avoid diverging sources.

## Testing Guidelines
- No automated test suite yet. Review diffed Markdown in a Markdown preview and load the generated HTML to confirm embeds, media paths, and `<details>` toggles.
- Validate external links and media manually; note issues in the PR if a source is temporarily unavailable.
- Optional: run `markdownlint **/*.md` if you have the CLI installed to catch structural lint issues.

## Commit & Pull Request Guidelines
- Follow existing short, descriptive messages in present tense (`Add week-02 media`, `Refresh site CSS`). Group related lesson updates in one commit to keep history readable.
- Pull requests should summarize scope, list primary files touched, and include screenshots or short screencasts for HTML/CSS changes. Reference related issues or lesson plans.
- Highlight any manual QA performed (e.g., “Previewed w1/site via http.server”) so reviewers can trust the export.
