<!-- markdownlint-disable -->

# Hardening Report: cbrgm--cleanup-stale-branches-action/v1.2.8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cbrgm--cleanup-stale-branches-action/v1.2.8** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image referenced by a mutable tag (':v1') instead of an immutable SHA digest. This means the image could be silently replaced with a different (potentially malicious) version. The failing reference is: `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1'`. It should be pinned to a full SHA256 digest, e.g. `image: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:36`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag ':v1' in action.yml line 36 with the immutable SHA256 digest. Updated reference: 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:f6ab8ed5a2ca53b8978f38a1b0aac6f0056926d2ed1c089ddd699bd9724593a3' # v1. The comment '# v1' is placed outside the YAML quotes to preserve readability while ensuring the image is pinned to an immutable digest.

