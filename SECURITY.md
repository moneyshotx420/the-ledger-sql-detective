# Security Policy

## What this project is

The Ledger is a static web page. There is no server, no account system and no database of users. Everything runs in your browser:

- The "city records" are an in-memory SQLite database, rebuilt on every page load. Nothing you run can change anything outside your own tab.
- Your progress is stored only in your browser's `localStorage` under the key `ledger-casebook-v1`. It is never sent anywhere.
- The only network requests are the page itself, the [sql.js](https://github.com/sql-js/sql.js) script from `cdnjs.cloudflare.com` (a pinned version), and the fonts from Google Fonts. There is no analytics or tracking.

## Supported versions

Only the latest version on the `main` branch, which is the version served on GitHub Pages, is supported. Older versions do not receive fixes.

## Reporting a vulnerability

Please report security problems privately, not in a public issue.

1. Go to the repository's **Security** tab and choose **Report a vulnerability**, if that option is shown.
2. If it is not shown, open a public issue that says only that you have a security report and want a private way to share it. **Do not put any details in the issue.** The maintainer will then arrange a private channel.

Please include what you found, the steps to reproduce it, the browser and device, and what you think the impact is.

This is a hobby project maintained in spare time. Reports are handled on a best-effort basis, and you will be credited in the changelog if you want to be.

## What counts as a security issue

Examples worth reporting:

- A way to get script execution from text the game displays, for example through a case, lesson, query result or a saved notebook entry (cross-site scripting).
- Anything that lets one page or site read or change another site's saved progress.
- A change to how scripts are loaded that would let a third party run code in the page.

## What does not

- **Bypassing the read-only query guard** so that you can change the in-memory database. It only affects your own tab and resets on reload. It is still a bug, so please open a normal issue.
- **Editing your own save** to give yourself XP, badges or stars. It is your game.
- **Slow or heavy queries.** The in-browser SQLite has no timeout. A very large cross join can slow your own tab. This is a known limitation, listed in the README.

## Known hardening opportunities

These are not vulnerabilities, but they are honest gaps:

- The sql.js script tag does not use Subresource Integrity (an `integrity` hash). The version is pinned, but a compromised CDN could still serve different code.
- The page does not set its own Content Security Policy. GitHub Pages does not allow custom headers, so one would need a `<meta>` tag.

Contributions that close these are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).
