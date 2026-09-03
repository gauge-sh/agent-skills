# Assets, experiments, and troubleshooting

## Skills and MCP servers

A Gauge skill is a named asset with immutable version labels such as `vendor-cli@v2`. Register a qualified public source with `gauge skills add`; inspect the exact accepted reference forms through `gauge skills add --help`. If a source contains several skills, select one explicitly. Do not let a fuzzy search silently choose a publisher.

Read `gauge skills get <ref> -o json` after ingest. Record the returned handle, label, content digest, source resolution, and warnings. Reference the exact `name@label` from scenarios. Re-ingestion may create a new immutable version; it does not redefine historical runs.

Use `gauge skills runs <ref> -o json` to find runs that staged a skill. To measure impact, also keep a matched no-skill scenario; a skill-only run list has no control population.

MCP servers are separate assets with separate capabilities and trust boundaries. Check `gauge models list -o json` before attaching one: a harness may support skills but not MCP. Never treat an MCP server or plugin as a skill bundle.

## Content experiments

`gauge experiments` operates Fixer content experiments around fetch boundaries. It does not change a staged skill at a mid-run checkpoint. Skill comparisons require fresh matched runs.

The typical content flow is eligible run → boundaries → create or resume draft → inspect/edit/write → launch → watch → export → optionally ship. Use `gauge experiments --help` and the nested command help because modes and state transitions matter. Creating, launching, canceling, and shipping are distinct operations; authorization for one does not imply the others.

## Diagnose before retrying

- **Command or flag rejected:** inspect `gauge --version` and nested help. If the server reports a moved path or minimum version, upgrade through the user's approved installation path.
- **Not authenticated:** use `gauge auth status` and `gauge whoami`. Never print or reconstruct the token. Browser/device authorization may require the user.
- **Wrong organization:** inspect `gauge orgs list` and local config; retry with an explicit `--org` only after resolving the target.
- **Invalid asset ref:** list/get the asset and use its exact immutable `name@label`.
- **Capability mismatch:** consult `gauge models list -o json`; choose a compatible harness or remove the unsupported axis instead of repeatedly launching.
- **Billing or provider-key gate:** report the server's condition and stop. Do not change billing or keys without explicit authorization.
- **Queued or asynchronous work:** watch or poll the appropriate status with a bounded interval. Do not create duplicates because completion was not immediate.
- **Failed run:** inspect run detail, logs, insight, and diff before retrying. Separate product/agent failure from infrastructure failure and preserve the run ID in the report.

Do not retry a consequential write more than once unless the response establishes that it is idempotent or that the earlier request was not accepted.
