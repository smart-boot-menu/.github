## Summary

<!-- Describe what this PR changes and why. -->

## Related Issues / Specs / ADRs

- Spec / ADR:
- Issue:

## Build vs. Reuse Justification (if applicable)

<!-- Required for new subsystems, major dependencies, architectural changes, or replacements of existing software (see .github/AGENTS.md and ADR-010). Leave N/A for minor fixes or incremental changes. -->

- Existing open-source alternatives evaluated (across languages/ecosystems):
- Why reuse / composition / thin integration / upstream contribution was chosen or insufficient:
- Evidence / baseline (if claiming performance, resource, or reliability advantages):

## Checklist

- [ ] Read [`.github/AGENTS.md`](https://github.com/smart-boot-menu/.github/blob/main/AGENTS.md) and repository `AGENTS.md`, and respected repository boundaries.
- [ ] Proportional build-versus-reuse evaluation completed for any new component, major dependency, architectural change, or replacement (`ADR-010`).
- [ ] No duplicated shared protocol/schema types (canonical source is `sbm-common`).
- [ ] Formatting and lint checks pass (`cargo fmt --check`, `cargo clippy`, or repo-equivalent checks).
- [ ] Automated tests added/updated and passing.
- [ ] Security & safety impact reviewed (no silent destructive disk/NVRAM changes, bounded network timeouts, offline behavior preserved).
- [ ] Documentation updated if behavior or interfaces changed.
