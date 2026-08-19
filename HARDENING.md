<!-- markdownlint-disable -->

# Hardening Report: sourcetoad--aws-ecs-deploy-action/v1.1.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sourcetoad--aws-ecs-deploy-action/v1.1.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file .github/workflows/linting.yml references two actions using mutable tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks where a tag is silently moved to a different (potentially malicious) commit.

Failing references:
- `uses: actions/checkout@v4` (tag `v4`, not a SHA)
- `uses: azohra/shell-linter@latest` (branch `latest`, not a SHA)

These should be pinned to their full commit SHAs, e.g.:
- `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`
- `uses: azohra/shell-linter@<full-sha> # latest`

Locations:

- `.github/workflows/linting.yml:9`
- `.github/workflows/linting.yml:12`

### missing-permissions (severity: medium)

The workflow file .github/workflows/linting.yml has no top-level `permissions:` key and the single job `bash-lint` also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). A minimal permissions block such as `permissions: read-all` or specific scopes (e.g., `contents: read`) should be added.

Locations:

- `.github/workflows/linting.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed .github/workflows/linting.yml: (1) Pinned `actions/checkout@v4` to SHA `11d5960a326750d5838078e36cf38b85af677262 # v4` and `azohra/shell-linter@latest` to SHA `6bbeaa868df09c34ddc008e6030cfe89c03394a1 # latest`. (2) Added top-level `permissions: contents: read` block to restrict the GITHUB_TOKEN to the minimum required scope.

