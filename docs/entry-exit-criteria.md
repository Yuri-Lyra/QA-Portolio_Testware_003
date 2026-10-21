# Entry and Exit Criteria (Testware #003)

## Entry criteria: must be met BEFORE test execution starts

| ID | Criterion | Notes |
|---|---|---|
| E1 | Resource availability: QA allocated; Provider App DEV build installed on an iPhone (iOS only); Admin in the test environment reachable; the test environment reachable; 4 working days confirmed (2026-10-19 to 2026-10-22). | Record the build version per platform. |
| E2 | Access: at least 6 test provider accounts on DEV (Provider App login), an Admin user with access to the Vacation list, and a test customer for the booking flow. |  |
| E3 | Test data ready: providers P1 to P6 in their starting states (0, 1 and 3 notices, one ongoing, one legacy, one with bookings on chosen dates), the relative date table, and a way to see the automated messages (logs or manual trigger). | See 'Test data requirements' in the Test Plan. |
| E4 | Test material available: PRD and feature description reviewed, Figma accessible (if any), and the build with the 3-notices change deployed to DEV (Jira story or release note). |  |
| E5 | Critical ambiguities answered: limit counting (A1), overlap rule and adjacent dates (A2), booking block scope (A5), existing bookings (A6), platform (A12): answered 2026-10-19. Still open: date and time zone semantics (A3), booking statuses and message for rule 13 (A6b), Backend validation (A7), booking inside a future vacation (A14). | Without these answers, the expected results of the high-priority tests are unknown. |
| E6 | Initial quality level: smoke test passed. The Vacation Notice screen opens, one notice can be created and deleted, and it appears in (and leaves) the Admin Vacation list. |  |

## Exit criteria: must be met to consider the cycle COMPLETE

| ID | Criterion | Notes |
|---|---|---|
| X1 | 100% of high priority test conditions (risk score 15 or higher: R2, R3, R7, R10, R19) executed. |  |
| X2 | All rules of the 'Overlap' decision table validated against the 1st, 2nd and 3rd existing notice, and all rules of the 'Existing booking' decision table validated (rule 13), on iOS. |  |
| X3 | The state transitions of the limit (0 > 1 > 2 > 3 blocked > delete > 2 > 3) and of a notice (Future > Ongoing > Past) validated. |  |
| X4 | Booking block and message suppression validated while EACH of the 3 periods is ongoing, and provider listed again after each period ends. Result of A14 (booking inside a future vacation) recorded and agreed with the stakeholders. |  |
| X5 | Zero open defects of Critical or High severity related to the limit, the overlap rule, the existing-booking rule, deletion, the booking block or the Admin data. |  |
| X6 | Defect density within the level agreed with the stakeholders. |  |
| X7 | All planned tests executed, deferred with a recorded reason, or blocked with a documented cause. |  |
| X8 | Regression of the single-vacation flow and of notices created before the change executed with no new failures. |  |
| X9 | Test Completion Report written and reviewed by the stakeholders (Engineering, Backend and Frontend leads). |  |

Entry criteria follow CTFL 4.0 section 5.1.3: resources available, test material available, and an initial quality level (smoke test). Exit criteria combine measures of thoroughness (coverage, open defects, defect density) and completion criteria (planned tests executed, report reviewed).
