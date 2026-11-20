# Ivy Tech Policy Assistant

> A plain-language policy search engine that lets any Ivy Tech staff member instantly look up institutional policies, procedures, and forms — no manual digging required.

**Ivy Tech IT Challenge · Team 09**

---

## What We Built

Ivy Tech staff across all campuses regularly need to answer student questions on topics ranging from FERPA privacy rules to emergency financial grants. Finding the right answer traditionally meant navigating multiple PDFs or asking a more experienced colleague — a process that could take 8–12 minutes and produce inconsistent results depending on staff tenure.

The **Ivy Tech Policy Assistant** is a lightweight, browser-based chat interface that accepts plain-language questions from staff and returns precise, policy-sourced answers in seconds. It runs entirely client-side from a single HTML file backed by `case_data.js` — no server, no database, no login required.

Staff type a question like _"Can I release grades to a parent?"_ or _"What is the emergency grant limit?"_ and receive the exact policy text, effective date, responsible office, step-by-step procedure, and the correct form to use.

| Policies | Procedures | Forms | Staff Profiles |
|----------|------------|-------|----------------|
| 6        | 5          | 4     | 4              |

---

## The Solution

### Policies Covered

| Policy ID | Title                        | Owner Office               | Roles               | Effective    |
|-----------|------------------------------|----------------------------|---------------------|--------------|
| POL-101   | Student Records Privacy      | Registrar                  | All staff           | 2025-07-01   |
| POL-205   | Refunds and Withdrawals      | Bursar                     | Bursar, Advising, Front desk | 2026-08-01 |
| POL-310   | Late Registration            | Registrar                  | Advising, Registrar | 2026-08-15   |
| POL-412   | Accessibility Accommodations | Disability Support Services | All staff          | 2025-09-01   |
| POL-520   | Emergency Grants             | Dean of Students           | Advising, Financial Aid | 2026-01-05 |
| POL-601   | Front Desk Service Standards | Student Services           | Front desk          | 2025-03-01   |

**Notable data points from `case_data.js`:**

- **POL-310** carries two versions — a superseded 2024 record and the current 2026-08-15 update. The assistant always serves the most recent effective date.
- **POL-520** has a campus-specific section (§5.3): Indianapolis staff see an elevated grant limit of **$750** under a pilot through June 2027; all other campuses see the standard **$500**.

### Procedures

Five step-by-step procedures are linked to their parent policies:

| ID    | Title                                       | Policy  | Form  |
|-------|---------------------------------------------|---------|-------|
| PR-01 | Process a course drop with refund           | POL-205 | F-210 |
| PR-02 | Approve late registration                   | POL-310 | F-115 |
| PR-03 | Refer a student for accommodations          | POL-412 | None  |
| PR-04 | Help a student apply for an emergency grant | POL-520 | F-520 |
| PR-05 | Release a record to a parent                | POL-101 | F-210 |

### Forms

| Form ID | Form Name                              | Purpose                                            | Where to Find                          |
|---------|----------------------------------------|----------------------------------------------------|----------------------------------------|
| F-115   | Late Registration Approval             | Instructor approval to add a class after it starts | Staff intranet, Registrar forms        |
| F-210   | Student Consent to Release Information | Student permission to share records with someone   | Staff intranet, Registrar forms        |
| F-330   | Refund Exception Request               | Medical or military refund exceptions              | Staff intranet, Bursar forms           |
| F-520   | Emergency Grant Application            | Short-term financial help for students             | Staff intranet, Dean of Students forms |

### Staff Profiles

Four representative profiles span the roles and campuses the assistant serves:

| ID  | Name          | Role                    | Department       | Campus       |
|-----|---------------|-------------------------|------------------|--------------|
| E01 | Jamie Cole    | Front desk              | Student Services | Indianapolis |
| E02 | Sam Ortiz     | Advisor                 | Advising         | Fort Wayne   |
| E03 | Taylor Nguyen | Bursar specialist       | Bursar           | Lafayette    |
| E04 | Chris Patel   | Financial aid counselor | Financial Aid    | Indianapolis |

---

## The AI Approach

The assistant uses a **keyword-intent scoring engine** — a deliberate, explainable design choice for a compliance-sensitive environment.

### How it works

1. **Tokenise** — The user's question is lowercased, stripped of punctuation, and split into tokens.
2. **Score** — Every policy in `case_data.js` is mapped to a curated keyword set. The engine scores each policy against the question tokens; the highest score wins.
3. **Shape the answer** — Supplementary keyword sets detect whether the user wants a procedure (`"how do I"`, `"steps"`) or a form (`"which form"`, `"paperwork"`), and the response is assembled accordingly from the matching policy, procedure, and form records.
4. **Conversation context** — The engine tracks the last answered policy. Short follow-up questions (`"what form?"`, `"and then?"`) receive a score boost on the previous topic so the conversation stays coherent across multiple turns.
5. **Campus awareness** — When `"Indianapolis"` appears in the question, the engine appends the campus-specific POL-520 §5.3 pilot record alongside the general answer.

