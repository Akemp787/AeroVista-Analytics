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
- Surfaces discrepancies and conflicts for human review — not silent assumptions
- Packages validated results into role-specific deliverables (Excel, PDF, summary reports)
- Saves successful workflows as recurring organizational procedures

**Raw data never leaves your machine.** No cloud upload, no SaaS, no data sharing.

---

## Core Capabilities

### Analysis Case Framework
Every analytical request creates a tracked **Analysis Case** — a first-class domain object that follows a full lifecycle from question to validated delivery.

```
Draft → Running → Needs Review → Validated → Delivered → Archived
```

Cases are stored locally in SQLite. Status transitions are enforced. A case cannot be marked Validated with open review items. Nothing falls through the cracks.

### Evidence-Backed Findings
Every material conclusion is a **Finding** linked to **Evidence Items** — specific source records, row counts, and computed values that support the claim. Not a narrative summary. Actual evidence.

### Exception Management
Discrepancies, conflicts, status mismatches, and ambiguous records are raised as **Exception Records** and routed to a **Review Center** where a human decides. Decisions are recorded, attributed, and reproducible.

### Dataset Comparison
Compare any two datasets regardless of schema:
- Records only in A or only in B
- Records matched by key with changed field values
- Duplicate key detection
- Summary/footer row detection
- Semantic column classification (IDs, emails, dates, amounts, statuses)

### SQL Safety Layer
All SQL runs through an AST-based validator before execution. Destructive operations (`DROP`, `DELETE`, `UPDATE`, `ALTER`, `TRUNCATE`, `INSERT`) are blocked at the parse level — not by pattern matching. DuckDB runs in read-only mode with enforced row limits.

### Source Registry
Every dataset has a stable ID derived from its content hash — not its filename. Renaming a file doesn't break anything. Uploading the same file twice doesn't create duplicates.

Supported formats: Excel (multi-sheet), CSV, TSV, JSON, JSONL, Parquet, SQLite, DuckDB.

### Workflow Studio
Analytical workflows are defined in YAML or JSON and stored with versioning:
- 28+ registered step types (source, transform, SQL, validation, review, visualization, deliverable)
- Cycle detection and dependency ordering
- Draft and published versioning
- Full run history

### Recurring Procedures
Any validated Analysis Case can be saved as a **Recurring Procedure** — a published workflow that can be re-run with new inputs, versioned, and compared run-over-run. Successful work becomes organizational memory.

### Visualization Engine
Chart generation with Plotly, Matplotlib, and Excel backends. 14 chart types. Charts are validated against the result data before rendering. Misleading configurations are rejected automatically.

### Data Trust Engine
Entity matching and golden record resolution across multiple source systems:
- Fuzzy name matching with configurable confidence thresholds
- Duplicate detection and merge proposals
- Field-level conflict tracking
- Audit trail for every resolution decision

### Run Manifests
Every run saves a complete reproducibility record: source content hashes, SQL executed, transformations applied, validation results, timings, deliverable paths, warnings, and errors. Any past run can be reproduced exactly.

---

## Who It's For

AeroVista is built for organizations that do real analytical work — membership reconciliations, financial audits, CRM cleanups, data migrations, status investigations — but don't have the budget or need for a full data team.

Typical use cases:
- Membership reconciliation (payments vs. active status vs. CRM)
- Pre-migration data audits (finding duplicates, missing fields, invalid formats)
- Period-end financial summaries and exception reports
- CRM data quality audits across multiple source systems
- Compliance evidence packages with complete source lineage

---

## Technology

| Layer | What's Used |
|---|---|
| Execution engine | Python, DuckDB, Polars |
| SQL safety | SQLGlot (AST-level parsing) |
| Persistence | SQLite (cases, workflows, procedures, manifests) |
| Visualization | Plotly, Matplotlib, openpyxl |
| Local app | Single-file Python HTTP server, no framework dependencies |
| AI narration | Ollama (local) — optional, gracefully disabled if not present |

---

## Status

AeroVista is in active development as a production internal tool.

| Phase | Description | Status |
|---|---|---|
| Phase 1 | Foundation — source registry, SQL safety, run manifests | Complete |
| Phase 2 | Wiring — dataset IDs, chart engine, workflow studio | Complete |
| Phase 3 | Domain model — Analysis Cases, evidence, review center, procedures | Complete |
| Phase 4 | Planned — business question planner, expanded workflow library | Upcoming |

**225+ tests passing across the full analytical stack.**

---

## About AeroVista Analytics

AeroVista Analytics is an independent analytics consultancy specializing in data reconciliation, migration support, and operational reporting for associations and small-to-mid-size organizations.

This engine is the internal tooling that powers our client delivery work.

---

*Source code is maintained in a private repository. This page describes the system's capabilities and current development status.*
