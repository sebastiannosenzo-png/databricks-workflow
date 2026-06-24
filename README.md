# Databricks AI Deliverable Workflow — Prompt Library

A reusable, **company-agnostic** prompt library for building a secure, in-house
Databricks AI system that helps staff produce policy memos, reports, and studies
faster and at higher quality — built step by step with **Genie Code**, Databricks'
native coding agent.

The system these prompts build: a user describes a deliverable; the system **plans
and gets human approval**, then researches, analyzes, charts, drafts, and cites —
returning everything to a person for review. A human is in the loop at the start,
the plan approval, and the end.

## 📖 Read it as a website

This repo is published with GitHub Pages:
**https://sebastiannosenzo-png.github.io/databricks-workflow/**

## How it works

Every build step has a **copy-paste prompt** containing no company specifics. You
add yours — catalog names, data sources, document types, model endpoint — when you
paste it into Genie Code in your own Databricks workspace (AWS, Enterprise tier).
Each prompt also instructs Genie Code to ask you for every specific it needs and to
wait for your approval before running anything.

### Build order

| # | Step |
|---|------|
| 0 | Prerequisites & gates |
| 1 | Unity Catalog foundation & perimeter |
| 2 | Lakeflow ingestion pipeline |
| 3 | Document layer & Vector Search |
| 4 | Agent v0 |
| 5 | Analysis & charts |
| 6 | Databricks App |
| 7 | Evaluation & iteration |

## Design principles baked into every prompt

1. Everything stays in the Databricks perimeter; nothing sensitive leaves.
2. A human approves the plan and reviews every output; the system never finalizes alone.
3. Figures come from executed code/queries — never model free-text.
4. Citations come only from actually-retrieved documents.
5. Access follows existing Unity Catalog permissions / need-to-know.
6. The model is interchangeable — swap the serving endpoint, no redesign.

## License

These prompts are shared as a public resource. No real organization data,
credentials, or identifiers appear anywhere in this repository.
