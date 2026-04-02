# Glossary

Ubiquitous language for the Unified Git Workflow. Define your vocabulary and half the confusion disappears.

This glossary is the shared contract. Every document in this framework uses these terms with these meanings. Agents, automation, and team members should interpret them identically.

---

## Source Control

### Change
A unit of work. Usually tied to an issue. The smallest meaningful delta to the codebase.

### Branch
A temporary integration workspace for a single change or cohesive set of changes. Branches are short-lived by design.

### Merge
The act that integrates approved work into `main`. The point where a change becomes part of the shared truth.

### Hotfix
An urgent production fix. Branched from the production tag or current stable commit, merged back into `main` after release. No divergence allowed to linger.

### Rollback
Returning an environment or channel to a previously known-good artifact. Rollback redeploys an existing artifact; it does not rebuild from old source.

---

## Collaboration

### Issue
A tracked piece of work. Types: **Feature**, **Bug**, **Chore**, **Spike**, **Release**, **Incident**. The atomic unit of planning.

### PR (Pull Request)
A proposal to merge a branch into `main`. The review and validation boundary. A PR is for review, not for long-lived parallel worlds.

---

## Build and Delivery

### Build
The CI process that compiles, tests, and packages code from a specific commit. Deterministic and reproducible.

### Artifact
The immutable output of a build: binary, wheel, container image, installer, zip, IPA, APK, etc. Once created, an artifact never changes.

### Promotion
Moving the same artifact to the next environment or channel. The artifact does not change; only where it runs changes.

---

## Versioning and Release

### Version
A product identifier. Typically SemVer-shaped: `MAJOR.MINOR.PATCH`.

- **MAJOR** = breaking change
- **MINOR** = backward-compatible feature
- **PATCH** = backward-compatible fix

### Tag
A git reference that marks a meaningful version or release point. Format: `v1.4.0`, `v2.0.0-beta.2`, `v2.0.0-rc.1`. Tags represent release points, not random milestones.

### Release
A versioned, tagged, published product state with notes and artifacts. A release is a named, auditable moment in the product's history.

---

## Infrastructure

### Environment
A runtime target. Where something runs.

| Environment | Purpose |
|---|---|
| **Dev** | Automatic from `main`. Fast iteration. |
| **Staging** | Candidate validation before production. |
| **Production** | Live traffic. |

### Channel
A release audience and risk lane. Who gets something and how risky it is.

Channels are **quality-gated**: a build gets promoted when it passes checks, not on a schedule.

| Channel | Purpose | Tier |
|---|---|---|
| **Dev** | Fast, frequent, low ceremony. May track every merge to `main`. | All |
| **Canary** | Limited real-world exposure. Low blast radius production validation. | Tier 3 |
| **Beta** | Broader but opt-in or limited audience. Pre-release testing. | Tier 2+ |
| **Stable** | General availability. Default channel for most users. | All |

**Related but distinct concepts:**

- **Nightly** is a time-gated build schedule, not a channel. Nightly builds may feed into `dev` or `canary`.
- **RC** (release candidate) is a version suffix (`v1.4.0-rc.1`), not a channel. RCs live on the `beta` to `stable` promotion path.
- **Edge** and **next** are ecosystem-specific aliases for `dev` or `canary`.
- **LTS** (long-term support) is a support policy, not a channel.

### The Distinction

Environments answer: **where is this running?**
Channels answer: **who gets this and how risky is it?**

A staging environment might host a release candidate. Production might host both canary and stable deployments. Internal builds may go to a dev environment but not be formal releases. Keep them separate.

---

## Identity Rules

These three identifiers are related but not identical:

| Identifier | What it tracks |
|---|---|
| **Git commit** | Source |
| **Artifact** | Build output |
| **Tag** | Release intent |
