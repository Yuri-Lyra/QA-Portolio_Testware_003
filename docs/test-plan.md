# Test Plan (Testware #003)

Feature: Vacation Notice - allow multiple vacations submission.  
Update of the existing 'Vacation Notice' feature: a provider can now keep up to 3 vacation periods instead of 1.  
Sources: PRD 'Provider vacations' and the internal feature description.

Status legend: **Confirmed** = agreed; **To confirm** = waiting for an answer from the team; **Pendent** = not reviewed yet.

## 1. CONTEXT OF TESTING

### Scope

> WHAT is tested and what is NOT. Writing the 'out of scope' list avoids discussions later about tests nobody planned (CTFL 5.1.1 / ISO 29119-3 test plan: context of testing).

Vacation Notice feature after the change from 1 to up to 3 vacation periods per provider, on 3 surfaces:  
(a) Provider App on iOS: More > Vacation Notice (DEV build);  
(b) Admin: Vacation list;  
(c) Effects of a vacation on other parts of the system: the booking flow schedule step (test environment) and the automated system messages.  
  
In scope:  
(a) creating 1, 2 and 3 vacation notices; the limit of 3 (Add button disabled and limit message); the slot freed after a delete;  
(b) date validation: Start Date and End Date required; Submit enabled only with both dates; End before Start; Start equal to End; overlap with an existing notice, including periods that touch on the same day (error 'You already have a vacation notice on this period'); a period that contains an existing booking of the provider (not allowed: the vacation must be before or after the booking);  
(c) lists: Vacation Notice screen and All Vacations screen, order, 'Ongoing' and 'Past' tags, trash icon, '<' navigation;  
(d) deletion: modal 'Delete vacation notice?', Cancel and Delete, removal from both lists, removal of the 'Ongoing' tag, Add button enabled again;  
(e) Admin Vacation list: one row per notice (up to 3 per provider), provider name, start date, end date, total days, name filter, pagination, read only (no create, edit or delete);  
(f) Admin palm tree icon: shown only while a vacation is ongoing, on every Admin page that shows the provider;  
(g) effects of EACH of the 3 periods (not only the first one) while it is ongoing: provider not listed in the booking flow schedule step, automated messages suppressed (mandatory open slots alert, 'availability reminder'), Provider App access unrestricted; provider listed again when no vacation is ongoing;  
(h) regression: the single-vacation flow and providers who already had a vacation notice before the change;  
  
Out of scope:  
(a) editing a vacation notice (the feature does not offer editing);  
(b) load and performance testing;  
(c) the content of the automated messages and the rest of the booking flow (only the provider being hidden or shown is tested here);  
(d) vacations for other roles (customers, admins);  
(e) the Provider App on Android (only iOS is used);

**Status:** Confirmed · **Notes:** Platform confirmed: iOS only.

### Test objectives

> WHY we test. Each objective must be checkable at the end of the cycle; the exit criteria are built from them.

Ensure that:  
(1) a provider can create up to 3 vacation notices and cannot create a 4th one (Add button disabled and limit message) until one is deleted;  
(2) invalid periods are rejected (missing date, End before Start, overlap with any existing notice, period containing an existing booking) and valid ones are accepted, including past dates;  
(3) both lists (Vacation Notice and All Vacations) show every notice correctly: dates, order, 'Ongoing' and 'Past' tags, trash icon;  
(4) deleting any notice (first, middle, last, ongoing) removes exactly that notice everywhere (app, Admin list, palm tree icon, booking block) and frees one slot;  
(5) the Admin Vacation list shows each notice with the correct provider, dates and total days, the name filter works, and no write action is possible;  
(6) while each of the 3 periods is ongoing the provider cannot be booked and receives no automated messages, and becomes available again when it ends;  
(7) the single-vacation behavior and the notices created before the change still work (no regression);

**Status:** Confirmed

### Constraints

> Limits that shape HOW we can test (time, tools, data, environment).

