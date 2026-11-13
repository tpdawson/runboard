Scent Work Run Manager v4 — Admin screen cleanup (2026-11-13)

Five focused Administrator sections:
- Overview: dispatch status, hold/resume, activity, event schedule, quick actions.
- Event & trials: event naming/creation, trial naming/creation, trial-day dates/labels/creation.
- Running order: visible table, show all, import CSV/TSV or paste, add team.
- Searches: independent search cards; update name, starting dog, on-deck size, active state, all with a single Save search button. Queue reset and remove are visibly separated.
- Backup & reset: download/restore JSON, day-only reset.

Mobile Admin navigation now displays all five sections, rather than hiding tabs offscreen. Existing My Dog/Steward/Admin separation remains. Skip defaults to 1 position, unchanged from v3.

Validated both 390px phone and 1200px desktop in a headless browser: no horizontal page overflow; all five sections; roster expansion; search edits; weather hold/resume; skip default; no JavaScript errors.

This is a self-contained local prototype. It has no real authentication, SMS, persistent server storage, or live synchronized queues. This version has not been uploaded to Bluehost.