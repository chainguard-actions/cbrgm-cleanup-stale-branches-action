<!-- markdownlint-disable -->

# Hardening Report: cbrgm--cleanup-stale-branches-action/v1.2.13

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cbrgm--cleanup-stale-branches-action/v1.2.13** was hardened automatically. 4 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml docker image reference uses a mutable tag `:v1` instead of a SHA digest. This means the image can be silently replaced with a different version, enabling supply-chain attacks. The failing reference is: `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1'`. It should be pinned to a specific SHA digest, e.g. `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:44`

### script-injection (severity: high)

Rule (a): Five `run:` steps in automerge.yml directly interpolate `${{ github.event.pull_request.html_url }}` into shell commands. Because YAML template substitution happens before the shell sees the string, an attacker who controls the PR URL could inject arbitrary shell commands. Offending lines:
- `run: gh pr review --approve ${{ github.event.pull_request.html_url }}` (dependabot/Approve request, line 25)
- `run: gh pr merge --rebase --auto ${{ github.event.pull_request.html_url }}` (dependabot/Enable automerge, line 31)
- `run: gh pr review --approve ${{ github.event.pull_request.html_url }}` (renovate/Approve request, line 39)
- `run: gh pr merge --rebase --auto ${{ github.event.pull_request.html_url }}` (renovate/Enable automerge, line 45)
- `run: gh pr merge --rebase --auto ${{ github.event.pull_request.html_url }}` (cbrgm/Enable automerge, line 54)
Fix: move the URL into an env var and pass it as a quoted shell variable.

Locations:

- `.github/workflows/automerge.yml:25`
- `.github/workflows/automerge.yml:31`
- `.github/workflows/automerge.yml:39`
- `.github/workflows/automerge.yml:45`
- `.github/workflows/automerge.yml:54`

### script-injection (severity: high)

Rule (a): The 'Publish Git Tag' step in tag.yml directly interpolates `${{ steps.bump-semver.outputs.new_version }}` into a `run:` block: `new_tag=${{ steps.bump-semver.outputs.new_version }}`. The `steps.*.outputs.*` context is substituted by the YAML template engine before the shell executes, allowing a compromised or malicious upstream action to inject arbitrary shell commands via the output value. Fix: move the value into an `env:` variable and reference it as a quoted shell variable (`"$NEW_TAG"`).

Locations:

- `.github/workflows/tag.yml:57`

### missing-permissions (severity: medium)

The workflow file stale-branches.yml has no top-level `permissions:` key and its only job (`cleanup-stale-branches`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal permissions block (e.g. `permissions: contents: read`) should be added.

Locations:

- `.github/workflows/stale-branches.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

1. action.yml: Pinned docker image from 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1' to 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1@sha256:ab8395a8fa08600ebcc0752e2aa345f7dd2f0b8f612f2014171e04311f37498f'. 2. automerge.yml: Fixed all 5 script injection instances by moving github.event.pull_request.html_url into env var PR_URL and referencing it as "$PR_URL" in shell commands. 3. tag.yml: Fixed script injection by moving steps.bump-semver.outputs.new_version into env var NEW_VERSION and referencing it as "$NEW_VERSION" in the shell script. 4. stale-branches.yml: Added top-level 'permissions: contents: write' block (needed for the action to delete stale branches).

### Iteration 2

**Fixes applied:** github-env-injection

**Notes:**

Fixed the github-env-injection finding in .github/workflows/tag.yml at line 46. The `latest_tag` value (derived from `git tag | sort -V | tail -n 1`) is now sanitized before being written to $GITHUB_ENV. Added `safe_latest_tag=$(printf '%s' "$latest_tag" | tr -d '\n\r')` to strip newline and carriage return characters that could be used to inject additional key=value pairs into the runner environment. The sanitized value is then written to $GITHUB_ENV instead of the raw value. Also quoted $GITHUB_ENV as "$GITHUB_ENV" for best practice.

