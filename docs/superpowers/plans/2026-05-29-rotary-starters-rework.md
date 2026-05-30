# Rotary Starters Page Rework — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Apply the brainstorming-approved content rework to `index.html` — tool-agnostic copy, pointer-only privacy notice, *AI, Actually* newsletter rebrand, mailto feedback channel, and light wording swaps on 9 starter customize tips that previously tempted prohibited content.

**Architecture:** Single static HTML file. All changes are HTML text edits inside `index.html`. No CSS changes, no JS changes, no new files, no build step, no dependencies. Verification is visual via the dev server that's already running on `http://127.0.0.1:8766/`.

**Tech Stack:** Plain HTML/CSS/vanilla-JS static page. No test framework — verification is curl/grep for text presence and Playwright screenshots for visual confirmation. TDD-style "write the failing test first" does not apply; the equivalent here is "grep that the new text isn't already present, edit, then grep that it is."

**Spec:** `/Users/michellesmac/development/kealani5000/docs/superpowers/specs/2026-05-29-rotary-starters-rework-design.md`

---

## File Structure

Only one file is modified:

- **`/Users/michellesmac/development/kealani5000/index.html`** — landing page. All edits target specific lines inside `<body>`. The `<style>` block (lines 10–312 in current state) is unchanged. The `<script>` block (lines 674–710) is unchanged.

No new files are created in the source. The spec doc and this plan live under `docs/superpowers/`.

## Pre-flight check

Confirm the dev server from earlier is still running and the file is in the state the plan expects.

- [ ] **Step 0.1: Verify dev server is responding**

Run: `curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:8766/index.html`

Expected: `200`

If not 200, restart the server with: `python3 -m http.server 8766 --bind 127.0.0.1` from `/Users/michellesmac/development/kealani5000`, run in the background, and re-verify.

- [ ] **Step 0.2: Confirm key strings are present (anchors all subsequent Edits)**

Run:
```bash
grep -c "Claude, Gemini, and NotebookLM" /Users/michellesmac/development/kealani5000/index.html
grep -c "ONE RULE BEFORE YOU START" /Users/michellesmac/development/kealani5000/index.html
grep -c "REPLACE_WITH_SIGNUP_LINK" /Users/michellesmac/development/kealani5000/index.html
grep -c "REPLACE_WITH_FEEDBACK_LINK" /Users/michellesmac/development/kealani5000/index.html
```

Expected: all four print `1`. If any print `0`, the file has drifted from spec assumptions — stop and re-read the spec before proceeding.

Note: `grep "ONE RULE BEFORE YOU START"` will not match because the actual rendered text uses uppercase via CSS `text-transform: uppercase` on `.notice-label`, while the source uses normal case `"One rule before you start"`. Use that exact string:

```bash
grep -c "One rule before you start" /Users/michellesmac/development/kealani5000/index.html
```

Expected: `1`.

---

## Task 1: Drop LLM product names from the hero lede

**Files:**
- Modify: `/Users/michellesmac/development/kealani5000/index.html` (line ~289)

**Spec ref:** Section 1 — Hero lede

- [ ] **Step 1.1: Confirm exact current text**

Run: `grep -n "Claude, Gemini, and NotebookLM" /Users/michellesmac/development/kealani5000/index.html`

Expected one line, currently around line 289:
```
289:    <p class="lede">A handful of ready-to-use starting points for working with AI tools like Claude, Gemini, and NotebookLM. Find your challenge, copy a starter, add your club's details, and go.</p>
```

- [ ] **Step 1.2: Apply edit**

Use the Edit tool on `/Users/michellesmac/development/kealani5000/index.html`:

`old_string`:
```
    <p class="lede">A handful of ready-to-use starting points for working with AI tools like Claude, Gemini, and NotebookLM. Find your challenge, copy a starter, add your club's details, and go.</p>
```

`new_string`:
```
    <p class="lede">A handful of ready-to-use starting points for working with AI tools. Find your challenge, copy a starter, add your club's details, and go.</p>
```

