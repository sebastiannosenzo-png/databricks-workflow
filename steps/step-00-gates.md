---
title: 0 · Prerequisites & gates
nav_order: 2
---

# Step 0 · Prerequisites & gates
{: .no_toc }

Two things gate everything else. Until both are settled, **do not build**.

1. **Model clearance** — confirm with compliance/legal which serving model(s)
   are cleared for the data you'll process. (For federal or regulated work, the
   model-procurement question is real; settle it in writing first.)
2. **Tier & access** — confirm an **Enterprise-tier** Databricks workspace on AWS
   and **account-admin** access, both needed for governance, serverless egress
   controls, and network policies.

The two prompts on this page do **not** build anything. They (a) establish a
reusable context the rest of the steps rely on, and (b) ask Genie Code to produce
a **read-only readiness report** so you know where the gaps are before writing code.

1. TOC
{:toc}

---

## Placeholder convention

Every prompt uses `[UPPERCASE_BRACKETS]` for things you fill in. Replace them all
before pasting. Common ones:

| Placeholder | Meaning |
|-------------|---------|
| `[COMPANY]` | The organization you're building for |
| `[CATALOG_NAME]` | Target Unity Catalog catalog |
| `[ENV]` | Environment label, e.g. `dev` / `prod` |
| `[CLOUD_REGION]` | AWS region, e.g. `us-east-1` |
| `[MODEL_ENDPOINT]` | The serving endpoint name for the cleared model |
| `[DATA_SOURCE]` | The first API source, e.g. an open data API |
| `[DOC_TYPE]` | The first deliverable type, e.g. policy-brief memo |

---

## Prompt 0A · Session context primer
{: .no_toc }

Paste this **once at the start of every Genie Code session**. It gives the agent
the architecture, the non-negotiable principles, and the working style, so each
later step's prompt can stay short. Nothing here is company-specific.

{: .note }
> Fill in `[COMPANY]`, `[CLOUD_REGION]`, and `[ENV]`. Leave the rest as written.

```text
You are helping build a secure, in-house Databricks system for [COMPANY] on AWS
(region [CLOUD_REGION], environment [ENV]). The system helps staff produce
deliverables (memos, reports, studies) faster and at higher quality. A user
describes a deliverable; the system plans, gets human approval, then researches,
analyzes, charts, drafts, and cites — returning everything to a person for review.

ARCHITECTURE (target end state):
- UI: a single Databricks App (input, plan approval, editing chat, results).
- Orchestrator: Mosaic AI Agent Framework (model-agnostic; reasoning runs on a
  configurable serving endpoint).
- Models: Databricks Model Serving / Foundation Model APIs behind AI Gateway
  (one governed entry point, with audit + lineage; prompt caching for static
  .md rule/style files).
- Tools the agent calls: Vector Search (RAG with citation metadata); Genie
  Conversation API (natural language to SQL over governed gold tables — this is
  how the agent gets correct numbers); a sandboxed code path for statistics/ML
  with MLflow logging; a chart engine (LLM-generated matplotlib/plotly executed
  to render visuals); and an external Web Research service via an external-model
  endpoint (so sensitive data never needs raw open-web egress).
- Data foundation: Unity Catalog over everything; Lakeflow declarative pipelines
  ingest from governed APIs into a medallion (bronze/silver/gold) layout and keep
  Vector Search indexes fresh; UC Volumes hold documents with a Delta registry of
  which project each doc is used in; MLflow + Model Registry; style rules and edit
  logs kept as .md files in Git folders.

NON-NEGOTIABLE PRINCIPLES — apply these to everything you propose:
1. Everything stays inside the Databricks perimeter; nothing sensitive leaves.
2. A human approves the plan and reviews every output; never finalize alone.
3. Figures come from executed code/queries (Genie, sandbox), never from model
   free-text.
4. Citations come only from documents actually retrieved.
5. Access follows existing Unity Catalog permissions and need-to-know.
6. The model must stay interchangeable — swapping the serving endpoint must not
   require a redesign. Never hard-code a vendor's API; go through the configured
   endpoint.

WORKING STYLE:
- Before asserting any version-specific Databricks detail (API names, syntax,
  feature availability), verify it against the current workspace or current docs.
  If you cannot verify, say so and propose how to check — do not guess.
- Be clear, concise, and professional. Flag risks honestly (hallucination,
  numerical accuracy, model clearance, cost). Do not overstate readiness.
- Propose a plan and wait for my approval before running anything that creates,
  modifies, or deletes resources.

Acknowledge that you understand this context and will hold to it for the rest of
the session. Then tell me which session-level specifics you'll need from me
([COMPANY], [CLOUD_REGION], [ENV]) and confirm them with me. Do not take any
action yet.
```

---

## Prompt 0B · Read-only readiness report
{: .no_toc }

Paste this **after** the primer. It asks Genie Code to inspect the workspace and
tell you whether the prerequisites are met — **without changing anything**.

```text
Before we build, produce a READ-ONLY readiness report for this workspace. Do not
create, modify, or delete any resource. Where a check needs account-admin rights
you don't have, say so and tell me exactly what to ask an account admin for.

Verify the current way to check each item (commands/APIs change), then report:

1. Tier & account
   - Is this an Enterprise-tier workspace? How did you determine it?
   - Is Unity Catalog enabled and the workspace assigned to a metastore?
   - Do I (the current principal) have account-admin and metastore-admin rights?

2. Compute & serverless
   - Is serverless compute available and enabled in region [CLOUD_REGION]?
   - What compute would each later step use (pipelines, agent, app)?

3. Networking & egress (the perimeter)
   - Is there a network policy / egress control in place for serverless?
   - What is required to add an egress allowlist for: (a) the external model
     provider(s), (b) the external Web Research service, (c) the data-source
     API(s)? List the destinations we will eventually need to allow.

4. Models
   - List the serving endpoints and Foundation Model APIs available in this
     workspace. Which could serve as the interchangeable reasoning endpoint?
   - Is AI Gateway available to put one governed entry point in front of them?
   - Flag: confirm with compliance which of these are CLEARED for our data — I
     will handle that off-platform; just tell me what's technically available.

5. Feature availability
   - Confirm availability of: Vector Search, Genie (Conversation API),
     Lakeflow declarative pipelines, MLflow + Model Registry, Databricks Apps,
     Agent Evaluation. Note anything not enabled and how to enable it.

Output a single table: Prerequisite | Status (met / gap / needs-admin / unknown)
| Evidence | What to do next. End with the top 3 blockers, if any.

Before you run any check, ask me for any specifics you need (at minimum confirm
[CLOUD_REGION]) and wait for my answer. This report must be strictly read-only;
do not create, modify, or delete anything.
```

{: .warning }
> The readiness report tells you what's **technically** present. It does **not**
> clear a model for your data — that's the compliance/legal decision in gate (1),
> which must be settled in writing before Step 4 (the agent) goes near real data.

---

## Exit criteria for Step 0

- [ ] Compliance/legal has named the **cleared model(s)** in writing.
- [ ] Workspace confirmed **Enterprise tier**, Unity Catalog enabled.
- [ ] You have (or have a named admin for) **account-admin / metastore-admin**.
- [ ] Readiness report shows **no blocking gaps** for Steps 1–2, or each gap has an owner.

Once these are green, continue to
[Step 1 · Unity Catalog foundation & perimeter]({% link steps/step-01-foundation.md %}).
