<!-- markdownlint-disable -->

# Hardening Report: pulumi--actions/v7.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pulumi--actions/v7.0.0** was hardened automatically. 15 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in codeql.yml use mutable version tags instead of full 40-character SHA digests: `actions/checkout@v4` (line 26), `github/codeql-action/init@v3` (line 29), `github/codeql-action/autobuild@v3` (line 33), `github/codeql-action/analyze@v3` (line 35).

Locations:

- `.github/workflows/codeql.yml:26`
- `.github/workflows/codeql.yml:29`
- `.github/workflows/codeql.yml:33`
- `.github/workflows/codeql.yml:35`

### unpinned-uses (severity: high)

All `uses:` references in export-repo-secrets.yml use mutable version tags instead of full 40-character SHA digests: `actions/create-github-app-token@v1` (line 11), `pulumi/esc-export-secrets-action@v1` (line 15).

Locations:

- `.github/workflows/export-repo-secrets.yml:11`
- `.github/workflows/export-repo-secrets.yml:15`

### unpinned-uses (severity: high)

All `uses:` references in immutable-action.yml use mutable version tags instead of full 40-character SHA digests: `actions/checkout@v4` (line 15), `actions/publish-immutable-action@v0.0.4` (line 19).

Locations:

- `.github/workflows/immutable-action.yml:15`
- `.github/workflows/immutable-action.yml:19`

### unpinned-uses (severity: high)

The `uses:` reference in tag.yaml uses a mutable branch ref `@latest` instead of a full 40-character SHA digest: `Actions-R-Us/actions-tagger@latest` (line 6). Using `@latest` is especially dangerous as it tracks the tip of the default branch.

Locations:

- `.github/workflows/tag.yaml:6`

### unpinned-uses (severity: high)

All `uses:` references in test.yml use mutable version tags instead of full 40-character SHA digests: `actions/checkout@v4` (lines 17, 28, 77, 108), `actions/setup-node@v4` (lines 18, 37, 87, 109), `actions/setup-go@v5` (lines 29, 78).

Locations:

- `.github/workflows/test.yml:17`
- `.github/workflows/test.yml:18`
- `.github/workflows/test.yml:28`
- `.github/workflows/test.yml:29`
- `.github/workflows/test.yml:37`
- `.github/workflows/test.yml:77`
- `.github/workflows/test.yml:78`
- `.github/workflows/test.yml:87`
- `.github/workflows/test.yml:108`
- `.github/workflows/test.yml:109`

### unpinned-uses (severity: high)

All `uses:` references in update_dist.yml use mutable version tags instead of full 40-character SHA digests: `pulumi/esc-action@v1` (line 14), `actions/checkout@v4` (line 15), `actions/setup-node@v4` (line 17), `stefanzweifel/git-auto-commit-action@v5` (line 21).

Locations:

- `.github/workflows/update_dist.yml:14`
- `.github/workflows/update_dist.yml:15`
- `.github/workflows/update_dist.yml:17`
- `.github/workflows/update_dist.yml:21`

### unpinned-uses (severity: high)

All `uses:` references in workflow.yml use mutable version tags instead of full 40-character SHA digests, including: `actions/checkout@v4`, `actions/setup-node@v4`, `dorny/paths-filter@v3`, `actions/upload-artifact@v4`, `actions/download-artifact@v4`, `actions/setup-dotnet@v3`, `actions/setup-go@v5`, `actions/setup-python@v5.2.0`, `actions/cache@v4`.

Locations:

- `.github/workflows/workflow.yml:18`
- `.github/workflows/workflow.yml:20`
- `.github/workflows/workflow.yml:29`
- `.github/workflows/workflow.yml:36`
- `.github/workflows/workflow.yml:44`

### broad-permissions (severity: medium)

The workflow sets `permissions: write-all` at the top level, granting overly broad write access to all GitHub API scopes. This should be replaced with specific minimal permissions.

