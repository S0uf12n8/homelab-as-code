# CI Fundamentals

## Overview

Continuous Integration (CI) is the practice of automatically checking changes to a project whenever code is pushed or a pull request is created.

In this homelab, GitHub Actions is used to run these checks automatically.

The goal is simple:

> Make sure changes committed to the repository are valid before they become part of the project.

---

## What happens when I push?

Our basic workflow is:

```text
Developer
    ↓
git commit
    ↓
git push
    ↓
GitHub
    ↓
GitHub Actions
    ↓
CI workflow
    ↓
Validation
