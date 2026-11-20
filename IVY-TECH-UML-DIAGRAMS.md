# Ivy Tech Staff Policy Agent — UML Diagrams

---

## 1. Use Case Diagram

```
+---------------------------------------------------------------+
|              Ivy Tech Staff Policy Agent System               |
|                                                               |
|   +------------------+      +-----------------------------+  |
|   |  Ask policy       |      |  Filter by role and campus  |  |
|   |  question         |<-----|  (staff.csv)                |  |
|   +------------------+      +-----------------------------+  |
|           |                                                   |
|           v                                                   |
|   +------------------+      +-----------------------------+  |
|   |  Receive AI       |      |  Match policy section       |  |
|   |  answer           |<-----|  (policies.csv)             |  |
|   +------------------+      +-----------------------------+  |
|           |                                                   |
|           v                                                   |
|   +------------------+      +-----------------------------+  |
|   |  View step-by-    |      |  Retrieve procedure steps   |  |
|   |  step procedure   |<-----|  (procedures.csv)           |  |
|   +------------------+      +-----------------------------+  |
|           |                                                   |
|           v                                                   |
|   +------------------+      +-----------------------------+  |
|   |  Verify source    |      |  Resolve form name and      |  |
|   |  link             |<-----|  location (forms.csv)       |  |
|   +------------------+      +-----------------------------+  |
|           |                                                   |
|           v                                                   |
|   +------------------+                                        |
|   |  Escalate case    |                                       |
|   |  if unresolved    |                                       |
|   +------------------+                                        |
|                                                               |
+---------------------------------------------------------------+
         ^                             ^
         |                             |
   [Staff Member]              [Student Services Intranet]
   Jamie Cole                  Source verification target
   Front Desk
   Indianapolis
```

---

## 2. Sequence Diagram

```
Staff Member        Policy Agent        policies.csv     procedures.csv    forms.csv        staff.csv        Intranet
     |                   |                   |                 |               |                |               |
     |--Ask question----->|                   |                 |               |                |               |
     |                   |--Lookup role--------------------------------------->|               |               |
     |                   |<--role + campus--------------------------------------------|       |               |
     |                   |                   |                 |               |                |               |
     |                   |--Match policy + filter role/campus->|               |                |               |
     |                   |<--Matched policy sections + policy_id---------------|                |               |
     |                   |                   |                 |               |                |               |
     |                   |--Find linked procedure------------->|               |                |               |
     |                   |<--Steps + form_id-----------------------------------|                |               |
     |                   |                   |                 |               |                |               |
     |                   |--Resolve form-----------------------------+--------->|               |               |
     |                   |<--form_name + where_to_find------------------------------|           |               |
     |                   |                   |                 |               |                |               |
     |<--Part 1: AI answer + procedure steps-|                 |               |                |               |
     |                   |                   |                 |               |                |               |
     |                   |--Build source link--------------------------------------------------->|              |
     |                   |<--Verified URL----------------------------------------------------|               |
     |                   |                   |                 |               |                |               |
     |<--Part 2: Source link (policy + procedure + form location)                               |               |
     |                   |                   |                 |               |                |               |
     |--Click to verify--------------------------------------------------|---->|               |               |
     |<--Original policy or form confirmed-------------------------------|                     |               |
     |                   |                   |                 |               |                |               |
```

---

## 3. Component Diagram

```
+---------------------------------------------------------------------+
|                          DATA LAYER                                 |
|                                                                     |
|  +----------------+  +------------------+  +--------------------+  |
|  | policies.csv   |  | procedures.csv   |  | forms.csv          |  |
|  | POL-101        |  | PR-01 to PR-05   |  | F-115 F-210        |  |
|  | POL-205        |  | Steps pipe-sep   |  | F-330 F-520        |  |
|  | POL-310        |  | Linked to policy |  | where_to_find      |  |
|  | POL-412        |  | and form         |  +--------------------+  |
|  | POL-520        |  +------------------+                          |
|  | POL-601        |                                                 |
|  +----------------+  +------------------+                          |
|                       | staff.csv        |                          |
|                       | Role + Campus    |                          |
|  +----------------+   | Indianapolis     |                          |
|  | case_data.js   |   | Fort Wayne       |                          |
|  | window.CASE_   |   | Lafayette        |                          |
|  | DATA (browser) |   +------------------+                          |
|  +----------------+                                                 |
+---------------------------------------------------------------------+
                              |
                              v
+---------------------------------------------------------------------+
|                         AGENT CORE                                  |
|                                                                     |
|  +--------------------+        +-------------------------------+    |
|  | Role + Campus      |------->| Query Matcher                 |    |
|  | Filter             |        | Maps plain language to        |    |
|  | (staff.csv)        |        | policy_id + procedure_id      |    |
|  +--------------------+        +-------------------------------+    |
|                                              |                      |
|                                              v                      |
|                               +-------------------------------+    |
|                               | Procedure Walker              |    |
|                               | Splits steps on pipe          |    |
|                               | Presents as numbered list     |    |
|                               +-------------------------------+    |
|                                              |                      |
|                                              v                      |
|                               +-------------------------------+    |
|                               | Form Resolver                 |    |
|                               | Matches form_id to            |    |
|                               | form_name + where_to_find     |    |
|                               +-------------------------------+    |
+---------------------------------------------------------------------+
                              |
                              v
+---------------------------------------------------------------------+
|                         OUTPUT LAYER                                |
|                                                                     |
|  +--------------------+  +------------------+  +----------------+  |
|  | AI Answer          |  | Source Citation  |  | Escalation     |  |
|  | Plain language     |  | policy_id        |  | Flag           |  |
|  | explanation        |  | section          |  | Routes to      |  |
|  | with procedure     |  | procedure title  |  | staff role     |  |
|  | steps              |  | form location    |  | and campus     |  |
|  +--------------------+  +------------------+  +----------------+  |
+---------------------------------------------------------------------+
```

---

## 4. Response Format (Output Specification)

Every agent response follows this two-part structure:

```
+----------------------------------------------------------+
|  PART 1 — AI Answer                                      |
|                                                          |
|  [Plain language explanation]                            |
|  [Numbered procedure steps]                              |
|  [Campus-specific rule flagged if applicable]            |
|  [Effective date shown for currency]                     |
|                                                          |
+----------------------------------------------------------+
|  PART 2 — Verify the Source                              |
|                                                          |
|  Policy:    [policy_id] § [section] — [section_title]   |
|             Effective: [effective_date]                  |
|             → [intranet path]                            |
|                                                          |
|  Procedure: [procedure_id] — [title]                     |
|             → [intranet path]                            |
|                                                          |
|  Form:      [form_id] — [form_name]                      |
|             → [where_to_find]                            |
+----------------------------------------------------------+
```

---

## 5. Data Relationship Diagram

```
staff.csv                policies.csv              procedures.csv
+----------+             +------------+            +--------------+
| staff_id |             | policy_id  |<-----------| policy_id    |
| name     |             | title      |            | procedure_id |
| role     |--filters--->| section    |            | title        |
| campus   |             | text       |            | steps        |
+----------+             | applies_to_|            | form         |
                         | roles      |            +--------------+
                         | applies_to_|                   |
                         | campus     |                   | references
                         | effective_ |                   v
                         | date       |            forms.csv
                         +------------+            +--------------+
                                                   | form_id      |
                                                   | form_name    |
                                                   | purpose      |
                                                   | where_to_find|
                                                   +--------------+
```