- 4 working days: 2026-10-19 to 2026-10-22  
- Manual execution on the Provider App (native mobile app, iOS only); simple Playwright checks only on web pages (Admin Vacation list, booking flow), because Playwright tests web pages, not native apps  
- Fictional test providers and test customers only  
- Several results depend on today's date ('Ongoing', 'Past', palm tree icon, booking block): tests need notices that started today, ended yesterday and start tomorrow  
- Automated messages run on schedules (monthly alert, availability reminder) and may not be triggered on demand: Backend help (logs or manual trigger) is needed  
- Vacation notices can only be created or deleted through the Provider App (the Admin is read only)

**Status:** Confirmed · **Notes:** Deadline confirmed (2026-10-19).

### Test basis

> The documents the tests are derived from. If a rule is not in the test basis, it becomes an assumption to confirm.

- PRD 'Provider vacations - allow multiple vacations submission' (one line: up to 3 different time periods)  
- Feature description internal feature description (Provider App, Admin, related actions, screenshots)  
- Figma design: to be added  
- Jira story: to be added

**Status:** Confirmed · **Notes:** The PRD is a single line: most rules come from the feature description, so the assumptions below matter.

## 2. ASSUMPTIONS AND PROJECT CONSTRAINTS

### Confirmed rules

> Rules already stated by the team. Expected results of the test cases come from here.

(1) Up to 3 vacation notices per provider (previously 1).  
(2) Start Date and End Date are required (date pickers). Submit is enabled only after both dates are selected. A confirmation screen 'Vacation Notice Submitted' with a Done button closes the flow; '<' goes back.  
(3) A notice inside the period of another notice is rejected with the error 'You already have a vacation notice on this period'; OK closes the modal.  
(4) Notices with past dates are allowed.  
(5) With 3 notices, 'Add Vacation Notice' is disabled and the message 'You can have up to 3 vacation notices. Delete one to add another.' is shown near the bottom of the screen.  
(6) Delete: trash icon > modal 'Delete vacation notice?' showing the period, with Cancel and Delete. After Delete the notice leaves the list and 'Add Vacation Notice' is enabled again. If the deleted notice was the ongoing one, the 'Ongoing' tag disappears.  
(7) Vacation Notice screen: notices with the 'Ongoing' tag (current period) and a trash icon; 'No vacation notices yet' when empty. All Vacations: past notices with the 'Past' tag, plus the current and future ones with their tag and trash icon.  
(8) Admin Vacation list: provider name, start date, end date, total days; filter by provider name; no create, edit or delete. A palm tree icon appears on the provider profile on every Admin page while a vacation is ongoing.  
(9) During an ongoing vacation the provider is not listed in the booking flow schedule step and receives no automated messages (for example the mandatory open slots alert and 'availability reminder'). Provider App access is not restricted.  
(10) [A1] Notices with the 'Past' tag do NOT count toward the limit of 3.  
(11) [A2] Periods that touch on the same day (End of one = Start of the next) are NOT allowed; any overlap is rejected.  
(12) [A5] The provider is hidden from the booking flow schedule step only WHILE a vacation is ongoing (not in advance for future periods).  
(13) [A6] A vacation notice cannot be created if the provider already has a booking inside that period: the vacation must be before or after the booking.  
(14) [A12] Provider App platform in scope: iOS only.

**Status:** Confirmed · **Notes:** Rules 1 to 9: internal feature description. Rules 10 to 14: answers from the clarification session (2026-10-19).

### Assumptions to confirm

> Anything we BELIEVE but nobody confirmed. Each one becomes a question to the team; the answer changes the expected result of some test cases.

