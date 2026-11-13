# Nosework Run Board — Master Event / Trial / Run Rules

Last updated: 2026-11-13 (America/Denver) — explicit active-run selection and optional back-to-back setup
Status: approved functional specification. These rules take precedence over any older demo defaults. This file documents requirements; deployment and implementation may lag.

## 1. Ownership and navigation

- The hierarchy is **Event → Trial → Run**. Every run belongs to exactly one trial; every trial belongs to exactly one event.
- Administrator first sees a list of this user's/person's **Events**, with **Add Event** and event editing.
- Selecting an Event opens that event's **Trials**, with **Add Trial** and trial editing.
- Selecting a Trial opens that trial's **Runs**, with **Add Run** and run editing.
- Only Administrator may create, configure, rename, or remove events, trials, or runs.
- Multiple trials can belong to the same event, multiple distinct trials can take place on the same day, and a trial can span multiple days. Every unique **Event + Trial** pair owns **exactly one master run order** (one roster of dogs/handlers and their base sequence). All runs within that trial reference this same order; do not make a separate master run order for each run.
- For trials spanning days, the trial retains its one master run order, while runs and operational dispatch progress remain isolated by trial/day. Each run can have its own starting dog, circular queue position, on-deck count, skip/move decisions, dispatch status and completion progress. Changes to a run's temporary operating queue do not alter the shared master order or another run's progress. Do not mix day queues.
- **Summit is a Trial Type**, not an Event type. Administrator selects **Summit** from the Trial Type dropdown under an ordinary selected Event. One Summit trial spans **two days (Day 1 and Day 2)**. The two days belong to that same trial and share its **one master run order**; configured runs, starting positions, queues, and dispatch progress are managed separately for each day. The number of Summit runs is configurable by Administrator for each day. **Summit runs have no element types.**
- Trial creation includes a **Trial Type dropdown**, **Day/date**, and **AM / PM / All Day** session. Trial type initializes the runs under that trial (and associated trial-day).

## 2. One master run order per event/trial

- When an administrator creates a trial under an event, the system creates **one run-order roster for that trial**. It is the single authoritative list of competitors (run number, dog, handler) and their base order.
- All runs generated for that trial (for example, the four NW1 searches or six NW3 searches) use that **same trial run order**. They do not get independent copies that can drift out of sync.
- Administrator can import, add, edit, and reorder the trial's master roster in the **Running Order** Admin section. A roster change is reflected in the setup for every run under that trial, without erasing prior dispatch history.
- Each run has its own **execution state**: starting dog/position, on-deck count, remaining circular sequence, dogs already sent, skipped or moved dogs, and completed/dropped status. Steward can adjust execution order **within a run** but cannot edit the shared master roster, event, trial, or run settings.
- A second trial, even when held on the same day and at the same event, receives its own master run order. Do not share the roster implicitly across different trials.

## 3. Trial type dropdown and initial run templates

The official agreed types for the dropdown and their initial run configurations:

| Type | Runs initially created | Run configuration |
| --- | --- | --- |
| NW1 | 4 | Exterior, Interior, Vehicle, Container (in that order) |
| NW2 | 5 | First four as NW1; fifth element selectable by Admin |
| NW3 | 6 | First four as NW1; fifth and sixth elements selectable by Admin |
| L1E / L2E / L3E | 4 each | All four Exterior |
| L1I / L2I / L3I | 4 each | All four Interior |
| L1V / L2V / L3V | 4 each | All four Vehicle |
| L1C / L2C / L3C | 4 each | All four Container |
| Elite | 3 | No element types; Admin can add more runs |
| Elite S (Elite Select) | 4 | No element types |
| Elite P (Elite Premier) | 4 | No element types |
| ORT | 3 | All three runs are Container (odor/station labels may distinguish Birch, Anise, and Clove, but element is Container) |
| Summit | 2-day trial; Administrator-configurable runs for Day 1 and Day 2 | No element types; each day owns independent run/dispatch state, sharing the trial master run order |
| Custom | Administrator-configurable | No fixed run count approved |

