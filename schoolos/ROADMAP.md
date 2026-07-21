# SchoolOS Roadmap

This roadmap defines capability milestones rather than fixed release dates. Security, privacy and recoverability are release gates for every phase.

## Phase 0 — Bootstrap

**Goal:** establish product boundaries and an executable development structure.

- [x] Create a dedicated SchoolOS branch.
- [x] Separate distribution work from upstream kernel source.
- [x] Document the initial architecture and security principles.
- [x] Define the product requirements and policy model.
- [x] Add repository structure validation.
- [ ] Record initial architecture decisions as ADRs.
- [ ] Define supported development and build hosts.
- [ ] Select the exact Debian release and snapshot strategy.

## Phase 1 — MVP 0.1: Bootable learning image

**Goal:** produce a reproducible ISO that can be installed and used safely on a test computer.

- Debian Stable base image built with `live-build`.
- UEFI installation and boot.
- KDE Plasma Wayland session with Tenvra branding.
- Restricted student account and separate administrator account.
- Browser, office suite, PDF reader and accessibility baseline.
- AppArmor enabled and enforcing for selected applications.
- Host firewall enabled by default.
- Automatic security updates.
- Btrfs layout with system snapshots.
- Local recovery option.
- Reproducible build metadata and checksums.
- Virtual-machine smoke tests.

### Exit criteria

- A clean build produces an installable image from documented commands.
- The student account cannot obtain persistent administrative privileges through supported UI paths.
- A failed update can be rolled back in a test environment.
- No production credentials or student data are present in the image.

## Phase 2 — MVP 0.2: Device enrollment and policy

**Goal:** allow a SchoolOS device to join a managed fleet.

- Rust device agent skeleton.
- Short-lived enrollment token.
- Per-device certificate and mutual TLS.
- Device inventory with data minimization.
- Signed, versioned baseline policy.
- Local policy cache for offline operation.
- Device groups and deployment rings.
- Audit log for privileged administrative actions.

### Exit criteria

- A fresh device can enroll without a shared fleet password.
- A revoked device can no longer retrieve policy.
- Invalid or unsigned policy is rejected.
- Tenant boundary tests pass.

## Phase 3 — MVP 0.3: Managed applications and configuration

**Goal:** centrally deliver approved software and school configuration.

- Curated application catalog.
- APT repository signing and metadata verification.
- Flatpak allow-list and controlled remotes.
- Wi-Fi, certificate, browser and printer profiles.
- Staged deployments with maintenance windows.
- Installation result reporting.
- Safe failure and retry behavior.

## Phase 4 — MVP 0.4: Teacher and classroom workflows

**Goal:** support classroom actions without creating a surveillance platform.

- Teacher-scoped classroom groups.
- Share approved links and lesson resources.
- Launch an approved application or web resource.
- Temporary focus policy.
- Explicit session indicators on student devices.
- Strong audit and time limits.
- No covert microphone, camera or keylogging capability.

## Phase 5 — MVP 0.5: Examination mode

**Goal:** provide a defensible, time-bounded examination environment.

- Signed exam profiles.
- Application and domain allow-lists.
- Optional removable-storage restrictions.
- Clipboard and screen-capture controls where technically enforceable.
- Offline continuation with preloaded policy.
- Clear entry and exit state.
- Incident and integrity event reporting.
- Recovery path that cannot be used to silently bypass an active exam.

## Phase 6 — MVP 0.6: Enterprise identity and administration

**Goal:** integrate with school and municipality identity systems.

- OpenID Connect and SAML evaluation.
- Role-based access control.
- Municipality, school, class and device-group hierarchy.
- Step-up authentication for destructive actions.
- Delegated administration.
- Privacy and retention controls.
- Export and deletion workflows.

## Phase 7 — MVP 0.7: Resilience and fleet recovery

**Goal:** make broad deployments supportable.

- Recovery image and verified reinstall.
- Remote lock and authorized wipe.
- Update rollback automation.
- Certificate rotation and revocation.
- Lost-device procedure.
- Backup and restore for School Hub configuration.
- Disaster-recovery exercises.

## Phase 8 — MVP 0.8: Hardware certification

**Goal:** establish a supported hardware matrix.

- Reference laptop profiles.
- Wi-Fi, graphics, audio, camera, touchpad and suspend tests.
- Battery-health and power-management validation.
- TPM and Secure Boot compatibility.
- Firmware update process.
- Minimum and recommended hardware requirements.

## Phase 9 — MVP 0.9: School pilot

**Goal:** validate real-world usability and operations with a limited pilot.

- Data-protection impact assessment.
- Support and escalation procedures.
- Teacher, student and IT onboarding.
- Accessibility evaluation.
- Incident-response exercise.
- Measured update success and device reliability.
- Feedback-driven corrections before production release.

## Phase 10 — SchoolOS 1.0

**Goal:** first supported production release.

Release requires:

- documented support lifecycle;
- signed and reproducible release artifacts;
- stable update and rollback process;
- completed threat model;
- completed privacy review;
- independent security testing;
- supported hardware list;
- administrator and end-user documentation;
- incident-response and vulnerability-disclosure process;
- software bill of materials for shipped components.

## Deferred decisions

The following are intentionally deferred until evidence from the MVP exists:

- immutable versus package-based operating-system updates;
- custom Linux kernel as the default production kernel;
- exact cloud or self-hosted School Hub deployment model;
- integration with specific Swedish school platforms;
- mobile-device support;
- classroom screen-viewing functionality;
- commercial licensing and support tiers.