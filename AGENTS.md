# Smart Boot Menu — Agent Entry Point (`.github/AGENTS.md`)

Applies to every repository in the `smart-boot-menu` organization (modeled on `0SFS/.github/AGENTS.md`). Read this file, then follow only the task-relevant rows in the routing index below and the repository-specific guardrails in [`agent-reference.md`](agent-reference.md#5-repository-specific-rules--guardrails). Individual repositories keep only a minimal pointer `AGENTS.md` back to this file so all agent instructions live in one central place.

> **Standalone / Single-Repo Agents:** If you were launched inside a single repository checkout and sibling directories (`../.github/`, `../docs/`, `../sbm/`) are not present locally, use the GitHub URLs linked here to read the required policy, ADRs, and contracts before proceeding.

## Open-Source Reuse & Engineering Principles (Canonical Policy)

> **Guiding Principle:** **Do not recreate inferior versions of existing software.**
>
> **Write less software. Reuse the best available implementations. Make meaningful contributions where existing solutions fall short.**
>
> Formal ADR: [`docs/architecture-decisions/ADR-010-open-source-reuse-and-engineering-principles.md`](https://github.com/smart-boot-menu/docs/blob/main/architecture-decisions/ADR-010-open-source-reuse-and-engineering-principles.md) (`../docs/architecture-decisions/ADR-010-open-source-reuse-and-engineering-principles.md`). Operational checklist: [`agent-reference.md`](agent-reference.md#4-proportional-build-vs-reuse-evaluation).

Our objective is to advance the state of the art, not duplicate it. Write original software only when we can provide meaningful, demonstrable improvements over existing solutions.

1. **Research Before Implementation:** Before designing or implementing a new component, investigate existing open-source projects solving the same or substantially similar problems. Search beyond familiar languages, ecosystems, and repositories. Examine actual implementations, documentation, limitations, benchmarks, licensing, and maintenance status. Do not assume a solution does not exist simply because you are unfamiliar with it.
2. **Order of Preference (Default Preference, Not Absolute Constraint):**
   1. Reuse existing software directly.
   2. Configure and compose existing software.
   3. Write thin integration layers.
   4. Contribute improvements upstream.
   5. Maintain a fork when justified.
   6. Develop an original implementation when it offers meaningful advantages.
3. **Justify Original Implementations:** An original implementation must demonstrate a meaningful advantage in one or more of: functionality, performance, energy efficiency, memory consumption, storage consumption, security, reliability, portability, maintainability, user experience, developer experience, or architectural simplicity. Evaluate against the **strongest relevant existing alternatives** (not weak baselines), accounting for total cost of ownership (dependencies, integration, maintenance, security updates, future development).
4. **Language Is Not Justification:** Rust is our preferred language for original systems software, but rewriting an existing C, C++, Python, or other implementation in Rust is **not** inherently an improvement. Language choice must produce tangible benefits that justify the rewrite.
5. **Avoid Sunk-Cost Reasoning:** Apply these principles retrospectively to existing SBM code. If an existing open-source project provides a superior solution, be willing to replace, simplify, or remove our own implementation. Previously invested effort is not justification for retaining inferior software.
6. **Contribute Upstream:** When an improvement is generally useful and not specific to SBM, prefer contributing it upstream to avoid unnecessary ecosystem fragmentation.
7. **Keep Research Proportional:** Do not turn every minor function or bug fix into an extensive research project. Scale research depth to complexity, novelty, risk, and long-term maintenance cost. New subsystems, major dependencies, architectural changes, and replacements of established software **require** explicit build-versus-reuse justification.
8. **Evidence Over Assumptions:** Distinguish measured results from theoretical advantages. Do not claim our implementation is faster, lighter, safer, or better without appropriate evidence. When practical, establish an empirical baseline against existing alternatives before implementing a replacement.

## Essential Universal Rules

- **Single Source of Truth:**
  - Central agent instructions and operational checklists belong ONLY in [`.github`](https://github.com/smart-boot-menu/.github) (`AGENTS.md` + `agent-reference.md`). Do not expand per-repo `AGENTS.md` files into duplicated rulebooks.
  - Normative specifications, ADRs, threat models, diagrams, and testing standards belong ONLY in [`docs`](https://github.com/smart-boot-menu/docs) (`../docs/`).
  - Primary-source ecosystem research, hardware probe notes, and retained benchmark evidence belong ONLY in [`research`](https://github.com/smart-boot-menu/research) (`../research/`).
  - Shared Rust data types, wire protocols, JSON Schemas, and golden test vectors belong ONLY in [`sbm-common`](https://github.com/smart-boot-menu/sbm-common) (`../sbm-common/`).
  - Never write project documentation, specs, or artifacts into `~/.gemini/` or outside `~/gh/smart-boot-menu/`.
- **Save Knowledge Worth Retaining:** Whenever you verify a firmware quirk, driver limitation, build constraint, open-source evaluation, or benchmark result, save it into `research/` (or update an ADR/spec in `docs/`) and update the relevant index.
- **Resource-Efficient Tiered Development:** Always use the cheapest validation level sufficient for your change (Level 0 native host tests via `just test` -> Level 1 native EFI smoke via `just efi-smoke` -> Level 2 incremental Linux via `just core-build` -> Level 3 integration -> Level 4 release). Default local builds to native host architecture without compression, reserve through `sbm-lab resources` before launching any VM, label mocks clearly (never count them as release qualification), and default all cleanup tooling to dry-run. See [`agent-reference.md` §6](agent-reference.md#6-tiered-development-workflow--host-resource-hygiene).
- **Hardware & Workstation Safety:** NEVER modify a development machine's physical boot configuration, macOS `LocalPolicy`/SIP, UEFI `BootOrder`, or physical partitions during development or automated tests. Use `sbm-lab` QEMU/OVMF/AAVMF/U-Boot copy-on-write QCOW2 fixtures.
- **Security & Privacy:** Never commit credentials, private keys, tokens, BitLocker recovery keys, ECIDs, hardware serial numbers, or personal data. Never allow arbitrary remote code execution in `sbm-uefi`.
- **Offline-First:** Every core local capability must work with zero internet access, zero account, and no `sbm-server`.
- **Honest Engineering:** Clearly distinguish *Verified* capabilities from *Experimental / WIP* and *Unverified / Open Questions*. Never invent upstream release versions or publish unmeasured performance numbers.
- **Git Hygiene:** Use `main` for default branches. Never force-push `main`. Never add AI author, committer, or co-author trailers. Preserve existing uncommitted work and stage explicit paths only.
- **Licensing:** All original SBM code and documentation are dual-licensed **`MIT OR Apache-2.0`**. Isolate GPL-2.0/3.0 CLI tools as separate subprocesses (`execve`, never shell interpolation) and do not statically link GPL/AGPL crates into permissive Rust crates.

## Task Routing Index

| When the task touches… | Read / inspect |
|---|---|
| Setup, commands, local scope | The target repository's `README.md` + [`agent-reference.md` §5](agent-reference.md#5-repository-specific-rules--guardrails) |
| Tiered dev workflow, `justfile`, caches & VM budgets | [`docs/testing/development-workflow.md`](https://github.com/smart-boot-menu/docs/blob/main/testing/development-workflow.md), [`sbm/justfile`](https://github.com/smart-boot-menu/sbm/blob/main/justfile), & [`agent-reference.md` §6](agent-reference.md#6-tiered-development-workflow--host-resource-hygiene) |
| Open-source reuse & build-vs-reuse policy | [`ADR-010`](https://github.com/smart-boot-menu/docs/blob/main/architecture-decisions/ADR-010-open-source-reuse-and-engineering-principles.md) & [`agent-reference.md` §4](agent-reference.md#4-proportional-build-vs-reuse-evaluation) |
| Repository ownership & boundaries | [`sbm/REPOSITORIES.md`](https://github.com/smart-boot-menu/sbm/blob/main/REPOSITORIES.md) (`../sbm/REPOSITORIES.md`) & [`agent-reference.md` §5](agent-reference.md#5-repository-specific-rules--guardrails) |
| Architecture overview & SBM 0.1 roadmap | [`sbm/ARCHITECTURE.md`](https://github.com/smart-boot-menu/sbm/blob/main/ARCHITECTURE.md) & [`sbm/ROADMAP.md`](https://github.com/smart-boot-menu/sbm/blob/main/ROADMAP.md) |
| Technology choices & rationale (ADRs 001–010) | [`docs/architecture-decisions/`](https://github.com/smart-boot-menu/docs/tree/main/architecture-decisions) (`../docs/architecture-decisions/`) |
| Wire protocols, schemas, or shared Rust types | [`docs/specifications/`](https://github.com/smart-boot-menu/docs/tree/main/specifications) & [`sbm-common`](https://github.com/smart-boot-menu/sbm-common) |
| Security, Secure Boot, BitLocker, threat model | [`docs/threat-model/README.md`](https://github.com/smart-boot-menu/docs/blob/main/threat-model/README.md) & [`research/architecture/security-and-supply-chain.md`](https://github.com/smart-boot-menu/research/blob/main/architecture/security-and-supply-chain.md) |
| Apple Silicon (`m1n1`, U-Boot, Asahi M1–M5) | [`research/hardware/apple-silicon-m1-to-m5.md`](https://github.com/smart-boot-menu/research/blob/main/hardware/apple-silicon-m1-to-m5.md) & [`docs/hardware/target-hardware.md`](https://github.com/smart-boot-menu/docs/blob/main/hardware/target-hardware.md) |
| UEFI graphics, Linux Wayland/GParted, GPUs | [`research/hardware/gpu-and-graphics-acceleration.md`](https://github.com/smart-boot-menu/research/blob/main/hardware/gpu-and-graphics-acceleration.md) & `ADR-002` |
| Minimal Linux `sbm-core` (`mkosi`, Fedora/Asahi) | [`research/architecture/minimal-linux-core-os.md`](https://github.com/smart-boot-menu/research/blob/main/architecture/minimal-linux-core-os.md) & `ADR-003` |
| Benchmarks, QEMU tests, fault injection | [`docs/testing/benchmarks.md`](https://github.com/smart-boot-menu/docs/blob/main/testing/benchmarks.md) & `ADR-008` |
| Changing a shared interface | [`agent-reference.md` §1](agent-reference.md#1-shared-interface-change-protocol) |
| Validation & recording benchmarks | [`agent-reference.md` §2–§3](agent-reference.md#2-standard-rust-validation-checklist) |

Keep this file as the single organization entry point: essential universal rules plus a task index. Put detailed procedures and repository-specific guardrails in [`agent-reference.md`](agent-reference.md) or linked guides, and update the index when adding a workflow.