(A1b) A NEW notice created with past dates gets the 'Past' tag at once, so it does not count toward the limit of 3 either.  
(A2b) Consecutive periods with no shared day (End 10/25, next Start 10/26) are allowed.  
(A3) Dates are inclusive full days in the provider's time zone: the provider is on vacation on the Start Date AND on the End Date. Which time zone defines 'Ongoing' and the booking block (provider, clinic or UTC)?  
(A4) Admin 'Days' column: the screenshot shows Days = End - Start (10/19/2026 to 10/25/2026 = 6), one day less than an inclusive count (7). Which is correct?  
(A6b) Rule 13 (no vacation over an existing booking): which booking statuses block the vacation (confirmed, pending, cancelled, completed)? What message is shown? Does a booking on the Start Date or the End Date block it?  
(A7) The limit of 3, the overlap rule and the booking rule are enforced by the Backend (API), not only by the app screen (double tap on Submit, two devices, direct API call).  
(A8) Is there a maximum duration or a maximum future date for a vacation? (The Admin screenshot shows a 367-day notice.)  
(A9) Both lists are ordered by Start Date, oldest first.  
(A10) Past notices can be deleted from All Vacations with the same flow.  
(A11) Deleted notices disappear from the Admin Vacation list. The Admin screenshot shows overlapping rows for the same provider (10/19/2026-10/25/2026 and 10/19/2026-10/22/2026) and more than 3 rows: legacy test data, history of deleted notices, or a defect?  
(A13) End Date before Start Date: blocked by the date picker, or by an error after Submit?  
(A14) NEW, raised by rules 12 + 13: a provider with a FUTURE vacation is still listed in the booking flow (rule 12), so a customer can book a slot inside that future vacation. Rule 13 only blocks the opposite order (booking first, vacation later). Is this booking allowed? If not, which side blocks it?

**Status:** To confirm · **Notes:** A14 is a possible gap in the business rule (Risk R19, High). Send A3, A6b, A7 and A14 first: they block the entry criteria (E5).

## 3. STAKEHOLDERS

### QA Analyst

> Who is involved and why. Knowing who answers each kind of question saves time.

QA Analyst and tester (author of this plan): plans, designs, executes and reports the tests.

**Status:** Confirmed

### Engineering Lead

technical approval; clarifies business rules (open assumptions) and architecture.

**Status:** Confirmed · **Notes:** Same stakeholders as Testware #001 and #002.

### Backend Lead

vacation rules in the API (limit of 3, overlap, dates and time zone, total days), provider availability in the booking flow, booking conflict check (rule 13), scheduled messages (logs or manual trigger), test data.

**Status:** Confirmed

### Frontend Lead

Provider App screens and states (Vacation Notice, All Vacations, modals, disabled button), Admin Vacation list and palm tree icon, test selectors (data-test).

**Status:** Confirmed

## 4. RISK REGISTER (SUMMARY)

### Reference

> Risk-based testing: the higher the risk score, the earlier and deeper we test that area (CTFL 5.2).

See the 'Risk Register' tab for the full list with probability, impact, score and mitigation.

**Status:** Confirmed

### Highest-severity risks (summary)

R2: the overlap rule only catches periods 'contained' in another one; partial overlaps or overlaps with the 2nd/3rd notice are accepted.  
R3: the booking block was built for 1 vacation and only works for the first or nearest one; the provider stays bookable while the 2nd or 3rd period is ongoing.  
R7: deleting a notice removes the wrong one, or the 'Ongoing' tag, palm tree icon and booking block stay after the ongoing notice is deleted.  
R19: a customer can book a slot inside a FUTURE vacation (the provider is only hidden while a vacation is ongoing), creating the booking-vacation conflict that rule 13 forbids in the other order (A14).  
R10: the check 'no vacation over an existing booking' (rule 13) misses bookings (2nd/3rd period, Start/End day, booking status) or blocks valid periods.

**Status:** Confirmed

## 5. TEST APPROACH

### Test levels

> Level = how big the piece under test is. Here: the whole system and its integrations, not single functions of the code.

(1) System testing of the Provider App and the Admin in the test environment;  
(2) System integration testing of the vacation effects: booking flow schedule step and automated messages;  
(3) Regression (smoke) of the single-vacation flow and of notices created before the change.

**Status:** Confirmed

### Test types

> Type = which quality characteristic we check (function, data, time, usability, robustness).

(i) Functional (create, limit, validation, lists, tags, delete);  
(ii) Data consistency (Provider App vs Admin vs booking availability vs API);  
(iii) Time-based (Future > Ongoing > Past transitions, date and time zone boundaries);  
(iv) UI and usability (empty state, disabled button, limit message, modals, Figma comparison);  
(v) Light robustness (double tap on Submit, two devices at the same time, slow or lost connection during Submit);  
(vi) Exploratory (session charters).

**Status:** Confirmed

### Test techniques

> Techniques = methods to choose test cases with the best coverage using few tests (CTFL chapter 4).

