# Agent Entry Point

You are an AI agent operating within a project that follows the **Unified Engineering Method (UEM)**. This page is your fast-path orientation. It tells you what to do, what not to do, and where to find everything else.

---

## Before You Start

Load context in this order before writing any code:

1. **Conventions file** — `CLAUDE.md`, `.cursorrules`, or equivalent. This contains project-specific rules the toolchain does not enforce.
2. **Project map** — `ARCHITECTURE.md` or equivalent. This explains how the codebase is organized.
3. **Relevant ADRs** — in `docs/adr/` or equivalent. These explain why the architecture is the way it is.
4. **Current issue** — the issue you are working on. This defines scope, intent, and acceptance criteria.
5. **Relevant code** — the files you will touch. Understand current patterns before generating new code.

Do not skip this. Generating code without context-loading produces output that is mechanically valid but misaligned with the project's intent and architecture.

See [Knowledge — How Agents Load Context](modules/knowledge/) for the full protocol.

---

## The Execution Loop

Every unit of work follows this loop:

```text
1. Select issue          — pick a specific, scoped unit of work
2. Inspect codebase      — understand current state, patterns, relevant files
3. State plan            — articulate what will change and why
4. Make focused changes  — implement within the bounded scope
5. Run validation        — execute golden commands (fmt, lint, typecheck, test)
6. Fix until clean       — iterate until the toolchain is satisfied
7. Summarize deltas      — describe what changed for the reviewer
8. Prepare for integration — open PR, link issue, confirm CI
```

You do not declare success. The toolchain does. If golden commands fail, fix the code and re-run.

See [Construction — The Agent Execution Loop](modules/construction/) for bounded task rationale and session boundaries.

---

## What You Must Always Do

- **Run golden commands before declaring work complete.** `just ci` (or equivalent) is the gate.
- **Link every change to an issue.** Branch names include the issue ID. PRs reference the issue.
- **Write meaningful commit messages.** Conventional commits. Capture _why_, not just _what_. See [Workflow — Commit Message Quality](modules/change-management/WORKFLOW.md).
- **Persist intent beyond the session.** If a plan, decision, or discovery matters after this conversation, put it in the project system — issue, ADR, commit message, or doc. Chat history is not persistence.
- **Pass the Context-Free Contributor test.** Could someone pick up this issue and complete it without reading this conversation? If not, update the issue.

---

## What You Must Not Do

- **Do not modify toolchain configuration** — linter rules, type checker settings, formatter config, CI pipelines — unless explicitly instructed. These define the constraint corridor. Loosening a rule to make your code pass is not a fix. See [Construction — Configuration Immutability](modules/construction/).
- **Do not commit directly to `main`** (Tier 2+). Work on a branch. Open a PR.
- **Do not skip validation.** Never declare a task complete without running golden commands.
- **Do not generate code before loading context.** Read the conventions file, the issue, and the relevant code first.
- **Do not add features, abstractions, or refactors beyond the issue scope.** Solve what was asked. Nothing more.

---

## Where to Find Things

| What you need                 | Where to look                                                   |
| ----------------------------- | --------------------------------------------------------------- |
| Project rules and conventions | `CLAUDE.md`, `.cursorrules`, or equivalent                      |
| Architecture orientation      | Project map (`ARCHITECTURE.md` or README section)               |
| Why decisions were made       | ADRs (`docs/adr/` or equivalent)                                |
| What to work on               | Issue tracker                                                   |
| How to validate               | Golden commands (`just ci`, `just fmt`, `just test`)            |
| Vocabulary                    | [Glossary](GLOSSARY.md)                                         |
| How ceremony scales           | [Adapting](ADAPTING.md) — tier and class determine what applies |

---

## The Three Layers

You interact with UEM through three layers:

- **Constraint layer** — toolchain configs enforce standards mechanically. You cannot produce code that violates them.
- **Direction layer** — issues scope your work. Conventions files communicate expectations the toolchain cannot enforce.
- **Validation layer** — golden commands provide pass/fail verification. You run them; the toolchain declares success.

The environment does the constraining. You operate inside it.

---

## Key Principles

1. **Constrain first, generate second.** The toolchain exists before you write code.
2. **Use tools for rules.** Do not rely on your training data when the local toolchain can verify.
3. **Persist all meaningful plans.** If it matters after this session, it must leave the chat.
4. **Every meaningful change maps to an issue.** Work is traceable and resumable.
5. **Small bounded tasks beat giant one-shot generations.** Stay within scope.
6. **Validation is mandatory.** The toolchain declares success, not you.
7. **Future agents are also maintainers.** Write for the agent that arrives after you with no context.

See [Principles](PRINCIPLES.md) for the full treatment.
