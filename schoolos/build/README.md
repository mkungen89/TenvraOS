# SchoolOS Build System

## Objective

The build system will produce reproducible, signed and testable SchoolOS installation and recovery images from pinned source inputs.

The first implementation target is Debian `live-build`. The build must use a stable Debian release and a controlled package snapshot or documented mirror strategy.

## Planned layout

```text
build/
├── live-build/
│   ├── auto/
│   └── config/
├── installer/
├── recovery/
├── packages/
├── manifests/
└── output/
```

Generated artifacts under `output/` must not be committed to Git.

## Required build outputs

Every release build must eventually produce:

- installation ISO;
- recovery image or recovery payload;
- SHA-256 checksums;
- package manifest with exact versions;
- source revision metadata;
- software bill of materials;
- build log and test summary;
- signature metadata;
- release-channel metadata.

## Build inputs

Inputs must be explicit and reviewable:

- Debian release and architecture;
- package repositories and signing keys;
- package include and exclude lists;
- SchoolOS package versions;
- kernel package and configuration;
- desktop configuration;
- bootloader configuration;
- installer defaults;
- branding assets;
- policy baseline;
- release channel.

## Build environments

The build should run in a clean, isolated environment such as a dedicated virtual machine or container-capable build runner. Production signing must remain separate from ordinary CI compilation.

GitHub-hosted CI may validate configuration and create development artifacts, but release signing keys must be held in protected signing infrastructure.

## Initial targets

The future `schoolos/Makefile` should expose:

```text
make validate       Validate repository structure and configuration
make image          Build a development ISO
make recovery       Build recovery artifacts
make test-image     Run VM smoke tests
make manifest       Generate package and source manifests
make clean          Remove generated output
```

## Reproducibility requirements

- Pin all distribution and SchoolOS package versions used by a release.
- Record the build container or host image version.
- Normalize timestamps where supported.
- Avoid downloading unversioned scripts during the build.
- Verify all downloaded metadata and artifacts.
- Record unavoidable sources of non-determinism.
- Compare repeated build output before declaring reproducibility.

## Image security checks

Before an image is promoted, automated checks must confirm:

- no private key or production token is embedded;
- no default fleet password exists;
- package signatures are enabled;
- expected Secure Boot components are present;
- the firewall is enabled;
- AppArmor is enabled;
- debug services are disabled;
- the student account has no persistent admin rights;
- management endpoints use production-safe TLS defaults;
- the package manifest matches installed packages.

## First implementation task

Create a minimal Debian live-build configuration that boots KDE Plasma in a virtual machine, includes a non-administrative test student account and generates a package manifest. Device enrollment and production disk encryption are later milestones.