---
name: gauge
description: Use Gauge Agents and its `gauge` CLI to measure and improve how coding agents discover, choose, and use a product. Interpret Agent Preference and Agent Experience results, compare scenarios, and operate evals and runs. This is not the unrelated Gauge test-automation framework, and the CLI does not operate Gauge Chat or Ask Gauge.
---

# Gauge Agents

Gauge Agents is an Agent Led Growth measurement platform. It runs real coding agents in real repositories, observes what they choose and how they work, judges outcomes, and turns those sessions into evidence about a product's agent experience.

The Gauge customer is usually a product, marketing, DevRel, or engineering team improving **their product for their users' coding agents**. A task performed during a Gauge run is an experimental probe. Do not mistake it for work that the Gauge customer is outsourcing for their own use.

## Identify the right Gauge product

Keep these boundaries explicit whenever the request is ambiguous:

- **Gauge Agents** measures whether coding agents discover, recommend, select, and successfully use a product. Its application is `agents.withgauge.com`; this is the product operated by the `gauge` CLI described here.
- **Gauge Chat** is Gauge's AI-visibility/GEO product. It tracks how brands appear in answer engines, analyzes prompts, mentions, citations, competitors, and traffic, and includes Ask Gauge and content workflows. It shares Gauge identity with Gauge Agents but has a separate application, product data, and session. The Gauge Agents CLI cannot configure Ask Gauge, inspect Gauge Chat conversations, or operate Gauge Chat's visibility/content workflows.
- The open-source **Gauge test-automation framework** is unrelated. Do not install or use its packages or documentation for Gauge Agents work.

If the available CLI exposes evals, preference, scenarios, runs, skills, and analytics at `agents.withgauge.com`, it is Gauge Agents.

## Understand what is measured

Gauge Agents has two complementary measurement surfaces:

- **Agent Preference** measures the decision before implementation: which products agents mention, recommend, choose, or install; which competitors win; and which sources or reasoning shape that choice.
- **Agent Experience** measures what happens after a product is in consideration: whether agents can complete representative tasks with it, where they become confused or stuck, and how product changes affect success. Evals define tasks and observable criteria; scenarios define repositories, agents/models, skills, MCP servers, and other experimental conditions; runs contain the resulting evidence.

An unbranded task that asks an agent to recommend or select a product normally measures Preference. A branded task that tells the agent which product to integrate normally measures Experience. Classify the intent of the measurement rather than relying only on the resource's name.

Use controls and matched scenarios to distinguish a product or guidance change from model variance. A skill, docs rewrite, SDK release, CLI change, or MCP server is a treatment to measure, not automatically the explanation for every difference.

## Interpret results as product evidence

When asked what to improve, answer the product question rather than merely recounting whether an agent finished:

1. Establish the population, time window, task, agents/models, scenarios, and sample size.
2. Start with the outcome: preference/selection rate or judged task success. Distinguish execution completion, judge completion, and analytics availability.
3. Inspect rationales, traces, logs, diffs, tool calls, sources, latency, turns, and cost to explain the outcome. Separate product friction from harness, provider, repository, or evaluator failures.
4. Identify the stage of the funnel and recommend changes at that stage:
   - not mentioned or considered: improve discoverability, positioning, terminology, and authoritative source coverage;
   - researched or considered but not chosen: improve persuasion, differentiation, trust signals, examples, compatibility evidence, and competitive clarity; substantial agent traffic to the product's docs is evidence against calling this merely a discovery problem;
   - chosen but implemented incorrectly: improve quickstarts, API/SDK/CLI ergonomics, error messages, examples, and agent-facing instructions;
   - correct but slow or inconsistent: reduce discovery steps, ambiguity, context load, and recovery cost.
5. State what the evidence supports, what remains uncertain, and the smallest next experiment. Prefer one changed axis, matched controls, and repeated samples.

A passed run means the tested agent satisfied the eval criteria in that scenario. It does not prove the product is universally easy to use. A failed run is not automatically an agent-quality problem: it may be the most useful evidence of a gap in the measured product.

Validate the measurement before prescribing a product change. A preference prompt framed around the wrong job or category can manufacture the wrong competitor set. An off-task implementation run, incomplete catalog scan, or infrastructure failure may carry little product signal. When designing evals, specify the user goal and observable outcome without prescribing exact APIs, providers, files, or architecture that the agent should discover.

Do not make absence or coverage claims from a partial list. Inspect the complete relevant catalog first. When Gauge results conflict with a one-off result from another chat, model, prompt, or environment, explain that both are samples under different conditions and propose a matched replication instead of declaring either one universally correct.

## Use the CLI as the evidence interface

Use `gauge --help`, nested help, and JSON responses as the source of truth for the installed CLI version. Prefer `-o json` for reads that inform analysis or another command, parse returned identifiers, and resolve the intended organization before org-scoped work.

For an improvement question, begin with the smallest relevant read:

- `gauge status -o json` for broad orientation and recommended actions;
- Agent Preference results when the question is about awareness, recommendation, or selection;
- eval and run results when the question is about successful product use;
- individual traces, logs, insights, and diffs when aggregate outcomes need explanation;
- analytics datasets and fields before constructing a custom query.

Do not dump every available surface. Follow the evidence until you can give a coherent, prioritized answer tied to observed behavior.

Lead with the diagnosis and why it follows from the evidence before proposing mutations. Finish with a bounded conclusion even when evidence is incomplete; name the missing evidence instead of exhausting the response in investigation.

For detailed operations:

- Read [references/evals-and-runs.md](references/evals-and-runs.md) for eval configuration, scenarios, launches, and run investigation.
- Read [references/preference-and-analytics.md](references/preference-and-analytics.md) for Agent Preference and aggregate analysis.
- Read [references/assets-experiments-troubleshooting.md](references/assets-experiments-troubleshooting.md) for skills, MCP servers, content experiments, and operational failures.

## Preserve consequential boundaries

Inspect before changing. Criteria lists and scenario skill/MCP sets may be replacement operations. Treat launches, recurring schedules, provider keys, billing, cancellations, shipping, and removals as consequential; explain scope, organization, and displayed run or credit impact unless the user already authorized that exact effect. A successful CLI exit may mean accepted rather than completed, so follow asynchronous work when completion matters.

Never improvise credentials, expose tokens, or use `--yes` merely to bypass confirmation. If `gauge` is missing for a read-only task, a temporary `npx --yes @withgauge/cli@latest ...` invocation is preferable when package execution is allowed; do not globally install it or modify project dependencies without authorization.