- [ ] **Step 1.3: Verify**

Run:
```bash
grep -c "Claude, Gemini, and NotebookLM" /Users/michellesmac/development/kealani5000/index.html
grep -c "starting points for working with AI tools. Find your challenge" /Users/michellesmac/development/kealani5000/index.html
```

Expected: `0` and `1`.

---

## Task 2: Strip the privacy notice down to a single pointer

**Files:**
- Modify: `/Users/michellesmac/development/kealani5000/index.html` (lines ~344–350)

**Spec ref:** Section 2 — Privacy notice. Keep amber styling, remove the label span, the quoted Section 4 list, the Section 4 citation, and the rotary.org pointer paragraph. Replace with one short pointer paragraph.

- [ ] **Step 2.1: Confirm exact current block**

Run: `sed -n '343,351p' /Users/michellesmac/development/kealani5000/index.html`

Expected:
```html
  <!-- ============ DATA PRIVACY NOTICE ============ -->
  <div class="notice" role="note" aria-label="Data privacy notice">
    <span class="notice-label">One rule before you start</span>
    <p>Never paste member names, contact information, financial records, donor data, or grant details into any AI tool. Use general descriptions, not real data. If you're not sure whether something is safe to share, don't upload it.</p>
    <p class="cite">— Rotary AI Guidelines, Section 4</p>
    <p class="cite">Rotary International has published AI guidelines for members. Review them before using AI for Rotary work — and check your district's and club's own policies too. Find them at rotary.org (search "AI Guidelines for Members").</p>
  </div>
```

- [ ] **Step 2.2: Apply edit**

Use the Edit tool on `/Users/michellesmac/development/kealani5000/index.html`:

`old_string`:
```
  <!-- ============ DATA PRIVACY NOTICE ============ -->
  <div class="notice" role="note" aria-label="Data privacy notice">
    <span class="notice-label">One rule before you start</span>
    <p>Never paste member names, contact information, financial records, donor data, or grant details into any AI tool. Use general descriptions, not real data. If you're not sure whether something is safe to share, don't upload it.</p>
    <p class="cite">— Rotary AI Guidelines, Section 4</p>
    <p class="cite">Rotary International has published AI guidelines for members. Review them before using AI for Rotary work — and check your district's and club's own policies too. Find them at rotary.org (search "AI Guidelines for Members").</p>
  </div>
```

`new_string`:
```
  <!-- ============ BEFORE-YOU-START POINTER ============ -->
  <div class="notice" role="note" aria-label="Before you start">
    <p>Before using AI for Rotary work, review Rotary International's AI Guidelines and check your district's and club's own policies.</p>
  </div>
```

- [ ] **Step 2.3: Verify**

Run:
```bash
grep -c "ONE RULE BEFORE YOU START\|One rule before you start" /Users/michellesmac/development/kealani5000/index.html
grep -c "Rotary AI Guidelines, Section 4" /Users/michellesmac/development/kealani5000/index.html
grep -c "search \"AI Guidelines for Members\"" /Users/michellesmac/development/kealani5000/index.html
grep -c "Before using AI for Rotary work, review Rotary International's AI Guidelines" /Users/michellesmac/development/kealani5000/index.html
```

Expected: `0`, `0`, `0`, `1`.

---

## Task 3: Light pass on 9 starter customize tips

**Files:**
- Modify: `/Users/michellesmac/development/kealani5000/index.html` (lines 413, 423, 433, 443, 474, 545, 566, 576, 617 in current state — line numbers will drift slightly after Task 2 reduces ~5 lines)

**Spec ref:** Section 3 — Starter customize tips, swaps 3.1 through 3.9.

Each step is one Edit call. Apply them sequentially. Do not batch into a multi-replace; each `old_string` is already unique in the file.

- [ ] **Step 3.1: Build a Three-Year Action Plan**

Use Edit on `/Users/michellesmac/development/kealani5000/index.html`:

