# SchoolOS Security Baseline

## 1. Purpose

SchoolOS will be deployed on devices used by children, teachers and school personnel. Security controls must protect devices and school systems without creating unnecessary surveillance or collecting data that is not required for operation.

This document defines mandatory baseline requirements. Detailed threat models and component-specific controls will be added as implementation begins.

## 2. Security objectives

SchoolOS must provide:

- a verified boot path from firmware to operating system;
- strong separation between student sessions and privileged services;
- authenticated and encrypted device-management communication;
- signed software, updates and policy;
- safe recovery from failed or malicious configuration;
- tenant isolation in School Hub;
- accountable administrative actions;
- rapid revocation of lost or compromised device identities;
- minimal exposed network services;
- secure defaults that do not depend on end-user configuration.

## 3. Threat categories

The initial threat model includes:

- a student attempting to bypass local restrictions;
- malicious or compromised applications;
- stolen or lost school devices;
- hostile networks and captive portals;
- compromised administrator credentials;
- malicious or malformed policy and update data;
- supply-chain compromise;
- cross-tenant access in School Hub;
- physical access to a powered-off device;
- rollback to a vulnerable operating-system version;
- accidental configuration that makes devices unusable;
- misuse of classroom management functions.

## 4. Boot and platform security

Production releases must support:

- UEFI Secure Boot;
- signed bootloader, kernel and kernel modules;
- verified initramfs construction;
- protection against unauthorized kernel command-line changes where feasible;
- TPM-backed secrets where supported;
- full-disk encryption for persistent user and system data;
- recovery keys managed separately from normal user credentials;
- firmware and boot-order protections documented for supported hardware.

Experimental unsigned kernels are restricted to development devices and must never be distributed through the stable channel.

## 5. Local privilege model

- Students do not receive persistent `sudo`, root or package-management privileges.
- Teacher privileges are distinct from operating-system administration.
- Administrative access is time-bounded whenever practical.
- Privileged SchoolOS services use dedicated users and restrictive systemd sandboxing.
- Network-facing parsing and privileged system modification should be separated into different processes.
- Polkit rules must explicitly allow required actions instead of broad group-based access.
- Debug interfaces are disabled in production images unless a documented support mode is activated.

## 6. Application security

- Only approved package repositories and Flatpak remotes may be configured.
- Package signatures and repository metadata must be verified.
- AppArmor profiles are required for high-risk or internet-facing applications where practical.
- Browser policies must be centrally managed and schema validated.
- Application installation policy is assigned by group or role.
- Unknown executables downloaded by students must not gain system privileges.
- Office macros, browser extensions and development tools require explicit policy decisions.

## 7. Device identity and enrollment

- Enrollment tokens are short-lived, single-purpose and single-use where possible.
- Each device uses a unique asymmetric key pair.
- Private device keys must not be exportable through normal application interfaces.
- Device certificates include no unnecessary student identity data.
- Revocation is checked before policy, command or update access is granted.
- Re-enrollment requires explicit authorization after wipe, ownership transfer or suspected compromise.
- Shared fleet secrets are prohibited.

## 8. Policy security

Policies must be:

- declarative rather than arbitrary shell commands;
- signed by an authorized policy-signing identity;
- versioned and immutable after publication;
- validated against a strict schema;
- scoped to a tenant and assignment target;
- protected against replay and downgrade;
- applied idempotently;
- accompanied by a safe failure mode;
- auditable from creation through device application.

Devices must retain the last known valid policy for offline operation.

## 9. Update security

- Release artifacts are signed using protected release keys.
- Signing keys are never stored in the repository or ordinary CI variables.
- Builds produce checksums, provenance metadata and a software bill of materials.
- Updates support staged deployment rings.
- Security updates may use an emergency rollout path with explicit authorization.
- A device creates a recovery point before high-risk system changes.
- Post-update health checks determine success or rollback.
- Version downgrade requires a documented recovery or security exception.

## 10. Network security

- The host firewall is enabled by default.
- No inbound listener is exposed unless required and documented.
- Device-to-Hub communication uses modern TLS and mutual authentication.
- Certificate validation cannot be disabled through ordinary policy.
- Proxy and certificate-authority configuration is centrally controlled and auditable.
- DNS and web filtering integrations must disclose what data leaves the device.
- Local network discovery is disabled unless a learning or printing workflow requires it.

## 11. School Hub security

- Every tenant-owned record carries a tenant identifier.
- Authorization is enforced server-side for every object operation.
- Administrative roles follow least privilege.
- Destructive and fleet-wide actions require step-up authentication.
- Sensitive actions are written to append-resistant audit storage.
- Sessions use short lifetimes and secure cookie or token handling.
- Secrets are stored in a dedicated secret-management system.
- Rate limits and abuse controls protect public and enrollment endpoints.
- Backup restoration is tested and included in incident procedures.

## 12. Classroom and examination safeguards

SchoolOS must not implement covert microphone activation, covert camera activation, keylogging or invisible screen recording.

Classroom actions must be:

- visible to affected users when appropriate;
- time-bounded;
- restricted to an assigned class or exam session;
- initiated by an authenticated and authorized role;
- logged with purpose and scope;
- automatically removed at session end.

Examination mode requires a documented threat model and must distinguish enforceable restrictions from best-effort controls.

## 13. Logging

Logs are divided into:

1. security and authentication events;
2. device health and update events;
3. administrative audit events;
4. optional classroom session events.

Logs must not contain document contents, keystrokes, browsing contents or credentials. Retention is configurable and purpose-limited.

## 14. Vulnerability management

Before SchoolOS 1.0 the project must provide:

- a security contact;
- a vulnerability-disclosure policy;
- a supported-version policy;
- severity and response targets;
- a process for emergency signing and release;
- dependency and container scanning;
- periodic penetration testing;
- incident communication templates.

## 15. Security release gates

A release cannot enter the stable channel when:

- critical or high-severity known vulnerabilities remain without an approved exception;
- Secure Boot or signature verification is unexpectedly disabled;
- rollback and recovery tests fail;
- tenant-isolation tests fail;
- production secrets are present in build output;
- audit logging for privileged actions is missing;
- the release cannot be reproduced or its source provenance cannot be identified.

## 16. Reporting

Do not disclose an unpatched SchoolOS vulnerability through a public issue. A private reporting channel will be documented before public testing begins.