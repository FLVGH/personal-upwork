# RUN FAILED - 1 Sep 2026 (sixth day running)

Ran 04:34 UTC. 4 queries attempted, 0 pulled, 0 unique. BID 0 - BORDERLINE 0 - NO 0.

**Sixth identical failure.** All four queries run and retried this time, all four HTTP 403,
`platform-feature-disabled`, "Monthly usage hard limit exceeded". Token authenticates, network
allowlist fine, proxy fine. This is the Apify billing cap, not the setup.

```
account   FREE plan (fmyt99)
cap       $5.00 / month
used      $6.05     <- unchanged since 28 Aug, nothing new burned
cycle     13 Aug 2026 -> 12 Sep 2026, resets 13 Sep
```

Twelve days left in the cycle. Six Claude runs now spent writing this same file.

## Pause the routine

Said this on the 31st, saying it again. Pause it at claude.ai/code/routines. It cannot succeed
before 13 Sep and every firing costs a run to tell you that. Turn it back on after the fix or
after the reset.

The fix, unchanged, five minutes:

1. **New free Apify account, swap the token.** console.apify.com/settings/integrations, then
   replace `APIFY_TOKEN` in the Default environment at claude.ai/code/environments. Another $5
   of credit, costs nothing.
2. **Set `maxResults` to 5** in the routine prompt at claude.ai/code/routines. Halves the burn to
   roughly $2.50/mo, which fits the free tier. Do this either way or the cap blows again around
   20 Sep. The stored prompt already says 5, so if you paste the repo spec over it, keep 5 rather
   than the 10 the repo file asks for.

## The bottleneck is still not the scraper

`tasks/context.md` still shows **nine proposals at SEND-READY with no sent date** and one BLANK
LEFT, oldest 26 Aug. Flagged 28, 29, 30, 31 Aug and now 1 Sep, unchanged. Ten bids of written
pipeline sitting still. Six working scraper days would not have found you ten.

If they went out and the log was never updated, fix the log so it stops being the headline of
every dead run.

| Status | Job | File |
|---|---|---|
| BLANK LEFT | 666 Media, Content Strategist | `outreach/666-media-content-strategist.md` |
| SEND-READY | 666 Media, Channel Manager | `outreach/666-media-channel-manager.md` |
| SEND-READY | Faceless Growth / CSM, $3,500/mo | `outreach/faceless-csm-client-success.md` |
| SEND-READY | Gestionnaire de chaîne YouTube | `outreach/gestionnaire-chaine-youtube.md` |
| SEND-READY | YouTube Strategy & Analytics, Bangladesh | `outreach/youtube-strategy-analytics-expert.md` |
| SEND-READY | Yoga / breathwork channel, Thailand | `outreach/youtube-coaching-yoga-breathwork.md` |
| SEND-READY | Growth & Monetization, Education / Psychology | `outreach/youtube-growth-monetization-education.md` |
| SEND-READY | Channel SEO / AEO | `outreach/youtube-seo-aeo-optimization.md` |
| SEND-READY | Physician / lifestyle channel, Washington | `outreach/youtube-growth-physician-lifestyle.md` |
| SEND-READY | B2B SaaS Channel Setup, Pleasanton | `outreach/b2b-saas-channel-setup.md` |

Physician / lifestyle (27 Aug, 5 hours old at capture, 5 to 10 proposals) was the freshest.
Five days on it and it is a coin flip whether it is still open. Open the link before spending a
connect.

While the scraper is down, the manual search URLs in CLAUDE.md sec. 5 still work and cost
nothing. Ten minutes on a phone gets the same list this routine used to.

---

# Carried forward: 26 Aug pull, now SIX DAYS STALE

Last honest data. Applicant counts have moved, some are closed. Open the link before spending a
connect. Full 30-job version preserved in git at `e711a68`.

At six days these are close to worthless. Kept only because nothing has replaced them.

- **6. Creator & Affiliate Operations Manager, TRYBE** - 2 applicants at capture, hourly, $8,178
  spent / 10 reviews / 4.83 / US - https://www.upwork.com/jobs/~022092548299949220480
  Creator and affiliate program end to end for a men's skincare brand. Still the best untouched
  job of that pull: Infivision team-running plus outreach in one seat. Open at $22, no boost.
- **7. UGC TikTok Outreach + Content Creation** - 2 applicants at capture, $10 to $20/hr, $39,288
  spent / 88 reviews / 4.91 / UK - https://www.upwork.com/jobs/~022092504071145276793
  Strongest client of the pull. Outreach half is your day job. Bid $19, not the $20 ceiling.
  Only if creator recruitment gets scoped out on the call.
- **16. Gaming Content Strategist, Australian streamer** - applicant count missing, no budget,
  $0 spent / 0 reviews - https://www.upwork.com/jobs/~022092553147739494009
  Strategy with production already carved out, which is the shape you want. Still $0 spent, so
  still a skip unless the live page now shows a budget and a real client.

---

## Standing notes

**Stored routine prompt is still the old one** (flagged 26 Aug through 1 Sep): `youtube editor`,
`youtube thumbnail`, `youtube strategy`, and a bare NO table. This run followed
`tasks/job-hunt-routine.md`, since both the file and the stored prompt agree the file wins.
Replacing it is a manual edit at claude.ai/code/routines, same screen as the `maxResults` fix.

**Pushing to main:** `git push -u origin main` gets rejected as non-fast-forward even when remote
main is at the exact parent of the local commit. `git push origin HEAD:refs/heads/main` works
first try. Use the explicit refspec.