`old_string`:
```
      <p class="customize"><strong>Customize:</strong> Have your last year's numbers handy — membership, finances, event turnout — so you can answer its questions with specifics.</p>
```

`new_string`:
```
      <p class="customize"><strong>Customize:</strong> Know your club's general shape — rough membership range, what you spent on the big things, how events turned out — so you can answer in ranges, not exact figures or real names.</p>
```

Verify: `grep -c "rough membership range, what you spent on the big things" /Users/michellesmac/development/kealani5000/index.html` → expect `1`.

- [ ] **Step 3.2: Solve a Recurring President-Elect Gap**

Use Edit:

`old_string`:
```
      <p class="customize"><strong>Customize:</strong> Name what you've already tried, honestly. That's what stops the AI from suggesting the obvious things you've already done.</p>
```

`new_string`:
```
      <p class="customize"><strong>Customize:</strong> Describe what you've already tried — what kind of outreach, what kind of person — without naming individuals. That's what stops the AI from suggesting the obvious.</p>
```

Verify: `grep -c "what kind of outreach, what kind of person" /Users/michellesmac/development/kealani5000/index.html` → expect `1`.

- [ ] **Step 3.3: Plan a Board Meeting Worth Attending**

Use Edit:

`old_string`:
```
      <p class="customize"><strong>Customize:</strong> Dump your raw notes in — the AI is good at turning a messy brain-dump into a structured agenda. Add your meeting length so the time blocks are realistic.</p>
```

`new_string`:
```
      <p class="customize"><strong>Customize:</strong> List the topics and decisions on your mind, even messily — without member names or specific financial figures. The AI is good at turning that into a structured agenda. Add your meeting length so the time blocks are realistic.</p>
```

Verify: `grep -c "List the topics and decisions on your mind" /Users/michellesmac/development/kealani5000/index.html` → expect `1`.

- [ ] **Step 3.4: Hand Off a Role Without Losing the Knowledge**

Use Edit:

`old_string`:
```
      <p class="customize"><strong>Customize:</strong> Do this as a conversation over coffee — answer its questions out loud and paste them back in. The "what nobody tells you" details are the real value.</p>
```

`new_string`:
```
      <p class="customize"><strong>Customize:</strong> Do this as a conversation over coffee — answer its questions out loud and paste them back in, keeping names, contact info, and specific financials out of what you type. The "what nobody tells you" details are the real value.</p>
```

Verify: `grep -c "keeping names, contact info, and specific financials" /Users/michellesmac/development/kealani5000/index.html` → expect `1`.

- [ ] **Step 3.5: Win Back Members Who Drifted Away** (highest-risk current wording)

Use Edit:

`old_string`:
```
      <p class="customize"><strong>Customize:</strong> Pick one real person and answer the AI's questions about them. A message written for one person beats a generic "we miss you" blast every time.</p>
```

`new_string`:
```
      <p class="customize"><strong>Customize:</strong> Have one specific person in mind and answer the AI's questions about them — using general details (how long they were in the club, why they joined, what changed) instead of their name or contact info. A message written for a real situation beats a generic "we miss you" blast every time.</p>
```

Verify: `grep -c "instead of their name or contact info" /Users/michellesmac/development/kealani5000/index.html` → expect `1`.

- [ ] **Step 3.6: Run a Post-Event Debrief That Sticks**

Use Edit:

`old_string`:
```
      <p class="customize"><strong>Customize:</strong> Do this within a week of the event while details are fresh. Dump everything in — the AI will sort the signal from the venting.</p>
```

`new_string`:
```
      <p class="customize"><strong>Customize:</strong> Do this within a week of the event while details are fresh. Share what happened in general terms — without naming attendees or sponsors — and the AI will sort the signal from the venting.</p>
```

Verify: `grep -c "without naming attendees or sponsors" /Users/michellesmac/development/kealani5000/index.html` → expect `1`.

- [ ] **Step 3.7: Turn Rough Notes Into a Monthly Newsletter**

Use Edit:

