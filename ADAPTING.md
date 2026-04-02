# Adapting

How the framework scales by team shape, user exposure, and product type.

The principles are universal. The tactics flex.

---

## By Team and Audience

The framework defines three operational tiers. Every project falls into one.

### Tier 1: Solo / Personal

Projects with no external users (yet). You are the only developer and the only audience.

| Concern | Adaptation |
|---|---|
| **Branches** | Short-lived branches recommended, but direct commits to `main` are acceptable for small changes |
| **PRs** | Optional. Useful for self-review on larger changes, but not required for every commit |
| **Issues** | Lightweight. TODOs, a simple list, or GitHub issues — whatever keeps you honest without slowing you down |
| **Commits** | Conventional commits still recommended — they cost nothing and pay off if the project grows |
| **CI** | Lint + test minimum. A passing `main` is still the goal |
| **Channels** | `dev` + `stable` only |
| **Environments** | Local + production (or none) |
| **Release notes** | Tag message is enough |
| **Hotfixes** | Fix on `main`, tag a new patch version |
| **Artifacts** | May be informal (local builds), but still tag what you ship |

**The principle that still holds:** `main` is the source of truth, artifacts are immutable once shipped, tags mark releases.

**What relaxes:** ceremony around PRs, issues, and multi-stage promotion.

---

### Tier 2: Solo / Consultancy

You are the only developer, but the project has real users or a client. Accountability and traceability matter.

| Concern | Adaptation |
|---|---|
| **Branches** | Required. Every change gets a branch |
| **PRs** | Required. The audit trail matters — clients, compliance, and your future self will thank you |
| **Issues** | Required. Tracked work gives the client visibility and gives you a paper trail |
| **Commits** | Conventional commits required. Changelog generation becomes valuable |
| **CI** | Full pipeline — lint, test, build, static analysis |
| **Channels** | `dev` + `beta` + `stable` |
| **Environments** | Dev + staging + production |
| **Release notes** | Required. Client communication, not just internal record |
| **Hotfixes** | Branch from tag, full validation, patch release, merge back to `main` |
| **Artifacts** | Formal. Traceable to commit, tag, and pipeline |

**The principle that still holds:** everything from Tier 1, plus full traceability and auditability.

**What relaxes:** you skip canary (no traffic to split), and review is self-review (but still happens via PR).

---

### Tier 3: Small Team

Multiple developers, real users. Coordination, review, and controlled rollout all matter.

| Concern | Adaptation |
|---|---|
| **Branches** | Required. Strictly short-lived |
| **PRs** | Required. Review by another human is the point |
| **Issues** | Required. Team coordination demands tracked work with clear ownership |
| **Commits** | Conventional commits required |
| **CI** | Full pipeline |
| **Channels** | `dev` + `canary` + `beta` + `stable` |
| **Environments** | Dev + staging + production |
| **Release notes** | Required. Team + user communication |
| **Hotfixes** | Branch from tag, full flow, peer review even under pressure |
| **Artifacts** | Formal with full provenance |

**The principle that still holds:** everything from Tier 2, plus peer review and channel-based rollout.

**What adds:** canary deployments, multi-reviewer PRs, team-visible issue boards.

---

### Tier Summary

| | Solo / Personal | Solo / Consultancy | Small Team |
|---|---|---|---|
| PRs | Optional | Required (self-review) | Required (peer review) |
| Issues | Lightweight | Required | Required |
| Channels | `dev` + `stable` | `dev` + `beta` + `stable` | `dev` + `canary` + `beta` + `stable` |
| Environments | Local + prod | Dev + staging + prod | Dev + staging + prod |
| CI | Lint + test | Full pipeline | Full pipeline |
| Release notes | Tag message | Required | Required |
| Hotfixes | Fix on `main` | Branch from tag | Branch from tag + review |

### Growing Between Tiers

Projects move up tiers. They rarely move down. When your personal project gets its first user, you are in Tier 2. When you add a second developer, you are in Tier 3.

The framework is designed so that moving up means **adding ceremony**, not restructuring. If you followed conventional commits in Tier 1, your changelog is ready for Tier 2. If you tagged releases in Tier 2, your promotion model is ready for Tier 3.

---

## By Product Type

Orthogonal to team tier. A solo developer can ship a SaaS; a small team can ship a CLI tool. The tier governs ceremony and coordination. The product type governs distribution mechanics.

### Libraries

- Artifact = published package (npm, PyPI, crates.io, etc.)
- Channel mapping may use package tags (`latest`, `next`, `beta`)
- "Deployment" means publishing to a registry
- Rollback means publishing a new patch or yanking a version

### SaaS / Backend Services

- Artifact = container image or deployment bundle
- Environments are literal infrastructure (dev cluster, staging cluster, production)
- Canary is a traffic-split deployment
- Rollback is redeploying the previous image

### Desktop Applications

- Artifact = installer, DMG, MSI, zip
- Channels may map to update tracks (stable, beta)
- Distribution may involve signing, notarization, update servers
- Rollback is publishing a new version pointing users to update

### Mobile Applications

- Artifact = IPA, APK, AAB
- Distribution goes through App Store / Play Store review
- Channels map to TestFlight, internal tracks, phased rollout percentages
- Rollback is often a new release (stores rarely allow true rollback)
- Build numbers add an extra layer of versioning

### CLI Tools

- Artifact = platform-specific binaries, Homebrew formula, package
- Channels may map to Homebrew taps, GitHub release tags, or install scripts
- Distribution may involve multiple package managers simultaneously

---

## Governance and Exceptions

Every framework needs an escape hatch.

### When to Bend the Rules

- Emergency production fixes may skip full review ceremony (but still get reviewed after)
- Spike branches may not map to formal issues
- Prototype work may skip CI enforcement temporarily
- Some changes (config, docs) may not need full PR ceremony
- Tier 1 projects can skip most ceremony by design

### When NOT to Bend the Rules

- Artifacts are always immutable — no exceptions
- Production always receives tagged releases — no exceptions (once you have users)
- Hotfixes always merge back to `main` — no exceptions
- Rollback never means rebuilding from old source — no exceptions

### The Test

If you are about to make an exception, ask:

> "Am I bending a tactic or breaking a principle?"

Bending tactics is fine. Breaking principles means the framework is failing and needs to be revisited, not worked around.

---

## Building Your Own Instance

To adopt this framework for a specific project:

1. **Pick your tier.** Solo/personal, solo/consultancy, or small team.
2. **Start with the glossary.** Make sure everyone (including agents) agrees on the vocabulary.
3. **Pick your merge policy.** Squash merge is the default recommendation.
4. **Define your artifact type.** What does CI produce?
5. **Map your environments.** What infrastructure do you deploy to?
6. **Map your channels.** What audiences do you serve?
7. **Define your promotion gates.** What checks must pass before promotion?
8. **Document your exceptions.** What rules can bend and which cannot?

The framework is the skeleton. The tier sets the weight. Each project fills in the muscle.
