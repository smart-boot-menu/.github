# Contributing to Smart Boot Menu

Thank you for contributing to Smart Boot Menu!

## Guiding Principles

1. **Safety first**: Never perform silent destructive disk, NVRAM, or boot-policy modifications.
2. **Offline-first**: Every local capability must work without internet access, an account, or `sbm-server`.
3. **Single source of truth**: Shared specifications live in `sbm/docs/specifications/`; shared Rust types live in `sbm-common`. Do not duplicate protocol definitions across repositories.
4. **Evidence over assumption**: Benchmark real hardware/VM latency, cite primary sources for firmware/OS behavior, and distinguish verified behavior from experimental proposals.

## Workflow

1. Read `AGENTS.md` and `README.md` in the target repository, plus the central docs in [`smart-boot-menu/sbm`](https://github.com/smart-boot-menu/sbm).
2. Create a topic branch from `main` (never force-push `main`).
3. Ensure formatting, linting, and tests pass (`cargo fmt --check`, `cargo clippy --all-targets`, `cargo test`, plus `no_std` UEFI target checks where applicable).
4. Submit a pull request referencing the relevant issue, specification, or ADR.

## Changing Shared Interfaces

Breaking or additive changes to shared schemas, wire protocols, or data types MUST follow the interface change process:
1. Open a specification PR or ADR in `smart-boot-menu/sbm`.
2. Update canonical Rust types, JSON schemas, and test vectors in `smart-boot-menu/sbm-common`.
3. Bump protocol/schema version fields according to compatibility negotiation rules.
4. Update downstream repositories (`sbm-uefi`, `sbm-core`, `sbm-server`, `sbm-agent`, `sbm-catalog`, `install`) and validate end-to-end in `sbm-lab`.

## Licensing

By contributing original code to Smart Boot Menu, you agree that your contributions are dual-licensed under the MIT License and the Apache License, Version 2.0 (`MIT OR Apache-2.0`), unless a repository explicitly specifies third-party license constraints.
