# Job Hunt Routine — setup + prompt

Claude runs itself on a timer in Anthropic's cloud, calls Apify, scores 100 YouTube
jobs, and writes `tasks/job-inbox.md` into this repo. You read the inbox on your phone
and say "draft 7".

No bot, no server, no code on your machine. Nothing runs on your laptop.

Docs: https://code.claude.com/docs/en/routines

---

## How it flows

```
   4:33 UTC daily  (10:03am IST)
          |
          v
  +----------------------+
  |  ROUTINE FIRES       |   prompt lives at claude.ai/code/routines
  +----------+-----------+   NOT in this repo. Only place it can be edited.
             |
             v
     Apify, 4 queries              -> 40 pulled, ~20-28 unique
             |
             v
     Kill editing + thumbnail work -> deliverable is a file, not a decision
             |
             v
     Door-check what is left       -> BID / BORDERLINE / NO
             |
             v
     Overwrite tasks/job-inbox.md
             |
             v
     Push to main
             |
             v
     Phone notification
             |
- - - - - - + - - - - - - - - - - - - - - - -  YOU PICK IT UP HERE
             |
             v
     Read the inbox on your phone
             |
             v
     Open each link. Still live? Applicants still under 5?     <-- the step people skip
             |                                                     numbers move in hours
             v
     "draft 3"
             |
             v
     Claude reads CLAUDE.md + tasks/context.md
     + profiles/ + past outreach/
             |
             v
     outreach/<slug>.md, send-ready
             |
             v
     "too soft, lead with the finance number"    <-- refine in the same chat
             |
             v
     Copy into Upwork. You send it. Nothing auto-submits.
             |
             v
     Logged to the sent proposals table
```

**The feedback loop.** Anything wrong at any step, say it in the same chat. It becomes a rule
in this file or in CLAUDE.md, and the next run picks it up. That is the entire maintenance model.

One exception, and it is the one that bit us: the routine prompt is stored outside the repo.
Fixing the file does not fix the routine. The stored prompt has to be replaced separately or the
scheduler keeps running whatever it was last given.

---

## Design rule: score, do not filter

Changed 13 Aug 2026. The routine used to delete jobs that failed the door-check and
show you only the survivors. It now keeps all 100 and puts a verdict on each one.

Reason: on the first real run, 23 of 25 jobs got dropped and you never saw them. You
want to make that call yourself, and you want to catch it if the scoring is wrong.

Three tiers, everything present:

| Tier | Meaning | How much detail |
|------|---------|-----------------|
| BID | passes every door-check | six lines: stats, link, what they want, verdict, why it fits, call |
| BORDERLINE | fails one thing, fit is strong | same six lines, plus what it fails and what would fix it |
| NO | fails hard | same six lines as BID (changed 25 Aug, a table was still burying jobs) |

---

## Setup — do these once

### Task 1. Turn on the Claude app on your phone

Install the Claude app, iOS or Android. Log in with the same account you use at
claude.ai. That's it. The Code tab is where the repo lives.

### Task 2. Let the cloud reach Apify

Go to https://claude.ai/code/environments and open the Default environment.

Find **Network access**. It is set to a default allowlist that does not include Apify.
Switch it to **Custom** and add one domain:

```
api.apify.com
```

Tick "also include default list of common package managers" while you're there.

Skip this and every run dies with a 403 `host_not_allowed`.

### Task 3. Give it your Apify token

Same environment page, find **Environment variables**. Add:

```
APIFY_TOKEN=<paste your token>
```

No quotes around the token. Quotes get saved as part of the value and the curl fails
with a confusing auth error.

Get the token from https://console.apify.com/settings/integrations if you need it again.

### Task 4. Let it push to main

Same area, **Permissions**. Turn on **unrestricted branch pushes** for
`FLVGH/personal-upwork`.

Without this Claude can only push to branches starting with `claude/`, and the inbox
never lands on main, which means you can't see it on your phone.

### Task 5. Tell Claude to create the routine

That's the part you don't do by hand. Ask in a session and it gets created through the
API with the prompt below.

To see or edit routines later: https://claude.ai/code/routines

---

## Daily use

Morning, on the phone. Open the Claude app, Code tab, pick `personal-upwork`, then:

> read tasks/job-inbox.md

Skim BID. Scan BORDERLINE. Glance at NO to check nothing good got buried. Then:

> draft 7

Claude reads CLAUDE.md, `tasks/context.md`, `profiles/`, and your past `outreach/`
files, then writes v1 to `outreach/<slug>.md`.

Push it around in the same chat until it's v2:

> too soft, lead with the Infivision number
> cut the second paragraph
> self-score it

You copy the final text into Upwork yourself. Nothing auto-submits.

