# DreamGuard / SentinelMask — Architecture & System Design

Interactive, code-backed architecture, sequence, data-flow, state machine, database schema, and ML pipeline specifications generated with [Archify](https://github.com/tt-a1i/archify).

All diagrams in this repository are derived directly from the source code of **DreamGuard / SentinelMask** (Commit: [`b3b4159`](https://github.com/Durga62823/talentAP/commit/b3b41597eb486dcd0f39abf02121b62c818bd1c6)), verified across all quality and layout gates (`validate`, `deliver`, `check`, `browser-check`).

---

## 🏛️ Diagram Suite Overview

| # | Diagram | Type | Focus & Code Evidence | Artifact Link |
|---|---|---|---|---|
| 1 | **Architecture** | System Topology | Deployed infrastructure across Browser, Vercel edge proxy (`frontend/vercel.json`), and Render FastAPI backend (`backend/render.yaml`, `backend/Dockerfile`). Details auth/RBAC (`auth.py`), SQL Guard AST (`sql_guard.py`), Policy Resolver (`policy.py`), Masking Engine (`masking.py`), Result Cache (`cache.py`), Hash-Chained Audit Log (`audit.py`), PII Discovery, and Read-Only SQLAlchemy Connection Pool for SQLite, MySQL, and PostgreSQL. | [`architecture/index.html`](architecture/index.html) |
| 2 | **Sequence** | Request Trace | End-to-end trace of a masked query execution by a Support analyst executing `SELECT email, salary FROM employees` via `/api/query/execute` (`routes/query.py`). Traces session verification, AST rewriting, per-column policy resolution, read-only cache hit/miss execution, role-based masking, hash-chained SHA-256 audit write, and HTTP 200 response. | [`sequence/index.html`](sequence/index.html) |
| 3 | **Dataflow** | Pipeline & ETL | The PII Discovery scan pipeline: streaming in-memory sample rows from target DBs, deterministic regex rule scanner (`rule_scanner.py`), sanitized statistical feature extraction (`ai_features.py`) ensuring raw data is discarded immediately, local ML classifier (`ml/classifier.py`), optional Gemini Pro integration (`gemini.py`), and the upgrade-only decision engine (`discovery.py`). | [`dataflow/index.html`](dataflow/index.html) |
| 4 | **Lifecycle** | State Machine | State machine governing column classifications: initial `RULE_SCANNED` state, automatic transitions (`AGREES`, `APPLIED` upgrade, `KEPT_RULE` downgrade block, `LOW_CONFIDENCE`, and `AI_UNAVAILABLE` fallback), and manual analyst review states (`HUMAN_REVIEWED` → `APPROVED`, `REJECTED`, or `OVERRIDDEN`) with immutable audit trails. | [`lifecycle/index.html`](lifecycle/index.html) |
| 5 | **ERD** | Database Schema | Complete schema of the internal metadata database (`metadb.py`) comprising 12 relational tables (`users`, `roles`, `sessions`, `connections`, `classifications`, `ai_classifications`, `ml_classifications`, `policies`, `policy_versions`, `grants`, `audit_events`, `scan_runs`, `uploads`) with exact SQL types, primary keys, and foreign key relations. | [`erd/index.html`](erd/index.html) |
| 6 | **ML Workflow** | ML Pipeline | The end-to-end ML training and benchmark pipeline in `app/ml/train.py`: synthetic corpus generation (`synth.py`), feature extraction, name-grouped split (`GroupShuffleSplit` on base names to prevent data leakage), Level 1 TF-IDF char n-gram model, Level 2 HistGradientBoosting model, Isotonic calibration (ECE minimization), dual evaluation against unseen names & real Saffron Bank ground-truth, and model artifact/card serialization. | [`workflow/index.html`](workflow/index.html) |

---

## 🚀 Interactive Exploration

Each diagram is a fully standalone HTML application supporting:
- Light & dark themes
- Pan & zoom canvas navigation
- Interactive node inspection with source code links
- High-resolution SVG / PNG exports

To browse the complete suite locally with tabbed navigation:
Open `index.html` in any modern web browser.
