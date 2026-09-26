---
name: "generative-control-surface"
description: "Give the current task a docked control surface in the client's side panel: a live HTML readout dashboard, a two-way fullstack panel app, or both. Trigger when work needs a persistent visual surface beside chat that in-chat widgets cannot provide."
Author: Muse AI
---

# Generative Control Surface

## Purpose

Build a docked, task-scoped interface in the client's side panel. Chat stays the narrative channel; the panel is the persistent state surface.

## Concept

A generative control surface is a thin interface the agent builds for itself, per task: a persistent, docked surface used to operate its own backend work — control room, live dashboards, two-way forms beside the chat, with the agent session as the backend. This inverts transient generative UI (A2UI, "just-in-time UI"): not disposable in-flow UI answering a prompt, but a persistent parallel surface that outlives chat scroll.

## Platform scope

Muse-specific. Depends on Muse platform primitives: the docked side panel (attached HTML renders live and refreshes on rewrite; fullstack artifacts open in the same panel), private fullstack artifacts with agent-retrievable storage, and the agent workspace as backend. The concept is portable; this implementation assumes the Muse client and toolset.

## The two mechanisms (do not conflate them)

1. Dashboard (static HTML): backend-to-panel readout. The backend rewrites the file; the open panel refreshes live. Page JavaScript executes, so client-side interactivity works — tabs, toggles, counters, collapsible sections. What it cannot do: write back to the workspace or reach the agent. No form submissions, no callbacks, no fetch-to-backend. Read-only with respect to the backend; interactive with respect to the user.
2. Two-way surface (fullstack web artifact): full duplex. Opens in the same docked panel. Forms and submissions persist server-side and are retrievable by the agent via artifact actions. This is the intended input path, not a workaround.

## Verified end-to-end (2026-09-25)

- HTML attachment renders as a full styled page in the docked panel; JavaScript runs (a timer-driven counter was observed incrementing).
- Rewriting the file refreshes the open panel without re-tapping.
- Fullstack artifact opens in the docked panel; a submitted string was read back through the `listtransmissions` action with matching id and timestamp.
- Fullstack artifacts are private and cannot be published.

## Workflow

1. Pick the mechanism:
   - Readout only (status, progress, queues) → dashboard.
   - Panel must take input (forms, submissions, decisions) → two-way surface.
   - Need both → build both.
2. Dashboard: write self-contained HTML (inline CSS/JS, no external assets) to `~/workspace/your_files/<name>.html`, starting from `assets/dashboard-shell.html`. Pair it with a `<name>-gen.py` generator that renders state into the HTML. Rewrite the file on state changes; the open panel refreshes live. Attach the file once in chat so the user can dock it. Stamp every build with build number and timestamp.
3. Two-way surface: `artifact.create_web_fullstack` with a form and server storage (private, cannot be published). The user opens it from Library; it renders in the docked panel. Read submissions back with `artifact.list_actions` / `artifact.invoke_action`.
4. Verify before claiming: confirm the panel shows current state, and read back at least one submitted value through the backend.

## Verification tests

Run these before claiming the surface works. T1–T2 are scriptable; T3–T5 need the live client.

- T1 Dashboard state accuracy: regenerate the dashboard, then confirm it shows the current book count, every open round, and the pending-action snapshot matching the widget's live `state.pending`. Any mismatch fails.
- T2 Dashboard interactivity: confirm tabs switch panes and collapsible sections expand/collapse in the docked panel. Confirm the HTML contains no write-back channel (no form submissions, no fetch to a backend, no callback URLs).
- T3 Panel opens docked: the fullstack artifact opens from Library into the docked side panel and renders its controls (book list, tabs, per-image Approve/Reject, pending banner).
- T4 Submission round-trip: submit one action through the panel (e.g. queue a no-op or a clearly-labeled TEST action). Read it back via the artifact's actions (`list_actions` / `invoke_action`) and confirm id, parameters, and timestamp match what was submitted.
- T5 Agent execution: with a real queued action, confirm the agent picks it up on the next chat turn, executes it against the backend through the normal turn loop, and the dashboard reflects the new state afterward.
- T6 No silent divergence: after T5, the widget, the dashboard, and the panel must agree on state (same pending action or none, same image statuses). Any divergence fails.

Record results with the build stamp; a surface that fails any test is not handed over.

## Operating rules

- The panel is user-opened (one tap on the attachment or Library entry); the agent cannot force it open.
- Do not declare the docked two-way surface impossible. It is verified working end-to-end. The failure mode to avoid is reasoning from the static dashboard's limits as if they applied to the fullstack path.
- Static dashboard: client-side interactive, backend read-only. Never claim a write-back channel from static HTML; none is verified.
- One surface per workstream.
- When a goal looks blocked by a stated constraint, check adjacent mechanisms before declaring impossibility.
