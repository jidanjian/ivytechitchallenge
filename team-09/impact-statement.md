# 2030 Impact Statement
## Ivy Tech Policy Assistant — Case 9: Employee Productivity Assistant
**Team 09 · 2026 Ivy Tech IT Challenge**

---

## Who Benefits

**Primary:** Ivy Tech student services staff — front-desk advisors, registrar specialists, bursar staff, financial aid counselors, and advising staff across all campuses.

**Indirect:** Every student those staff members serve. Faster, more confident answers mean shorter wait times, more time spent on the actual advising conversation, and fewer students sent away with the wrong information.

---

## Which Metric Moves — and by How Much

**Seed 2030 Metric:** Staff hours returned to student-facing work.

| Current State | With Policy Assistant | Improvement |
|---|---|---|
| ~3–5 min per policy lookup | Under 10 seconds | ~95% reduction in lookup time |
| Staff interrupt the conversation to search | Answer surfaced mid-conversation | Zero conversation breaks |
| Inconsistent answers across staff/campuses | Consistent, policy-grounded answers | Reduced error and rework |

**Conservative estimate at scale:**
- Ivy Tech serves ~160,000 students across 19 campuses.
- If student services staff handle even 10 policy questions per day per campus, that is ~190 questions per day statewide.
- Saving 3 minutes per question = **~570 staff minutes (9.5 hours) returned to students per day** across the system.
- Over a 200-day academic year: **~1,900 staff hours returned annually** — without adding a single staff member.

**Secondary metric:** Student response time (POL-601 target: email response within 1 business day). Faster policy lookups reduce the time between a student's question and a staff member's confident reply.

---

## What It Would Take to Roll This Out Statewide by 2030

| Step | What is needed |
|---|---|
| **Phase 1 — Pilot (2027)** | Deploy the current prototype at 2–3 campuses. Collect usage data: questions asked, response time, staff satisfaction. Requires: IT approval for GitHub Pages hosting or intranet deployment, faculty/staff onboarding session. |
| **Phase 2 — Expand data (2028)** | Connect to the full Ivy Tech policy library (not just the 6 prototype policies). Requires: a structured policy data export from the existing policy management system, or a one-time data migration by a small team. |
| **Phase 3 — LLM upgrade (2029)** | Replace keyword scoring with an IBM watsonx or similar on-premises LLM for broader natural-language coverage. Requires: IT security review, API key management, and a staff data privacy review to confirm no student PII is processed. |
| **Phase 4 — Statewide (2030)** | Embed in the existing staff intranet or mobile app. Tie into the HR system for role-based policy filtering (e.g. front-desk staff see only relevant policies). Requires: standard IT procurement and change management. |

---

## The One Risk We Would Watch

**Policy drift.** The tool's answers are only as current as the data it reads. If a policy changes and `case_data.js` is not updated, staff will get outdated answers with apparent authority. 

**Mitigation:** In Phase 1, assign one policy owner per department to maintain the data file. In Phase 2+, automate a sync from the policy management system. Every answer already cites the policy ID and effective date, so staff can spot-check currency.

---

## The A-Ivy Alignment

> *"AI supports people and does not replace them. Ground answers in approved sources, give users a path to a human, protect personal data, keep a person in charge of high-impact decisions, and design for accessibility."*

This tool was designed from the first line of code with these principles:
- **Grounded in approved sources** — every answer comes directly from official policy text, cited by ID and date.
- **Path to a human** — answers surface the responsible office and procedure owner, not just the rule.
- **No personal data** — zero student or employee information is collected, stored, or processed.
- **Human in charge** — the tool informs; staff decide. No automated actions.
- **Accessible by design** — plain-language answers, keyboard-navigable UI, no images required for comprehension.

---

*Prototype — Ivy Tech IT Challenge · Team 09 · Strategic Plan 2030: www.ivytech.edu/about-ivy-tech/college-operations/strategic-plan*
