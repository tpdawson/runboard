# Nosework Run Board — v9 (November 13, 2026)

## What changed from v8

- Administrator → Runs now has a single **Active runs & back-to-back setup** panel for the selected trial day.
- Admin can select exactly which configured runs are active using checkboxes, then **Save active runs**. Shortcuts: **Select all configured** and **Select none**.
- Runs missing a required element (e.g., extra NW2/NW3 runs) cannot be activated until the element is selected and saved.
- Optional **First run / Second run** pairing is in the same panel, with existing pairs and **Unpair** controls shown by saved run name.
- Paired runs must be active or inactive together. The demo synchronizes both checkboxes; pairing initializes the second run from the first run's active state and queue, subject to confirmation. Pairing is only allowed before dispatch.
- Inactive runs disappear from My Dog, Gate Steward, Parking Lot Steward and the live running board without resetting or deleting their queues and history.
- Master rules updated; all prior v8 trial templates, editable run names, shared trial roster, back-to-back dispatch/staging, and multi-day rules are retained.

## Validation

- Headless Chromium tests at 390px and 1200px: activate/deactivate independently; linked pair toggles; create pair; role-specific live views; no page overflow or JavaScript exceptions.
- NW2 additional run correctly remains inactive until its required element has been saved; JSON backup and restore preserve active-run status.

## Scope

This is an offline single-file UX prototype, **not** an authenticated multi-device application. Live PHP/MySQL backend, synchronization, and actual SMS are not implemented. GitHub and Bluehost were not updated by this version. Production deployment must stay within the agreed no-additional-cost constraints unless separately approved.