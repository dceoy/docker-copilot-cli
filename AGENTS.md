# Repository Guidelines

## Project Structure & Module Organization

This repository is intentionally small and Docker-centric.

- `Dockerfile`: Builds the `copilot` CLI image on Ubuntu.
- `compose.yml`: Local runtime defaults (container name, mount, env wiring, entrypoint).
- `README.md`: Primary usage documentation.
- `CLAUDE.md`: Symlink to `AGENTS.md` for Claude-compatible repo guidance.
- `.agents/skills/local-qa/`: Local QA skill definition and `scripts/qa.sh`.
- `.github/workflows/ci.yml`: CI/CD orchestration (lint/scan, build/push, Dependabot auto-merge).
- `.github/dependabot.yml` and `.github/renovate.json`: dependency update automation.

Keep new files at the repo root unless they are CI/config/automation artifacts (`.github/...`, `.agents/...`).

## Build, Test, and Development Commands

Use Docker Compose for all local workflows:

- `docker compose build` builds the `copilot-cli` image from `Dockerfile`.
- `GITHUB_TOKEN=... docker compose run --rm copilot-cli` starts an interactive CLI session.
- `cd .agents/skills/local-qa && ./scripts/qa.sh` runs repository markdown formatting plus lint/security checks.

Before opening a PR, at minimum run a fresh build and one container smoke test.

## Coding Style & Naming Conventions

- YAML uses 2-space indentation and lowercase keys.
- Dockerfile instructions are uppercase (`FROM`, `RUN`, `ENV`) with grouped, multi-line `RUN` blocks.
- Prefer explicit shell safety (`bash -euo pipefail`) and deterministic install steps.
- Keep filenames lowercase (`compose.yml`, `ci.yml`) and use descriptive, tool-oriented names.

## Testing Guidelines

There is no unit-test framework in this repo. Validation is operational:

- Local: run `docker compose build`, at least one `docker compose run --rm copilot-cli ...` smoke test, and `.agents/skills/local-qa/scripts/qa.sh`.
- CI:
  - `docker-lint-and-scan` runs on pushes and pull requests to `main` (and via `workflow_dispatch` when `workflow=lint-and-scan`).
  - `docker-build-and-push` runs only via `workflow_dispatch` when `workflow=build`.

If behavior changes, include exact verification commands and outcomes in the PR description.

## Commit & Pull Request Guidelines

Follow the existing history's style: concise, imperative, sentence-case summaries (example: `Add Dockerfile and compose.yml for GitHub Copilot CLI`).

PRs should include:

- What changed and why.
- Any related issue link.
- Local validation commands executed.
- Screenshots only when UI/output clarity benefits from them.

## Security & Configuration Tips

Never commit API keys or tokens. Provide secrets via environment variables at runtime. Keep `GITHUB_TOKEN` and other credentials local-only.
