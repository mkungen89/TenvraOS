# ADR 0001: Use Debian Stable as the initial SchoolOS distribution foundation

- **Status:** Accepted for bootstrap and MVP 0.1
- **Date:** 2026-07-21
- **Decision owners:** Tenvra SchoolOS maintainers

## Context

The repository contains a mainline Linux kernel source tree. A complete school operating system additionally requires a maintained userspace, package ecosystem, installer, desktop, security updates, hardware support and a reproducible image-building process.

Building all userspace components directly from source would substantially increase security, maintenance and release risk before the product requirements have been validated.

SchoolOS needs a conservative production foundation while retaining the ability to test and maintain Tenvra-specific kernel configurations or patches separately.

## Decision

SchoolOS MVP 0.1 will:

- use Debian Stable as its base distribution;
- use Debian `live-build` for the first image-building pipeline;
- use KDE Plasma on Wayland as the initial desktop;
- use the stable signed distribution kernel for production images;
- keep experimental Tenvra or mainline kernel work isolated from production images;
- use APT for system packages and curated Flatpak applications where sandboxing and lifecycle requirements make Flatpak appropriate;
- use Btrfs snapshots as the first rollback mechanism, subject to installer validation.

## Consequences

### Positive

- Security updates and package maintenance are inherited from a mature distribution.
- The team can focus on school workflows, management, privacy and recoverability.
- Existing Linux hardware support and packaging tools reduce MVP risk.
- Kernel experimentation remains possible without forcing unstable kernels onto school devices.

### Negative

- SchoolOS inherits Debian package versions and release cadence.
- KDE customization and package selection require ongoing integration testing.
- A package-based mutable system may be harder to recover than a future immutable image model.
- Debian branding and licensing requirements must be handled correctly.

## Alternatives considered

### Build a distribution directly from the kernel repository

Rejected for MVP because the kernel does not provide the userspace, installer, package lifecycle or security-maintenance infrastructure needed for a complete product.

### Ubuntu LTS

Viable, but not selected for the initial architecture because SchoolOS currently favors a distribution-neutral, community-based Debian foundation. This can be revisited if hardware enablement or enterprise lifecycle evidence supports a change.

### Fedora or an immutable Fedora variant

Strong security and image-based options, but faster release cadence and different management tradeoffs increase initial product risk.

### Arch Linux

Rejected for production school devices because its rolling-release model conflicts with the initial stability and staged-support goals.

### Buildroot or Yocto

Excellent for appliances and tightly controlled embedded systems, but a general school desktop, broad application catalog and laptop hardware support would require significantly more integration work.

## Revisit criteria

Revisit this decision when:

- MVP hardware compatibility cannot be achieved;
- update rollback proves unreliable;
- an immutable image model demonstrates lower operational risk;
- support-lifecycle requirements exceed the selected Debian release model;
- package availability blocks required educational applications.