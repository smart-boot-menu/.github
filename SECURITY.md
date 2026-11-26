# Security Policy

Smart Boot Menu (SBM) is highly privileged pre-boot and bare-metal recovery software. Security vulnerabilities in any SBM component can affect every operating system and storage device attached to a machine. We take security reports seriously.

## Reporting a Vulnerability

**Do NOT open public GitHub issues for suspected security vulnerabilities.**

Please report vulnerabilities privately via GitHub Security Advisories on the affected repository (or on [`smart-boot-menu/sbm`](https://github.com/smart-boot-menu/sbm/security/advisories/new) if cross-repository).

Include where possible:
- Affected repository, commit hash or release version, and target platform/architecture.
- Clear reproduction steps or proof-of-concept.
- Security impact assessment (e.g., Secure Boot bypass, arbitrary EFI execution, unauthorized disk modification, command replay, privilege escalation).

## Response Targets

- Initial acknowledgment: within 72 hours.
- Triage and severity classification: within 7 days.
- Coordinated disclosure: we aim for a 90-day disclosure window, adjustable by mutual agreement for firmware/boot-chain fixes.

## Canonical Security Documentation

See [`smart-boot-menu/sbm/SECURITY.md`](https://github.com/smart-boot-menu/sbm/blob/main/SECURITY.md) and [`docs/threat-model/`](https://github.com/smart-boot-menu/sbm/tree/main/docs/threat-model) for the full threat model, trust boundaries, and safety invariants.
