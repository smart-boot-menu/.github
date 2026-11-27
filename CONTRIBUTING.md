# Contributing to Smart Boot Menu

Thank you for contributing to Smart Boot Menu!

## Guiding Principles

1. **Reuse before building**: Do not recreate inferior versions of existing software. Follow the organization-wide **Open-Source Reuse & Engineering Principles** in [`.github/AGENTS.md`](AGENTS.md) and [`ADR-010`](https://github.com/smart-boot-menu/docs/blob/main/architecture-decisions/ADR-010-open-source-reuse-and-engineering-principles.md): prefer reusing, composing, wrapping, or contributing upstream over writing original implementations, and provide proportional build-versus-reuse justification for new subsystems, major dependencies, architectural changes, or replacements.
2. **Safety first**: Never perform silent destructive disk, NVRAM, or boot-policy modifications.
3. **Offline-first**: Every local capability must work without internet access, an account, or `sbm-server`.
4. **Single source of truth**: Shared specifications and ADRs live in [`smart-boot-menu/docs`](https://github.com/smart-boot-menu/docs); primary-source research and empirical evidence live in [`smart-boot-menu/research`](https://github.com/smart-boot-menu/research); shared Rust types and schemas live in [`smart-boot-menu/sbm-common`](https://github.com/smart-boot-menu/sbm-common). Do not duplicate policies or protocol definitions across repositories.
5. **Evidence over assumption**: Benchmark real hardware/VM latency, cite primary sources for firmware/OS behavior, and distinguish verified behavior from experimental proposals.

## Workflow

1. Read [`.github/AGENTS.md`](AGENTS.md), `AGENTS.md`, and `README.md` in the target repository, plus the central docs in [`smart-boot-menu/docs`](https://github.com/smart-boot-menu/docs) and [`smart-boot-menu/sbm`](https://github.com/smart-boot-menu/sbm).
2. For new components, major dependencies, architectural changes, or replacements of established software, complete a proportional build-versus-reuse evaluation per [`agent-reference.md`](agent-reference.md#4-proportional-build-vs-reuse-evaluation).
3. Create a topic branch from `main` (never force-push `main`).
4. Ensure formatting, linting, and tests pass (`cargo fmt --check`, `cargo clippy --all-targets`, `cargo test`, plus `no_std` UEFI target checks where applicable).
5. Submit a pull request referencing the relevant issue, specification, or ADR.

## Changing Shared Interfaces

Breaking or additive changes to shared schemas, wire protocols, or data types MUST follow the interface change process:
1. Open a specification PR or ADR in `smart-boot-menu/docs`.
2. Update canonical Rust types, JSON schemas, and test vectors in `smart-boot-menu/sbm-common`.
3. Bump protocol/schema version fields according to compatibility negotiation rules.
4. Update downstream repositories (`sbm-uefi`, `sbm-core`, `sbm-server`, `sbm-agent`, `sbm-catalog`, `install`) and validate end-to-end in `sbm-lab`.

## Licensing

By contributing original code to Smart Boot Menu, you agree that your contributions are dual-licensed under the MIT License and the Apache License, Version 2.0 (`MIT OR Apache-2.0`), unless a repository explicitly specifies third-party license constraints.
