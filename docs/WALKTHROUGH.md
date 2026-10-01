# Walkthrough (spoilers)

Every lead has more than one correct query. The ones below are the reference solutions. Column names and extra columns do not matter, and row order only matters where marked.

Try the hints in the game first. They cost a little XP, but this page costs you the mystery.

---

## Case 1: The Missing Crate

Suspects: Otto Brandt (#7), Vera Kline (#12), Silas Moreau (#17), Dex Harlan (#23).

### Lead 1: Open the files (`SELECT`)

```sql
SELECT date, type, location FROM crime_reports;
```

80 rows. Nothing special yet. The point is learning the shape of `SELECT … FROM`.

### Lead 2: Narrow the search (`WHERE`)

```sql
SELECT date, type, description FROM crime_reports WHERE location = 'Warehouse 9';
```

3 rows. The 14 March theft: crate C-7734, lock intact, taken between 23:00 and midnight.

Red herring: `WHERE type = 'Theft'` returns every theft in the city. Filter on *where*, not *what*.

### Lead 3: Witness statements (`LIKE`)

```sql
SELECT person_id, transcript FROM interviews WHERE transcript LIKE '%key%';
```

10 rows, most of them noise (a monkey, a jockey, a donkey). Vera says only three keys exist: hers, Otto's and the night clerk's. **Dex is cleared**, because he has no key.

Red herring: `LIKE 'key'` with no `%` returns 0 rows.

### Lead 4: Roll the tape (`ORDER BY`, `AND`)

```sql
SELECT person_id, timestamp
FROM cctv_sightings
WHERE location = 'Warehouse 9' AND timestamp LIKE '2026-03-14%'
ORDER BY timestamp;
```

7 rows, earliest first. Otto never appears inside the warehouse. **Otto is cleared.**

Red herring: dropping the date filter returns every day on the tape (22 rows).

### Lead 5: The last faces (`LIMIT`)

```sql
SELECT person_id, timestamp
FROM cctv_sightings
WHERE location = 'Warehouse 9' AND timestamp LIKE '2026-03-14%'
ORDER BY timestamp DESC
LIMIT 3;
```

Silas at 23:49, Silas at 23:12, Vera at 21:58. Vera left before the 23:00 window. **Vera is cleared.**

Red herrings: without `DESC` you get the morning shift. Without the date filter you get Dex on the 20th.

### The accusation: Silas Moreau

Select Silas, then run something like:

```sql
SELECT person_id
FROM cctv_sightings
WHERE location = 'Warehouse 9'
  AND timestamp > '2026-03-14 23:00'
  AND timestamp < '2026-03-15';
```

Every row is Silas. A bare lookup of Silas in `people` is rejected as evidence. The query must use `cctv_sightings`.

**Twist:** Silas only opened the door. A voice on the phone told him when, and money told him why. "The Ledger always balances his books."

---

## Case 2: The Broken Alibi

Suspects: Ivo Rask (#44), Rosa Moreau (#31), Vera Kline (#12), Hollis Crane (#52). Silas's phone is `555-0193` and his account is `ACC-1093`.

### Lead 1: Count the calls (`COUNT`)

```sql
SELECT COUNT(*) FROM phone_calls WHERE receiver = '555-0193';
```

17 calls. Red herrings: `caller = '555-0193'` counts the calls he *made*. No `WHERE` counts every call in the city.

### Lead 2: Who keeps calling? (`GROUP BY`)

```sql
SELECT caller, COUNT(*) FROM phone_calls WHERE receiver = '555-0193' GROUP BY caller;
```

6 numbers. `555-0166` rang 6 times. Red herring: grouping without the `WHERE` returns 81 callers.

### Lead 3: The regulars (`HAVING`)

```sql
SELECT caller, COUNT(*)
FROM phone_calls
WHERE receiver = '555-0193'
GROUP BY caller
HAVING COUNT(*) >= 3;
```

`555-0111` (Vera, 3), `555-0150` (Rosa, 4) and `555-0166` (unknown, 6). **Hollis is cleared**, since he rang only twice.

Red herring: `> 3` drops Vera.

### Lead 4: Short calls (`AVG`)

```sql
SELECT caller, AVG(duration)
FROM phone_calls
WHERE receiver = '555-0193'
GROUP BY caller
HAVING COUNT(*) >= 3;
```

Vera 140 s, Rosa 1,320 s (22 minutes), `555-0166` 41 s. **Rosa is cleared.** Twenty-two-minute calls are a sister, not a handler.

### Lead 5: Follow the money (`SUM`)

```sql
SELECT from_account, SUM(amount)
FROM bank_transactions
WHERE to_account = 'ACC-1093'
GROUP BY from_account
HAVING SUM(amount) > 1000;
```

`ACC-0001` (port payroll, 2,800) and `ACC-3317` (5,000 in four drops). **Vera is cleared**, because her account never paid Silas.

### The accusation: Ivo Rask

Select Ivo, then run:

```sql
SELECT name FROM people WHERE phone = '555-0166';
```

(or the same with `account = 'ACC-3317'`). The query must mention the number or account, or use `phone_calls` or `bank_transactions`.

**Twist:** Ivo is only a post box. Every penny in `ACC-3317` came from `ACC-9000`, registered to Tidewater Holdings. A harbour camera shows a grey van leaving the east docks without lights, which leads into Case 3.