`old_string`:
```
      <p class="customize"><strong>Customize:</strong> Don't polish your notes first — messy bullets are exactly what the AI is good at shaping. Do specify the tone you want.</p>
```

`new_string`:
```
      <p class="customize"><strong>Customize:</strong> Don't polish your notes first — messy bullets are exactly what the AI is good at shaping. Keep names and personal details out until you're ready to add them in yourself. Do specify the tone you want.</p>
```

Verify: `grep -c "Keep names and personal details out until you're ready" /Users/michellesmac/development/kealani5000/index.html` → expect `1`.

- [ ] **Step 3.8: Write a Press Release for a Service Project**

Use Edit:

`old_string`:
```
      <p class="customize"><strong>Customize:</strong> Have the who/what/where/when ready. Let the AI ask what's newsworthy — clubs often lead with the routine and bury the human story.</p>
```

`new_string`:
```
      <p class="customize"><strong>Customize:</strong> Have the what/where/when ready — and describe the people involved by role instead of name until you're ready to add real names in your own edit. Let the AI ask what's newsworthy — clubs often lead with the routine and bury the human story.</p>
```

Verify: `grep -c "describe the people involved by role instead of name" /Users/michellesmac/development/kealani5000/index.html` → expect `1`.

- [ ] **Step 3.9: Feed AI Your Own Documents** (drops NotebookLM reference and quoted rule list)

Use Edit:

`old_string`:
```
      <p class="customize"><strong>Customize:</strong> Stick to material that's already public or that contains no personal, financial, donor, or grant information — past event write-ups, published newsletters, program descriptions, strategic plans. Tools like NotebookLM are built specifically for working from your own documents. If you're not sure something is safe to share, don't upload it.</p>
```

`new_string`:
```
      <p class="customize"><strong>Customize:</strong> Stick to material that's already public or doesn't contain sensitive details — past event write-ups, published newsletters, program descriptions, strategic plans. Some AI tools are designed specifically for working from your own documents. When in doubt, don't upload it.</p>
```

Verify:
```bash
grep -c "NotebookLM" /Users/michellesmac/development/kealani5000/index.html
grep -c "personal, financial, donor, or grant information" /Users/michellesmac/development/kealani5000/index.html
grep -c "doesn't contain sensitive details" /Users/michellesmac/development/kealani5000/index.html
```
Expect: `0`, `0`, `1`.

- [ ] **Step 3.10: Final sweep on Task 3**

Run: `grep -c '<p class="customize">' /Users/michellesmac/development/kealani5000/index.html`

