<!-- markdownlint-disable -->

# Hardening Report: cbrgm--cleanup-stale-branches-action/v1.2.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cbrgm--cleanup-stale-branches-action/v1.2.6** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag (':v1') instead of an immutable SHA digest. This means the action could silently pull a different (potentially malicious) image on future runs. The failing reference is: `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1'`. It should be pinned to a full SHA256 digest, e.g. `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:33`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag ':v1' with the immutable SHA256 digest in action.yml. The image reference is now: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:f6ab8ed5a2ca53b8978f38a1b0aac6f0056926d2ed1c089ddd699bd9724593a3' # v1

