# docker-copilot-cli

Dockerfile and compose.yml for [GitHub Copilot CLI](https://github.com/github/copilot-cli)

[![CI/CD](https://github.com/dceoy/docker-copilot-cli/actions/workflows/ci.yml/badge.svg)](https://github.com/dceoy/docker-copilot-cli/actions/workflows/ci.yml)

## Included tools

- [GitHub Copilot CLI](https://github.com/github/copilot-cli) (`@github/copilot`)
- [GitHub CLI](https://cli.github.com/) (`gh`)
- git, jq, npm, pipx, python3-pip, ripgrep, unzip, uv, vim, wget, zsh
- [Oh My Zsh](https://ohmyz.sh/)
- [print-github-tags](https://github.com/dceoy/print-github-tags)

## Quick start

1. Build the image:

   ```bash
   docker compose build
   ```

2. Start an interactive session:

   ```bash
   docker compose run --rm copilot-cli
   ```
