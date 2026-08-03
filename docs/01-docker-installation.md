# Docker Engine Installation

## Objective

Provision Docker Engine on all homelab nodes using Ansible.

This playbook installs Docker from Docker's official APT repository, enables the Docker service, and ensures it starts automatically on boot.

---

## Linux Concepts

During this ticket, the following Linux concepts were learned:

- APT repositories
- Third-party package repositories
- GPG repository signing
- `/etc/apt/keyrings`
- Package installation using APT
- Systemd service management

---

## Ansible Modules

| Module | Purpose |
|----------|---------|
| `file` | Create the Docker keyrings directory |
| `get_url` | Download Docker's GPG key |
| `apt_repository` | Add Docker's official repository |
| `apt` | Update package cache and install Docker packages |
| `systemd` | Enable and start the Docker service |

---

## Playbook

```
playbooks/docker-install.yml
```

---

## Tasks Performed

1. Created `/etc/apt/keyrings`
2. Downloaded Docker's GPG key
3. Added Docker's official APT repository
4. Updated the APT package cache
5. Installed Docker Engine packages
6. Enabled and started the Docker service

---

## Verification Commands

```bash
docker --version
docker compose version
docker info
docker ps
systemctl status docker
systemctl is-enabled docker
```

---

## Playbook Result

```
ok=7
changed=4
failed=0
```

Docker Engine was successfully installed on:

- node1
- node2

---

## Notes

- Docker was installed from Docker's official repository.
- Docker Compose Plugin was installed alongside Docker Engine.
- Docker service is enabled and starts automatically after reboot.
- Ansible reports a deprecation warning for `apt_repository`. Future work should migrate to `deb822_repository`.

---

## Lessons Learned

This ticket introduced package repositories beyond Ubuntu's default repositories.

Key concepts learned:

- Difference between `apt update` and `apt install`
- Why Docker provides its own repository
- Repository authentication using GPG keys
- Managing repositories with Ansible
