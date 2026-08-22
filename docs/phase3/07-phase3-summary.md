# Phase 3 — CI/CD

## Objective

Build a CI/CD pipeline that validates the Ansible repository, builds a Docker image, publishes it to GitHub Container Registry, and deploys the exact image version to node2.

## Pipeline

```text
Git push
    ↓
GitHub Actions
    ↓
Ansible syntax validation
    ↓
ansible-lint
    ↓
Docker build
    ↓
GHCR
    ↓
SSH
    ↓
Ansible
    ↓
node2
    ↓
Docker container
