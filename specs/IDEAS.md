# Captured ideas

Not specs. Ideas with enough detail written down that they can be picked up cold,
so they are not lost between sessions. Promote one to a numbered spec when it is
actually next.

---

## Editable dashboard headline

**Captured 2026-09-14, from Frank.**

The dashboard currently opens with a fixed three-sentence story about the car.
Something like: the car has driven 2 568 km at 27,5 Kc per km, you burned
251 litres at 8,35 litres per 100 km, and 38 percent of your spending was admin.

The idea: let the driver choose what that sentence talks about. Tick the
categories and figures you care about, and the app composes a sentence from your
selection using your real data.

**Why it is a good fit.** The story is already the most distinctive thing on the
screen and the part that sounds like a person rather than a dashboard. Different
drivers care about different things: one watches fuel economy, another watches
what servicing is costing, another only wants the headline cost per kilometre.
Fixed sentences serve the average of those people and therefore none of them.

**Frank's own constraint, worth keeping.** There must be a maximum, or the
sentence grows without end and stops being a headline. Probably three or four
items. Past that it is a list, not a story, and the app already has screens for
lists.

**Open questions for whenever this gets specced.**

- Does the choice belong to the driver, or to each car? A daily runabout and a
  restoration project might warrant different headlines.
- What happens when a chosen figure has no data yet, for example consumption
  before two full tanks have been logged? Hide the clause, or say plainly that it
  is not known yet? The app's honesty rule points at saying so.
- Does the sentence still read well in every combination, or do some pairings
  produce nonsense? This is a writing problem as much as a code one, and Frank is
  better placed to judge it than any test.
- Is there a sensible default for someone who has just signed up and has not
  chosen anything?

**Related code.** `src/components/story-narrative.tsx` is where the sentences are
built today.

**Relevant rule.** Every number in it must go through the single formatting layer,
per Constitution Principle II, rather than being assembled inline. The story is
exactly the kind of place where a stray formatting call would otherwise appear.
