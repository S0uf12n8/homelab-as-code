# Fail2Ban

## Objective

Protect SSH against brute-force attacks by automatically banning IP addresses that repeatedly fail authentication.

The goal is to reduce automated login attempts while integrating with the existing UFW firewall configuration.

---

## Implementation

The playbook performs the following tasks:

- Installs the `fail2ban` package.
- Deploys a managed `jail.local` configuration using the Ansible `template` module.
- Ensures the Fail2Ban service is enabled and running.
- Restarts the service automatically when the configuration changes using an Ansible handler.

---

## Configuration

The deployed SSH jail configuration:

```ini
[DEFAULT]
bantime = 1h
findtime = 10m
maxretry = 5

[sshd]
enabled = true
backend = systemd
```

---

## Verification

### Package installation

```bash
dpkg -l fail2ban
```

Confirmed that the package is installed.

---

### Service status

```bash
systemctl status fail2ban
```

Verified the service is active and running.

---

### Jail status

```bash
sudo fail2ban-client status
```

Confirmed that the SSH jail is loaded.

---

### SSH jail details

```bash
sudo fail2ban-client status sshd
```

Verified the jail configuration and current statistics.

---

### Idempotency

The playbook was executed multiple times.

```
changed=0
```

confirming that no unnecessary changes are made after the desired state is reached.

---

## Problem Encountered

After deployment, Fail2Ban failed to start.

```
Failed during configuration:
Have not found any log file for sshd jail
```

The issue was diagnosed using:

```bash
journalctl -u fail2ban
```

Ubuntu 22.04 on this server uses the systemd journal for SSH logs instead of a traditional log file.

The solution was to explicitly configure:

```ini
backend = systemd
```

inside the SSH jail.

---

## Lessons Learned

- Read service logs before changing configuration.
- Validate the actual system state instead of trusting automation output.
- Configuration management includes debugging and verification.
- Handlers should restart services only when configuration changes.
