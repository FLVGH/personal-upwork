# ARCHIVE — the n8n auto-bid plan (superseded 2026-07-15)

Kept for the record. This is the plan exactly as it stood in `tasks/todo.md` before
Claude Code Routines replaced it. Nothing here is wrong; it was the right build for
the tools available in June 2026.

**Superseded by:** `tasks/job-hunt-routine.md`. Routines + the Claude phone app do the
same job with no n8n, no Telegram bot, no server, and no condensed system prompt.

**What survived the switch, unchanged:**
- Black Falcon as the actor (`blackfalcondata/upwork-scraper`)
- The 8 bucket queries, YouTube-weighted
- The door-check, including "missing applicant field = SKIP, never 0"
- Manual copy-paste into Upwork. Nothing auto-submits.
- Every honest flag below (scraping breaks on markup changes, no official API)

**Why it's still worth reading:** the v1-to-v5 design arc in `sessions/2026-06-23.md`
sec. 4 is what produced the phone-first requirement, and that requirement is exactly
what Routines satisfies. The thinking was right before the tool existed.

---

## Original text below this line

### n8n Workflow

**Goal:** Apify scrapes Upwork -> n8n filters (door-check) -> Telegram alert ->
tap Draft -> Claude writes 5-slot proposal -> refine loop -> I copy + paste into
Upwork myself. Nothing auto-submits.

Flow:
```
Apify (Upwork scraper)
  -> n8n cron (every 2-3 hrs)
  -> dedupe vs Google Sheet (or actor incremental mode)
  -> door-check filter (payment verified, <5 proposals, budget present, recency)
  -> Telegram message w/ [Draft] [Skip] buttons
  -> tap Draft -> Claude API (5-slot, bio + rules baked in)
  -> proposal back in Telegram -> [Refine] loop
  -> I copy, paste into Upwork
```

**Honest flags:**
- Upwork scraping breaks when they change markup. Maintenance, not set-and-forget.
- Proposals-count is the make-or-break field. Not all actors return it.
- No official Upwork API. Apify is the route.

**Build phases + time (assuming comfortable in n8n):**

| Phase | Platform | Hours |
|-------|----------|-------|
| 0. Keys/accounts (Apify, BotFather, Anthropic, Sheet) | setup | ~0.75 |
| 1. Apify scraper — TEST FIRST | Apify | ~2 |
| 2. Ingestion + dedupe | n8n | ~3 |
| 3. Door-check filter + Telegram push | n8n + Telegram | ~3 |
| 4. Claude drafting | n8n + Anthropic | ~6 |
| 5. Refine loop | n8n + Telegram | ~4 |
| 6. Hardening | n8n | ~2 |

- MVP (Phases 0-3, alerts only, no AI) = ~8-9 hrs / 2 evenings
- Full (with drafting + refine) = ~17-20 hrs / 4-5 evenings
- Ship MVP first, prove the scraper holds, then add Claude.

**Open decisions:** dedupe store = Google Sheets (default); Claude model =
Sonnet 4.6 for drafts; n8n = self-host; cron = every 2 hrs, 8am-10pm.

---

#### Phase 1 — Apify actor test (DO THIS FIRST)

Testing all 3 actors on the SAME search, no filters, full output, limit 4.
Make-or-break field = applicant/proposals count (the `<5` door-check filter).

Candidates:
- **#1 Black Falcon** ($0.001/job, applicant count FREE in base, raw URL, incremental
  dedup, Telegram built in, but no client avg $/hr; newest — 3 reviews)
- **#2 Upwork Vibe** ($0.0035 + addons, applicant count via addon, HAS client avgRate
  for bid math, no raw URL, explicit n8n example; 20 reviews)
- **#3 Neatrat** ($0.0035, applicant count undocumented, raw URL, authenticated by
  default, allowedApplicantCountries filter — good for India; most proven, 31 reviews)

Test inputs (query "youtube channel manager", limit 4):

#1 Black Falcon:
```json
{ "query": "youtube channel manager", "maxResults": 4 }
```

#2 Upwork Vibe (addons ON to see everything):
```json
{
  "limit": 4,
  "includeKeywords.keywords": ["youtube channel manager"],
  "includeKeywords.matchTitle": true,
  "includeKeywords.matchDescription": true,
  "addons.enableClientActivity": true,
  "addons.enableClientDetails": true
}
```

#3 Neatrat:
```json
{ "query": "youtube channel manager", "resultsPerPage": 4 }
```

Score each run on: (1) applicant count present + not null, (2) payment verified,
(3) budget/hourly, (4) client avg $/hr, (5) returned YT jobs fast, no errors.
Field #1 is the tiebreaker. Paste outputs back to Claude to pick the winner.

Recommendation before testing: #1 primary (only one with free applicant count +
cheapest + incremental), #3 backup (most proven + country filter), #2 only if
client avgRate for bid math proves worth the extra cost.

#### n8n build tasks (after actor picked)
- [x] Phase 1 ACTOR PICKED: **Black Falcon** primary, Neatrat backup. All 3 tested 2026-06-23, see research/apify-actor-test.md (Black Falcon 4/5, Neatrat 5/5, Upwork Vibe 2.5/5 — rejected, no applicant count + stale jobs)
**MODEL LOCKED 2026-06-23: on-demand, PHONE-FIRST, NOT cron.** Goal = faster proposals,
quickness key. n8n does the finding 50% (surface + summarize job). Flavio taps to trigger
Claude, edits the draft, submits from the Upwork MOBILE app. Whole loop on the phone —
so drafting runs INSIDE n8n (Anthropic API) and replies to Telegram, NOT in the desktop
Claude chat. Human touch = Flavio edits the returned draft before pasting.
FOCUS: YouTube (Bucket B) over Sales. Profile work paused by Flavio's call.
Killed: cron, Google Sheet dedup, refine-loop node. Anthropic key BACK IN (drafting).
Cost: Apify ~$0.05/trigger + Claude ~$0.01-0.03/draft. Negligible.

Lean loop: tap Telegram /jobs -> n8n Telegram Trigger -> Apify Black Falcon (8 bucket
queries) -> door-check filter -> reply per job (title + summary + link + applicant count +
[Draft] button) -> tap Draft -> n8n Claude node (5-slot + playbook rules baked in) ->
draft text to Telegram -> Flavio edits + pastes into Upwork mobile.

- [ ] Phase 0 (keys): Telegram bot via @BotFather (token) + Anthropic key. Apify token works.
      NO Google Sheet.
- [ ] Phase 0 (infra): n8n running with a public webhook URL (self-host + tunnel) so the
      Telegram Trigger can reach it when tapped.
- [ ] Phase 1: save Black Falcon as a reusable Apify task with the 8 bucket queries
      (YouTube-weighted: more YT queries than closing per focus), maxResults tuned.
- [ ] Phase 2: n8n workflow = Telegram Trigger -> Apify -> door-check filter
      (paymentVerified, totalApplicants<5 AND present, budget present) -> reply w/ [Draft] btn.
      Filter MUST treat missing applicant field as skip (Neatrat dropped it twice; BF safe).
- [ ] Phase 3: Claude drafting node — bake CLAUDE.md 5-slot rules + Flavio's proof points
      into the system prompt (the big quality risk; full playbook context lives in chat,
      must be condensed for n8n). Model: Sonnet 4.6.
- [ ] Phase 4: end-to-end test — tap /jobs -> pick job -> tap Draft -> get usable proposal.
- [ ] Confirm daily job application volume (~10 bids/week sustainable, YouTube-weighted)
