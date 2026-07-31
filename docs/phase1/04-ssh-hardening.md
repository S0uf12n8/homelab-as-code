# SSH Hardening

## Objective

The goal of this task was to improve the security of the SSH service on all managed Ubuntu servers using Ansible while following production-inspired practices.

The objectives were:

* Disable password authentication.
* Disable root SSH login.
* Keep public key authentication enabled.
* Validate the SSH configuration before applying changes.
* Reload the SSH service only when configuration changes.
* Verify that remote access remains functional.
* Ensure the playbook is idempotent.

---

## Implementation

A new SSH configuration file was deployed using the `ansible.builtin.copy` module:

```text
/etc/ssh/sshd_config.d/99-hardening.conf
```

Instead of modifying Ubuntu's default configuration files, a dedicated configuration file was created to keep custom hardening separate from vendor-managed files.

The deployed configuration contains:

```text
PasswordAuthentication no
PermitRootLogin no
PubkeyAuthentication yes
```

The playbook validates the configuration using:

```text
/usr/sbin/sshd -t -f %s
```

before replacing the destination file.

A handler reloads the SSH service only when the configuration changes.

---

## Verification

The following verification steps were completed:

* Performed an Ansible syntax check successfully.
* Executed the playbook against both nodes.
* Confirmed that the configuration file was deployed.
* Verified that the SSH handler reloaded the service.
* Opened a new SSH session using the `deploy` account after the changes.
* Executed the playbook a second time and confirmed idempotency (`changed=0`).

---

## Problems Encountered

### Understanding Ansible playbook structure

Initially, I struggled to understand the hierarchy of a playbook, especially where `tasks` and `handlers` belong and how YAML indentation defines the structure.

### Choosing the correct configuration file

At first, I planned to modify `/etc/ssh/sshd_config`.

After inspecting the server, I discovered that Ubuntu includes additional configuration files from:

```text
/etc/ssh/sshd_config.d/
```

This led to the decision to manage a dedicated file (`99-hardening.conf`) instead of modifying Ubuntu's default configuration.

### Understanding handlers

I initially thought handlers were simply another type of task.

Through testing, I learned that handlers execute only when notified by a task that reports a change, preventing unnecessary service reloads.

### Running the playbook from the wrong directory

The first execution failed because Ansible could not locate the inventory file.

I learned that the working directory affects how relative paths in `ansible.cfg` are resolved.

Running the playbook from the repository root allowed Ansible to correctly load the inventory.

### Understanding idempotency

Running the playbook twice demonstrated Ansible's idempotent behavior.

* First execution: configuration deployed (`changed`).
* Second execution: no changes required (`ok`).

This confirmed that the playbook only modifies the system when necessary.

---

## Lessons Learned

* Keep custom configuration separate from operating system managed files.
* Always validate configuration before reloading a service.
* Use handlers to avoid unnecessary service reloads.
* Verify remote access after modifying SSH configuration.
* Execute playbooks from the repository root so Ansible can locate its configuration and inventory.
* Always verify idempotency by running the playbook more than once.
