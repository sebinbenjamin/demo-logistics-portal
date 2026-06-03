# Workshop Logistics Demo — browser-automation training pages

Two static HTML pages for a session on safe browser automation.
The teaching point: when work lives in a portal with **no API, no export, no
connector**, a browser bot (e.g. Axiom) can still move _visible evidence_
between systems — but a human owns interpretation, decisions, accountability,
and the actual send.

| File                             | Role                                                                                          |
| -------------------------------- | --------------------------------------------------------------------------------------------- |
| `workshop-logistics-portal.html` | Read-only **source** page. Coordinator checks logistics at 9:30 AM before a 1:00 PM workshop. |
| `team-update-draft.html`         | Write **destination** page. Bot pastes copied evidence; a human reviews and sends manually.   |

Both are self-contained: inline CSS + a tiny inline `<script>`, no build step,
no external fonts/images/CDN/JS. They use **relative** links to each other, so
they work both locally and when deployed.

> Demo data only. No real client, supplier, staff, address, or contact details
> are used.

## Deploy to GitHub Pages

1. Create a **public** repo named `demo-logistics-portal` and push these files
   to the default branch (e.g. `main`) at the repo root.
2. In the repo: **Settings → Pages → Build and deployment → Deploy from a
   branch**, select `main` / `/ (root)`, save.
3. After a minute the site is live at:
   `https://<your-username>.github.io/demo-logistics-portal/`
   - Page 1: `.../demo-logistics-portal/workshop-logistics-portal.html`
   - Page 2: `.../demo-logistics-portal/team-update-draft.html`

Axiom works more naturally against an HTTPS page than a local `file://`, so use
the GitHub Pages URLs for the live demo. (Local opening still works for
development.)

## Local development

Open either `.html` file directly in a browser, or serve the folder:

```powershell
# from this folder
python -m http.server 8000
# then open http://localhost:8000/workshop-logistics-portal.html
```

## Automation handles

Every interactive element exposes three selection paths so Axiom can use
point-and-click capture, visible text, or a custom CSS selector:

- Visible text (matches the brief verbatim)
- a stable `id`
- a matching `data-automation` attribute

Useful selectors:

| Purpose                    | Selector                                      |
| -------------------------- | --------------------------------------------- |
| "Exceptions only" filter   | `#filter-exceptions` / text "Exceptions only" |
| Logistics table body       | `#logistics-rows`                             |
| Currently **visible** rows | `#logistics-rows tr:not([hidden])`            |
| Fallback plain-text copy   | `#visible-exception-summary`                  |
| Bot paste target (Page 2)  | `#copied-exception-rows`                      |
| Human's final message      | `#final-reviewed-message`                     |
| Review status dropdown     | `#review-status`                              |
| Cross-page link            | `#open-team-update-draft`                     |

Filtered-out rows get the real `hidden` attribute (plus `.is-hidden` and
`aria-hidden`), so `tr:not([hidden])` reliably returns only the visible rows.

## Suggested Axiom run

1. Open the Page 1 URL → click **"Exceptions only"**
   (by text or `#filter-exceptions`).
2. Extract the visible rows via `#logistics-rows tr:not([hidden])`
   (fallback: copy `#visible-exception-summary`).
3. Open the Page 2 URL (or click **"Open Team Update Draft"**) → enter the
   copied rows into `#copied-exception-rows`.
4. **Bot stops at the automation stop point.** A human then checks the source,
   assigns owners, writes `#final-reviewed-message`, sets review status to
   "Ready to send", and sends manually. The page never sends anything itself.
