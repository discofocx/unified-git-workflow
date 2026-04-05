# CLAUDE.md

This is a text-only reference repository. There is no code to build or test.

## What This Is

A Deterministic Engineering Operating System — an opinionated framework for how software is conceived, constructed, validated, integrated, released, and evolved. Designed for consumption by humans and AI agents.

## Reading Order

1. **[DOCTRINE.md](DOCTRINE.md)** — First principles. The seven non-negotiable principles and project classification.
2. **[GLOSSARY.md](GLOSSARY.md)** — Ubiquitous language. Adopt this vocabulary exactly.
3. **[modules/construction/](modules/construction/)** — How to approach building: toolchain selection, validation surface, agent execution loop.
4. **[modules/change-management/](modules/change-management/)** — Day-to-day development: branches, PRs, commits, issues, releases.
5. **[modules/delivery/](modules/delivery/)** — Delivery operations: artifacts, promotion, environments, channels, CI/CD.
6. **[modules/knowledge/](modules/knowledge/)** — Knowledge persistence: ADRs, plans, documentation, context management.
7. **[ADAPTING.md](ADAPTING.md)** — Operational tiers (solo/personal, solo/consultancy, small team), project classes, and product type variations.

## When Referencing This Framework

- Determine which tier and project class apply to the target project (see ADAPTING.md and DOCTRINE.md)
- Set up the toolchain before generating code (see modules/construction/)
- Follow the tier-appropriate level of ceremony
- Use the glossary terms consistently
- Validate with golden commands before declaring work complete
