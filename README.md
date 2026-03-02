# agent-TRIO

![No Doc, No Code](https://github.com/docularxu/agent-TRIO/releases/download/v0.1/no-doc-no-code.jpg)

A multi-agent software development framework built on one principle: **No Doc, No Code**.

Three AI agents - **Architect**, **Coder**, and **Reviewer** - collaborate through a structured two-phase workflow to deliver production-quality code.

## The Trio

| Role | Responsibility |
|------|---------------|
| **Architect** | Writes specs, orchestrates tasks, maintains SOPs |
| **Coder** | Implements code strictly following specs |
| **Reviewer** | Guards spec compliance and code quality |

## Two-Phase Workflow

![Two-Phase Development Flow](https://github.com/docularxu/agent-TRIO/releases/download/v0.1/phase-diagram.png)

**Phase 1: Spec Review** - Architect writes the spec, Reviewer challenges it (up to 3 rounds), Boss signs off.

**Phase 2: Task Execution (loop)** - For each task: Coder writes code → Reviewer audits against spec → git commit → SOP retro.

The spec documents (`PROJECT.md` + `docs/*.md` + `tasks-backlog.md`) serve as the bridge between the two phases.

## Key Design Decisions

- **Spec is law** - Coder implements only what the spec says. No freelancing.
- **Adversarial review** - Reviewer challenges both specs and code. No rubber stamps.
- **Shared blackboard** - All coordination happens through files (`.blackboard/`), not direct messaging.
- **SOP retro after every task** - Continuous improvement baked into the loop.
- **SOUL.md = Agent identity** - Each agent has a stable Core Principles section and an evolving Learned Rules section.

## Getting Started

1. Clone this repo
2. Copy `templates/*.md` to your agent workspace as `SOUL.md` for each agent
3. Create your project's `.blackboard/` directory:

```
.blackboard/
├── PROJECT.md          # Project description, goals, constraints
├── docs/*.md           # Module specs
├── tasks-backlog.md    # Task list and progress
├── task.md             # Current task details
├── review.md           # Review history
├── changelog.md        # Change log
└── retro.md            # SOP retro notes
```

4. Write your `PROJECT.md`, then let the Trio take it from there.

## Files

- **[framework.html](framework.html)** - Full framework specification (open in browser)
- **[article.md](article.md)** - Design article: how and why this framework was built
- **templates/** - SOUL.md templates for each agent role
  - [architect-soul.md](templates/architect-soul.md)
  - [coder-soul.md](templates/coder-soul.md)
  - [reviewer-soul.md](templates/reviewer-soul.md)

## Role Interaction

![Role Interaction Diagram](https://github.com/docularxu/agent-TRIO/releases/download/v0.1/roles-diagram.png)

## Origin

This framework was designed through a collaboration between a human (20+ years kernel dev) and AI agents, solving real problems encountered while building a quantitative trading system. The core insight: **AI agents without clear specs produce unreliable code. Give them specs, adversarial review, and structured workflows - and they deliver.**

## License

MIT