- **Summit appears in the Trial Type dropdown.** When selected, trial creation must establish **two days for one trial**. Administrator configures runs separately for Day 1 and Day 2 and can choose the date for each day; the trial remains under its selected Event.
- Keep **all three Elite categories** (Elite, Elite S, Elite P) in the dropdown; do not merge or drop standard Elite.
- For NW2/NW3, extra run elements must be chosen (not silently fixed to an arbitrary element). **Elite, Elite S, Elite P, and Summit runs do not have element types.** Do not display or require an element selector for these trial types.
- When creating a trial, immediately populate that trial's Runs with its type-specific template. Administrator may add runs when the trial format permits and edit the run configurations.
- This hierarchy and run-template behavior apply to trial creation and configuration, not to steward operations.

## 4. Admin vs Steward vs My Dog

- **Administrator:** controls Event, Trial, and Run setup; manages the **one master run order per Event + Trial** (including competitor/roster entries); can configure each run's starting dog, on-deck count, activation and other setup; has higher-level event/trial controls and backup.
- **Steward:** **cannot modify event, trial, run configuration, or the shared master run order**. Steward operates only the temporary running/dispatch queue within already configured runs: dispatch, skip, move/reorder dogs, and handle out-of-order workflow as supported.
- **Skip defaults to 1 position** (not to the end); may be set to a larger number when needed and resets to 1 after a skip.
- **Competitor / My Dog:** **View-only access**. Competitors may select their own dog/run number as a viewing filter and see the event, trial, runs, current positions, on-deck/next-to-send status, and dispatch progress across active runs. They may **not add, edit, delete, reorder, dispatch, skip, move, configure, reset, or otherwise modify** events, trials, runs, the master run order, roster, or any operating queue. Selecting a dog changes only that viewer’s display and is not an edit to shared data. State reflects the actual remaining circular queue, including dispatches, skips, and moves made by authorized staff.
- **Access matrix (enforced by the production backend):**

  | Capability | Administrator | Steward | Competitor / My Dog |
  | --- | --- | --- | --- |
  | View event, trial, run and status | Yes | Yes | Yes |
  | Create/edit/delete event, trial, or run | Yes | No | No |
  | Manage trial master roster/run order | Yes | No | No |
  | Configure starting dog, on-deck count, or activation | Yes | No | No |
  | Dispatch, skip, move, or reorder a run's operating queue | Yes | Gate Steward only, one assigned run | No |
  | Choose a dog/run number to *view* status | Yes | Yes | Yes |

- The three top-level modes must stay clearly labeled **My Dog**, **Steward**, and **Admin** (Administrator), with setup only within Admin.
- Offline HTML demo navigation is not authentication. Real authorization, synchronization, and live SMS require a production backend.

## 5. Back-to-back paired runs (new approved rule)

- In Administrator → Trial → Runs, **any two eligible runs belonging to the same trial and trial day may be explicitly paired as Back-to-Back**. Pairing is optional; unpaired runs operate independently. No automatic pairing.
- Administrator selects the **two specific runs** and their order, **Run A → Run B**, using a Pair Back-to-Back control. Display the pairing clearly on the Admin, Steward and My Dog views; Administrator may unpair them.
- A dog runs **Run A immediately followed by Run B** before the paired unit advances to the next dog. The run pair uses the trial's single master running order and starts from the same dog. **One steward dispatch/advance action operates the two runs as one back-to-back unit**, while the two individual runs remain separately identifiable and separately recorded.
- When paired, a dog's queue placement, skip, move and next/on-deck positioning remain aligned across those two runs, avoiding contradictory queue positions. The pair does not change the trial's master roster or affect other runs.
- Keep each member's run identity, element (where applicable), and individual result/completion status distinct. Pairing is a dispatch coordination rule, not a merger of two run records or two trial types.
- A run may belong to **at most one active back-to-back pair**. Do not pair runs from different trials or different trial days. For multi-day Summit trials, pairs must be configured separately for each day.
- Only **Administrator** can create, change, or remove a pairing. **Steward** may dispatch, skip and move the paired execution queue but cannot edit the pair configuration. **My Dog** remains read-only and shows both paired runs and the dog's shared queue position.
- The production backend must validate pairing and synchronized queue operations atomically, including when two staff devices act simultaneously. The offline demonstration is a UX prototype, not live synchronization.