(a) Equivalence partitioning and boundary value analysis: number of notices (0, 1, 2, 3, attempt of a 4th); dates (past, today, future; Start = End; End = Start - 1; year boundary 2026-12-31 / 2027-01-01; leap day 2028-02-29);  
(b) Decision tables: (i) 'Overlap': position of the new period against an existing one (before with a gap, consecutive before, touching on the same day, overlaps the start, inside, contains, identical, same start, same end, overlaps the end, touching after, consecutive after, after with a gap), applied against the 1st, 2nd and 3rd notice; (ii) 'Existing booking': booking date (before Start, on Start, inside, on End, after End) x booking status;  
(c) State transition testing: (i) number of notices 0 > 1 > 2 > 3 (Add disabled) > delete > 2 (Add enabled); (ii) life of a notice Future > Ongoing > Past;  
(d) Error guessing: double tap, two devices, midnight and time zone, deleting the ongoing notice, deleting the middle one of 3, lost connection;  
(e) Exploratory charters;  
(f) Simple Playwright checks on the Admin Vacation list (rows, filter, read only) and on the booking flow schedule step.

**Status:** Confirmed

### Test deliverables

> The work products this cycle will produce (ISO 29119-3).

(1) Test Plan; (2) Risk Register; (3) Entry and exit criteria; (4) Schedule; (5) Test conditions, test cases, charters and coverage items; (6) Test sets, procedures, test data and automation checks; (7) Execution records; (8) Defect reports; (9) Test completion report.

**Status:** Confirmed · **Notes:** This file covers deliverables 1 to 4.

### Entry and exit criteria

> Entry = what must be true to START executing. Exit = what must be true to say we are DONE.

See the 'Entry-Exit Criteria' tab.

**Status:** Confirmed

### Test independence

> An independent tester finds different defects than the author of the code.

Tests are run by QA, independent from the team that built the feature, which reduces confirmation bias.

**Status:** Confirmed

### Metrics to be collected

> The numbers that show progress and quality during and at the end of the cycle.

(1) Cases executed vs planned; (2) cases passed, failed and blocked; (3) defects by severity; (4) open vs closed defects; (5) coverage of high-priority conditions; (6) overlap decision table rules validated, per existing notice (1st, 2nd, 3rd); (7) 'Existing booking' decision table rules validated; (8) state transitions covered; (9) booking block checked per period (3 of 3); (10) automated checks passed vs total.

**Status:** Confirmed

### Test data requirements

> The data that must exist BEFORE the tests run. Missing data is a common reason for blocked tests.

(a) At least 6 fictional test providers with Provider App login on DEV, in these starting states: P1 no notice; P2 1 future notice; P3 3 notices (limit reached); P4 1 notice ongoing today plus past notices; P5 a notice created before the change (legacy); P6 bookings on chosen dates (rule 13);  
(b) A table of dates relative to the execution day: yesterday, today, tomorrow, +7, +30, year boundary (2026-12-31 / 2027-01-01), leap day (2028-02-29);  
(c) A test customer who can run the booking flow in the service area of the test providers;  
(d) Test providers with existing bookings on chosen dates (before Start, on Start, inside, on End, after End) and in different statuses, for rule 13; a customer able to book a future date inside an already created vacation (A14);  
(e) A way to see the automated messages (Backend logs or a manual trigger) for the mandatory open slots alert and availability reminder.

**Status:** Pendent · **Notes:** Past dates are allowed, so 'Past' and 'Ongoing' notices can be created directly in the app.

### Test environment requirements

> Tools, devices and environments needed. Record versions: a defect is only useful if it can be reproduced in the same configuration.

(1) Provider App DEV build installed on an iPhone (iOS only; version recorded for each run); (2) Admin in the test environment with access to the Vacation list; (3) Booking flow in the test environment (and the Customer App, if in scope); (4) API inspection (network tab or proxy) and Backend logs; (5) Playwright installed locally for the Admin and booking checks.

**Status:** Confirmed

### Deviations from policy or strategy

None identified. Testware #002 is the format baseline.

**Status:** Confirmed

## 6. BUDGET AND SCHEDULE

### Budget

Not applicable (internal effort, no extra tool cost).

**Status:** Confirmed

### Schedule

4 working days: 2026-10-19 to 2026-10-22. See the 'Schedule' tab.

**Status:** Confirmed
