# Pre-launch checklist

Frank's list, checked against the code on 2026-09-14. Status is what the code
says today, not what anyone remembers.

Two of these are hard blockers. Everything else is finishable in a session or
two.

## Blocking

| Item | Status | What is actually there |
|---|---|---|
| Privacy policy page | **Placeholder** | The page exists and says so on screen: "Placeholder wording, must be reviewed by a human". |
| Terms and conditions | **Placeholder** | Same. Signup makes people tick a box agreeing to terms that have not been written. |
| Real contact address | **Placeholder** | `contact.tsx` says the legal and business details need filling in. |

Publishing terms you have not written, while collecting consent to them, is the
single biggest non-code launch blocker. All three need real wording and a look
from someone qualified.

## Missing, but small

| Item | Status | Note |
|---|---|---|
| robots.txt | Missing | Not in `public/`. A few lines. |
| sitemap.xml | Missing | Not in `public/`. The app is mostly behind a login, so this only matters for the public pages. |
| Open Graph image | Missing | No route sets `og:image`. This is what appears when someone shares a link. |
| Analytics | Missing | Nothing installed. Pick a cookieless one, see below. |
| Compressed images | Not done | A single vehicle photo in storage is 9 MB. Nothing resizes or compresses on upload. |

## Partly done

| Item | Status | Detail |
|---|---|---|
| Meta title on every page | 11 of 14 | Missing on `index.tsx`, `_authenticated/route.tsx`, and the OAuth consent page. |
| Meta description on every page | 5 of 14 | Missing on index, dashboard, expenses, garage, insights, onboarding, settings, the layout route and the consent page. |
| Loading states | Partly | Known defect: the dashboard and insights screens show numbers before the data has loaded, so a confident but wrong cost per kilometre flashes up. Recorded in the August audit. |
| Form error states | Partly | Some forms have them, not all. |
| Favicon | Partly | `favicon.svg` exists. |
| **Installable as an app** | **Fixed 2026-09-14, needs testing on a real phone** | The two missing icon files were generated from the existing mark, an `apple-touch-icon` was added because iOS ignores the manifest icons, the manifest is now linked from the page, and the iOS standalone tags were added. Untested on an actual iPhone until the next deploy. |

## Already done

- **Custom 404 page.** Real component wired up in `__root.tsx`.
- **Alt text on every image.** Both image tags in the app have it.
- **Mobile breakpoints.** The app is mobile-first already.

## Needs your eye, not mine

These are judgement, and you are better placed than any check I can write.

- **CTA above the fold.** What do you want someone to do in the first screenful?
- **Sticky mobile CTA.** This overlaps directly with spec 001: the always-available
  LOG button is exactly a sticky mobile call to action. Do not design them twice.
- **Thank you page.** Unclear this app has a moment that needs one. Possibly after
  signup, possibly not at all.

## One item that is not as simple as it looks

**Cookie banner.** Do not add one reflexively.

Under GDPR and the ePrivacy rules, cookies that are strictly necessary for a
service the user asked for, which includes staying signed in, do not require
consent. RevTab currently sets nothing else. A banner today would be asking
permission for something that does not need it, and trains people to dismiss the
thing without reading.

The banner becomes necessary the moment you add analytics that track people. So
the two items are linked, and there is a way to avoid both: pick analytics that
does not use cookies or identify individuals, and you need no banner at all.

Decide analytics first. The banner follows from that choice.

### Which cookieless analytics

All of these count visits without cookies and without identifying individuals,
so none of them triggers a consent banner.

| Option | Cost | Notes |
|---|---|---|
| **Cloudflare Web Analytics** | Free | No cookies, no limits worth worrying about. One script tag. The obvious starting point when the question is whether anyone is using the app at all. |
| **Plausible** | About 9 EUR a month | Cookieless, EU hosted, nicer dashboard, exportable. Worth paying for once there are enough users that you care about trends rather than existence. |
| **Fathom** | About 14 USD a month | Similar to Plausible. |
| **Umami** | Free to self-host | Cheapest long term, but you are then running a service, which is a job. |

Recommendation: Cloudflare to begin with, because it is free and answers the only
question that matters right now, which is whether anyone opens the app twice.
Move to Plausible if and when the numbers are worth reading properly.

Avoid Google Analytics. It sets cookies, identifies visitors, and puts you
straight back into needing a consent banner, which is the thing this whole
section exists to avoid.

## How this list came about

Frank supplied the list. Every line was checked against the code rather than
assumed. One item, alt text, first looked missing and turned out to be fine on
closer inspection, which is why the status column says what was actually found.
