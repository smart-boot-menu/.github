# Smart Boot Menu (SBM)

**Open-source, network-native, operating-system-independent bare-metal lifecycle management.**

Install Smart Boot Menu once, and manage the entire lifecycle of your computer:
- **Fast Path (`sbm-uefi`)** — Native Rust graphical UEFI boot manager (`x86_64` & `aarch64`) with automatic OS/BLS/UKI discovery, EFI chainloading, offline-first local policies, and optional bounded remote boot selection without initializing Linux.
- **Management Path (`sbm-core`)** — Immutable, modular, RAM-capable minimal Linux recovery & management OS (`mkosi`: Fedora Minimal `< 650 MB` for x86-64/ARM64 PCs and super-light Fedora Asahi Minimal `< 900 MB` for Apple Silicon) featuring GParted, safe disk planning, verified OS installation, backup/cloning, diagnostics, and browser-native remote desktop (`wayvnc`/`noVNC`).
- **Apple Silicon Bootstrap (`sbm-as`)** — Upstream `m1n1` + `AsahiLinux/u-boot` integration booting the shared `aarch64` `sbm-uefi` binary, with strict Apple APFS/Preboot protection and external USB3 boot support on supported generations.
- **Optional Self-Hosted Control Plane (`sbm-server` & `sbm-agent`)** — 100% functional without internet, accounts, or servers; optionally connect enrolled machines via Passkeys, mTLS, and Ed25519-signed replay-protected command envelopes.

## Repositories (14)

| Repository | Role |
|---|---|
| [`sbm`](https://github.com/smart-boot-menu/sbm) | Central coordination, vision, architecture summary, roadmap, release manifests & agent prompts |
| [`docs`](https://github.com/smart-boot-menu/docs) | Canonical normative specifications (v1), ADRs (001–008), threat model, hardware profiles, testing & Mermaid diagrams |
| [`research`](https://github.com/smart-boot-menu/research) | Primary-source ecosystem surveys, Apple Silicon studies, technology evaluations & retained validation evidence |
| [`sbm-common`](https://github.com/smart-boot-menu/sbm-common) | Shared Rust crates (`no_std`+`alloc` `sbm-types`, `sbm-protocol`; `sbm-schema`), JSON Schemas & test vectors |
| [`sbm-uefi`](https://github.com/smart-boot-menu/sbm-uefi) | Native Rust graphical UEFI boot manager (`x86_64` & `aarch64`) |
| [`sbm-core`](https://github.com/smart-boot-menu/sbm-core) | Minimal immutable Linux management, OS installation & recovery OS (`mkosi`) |
| [`sbm-as`](https://github.com/smart-boot-menu/sbm-as) | Apple Silicon `m1n1` / Asahi U-Boot integration, light Asahi profile & external boot |
| [`install`](https://github.com/smart-boot-menu/install) | Cross-platform installer, updater, uninstaller & bootable USB media creator for SBM |
| [`sbm-server`](https://github.com/smart-boot-menu/sbm-server) | Optional self-hosted management server (`axum`/`sqlx`/SvelteKit/`noVNC`) |
| [`sbm-catalog`](https://github.com/smart-boot-menu/sbm-catalog) | TUF-signed declarative OS catalog & verified installation recipes |
| [`sbm-agent`](https://github.com/smart-boot-menu/sbm-agent) | Optional guest OS background daemon (boot-success confirmation & remote reboot) |
| [`sbm-lab`](https://github.com/smart-boot-menu/sbm-lab) | QEMU / OVMF / AAVMF / U-Boot / `swtpm` virtual hardware lab, benchmarks & fault injection |
| [`smart-boot-menu.github.io`](https://github.com/smart-boot-menu/smart-boot-menu.github.io) | Official project website, documentation portal & downloads |
| [`.github`](https://github.com/smart-boot-menu/.github) | Organization profile, shared `AGENTS.md`, policies & issue/PR templates |

## License

Original Smart Boot Menu source code and documentation are dual-licensed under **MIT OR Apache-2.0**, subject to third-party licensing requirements for bundled external utilities.
