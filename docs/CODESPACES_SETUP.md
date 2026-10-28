# GitHub Codespaces Setup — Phase 1

This procedure generates the Laravel application in a Codespace while preserving the planning documents already committed to the repository.

## 1. Create the Codespace

1. Open https://github.com/chaekas/MLF_pd_surveillance.
2. Select **Code → Codespaces → Create codespace on main**.
3. Wait for the VS Code browser editor and terminal to open.
4. Confirm the repository root and inspect its current state:

```bash
pwd
git status --short
find . -maxdepth 2 -type f | sort | head -80
```

Do not proceed if you have uncommitted work you need to preserve. The scaffold step below copies generated framework files into the repository.

## 2. Check runtime prerequisites

```bash
php -v
composer --version
node -v
npm -v
```

Laravel 13 requires PHP 8.3 or later. If the Codespace was created before `.devcontainer/devcontainer.json` was added, rebuild the container from the Command Palette using **Codespaces: Rebuild Container**.

## 3. Generate Laravel in a temporary directory

Use a temporary directory so the scaffold generator does not overwrite the repository's existing files in place. The Laravel installer can offer an official starter kit; choose the **Livewire** starter kit if prompted, and select **Pest** or PHPUnit consistently for tests. If the installer is not available, install it first.

```bash
composer global require laravel/installer
export PATH="$HOME/.composer/vendor/bin:$PATH"
laravel new /tmp/tnpds-ews
```

If the installer rejects an absolute target path, run it from `/tmp` with `laravel new tnpds-ews`; the generated project will be at `/tmp/tnpds-ews`.

When prompted:
- Starter kit: **Livewire** (if available)
- Database: **PostgreSQL** for the local application configuration
- Testing framework: **Pest** (recommended) or PHPUnit
- Do not provide production credentials or real surveillance data

## 4. Copy scaffold files without replacing project documentation

From the repository root, run:

```bash
rsync -av /tmp/tnpds-ews/ ./ \
  --exclude='.git/' \
  --exclude='README.md' \
  --exclude='docs/' \
  --exclude='.env' \
  --exclude='.env.example' \
  --exclude='.gitignore'
```

This preserves the committed `README.md`, `docs/`, `.env.example`, and `.gitignore`. Review the generated scaffold and merge any required Laravel ignore rules into the existing `.gitignore` rather than blindly replacing it.

Inspect the changes:

```bash
git status --short
git diff --stat
git diff -- README.md docs .env.example .gitignore
```

The last command should show no changes to those preserved files from the copy operation.

## 5. Configure local environment and PostgreSQL

Create the local environment file from Laravel's generated example only after the scaffold has been copied:

```bash
cp .env.example .env
```

Edit `.env` locally; never commit it. Use a dedicated development database and least-privilege database user. Example values (replace the password locally; do not use this example password as a credential):

```dotenv
APP_NAME="TNPDS-EWS"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=tnpds_ews_dev
DB_USERNAME=tnpds_dev
DB_PASSWORD=REPLACE_WITH_A_LOCAL_SECRET
```

A Codespace does not automatically guarantee that a PostgreSQL server is running. If PostgreSQL is not available, use a separate development-only PostgreSQL service/container or an explicitly approved development database. Do not use production connection details. Do not run migrations until the target database is confirmed to be disposable and dedicated to development.

Then generate the application key:

```bash
php artisan key:generate
```

## 6. Inspect and validate the scaffold

Do not claim tests have passed until they have been executed. After reviewing the files and confirming the development database target, run:

```bash
composer install
php artisan about
php artisan route:list
php artisan test
```

Only run `php artisan migrate` after confirming the connection points to the dedicated development database. The initial Laravel migration is expected to create the framework's own tables.

## 7. Commit safely

Review the exact files before committing:

```bash
git status --short
git diff --check
git diff --stat
```

Confirm `.env` is ignored and not staged:

```bash
git check-ignore -v .env
git status --short
```

Commit only intended source files. Keep all credentials, real farm records, outbreak reports, sample results, and personal information out of Git history.

## 8. Authentication and authorization tests — next action

Once the generated scaffold and selected starter kit are visible in the repository, the next step is to implement and test:
- unauthenticated users are redirected from protected pages;
- authenticated users can access allowed pages;
- a user without the required role is forbidden from administrative actions;
- area-scoped users cannot access records outside their assigned administrative area;
- role checks are enforced server-side, not only by navigation visibility.

The exact test and policy files depend on the generated Laravel starter kit and test framework. Do not add guessed tests until the scaffold is inspected.

## Verification status

This document and the Codespaces configuration are setup guidance. No Codespace has been run from this conversation, no PHP/Composer commands have been executed in the user's environment, and no Laravel test result is implied.
