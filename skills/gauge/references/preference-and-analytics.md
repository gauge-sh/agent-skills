# Agent Preference and analytics

Agent Preference asks what an agent mentions, chooses, or installs. It is distinct from an eval, which judges whether a task's observable criteria passed.

Interpret preference as a decision funnel, not a generic popularity score:

- **Not mentioned or considered:** investigate discoverability, category language, authoritative sources, and whether agents recognize the product as relevant.
- **Considered but not chosen:** inspect per-run rationale, competitors, trust signals, compatibility, differentiation, and source evidence.
- **Chosen or installed:** inspect what caused the choice, then use Agent Experience to measure whether implementation succeeds.

The objective is to improve the customer's product and its representation to coding agents. Do not frame a preference run as Gauge doing the underlying coding task for the customer.

Use `gauge preference --help` to discover the current create, edit, scenario, schedule, run, and synthesis operations. Read a prompt before editing it, preserve its immutable kind, and verify its scenarios and schedule separately.

Launching a preference prompt and synthesizing org-wide insights are consequential operations. State the target, configurations, expected run count, and credit impact before an unauthorized launch. Synthesis reads the whole Agent Preference surface and may complete asynchronously.

For broad orientation, `gauge status -o json` summarizes actions, visibility, Agent Preference, and agent experience. For reproducible reports:

1. Run `gauge query datasets -o json`.
2. Inspect `gauge query fields --dataset <name> -o json`.
3. Build a report only from returned dimensions and metrics.
4. Use the `run_preset` dimension—presented elsewhere as a scenario—to compare configured cells when a direct asset dimension is unavailable.

Use `gauge stats --help` for purpose-built summaries and `gauge query --help` for flexible warehouse reports. Do not invent field names or assume a recently finished run has already reached the warehouse.

When reporting results, give the population and time window, distinguish runs from events, and preserve zeroes. A missing or pending result is not a measured zero.
