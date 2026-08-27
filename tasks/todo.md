# To Do

**Sitting down to bid? Open `tasks/one-hour-sit.md`.** Minute-by-minute runbook for the hour.

**Setting up the YouTube outreach? Open `tasks/youtube-outreach-setup.md`** (added 27 Aug).
One ordered list: what is blocking money today, the questionnaire and competitor-research
pieces the audit deliverable is still missing, profile, proof, and the Apify decision.

## DO THESE FOUR FIRST (added 14 Aug 2026)

Everything else in this file is downstream of these. Roughly two hours total.

- [ ] Paste bio from `profiles/personal-bio-ready.md` into the Upwork profile
- [ ] Rate $16.70 to $18.50
- [ ] Three portfolio items, outcome-first titles, from `profiles/youtube-portfolio-entries.md`
- [ ] Update the stored routine prompt at claude.ai/code/routines to match
      `tasks/job-hunt-routine.md`. The repo file was fixed 19 Aug, the scheduler was not, so
      the 14 Aug run still used the old `youtube editor` / `youtube thumbnail` queries and the
      old NO table. That is the last piece of the routine problem.

Why: you are bidding against a profile that context.md calls generic AI-speak. The Wichita
client clicks it within eight seconds of being interested.

**Strategy for $10K/month: `tasks/10k-plan.md`.** Short version, hourly cannot get you there, so
Upwork is the acquisition channel and the retainer is the business. Four clients at $3,000 beats
seven at $1,500. Hire on client two, not client four.

**Audit is the wedge: `deliverables/channel-audit-template.md`.** Bid, then $300-500 audit, then
present it live, then retainer. The audit exists to manufacture a sales call, which is the part
you are already better at than the people you compete with.

## NEXT SESSION

### Profile (UN-PAUSED 29 Jun — focus shifted here, away from sales bidding)

YouTube agency (decided: separate Upwork Agency account):
- [ ] Pick agency name from `profiles/youtube-agency-bio-ready.md` (4 options listed)
- [ ] Create agency in Upwork Settings -> Agencies; add logo + tagline + overview
- [ ] Add 3 portfolio items (Infivision 100% growth, content team; 3rd = fill honest win)
- [ ] Set services + retainer rate ($1,000-$2,000/mo, audit $300-$500)
- [ ] HYBRID: keep bidding YouTube from personal Top Rated profile until agency has
      reviews + a vetted editor (see strategic flag in the bio file)

Testimonials (asked, none identified yet — sourcing list in outreach/testimonial-requests.md):
- [ ] Ask Infivision Media contact first (Template A or B)
- [ ] Scan LinkedIn for past managers/founders + content-team peers
- [ ] Fill the tracking table as you send

Personal sales profile (still pending, lower priority now):
- [ ] Paste bio from `profiles/personal-bio-ready.md` into Upwork personal profile
- [ ] Change rate from $16.70 to $18.50
- [ ] Add 3 portfolio items with outcome-first titles (see bio file)
- [ ] Record 90-second video intro (YouTube unlisted, Capte subtitles font 32)
- [ ] Fill in hours per week

### Job hunt pipeline — n8n plan SUPERSEDED 2026-07-15

**Old plan kept in full: `tasks/archive/n8n-plan-superseded-2026-07-15.md`.** Not deleted.
It was the right build for June's tools, and the actor bake-off + the v1-to-v5 design
history behind it still stand.

**Why it's parked:** Claude Code on the web + Routines now does it natively:
Claude runs itself on a schedule in Anthropic's cloud, calls Apify, applies the
door-check from CLAUDE.md sec. 4, and pushes `tasks/job-inbox.md` to this repo. Then
drafting happens in the Claude phone app with FULL repo context (no condensed prompt,
which was the big quality risk in the n8n plan).

Not needed for now: n8n, self-hosting, webhook tunnel, Telegram bot, BotFather,
Anthropic API key, Google Sheet dedup, the condensed system prompt. ~17-20 hrs of
build -> a web form.

**If Routines disappoint, the archived n8n plan is the fallback.** It's still buildable
as written; the actor, queries and door-check are shared, so nothing is wasted either way.

Kept as-is: Black Falcon actor (`blackfalcondata/upwork-scraper`), the 8 bucket
queries, the door-check, manual copy-paste into Upwork. Nothing auto-submits.

**-> Full setup steps + the routine prompt: `tasks/job-hunt-routine.md`**

- [x] Actor picked: **Black Falcon** primary, Neatrat backup (tested 2026-06-23, see
      research/apify-actor-test.md — BF 4/5, Neatrat 5/5, Upwork Vibe 2.5/5 rejected:
      no applicant count + stale jobs)
- [x] Routine prompt written (`tasks/job-hunt-routine.md`)
- [ ] Connect GitHub at claude.ai/code + install the Claude phone app
- [ ] Create the routine: paste prompt, allow `api.apify.com`, set `APIFY_TOKEN`,
      turn on unrestricted branch pushes, daily schedule
- [ ] **Run now** once and check the inbox is real (right jobs, applicant counts sane)
- [ ] End-to-end on the phone: read inbox -> "draft 3" -> refine to v2 -> paste to Upwork
- [ ] Confirm daily volume (~10 bids/week sustainable, YouTube-weighted)

**Carry-forward flags (still true):**
- Upwork scraping breaks when they change markup. Maintenance, not set-and-forget.
- Applicant count is the make-or-break field. Missing field = SKIP the job, never
  read it as 0 (Neatrat dropped it twice in testing).
- No official Upwork API. Apify is the route.
- Routines are research preview: daily run cap, and runs eat normal Claude usage.
- **Bidding still can't start until the profile is live.** The pipeline surfaces jobs
  and drafts, but a sent proposal points at an unfinished profile.

**Phase 2 (friends, NOT built):** Routine API trigger + a ~30-line Telegram bot that
forwards "this is A: <link>" into the endpoint, plus `people/a/` for A's context.
Runs bill to Flavio's account and commit under his GitHub identity. Fine as a favour,
wrong shape if it becomes a paid product. See `tasks/productization.md`.

### Bidding
- [ ] Sales Bucket A: 3 bids this week (high ticket closer, sales closer, appointment setter, remote closer)
- [ ] YouTube Bucket B: 6 bids this week
- [ ] Hannes WhatsApp: send missed-call text, one shot only

---

## Bucket targets (Phase 1)

| Bucket | Type | Jobs/week |
|--------|------|-----------|
| B | YouTube channel management | 6 |
| A | Sales / BD / cold email / lead gen | 3 |
| C | Sales ops / CRM / RevOps | 1 (opportunistic) |

---

## Sent proposals log

| Date | Job | File | Status |
|------|-----|------|--------|
| 28 May 2026 | Youtube Experto — Valencia Spain | outreach/youtube-experto-spain.md | Sent |
| 31 May 2026 | YouTube Expert — Hannes Media Agency | outreach/youtube-hannes-media-agency.md | Sent |
| 11 Jun 2026 | YouTube Client Acquisition Specialist — lead gen for strategist | outreach/youtube-strategist-leadgen.md | Sent |
