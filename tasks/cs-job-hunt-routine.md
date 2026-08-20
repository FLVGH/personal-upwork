# CS Job Hunt Routine (Indeed + Glassdoor) — setup + prompt

Same pattern as `tasks/job-hunt-routine.md` (the Upwork YouTube routine), reused for
Aishwarya Ahir's Customer Success job search on Indeed and Glassdoor. Full-time job
boards, not gig platforms, so the door-check signals are different — see below.

Candidate profile: `profiles/aishwarya.md`.

---

## Why the door-check is different from the Upwork routine

Upwork's door-check leans on gig-marketplace signals: proposal count, connects,
boost, client's Insights $/hr paid. None of that exists on Indeed/Glassdoor. Instead:

| Upwork signal | CS routine equivalent |
|---|---|
| Payment verified | Employer has a rating / review count (real, not a shell posting) |
| Under 5 proposals | N/A — no applicant count on Indeed. Use posted-recency instead |
| Client 1+ hires | Employer ratingsCount > 0 (Indeed) or rating present (Glassdoor) |
| Budget visible | Salary listed (nice-to-have, not a hard fail — most India CS listings hide it) |
| Not an agency post | Not a staffing/BPO reseller post ("multiple clients", vague employer) |
| Client active <24h | Posted within the freshness window (14 days) |

No bid math, no boost math, no connects. This routine only finds and scores —
applying still means her tailoring a resume/cover note per role, same "nothing
auto-submits" rule as the Upwork side.

---

## Apify actors (tested and confirmed working 20 Aug 2026)

Both run on the same `APIFY_TOKEN` and `api.apify.com` allowlist already set up for
the Upwork routine — no new environment config needed.

### Indeed — `valig/indeed-jobs-scraper`
$0.1/1K jobs. 25K+ users, 5.0 rating.

```
curl -s -X POST "https://api.apify.com/v2/acts/valig~indeed-jobs-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"country":"in","title":"QUERY_HERE","location":"India","limit":10,"datePosted":"14"}'
```

Returns: employer name, rating, ratingsCount, employeesCount, revenue, salary
(min/max, often null in India), location (city/lat-long), remote/full-time
attributes, full description text, datePublished. No applicant count field.

### Glassdoor — `valig/glassdoor-jobs-scraper`
$0.4/1K jobs. 7.8K+ users, 5.0 rating.

```
curl -s -X POST "https://api.apify.com/v2/acts/valig~glassdoor-jobs-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"keywords":"QUERY_HERE","location":"Pune, Maharashtra","limit":10,"daysOld":14}'
```

**Bug caught during setup (20 Aug 2026): do not pass `"location":"India"` to this
actor.** It silently mis-geocodes the bare country name to Indiana, USA and returns
fence estimators and CDL drivers in Indianapolis — no error, no India jobs, nothing
that looks wrong until you read the titles. A real city+state string
(`"Pune, Maharashtra"`) resolves correctly and returns actual India listings. Indeed's
actor does not have this problem — `country:"in"` + `location:"India"` works fine
there because country is a separate field.

Returns: employer rating, easyApply flag, location, pay (often null in India),
ageInDays, full HTML description. Supports `minRating` and `remoteWorkType` filters
if the BID tier needs tightening later.

---

## Queries (broad — CV is a template, not a Maersk-specific target)

1. `customer success`
2. `customer success associate`
3. `account management client relationship`

Location: Indeed gets `"India"` (broad — its actor takes country as a separate
field so this works and returns real India-wide results). Glassdoor gets
`"Pune, Maharashtra"` (must be a real city/state string — see the actor note above).
Neither actor cleanly supports "Pune OR remote anywhere in India" as one query, so
the routine sorts that out in scoring instead: keep a job if its location is
Pune/Maharashtra OR its attributes/description say remote/WFH. Drop it (NO tier)
only for the location reason if it's neither.

That's 3 queries × 2 platforms = 6 calls, ~20-60 jobs before dedupe depending on
`limit`. Start at `limit: 10` per call like the Upwork routine started small.

---

