<!-- markdownlint-disable -->

# Hardening Report: cbrgm--cleanup-stale-branches-action/v1.2.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cbrgm--cleanup-stale-branches-action/v1.2.9** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image with a mutable tag instead of a SHA digest. `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1'` references the `:v1` tag, which can be silently updated to point to a different (potentially malicious) image. It should be pinned to a specific SHA digest, e.g. `docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:35`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag `ghcr.io/cbrgm/cleanup-stale-branches-action:v1` with the immutable SHA digest `ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:f6ab8ed5a2ca53b8978f38a1b0aac6f0056926d2ed1c089ddd699bd9724593a3` in action.yml line 35. The original `:v1` tag is preserved as a comment for readability.

