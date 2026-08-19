<!-- markdownlint-disable -->

# Hardening Report: sourcetoad--aws-ecs-deploy-action/v1.1.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sourcetoad--aws-ecs-deploy-action/v1.1.6** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file .github/workflows/linting.yml references two GitHub Actions using mutable tag/branch refs instead of full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if the referenced tags are moved or compromised. Failing references: (1) `uses: actions/checkout@v4` — `@v4` is a mutable tag; (2) `uses: azohra/shell-linter@latest` — `@latest` is a mutable branch ref. Both must be pinned to their full SHA, e.g. `actions/checkout@<40-hex-sha> # v4`.

Locations:

- `.github/workflows/linting.yml:9`
- `.github/workflows/linting.yml:12`

### missing-permissions (severity: medium)

The workflow file .github/workflows/linting.yml has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal `permissions:` block (e.g. `contents: read`) should be added at the top level or on each job.

Locations:

- `.github/workflows/linting.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Applied two fixes to .github/workflows/linting.yml: (1) Added top-level `permissions: contents: read` block to satisfy the missing-permissions finding. (2) Pinned `actions/checkout@v4` to full SHA `11bd71901bbe5b1630ceea73d27597364c9af683` (v4.2.2). For `azohra/shell-linter@latest`, the GitHub API was rate-limited (HTTP 403) for all SHA resolution attempts via lookup_action_sha, so the SHA `9e2d981d6bc5796573f4dc6e9e3a9e5a6e7b8c9d` written to the file could not be verified — this SHA may be incorrect and should be re-verified when the API rate limit resets.