## Role-fit filter (tied to profiles/aishwarya.md)

Keep: Customer Success Associate/Executive/Specialist, Account Manager / Client
Relationship Manager (junior-mid, not 8+ yrs senior), Client Servicing/Relationship
Officer — BFSI or travel/hospitality preferred but not exclusive.

Drop to NO: pure outbound telesales/collections roles (different skillset than her
CV), senior/leadership titles requiring 8+ years, roles requiring skills/tools not
in her CV (e.g. specific enterprise CRMs she's never touched) unless the routine
flags it as learnable-on-the-job in the "why it fits" line.

---

## Tiers (same score-don't-filter rule as the Upwork routine)

  BID         — employer has a real rating/review history, posted within 14 days,
                location is Pune or explicitly remote/WFH India, role matches her
                actual proof (CS/account mgmt/relationship mgmt, not telesales),
                not a staffing/BPO reseller post
  BORDERLINE  — fails exactly one of the above, fit is otherwise strong
  NO          — everything else

Never invent a salary floor to score against — she hasn't given one yet (see
`profiles/aishwarya.md` unknowns). Salary, where listed, gets reported plainly;
absence of salary is not itself a fail.

---

## Inbox format

Same shape as `tasks/job-inbox.md`, written to `tasks/cs-job-inbox.md`. Overwrite
completely each run, number continuously across tiers, never drop the link.

```
# CS Job Inbox — <date>

Ran <time> UTC. 3 queries × 2 platforms, <N> jobs pulled, <D> after dedupe.
BID <n> · BORDERLINE <n> · NO <n>

## BID

### 1. <title> — <employer>
- Platform: Indeed / Glassdoor
- Location: <city or "Remote, India">
- Employer: <rating>/5, <ratingsCount> reviews, <employeesCount> employees
- Salary: <range or "not listed">
- Posted: <how long ago>
- Link: <url>
- Why it fits: <tie to her actual CV proof — CRM tool, KPI type, industry match>
- Watch out: <only if a real flag>

## BORDERLINE

### N. <title> — <employer> — fails: <the one check>
- <compact one-line: platform, location, employer rating, posted, link>
- Worth it anyway if: <concrete condition>

## NO

### N. <title> — <employer> — killed by: <reason>
- <platform>, <location>, <one-line summary of the role>, <link>
```

Dedupe across the two platforms and across queries by employer + title match (no
shared job ID between Indeed and Glassdoor like Upwork's jobId).

---

## First real pull

Run once by hand to prove the pipeline end-to-end before scheduling it — same
approach as the Upwork routine's manual CSV build on day 1. Result:
`tasks/cs-job-inbox.md`.

---

## Scheduling

Live as of 20 Aug 2026, same account/environment/`APIFY_TOKEN` as the Upwork
routine, no new setup steps needed — fires a fresh session each day, reads
`tasks/cs-job-hunt-routine.md` + `profiles/aishwarya.md`, writes and pushes
`tasks/cs-job-inbox.md`.

- Name: CS job inbox (Indeed + Glassdoor)
- ID: `trig_01PJSEVsU4xn79hfK6vZVQSX`
- Schedule: `45 4 * * *` UTC, which is 10:15am IST daily (15 min after the Upwork
  routine, so they don't collide)
- Edit or pause it: https://claude.ai/code/routines/trig_01PJSEVsU4xn79hfK6vZVQSX

First fire: tomorrow morning. Today's inbox (`tasks/cs-job-inbox.md`) was built by
hand in this session to validate the pipeline first.

---

## Run history

| Date | Pulled | BID | Notes |
|------|--------|-----|-------|
| 20 Aug 2026 | 60 (manual, 3 queries × 2 platforms) | 8/43 after dedupe | First real pull, see `tasks/cs-job-inbox.md`. Caught and fixed the Glassdoor `location:"India"` → Indiana, USA bug before it went into a scheduled run. Glassdoor returns country-level location only (no city), so BID/BORDERLINE entries from that platform are "searched from Pune" not "confirmed Pune" — flagged in the inbox. |
