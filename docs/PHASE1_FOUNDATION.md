# Phase 1 — Application Foundation

## Scope

This phase establishes a maintainable Laravel application baseline. It does not implement or claim to validate disease surveillance workflows yet.

### Work items

1. Generate a clean Laravel 13 application using PHP 8.3+.
2. Configure PostgreSQL using environment variables and a dedicated development database.
3. Configure authentication using a maintained Laravel-compatible approach.
4. Implement server-side authorization and scoped permissions before adding operational data screens.
5. Establish automated tests for unauthenticated access, role checks, and area-scoped access.
6. Add code formatting and continuous integration after the framework scaffold is committed.
7. Document local setup, migration, test, backup and security expectations.

## Proposed authorization roles

| Role | Intended scope |
|---|---|
| System administrator | Manage system configuration and accounts; access to sensitive data must still be logged |
| National surveillance officer | National surveillance overview and authorized case coordination |
| Regional veterinary officer | Authorized regional records and escalations |
| District veterinary officer | Authorized district records and investigations |
| Field surveillance officer | Assigned field notifications, visits and investigations |
| Laboratory officer | Assigned samples, tests and laboratory results |
| Poultry farmer / farm manager | Own farm/flock information and own notifications |
| Policymaker / read-only analyst | Approved aggregate reports; no routine editing |

These roles are a starting proposal. Ministry owners must confirm the exact permissions, organizational hierarchy, and escalation authority before production use.

## Initial data domains

- Users and role assignments
- Administrative areas and user area assignments
- Poultry farms and flocks
- Disease notifications / events
- Investigations and surveillance visits
- Samples and laboratory tests/results
- Response actions and follow-up
- Vaccination and biosecurity records
- Alerts and alert acknowledgements
- Audit events

Implement these through versioned migrations, foreign keys, validation, and tests. Do not load invented Tanzanian administrative boundaries, official disease lists, or alert thresholds as if they were authoritative.

## Acceptance criteria

- [ ] A fresh Laravel installation starts locally with documented steps.
- [ ] PostgreSQL connection is environment-configurable and uses a dedicated development database.
- [ ] No secrets or real personal/surveillance data are committed.
- [ ] Authentication is required for protected application routes.
- [ ] Server-side authorization is covered by automated tests.
- [ ] Role checks and administrative-area scoping are enforced on data access, not just in the interface.
- [ ] Migrations can be run against a disposable development/test database.
- [ ] Automated tests and code-quality checks run in CI.
- [ ] The README distinguishes implemented features from planned work.

## Safety and privacy

Use synthetic records in development. Do not connect this project to production databases or ingest real surveillance data until access approval, data governance, retention, audit, backup, and incident-response requirements have been approved.
