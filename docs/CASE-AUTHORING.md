# Adding or editing a case

All story and puzzle content lives in the `CASES` array in `index.html`. The engine reads it; you do not touch engine code to add a case.

A sealed placeholder looks like this. Replace it with a full case object to make it playable:

```js
{ id:'c3', num:3, title:'The Getaway Car', skills:['INNER JOIN','LEFT JOIN'], sealed:true, tagline:'…' }
```

## Case object

```js
{
  id:'c3', num:3, title:'The Getaway Car',
  skills:['INNER JOIN', 'LEFT JOIN'],            // shown on the folder and the tile
  tagline:'One line for the home screen.',
  briefing:[ 'Short, punchy lines.', '…' ],       // typed onto the folder, one paragraph per entry
  suspects:[ids…],                                // people.id values shown in the lineup (4 fits the layout)
  culprit: id,                                    // must be one of suspects
  culpritLinks:[stepIndexes…],                    // red strings from the culprit card to these exhibits
  notes:{ [id]:'One line shown when you tap a suspect.' },
  intro:'Partner line shown when the case opens.',
  steps:[ …4 to 6 step objects… ],
  accusation:{ … },
  wrong:{ [id]:'Rebuttal for each non-culprit suspect.' },
  twist:[ 'Lines typed after CASE CLOSED.', 'The last ones point at the next case.' ],
}
```

## Step object

```js
{
  id:'s1', concept:'INNER JOIN', title:'Two drawers, one question',
  lesson:'2 to 3 lines that teach one idea.',
  example:"SELECT … ;",                         // one worked example, shown highlighted
  task:'What the player must produce, in plain words.',
  expected:"SELECT … ",                         // the reference query. Its RESULT is the answer.
  ordered:true,                                 // optional: row order must match (use for ORDER BY / LIMIT steps)
  tables:['people', 'vehicles'],                // optional: tables for the free tip. Defaults to the tables found in `expected`
  herrings:[                                    // optional: common mistakes that return a plausible wrong answer
    { sql:"SELECT … (the mistake)", msg:'Partner explains what was missed.' },
  ],
  hints:[ 'nudge', 'keyword', 'partial query with ___' ],   // exactly three, costing 10 / 20 / 30 XP
  clue:{ title:'Short', text:'What the evidence card says.', links:[0] },  // links: indexes of EARLIER-or-later steps to string to
  clears:[ { id:23, why:'Why the evidence rules this suspect out.' } ],   // optional: stamps CLEARED
  win:'Partner reaction to a correct answer.',
}
```

Design guidance:

- **One idea per step.** The lesson is 2 to 3 lines plus one example. Do not teach a second concept in the task.
- **Link the concept to an Academy lesson.** The `concept` string (for example `'INNER JOIN'`) becomes a clickable chip on the lead. Add it to `CONCEPT_LESSON` so it opens the matching lesson. The self-test fails if a lead's concept has no lesson. See [ACADEMY.md](ACADEMY.md).
- **Make the mistake teach.** A red herring should be the query a learner actually writes, and its `msg` should say what they missed, not just "wrong".
- **Check what the wrong query returns.** The red herring's result must differ from the expected result, otherwise the step cannot be told apart from the mistake. `__selfTest()` checks this.
- **Hide clues among innocents.** Add noise rows so that filtering matters.
- **Every non-culprit suspect must be cleared** by some step's `clears`, so the lineup empties as the evidence comes in. The self-test enforces it.
- **Row order:** only set `ordered:true` when the lesson is about order. Otherwise any order passes.
- **Keep numbers stable.** If a step reports "17 calls", make sure the data really gives 17.

## Accusation object

```js
accusation:{
  text:'What the player is asked to do.',
  tables:['cctv_sightings', 'people'],          // shown as the free tip
  evidence:/cctv_sightings/i,                   // the query text must match this, so a bare name lookup cannot pass
  needs:'Shown when the evidence is rejected for not matching.',
  proof:"SELECT … ",                            // a reference accusation query. Used by __selfTest().
  hints:[ 'nudge', 'partial query' ],           // exactly two
}
```

An accusation is accepted when the culprit is right, the query returns rows, the query text matches `evidence`, every row mentions the culprit (by `person_id`/`owner_id`, by `id` alongside a `name` column, or by name, phone or account text), and no row mentions another suspect. Mention rules live in `mentions()` and the verdict in `judgeAccusation()`.

## Data

All rows come from `buildData()`, using a seeded generator (`rng(314159)`).

- **Plot people** are listed in the `PLOT` array with fixed ids (7, 12, 17 …). Add new characters there. They keep the same id every time.
- **Random innocents** fill the other ids. Because the generator is a single sequence, adding or removing a random call in the middle shifts every value after it. When you add rows, append them after the existing generation code for that table, or re-run the self-test and re-check any counts quoted in clue text.
- **Plot rows** are added explicitly (for example the Silas phone calls and the `ACC-3317` payments), then shuffled in with the noise.
- Keep each table between **50 and 150 rows**. The self-test enforces this.
- New tables need an entry in `SCHEMA`, `TABLE_INFO`, and an INSERT in `buildDB()` (the loop handles it automatically once `SCHEMA` and the data both have the table).

## Unlocking and rewards

- A case unlocks when the previous case has been closed (`unlocked()`). Remove `sealed:true` when the case is ready.
- Add badges in `BADGES` and award them with `award('id')` in the `closeCase` timeout. Badges for Cases 3 to 5 already exist and are marked as reserved.
- Rank thresholds are in `RANKS`. They are tuned so promotions arrive roughly across five cases.
- `closeCase` currently awards `case_cracker` for `c1` and `count_on_me` for `c2`. Add the equivalent line for new cases (`join_master` for `c3`, `paper_pusher` for `c4`, `unmasked` for `c5`).

## Checklist before you ship a case

1. Run `__selfTest()` in the console. Fix anything it reports.
2. Play the case start to finish, once with the reference queries and once making the common mistakes. Check that each lead's concept chip opens the right Academy lesson.
3. Check the phone width.
4. Walk through [TESTING.md](TESTING.md).
5. Add the solutions to [WALKTHROUGH.md](WALKTHROUGH.md).
