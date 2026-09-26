# Generative Control Surfaces

`[Written in ChatGPT Codex, Using GPT-6 Astra Medium based on notes and skill file developed using Muse AI]`

*Task-specific interfaces for directing ongoing AI work.*

Working with AI often means describing changes in messages: choose these options, move this item, keep that decision, run the next step. Conversation makes it easy to express intent, but becomes cumbersome when every adjustment requires another explanation.

A generative control surface gives that work an interface. The system creates controls suited to the current task, connected to the information and decisions being used to carry it out. People can interact with those controls while continuing the conversation.

For example, while developing a plan, the system could generate a small panel for adjusting priorities and comparing alternatives. Changes made there become part of the task’s recorded state. Conversation remains available to explain a tradeoff or request a different approach—including changes to the panel itself.

“Generative control surface” names the interface. “Just-in-time UI” describes the pattern: create the controls when the need becomes clear.

## Why it matters

A chat interface can discuss almost any task, but offers essentially the same interaction for all of them: another message. Conventional software provides more specific controls, but someone must anticipate and build them.

Generative control surfaces connect these possibilities. Conversation can establish what is needed, and the system can create an interface for the parts that benefit from direct manipulation. As the work develops, that interface can develop with it.

This gives people a more concrete way to direct AI work. Decisions remain visible and adjustable. Several changes can be reviewed together before requesting further work. Routine interactions can happen without a model call for every click.

The opportunity is to make useful software controls available for tasks too particular or short-lived to justify building a dedicated application.

## A workbench that continues with the task

The surface participates in ongoing work. It can present results, accept changes, and show what happened next.

Its recorded state should survive changes to the interface. Rebuilding a panel should not erase previous decisions or require them to be entered again. What persists is the work; the presentation can evolve.

Controls must also make their consequences clear. Saving an adjustment and asking the system to act on it are different operations. An interface should indicate which has happened, rather than treating every click as completed work.

## What this repository provides

This repository explores the pattern through example skills for Muse and Codex. Each skill guides a system in creating a small, useful surface and connecting it to the ongoing task.

The implementations use their host’s available capabilities. A docked side panel next to a chat window is useful, but neither its location nor a particular storage or connection mechanism defines the concept.

The shared aim is straightforward: let conversation establish the work, let generated controls make it easier to direct, and retain the decisions as both evolve.
