# 3-Day Goal Plan to Understand the Crush Codebase

This plan is for maximum-focus onboarding.

The goal is not to read every file.
The goal is to understand how the system is built, how requests move through it,
and how to start making safe changes with confidence.

---

## Main goal

By the end of 3 days, you should be able to:

- explain the architecture of the repo in simple words
- trace a request from CLI entry to model response
- understand where sessions, messages, tools, and permissions live
- make a small code change safely
- know where to look when you want to change behavior

---

## Learning strategy

For every file you read, answer these 3 questions:

1. What problem does this file solve?
2. Who calls this file or uses this code?
3. What would break if I changed it?

Do not try to memorize everything.
Build a mental map.

---

## Day 1 — Understand the request lifecycle

## Objective

Understand how Crush starts, loads config, creates the app, runs an agent, and
streams a response.

## Files to read in order

1. `README.md`
2. `AGENTS.md`
3. `main.go`
4. `internal/cmd/run.go`
5. `internal/cmd/root.go`
6. `internal/app/app.go`
7. `internal/agent/coordinator.go`
8. `internal/agent/agent.go`

## What to focus on

- how the process starts
- how `crush run "prompt"` is handled
- how config is turned into runtime services
- how a session is created
- how the agent is built
- how the response is streamed back

## Output you should produce for yourself

Write a short note answering:

- what happens first?
- where is the app assembled?
- where does the actual LLM call happen?
- where is stdout updated?

## Small practice task

Make one tiny safe change such as:

- update a doc example
- adjust a harmless user-facing string

Then run tests.

## End-of-day checkpoint

By the end of Day 1, you should be able to explain this flow:

> `main.go` -> `internal/cmd` -> `internal/app` -> `internal/agent/coordinator`
> -> `internal/agent/agent` -> messages/session state -> output to terminal

---

## Day 2 — Understand state, tools, and safety

## Objective

Understand how Crush stores work, how the agent uses tools, and how the repo
prevents unsafe actions.

## Files to read in order

1. `internal/session/session.go`
2. `internal/message/message.go`
3. `internal/message/content.go`
4. `internal/db/connect.go`
5. `internal/agent/tools/` for:
   - `view`
   - `grep`
   - `glob`
   - `edit`
   - `write`
6. `internal/permission/permission.go`

## What to focus on

- how sessions are created and saved
- how messages are structured
- why message parts are richer than plain text
- how tools are registered and used
- how permissions are requested and granted
- why the model acts through tools instead of direct access

## Key concept to understand

Crush is not just a chatbot.
It is a local application that gives an LLM a structured, permissioned tool
interface.

## Output you should produce for yourself

Write a short note answering:

- what is a session?
- what is a message in this repo?
- why do tool calls and tool results exist as message parts?
- when is permission required?

## Small practice task

Pick one:

- improve one tool description
- add one doc/example for a tool
- add one tiny test in a small package

Then run tests.

## End-of-day checkpoint

By the end of Day 2, you should be able to explain:

> how the agent reads files, searches code, edits files, and asks permission
> before doing risky work.

---

## Day 3 — Understand configuration, extensibility, and product shape

## Objective

Understand how Crush supports multiple providers, LSP, MCP, skills, and the TUI.

## Files to read in order

1. `internal/config/config.go`
2. `internal/config/load.go`
3. `internal/lsp/manager.go`
4. `internal/agent/prompts.go`
5. `internal/agent/templates/`
6. `internal/skills/skills.go`
7. `internal/agent/tools/mcp/`
8. `internal/ui/AGENTS.md`
9. `internal/ui/model/` overview pass

## What to focus on

- how providers and models are configured
- how env vars and config merge together
- why LSP is lazy-loaded
- how prompts are built
- how skills change instructions
- how MCP extends runtime capabilities
- how the UI is organized at a high level

## Output you should produce for yourself

Write a short note answering:

- how does Crush support many providers?
- what is the difference between built-in tools, skills, and MCP?
- why is LSP helpful here?
- what is the role of the UI vs the app vs the agent?

## Small practice task

Pick one:

- improve architecture docs
- improve config docs
- add a tiny test for config or helper code
- trace one UI flow and document it

Then run tests.

## End-of-day checkpoint

By the end of Day 3, you should be able to explain the repo like this:

> Crush is a terminal-first AI coding assistant built as a stateful local app
> that orchestrates LLMs, tools, permissions, persistence, LSP, and extension
> points into one developer workflow.

---

## Priority reading list if time is tight

If you only have enough focus for the most important files, read these first:

1. `main.go`
2. `internal/cmd/run.go`
3. `internal/cmd/root.go`
4. `internal/app/app.go`
5. `internal/agent/coordinator.go`
6. `internal/agent/agent.go`
7. `internal/message/content.go`
8. `internal/session/session.go`
9. `internal/permission/permission.go`
10. `internal/config/load.go`

---

## Simple beginner tasks list

Use these while learning:

1. Change a harmless UI or doc string
2. Add a README/example improvement
3. Trace `crush run` and document the flow
4. Read one built-in tool fully and explain it
5. Add one tiny unit test in a helper package
6. Improve `PROJECT_WALKTHROUGH.md`
7. Add one config explanation note

---

## Final success checklist

At the end of 3 days, check whether you can answer these clearly:

- Where does the app start?
- Where is config loaded?
- Where is the app wired together?
- Where is the agent created?
- Where does tool execution happen?
- Where are sessions and messages stored?
- Where does permission logic live?
- Where would I go to change provider behavior?
- Where would I go to change UI behavior?
- Where would I go to add or modify a tool?

If you can answer those, you understand the codebase well enough to begin real
work safely.
