# CAMP - Corporate Analytics & Management Platform

CAMP is an enterprise analytics and management platform designed for governed reporting, operational review, task execution, and model-driven analytical outputs.

The platform is built around a simple institutional need: analytical findings should not remain as isolated SQL results, exported files, or informal follow-up lists. CAMP connects analytical design, report definition, scheduling, ownership assignment, record-level review, team execution, audit evidence, and ML workflow governance within one controlled operating model.

## Product Vision

Control, audit, risk, compliance, and data analytics teams often face the same pattern:

- analytical scenarios are prepared by specialists,
- recurring outputs are distributed through fragmented channels,
- ownership is tracked outside the analytical platform,
- review evidence is collected manually,
- report and model logic is difficult to reproduce historically,
- model outputs are not governed with the same discipline as report outputs.

CAMP treats analytics as an operational lifecycle. A query, report, exception list, model result, or review package can be cataloged, scheduled, assigned, reviewed, evidenced, and traced through completion.

## Core Capabilities

| Area | Capability |
|---|---|
| Data Analytics | SQL-based analysis across managed SQL Server and Oracle sources. |
| Report Management | Governed report catalog with versioning, parameters, references, ownership, and activation state. |
| Assignments and Dispatcher | Periodic execution, responsible user assignment, notification rules, export options, and review routing. |
| Self-Service Reporting | Controlled parameterized report requests for business users. |
| Record Review | Row-level review workbench for assigned analytical results. |
| Transaction Analysis | Search and drill-down across persistent analytical result sets. |
| Dashboard | Operational visibility over reports, assignments, review status, and workload. |
| Team Management | Structured task portfolio, assignment, progress, history, dashboard, and expense tracking. |
| ML Workflow Studio | Canvas-based ML workflow governance with MDS inputs, Python steps, UAT/Prod lifecycle, artifacts, metrics, and MDL outputs. |
| Security and Audit | Role-based access, optional MFA, service-account execution, centralized logging, and operational traceability. |

## Operating Model

CAMP separates analytical design from operational execution.

1. Analytical users prepare SQL logic and report definitions.
2. Reports are cataloged with metadata, parameters, source context, and version history.
3. Assignments determine who owns the output, when it runs, how it is delivered, and whether records flow into review.
4. The dispatcher executes due assignments through a controlled service-account model.
5. Persistent result tables, review queues, activity logs, and audit records preserve the operational trail.
6. ML workflows follow the same governance pattern: model outputs become assignable MDL result packages.

## Data Analytics

The Data Analytics workspace includes SQL Editor, report definitions, assignments, self-service reports, scheduled execution monitoring, transaction analysis, record review, and dashboards.

Reports are versioned governed assets. Recurring FR/SR reports can be assigned and scheduled. SSR reports are requested on demand. MDS reports provide model dataset inputs. MDL reports represent model output packages created by ML Workflow Studio.

## Team Management

Team Management supports structured audit, control, and operational work portfolios. It includes annual planning, in-year assignments, responsible teams, task status, progress tracking, working summaries, extension and suspension flows, history, dashboards, role permissions, and expense tracking.

## ML Workflow Studio

ML Workflow Studio governs model workflows without turning the application into a general-purpose IDE. Model development can happen in external tools, while CAMP manages the enterprise workflow around datasets, run history, artifacts, metrics, publishing, assignments, and traceability.

A typical ML flow is:

1. Prepare raw data logic in Data Analytics.
2. Save the raw dataset as an MDS report.
3. Connect one or more MDS Dataset nodes in the ML canvas.
4. Run Python-based preparation, EDA, feature engineering, training, evaluation, scoring, and publishing steps.
5. Catalog generated artifacts and metrics.
6. Publish one or more MDL output packages.
7. Assign MDL outputs in Data Analytics Assignments.
8. Route selected outputs to Record Review.

UAT workflows are used for experimentation and can be cleaned. Prod workflows preserve model version, scripts, artifacts, metrics, run history, and published MDL outputs as governed operational records.

## Governance Principles

CAMP is designed around the following principles:

- analytical outputs should have accountable ownership,
- recurring controls should be reproducible,
- report and model definitions should be versioned,
- review actions should be traceable at row level,
- execution should run under controlled service context,
- users should see only the sources and actions allowed by policy,
- operational evidence should be available without reconstructing history manually.

## Documentation

- [Product Guide](docs/PRODUCT_GUIDE.md): functional modules, user workflows, analytics lifecycle, team management, and ML Workflow Studio.
- [Operations Guide](docs/OPERATIONS_GUIDE.md): installation, configuration, operations, scheduler, healthcheck, reset, update, backup, and recovery procedures.
- [Technical Reference](docs/TECHNICAL_REFERENCE.md): architecture, schemas, service layers, security model, lifecycle flows, and traceability design.

## Version

Current documentation snapshot: `1.0.0`.
