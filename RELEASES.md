# Releases

Tags, versions, artifacts, channels, environments, promotion, hotfixes, and rollback.

---

## Versioning

Use SemVer or something close. You do not have to worship it, but it is useful as a shared language.

```
MAJOR.MINOR.PATCH
```

- **MAJOR** = breaking change
- **MINOR** = backward-compatible feature
- **PATCH** = backward-compatible fix

For internal tools, you can be more pragmatic, but still keep the shape.

### Prerelease Suffixes

Use when needed. They map naturally to channels.

```
v1.8.0-alpha.1
v1.8.0-beta.3
v1.8.0-rc.1
```

---

## Tags

Use annotated tags for releases.

```
v1.4.0
v1.4.1
v2.0.0-beta.2
v2.0.0-rc.1
```

Tags represent release points, not random milestones.

---

## Artifacts

Treat artifacts as first-class citizens.

### Every Artifact Must Have

- Version
- Commit SHA
- Build timestamp
- Build pipeline ID
- Provenance if possible

### Examples

| Type | Format |
|---|---|
| Docker image | `1.4.0`, `1.4`, `sha-abc1234` |
| CLI binary | `tool-v1.4.0-linux-amd64.zip` |
| Python wheel | Versioned package |
| Installer | Versioned executable |
| Mobile build | Version + build number |

### Rules

- Artifacts are immutable
- Artifacts are traceable to commit, tag, and pipeline
- Artifacts are retained long enough for rollback and audit

### Identity

| Identifier | Tracks |
|---|---|
| Git commit | Source |
| Artifact | Build output |
| Tag | Release intent |

These are related but not identical.

---

## The Promotion Model

**Build once, promote many.** This is one of the strongest patterns you can adopt.

Do not rebuild separately for staging and production if you can avoid it.

```
1. Commit merges to main
2. CI builds artifact once
3. Artifact gets a unique build ID / commit SHA
4. Deploy artifact to dev
5. Promote same artifact to staging
6. Promote same artifact to canary
7. Promote same artifact to stable
```

You are testing and promoting **the same thing**, not similar things.

---

## Environments

Runtime targets. Where something runs.

| Environment | Purpose | Deployment |
|---|---|---|
| **Dev** | Fast iteration | Automatic from `main` |
| **Staging** | Candidate validation | Manual or gated promotion |
| **Production** | Live traffic | Gated promotion from staging |

### Rules

- Environments are deployment targets, not git branches
- Same artifact moves across environments
- Config differs by environment; code does not

---

## Channels

Release audience and risk lanes. Who gets something and how risky it is.

Channels are **quality-gated** — a build gets promoted when it passes checks, not on a schedule.

| Channel | Exposure | Ceremony | Tier |
|---|---|---|---|
| **Dev** | Team only | Low — may track every merge | All |
| **Canary** | Limited production users | Medium — requires objective checks | Tier 3 |
| **Beta** | Opt-in or limited audience | Medium — versioned prereleases | Tier 2+ |
| **Stable** | General availability | High — tagged, noted, published | All |

### Channel Sets by Tier

| Tier | Channels | Rationale |
|---|---|---|
| **Solo / Personal** | `dev` + `stable` | No users to protect with intermediate gates |
| **Solo / Consultancy** | `dev` + `beta` + `stable` | Beta gives clients a preview; no traffic to split for canary |
| **Small Team** | `dev` + `canary` + `beta` + `stable` | Full model — canary enables low-blast-radius production validation |

### Mapping Versions to Channels

- Every merge to `main` → dev build
- Selected builds → canary (Tier 3)
- Versioned prereleases (`-beta.N`, `-rc.N`) → beta
- Approved tags → stable

### What About Nightly?

Nightly is a **time-gated build schedule**, not a quality-gated channel. If you produce nightly builds, they feed into `dev` or `canary`. The build cadence is a CI concern; the channel model is a delivery concern.

### Rules

- Canary is limited exposure
- Stable is default public release
- Dev is not guaranteed stable
- Promotion requires objective checks

---

## CI and CD

### CI Is About Trust

CI answers: **"Is this change good enough to integrate?"**

| Step | Purpose |
|---|---|
| Format / lint | Code quality |
| Unit tests | Correctness |
| Integration tests | System behavior |
| Static analysis | Safety |
| Build / package | Artifact creation |
| Generate artifacts | Deliverables |
| Preview builds | Early feedback |

### CD Is About Movement

CD answers: **"Can we safely move this artifact through environments and channels?"**

| Step | Purpose |
|---|---|
| Deploy to environment | Runtime placement |
| Smoke tests | Basic validation |
| Gate promotion | Confidence checkpoint |
| Rollback support | Safety net |
| Publish release notes | Communication |
| Publish binaries | Distribution |

### CI on Merge to Main

- Build artifact
- Assign build metadata
- Publish internal artifact
- Deploy automatically to dev
- Run smoke tests

---

## Release Flow

The full sequence from merge to stable:

```
1. Decide release candidate from main
2. Tag prerelease if needed (v1.4.0-rc.1)
3. Deploy same artifact to staging
4. Validate
5. Promote to canary
6. Observe
7. Promote to stable
8. Tag final release (v1.4.0)
9. Publish release notes
10. Attach binaries / packages / images
```

### Release Rules

- Every release has a tag
- Every tag maps to artifacts
- Every stable release has notes
- Production only receives tagged releases
- Hotfixes become patch releases

---

## Hotfixes

This is where a lot of frameworks break down.

### By Tier

**Tier 1 (Solo / Personal):** Fix on `main` directly. Tag a new patch version. Done.

**Tier 2+ (Consultancy, Small Team):** Full hotfix flow:

```
1. Branch from the production tag or current stable commit
2. Make the minimal fix
3. Validate
4. Release a patch version
5. Merge the fix back into main
```

**Tier 3 adds:** peer review even under time pressure.

### Example

- Stable is `v1.4.2`
- Production issue found
- Create `hotfix/412-fix-crash`
- Release `v1.4.3`
- Merge back to `main`

**No divergence allowed to linger.**

---

## Rollback

- Rollback by redeploying the prior artifact or tag
- Never patch production manually
- Never rebuild old source for rollback if the old artifact exists

Rollback is a deployment operation, not a source control operation.

---

## Long-Term Support

You only need release branches if you support multiple versions at the same time.

```
main         → next version
release/1.8  → maintained stable line
release/1.7  → emergency support only
```

If you are solo or a small team, **avoid this unless absolutely necessary.** It adds real operational cost.
