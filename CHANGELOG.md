# Changelog

All notable changes to The Ledger are recorded here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). The version numbers are informal markers for milestones, not a published API.

## Unreleased

### Added
- `CONTRIBUTING.md`, `docs/ARCHITECTURE.md`, a docs index (`docs/README.md`), issue forms (bug report, content error, feature request) and a pull request template.

### Changed
- `SECURITY.md` and `CODE_OF_CONDUCT.md` now describe this project and give a real private route for reports. Before, they were unedited GitHub templates, the security policy listed made-up versions, and the Code of Conduct had a blank contact.
- The README is reorganised with a contents list, a repository layout and a documentation index.

## 0.3.0 - 2026-10-02

### Added
- **SQL Academy.** Fourteen short lessons, from `SELECT` to window functions, in a wide drawer opened from **Academy** in the top bar. Each has an explanation, a syntax pattern, editable examples that run against the city's records, common slips with a "see what happens" button, and a practice question checked by result-set comparison. See [docs/ACADEMY.md](docs/ACADEMY.md).
- A topic chip on every lead (for example `WHERE ↗`) that opens the matching lesson.
- **Top of the Class** badge for studying every lesson. There are now 14 badges.
- **XP pop-out.** A spinning 3D coin pops out of the "+N XP" total, flies to the rank bar with mini coins, and the bar and XP count up when it lands. Hint costs float a red label.
- **Little 3D touches.** Pointer tilt with glare on case tiles, suspects, evidence cards, lesson cards and the title. New leads flip in, pins drop, stamps tilt in, the CASE CLOSED stamp slams in 3D, and the rank insignia spins. All of it is off for touch and for reduced motion.
- `docs/ACADEMY.md`, a guide to editing and adding lessons.

### Changed
- The music no longer has a continuous rain sound under it. The falling-rain visual is unchanged.
- The self-test now runs 273 checks and includes every Academy lesson.
- Query results are capped at 5,000 rows and flagged as truncated (shown as `5000+ rows`).
- The "no such column" message now covers plain typos as well as missing quotes.

### Fixed
- The HAVING lesson's off-by-one example showed no difference. It now uses a threshold where `>` and `>=` give different results.

## 0.2.0 - 2026-10-01

First public release on GitHub, with GitHub Pages.

### Added
- **Free records-room tip** on every lead and on the accusation: the table name(s) to use and their columns, with a tap-to-peek preview.
- **Background music**, a synthesised noir jazz loop, with separate **Music** and **Sound** buttons.
- The built-in `__selfTest()` content checker.
- Documentation: README, walkthrough, testing checklist, case-authoring guide, and an MIT license.

### Changed
- An accusation now has to be backed by the real evidence table, not just a lookup of the suspect's name.
- Rank thresholds were raised so promotions spread across all five cases.
- The hot-lead streak lasts one sitting and resets when the page reloads.
- The Records room **Peek** shows rows in place and works from the home screen.

### Fixed
- After solving a lead, a mistyped query removed the **Next lead** button, and also cost the next lead its first-try bonus.
- The briefing folder could not be closed before it was opened, and opening a second briefing quickly mixed the text of the two.
- `WITH RECURSIVE`, `RANDOMBLOB` and `ZEROBLOB` could freeze the tab. They are now blocked.
- A save written by an older version, missing newer fields, could break the game. Saves are now healed on load, and an unreadable save starts fresh.
- An unsent query was lost when you left the case. Drafts are now saved as you type.
- The editor did not resize with the window, and iOS zoomed in on it. It now resizes, and uses a 16 px font on phones.

## 0.1.0 - 2026-09-30

First playable build.

### Added
- **Case 1, The Missing Crate** (`SELECT`, `WHERE`, `LIKE`, `ORDER BY`, `LIMIT`) and **Case 2, The Broken Alibi** (`COUNT`, `GROUP BY`, `HAVING`, `AVG`, `SUM`), five leads each.
- A seeded city database of eleven tables, with the real clues hidden among innocent rows.
- Result-set answer checking, red herrings, near-miss feedback and plain-English SQL errors.
- Evidence board, suspect lineup, accusations, notebook and records room.
- XP, hot-lead streak, five ranks, one to three stars per case, and badges.
- Case-file folder briefing, CASE CLOSED stamp, sound effects, falling rain, and saving in `localStorage`.
