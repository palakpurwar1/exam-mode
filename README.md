# Last Minute Revision — CuriousJr

Single-file, self-contained revision notes page for K8 (Class 1–8). Chapter cards open the
Final CLP PDF for each chapter straight from Google Drive.

`index.html` has no build step and no external dependencies (Nunito is the only remote asset).
Deploy by serving the file, or drop it on Netlify.

## Two views

**Default** — parent picks board → class → subject.

```
/index.html
```

**Class-locked** — class comes from the URL and the class picker is hidden. Use this for
per-class links sent to a specific batch.

```
/index.html?class=5
/index.html?class=7&board=ICSE
```

### Query params

| Param | Aliases | Values | Effect |
|---|---|---|---|
| `class` | `grade`, `c` | `1`–`8` | Locks to that class and hides the class picker |
| `board` | `b` | `CBSE`, `ICSE` | Preselects the board |

Non-digits in `class` are stripped, so `?class=Class%208` resolves to `8`. An unknown class
(e.g. `?class=99`) is ignored and the page falls back to the default view. `board` is ignored
if that class has no chapters for it. The board toggle hides itself automatically when a class
has data for only one board.

## Data

Chapters are embedded in `index.html` as a `const DATA` array, generated from the CLP tracker
sheet. Each entry:

```js
{ gradeNum: "7", board: "ICSE", chapter: "Motion", subject: "Physics",
  pdfUrls: ["https://drive.google.com/file/d/<id>/preview", ...] }
```

Links are read from the column **labelled `Final CLP PDF`**, located per sheet rather than by a
fixed letter — it sits in column **J** in 30 sheets and column **K** in 6 sheets that carry an
extra unlabelled link column at J. Reading a fixed index picks up the wrong (stale) links.

Current data: 349 chapters. Seven have multiple parts and render Part 1 / Part 2 / Part 3
buttons — 6 CBSE English (Conjunctions, Prepositions), 7 CBSE English (Adjectives),
7 ICSE Physics (Motion, Energy, Light energy), 8 ICSE Physics (Force and Pressure).

## Sharing

Each card has a share button. On mobile it opens the native share tray, pre-filled with
`Revise <chapter>:` plus the link. On desktop it copies the link and shows a confirmation
toast. If the share tray is blocked — which is the case inside an iframe without
`allow="web-share"`, e.g. a Google Sites embed — it falls back to copy-and-toast.
