[中文](README.md) | English

# Harness Engineering Study Guide

> A deep-dive learning archive on Harness Engineering — from concept to practice

## Introduction

This is an evolving learning project. **Harness Engineering** is an engineering paradigm proposed by OpenAI in February 2026: engineers stop writing code and instead design environments, clarify intent, and build feedback loops so AI agents can work reliably.

> **Humans steer. Agents execute.**

This repository documents the full learning journey — from reading the original article, breaking down concepts, forming independent thoughts, hands-on experiments, to producing shareable work. We hope it helps others exploring AI-native engineering.

Source: [OpenAI — Harness Engineering: Harnessing Codex in an Agent-First World](https://openai.com/zh-Hans-CN/index/harness-engineering/)

> **Note:** The insights shared here are not universally applicable. Please adapt them to your own context.

## ⚡ In One Sentence

```
Traditional:          Humans write code → Machines run code
Harness Engineering:  Humans design constraints → Agents write code → Machines run code
```

The core shift: **an engineer's output moves from code to constraint systems** — AGENTS.md, architecture rules, custom linters, and feedback loops.

## 🧭 Six Core Concepts

<details>
<summary><b>1. Repo as System of Record</b> — If it's not in the repo, it doesn't exist for the agent</summary>

Slack threads, Google Docs, knowledge in people's heads = invisible to the agent. All decisions, specs, and plans must be committed as versioned artifacts.

→ See [concepts/01-repo-as-source-of-truth.md](concepts/01-repo-as-source-of-truth.md)
</details>

<details>
<summary><b>2. Map, Not Manual</b> — AGENTS.md is a table of contents, not an encyclopedia</summary>

A ~100-line entry file pointing to deeper docs. Progressive disclosure: the agent starts from a small, stable entry point and is guided where to look next. Three ways a giant instruction file fails: crowds out context, impossible to maintain, can't be mechanically verified.

→ See [concepts/00-overview.md](concepts/00-overview.md)
</details>

<details>
<summary><b>3. Mechanical Enforcement</b> — Docs rot; lint rules don't</summary>

Custom linters + structural tests = invariant guardians. Lint error messages embed fix instructions so agents can self-correct. Enforce boundaries centrally, allow autonomy locally.

→ See [concepts/02-mechanical-enforcement.md](concepts/02-mechanical-enforcement.md)
</details>

<details>
<summary><b>4. Agent Readability</b> — Optimize for the agent's ability to reason</summary>

Prefer "boring" technologies (stable APIs, well-represented in training data). Sometimes re-implementing a focused subset is cheaper than wrapping opaque upstream behavior. Make the app launchable per git worktree.

→ See [concepts/04-agent-readability.md](concepts/04-agent-readability.md)
</details>

<details>
<summary><b>5. Throughput Changes Merge Philosophy</b> — Correction is cheap; waiting is expensive</summary>

Short PR lifecycles. Flaky tests resolved by re-runs rather than blocking indefinitely. In a system where agent throughput far exceeds human attention, this is usually the right call.

→ See [concepts/05-throughput-changes-merge.md](concepts/05-throughput-changes-merge.md)
</details>

<details>
<summary><b>6. Entropy Management = Garbage Collection</b> — Tech debt is a high-interest loan</summary>

Agents reproduce existing patterns in the repo — including bad ones. Codify "golden rules" into the repo. Run periodic background tasks to scan for drift, update quality scores, and open targeted refactoring PRs.

→ See [concepts/03-entropy-and-garbage-collection.md](concepts/03-entropy-and-garbage-collection.md)
</details>

## 🔑 Key Data Points

| Metric | Data |
|--------|------|
| Team size | 3 → 7 engineers |
| Time span | 5 months |
| Codebase | ~1 million lines |
| PRs merged | ~1,500 |
| PRs per engineer per day | 3.5 (still growing after scaling) |
| Single run duration | 6+ hours (often during human sleep) |
| Efficiency estimate | ~1/10 of manual coding time |

## 📂 Repository Structure

```
harness-engineering/
├── README.md           ← Chinese (primary)
├── README.en.md        ← You are here
├── AGENTS.md           ← Repo navigation entry (for agents)
│
├── concepts/           # Phase 1: Concept notes
│   ├── AGENTS.md       #   Directory guide + content index
│   ├── 00-overview.md  #   Overview of all six concepts
│   ├── 01-...          #   Repo as source of truth
│   ├── 02-...          #   Mechanical enforcement
│   └── 03-...          #   Entropy & garbage collection
│
├── thinking/           # Phase 2: Independent thinking & questioning
├── practice/           # Phase 3: Hands-on experiments
├── feedback/           # Phase 4: Lessons learned & iterations
├── works/              # Phase 5: Shareable outputs
├── prompts/            # Validated prompts collection
└── references/         # External resource index
```

Each subdirectory has its own `AGENTS.md` explaining its purpose and conventions — a direct practice of the "progressive disclosure" principle from the original article.

## 🚀 Learning Path

- [ ] **Phase 1: Understand core concepts** — Read `concepts/`, break down the six concepts
- [ ] **Phase 2: Form your own opinions** — Write critiques and extensions in `thinking/`
- [ ] **Phase 3: Pick a small project to practice** — Build from scratch with AI agents in `practice/`
- [ ] **Phase 4: Record feedback & iterations** — Document pitfalls and fixes in `feedback/`
- [ ] **Phase 5: Produce shareable work** — Distill into articles or tools in `works/`

## 🔗 Related Projects & Resources

### Source Material

| Resource | Description |
|----------|-------------|
| [OpenAI Original Article](https://openai.com/zh-Hans-CN/index/harness-engineering/) | The full Harness Engineering exposition |

### Ralph Series — Harness Engineering in Practice

The "Ralph Wiggum Loop" is the core implementation pattern of Harness Engineering: agents work autonomously in a loop until the task is complete.

| Project | Stars | Description |
|---------|-------|-------------|
| [snarktank/ralph](https://github.com/snarktank/ralph) | 13.6k | Original Ralph: bash script that repeatedly spawns AI with fresh context until all PRD items pass. 6 core tenets |
| [ralph-orchestrator](https://mikeyobrien.github.io/ralph-orchestrator/) | 2.3k | Rust evolution: Hat-based personas + event-driven coordination + multi-backend (Claude/Kiro/Gemini/Codex) + backpressure gates + persistent memory |
| [bmad-ralph](https://github.com/qianxiaofeng/bmad-ralph) | 2 | BMAD method + Ralph: parallel Claude Code worktrees + three-layer self-healing (retry → restart → diagnose) + SQLite state machine |

### Ralph Tenets ↔ Harness Engineering Mapping

| Ralph Tenet | Harness Engineering Concept |
|-------------|----------------------------|
| Fresh Context Is Reliability | Agent Readability — re-read everything each iteration |
| Backpressure Over Prescription | Mechanical Enforcement — don't prescribe how; gate bad output |
| The Plan Is Disposable | Entropy Management — regeneration costs one planning loop |
| Disk Is State, Git Is Memory | Repo as System of Record — files are the handoff mechanism |
| Steer With Signals, Not Scripts | Humans Steer — add signs, not scripts |
| Let Ralph Ralph | Agents Execute — sit on the loop, not in it |

### Anthropic Official — Harness Design in Practice

| Resource | Description |
|----------|-------------|
| [Harness design for long-running apps](https://www.anthropic.com/engineering/harness-design-long-running-apps) | Anthropic Labs: GAN-inspired three-agent architecture (Planner→Generator→Evaluator), sprint contracts, Context Anxiety fix, harness simplification as models improve |

### Efficiency Paradox & Capability Evolution

| Resource | Description |
|----------|-------------|
| [Why AI Codes Faster But Delivery Hasn't Changed](https://yousali.com/posts/20260303-ai-coding-efficiency-to-evolution/) | 16,667-word deep dive: Theory of Constraints on efficiency paradox, Spec/Rule/Skill separation, verification loops, concurrency strategies. "AI is today's NCX-10" |

### Community Resources

| Resource | Description |
|----------|-------------|
| [vibe-coding-cn](https://github.com/tukuaiai/vibe-coding-cn) | Chinese Vibe Coding community guide — great repo organization reference |

## 🤝 Contributing

Contributions via Issues and PRs are welcome:
- Add concept notes (`concepts/` has gaps to fill)
- Share your independent thinking (`thinking/`)
- Contribute practice cases (`practice/`)
- Recommend related resources (`references/`)

## 📞 Contact

| Channel | Link |
|---------|------|
| GitHub | [@deusyu](https://github.com/deusyu) |
| X (Twitter) | [@0xdeusyu](https://x.com/0xdeusyu) |
| Telegram | [@DeusThink](https://t.me/DeusThink) |
| Telegram Group | [@talkdeusyu](https://t.me/talkdeusyu) |
| Telegram Channel | [@lovedesuyu](https://t.me/lovedesuyu) |
| Email | [rainman.deus@gmail.com](mailto:rainman.deus@gmail.com) |

## Star History

If you find this project helpful, please consider giving it a Star ⭐!

[![Star History Chart](https://api.star-history.com/svg?repos=deusyu/harness-engineering&type=Date)](https://star-history.com/#deusyu/harness-engineering&Date)

## 📄 License

MIT
