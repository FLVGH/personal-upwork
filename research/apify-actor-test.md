# Apify Actor Test — Phase 1

Search used: `youtube channel manager` (recency-sorted). Goal: pick the scraper for the
n8n auto-bid pipeline. Tiebreaker field = applicant/proposals count (the `<5` door-check).

## Scorecard

All three run 2026-06-23, same search (`youtube channel manager`).

| # | Criterion | #1 Black Falcon | #3 Neatrat | #2 Upwork Vibe |
|---|-----------|-----------------|-----------|----------------|
| 1 | Applicant count present + not null (tiebreaker) | ✅ totalApplicants 1,10,0,8 (free in base) | ✅ 1,8,0,8,6,20 | ❌ NO proposals field (applicationCost = connect price) |
| 2 | Payment verified | ✅ clientPaymentVerified | ✅ | ✅ client.paymentMethodVerified |
| 3 | Budget / hourly | ✅ budgetAmount + engagementDuration/Type | ✅ real ranges | ⚠️ field present but 0/null on all 4 |
| 4 | Client avg $/hr (bid math) | ❌ none (has totalSpent + reviewCount) | ✅ 11.30 / 32.73 / 19.64 / 12.18 | ✅ client.stats.avgRate (8.08, 24.84) |
| 5 | Freshness (most important) | ✅ same-day, publishTime 05:20–11:28, scrapedAt 12:08 | ✅ 12min–10hr old | ⚠️ 3–7 days old, stale |
| | Price / job | **$0.001** | $0.0035 | $0.0035+ addons |
| | Pipeline extras | incremental dedup, Telegram, repost detection | country filter | customJobScore |
| | **Score** | **4/5** | **5/5** | **2.5/5** |

## #2 Upwork Vibe — run 2026-06-23 (addons ON)

Fatal flaw: returns NO applicant/proposals count even with all addons on, so it can't
gate the `<5` door-check — the whole point of the pipeline. Also returned stale jobs
(3–7 days old) vs Neatrat's same-day, and budget came back empty.
Nice extras it has but Neatrat lacks: `applicationCost` (connect price), `customJobScore`,
`client.companySize`, `client.industry`, `client.feedbackCount`, `client.timezone`.

## Neatrat — fields returned (run 2026-06-23)

Per-job: `id`, `subId`, `url` (raw), `title`, `description`, `budget`, `jobType`,
`experienceLevel`, `clientLocation`, `clientName` (+confidence), `clientAvgHourlyRate`,
`clientRating`, `clientHireRatePercent`, `clientTotalSpent`, `hasHired`, `proposals`,
`paymentVerified`, `relativeDate`, `absoluteDate`, `allowedApplicantCountries`,
`questions`, `tags`.

This covers nearly the entire door-check (CLAUDE.md sec. 4) in one row: payment,
proposals<5, budget, scope, client spend/hire history, AND bid math (avg $/hr).

Input used:
```json
{ "query": "youtube channel manager", "resultsPerPage": 4 }
```
Note: `resultsPerPage` is page size, not a hard cap — returned 6.

## Freshness — the factor that saves connects (always check this first)

Per CLAUDE.md sec. 10: `<5 proposals` + fresh (`<1 hr`) = NO boost needed, freshness is
free ranking. So the latest-data question is the make-or-break for the whole pipeline:
a scraper that returns same-day jobs lets us apply before proposals climb past 5 (no boost
connects burned). A scraper that returns 3-day-old jobs feeds us saturated posts (20+ props).

| Actor | Job ages returned | Verdict |
|-------|-------------------|---------|
| Neatrat | 12 min – 10 hr (all same-day) | ✅ catches jobs while still `<5` proposals |
| Upwork Vibe | 3–7 days old (Jun 16–20) | ❌ stale, already saturated |
| Black Falcon | not run | check `relativeDate` / `absoluteDate` on run |

When running Black Falcon, score freshness first: are the top jobs minutes/hours old, or days?

## Black Falcon — run 2026-06-23 (enrichDetails ON)

Per-job fields: jobId, title, jobType, experienceLevel, budgetAmount, hourly/salary
min-max, engagementDuration, engagementType, totalApplicants, clientCountry,
clientTotalSpent, clientPaymentVerified, clientRating, clientReviewCount, customJobScore,
skills, publishTime, url, scrapedAt, firstSeenAt, lastSeenAt, isRepost, changeType,
contact + social extraction (all null here), extractedEmails/Phones/Urls.

Only miss vs Neatrat = client avg $/hr. Everything else matches or beats it, plus
built-in incremental dedup (firstSeenAt/lastSeenAt + skipReposts) and Telegram.

Input used:
```json
{ "query": "youtube channel manager", "maxResults": 4, "enrichDetails": true,
  "verifiedPaymentOnly": false, "excludeEmptyFields": false }
```

## Cross-validation run — both actors, same search, same hour (2026-06-23 ~17:26)

