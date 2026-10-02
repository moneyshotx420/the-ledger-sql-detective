# Testing

## Automated content check

Open the game, open the browser console, run:

```js
__selfTest()
```

Expected output: `Self-test passed: 273 checks`. A failure lists exactly which case, step, lesson or table broke.

It verifies:

- Every table has 50 to 150 rows.
- Each step's expected SQL runs, returns rows, and matches itself.
- Each step has three hints and at least one table for the free tip.
- Clue links point at real, different exhibits.
- No red-herring query can be mistaken for the real answer.
- Every non-culprit suspect is cleared by some step, and the culprit never is.
- The reference accusation is accepted, every wrong suspect is rejected, and each wrong suspect has a rebuttal line.
- A bare `SELECT * FROM people WHERE id = <culprit>` is rejected as evidence.
- Academy: lesson ids are unique, every lesson has a body, at least two examples, at least two slips and a practice question, every example returns rows, every practice answer runs, matches itself and satisfies its own required pattern, no slip would be blocked by the read-only guard, and every lead topic in `CASES` has a lesson in `CONCEPT_LESSON`.

The self-test cannot check that a slip's explanation is true. When you add one, run it from the lesson ("See what happens") and make sure the result or error matches what the text says. See [ACADEMY.md](ACADEMY.md).

Run it after any edit to `CASES`, `ACADEMY`, the seeded data, or `judgeAccusation`.

## Manual UI checklist

This is the list of paths that were exercised, with the expected result. Use it as a regression pass.

### Home
- [ ] Five case tiles. Case 1 open. Case 2 shows "Close Case 1 first". Cases 3 to 5 show "In development".
- [ ] Clicking a sealed or locked tile shakes it and does nothing else.
- [ ] Header chips show rank, XP, cases closed, stars and badges.

### Briefing folder
- [ ] Clicking an open case shows a closed folder cover with a **×** button.
- [ ] **×**, **Esc**, or clicking outside the folder closes it and returns home. The case is not started.
- [ ] **Open the file** flips the cover and types the briefing with clacks. Clicking the page skips ahead.
- [ ] Clicking **Open the file** twice does nothing extra.
- [ ] Closing mid-typing and opening another briefing does not leak text or buttons from the first.
- [ ] **Not yet** returns home. **Take the case** starts the case.

### Case screen
- [ ] Every lead shows a free **Records room tip** with the table name(s) and column names.
- [ ] Tapping a table name shows its first 5 rows in the results panel, with no penalty.
- [ ] **Ask Calloway** gives nudge (−10), keyword (−20), partial query (−30), then becomes "No more hints". The partial query has a paste button.
- [ ] **Clear** empties the editor. Running an empty editor gives a gentle message.
- [ ] Typing highlights keywords, functions, tables, strings and numbers. **Ctrl+Enter** (Cmd+Enter) runs.
- [ ] `DELETE`, `DROP` and similar show "Read-only". `WITH RECURSIVE` shows "Not allowed here".
- [ ] A missing table, a missing column, a syntax error and an open quote each give a plain-English explanation plus the raw SQLite text.
- [ ] Only the first statement of `a; b` runs, and the results header says so.
- [ ] A wrong result shows your shape beside the expected shape, the expected columns, and marks surplus rows in red.
- [ ] A red-herring query shows the targeted explanation and breaks the streak.
- [ ] A correct result pins a clue with a card flip, draws red string, shows the XP breakdown, and may stamp a suspect CLEARED.
- [ ] After solving, running further queries (even broken ones) never removes the **Next lead** button. Errors there do not cost the next lead its first-try bonus.
- [ ] **Next lead** shows the next lesson. After the last lead it shows the accusation card.

### Accusation
- [ ] **Accuse** with no suspect selected shakes the lineup.
- [ ] **Accuse** with no query asks for evidence.
- [ ] Wrong suspect: screen shake and thud, credibility −20, streak reset, a rebuttal specific to that suspect.
- [ ] Right suspect with a bare name lookup, or with rows that include other suspects: "Evidence too thin", no penalty.
- [ ] Right suspect with a clean evidence query: case closes.

