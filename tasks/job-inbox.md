# Job Inbox - 7 Sep 2026

Ran 04:34 UTC. 4 queries attempted, 0 jobs pulled, 0 after dedupe.
BID 0 - BORDERLINE 0 - NO 0

**The run failed. No jobs today, and none tomorrow either unless you act.**

Every Apify call came back HTTP 403:

```
{"error":{"type":"platform-feature-disabled","message":"Monthly usage hard limit exceeded"}}
```

Retried once as the spec says. Same error on both queries tested, so it is
account-level, not one bad query. All 4 queries are dead: `youtube channel manager`,
`youtube channel audit`, `youtube consultant`, `video content manager`.

## What is actually wrong

Apify account `fmyt99`, FREE plan.

| | |
|---|---|
| Monthly cap | $5.00 |
| Used this cycle | $6.05 |
| Cycle ends | 12 Sep 2026, 23:59 UTC |

You are $1.05 over a $5 ceiling. The platform has cut off actor runs for the rest of
the cycle. **The routine will fail every morning through 12 Sep and start working again
by itself on 13 Sep.**

## Three ways out, cheapest first

1. **Do nothing.** Six blank mornings, back to normal 13 Sep. Free. You lose a week of
   inbox at a point where BID has been thin anyway.
2. **Raise the hard limit on the free plan.** https://console.apify.com/billing goes to
   Limits, raise the monthly cap above $5. Anything over $5 bills you at usage rates.
   Roughly $0.20/day at the current settings, so about $1.20 to buy back the six days.
3. **Upgrade to Starter.** $39/mo. Overkill for 40 job pulls a day.

Option 2 is the one that matches the volume.

## Worth knowing regardless

The cost note in `tasks/job-hunt-routine.md` says 20 jobs a day is about $0.02 a day and
100 jobs is $0.10. Real spend is $6.05 in a 30-day cycle, roughly $0.20 a day, which is
ten times the estimate. `enrichDetails: true` is the likely reason: it opens each job
page separately, so the per-job price is not what the actor advertises for a plain
listing pull. If you take option 2, set the new cap with $0.20/day in mind, not $0.02.

Also unresolved from the 14 Aug run: the scheduler is still serving a stored prompt that
differs from the spec in `tasks/job-hunt-routine.md` (it asks for `youtube editor` and
`youtube thumbnail` queries, `maxResults` 5, and a bare table for the NO tier, all of
which the file supersedes). This run followed the file, as both documents instruct. The
stored prompt at https://claude.ai/code/routines still needs replacing by hand.

## BID

None. The scraper never ran.

## BORDERLINE

None. The scraper never ran.

## NO

None. The scraper never ran. This is an empty list because of a billing block, not
because 20 jobs were pulled and all 20 were bad. Nothing was scored today.
