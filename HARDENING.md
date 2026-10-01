<!-- markdownlint-disable -->

# Hardening Report: cbrgm--cleanup-stale-branches-action/v1.2.14

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cbrgm--cleanup-stale-branches-action/v1.2.14** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action's Docker image reference uses a mutable tag `:v1` instead of an immutable SHA digest. This means the image can be silently replaced with a different (potentially malicious) version without any change to the action.yml file, creating a supply-chain risk. The failing reference is: `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1'`. It should be pinned to a full SHA256 digest, e.g. `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:43`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from `docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1` to `docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1@sha256:e0c9d2760d7ef3fa37768116552326a8797fac0e939aed1223d16e9882575a56`. The `docker://` scheme and `:v1` tag are preserved for readability while the SHA256 digest ensures the reference is immutable.

