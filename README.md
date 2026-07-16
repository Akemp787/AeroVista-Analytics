# AeroVista Analytics Engine

**A local, private analytical operations system for small-to-mid-size organizations.**

AeroVista turns a business question into a validated, evidence-backed deliverable — without spreadsheet gymnastics, without your data leaving your machine, and without a data engineer on staff.

```
Ask → Ground → Execute → Validate → Review → Deliver → Reuse
```

---

## What it does

You ask, in plain language, what you need to determine, reconcile, investigate, or report. AeroVista does the work and shows you how it got there.

- Understands the question and states its interpretation before it runs
- Resolves every dataset, column, metric, and join against your real data — it will not invent one
- Builds and executes a structured analytical plan
- Backs every sentence of the answer with evidence linked to specific records
- Surfaces discrepancies, exceptions, and anything it could not determine
- Routes conclusions through human review before anything is published
- Packages validated results into finished deliverables
- Remembers what you decided, so the next investigation starts smarter

**Your data stays on your machine.** No cloud upload, no SaaS, no data sharing.

---

## Ask a question, get an investigation

The app opens on a single question box. Ask something like *"identify membership records with payments but lapsed status"* and Aero investigates.

**The answer leads with the answer.** A headline with the number that matters, a short list of key findings, a validation and confidence card, the sources it used, and the records it flagged. Not a wall of technical output.

**Open the investigation** for the full picture, organized into four views:

| View | What you get |
|---|---|
| **Summary** | The headline, the key findings, and the distribution behind them |
| **Findings** | Every statement, grouped by what it means, with the evidence behind each |
| **Data Records** | The actual row-level records — searchable, sortable, inspectable |
| **Reasoning** | How Aero interpreted the question, what it tested, and exactly what it executed |

**Ask follow-ups without starting over.** The question box stays with the investigation, so "why?", "exclude lifetime members", or "use June 30 as the cutoff" revises the same investigation rather than launching a new one.

---

## Written for the person reading it

Findings are grouped the way a person thinks about them — **what Aero found**, **what the evidence disagrees with**, and **what it couldn't determine** — in plain language, not engine vocabulary. Things it could not determine are counted and stated with the reason, rather than repeated line by line.

Choose the reading level and the same findings are re-written for that reader:

- **Executive** — the conclusion and what to do about it, in as few words as possible
- **Analyst** — the findings with the evidence behind each one
- **Technical** — full detail: calculations, provenance, identifiers

The evidence never changes between reading levels — only the words. And every statement, at every level, keeps a **"Why we know this"** link to the records and rules behind it.

---

## See the raw data behind any finding

Select any flagged record and open **Finding Inspection**:

- The record's fields exactly as they came from the source — nothing rewritten
- Which source each individual field came from
- The evidence Aero reviewed, grouped by source
- The engine's own recorded reason for flagging that record
- The rules checked and the run it came from

Nothing is a summary of a summary. You can always get to the row.

---

## Evidence, not assertions

Every finding is derived from executed results. Numbers are computed by code — never written by a language model.

- **It cannot invent.** Datasets, columns, metrics, and joins are resolved against your actual data. If a reference is ambiguous, you get a specific question, not a silent guess. If it can't be grounded, the run stops and tells you why.
- **It cannot overstate.** Causal language is bounded by the evidence; confidence is capped by the weakest fact behind it.
- **It cannot quietly fail.** A branch that couldn't run is disclosed as a skip. "Not determined" is a real, visible answer.
- **It shows its work.** Every value traces back through lineage to the source field and the rule that produced it.

---

## Human review before anything is published

No conclusion reaches a client because software decided it was ready.

- Every claim gets an independent, attributed human decision — approve, suppress with a reason, dispute with a reason, or request more evidence
- Recommendations require explicit approval
- Decisions are recorded against the evidence they were made on; if that evidence changes, the review goes stale and cannot approve or publish until a human confirms
- Published outputs are immutable, versioned snapshots — suppressed claims appear in no output
- Deliverables all render from that one approved snapshot, so the report, the workpaper, and the evidence file cannot disagree

**No model can approve, publish, or bypass validation.** That is a human decision, by design.

---

## Bring your own model — or none at all

AeroVista is model-agnostic and works without any AI configured.

- Choose **Auto**, a local model via **Ollama**, or **OpenAI**, **Anthropic**, **Google Gemini**, or any OpenAI-compatible endpoint
- **Auto** prefers a healthy local model, falls back to a permitted external one, and finally to deterministic logic — and tells you which it used and why
- **Your keys, your account.** AeroVista never pays for or proxies your model usage
- Keys are stored in your OS keyring, shown only masked, and never written to logs, exports, or the database
- **Privacy modes** decide whether any context may leave the machine at all — enforced before a provider is ever called

The model interprets the question and words the answer. It does not count records, compare rows, calculate totals, or decide what is true.

---

## Connect your data

Work from files or straight from live sources.

- **Files** — CSV, TSV, JSON, JSONL, Excel, Parquet
- **Live sources** — PostgreSQL, Amazon S3, BigQuery, SQLite, DuckDB

Every connection is scoped to approved tables and paths, read-only, credential-isolated, and audited. Queries are validated before execution; anything out of scope is rejected before a single row is read.

---

## Deliverables

From an approved investigation:

- Executive report (**DOCX**, **PDF**)
- Analyst workpaper with the full evidence chain
- Excel workbooks and HTML reports
- A machine-readable evidence package
- Refreshable export packages for **Power BI** and **Tableau**

All of them derive from the same approved snapshot.

---

## Built for practitioners with more than one client

Each client is a completely isolated workspace. Sources, investigations, rules, mappings, decisions, and deliverables are separated at the backend — no data from one client can surface in another.

Successful analyses can be saved as **recurring procedures** and re-run against new data, with a clear report of what changed since last time.

---

## Use cases

- **Membership reconciliation** — members who paid but are still marked lapsed, duplicates, expiring records
- **Payment verification** — cross-reference payments against membership status
- **Data audit** — nulls, type inconsistencies, duplicates, quality issues
- **Cross-source comparison** — compare two exports and produce a structured change report
- **Recurring reporting** — build it once, run it monthly with new data
- **Live database analysis** — investigate a production database or bucket without exporting first

---

## What it runs on

- **Python 3.11+**, runs locally as a single process and opens in your browser
- SQLite for local persistence
- Optional: **Ollama** for a fully on-machine language model
- Optional: connector libraries for live remote sources

---

## What AeroVista is not

- Not a dashboard platform
- Not a drag-and-drop ETL tool
- Not a data-science notebook
- Not a cloud service or SaaS
- Not a general-purpose AI chatbot

AeroVista completes analytical work. You ask a business question. It does the work, shows the evidence, tells you what it could not determine, routes uncertainty to you, produces finished outputs, and preserves the process as organizational memory.
