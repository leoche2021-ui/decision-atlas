# Durable planning records

For a local map, use `planning/<project-name>/MAP.md` and `tickets/<stable-id>.md`. Choose a location consistent with the repository. Keep file identifiers stable when titles change. The following is a format example, not a requirement to create these exact tickets.

## Map example

```markdown
# Import Architecture

## Destination
Choose an import architecture for the first release.

## Acceptance criteria
Agree on source formats, failure recovery, and ownership of rejected rows.

## Constraints and owner
No new paid infrastructure. Product owner approves workflow tradeoffs.

## Decisions so far
- [Input format](tickets/input-format.md): CSV is the initial supported format.

## Open work
- [Failure recovery](tickets/failure-recovery.md): waiting for owner input.

## Not yet specified
How support will investigate repeated customer failures.

## Out of scope
Continuous synchronization: the first release is a manual import.

## Accepted uncertainty and follow-up
None yet.
```

## Ticket example

```markdown
---
id: failure-recovery
title: Failure recovery
status: waiting
mode: discussion
blocked_by: [input-format]
claimed_by: null
claimed_at: null
decision_owner: product-owner
updated_at: "2026-10-24T12:00:00Z"
---

## Question
Should a malformed row reject the entire upload or only that row?

## Acceptance criteria
The owner selects a recovery behavior and accepts its support implications.

## Evidence and alternatives
Whole-upload rejection simplifies atomicity. Partial import needs a durable
rejection report and a way to avoid duplicating successful rows on retry.
These are design implications, not measured customer preferences.

## Proposed result
Recommend whole-upload rejection for the first release, pending owner input.

## Next action
Ask the owner whether partial progress is essential for release one.
```

Use `open`, `in_progress`, `waiting`, `resolved`, `cancelled`, or `superseded` consistently. A resolved research ticket supplies facts; it does not imply approval of a separate discussion ticket. For accepted decisions, add a `Resolution` section recording the outcome, rationale, evidence, owner acceptance when required, date, and consequences. For cancellation or supersession, state why and link the successor if one exists.

## Tracker adaptation

Use native parent/child and dependency relationships when available. Otherwise use explicit linked fields in ticket bodies. Map these logical statuses onto the tracker without treating every closed issue as an accepted decision. Record session claims in a dedicated field or comment when assignees cannot represent them.

Before a write, refresh the current record. Preserve concurrent changes and use revision checks when available. After a partial failure, reconcile the saved ticket, status, and index before retrying. Local Markdown requires a single writer or a separate locking mechanism for reliable concurrent claims.

## Resume check

Read the map, refresh open-ticket status and claims, validate dependency links, and identify owner input received since the previous session. Check the selected ticket's evidence and prerequisites before continuing. If no ticket is ready, distinguish completion from waiting, stale claims, missing dependencies, and cycles.
