---
paths:
  - "agent_reach/channels/**/*.py"
---

# Channel rules

- Subclass `Channel` from `agent_reach/channels/base.py`. Set `name`, `description`, `backends` and `tier`.
- Required: `can_handle(url) -> bool` and `check(config=None) -> (status, message)`. `read`/`search` exist only where a channel routes calls itself (e.g. `web.py`, `v2ex.py`).
- `backends` is an ordered list: `[0]` is preferred, the rest are fallbacks. Switching backend means reordering, not rewriting.
- `check()` must set `self.active_backend` to the backend actually serving the channel, or `None`.
- `shutil.which()` is not proof of health. Execute a lightweight command (see `agent_reach.probe`) before marking a backend active.
- Honour user overrides through `ordered_backends(config)` (config key `<channel>_backend`, env `<CHANNEL>_BACKEND`).
- Register new channels in `channels/__init__.py` (`ALL_CHANNELS`), and add tests per `.claude/rules/testing.md`.
- Never modify an upstream tool's source. Call its public CLI or API only.
