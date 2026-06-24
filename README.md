# AeroVista Analytics Engine

**A local, private Analytical Operations System for small-to-mid-size organizations.**

AeroVista turns business questions into validated analytical deliverables — without spreadsheet gymnastics, data leaving your machine, or a data engineer on staff.

```
Ask → Execute → Validate → Review → Deliver → Reuse
```

---

## What It Does

You describe what you need to determine, reconcile, investigate, or report.  
AeroVista does the work.

- Selects the relevant data from your registered sources
- Builds and executes a structured analytical plan using deterministic Python and SQL
- Backs every finding with evidence linked to specific source records
- Surfaces discrepancies and exceptions for your review — with evidence and recommended decisions
- Packages validated results into finished deliverables (Excel workbooks, HTML reports)
- Saves successful analyses as recurring procedures that can be re-run with new data

**Your data never leaves your machine.** No cloud upload, no SaaS, no data sharing.

---

## Multi-Client Workspace Isolation

AeroVista is built for practitioners who work with multiple clients or projects. Each client has a completely isolated workspace — sources, cases, rules, mappings, and deliverables are separated at the backend. No data from one client can appear in another client's workspace.

---

## Key Capabilities

### What You Get in a Single Run
- A structured **Analysis Case** with status tracking from question to delivery
- **Findings** — material conclusions, each backed by specific evidence records
- **Exception Records** — discrepancies, conflicts, and uncertainties automatically classified by type and severity
- **Review Tasks** — items requiring a human decision, presented with evidence and options
- **Deliverables** — Excel workbook and HTML report, ready to share

### Dataset Comparison
Compare any two datasets regardless of schema. AeroVista automatically detects:
- Records only in one source
- Matched records with changed values
- Duplicate keys
- Footer/total rows
- Semantic column types (IDs, emails, names, dates, amounts, status)

### Recurring Procedures
Any completed analysis can be saved as a procedure. Re-run it later with new files — AeroVista executes deterministically and shows exactly what changed since last time.

### Business Rules Library
Per-client, versioned organizational definitions referenced by name in analyses. Rules are versioned — past runs retain the rule version that was active at execution time.

### Field Mapping Library
Maps your source column names to universal semantic roles (e.g. `MemberID` → `customer_identifier`). Versioned, per-client, with drift detection when a mapped column disappears from the source.

### Workflow Recommendations
AeroVista tracks which workflows you've run for each client and surfaces recommendations when you start a new analysis, scored by recency, frequency, and keyword matching.

### Deterministic Execution
All calculations are performed by Python, DuckDB SQL, Polars, and registered statistical libraries. The language model (if configured) interprets requests, proposes plans, and narrates validated results — it does not count records, compare rows, calculate totals, or generate financial values.

### SQL Safety
All SQL passes through a SQLGlot AST-based safety validator before execution. Destructive operations (`DROP`, `DELETE`, `UPDATE`, `ALTER`, etc.) are blocked. DuckDB runs in read-only mode.

### Visualization Engine
Chart generation with Plotly, Matplotlib, and Excel backends. Supports KPI cards, bar, line, area, scatter, histogram, pie/donut, waterfall, and actual-vs-forecast charts.

---

## What It Runs On

- **Python 3.11+**, runs locally as a single HTTP server on `localhost:8513`
- Browser-based UI — no Electron, no installer
- SQLite for all persistence (cases, procedures, clients, mappings)
- Optional: Ollama for local LLM narration (keeps data fully on-machine)

---

## Use Cases

- **Membership reconciliation** — find members who paid but are still inactive, lapsed records, duplicate memberships
- **Payment verification** — cross-reference payment files against membership status
- **Data audit** — profile columns, find nulls, type inconsistencies, duplicates
- **Cross-source comparison** — compare two exports and produce a structured change report
- **Recurring reporting** — build a procedure once, run it each month with new files

---

## Local Desktop App

```bash
cd 00_Core_Library
pip install -e .
python apps/aerovista_local_desktop.py
# Opens at http://localhost:8513
```

### App Pages

| Page | Purpose |
|---|---|
| **Home** | New analysis prompt, review queue, recent cases, recurring procedures |
| **Analysis Cases** | Full history of completed analyses with status filter |
| **Review Center** | Resolve open exceptions — view evidence, make decisions |
| **Procedures** | Published and draft recurring procedures |
| **Client Workspaces** | Create and switch between isolated client environments |
| **Business Rules** | Per-client versioned rule library |
| **Field Mappings** | Per-client column-to-semantic-role mapping with drift detection |
| **Data Sources** | Registered datasets with stable IDs |
| **Deliverables** | Download generated workbooks and reports |

---

## Phase History

| Phase | What Was Built |
|---|---|
| 1 | Foundation: SourceRegistry, SQLGlot SQL safety, DuckDB execution, RunManifest |
| 2 | ChartEngine, WorkflowStudio, dataset_id wired to frontend |
| 3 | Analytical Operations System: AnalysisCase, Finding, ExceptionRecord, ReviewTask, RecurringProcedure |
| 4 | Dataset Comparison UI, exception classification, side-by-side procedure run comparison |
| 5 | Multi-client workspace isolation, Field Mapping Library, Workflow History and Recommendations |

**459 tests passing.**

---

## What AeroVista Is Not

- Not a dashboard platform
- Not a drag-and-drop ETL tool
- Not a data-science notebook
- Not a cloud service or SaaS
- Not a general-purpose AI chatbot

AeroVista completes analytical work. You ask a business question. It does the work, shows the evidence, routes uncertainty for your review, produces finished outputs, and preserves the process as organizational memory.
