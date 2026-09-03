---
name: gauge
description: Operate the Gauge platform with its `gauge` CLI to configure agent evaluations, scenarios, schedules, Agent Preference measurements, runs, skills, MCP servers, experiments, analytics, and organization settings. Use when a user asks to inspect or change Gauge, diagnose Gauge runs, or automate a Gauge workflow. Do not use merely because an unrelated project contains a gauge or measurement.
---

# Gauge CLI

Use the `gauge` CLI as the programmatic interface to Gauge. Help output and JSON responses are the source of truth for the installed version; do not guess flags from memory.

## Begin safely

1. Run `gauge --version`, then `gauge auth status` and `gauge whoami` when authentication or organization context is uncertain.
2. If `gauge` is missing and the task only needs help or read-only discovery, prefer a temporary `npx --yes @withgauge/cli@latest ...` invocation when package execution is allowed. Do not globally install it or change project dependencies without authorization. If temporary execution is unavailable, tell the user instead of searching unrelated `gauge` packages or the whole filesystem.
3. Resolve the intended organization before an org-scoped write. Prefer an explicit global `--org <slug-or-id>` for automation; otherwise inspect `gauge orgs list` and the configured default.
4. Use `gauge <group> --help` and `gauge <group> <command> --help` before unfamiliar or consequential operations.
5. Prefer `-o json` for reads that inform another command. Parse identifiers from JSON; never scrape the human table.

Gauge's core model is:

- An **eval** defines a task and observable pass criteria.
- An **Agent Preference prompt** measures what agents mention, choose, and use.
- A **scenario** defines what runs: repository, persona, agent/model roster, skills, and MCP servers.
- A **schedule** defines when an eval or preference prompt recurs. `NONE` is manual.
- A manual **run** is a separate launch; configuring or attaching something does not necessarily run it now.

Inspect before changing. Preserve IDs and immutable version refs returned by Gauge. Do not replace a scenario, criteria list, skill set, MCP set, or dashboard filter set until you have read the current object and confirmed replacement is intended.

## Authorization boundaries

Treat commands that launch runs, attach recurring configuration, change schedules, alter provider keys or billing, cancel work, ship experiments, or remove resources as consequential.

- Explain the concrete effect, scope, run count or credit impact shown by the CLI, and target organization before acting when the user has not already authorized that effect.
- Never add `--yes` merely to bypass a prompt. Use it only after the user has authorized that exact launch or recurring change, or in an already-approved unattended workflow with bounded scope.
- Authentication, browser sign-in, checkout, GitHub installation, and ambiguous destructive choices may require the user. Do not improvise credentials or expose tokens in output, files, command arguments, or traces.
- A CLI command's successful exit confirms acceptance, not completion of asynchronous work. Watch or re-read status when completion matters.

## Route by task

- For eval creation, scenario attachment, schedules, launching, and result review, read [references/evals-and-runs.md](references/evals-and-runs.md).
- For Agent Preference measurement and analytics, read [references/preference-and-analytics.md](references/preference-and-analytics.md).
- For skills, MCP servers, content experiments, and troubleshooting, read [references/assets-experiments-troubleshooting.md](references/assets-experiments-troubleshooting.md).

Keep reports outcome-first: state what changed or what was found, identify affected resources, and distinguish accepted, running, and completed states. Include IDs needed for the next operation without dumping secrets or irrelevant response bodies.

When creating reusable Gauge automation without usable credentials, test both failure and successful-authentication paths with a small local stub of the `gauge` executable. Exercise the final JSON assembly and exit codes end to end; a syntax check or unauthenticated-path test alone is insufficient.
