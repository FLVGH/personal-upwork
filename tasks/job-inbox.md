# Job Inbox - 12 Sep 2026

Ran 04:34 UTC. 4 queries attempted, 0 jobs pulled, 0 unique.
BID 0 - BORDERLINE 0 - NO 0

**THE RUN FAILED AGAIN. No jobs were scored today. This is not an empty BID tier, it is no
data at all.** Day 3 of 3. This is the last one. Yesterday's inbox called this exact
outcome, and nothing here contradicts it.

## What broke

Same error as 10 and 11 Sep, on the first attempt and on the retry:

```
{"error":{"type":"platform-feature-disabled","message":"Monthly usage hard limit exceeded"}}
```

Checked the account directly again:

| Field | Value |
|-------|-------|
| Apify account | `fmyt99`, Flavio Mendes, fmyt99@gmail.com |
| Plan | Free, $5 monthly usage cap |
| Current usage | **$6.0465** against the $5 cap |
| Cycle | 13 Aug 2026 to **12 Sep 2026 23:59 UTC** (ends tonight) |

Usage moved by nine ten-thousandths of a dollar in 24 hours, which is the failed requests
themselves and nothing else. No scrape has run since 9 Sep.

## Nothing to do

The cycle ends tonight at 23:59 UTC. The routine fires at 04:33 UTC on 13 Sep, which is
after the reset, so **tomorrow morning's run recovers on its own.** No action needed, no
cap to raise, no setting to flip.

If the 13 Sep run fails too, that is a new problem and not this one, and it will say so.

Cost of the blackout: three days, 10 to 12 Sep. Roughly 60 jobs never looked at on paper.
The real number is lower, since a fresh-post-under-5-proposals job is picked over within
hours anyway, so most of what was missed was already cold by the time the routine would
have written it up.

## Manual cover for today

Apify is the only thing that is down. The sidebar filter does the same job by hand. Open
each, set **Number of proposals → Less than 5**, sort by newest:

- https://www.upwork.com/nx/search/jobs/?q=youtube%20channel%20manager&payment_verified=1&sort=recency
- https://www.upwork.com/nx/search/jobs/?q=youtube%20channel%20audit&payment_verified=1&sort=recency
- https://www.upwork.com/nx/search/jobs/?q=youtube%20consultant&payment_verified=1&sort=recency
- https://www.upwork.com/nx/search/jobs/?q=video%20content%20manager&payment_verified=1&sort=recency

Paste any link into the chat and the door-check plus draft flow runs normally. That path
never touched Apify.

## Note on the routine spec

Still open, third day running. The stored routine prompt at claude.ai/code/routines is the
pre-19-Aug version: it asks for the queries `youtube editor` and `youtube thumbnail`
(hands-on production work, outside the bucket), `maxResults` 5, and a bare NO table.
Confirmed directly against the trigger record today, last edited 26 Aug.

`tasks/job-hunt-routine.md` wins per its own rule, so this run used the file's four queries
at `maxResults` 10. Moot again today since nothing returned, but it will stop being moot
tomorrow when data comes back. Replace the stored prompt by hand at
https://claude.ai/code/routines/trig_017tZTNWZ8VymdH5tasfD31J with the block in
`tasks/job-hunt-routine.md`, or every run keeps drifting back to the old spec.

## BID

None. No data.

## BORDERLINE

None. No data.

## NO

None. No data.
