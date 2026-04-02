# Unified Git Workflow

An opinionated framework for source control, delivery, and release operations.

This is not a universal template. It is a coherent operating model built on patterns that keep reappearing in healthy teams: **trunk-based mainline, short-lived branches, CI-enforced quality, immutable artifacts, tagged releases, environment promotion, and channel-based rollout.**

## The Core Insight

There is no single universally proven master pattern that cleanly solves git + branches + PRs + issues + tags + releases + environments + binary distribution + CI/CD + channels for every team and product.

But there is a strong shape that keeps reappearing:

> Small mainline development, short-lived branches, automated CI, immutable build artifacts, versioned releases, environment promotion, and explicit release channels.

What changes from team to team is mostly the **tactics**, not the underlying **principles**.

Where teams go wrong is usually when they mix too many orthogonal models: GitFlow plus trunk-based habits plus manual releases plus mutable staging plus unclear versioning plus ad hoc tagging.

**The real win is not "the one true industry pattern." It is choosing one coherent operating model and using it consistently.**

## The Meta-Pattern

A healthy delivery system separates five concerns:

| Concern | Responsibility |
|---|---|
| **Source control** | Git tracks source history |
| **Build identity** | CI creates immutable artifacts |
| **Release identity** | Tags mark release intent |
| **Runtime environment** | Environments host runtime instances |
| **Exposure channel** | Channels control audience and risk |

Once those are clearly separated, everything becomes coherent.

In one sentence:

> A healthy delivery system separates source control, build identity, release identity, runtime environment, and exposure channel.

## Philosophy

These are non-negotiable principles:

**Mainline is sacred.** `main` always represents the latest integrated state of the product.

**Branches are temporary.** A branch exists to deliver one focused change, then dies.

**Artifacts are immutable.** Once a binary, package, or container is built in CI, that exact artifact is what moves forward.

**Environments are not branches.** You do not create staging or production branches as long-lived truth sources. Environments are deployment targets, not development lines.

**Releases are named states.** A release is a tagged, auditable point in history, with associated artifacts and notes.

**Channels communicate risk.** Canary, beta, stable are not just labels; they are promises about exposure and confidence.

**Automation is default.** If something happens often, it should be in CI/CD rather than in a human checklist.

## Operational Tiers

The framework scales across three operational tiers. Every project falls into one:

| Tier | Description | Channels |
|---|---|---|
| **Solo / Personal** | No external users. Low ceremony. | `dev` + `stable` |
| **Solo / Consultancy** | Real users or client. Traceability matters. | `dev` + `beta` + `stable` |
| **Small Team** | Multiple developers, real users. Coordination and rollout. | `dev` + `canary` + `beta` + `stable` |

Projects move up tiers as they grow. The framework adds ceremony without restructuring. See [Adapting](ADAPTING.md) for full details.

## Documents

| Document | What it covers |
|---|---|
| [Glossary](GLOSSARY.md) | Ubiquitous language — shared vocabulary for the entire framework |
| [Workflow](WORKFLOW.md) | Branching, PRs, commits, issues, merges — day-to-day development |
| [Releases](RELEASES.md) | Tags, versions, artifacts, channels, environments, promotion, hotfixes |
| [Adapting](ADAPTING.md) | Operational tiers, product type variations, and where the pattern flexes |

## For Agents

If you are an AI agent consuming this framework:

1. Read **GLOSSARY.md** first. Adopt this vocabulary exactly.
2. Read **WORKFLOW.md** for day-to-day development rules.
3. Read **RELEASES.md** for delivery and release operations.
4. Read **ADAPTING.md** to determine which tier applies to the current project.

When working on a project that references this framework: create branches with the correct naming convention, write conventional commits, open PRs with issue links, and follow the tier-appropriate level of ceremony.

## The Direct Answer

Is there a single unified proven pattern? Not really as a universal template.

But there is a highly repeatable proven architecture for delivery operations:

> Trunk-based mainline + short-lived branches + CI-enforced quality + immutable artifacts + tagged releases + environment promotion + channel-based rollout.

That is the best foundation to unify your own opinionated framework. This repo is that framework.
