# Agents Guide

## Project Overview

systemcheck is a system health check tool for Kicksecure and Whonix.
It runs a series of diagnostic functions and reports results via CLI and GUI (msgcollector).

## Tests

Comprehensive regression + hardening tests for systemcheck -- static-analysis
invariants (untrusted command/log output HTML-neutralised before the GUI
`setHtml()` path, the shared green `$status_ok` result token, pure-ASCII menu
breadcrumbs, always-quoted `${output_opts[@]}`, unambiguous `parse_cmd` short
options) plus functional checks of the pure bash helpers (`leaprun_cmd_describe`,
`remediation_instructions`) run in isolation -- are too high-volume for human
review and live in the AI-maintained dist-ai repo, not here:

  https://github.com/org-ai-assisted/dist-ai -> `usr/share/systemcheck-tests/`

Run them against this checkout:

    SYSTEMCHECK_REPO="$PWD" systemcheck-tests

## Architecture

- Shell scripts in `usr/libexec/systemcheck/` with a `.bsh` file name
  extension are sourced by the main `systemcheck` script.
- `preparation.bsh`'s `preparation` function is run before any other checks, and provides shared helpers.
- `updatecheck` is a related tool that checks for available package updates.
- The `canary` system (including the `canary-download` Python script) fetches and
  verifies Kicksecure's warrant canary. It runs as user `canary` under AppArmor
  confinement.
- `log-checker` is a standalone bash script invoked via `leaprun` (privilege escalation wrapper).
- Configuration is sourced from `/etc/systemcheck.d/*.conf` and `/usr/local/etc/systemcheck.d/*.conf`.

## Security review guidelines

See `agents/security.md`.
