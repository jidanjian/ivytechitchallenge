# Ivy Tech Policy Assistant

> A plain-language policy search engine for Ivy Tech staff — built entirely on the data in `case_data.js`

**Ivy Tech IT Challenge · Team 09**

---

## At a Glance

| Policies | Procedures | Forms | Staff Profiles |
|----------|------------|-------|----------------|
| 6        | 5          | 4     | 4              |

---

## Policies Covered

The assistant answers questions across 11 policy sections spanning 6 policy IDs. All sections carry an effective date and owning office so every answer is sourced and traceable.

### POL-101 — Student Records Privacy
**Owner:** Registrar · **Roles:** All staff · **Effective:** 2025-07-01

- **§1.2 Releasing information** — Staff may release directory information unless a student has requested confidentiality. Grades, schedules, and financial details are never released without the student's signed consent on form F-210, except to college officials with a legitimate need.
- **§1.4 Verifying identity** — Before discussing a record, verify identity with the student ID number and two of: date of birth, address on file, or last four digits of phone. Do not use Social Security numbers.

### POL-205 — Refunds and Withdrawals
**Owner:** Bursar · **Roles:** Bursar, Advising, Front desk · **Effective:** 2026-08-01

- **§2.1 Refund schedule** — Students who drop a 16-week course in the first week receive a 100 percent refund. No refund is given after the first week. 8-week courses follow a shortened schedule in the academic calendar.
- **§2.3 Exceptions** — Refund exceptions for medical or military reasons require form F-330 and documentation, reviewed within 10 business days.

### POL-310 — Late Registration
**Owner:** Registrar · **Roles:** Advising, Registrar · **Latest effective:** 2026-08-15

- **§3.1 Approvals (current)** — Late registration after the first class meeting requires instructor approval through form F-115. After the first week, it is not allowed except for 8-week second-half courses.
- _Superseded version (2024-01-10): required department chair approval after the first week — replaced by the 2026 update._

### POL-412 — Accessibility Accommodations
**Owner:** Disability Support Services · **Roles:** All staff · **Effective:** 2025-09-01

- **§4.2 Requesting accommodations** — Refer students to Disability Support Services. Staff must not ask about a diagnosis. Accommodation letters are sent to instructors by DSS.

### POL-520 — Emergency Grants
**Owner:** Dean of Students · **Roles:** Advising, Financial Aid

- **§5.1 Eligibility** _(all campuses, eff. 2026-01-05)_ — Currently enrolled students facing an unexpected expense of up to $500.00 may apply once per term using form F-520.
- **§5.3 Indianapolis pilot** _(eff. 2026-09-01)_ — At the Indianapolis campus, emergency grants up to $750.00 are available through June 2027 under a pilot program.

### POL-601 — Front Desk Service Standards
**Owner:** Student Services · **Roles:** Front desk · **Effective:** 2025-03-01

- **§6.1 Response times** — Answer phone calls within three rings. Respond to emails within one business day. Escalate unresolved issues to a supervisor after 48 hours.
- **§6.4 Walk-in check-in** — Check in walk-in students using the queue system and give an estimated wait time.

---

## Step-by-Step Procedures

Five procedures are linked to their parent policies. Each defines a sequence of steps and the form required to complete the action.

| ID    | Title                                      | Policy  | Form  | Steps |
|-------|--------------------------------------------|---------|-------|-------|
| PR-01 | Process a course drop with refund          | POL-205 | F-210 | Verify identity → Check drop date → Submit drop → Note refund amount |
| PR-02 | Approve late registration                  | POL-310 | F-115 | Confirm class started → Get instructor approval → Register student → Email confirmation |
| PR-03 | Refer a student for accommodations         | POL-412 | None  | Listen without asking diagnosis → Share DSS contact → Offer to walk to DSS or book appointment |
| PR-04 | Help a student apply for an emergency grant | POL-520 | F-520 | Confirm enrollment → Share form F-520 → Check campus for pilot limits → Send to Dean of Students |
| PR-05 | Release a record to a parent               | POL-101 | F-210 | Verify student consent → Verify parent identity → Release only listed items |

---

## Forms Catalogue

Four official forms are referenced across the policies and procedures. All are located on the Staff intranet.

| Form ID | Form Name                              | Purpose                                       | Where to Find                        |
|---------|----------------------------------------|-----------------------------------------------|--------------------------------------|
| F-115   | Late Registration Approval             | Instructor approval to add a class after it starts | Staff intranet, Registrar forms  |
| F-210   | Student Consent to Release Information | Student permission to share records with someone   | Staff intranet, Registrar forms  |
| F-330   | Refund Exception Request               | Medical or military refund exceptions         | Staff intranet, Bursar forms         |
| F-520   | Emergency Grant Application            | Short-term financial help for students        | Staff intranet, Dean of Students forms |

---

## Staff Profiles

Four representative staff profiles span the roles and campuses that the assistant is designed to serve.

| ID  | Name          | Role                     | Department      | Campus       |
|-----|---------------|--------------------------|-----------------|--------------|
| E01 | Jamie Cole    | Front desk               | Student Services | Indianapolis |
| E02 | Sam Ortiz     | Advisor                  | Advising         | Fort Wayne   |
| E03 | Taylor Nguyen | Bursar specialist        | Bursar           | Lafayette    |
| E04 | Chris Patel   | Financial aid counselor  | Financial Aid    | Indianapolis |

---

## Data Structure

All content is stored in a single `case_data.js` file as a `window.CASE_DATA` object with four top-level arrays. This separation means policy content can be updated without touching any application logic.

| Array        | Records                          | Key Fields |
|--------------|----------------------------------|------------|
| `policies`   | 11 sections across 6 policy IDs  | `policy_id`, `section`, `section_title`, `effective_date`, `owner_office`, `applies_to_roles`, `applies_to_campus`, `text` |
| `procedures` | 5 procedures                     | `procedure_id`, `title`, `policy_id`, `steps` (pipe-delimited), `form` |
| `forms`      | 4 forms                          | `form_id`, `form_name`, `purpose`, `where_to_find` |
| `staff`      | 4 profiles                       | `staff_id`, `name`, `role`, `department`, `campus` |

---

_Ivy Tech IT Challenge · Team 09_
