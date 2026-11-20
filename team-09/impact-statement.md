# Impact Statement
## Ivy Tech Policy Assistant — Case 9: Employee Productivity Assistant
**Team 09 · 2026 Ivy Tech IT Challenge**

---

## Who Benefits

**Primary:** Front-desk staff, advisors, registrar and bursar specialists — anyone who fields student policy questions during the workday.

**Secondary:** Every student those staff serve. Faster, accurate answers mean shorter wait times and fewer students redirected for a follow-up.

---

## The Problem Being Solved

Staff currently pause mid-conversation to hunt through printed handbooks or shared drives for policy answers. A single lookup takes 3–5 minutes and often produces inconsistent results across staff and campuses.

---

## What This Tool Does Differently

The Policy Assistant covers all 6 operational policies in the prototype dataset — covering refunds, records privacy, late registration, accommodations, emergency grants, and service standards. A staff member selects their role on first use, then types a plain-language question and gets only the policy text, procedure steps, and forms relevant to their position — in under 10 seconds, without leaving the conversation or opening a second tab.

**Role-based responses** ensure each staff member sees only what applies to them:

| Role | Policies surfaced |
|---|---|
| Front Desk | Records privacy, refund schedule (view only), accommodations, service standards |
| Advisor | Records privacy, refunds, late registration, accommodations, emergency grants |
| Bursar Specialist | Records privacy, full refund policy incl. exceptions, accommodations |
| Financial Aid Counselor | Records privacy, emergency grants (incl. Indianapolis pilot), accommodations |

---

## Metric Impact

**Seed 2030 target:** Staff hours returned to student-facing work.

- Baseline: ~190 policy lookups per day across 19 campuses (10 per campus)
- Time saved: 3 min × 190 = **~570 staff minutes returned per day**
- Over a 200-day academic year: **~1,900 staff hours returned annually** — no new headcount required

---

## Path to 2030

| Phase | Action |
|---|---|
| **2027 — Pilot** | Deploy at 2–3 campuses via GitHub Pages or intranet; collect usage and accuracy data |
| **2028 — Full data** | Migrate from the 6-policy prototype to the complete Ivy Tech policy library |
| **2029 — LLM upgrade** | Replace keyword engine with IBM watsonx for broader natural-language coverage; complete IT security and PII review |
| **2030 — Statewide** | Embed in staff intranet; role selection tied to HR system so the correct role loads automatically on login |

---

## Key Risk

**Policy drift** — outdated data produces authoritative-sounding wrong answers. Mitigated by citing policy ID and effective date on every answer, and assigning a policy owner per department to maintain the data file.

---

*Prototype — Ivy Tech IT Challenge · Team 09 · Strategic Plan 2030: www.ivytech.edu/about-ivy-tech/college-operations/strategic-plan*
