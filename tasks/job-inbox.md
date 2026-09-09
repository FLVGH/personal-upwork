# Job Inbox - 9 Sep 2026

Ran 04:33 UTC. 4 queries, 0 jobs pulled, 0 after dedupe.
BID 0 - BORDERLINE 0 - NO 0

**The run did not fetch anything. This is not an empty market, it is a broken pipe.**

All 4 queries returned HTTP 403 from Apify:

```
{"error":{"type":"platform-feature-disabled","message":"Monthly usage hard limit exceeded"}}
```

Each query was retried once. Same result on all four:
- youtube channel manager - FAILED
- youtube editor - FAILED
- youtube thumbnail - FAILED
- youtube strategy - FAILED

## What is actually wrong

The token works. The account is fine. The credit is gone.

- Apify account: fmyt99, plan FREE, $5 of usage credit a month
- Used this cycle: $6.05 against a $5 cap
- Billing cycle: 13 Aug 2026 to 12 Sep 2026, resets 13 Sep 2026 00:00 UTC

So the scraper is locked out for the next 3 days and every daily run between now and 13 Sep will die the same way unless something changes.

## Fix, pick one

1. Do nothing. It comes back on its own 13 Sep. You lose the 10, 11 and 12 Sep inboxes.
2. Upgrade Apify at https://console.apify.com/billing. Starter is $39/mo, which is a lot of money for a job list that the routine doc costs out at about $3/mo of actual scraping.
3. Cheaper and probably right: the $5 free credit is being burned faster than 20 jobs a day should burn it. `enrichDetails: true` is the expensive flag, it fetches each job's full detail page. Worth checking the run history at https://console.apify.com/actors/runs before paying for more of the same.

Nothing was scored today because nothing was pulled. No jobs were invented to fill the file.
