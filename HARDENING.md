<!-- markdownlint-disable -->

# Hardening Report: sourcetoad--aws-ecs-deploy-action/v1.1.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sourcetoad--aws-ecs-deploy-action/v1.1.7** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file .github/workflows/linting.yml references two actions using mutable refs instead of full 40-character commit SHAs. 'actions/checkout@v4' uses a version tag and 'azohra/shell-linter@latest' uses a branch name. Both can be silently updated to point to different (potentially malicious) code without any change to the workflow file, enabling supply-chain attacks.

Locations:

- `.github/workflows/linting.yml:9`
- `.github/workflows/linting.yml:12`

### missing-permissions (severity: medium)

The workflow file .github/workflows/linting.yml has no top-level 'permissions:' key and the only job ('bash-lint') also has no job-level 'permissions:' key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. write access to contents). Minimal explicit permissions should be declared.

Locations:

- `.github/workflows/linting.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed .github/workflows/linting.yml: (1) Pinned actions/checkout@v4 to full SHA 34e114876b0b11c390a56381ad16ebd13914f8d5 and azohra/shell-linter@latest to full SHA 6bbeaa868df09c34ddc008e6030cfe89c03394a1, preserving original refs as inline comments. (2) Added top-level `permissions: {}` to explicitly deny all token permissions, since the linting workflow requires no write access.

