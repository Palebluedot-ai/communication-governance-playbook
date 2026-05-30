---
title: Global-InScope-Lock Protocol
version: v1
status: active
owner: Bigchao + Hermes
updated_at: 2026-05-01
---

# 0) Purpose
Provide a reusable anti-drift communication protocol that works across channels, threads, and new computers.

# 1) Fixed Response Frame (every important reply)
Assistant must always include:

- **Global**: long-term objective (stable across sessions)
- **InScope (this phase)**: what is being changed now
- **OutOfScope (not now)**: explicit non-goals for this phase
- **Next-1 (Locked)**: one single next action
- **Unlock Condition**: exact phrase from user required to switch phase

# 2) Lock Rules
1. Only one phase can be active at a time.
2. Clarifying questions do not change phase.
3. Phase switch is allowed only if user says one of:
   - "切阶段到 ..."
   - "暂停当前，改做 ..."
   - "重排 roadmap"
4. If no switch phrase, assistant must stay on current Next-1.

# 3) Session Start Handshake (cross-channel / cross-device)
At the first message of any new thread/channel, assistant should run this handshake:

1. Restate Global (1 sentence)
2. Restate current phase + Next-1
3. Ask user to confirm one of:
   - Continue current lock
   - Switch phase explicitly

# 4) Truth Table Requirement
Before any major implementation, produce/update a truth table:

- What is already automated
- What is manual
- What is not implemented
- What is environment-dependent (keys/services/cron)

No major build steps before truth table is confirmed.

# 5) Artifacts (portable)
Keep this protocol portable via repo files:

- `04_protocols/global-inscope-lock-protocol-v1.md` (this file)
- `04_protocols/truth-table-v1.md` (project capability truth)
- `04_protocols/thread-kickoff-template-v1.md` (new thread bootstrap prompt)

# 6) Thread/Channel Kickoff Template
Use this exact opening in a new thread:

```md
[Kickoff]
Global: <one-line long-term objective>
InScope: <current phase scope>
OutOfScope: <what we will NOT do in this phase>
Next-1 (Locked): <single action>
Unlock condition: say "切阶段到 ..." to switch.
Please confirm: Continue lock / Switch phase.
```

# 7) Governance Notes
- This protocol is a governance contract, not a model prompt trick.
- Store in Git (source of truth), not only in chat memory.
- Hermes memory stores preferences; repo protocol stores operational rules.

# 8) Definition of Done for each phase
A phase is done only if:
1. Deliverable file/artifact exists,
2. User confirms acceptance,
3. Next phase lock is explicitly created.
