# Nosework Run Board — Master Event / Trial / Run Rules

Last updated: 2026-11-13 (America/Denver) — v13.1 mobile hotfix verified
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
- **Parking Lot Steward:** Show a compact card for **each active run** on the selected trial day. Each card identifies the run and shows **(a) the complete on-deck list, up to its configured on-deck count, (b) the next unstaged dog not on deck (run number, dog name, handler), and (c) whether a prior parking-lot call is awaiting gate dispatch**. The next dog remains visible while the call is pending; the Call action becomes available after the next actual gate send advances the staging line. Inactive runs are omitted. Back-to-back pairs share aligned staging positions.
- Record parking-lot staging calls separately from gate dispatch; a staging call never counts as starting or completing a search. A new call must not be issued while a previous call is awaiting gate advancement. **When the Gate Steward actually sends a dog** (ordinary send or send out of order), every dog in staging moves forward and the first-staging call slot becomes available automatically; there is **no separate Mark first staging empty button**. A skip, move, or drop without sending does not release the slot. One gate send releases a shared slot for both partners of a back-to-back pair atomically. The next not-on-deck dog can then be called.
- Gate dispatch, skip and reorder actions are unavailable on the Parking Lot Steward screen. Parking Lot Steward cannot edit trial setup or the shared master running order; Gate Steward cannot change configuration or another gate's run from its focused screen.
- Administrator retains authority to manage run configuration and roster; My Dog remains view-only. For production, role/station permissions, assigned run boundaries, staging transitions, and paired-run actions must all be validated server-side and stored in the same MySQL-backed event/trial/run data model.
- A weather hold pauses new gate dispatches and first-staging actions, while preserving both queues and staging status. After hold is lifted, displays resume at their correct outstanding dog.

## 8. Administrator selection of active runs and optional back-to-back pairs (approved November 13, 2026)

- In **Admin → Overview** (updated placement; see Section 9), provide one clearly visible **Active runs & back-to-back setup** panel for the currently selected **trial day**. List every configured run by its saved run name with a selectable **Active** checkbox or equivalent. The Administrator decides exactly which runs are active; mere creation of a run does not guarantee activation.
- Provide a clear **Save active runs** action. Changes are scoped to this trial day and must be reflected in the Gate Steward, Parking Lot Steward, and My Dog/Competitor screens: only active runs are displayed for live operations. Deactivating a run preserves its configuration, staging and dispatch history for later reactivation; it is not a queue reset or deletion.
- Offer optional conveniences to **Select all configured** and **Select none**. A run whose trial type requires an element cannot be activated until the Administrator has selected its element and saved the run. Incomplete extra NW2/NW3 runs remain inactive.
- In the **same Overview panel**, let Administrator choose **First run** and **Second run** and explicitly create a **Back-to-Back** pair or unpair an existing pair. No pairing is automatic; an unpaired run remains independent. Only two eligible runs from the same trial day may form a pair, and a run can belong to at most one pair.
- Back-to-back partners must always have the **same active status**: selecting either partner to be active or inactive selects the other as well. When initially paired, show and confirm that the two runs adopt the logical OR of their prior active states (both active if either was active), while the second run adopts the first run's starting dog and aligned queue. Do not allow a pair to become active if a required element is unset. A pairing must be established before dispatch, or the affected queues must be reset beforehand.
- Display existing pairs by **saved run names**, with their first → second order and an **Unpair** control. The pairing does not merge the run names, elements or results. Both runs share synchronized staging and dispatch transitions as defined in §5.
- Only Administrator may change active-run selections or pairing configuration. Gate Steward works one selected active run; Parking Lot Steward displays the on-deck list and the next not-on-deck dog for each active run, and may call that dog once first staging is available following a gate send; My Dog/Competitor is read-only. All of these rules require backend authorization and atomic queue updates in production.

## 9. Administrator Overview activation and safeguarded event deletion (approved November 13, 2026)

- Move the **Active Runs & Back-to-Back** controls to the top of the **Administrator Overview** for the currently selected trial day. They are no longer buried in the Runs configuration screen. The Runs screen still links directly to Overview and retains run naming, elements, starting dog and on-deck configuration.
- The Overview lists every configured run, its saved name and current activation status. Administrator can select runs individually, select all eligible runs or select none, then use **Save active runs**. Incomplete runs requiring an element cannot be activated until configured.
- When two runs are paired back-to-back, **present a single activation checkbox for the pair**. Activating or deactivating that checkbox always updates **both underlying runs atomically**. Never permit one paired run to be active while its partner is inactive. The pair remains two separately named runs with synchronized staging and dispatch progression.
- Pairing controls remain on Overview: choose Run A and Run B, explicitly pair or unpair, and display existing pairs. Pairing is optional, never automatic, and allowed only within one trial day before dispatch. If either run was active when they are paired, both become active; if both were inactive, both remain inactive. Reject activation if a required element is unset. A pair adopts the first run's starting dog and aligned queue.
- Administrator can delete a selected Event from **Admin → Event & trials**. This must require **two explicit confirmation steps**, not a single accidental click: first request deletion and type the exact Event name into a warning panel, then accept a separate final confirmation dialog. Cancel or mismatch causes no deletion.
- Successful Event deletion cascades to **only that Event's trials and their associated trial days, runs, paired-run configuration, one master running order per trial, staging, dispatch and activity history**. Other events and their trials must remain unchanged. Once the last Event is deleted, show an empty project screen from which Administrator can create a new event/trial without a crash. In the offline demo deletion cannot be undone except through a previously exported backup.
- The production PHP/MySQL backend must implement role-based Administrator-only authorization, transactionally enforced cascading deletes and paired-run activation, validation of the typed confirmation in the UI (not as sole security), and backups/audit policy before destructive operations go live.

