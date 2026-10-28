# Tanzania National Poultry Disease Surveillance and Early Warning System (TNPDS-EWS)

A planned national platform to register poultry farms and flocks, receive disease notifications, coordinate veterinary investigations and laboratory testing, and support timely disease alerts and evidence-based decisions in Tanzania.

> **Development status:** Phase 1 foundation planning. This repository does not yet contain a runnable Laravel application. Do not use it for real farmer, farm, animal-health, or outbreak data.

## Technology baseline

- **Laravel 13** with PHP **8.3 or later**
- Blade and Livewire for server-rendered interactive workflows
- PostgreSQL as the primary database; PostGIS for spatial features when enabled
- Laravel authorization policies and role-based access control
- Pest or PHPUnit for automated tests
- Leaflet for later map views
- Python analytics may be introduced later as a separately tested service

Laravel 13's minimum PHP version is 8.3. Check the official [Laravel release and support policy](https://laravel.com/docs/releases) before upgrading major versions.

## Core workflow

1. Register a poultry farm and its flocks.
2. Submit a suspected disease notification.
3. Review and triage the notification.
4. Investigate the farm and record clinical and epidemiological findings.
5. Collect and track laboratory samples and results.
6. Record response measures, follow-up visits, and case closure.
7. Display authorized operational summaries and geographic patterns.
8. Issue alerts using approved veterinary rules and verified thresholds.

The system must not independently diagnose disease. Alerts and case classifications must follow Ministry-approved veterinary guidance and be reviewable by authorized officers.

## Initial user roles

- System administrator
- National surveillance officer
- Regional veterinary officer
- District veterinary officer
- Field surveillance officer
- Laboratory officer
- Poultry farmer or farm manager
- Policymaker / read-only analyst

Access must be scoped to the user's authorized administrative area and duties. Do not rely on hiding menu items as an authorization mechanism; enforce access in server-side policies and tests.

## Getting started

The repository currently contains planning and setup guidance rather than a generated Laravel framework skeleton. In GitHub Codespaces, scaffold the app in a temporary directory and copy it into the repository so the existing documentation is preserved:

```bash
composer create-project laravel/laravel:^13.0 /tmp/tnpds-ews
cp -a /tmp/tnpds-ews/. .
```

Review the copied files before committing. Then install/configure PostgreSQL and set local values in `.env` using `.env.example` as a guide. Never commit `.env`, credentials, API keys, real surveillance records, or personal data.

After the Laravel scaffold is present:

```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan test
```

The commands above are setup guidance and have **not** been run against this repository. Configure a dedicated development database before running migrations. Do not point development tools at a production database.

## Development principles

- Collect only information needed for public veterinary surveillance.
- Validate all submitted data and use least-privilege access.
- Record important changes to disease events, investigations, laboratory results, and response actions in an audit trail.
- Use synthetic data for development and automated tests.
- Define disease lists, case definitions, reporting timelines, alert thresholds, and administrative boundaries with authorized Tanzanian veterinary stakeholders.
- Plan encrypted transport, secure backups, retention rules, recovery tests, and incident response before production deployment.
- Design for intermittent connectivity as a later, explicitly tested capability rather than assuming offline support exists.

## Roadmap

- Phase 1: application scaffold, configuration, authentication and access-control foundation
- Phase 2: administrative areas and reference data
- Phase 3: farm and flock registry
- Phase 4: notifications, triage, investigation and surveillance dashboard
- Phase 5: sample tracking and laboratory results
- Phase 6: maps and operational reporting
- Phase 7: vaccination, biosecurity and poultry movement records
- Phase 8: configurable rule-based alerts and escalation
- Phase 9: security review, operational readiness and deployment
- Phase 10: validated analytics and forecasting

## License and governance

Confirm the Ministry's intended ownership, reuse, and open-source licensing terms before selecting a license. No license has been assigned yet.
