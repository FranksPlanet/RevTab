# Feature Specification: Capture at the pump, review at a desk

**Feature Directory**: `specs/001-pump-to-desk-redesign`
**Created**: 2026-09-14
**Rewritten**: 2026-11-11, after Frank described what he actually does at a
petrol station. The earlier version was designed around assumptions that did not
survive contact with real use. See "What changed and why" at the end.
**Status**: Draft

## Why this exists

RevTab is used in two situations that today share one screen design.

At the pump there is a queue behind you, a receipt in your hand, and no patience
at all. At a desk there is time, a keyboard, and room for charts. A layout that
shrinks to fit a phone serves neither.

The split is sharper than "phone and desktop". They are different activities.
One is capture. The other is understanding. Nothing about them should be the
same except the data underneath.

## What Frank actually does today

Recorded verbatim, because it is the source for everything below:

> Sitting down, taking the receipt, opening a shortcut button to adding an
> expense, taking 2 pictures, opening navigation, driving away.

He does not check anything at the pump. He captures and leaves. The checking
happens later, at a desk. RevTab is already his system of record for three cars,
including his father's Volvo P2 which has been sold but whose history is kept.

## User Scenarios & Testing

### User Story 1 - Capture at the pump (Priority: P1)

The driver gets back in the car, opens RevTab, taps one obvious button, and
photographs two things: the receipt, then the odometer. The app says it is saved.
They open navigation and drive away.

Nothing is scanned. Nothing is confirmed. No number is shown. The whole
interaction is three or four seconds and it never blocks on anything.

**Why this priority**: This is the moment the product lives or dies. If it is
slower than remembering to do it later, people stop logging, and an app with no
data answers no questions. Everything else here depends on this working.

**Independent Test**: At a real petrol station, from opening the app to the
capture being saved, in under ten seconds, with a queue behind you.

**Acceptance Scenarios**:

1. Given the driver opens the app, when they look at the screen, then a capture
   action is visible immediately without navigating anywhere.
2. Given they tap it, then the camera opens directly, with no form or menu first.
3. Given the camera, when they photograph the receipt, then they are prompted for
   the odometer as a second shot, and it is clear which is which.
4. Given both photographs, when the second is taken, then the capture is saved
   and the driver is released. No scan runs in front of them and nothing waits
   for a network.
5. Given there is no mobile signal, when they capture, then it still saves
   completely, because saving two photographs needs no connection.
6. Given the driver has more than one car, when they capture, then the car is
   either already right or chosen in a single tap.
7. Given a capture is saved, when the driver looks for reassurance, then the app
   confirms it plainly, without showing figures that have not been checked yet.

### User Story 2 - Review at a desk (Priority: P2)

Later, with time and a real screen, the driver opens a queue of captures that
have not been dealt with. Each one shows both photographs beside what the scanner
read from them. They confirm, correct, or type the values in themselves.

This is where the numbers change.

**Why this priority**: Capture without review produces photographs, not data.
These two together are the smallest thing that replaces the old habit of
emailing a photo to yourself and typing it into a spreadsheet.

**Independent Test**: Capture three fill-ups over a week without looking at the
app, then clear the queue in one sitting and confirm the resulting figures match
what the receipts say.

**Acceptance Scenarios**:

1. Given captures are waiting, when the driver opens the app anywhere, then the
   number waiting is visible without hunting for it.
2. Given a capture in the queue, when it is opened, then both photographs are
   shown beside the extracted values, large enough to read and able to be
   enlarged.
3. Given extracted values, when any was guessed rather than read confidently,
   then it is marked as a guess, consistent with how the app treats every other
   estimate.
4. Given a wrong value, when the driver corrects it, then no re-scan is needed.
5. Given the scanner could not read something, when the driver reviews it, then
   the app says which value is missing and asks only for that one.
6. Given an odometer reading lower than one already logged for that car, when it
   is reviewed, then the app questions it rather than accepting a reading that
   would corrupt every distance figure.
7. Given a capture is confirmed, when the driver returns to the dashboard, then
   the figures have updated to include it.
8. Given captures have not been reviewed, when the driver looks at any figure,
   then those captures are **not** counted in it. Numbers in RevTab are always
   confirmed numbers.

### User Story 3 - Noticing what is missing (Priority: P3)

The app tells the driver when its own picture of events looks incomplete.

**Why this priority**: Small, and it protects the trustworthiness of everything
else. A cost per kilometre built on half the fill-ups is wrong without looking
wrong.

