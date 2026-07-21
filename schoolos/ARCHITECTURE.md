# SchoolOS Architecture

## 1. Scope

SchoolOS is a managed endpoint operating system for school-owned laptops and desktops. The complete product consists of a device operating system, signed software repositories, a device-management service and administrative interfaces.

The Linux kernel alone is not the operating system product. SchoolOS adds the userspace, installer, desktop, policy engine, update system, recovery environment and central management plane required for deployment in schools.

## 2. Architectural layers

```text
┌───────────────────────────────────────────────────────────────┐
│ Tenvra School Hub                                             │
│ Organization, identity, policy, inventory, audit, deployment  │
└───────────────────────────┬───────────────────────────────────┘
                            │ HTTPS + mTLS
┌───────────────────────────▼───────────────────────────────────┐
│ Device management services                                   │
│ Enrollment, policy, updates, classroom, exam, recovery        │
├───────────────────────────────────────────────────────────────┤
│ Desktop and applications                                      │
│ KDE Plasma, browser, office, accessibility, curated apps      │
├───────────────────────────────────────────────────────────────┤
│ Base operating system                                         │
│ Debian Stable, systemd, APT, Flatpak, AppArmor, Btrfs          │
├───────────────────────────────────────────────────────────────┤
│ Kernel and boot chain                                          │
│ UEFI, Secure Boot, signed kernel, initramfs, hardware drivers  │
└───────────────────────────────────────────────────────────────┘
```

## 3. Device components

### 3.1 Device agent

The device agent is the primary authenticated client of School Hub. It must:

- enroll a device and generate or receive a device identity;
- maintain a mutually authenticated connection to School Hub;
- retrieve signed policy assignments;
- report minimal health and inventory data;
- coordinate application, configuration and update jobs;
- expose no network listener unless strictly required;
- operate with narrowly scoped privileges.

The initial implementation should use Rust and split privileged operations from unprivileged networking and parsing.

### 3.2 Policy engine

The policy engine converts centrally assigned declarative policy into local configuration. Policies must be:

- versioned;
- signed;
- schema validated;
- idempotent;
- reversible where practical;
- assigned with explicit organizational scope;
- cached for offline operation.

A policy is desired state, not an arbitrary remote shell command.

### 3.3 Update agent

The update agent coordinates operating-system updates and SchoolOS component updates. It must support staged rollout, maintenance windows, health checks and rollback.

Initial releases may use APT packages and Btrfs snapshots. A future immutable or image-based update model can be evaluated after the MVP.

### 3.4 Exam agent

The exam agent activates a signed, time-bounded examination profile. It may restrict applications, domains, removable storage, clipboard behavior and secondary displays when policy allows.

Exam mode must fail safely. Loss of connectivity must not silently remove restrictions, and an expired or revoked exam policy must not permanently lock a device.

### 3.5 Recovery agent

Recovery provides a local and remotely initiated path to restore a known-good system. Destructive actions require explicit authorization, audit logging and protection against replay.

## 4. School Hub components

### 4.1 Management API

The management API owns organization, school, group, device, policy, deployment and audit resources. APIs must be versioned and enforce tenant boundaries at every request.

### 4.2 Device gateway

The device gateway terminates mutually authenticated device sessions. It validates device certificates, checks revocation and exposes only device-specific operations.

### 4.3 Administrative dashboard

The dashboard provides scoped management for municipality, school and IT roles. High-risk operations require step-up authentication and clear confirmation.

### 4.4 Data layer

PostgreSQL is the initial system of record. Tenant identifiers must be present in all tenant-owned records and enforced through application authorization plus database-level controls where possible.

## 5. Identity model

SchoolOS separates four identities:

1. **Device identity** — hardware-bound or securely stored certificate used for management communication.
2. **Operating-system user identity** — local or federated login session on the device.
3. **School identity** — user identity supplied by the municipality or school identity provider.
4. **Administrative identity** — strongly authenticated identity with scoped School Hub privileges.

These identities must not be treated as interchangeable.

## 6. Trust boundaries

Primary trust boundaries are:

- firmware and Secure Boot to bootloader;
- bootloader to signed kernel and initramfs;
- kernel to privileged system services;
- privileged service to desktop session;
- device to School Hub;
- one tenant to another tenant;
- administrator to privileged operation;
- normal learning mode to examination mode.

Every crossing requires authentication, authorization, validation and auditable failure behavior.

## 7. Data flows

### Enrollment

1. Device boots a trusted SchoolOS image.
2. Administrator supplies a short-lived enrollment token.
3. Device generates a key pair locally.
4. School Hub validates the token and issues a device certificate.
5. Device retrieves its first signed baseline policy.

### Policy delivery

1. Administrator publishes a policy version.
2. School Hub resolves assignments by organization, school, group and device.
3. Device retrieves the policy through mTLS.
4. Device validates signature, schema, scope and version.
5. Policy engine applies changes and reports a result code.

### Update rollout

1. Release is signed and published.
2. IT assigns it to a deployment ring.
3. Device creates a recovery point.
4. Update installs during the allowed window.
5. Post-boot health checks confirm success or trigger rollback.

## 8. Deployment rings

- `development`: engineering devices and virtual machines;
- `canary`: small internal or pilot fleet;
- `pilot`: selected classrooms or schools;
- `stable`: broad production deployment;
- `extended`: slower-moving compatibility channel when required.

## 9. Initial repository boundaries

- Upstream kernel source remains at repository root.
- Distribution-specific work lives in `schoolos/`.
- Kernel configuration lives in `schoolos/kernel/configs/`.
- Product kernel patches live in `schoolos/kernel/patches/` until a dedicated maintained kernel branch is established.
- Secrets and private signing material are never stored in Git.

## 10. Architecture decision records

Material decisions must be captured under `schoolos/docs/architecture/decisions/` using numbered ADR files. An ADR states context, decision, alternatives, consequences and status.