# Job Inbox - 6 Sep 2026

Ran 04:33 UTC. 4 queries attempted, 0 returned, 0 jobs pulled, 0 after dedupe.
BID 0 - BORDERLINE 0 - NO 0

**Day five. Same wall.**

All four queries returned `HTTP 403 platform-feature-disabled`, message "Monthly
usage hard limit exceeded". Retried all four, same answer. It is account level,
so there is no partial result to salvage and no single query to drop. Nothing
was scored today because nothing was pulled.

Five days of YouTube postings have gone past unseen. At roughly 20 to 28 unique
jobs a run, that is somewhere over 100 jobs never looked at. Under-5-applicant
posts do not keep.

Account state, read live from `api.apify.com/v2/users/me/limits`:

- Plan: FREE, cap $5.00 per usage cycle
- Used: $6.0463
- Cycle: 13 Aug 2026 to 12 Sep 2026

## Nothing changed since yesterday

The spend is still $6.0463, unmoved for 96 hours. The failing runs cost nothing.
There is no new diagnostic information today and there will not be any tomorrow.
This is not a problem that develops, it is a problem that waits.

## Fix, pick one

1. Raise the hard limit or leave the free plan at
   https://console.apify.com/billing. Two minutes. The inbox is back the next
   morning, and 20 jobs a day costs about $0.02.
2. Wait for the cycle to roll on 13 Sep. Seven more days of no inbox.
3. Pause the routine at
   https://claude.ai/code/routines/trig_017tZTNWZ8VymdH5tasfD31J so it stops
   firing into a wall, then re-enable once 1 is done.

Doing nothing is option 2 by default, and it has been the default for five days.

## Manual fallback while this is down

The searches still work in a browser. Section 5 of CLAUDE.md has the URLs, and
the sidebar filter is Number of proposals, Less than 5. Paste any job link into
a session and the door-check and draft still run normally. The scraper is the
only broken part, not the pipeline.

## Which spec this run used

The stored routine prompt and `tasks/job-hunt-routine.md` still disagree, sixth
run running. The stored prompt asks for `youtube editor` / `youtube thumbnail` /
`youtube strategy` at maxResults 5, which is the pre-19-Aug version, and it also
says the repo file wins on any disagreement. So this run used the file:
`youtube channel manager`, `youtube channel audit`, `youtube consultant`,
`video content manager` at maxResults 10, with the editing and thumbnail kill
rule and the six-line format for every tier.

That drift needs correcting at https://claude.ai/code/routines. Editing the repo
file does not change what the scheduler serves. I can make that edit through the
API if you tell me to, but it rewrites what every future run executes, so I am
not doing it unasked. Since the scraper is down anyway, both fixes are one
sitting.

No jobs invented, no tier padded.
