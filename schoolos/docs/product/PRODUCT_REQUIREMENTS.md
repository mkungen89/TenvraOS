# SchoolOS Product Requirements

## 1. Product statement

Tenvra SchoolOS is a Linux-based operating system for school-owned computers. It provides a focused study environment, centrally managed security and software, role-based classroom functionality and a recoverable device lifecycle.

## 2. Primary users

- Students using assigned or shared computers.
- Teachers leading lessons and examinations.
- School IT personnel supporting devices and applications.
- Municipality or organization administrators governing multiple schools.
- Tenvra engineering and support personnel operating the platform.

## 3. Functional requirements

### 3.1 Installation and first boot

- `FR-INSTALL-001`: The project shall produce a bootable UEFI ISO.
- `FR-INSTALL-002`: The installer shall support a documented reference partition layout.
- `FR-INSTALL-003`: Installation shall offer encrypted persistent storage.
- `FR-INSTALL-004`: First boot shall establish device ownership and management state.
- `FR-INSTALL-005`: A managed installation shall require a short-lived enrollment credential rather than a permanent fleet password.
- `FR-INSTALL-006`: The system shall expose a recovery path from the boot environment.

### 3.2 Student environment

- `FR-STUDENT-001`: Students shall not receive persistent administrative privileges.
- `FR-STUDENT-002`: The desktop shall present approved learning applications and school resources.
- `FR-STUDENT-003`: The system shall support accessibility settings without requiring administrator access.
- `FR-STUDENT-004`: The system shall remain usable with the last valid policy during temporary network loss.
- `FR-STUDENT-005`: Student configuration shall be restorable without reinstalling the complete device.
- `FR-STUDENT-006`: Shared-device sessions shall support removal of temporary local user data according to school policy.

### 3.3 Teacher environment

- `FR-TEACHER-001`: Teachers shall access only assigned classes or groups.
- `FR-TEACHER-002`: Teachers shall be able to distribute approved links and resources.
- `FR-TEACHER-003`: Teachers shall be able to request a temporary classroom focus policy.
- `FR-TEACHER-004`: Teacher classroom rights shall not grant operating-system administrator rights.
- `FR-TEACHER-005`: Classroom sessions shall have a visible start, active state and end.

### 3.4 Examination mode

- `FR-EXAM-001`: Examination profiles shall be signed, versioned and time-bounded.
- `FR-EXAM-002`: Examination profiles shall support application allow-lists.
- `FR-EXAM-003`: Examination profiles shall support network-domain allow-lists.
- `FR-EXAM-004`: Examination profiles shall support configurable removable-storage restrictions.
- `FR-EXAM-005`: Active examination restrictions shall continue during temporary management-service outages.
- `FR-EXAM-006`: The system shall clearly indicate when examination mode is active.
- `FR-EXAM-007`: Examination events shall be presented as technical observations, not automatic misconduct determinations.

### 3.5 Device management

- `FR-MGMT-001`: Administrators shall organize devices by tenant, school, group and deployment ring.
- `FR-MGMT-002`: Every managed device shall use a unique device identity.
- `FR-MGMT-003`: Administrators shall assign versioned policy to groups and devices.
- `FR-MGMT-004`: Devices shall report policy application success or a structured failure code.
- `FR-MGMT-005`: Administrators shall deploy approved applications and updates.
- `FR-MGMT-006`: Administrators shall revoke a lost or compromised device.
- `FR-MGMT-007`: Destructive actions shall require elevated confirmation and audit logging.
- `FR-MGMT-008`: School Hub shall provide an inventory view using minimized device data.

### 3.6 Applications

- `FR-APP-001`: Only approved package repositories and Flatpak remotes shall be enabled by default.
- `FR-APP-002`: Schools shall be able to curate applications by role or group.
- `FR-APP-003`: Application deployments shall support staged rollout and failure reporting.
- `FR-APP-004`: Application removal shall not delete user-created documents unless explicitly stated and authorized.

### 3.7 Updates and recovery

- `FR-UPDATE-001`: System releases shall be cryptographically signed.
- `FR-UPDATE-002`: Updates shall support development, canary, pilot and stable rings.
- `FR-UPDATE-003`: High-risk updates shall create a recovery point before installation.
- `FR-UPDATE-004`: Post-update health checks shall confirm success.
- `FR-UPDATE-005`: Failed system updates shall support automatic or administrator-initiated rollback.
- `FR-UPDATE-006`: Administrators shall be able to pause a problematic rollout.

## 4. Non-functional requirements

### Security

- `NFR-SEC-001`: Production devices shall use a signed boot chain.
- `NFR-SEC-002`: Device management shall use mutually authenticated encrypted transport.
- `NFR-SEC-003`: Policies and releases shall be signed and protected against downgrade.
- `NFR-SEC-004`: Tenant isolation shall be covered by automated tests.
- `NFR-SEC-005`: Production images shall expose no undocumented inbound network service.

### Privacy

- `NFR-PRIV-001`: Default telemetry shall exclude document content, keystrokes, screenshots and full browsing history.
- `NFR-PRIV-002`: Data collection shall have a documented purpose and retention category.
- `NFR-PRIV-003`: Administrative support access shall be scoped, time-limited and audited.
- `NFR-PRIV-004`: Classroom management shall not include covert recording or keylogging.

### Reliability

- `NFR-REL-001`: The desktop shall boot into a usable state after an interrupted ordinary update.
- `NFR-REL-002`: The last valid policy shall remain available offline.
- `NFR-REL-003`: Policy application shall be idempotent.
- `NFR-REL-004`: Management-service failure shall not block local learning applications that do not require the service.

### Performance

- `NFR-PERF-001`: Hardware targets and boot-performance budgets shall be defined before MVP hardware certification.
- `NFR-PERF-002`: The device agent shall have explicit memory, CPU, wake-up and network budgets.
- `NFR-PERF-003`: Background management operations shall avoid disrupting active lessons.

### Accessibility

- `NFR-A11Y-001`: Core login, desktop, settings and recovery workflows shall be keyboard accessible.
- `NFR-A11Y-002`: Screen reader, scaling, contrast and input assistance shall be supported.
- `NFR-A11Y-003`: Accessibility preferences shall not require repeated administrator approval.

### Maintainability

- `NFR-MAINT-001`: Build and release procedures shall be documented and automated.
- `NFR-MAINT-002`: Configuration formats and management APIs shall be versioned.
- `NFR-MAINT-003`: Architecture-changing decisions shall be recorded as ADRs.
- `NFR-MAINT-004`: New components shall declare ownership, license, threat surface and test strategy.

## 5. Explicit non-goals for the first MVP

- Building a new general-purpose Linux distribution entirely from source.
- Replacing the stable production kernel with the current mainline release candidate.
- Supporting personal unmanaged devices.
- Implementing covert monitoring functions.
- Supporting every laptop model.
- Integrating every Swedish school platform before the core device lifecycle works.
- Guaranteeing a fully cheat-proof examination environment.

## 6. MVP 0.1 acceptance criteria

MVP 0.1 is accepted when:

1. A documented clean build creates a bootable ISO.
2. The ISO installs on the selected reference VM and at least one reference laptop.
3. KDE Plasma starts in Wayland mode with Tenvra branding.
4. A student account can complete normal learning tasks without administrative access.
5. Security updates are enabled and verifiable.
6. AppArmor and the host firewall are enabled.
7. The system creates and restores a tested recovery point.
8. The build records source revision, package manifest and image checksum.
9. Automated smoke tests run in CI or a documented external build runner.
10. The image contains no embedded secrets or real user data.