## 10. Deployment/project constraints

- Standalone app, separate from WordPress. Local demo may run from a single HTML file offline. GitHub and Bluehost deployments are distinct actions and are not implied by saving this master file.
- Preserve the one shared master roster/order per Event + Trial, independent per-run/per-trial-day execution states, and event grouping when migrating the data model.
- **Binding budget requirement: $0 in additional recurring charges.** Use only the user's existing Bluehost WordPress Choice Plus hosting plan and free GitHub capabilities; do not introduce a paid backend, upgraded hosting tier, separately billed database, paid real-time platform, or paid plugin without explicit prior approval.
- **Production hosting approach:** host the standalone Nosework Run Board frontend (HTML/CSS/JavaScript) on Bluehost outside WordPress; use PHP HTTPS endpoints and a MySQL database on the same existing hosting account, subject to the account's actual PHP/MySQL access and resource limits. Existing hosting charges continue, but this architecture must not add a new subscription.
- **Live board updates:** because shared hosting should not be assumed to support persistent WebSocket processes or reliable long-lived SSE connections, use efficient version-based short polling over HTTPS. Start with about 10 seconds between checks on active viewer screens for a design target of approximately 100 concurrent competitor devices. Return unchanged/no-body responses when no updates occurred, transfer only deltas, apply staggered intervals and backoff for background or idle tabs, and load-test against the actual hosting account before live trial use. This is near-real-time refresh, not true server-initiated push.
- **Safety and reliability:** validate authorization on the PHP server for every write; My Dog/Competitor access remains read-only. Use transactions for dispatch/skip updates, authenticated staff sessions, rate limiting, and database backups within available hosting capabilities.
- **No-cost scope:** outbound SMS is not enabled in this phase because carrier or third-party messaging typically has usage costs. Do not enroll in paid service trials or upgrades automatically. If tests show hosting capacity is insufficient, optimize/poll less often or report the limitation; obtain approval before any spending.

## 11. Outstanding details not yet approved

- Any additional non-element configuration fields or naming conventions for standard Elite runs beyond the agreed initial count of 3 and ability to add more.
- The specific run count for each day of a Summit trial is not fixed; Administrator configures counts separately for Day 1 and Day 2. There are no Summit element types.
- Whether specific run templates impose maximum run counts; do not infer a hard limit from the initial auto-created run count.


## 12. Parking-lot call timing and run completion (updated November 13, 2026)

- **Parking Lot Steward:** For each active run, display the full ordered **on-deck group** (first `onDeckCount` dogs in the remaining gate queue) and the **next eligible dog who is not on deck**. Identify each dog by run number, name and handler.
- **Keep the next dog visible even while an earlier staging call is pending**, but block a second call until the gate sends a dog. A parking-lot call reserves one first-staging call slot for that run; the gate's **Send next** or **Send out of order** action advances the line and **automatically frees the slot**. No separate stage-empty confirmation is needed. Skips, moves and drops do not free it. A new call still requires an active run, no weather hold, an incomplete run order, and an eligible unstaged dog. A parking-lot call does not mark the dog as searched or finished.
- **Back-to-back pair:** When paired runs have different on-deck sizes, select the next staging candidate **after the larger on-deck group**, so the called dog is not on deck in either run. Both run records share one first-stage call slot and one staging call. A gate send must release the slot for both partners atomically. The Parking Lot display may show both run cards, but must not enable duplicate calls for an occupied shared location.
- **Run order complete:** As soon as the remaining dispatch queue has **no dogs awaiting gate dispatch**, show **Run order complete** to both gate and parking-lot stewards; stop further dispatch/staging calls. This means the running order has been processed, **not** that the final dog has finished its search.
- **Admin final status:** After the last dog has actually completed the run, Administrator uses a clearly labeled **Mark Done** control on Overview to set its final status to **Done**. This action requires explicit Administrator confirmation and may not occur before the run order completes. A back-to-back pair is confirmed and marked Done together because both searches are run consecutively; their names, elements and records remain separate. Stewards then see **Done**. Provide an Admin-only Reopen option for corrections. Preserve status through backup/restore.
- **Permissions:** Neither Gate nor Parking Lot Steward can mark a run Done. My Dog remains view-only. In the production PHP/MySQL backend, gate-send-driven first-stage release, staging calls, paired completion changes, and Administrator finalization must be transactionally consistent and permission-checked.

## 13. Visual status colors and compact mobile header (approved, pending implementation)

