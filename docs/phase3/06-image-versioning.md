
## `06-image-versioning.md`

```markdown
# Docker Image Versioning

## Overview

The initial CI pipeline used only the `latest` Docker tag.

This was changed so that every CI build also receives a tag based on the Git commit SHA.

## Problem With `latest`

The image initially used:

```text
ghcr.io/s0uf12n8/homelab-ci-demo:latest
