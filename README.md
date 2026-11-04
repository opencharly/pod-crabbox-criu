# pod-crabbox-criu

The **CRIU lease image** for the Crabbox `containerd` provider — the base
[`pod-crabbox`](https://github.com/opencharly/pod-crabbox) lease contract with `criu`
installed so a lease can checkpoint and restore its own processes.

## What it provides

| Property | Value |
|---|---|
| Base | CachyOS base image `ghcr.io/opencharly/pod-crabbox:2026.307.1828` |
| Tool | `criu` from `opencharly/layer-criu` |
| Pre-flight | `criu check --unprivileged --feature seccomp_suspend` (runtime) |

## How to use it

```sh
ssh crabbox@<lease> criu --version
# checkpoint a process tree the lease owns; the seccomp pre-flight needs the
# namespace the process actually runs in
```

## Layout

- `charly.yml` — the `pod-crabbox-criu` image (pinned base image + entrypoint).
- `candy/` — the variant's own layer: packages, files and `check:` steps.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `.github/workflows/image.yml` — builds on `pull_request` and publishes at the merge
  tag, so the image tag equals the git tag.
- `CHANGELOG/` — per-CalVer history.

## Related

- [`opencharly/pod-crabbox`](https://github.com/opencharly/pod-crabbox) — the lease
  base image this family builds on.
- [`CONTAINERD-IMAGES.md`](https://github.com/opencharly/opencharly/blob/main/CONTAINERD-IMAGES.md)
  §2 — the lease-image contract.
