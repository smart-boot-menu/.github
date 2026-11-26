# Smart Boot Menu — Shared Agent Rules (`.github/AGENTS.md`)

Universal engineering rules for every autonomous agent and contributor across all 14 `smart-boot-menu` repositories (modeled on `0SFS/.github/AGENTS.md`). Each repository's local `AGENTS.md` extends these rules with repository-specific boundaries.

## Essential Universal Rules

- **Single Source of Truth:**
  - Normative specifications, ADRs, threat models, diagrams, and testing standards belong ONLY in [`docs`](https://github.com/smart-boot-menu/docs) (`../docs/`).
  - Primary-source ecosystem research, hardware probe notes, and retained benchmark evidence belong ONLY in [`research`](https://github.com/smart-boot-menu/research) (`../research/`).
  - Shared Rust data types, wire protocols, JSON Schemas, and golden test vectors belong ONLY in [`sbm-common`](https://github.com/smart-boot-menu/sbm-common) (`../sbm-common/`).
  - Never write project documentation, specs, or artifacts into `~/.gemini/` or outside `~/gh/smart-boot-menu/`.
- **Save Knowledge Worth Retaining:** Whenever you verify a firmware quirk, driver limitation, build constraint, or benchmark result, save it into `research/` (or update an ADR/spec in `docs/`) and update the relevant index.
- **Hardware & Workstation Safety:** NEVER modify a development machine's physical boot configuration, macOS `LocalPolicy`/SIP, UEFI `BootOrder`, or physical partitions during development or automated tests. Use `sbm-lab` QEMU/OVMF/AAVMF/U-Boot copy-on-write QCOW2 fixtures.
- **Security & Privacy:** Never commit credentials, private keys, tokens, BitLocker recovery keys, ECIDs, hardware serial numbers, or personal data. Never allow arbitrary remote code execution in `sbm-uefi`.
- **Offline-First:** Every core local capability must work with zero internet access, zero account, and no `sbm-server`.
- **Honest Engineering:** Clearly distinguish *Verified* capabilities from *Experimental / WIP* and *Unverified / Open Questions*. Never invent upstream release versions or publish unmeasured performance numbers.
- **Git Hygiene:** Use `main` for default branches. Never force-push `main`. Never add AI author, committer, or co-author trailers. Keep unrelated work out of commits.
- **Licensing:** All original SBM code and documentation are dual-licensed **`MIT OR Apache-2.0`**. Isolate GPL-2.0/3.0 CLI tools as separate subprocesses (`execve`, never shell interpolation) and do not statically link GPL/AGPL crates into permissive Rust crates.

## Task Routing Index

| When the task touches… | Read / inspect |
|---|---|
| Repository ownership & boundaries | [`sbm/REPOSITORIES.md`](https://github.com/smart-boot-menu/sbm/blob/main/REPOSITORIES.md) (`../sbm/REPOSITORIES.md`) |
| Architecture overview & SBM 0.1 roadmap | [`sbm/ARCHITECTURE.md`](https://github.com/smart-boot-menu/sbm/blob/main/ARCHITECTURE.md) & [`sbm/ROADMAP.md`](https://github.com/smart-boot-menu/sbm/blob/main/ROADMAP.md) |
| Technology choices & rationale (ADRs 001–008) | [`docs/architecture-decisions/`](https://github.com/smart-boot-menu/docs/tree/main/architecture-decisions) (`../docs/architecture-decisions/`) |
| Wire protocols, schemas, or shared Rust types | [`docs/specifications/`](https://github.com/smart-boot-menu/docs/tree/main/specifications) & [`sbm-common`](https://github.com/smart-boot-menu/sbm-common) |
| Security, Secure Boot, BitLocker, threat model | [`docs/threat-model/README.md`](https://github.com/smart-boot-menu/docs/blob/main/threat-model/README.md) & [`research/architecture/security-and-supply-chain.md`](https://github.com/smart-boot-menu/research/blob/main/architecture/security-and-supply-chain.md) |
| Apple Silicon (`m1n1`, U-Boot, Asahi M1–M5) | [`research/hardware/apple-silicon-m1-to-m5.md`](https://github.com/smart-boot-menu/research/blob/main/hardware/apple-silicon-m1-to-m5.md) & [`docs/hardware/target-hardware.md`](https://github.com/smart-boot-menu/docs/blob/main/hardware/target-hardware.md) |
| UEFI graphics, Linux Wayland/GParted, GPUs | [`research/hardware/gpu-and-graphics-acceleration.md`](https://github.com/smart-boot-menu/research/blob/main/hardware/gpu-and-graphics-acceleration.md) & `ADR-002` |
| Minimal Linux `sbm-core` (`mkosi`, Fedora/Asahi) | [`research/architecture/minimal-linux-core-os.md`](https://github.com/smart-boot-menu/research/blob/main/architecture/minimal-linux-core-os.md) & `ADR-003` |
| Benchmarks, QEMU tests, fault injection | [`docs/testing/benchmarks.md`](https://github.com/smart-boot-menu/docs/blob/main/testing/benchmarks.md) & `ADR-008` |
| Changing a shared interface | [`agent-reference.md`](agent-reference.md) (Interface Change Protocol) |
