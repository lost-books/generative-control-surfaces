---
Author: Codex GPT-6 Astra Light
Name: generative-control-surface
Description: Create or update a task-specific interface connected to ongoing AI work, in chat environments with or without Codex. Use when invoked as CONTROL SURFACE, Generative Control Surface, or Generative Control Surfaces, or when the user requests a shared interactive workbench alongside conversation. Not for an explanatory visualization alone or an unrelated standalone application.
---

# Generative Control Surface

Give ongoing work a small interface: a place to inspect information, change decisions, and direct what happens next while conversation continues. The system can revise the interface as needs change, while retaining the work behind it.

Invocation examples: “CONTROL SURFACE for this task” or “Generative Control Surface: compare these options.” Use the current conversation to determine the task. Ask one focused question only if the intended work cannot be inferred.

## Build around the work

Identify what would be easier to inspect or manipulate through controls. Generate the smallest useful interface for that purpose, using existing task records where available. Do not create a second competing source of truth or a dashboard merely to display activity.

Keep task data separate from presentation in a task-specific workspace folder. Record stable item identities, revisions, user decisions, and any results needed to continue. Preserve existing records when changing the layout or reopening the surface. State explicitly which persistence boundary is supported; a file surviving does not mean a server remains running.

Use plain labels and visible feedback. Distinguish unsaved edits, saved or submitted decisions, and work actually completed. Include only states relevant to the task. Sorting, filtering, selection, and other routine interactions should not require model calls. Batch changes when useful.

## Choose the available connection

Check what the current environment can actually render, retain, and exchange. Use the simplest working path. Do not assume APIs from another platform are available.

- Host-supported artifacts or widgets: use the available documented tools for rendering, storage, submissions, and updates. Verify each needed capability separately. A normal browser page does not automatically have access to widget hooks.
- Local workspace and browser: use a small local page and shared state service, following the optional local pattern below.
- Chat with generated attachments but no live connection: provide an interactive file with structured export/import. Import submitted records into the ongoing task and return an updated file or acknowledgment. Label the exchange as manual.
- Text-only chat: provide a compact numbered decision table and accept choices by stable ID. Return a structured state snapshot for reuse. Describe this as a text fallback, not a functioning graphical interface or durable external storage.

A shared chat or shared link does not by itself establish shared storage, participant permissions, or synchronized edits. Claim collaborative behavior only when the host supports it and it has been verified. Otherwise use explicit submitted snapshots and reconcile conflicting revisions.

Saving input does not necessarily start a system turn. State the actual behavior in the surface. When work resumes, read pending submissions, perform authorized task work, and record acknowledgments or results. Track submission identities so completed work is not repeated. Never label a pending submission “applied.” If automatic execution is requested, use an available execution connection and test it separately.

## Optional local pattern, including Codex

Use this only where filesystem access, local execution, and a browser are available. It requires a running local service; opening a shared chat elsewhere does not make that service accessible.

Serve the interface and JSON state endpoints from the same loopback origin, bound to `127.0.0.1`. Serve only the surface's files, validate write requests and their origin, and expose no arbitrary file access or command execution.

Give browser and system updates the same revision-checked write path. Reject stale writes with a visible message while preserving unsaved edits. Use atomic file replacement and serialize read-modify-write operations; do not let the system bypass the service by overwriting the live state file. Keep decisions separate from execution results.

Let the page retrieve updates without overwriting unsaved drafts. Show connection failures. Keep the service running through an available managed process and record its address and restart procedure with the task files. Saving through this pattern alone does not wake the system; pending input is read on a later conversation turn.

In Codex desktop, when available, `mcp__codex_app__open_in_codex` can open the local URL as a browser target with right placement. It opens the page; it does not provide the state connection. In other environments, use their supported browser or artifact-opening mechanism.

## Validation tests

Run these checks for each new connection and after changes affecting it. Use isolated test records and a harmless test action. Never test by executing a real external action. Reuse prior evidence for unchanged behavior instead of repeating a full suite for cosmetic edits.

| Test | Procedure and pass criterion |
| --- | --- |
| Render and control | Open the actual surface and change a control. The visible value changes correctly; unrelated choices remain intact. For text fallback, verify an ID-based reply updates only the named record. |
| Draft versus submission | Change a value without submitting. It is not recorded as a submitted decision or executed work. Save or submit explicitly and verify the indicated state matches the stored record. |
| Round trip | Submit a unique test ID and value through the real input path. Read both through the system's path, record an acknowledgment, and confirm the surface shows that acknowledgment for the same submission. For manual exchange, test export, import, and the returned update. |
| Continuity | Save choices, reload or reconstruct the surface, and verify those choices and IDs survive. Test a later session only if session persistence is claimed. For text fallback, reconstruct from the exported snapshot and state that the snapshot must be retained. |
| Conflict handling | Submit two different edits based on the same revision. The second stale write is rejected or explicitly reconciled; it never silently erases the accepted edit. Manual imports must follow the same rule. |
| Execution and replay | Submit a harmless action through the declared trigger path. Verify its result is recorded only after execution. Present the same submission again; completed work is not repeated. Mark this test not applicable if execution controls are absent. |
| Failure feedback | Interrupt the connection or supply invalid input. The surface reports failure and preserves recoverable edits; it does not show a successful save or execution. For manual exchange, reject an invalid import without changing accepted state. |

Record pass, fail, not tested, or not applicable for each check, with brief evidence and the tested revision. Fix failures in advertised capabilities before handover, or disable the affected feature and label the limitation. Never count an unavailable test as a pass.

Use available browser tools according to their instructions. Server tests alone do not establish that browser interaction works. Shared multi-user behavior additionally requires testing two participant contexts against the same record; otherwise leave that capability unverified.

## Hand over

Finish with the surface location, what its controls do, where decisions are retained, and whether further work starts automatically or on a later conversation turn. Keep the explanation short. Refer to the assistant as “the system.” Creating a surface does not authorize unrelated external actions.
