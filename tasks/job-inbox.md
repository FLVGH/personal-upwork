# RUN FAILED - 29 Aug 2026 (third day running)

Ran 04:34 UTC. 4 queries attempted, 0 pulled, 0 unique. BID 0 - BORDERLINE 0 - NO 0.

**Third dead run. This one is on the account, not the code, and it will not clear until 13 Sep.**

All four queries returned HTTP 403, `platform-feature-disabled`, "Monthly usage hard limit
exceeded". Retried per the spec, same result. The token is fine, the network allowlist is fine,
the proxy is fine. `users/me` authenticates and returns the account without complaint.

```
account      fmyt99 (Flavio Mendes), FREE plan
cap          $5.00 / month
used         $6.05          <- up from $6.05 yesterday, nothing new burned
cycle        13 Aug 2026 -> 12 Sep 2026, resets 13 Sep
```

You are $1.05 over a hard $5 ceiling with **fourteen days left in the cycle**. Nothing has moved
since the 27th. Three runs have now produced no jobs, and every run between now and 13 Sep will
produce no jobs unless the token changes.

## The fix is one field, five minutes

1. **New free Apify account, swap the token.** Sign up, take the token from
   console.apify.com/settings/integrations, replace `APIFY_TOKEN` in the Default environment at
   claude.ai/code/environments. Another $5 of credit, which at reduced volume covers the rest of
   the month. Costs nothing.
2. **Drop `maxResults` from 10 to 5** in the routine prompt at claude.ai/code/routines. Roughly
   halves the burn to about $2.50 a month, which fits inside the free tier instead of blowing
   through it in two weeks. Do this one whatever else you decide, or the same thing happens again
   around 20 Sep. Note that the stored prompt is already asking for 5, so if you replace it with
   the current spec (see below) that number needs to stay at 5, not go back to 10.
3. **Pause the routine until 13 Sep** and bid from the manual search URLs in CLAUDE.md sec. 5.
   Every firing until then costs a Claude run and returns nothing.
4. Upgrade to Starter, $39/mo. Not worth it against $3/mo of actual scraping.

1 plus 2 is the fix. If you are not going to do either today, do 3, because the routine is
currently burning a daily run to write this same file.

## What to do with the fourteen days

The scraper being down is not the bottleneck this week. The sent-proposals log in
`tasks/context.md` still shows **eight proposals sitting at SEND-READY with no sent date**, the
oldest from 26 Aug, plus one BLANK LEFT that needs a viral-video breakdown filled in before it
can go anywhere. That is nine bids of pipeline already written and sitting still. Three days of a
working scraper would not have found you nine.

Flagged on 28 Aug, unchanged today. If they went out and the log was never updated, fix the log
so this stops being the first thing every dead run points at.

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

The two from 27 Aug are the freshest and the most likely to still be open. Physician channel was
5 hours old at capture with 5 to 10 proposals. B2B SaaS was already at 20 to 50 and needed a
boost, so that one is probably gone.

---

# Carried forward: 26 Aug pull, now THREE DAYS STALE

Not re-checked, because nothing could be pulled. Assume applicant counts have doubled and some of
these are closed. Open the link before spending a connect. The full 30-job version with every
entry is preserved in git at commit `e711a68`, and yesterday's compressed version at `bfb9be9`.

Only the ones still worth opening are repeated here. Everything else from that pull was skipped
on merit and three days have not improved it.

### 6. Creator & Affiliate Operations Manager, TRYBE
- 2 applicants at capture - hourly, no rate given - $8,178 spent, 10 reviews, 4.83 stars, US
- Link: https://www.upwork.com/jobs/~022092548299949220480
- What they want: Own a men's skincare brand's creator and affiliate program end to end. 70%
  creator ops, 30% keeping content organised from raw asset to usable file.
- Still the best untouched job from that pull. Not YouTube, but it is the Infivision
  team-running half plus the outreach half in one seat, which is closer to your real proof than
  most of what the YouTube queries return. Open at $22 or ask their rate. No boost. TRYBE is a
  tool you have not touched, so claim the pattern and name HubSpot and GoHighLevel.
- Three days at two applicants means that number has moved. Check before drafting.

### 16. Gaming Content Strategist, Australian streamer
- Applicant count missing - hourly, no rate given - $0 spent, 0 reviews, Australia
- Link: https://www.upwork.com/jobs/~022092553147739494009
- What they want: Cross-platform growth and distribution strategy. They already have an editor,
  so the production is carved out and this is strategy and analytics only.
- Scored NO on 26 Aug for the missing applicant count, no budget and no client history. Worth
  opening anyway because strategy with the editing removed is the exact shape you want and it is
  rare. If the live page now shows a budget and a real client, it is a bid. Still $0 spent, skip.

### 7. UGC TikTok Outreach + Content Creation
- 2 applicants at capture - $10 to $20 per hour - $39,288 spent, 88 reviews, 4.91 stars, UK
- Link: https://www.upwork.com/jobs/~022092504071145276793
- What they want: Recruit TikTok and Instagram creators by DM for a language-learning startup,
  onboard them, then help them produce the short-form themselves.
- Strongest client of the pull. The outreach half is your day job. Only worth it if creator
  recruitment can be scoped out on the call, and the post says outright they do not want someone
  who only sends messages, so that is arguing with the ad. Bid $19, not the $20 ceiling.

---

## Note on the routine spec, flagged 26, 27, 28 Aug, still true

The stored scheduler prompt is still the old one: `youtube editor`, `youtube thumbnail` and
`youtube strategy` queries, and a bare table for the NO tier. This run followed
`tasks/job-hunt-routine.md` instead, since both the file and the stored prompt agree the file
wins. That means the diversified queries, the editing rule, and six lines on every tier.

The stored prompt at claude.ai/code/routines still has to be replaced by hand. Fixing the repo
file does not fix the scheduler. While you are in there, that is also where `maxResults` gets
changed, so fix #2 above and this can be the same edit.

## Note on pushing to main, from 28 Aug

`git push -u origin main` was rejected twice as "non-fast-forward, branch tip is behind its
remote counterpart" when remote main was at the exact parent of the local commit.
`git push origin HEAD:refs/heads/main` went through first try. The explicit refspec is the form
to use, and the six days of failed pushes in the run history may have been this all along rather
than a permissions problem.
