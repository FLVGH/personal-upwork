# Job Inbox - 3 Sep 2026

Ran 04:34 UTC. 4 queries attempted, 0 returned, 0 jobs pulled, 0 after dedupe.
BID 0 - BORDERLINE 0 - NO 0

**Second day in a row with no run. Apify is out of credit, not out of jobs.**

Same wall as 2 Sep. All 4 queries returned `HTTP 403 platform-feature-disabled`,
message "Monthly usage hard limit exceeded". Retried one, same answer. It is an
account level block, so there is no partial result to salvage and no query to
drop. There is no BID tier today because nothing was scraped. That is not the
same as nothing being out there.

Account state, read live from `api.apify.com/v2/users/me/limits`:

- Plan: FREE, cap $5.00 per usage cycle
- Used: $6.05
- Cycle: 13 Aug 2026 to 12 Sep 2026

The account is $1.05 over a $5 ceiling. The scraper stays off until the cycle
rolls on **13 Sep 2026**. That is nine more daily runs that will fail exactly
like this one unless something below gets done.

## Fix, pick one

1. Raise the hard limit or leave the free plan at
   https://console.apify.com/billing. Black Falcon charges about $0.001 a job,
   so 20 jobs a day is roughly $0.02. The $5 free cap is the binding
   constraint, not the cost.
2. Wait until 13 Sep. Nine days with no inbox.
3. Pause the routine at
   https://claude.ai/code/routines/trig_017tZTNWZ8VymdH5tasfD31J so it stops
   firing into a wall, and re-enable it once 1 is done.

Worth checking where the $6.05 went, since 20 jobs a day should not spend $5 in
a cycle. Run history is at https://console.apify.com/actors/runs. If a run is
retrying or pulling far more than 20 jobs, raising the cap alone will not hold.

## Which spec this run used

The stored routine prompt and `tasks/job-hunt-routine.md` still disagree, third
run running. The stored prompt asks for `youtube editor` / `youtube thumbnail` /
`youtube strategy` at maxResults 5, which is the pre-19-Aug version, and it also
says the repo file wins on any disagreement. So this run used the file:
`youtube channel manager`, `youtube channel audit`, `youtube consultant`,
`video content manager` at maxResults 10, with the editing and thumbnail kill
rule and the six-line format for every tier.

That drift has to be corrected by hand at https://claude.ai/code/routines.
Editing the repo file does not change what the scheduler serves.

No jobs invented, no tier padded. Nothing was scored because nothing was pulled.
