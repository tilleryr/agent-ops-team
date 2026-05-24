# Onboarding Run — Transcript

A recorded run of the agent ops team onboarding **Marcus Ellison** (Monitoring & Evaluation Specialist, start date 2026-06-15) from the input in [`inputs/new-hire-form.json`](inputs/new-hire-form.json).

This transcript follows the Agent Teams run mode: the lead session embodies **HR** and spawns the other agents as teammates. Artifacts are lightly condensed for readability; the structure and field set match each agent's definition in [`.claude/agents/`](../.claude/agents/).

---

## Phase 0 — HR (lead) ingests the form

> **HR (lead):** Read `demo/inputs/new-hire-form.json`. New hire: Marcus Ellison, M&E Specialist, HQ (Washington, DC), start 2026-06-15, manager Priya Anand. Before opening a record, I need Recruitment to confirm the offer is accepted and hand me a clean packet.

---

## Phase 1 — Recruitment → HR

> **Recruitment (teammate):** Offer verified.

**Accepted-offer packet → HR**

```
onboarding_id:        ONB-2026-0142
candidate_name:       Marcus Ellison
role:                 Monitoring & Evaluation Specialist
start_date:           2026-06-15
hiring_manager:       Priya Anand
location:             Washington, DC — HQ
equipment_preferences: MacBook Pro 14" (macOS), 27" monitor, docking station, wired headset
offer_status:         accepted
```

> All required fields present, `offer_status = accepted`. Selection and the hiring decision were made by the hiring panel; this packet is logistics only. Handing to HR.

---

## Phase 2 — HR opens the record and dispatches four parallel tracks

> **HR (lead):** Record `ONB-2026-0142` opened in the register — state `in-progress`. Dispatching four tracks in parallel, each with a scoped brief. IT and Facilities get no compensation or I-9 data.

| Track | Brief sent | Status |
|---|---|---|
| IT | role, start_date, location, equipment_preferences | dispatched |
| Facilities | role, start_date, location | dispatched |
| Hiring Manager | candidate_name, role, start_date, hiring_manager | dispatched |
| Contracts/Compliance | candidate_name, role, start_date, location | dispatched |

---

## Phase 3 — Specialists return artifacts

### IT → HR

**1. Laptop request**

```
Asset:        MacBook Pro 14" (macOS), 32 GB
Accessories:  Docking station, 27" external monitor, wired headset
Ship to:      HQ IT Desk, Washington, DC — hold for desk 4-118
Needed by:    2026-06-12 (imaged and ready before start)
Asset tag:    to be assigned on imaging
```

**2. Account provisioning list** — *all granted at least privilege; activation date 2026-06-15*

```
- Identity/SSO (Okta)                      — standard staff
- Email: mellison@ibi-usa.com              — standard mailbox
- Microsoft 365 + Copilot (enterprise)     — approved-registry AI tool
- Collaboration (Teams/Slack)              — Programs workspace
- M&E data platform                        — READ only (no PII scope until project assignment)
- VPN                                       — standard remote
- Timekeeping & expense                    — standard staff
```

> Access scoped to the role; no standing elevated access. Any AI tool beyond the approved registry would need IT review before provisioning. Equipment and accounts are queued for a human to execute.

### Facilities → HR

**3. Desk assignment + badge request**

```
Desk:        HQ, 4th floor, desk 4-118 (near Programs pod)
Available:   2026-06-12
Badge:       Standard staff — HQ building + 4th floor
After-hours: not included by default (flag to manager if the role needs it)
Needed by:   2026-06-15
Parking:     waitlist (HQ garage); transit benefit enrollment noted for HR
```

### Hiring Manager → HR

**4. Welcome message** *(draft, for Priya to send)*

> Marcus — welcome to the Programs team. I'm glad you're joining us as our new M&E Specialist. Your first day is Monday, June 15; come to HQ reception at 9:00 and someone will bring you up to the 4th floor. The first week is mostly meeting the team and getting oriented — no deliverables yet. Looking forward to working with you. — Priya

**5. First-week orientation plan + team announcement**

```
First-week orientation
  Day 1   — Reception 9:00; desk + equipment setup with IT; HR welcome; team lunch
  Day 2   — 1:1 with Priya (role, goals, current portfolio); read AI Governance Policy
  Day 3   — Shadow current M&E review; data platform walkthrough (read-only)
  Day 4   — Meet partner-facing leads; intro to active projects
  Day 5   — 1:1 with Priya (questions, 30/60/90 framing)
Early goals
  - Complete all policy acknowledgments by end of week 1
  - Understand the M&E data handling and consent standards before any data access
```

**Team announcement** *(draft, for distribution ~2026-06-15)*

> Please join me in welcoming Marcus Ellison to the Programs team as our new Monitoring & Evaluation Specialist, starting June 15. Marcus will support M&E across our active portfolio. Say hello when you see him on the 4th floor. — Priya

### Contracts/Compliance → HR

**6. Signed-paperwork bundle**

```
Employment agreement   — assembled for M&E Specialist, DC jurisdiction (awaiting signature)
I-9                     — Section 1 by 2026-06-15; Section 2 by 2026-06-18 (within 3 business days)
Background check        — initiated; status pending
Policy acknowledgments  — Code of Conduct, Data Handling & Privacy, AI Governance Policy
```

> Documents assembled from approved templates and routed for signature. I-9 and background-check data are regulated personal data — held in the secured system only, minimum fields, not shared to the IT or Facilities tracks. Legal determinations remain with the human Contracts and Compliance team.

---

## Phase 4 — HR synthesizes the day-1 readiness packet

### 7. Day-1 schedule — Monday, 2026-06-15

```
09:00  Reception (HQ) → escort to 4th floor, desk 4-118
09:30  IT setup — laptop (asset ready), accounts activated, badge issued
10:30  HR welcome — benefits overview, policy acknowledgments started
12:00  Team lunch with Programs
13:30  1:1 with Priya Anand (manager)
15:00  Orientation reading — AI Governance Policy; data handling standards
```

### 8. Readiness summary — for the hiring manager (Priya Anand)

| Track | Artifact | Status |
|---|---|---|
| IT | Laptop + accounts (least-privilege) | Ready — execute by 06-12 |
| Facilities | Desk 4-118 + standard badge | Ready — available 06-12 |
| Hiring Manager | Welcome + orientation + announcement | Drafted — Priya to send |
| Contracts/Compliance | Agreement + I-9 + policy bundle | In progress — **I-9 Section 2 due 06-18** |

**Readiness state: READY** — with one tracked deadline (I-9 Section 2 by 2026-06-18) and two human actions outstanding (manager sends welcome/announcement; new hire signs agreement). No track returned incomplete.

---

## Audit trail

- Every artifact routed through HR; no specialist contacted another directly.
- Least-privilege held on both data (scoped briefs) and access (IT, Facilities).
- No agent took a cross-domain action — the adjacent-tasks clause was not triggered on this run. Had a specialist been unavailable and another picked up its work, that action would appear here, flagged for the lead.
- Record `ONB-2026-0142` retained in the register; it will be marked `closed` after day-1 confirmation, not deleted.
