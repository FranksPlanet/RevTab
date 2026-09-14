# Feature Specification: Pump to Desk Redesign

**Feature Directory**: `specs/001-pump-to-desk-redesign`
**Created**: 2026-09-14
**Status**: Draft. Open questions resolved 2026-09-14.
**Input**: Reshape RevTab around two moments of use. At the petrol station the
driver sits back in the car with a receipt and a phone, takes one photograph of
the receipt with the odometer visible in the same frame, and the app works out
the rest. Later, at a desk with a real screen, the same person reviews graphs
and detail. Plus a way to see what a whole trip cost.

## Why this exists

RevTab is used in two completely different situations that today share one
screen design.

At the pump the driver has one hand, poor light, a queue behind them and about
fifteen seconds of patience. At a desk they have time, a keyboard and space for
charts. A layout that merely shrinks to fit a phone serves neither.

## User Scenarios & Testing

### User Story 1 - One photograph logs the fill-up (Priority: P1)

The driver returns to the car after paying, sits down, and places the receipt
so the dashboard odometer is visible behind or beside it. They open RevTab, tap
one obvious button, and photograph both together. The app reads the fuel amount,
the price, the date and the odometer reading from that single image, shows what
it understood, and saves once the driver agrees. They drive away.

**Why this priority**: This is the moment the product lives or dies. If logging
a fill-up is slower than remembering to do it later, people stop logging, and an
app with no data answers no questions. It also ships alone: it changes nothing
about how anything is displayed elsewhere.

**Independent Test**: Log a real fill-up at a real petrol station using only one
photograph, from opening the app to the entry being saved, and confirm the fuel
quantity, price, date and odometer are all correct without typing any of them.

**Acceptance Scenarios**:

1. Given the driver is signed in with one car, when they tap the log button,
   then the camera opens directly, with no menu or form in between.
2. Given a photograph containing a fuel receipt and a legible odometer, when the
   scan completes, then the fuel quantity, total price, date and odometer
   reading are all filled in and shown for confirmation before anything is saved.
3. Given the shown values are correct, when the driver confirms, then the expense
   is saved against the right car and the driver is returned to a screen showing
   their updated cost per kilometre.
4. Given the odometer could not be read from the image, when the scan completes,
   then the app says so plainly and asks for that one number, rather than failing
   the whole capture or silently saving without it.
5. Given any value was guessed rather than read with confidence, when it is shown
   for confirmation, then it is visually marked as a guess, consistent with the
   app's existing treatment of estimates.
6. Given the driver has more than one car, when they open the log flow, then the
   car is either already correct or chosen in a single tap.

### User Story 2 - One receipt, several things bought (Priority: P2)

Petrol station receipts are rarely just fuel. The same slip can carry a coffee,
an ice cream, and a bottle of screenwash. The app separates those into their own
entries and guesses a sensible category for each, so the driver corrects rather
than types.

**Why this priority**: It significantly improves accuracy of the cost split,
which is the app's second promise after cost per kilometre. But P1 is still
useful without it: a mixed receipt logged as one fuel entry is wrong in its
categories while still being right about the money and the distance.

**Independent Test**: Photograph a receipt containing fuel plus at least two
non-fuel items, and confirm each item appears as its own entry with a plausible
category, and that the totals still add up to the receipt total.

**Acceptance Scenarios**:

1. Given a receipt with fuel and other items, when the scan completes, then each
   item is listed separately with its own amount and a suggested category.
2. Given the suggested categories, when the driver reviews them, then every
   suggestion is marked as a guess and any one of them can be changed without
   re-scanning.
3. Given the individual amounts, when they are summed, then they equal the
   receipt total, and if they do not, the driver is told before saving.
4. Given the driver does not care about the detail right now, when they choose to
   save it as a single entry instead, then that remains possible in one tap.

### User Story 3 - Reviewing at a desk (Priority: P3)

