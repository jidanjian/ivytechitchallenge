# Policy Assistant — Prototype Plan

## Overview

Build a single-file, browser-only policy assistant that lets Ivy Tech staff ask natural-language questions about institutional policies and get plain-language answers. The tool handles follow-up questions, surfaces related procedures and forms, and provides quick-action shortcuts for repeat tasks. No server, no build step, no API key — it opens in any browser and is shareable via GitHub Pages or a USB stick.

**Primary users:** Front-desk staff and advisors during their daily work; judges/evaluators during the demo.

**KPIs addressed:**
- **Staff hours with students** — faster policy answers mean less time hunting handbooks and more time helping the student in front of them.
- **Response time** — a staff member gets a correct policy answer in seconds, not minutes.

**Data source:** `team-09/data/case_data.js` — 11 policy sections, 5 procedures, 4 forms, 4 staff records. Loaded via `<script>` tag; no fetch or server needed.

---

## Sub-Tasks

---

### Sub-Task 1 — Project scaffold and data wiring

**Status:** `[x] done`

**Intent**
Create the single HTML file at `team-09/index.html` with the correct structure, load `case_data.js`, and verify the data is accessible before any UI or logic is written.

**Expected Outcomes**
- `team-09/index.html` exists and opens in a browser without errors.
- `window.CASE_DATA` is accessible in the browser console.
- The page shows a placeholder heading (e.g. "Policy Assistant — loading…") confirming the scaffold works.

**Todo List**
1. Create `team-09/index.html` with standard HTML5 boilerplate.
2. Add `<script src="data/case_data.js"></script>` to load the data.
3. Add a minimal inline `<script>` that logs `window.CASE_DATA` to the console so the data wire-up can be verified.
4. Add a top-level `<div id="app">` placeholder with a heading.

**Relevant Context**
- Data file: `team-09/data/case_data.js` — exports `window.CASE_DATA` with keys `policies`, `procedures`, `forms`, `staff`.
- No npm, no bundler — everything must be inline or relative-path `<script>`/`<link>` tags.

---

### Sub-Task 2 — Chat UI shell

**Status:** `[x] done`

**Intent**
Build the visual shell of the chat interface: a conversation thread area, a text input with a submit button, and a KPI banner. No logic yet — just the layout and styling so the demo looks polished from the first interaction.

**Expected Outcomes**
- Page renders a clean, professional chat UI with Ivy Tech branding colors (blue `#005EB8`, white, light grey).
- A scrollable conversation thread area is visible.
- A text input and "Ask" button are fixed at the bottom.
- A KPI banner at the top shows two counters: "Questions answered: 0" and "Est. time saved: 0 min".
- The UI is fully responsive (readable on a laptop screen at any common resolution).
- Typing in the input and clicking Ask adds a "user bubble" to the thread (the response logic comes in Sub-Task 3).

**Todo List**
1. Add CSS (inline `<style>`) for the page layout: header/banner, chat thread, input bar.
2. Create the KPI banner element with two counter spans (`#kpi-questions`, `#kpi-minutes`).
3. Create the chat thread `<div id="chat-thread">` with overflow-y scroll.
4. Create the input bar: `<input id="user-input">` + `<button id="ask-btn">Ask</button>`.
5. Add a quick welcome message bubble from "Assistant" that appears on page load, explaining what the tool can help with.
6. Wire the Ask button (and Enter key) to append a user message bubble — no answer logic yet, just bubble rendering.

**Relevant Context**
- Ivy Tech brand blue: `#005EB8`. Use white text on blue, dark grey (`#333`) on white.
- Keep styling inline in `<style>` — no external CSS frameworks (keeps the file self-contained).
- Message bubbles: user messages right-aligned in blue, assistant messages left-aligned in light grey.

---

### Sub-Task 3 — In-browser query engine

**Status:** `[x] done`

**Intent**
Build the core JavaScript query engine that takes a natural-language question string and returns a plain-language answer by matching against `window.CASE_DATA`. This is the intelligence of the tool.

**Expected Outcomes**
- Asking "can I release grades to a parent?" returns the POL-101 privacy rule in plain language.
- Asking "how do I process a late registration?" returns the PR-02 procedure steps.
- Asking "what form do I need for a refund exception?" returns F-330 details.
- Asking about "emergency grant Indianapolis" returns the campus-specific pilot (POL-520 section 5.3).
- Unrecognised questions return a helpful fallback ("I didn't find a match — try asking about refunds, late registration, accommodations, emergency grants, or records.").

**Todo List**
1. Create a `queryEngine(question)` function in an inline `<script>` block.
2. Normalise the input: lowercase, strip punctuation, split into tokens.
3. Define an intent map — a prioritised list of keyword sets mapped to policy IDs, procedure IDs, or form IDs. Cover at least: privacy/records, refunds/withdrawals, late registration, accommodations/disability, emergency grant, front desk/response time.
4. Score each intent by counting keyword hits in the question tokens; return the highest-scoring match.
5. Format the matched record(s) into a plain-language answer string. For policies, include the rule text and the effective date. For procedures, list the steps. For forms, include the form name and where to find it.
6. Handle the campus-specific case: if "Indianapolis" is in the question tokens and the matched policy has a campus-specific section, prefer that section.
7. Return a structured result object: `{ answer: string, relatedProcedure: id|null, relatedForm: id|null, policyId: string|null }`.
8. Connect `queryEngine` to the Ask button handler from Sub-Task 2 so answers appear as assistant bubbles.
9. Update the KPI counters: increment questions answered by 1 and add 3 minutes (estimated lookup time saved) per question.

