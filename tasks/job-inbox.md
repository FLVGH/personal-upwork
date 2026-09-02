# Job Inbox - 2 Sep 2026

Ran 04:34 UTC. 4 queries attempted, 0 returned, 0 jobs pulled, 0 after dedupe.
BID 0 - BORDERLINE 0 - NO 0

**The run did not happen. Apify is out of credit, not out of jobs.**

Every one of the 4 queries came back `HTTP 403 platform-feature-disabled`,
message "Monthly usage hard limit exceeded". Retried, same. This is account
level, not query level, so there is no partial result to salvage. There is no
BID tier today because nothing was scraped, which is not the same as nothing
being out there.

Account state, straight from `api.apify.com/v2/users/me/limits`:

- Plan cap: $5.00 per usage cycle
- Used: $6.05
- Current cycle: 13 Aug 2026 to 12 Sep 2026

So the account is $1.05 over a $5 ceiling and the scraper is switched off until
the cycle rolls on **13 Sep 2026**. Ten more daily runs between now and then all
fail the same way unless one of the below is done.

## Fix, pick one

1. Raise the hard limit or move off the free plan at
   https://console.apify.com/billing. The $5 cap is the free tier. At the
   $0.001 per job Black Falcon charges, this scraping is pennies, so the cap is
   the binding constraint, not the cost.
2. Wait until 13 Sep. Eleven days with no inbox.
3. Pause the routine at https://claude.ai/code/routines/trig_017tZTNWZ8VymdH5tasfD31J
   until whichever of the above is done, so it stops firing into a wall.

## Note on which spec this run used

The stored routine prompt and `tasks/job-hunt-routine.md` still disagree. The
stored prompt asks for `youtube editor` / `youtube thumbnail` / `youtube
strategy` at maxResults 5, which is the pre-19-Aug version, and the file's rules
say the file wins. This run used the file: `youtube channel manager`, `youtube
channel audit`, `youtube consultant`, `video content manager` at maxResults 10.
That drift is still unfixed and still has to be corrected by hand at
https://claude.ai/code/routines, since editing the repo file does not change
what the scheduler serves.

No jobs invented, no tier padded. Nothing was scored because nothing was pulled.