## 6. Administrator-editable run names (approved November 13, 2026)

- Every Run has its own required, editable **run name** field in Administrator → Event → Trial → Runs. It is independent of its underlying element, queue and run number. All displays (Admin, Gate Steward, Parking Lot Steward, My Dog) use the saved run name.
- When a trial's run has an **element**, its default name is exactly the element name if there is only one run of that element on the selected trial day: e.g., **Exterior**, **Interior**, **Vehicle**, **Container**.
- When **two or more runs share the same element** within the trial day, automatically number that element's default names in their sequence: **Exterior 1**, **Exterior 2**, etc. This rule applies to all trial types, including NW2/NW3 when an optional element is selected, specialty trials and ORT (thus ORT defaults to **Container 1**, **Container 2**, **Container 3**).
- When a trial type **has no elements** (Elite, Elite S, Elite P, Summit, or a non-element Custom run), default names are **Run 1**, **Run 2**, etc., numbered by their order within the trial day.
- Administrator can override any default by typing a custom run name and saving it. **Do not automatically overwrite a manually set name** when other runs are added, elements are assigned, or default numbering changes. Provide a means to restore the generated default name.
- Default names are per trial day, not across different trials or days. Editing a run name must not reset its dispatch, staging, or pairing state.

## 7. Separate Gate Steward and Parking Lot Steward displays (approved November 13, 2026)

- Steward mode provides **Gate Steward** and **Parking Lot Steward** station displays. These are different operational views, not new trial types or administrator setup menus.
- **Gate Steward:** A gate steward chooses their assigned active run; their operational screen shows **only that one run**, including its name, next dog, on-deck group, last sent and dispatch/skip/move actions. The screen must not show all the other runs' dispatch cards. When that run is part of a back-to-back pair, show the pairing label and coordinate the two underlying run queues together while the visible gate screen remains focused on the selected run.
- **Parking Lot Steward:** Show a compact card for **each active run** on the selected trial day. Each card identifies that run by saved name and displays the **next dog (run number, dog name, handler) that still needs to be sent to the first staging location**. Inactive runs are omitted. Back-to-back pairs are marked and should share aligned staging positions.
- Record first-staging progress separately from gate dispatch progress. A parking lot steward can mark the displayed dog as **sent to first staging**; this updates the staging state and reveals the next unstaged waiting dog for that run **without marking the dog sent through the search**. For paired runs, that one staging action updates both runs in the pair; never send duplicate staging calls for the same dog.
- Gate dispatch, skip and reorder actions are unavailable on the Parking Lot Steward screen. Parking Lot Steward cannot edit trial setup or the shared master running order; Gate Steward cannot change configuration or another gate's run from its focused screen.
- Administrator retains authority to manage run configuration and roster; My Dog remains view-only. For production, role/station permissions, assigned run boundaries, staging transitions, and paired-run actions must all be validated server-side and stored in the same MySQL-backed event/trial/run data model.
- A weather hold pauses new gate dispatches and first-staging actions, while preserving both queues and staging status. After hold is lifted, displays resume at their correct outstanding dog.

## 8. Administrator selection of active runs and optional back-to-back pairs (approved November 13, 2026)

