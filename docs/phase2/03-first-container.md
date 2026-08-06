# First Docker Container

## Objective

Deploy the first containerized application using Ansible.

This playbook ensures that an Nginx container exists, is running, exposes port 80, and automatically restarts after a system reboot.

---

## Engineering Problem

A Docker host without containers provides no application or service.

The goal of this ticket is to deploy a web server in a repeatable and declarative manner using Ansible instead of manually executing Docker commands.

---

## Linux Concepts

During this ticket, the following Docker concepts were learned:

- Docker images
- Docker containers
- Docker Hub
- Container lifecycle
- Port publishing
- Restart policies
- Docker daemon

---

## Manual Linux Command

```bash
docker run -d \
  --name nginx \
  --restart unless-stopped \
  -p 80:80 \
  nginx:latest
```

---

## Linux to Ansible Mapping

| Docker CLI | Ansible |
|------------|----------|
| `docker run` | `community.docker.docker_container` |
| `--name nginx` | `name: nginx` |
| `nginx:latest` | `image: nginx:latest` |
| `-p 80:80` | `published_ports` |
| `--restart unless-stopped` | `restart_policy: unless-stopped` |
| Running container | `state: started` |

---

## Ansible Module

| Module | Purpose |
|----------|---------|
| `community.docker.docker_container` | Ensure Docker containers exist and are in the desired state |

---

## Playbook

```text
playbooks/docker-container.yml
```

---

## Tasks Performed

1. Ensure the Nginx image is available.
2. Create the container if it does not exist.
3. Publish host port 80 to container port 80.
4. Configure the restart policy.
5. Ensure the container is running.

---

## Verification Commands

Verify running container:

```bash
docker ps
```

Verify downloaded image:

```bash
docker images
```

Verify restart policy:

```bash
docker inspect nginx
```

Verify published ports:

```bash
docker port nginx
```

Verify web server:

```bash
curl http://localhost
```

Verify idempotency:

```bash
ansible-playbook playbooks/docker-container.yml
```

Expected result:

```text
changed=0
```

---

## Lessons Learned

- Docker images are templates used to create containers.
- Containers are running instances of images.
- Docker automatically downloads missing images from Docker Hub.
- Ansible manages the desired state of containers instead of executing Docker commands directly.
- The `community.docker.docker_container` module is idempotent.

---

## Common Mistakes

- Forgetting to publish ports.
- Using the wrong restart policy.
- Confusing images with containers.
- Assuming `docker run` is idempotent.
- Forgetting to verify the deployed service from the operating system.

---

## Key Takeaways

- Images are immutable templates.
- Containers are running instances of images.
- One image can create multiple containers.
- Infrastructure should be declared, verified, and reproducible.
