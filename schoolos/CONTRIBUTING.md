# Contributing to Tenvra SchoolOS

## Working agreement

SchoolOS targets managed computers used in educational environments. Changes affecting authentication, device management, student restrictions, classroom functions, examination mode, privacy, updates or recovery require a higher standard of review than ordinary desktop customization.

## Branches

Use focused branches based on `schoolos/bootstrap` during the bootstrap phase:

```text
schoolos/<capability>
agent/<maintenance-task>
kernel/<kernel-purpose>
docs/<documentation-topic>
```

Do not develop SchoolOS features directly on `master`. The root source tree follows Linux kernel development and must remain separable from the SchoolOS distribution work.

## Pull requests

A pull request should state:

- what changed;
- why the change is needed;
- which user or administrator workflow is affected;
- security and privacy impact;
- rollback or migration behavior;
- tests performed;
- documentation changed;
- known limitations.

Draft pull requests are encouraged for architecture and security-sensitive work.

## Change requirements

### Architecture changes

Add or update an ADR when a change affects:

- distribution base;
- boot or update model;
- identity and authentication;
- trust boundaries;
- management protocol;
- policy format;
- storage or tenant model;
- classroom or exam capabilities;
- collection of user or device data.

### New services

Every new service must document:

- purpose and owner;
- privilege level;
- network listeners and outbound connections;
- persistent data;
- configuration format;
- threat surface;
- failure behavior;
- logging and privacy impact;
- update and rollback behavior;
- test strategy.

### Dependencies

Before adding a dependency, evaluate:

- active maintenance;
- security history;
- license compatibility;
- transitive dependency size;
- release cadence;
- reproducible-build impact;
- availability in the selected distribution;
- whether a smaller standard-library solution is reasonable.

## Code standards

- Prefer small components with typed interfaces.
- Treat all network, policy and removable-media input as untrusted.
- Avoid shell command construction from untrusted values.
- Do not weaken TLS, signature or certificate validation for convenience.
- Do not introduce shared administrator passwords.
- Do not store secrets in source control, examples, test fixtures or CI logs.
- Use structured error codes for device-management operations.
- Keep privileged code minimal and separately testable.

## Documentation standards

- Use clear English for technical repository documentation.
- Define acronyms on first use.
- Separate current behavior from proposed behavior.
- Mark security assumptions explicitly.
- Link requirements to implementation and tests when development begins.
- Avoid claiming compliance before legal and technical verification is complete.

## Testing

Run:

```bash
bash schoolos/scripts/validate-structure.sh
```

Component changes must add relevant unit and integration tests. Security-sensitive parsers should add malformed-input tests and fuzzing where practical.

## Kernel changes

Kernel work must follow the Linux kernel contribution, licensing and Developer Certificate of Origin requirements already documented in the root source tree.

SchoolOS-specific kernel patches must:

- have a documented product requirement;
- avoid duplicating functionality available safely in userspace;
- identify upstream status;
- include a maintenance and backport plan;
- be testable on supported hardware;
- remain isolated from the production kernel until reviewed.

## Security reports

Do not open public issues for unpatched vulnerabilities. A private reporting process will be published before external testing.