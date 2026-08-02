# Automatic Security Updates

## Objective

Configure automatic installation of Ubuntu security updates using Ansible.

The goal is to ensure that all managed servers automatically receive security patches without requiring manual intervention.

## Implementation

The playbook performs the following tasks:

- Installs the `unattended-upgrades` package.
- Deploys a managed configuration file using the Ansible `template` module.
- Ensures the configuration file has the correct ownership and permissions.

## Verification

The following verification steps were performed after deployment:

### Package installation

```bash
dpkg -l unattended-upgrades
```

Confirmed that the package is installed.

### Configuration file

```bash
cat /etc/apt/apt.conf.d/20auto-upgrades
```

Verified that the deployed configuration matches the managed template.

### File permissions

```bash
ls -l /etc/apt/apt.conf.d/20auto-upgrades
```

Confirmed:

- Owner: `root`
- Group: `root`
- Permissions: `0644`

### Automatic update timer

```bash
systemctl status apt-daily-upgrade.timer
```

Verified that the systemd timer is enabled and waiting for the next scheduled execution.

### Idempotency

The playbook was executed multiple times.

```
changed=0
```

confirming that no unnecessary changes are made after the desired state is reached.

## Problems Encountered

### Choosing the correct module

Initially it was unclear whether to use the `copy` or `template` module.

The `template` module was selected to introduce configuration templating and establish a reusable repository structure for future services.

### Absolute vs relative paths

The first version used an absolute source path.

This was replaced with a repository-relative path so the project can be cloned and executed on any control machine.

### Linux ownership

The first attempt assigned the configuration file to the administrative user.

After reviewing Linux conventions, ownership was changed to `root:root`, matching the system defaults.

## Lessons Learned

- `template` and `copy` have very similar interfaces.
- Templates separate automation logic from configuration content.
- Infrastructure should be verified independently of Ansible using Linux commands.
- Understanding Linux file ownership is just as important as understanding Ansible modules.