### Why keyword intent (not a neural LLM)

Every answer cites a **policy ID, section, and effective date** drawn directly from `case_data.js`. There are no hallucinated policies — if a topic isn't in the data, the assistant says so and lists what it does cover. This makes the system inherently auditable: compliance officers can trace any staff action back to the exact policy version that was served.

---

## Innovation & Future Readiness Pillars

### 1. Zero Infrastructure Barrier
The entire solution runs in a single HTML file with a companion `case_data.js` file. No backend, no cloud dependency, no procurement cycle. Any campus can deploy it by placing two files on an intranet page.

### 2. Data–Logic Separation
Policy content lives in `case_data.js` — four clean arrays (`policies`, `procedures`, `forms`, `staff`) — entirely separate from application logic. When a policy changes, only the data file needs updating. No code redeploy. This makes it maintainable by non-engineers and ready for a CMS-driven data pipeline.

### 3. Explainable & Auditable AI
Every answer surfaces the policy ID, section title, effective date, and owning office. Staff and compliance officers can verify the source of any answer. The system never presents a confident guess — it presents sourced facts.

### 4. Upgrade Path to Generative AI
The keyword intent engine is an intentional first step. The same `case_data.js` data layer is immediately consumable by a large language model via RAG (retrieval-augmented generation). Swapping the query engine for an LLM-backed one is an isolated change — the UI, data, and KPI layer stay identical.

### 5. Conversational UX
Pre-built starter chips lower the barrier for staff who don't know what to ask. Post-answer chips ("Show me the steps", "What form do I need?") guide users deeper into the same topic without rephrasing — a pattern borrowed directly from enterprise AI assistant design.

---

## Strategic Alignment

| Goal | How the Assistant Delivers |
|------|---------------------------|
| **Productivity at scale** | 3 minutes saved per answered question (tracked live in the KPI banner). Across 19 campuses this compounds to hundreds of recovered staff-hours per week. |
| **Consistency of service** | Every staff member — tenured advisor or first-week hire — receives the same authoritative, policy-sourced answer. Reduces liability and builds student confidence. |
| **Equity across campuses** | Smaller campuses without on-site policy specialists gain the same institutional knowledge base as large ones. |
| **Digital-first workforce** | Embedding an AI-assisted tool in daily workflows normalises human-AI collaboration in a low-risk, high-trust environment — building readiness for broader AI adoption. |

---

## How IBM Bob Was Used

IBM Bob served as the team's AI engineering co-pilot throughout the project, accelerating development from concept to working prototype.

| Task | Bob's Role |
|------|-----------|
| **Solution architecture** | Designed the data–logic separation pattern — `case_data.js` as a standalone content layer decoupled from the application. |
| **Query engine** | Authored the intent-scoring engine, conversation context tracking, follow-up detection, and campus-aware branching — the core AI of the assistant. |
| **Data modelling** | Defined the schemas for `policies`, `procedures`, `forms`, and `staff` arrays and generated the `case_data.js` file that bridges data to the browser. |
| **UI & UX** | Built the full chat interface — bubble layout, animated message rendering, starter chips, post-answer quick-action chips, and the live KPI banner. |
| **Impact analysis** | Generated the impact statement (who benefits, metric projections, rollout roadmap to 2030, risk analysis) directly from reading the codebase. |
| **Documentation** | Synthesised all project artifacts into this README, grounded entirely in the actual `case_data.js` file — no assumptions. |

> Bob operated as a code-first partner. Every output was grounded in the actual files, never speculative. The team directed the problem framing and policy domain expertise; Bob translated that into working, maintainable code.

---

## Data Structure Reference

All content is served from `window.CASE_DATA` in `case_data.js`:

```js
window.CASE_DATA = {
  policies:   [ /* 11 sections across 6 policy IDs */ ],
  procedures: [ /* 5 procedures with pipe-delimited steps */ ],
  forms:      [ /* 4 forms */ ],
  staff:      [ /* 4 profiles */ ]
};
```

| Array        | Records                         | Key Fields |
|--------------|---------------------------------|------------|
| `policies`   | 11 sections / 6 policy IDs      | `policy_id`, `section`, `section_title`, `effective_date`, `owner_office`, `applies_to_roles`, `applies_to_campus`, `text` |
| `procedures` | 5                               | `procedure_id`, `title`, `policy_id`, `steps` (pipe-delimited), `form` |
| `forms`      | 4                               | `form_id`, `form_name`, `purpose`, `where_to_find` |
| `staff`      | 4                               | `staff_id`, `name`, `role`, `department`, `campus` |

---

_Ivy Tech IT Challenge · Team 09 · November 2026_
