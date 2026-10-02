# CLAUDE.md

## Project
Agent Reach — Python CLI + library that gives AI agents read/search access to 15 internet platforms.
Positioning: installer + doctor + config tool. NOT a wrapper — after install, agents call upstream tools directly.
This repo: github.com/rodrigotrigo80/agent-reach (fork of Panniantong/Agent-Reach) | License: MIT | Version: 1.5.0

## Commands
- `pip install -c constraints.txt -e ".[dev]"` — Dev install (same as CI; use a venv)
- `pytest -q` — All tests (what CI runs, on Python 3.10–3.13)
- `pytest tests/test_cli.py -v` — CLI tests only
- `ruff check .` — Lint (rules E, F, I; line length 100). Not enforced in CI; existing errors predate this file
- `mypy agent_reach` — Type check (tests excluded)
- `python -m build` — Build the wheel; CI checks it ships `skill/SKILL.md`, `guides/`, `scripts/`, `skill/references/`
- `python -m agent_reach.cli doctor` — Run diagnostics
- `python -m agent_reach.cli install --env=auto` — Auto-configure
- `bash test.sh` — Live smoke test. NOTE: it installs from the upstream GitHub `main` zip, so it does NOT test local changes

## Structure
- `agent_reach/cli.py` — CLI entry point (argparse)
- `agent_reach/core.py` — Core read/search routing logic
- `agent_reach/config.py` — Config management (YAML, env vars)
- `agent_reach/doctor.py` — Diagnostics engine
- `agent_reach/channels/` — One file per platform; registered in `channels/__init__.py` (`ALL_CHANNELS`)
- `agent_reach/channels/base.py` — `Channel` abstract base class
- `agent_reach/integrations/mcp_server.py` — MCP server integration
- `agent_reach/skill/`, `agent_reach/guides/` — Skill files and usage guides (shipped in the wheel)
- `tests/` — pytest; `tests/conftest.py` auto-isolates HOME and config for every test
- `config/mcporter.json` — MCP tool config

## Conventions
- Python 3.10+ with type hints
- Use `loguru` for logging, `rich` for CLI output
- Commit format: `type(scope): message` (one commit = one thing)
- All upstream tool calls go through public API/CLI, never hack internals
- Channel rules: `.claude/rules/channels.md` (loads when editing `agent_reach/channels/`)
- Testing rules: `.claude/rules/testing.md`

## Rules
- NEVER modify upstream open source projects' source code
- Agent Reach is a "glue layer" — only route and call, don't reimagine
- Version must match in `pyproject.toml` and `agent_reach/__init__.py`
- Always new branch for changes, PR to main, never push to main directly
- Run `pytest -q` before committing — all tests must pass
- Cookie-based auth (Twitter, XHS): use Cookie-Editor export method only, no QR scan (QR will hang)
