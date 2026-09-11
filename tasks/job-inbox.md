# Job Inbox - 11 Sep 2026

Ran 04:35 UTC. 4 queries attempted, 0 jobs pulled, 0 unique.
BID 0 - BORDERLINE 0 - NO 0

**THE RUN FAILED AGAIN. No jobs were scored today. This is not an empty BID tier, it is no
data at all.** Day 2 of the 3-day Apify blackout called in yesterday's inbox.

## What broke

Same error as 10 Sep, on the first attempt and on the retry, for all 4 queries:

```
{"error":{"type":"platform-feature-disabled","message":"Monthly usage hard limit exceeded"}}
```

Checked the account directly. Nothing has changed since yesterday:

| Field | Value |
|-------|-------|
| Apify account | `fmyt99`, Flavio Mendes, fmyt99@gmail.com |
| Plan | Free, $5 monthly usage cap |
| Current usage | **$6.046** against the $5 cap |
| Cycle | 13 Aug 2026 to **12 Sep 2026 23:59 UTC** |

Usage is byte-for-byte identical to yesterday's reading ($6.046442), which confirms nothing
ran and no further spend happened. The scraper refuses to start, so nothing was pulled and
nothing could be scored.

**The 12 Sep run will fail the same way.** The cycle resets 13 Sep and that morning's run
recovers on its own unless the cap is raised sooner.

## Fix, one of two

1. **Wait it out.** Do nothing. The 13 Sep run works again. Cost: two more days of no
   inbox, 11 and 12 Sep. At 20 jobs a day that is roughly 40 jobs never looked at, though
   the good ones on a fresh-post filter are gone in hours anyway, so the real loss is
   smaller than the number suggests.
2. **Raise the cap.** https://console.apify.com/billing → upgrade or set a higher hard
   limit. The scrape itself is about $0.02 a day at 20 jobs. The $6.05 that blew the cap
   was not this routine's steady-state cost, it was the earlier high-volume runs.

Recommendation: wait. Two days out of a 30-day cycle is not worth a plan change, and the
routine self-heals on 13 Sep.

## Manual cover for today

If you want jobs this morning without Apify, the sidebar filter does the same job by hand.
Open each, set **Number of proposals → Less than 5**, sort by newest:

- https://www.upwork.com/nx/search/jobs/?q=youtube%20channel%20manager&payment_verified=1&sort=recency
- https://www.upwork.com/nx/search/jobs/?q=youtube%20channel%20audit&payment_verified=1&sort=recency
- https://www.upwork.com/nx/search/jobs/?q=youtube%20consultant&payment_verified=1&sort=recency
- https://www.upwork.com/nx/search/jobs/?q=video%20content%20manager&payment_verified=1&sort=recency

Paste any link into the chat and the normal door-check plus draft flow still works. That
path never touched Apify.

## Note on the routine spec

The stored routine prompt at claude.ai/code/routines is still the pre-19-Aug version: it
asks for the queries `youtube editor` and `youtube thumbnail` (hands-on production work,
outside the bucket), `maxResults` 5, and a bare NO table. `tasks/job-hunt-routine.md` wins
per its own rule, so this run used the file's four queries at `maxResults` 10. Moot today
since nothing returned, but the stored prompt still has to be replaced by hand at
https://claude.ai/code/routines/trig_017tZTNWZ8VymdH5tasfD31J or every run keeps drifting
back to the old spec.

## BID

None. No data.

## BORDERLINE

None. No data.

## NO

None. No data.
