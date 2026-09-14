# Working on RevTab

This file exists because a Claude account migration in September 2026 destroyed
Frank's conversation history, and with it everything about how we worked
together. The code survived. The understanding did not.

If you are an assistant starting fresh on this project, read this first. If you
are Frank re-reading it after losing your history again, this is the thing that
gets us back to where we were.

The binding rules are in `.specify/memory/constitution.md`. This file is how we
work. Read both.

## Who you are working with

Frank runs a training company and does design work. He does not write code and
does not intend to. He drives by describing what he wants; the assistant writes
it and runs it.

He is not a beginner at judgment. He ran a web design studio and is used to
directing developers whose work he could not read line by line. He is very good
at spotting when something looks wrong on screen and at knowing whether a number
about his own car is plausible. He is not able to evaluate a claim about the code
by inspecting the code.

That has one consequence that governs everything else: **work he cannot evaluate
is, from where he sits, indistinguishable from no work at all.** Two sessions in
August and September 2026 both shipped verified fixes, and he still experienced
them as scrambling, because the reports were written in language he could not
follow. That was a communication failure, not a productivity failure.

### How to talk to him

- Say what you are about to do before you do it, in one plain sentence.
- Define a term the first time you use it. Every time. Assume nothing carried
  over from a previous session, because it may not have.
- Prefer a concrete example from his own app over an abstract description. When
  he said he did not understand a question about the design system, what worked
  was showing him that his Insights page displays "0.0 yr" with a dot instead of
  a comma. The abstract version had failed twice.
- When he says he does not understand, that is a defect in the output. Rewrite
  it differently. Do not repeat it more slowly.
- Be brief. No preamble.
- Disagree out loud. He asks for this directly and means it. He asked to be
  challenged on the word "restart" and was right to.
- Never use the em dash character, anywhere, including in code comments and docs.
- Ask when something is ambiguous instead of picking a sensible default and
  moving on.

## What RevTab is

A car running-cost tracker, live at revtab.lovable.app. It answers one question:
what does one kilometre of driving really cost, and where does the money go.

Users log expenses against odometer readings. The app works out cost per
kilometre, splits it by category, tracks fuel consumption between full tanks,
flags unusual entries, and projects the total cost of owning the car.

The product promise is honesty about estimates. Anything the app extrapolated
rather than measured must look visibly different from real logged data, and must
recompute as real data arrives. That promise is the reason anyone would trust
the numbers, so it outranks convenience.

It is installable on a phone, works for multiple cars, and each user sees only
their own data.

## Where the code lives

Three copies that are expected to agree:

1. The Lovable project "revtab"
2. GitHub: `https://github.com/FranksPlanet/carun` (public)
3. Frank's local clone

**A trap worth knowing:** the local checkout is at
`~/Documents/GitHub/RevTab/carun`. The git repository is the `carun` folder, not
`RevTab`. The folder above it is not version controlled, and in September 2026
two important reports were found sitting there, outside git, invisible to
everyone. Check which folder you are in before concluding a file is missing or
untracked.

## How to work with Lovable

Lovable is the tool that actually writes the code. The loop is: send it a written
instruction, it edits the project and commits to GitHub, then verify the result.

- Send with `wait: false`. With the default wait the call times out after ten
  minutes even though the build succeeds, which looks like a failure and is not.
- Then watch for the commit with
  `git ls-remote https://github.com/FranksPlanet/carun.git refs/heads/main` in a
  background loop. Lovable commits every edit, so a changed hash means it
  finished.
- Do not call `list_messages` to check progress. One page of it was 1.3 million
  characters. Use `get_message` with the specific message id instead.
- Messages queue, so several instructions can be sent back to back.

Write instructions that say what outcome is wanted and why it matters, not how to
implement it. Include the constraints that must not be broken. Ask it to report
what it could not verify. It is good at saying so when asked directly.

## Verify, do not relay

This is the most important working habit in this file.

Frank cannot check the code, so passing along an assistant's own summary of its
own work gives him nothing. Verify independently and say plainly which claims
were checked and which are somebody's word.