**Independent Test**: Log two fill-ups 900 km apart on a car that realistically
cannot travel that far on one tank, and confirm the app raises it.

**Acceptance Scenarios**:

1. Given captures sitting unreviewed, when the driver opens the app, then a clear
   warning says how many are waiting, because unreviewed captures mean the
   figures on screen are behind reality.
2. Given a jump in odometer readings with no fuel logged in between that the car
   could not plausibly cover on one tank, when the driver looks at their history,
   then the app says it thinks a fill-up is missing, and where.
3. Given such a warning, when the driver knows it is fine, for example the car was
   lent out or filled by somebody else, then it can be dismissed and stays
   dismissed.

### User Story 4 - Where you fill up, and what it costs there (Priority: P4)

The station name is kept as a real piece of data rather than a note. The driver
can see where they fill up most, what they pay at each place, and how those
prices have moved.

**Why this priority**: This is the first thing in RevTab that gives advice rather
than a record, and it needs no judgement from the app at all. It is arithmetic
over data already being collected. It is also the cheapest advisory feature that
will ever exist here, because the scanner **already reads the station name** and
currently discards it into a free-text note.

**Independent Test**: After ten fill-ups across at least two stations, see which
is cheaper and by how much, and check it by hand against the receipts.

**Acceptance Scenarios**:

1. Given a scanned receipt, when the station is identified, then it is stored as
   its own field, not inside a note.
2. Given fill-ups at several stations, when the driver looks at them, then they
   can see which they use most and the price paid at each.
3. Given price history at a station, when it is displayed, then changes over time
   are visible.
4. Given two stations with a meaningful price difference, when that is shown, then
   it is expressed as something actionable, for example what it costs over a year
   at the driver's own rate of use.
5. Given the station could not be read, when the driver reviews the capture, then
   it can be entered or corrected by hand.
6. Given the same station written differently on different receipts, when they are
   counted, then the driver can treat them as one place.

### User Story 5 - Reviewing properly at a desk (Priority: P5)

Charts, category breakdowns, consumption over time and projections, arranged for a
large screen rather than a stretched phone column.

**Why this priority**: It creates understanding rather than data, but depends on
there being data worth exploring. Nobody is blocked today, the screens are merely
cramped.

**Independent Test**: On a desktop browser, answer without scrolling past three
screens: what the car cost per kilometre this year, which category grew fastest,
and whether consumption is getting worse.

**Acceptance Scenarios**:

1. Given a desktop-width screen, when review screens load, then the layout uses
   the width available.
2. Given a phone, when the same screens open, then they remain usable.
3. Given estimated figures beside logged ones, when displayed, then the difference
   stays obvious.

### User Story 6 - One receipt, several things bought (Priority: P6)

A petrol station receipt is rarely only fuel. The same slip carries a coffee, an
ice cream, a bottle of screenwash. At review time these are separated into their
own entries with a suggested category for each.

**Why this priority**: It improves the accuracy of the category split, which is
RevTab's second promise. But a mixed receipt logged as one fuel entry still has
the money and the distance right, so nothing is blocked without it.

**Independent Test**: Review a receipt with fuel and at least two other items, and
confirm each appears separately with a plausible category and the amounts still
sum to the receipt total.

**Acceptance Scenarios**:

1. Given a receipt with several purchases, when reviewed, then each item is listed
   separately with its own amount and a suggested category.
2. Given those suggestions, when the driver checks them, then each is marked as a
   guess and any can be changed.
3. Given the separated amounts, when summed, then they equal the receipt total,
   and the driver is told before saving if they do not.
4. Given the driver does not care about the detail, when they choose, then it can
   be saved as a single entry in one tap.
5. Given several separated items, when reviewed, then the photograph stays visible
   beside them, because checking a split from memory is not realistic.

### User Story 7 - What will this trip cost? (Priority: P7)

The driver is planning a journey. They enter a distance, and the app multiplies it
by what their car actually costs per kilometre.

**Why this priority**: Cheapest thing here, needs no new stored data, and it is
the only feature that helps before the driving rather than after it.

**Independent Test**: Enter a distance, see a cost, check it by hand against the
dashboard figure.

**Acceptance Scenarios**:

1. Given a distance, when an estimate is requested, then the cost is shown using
   that car's own cost per kilometre.
2. Given the app computes three different costs per kilometre (running costs only,
   with depreciation, with the full purchase price), when an estimate is shown,
   then it is clear which was used and the driver can switch.
