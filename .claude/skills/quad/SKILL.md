---
name: quad
description: Run the "4check" verification pass on the most recent Upwork proposal draft in this conversation before it gets sent — catches em dashes, consultant-clichés, label tags, verbatim job-post quotes, day-by-day timelines, and missing Preflight/Postflight/self-score. Trigger this whenever the user types "quad" or "4check" (with or without a slash), or asks to "verify", "check", or "run 4check on" the proposal/draft/outreach text. Only applies to Upwork proposal drafts (5-slot/7-slot outreach text meant for outreach/<slug>.md) — do NOT run this against code, automation/routine files, CVs, or any other non-proposal content; if there's no proposal draft in the recent conversation, say so instead of checking something else.
---

# quad / 4check verification

This is the manual verification pass defined in this repo's `CLAUDE.md` (sec. 15),
packaged as a skill so it's reliable to invoke instead of depending on the whole
CLAUDE.md being re-read correctly every time.

It exists because proposal drafts are easy to get 90% right and still ship with one
tell — an em dash, a leftover label, a cliché — that reads as AI-generated to a
client skimming Upwork bids. Running this checklist before sending catches that
before Flavio pastes it in.

## Scope check first

Before running anything, confirm there's an actual proposal draft to check: text
written in the 5-slot or 7-slot pattern (CLAUDE.md sec. 6–7), meant for
`outreach/<slug>.md`. If the most recent relevant work in the conversation is
something else — code, the CS/Upwork job-hunt routine files, a CV, general
Q&A — say plainly that there's no proposal draft to verify, and don't force the
checklist onto it. Running a proposal checklist against a routine config file would
produce meaningless output and hide the fact that nothing was actually checked.

## The checklist

Run all three sections against the draft. For each item, look at the actual text —
don't take a summary of it on faith.

### 1. Code/content quality
- Any unfinished logic or stand-in/placeholder copy (e.g. "[insert client name]"
  left in)?
- Anything missing from what was actually asked for?
- Any edge case in the ask left unhandled?

### 2. Rules compliance
Check the draft text character-by-character for these, not just at a skim:
- **Em dashes anywhere** — including inside a sentence, not just as a standalone
  punctuation mark. Zero tolerance; this is the single most common tell.
- **Consultant-clichés**: "happy to", "I'd love to", "leverage", "deep dive",
  "rest assured", "looking forward", "single source of truth", "hardened for",
  "best practices", "Results-Driven", "Multichannel Outreach Mastery",
  "Data-Driven Approach", "Problem-Solving".
- **Labels in the proposal body**: "Free offer:", "Pitfalls:", "Stack match:",
  "Phase 1:"/"Phase 2:"/"Phase 3:", or any other bracketed/colon-terminated tag
  that isn't natural sentence prose.
- **Verbatim job-post quote** — a phrase lifted directly from the client's post
  instead of reframed in Flavio's own words.
- **Day-by-day timeline pre-call** — anything that reads like a Gantt commitment
  ("Day 1-2: ..., Day 3: ..."). A total estimate is fine; a per-day breakdown before
  the scoping call is not.
- **Gap-disclosure phrasing** — "I haven't used X by name" or equivalent. The rule
  is to claim the pattern or suggest a 3rd party, never to name the gap directly.
- **Self-score present, and 7 or higher** — if the draft doesn't already carry a
  self-score line, compute one now using the CLAUDE.md sec. 8 rubric (below) rather
  than treating "missing" as a pass.

**Self-score rubric** (only needed if the draft doesn't already have one):
Start at a baseline and adjust:
- −1 each: consultant-cliché present, label tag in body, verbatim job-post quote,
  generic social proof not tied to their domain, em dash anywhere, day-by-day
  timeline commit pre-call, gap-disclosure phrasing.
- +1 each: real sentence-length variance (mix of fragments and longer sentences),
  a specific number that could only come from having done the work, a personalised
  opening line naming a detail from their actual post, a negative-phrased call ask
  ("Up for a call tomorrow?" rather than "I'd love to hop on a call").
7+ ships. Below 7 needs a rewrite, not just a patch.

### 3. Completeness
- Was the full request actually done, or only described / half-drafted?
- Is the send-ready text saved to `outreach/<slug>.md` in a code block, or still
  sitting only in the chat reply?
- Are both a Preflight block and a Postflight block present, in the CLAUDE.md
  sec. 9 format?

**Preflight** (before drafting — confirm this exists, don't regenerate it after the
fact if it's missing; flag the miss instead):
```
**Preflight**
- Read: profiles/bio-ready.md, outreach/<similar-past-job>.md (if any)
- Variant: 5-slot default / 7-slot advanced
- Hard rules: no em dashes, no labels, [any job-specific rule]
- Bid: $X/hr (avg paid: $Y from Insights tab)
- Boost: yes/no
```

**Postflight** (after the send-ready text):
```
**Postflight**
- Em dashes: 0
- Word count: ~XXX
- Structure: [1] personalised [2-3] social proof [4] how we help [5] stack-handling [call ask]
- AI-cliché check: clean / flagged: [list]
- Unverified flagged: [any claim I'm making that I can't back on the call]
- Self-score: X/10
```

## Output

**If any check fails**: list each failed item with what's specifically wrong (quote
the offending phrase, don't just name the category) and the concrete fix. Then apply
the fixes to the draft and re-save it to `outreach/<slug>.md` before presenting the
corrected version — don't just report the problems and stop.

**If everything is clean**: reply with exactly:

```
Verification: PASS
```

No extra commentary around it — that line is the whole point, a fast unambiguous
signal that this draft is ship-ready.
