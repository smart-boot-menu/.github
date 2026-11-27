# Smart Boot Menu — Agent Reference Guide

Detailed operational procedures referenced from [`.github/AGENTS.md`](AGENTS.md).

## 1. Shared Interface Change Protocol

Whenever an agent needs to add or modify a shared struct, enum, file format, or wire message:
1. Check `../docs/specifications/` and `../sbm-common/crates/` first to verify whether an existing type already covers the need.
2. Update the normative specification in `smart-boot-menu/docs/specifications/`.
3. Update the canonical Rust types in `smart-boot-menu/sbm-common` (`sbm-types` / `sbm-protocol`), keeping `no_std` + `alloc` compatibility intact.
4. Regenerate JSON Schemas (`cargo run -p sbm-schema -- schemas`) and add/update golden test vectors under `sbm-common/test-vectors/`.
5. Verify ALL checks in `sbm-common`:
   - `cargo fmt --all -- --check`
   - `cargo clippy --workspace --all-targets -- -D warnings`
   - `cargo test --workspace`
   - `cargo check -p sbm-types -p sbm-protocol --no-default-features --target x86_64-unknown-uefi`
   - `cargo check -p sbm-types -p sbm-protocol --no-default-features --target aarch64-unknown-uefi`
6. Bump `ProtocolVersion` / `schema_version` if breaking, and verify end-to-end in `sbm-lab`.

## 2. Standard Rust Validation Checklist

Before committing in any Rust repository:
- `cargo fmt --all -- --check`
- `cargo clippy --all-targets -- -D warnings`
- `cargo test`
- For UEFI crates (`sbm-common`, `sbm-uefi`): cross-check both `x86_64-unknown-uefi` and `aarch64-unknown-uefi`.

## 3. Recording Benchmarks & Hardware Evidence

Never put unverified performance numbers in READMEs or docs. Archive raw measured outputs (with exact git commits, hardware/QEMU configuration, and `N >= 10` runs) in `smart-boot-menu/research/validation/evidence/<component>-<YYYY-MM-DD>.md`, then summarize and link from `docs/`.

## 4. Proportional Build-vs-Reuse Evaluation

Operational procedure for the **Open-Source Reuse & Engineering Principles** in [`.github/AGENTS.md`](AGENTS.md) and [`ADR-010`](https://github.com/smart-boot-menu/docs/blob/main/architecture-decisions/ADR-010-open-source-reuse-and-engineering-principles.md).

### Proportionality Gate

Match investigation depth to complexity, novelty, risk, and long-term maintenance cost:
- **Lightweight / Skip formal study:** Bug fixes, small internal helpers, test additions, and incremental changes inside an already-justified component.
- **Explicit build-versus-reuse evaluation REQUIRED:** New subsystems, major dependencies, architectural changes, new standalone tools/daemons/harnesses, and replacements of established open-source or existing SBM software.

### Evaluation Checklist

Before designing or implementing a component that crosses the proportionality gate:
1. **Cross-Ecosystem Survey:** Search beyond Rust and familiar repositories (inspect C, C++, Python, Go, shell/systemd, and distro tooling where relevant). Inspect actual source code, documentation, known limitations, benchmarks, license compatibility, and upstream maintenance health.
2. **Apply the Order of Preference:** Evaluate options in order before concluding an original implementation is needed:
   1. Reuse directly -> 2. Configure & compose -> 3. Thin integration layer -> 4. Upstream contribution -> 5. Justified fork -> 6. Original implementation.
3. **Compare Against the Strongest Alternatives & TCO:** Measure or document concrete advantages (functionality, performance, energy/memory/storage efficiency, security, reliability, portability, maintainability, UX/DX, architectural simplicity) against the strongest existing alternatives—not weak baselines—factoring in total cost of ownership (dependencies, integration, security updates, long-term maintenance).
4. **Reject Language-Only Rationales:** Confirm the choice is not a Rust rewrite of working software without tangible, verifiable advantages.
5. **Respect Licensing & Safety Invariants:** Confirm `MIT OR Apache-2.0` compatibility (subprocess isolation via `execve` for `GPL-2.0`/`GPL-3.0` CLI tools; no static linking of `GPL`/`AGPL` crates), offline-first operation, and `docs/threat-model/README.md` invariants.
6. **Establish an Empirical Baseline:** When replacing existing software or claiming performance/resource/reliability gains, collect baseline measurements before writing a replacement and distinguish measured evidence from theoretical expectations.
7. **Check Retrospectively (No Sunk-Cost Bias):** When working on existing SBM components, flag or propose replacing/simplifying custom SBM code if a superior open-source alternative is discovered.
8. **Record the Decision:** Save reusable comparative research in `smart-boot-menu/research/`, and record the build-versus-reuse rationale in the component's `IMPLEMENTATION_PLAN.md` (or a new ADR in `smart-boot-menu/docs/architecture-decisions/` for cross-repository architectural choices).
