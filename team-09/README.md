# Ivy Tech Policy Assistant
### 2026 Ivy Tech IT Challenge — Case 9: Employee Productivity Assistant
**Team 09 · Data Analytics, Applied AI & Informatics**

---

## The Problem

Ivy Tech student services staff lose significant time searching for the right policy, procedure, or form before they can help the student sitting in front of them. A front-desk advisor may spend 3–5 minutes per interaction hunting through handbooks, shared drives, or email threads to answer a routine question like *"Can I get a refund if I drop this class?"* or *"What do I need to do for a late registration?"*

This directly hurts two KPIs the institution cares about:

| KPI | Impact |
|---|---|
| **Staff hours returned to students** | Every minute spent on policy lookup is a minute not spent coaching, advising, or problem-solving with the student. |
| **Student response time** | Slow answers frustrate students and erode trust — especially for time-sensitive situations like emergency grants or refund deadlines. |

---

## The Solution

**A single-file, browser-based Policy Assistant** that lets any Ivy Tech staff member type (or click) a question in plain English and receive an accurate, plain-language answer in seconds — with no login, no server, and no internet connection required.

The tool covers the full set of sample institutional policies:

| Policy | Coverage |
|---|---|
| POL-101 | Student Records Privacy (FERPA, identity verification, grade release) |
| POL-205 | Refunds & Withdrawals (refund schedule, medical/military exceptions) |
| POL-310 | Late Registration (instructor approval, 8-week course rules) |
| POL-412 | Accessibility Accommodations (DSS referral, accommodation letters) |
| POL-520 | Emergency Grants (eligibility, Indianapolis campus pilot) |
| POL-601 | Front Desk Service Standards (response times, walk-in queue) |

**Key capabilities:**

- **Natural-language Q&A** — answers policy questions in plain English, not policy-manual jargon.
- **Follow-up memory** — remembers the topic of the last answer so staff can ask "what's the exception?" or "what form do I need?" without repeating context.
- **Quick-action chips** — one-click shortcuts for the most common next step after each answer (procedure steps, required form, start over).
- **Linked procedures & forms** — every policy answer surfaces the related step-by-step procedure and the correct form with its location.
- **Role-based filtering** — staff select their role (Front Desk, Advisor, Bursar Specialist, Financial Aid Counselor) on first use. The tool shows only policies and starter questions relevant to that role, using the `applies_to_roles` field already present in the data.
- **Campus-aware answers** — detects campus-specific rules (e.g. the Indianapolis emergency grant pilot) and surfaces both the general and campus-specific information.
- **Live KPI banner** — tracks questions answered and estimated staff minutes saved during the session, making the productivity impact immediately visible.

---

## Technical Approach

### Architecture

The prototype is a **zero-dependency, single HTML file** (`team-09/index.html`). It runs entirely in the browser with no server, no build step, and no external API calls.

```
team-09/
├── index.html          ← entire application (UI + engine + styles)
└── data/
    ├── case_data.js    ← policy, procedure, form, and staff data (window.CASE_DATA)
    ├── policies.csv
    ├── procedures.csv
    ├── forms.csv
    └── staff.csv
```

### Data Layer

Policy data is loaded via a plain `<script>` tag that exposes `window.CASE_DATA` — a structured JavaScript object containing:
- **11 policy sections** across 6 policies
- **5 step-by-step procedures** linked to policies
- **4 forms** with names, purposes, and locations
- **4 staff records** with roles, departments, and campuses

### Query Engine

The in-browser query engine uses **keyword intent scoring**:

1. **Tokenise** — the user's question is lowercased, stripped of punctuation, and split into tokens.
2. **Score intents** — each policy in the INTENT_MAP is scored by counting keyword hits in the token set. Multi-word phrases (e.g. "front desk") are matched against the joined token string.
3. **Role filter** — after scoring, only policy sections where `applies_to_roles` matches the active role (or is "All staff") are returned. This prevents, e.g., a Front Desk user from seeing Bursar-only exception handling.
4. **Context boost** — if the question is short or contains follow-up phrases ("what about", "steps", "exception"), the last-answered policy gets a score boost (`+5`) so follow-up questions resolve without re-typing context. A strong new-intent score overrides the boost.
5. **Campus detection** — if "Indianapolis" appears in the tokens and the winning policy has a campus-specific section, both the general and campus-specific records are returned.
6. **Format answer** — the winning policy's sections are formatted into plain-language output including rule text, effective date, related procedure steps (on request), and form details.

