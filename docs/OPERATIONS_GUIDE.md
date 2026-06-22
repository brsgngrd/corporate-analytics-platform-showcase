# CAMP Operations Guide

This guide describes operational procedures for installing, configuring, maintaining, updating, resetting, and monitoring CAMP in an enterprise environment.

## 1. Operational Scope

CAMP is deployed as a Windows desktop platform with a controlled local service layer and a central SQL Server database. Analytical source systems such as SQL Server and Oracle are configured as managed data sources.

The operations guide is intended for authorized system administrators, platform owners, and support teams responsible for production readiness and continuity.

## 2. Deployment Components

| Component | Purpose |
|---|---|
| Desktop application | User interface for analytics, reports, assignments, reviews, tasks, and ML workflows. |
| Local broker | Controlled execution layer for database access, exports, queued jobs, and privileged operations. |
| Dispatcher | Scheduled execution process for due report and model assignments. |
| Central database | Metadata, users, roles, reports, assignments, logs, results, reviews, tasks, and ML workflow records. |
| Source databases | SQL Server and Oracle analytical source systems governed by CAMP source profiles. |
| Runtime folders | Local operational folders for logs, exports, artifacts, model workspaces, and protected configuration. |

## 3. Installation Overview

A production installation should be performed from an authorized release package. The package should be placed in the target application directory according to the organization's workstation or server deployment policy.

Typical installation activities are:

1. verify Windows and Python prerequisites,
2. place the release package in the approved application directory,
3. initialize the application runtime,
4. configure central database connection,
5. create or validate central schemas,
6. configure service-account execution,
7. configure data sources,
8. approve users and roles,
9. configure scheduler and dispatcher,
10. run health checks.

## 4. Central Database

The central SQL Server database stores CAMP metadata and operational records. It is separate from analytical source databases.

Central database responsibilities include user and role records, permissions, data source profiles, report metadata and versions, assignments, dispatcher state, execution history, persistent report results, review items, task management records, ML workflow metadata, model run history, audit, and system logs.

Initial schema creation and subsequent schema guards are idempotent. Running the schema preparation process more than once should not destroy existing data.

## 5. Service Account Model

CAMP is designed around controlled service-account execution. Users interact with the desktop interface, but privileged database operations and scheduled workloads are executed through the broker and dispatcher context.

The service account should have:

- permission to run scheduled tasks or batch jobs,
- required access to the central database,
- required access to configured analytical sources,
- access to runtime folders required by the application,
- permission to send notifications if SMTP integration is used.

The service account should not be treated as a human business owner. Business ownership remains assigned to named application users.

## 6. First Launch and Bootstrap

On first launch, the platform validates core configuration and prepares missing central database objects. If no approved administrator exists, the first successful authorized user can be established as the initial administrator according to the deployment policy.

After bootstrap, the administrator should complete central settings validation, user approval, role assignment, data source definition, source access rule configuration, SMTP configuration, MFA policy configuration where used, scheduler setup, and healthcheck verification.

## 7. Data Source Configuration

Data sources are configured by authorized administrators. Each source profile should include source type, endpoint, database or schema context, authentication mode, operational status, access rules, and usage restrictions where applicable.

Connection credentials should be stored using the configured protected storage model. Source connection validation should be performed after each new source is added or modified.

## 8. SMTP and Notification Settings

SMTP settings enable report delivery, operational notifications, review notifications, and selected system messages.

The operational team should validate SMTP server and port, sender identity, allowed domains, authentication method, TLS policy, delivery test results, and failure logging.

Missing user email addresses should not break analytical execution. They should be logged as notification resolution issues.

## 9. MFA Configuration

If MFA is enabled, administrators should define the applicable provider and enrollment policy. MFA may be local authenticator based or integrated with an external enterprise MFA provider depending on deployment design.

Operational controls should include enrollment, recovery, administrative override policy, audit logging, and fallback procedure for provider outages.

## 10. Scheduler and Dispatcher Operations

The dispatcher is responsible for executing due recurring assignments. It should be registered according to the production scheduling policy.

Dispatcher responsibilities include:

- evaluating due FR and SR assignments,
- grouping due ML-origin MDL assignments by workflow/model,
- running eligible workloads through controlled execution,
- recording execution history,
- applying notification and export rules,
- creating review items when enabled,
- recording errors and retry status.

For ML-origin outputs, dispatcher timing is driven by Data Analytics Assignments, not by a separate schedule inside ML Workflow Studio. MDS inputs run as part of the model workflow when the model is triggered.

## 11. Report and Model Execution Operations

Standard recurring reports run according to assignment configuration. Each execution records report version, assignment context, run status, row count, timing, and error information.

Self-service requests are queued and executed by the controlled service layer. Support teams can inspect queue state, request status, execution errors, and delivery outcomes.

