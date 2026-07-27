# Task 2.5 – Passwordless Sudo

## Goal

Allow the `deploy` automation account to execute administrative commands without a password while authenticating via SSH keys.

## Why

- Enables Ansible automation.
- Avoids managing Linux passwords for automation accounts.
- Follows common cloud administration practices.

## Implementation

- Created `/etc/sudoers.d/deploy`
- Managed with `ansible.builtin.copy`
- Owner: `root`
- Group: `root`
- Mode: `0440`
- Validated with `visudo -cf`

## Verification

```bash
sudo whoami
sudo -l
