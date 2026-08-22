# GitHub Container Registry (GHCR)

## Overview

GitHub Container Registry (GHCR) is a container registry integrated with GitHub.

It provides persistent storage for Docker images produced by the CI pipeline.

---

## Why GHCR Was Added

A Docker image built inside GitHub Actions exists on the temporary GitHub Actions runner.

When the workflow finishes, the runner is removed.

GHCR provides persistent storage:

GitHub Actions
    ↓
Docker image
    ↓
GHCR
    ↓
Image remains available

---

## Image Name

Our image is:

ghcr.io/s0uf12n8/homelab-ci-demo

The components are:

- `ghcr.io` — GitHub Container Registry
- `s0uf12n8` — GitHub account
- `homelab-ci-demo` — container image name

---

## GitHub Actions Permissions

The workflow requires permission to publish packages:

```yaml
permissions:
  contents: read
  packages: write