At a computer, the same person explores rather than logs. Charts, per-category
breakdowns, consumption over time and projections get the room they need,
arranged for a large screen rather than a stretched phone layout.

**Why this priority**: It is the part that creates understanding rather than
data, but it depends on there being data worth exploring, which P1 provides.
Nobody is blocked by the current screens; they are merely cramped.

**Independent Test**: Open the app on a desktop browser and answer, without
scrolling past three screens, what the car cost per kilometre this year, which
category grew fastest, and whether fuel consumption is getting worse.

**Acceptance Scenarios**:

1. Given a desktop-width screen, when the review screens load, then the layout
   uses the available width rather than presenting a narrow phone column.
2. Given the driver is on a phone, when they open the same screens, then they
   remain usable, because a phone is still where most people will look.
3. Given estimated or backfilled figures appear alongside logged ones, when they
   are displayed, then the difference remains visually obvious.

### User Story 4 - What will this trip cost? (Priority: P4)

The driver is thinking about a journey they have not taken yet. They are
planning to drive to Croatia, they know it is roughly 850 km each way, and they
want a number before they decide. They enter the distance and the app multiplies
it by what their car actually costs per kilometre, using their own logged data
rather than a generic figure.

**Why this priority**: It is the cheapest thing in this document and needs no new
stored data, because it is arithmetic over a number the app already knows. It is
also the first feature that helps before the driving rather than after it, which
is a different kind of usefulness to everything else RevTab does.

**Independent Test**: Enter a distance, see a cost, and check it by hand against
the cost per kilometre shown on the dashboard.

**Acceptance Scenarios**:

1. Given a distance in kilometres, when the driver asks for an estimate, then the
   app shows the expected cost, calculated from that car's own cost per kilometre.
2. Given the app already works out three different costs per kilometre (running
   costs only, including depreciation, and including the full purchase price),
   when an estimate is shown, then it is clear which of the three it used, and
   the driver can switch between them.
3. Given the estimate is a projection rather than a record, when it is displayed,
   then it is marked as an estimate in the same way every other projected figure
   in the app is.
4. Given the driver has more than one car, when they ask for an estimate, then
   they can see what the journey would cost in each.
5. Given a return journey, when the driver indicates as much, then the distance
   used is doubled without them having to do the arithmetic.

### Edge Cases

- The photograph is too dark, blurred, or the receipt is folded. The driver is
  told what went wrong and can retake without losing their place.
- The odometer in the frame reads lower than the last logged reading. The app
  questions it rather than accepting a reading that would corrupt the distance
  calculation.
- The receipt is in a foreign language or currency, taken abroad on the kind of
  trip User Story 4 is about.
- There is no mobile signal on the forecourt, which is common in rural stations
  and underground car parks. See the open question below.
- The driver photographs the same receipt twice.
- The receipt shows a fuel type or unit the car is not set up for, for example
  charging in kWh on a car recorded as diesel.

## Requirements

### Functional Requirements

- **FR-001**: The app MUST offer a single, obvious action that goes straight to
  the camera, reachable in one tap from opening the app.
- **FR-002**: The app MUST extract fuel quantity, total price, date and odometer
  reading from one photograph containing both a receipt and an odometer.
- **FR-003**: The app MUST show every extracted value for confirmation before
  saving anything.
- **FR-004**: The app MUST visually distinguish values it guessed from values the
  driver entered or confirmed, and MUST carry that distinction through to any
  figure calculated from them.
- **FR-005**: The app MUST allow any extracted value to be corrected without
  retaking the photograph.
- **FR-006**: The app MUST continue the capture when the odometer cannot be read,
  asking only for the missing value.
- **FR-007**: The app MUST reject or question an odometer reading lower than the
  most recent logged reading for that car.
- **FR-008**: The app MUST separate a receipt containing several purchases into
  individual entries with a suggested category for each. (P2)
- **FR-009**: The app MUST warn when separated amounts do not sum to the receipt
  total. (P2)
