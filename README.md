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
All SQL passes through a SQLGlot AST-based safety validator before execution. The validator keyword-filters input, enforces a single SELECT statement, rejects recursive CTEs, and walks the full parse tree rejecting every non-SELECT node type. Destructive operations (`DROP`, `DELETE`, `UPDATE`, `ALTER`, `EXEC`, and others) are blocked at both the keyword and AST level. The validator fails closed if SQLGlot is not installed — it never defaults to allowing unvalidated SQL. DuckDB runs in read-only mode.

### Secure Remote Data Connections

AeroVista now connects directly to live remote data sources for clients who need analysis against production databases and cloud storage, not file exports.

**Supported connectors:** PostgreSQL, Amazon S3, BigQuery, SQLite, DuckDB. Each connector publishes a formal **capability contract** — SQL dialect, authentication methods, supported pushdowns, schema/table discovery, streaming, pagination, sampling, row-count estimation, and audit events — so the engine knows what a source can do before it connects. Additional connectors (SQL Server, MySQL, Snowflake, Redshift, Databricks, Google Cloud Storage, Azure Blob Storage) are declared in the connector catalog and fail closed until implemented — never a silent fallback.

Every connection request passes through five independent enforcement layers:

- **Credential isolation** — stored in OS keyring; never logged, never in workflow YAML, never returned to the UI after initial entry. Production mode fails closed if keyring is unavailable.
- **Scope enforcement** — table and path access is validated before any SQL is built or I/O begins. Out-of-scope requests are rejected immediately with a typed error.
- **Path authorization** — S3 object keys are URL-decoded, checked for traversal components (including `%2e%2e` encoded variants), and verified against approved prefixes at the component level — `approved/data_other` cannot satisfy an `approved/data` prefix.
- **SQL AST allowlist** — LLM-generated and user-provided SQL must pass the AST validator before execution against any remote source.
- **Client isolation** — connection registry enforces `client_id` on every read, write, and list. Cross-client access returns nothing or raises a permission error.

### Query Pushdown

AeroVista plans queries before executing them. The query planner determines which operations (column projection, predicate filtering, LIMIT) can be pushed to the remote source and which must stay local. Parquet files on S3 support column projection and predicate pushdown via PyArrow. BigQuery supports dry-run cost estimation before execution.

### Audit Trail

Every connector call — queries, connection tests, catalog inspections — writes an immutable audit entry with event type, timestamps, rows returned, duration, status, and a secrets-scrubbed error message (when applicable). The audit log is client-scoped and queryable by event type, status, connection, and date. A built-in Streamlit component renders the full audit history for any client workspace.

### Compiled Execution Runtime

AeroVista no longer behaves like a loose collection of Python workflow functions. Underneath the familiar Analysis Case experience there is now a **local-first workflow compiler and execution runtime**: you describe what you need as a typed workflow, and AeroVista compiles it into an optimized execution graph, runs it through specialized engines in isolated worker processes, records every intermediate result as an immutable artifact, logs an append-only event history, and validates the output independently.

- **Typed workflow plans** — workflows are typed, versioned, serializable data (no embedded code), so the same analysis is reproducible and reviewable.
- **A real compiler** — six stages: validate → resolve client context (datasets, rule and mapping versions, scope) → logical plan → optimize → select engines → physical graph. Optimizations include predicate and column pushdown, redundant-scan elimination, and a guard against accidental many-to-many blow-ups.
- **Specialized backends** — heavy work runs on DuckDB, Polars, and Apache Arrow rather than row-by-row Python.
- **Observable execution** — every run is a directed graph of steps with durable state. Runs can be cancelled, resumed after an interruption, and partially re-run; completed steps are cached by a content fingerprint and never reused across clients.
- **Isolated workers** — a workflow failure can never crash the app; a worker crash or timeout is reported as a failed step.
- **Immutable artifacts & audit events** — results are written once and never mutated in place; an append-only event log records the full run history with secrets redacted.
- **Independent validation** — execution completing and the result being *validated* are separate states. A case is not "validated" while required checks fail.

**Dataset Comparison** is the first workflow fully migrated to this runtime, and it is verified by a **dual-run** check: the same comparison is executed through both the legacy engine and the new compiled runtime, and the reconciled counts and record classifications must match exactly before the migration is trusted.

Your existing cases, procedures, rules, and mappings are preserved — the new engine produces the same trusted Analysis Case experience through a stronger foundation.

### BI-Compatible Outputs

AeroVista interoperates with Power BI and Tableau instead of trying to replace them. From any analysis it can produce a **refreshable export package**: clean tables in Parquet, CSV, and Excel; a data dictionary (column types, semantic roles, null rates); detected relationships between tables; a star-schema fact/dimension model; and recommended measures and dimensions — plus a manifest and documented connection paths for both Power BI and Tableau. Paths stay stable across refreshes, so a BI data source keeps working as the data updates. All outputs are vendor-neutral; no BI tool is required to produce or read them.