Expected: `22` (same total — we modified 9 in place, didn't add or remove any).

---

## Task 4: Newsletter block — *AI, Actually* rebrand

**Files:**
- Modify: `/Users/michellesmac/development/kealani5000/index.html` (newsletter `.footer-block` near line ~655)

**Spec ref:** Section 4 — Newsletter block

- [ ] **Step 4.1: Confirm exact current block**

Run: `grep -n -A4 "Get the follow-up resources" /Users/michellesmac/development/kealani5000/index.html`

Expected (approximate line numbers, will have shifted slightly after Tasks 2/3):
```html
    <h2>Get the follow-up resources</h2>
    <p>Get general AI tips and techniques you can apply to your Rotary work — added as we find things worth sharing. Not a Rotary-curated or officially scheduled resource. No spam, easy to unsubscribe.</p>
    <a class="cta" href="REPLACE_WITH_SIGNUP_LINK" target="_blank" rel="noopener">Sign up for updates</a>
```

- [ ] **Step 4.2: Apply edit**

Use Edit on `/Users/michellesmac/development/kealani5000/index.html`:

`old_string`:
```
    <h2>Get the follow-up resources</h2>
    <p>Get general AI tips and techniques you can apply to your Rotary work — added as we find things worth sharing. Not a Rotary-curated or officially scheduled resource. No spam, easy to unsubscribe.</p>
    <a class="cta" href="REPLACE_WITH_SIGNUP_LINK" target="_blank" rel="noopener">Sign up for updates</a>
```

`new_string`:
```
    <h2>Get <em>AI, Actually</em> in your inbox</h2>
    <p>Bi-weekly practical AI tips and things to try — for work, Rotary, and everything in between.</p>
    <a class="cta" href="https://aiactuallyhi.substack.com/subscribe" target="_blank" rel="noopener">Subscribe</a>
```

- [ ] **Step 4.3: Verify**

Run:
```bash
grep -c "REPLACE_WITH_SIGNUP_LINK" /Users/michellesmac/development/kealani5000/index.html
grep -c "Get the follow-up resources" /Users/michellesmac/development/kealani5000/index.html
grep -c "Get <em>AI, Actually</em> in your inbox" /Users/michellesmac/development/kealani5000/index.html
grep -c "https://aiactuallyhi.substack.com/subscribe" /Users/michellesmac/development/kealani5000/index.html
```

Expected: `0`, `0`, `1`, `1`.

---

## Task 5: Feedback block — mailto with prefilled subject

**Files:**
- Modify: `/Users/michellesmac/development/kealani5000/index.html` (feedback `.footer-block` immediately after the newsletter block)

**Spec ref:** Section 5 — Feedback block. Body intentionally omits the email address as plain text; the address lives only inside the mailto `href`.

- [ ] **Step 5.1: Confirm exact current block**

Run: `grep -n -A4 "Tell us what worked" /Users/michellesmac/development/kealani5000/index.html`

Expected:
```html
    <h2>Tell us what worked (and what didn't)</h2>
    <p>Tried one of these with your club? We'd love to hear how it went and what you'd want next.</p>
    <a class="cta secondary" href="REPLACE_WITH_FEEDBACK_LINK" target="_blank" rel="noopener">Share your feedback</a>
```

- [ ] **Step 5.2: Apply edit**

Use Edit on `/Users/michellesmac/development/kealani5000/index.html`:

`old_string`:
```
    <h2>Tell us what worked (and what didn't)</h2>
    <p>Tried one of these with your club? We'd love to hear how it went and what you'd want next.</p>
    <a class="cta secondary" href="REPLACE_WITH_FEEDBACK_LINK" target="_blank" rel="noopener">Share your feedback</a>
```

`new_string`:
```
    <h2>Questions? Feedback? Get in touch.</h2>
    <p>Share what's working, what's not, or ask us anything — we read every message.</p>
    <a class="cta secondary" href="mailto:info@kealanisolutions.com?subject=AI%20Starters%20%E2%80%94%20feedback%20or%20question">Email us</a>
```

Note: dropped `target="_blank" rel="noopener"` from the mailto link — these attributes don't make sense for `mailto:` (no new tab to open; rel="noopener" is browser-tab safety).

- [ ] **Step 5.3: Verify**

Run:
```bash
grep -c "REPLACE_WITH_FEEDBACK_LINK" /Users/michellesmac/development/kealani5000/index.html
grep -c "Tell us what worked" /Users/michellesmac/development/kealani5000/index.html
grep -c "Questions? Feedback? Get in touch." /Users/michellesmac/development/kealani5000/index.html
grep -c "mailto:info@kealanisolutions.com?subject=" /Users/michellesmac/development/kealani5000/index.html
```

Expected: `0`, `0`, `1`, `1`.

Also confirm the email address does NOT appear in the body text:
```bash
grep -c "info@kealanisolutions.com" /Users/michellesmac/development/kealani5000/index.html
```
Expected: `1` (only the one occurrence inside the `mailto:` href; not in body text).

---

## Task 6: Whole-file sanity sweep

**Files:** none modified — read-only verification

**Spec ref:** "What's explicitly NOT changing" section of spec. This task makes sure we didn't accidentally touch anything we shouldn't.

- [ ] **Step 6.1: Confirm starter prompt count unchanged**

Run: `grep -c '<div class="prompt">' /Users/michellesmac/development/kealani5000/index.html`

Expected: `22` (3 in Start Here + 4 per topic section × 5 topic sections − 1 absent + 4 in Taking It Further; equals the count from before edits).

Actually count exactly: `grep -c '<div class="prompt">' /Users/michellesmac/development/kealani5000/index.html` — compare to the pre-edit count by running `git show HEAD:index.html | grep -c '<div class="prompt">'`.

Expected: pre and post counts match.

- [ ] **Step 6.2: Confirm 22 customize tips total**

Run: `grep -c '<p class="customize">' /Users/michellesmac/development/kealani5000/index.html`

Expected: `22`. (Same total as before — we modified 9 in place, didn't add/remove any.)

- [ ] **Step 6.3: Confirm footer is untouched**

Run: `grep -n -A6 'footer class="fine"' /Users/michellesmac/development/kealani5000/index.html`

Expected exactly:
```html
  <footer class="fine">
    <p>Made for Rotary club leaders &amp; officers · Built during our AI training session</p>
    <p>A resource from Kealani Solutions — not affiliated with or endorsed by Rotary International.</p>
    <p>Starters on this page were developed with AI assistance and reviewed by Kealani Solutions.</p>
  </footer>
```

- [ ] **Step 6.4: Confirm org-line below hero is untouched**

Run: `grep -c "A resource from Kealani Solutions — not affiliated with or endorsed by Rotary International." /Users/michellesmac/development/kealani5000/index.html`

Expected: `2` (one in `.org-line` under hero, one in footer).

- [ ] **Step 6.5: Confirm CSS block is untouched**

Run: `grep -c "footer.fine\|\.notice\|\.org-line" /Users/michellesmac/development/kealani5000/index.html`

Expected: `7` (footer.fine + footer.fine p + footer.fine p:last-child + .notice + .notice .notice-label + .notice .cite + .notice a + .org-line — minus matches that grep collapses; an exact value will depend on regex behavior, but the count should be > 5 confirming the rules are still present). If unsure, run `sed -n '/^<style>/,/^<\/style>/p' /Users/michellesmac/development/kealani5000/index.html | wc -l` and confirm it matches the pre-edit byte/line count.

- [ ] **Step 6.6: Confirm JS block is untouched**

Run: `grep -c "document.querySelectorAll('.copy-btn')" /Users/michellesmac/development/kealani5000/index.html`

Expected: `1`.

---

## Task 7: Browser verification

**Files:** none modified — visual confirmation only

**Spec ref:** "Verification" section of spec.

- [ ] **Step 7.1: Reload the page and screenshot the top**

Use Playwright (already loaded earlier in session) or a fresh navigation:

```
Navigate to: http://127.0.0.1:8766/index.html
Take screenshot: top-after.png (viewport)
```

Visually confirm:
- Hero lede reads "…for working with AI tools. Find your challenge…" with NO product names
- Below the hero, org-line ("A resource from Kealani Solutions…") still appears
- Amber notice box contains exactly ONE short sentence: "Before using AI for Rotary work, review Rotary International's AI Guidelines and check your district's and club's own policies."

- [ ] **Step 7.2: Screenshot full page and spot-check the 9 customize tips**

Take full-page screenshot. Visually scan or crop the relevant cards to confirm each rewrite reads naturally and renders without HTML escape issues. The 9 cards are:
- Build a Three-Year Action Plan
- Solve a Recurring President-Elect Gap
- Plan a Board Meeting Worth Attending
- Hand Off a Role Without Losing the Knowledge
- Win Back Members Who Drifted Away
- Run a Post-Event Debrief That Sticks
- Turn Rough Notes Into a Monthly Newsletter
- Write a Press Release for a Service Project
- Feed AI Your Own Documents

- [ ] **Step 7.3: Screenshot the footer area**

Take a viewport screenshot of the bottom of the page. Visually confirm:
- Newsletter block heading reads "Get *AI, Actually* in your inbox" with "AI, Actually" rendered in italic
- Newsletter body reads "Bi-weekly practical AI tips and things to try — for work, Rotary, and everything in between."
- Button text is "Subscribe"
- Feedback block heading reads "Questions? Feedback? Get in touch."
- Feedback body reads "Share what's working, what's not, or ask us anything — we read every message."
- Button text is "Email us"
- Footer's three lines still present

- [ ] **Step 7.4: Click-test the Subscribe button**

In Playwright, click the Subscribe `<a>`. Confirm the navigation target equals `https://aiactuallyhi.substack.com/subscribe`. (You can do this via DOM evaluation rather than actually navigating: `document.querySelector('a.cta[href*="aiactuallyhi"]').href`.)

- [ ] **Step 7.5: Click-test the Email us button**

In Playwright, evaluate the href of the Email us `<a>`:

```
document.querySelector('a.cta.secondary').href
```

Expected: `mailto:info@kealanisolutions.com?subject=AI%20Starters%20%E2%80%94%20feedback%20or%20question`

The actual mail-client launch can't be verified in headless browser; the `href` check is sufficient.

---

## Task 8: Commit

**Files:** stages and commits the `index.html` change. Does NOT include the unrelated untracked files (screenshots, PDFs, `.playwright-mcp/`) which are out of scope for this commit.

- [ ] **Step 8.1: Review the diff one final time**

Run: `git -C /Users/michellesmac/development/kealani5000 diff --stat index.html` and `git -C /Users/michellesmac/development/kealani5000 diff index.html | head -200`.

Confirm only `index.html` shows changes and that the diff matches expectations from Tasks 1–5.

- [ ] **Step 8.2: Stage and commit**

Run:
```bash
git -C /Users/michellesmac/development/kealani5000 add index.html
git -C /Users/michellesmac/development/kealani5000 commit -m "$(cat <<'EOF'
Rework starters page: tool-agnostic copy, pointer-only privacy notice, AI Actually newsletter, mailto feedback

Apply brainstorming-approved changes per
docs/superpowers/specs/2026-05-29-rotary-starters-rework-design.md:
- Drop Claude/Gemini/NotebookLM mentions in hero and "Feed AI" tip
- Strip the amber notice down to a single pointer to Rotary guidelines
- Light wording pass on 9 customize tips that previously tempted
  member names or financial specifics
- Rebrand newsletter as "AI, Actually" with Substack subscribe link
- Replace feedback form placeholder with mailto to info@kealanisolutions.com

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

- [ ] **Step 8.3: Confirm commit landed**

Run: `git -C /Users/michellesmac/development/kealani5000 log --oneline -3`

Expected: top entry is the new commit; below it is `1ae84a0 Add design spec for Rotary starters page content rework`; below that is `a625829 Remove .gitignore`.

---

## Out of scope for this plan

The following are deliberately left untouched. Don't address them as part of this plan:

- Prior compliance-fix edits already in working tree (`M index.html` will include both compliance work and rework work when committed) — actually, see note below
- Untracked files: `.playwright-mcp/`, `top-of-page.png`, `full-page.png`, `footer-crop.png`, the two Rotary PDFs
- README updates (the README still mentions the `REPLACE_WITH_*` placeholders that this plan removes; that's a cosmetic doc-debt item to address separately if hosting the page elsewhere)
- Hosted contact form, JS-obfuscated email display, or any other improvement past the spec

### IMPORTANT note on the working-tree state at plan-execution time

When this plan begins, `index.html` already has **uncommitted changes from prior compliance work** sitting in the working tree. The Task 8 commit will therefore bundle BOTH the prior compliance fixes AND the new rework into one commit, which is suboptimal.

**Two clean options for the executor:**

1. **Commit the prior compliance work first** (before starting Task 1) as a separate commit, e.g. with message `"Apply Rotary compliance fixes (Feb 2026 guidelines)"`. Then execute this plan, and Task 8 will produce a clean second commit containing only the rework.
2. **Bundle both into one commit** at Task 8 with a combined message naming both changes.

Option 1 is cleaner history and recommended. If chosen, do it before Step 0.1 and verify the working tree is clean (`git status` shows nothing modified) before proceeding.
