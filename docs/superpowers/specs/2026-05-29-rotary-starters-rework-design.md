# Rotary Starters Page — Content Rework Design

**Date:** 2026-05-29
**Author:** Kealani Solutions (with AI assistance)
**File affected:** `index.html` (single file, no new files, no CSS rewrite, no dependency changes)
**Status:** Approved in brainstorming; ready for implementation plan.

---

## Goal

Rework the AI Starters for Rotary Leaders landing page to be more durable and lower-maintenance, by:

1. **Tool-agnostic** — remove product names (Claude, Gemini, NotebookLM) so the page applies to any modern AI tool and doesn't implicitly endorse specific products.
2. **Rule-pointer, not rule-quoter** — stop quoting specific Rotary AI Guidelines language (which can change). Instead, point users to Rotary's guidelines and their own club/district policies. This protects Kealani Solutions from having to track and re-edit quoted text whenever Rotary revises the document.
3. **Reframe the newsletter** as *AI, Actually* — Kealani Solutions' personal bi-weekly publication on practical AI, not a Rotary-specific resource.
4. **Open a real feedback/questions channel** to `info@kealanisolutions.com` via mailto with a prefilled subject line.
5. **Soften 9 starter customize tips** that currently tempt users to paste prohibited content (member names, real financials, donor info). Wording-only swaps; no starter prompts change.

## Non-goals

- Visual redesign. The CSS, color palette, typography, spacing, structure, and Manrope-based system stay exactly as-is.
- Changes to any of the 18 starter prompts (the actual text users will copy into AI tools).
- Changes to any "What it teaches" lines.
- New starters, new sections, new JS, or new dependencies.
- Adding a hosted contact form (mailto is sufficient for current volume; can be revisited later).
- Per-starter AI-output disclosure nudges (explicitly declined in brainstorming).

## Decisions captured from brainstorming

| # | Topic | Decision |
|---|---|---|
| Q1 | Privacy notice contents | Strip to pointer only — no quoted rules, no section numbers |
| Q2 | Risky starter customize tips | Light pass — small wording swaps on 9 tips |
| Q3 | User-output disclosure nudge | Skip — page silent on this |
| Q4 | LLM product names | Drop "Claude, Gemini, NotebookLM" from hero and "NotebookLM" from customize tip |
| Q5 | Newsletter copy | C-style with name worked in; remove "no spam, easy to unsubscribe" |
| Q6 | Newsletter wording | "Get *AI, Actually* in your inbox" / "Bi-weekly practical AI tips and things to try…" |
| Q7 | Feedback section | C-style, mailto, prefill subject, no visible address in body |

---

## Section-by-section changes

### 1. Hero lede

**Currently** (line 289):
> A handful of ready-to-use starting points for working with AI tools like Claude, Gemini, and NotebookLM. Find your challenge, copy a starter, add your club's details, and go.

**New:**
> A handful of ready-to-use starting points for working with AI tools. Find your challenge, copy a starter, add your club's details, and go.

Second lede sentence ("These aren't magic phrases…") unchanged. Org-line ("A resource from Kealani Solutions — not affiliated with or endorsed by Rotary International.") unchanged.

### 2. Privacy notice (amber callout)

**Currently** (lines 344–350): A multi-paragraph callout with a "ONE RULE BEFORE YOU START" label, a quoted list of prohibited content from Rotary AI Guidelines Section 4, a "Rotary AI Guidelines, Section 4" citation, and a paragraph pointing readers to rotary.org.

**New:** Single short pointer. Amber styling stays (still signals "read this"). The `.notice-label`, the quoted prohibited-items text, and the "Section 4" citation all go.

```html
<div class="notice" role="note" aria-label="Before you start">
  <p>Before using AI for Rotary work, review Rotary International's AI Guidelines and check your district's and club's own policies.</p>
</div>
```

No `<span class="notice-label">`, no second paragraph, no rules quoted.

### 3. Starter customize tips — 9 light-pass swaps

All swaps are inside `<p class="customize">` blocks. Prompt text and "What it teaches" lines unchanged.