- **FR-010**: The app MUST let the driver save a mixed receipt as one entry
  instead of splitting it. (P2)
- **FR-011**: Review screens MUST use the full width of a desktop display while
  remaining usable on a phone. (P3)
- **FR-012**: The app MUST estimate the cost of a journey from a distance the
  driver enters, using that car's own cost per kilometre, and MUST make clear
  which cost basis was used and that the result is an estimate. (P4)
- **FR-013**: All figures introduced by this feature MUST be formatted through
  the app's single formatting layer, per Constitution Principle II.
- **FR-014**: After a successful log, the app MUST show the driver their updated
  cost per kilometre, since that is the reason they logged it.

### Non-Functional Requirements

- **NFR-001**: From opening the app to a saved fill-up SHOULD take under 30
  seconds including the scan, on a phone, one-handed.
- **NFR-002**: The capture flow MUST be operable one-handed.
- **NFR-003**: The scan MUST tell the driver it is working, and MUST NOT appear
  frozen while the image is processed.

## Resolved Questions

All three were answered by Frank on 2026-09-14.

**Does capture have to work without signal? No, not for now.** It would be
welcome later. So P1 assumes a working connection, and the driver must be told
clearly and immediately when there is none, rather than losing the photograph or
believing something was saved when it was not. Offline capture is recorded under
Future Work rather than being designed in now. Worth knowing the history: an
earlier attempt at offline support was abandoned and its code deleted, so this
would be built from nothing.

**Is a trip stored, or worked out? Neither.** A trip is not a record at all. It
is a calculator: distance in, estimated cost out, based on the car's real cost
per kilometre. Nothing new is stored, no journey is tracked, and the app never
needs to know whether the trip actually happened. This is dramatically smaller
than either option originally offered and is the reason P4 moved from
"what did that cost" to "what would that cost".

**Are receipt photographs kept? On the device only, not on the server.** This
avoids putting images of other people's payment slips, which can carry the last
digits of a card, into server storage. The honest limits are recorded under
Risks below, because device storage is not an archive.

## Assumptions

Recorded where a reasonable default was chosen rather than asked about.

- Extraction happens on the server, as receipt scanning does today, rather than
  on the phone.
- The driver confirms before anything is saved, in every case. This follows the
  app's existing promise of honesty about estimates and prevents a
  misread odometer silently corrupting the distance figures.
- A car with one fuel type continues to behave exactly as it does now. Nothing
  here changes the calculation engine, and Constitution Principle I holds: if any
  change to the maths does become necessary, the parity tripwire must still
  reproduce before it is considered safe.
- The existing Expenses screen remains for entries that are not made at a pump.
- Sign-in is unchanged.

## Risks

**Receipt photographs kept on the device are not a reliable archive.** Storage
inside a web app can be cleared by the browser when the phone runs short of
space, is lost if the app's data is cleared or the phone is replaced, and does
not appear on any other device. For checking a scan a few minutes or days later,
which is the actual need, this is fine. For finding a receipt from eight months
ago to settle a dispute, it is not. If that second need turns out to matter, this
decision has to be revisited rather than quietly relied upon.

## Future Work

Deliberately not in this feature, recorded so it is not lost.

- **Capture without a signal.** Photograph now, queue, upload when back in range.
  Wanted, not required yet.
- **Reading an odometer photographed on its own**, without a receipt.

## Success Criteria

- Frank logs a real fill-up at a real petrol station, using one photograph, and
  types nothing.
- A mixed receipt produces correctly separated entries that sum to the receipt
  total.
- The cost per kilometre shown after logging matches what the parity tripwire
  would produce for the same data.
- A month later, Frank has logged every fill-up, because doing so was not
  annoying enough to skip.

## Out of Scope

- Changing how any existing figure is calculated.
- Sign-in, account management, and billing.
- Reading a photograph of an odometer on its own, without a receipt.
- Automatic detection that a journey has begun, for example from location.
