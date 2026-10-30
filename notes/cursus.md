---
layout: page
title: Development to Production
description: >
    Notes I keep for myself about developing, validating, deploying, and maintaining production applications.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What I Need to Know About Development and Production

I think of software development and production as two separate environments with different responsibilities.

**Development** is where I write code, experiment, fix bugs, and build new features.

**Production** is where the application runs for its actual users. Production contains real application data and must remain available and consistent.

For Cursus, my current setup is:

| Component | Purpose |
|---|---|
| Mac Mini (`M59D4`) | My development workstation |
| Raspberry Pi (`S59D4`) | My self-hosted production server |
| Private GitHub repository | Stores version-controlled source code |
| `/home/jcnccsr/apps/c` | Source repository on the Raspberry Pi |
| `/opt/cursus/releases/` | Stores separate production releases |
| `/opt/cursus/current` | Symbolic link to the active production release |
| `/var/lib/cursus/cursus.db` | Production SQLite database |
| `/var/lib/cursus/backups/` | Production database backups |
| `cursus.service` | Systemd service that runs the application |
| `https://c.jccesar.com` | Public production application |

Remember that source code, the production application, and production data are three different things.

- Git stores my source code and its history.
- A production release contains the application files needed to run a particular version.
- The database stores persistent application data, including learner progress and other records.

Changing the source code does not automatically update production. Deploying a new release does not necessarily change the database. Restoring the source code does not restore lost database records.

## The Development-to-Production Process

My standard deployment workflow is:

1. Develop the feature.
2. Review and save the changes in Git.
3. Validate the code.
4. Prepare a production build.
5. Back up production data.
6. Stage a new release.
7. Switch production to the new release.
8. Verify the running application.
9. Roll back if deployment fails.

The goal is to make deployment repeatable. I should not need to improvise a new procedure every time I want to publish a change.

## Development

### Working on the Source Code

I make changes in my development working tree.

For example, I might change:

- A React component.
- A Next.js page or API route.
- CSS styling.
- Curriculum content.
- Database access logic.
- Authentication.
- Tests.
- Backup or deployment scripts.

During development, I should work on the smallest part of the application that solves the problem.

I do not need to deploy every change immediately. Several related changes can be grouped into one release after they are ready.

### Checking the Current Git State

Before making changes or preparing a release, I can inspect the repository:

```bash
cd /home/jcnccsr/apps/c

git status
git branch --show-current
git log -5 --oneline
```

`git status` shows modified, deleted, and untracked files.

`git branch --show-current` tells me which branch I am working on.

`git log -5 --oneline` shows the five most recent commits.

I should understand the state of the working tree before staging or committing changes.

### Reviewing Changes

I can inspect the changes to tracked files with:

```bash
git diff
```

I can inspect the summary with:

```bash
git diff --stat
```

Untracked files do not appear in the ordinary `git diff`, so I should also inspect `git status`.

Before committing, I should make sure that I am not accidentally including:

- Passwords or authentication secrets.
- Environment files.
- Database files.
- Private backups.
- Generated build output.
- Temporary files.
- Unrelated changes.

I should not delete unfamiliar files simply because they appear in Git status. They may be intentional work that has not yet been committed.

## Git and Version Control

### What Git Does

Git records changes to my source code over time.

A commit is a snapshot of the tracked repository state. It provides a point in history that I can inspect, compare, and use as a basis for future work.

A commit does not automatically deploy anything.

Likewise, pushing a commit to GitHub does not automatically update my production server unless I have configured a deployment workflow to do so.

### Staging and Committing Changes

I can stage selected files:

```bash
git add app/page.tsx app/globals.css
```

Then inspect what I staged:

```bash
git diff --cached
```

If the staged changes are correct, I can commit them:

```bash
git commit -m "Improve learning dashboard"
```

The commit message should describe the change.

For example:

- `Improve learning dashboard`
- `Add curriculum validation`
- `Fix backup permissions`
- `Update lesson navigation`

### Pushing Changes to GitHub

I can push my current branch:

```bash
git push
```

GitHub now contains the commit, assuming the push succeeds.

GitHub is my version-controlled source repository. It is not a substitute for a production database backup.

A Git commit does not protect learner progress stored in SQLite.

### A Note About Uncommitted Changes

I should not use commands such as:

```bash
git reset --hard
git clean -fd
```

as routine cleanup commands.

These commands can destroy uncommitted work or remove untracked files.

If I need to discard changes, I should first identify exactly which files are involved and confirm that I no longer need them.

## Validation Before Deployment

Before publishing a release, I need evidence that the application is likely to work.

Validation should be proportional to the change. During development, I can run focused checks. Before a production release, I should run the project's standard release checks.

### Linting

Linting identifies suspicious code patterns and violations of the project's coding rules.

For Cursus:

```bash
npm run lint
```

A successful result means the configured ESLint checks passed.

It does not prove that the application behaves correctly.

### Type Checking

Cursus uses TypeScript. I can check its types with:

```bash
npm run typecheck
```

Type checking helps detect problems such as incorrect function arguments, incompatible values, and missing type definitions.

A successful type check does not replace runtime testing.

### Automated Tests

I can run the project's test suite:

```bash
npm test
```

Tests exercise selected behaviours and help detect regressions.

For Cursus, tests cover areas such as authentication, learning functionality, curriculum validation, database operations, and backup behaviour.

I should investigate a failed test before deploying. I should not disable a failing test merely to make the release pass.

### Curriculum Validation

Cursus has a dedicated curriculum validation command:

```bash
npm run curriculum:validate
```

This checks the structure of the curriculum data, including relationships and required content.

I should run it when changes affect curriculum content or structure.

A structurally valid curriculum is not necessarily educationally complete, but validation helps prevent malformed data from reaching the application.