3. Given it is a projection, when shown, then it is marked as an estimate like
   every other projected figure.
4. Given several cars, when estimating, then the journey can be compared across
   them.
5. Given a return journey, when indicated, then the distance doubles without the
   driver doing the arithmetic.

### Edge Cases

- A photograph is too dark, blurred, or the receipt is folded. Found at review,
  not at the pump, so the driver is not stuck on a forecourt. They correct by
  hand or retake from the original photo.
- The odometer photograph is unreadable. The driver types four digits. This is the
  same manual path the app needs anyway.
- The odometer reads lower than the last logged value.
- The receipt is foreign, in another language or currency.
- The same receipt is captured twice.
- A fuel type or unit the car is not set up for, for example kWh on a car recorded
  as diesel.
- A capture sits unreviewed for weeks, and other fill-ups are logged in the
  meantime, so its odometer reading is now out of sequence.
- The driver captures the receipt but forgets the odometer, or photographs the
  same thing twice.

## Requirements

### Functional Requirements

- **FR-001**: Capture MUST be reachable in one tap from opening the app, and MUST
  be a page with its own address so it can be linked to directly. Today it is a
  dialog on top of the expenses list and cannot be linked to at all.
- **FR-002**: Capture MUST take two photographs, the receipt and the odometer, and
  MUST make clear which is being asked for.
- **FR-003**: Capture MUST complete without a network connection, without running
  a scan, and without the driver confirming anything.
- **FR-004**: The app MUST NOT display figures derived from captures that have not
  been reviewed.
- **FR-005**: The app MUST show, wherever the driver is, how many captures are
  waiting to be reviewed.
- **FR-006**: Review MUST show both photographs beside the extracted values, and
  MUST allow them to be enlarged.
- **FR-007**: The app MUST visually distinguish guessed values from confirmed
  ones, and carry that distinction into anything calculated from them.
- **FR-008**: Any extracted value MUST be correctable without re-scanning.
- **FR-009**: The app MUST allow a capture to be completed entirely by hand,
  without any scan, so a driver who does not want a photograph leaving the device
  has a complete path.
- **FR-010**: The app MUST question an odometer reading lower than the most recent
  confirmed reading for that car, including when a delayed review puts it out of
  sequence.
- **FR-011**: The app MUST warn when a gap in odometer readings suggests a fill-up
  was never logged, and allow that warning to be dismissed permanently.
- **FR-012**: The station MUST be stored as its own field, not inside a note.
- **FR-013**: The app MUST show fuel price by station and how it has changed.
- **FR-014**: Review MUST separate a receipt containing several purchases into
  individual entries with a suggested category for each. (P6)
- **FR-015**: The app MUST warn when separated amounts do not sum to the receipt
  total. (P6)
- **FR-016**: Review screens MUST use the full width of a desktop display while
  remaining usable on a phone. (P5)
- **FR-017**: The app MUST estimate the cost of a journey from a distance the
  driver enters, stating which cost basis was used and that it is an estimate. (P7)
- **FR-018**: Every figure introduced here MUST be formatted through the single
  formatting layer, per Constitution Principle II.
- **FR-019**: Features that send data to an AI service MUST be identifiable as
  such in the interface.

### Non-Functional Requirements

- **NFR-001**: Capture, from opening the app to released, SHOULD take under ten
  seconds.
- **NFR-002**: Capture MUST be operable one-handed.
- **NFR-003**: Capture MUST never block on a network request.

## Decisions

Settled, with reasons, so they are not relitigated.

**Two photographs, not one frame.** The original design had the receipt and the
odometer in a single shot. Frank tried it and it does not work in real life.
Dashboard digits are a glowing display at an angle, and sharing a frame with a
receipt makes both worse. Two deliberate photographs are easier to frame and far
easier to read.

**Not the marker trick.** Frank currently writes the odometer on the receipt by
hand before photographing it, which works well and would make this a single
photograph. It was rejected as a design: it needs a working marker in the car,
and an app that requires its user to bring a pen is not solving the problem.

**No confirmation at the pump.** The earlier version had the driver check
extracted values on the forecourt within fifteen seconds. That was an assumption,
and Frank's actual behaviour over three years contradicts it. Checking belongs at
a desk. Removing it makes capture faster, removes all waiting, and makes offline
capture nearly free.

**Unreviewed captures do not count.** A number in RevTab is a confirmed number.
The cost of this is that figures lag until the driver sits down, which is handled
by warning them clearly rather than by quietly including unchecked data.

