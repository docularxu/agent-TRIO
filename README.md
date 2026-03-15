# 🤖 agent-TRIO

![No Doc, No Code](https://github.com/user-attachments/assets/9daa2b38-b6c6-4432-bdec-cd3f2f43594b)

A multi-agent software development framework built on one principle: **No Doc, No Code**.

Three AI agents - **Architect**, **Coder**, and **Reviewer** - collaborate through a structured two-phase workflow to deliver production-quality code. The spec documents serve as the contract between phases: Architect writes them, Reviewer challenges them, and Coder implements strictly against them.

Looking for code **analysis** instead of development? Check out [Source-Driven Agent Framework](https://github.com/docularxu/source-driven-agent-framework) - same team structure, but for reverse engineering and architecture analysis. **No Evidence, No Claim** instead of No Doc, No Code.

## Table of Contents

- [Core Design Principles](#-core-design-principles)
- [The Agent Team](#-the-agent-team)
- [Quick Start](#-quick-start)
- [Two-Phase Workflow](#two-phase-workflow)
- [Repository Structure](#-repository-structure)
- [Role Interaction](#role-interaction)
- [Experience Accumulation](#-experience-accumulation)
- [Adapting to Other Platforms](#-adapting-to-other-platforms)
- [Origin](#origin)
- [License](#license)

## 🌟 Core Design Principles

1. **No Doc, No Code**: Spec is law. Code without spec backing is a red flag. Code wrong? Fix the spec first, then regenerate.

2. **Adversarial Spec Review**: Before any code is written, Reviewer challenges the spec (up to 3 rounds). Garbage spec → garbage code.

3. **Shared Blackboard**: All coordination happens through files (`.blackboard/`), not chat history. Files persist across sessions, are auditable, and don't vanish on restart.

4. **SOP Self-Evolution**: Every task ends with a retro. Lessons become rules in each agent's `SOUL.md` (Learned Rules section), pruned periodically to prevent overfitting.

## 👥 The Agent Team

A "permanent team, rotating projects" approach - the agents persist across projects, accumulating expertise over time.

- **⚡ Architect**: Writes specs, orchestrates tasks, maintains SOPs. The bridge between human intent and agent execution.

- **💻 Coder**: Implements code strictly following specs. No freelancing. Every change must cite its spec source in the changelog.

- **🔍 Reviewer**: Guards spec compliance and code quality. Challenges specs in Phase 1, audits code in Phase 2. No rubber stamps - structured review.md required.

## 🚀 Quick Start

### Prerequisites

- [OpenClaw](https://github.com/openclaw/openclaw) installed and running
- An LLM provider configured (Anthropic, Google, OpenAI, etc.)

There are two ways to deploy:

**Option A: Let your AI agent do it.** Clone this repo, then tell your existing AI agent:

> "Read the README and configs at `~/agent-trio/`. Set up the three agents (Architect, Coder, Reviewer) with their SOUL.md files, configure communication, and verify everything works."

Your agent reads this repo, executes all the steps below, and reports back. You describe the intent, the agent handles the rest. This is how deployment works in the AI agent era.

**Option B: Do it yourself.** The detailed steps below are written so that either an AI agent or a human can follow them. If you prefer hands-on control, or if your agent needs a reference, here they are:

### 1. Deploy the Prompts

Clone this repo. Copy each agent's `SOUL.md` into their OpenClaw workspace:

```bash
# Create agents
openclaw agents add architect --model <your-model-id>
openclaw agents add coder --model <your-model-id>
openclaw agents add reviewer --model <your-model-id>

# Install SOUL.md
cp agents/architect/SOUL.md ~/.openclaw/agents/architect/SOUL.md
cp agents/coder/SOUL.md     ~/.openclaw/agents/coder/SOUL.md
cp agents/reviewer/SOUL.md  ~/.openclaw/agents/reviewer/SOUL.md
```

See `configs/openclaw-example.yaml` for a full configuration example with model choices and tool permissions.

### 2. Verify Agent Communication

Before starting real work, run a quick smoke test:

```
You → Architect:  "Ping Coder and Reviewer, confirm communication."
```

Expected result:
- Architect sends a message to Coder → Coder acknowledges
- Architect sends a message to Reviewer → Reviewer acknowledges
- Architect reports back: "All agents online, communication verified."

If any link fails, check `subagents.allowAgents` in your OpenClaw config (see `configs/openclaw-example.yaml`).

### 3. Set Up Your Project

Create a project directory with the blackboard structure:

```bash
mkdir -p my-project/.blackboard/archive my-project/docs
```

Write your `PROJECT.md` - describe what you're building, the tech stack, architecture, and constraints. This is the most important document: everything flows from it.

### 4. Run Phase 1 - Spec Review

Tell the Architect your intent. It will write the spec and trigger Reviewer:

```bash
openclaw agent --agent architect --message \
  "New project at ~/my-project/. Read PROJECT.md, write module specs in docs/ \
   and task list in .blackboard/tasks-backlog.md. Then trigger Reviewer for spec review."
```

Architect will:
1. Write `docs/*.md` module specs + `tasks-backlog.md`
2. Trigger Reviewer to challenge the spec (up to 3 rounds)
3. Ask you for sign-off when both sides agree

### 5. Run Phase 2 - Task Execution

After sign-off, Architect drives the task loop:

```bash
openclaw agent --agent architect --message \
  "Phase 1 signed off. Start Phase 2: dispatch the first task to Coder."
```

For each task: Architect writes `task.md` → Coder implements → Reviewer audits → git commit → retro. Reviewer's approval gates each task; you only intervene when 3 rounds fail.

### Tips

- **Use different models** for Coder and Reviewer for cross-model verification (e.g., Claude for Coder, Gemini for Reviewer).
- **Start serial** - one task at a time. Parallel execution is designed for but not needed initially.
- The blackboard files are the single source of truth. Agents communicate through files, not chat history.

## Two-Phase Workflow

![Two-Phase Development Flow](https://github.com/user-attachments/assets/aea6d618-2395-4a86-a618-59f0345a376d)

| Phase | Input | Output | Gate |
|-------|-------|--------|------|
| **1. Spec Review** (once) | Boss intent + PROJECT.md | docs/*.md, tasks-backlog.md, spec-review.md | Reviewer pass + Boss sign-off |
| **2. Task Execution** (loop per task) | task.md + specs + source code | Code + tests, review.md, changelog.md, retro.md | Reviewer pass (≤3 rounds) |

## 📁 Repository Structure

```
agent-trio/
├── README.md
├── agents/                         # Agent System Prompts (SOUL.md)
│   ├── architect/SOUL.md           #   Spec writer, orchestrator
│   ├── coder/SOUL.md               #   Strict implementer
│   └── reviewer/SOUL.md            #   Spec guardian, code auditor
├── configs/
│   └── openclaw-example.yaml       # OpenClaw configuration example
├── templates/                      # Legacy location (same as agents/)
│   ├── architect-soul.md
│   ├── coder-soul.md
│   └── reviewer-soul.md
├── docs/
│   └── framework.html              # Full framework specification (open in browser)
├── images/                         # Diagrams
│   ├── no-doc-no-code.jpg
│   ├── roles-diagram.png
│   └── phase-diagram.png
└── article.md                      # Design article: how and why
```

## Role Interaction

![Role Interaction Diagram](https://github.com/user-attachments/assets/08fefb51-795f-4bf2-ba15-b7ff5859eebe)

## 💡 Experience Accumulation

"Permanent team, rotating projects." When a project wraps up, don't delete the agents. Append lessons learned to each agent's `MEMORY.md` (SOUL.md Core Principles are read-only - only the human owner modifies them). The Learned Rules section evolves through retros. Your team gets sharper with every project.

## 🔄 Adapting to Other Platforms

The framework is designed around OpenClaw but the core ideas are platform-independent. The SOUL.md files work as system prompts for any multi-agent engine:

- **OpenClaw** - Use `openclaw agents add` as shown above
- **Claude Code / Cursor** - Use SOUL.md content as system prompts or project rules
- **AutoGen / CrewAI / LangGraph** - Map each SOUL.md to an agent definition; use `.blackboard/` as shared state

The key requirements: isolated agent sessions, file system access, and inter-agent messaging.

## Origin

This framework was designed through a collaboration between a human (20+ years kernel dev) and AI agents, solving real problems encountered while building a quantitative trading system. The core insight: **AI agents without clear specs produce unreliable code. Give them specs, adversarial review, and structured workflows - and they deliver.**

## License

MIT

---

*Designed by Guodong Xu - [docularxu](https://github.com/docularxu)*