If the scoring is wrong, say so in the same chat. "Stop marking $500 fixed as
borderline, that's a retainer" becomes a rule in this file and the routine picks it
up on the next run.

---

## Cost

20 jobs a day at Black Falcon's $0.001 per job is about $0.02 a day. At 100 it's
$0.10 a day, $3 a month. Apify is not the expense here.

The routine run itself eats normal Claude usage. One run a day is fine.

Routines are research preview and have a daily cap on runs.

---

## Live routine

- Name: Upwork YouTube job inbox
- ID: `trig_017tZTNWZ8VymdH5tasfD31J`
- Schedule: `33 4 * * *` UTC, which is 10:03am IST daily
- Model: Opus 5
- Edit or pause it: https://claude.ai/code/routines/trig_017tZTNWZ8VymdH5tasfD31J

---

## The prompt (this is what the routine runs)

**Paste everything between the fences into claude.ai/code/routines. Nothing else.**
Last changed 25 Aug 2026: queries diversified after the overlap bug, maxResults 5 to 10,
editing kill rule added, and every tier now uses the same six-line format.

```
You are running Flavio's Upwork job hunt in the personal-upwork repo. You FIND and SCORE jobs. You do not write proposals.

STEP 1 - read the rules
Read CLAUDE.md section 4 (door-check) and section 3 (bucket strategy), and tasks/context.md (real numbers, rate floor, what he can actually claim). Also read tasks/job-hunt-routine.md, which holds this same spec plus any rule Flavio has added since. Those files win over anything below if they ever disagree. If the file and this prompt disagree, follow the file and say so in the inbox header.

STEP 2 - pull the jobs
Run this once per query, 4 queries. $APIFY_TOKEN is in the environment.

curl -s -X POST "https://api.apify.com/v2/acts/blackfalcondata~upwork-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" -H "Content-Type: application/json" -d '{"query":"QUERY_HERE","maxResults":10,"enrichDetails":true,"verifiedPaymentOnly":true,"incrementalMode":false,"skipReposts":false}'

Queries:
  youtube channel manager
  youtube channel audit
  youtube consultant
  video content manager

That is 40 jobs pulled, expect roughly 20 to 28 unique. If a curl fails, retry it once, then carry on with the queries that worked and name the failed ones in the output. One bad query does not kill the run.

Dedupe across queries by jobId. Report both numbers in the header: pulled and unique. If unique drops below 15, the queries have collapsed into each other again and the header must say so.

STEP 2b - the editing and thumbnail rule
Flavio does not sell hands-on editing or thumbnail design. He directs people who do. The test: would the deliverable be a video file or an image file? Then the verdict is NO, reason "editing work, not strategy" or "thumbnail design, not strategy". Would it be a decision, a plan, a document or a report? Then score it normally. Strategy work that mentions editing as one line of a longer scope stays in. Judge the deliverable, not the job title.

THIS RULE MARKS A JOB, IT NEVER HIDES ONE. A job it kills gets the exact same six lines as a BID. The filtering is not trusted yet and Flavio reads every job to catch it being wrong.

STEP 3 - score every job, delete nothing
Check each job against these. Do not drop anything. Record which checks it fails.

  - clientPaymentVerified is true
  - totalApplicants is PRESENT and under 5
  - budget is real: budgetAmount > 0 OR hourlyBudgetMin > 0
  - client has history: clientTotalSpent > 0 OR clientReviewCount > 0
  - publishTime is within the last 24 hours
  - the description names actual scope, not "grow my channel, what do you charge"
  - not an agency fishing post. Tell: vague scope, big budget, every skill category at once

If totalApplicants is missing or null, that is a FAIL, not a pass. A missing field is unknown, never zero. Say "applicant count missing" so Flavio can see it.

Then sort into three tiers:

  BID         - zero fails
  BORDERLINE  - exactly one fail, AND the work matches his real proof in tasks/context.md (YouTube channel management, content team ops, growth strategy, retention)
  NO          - everything else

A job whose only fail is "one applicant over" or "26 hours old" is BORDERLINE, not NO. A job at $5/hr is NO even if it passes every check, because it is under his $18.50 floor.

STEP 4 - note what he has already touched
Check outreach/ and the sent proposals log in tasks/context.md. If a job is already covered, keep it in the list but mark it "already bid" in place of the verdict. Match on client or job title, not filename.

STEP 5 - write the inbox
Overwrite tasks/job-inbox.md completely. Do not append. Number every job continuously across all three tiers so "draft 7" is never ambiguous.

EVERY job gets all six lines below. No exceptions, no tier gets a shorter form, NEVER a bare table. Flavio does not trust the filtering yet and reads the whole list to catch the scoring being wrong, which he cannot do if a job is reduced to a "killed by" reason. The tiers are a sort order and a recommendation, not a visibility setting.

The two lines that carry the value are "What they want" and "Call". They are what make the list skimmable and what let him overrule you. Never drop them.

# Job Inbox - <date>

Ran <time> UTC. 4 queries, <N> jobs pulled, <D> unique after dedupe.
BID <n> - BORDERLINE <n> - NO <n>
<if any tier is empty, or if unique dropped below 15, say so plainly here>

## BID

### 1. <title>
- <n> applicants - <$X fixed / $X-Y per hour> - <$X spent, X reviews, X.X stars, country> - posted <how long ago>
- Link: <url>
- What they want: <one plain line. The actual ask in your words, not their marketing. Someone reading only this line should know whether to care.>
- Verdict: BID
- Why it fits: <tied to his ACTUAL proof in tasks/context.md. Name the client or the number. Never "good match for your skills".>
- Call: <what to actually do. Bid $X, boost or not, and the one thing to lead with.>
- Watch out: <only if there is a real flag. Skip the line if there isn't.>

## BORDERLINE

### 8. <title>
- <same one-line stats row> - Link: <url>
- What they want: <one plain line>
- Verdict: BORDERLINE, fails <the one check>
- Call: <the concrete condition that would make this a yes, and what to bid if it is met>

## NO

### 15. <title>
- <same one-line stats row> - Link: <url>
- What they want: <one plain line, same quality as the BID ones. This is the tier where a wrong verdict hides, so this line has to be good enough for him to catch you.>
- Verdict: NO, <the reason>
- Call: <usually "skip". But if there is a version of this that would be worth it, say it. "Skip unless he raises the budget." "Skip, but this client posts weekly, worth watching.">

Sort BID best-fit first. Best-fit means: matches his real proof, low applicant count, budget above his $18.50/hr floor or a fixed price worth the hours.

Keep it readable on a phone. Short lines, no wide tables. Twenty-plus jobs at six lines each is long, and that is fine, it is meant to be scrolled.

STEP 6 - commit and push to main. Commit message: "job inbox <date>".

An empty BID tier is a real answer and will be common. If nothing reaches BID, say so plainly at the top and still write every job. Never invent a job, never pad a tier, never soften the scoring to fill BID.

Do not write proposals. Do not edit any file other than tasks/job-inbox.md.
```