UAT workflows are manually triggered and can be cleaned. Prod workflows are triggered through due MDL assignments. A workflow run starts with MDS dataset execution, continues through Python steps, and publishes MDL output packages.

Model outputs can be distributed to different users through assignments while preserving a shared model period.

## 12. Runtime Folders

Runtime folders are created locally according to platform configuration. They may include user data, logs, exports, report outputs, ML model workspaces, UAT and Prod model runtime folders, run artifacts, and application update backups.

Runtime data should be excluded from source packages and handled according to backup, retention, and security policies.

## 13. Model Runtime Operations

ML models may require dependencies that are not part of the CAMP application runtime. Each model can use an isolated runtime under its model workspace.

Operational recommendations:

- keep the application runtime separate from model runtimes,
- define model-specific dependency files,
- validate model runtime creation before Prod promotion,
- preserve Prod runtime snapshots required for reproducibility,
- monitor artifact and model workspace size,
- clean UAT runtime outputs according to policy.

This prevents model dependency changes from destabilizing the core CAMP application.

## 14. Healthcheck

A healthcheck should validate:

- central database connectivity,
- required schemas and tables,
- broker availability,
- dispatcher configuration,
- source profile connectivity,
- SMTP status if configured,
- permission and role matrix integrity,
- runtime folder accessibility,
- ML workflow schema readiness,
- scheduled task registration,
- logging availability.

Healthcheck results should be reviewed before production use, after updates, and after infrastructure changes.

## 15. Updates

Application updates should preserve operational data unless an explicit reset is required.

A normal update should:

1. stop the broker if required,
2. back up current application files according to policy,
3. apply the new release package,
4. update dependencies if required,
5. run idempotent schema guards,
6. restart broker and scheduler components,
7. perform healthcheck,
8. validate login, report listing, assignment listing, and sample execution.

The update process should not remove central data, protected runtime credentials, user data, report definitions, assignments, task records, review history, or model registry records unless an explicit approved maintenance procedure requires it.

## 16. Factory Reset

Factory reset is a controlled maintenance procedure. It should be used only when a clean environment or intentional data cleanup is required.

| Mode | Purpose |
|---|---|
| Protected reset | Keeps core definitions such as users, roles, settings, reports, and assignments while cleaning operational results, queues, logs, and runtime outputs according to policy. |
| Full reset | Removes application-owned runtime data and central schemas to prepare a clean installation state. |

Production use of reset procedures should require explicit approval and backup confirmation.

## 17. Backup and Recovery

Backup policy should cover:

- central SQL Server database,
- report and assignment definitions,
- review records,
- task management records,
- ML model registry and Prod snapshots,
- runtime artifact storage where required,
- configuration files and protected settings according to security policy.

Recovery procedures should validate central database restore, application version compatibility, schema readiness, broker and dispatcher startup, source connectivity, model runtime availability, and selected report or ML execution checks.

## 18. Monitoring and Logs

CAMP records operational logs for support and audit purposes. Log areas may include user activity, session events, query execution, report execution, source connection events, export activity, notification activity, SSR queue processing, transaction analysis queue, errors, dispatcher events, and ML workflow runs.

Support teams should use logs to understand operational state without relying on user screenshots or manual reconstruction.

## 19. Operational Readiness Checklist

Before production use, verify:

- central database is reachable and backed up,
- all required schemas are present,
- initial administrator is approved,
- roles and permissions are reviewed,
- data sources are validated,
- source access rules are configured,
- SMTP is tested if enabled,
- MFA policy is confirmed if enabled,
- broker starts under the expected identity,
- dispatcher schedule is active,
- sample report execution succeeds,
- sample assignment execution succeeds,
- Record Review receives rows when enabled,
- Team Management screens open and save expected records,
- ML UAT run works with approved non-production data,
- Prod model assignment behavior is validated where applicable,
- healthcheck is clean or accepted with documented exceptions.

## 20. Common Operational Situations

### A user cannot access a screen

Check user status, role membership, screen permission, action permission, and source access rules.

### A report does not run on schedule

Check assignment status, period rule, dispatcher schedule, broker status, report activation state, source connectivity, and execution history.

### A review queue is empty

Check whether the assignment has review routing enabled, whether the execution produced rows, and whether review items were already created or completed.

### An ML output is not assigned

Check that the workflow is Prod, Publish produced MDL packages, the MDL reports exist in the catalog, and assignments were created for the desired outputs.

### A model run fails because of dependencies

Check the model-specific runtime, dependency installation, selected Python runtime mode, run logs, and artifact directory permissions.

### Notification was not sent

Check SMTP configuration, user email address, allowed domain policy, assignment notification mode, and notification logs.
