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

1. Copy `GOVERNANCE.md` into your project root.
2. Add the snippet below to your agent instruction file — `AGENTS.md`
   (Codex), `CLAUDE.md` (Claude Code), or both. Identical text in both is
   fine; the snippet is short. If your `AGENTS.md` already points at
   `CLAUDE.md`, put it in `CLAUDE.md` only.
3. Do not paste `GOVERNANCE.md` itself into either file. Copies of the
   snippet are cheap; copies of the document drift.

```markdown
## Engineering governance

Read `GOVERNANCE.md` at the start of this session, before implementation
work. It is the constraint set for this project, not reference material.

Re-read it after any context compaction.

When a change touches an architectural concern, follow the pre-flight step
at the top of the document before writing code.

If a request conflicts with the document, say so before writing code.
```

## Check that it's working

Look at the session's tool log, not the agent's prose. `GOVERNANCE.md` should
appear as an actual file read near the start of the session. A statement that
it was read is not evidence; the tool call is.

Then test behavior on a task whose answer you already know: ask for a helper
that already exists in your codebase and see whether the pre-flight names it
under `Extending`. That measures whether the document changed what the agent
did — the only thing worth measuring.

## How to use it

The agent reads it once per session, at the start. You do not need to paste it
into prompts.

It governs how the project changes — not formatting or technology choices.
Small edits stay small: the document's opening section states the narrow
conditions under which the pre-flight is skipped.

When a task forces a genuine architectural decision, the agent stops and
presents it rather than deciding silently. Once you've decided, the agent
records the outcome in the project's existing repository documentation. It
asks first if the established mechanism is external — an issue tracker, wiki,
or anything outside the repo — or if no decision mechanism exists.

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
- enforcement — nothing here fails a build or blocks a commit

Those choices belong to the project. This document protects the coherence of
whatever architecture the project already has.

## Maintaining the document

Treat `GOVERNANCE.md` as stable infrastructure. Change it when you want to
change how agents are allowed to modify the project—not to record temporary
project status or task-specific instructions.

If you add or remove rules, keep the pre-flight step first in the document —
the snippet refers to it by position.

If you customize it, prefer a few rules that your project will actually enforce
over a large handbook that agents will skim or ignore.
