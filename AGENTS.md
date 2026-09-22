# AGENTS.md

## Project overview

This repository contains a static web application for tracking progress through A2 and B1 driving theory classes. It has no dependencies, server, database, or build process: the entire application lives in `index.html`.

## Main files

- `index.html`: application structure, styles, class data, and behavior.
- `README.md`: user-facing functional description.
- `.github/workflows/deploy-pages.yml`: automatic GitHub Pages deployment.
- `LICENSE`: the project's GPL-3.0 license.

## Development rules

- Keep the project in native HTML, CSS, and JavaScript. Do not add frameworks, dependencies, or a build system unless explicitly requested.
- Keep user-visible text, the README, and workflow step names in Spanish.
- Preserve the responsive design and verify changes on desktop and screens up to 700 px wide.
- Use `CLASSES` as the single source of truth for the class list and `CLASS_DURATION_HOURS` for class duration.
- Do not duplicate derived data such as totals, hours, or statuses.
- Keep confirmation prompts before marking, unmarking, or clearing classes.

## URL state and compatibility

The application stores its state in the URL fragment using `URLSearchParams`:

- `done`: identifiers of completed classes.
- `at`: completion date and time for each class as a Unix timestamp in milliseconds.
- `license`: license filter.
- `status`: status filter.

Search input is temporary and must not be stored in the URL. Any changes to the URL format must remain compatible with older links that contain only `done`.

Marking a class records `Date.now()`. Unmarking a class or clearing progress must also remove its timestamp. Dates are displayed in the browser's local time using the `es-CO` locale.

## Counter behavior

- **Clases totales**, **Completadas**, and **Pendientes** depend only on the license filter.
- The status filter and search input only affect the displayed list.
- **Horas vistas** always represents global progress and is independent of all filters.
- Each completed class adds two hours through `CLASS_DURATION_HOURS`.

## Change validation

Before completing a change:

1. Run `git diff --check`.
2. Confirm that `index.html` remains a complete HTML document and has no duplicate HTML IDs.
3. Test marking, unmarking, and clearing classes.
4. Reload a copied URL and verify that it restores progress, timestamps, and filters.
5. Verify that search works without changing the URL.
6. Confirm that older links without the `at` parameter still work.

## Deployment

Changes pushed to `main` are published through `.github/workflows/deploy-pages.yml`. The workflow copies `index.html` into the GitHub Pages artifact. Do not manually create or commit the `_site` directory.
