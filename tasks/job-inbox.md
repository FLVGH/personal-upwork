# Job Inbox - 10 Sep 2026

Ran 04:35 UTC. 4 queries attempted, 0 jobs pulled, 0 unique.
BID 0 - BORDERLINE 0 - NO 0

**THE RUN FAILED. No jobs were scored today. This is not an empty BID tier, it is no data at all.**

## What broke

Every one of the 4 Apify calls came back with the same error, on the first attempt and on
the retry:

```
{"error":{"type":"platform-feature-disabled","message":"Monthly usage hard limit exceeded"}}
```

The Apify account (`fmyt99`, Flavio Mendes, fmyt99@gmail.com) is on the FREE plan, which
caps at $5 of usage per monthly cycle. Current usage is **$6.05 against a $5 cap**. The
scraper is refusing to start, so nothing was pulled and nothing could be scored.

Billing cycle: 13 Aug 2026 to **12 Sep 2026 23:59 UTC**.

That means the next two scheduled runs, **11 Sep and 12 Sep, will fail the same way**.
The cycle resets on **13 Sep 2026** and the run recovers on its own that morning unless
the cap is raised sooner.

## Fix, one of two

1. **Wait it out.** Do nothing. The cycle resets 13 Sep and the 13 Sep run works again.
   Cost: three days of no inbox, 10 to 12 Sep. Given 20 jobs a day, that is roughly 60
   jobs never looked at.
2. **Upgrade or raise the cap** at https://console.apify.com/billing. The paid tier starts
   at $39/month, which is far more than this workload needs. `tasks/job-hunt-routine.md`
   estimates the actual scraping cost at about $0.02 a day at 20 jobs, $3 a month at 100,
   so the free $5 should be ample. Worth checking the usage breakdown at
   https://console.apify.com/billing/usage before paying anything, because $6.05 in one
   cycle is higher than this routine alone should have produced. Something else on that
   account may be burning credit, or `enrichDetails` may cost more per job than the
   estimate in the routine doc assumed.

Nothing to do in the repo. No queries, credentials or permissions are broken. The token
authenticates fine, the network path to `api.apify.com` is open, and the account details
came back normally. It is purely the usage cap.

## Queries attempted (all 4 failed identically)

1. youtube channel manager
2. youtube channel audit
3. youtube consultant
4. video content manager

## BID

None. No data pulled.

## BORDERLINE

None. No data pulled.

## NO

None. No data pulled.

---

## Note on the prompt mismatch (unchanged from previous runs)

The stored routine prompt at claude.ai/code/routines still disagrees with
`tasks/job-hunt-routine.md`. The stored version asks for the queries `youtube editor`,
`youtube thumbnail` and `youtube strategy` at `maxResults: 5`, has no editing/thumbnail
kill rule, and asks for the NO tier as a bare table. The repo file wins per its own
instruction and per the stored prompt's own STEP 1, so this run used the file's four
queries at `maxResults: 10`.

Fixing the repo file does not fix the routine. The stored prompt has to be replaced by
hand at https://claude.ai/code/routines/trig_017tZTNWZ8VymdH5tasfD31J or the scheduler
keeps serving the old one.
