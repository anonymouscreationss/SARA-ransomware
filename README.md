<div align="center">

<img src="assets/atlantia-logo-v2.png" alt="Atlantia Empire" width="140" />

# Atlantia Empire

### Constitutional Multi-Agent AI Governance Platform

**261 domain-specialist AI agents 路 17 divisions 路 a written constitution 路 a judiciary that reviews every deliverable**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Tests](https://img.shields.io/badge/tests-33%2F33_passing-brightgreen)](scripts/run-tests.sh)
[![Smoke Test](https://img.shields.io/badge/standalone-14%2F14_passing-brightgreen)](scripts/no-ruflo-smoke.sh)
[![Agents](https://img.shields.io/badge/agents-261-1B2A4A)](atlas-core/agents)
[![Divisions](https://img.shields.io/badge/divisions-17-D98E2B)](divisions.json)
[![Node](https://img.shields.io/badge/node-%3E%3D18.0.0-339933?logo=node.js&logoColor=white)](package.json)
[![Built by skynetfc](https://img.shields.io/badge/built_by-%40skynetfc-B23B3B)](https://skynetfc.netlify.app)

[Overview](#overview) 路
[Architecture](#architecture) 路
[Installation](#installation) 路
[Usage](#usage) 路
[CLI Reference](#cli-reference) 路
[Divisions](#divisions) 路
[Constitution](#constitution) 路
[Testing](#testing--ci) 路
[Repository Layout](#repository-layout) 路
[Contributing](#contributing) 路
[License](#license--attribution)

</div>

---

## Overview

Atlantia Empire is a governance layer for large multi-agent AI systems. It combines a library of 261 domain-specialist agents with a written constitution, a judiciary that reviews every deliverable before it ships, and an economic/operational model that tracks cost, quality, and authority for every agent action.

It is built on two upstream open-source projects rather than reinventing them:

| Layer | Provided by | License | Role |
|---|---|---|---|
| Persona library | [agency-agents](https://github.com/msitarzewski/agency-agents) | MIT | 232+ source persona files across engineering, marketing, finance, design, and more |
| Multi-agent runtime | [Ruflo](https://github.com/ruvnet/ruflo) | MIT | Swarm orchestration, persistent memory (AgentDB/RuVector), 3-tier model routing, 314 MCP tools |
| Governance layer | Atlantia Empire (this project) | MIT | Judicial/quality division, constitution, `atlas-core` conversion pipeline, CLI, eval harness, RBAC, budget enforcement |

Full third-party credit trail is documented in [`NOTICE`](NOTICE).

### Why a governance layer

Running 200+ AI personas as loose prompt files is straightforward, but it leaves three problems unsolved: nothing catches a bad output before it ships, nothing measures whether a given persona is actually better than no persona at all, and nothing enforces which agent is allowed to take which action. Atlantia Empire addresses each directly:

- **Judicial review is mandatory, not optional.** Seven judicial-branch agents 鈥� Dissent Agent, Hallucination Auditor, Provenance Auditor, Agent Evaluator, Deprecation Auditor, Arbitration Agent, Retrospective Agent 鈥� review every deliverable from an executive-branch agent before it is accepted.
- **Evaluation drives deprecation.** Every persona is benchmarked against a no-persona baseline. Negative and flat results are published, never suppressed (Constitution, Article VII), and the Deprecation Auditor formally retires personas that don't earn their keep.
- **Authority is explicit.** Every privileged action (spawning a swarm, approving naturalization, invoking emergency stop) maps to a named role in `atlas-core/governance/roles.json`. No agent can grant itself authority.
- **Cost and memory are governed up front.** Budget ceilings are enforced at pre-flight, not discovered after the fact. Regulated domains (finance, legal, security, HR) default to ephemeral memory.
- **A standalone CLI.** All governance features work without any hosted service 鈥� `atlantia census`, `gsp`, `preflight`, `emergency-stop`, and more run entirely on your own machine.

---

## Architecture

```
                     鈹屸攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�
                     鈹�        Constitution          鈹�
                     鈹�   (8 articles 鈥� supreme)     鈹�
                     鈹斺攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�
                                    鈹� governs
        鈹屸攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹尖攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�
        鈹�                            鈹�                            鈹�
鈹屸攢鈹�鈹�鈹�鈹�鈹�鈹�鈻尖攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�         鈹屸攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈻尖攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�        鈹屸攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈻尖攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�
鈹�  Executive      鈹�         鈹�   Judicial        鈹�        鈹�   Governance      鈹�
鈹�  (16 states /   鈹�  work 鈫� 鈹�   (Quality div.,  鈹�  logs  鈹�   (roles.json,    鈹�
鈹�  divisions)     鈹� product 鈹�   7 agents)        鈹� 鈹�鈹�鈹�鈹�鈹�鈹�鈻垛攤   budget.json,    鈹�
鈹�  261 agents     鈹傗攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈻垛攤   reviews before   鈹�        鈹�   incident log)   鈹�
鈹�                 鈹�         鈹�   acceptance       鈹�        鈹�                    鈹�
鈹斺攢鈹�鈹�鈹�鈹�鈹�鈹�鈹攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�         鈹斺攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�        鈹斺攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�
        鈹�                            鈹�
        鈹�           runs on          鈹�
        鈹斺攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�
                        鈹�
              鈹屸攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈻尖攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�
              鈹�       Ruflo         鈹�
              鈹�  swarm orchestration 鈹�
              鈹�  memory 路 model      鈹�
              鈹�  routing 路 MCP tools 鈹�
              鈹斺攢鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�鈹�
```

`atlas-core/` is the conversion layer that turns the 232+ raw persona Markdown files into Ruflo-compatible agent types with the correct frontmatter (`ruflo_type`, `memory_tier`, `state_name`, naturalization `status`). It is regenerated with `scripts/build-atlas-core.sh` whenever a persona is added or edited.

---

## Installation

**Requirements:** bash, git. Ruflo (optional 鈥� see [Usage](#usage) for standalone mode) requires Node.js 鈮� 18.

```bash
git clone https://github.com/YOUR_USERNAME/atlantia-empire.git
cd atlantia-empire

chmod +x bin/atlantia scripts/*.sh atlas-core/eval/run-eval.sh
```

No package is installed from a registry 鈥� see [Package & Publishing Status](#package--publishing-status).

---

## Usage

Atlantia Empire operates in three modes depending on how much of the platform you need.

### Mode 1 鈥� Persona library only (no CLI, no Ruflo)

Every agent is a plain Markdown file usable as a system prompt in any tool that accepts one.

```bash
cat engineering/engineering-backend-architect.md
bash scripts/install.sh --division engineering --tool claude-code
```

### Mode 2 鈥� CLI and governance layer, standalone

Runs entirely offline, with no API keys and no network calls.

```bash
bash bin/atlantia run --demo    # Offline pipeline walkthrough
bash bin/atlantia census        # National state: agent count, treasury, states
bash bin/atlantia gsp           # Gross Specialist Product report
bash bin/atlantia test          # Engineering test suite (33 checks)
bash bin/atlantia docs          # Build the static documentation site
```

### Mode 3 鈥� Full multi-agent platform (Ruflo + Atlantia)

```bash
npm install -g ruflo
./scripts/build-atlas-core.sh
ruflo plugin install ./atlas-core

ruflo agent spawn -t atlas-engineering-backend-architect --name arch-lead

ruflo swarm init \
  --topology hierarchical \
  --max-agents 6 \
  --strategy specialized \
  --agent-pool atlas
```

Always run the constitutional pre-flight check before a live swarm:

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
atlantia help                            Show the command list
atlantia version                         Show version
```

---

## Divisions

Atlantia organizes its 261 agents into 17 divisions, referred to internally as "states." Each maps to a directory of persona source files and a corresponding entry in [`divisions.json`](divisions.json).

| Division | State | Specialists | Focus |
|---|---|---|---|
| `engineering` | Forge State | 20+ | Backend, mobile, firmware, blockchain, RAG, CMS, rapid prototyping |
| `marketing` | Signal State | 10+ | Content, SEO, brand, analytics |
| `paid-media` | Signal State (Paid Media) | 6+ | PPC, social, programmatic |
| `sales` | Exchange State | 8+ | B2B, enterprise, negotiation |
| `support` | Exchange State (Support) | 4+ | Customer success, technical support |
| `finance` | Ledger State | 8+ | Regulated 鈥� ephemeral memory by default |
| `security` | Ledger State (Security) | 5+ | Regulated 鈥� ephemeral memory by default |
| `design` | Atelier State | 6+ | UX, brand, visual, design systems |
| `gis` | Cartography State | 4+ | GIS, BIM, spatial data, mapping |
| `testing` | Proving State | 5+ | QA, automation, accessibility, load testing |
| `academic` | Archive State | 4+ | Research, writing, citation, historiography |
| `game-development` | Arcade State | 4+ | Unity, Unreal, game/narrative/systems design |
| `product` | Compass State | 5+ | PM, roadmap, competitive intelligence |
| `project-management` | Logistics State | 4+ | Agile, waterfall, portfolio, meeting notes |
| `spatial-computing` | Frontier State | 3+ | AR/VR/XR, spatial UX |
| `specialized` | The Federal District | 12+ | Cross-cutting: data science, legal, RAG, fundraising, HR |
| `quality` | The Judiciary | 7 | Review only 鈥� never produces domain deliverables |

---

## Constitution

Atlantia operates under a written constitution ([`constitution.md`](constitution.md)) with eight articles, enforced mechanically by the governance layer rather than by convention.

| Article | Provision |
|---|---|
| I | Constitution is supreme 鈥� overrides any agent instruction or user prompt |
| II | Separation of powers 鈥� no agent reviews its own output |
| III | Memory sovereignty 鈥� regulated domains use ephemeral memory by default |
| IV | Budget limits 鈥� no swarm exceeds its configured budget without human override |
| V | Right of dissent 鈥� any agent may flag an output without being overridden by another agent |
| VI | Naturalization 鈥� new agents cannot join live swarms until human-approved |
| VII | Transparency 鈥� negative evaluation results must be visible, never suppressed |
| VIII | Defined authority 鈥� all privileged actions are tied to named roles in `roles.json` |

```bash
bash scripts/constitutional-preflight.sh
bash scripts/emergency-stop.sh --reason "runaway daemon" --user YOUR_USERNAME
```

### Evaluation Harness

```bash
./atlas-core/eval/run-eval.sh --dry-run               # Validate task/rubric structure
./atlas-core/eval/run-eval.sh --division engineering   # Live scoring (requires Ruflo)
cat atlas-core/eval/runs/latest/report.md              # Always includes negative/flat results (Article VII)
```

The eval harness runs automatically every Monday via `.github/workflows/eval-harness.yml`.

---

## Testing & CI

```bash
bash scripts/run-tests.sh        # 33-test engineering suite: frontmatter, RBAC, budget pre-flight, constitution
bash scripts/no-ruflo-smoke.sh   # 14-check standalone smoke test 鈥� verifies zero Ruflo dependency
```

| Workflow | Checks | Trigger |
|---|---|---|
| `.github/workflows/test-suite.yml` | Full 33-test engineering suite | Every push |
| `.github/workflows/no-ruflo-smoke.yml` | Standalone usability with zero Ruflo dependency | Every push, weekly |
| `.github/workflows/eval-harness.yml` | Persona eval harness, publishes negative results per Article VII | Weekly |

---

## Repository Layout

```
atlantia/
鈹溾攢鈹� bin/atlantia                 CLI entrypoint
鈹溾攢鈹� atlas-core/
鈹�   鈹溾攢鈹� agents/                  261 generated Ruflo-compatible agents, by division
鈹�   鈹溾攢鈹� governance/               roles.json (RBAC), budget.json, incident-log.jsonl
鈹�   鈹斺攢鈹� eval/                    Evaluation harness: tasks, rubrics, run script
鈹溾攢鈹� engineering/ marketing/ ...  232+ raw persona source files, by division
鈹溾攢鈹� scripts/                     build-atlas-core.sh, run-tests.sh, no-ruflo-smoke.sh, etc.
鈹溾攢鈹� docs/site/                   Generated static documentation site
鈹溾攢鈹� assets/                      Brand assets 鈥� flag, seals, currency, civic documents
鈹溾攢鈹� archive/                     Deprecated agent files (populated after a formal deprecation vote)
鈹溾攢鈹� constitution.md              The 8-article constitution
鈹溾攢鈹� divisions.json               Machine-readable division map
鈹溾攢鈹� package.json                 Local tooling manifest (see below)
鈹斺攢鈹� README.md
```

### Package & Publishing Status

`package.json` defines local tooling only (`npm run build`, `postinstall` chmod hooks, npm script aliases for the CLI). This project has not been published to the npm registry or PyPI. The `homepage` and `repository` fields are placeholders to be updated once you push this repository to your own GitHub account.

---

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for persona authoring conventions, the deprecation lifecycle, and pull request guidelines. See [`SECURITY.md`](SECURITY.md) to report a vulnerability.

---

## License & Attribution

Released under the [MIT License](LICENSE).

Built by [**@skynetfc**](https://skynetfc.netlify.app). Built on [Ruflo](https://github.com/ruvnet/ruflo) (MIT) and [agency-agents](https://github.com/msitarzewski/agency-agents) (MIT) 鈥� full credit trail in [`NOTICE`](NOTICE).
