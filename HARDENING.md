<!-- markdownlint-disable -->

# Hardening Report: pulumi--actions/v6.6.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **pulumi--actions/v6.6.1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Every workflow file uses mutable tag-based or branch-based `uses:` references instead of immutable 40-character SHA commit hashes, making the workflows vulnerable to supply-chain attacks if any referenced action is compromised or its tag is moved.

Failing references include:
- codeql.yml: actions/checkout@v4, github/codeql-action/init@v3, github/codeql-action/autobuild@v3, github/codeql-action/analyze@v3
- export-repo-secrets.yml: actions/create-github-app-token@v1, pulumi/esc-export-secrets-action@v1
- immutable-action.yml: actions/checkout@v4, actions/publish-immutable-action@v0.0.4
- tag.yaml: Actions-R-Us/actions-tagger@latest
- test.yml: actions/checkout@v4, actions/setup-node@v4, actions/setup-go@v5
- update_dist.yml: pulumi/esc-action@v1, actions/checkout@v4, actions/setup-node@v4, stefanzweifel/git-auto-commit-action@v5
- workflow.yml: actions/checkout@v4, actions/setup-node@v4, dorny/paths-filter@v3, actions/upload-artifact@v4, actions/download-artifact@v4, actions/setup-dotnet@v3, actions/setup-go@v5, actions/setup-python@v5.2.0, actions/cache@v4

Locations:

- `.github/workflows/codeql.yml:27`
- `.github/workflows/export-repo-secrets.yml:10`
- `.github/workflows/immutable-action.yml:16`
- `.github/workflows/tag.yaml:7`
- `.github/workflows/test.yml:18`
- `.github/workflows/update_dist.yml:18`
- `.github/workflows/workflow.yml:17`

### broad-permissions (severity: medium)

Two workflow files set `permissions: write-all` at the top level, granting every available GitHub Actions permission to all jobs. This violates the principle of least privilege and should be replaced with specific minimal permission scopes.

- export-repo-secrets.yml: `permissions: write-all` at line 1
- update_dist.yml: `permissions: write-all` at the bottom of the file (applies globally)

Locations:

- `.github/workflows/export-repo-secrets.yml:1`
- `.github/workflows/update_dist.yml:28`

### missing-permissions (severity: medium)

Three workflow files have no `permissions:` key at the top level and no job-level `permissions:` blocks on any of their jobs. Without an explicit permissions block, workflows inherit the repository's default token permissions, which may be overly broad (write access to contents, etc.).

- tag.yaml: no permissions key; single job `tag-major` has no permissions
- test.yml: no permissions key; jobs `build-test`, `test-output`, `test-update-plan` have no permissions
- workflow.yml: no permissions key; all jobs (`install-and-build`, `test-install-only-without-removal-of-pre-installed-pulumi`, `test-install-only`, `test-dotnet-stack`, `test-golang-stack`, `test-nodejs-stack`, `test-python-stack`, `test-generic-inputs`) have no permissions

Locations:

- `.github/workflows/tag.yaml:1`
- `.github/workflows/test.yml:1`
- `.github/workflows/workflow.yml:1`

### script-injection (severity: high)

Three `run:` steps in test.yml directly interpolate `${{ steps.*.outputs.* }}` expressions inside shell command strings (sub-rule a). The `steps.*.outputs.*` context is workflow-controllable and flows through YAML template substitution before the shell processes it, allowing an attacker to inject arbitrary shell commands if the output value contains shell metacharacters.

Offending lines:
- Line 57: `run: echo 'The random string is \`${{ steps.pulumi.outputs.name }}\`'`
- Line 65: `run: echo 'The random string is \`${{ steps.pulumioutput.outputs.name }}\`'`
- Line 113: `run: echo 'The random string is \`${{ steps.pulumi-up.outputs.name }}\`'`

Fix: move the value into an `env:` variable and reference it as a quoted shell variable, e.g. `echo "The random string is $STEP_OUTPUT"`.

Locations:

- `.github/workflows/test.yml:57`
- `.github/workflows/test.yml:65`
- `.github/workflows/test.yml:113`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, broad-permissions, missing-permissions, script-injection

**Notes:**

Fixed all 4 findings across 7 workflow files:

1. unpinned-uses: Pinned all action references to full SHA commits in codeql.yml, export-repo-secrets.yml, immutable-action.yml, tag.yaml, test.yml, update_dist.yml, and workflow.yml. Used lookup_action_sha to resolve each tag to its real commit SHA.

2. broad-permissions: Replaced `permissions: write-all` in export-repo-secrets.yml (contents: read + id-token: write) and update_dist.yml (contents: write + id-token: write) with minimal specific permissions.

3. missing-permissions: Added top-level permissions blocks to tag.yaml (contents: write for tag pushing), test.yml (contents: read), and workflow.yml (contents: read).

4. script-injection: Fixed 3 run: steps in test.yml that directly interpolated ${{ steps.*.outputs.* }} expressions. Moved each expression into an env: block (PULUMI_OUTPUT_NAME, PULUMI_UP_OUTPUT_NAME) and referenced them as plain shell variables in the run: commands.

