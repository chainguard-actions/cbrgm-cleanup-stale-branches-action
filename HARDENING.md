<!-- markdownlint-disable -->

# Hardening Report: cbrgm--cleanup-stale-branches-action/v1.2.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cbrgm--cleanup-stale-branches-action/v1.2.7** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml `runs.image:` field references the Docker image `docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1` using a mutable version tag (`:v1`) instead of an immutable SHA digest. This means the action could silently pull a different (potentially malicious) image if the tag is moved. It should be pinned to a specific SHA digest, e.g. `docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:36`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1' to 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:f6ab8ed5a2ca53b8978f38a1b0aac6f0056926d2ed1c089ddd699bd9724593a3' # v1. The SHA digest was resolved via the Docker Registry HTTP API v2.

