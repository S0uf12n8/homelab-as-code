# Ansible Validation

## Overview

This ticket added automatic validation of the Ansible project to the CI pipeline.

The goal is to catch problems before changes are accepted as valid.

We use two different validation methods:

```text
Ansible syntax check
        +
ansible-lint