**Relevant Context**
- Policy data lives in `window.CASE_DATA.policies` — each record has `policy_id`, `section`, `text`, `applies_to_campus`.
- Procedures in `window.CASE_DATA.procedures` link back to policies via `policy_id`.
- Forms in `window.CASE_DATA.forms` have `form_id`, `form_name`, `purpose`, `where_to_find`.
- POL-310 has two records with different `effective_date` values — always use the most recent (2026-08-15).
- POL-520 has a general section and an Indianapolis-specific section — both may be relevant.

---

### Sub-Task 4 — Follow-up context and conversation memory

**Status:** `[x] done`

**Intent**
Make follow-up questions work naturally by remembering the topic of the last answer. A staff member who just asked about refunds should be able to ask "what's the exception?" without repeating the full context.

**Expected Outcomes**
- After an answer about refunds, asking "what about medical reasons?" correctly returns the refund exception rule (POL-205 s2.3).
- After an answer about privacy/records, asking "what form do I need?" returns F-210 without re-asking.
- After an answer about late registration, asking "what are the steps?" returns PR-02.
- Asking an unrelated question resets context correctly to the new topic.
- Asking "start over" or "clear" resets the conversation and context.

**Todo List**
1. Add a `conversationContext` object (module-level variable) tracking: `lastPolicyId`, `lastProcedureId`, `lastFormId`.
2. Update `queryEngine` to accept the current context as a second argument and boost the score of records matching the context topic when no clear new intent is found.
3. After each answer, update `conversationContext` with the result's `policyId`, `relatedProcedure`, and `relatedForm`.
4. Add follow-up phrase detection: short questions containing "what form", "steps", "how do I", "exception", "what about" should trigger context-boosted lookup before falling through to full keyword scan.
5. Add a "Start over" assistant button that clears `conversationContext` and the chat thread (keeping only the welcome message).

**Relevant Context**
- Context is in-memory only — no localStorage needed for the prototype.
- The follow-up boost should not override a strong new-intent match (e.g. the user asking about a completely different policy).

---

### Sub-Task 5 — Quick-action chips

**Status:** `[x] done`

**Intent**
After each assistant answer, surface 2–3 contextual quick-action buttons ("chips") so staff can get the next piece of information with one click instead of typing. This directly reduces the time cost of multi-step lookups and reinforces the KPI story for evaluators.

**Expected Outcomes**
- After a policy answer that has a linked procedure, a chip "Show me the steps" appears.
- After a policy answer that has a linked form, a chip "Which form do I need?" appears.
- After any answer, a chip "Start over" is always present.
- Clicking a chip fires the same query pipeline as typing — the chip label becomes a user bubble and the answer appears normally.
- Chips disappear when the next question is asked and are replaced by chips relevant to the new answer.

**Todo List**
1. Add a `<div id="chip-bar">` below the chat thread and above the input bar.
2. After each assistant answer, call a `renderChips(result)` function that reads `result.relatedProcedure` and `result.relatedForm` to decide which chips to show.
3. Each chip is a `<button class="chip">` whose click handler calls the main Ask handler with the chip's pre-set query string.
4. Clear the chip bar before rendering new chips (so stale chips don't accumulate).
5. The "Start over" chip always renders after any answer.

**Relevant Context**
- Chips should feel like natural next questions, not UI controls — style them as rounded pill buttons in Ivy Tech blue outline with blue text.
- Chip queries should be short plain-language strings that the query engine already handles (e.g. "Show me the procedure for this", "What form do I need?").

---

### Sub-Task 6 — Demo polish and KPI narrative

**Status:** `[x] done`

**Intent**
Finalize the page for a live demo: smooth animations, a clear Ivy Tech header, the KPI banner wired to real counts, and a set of suggested starter questions visible on load so a judge or evaluator can immediately see the range of the tool without needing to know what to type.

**Expected Outcomes**
- The page header shows "Ivy Tech Policy Assistant" with the Ivy Tech logo treatment (text-based, no image dependency).
- The KPI banner accurately reflects the session's question count and estimated minutes saved (3 min × questions).
- 4 starter question chips are shown on load before any question is asked: "Can I release a student's grades to their parent?", "How do I process a late registration?", "What is the emergency grant limit?", "What are the front desk response time rules?".
- Messages animate in smoothly (fade + slide up).
- The page title (`<title>`) is "Ivy Tech Policy Assistant".
- Tested against the 4 key demo scenarios from the overview and all return correct answers.

**Todo List**
1. Update the `<title>` and header to "Ivy Tech Policy Assistant".
2. Style the header with Ivy Tech blue background, white text, and a subtle tagline ("Ask a policy question in plain language").
3. Wire `#kpi-questions` and `#kpi-minutes` to the actual counters updated in Sub-Task 3.
4. Add CSS keyframe animation (`fadeSlideIn`) applied to each new message bubble.
5. Render the 4 starter chips on page load (before any answer exists) using the same `renderChips` mechanism.
6. Manually test all 4 demo scenarios and fix any keyword gaps in the query engine.
7. Add a short `<footer>` with "Prototype — Ivy Tech IT Challenge · Team 09".

**Relevant Context**
- Starter chips replace the chip bar content on load; they are cleared and replaced when the first real answer arrives.
- No images or external fonts — keep the file self-contained for offline demos.
- The 4 key demo scenarios: (1) grades release to parent, (2) late registration steps, (3) emergency grant + Indianapolis campus, (4) refund medical exception.