Things this habit has actually caught:

- A change raised the minimum password length and applied it to the sign-in form
  as well as sign-up. Every existing user with a short password would have been
  locked out of an app they could previously use. The tests passed and the
  typecheck passed.
- A password-change screen asked for the current password and sent it to an API
  that ignores that field. Testing against the real backend showed a
  deliberately wrong password returned success and changed the password anyway.
  The code was correct-looking and well commented. It verified nothing.
- A regression test was quietly drifting and would have started failing on
  25 December 2026 for no reason connected to any change. It was found by
  comparing one number across two reports, not by any tool.

None of those were findable by running the tests. Ask for empirical proof rather
than assurances. "A wrong password returned 200 and changed it anyway" is worth
more than any amount of confident commenting.

## Where things stand

As of 14 September 2026:

- 58 tests pass. The calculation tripwire reproduces.
- The calculation engine has not changed since 2 August 2026 and is stable.
- Local clone and GitHub are identical, nothing uncommitted.
- Work from the last two sessions is committed but **not deployed**. Deploying is
  Frank's call.
- `.env` is committed to the public repo but contains only the publishable key,
  which is designed to be public. No secret is exposed.
- `npm ci` does not work: `package-lock.json` is out of sync with
  `package.json`, because the project really builds with bun. Use
  `npm install --no-save --no-package-lock` to install dependencies.

## What is deliberately still open

Not forgotten. Decided against, for now, with reasons.

- **Recurring costs and past repairs can only be created, never edited or
  deleted.** The only way to fix a typo is deleting the car, which deletes its
  expenses too. This is a real data-loss trap. It was deferred into a planned
  redesign, and must not quietly disappear.
- **Ten places format numbers without going through the formatting file.** Fixing
  them is now required before the automated check in Principle II can be turned
  on.
- **The password reset email** uses a generic template from a default sender and
  does not mention RevTab. It is the product's first ever email to a user.
- **The redirect allow-list for password reset was never verified.** If the
  emailed link lands on the site root instead of the reset page, that is the
  cause.
- **Adding a password to a Google or Apple account has never been tested** on a
  real account of that kind.
- Roughly 87 low-severity audit findings were never checked. The refutation rate
  on findings that were checked was high, 14 out of 32, so treat that list as
  things to verify, not things to fix.

## How we got here

Useful for judging what kind of project this is.

464 commits sounds enormous but is misleading: about 390 are automatic saves
Lovable makes on every edit. Only about 72 were deliberate steps, across 13
working days spread over three months.

- **11 to 18 June 2026:** the whole app was built, scaffold to working product.
- **8 July:** one small cosmetic session.
- **1 to 5 August:** it got more capable. Multi-fuel, VAT, receipt scanning,
  anomaly detection.
- **25 August:** an audit and twelve fixes.
- **3 September:** password reset and sign-in methods.

Very little was ever thrown away: five source files total, and two of those were
pages merged into another screen on the first day. The churn is concentrated in
three screens, Expenses, Insights and Dashboard. The foundation settled early and
the surface kept moving.

The last three sessions were all defensive: audit, repair, authentication. Each
finished what it started. None moved the product towards launch. The planned next
step is a redesign around two different ways of using the app: fast one-handed
logging at the petrol pump on a phone, and detailed analysis later at a desk.
That has not started.

## Files worth knowing about

- `.specify/memory/constitution.md` is the binding rules. Short, read it.
- `src/lib/calc.ts` is the calculation engine. Locked. See Principle I.
- `tests/fixtures/parity-vehicle.json` is the tripwire: a real car and the
  answers the engine must still produce for it.
- `scripts/verify-account-deletion.sql` is the only check that genuinely proves
  account deletion works. It has to be run by hand.
- Two reports from earlier sessions, `AUDIT-2026-08-12.md` and `AFK-REPORT.md`,
  currently sit in the folder **above** the repository and are not tracked. They
  should be moved in. Principle III exists because of them.
