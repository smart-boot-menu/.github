# Smart Boot Menu — Agent Reference Guide

Central operational procedures and repository-specific guardrails referenced from [`.github/AGENTS.md`](AGENTS.md). Individual repositories keep only a minimal pointer `AGENTS.md` back to `.github/AGENTS.md`; all shared and per-repository agent rules live centrally here and in [`sbm/REPOSITORIES.md`](https://github.com/smart-boot-menu/sbm/blob/main/REPOSITORIES.md).

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

For non-Rust repositories (e.g., `smart-boot-menu.github.io`, `docs`, `research`, `sbm`), run the repository's own formatting, linting, static build, and CI checks.

## 3. Recording Benchmarks & Hardware Evidence

Never put unverified performance numbers in READMEs or docs. Archive raw measured outputs (with exact git commits, hardware/QEMU configuration, and `N >= 10` runs) in `smart-boot-menu/research/validation/evidence/<component>-<YYYY-MM-DD>.md`, then summarize and link from `docs/`. Targets listed in `docs/testing/benchmarks.md` are engineering goals, not claims.

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

## 5. Repository-Specific Rules & Guardrails

Read the target repository's `README.md` for local layout and commands. All repositories must enforce [`docs/threat-model/README.md`](https://github.com/smart-boot-menu/docs/blob/main/threat-model/README.md) and follow §1 before changing shared types or wire formats. Additional non-obvious per-repository guardrails live here:

### `docs`
- Canonical owner of cross-repo specifications (`specifications/`), ADRs (`architecture-decisions/`), threat models, hardware profiles, testing standards, diagrams, proposals, and validation layout.
- Do NOT place raw ecosystem surveys or raw benchmark dumps here (`research`), nor Rust code / JSON schemas (`sbm-common`).
- Specification edits require a coordinated PR in `sbm-common` (types, schemas, test vectors).

### `research`
- Canonical owner of primary-source ecosystem evaluations (`ecosystem/`), hardware probes (`hardware/`), comparative deep dives (`architecture/`), raw empirical evidence (`validation/evidence/`), and exploratory memos (`proposals/`).
- Primary sources only: cite upstream URLs, dates (`YYYY-MM-DD`), and tags/commits. Explicitly label *Verified*, *Experimental / WIP*, or *Unverified / Open Question*.
- Never duplicate normative specs/ADRs here (`docs`). Always redact ECIDs, UUIDs, serial numbers, MAC addresses, and private keys. Update `research/README.md` when adding findings.

### `sbm`
- Executive coordination hub: vision, architecture overview, roadmap, repository map, workspace `justfile`, release manifests (`releases/`), and agent launch prompts (`agents/`).
- Do NOT duplicate deep specs/ADRs (`docs`), raw research (`research`), shared crates/schemas (`sbm-common`), or component code.

### `sbm-common`
- Single canonical Rust & JSON Schema implementation of `docs/specifications/01..07`.
- **`no_std` + `alloc` guarantee:** `sbm-types` and `sbm-protocol` MUST compile with `--no-default-features` for both `x86_64-unknown-uefi` and `aarch64-unknown-uefi`. Never add OS-dependent crates to their non-`std` dependency graph.
- **`#![forbid(unsafe_code)]`:** Keep all crates in `sbm-common` 100% safe Rust.
- **Keep lean:** Data types, serialization, protocol validation, and schemas only — never pull in `uefi-rs`, `tokio`, `axum`, or Linux syscalls.

### `sbm-uefi`
- Pre-OS graphical GOP boot manager (`x86_64-unknown-uefi` & `aarch64-unknown-uefi`), OS/BLS/UKI discovery, EFI chainloading, ESP-backed atomic persistence, bounded remote `boot_select` policy decision, and `.sbat`/`NX_COMPAT` Secure Boot readiness.
- Do NOT implement Linux disk/OS installation (`sbm-core`), Apple `m1n1` bootstrap (`sbm-as`), copy `sbm-common` structs, or allow arbitrary RCE or destructive storage actions in UEFI.
- **0.1 milestone & checks:** Read `sbm-uefi/HANDOFF.md` before changing recovery JSON or the QEMU disk; guest timings are in `sbm-uefi/docs/MEASUREMENTS.md`.
  - Level 0 host checks (< 1 s): `cargo fmt --all`, `cargo clippy -p sbm-boot -p sbm-gfx -p xtask --all-targets -- -D warnings`, `cargo test`, `cargo run -p xtask -- preview` (static mock preview only, not firmware qualification).
  - Level 1 local EFI smoke checks default to native arch: `cargo run -p xtask -- build native` and `cargo run -p xtask -- qemu native` (use `all` for both architectures).
  - Release UEFI checks: `cargo clippy` and `cargo build` for `sbm-app` and `sbm-fixture` with `--release` on both UEFI targets.
  - **Do not set `profile.dev` `panic = "abort"`.**

### `sbm-core`
- Unified `mkosi` immutable Linux recovery/management OS (Fedora Minimal for x86/ARM64 PCs; Fedora Asahi Minimal `kernel-16k` for Apple Silicon), adaptive RAM mode (`sbm.toram=auto`), `labwc` + XWayland (GParted) + `wayvnc` + `Iced` UI, and `sbm-cored` storage/OS safety orchestrator.
- Do NOT rewrite mature filesystem/storage utilities (`e2fsprogs`, `ntfs-3g`, `partclone`, `wimlib`, `gparted`, `cryptsetup`) in Rust; invoke via `execve` without shell interpolation (`ADR-006`). Do NOT install/uninstall SBM itself (`install`).
- Default local builds use `SBM_BUILD_PROFILE=dev` (native architecture only, incremental Cargo/mkosi, uncompressed). Use `SBM_BUILD_PROFILE=release` only for explicit release qualification.

### `sbm-as`
- Apple Silicon bootstrap integrating upstream `AsahiLinux/m1n1` and `AsahiLinux/u-boot` (`CONFIG_EFI_LOADER`) to boot the unmodified `aarch64-unknown-uefi` binary from `sbm-uefi`, Apple `PlatformCapabilities` / partition guardrails (`iSCPreboot`, `RecoveryOS`, macOS APFS), light `aarch64-apple` Asahi profile, and local `asahi-fwextract`.
- NEVER fork/duplicate `sbm-uefi`. NEVER modify a developer Mac's boot policy or partitions. NEVER redistribute Apple firmware blobs. NEVER claim M5 bare-metal support before upstream Asahi lands DART/PCIe/USB3.

### `install`
- Removable SBM media creation (single-architecture recommended; dual-architecture experimental/optional per ADR-009 amendment), TUF-verified SBM install/A/B update (`tough`), clean uninstall preserving `BootOrder`, Windows BitLocker pre-flight Recovery Key gate, and Apple Silicon delegation to `sbm-as`.
- Do NOT implement general OS installation (`sbm-core`). Never modify host disks during automated tests. Use `--no-compress` (`SBM_COMPRESS=0`) for local non-release media checks.

### `sbm-server`
- Optional self-hosted management server (`axum`/`sqlx`/SQLite+PostgreSQL, mTLS via `rcgen`, Passkeys/RBAC with destructive step-up, signed `CommandEnvelope` API, embedded SvelteKit + `@novnc/novnc` + `xterm.js` dashboard).
- Do NOT make `sbm-server` mandatory for local SBM use. Do NOT link `AGPL-3.0` libraries. Do NOT redefine `sbm-common` protocol types.

### `sbm-catalog`
- Declarative `OsCatalogEntry` JSON definitions under `catalog/{linux,bsd,windows,recovery}/` conforming strictly to `sbm-common` JSON Schema, accurate `architectures`/`supported_platforms` tagging, mirror/checksum verification, and TUF signing.
- ZERO arbitrary executable shell scripts in catalog definitions. Never redistribute proprietary Windows ISOs or Apple firmware.

### `sbm-agent`
- Optional guest OS daemon confirming healthy boot (`last_attempt.json` / `BootSuccessReport`), coordinating signed `BootCommand` next-boot staging and graceful reboots, and service packaging (`systemd`, Windows Service, `launchd`).
- Must remain 100% optional. Do NOT perform disk partitioning or OS installation inside the guest agent. Never store plaintext operator credentials.

### `sbm-lab`
- Thin integration & virtual hardware laboratory (QEMU + OVMF/AAVMF/U-Boot `qemu_arm64` + `swtpm`, HVF/KVM/TCG acceleration, shared host resource manager, golden copy-on-write QCOW2 fixtures, QMP `screendump`/timing assertions, fault injection).
- Do NOT expand `sbm-lab` into a general-purpose virtualization framework or duplicate `sbm-media` GPT/FAT code. Do NOT put production runtime code here. NEVER touch physical host storage or macOS boot configuration.

### `smart-boot-menu.github.io`
- Static documentation, landing site, honest compatibility matrix, and verified download portal consuming `ReleaseManifest` JSON with zero third-party analytics/tracking/telemetry.
- Do NOT fork normative specifications out of `docs`. Do NOT claim unverified hardware support or unmeasured benchmarks. Do NOT publish/deploy without explicit maintainer approval.

## 6. Tiered Development Workflow & Host Resource Hygiene

Always validate changes at the cheapest sufficient level (full guide: [`docs/testing/development-workflow.md`](https://github.com/smart-boot-menu/docs/blob/main/testing/development-workflow.md); workspace runner: [`sbm/justfile`](https://github.com/smart-boot-menu/sbm/blob/main/justfile)):

| Level | Scope | Typical cost | Primary entry point |
|---|---|---|---|
| **0 — Host** | Native Rust unit tests, clippy/fmt, synthetic fixtures, mock previews | `< 10 s` | `just test`, `just check <repo>`, `just preview-uefi`, `just preview` |
| **1 — Native EFI** | Native-arch `sbm-uefi` build + QEMU HVF/KVM smoke | `~2–5 s` | `just efi-build`, `just efi-smoke` |
| **2 — Incremental Linux** | Single-crate `cargo build` or native-arch uncompressed `dev` `mkosi` image | `~5 s` crate / `~2 min` image | `just core-cargo`, `just core-build`, `just core-smoke` |
| **3 — Integration** | Full-system QEMU graphical recovery gates (`sbm-lab`) | `~1–3 min` | `just lab-smoke`, `just lab-integration <image>` |
| **4 — Release** | Dual-arch compressed media, reproducibility, CI qualification | `~11+ min` | `just release-core`, `just release-media`, `just release-repro` |

Operational guardrails on shared 16 GiB developer laptops:
- **Check reservations first:** Run `just status` (or `sbm-lab resources status`) before starting any VM. Never compete with another agent's heavy/graphical QEMU reservation.
- **Native architecture by default:** Never run `x86_64` TCG emulation or dual-arch builds on Apple Silicon unless explicitly qualifying cross-architecture or release behavior.
- **Preview ownership:** Interactive native SBM UI preview (`just preview`) is owned by Agent 18; do not duplicate interactive UI preview tooling. Static mock previews (`just preview-uefi`) and mocked UI modes must be clearly labeled `MOCK` and never count as release qualification.
- **Safe storage hygiene:** Cleanup commands default to dry-run (`just clean-dry-run`, `just storage`). Never delete `sbm-core/build/evidence/`, `install/build/out/release/`, `sbm-lab/var/runs/int-*`, `sbm-catalog/cache/`, backing images, or `~/.cargo/registry/`.
