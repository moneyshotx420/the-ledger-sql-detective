# Contributing to The Ledger

Thanks for wanting to help. The Ledger is a small project and every kind of help is welcome: bug reports, a typo in a lesson, a better explanation, a new Academy lesson, or a whole new case.

By taking part you agree to follow the [Code of Conduct](CODE_OF_CONDUCT.md). Security problems go through [SECURITY.md](SECURITY.md), not a public issue.

## Ways to contribute

| You want to | Do this |
|---|---|
| Report a bug | Open an issue with the **Bug report** form. The more exact your steps, the faster it gets fixed. |
| Point out a wrong or confusing lesson, clue or hint | Open an issue with the **Content error** form. Say which lesson or lead and quote the text. |
| Suggest a feature | Open an issue with the **Feature request** form. |
| Fix something small | Open a pull request directly. |
| Add a case or lesson | Open an issue first so we can agree on the idea, then follow [docs/CASE-AUTHORING.md](docs/CASE-AUTHORING.md) or [docs/ACADEMY.md](docs/ACADEMY.md). |

## How the project is built

The whole game is one file, `index.html`: styles, markup, then one script. There is no build step, no package manager and no framework, on purpose. Anyone can open the file, read it and change it. Please keep it that way. If a change would need a build tool or a dependency, open an issue to discuss it first.

For a tour of the code, read [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Running it locally

Clone the repository and open `index.html` in a modern browser. It needs internet access to load the SQL engine and fonts.

Saving progress needs `localStorage`, which some browsers block on `file://` pages. If your progress does not save, serve the folder instead:

```bash
npx serve .
```

Then open the address it prints.

## Before you open a pull request

Run through this list. The pull request template repeats it.

1. **Run the self-test.** Open the browser console on your build and run:

   ```js
   __selfTest()
   ```

   It must report `Self-test passed`. It checks every case and every Academy lesson. If you added content, it also checks that your content is consistent.
2. **Play what you changed.** Use the relevant sections of [docs/TESTING.md](docs/TESTING.md). If you touched the UI, check it at about 375 px wide, and once with "reduce motion" turned on in your operating system.
3. **Check there are no console errors** while you do it.
4. **Update the docs.** If behaviour, scoring or content changed, update the README or the matching file in `docs/`. If a self-test count changed, update the number in the README and in `docs/TESTING.md`.
5. **Add a line to [CHANGELOG.md](CHANGELOG.md)** under `Unreleased`.

## Style

- **Match the surrounding code.** Same naming, same comment density, same idioms. The script uses plain modern JavaScript, string templates for markup, and one delegated event listener per area.
- **Escape anything that comes from the player.** Text built from a query, a transcript or a saved value goes through `esc()` before it goes into `innerHTML`.
- **Keep colours in the CSS variables** at the top of the stylesheet. Do not hard-code a new colour inline.
- **Respect reduced motion and touch.** Anything that animates must still work, quietly, with `prefers-reduced-motion`. Pointer-only effects must skip touch.
- **Comments say why, not what.**

## Writing content

- **Story text** is short, punchy lines. No walls of text.
- **A lead teaches one idea.** A lesson is two or three lines plus one example.
- **Check what a wrong query returns.** Every red herring must give a different result from the right answer.
- **Check that explanations are true.** The self-test cannot tell whether a "common slip" really behaves as described. Run it from the lesson and read the result.

## Commits and pull requests

- Write the commit summary in the imperative, under about 70 characters: "Add LEFT JOIN lesson", not "Added" or "Adding". Use the body to explain why.
- Keep a pull request to one topic. Smaller is easier to review.
- Describe what you changed, why, and how you tested it. Screenshots help for anything visual.

## License

By contributing, you agree that your contribution is licensed under the [MIT License](LICENSE), the same as the rest of the project.
