# Testing rules

- Every new channel needs `tests/test_<name>_channel.py`. Every changed channel needs its tests updated in the same commit.
- Minimum per channel: `can_handle()` accepts the platform's real URL forms (including upper-case and short links) and rejects other hosts and the empty string; `check()` covers "not installed", "installed but not configured" and "healthy".
- Use pytest with `unittest.mock.patch`. Never make real network calls or run real upstream CLIs in tests. Patch `shutil.which` and subprocess calls instead.
- Do not write to the real home directory. `tests/conftest.py` already redirects HOME and the config dir; don't bypass it.
- Do not depend on ambient environment variables. Clear what a test reads with `monkeypatch.delenv(..., raising=False)`. Known gap: `tests/test_config.py::TestConfig::test_get_configured_features` fails whenever `GITHUB_TOKEN` or `GH_TOKEN` is set.
- Run `pytest -q` before every commit; all tests must pass.