**3.1. Build a Three-Year Action Plan** (line 413)
- Before: *"Have your last year's numbers handy — membership, finances, event turnout — so you can answer its questions with specifics."*
- After: *"Know your club's general shape — rough membership range, what you spent on the big things, how events turned out — so you can answer in ranges, not exact figures or real names."*

**3.2. Solve a Recurring President-Elect Gap** (line 423)
- Before: *"Name what you've already tried, honestly. That's what stops the AI from suggesting the obvious things you've already done."*
- After: *"Describe what you've already tried — what kind of outreach, what kind of person — without naming individuals. That's what stops the AI from suggesting the obvious."*

**3.3. Plan a Board Meeting Worth Attending** (line 433)
- Before: *"Dump your raw notes in — the AI is good at turning a messy brain-dump into a structured agenda. Add your meeting length so the time blocks are realistic."*
- After: *"List the topics and decisions on your mind, even messily — without member names or specific financial figures. The AI is good at turning that into a structured agenda. Add your meeting length so the time blocks are realistic."*

**3.4. Hand Off a Role Without Losing the Knowledge** (line 443)
- Before: *"Do this as a conversation over coffee — answer its questions out loud and paste them back in. The 'what nobody tells you' details are the real value."*
- After: *"Do this as a conversation over coffee — answer its questions out loud and paste them back in, keeping names, contact info, and specific financials out of what you type. The 'what nobody tells you' details are the real value."*

**3.5. Win Back Members Who Drifted Away** (line 474) — highest-risk current wording
- Before: *"Pick one real person and answer the AI's questions about them. A message written for one person beats a generic 'we miss you' blast every time."*
- After: *"Have one specific person in mind and answer the AI's questions about them — using general details (how long they were in the club, why they joined, what changed) instead of their name or contact info. A message written for a real situation beats a generic 'we miss you' blast every time."*

**3.6. Run a Post-Event Debrief That Sticks** (line 545)
- Before: *"Do this within a week of the event while details are fresh. Dump everything in — the AI will sort the signal from the venting."*
- After: *"Do this within a week of the event while details are fresh. Share what happened in general terms — without naming attendees or sponsors — and the AI will sort the signal from the venting."*

**3.7. Turn Rough Notes Into a Monthly Newsletter** (line 566)
- Before: *"Don't polish your notes first — messy bullets are exactly what the AI is good at shaping. Do specify the tone you want."*
- After: *"Don't polish your notes first — messy bullets are exactly what the AI is good at shaping. Keep names and personal details out until you're ready to add them in yourself. Do specify the tone you want."*

**3.8. Write a Press Release for a Service Project** (line 576)
- Before: *"Have the who/what/where/when ready. Let the AI ask what's newsworthy — clubs often lead with the routine and bury the human story."*
- After: *"Have the what/where/when ready — and describe the people involved by role instead of name until you're ready to add real names in your own edit. Let the AI ask what's newsworthy — clubs often lead with the routine and bury the human story."*

**3.9. Feed AI Your Own Documents** (line 617) — drops NotebookLM reference and the quoted rule list
- Before: *"Stick to material that's already public or that contains no personal, financial, donor, or grant information — past event write-ups, published newsletters, program descriptions, strategic plans. Tools like NotebookLM are built specifically for working from your own documents. If you're not sure something is safe to share, don't upload it."*
- After: *"Stick to material that's already public or doesn't contain sensitive details — past event write-ups, published newsletters, program descriptions, strategic plans. Some AI tools are designed specifically for working from your own documents. When in doubt, don't upload it."*

### 4. Newsletter block

**Currently** (lines 656–659):
- Heading: "Get the follow-up resources"
- Body: "Get general AI tips and techniques you can apply to your Rotary work — added as we find things worth sharing. Not a Rotary-curated or officially scheduled resource. No spam, easy to unsubscribe."
- Button: "Sign up for updates" → `REPLACE_WITH_SIGNUP_LINK`

**New:**
- Heading: `Get <em>AI, Actually</em> in your inbox` (italic on the publication title)
- Body: "Bi-weekly practical AI tips and things to try — for work, Rotary, and everything in between."
- Button: "Subscribe" → `https://aiactuallyhi.substack.com/subscribe`

### 5. Feedback block

