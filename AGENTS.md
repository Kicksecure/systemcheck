# Agents Guide

## Project Overview

systemcheck is a system health check tool for Kicksecure and Whonix.
It runs a series of diagnostic functions and reports results via CLI and GUI (msgcollector).

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
