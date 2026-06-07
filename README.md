# cc

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-v2.1+-blue.svg)](https://code.claude.com)

A lean, personal Claude Code workflow plugin — specialized agents, a curated set of
multi-agent slash commands, on-demand knowledge skills, and a branch-protection guardrail.
Forked and trimmed from [CloudAI-X/claude-workflow-v2](https://github.com/CloudAI-X/claude-workflow-v2).

> Slash commands are namespaced by the plugin name, so every command is `/cc:<command>`.

---

## Install

This repo is its own marketplace. Install it locally:

```bash
# Register this directory as a marketplace, then install the plugin
claude plugin marketplace add /path/to/claude-workflow-v2
claude plugin install cc@claude-workflow --scope user
```

Or run a session against it without installing:

```bash
claude --plugin-dir /path/to/claude-workflow-v2
```

Verify with `/plugin` → **Installed** should list `cc`. After install, run `/reload-plugins`
(or restart) so the `/cc:` commands appear.

---

## What's Included

| Component    | Count | Notes                                                                       |
| ------------ | ----- | --------------------------------------------------------------------------- |
| **Agents**   | 7     | orchestrator + code-reviewer, debugger, docs-writer, security-auditor, refactorer, test-architect |
| **Commands** | 7     | multi-agent fan-out, planning, and onboarding (`/cc:<command>`)             |
| **Skills**   | 9     | on-demand knowledge domains (auto-load on topic match)                      |
| **Hooks**    | 1     | `branch-protection` wired; other guardrail scripts ship dormant in `hooks/` |

Always-on context cost: **~1,900 tokens/session** (component descriptions).

---

## Commands

All commands are invoked as `/cc:<command>`.

| Command                  | Purpose                                                       |
| ------------------------ | ------------------------------------------------------------ |
| `/cc:verify-changes`     | Multi-subagent adversarial verification of recent changes    |
| `/cc:parallel-review`    | Review multiple files/dirs in parallel via subagents         |
| `/cc:parallel-analyze`   | Multi-perspective analysis (arch/security/perf/testing)      |
| `/cc:security-scan`      | Security vulnerability scan                                  |
| `/cc:bootstrap-repo`     | 10-agent parallel exploration → generates `CODEBASE.md`      |
| `/cc:plan`               | Persistent `PLAN.md` with phases and progress tracking       |
| `/cc:refactor-guided`    | 4-phase systematic refactoring with safety rules            |

---

## Agents

Claude spawns these automatically based on your task (the orchestrator can fan them out in parallel).

| Agent              | Purpose                          | Auto-triggers                                                |
| ------------------ | -------------------------------- | ------------------------------------------------------------ |
| `orchestrator`     | Coordinate multi-step tasks      | "improve", "build", "architecture", complex multi-part tasks |
| `code-reviewer`    | Review code quality (read-only)  | "review", "PR review", code changes                          |
| `debugger`         | Systematic bug investigation     | errors, crashes, leaks, timeouts, race conditions            |
| `docs-writer`      | Technical documentation          | README, changelogs, migration guides                         |
| `security-auditor` | Vulnerability detection (read-only) | auth, encryption, secrets, OAuth, JWT, CORS               |
| `refactorer`       | Behavior-preserving restructuring | tech debt, code smells, complexity reduction                |
| `test-architect`   | Test strategy & test writing     | test plans, mocking, flaky tests, integration/E2E            |

---

## Skills

Knowledge domains Claude loads autonomously when a task matches their description.

| Skill                     | Domain                                                |
| ------------------------- | ----------------------------------------------------- |
| `analyzing-projects`      | Map an unfamiliar codebase (tech-stack & infra detection) |
| `database-design`         | Schema, indexing, query tuning, migrations, pooling   |
| `designing-architecture`  | Pattern selection (Clean, Hexagonal, CQRS, Event-Driven) |
| `devops-infrastructure`   | Docker, CI/CD, deployment, IaC, observability         |
| `error-handling`          | Circuit breakers, correlation IDs, structured logging |
| `managing-git`            | Conventional commits, rescue ops, advanced git        |
| `parallel-execution`      | Multi-subagent fan-out patterns (Task tool)           |
| `security-patterns`       | Auth/RBAC/encryption/secrets/rate-limit implementation |
| `web-design-guidelines`   | ARIA, Core Web Vitals, CSS-engineering UI audit       |

---

## Hooks

The plugin wires a single guardrail by default:

| Hook                | Trigger      | Action                                  |
| ------------------- | ------------ | --------------------------------------- |
| `branch-protection` | PreToolUse(Bash) | Warns on commits/pushes to protected branches |

Additional hardened guardrail scripts ship dormant under `hooks/` (`protect-files.py`,
`security-check.py`, `pre-commit-check.py`, `format-on-edit.py`, …). Wire any of them by
adding an entry to `hooks/hooks.json`.

---

## Examples

See [examples/](./examples/) for worked multi-agent orchestration walkthroughs:

| Example | Description |
| --- | --- |
| [Comprehensive Code Review](./examples/orchestration/comprehensive-code-review/) | Multi-agent workflow for thorough code analysis |
| [Parallel Execution](./examples/orchestration/parallel-execution/) | Fan-out multi-subagent workflow for independent tasks |

---

## Configuration templates

Copy-paste starters live in [templates/](./templates/):

```bash
cp templates/settings.local.json.template /path/to/project/.claude/settings.local.json
cp templates/CLAUDE.md.template          /path/to/project/CLAUDE.md
cp templates/mcp.json.template           /path/to/project/.mcp.json
```

`CLAUDE.md.template` ships with the personal git/PR conventions (one-commit-per-file,
small PRs, squash-merge) pre-populated.

---

## Extending

Add components by dropping files into the plugin:

- **Commands** — `commands/<name>.md` with `description` (and optional `allowed-tools`) frontmatter → `/cc:<name>`.
- **Agents** — `agents/<name>.md` with `name`, `description`, `tools`, `model` frontmatter.
- **Skills** — `skills/<name>/SKILL.md` with `name` + `description` frontmatter.

---

## Plugin structure

```
claude-workflow-v2/
├── .claude-plugin/
│   ├── plugin.json           # Identity (name "cc" → /cc: namespace)
│   └── marketplace.json      # Marketplace + skill list
├── agents/                   # 7 specialized agents
├── commands/                 # 7 slash commands
│   ├── verify-changes.md
│   ├── parallel-review.md
│   ├── parallel-analyze.md
│   ├── security-scan.md
│   ├── bootstrap-repo.md
│   ├── plan.md
│   └── refactor-guided.md
├── skills/                   # 9 knowledge domains
├── hooks/
│   ├── hooks.json            # Wires branch-protection only
│   └── *.py / *.sh           # branch-protection active; others dormant
├── templates/                # User-copyable templates
├── examples/                 # Orchestration walkthroughs
├── CLAUDE.md                 # Plugin development guidelines
└── README.md
```

---

## Requirements

- **Claude Code** v2.1 or later
- **Python 3** (for hook scripts)
- **Git**

---

## Credits

Forked from [CloudAI-X/claude-workflow-v2](https://github.com/CloudAI-X/claude-workflow-v2)
(original plugin by [@cloudxdev](https://x.com/cloudxdev)). Workflow patterns inspired by
[Boris Cherny](https://x.com/bcherny). MIT — see [LICENSE](./LICENSE); the upstream copyright is retained.

## License

MIT — see [LICENSE](./LICENSE) for details.
