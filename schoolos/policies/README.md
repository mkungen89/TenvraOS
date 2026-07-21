# SchoolOS Policy Model

SchoolOS policy is declarative desired state delivered by Tenvra School Hub. Policy must never be an unrestricted remote-command channel.

## Policy profiles

Initial profiles are:

- `student`: normal restricted learning environment;
- `teacher`: classroom tools without operating-system administration;
- `exam`: signed and time-bounded examination restrictions;
- `shared-device`: session cleanup and shared-hardware behavior;
- `administrator`: IT-maintenance controls;
- `baseline`: security requirements applied to every managed device.

## Resolution order

Policy is resolved from broad to specific scope:

```text
platform baseline
→ tenant
→ school
→ device group
→ user role
→ temporary session policy
→ emergency security override
```

A more specific policy may override only fields explicitly marked as overridable. Security invariants cannot be weakened by a lower-trust scope.

## Required policy metadata

Every published policy document must include:

```yaml
apiVersion: schoolos.tenvra.io/v1alpha1
kind: DevicePolicy
metadata:
  id: unique-policy-id
  version: 1
  tenantId: tenant-id
  createdAt: RFC3339 timestamp
  notBefore: RFC3339 timestamp
  expiresAt: RFC3339 timestamp or null
  signer: signing-key-id
spec: {}
```

The transport envelope must contain a cryptographic signature. The canonical serialization and signature algorithm will be defined in an ADR before implementation.

## Policy categories

Planned categories include:

- account and session;
- desktop and accessibility;
- browser configuration;
- application allow-list and deployment;
- networking, Wi-Fi, proxy and certificates;
- removable storage;
- printing;
- updates and maintenance windows;
- telemetry and retention;
- classroom session;
- examination mode;
- recovery and support mode.

## Safety rules

- Unknown required fields cause rejection.
- Unknown optional fields are ignored only when the schema permits it.
- Invalid signatures cause rejection.
- Expired policy cannot be newly activated.
- A lower version cannot replace a higher version without an authorized rollback marker.
- Partial application must return structured per-setting status.
- Reapplying the same policy must be safe.
- Every local change must identify its owning policy.
- Removal of a policy must restore the previous managed value or documented default.

## Local storage

Devices retain:

- the last known valid resolved policy;
- the current application state;
- a bounded history of policy result metadata;
- no unnecessary administrator identity or free-form notes.

Cached policy must be integrity protected. Secrets embedded in policy should be avoided; references to separately encrypted secret delivery are preferred.

## Directory plan

```text
policies/
├── schemas/
├── examples/
├── baseline/
├── student/
├── teacher/
├── exam/
├── shared-device/
└── administrator/
```

Schemas and safe examples will be added when the first policy engine prototype starts.