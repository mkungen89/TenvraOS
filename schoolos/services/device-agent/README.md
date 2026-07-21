# SchoolOS Device Agent

## Purpose

The device agent is the local management client for Tenvra School Hub. It maintains device identity, retrieves signed desired state and coordinates narrowly defined local operations.

The device agent is not a general remote shell and must not accept arbitrary commands.

## Planned responsibilities

- device enrollment;
- certificate renewal and revocation handling;
- policy retrieval and verification;
- policy application orchestration;
- update-job coordination;
- minimized inventory and health reporting;
- structured job results;
- offline policy cache;
- local management status for the desktop privacy page.

## Non-responsibilities

The agent must not:

- capture keystrokes;
- record microphone or camera content;
- collect document contents;
- provide unrestricted remote command execution;
- store long-lived administrator credentials;
- make authorization decisions that belong to School Hub;
- silently disable TLS or signature validation.

## Process design

The initial design should separate:

1. `schoolos-agent` — unprivileged network client and state coordinator;
2. `schoolos-policy-helper` — privileged helper with a small, typed operation surface;
3. `schoolos-status` — read-only local status interface for desktop components.

Communication between processes should use a typed local IPC mechanism with peer-credential checks. The privileged helper must not parse complex network payloads directly.

## State machine

```text
UNENROLLED
  → ENROLLING
  → MANAGED
  → DEGRADED_OFFLINE
  → MANAGED

MANAGED
  → REVOKED
  → RECOVERY_REQUIRED
```

Every transition must be explicit and testable. A device with an invalid or revoked identity must not continue receiving new policy.

## Local state

Proposed state locations:

```text
/var/lib/schoolos-agent/       Persistent device state
/var/cache/schoolos-agent/     Bounded policy and update cache
/run/schoolos-agent/           Runtime sockets and transient state
/etc/schoolos-agent/           Non-secret bootstrap configuration
```

Private keys should use TPM-backed storage where supported. File-based fallback keys require restrictive permissions and encrypted storage.

## API properties

The device protocol must provide:

- protocol version negotiation;
- unique request and job identifiers;
- idempotency keys for mutating jobs;
- monotonic policy versions;
- expiry and replay protection;
- bounded payload sizes;
- structured error codes;
- server and client clock-skew handling;
- certificate rotation without full re-enrollment.

## Rust implementation baseline

When implementation begins, use:

- stable Rust toolchain pinned for releases;
- strict clippy and formatting checks;
- minimal dependency set;
- memory-safe cryptographic libraries with active maintenance;
- explicit serialization schemas;
- fuzzing for policy and protocol parsing;
- systemd hardening directives;
- no unsafe Rust without a documented review.

## Initial deliverable

The first executable version will:

1. start as an unprivileged systemd service;
2. expose a local health status;
3. load a development configuration;
4. validate a signed example policy;
5. write no system configuration;
6. include unit tests for state transitions and rejection behavior.

Privileged policy application will be introduced only after the policy schema and threat model are reviewed.