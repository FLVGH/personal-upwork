# YouTube Channel Audit — the deliverable that sells the retainer

This is the product you sell at $300 to $500, and it is also your sales call. Built to be
runnable in 60 to 90 minutes per channel once you have done three of them.

Read this before the first one. The decision tree in section 2 is the whole thing. Everything
else is presentation.

---

## 0. Why the audit and not the retainer

You cannot sell a $2,000 monthly retainer to a stranger who has never worked with you. You can
sell a $400 audit to almost anyone, because the risk is nothing and the curiosity is real.

What the audit actually buys you:

1. A review on Upwork, which is what unlocks Phase 2 of the bucket strategy
2. Access to their Studio analytics, so you price the retainer against real problems
3. A scheduled call where you present findings. That is a warm sales call disguised as a
   delivery call, and you have run a thousand of those.

Most YouTube freelancers cannot sell. You closed 250K. The audit exists to get you on the call,
because the call is where you win. Do not skip the presentation and email a PDF.

---

## 1. What you need from them before you start

Ask for these in one message. If they will not give Studio access, the audit is guesswork and
you should say so rather than fake it.

- Viewer access to YouTube Studio, or a screen-share where they drive
- Their offer. What they sell, what it costs, who buys it
- Where a lead is supposed to go. Link in description, lead magnet, booking page, nothing
- Their three closest competitors, or the channels they wish they were

The offer question is the one nobody else asks. Ask it first.

---

## 2. The diagnostic tree — run this before you write a word

Almost every channel problem is one of four things, and they need opposite fixes. Getting this
right in the first ten minutes is what makes you look like an operator instead of a reviewer.

Pull the last 10 to 20 videos in Studio. Advanced mode, sort by impressions.

```
Low impressions?
  YES -> TOPIC problem. YouTube is not offering the video to anyone.
         The subject has no demand, or the title does not match a search or
         browse intent that exists. Packaging fixes will do nothing here.
         This is the most commonly misdiagnosed one.

  NO, impressions are fine -> check CTR
      CTR under 4% -> PACKAGING problem. The offer is being shown and
                      ignored. Title and thumbnail, in that order.
      CTR 4-6%     -> normal. Look further down.
      CTR over 8%  -> packaging is working. Do not touch it. Problem is
                      downstream.

  CTR fine -> check average view duration and the retention curve
      Big drop in first 30 seconds -> HOOK problem. The video did not
                                      restate the promise the title made.
      Steady bleed after 2 minutes -> PACING or structure problem.
      Retention fine               -> the content works. Go to the funnel.

  Everything above fine but no leads -> FUNNEL problem, and this is your
      specialty. Views are landing, trust is building, and nothing captures
      it. No CTA, a CTA pointed at the wrong step, or a cold viewer being
      sent straight to a paid page.
```

Write down which of the four it is before you open a document. One channel usually has one
primary and one secondary. Not four.

---

## 3. The report

Seven sections. Keep it to two or three pages. A long audit reads as padding.

### 3.1 The one line
Open with the single sentence version of what is wrong. No preamble.

> Your packaging is working and your funnel is empty. Videos are pulling a 7.2% click
> through rate and nothing on the channel asks the viewer to do anything.

### 3.2 What is working
Two or three real things, named specifically. This is not politeness. If you only bring
problems you read as a critic, and people do not hire critics to run their channel.

### 3.3 The primary problem
Which of the four, the evidence from Studio, and what it is costing them in their own terms.
Translate to their business: not "retention is 28%" but "viewers leave before you ever mention
what you sell."

### 3.4 Packaging teardown
Their last five titles and thumbnails against their three competitors. Screenshot the grid side
by side. This section sells itself visually and takes fifteen minutes.

For each: what the title promises, whether the thumbnail says the same thing, and whether a
person scrolling at phone size understands it in half a second.

### 3.5 The funnel past the click
Nobody else does this section. It is why you get the retainer.

Walk the path: video, description, link, landing page, what happens next. Name every place the
intent leaks. Common ones:

- No CTA at all, or one CTA buried at the end where 20% of viewers remain
- CTA sends a viewer who found them 90 seconds ago straight to a paid checkout
- The link goes to a homepage instead of the thing the video was about
- Nothing captures an email, so every non-buyer is lost permanently

### 3.6 Three topics to test
Concrete titles, not themes. Tied to buyer intent, not view potential. For each, one line on
who clicks it and why that person is worth having.

### 3.7 What the first 90 days would look like
Week blocks, never day by day. This is the retainer pitch and it should read as the obvious
next step, not as a separate sales pitch bolted on.

---

## 4. Running it fast

The vidIQ connector in this repo does the competitor and packaging work in minutes. Useful calls:

- `vidiq_channel_analytics` and `vidiq_channel_stats` for the baseline
- `vidiq_channel_videos` sorted for outliers, to find what already worked for them
- `vidiq_similar_channels` and `vidiq_outliers` for the competitor grid in 3.4
- `vidiq_score_thumbnail` and `vidiq_score_title` for a second opinion on packaging
- `vidiq_keyword_research` to confirm or kill a topic before you recommend it

Do not paste vidIQ scores into the report as authority. Use them to find the thing, then say it
in your own words with the Studio number next to it. A client paying you for judgment does not
want a tool's output forwarded.

---

## 5. The call, and the close

Present it live. Screen share. Twenty minutes of findings, ten of questions.

Do not send the retainer proposal on the call. Close on the next step instead:

> That is what I would fix. Want me to run it for 90 days?

If yes, price against the problem you just showed them, not against your hours. A channel losing
leads because there is no capture step is worth more to fix than one that needs better
thumbnails, and the retainer should say so.

Floor: $1,500/month. Do not quote hourly for this. See tasks/10k-plan.md for why the retainer
number is the whole business.

---

## 6. Reuse

Save every audit to `deliverables/audits/<client>-<date>.md`. After three, the packaging
teardown and funnel sections become largely templated and the audit takes an hour.

That reusability is the actual asset. It is also what you would hand an employee later, which
is the same thing BID #1 and #2 in the inbox are paying you to write for somebody else.
