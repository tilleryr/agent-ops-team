# Onboarding Workflow

This document describes how the six agents coordinate to take a new hire from accepted offer to day-1 readiness. It is the orchestration spec the demo follows.

## Trigger

A populated [`demo/inputs/new-hire-form.json`](../demo/inputs/new-hire-form.json): candidate name, role, start date, hiring manager, location, and equipment preferences. Nothing downstream runs until Recruitment confirms the offer is accepted.

## The handoff chain

```
Recruitment ─► HR ─┬─► IT                    ─┐
                   ├─► Facilities             ─┤
                   ├─► Hiring Manager         ─┼─► HR (synthesis) ─► Day-1 readiness packet
                   └─► Contracts/Compliance   ─┘
```

HR is the orchestration anchor. Recruitment initiates; the four specialist tracks run in parallel and each returns one structured artifact to HR; HR synthesizes the result. No specialist talks to another — every handoff routes through HR, which keeps the chain auditable through a single coordinator.

## Sequence

1. **Recruitment → HR.** Recruitment verifies `offer_status` is `accepted`, assembles the accepted-offer packet, and hands it to HR.
2. **HR opens the record.** HR writes a "new hire in flight" entry to its persistent register, then dispatches four parallel tracks, each with a scoped brief carrying only the fields that track needs:
   - **IT** — role, start_date, location, equipment_preferences → laptop request + account provisioning list
   - **Facilities** — role, start_date, location → desk assignment + badge request
   - **Hiring Manager** — candidate_name, role, start_date, hiring_manager → welcome message + first-week orientation plan + team announcement
   - **Contracts/Compliance** — candidate_name, role, start_date, location → employment agreement + I-9 packet + policy-acknowledgment bundle
3. **Specialists return artifacts to HR.** Each returns one structured artifact; HR records each on arrival and updates the register.
4. **HR synthesizes.** HR assembles the day-1 schedule and the readiness summary for the hiring manager, and declares the hire `ready` or `blocked`. It will not declare `ready` until every required artifact is present or a track has been explicitly waived by a qualified human.

## Expected outputs (8)

1. Laptop request — IT
2. Account provisioning list — IT
3. Desk assignment + badge request — Facilities
4. Welcome message — Hiring Manager
5. First-week orientation plan + team announcement — Hiring Manager
6. Employment agreement + I-9 packet + policy-acknowledgment bundle — Contracts/Compliance
7. Day-1 schedule — HR (synthesized)
8. Readiness summary for the hiring manager — HR (synthesized)

## How to run it

The same six definitions in [`.claude/agents/`](../.claude/agents/) drive both run modes. Pick the mode by whether the experimental Agent Teams feature is enabled.

### Mode 1 — Agent Teams (experimental)

Requires Claude Code v2.1.32+ and the enable flag already set in [`.claude/settings.json`](../.claude/settings.json) (`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`).

The session you start is the **team lead and embodies HR**, the orchestration anchor. The lead is fixed for the team's lifetime and cannot also be a teammate, which is why HR is the lead rather than a spawned teammate. Teammates are spawned from the subagent definitions in `.claude/agents/`, referenced by name; each teammate honors that definition's `tools` and `model`.

Launch prompt:

```text
You are the HR orchestration anchor for a new-hire onboarding, and the team lead.
Read demo/inputs/new-hire-form.json, then run the onboarding handoff chain:

1. Spawn a teammate from the `recruitment` agent type to confirm the offer is
   accepted and assemble the accepted-offer packet. Do not proceed until it returns.
2. Open the onboarding record. Then spawn teammates from these agent types and
   dispatch each its scoped brief, in parallel: `it`, `facilities`,
   `hiring-manager`, `contracts-compliance`.
3. Collect each returned artifact, then synthesize the day-1 schedule and the
   readiness summary for the hiring manager. Declare the hire ready or blocked.

Follow each agent's definition. Flag any cross-domain action per the
adjacent-tasks clause so it can be audited.
```

### Mode 2 — Subagent fallback (stable)

Works on any current Claude Code with no experimental features. Leave the enable flag off (or run from a session that ignores it).

The **main session acts as HR / orchestrator** and delegates each track to the matching subagent; subagents return their results to the caller. A subagent cannot spawn other subagents, so HR's coordination runs at the top level in this mode. Invoke it by asking Claude to run the onboarding for `demo/inputs/new-hire-form.json`, delegating each track to the matching agent and then synthesizing the readiness packet.

## Least-privilege and audit

- Each specialist receives only the fields its track needs. IT and Facilities never receive compensation or I-9 data.
- Only HR holds delegation authority; the specialists return artifacts to HR and never call one another.
- Any agent that handles work outside its lane flags it under the adjacent-tasks clause so the lead can audit the cross-domain action.
- No record is deleted; closed onboardings are marked closed and retained.
