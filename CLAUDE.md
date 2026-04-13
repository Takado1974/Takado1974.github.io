# Claude Instructions for Takado Website

## Project Overview

Static website for **Takado** — a martial arts club in Espoo, Finland.
Single-page app (`index.html`) that loads Markdown content dynamically using `marked.js`.

## Architecture

- `index.html` — single HTML file, all JS inline, loads Markdown pages via `fetch()`
- `style.css` — all styles
- `locales.json` — UI label translations (Finnish + English)
- `pages/fi/*.md` — Finnish content pages
- `pages/en/*.md` — English content pages
- `images/` — all images

### Pages (must exist in both `fi/` and `en/`)

| File | Finnish nav | English nav |
|------|-------------|-------------|
| `home.md` | (home) | (home) |
| `tule-mukaan.md` | Tule mukaan | Join us |
| `lajikuvaukset.md` | Lajikuvaukset | Disciplines |
| `junioritoiminta.md` | Junioritoiminta | Juniors |
| `harjoitusajat.md` | Harjoitusajat | Schedules |
| `hinnasto.md` | Hinnasto | Prices |
| `ohjaajat.md` | Ohjaajat | Instructors |

## Content Rules

- **Always maintain both language versions** — when changing `pages/fi/*.md`, update `pages/en/*.md` too, and vice versa.
- Content is in Markdown. HTML tags inside `.md` files work but should be avoided unless necessary.
- Images are referenced as `../../images/filename.ext` from inside Markdown files.
- Nav labels and UI strings live in `locales.json`, not in Markdown files.

## Sidebar

The left sidebar contains two stacked panels inside `.sidebar-column`:
- `#news-sidebar` — static news items (hardcoded HTML)
- `#notifications-sidebar` — dynamic, fetched from `https://myClub-proxy.netlify.app/.netlify/functions/notifications`

Notifications are fetched once on page load via `fetchNotifications()`. The data is stored in `notificationsData` (module-level). On language switch, `renderNotifications()` re-renders the cards with the new locale labels. The notification body text is displayed as plain text (escaped); cards with body > 200 characters get a "Lue lisää / Read more" toggle button. Expected API response shape: array of objects with `title`, `date` (or `created_at`), and `body` fields.

## Coding Conventions

- No build step, no npm, no bundler — plain HTML/CSS/JS only.
- All JavaScript is inline in `index.html`. Do not create separate `.js` files.
- Keep the single-file structure of `index.html`.
- CSS uses no preprocessor — edit `style.css` directly.

## Commit Style

- Write concise commit messages in English.
- Group related changes (e.g., both language versions of a page) into one commit.
- Do not commit unrelated files together.