### Case closed
- [ ] The report opens with the CASE CLOSED stamp slamming, a thud and screen shake.
- [ ] The twist types out. **×** closes it and leaves you on the case screen.
- [ ] Buttons: **Next case** (opens its briefing, or says sealed), **See the board**, **Replay for 3 stars** (only below 3 stars).
- [ ] The closed case card offers free play, **Next case**, and **Replay**. Replay resets progress for that case but keeps the best stars.
- [ ] Promotions queue after the report. **Esc** or **Back to work** dismisses them one at a time.

### Drawers and toggles
- [ ] **Records room** lists 11 tables with row counts and columns. **Peek** shows 5 rows in place, on the home screen too. **Use in my query** appears inside a case.
- [ ] Peeking all 11 tables awards No Stone Unturned.
- [ ] **Notebook** lists solved queries and **Load into editor** works inside a case.
- [ ] **Badges** shows 14, earned ones lit.
- [ ] **Esc** closes the drawer. The folder and promotion overlays also close with **Esc**.
- [ ] **Music** toggles the loop. **Sound** mutes everything. Turning Music on while muted explains why you hear nothing.

### SQL Academy
- [ ] **Academy** in the top bar opens a wide drawer with 14 lesson cards in three groups and a progress bar. Cards lean toward the mouse.
- [ ] A lesson shows the explanation, the syntax pattern, examples, slips and a practice question, with Previous and Next buttons and **← All lessons**.
- [ ] **Run** shows a result table (first 8 rows and a row count). Editing the SQL and running again shows the new result. **Reset** restores the original.
- [ ] Running an example marks the lesson studied (a toast, "studied" in the heading, a STUDIED stamp on the list card).
- [ ] **See what happens** on a slip runs it and shows the error in plain English, or the misleading result.
- [ ] The practice check says how many rows you returned against how many are expected, shows a hint after two misses, requires `WITH`, `CASE` or `OVER` where the question is about them, and shows PASSED when the rows match. **Ctrl+Enter** checks.
- [ ] Studying all 14 lessons awards Top of the Class.
- [ ] Inside a lead, the topic chip (for example `WHERE ↗`) opens that lesson. Inside a case, **Use in my query** copies an example into the editor and closes the drawer.
- [ ] Esc closes the drawer. At 375 px wide the drawer fills the screen with no sideways scroll.

### XP pop-out and 3D
- [ ] Solving a lead pops a spinning 3D coin out of the "+N XP" total, then it flies to the rank bar with mini coins. The top bar and rank only update when it lands, then pulse and count up.
- [ ] Closing a case does the same from the report. A rank-up shows the promotion screen after the coin lands, with the insignia spinning in.
- [ ] Asking for a hint floats a red "Hint −N XP" label.
- [ ] A new lead flips in. Evidence cards flip when pinned and the pin drops. Stamps tilt in. The CASE CLOSED stamp slams in 3D.
- [ ] Case tiles, suspects, evidence cards and the home title lean toward a mouse pointer. Sealed tiles do not. Nothing tilts on touch.
- [ ] With "reduce motion" on, there is no coin, tilt or flip. XP simply updates.

### Save and restore
- [ ] XP, rank, stars, badges, notebook, case progress, the unsent query draft, and the Music and Sound settings survive a reload.
- [ ] The hot-lead streak resets on reload.
- [ ] A corrupted or old-format save does not crash the game.
- [ ] If storage is blocked (private window), the game still plays and simply does not save.

### Layout
- [ ] At 375 px wide: no horizontal page scroll, the header wraps, the editor font is 16 px so iOS does not zoom, the folder **×** is reachable.
- [ ] Suspect lineup, evidence board and result tables stay inside the screen (tables scroll inside their own box).
- [ ] With "reduce motion" enabled, animations are effectively disabled and the rain is static.

### Audio
- [ ] Nothing plays before the first click or key press.
- [ ] After the first click the music fades in. Measured level is steady and quiet, with no clipping. There is no continuous rain or hiss under it, only the brush drums on the beat.
- [ ] Switching tabs pauses audio and resumes it when you return.
