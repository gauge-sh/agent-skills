# Evals, scenarios, schedules, and runs

Agent Experience evals measure whether representative users' coding agents can use the customer's product successfully. The task is experimental stimulus; the run's value is the evidence it produces about product usability, documentation, APIs, SDKs, CLIs, and agent-facing context.

## Build the configuration deliberately

Read the available models with `gauge models list -o json` before pinning a model. Capability flags determine whether a harness supports skills, MCP servers, connections, or agent context.

Create or select a scenario for each configuration to compare. A scenario is the experimental cell, so keep every axis identical except the one being tested. Skill comparisons normally use matched scenarios carrying no skill, the current skill version, and a candidate version.

An eval prompt describes the developer's goal and starting state, not the command sequence the agent should discover. Each criterion should test one precise observable outcome. Avoid criteria based only on style or confidence when a file, command result, trace event, or external state can be checked.

Do not bake the expected implementation into the task. Exact APIs, providers, files, and architecture belong in criteria only when the user outcome truly requires them; otherwise they hide whether the product is discoverable and constrain the agent away from realistic behavior.

Before an edit, run the corresponding `get` command. In particular, providing criteria during `gauge evals edit` replaces the complete criteria list, and `--skill` or `--mcp` during a scenario edit replaces that complete asset set.

Use the subcommand help for current syntax:

```sh
gauge evals --help
gauge scenarios --help
gauge schedules --help
```

Attaching a scenario changes what an eval runs. Moving an eval or preference prompt onto a recurring schedule changes when future work is launched. These are different from `gauge evals run`, which launches immediately.

## Launch and observe

Before launching, identify the eval, selected scenarios, agent/model matrix, sample count, target organization, and the CLI's displayed run or credit arithmetic. Preserve interactive confirmation unless the exact launch was already authorized.

After acceptance:

- `gauge evals runs <id> -o json` returns judged run results for an eval.
- `gauge scenarios runs <id> -o json` isolates runs from one configuration.
- `gauge runs get <run-id> -o json` shows lifecycle, cost, turns, and exit information.
- `gauge runs watch <run-id>` follows a live run.
- `gauge runs logs <run-id>`, `insight`, and `diff` explain behavior and output.

Do not call a run failed merely because judging or warehouse ingestion is still pending. Distinguish queued/running, terminal execution, judge completion, and analytics availability.

## Compare candidates

Use repeated samples when agent behavior is stochastic. Compare pass rate first, then failure modes, turns, duration, tokens, cost, tool choices, recovery behavior, and unnecessary user handoffs. Inspect traces for causal evidence rather than attributing every outcome difference to the skill.

Translate observed failures into the product surface that can plausibly change. For example, distinguish failure to find the right package, failure to choose the product, failure to understand setup, incorrect implementation after reading the docs, and infrastructure failure. Report the evidence for that diagnosis and propose a controlled product or guidance change rather than treating completion of the run's coding task as the customer's end goal.

Change one meaningful instruction at a time. Register each candidate as an immutable skill version and rerun the same held-constant scenarios. Keep a no-skill control when measuring whether the skill helps at all.
