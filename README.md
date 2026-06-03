# Workshop Logistics Portal — browser-automation training demo

A single static page for a session on safe browser automation. The teaching
point: professional work is often trapped in an **ordinary browser portal** with
no API, no export, and no connector — yet a bot can still operate through the
same surface a person uses. The automation moves *visible evidence*; a human
owns interpretation, decisions, accountability, and any communication.

End-to-end shape:

> Ordinary mock portal (GitHub Pages) → Axiom clicks **Exceptions only** → Axiom
> extracts the visible table rows → Axiom writes them to a **Google Sheet review
> register** → the automation stops → a human reviews the evidence, adds owners
> and actions, and sends any team update manually.

The boundary the demo dramatizes: **Axiom writes evidence; humans write
judgement.**

| File                             | Role                                                                    |
| -------------------------------- | ----------------------------------------------------------------------- |
| `workshop-logistics-portal.html` | The mock source portal. Self-contained: inline CSS + tiny inline JS.    |
| `review-register-template.csv`   | Header row for the Google Sheet review register (import to set up tabs).|

The portal is deliberately **automation-neutral** — it looks like a normal
internal logistics tool, not a page built for a bot. The only artificial part is
that it is a safe mock with fictional data.

> Fictional demo data. No real client, supplier, staff, address, or contact
> details are used.

## Deploy to GitHub Pages

1. Push to the default branch (`main`) of a **public** repo at the repo root.
2. **Settings → Pages → Deploy from a branch** → `main` / `/ (root)`.
3. Live at `https://<your-username>.github.io/demo-logistics-portal/workshop-logistics-portal.html`.

Use the HTTPS Pages URL for the live demo (Axiom works more naturally against
HTTPS than a local `file://`). Opening the file locally still works for dev:

```powershell
python -m http.server 8000
# http://localhost:8000/workshop-logistics-portal.html
```

## What the portal exposes

It behaves like a normal portal — the bot works with what a human can see and
click. There are **no** automation-specific affordances (no hidden export, no
helper textarea, no API). Useful natural selectors:

| Purpose                    | Selector / cue                                |
| -------------------------- | --------------------------------------------- |
| "Exceptions only" filter   | visible text "Exceptions only" / `#filter-exceptions` |
| Logistics table            | `#logistics-items`                            |
| Currently **visible** rows | `#logistics-items tbody tr:not([hidden])`     |

Filtered-out rows get the real `hidden` attribute, so after a filter is clicked
the table genuinely contains only the visible rows — for both a learner and for
Axiom's extraction. There is no plain-text helper block to extract from; Axiom
reads the visible filtered table itself.

The seven-row dataset (fixed): 3 Ready, 4 exceptions (HDMI adapter, Printed
handouts, Sponsor pack, Catering confirmation), 1 High impact (HDMI adapter).

## Google Sheet review register

Create a Google Sheet named **`Workshop Logistics Review Register`** with a tab
named **`Current exceptions`**. Import `review-register-template.csv` (File →
Import → Insert new sheet, or paste the header row) to lay out the columns.

Column ownership — the heart of the lesson:

| Column                 | Filled by                 |
| ---------------------- | ------------------------- |
| Run ID                 | Axiom or prefilled/manual |
| Checked at             | Axiom or manual           |
| Source page            | Fixed value, or Axiom/manual |
| Item                   | Axiom                     |
| Type                   | Axiom                     |
| Ref                    | Axiom                     |
| Status                 | Axiom                     |
| ETA / update           | Axiom                     |
| Required by            | Axiom                     |
| Impact                 | Axiom                     |
| Portal owner           | Axiom                     |
| Source notes           | Axiom                     |
| Human review status    | Human                     |
| Action owner           | Human                     |
| Next action            | Human                     |
| Ready for team update? | Human                     |
| Reviewer notes         | Human                     |

> Axiom writes the evidence columns. A human fills the judgement columns.

**Two distinct time values.** "Checked at" is the *automation run time* — when
Axiom read the portal — while the portal's own "Portal last updated: 9:30 AM" is
a separate piece of evidence about the source. Keep them in different columns; a
fixed value for "Checked at" is fine for the live demo.

### Axiom write settings

- **Write option** — for the teaching demo use **Clear data before writing** so
  each run resets cleanly. For a real workflow use **Add to existing data** to
  preserve run history. Contrast the two during the session — it supports the
  accountability discussion.
- **Write method** — use **raw**. Store what was seen as evidence; don't let the
  sheet interpret copied text as formulas. (Store what was seen; don't transform
  it silently.)

## Suggested Axiom run

1. Open the portal's GitHub Pages URL.
2. Click **Exceptions only**.
3. Extract the visible rows from the `Logistics items` table
   (`#logistics-items tbody tr:not([hidden])`).
4. Write the extracted rows to the `Current exceptions` tab of the Google Sheet.
5. **Stop.** A human reviews the evidence, adds owners/actions, marks
   "Ready for team update?", and sends any team update manually.

The bot flow never touches the item-details panel on the portal — that panel is
for the trainer to walk through human review, not part of the automation.

## Framing for the session

> A coordinator normally checks this browser-only workshop logistics portal
> before a client workshop. There is no approved API, export, or connector.
> Axiom clicks the same filter a person would use, extracts the visible
> exception rows, and writes them into a Google Sheet review register. The
> automation stops there. A human reviews the evidence, adds owners and actions,
> and manually sends any team update.
