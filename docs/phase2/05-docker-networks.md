# Docker Networks

## Objective

Create and manage Docker networks using Ansible.

This playbook ensures that a dedicated Docker bridge network exists, providing network isolation and communication between related containers.

---

## Engineering Problem

By default, Docker places containers on the default `bridge` network.

While suitable for simple environments, production deployments use dedicated networks to:

- Isolate applications
- Control communication between containers
- Improve security
- Organize multi-container applications

---

## Linux Concepts

During this ticket, the following concepts were learned:

- Docker networks
- Bridge network driver
- Network isolation
- Container-to-container communication
- Docker network drivers

---

## Manual Linux Command

```bash
docker network create --driver bridge frontend
```

---

## Linux to Ansible Mapping

| Docker CLI | Ansible |
|------------|----------|
| `docker network create` | `community.docker.docker_network` |
| `frontend` | `name: frontend` |
| `--driver bridge` | `driver: bridge` |
| Create if missing | `state: present` |

---

## Ansible Module

| Module | Purpose |
|---------|---------|
| `community.docker.docker_network` | Ensure Docker networks exist with the desired configuration |

---

## Playbook

```text
playbooks/docker/network.yml
```

---

## Tasks Performed

1. Connected to the Docker Engine API.
2. Checked whether the `frontend` network already existed.
3. Created the network if it was missing.
4. Configured it to use the `bridge` driver.
5. Left the system unchanged if the desired state already existed.

---

## Verification Commands

List Docker networks:

```bash
docker network ls
```

Inspect the network:

```bash
docker network inspect frontend
```

Verify idempotency:

```bash
ansible-playbook playbooks/docker/network.yml
```

Expected result:

```text
changed=0
```

---

## Lessons Learned

- Docker networks isolate container communication.
- The default `bridge` network is suitable for simple workloads but custom networks are preferred in production.
- Containers attached to the same custom bridge network can communicate using container names.
- Ansible manages Docker networking declaratively through the Docker Engine API.

---

## Common Mistakes

- Using only the default bridge network for every application.
- Confusing Docker networks with host network interfaces.
- Forgetting to verify network creation after deployment.

---

## Key Takeaways

- Networks define how containers communicate.
- Custom bridge networks improve organization and isolation.
- Infrastructure should be reproducible through Ansible.
