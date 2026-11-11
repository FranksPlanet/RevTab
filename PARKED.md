# Parked, 11 November 2026

Frank paused RevTab to finish work that needed him more urgently. This is not
abandonment and nothing here is broken. Read this first on returning.

## The decision that stands

**Leave Lovable. Run RevTab on Frank's own Supabase account.** Agreed in
principle, not started. Nothing in the codebase has been changed for it.

The reason is not the reported Lovable incident, which turned out to be narrower
than the headlines said and did not touch this project. It is that RevTab is
meant to sell data protection, and three things currently sit outside Frank's
control: the database project, the key that bypasses row-level security, and the
AI gateway that receipt photographs are sent to.

## Order of operations when work resumes

Each step is independently useful and independently abandonable. Do not do them
in one go.

1. **Move the database to Frank's own Supabase account.** This is the whole
   point and everything else is optional after it.
2. **Drop Google and Apple sign-in**, leaving email and password. They are the
   only reason Lovable brokers anything on the auth side. Frank already has a
   password on his account, so nothing is lost by removing them.
3. **Replace the AI gateway** used by receipt scanning. Today photographs go to
   `ai.gateway.lovable.dev`. This is the step that matters most for the privacy
   claim.
4. **Move hosting.**
5. **Drop the Vite wrapper last**, if at all. It is the least urgent and the most
   disruptive.

## How hard is the migration, really

Not hard. The data is small and the code already lives in git.

- 337 expenses, 3 vehicles, 1 user, 4 image files, about 25 MB.
- The schema is already in `supabase/migrations/`.
- A verified full export sits in `exports/2026-11-11/`, taken the day the project
  was parked. It is gitignored because it holds personal data and this repo is
  public.

The honest cost is not moving the rows. It is re-pointing the app, re-testing
sign-in, and re-doing the receipt scanning without Lovable's gateway.

## One thing found while taking the export

**`categories` has no foreign key to `auth.users`.** Every other table has one
with `ON DELETE CASCADE`. Categories does not, so when an account is deleted its
categories stay in the database forever. There are 19 such rows right now,
belonging to four accounts that no longer exist, left over from testing account
deletion in August and September 2026.

Verified, not assumed: the constraint list was read from `pg_constraint` and the
orphan rows counted against `auth.users`.

This matters more than its size. A product that sells data protection must not
keep rows belonging to deleted accounts. The fix is one migration: delete the
orphans, then add the missing constraint. It was not done on 11 November because
the project was being parked, not worked on.

## State at the moment of parking

- Everything is pushed. `main` is at `7900bdd` on
  `github.com/FranksPlanet/RevTab` (the repo was renamed from `carun`).
- **Deployed on 11 November 2026**, with Frank's go-ahead, ending the gap since
  3 September. Lovable reports `latest_commit_sha` 7900bdd, so what is live is
  exactly what is in git. Verified independently: `apple-touch-icon.png` serves
  200 at 6655 bytes, matching the committed file, and the installability meta
  tags are in the served HTML.
- **Cost per kilometre is now correct in the live app.** The odometer fix had
  been sitting unshipped since 25 August.
- Publishing did not cost credits. Lovable's docs list Plan, Build and Chat
  messages, Cloud, the AI gateway, connectors and media generation as the
  credit-consuming actions. Publishing is not one, and the deploy went through
  the API rather than a build message. Worth remembering: being out of build
  credits does not stop shipping what is already built.
- `.env` is tracked in the public repo but contains only the publishable key,
  which is designed to be public. Re-checked on 11 November. No secret is
  exposed.
- Spec 001 is written and has seven stories. None are built. `tasks` was never
  run on it.

## The trap to avoid on returning

The temptation will be to re-plan. Do not. The planning is done and it is good.
Seven stories are specced and ordered, and P1 is deliberately the smallest useful
thing: one tap, two photographs, saved.

The thing that killed momentum last time was not bad code or a bad plan. It was
losing the thread. The answer to that is to ship P1, not to spec P8.
