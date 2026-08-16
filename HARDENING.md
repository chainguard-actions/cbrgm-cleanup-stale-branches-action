<!-- markdownlint-disable -->

# Hardening Report: cbrgm--cleanup-stale-branches-action/v1.2.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cbrgm--cleanup-stale-branches-action/v1.2.7** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple unpinned action/image references found:

1. action.yml: `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1'` uses a mutable tag (v1) instead of a SHA digest (e.g. `@sha256:<64-hex-char-digest>`). A tag can be silently moved to point to a different image.

2. .github/workflows/stale-branches.yml: `uses: cbrgm/cleanup-stale-branches-action@main` — branch ref, not a pinned commit SHA.

3. .github/workflows/stale.yml: `uses: cbrgm/cleanup-stale-branches-action@main` — branch ref, not a pinned commit SHA.

4. .github/workflows/tag.yml: `uses: cbrgm/semver-bump-action@main` — branch ref, not a pinned commit SHA.

Locations:

- `action.yml:35`
- `.github/workflows/stale-branches.yml:12`
- `.github/workflows/stale.yml:32`
- `.github/workflows/tag.yml:36`

### script-injection (severity: high)

GitHub Actions expressions are interpolated directly inside `run:` shell command strings (sub-rule a), allowing an attacker to inject arbitrary shell commands.

1. automerge.yml — five `run:` steps directly embed `${{ github.event.pull_request.html_url }}` into `gh pr review` and `gh pr merge` commands. A malicious PR URL (e.g. containing shell metacharacters) would be executed verbatim by the shell. Affected steps: 'Approve request' (dependabot job, line 24), 'Enable automerge' (dependabot job, line 30), 'Approve request' (renovate job, line 38), 'Enable automerge' (renovate job, line 44), 'Enable automerge' (cbrgm job, line 52). Fix: pass the URL via an env var and double-quote the expansion.

2. tag.yml — the 'Publish Git Tag' run block contains `new_tag=${{ steps.bump-semver.outputs.new_version }}`, directly interpolating a step output into the shell script. The value is then used unquoted in `git tag $new_tag` and `git push origin $new_tag`. Fix: route through an env var and double-quote all expansions.

Locations:

- `.github/workflows/automerge.yml:24`
- `.github/workflows/automerge.yml:30`
- `.github/workflows/automerge.yml:38`
- `.github/workflows/automerge.yml:44`
- `.github/workflows/automerge.yml:52`
- `.github/workflows/tag.yml:55`

### missing-permissions (severity: medium)

The workflow file .github/workflows/stale-branches.yml has no top-level `permissions:` key and its only job (`cleanup-stale-branches`) also has no job-level `permissions:` key. Without an explicit permissions block the workflow inherits the repository's default token permissions, which may be broader than necessary (e.g. write access to contents). A minimal permissions block such as `permissions: {}` or scoped to only what the action requires should be added.

Locations:

- `.github/workflows/stale-branches.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings: (1) Pinned docker image in action.yml to sha256 digest, pinned cbrgm/cleanup-stale-branches-action@main in stale-branches.yml and stale.yml to commit SHA 1cd2068354f38284bb05b8ba279ae30790d68c44, and pinned cbrgm/semver-bump-action@main in tag.yml to commit SHA cc89dae95968de9a49b9a4879290be60e1dd5600. (2) Fixed script injection in all 5 automerge.yml run steps by moving github.event.pull_request.html_url into PR_URL env vars, and fixed tag.yml's Publish Git Tag step by moving steps.bump-semver.outputs.new_version into NEW_VERSION env var with all shell expansions properly double-quoted. (3) Added 'permissions: contents: write' to stale-branches.yml (required for branch deletion).

