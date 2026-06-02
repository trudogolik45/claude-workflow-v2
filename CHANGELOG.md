# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.6] - 2026-02-14

First 2.x release. This entry consolidates the 2.0.x line, which shipped rapidly
after 1.2.0 and was not previously recorded here.

### Added

- **skills.sh cross-agent compatibility** — the workflow can be distributed to 38+ AI coding agents.
- **npm installer** (`packages/add-skill`) — `npx install-claude-workflow-v2` installs the plugin's agents, commands, skills, and hooks into a project.
- **Codex-compatible plugin artifacts** — `.codex/` and `.codex-plugin/` mirror the seven agents and the hook set for Codex-compatible clients, with a CI validation workflow.

### Changed

- Plugin, marketplace, and npm package versions reconciled at `2.0.6` (across `plugin.json`, `marketplace.json`, and `package.json`).
- Reworked the `verify-on-complete.py` Stop hook.
- Refreshed the `error-handling` and `security-patterns` skills.

### Fixed

- Resolved a batch of audit findings across agents, hooks, skills, and documentation.
- `security-check.py` no longer blocks documentation/example files that contain illustrative credential strings, and routes its block reason to stderr so Claude receives it.
- Restored the `convex-backend` and `vercel-react-best-practices` skill reference documents (`AGENTS.md`) that two SKILL.md files link to.
- Removed references to a non-existent `TaskOutput` tool from the orchestrator agent and parallel commands.

## [1.2.0] - 2026-02-14

### Added

- **4 New Skills**
  - `database-design` - Schema design, indexing strategies, query optimization, migrations
  - `devops-infrastructure` - Docker, CI/CD, deployment strategies, IaC, monitoring
  - `error-handling` - Error patterns by language, structured logging, retry/circuit breakers
  - `security-patterns` - JWT/OAuth auth, RBAC, secrets management, CORS, rate limiting

- **7 New Commands**
  - `/project-starter:refactor-guided` - 4-phase systematic refactoring with safety rules
  - `/project-starter:dependency-upgrade` - Safe dependency upgrades with rollback
  - `/project-starter:plan` - Manus-style persistent PLAN.md with phase tracking
  - `/project-starter:tutorial` - Interactive guided tutorial for first-time users
  - `/project-starter:bootstrap-repo` - 10-agent parallel repo exploration and CODEBASE.md generation
  - `/project-starter:save-session-learnings` - Persist session discoveries to CLAUDE.md/AGENTS.md
  - `/project-starter:metrics` - View agent performance metrics and session history

- **5 New Hooks**
  - `pre-commit-check.py` - Detects debug statements, temp markers, large file content
  - `branch-protection.sh` - Warns on git operations targeting protected branches
  - `typescript-check.py` - Runs `tsc --noEmit` after editing .ts/.tsx files
  - `suggest-doc-updates.py` - Suggests CLAUDE.md updates when significant changes detected
  - `track-metrics.py` - Logs session telemetry to `.claude/agent-metrics.jsonl`

- **On-Demand Context Loading**
  - All 14 skills now include "When to Load" sections with trigger/skip conditions

- **CI/CD Pipeline**
  - `.github/workflows/validate.yml` - Plugin validation (JSON, scripts, frontmatter, links)

### Changed

- **All 7 Agents Upgraded** with:
  - Action-first directive (act before explaining)
  - Adaptive effort scaling (Instant/Light/Deep/Exhaustive)
  - Adversarial self-review protocol (5-point checklist)
  - Intellectual honesty framework (Certain/Likely/Uncertain)
  - Expanded auto-trigger keywords
  - Paired WRONG/CORRECT anti-pattern examples for each domain
- **`web-design-guidelines` skill** rewritten as self-contained 190-line reference (was 37-line external URL dependency)
- **Hook error messages** now include actionable remediation suggestions across all hooks
- **`debugger` agent** gains escalation protocol (after 3 failed attempts: web search, backtrack)
- **`security-auditor` agent** gains dependency vulnerability checking section

---

## [1.1.0] - 2025-01-07

### Added

- **Parallel Execution Support**
  - Orchestrator can now spawn N subagents simultaneously for N independent tasks
  - Uses `run_in_background: true` with Task tool for concurrent execution
  - Results collected via TaskOutput after all agents complete
  - Roughly Nx faster than sequential execution

- **New Commands**
  - `/project-starter:parallel-review` - Review multiple directories in parallel
  - `/project-starter:parallel-analyze` - Analyze code from architecture, security, performance, and testing perspectives simultaneously

- **New Skill**
  - `parallel-execution` - Patterns for spawning dynamic subagents, best practices for parallelization, and TodoWrite integration

### Changed

- Updated orchestrator agent with Parallel Execution Protocol section
- Enhanced verify-changes command with explicit parallel syntax
- Added parallel execution examples and diagrams to documentation

---

## [1.0.0] - 2025-01-01

### Added

- **7 Specialized Agents**
  - `orchestrator` - Coordinate complex multi-step tasks
  - `code-reviewer` - Review code quality and best practices
  - `debugger` - Systematic bug investigation and fixing
  - `docs-writer` - Create technical documentation
  - `security-auditor` - Security vulnerability detection
  - `refactorer` - Code structure improvements
  - `test-architect` - Design comprehensive test strategies

- **6 Knowledge Skills**
  - `project-analysis` - Understand any codebase structure and patterns
  - `testing-strategy` - Design test approaches (unit, integration, E2E)
  - `architecture-patterns` - System design guidance
  - `performance-optimization` - Speed up applications, identify bottlenecks
  - `git-workflow` - Version control best practices
  - `api-design` - REST/GraphQL API patterns

- **4 Output Styles**
  - `/project-starter:architect` - System design mode
  - `/project-starter:rapid` - Fast development mode
  - `/project-starter:mentor` - Learning/teaching mode
  - `/project-starter:review` - Code review mode

- **8 Automation Hooks**
  - Security scan (blocks commits with potential secrets)
  - File protection (blocks edits to lock files, .env, .git)
  - Auto-format (runs prettier/black/gofmt based on file type)
  - Command logging (logs to `.claude/command-history.log`)
  - Environment check (validates Node.js, Python, Git)
  - Prompt analysis (suggests appropriate agents)
  - Input notification (desktop notification when input needed)
  - Complete notification (desktop notification when task finishes)

- Cross-platform notification support (macOS, Linux, Windows)
- Comprehensive documentation (README, PERMISSIONS, MCP servers guide)
- MIT License

[2.0.6]: https://github.com/CloudAI-X/claude-workflow-v2/releases/tag/v2.0.6
[1.2.0]: https://github.com/CloudAI-X/claude-workflow-v2/releases/tag/v1.2.0
[1.1.0]: https://github.com/CloudAI-X/claude-workflow-v2/releases/tag/v1.1.0
[1.0.0]: https://github.com/CloudAI-X/claude-workflow-v2/releases/tag/v1.0.0