**Currently** (lines 663–666):
- Heading: "Tell us what worked (and what didn't)"
- Body: "Tried one of these with your club? We'd love to hear how it went and what you'd want next."
- Button: "Share your feedback" → `REPLACE_WITH_FEEDBACK_LINK`

**New:**
- Heading: "Questions? Feedback? Get in touch."
- Body: "Share what's working, what's not, or ask us anything — we read every message."
- Button: "Email us" → `mailto:info@kealanisolutions.com?subject=AI%20Starters%20%E2%80%94%20feedback%20or%20question`

Body intentionally does not show the email address as plain text (spam-minimization decision). The address lives only inside the `mailto:` href.

### 6. Footer

No change. The three existing lines remain:

1. "Made for Rotary club leaders & officers · Built during our AI training session"
2. "A resource from Kealani Solutions — not affiliated with or endorsed by Rotary International."
3. "Starters on this page were developed with AI assistance and reviewed by Kealani Solutions."

None of these quote Rotary rules; they are factual / transparency statements about Kealani Solutions' own work and stay durable across future Rotary guideline revisions.

---

## CSS changes

None required. The `.notice` class stays (still used for the simplified pointer). The `.notice-label` class becomes unused — we leave the CSS rule in place (one orphaned class, no harm) rather than touch the stylesheet. The `.org-line` class stays (still used below the hero). All other rules unchanged.

## What's explicitly NOT changing

- All 18 starter prompt texts (the `<div class="prompt">` blocks users will copy)
- All "What it teaches" lines (`<div class="teaches">`)
- The Start Here → Find Your Challenge → 5 collapsible sections structure
- The CSS — including the amber `.notice` styling, all color variables, typography, layout
- The Copy-button JavaScript (lines 674–710)
- The org-line below the hero (added in prior compliance work)
- The 3 footer lines (added in prior compliance work)
- The Manrope font import

## Verification

Static-file edit, no build step. After edits:

1. Reload `http://127.0.0.1:8766/index.html` (dev server already running, background task `brl3n9q0g`)
2. Screenshot top of page → confirm hero lede no longer names LLMs; amber notice is one short sentence
3. Screenshot mid-page → spot-check the 9 customize-tip rewrites read naturally
4. Screenshot bottom → confirm newsletter shows "Get *AI, Actually* in your inbox" with Subscribe button; feedback shows new heading and "Email us" button
5. Click "Subscribe" → confirms navigation to `https://aiactuallyhi.substack.com/subscribe`
6. Click "Email us" → confirms default mail client opens with subject `AI Starters — feedback or question`

No automated tests; this is a static page with no test suite.

## Considered but intentionally not changed

- **"Feed AI Your Own Documents" prompt bracket** (line 614 area) currently includes the hint *"a strategic planning document with no personal or financial data"* inside the user-fill bracket. This is in-prompt guidance for the user (not a rule citation in page chrome), and removing it would weaken the practical instruction. Left in place. If you want it softened, flag during implementation.
- **Customize tips at lines 464, 484, 494, 515, 525, 535, 586, 596, 647** were reviewed and judged not to need a swap — they describe aggregate club facts (size, culture, capacity, platforms) rather than tempting users toward member names or real financials.

## Risks and open items

- **Substack URL** — user provided a URL with tracking parameters and a `just_signed_up=true` flag from their own signup flow. Spec uses the canonical `https://aiactuallyhi.substack.com/subscribe`. If this doesn't behave as expected when the user verifies, swap for `https://aiactuallyhi.substack.com/` (Substack homepage with inline subscribe form).
- **Mailto fallback** — users on a device without a default mail client configured (e.g., browser-only Gmail users without a registered handler) will see no action when clicking "Email us." This is accepted tradeoff vs. the spam exposure of showing the address. Can be revisited later with a JS-obfuscated visible address or a hosted form.
- **`REPLACE_WITH_FEEDBACK_LINK` placeholder** — this gets replaced by the mailto. README still mentions both placeholders; README update can happen in the implementation pass or be skipped (the README's accuracy on placeholders only matters for users hosting their own copy of the page, which doesn't apply if the live deployment is solely Kealani Solutions').
