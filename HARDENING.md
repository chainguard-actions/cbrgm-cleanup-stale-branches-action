<!-- markdownlint-disable -->

# Hardening Report: cbrgm--cleanup-stale-branches-action/v1.2.15

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cbrgm--cleanup-stale-branches-action/v1.2.15** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The Docker action image is referenced by the mutable tag 'v1' instead of a SHA256 digest: `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1'`. To fix this, pin it to ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:<digest> # v1.

Locations:

- `action.yml:43`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned runs.image to docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1@sha256:fbb4c85e7e83af78681f87b807a696df7d28eaaffed7bffbb7fdb5aba016a8df (digest matches both the v1 and v1.2.15 tags). Docker args left unchanged.

