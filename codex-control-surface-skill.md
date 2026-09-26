---
name: generative-control-surface
description: "Create a shared workbench for an ongoing AI task: generate useful controls, connect them to retained task state, and carry decisions back into the work. Invoke with CONTROL SURFACE or Generative Control Surface(s). Works with available chat, artifact, or workspace capabilities; Codex is optional. Use for directing ongoing work, not merely illustrating an answer."
author: Codex GPT-6 Astra Medium
---

# Generative Control Surface

Create a small interface that lets a person work directly with an ongoing task while continuing the conversation. Changes made through the interface must become available to the system through a clear, tested path. Conversation can also change the interface itself.

The aim is less repeated explanation and more visible control. Build only what the current task needs. Preserve the work when the interface changes.

## Start from the current task

Treat “CONTROL SURFACE” and “Generative Control Surface” (including the plural) as invocations. Infer the purpose from the conversation and existing task records. Ask one focused question only when the intended task cannot be inferred.

Identify the decisions or adjustments the surface should support and what happens after submission. Generate a usable first version rather than stopping at a design proposal. Reuse an existing surface when it already serves the task.

Use familiar controls and plain labels. Keep relevant information near the controls it informs. Keep established decisions available without repeatedly presenting them as unresolved work. Prefer a surface that remains accessible alongside conversation when the host supports it.

## Establish the connection

Check the environment's actual capabilities before building. Prefer an existing task connection or supported artifact runtime. Test rendering, input submission, state retrieval, and updates separately; success or failure in one does not establish the others. A failed approach is evidence about that approach, not proof that the host cannot support a control surface.

Use the smallest supported path:

- Host integration: connect through documented rendering, storage, and submission tools that are actually available.
- Local workspace: use a local page and a small state service when filesystem access and process execution are available. See the local pattern below.
- Graphical file with manual exchange: when automatic exchange is unavailable, create an interactive page or artifact that exports structured decisions with a submission ID and revision. Import them into the task and return an updated artifact or acknowledgment. Demonstrate the complete exchange and label it as manual.

Every successful use of this skill produces a graphical interface with working controls. If the first path fails, inspect adjacent host mechanisms and test a minimal round trip through each plausible path before declaring the task blocked. Report the specific paths tested and their failures if no graphical route works. Do not substitute a text table for the requested surface.

A shared conversation does not establish shared storage or simultaneous editing. Do not assume Codex, a docked panel, or a particular API exists in another host.

## Keep the work consistent

Use existing authoritative task records where possible. Otherwise establish a structured record separate from the layout. Include only what is needed: stable item IDs, a state revision, decisions, submission IDs, and outcomes. Store it outside the transcript when supported; otherwise make the snapshot portable and explain its retention limits.

Make the action sequence visible. Editing changes a draft. Saving retains a value. Submitting makes a decision available for processing. Applying it changes the task or produces a result. Combine buttons when appropriate, but label their actual effects. A receipt proves receipt, not completed work.

Preserve decisions when regenerating the surface. Never overwrite unsaved edits during refresh. Reject or explicitly reconcile stale submissions. Process each submission once; if execution was interrupted and its outcome is uncertain, inspect the result before retrying.

Keep routine interaction deterministic where practical. Sorting or selecting should not invoke the model. Batch changes and involve the system when interpretation or further work is needed.

## Continue the task

State whether submission starts work automatically or waits for another conversation turn. On resuming work, read pending submissions before acting, apply authorized decisions, and return an acknowledgment or result to the same workbench. Record failures as failures and leave unfinished work identifiable.

If conversation changes the underlying task, update the shared record and surface accordingly. Do not silently create competing versions of the work. Opening a surface or submitting a value does not authorize unrelated external actions.

## Optional local pattern

Serve a small HTML page and JSON state endpoints from the same loopback origin, bound to `127.0.0.1`. Restrict file access to the surface's files, validate input and write origins, and expose no arbitrary command execution.

Route both browser and system updates through one revision-checked write path. Serialize updates and replace stored state atomically. Retrieve updates without erasing drafts, and show connection failures. Record the service address and restart procedure; saved files can survive after the service stops.

In Codex desktop, use `mcp__codex_app__open_in_codex` to open the browser URL beside conversation when available. This opens the page; the local service provides the connection. Saving alone does not start a Codex turn. A local URL is not accessible to other readers merely because the conversation is shared.

## Validation tests

Test the actual chosen path with isolated records and harmless actions. Use browser tools when available; endpoint tests alone do not prove that interface controls work.

| Test | Pass criterion |
| --- | --- |
| Controls | Change a value in the actual interface. The intended record changes and unrelated choices remain intact. |
| Submission boundary | Edit without submitting, then submit explicitly. The draft is not treated as submitted work; the accepted value and displayed status agree. |
| Round trip | Submit a unique test ID and value. The system reads both, records an acknowledgment, and the surface displays it for that submission. Manual exchange includes export, import, and a returned update. |
| Continuity | Save decisions, then reload or regenerate the surface. IDs and decisions survive. Test a new session separately before claiming persistence across sessions. |
| Conflicts | Send different edits based on the same revision. The later stale edit is rejected or explicitly reconciled without silently losing the accepted edit. |
| Execution | Process a harmless submitted action, then present it again. A result appears only after execution, and completed work is not repeated. |
| Failure | Break the connection or supply invalid input. Failure is visible, accepted state remains intact, and recoverable edits are retained. |

Record pass, fail, not tested, or not applicable, with brief evidence and the tested version. Execution tests may be inapplicable when there are no execution controls. For claimed simultaneous collaboration, test separate participant contexts against the same record.

Fix failed capabilities or disable and label them before handover. Never count an unavailable test as a pass. After later changes, rerun affected tests rather than the entire suite for cosmetic edits. Remove only isolated test records.

## Hand over

Provide the surface location, a short explanation of its controls, where decisions are retained, and what starts further system work. Summarize validation and any unverified capability. Refer to the assistant as “the system.”