### Native Dashboards

A typed, code-free dashboard layer turns results into KPI cards, tables, and charts driven by **specifications rather than hand-written UI code**. Dashboards support global and per-panel filters, drill-through to the supporting evidence rows, downloadable panel data, and saved, reusable layouts that can be re-run against new data. Because a dashboard is just typed data, the same definition is reproducible and shareable.

### Visualization Engine
Chart generation with Plotly, Matplotlib, and Excel backends. Supports KPI cards, bar, line, area, scatter, histogram, pie/donut, waterfall, and actual-vs-forecast charts.

---

## What It Runs On

- **Python 3.11+**, runs locally as a single HTTP server on `localhost:8513`
- Browser-based UI — no Electron, no installer
- SQLite for all persistence (cases, procedures, clients, connections, audit log)
- Optional: Ollama for local LLM narration (keeps data fully on-machine)
- Optional: boto3, psycopg2-binary, google-cloud-bigquery for live remote connections

---

## Use Cases

- **Membership reconciliation** — find members who paid but are still inactive, lapsed records, duplicate memberships
- **Payment verification** — cross-reference payment files against membership status
- **Data audit** — profile columns, find nulls, type inconsistencies, duplicates
- **Cross-source comparison** — compare two exports and produce a structured change report
- **Recurring reporting** — build a procedure once, run it each month with new files
- **Live database analysis** — connect directly to a PostgreSQL database or S3 bucket and run scoped, validated queries without exporting data first

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
| **Connections** | Manage live remote data connections per client workspace |
| **Audit Log** | Client-scoped history of every connector call with status and timing |
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
| 7 | Secure Data Connections: PostgreSQL, S3, BigQuery connectors; CredentialStore; ConnectionRegistry; RemoteQueryPlanner; SQL AST validator; query fingerprint caching |
| 7.1 | Security hardening: URL-encoded path traversal prevention; component-level prefix comparison; credential fail-closed gate; scope enforcement in all three connectors before SQL build; ResolvedDataset no-secrets contract; 219 new security tests |
| 8 | Audit trail: ConnectorExecutor wraps all I/O with automatic audit writes; list_audit filtering by event type and status; audit_summary aggregates; client-scoped Streamlit audit UI component |
| 9 | Compiled execution runtime: typed Workflow IR, action/algorithm registries, six-stage compiler with pushdown optimization, physical execution DAG, durable job coordinator, isolated multiprocessing workers, immutable artifact store, append-only event store, fingerprint cache, formal validation runtime, and Dataset Comparison migrated with legacy dual-run verification |
| 10 | External analytical systems: connector capability contracts + extensible catalog; SQLite and DuckDB local connectors; BI-compatible exports (data dictionaries, star schema, refreshable Power BI / Tableau packages); native typed-spec dashboards with filters, drill-through, and saved layouts |

**Over 800 tests passing across all lines of work.**

---

## What's Next

The immediate roadmap from here:

- **Modern application split** — a FastAPI backend with typed, OpenAPI-documented endpoints and a React/TypeScript frontend, migrated incrementally (strangler pattern) beside the existing app with verified parity before any cutover.
- **Migrate more workflows to the compiled runtime** — Join & Reconciliation next, then Entity Matching and Duplicate Detection, each with golden fixtures and legacy dual-run verification before sign-off.
- **Run, execution, and technical-plan screens** — show the business objective and estimated plan before a run; live progress, current step, and cancellation during a run; and an expandable technical view of the logical plan, physical graph, selected engines, pushdown decisions, and validation results.
- **Connections UI page** — full Streamlit page for managing live connections per client: create, test, edit, disable, and browse the catalog. Integrates the audit log component.
- **LLM-assisted query generation** — accept a natural-language question, produce parameterized SQL through the AST validator, execute via `ConnectorExecutor`, and return results as an Analysis Case finding.
- **Scheduled procedures** — run saved procedures on a cron or file-arrival trigger; results written to the analysis case history automatically.
- **Schema drift detection** — compare the current remote schema fingerprint against the last-recorded version; surface drift as a system warning before executing a procedure that depends on the changed table.
- **Additional connectors** — MySQL, SQL Server, Snowflake, Google Cloud Storage (blocked in Phase 7.1 scope; next connector phase after hardening is verified stable).
- **Exportable audit reports** — produce an Excel or PDF audit trail report for a given client, time range, or event type — useful for compliance reviews and client deliverables.

---

## What AeroVista Is Not

- Not a dashboard platform
- Not a drag-and-drop ETL tool
- Not a data-science notebook
- Not a cloud service or SaaS
- Not a general-purpose AI chatbot

AeroVista completes analytical work. You ask a business question. It does the work, shows the evidence, routes uncertainty for your review, produces finished outputs, and preserves the process as organizational memory.
