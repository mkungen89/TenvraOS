# Tenvra SchoolOS

Tenvra SchoolOS is a secure, centrally managed Linux-based operating system for school-owned computers. It is designed for students, teachers and school IT administrators.

> This repository currently contains the upstream Linux kernel source tree. SchoolOS distribution work lives under `schoolos/` so that operating-system product development remains separated from kernel development.

## Product goals

- Provide a focused and accessible learning environment.
- Minimize local administrator access and accidental system damage.
- Support centrally managed devices, applications, certificates, Wi-Fi and updates.
- Provide separate student, teacher, examination and administrator policy profiles.
- Protect student privacy through data minimization and transparent administration.
- Support secure rollback and recovery when an update or configuration fails.
- Remain usable during temporary internet outages.

## Initial technical direction

| Layer | Initial choice |
|---|---|
| Distribution base | Debian Stable |
| Desktop | KDE Plasma on Wayland |
| Filesystem | Btrfs with snapshots |
| Mandatory access control | AppArmor |
| Application delivery | APT plus curated Flatpak applications |
| Device agent | Rust |
| Management API | Versioned HTTPS API with mutual TLS for devices |
| Management backend | PostgreSQL-backed Tenvra School Hub |
| Image construction | Debian live-build |
| Production kernel | Stable signed distribution kernel initially |
| Experimental kernel | Tenvra kernel branch maintained separately |

These choices are architecture defaults, not irreversible commitments. Changes must be recorded as architecture decisions.

## Repository structure

```text
schoolos/
├── docs/              Product and architecture documentation
├── build/             ISO, installer and recovery-image construction
├── desktop/           Branding and desktop configuration
├── packages/          Tenvra package definitions
├── services/          Device-side services and agents
├── policies/          Student, teacher, exam and admin policy profiles
├── hub/               Central management platform contracts and components
├── scripts/           Development and validation utilities
└── tests/             Unit, integration, security and hardware tests
```

## Supported roles

### Student

A restricted account with approved applications, learning resources, accessibility features and no permanent administrative privileges.

### Teacher

A classroom-oriented account that can distribute approved material and activate centrally authorized classroom or examination policies. Teacher access must not bypass school privacy controls.

### IT administrator

A privileged School Hub role for fleet configuration, software deployment, certificate lifecycle, recovery, security response and audit review.

### School or municipality administrator

A scoped administrative role for organizational policy, data governance and delegated access. It must not automatically grant unrestricted device access.

## Security principles

1. Secure by default.
2. Least privilege.
3. Signed software and configuration.
4. Device identity instead of shared secrets.
5. Recoverability and safe rollback.
6. Privacy by design and by default.
7. Explicit auditability of privileged actions.
8. No hidden surveillance functionality.

See [`SECURITY.md`](SECURITY.md) and [`PRIVACY.md`](PRIVACY.md).

## Bootstrap status

The bootstrap phase defines the product boundaries, directory structure, security model and validation rules. It does not yet build an installable ISO.

The first executable milestone is SchoolOS MVP 0.1: a reproducible Debian-based image with Tenvra branding, restricted student accounts, automatic security updates, local recovery and initial device enrollment.

## Development workflow

1. Work from a SchoolOS feature branch.
2. Keep kernel patches isolated under `schoolos/kernel/patches/` or a dedicated kernel branch.
3. Add or update documentation whenever behavior or trust boundaries change.
4. Run `bash schoolos/scripts/validate-structure.sh` before opening a pull request.
5. Never commit production credentials, signing keys or student data.

## License

Kernel source remains governed by its existing licenses. New SchoolOS components must declare their license explicitly before release. Third-party package licenses must be tracked in the future software bill of materials.