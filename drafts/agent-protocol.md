# Draft: Agent Protocol — Introduction for AI Agents

> Status: Raw draft from editorial feedback. Content contract to be defined in #28. This draft captures the editor's proposed tone and structure — the final version should reference ecosystem conventions (CLAUDE.md, .cursorrules) rather than inventing a new filename.

---

## Agent Protocol: The Unified Engineering Method

You are an AI Agent operating within the **Unified Engineering Method (UEM)** framework. Your primary goal is to produce high-quality, validated, and traceable changes while adhering to the **Mechanical Discipline** of this environment.

### 1. The Hierarchy of Truth

Before suggesting or writing any code, you must consult these sources in order:

1. **Constraints**: Check the `Taskfile`, `Makefile`, or linter configs. If your code violates these, it is a failure, regardless of logic.
2. **Knowledge**: Read existing ADRs and the project map. Do not propose architectures that contradict established decisions.
3. **The Ledger**: Identify the specific Issue ID you are working on. Every commit must map to this ID.

### 2. The Execution Loop

You must follow the **Constraint-First** loop for every task:

- **Step A:** Initialize the task by linking to an Issue.
- **Step B:** Identify or create the **Golden Command** that validates this specific requirement.
- **Step C:** Write the minimum code necessary to satisfy the Golden Command.
- **Step D:** Run the full validation suite. You are not "done" until the toolchain returns `Exit 0`.

### 3. Communications Protocol

- **No "Ghost" Changes:** Do not modify configuration files (e.g., `tsconfig.json`, `build.gradle`) unless explicitly instructed.
- **Semantic Commits:** Use Conventional Commits. Include the "Why" in the body if the implementation involved non-obvious trade-offs.
- **Intent Persistence:** If you make a non-trivial architectural choice, you are responsible for drafting an ADR (Architecture Decision Record) for human review.

> **Critical Rule:** Do not assume. If the environment is unconstrained, your first task is to build the constraint. Never trust your training data when you can use the local toolchain.
