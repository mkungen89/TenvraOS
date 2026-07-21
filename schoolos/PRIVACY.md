# SchoolOS Privacy Principles

## 1. Purpose

SchoolOS is intended for educational environments and may be used by minors. Privacy must therefore be designed into the operating system, School Hub and administrative workflows from the beginning.

This document is a product and engineering baseline. It is not a substitute for the controller's legal documentation, records of processing, contracts or data-protection impact assessment.

## 2. Core principles

SchoolOS follows these principles:

- collect only data required for a defined function;
- make administrative capabilities visible and understandable;
- separate device management from content surveillance;
- use role and organizational scope for every access decision;
- retain data only for documented periods;
- protect data in transit and at rest;
- support access, correction, export and deletion workflows;
- avoid storing student identity on devices or in certificates unless required;
- never monetize student or school data through advertising or profiling.

## 3. Default device telemetry

The default device-health record may include:

- device identifier;
- SchoolOS version and update channel;
- hardware model and supported component identifiers;
- storage capacity and health status;
- battery health summary;
- last successful management contact;
- policy version and application result;
- update status and error codes;
- security state such as Secure Boot and disk-encryption status.

The default record must not include:

- document contents or filenames;
- keystrokes;
- microphone or camera recordings;
- message contents;
- screenshots;
- full browsing history;
- passwords or authentication tokens;
- precise location;
- unrelated personal files;
- application content merely because a process crashed.

## 4. Classroom functions

Classroom functions must have a clear educational purpose and a visible active-session state. They must not silently expand into general employee or student monitoring.

Any future screen-viewing or screen-sharing feature requires:

- an explicit architecture and privacy review;
- visible indication on the student device;
- strict class and session scope;
- no automatic recording;
- configurable school policy;
- documented retention behavior;
- a complete administrative audit trail.

## 5. Examination mode

Examination mode may record integrity-relevant events such as policy activation, prohibited device connection or attempted launch of a blocked application. It must not collect more information than needed to evaluate the exam environment.

Logs must distinguish a technical event from an allegation of misconduct. Automated events are evidence for review, not automatic proof of cheating.

## 6. Tenant separation

School Hub data is scoped by municipality or school tenant. Cross-tenant access is forbidden unless a documented support workflow has been explicitly authorized and audited.

Support personnel must use time-limited access with a stated purpose. Production database access must not be the normal support interface.

## 7. Roles and access

Access is granted according to purpose:

- students access their own session and approved learning services;
- teachers access assigned classes and classroom functions;
- school IT accesses managed device and deployment data;
- municipality administrators access delegated organizational configuration;
- Tenvra support accesses only the minimum required support data through audited workflows.

A higher organizational role does not automatically grant access to student content.

## 8. Retention

Retention periods must be configurable by data category. Default periods will be defined before pilot deployment.

At minimum, the system separates:

- current device state;
- historical health events;
- security events;
- administrative audit logs;
- classroom session events;
- examination integrity events;
- support case data.

Deletion must include active systems, indexes and scheduled backup expiration according to the documented retention model.

## 9. Transparency

Schools must be able to explain:

- what SchoolOS collects;
- why each category is collected;
- who can access it;
- how long it is retained;
- which administrative actions are possible;
- whether external processors receive data;
- how a data subject request is handled.

The device should expose a local privacy page showing active management, assigned organization, key data categories and current classroom or exam state.

## 10. Data location and processors

Deployment documentation must identify storage regions, subprocessors and international transfers. Region selection alone does not establish compliance; contracts, access controls, backups, support access and processor terms must also be reviewed.

## 11. Development and testing data

- Production student data must not be copied into development environments.
- Test fixtures use synthetic identities and content.
- Logs and crash reports are scrubbed for secrets and personal content.
- Demo tenants contain no real student information.
- Developers receive no standing production database access.

## 12. Data-protection impact assessment

A DPIA is required before a school pilot where processing is likely to create elevated risk, particularly for examination monitoring, classroom screen functions, behavioral analytics or large-scale processing of minors' data.

The DPIA must be treated as an engineering input. Mitigations and unresolved risks must be tracked in the product backlog and release gates.