### Operations Checks

Cursus also provides:

```bash
npm run ops:check
```

This checks selected operational requirements, such as runtime details, native modules, database permissions, and storage conditions.

The result must be interpreted in context. For example, a check executed in a temporary directory might report a different state directory from production.

I should not assume that a successful check in one environment proves that every production condition is correct.

### Production Build

I can build the application with:

```bash
npm run build
```

For Cursus, this produces the optimized Next.js production build.

A successful build confirms that Next.js could compile and prepare the application under the environment used for the build.

It does not prove that the production service will start successfully or that every page will work.

### The Standard Release Check

For Cursus, the standard release validation sequence is:

```bash
set -e

npm run lint
npm run typecheck
npm test
npm run curriculum:validate
npm run ops:check
npm run build

echo "RELEASE VALIDATION PASSED"
```

`set -e` stops the sequence when a command fails.

I should run this in the intended release candidate directory, using the same Node.js version as production.

I do not need to run this entire sequence after every small edit. I need it to pass before the release is approved for deployment.

## Runtime and Environment Compatibility

### Why the Node.js Version Matters

An application may work under one Node.js version and fail under another.

The production service currently uses:

```text
Node.js v24.22.1
npm 11.19.1
```

I should use that runtime when preparing and validating the production release.

I can check the versions with:

```bash
node --version
npm --version
```

On the Raspberry Pi, I can explicitly select the production Node.js installation:

```bash
export PATH="/opt/nodejs/24.22.1/bin:$PATH"

node --version
npm --version
```

This is especially important for native dependencies such as SQLite and Argon2.

Native modules may depend on the Node.js runtime, operating system, and processor architecture. A dependency installation prepared for a different environment should not automatically be assumed to work in production.

### Installing Dependencies

The lockfile records the dependency versions used by the project.

I can install the locked dependencies with:

```bash
npm ci
```

Unlike a normal `npm install`, `npm ci` is intended for clean, reproducible installations based on the lockfile. It removes the existing `node_modules` directory before installing the dependencies.

I should run it in the candidate directory, not in the active production release.

I should not run dependency upgrades as part of an ordinary deployment unless those upgrades have been intentionally reviewed and validated.

## Preparing a Production Release

### Why Releases Should Be Separate

I should not overwrite the running application file by file.

If a deployment replaces files while the service is running, the application could temporarily contain a mixture of old and new files. A failed copy operation could also leave an incomplete application.

Instead, I prepare a complete new release in its own directory.

For example:

```text
/opt/cursus/
├── current -> /opt/cursus/releases/20261030-1
└── releases/
    ├── 20261029-3/
    ├── 20261030-1/
    └── ...
```

Each release directory contains the application version that was prepared for deployment.

The `current` symbolic link identifies the active release.

The old release remains available after I deploy a new one, making rollback much easier.

### What a Release Contains

A release generally contains:

- Application source and configuration files needed at runtime.
- The production build output.
- Production dependencies.
- Public assets.
- The package manifest and lockfile.

It should not contain:

- Production database files.
- Private database backups.
- Environment files containing secrets.
- Unnecessary Git history.
- Temporary development files.

Cursus keeps its database and backup directory outside the release tree, so switching releases does not replace those files.

### Preparing the Candidate

I should prepare the new release directory without changing the active symbolic link.

For example:

```text
/opt/cursus/releases/20261030-1
```

I then copy the validated candidate into that directory, excluding private files and temporary data.

After copying, I should confirm that the build output and dependencies are present.

For Cursus, useful checks include:

```bash
test -f /opt/cursus/releases/20261030-1/.next/BUILD_ID
test -f /opt/cursus/releases/20261030-1/node_modules/next/package.json
```

If either command fails, the release is incomplete and should not be activated.

## Production Database and Backups

### Why the Database Is Different from Source Code

Cursus uses SQLite for persistent application data.

The production database is located at:

```text
/var/lib/cursus/cursus.db
```

Backups are stored under:

```text
/var/lib/cursus/backups/
```

The database may contain information that cannot be reconstructed from Git, including learner progress and administrative records.

I should therefore treat production data as persistent state, separate from the application's release files.

### Creating a Backup

Cursus provides a backup command:

```bash
npm run db:backup
```

However, the command uses the configured database and backup paths. I must ensure those paths refer to the intended database before running it.

The backup script creates a separate database file, verifies SQLite integrity and foreign-key consistency, and applies restricted file permissions.

A backup should not be considered verified merely because a file exists. The backup operation should complete successfully and perform its integrity checks.

### Verifying a Backup

For Cursus, a successful backup command reports the verified backup path.

I can inspect the file and its permissions:

```bash
sudo ls -l /var/lib/cursus/backups/
```

The database backup should be readable by the intended recovery process but not unnecessarily accessible to other users.

I should keep backups separate from the release directories so that release cleanup cannot accidentally delete them.

### Database Migrations

A database migration changes the database structure.

For example, a migration might add a column, create a table, or change an index.

Migrations require more care than deploying ordinary application code because database changes can affect compatibility with both the new and previous releases.

Cursus currently defines this command:

```bash
npm run db:migrate
```

I must understand what it does before running it. In the current project, the command also runs curriculum and other seed scripts.

That means it is not simply a schema migration command.

I should not run it automatically during every deployment. I should first establish whether a migration is required, inspect its effects, and confirm that a recovery plan exists.

A code-only deployment should not change production database contents unnecessarily.

### Backups Must Be Recoverable

A backup is useful only if I can restore it.

I should periodically verify that I can recover a database backup in a safe test environment.

I must never experiment with a restore command against the live database without explicitly intending to replace production data.

A restore procedure should identify the source backup
