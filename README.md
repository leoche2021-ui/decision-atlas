# Decision Atlas

**Turn an ambiguous project into a navigable set of decisions.**

Decision Atlas is an agent skill for planning work that spans multiple conversations. It turns a broad goal into focused questions, connects their dependencies, and preserves the evidence behind each choice. The next session can resume from a durable map instead of reconstructing the discussion.

It is useful for architecture choices, product discovery, migration planning, and any effort where the important uncertainty is what to decide before building. It is unnecessary for a small task whose requirements and implementation path are already clear.

## How it works

1. Define the destination and what must be known to reach it.
2. Separate precise decision questions from uncertainty that still needs exploration.
3. Connect dependencies and select a ready question with useful downstream impact.
4. Gather evidence, compare alternatives, and involve the actual decision owner where needed.
5. Record the result, expose newly answerable questions, and leave a reliable handoff.

The map is an index. Individual tickets hold the detailed rationale. Research establishes facts; discussions establish preferences; prototypes test uncertainty; prerequisites unlock further decisions. These distinctions prevent a technical finding from being mistaken for stakeholder approval.

## Example prompts

```text
Use $decision-atlas to plan our CSV import feature. We need to decide
validation, recovery, and support workflows before implementation.
Keep the map in local Markdown.
```

```text
Use $decision-atlas to resume planning/import/MAP.md. Identify the most
important ready decision, investigate it, and continue until owner input
is required.
```

```text
Use $decision-atlas to compare migration approaches, then implement the
selected approach if it fits the constraints we have already agreed on.
```

## Expected output

A local workflow typically produces:

```text
planning/import/
  MAP.md
  tickets/
    input-format.md
    failure-recovery.md
```

The map records the destination, accepted decisions, open work, unresolved uncertainty, and exclusions. Tickets preserve evidence, alternatives, status, ownership, and next actions. See [the record format and worked example](references/records.md).

An existing issue tracker can hold equivalent records when its integration and the user's authorization permit it. No particular tracker, paid service, or subagent framework is required.

## Installation

Copy this repository's contents into a skill directory named `decision-atlas` in your agent's configured skill location. Keep `SKILL.md`, `references/`, and `agents/` together. Invoke it by name in a host that supports named skills. Discovery and invocation syntax depend on the host.

`SKILL.md` contains the agent instructions. `agents/openai.yaml` supplies optional UI metadata. Other hosts may ignore that metadata. This package uses portable skill frontmatter; it is not a WorkBuddy marketplace upload bundle.

## What changed from decision-map

This English revision replaces the original fixed one-ticket-per-session limit with progress bounded by the user's request and actual dependencies. It removes mandatory research delegation and disposable branches. It adds explicit acceptance criteria, proposed versus accepted outcomes, session-specific claims, recovery after partial writes, and retained supersession history.

Completion is stricter: an empty frontier may mean blocked work, a cycle, or a stale claim. A map is ready only when remaining uncertainty has a clear disposition and no unresolved decision blocks the next phase.

## Limits

Plain Markdown does not provide atomic locking. Serialize concurrent writers or use a tracker with suitable concurrency controls. The skill cannot supply missing stakeholder approval or guarantee that research is complete. It produces a justified planning state, not proof that implementation is finished.

## Provenance

Adapted from the user-supplied `decision-map` skill and substantially revised as Decision Atlas. The supplied materials did not identify an upstream author or license. No new redistribution license is asserted here; upstream attribution and licensing remain unverified.