**Decision recorded November 13, 2026. The Administrator subsequently authorized the v13 fixes.** The approved changes below are implemented in **v13 local demo**:

- Preserve the existing **navy-and-teal** application identity, white cards, subtle gray background, and existing outdoor/high-contrast mode. Do not replace the overall design palette.
- Make live operational statuses quickly distinguishable across Gate Steward, Parking Lot Steward, My Dog, and Admin screens, using these color associations:
  - **Active run:** green.
  - **Next dog / Send action:** blue (keep primary controls visually distinct from informational status).
  - **On deck:** amber.
  - **Weather or operational hold:** red.
  - **Run order complete:** neutral gray; distinguish from final Done status.
  - **Done, confirmed by Admin:** dark green.
- Use explicit **text labels** and, when helpful, icons as well as colors: no status may depend on color alone. Maintain readable contrast in standard and outdoor modes, including outdoors in sunlight.
- Increase the legibility of small operating labels, especially the Parking Lot Steward's dog details, next-call status, and complete on-deck list, without crowding the mobile view.
- Make the **mobile header substantially shorter**: tighten top/bottom spacing, shrink logo/mark and heading, hide or condense nonessential subtitle and decorative badges on narrow screens. Preserve access to navigation, app identity, mode/status cues, and important controls; do not shrink tap targets below comfortable phone sizes. The desktop header may retain its existing proportions.
- Verify both narrow-phone and desktop layouts, outdoor mode, color semantics, contrast, and no horizontal overflow before saving a new demo release.

**Implementation:** The v13 local demo implements these mobile and visual changes. This approval does not authorize a GitHub push or Bluehost deployment. v12 remains available as the prior baseline.


## 14. v13 local release: critical flow corrections (November 13, 2026)

**Authorized by Administrator:** fix the documented v12 audit findings, retain project/master history, and save the updated local demo. The deployment boundary remains unchanged; do not infer authorization to publish to GitHub or Bluehost.

- **Competitor status:** For each run, position 0 is `NEXT TO SEND`. Positive queue positions `n` are `ON DECK` only when `n < onDeckCount`. Otherwise display Waiting. On-deck count includes the next dog. Never assume three dogs are on deck; supports configured counts 1–10.
- **Dispatched versus Done:** A gate send is `Sent to search`, not completed. The dispatch queue becoming empty is `Run order complete`; the run is `Done` only after Administrator confirmation. Distinguish each state visually and textually.
- **Full day reset:** Reset queue, sent and dropped lists, staging history, first-stage occupant, per-run Done flags, logs, weather hold and simulated notifications for the selected trial day. Preserve the one shared trial roster, configured runs, starting dogs, on-deck counts, active selections, custom names and back-to-back pairs. Reset both paired execution queues consistently while keeping the pair configuration.
- **Stable dog ID:** Run number is display/order data, not the identity key. A competitor's permanent internal ID must not change when Admin edits its run number. Queue, sent/dropped, staging, selected dog, and subscription references remain linked by stable ID. CSV import reconciles existing competitors by dog and handler first; uniquely matched dog names second; new competitors receive new IDs. Preserve compatibility with older numeric IDs in backups.
- **Compact mobile:** Shortened header, trial/day summary with expandable selectors, controls near top, larger staging details, and no horizontal overflow on 320, 360, 390, 768 or 1280 pixel screens. The earlier unclosed trial selector wrapper was repaired. Desktop retains readable layout; outdoor mode remains available.
- **Color + version:** Active green, next/send blue, on-deck amber, hold red, run order complete gray, confirmed Done dark green. Use labels alongside colors. Display/version/backup file names identify v13.
- **Demo boundaries:** Offline single-file HTML with in-memory state and JSON export/restore; not production authorization, multi-device synchronization, actual SMS or PHP/MySQL backend. No extra recurring spending.

**Verification evidence:** Chromium regression tests covered on-deck counts 1/2, sent vs Done status, staging call/renumber, full day reset, paired staging and dispatch, paired reset, mobile/desktop layout, outdoor mode, and CSV/individual roster edits. This is prototype-level validation, not live-trial certification.


## 15. v13.1 mobile navigation hotfix (November 13, 2026)

- v13.1 is the **latest tested offline demo**. v12 and v13 are retained for history; v13 has a confirmed mobile HTML nesting issue.
- The trial/date/theme chooser must be a properly closed sibling of the main app navigation and station content, never a parent of them. When collapsed on narrow phones, the chooser alone is hidden; **My Dog, Steward, Admin, main navigation, and operational station controls must remain visible and interactive**.
- Parking Lot Steward's “Call to first staging” action uses a distinct blue action style while the active run status retains green. Explicit labels remain mandatory.
- Version title/header and backup filename use 13.1. Approved staging, paired-run, permissions, reset and completion rules from §§1–14 are unchanged.
- Regression checks cover 320/360/390/768/1280 px across three roles, both steward stations, five Admin sections, operational calls/sends, weather hold, reset, and zero horizontal overflow.
- Release is local only. No automatic permission to push to GitHub or deploy to Bluehost; the PHP/MySQL backend still requires separate work.
