# Job Inbox - 4 Sep 2026

Ran 04:34 UTC. 4 queries attempted, 0 returned, 0 jobs pulled, 0 after dedupe.
BID 0 - BORDERLINE 0 - NO 0

**Third day in a row with no run. This one will not fix itself.**

Same block as 2 Sep and 3 Sep. All 4 queries returned `HTTP 403
platform-feature-disabled`, message "Monthly usage hard limit exceeded".
Retried one, same answer. Account level, so there is no partial result to
salvage and no single query to drop. Nothing was scored today because nothing
was pulled. That is not the same as nothing being out there, and three days of
YouTube postings have now gone past unseen.

Account state, read live from `api.apify.com/v2/users/me/limits`:

- Plan: FREE, cap $5.00 per usage cycle
- Used: $6.05
- Cycle: 13 Aug 2026 to 12 Sep 2026

## One thing that is new today

The spend number has not moved in 24 hours. It was $6.0463 yesterday and it is
$6.0463 now. So the failing runs cost nothing, and the $6.05 was already spent
before the first failure on 2 Sep. Yesterday's open question about where the
money went is answered: it was not the daily job pulls. Raising the cap will
hold fine at 20 jobs a day, which is about $0.02.

Worth a glance at https://console.apify.com/actors/runs anyway to see what ate
the first $5, but it is not a blocker.

## Fix, pick one

1. Raise the hard limit or leave the free plan at
   https://console.apify.com/billing. Two minutes. The inbox is back tomorrow.
2. Wait until 13 Sep. Eight more days with no inbox.
3. Pause the routine at
   https://claude.ai/code/routines/trig_017tZTNWZ8VymdH5tasfD31J so it stops
   firing into a wall, and re-enable it once 1 is done.

Doing nothing is option 2 by default.

## Which spec this run used

The stored routine prompt and `tasks/job-hunt-routine.md` still disagree, fourth
run running. The stored prompt asks for `youtube editor` / `youtube thumbnail` /
`youtube strategy` at maxResults 5, which is the pre-19-Aug version, and it also
says the repo file wins on any disagreement. So this run used the file:
`youtube channel manager`, `youtube channel audit`, `youtube consultant`,
`video content manager` at maxResults 10, with the editing and thumbnail kill
rule and the six-line format for every tier.

That drift has to be corrected by hand at https://claude.ai/code/routines.
Editing the repo file does not change what the scheduler serves. Since the
scraper is down anyway, this is a good moment to do both fixes in one sitting.

No jobs invented, no tier padded.