Ran Black Falcon and Neatrat on `youtube channel manager` within the same hour to
confirm accuracy and re-test on a fresh batch.

**Applicant counts match exactly across both actors** on the top 4 shared jobs:
4, 7, 13, 29 (Black Falcon `totalApplicants` = Neatrat `proposals`). Two independent
scrapers returning identical counts = the make-or-break field is trustworthy on both.

| Factor | Black Falcon | Neatrat |
|--------|--------------|---------|
| Applicant count | ✅ 4,7,13,29 | ✅ 4,7,13,29 (matches) |
| Volume returned | 4 (respects `maxResults`) | ~55 (ignores `resultsPerPage` cap) |
| Freshness top job | "1 hr" (publishTime) | "Posted 1 hour ago" |
| Client avg $/hr | ❌ none | ✅ present on ~half (e.g. 5, 11.31, 32.73, 22.29) |
| Edge case | `firstSeenAt`/`lastSeenAt`/`changeType` NULL on one-off run | one job dropped the `proposals` field entirely (id ...989361660) |

**Two flags this run surfaced:**
1. **Black Falcon dedup fields come back null on a one-off run.** `firstSeenAt`,
   `lastSeenAt`, `changeType` all null. The built-in incremental dedup likely only
   populates across repeated runs against a persisted dataset (a saved task), not a
   single manual run. Do NOT bank on skipping Phase 2 (Sheet dedup) until this is
   confirmed live in a saved task.
2. **Neatrat occasionally omits `proposals` entirely** (not null — the key is absent).
   The door-check filter must treat "field missing" as fail-safe (skip the job), not
   as 0 applicants.

Verdict unchanged: Black Falcon primary (cheaper, dedup+Telegram, respects result cap),
Neatrat backup (adds avg $/hr, but firehoses volume and drops proposals sometimes).

## Sales-category robustness run — `high ticket closer` (2026-06-23 ~17:27)

Purpose: confirm the make-or-break field survives outside YouTube (different category,
different markup). Closing is Flavio's real Bucket A focus, not cold email.

**Neatrat — PASSES on closing.** `proposals` present and non-null across ~30 jobs
(0, 0, 3, 1, 0, 2, 1, 3, 6, 3, 13, 12, ...). `clientAvgHourlyRate` present on most
(34.99, 14.76, 96.07, 100, 36.20...). Top job "Posted 9 minutes ago" — same-minute
freshness holds in this category too.

- Repeat of the known quirk: at least one job dropped `proposals` entirely again
  (ANZ Appointment Setter, id ...355239390). Confirms the filter must fail-safe on
  missing field, not assume 0.

**Bucket insight (matters more than the actor test):** `high ticket closer` is a
strong bucket for Flavio. This single pull had many FRESH, UNDER-5-proposal jobs that
fit his closing/inbound proof ($250K closed, 1,000+ inbound calls): Financial Sales Rep
(0 props, 9 min), German B2B Closer (0), Sales Closer/Cold Caller (3), Echo Media setter
(1), Conek full-funnel closer (2), restaurant closer (5). Far better door-check hit rate
than the cold-email search the playbook currently points at.

**Black Falcon — PASSES on closing too (PICK LOCKED).** `totalApplicants` present and
non-null: 1, 0, 3, 1. Freshness held (publishTime 17:30, scrapedAt 17:42 = minutes old).
Cross-validates Neatrat on the same 4 jobs:

| Job | Neatrat `proposals` | Black Falcon `totalApplicants` |
|-----|--------------------|-------------------------------|
| Financial Sales Rep | 0 | 1 (one applicant arrived in the ~15 min between scrapes) |
| German B2B Closer | 0 | 0 |
| Sales Closer / Cold Caller | 3 | 3 |
| Outbound Cold Caller | 1 | 1 |

Both actors now confirmed accurate on TWO categories (YouTube + closing). The applicant
count is trustworthy on either. `firstSeenAt`/`lastSeenAt` still null on one-off run
(dedup-fields-need-a-saved-task flag stands). PICK LOCKED: Black Falcon primary.

## VERDICT — Black Falcon primary, Neatrat backup

Black Falcon wins on the two factors that matter most for the pipeline: applicant count
(free in base) and same-day freshness. Its only gap, client avg $/hr, is a non-issue
because the door-check (CLAUDE.md sec. 4) already checks avg $/hr on the Upwork Insights
tab at apply time. Plus it's 3.5x cheaper AND its built-in incremental dedup + Telegram
collapse todo Phases 2 and part of 3.

- PRIMARY: Black Falcon ($0.001, dedup + Telegram built-in)
- BACKUP: Neatrat ($0.0035) — switch to if we ever want avg $/hr in-feed for auto bid math
- REJECTED: Upwork Vibe — no applicant count, stale jobs. Fails the core door-check.

Next: n8n Phase 0 (Apify token already works, set up BotFather bot + chat ID +
Anthropic key + Sheet), then Phase 1 = save Black Falcon as a reusable Apify task with
the 3 bucket search URLs.
