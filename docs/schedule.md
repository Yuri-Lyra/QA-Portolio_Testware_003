# Test Schedule (Testware #003)

Total planned time: 4 working days (2026-10-19 to 2026-10-22).

| Step | Phase (ISO/IEC/IEEE 29119-2) | Activity | Start | End | Working days |
|---|---|---|---|---|---|
| 1 | Test Planning | Draft the Test Plan, Schedule, Risk Register and Entry/Exit Criteria. Send the open assumptions (A1b to A14) to the stakeholders. | 2026-10-19 | 2026-10-19 | 1 |
| 2 | Test Analysis | Derive test conditions from the PRD and the feature description. Build the 'Overlap' and 'Existing booking' decision tables, the state transition models (number of notices; Future > Ongoing > Past) and the boundary dates. Prioritize by risk, coverage and requirement. | 2026-10-19 | 2026-10-20 | 2 |
| 3 | Test Design | Write test cases, charters, coverage items, test data requirements and environment requirements. | 2026-10-20 | 2026-10-20 | 1 |
| 4 | Test Implementation | Build test sets and procedures. Create the test providers in their starting states (P1 to P6), the bookings for rule 13 and the date table. Create the first Playwright checks (Admin Vacation list, booking flow). Run the entry-criteria smoke test (create, list and delete one notice; see it in the Admin). | 2026-10-20 | 2026-10-21 | 2 |
| 5 | Test Execution | Run the high priority sets: limit of 3 and state transitions, 'Overlap' decision table against the 1st, 2nd and 3rd notice, 'Existing booking' decision table (rule 13), deletion (first, middle, last, ongoing), booking block while each of the 3 periods is ongoing, booking inside a future vacation (A14). iOS. Log defects. | 2026-10-21 | 2026-10-21 | 1 |
| 6 | Test Execution (cont.) | Run the medium and low priority sets: lists and tags, time transitions, automated messages, Admin list and palm tree icon, robustness (double tap, two devices), regression of the single-vacation flow and legacy notices, charters. Retest fixed defects. | 2026-10-22 | 2026-10-22 | 1 |
| 7 | Test Completion | Consolidate metrics, write the Test Completion Report, lessons learned and improvement actions. Validate the exit criteria. | 2026-10-22 | 2026-10-22 | 1 |
