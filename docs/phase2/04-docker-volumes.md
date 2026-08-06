# Docker Volumes

## Objective

Create and manage persistent Docker volumes using Ansible.

This playbook ensures that a Docker volume exists before it is used by containers, allowing application data to survive container deletion and recreation.

---

## Engineering Problem

Containers are ephemeral by design.

If application data is stored only inside the container filesystem, deleting the container also deletes the data.

Docker Volumes separate application data from the container lifecycle.

---

## Linux Concepts

During this ticket, the following concepts were learned:

- Docker Volumes
- Persistent storage
- Docker volume drivers
- Docker volume mount points
- Container data persistence

---

## Manual Linux Command

```bash
docker volume create nginx_data
```

---

## Linux to Ansible Mapping

| Docker CLI | Ansible |
|------------|----------|
| `docker volume create nginx_data` | `community.docker.docker_volume` |
| `nginx_data` | `name: nginx_data` |
| Create if missing | `state: present` |

---

## Ansible Module

| Module | Purpose |
|----------|---------|
| `community.docker.docker_volume` | Ensure Docker volumes exist |

---

## Playbook

```text
playbooks/docker-volume.yml
```

---

## Tasks Performed

1. Connected to the Docker Engine.
2. Checked whether the volume already existed.
3. Created the volume if it was missing.
4. Left the system unchanged if the volume already existed.

---

## Verification Commands

List Docker volumes:

```bash
docker volume ls
```

Inspect the volume:

```bash
docker volume inspect nginx_data
```

Verify the mount point:

```bash
sudo ls -l /var/lib/docker/volumes
```

Verify idempotency:

```bash
ansible-playbook playbooks/docker-volume.yml
```

Expected result:

```text
changed=0
```

---

## Lessons Learned

- Docker volumes persist independently of containers.
- Removing a container does not remove its attached volumes.
- Docker stores local volumes under `/var/lib/docker/volumes/`.
- Ansible manages Docker resources declaratively through the Docker Engine API.

---

## Common Mistakes

- Storing important data only inside containers.
- Assuming deleting a container preserves its filesystem.
- Confusing Docker volumes with bind mounts.

---

## Key Takeaways

- Containers are ephemeral.
- Volumes are persistent.
- Always store production data inside volumes.
