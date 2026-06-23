# Productization Vision — AI Job-Application Assistant (reusable for others)

SEPARATE from the Upwork build. This is the "clone it for other people / other
platforms / other roles / resumes" plan. Different rules — captured here in full.

Core idea: the Upwork system is really a generic ENGINE. Once it exists, you can
spin up lean versions for other people that help them apply to normal jobs
(LinkedIn / Indeed / anywhere) FAST, BEST, and REFINED — without rebuilding.

---

## 1. ENGINE vs CONFIG+CONTENT (the whole principle)

The plumbing is built once and reused. Only content + small config changes per
customer. Keep platform-specific bits in separate config/files, NEVER hardcoded
into the flow — that one discipline is what makes clones cheap.

| Component | Reusable? | Changes per customer/platform/role |
|-----------|-----------|------------------------------------|
| Telegram bridge (n8n) | Fully | Bot token + chat ID |
| Claude integration + refine loop | Fully | The prompt + the context file |
| The playbook / master resume | Per customer | New content |
| Source (RSS / Apify actor) | Swappable | Different feed/actor + field adapter |
| Door-check / filter | Pattern reusable | Rules differ per platform |

The MOAT is not the n8n flow (anyone can wire that). It's the role-specific
playbooks and the per-person content. That's what someone pays for.

---

## 2. TWO PRODUCT TYPES — and resumes are the EASIER one

**Proposals (Upwork-style):** generative, from scratch, persuasion, voice. HARD.

**Resume tailoring (normal jobs):** constrained editing, not generation. You already
have the truth (master resume); the AI just re-emphasizes + rewords + mirrors the
job's keywords. EASIER, safer, more repeatable. This is the better product for
"other jobs."

Resume flow:
```
Job description (pasted, or RSS/Apify)
  + master resume (the person's REAL experience)
  -> AI: pick matching experience, rewrite the summary line, reorder/sharpen
     bullets, mirror the job's keywords for ATS
  -> tailored resume (a few changed lines)
  -> refine in Telegram ("lean harder on CS metrics") -> done
```
(Flavio already has a base resume in the repo: `Flavio Mendes - Account Executive
- Resume.pdf` — that's the "master resume" input shape.)

### RESUME RULES (must remember — different from proposals):
1. **NEVER fabricate.** A resume is factual claims an employer verifies. The #1
   prompt rule: only re-emphasize and reword what is ALREADY TRUE in the master
   resume; never invent experience. (Proposals can be looser; resumes cannot.)
2. **ATS keyword mirroring is the real value.** Most applications are first read by
   an Applicant Tracking System scanning for the job's keywords. Echoing the
   posting's language is what gets it past the filter — concrete, sellable benefit.
3. **Keep formatting ATS-safe.** No multi-column/table layouts the parser chokes on.
   Easiest: person keeps a clean master resume, AI outputs tailored TEXT, they paste
   into their template. Or generate a .docx via Claude's docx skill for full done.

---

## 3. THE 3-HOUR "LITE" VERSION (no scraper)

For helping someone apply fast, you DON'T need the scraper. The value is the
draft-fast + best + refine loop. Drop the expensive/fragile pieces:

CUT: Apify scraper, door-check filter, field adapters, Managed Agent + git/repo
memory. (Repo memory only matters if it must learn from past outputs over time.)

KEEP: Telegram bot -> Claude API (playbook/master-resume in the prompt) -> draft
-> refine in the same thread -> done. Person finds the job themselves and pastes
it in (finding is cheap + unbreakable; writing well + fast is the hard part).

This uses a plain Claude API node, NOT the Managed Agent — simpler than the Upwork
build.

---

## 4. FINDING JOBS IS THE CHEAP PART — RSS-first, Apify-fallback

| Platform | Cheapest source |
|----------|-----------------|
| Remote/niche boards (WeWorkRemotely, RemoteOK, StackOverflow, HN Who's Hiring) | RSS — free, stable, zero ban risk, native n8n RSS Trigger node |
| Company career pages | Often RSS, or a cheap generic crawler |
| Indeed | Apify actor (public RSS deprecated) |
| LinkedIn | Apify actor (no public job RSS; most fragile + aggressive anti-scrape) |

Prefer RSS whenever it exists: free, won't break, thin fields are fine for a
find->paste->AI flow. Only Indeed/LinkedIn force a paid actor.

---

## 5. TIME / EFFORT MODEL (once the Upwork engine exists)

Cost is front-loaded into (1) the engine [building now for Upwork] and (2) ONE good
playbook per document TYPE. After that, clones are cheap.

| Adding... | Time |
|-----------|------|
| First resume-tailoring variant (write the resume playbook once) | ~2-3 hr |
| Another PERSON on an existing type (swap their content) | ~30-60 min |
| Another ROLE (CS vs sales vs dev — tweak emphasis) | ~1 hr |
| Another platform via RSS | minutes |
| Another platform via Apify (LinkedIn/Indeed) | +2-4 hr (actor + adapter) |
| Full self-serve SaaS for strangers (multi-tenant, billing, onboarding) | weeks+ |

The real ongoing cost of a multi-platform product = SCRAPER MAINTENANCE (each
platform breaks on site redesigns). Not the AI. RSS sources don't have this problem.

---

## 6. "TRAINING THE AI FOR A ROLE" = NOT ML

No model training, no datasets. "Training for CS roles" = writing the role's
playbook / tailoring rules (prompt + context). Cheap, fast, editable. This is the
key reframe — the per-role work is CONTENT authoring, not engineering.

---

## 7. NEXT STEPS (when ready to build the "other" version)
- [ ] Finish the Upwork engine first (the reusable bridge + Claude integration).
- [ ] Write the resume-tailoring playbook once (rules in §2).
- [ ] Prep one master resume for the first test person.
- [ ] Wire a 2nd Telegram bot (or a "resume mode") -> Claude API -> refine.
- [ ] Pick the first platform: start with an RSS board (free) before any Apify actor.
- [ ] Decide later: keep it as a per-person favor, or productize (then tackle
      multi-tenancy + scraper maintenance).
