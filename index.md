---
title: Home
layout: home
nav_order: 1
---

# Building a Secure Databricks AI Deliverable Workflow
{: .fs-9 }

A reusable, **company-agnostic prompt library** for building a secure, in-house
Databricks system that helps staff produce policy memos, reports, and studies
faster and at higher quality — built step by step with **Genie Code**,
Databricks' native coding agent.
{: .fs-6 .fw-300 }

[Start with the prerequisites](#the-build-order){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[How to use these prompts](#how-to-use-these-prompts){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## What this is

This site is a set of **copy-paste prompts**. Each step in the build has a prompt
written with **no company specifics** in it. You add your specifics — catalog
names, data sources, document types, model endpoint — at the moment you paste it
into Genie Code in your own Databricks workspace.

The goal is a system where a user describes a deliverable, the system **plans and
gets human approval**, then researches, analyzes, charts, drafts, and cites —
returning everything to a person for review. A human is in the loop at the
**start**, the **plan approval**, and the **end**.

## How to use these prompts

1. Open your Databricks workspace (AWS, Enterprise tier) and start a **Genie Code** session.
2. Work the steps **in order** — each builds on the last. Don't skip the prerequisites.
3. Copy the step's prompt, **fill in the bracketed placeholders** (e.g. `[CATALOG_NAME]`), and paste it into Genie Code.
4. Review what Genie Code proposes **before** it runs anything. Iterate in the editing chat until correct.
5. Move to the next step once the current one is verified.

{: .note }
> These prompts tell Genie Code to **verify current Databricks syntax** before
> generating code. The platform moves fast; treat anything version-specific as
> something to confirm in your workspace, not as gospel.

## What each step prompt contains

Every prompt is written to give Genie Code everything it needs to execute
flawlessly — **except the names of things**, which you supply. Each one includes:

- a **placeholder list** (`[UPPERCASE_BRACKETS]`) you fill in;
- **reusable context** and the non-negotiable principles;
- **ground truth** — current Databricks facts the agent must use instead of stale patterns;
- the **task**: goal, what to build, specs, setup, and acceptance checks to run;
- a **reference implementation** — complete example code, placeholders aside;
- **guardrails**; and
- a closing instruction telling Genie Code to **ask you for every specific it needs and wait for your approval before running anything**.

## Design principles baked into every prompt

{: .principle }
> - **Everything stays in the Databricks perimeter** — nothing sensitive leaves.
> - **A human approves the plan and reviews every output** — the system never finalizes alone.
> - **Figures come from executed code/queries** (Genie, sandbox) — never LLM free-text.
> - **Citations come only from actually-retrieved documents.**
> - **Access follows existing Unity Catalog permissions** / need-to-know.
> - **The model is interchangeable** — swap the serving endpoint, no redesign.

## The build order

Build a single **vertical slice** first — one document type (a policy-brief memo),
end to end — rather than the whole platform at once.

| # | Step | What it produces |
|---|------|------------------|
| 0 | [Prerequisites & gates]({% link steps/step-00-gates.md %}) | Model clearance + tier/access confirmed |
| 1 | [Unity Catalog foundation & perimeter]({% link steps/step-01-foundation.md %}) | Governed catalog, schemas, volumes, network/egress policy |
| 2 | [Lakeflow ingestion pipeline]({% link steps/step-02-ingestion.md %}) | One API source → bronze/silver/gold + a Genie space |
| 3 | [Document layer & Vector Search]({% link steps/step-03-documents.md %}) | Volumes, project registry, RAG index with citations |
| 4 | [Agent v0]({% link steps/step-04-agent.md %}) | Mosaic AI agent: plan → approve → retrieve → draft |
| 5 | [Analysis & charts]({% link steps/step-05-analysis-charts.md %}) | Sandboxed code + style-compliant visuals |
| 6 | [Databricks App]({% link steps/step-06-app.md %}) | The single UI: input → approval → editing → results |
| 7 | [Evaluation & iteration]({% link steps/step-07-evaluation.md %}) | Agent Evaluation + human review capture |

---

{: .note }
> This is a living resource. Each step's prompt is refined as the build progresses
> and as Databricks features change.