**Scanning goes directly to a chosen AI provider.** Not through Lovable's gateway,
which currently sits between RevTab and Google and receives every receipt. On
device scanning was considered and rejected: a cheap reader handles a creased
thermal receipt poorly and a dashboard display worse, and the mixed-receipt split
needs a model that understands documents, not one that reads text. The honest
claim is therefore not "never leaves your phone" but "goes to exactly one named
company, and you can type it in yourself instead".

**Photographs are kept on the device.** Optional cropping before sending, for
drivers who want to remove a card number. Worth knowing it has a cost: cropping
away everything except the total also removes the station name, which is what
powers User Story 4.

**Location: open, not decided.** Frank suggested receipts might carry the
station address, which would give location without the phone ever reporting
where it is. That needs checking against real receipts before it is a decision.
Recorded here as a question, not a rule, because it was a suggestion and this
document turned it into a rule too quickly once already.

What is settled is narrower: if location is ever collected, it is for a named
feature, and the driver knows. See the open question below.

**The home screen shortcut works.** Frank added `revtab.lovable.app/expenses` to
his iPhone home screen and it behaves well. It is a separate app with its own
storage, so it needs signing in once, after which it stays. An earlier test was
recorded as a failure because that one-time sign-in was mistaken for a permanent
problem. Once capture has its own address, the same trick lands directly in the
camera. On Android, manifest shortcuts achieve this inside the installed app.

## Assumptions

- Scanning happens on the server, as it does today.
- A car with one fuel type behaves exactly as it does now. Nothing here changes
  the calculation engine. If a change to the maths ever becomes necessary, the
  parity tripwire must still reproduce before it is considered safe, per
  Constitution Principle I.
- The existing Expenses screen remains for entries not made at a pump.
- Sign-in is unchanged.

## Success Criteria

- Frank captures a fill-up at a real station in under ten seconds, with a queue
  behind him, and drives away without waiting for anything.
- A week of captures is cleared from the review queue in one sitting, and the
  resulting figures match the receipts.
- The app correctly names a fill-up that was never logged.
- Frank can say which of his usual stations is cheaper, and by how much a year.
- A month later every fill-up has been captured, because capturing was not
  annoying enough to skip.

## Out of Scope

- Changing how any existing figure is calculated.
- Sign-in, account management, billing.
- Advisory features: whether to repair a car, whether to replace it, whether a
  quote is fair. These need good records first, and this feature is the records.
- A native iOS application.
- Recording the phone's location.

## Future Work

- **Advisory layer.** Keep or replace, is this repair worth it, is this quote
  reasonable. Explicitly sequenced after records, because an advisor working from
  thin data gives bad advice, which is worse than none.
- **Importing historical receipts.** Frank has two to three years of paper and
  emailed photographs. Backfilling them would make the app immediately valuable.
- **Uploading photographs off the device**, so they survive a lost phone and
  appear on a laptop. Plausibly a paid feature. Note "upload", not "sync":
  synchronising means two devices changing the same thing, which is a far larger
  job.
- **Reading an odometer photographed on its own**, outside this flow.
- **Fun statistics**, for example the furthest from home a fill-up has happened.
  Blocked on the open question about addresses below.

## Open Question

**How much should RevTab collect?** Frank is undecided between holding plenty of
data and doing interesting things with it, and holding only what is needed and
making privacy the point.

The resolving test, proposed and not yet agreed: **every field must have a
feature attached to it.** If the feature cannot be named, the field is not
collected. Station name earns its place because it produces price comparison.
The phone's location does not, because the only feature anyone has suggested for
it is a novelty statistic.

That is not "collect less". It is "collect on purpose", which is also what data
protection law actually asks for. It would mean checking whether receipts carry
addresses, since that would answer the question without the app ever knowing
where its users are.

## What changed and why, 2026-11-11

The first version of this document was written from assumptions. Frank then
described what he actually does, and most of them were wrong.

- One photograph became two. The single-frame design does not work in practice.
- Confirmation at the pump was removed entirely. He does not do it and never has.
- Offline capture went from a large separate story to a property of capture, since
  storing two photographs needs no connection.
- A review queue became the centre of the design rather than an afterthought.
- Station data was promoted from a note to a first-class feature, after it emerged
  that the scanner already reads it and throws it away.
- The home screen shortcut was restored after being wrongly recorded as impossible.

The old version is in git history if the reasoning is ever needed.
