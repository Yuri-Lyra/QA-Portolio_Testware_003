# QA Portfolio: Provider Vacations (Testware #003)

An anonymized showcase of how I planned the tests for a change to a vacation feature in a provider mobile app of a health & aesthetics services marketplace. It follows ISO/IEC/IEEE 29119 and ISTQB CTFL 4.0. This cycle covers the **test planning** work products: test plan, schedule, risk register, and entry and exit criteria. No internal company data, names or links are included.

---

## What this is

Providers could register **one** vacation period. The change lets them keep **up to three**. While a vacation is ongoing, the provider is hidden from the customer booking flow and stops receiving automated messages. An admin back office lists every vacation.

A one-line requirement ("allow up to 3 periods") touches four surfaces: the provider app, the admin list, the booking flow and scheduled messages. Most rules had been written with a single vacation in mind, and the plan focuses on what breaks when there are three.

All company, product, person and environment names were removed. Any test data mentioned is fictional.

## Rules under test (summary)

| # | Rule |
|---|------|
| 1 | Up to **3** vacation notices per provider; with 3, the Add button is disabled and a limit message is shown |
| 2 | Start and End dates are required; past dates are allowed |
| 3 | A new period may **not overlap** an existing one, not even on a single shared day |
| 4 | A vacation may **not cover an existing booking** of the provider |
| 5 | Notices tagged **Past** do not count toward the limit |
| 6 | The provider is hidden from the booking flow **only while** a vacation is ongoing |
| 7 | Deleting a notice frees a slot and removes every effect of that notice |
| 8 | The admin list is read-only and shows each notice with its total days |

The full list (14 confirmed rules and 12 open assumptions) is in [`docs/test-plan.md`](docs/test-plan.md).

## Highlight: a gap found during planning

Rules 4 and 6 together leave a hole. A provider with a **future** vacation is still visible in the booking flow (rule 6), so a customer can book a slot **inside** that vacation. Rule 4 only blocks the opposite order (booking first, vacation later). The end state is the same conflict the business wanted to forbid.

This was logged as risk **R19 (High)** and raised to the team as an open question before any test was executed. It is a good example of static testing: reading two requirements side by side found a likely defect at no execution cost.

## Repository map

| Path | Content |
|------|---------|
| `docs/test-plan.md` | Scope (in and out), objectives, constraints, test basis, confirmed rules, open assumptions, stakeholders, approach, metrics, test data and environment |
| `docs/schedule.md` | 7 steps of the ISO/IEC/IEEE 29119-2 test process over 4 working days |
| `test-design/risk-register.md` | 19 product and project risks scored by probability x impact, with mitigation |
| `docs/entry-exit-criteria.md` | 6 entry criteria and 9 exit criteria |
| `docs/test-planning-workbook.xlsx` | The original planning workbook (4 tabs, with formulas, status lists and explanatory tooltips) |

## Test design techniques planned

- **Equivalence partitioning and boundary value analysis:** number of notices (0, 1, 2, 3, a 4th attempt); dates (Start = End, End = Start - 1, year boundary, leap day).
- **Decision tables:** "Overlap" (13 positions of a new period against an existing one, applied to the 1st, 2nd and 3rd notice) and "Existing booking" (booking date x booking status).
- **State transition testing:** 0 > 1 > 2 > 3 (blocked) > delete > 2; and the life of a notice, Future > Ongoing > Past.
- **Error guessing:** double tap on Submit, two devices at once, midnight and time zones, deleting the middle of three notices.
- **Exploratory charters** for the areas the rules do not fully describe.

## Risk-based priorities

19 risks: **5 High**, 10 Medium, 4 Low. The High ones drive the first execution day:

- **R2:** the overlap check only catches periods fully inside another one, or only checks the first notice.
- **R3:** the booking block works only for the first or nearest vacation, so the provider stays bookable during the 2nd or 3rd.
- **R7:** deleting a notice removes the wrong one, or leaves its effects behind.
- **R10:** the "no vacation over a booking" check misses bookings on the boundary days or in some statuses.
- **R19:** the booking-inside-a-future-vacation gap described above.

## Skills shown

Reading short requirements critically, turning ambiguity into explicit assumptions, risk-based prioritization, test planning and estimation, entry and exit criteria tied to measurable objectives, and clear stakeholder communication.

## Status

Planning is complete. Execution results are not included because the target application is private.