- In **Admin → Event → Trial → Runs**, provide one clearly visible **Active runs & back-to-back setup** panel for the currently selected **trial day**. List every configured run by its saved run name with a selectable **Active** checkbox or equivalent. The Administrator decides exactly which runs are active; mere creation of a run does not guarantee activation.
- Provide a clear **Save active runs** action. Changes are scoped to this trial day and must be reflected in the Gate Steward, Parking Lot Steward, and My Dog/Competitor screens: only active runs are displayed for live operations. Deactivating a run preserves its configuration, staging and dispatch history for later reactivation; it is not a queue reset or deletion.
- Offer optional conveniences to **Select all configured** and **Select none**. A run whose trial type requires an element cannot be activated until the Administrator has selected its element and saved the run. Incomplete extra NW2/NW3 runs remain inactive.
- In the **same setup panel**, let Administrator choose **First run** and **Second run** and explicitly create a **Back-to-Back** pair or unpair an existing pair. No pairing is automatic; an unpaired run remains independent. Only two eligible runs from the same trial day may form a pair, and a run can belong to at most one pair.
- Back-to-back partners must always have the **same active status**: selecting either partner to be active or inactive selects the other as well. When initially paired, show and confirm that the second run adopts the first run's active state, starting dog and aligned queue. Do not allow a pair to become active if a required element is unset. A pairing must be established before dispatch, or the affected queues must be reset beforehand.
- Display existing pairs by **saved run names**, with their first → second order and an **Unpair** control. The pairing does not merge the run names, elements or results. Both runs share synchronized staging and dispatch transitions as defined in §5.
- Only Administrator may change active-run selections or pairing configuration. Gate Steward works one selected active run; Parking Lot Steward displays the next unstaged dog for each active run; My Dog/Competitor is read-only. All of these rules require backend authorization and atomic queue updates in production.

## 9. Deployment/project constraints

- Standalone app, separate from WordPress. Local demo may run from a single HTML file offline. GitHub and Bluehost deployments are distinct actions and are not implied by saving this master file.
- Preserve the one shared master roster/order per Event + Trial, independent per-run/per-trial-day execution states, and event grouping when migrating the data model.
- **Binding budget requirement: $0 in additional recurring charges.** Use only the user's existing Bluehost WordPress Choice Plus hosting plan and free GitHub capabilities; do not introduce a paid backend, upgraded hosting tier, separately billed database, paid real-time platform, or paid plugin without explicit prior approval.
- **Production hosting approach:** host the standalone Nosework Run Board frontend (HTML/CSS/JavaScript) on Bluehost outside WordPress; use PHP HTTPS endpoints and a MySQL database on the same existing hosting account, subject to the account's actual PHP/MySQL access and resource limits. Existing hosting charges continue, but this architecture must not add a new subscription.
- **Live board updates:** because shared hosting should not be assumed to support persistent WebSocket processes or reliable long-lived SSE connections, use efficient version-based short polling over HTTPS. Start with about 10 seconds between checks on active viewer screens for a design target of approximately 100 concurrent competitor devices. Return unchanged/no-body responses when no updates occurred, transfer only deltas, apply staggered intervals and backoff for background or idle tabs, and load-test against the actual hosting account before live trial use. This is near-real-time refresh, not true server-initiated push.
- **Safety and reliability:** validate authorization on the PHP server for every write; My Dog/Competitor access remains read-only. Use transactions for dispatch/skip updates, authenticated staff sessions, rate limiting, and database backups within available hosting capabilities.
- **No-cost scope:** outbound SMS is not enabled in this phase because carrier or third-party messaging typically has usage costs. Do not enroll in paid service trials or upgrades automatically. If tests show hosting capacity is insufficient, optimize/poll less often or report the limitation; obtain approval before any spending.

## 10. Outstanding details not yet approved

- Any additional non-element configuration fields or naming conventions for standard Elite runs beyond the agreed initial count of 3 and ability to add more.
- The specific run count for each day of a Summit trial is not fixed; Administrator configures counts separately for Day 1 and Day 2. There are no Summit element types.
- Whether specific run templates impose maximum run counts; do not infer a hard limit from the initial auto-created run count.