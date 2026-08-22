# Docker Build in CI

## Overview

After validating the Ansible project, we extended the GitHub Actions workflow to build a Docker image automatically.

The purpose was to make CI verify not only the Ansible code, but also that the Docker application can actually be built.

---

## Docker Demo

We created a small Docker application specifically for testing the CI pipeline:

```text
docker/
├── ci-demo/
│   ├── Dockerfile
│   └── index.html
└── compose.yml