### Role-Based Filtering

Staff select their role from a full-screen picker on first load. The active role is stored in `currentRole` and controls:

| Role | Sees |
|---|---|
| **Front Desk** | POL-101 (all), POL-205 refund schedule, POL-412, POL-601 |
| **Advisor** | POL-101, POL-205, POL-310, POL-412, POL-520 |
| **Bursar Specialist** | POL-101, POL-205 (all sections incl. exceptions), POL-412 |
| **Financial Aid Counselor** | POL-101, POL-412, POL-520 |

Each role also gets tailored starter chips and a role-specific welcome message. The role badge in the header is clickable to switch roles at any time.

### Follow-up Memory

A `conversationContext` object tracks `lastPolicyId`, `lastProcedureId`, and `lastFormId` after every answer. It is passed to `queryEngine()` on every call and cleared when the user opens the role picker ("Start over").

### UI

Built with vanilla HTML/CSS/JS — no frameworks. Ivy Tech brand blue `#005EB8`. Message bubbles animate in with a 220ms `fadeSlideIn` keyframe. The chip bar re-renders after every answer with contextually relevant one-click shortcuts. A full-screen role picker on load personalises the session before the chat begins.

---

## Pillar & Statewide Strategy

**A-Ivy Focus Area:** Staff productivity and service  
**Strategic Plan 2030 Pillar:** Innovation & Future Readiness  
**Statewide Strategy:** Optimize & Enhance Technology

This tool directly serves the **A-Ivy plan** by using AI to return staff hours to student-facing work. The seed metric is *staff hours returned to students* — by reducing per-interaction policy lookup time from ~3–5 minutes to under 10 seconds, the tool addresses this metric at scale across all Ivy Tech campuses.

The solution is intentionally designed around **responsible AI principles**:
- Answers are grounded in approved policy documents only — no hallucination risk.
- Every answer cites the policy ID and effective date so staff can verify the source.
- A human remains in charge of every decision; the tool surfaces information, not decisions.
- No personal student or employee data is collected or processed.

---

## How IBM Bob Was Used

IBM Bob was used throughout the full development lifecycle of this prototype:

| Phase | How Bob helped |
|---|---|
| **Discover & Define** | Bob turned the user story ("As an Ivy Tech employee, I can get quick answers about policies so I spend more time with students") into a structured build plan with 6 ordered sub-tasks, expected outcomes, and relevant code pointers. |
| **Architecture design** | Bob proposed the single-file, zero-dependency architecture — the right choice for a demo that needs to run offline on any device without setup. |
| **Query engine** | Bob designed and implemented the keyword intent scoring system, the follow-up context boost, the campus-specific detection logic, and the POL-310 effective-date deduplication — all grounded in the actual `case_data.js` schema. |
| **Role-based filtering** | Bob designed and implemented the full role picker UI, `ROLE_CONFIG` data structure, `roleMatches()` filter, role-aware starter chips, role-specific welcome messages, and the header role badge — using the `applies_to_roles` field already present in the dataset. |
| **UI/UX** | Bob built the full chat interface including the KPI banner, animated message bubbles, chip bar, role picker overlay, and start-over flow from a single set of requirements. |
| **Iteration** | Bob identified and fixed keyword gaps in the INTENT_MAP during the demo scenario trace (e.g. the medical refund exception F-330 override, the front desk "time/rules" keywords). |
| **Documentation** | Bob generated this README, the UML component diagram, and the Impact Statement from the completed codebase and challenge brief, and updated all documentation when role-based filtering was added. |

---

## Running the Prototype

1. Clone or download this repository.
2. Open `team-09/index.html` in any modern browser.
3. No install, no server, no internet connection required.

**Suggested demo flow:**

| Step | What to click/type | What it shows |
|---|---|---|
| 1 | Click **"Can I release grades to a parent?"** | Privacy policy + required form + "Show me the steps" chip |
| 2 | Click **"Show me the steps"** | Follow-up memory — resolves PR-05 without re-typing context |
| 3 | Click **"Start over"**, then type **"emergency grant Indianapolis"** | Campus-aware answer — returns both general and Indianapolis pilot rules |
| 4 | Type **"what if it's for medical reasons"** after a refund question | Follow-up context resolves to refund exception (F-330), not the standard rule |

---

*Prototype — Ivy Tech IT Challenge · Team 09*