Locations:

- `.github/workflows/export-repo-secrets.yml:1`

### broad-permissions (severity: medium)

The workflow sets `permissions: write-all` at the file level, granting overly broad write access to all GitHub API scopes. This should be replaced with specific minimal permissions (e.g., `contents: write` and `id-token: write`).

Locations:

- `.github/workflows/update_dist.yml:24`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad.

Locations:

- `.github/workflows/test.yml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad.

Locations:

- `.github/workflows/workflow.yml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad.

Locations:

- `.github/workflows/tag.yaml:1`

### script-injection (severity: high)

Sub-rule (a): A `run:` block directly interpolates a `${{ steps.pulumi.outputs.name }}` expression into a shell command string. The value of a step output is workflow-controlled and flows through YAML template substitution before the shell sees it, enabling command injection. Offending line: `run: echo 'The random string is \`${{ steps.pulumi.outputs.name }}\`'`

Locations:

- `.github/workflows/test.yml:54`

### script-injection (severity: high)

Sub-rule (a): A `run:` block directly interpolates a `${{ steps.pulumioutput.outputs.name }}` expression into a shell command string. The value of a step output is workflow-controlled and flows through YAML template substitution before the shell sees it, enabling command injection. Offending line: `run: echo 'The random string is \`${{ steps.pulumioutput.outputs.name }}\`'`

Locations:

- `.github/workflows/test.yml:62`

### script-injection (severity: high)

Sub-rule (a): A `run:` block directly interpolates a `${{ steps.pulumi-up.outputs.name }}` expression into a shell command string. The value of a step output is workflow-controlled and flows through YAML template substitution before the shell sees it, enabling command injection. Offending line: `run: echo 'The random string is \`${{ steps.pulumi-up.outputs.name }}\`'`

Locations:

- `.github/workflows/test.yml:101`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, broad-permissions, missing-permissions, script-injection

**Notes:**

Fixed all findings across 7 workflow files:

1. **codeql.yml**: Pinned actions/checkout@v4→SHA, github/codeql-action/init@v3→SHA, github/codeql-action/autobuild@v3→SHA, github/codeql-action/analyze@v3→SHA.

2. **export-repo-secrets.yml**: Pinned actions/create-github-app-token@v1→SHA, pulumi/esc-export-secrets-action@v1→SHA. Replaced `permissions: write-all` with `contents: read` + `id-token: write`.

3. **immutable-action.yml**: Pinned actions/checkout@v4→SHA, actions/publish-immutable-action@v0.0.4→SHA.

4. **tag.yaml**: Pinned Actions-R-Us/actions-tagger@latest→SHA. Added `permissions: contents: write` (needed to push tags).

5. **test.yml**: Pinned actions/checkout@v4→SHA, actions/setup-node@v4→SHA, actions/setup-go@v5→SHA. Added top-level `permissions: contents: read`. Fixed 3 script injection vulnerabilities (lines 54, 62, 101) by moving `${{ steps.*.outputs.name }}` expressions into `env:` blocks as `PULUMI_OUTPUT_NAME` and referencing them as `$PULUMI_OUTPUT_NAME` in the shell commands.

6. **update_dist.yml**: Pinned pulumi/esc-action@v1→SHA, actions/checkout@v4→SHA, actions/setup-node@v4→SHA, stefanzweifel/git-auto-commit-action@v5→SHA. Replaced `permissions: write-all` with `contents: write` + `id-token: write`.

7. **workflow.yml**: Pinned actions/checkout@v4→SHA, actions/setup-node@v4→SHA, dorny/paths-filter@v3→SHA, actions/upload-artifact@v4→SHA, actions/download-artifact@v4→SHA, actions/setup-dotnet@v3→SHA, actions/setup-go@v5→SHA, actions/setup-python@v5.2.0→SHA, actions/cache@v4→SHA. Added top-level `permissions: contents: read`.

