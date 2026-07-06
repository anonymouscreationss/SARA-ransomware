<div align="center">

<img src="assets/atlantia-logo-v2.png" alt="Atlantia Empire" width="140" />

# Atlantia Empire

### A Constitutional Governance Platform for Multi-Agent AI Systems

**261 domain-specialist personas · 185 Ruflo-native generated agents · 17 divisions · a written constitution · a judiciary that reviews every deliverable before it ships**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Tests](https://img.shields.io/badge/engineering_tests-33%2F33_passing-brightgreen)](scripts/run-tests.sh)
[![Smoke Test](https://img.shields.io/badge/standalone_smoke_test-14%2F14_passing-brightgreen)](scripts/no-ruflo-smoke.sh)
[![Agents](https://img.shields.io/badge/agents-261-1B2A4A)](atlas-core/agents)
[![Divisions](https://img.shields.io/badge/divisions-17-D98E2B)](divisions.json)
[![Constitution](https://img.shields.io/badge/constitution-8_articles-2E6B6B)](constitution.md)
[![Node](https://img.shields.io/badge/node-%3E%3D18.0.0-339933?logo=node.js&logoColor=white)](package.json)
[![Built by skynetfc](https://img.shields.io/badge/built_by-%40skynetfc-B23B3B)](https://skynetfc.netlify.app)

</div>

---

## Table of Contents

- [Overview](#overview)
- [Why This Exists](#why-this-exists)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
  - [Mode 1 — Persona Library Only](#mode-1--persona-library-only-no-cli-no-ruflo)
  - [Mode 2 — CLI and Governance Layer, Standalone](#mode-2--cli-and-governance-layer-standalone)
  - [Mode 3 — Full Multi-Agent Platform](#mode-3--full-multi-agent-platform-ruflo--atlantia)
- [CLI Reference](#cli-reference)
- [Divisions](#divisions)
- [The Constitution](#the-constitution)
- [Governance Mechanics](#governance-mechanics)
  - [Role-Based Access Control](#role-based-access-control)
  - [Budget Enforcement](#budget-enforcement)
  - [Memory Tiering](#memory-tiering)
  - [Naturalization Lifecycle](#naturalization-lifecycle)
  - [The Judiciary](#the-judiciary)
- [Economic Model — Gross Specialist Product](#economic-model--gross-specialist-product)
- [Evaluation Harness](#evaluation-harness)
- [Testing & Continuous Integration](#testing--continuous-integration)
- [Repository Layout](#repository-layout)
- [Brand & National Identity](#brand--national-identity)
- [Package & Publishing Status](#package--publishing-status)
- [Known Limitations](#known-limitations)
- [Contributing](#contributing)
- [Security](#security)
- [License & Attribution](#license--attribution)

---

## Overview

Atlantia Empire is a governance layer for large multi-agent AI systems. It takes a library of 261 domain-specialist personas and wraps them in a written constitution, a standing judiciary that reviews every deliverable before it is accepted, a role-based authority model, and a budget/memory enforcement system — so that scaling from one agent to a coordinated swarm doesn't also mean scaling risk, unreviewed output, and untracked cost.

It is deliberately built as a merger of two existing, independently maintained open-source projects rather than a reimplementation of either:

| Layer | Provided by | License | Role in Atlantia |
|---|---|---|---|
| Persona library | [agency-agents](https://github.com/msitarzewski/agency-agents) | MIT | 232+ source persona Markdown files across engineering, marketing, finance, design, sales, and more |
| Multi-agent runtime | [Ruflo](https://github.com/ruvnet/ruflo) | MIT | Swarm orchestration, persistent memory (AgentDB/RuVector), 3-tier model routing, 314 MCP tools, agent federation |
| Governance layer | Atlantia Empire (this repository) | MIT | Judicial/quality division, the constitution, `atlas-core` conversion pipeline, CLI, evaluation harness, RBAC, budget enforcement, brand/nation identity |

Atlantia does not claim Ruflo's runtime engineering or agency-agents' persona content as its own work. The full, itemized credit trail — including exactly which files originate where — is documented in [`NOTICE`](NOTICE).

---

## Why This Exists

Running 200+ AI personas as loose prompt files works for a single operator on a single task. It stops working once multiple agents run concurrently, make decisions with real cost, and produce deliverables nobody else checks. Three problems specifically motivated this project:

**1. Nothing catches a bad output before it ships.** A persona can be confidently wrong, and without a second, independently-authored reviewer, that output goes straight to the user. Atlantia's Judicial branch exists specifically to intercept this — no agent may approve its own work (Constitution, Article II).

**2. Nothing measures whether a persona is actually earning its place.** Adding personas is easy; nothing normally forces the question "is this actually better than not having this persona at all?" Atlantia's Agent Evaluator benchmarks every persona against a no-persona baseline and publishes the result even when it's unflattering (Article VII), and the Deprecation Auditor acts on that data.

**3. Nothing enforces who is allowed to do what.** "A human approved it" is not an audit trail. Atlantia ties every privileged action — approving a new agent, adjusting a budget, triggering an emergency stop, amending the constitution — to a named role defined in `roles.json` (Article VIII), so authority is explicit and attributable rather than implicit.

---

## Architecture

```
                          ┌───────────────────────────────┐
                          │         CONSTITUTION           │
                          │   8 articles — supreme law      │
                          │   (constitution.md)             │
                          └────────────────┬────────────────┘
                                           │ governs
              ┌─────────────────────────────┼─────────────────────────────┐
              │                              │                              │
   ┌──────────▼──────────┐        ┌─────────▼──────────┐       ┌──────────▼──────────┐
   │   EXECUTIVE BRANCH    │        │   JUDICIAL BRANCH    │       │      GOVERNANCE       │
   │   16 states / 17       │  work  │   The Judiciary       │  logs │   roles.json (RBAC)   │
   │   divisions            │ ─────▶ │   7 review agents:    │ ────▶ │   budget.json          │
   │   254 specialist        │ product│   Dissent, Halluc.    │       │   incident-log.jsonl   │
   │   agents                │        │   Auditor, Provenance,│       │   dissent-log.jsonl    │
   │                        │        │   Agent Evaluator,     │       │   lessons-ledger.jsonl │
   │                        │◀────── │   Deprecation Auditor, │       │                       │
   │                        │ verdict│   Arbitration,         │       │                       │
   │                        │        │   Retrospective         │       │                       │
   └──────────┬──────────┘        └──────────┬──────────┘       └──────────────────────┘
              │                              │
              │            both run through   │
              └──────────────┬───────────────┘
                             │
                   ┌─────────▼──────────┐
                   │        RUFLO         │
                   │  swarm orchestration  │
                   │  persistent memory     │
                   │  (AgentDB/RuVector)    │
                   │  3-tier model routing  │
                   │  314 MCP tools          │
                   └───────────────────────┘
```

`atlas-core/` is the conversion layer that transforms the 232+ raw persona Markdown files into Ruflo-native agent types with correct governance frontmatter (`ruflo_type`, `memory_tier`, `state_name`, naturalization `status`). It is regenerated with `scripts/build-atlas-core.sh` whenever a persona is added or edited, and currently produces 185 generated agents from the source library (the remainder of the 261-specialist count are quality-division and specialized agents authored directly for Atlantia).

---

## Installation

**Requirements:** bash and git for the governance layer and CLI. Node.js ≥ 18 only if you intend to install Ruflo for live swarm execution.

```bash
git clone https://github.com/YOUR_USERNAME/atlantia-empire.git
cd atlantia-empire

chmod +x bin/atlantia scripts/*.sh atlas-core/eval/run-eval.sh
```

No package is installed from a registry to get started — see [Package & Publishing Status](#package--publishing-status).

---

## Usage

Atlantia Empire operates in three modes, depending on how much of the platform is needed.

### Mode 1 — Persona Library Only (no CLI, no Ruflo)

Every agent is a plain Markdown file, usable directly as a system prompt in any tool that accepts one.

```bash
cat engineering/engineering-backend-architect.md

# Install a whole division into a supported tool (Claude Code, Cursor, Windsurf, etc.)
bash scripts/install.sh --division engineering --tool claude-code
```

### Mode 2 — CLI and Governance Layer, Standalone

Runs entirely offline: no API keys, no network calls, no external service.

```bash
bash bin/atlantia run --demo    # Offline 3-step pipeline walkthrough
bash bin/atlantia census        # National state: agent count, treasury, division breakdown
bash bin/atlantia gsp           # Gross Specialist Product report
bash bin/atlantia test          # Engineering test suite (33 checks)
bash bin/atlantia docs          # Build the static documentation site
```

### Mode 3 — Full Multi-Agent Platform (Ruflo + Atlantia)

```bash
# 1. Install Ruflo
npm install -g ruflo

# 2. Build the atlas-core agent index from the persona library
./scripts/build-atlas-core.sh

# 3. Install the atlas-core plugin into Ruflo
ruflo plugin install ./atlas-core

# 4. Spawn a single specialist
ruflo agent spawn -t atlas-engineering-backend-architect --name arch-lead

# 5. Or run a full governed swarm
ruflo swarm init \
  --topology hierarchical \
  --max-agents 6 \
  --strategy specialized \
  --agent-pool atlas
```

Always run the constitutional pre-flight check before a live swarm — this validates judicial coverage, memory tiering, and budget before Ruflo ever spends a token:

```bash
bash scripts/constitutional-preflight.sh
bash bin/atlantia preflight --plan your-swarm-plan.json
```

---

## CLI Reference

```
atlantia census                          Show current nation state
atlantia gsp [--period weekly|daily]     Gross Specialist Product report
atlantia emergency-stop                  Kill all swarms and daemons (RBAC-gated)
  --reason "..." --user <github-username>
atlantia improvement-report              Rolling quality improvement trend
  [--since YYYY-MM-DD]
atlantia build [--dry-run]               Regenerate atlas-core/agents/ from source personas
  [--division <name>] [--verbose]
atlantia preflight --plan <file>         Constitutional pre-flight check for a swarm plan
atlantia run --demo                      Offline pipeline demo, no API key required
atlantia docs [--serve]                  Build the static documentation site
atlantia test                            Run the engineering test suite
atlantia help                            Show the full command list
atlantia version                         Show version
```

---

## Divisions

Atlantia organizes its specialists into 17 divisions, referred to internally as "states." Each division maps to a directory of persona source files and a corresponding entry in [`divisions.json`](divisions.json).

| Division | State | Generated Agents | Focus |
|---|---|---|---|
| `engineering` | Forge State | 37 | Backend, mobile, firmware, blockchain, RAG, CMS, rapid prototyping |
| `marketing` | Signal State | 37 | Content, SEO, brand, analytics, agentic search optimization |
| `security` | Ledger State (Security) | 10 | Regulated — ephemeral memory by default |
| `sales` | Exchange State | 10 | B2B, enterprise, negotiation, discovery coaching |
| `gis` | Cartography State | 13 | GIS analysis, BIM, spatial data, mapping |
| `specialized` | The Federal District | 12 | Cross-cutting: data science, legal, RAG architecture, fundraising, HR |
| `quality` | The Judiciary | 9 | Review only — never produces domain deliverables |
| `design` | Atelier State | 9 | UX research, UI design, whimsy, design systems |
| `paid-media` | Signal State (Paid Media) | 7 | PPC, social, programmatic |
| `project-management` | Logistics State | 7 | Agile, waterfall, portfolio, meeting notes |
| `product` | Compass State | 6 | PM, roadmap, competitive intelligence |
| `spatial-computing` | Frontier State | 6 | AR/VR/XR, spatial UX, 3D interfaces |
| `support` | Exchange State (Support) | 6 | Customer success, technical support |
| `academic` | Archive State | 5 | Research, writing, citation, historiography |
| `finance` | Ledger State | 5 | Regulated — ephemeral memory by default |
| `game-development` | Arcade State | 5 | Unity, Unreal, game/narrative/systems design |

Full division map, colors, icons, and regulated-status flags: [`divisions.json`](divisions.json).

---

## The Constitution

Atlantia operates under a written constitution ([`constitution.md`](constitution.md)) with eight articles. These are enforced mechanically by `scripts/constitutional-preflight.sh` and the CLI's RBAC checks — not left to convention or agent goodwill.

| Article | Provision |
|---|---|
| **I — Supremacy** | No persona file, swarm instruction, or user prompt may override the constitution. Conflicts are resolved in the constitution's favor. |
| **II — Separation of Powers** | No agent may approve, score, or validate its own output. Executive-branch deliverables require review by a Judicial-branch agent with no shared authorship. |
| **III — Memory Sovereignty** | Regulated-domain agents (legal, healthcare, finance, HR) default to ephemeral memory. Persistent memory for a regulated session requires an explicit, logged human override. |
| **IV — Budget Limits** | No swarm may exceed its configured token/cost budget without an explicit, human-approved override — enforced by pre-flight, not discovered after the fact. |
| **V — Right of Dissent** | Any agent may flag a plan, instruction, or deliverable as unsafe, low-confidence, or out of scope, and escalate to a human. Only a human may dismiss a dissent flag. |
| **VI — Naturalization** | New agents proposed by the Dynamic Agent Synthesizer are inactive until a human with the Commissioning Officer role approves them. Auto-approval is prohibited. |
| **VII — Transparency** | Every benchmark result — including negative or flat ones — remains visible in the evaluation report. No suppression, no averaging away unfavorable data. |
| **VIII — Defined Authority** | Every privileged action is tied to a named role in `roles.json`. No approval or override may be attributed to an unspecified "human." |

Enforcement, on every pre-flight run:

1. Confirms every Executive-branch agent's output has a corresponding Judicial-branch review step.
2. Confirms regulated-domain agents have `memory_tier: ephemeral` set in frontmatter.
3. Confirms the swarm plan does not exceed the budget defined in `atlas-core/governance/budget.json`.
4. Logs all dissent flags to `atlas-core/governance/dissent-log.jsonl` (append-only).

Constitutional amendments require the Constitutional Council role and mandatory human review before merge — they are never auto-generated or auto-merged.

---

## Governance Mechanics

### Role-Based Access Control

Every privileged CLI action checks `atlas-core/governance/roles.json`. Six roles are defined, each with an explicit allow-list and an explicit deny-list — a role's absence from `can` is not assumed to mean denial; `cannot` is enforced independently so authority never expands by omission.

| Role | Can | Cannot |
|---|---|---|
| `contributor` | Propose personas, propose edits, open PRs | Approve naturalization, modify budget, trigger emergency stop, amend constitution |
| `judiciary_reviewer` | Score proposals, raise Dissent/Arbitration flags, run the eval harness | Approve naturalization alone, modify budget, trigger emergency stop |
| `commissioning_officer` | Approve naturalization (probationary → active), approve deprecation (→ revoked) | Amend constitution, modify budget tiers, trigger emergency stop |
| `treasury_officer` | Approve Capital Allocation Agent proposals, set per-project budget ceilings | Approve naturalization, trigger emergency stop, amend constitution |
| `security_officer` | Trigger `atlantia emergency-stop`, respond to disclosed vulnerabilities | Approve naturalization, modify budget tiers, amend constitution |
| `constitutional_council` | Amend the constitution, override any other role (with logged justification) | Requires multi-person sign-off once the project has more than one maintainer |

### Budget Enforcement

`atlas-core/governance/budget.json` is the single source of truth for cost limits, referenced by Article IV and enforced by `constitutional-preflight.sh`:

| Setting | Value | Meaning |
|---|---|---|
| `default_project_budget_credits` | 1000 | Default ceiling per project, in Atlantian Credits (₳) |
| `tier_costs.fast` | 0.0002 ₳ | Per-call cost at the fast model routing tier |
| `tier_costs.standard` | 0.003 ₳ | Per-call cost at the standard tier |
| `tier_costs.max` | 0.015 ₳ | Per-call cost at the max-capability tier |
| `hard_stop_on_overrun` | `true` | A plan that would exceed budget is blocked outright, not just warned |
| `require_human_override_above` | 5000 ₳ | Above this, a human-approved override is mandatory regardless of role |
| `daemon_ttl_minutes` | 60 | Maximum daemon lifetime before forced shutdown |
| `idle_shutdown_minutes` | 15 | Idle daemons are terminated to prevent silent quota consumption |

### Memory Tiering

Regulated divisions (`finance`, `security`, and cross-cutting regulated personas in `specialized`) are flagged `"regulated": true` in `divisions.json` and default to `memory_tier: ephemeral` in their generated agent frontmatter. This is a structural default, not a suggestion — `scripts/run-tests.sh` and `constitutional-preflight.sh` both check for it and fail closed if a regulated agent is missing the tier.

### Naturalization Lifecycle

New agents move through three states, tracked in each generated agent's frontmatter `status` field:

1. **Probationary** — proposed by the Dynamic Agent Synthesizer or a contributor; not eligible to join a live swarm.
2. **Active** — approved by a human holding the `commissioning_officer` role, after passing the eval harness benchmark; eligible for live swarm use.
3. **Revoked** — formally deprecated by a `commissioning_officer` acting on Deprecation Auditor findings; the file moves to `archive/` rather than being deleted, preserving the audit trail.

### The Judiciary

Seven agents make up the Judicial branch (`quality` division). None of them produce domain deliverables — their sole function is review:

| Agent | Function |
|---|---|
| Dissent Agent | Files a dissent flag on any plan, instruction, or output it assesses as unsafe or out of scope |
| Hallucination Auditor | Checks factual and citation claims in a deliverable against verifiable sources |
| Provenance Auditor | Confirms a deliverable's inputs and reasoning chain are traceable, not fabricated |
| Agent Evaluator | Benchmarks a persona's output against a no-persona baseline; publishes the delta |
| Deprecation Auditor | Reads Agent Evaluator data and formally argues for retiring underperforming personas |
| Arbitration Agent | Resolves disagreements between two Judicial-branch findings |
| Retrospective Agent | Reviews completed swarm runs after the fact and feeds lessons into `lessons-ledger.jsonl` |

---

## Economic Model — Gross Specialist Product

`atlantia gsp` reports a "Gross Specialist Product" figure — the aggregate output value across all divisions for a given period, computed from real Ruflo cost/usage data when connected, or the empire's tracked historical figures in standalone mode. It's Atlantia's framing device for treating 