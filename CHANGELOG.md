# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-06-07

Initial release of this personal fork, rebranded from
[CloudAI-X/claude-workflow-v2](https://github.com/CloudAI-X/claude-workflow-v2)
(`project-starter`, MIT). Full upstream history is preserved in the git log.

### Changed

- Renamed the plugin to `cc`; all slash commands now use the
  `/cc:<command>` namespace.
- Rebranded author, repository, and homepage metadata to `trudogolik45`.

### Removed

- OpenAI Codex packaging (`.codex/`, `.codex-plugin/`, scanner config) — this
  fork targets Claude Code only.
- Stack-specific skills `convex-backend` and `vercel-react-best-practices`.
- `log-commands.sh` hook — logged raw Bash commands (secrets in args included)
  to a plaintext file with no rotation.
- `typescript-check.py` hook — ran `npx tsc` on every edit (latency +
  supply-chain exposure); covered by dedicated lint/verify steps instead.
- `validate-prompt.py` hook — always-advisory prompt scanner (noise + latency).
- Stale `CODEBASE.md` snapshot.

### Security

- `security-check.py`: replaced the over-broad `"test"/"spec"` substring skip
  with path-segment matching, so paths like `src/contest/` are still scanned.
- `track-metrics.py`: records only the commit SHA, never the commit message.
- `hooks.json`: quoted all `${CLAUDE_PLUGIN_ROOT}` expansions to prevent
  word-splitting on paths with spaces.
- `settings.json.template`: narrowed broad `curl` / `wget` / `docker run` /
  `npx` permission grants.

[1.0.0]: https://github.com/trudogolik45/claude-workflow-v2/releases/tag/v1.0.0
