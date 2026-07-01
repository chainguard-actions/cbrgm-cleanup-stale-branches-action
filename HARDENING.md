<!-- markdownlint-disable -->

# Hardening Report: cbrgm--cleanup-stale-branches-action/v1.2.11

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **cbrgm--cleanup-stale-branches-action/v1.2.11** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag ('docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1') instead of an immutable SHA digest. This means the image could be silently replaced with a different (potentially malicious) version without any change to the action definition. It should be pinned to a specific SHA digest, e.g. 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:<64-hex-char-digest>'.

Locations:

- `action.yml:37`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from the mutable tag 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action:v1' to the immutable digest 'docker://ghcr.io/cbrgm/cleanup-stale-branches-action@sha256:3fea970bbbd86003acc0d56af312572ee398515332f62aeab7dd06c04cffbdc3' with '# v1' comment for readability.

