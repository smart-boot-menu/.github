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
