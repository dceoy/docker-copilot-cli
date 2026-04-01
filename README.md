# docker-copilot-cli

Dockerfile and compose.yml for [GitHub Copilot CLI](https://github.com/github/copilot-cli)

[![CI/CD](https://github.com/dceoy/docker-copilot-cli/actions/workflows/ci.yml/badge.svg)](https://github.com/dceoy/docker-copilot-cli/actions/workflows/ci.yml)

This repository provides a Docker Compose workflow that:

- installs the GitHub Copilot CLI into `/usr/local` during the image build
- runs the container as a non-root `agent` user by default
- mounts the current repository at `/workspace`
- persists Copilot and GitHub CLI state in named Docker volumes

## Included tools

- [GitHub Copilot CLI](https://github.com/github/copilot-cli) (`@github/copilot`)
- [GitHub CLI](https://cli.github.com/) (`gh`)
- git, jq, npm, pipx, python3-pip, ripgrep, tree, unzip, uv, vim, wget, zsh
- [Oh My Zsh](https://ohmyz.sh/)
- [print-github-tags](https://github.com/dceoy/print-github-tags)

## Quick start

1. Build the image:

   ```bash
   docker compose build
   ```

2. Start an interactive Copilot CLI session:

   ```bash
   docker compose run --rm copilot-cli
   ```

   The service starts in `/workspace` and runs:

   ```bash
   zsh -lc 'copilot --yolo'
   ```

## Common commands

Override the default command when you want to run a specific tool inside the container:

```bash
docker compose run --rm copilot-cli -lc 'copilot --version'
docker compose run --rm copilot-cli -lc 'gh auth status'
docker compose run --rm copilot-cli -lc 'git status'
```

## Runtime layout

- The repository root is mounted to `/workspace`.
- Copilot state is persisted in the `copilot-data` volume at `/home/agent/.copilot`.
- GitHub CLI and other user config are persisted in the `config-data` volume at `/home/agent/.config`.
- The Compose service repairs volume ownership on startup when previously created as `root:root`.
- Build args `USER_NAME`, `USER_UID`, and `USER_GID` default to `agent`, `1001`, and `1001`.