### Raising the volume later

`maxResults` is the only number to change, in the routine prompt at
https://claude.ai/code/routines. 5 per query is 20 jobs a day. 25 per query is 100.

Raise it when BID keeps coming back empty two or three days running. That means the
top 5 per search are already picked over by the time the routine looks, and you need
to go deeper down the list.

---

## Run history

| Date | Pulled | BID | Notes |
|------|--------|-----|-------|
| 13 Aug 2026 | 25 (manual CSV, 1 query) | 2 | first real door-check. 666 Media $500 fixed + $50/mo forex channel. 23 dropped, which is what triggered the score-do-not-filter rewrite |
| 14 Aug 2026 (re-run) | 20, 19 after dedupe | 5 | 4/4 queries returned. BID 5, BORDERLINE 2, NO 12. Landed on main. **Two findings.** (1) The 403 was NOT Task 4. Pushes started working the instant the GitHub connector reconnected mid-session, so the cause was the credential helper being unavailable, not a branch-push permission. Nothing for Flavio to flip. (2) The scheduler is still serving the PRE-19-Aug prompt: this run used `youtube editor` and `youtube thumbnail` and wrote the old bare NO table, which is why 2/3 of the NO tier was off-bucket editing work. Fixing the repo file did not fix the routine. The stored prompt at claude.ai/code/routines has to be replaced by hand. |
| 14–19 Aug 2026 | ~20/day, 6 runs | — | routine ran daily on schedule but every push to GitHub 403'd (believed at the time to be Task 4, see the 14 Aug re-run row for the actual cause), so `job-inbox.md` in the repo never updated — each day's real inbox only reached Flavio as a phone push + attached file, never landed here. Also: 2 of the 4 queries (`youtube editor`, `youtube thumbnail`) were pulling hands-on editing/thumbnail-design jobs outside the bucket, and NO tier had no link or summary, so the daily inbox looked more "filtered" than it should. Paused 19 Aug 2026 to fix both before re-enabling. |

---

## Later: friends (Phase 2, not built)

The phone app is your login, so a friend cannot text into it. The route is a Routine
**API trigger**: it gives you a URL and a bearer token, and a POST carries a `text`
field into the run. A ~30-line Telegram bot that forwards "this is A: <job link>"
into that URL is the whole build.

Then: add `people/a/` with A's context, and the routine prompt picks the folder from
the name in the text.

Two things to know before that: every run bills to your account and your usage cap,
and commits land under your GitHub identity. Fine for a favour, wrong shape if it
grows into something you charge for.
