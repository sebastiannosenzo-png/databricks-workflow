---
title: What is the project
nav_order: 2
---

# What is the project
{: .no_toc }

Before the build steps, here is the system these prompts create — so you can decide
whether it fits your organization.

1. TOC
{:toc}

---

## Goal

Establish an intelligently automated, secure system that helps **Company X** staff
prepare policy memos, reports, and studies faster and to a higher standard. The
system drafts text, analyzes data, and produces charts — cutting the time spent on
routine data pulls and chart-building — while users retain full control and review
every output before it is used.

## Scope

Much of an analytical office's work is reiterative. Memos and projects closely
resemble earlier ones, drawing on many of the same datasets and similar forms of
analysis, yet each is rebuilt largely from scratch. Staff spend considerable time
on routine groundwork — locating data, cleaning it, and generating charts — before
the substantive work can begin.

Databricks is the secure platform the office already uses to store data and operate
software. Because the information is sensitive, **all work takes place within the
platform**: no data is transmitted to public websites or external services, and
external research is conducted through the organization's own secured service. The
project introduces a new system inside that platform. A user specifies the
deliverable required — for example, a policy brief on a given topic. The system
reviews internal datasets, style guides, and workspace rules before proposing a
structured plan, and proceeds only after the user approves it. It then assembles
the relevant information and data, performs the analysis, generates charts, drafts
the document, and compiles its sources. Users review and revise until the draft is
complete.

With user involvement **at the outset, at approval, and at completion**, every
output is reviewed by a person before use.

## The workflow

Every project follows five steps. The user defines the deliverable and approves the
plan; the system then carries out the research, analysis, charts, and drafting, and
returns the result for review.

| Step | Owner | What happens |
|------|-------|--------------|
| 1 · Describe need | **User** | Type, goal, topic |
| 2 · Plan proposed | System | Research & build steps drafted |
| 3 · Approve plan | **User** | Human gate |
| 4 · Research & build | System | Analysis, visuals, citations |
| 5 · User edits | **User** | Revise to final product |

By automating the routine intermediary steps, the system shortens the path from
request to finished draft and produces more consistent, higher-quality outputs. The
greatest time savings come from data pulling, cleaning, and presentation; the
clearest quality gains appear in analysis and citation, where automated calculation
and references drawn directly from source documents reduce the errors most common in
manual work.

## Design decisions and safeguards

1. **The documents, the data, and the system itself reside in a single protected
   environment** — no sensitive material exits the system.
2. **Final judgment remains with users.** Automated systems can produce errors that
   appear authoritative, so a person confirms every output is accurate and suitable
   for policy use.
3. **Figures are derived from calculations, not generated as text.** Data is drawn
   directly from approved sources and all analysis is computed and recorded, so every
   figure is reliable and traceable to its source.
4. **Citations are drawn only from the documents used** — preventing fabricated or
   misattributed references and ensuring every citation can be verified.
5. **Access adheres to the office's existing permissions.** Users may use only the
   documents and prior projects they are already authorized to access.
6. **The underlying AI model is interchangeable** — letting the system meet security
   and procurement requirements without redevelopment.

## System architecture

Users work in the **Databricks App**. Requests pass to the **Mosaic AI agent**, the
platform's native coordinator that plans the work and directs the other tools. Its
reasoning runs on interchangeable language models — Claude, GPT OSS, or Gemini —
served and governed through Databricks.

To carry out a request, the agent uses specialized tools: **Vector Search**
(retrieves passages from the office's documents and past projects), **Genie**
(converts plain-language questions into database queries), **Analysis & ML** (runs
statistics and machine-learning methods, every run recorded in MLflow), and the
**Chart Engine** (produces visuals). **Web Research**, the one external tool,
conducts outside research through the organization's secured service.

These tools read from a governed data foundation built on Databricks-native
services: **Unity Catalog** controls access and logs all activity, and scheduled
**Lakeflow** pipelines keep datasets current. The foundation holds the datasets, the
document store and project registry, the search index, analysis records, and style
rules. The source datasets themselves are external. Finished results flow back up
the same path to the App for user review, and **every component other than the user
and the external data sources remains within the Databricks security perimeter**.

<p style="text-align:center;margin:1.5rem 0;">
  <img src="{{ '/assets/architecture.svg' | relative_url }}"
       alt="End-to-end system architecture on Databricks: User → Databricks App → Mosaic AI Agent (Claude / GPT OSS / Gemini) → tools (Vector Search, Genie, Analysis & ML, Chart Engine, Web Research AI) → Unity Catalog governed data foundation, all inside the Databricks security perimeter."
       style="max-width:100%;height:auto;border:1px solid #e3e6cf;border-radius:8px;padding:8px;background:#fff;" />
</p>

*Figure. End-to-end system architecture on Databricks. Lighter boxes denote
user-controlled steps; darker boxes denote system steps. The dashed boundary is the
Databricks security perimeter; only the external web and source datasets sit outside
it.*
{: .fs-2 }

---

Ready to build it? Start with [Step 0 · Prerequisites & gates]({% link steps/step-00-gates.md %}).
