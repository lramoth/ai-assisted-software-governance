# AI-Assisted Software Governance

A small, drop-in set of engineering constraints for people building software
with coding agents.

AI-assisted projects rarely become hard to maintain because of one terrible
decision. They become hard to maintain because each session solves a local
problem in a slightly different way. Over time, responsibilities blur, state is
duplicated, dependencies point everywhere, and no one—including the agent—can
confidently explain how the application fits together.

[`GOVERNANCE.md`](GOVERNANCE.md) gives your coding agent a stable set of rules
for preventing that architectural drift. It is intentionally short,
tool-agnostic, and suitable for an existing project.

## Add it to a project

1. Copy `GOVERNANCE.md` into the root of your project.
2. Keep it there as the single canonical copy of the principles.
3. Add a short instruction to the agent guidance file your project uses.

Do not replace an existing `AGENTS.md` or `CLAUDE.md`. Add the relevant
instruction to it.

### If your project has `AGENTS.md`

Add:

```markdown
## Engineering governance

Before non-trivial implementation work, read and follow `GOVERNANCE.md`.
Treat it as a set of constraints, not suggestions.

If a requested change conflicts with it or requires an architectural decision
outside the task's scope, identify the conflict before establishing a new
pattern.
```

### If your project has `CLAUDE.md`

Add:

```markdown
## Engineering governance

Before non-trivial implementation work, read and follow `GOVERNANCE.md`.
Treat it as a set of constraints, not suggestions.

If a requested change conflicts with it or requires an architectural decision
outside the task's scope, identify the conflict before establishing a new
pattern.
```

### If your project has both

Add the instruction to both files. Each agent will discover its own instruction
file, while both point to the same `GOVERNANCE.md`.

Do not copy the full governance document into each file. Multiple copies will
eventually drift apart.

### If your project has neither

Create the instruction file used by your coding agent:

- Codex: create `AGENTS.md`
- Claude Code: create `CLAUDE.md`
- Both tools: create both files

Use this minimal content:

```markdown
# Agent Instructions

Before non-trivial implementation work, read and follow `GOVERNANCE.md`.
Treat it as a set of constraints, not suggestions.

If a requested change conflicts with it or requires an architectural decision
outside the task's scope, identify the conflict before establishing a new
pattern.

Prefer inspecting the repository over making assumptions about it.
```

## How to use it

The document is meant to govern how the project changes, not dictate formatting
or technology choices. It should influence feature work, bug fixes, refactors,
and code review without turning every small edit into an architecture exercise.

The agent should read it before non-trivial implementation work. You do not need
to paste it into every prompt once the project's agent instruction file points
to it.

When a task exposes a genuine conflict, the expected behavior is not automatic
refusal. The agent should explain the conflict, propose the smallest coherent
option, and ask for a decision when the choice would establish a lasting
architectural precedent.

## What this does not include

This package does not prescribe:

- a programming language or framework
- formatting and naming conventions
- a specific project structure
- an architecture for your application
- heavyweight engineering process

Those choices belong to the project. This document protects the coherence of
whatever architecture the project already has.

## Maintaining the document

Treat `GOVERNANCE.md` as stable infrastructure. Change it when you want to
change how agents are allowed to modify the project—not to record temporary
project status or task-specific instructions.

If you customize it, prefer a few rules that your project will actually enforce
over a large handbook that agents will skim or ignore.
