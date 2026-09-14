# RevTab Constitution

RevTab is a multi-vehicle car running-cost tracker. It answers one question:
what does one kilometre of driving actually cost, and where does the money go.

This file holds the rules that govern how work on RevTab is done. It is short on
purpose. Every rule here is one Frank has agreed to enforce.

## Core Principles

### I. The maths is protected

`src/lib/calc.ts` is the calculation engine. It takes numbers in and gives
numbers out, and it touches nothing else: no database, no screen, no network.
That is what makes it testable in isolation.

It is not edited unless the task is explicitly about changing the maths.

`tests/fixtures/parity-vehicle.json` holds a snapshot of a real car, together
with the answers the engine is known to produce for it. This is called the
tripwire. Any change to the engine must still reproduce those answers before the
change is considered safe.

The tripwire numbers, as of ratification:

| What | Value |
|---|---|
| Consumption, normal driving | 7,56 l / 100 km |
| Consumption, loaded or towing | 10,00 l / 100 km |
| Measured average consumption | 7,85 l / 100 km |
| Running costs before tracking | 36 901 Kc |
| Maintenance rate | 3,01 Kc / km |
| Five-year projected total | 546 853 Kc |
| Lifetime cost so far | about 513 000 Kc |

**How this is checked:** run `npx vitest run`. The parity test prints these
numbers next to what the engine produces today. Frank can read that table and
compare it against what he knows his own car costs. He does not need to read any
code to judge it.

The tripwire car has exactly one fuel type. That is deliberate: it means the
single-fuel case is pinned by this test, and cannot silently change when the
multi-fuel code is touched.

### II. Numbers are formatted in one place

RevTab writes numbers in Czech style: a comma for the decimal, so "7,56" and not
"7.56", with a non-breaking space between a number and its unit.

`src/lib/format.ts` is the only file allowed to decide how a number looks.
Everywhere else asks it. No screen writes a number directly.

**How this is checked:** an automated check fails the test run whenever code
formats a number without going through that file.

**Outstanding at ratification:** 10 places currently bypass it, which is why
"0.0 yr" appears on the Insights page and why a percentage reads "38%" on
Expenses but "38 %" on the Dashboard. Those must be fixed before the automated
check can be switched on. Until then this principle is agreed but not yet
enforced, and saying so plainly is part of keeping it honest.

### III. Decisions live in the repository

Anything decided gets written into a tracked file inside this repository before
the session ends. Not into a chat window, and not into a folder beside the
repository.

This rule exists because of a specific failure. In September 2026 a Claude
account migration destroyed the conversation history, and 56 KB of analysis and
agreed decisions turned out to be sitting in a folder above the repository,
untracked. The code survived. The understanding did not.

**How this is checked:** the decision is in a file, the file is committed, and
`git log` shows it. If it only exists in a conversation, it does not exist.

### IV. Explanations are built for Frank, and Frank is the judge

Frank runs a training company and does design work. He does not write code and
does not intend to. He is the person who decides whether RevTab is right, so he
has to be able to follow what is being done to it.

- Before doing something, name it in one plain sentence: what is about to happen,
  and why.
- Define any term the first time it appears.
- If Frank says he does not understand, that output was defective. Rewrite it
  differently, with a concrete example from his own app. Do not repeat the same
  explanation more slowly.

**How this is checked:** Frank can say back, in his own words, what was done and
why it mattered. If he cannot, the explanation failed, however accurate it was.
The defect is in the output, never in Frank.

## Recorded Constraints

These are real and must be respected, but no automated check enforces them. They
are written down so the knowledge survives, not so it can be pretended it is
guarded.

**Visual design system, "ink and acid".** Paper `#FAFAF7`, ink `#16150F`,
acid-lime `#C8F031`, muted `#6B6B63`, hairline `#E2E2DB`. Sharp 2px corners
everywhere, with the MENU pill as the single deliberate exception. Instrument
Serif for headings, Inter for interface text and all numbers, using tabular
figures so columns line up. UK English spelling throughout, so "litres" and
"kilometre". Frank judges these by eye, which is what he is good at.

**Multi-fuel model.** A vehicle can have any number of fuel categories, each
with its own unit (litres, kWh, kg) and its own independent consumption series.
It is not a fixed petrol-or-electric switch. A car with exactly one fuel
category must behave exactly as it did before this model existed, and Principle I
is what holds that in place.

**Honesty about estimates.** Anything extrapolated or approximate must be
visually distinct from data the user actually logged, and must recompute as real
data arrives. This is the product's core promise and the reason anyone would
trust its numbers.

## Development Workflow

**Where the code lives.** Three copies: the Lovable project "revtab", the public
GitHub repository `FranksPlanet/carun`, and Frank's local clone. They are
expected to agree. When they do not, say so before doing anything else.

**Publishing is gated.** Work can be built freely. Deploying to the live app is
Frank's decision alone and is never done without him saying so.

**Running the tests.** `npx vitest run` from inside the `carun` folder. Note
that `npm ci` does not work: `package-lock.json` is out of sync with
`package.json`, because the project really builds with bun. Use
`npm install --no-save --no-package-lock` to install dependencies without
changing either lockfile.

## Governance

This constitution takes precedence over habit, over convenience, and over
finishing something quickly.

**When a rule is in the way:** stop. Tell Frank plainly which rule is blocking
the work and why, and wait for his decision. Do not proceed and explain
afterwards. Frank is relying on being told at the time, because he cannot read
the code to find out later.

**Amendments:** only Frank changes these rules, and the reason is written down
with the change. A rule may be argued against at any time, and should be if it
has stopped making sense. It does not move on its own. A locked design system
that drifted into 11 violations is what happens when guardrails move quietly.

**Version numbering:** MAJOR when a principle is removed or redefined, MINOR when
one is added or materially widened, PATCH for wording and clarifications.

**Version**: 1.0.0 | **Ratified**: 2026-09-14 | **Last Amended**: 2026-09-14
