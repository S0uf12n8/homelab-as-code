# Docker Compose Deployment

## Objective

Deploy a multi-container application using Docker Compose and automate the deployment with Ansible.

The application consists of:

* Nginx
* Traefik whoami
* A dedicated Docker network
* A persistent Docker volume
* A published HTTP port

The Compose application is deployed to both homelab nodes through Ansible.

---

## Architecture

```text
                    Docker Host
                        │
                  Docker Compose
                   homelab-app
                        │
             ┌──────────┴──────────┐
             │                     │
          nginx                  whoami
             │                     │
             └────── frontend ────┘
                    network
             
Host port 8080
      │
      ▼
nginx container :80
```

### Resources

| Resource                 | Purpose                                                                |
| ------------------------ | ---------------------------------------------------------------------- |
| `nginx`                  | Web server                                                             |
| `whoami`                 | Simple HTTP application used to demonstrate multi-container networking |
| `homelab-app_frontend`   | Compose-managed application network                                    |
| `homelab-app_nginx_data` | Persistent Nginx volume                                                |
| `8080:80`                | Publishes Nginx HTTP service on the Docker host                        |

---

## Compose Configuration

The Compose definition is stored at:

```text
docker/compose.yml
```

The configuration contains:

* `nginx` service using `nginx:latest`
* `whoami` service using `traefik/whoami:latest`
* `frontend` application network
* `nginx_data` persistent volume
* Nginx host port `8080` mapped to container port `80`
* Restart policies

The Compose project name used by the deployment results in Docker resources such as:

```text
homelab-app_frontend
homelab-app_nginx_data
```

---

## Ansible Deployment

The Ansible playbook is:

```text
playbooks/docker/compose.yml
```

The playbook performs three main operations:

1. Creates the application directory:

```text
/opt/homelab-app
```

2. Copies the repository Compose definition to:

```text
/opt/homelab-app/compose.yml
```

3. Deploys the application using:

```text
community.docker.docker_compose_v2
```

with:

```text
project_src: /opt/homelab-app
```

This separates the application definition stored in Git from the deployment location on the managed hosts.

---

## Deployment

The application is deployed with:

```bash
ansible-playbook playbooks/docker/compose.yml
```

The successful deployment produced:

```text
node1 : ok=4 changed=1 failed=0
node2 : ok=4 changed=1 failed=0
```

Both nodes successfully created and started the Compose application.

---

## Verification

Container status was verified with:

```bash
ansible homelab -b -m shell -a 'docker ps'
```

Both nodes reported:

```text
nginx    Up
whoami   Up
```

Nginx was published on:

```text
0.0.0.0:8080 -> 80/tcp
```

Local HTTP verification on node1 was performed with:

```bash
ansible node1 -b -m shell -a 'curl -I --max-time 5 http://127.0.0.1:8080'
```

The result was:

```text
HTTP/1.1 200 OK
Server: nginx/1.31.3
```

This verifies the complete local application path:

```text
Ansible
   ↓
Docker Compose
   ↓
Nginx container
   ↓
Published Docker port
   ↓
HTTP response
```

---

## Conflict Resolution

The first Compose deployment failed because an older manually-created container already used the name:

```text
whoami
```

Docker therefore reported:

```text
Conflict. The container name "/whoami" is already in use
```

The existing Docker state was inspected before removing anything.

The old standalone containers were:

```text
nginx
whoami
```

Both were attached to the old manually-created network:

```text
frontend
```

The old Nginx container also used:

```text
nginx_data
```

The Compose deployment used separate project-scoped resources:

```text
homelab-app_frontend
homelab-app_nginx_data
```

The old containers were removed:

```bash
docker rm -f nginx whoami
```

The old `frontend` network was inspected and confirmed to contain no containers:

```text
"Containers": {}
```

It was then removed:

```bash
docker network rm frontend
```

The old `nginx_data` volume was intentionally preserved because it contained persistent data previously used by the standalone Nginx container.

The Compose-managed network and volume were also preserved.

This avoided destructive cleanup such as:

```bash
docker system prune
```

or removing all volumes.

---

## Ansible and Docker Permissions

During verification, Docker commands executed directly as the `ubuntu` SSH user failed because `ubuntu` was not a member of the Docker group.

The Docker group contained:

```text
docker:x:999:deploy
```

The `deploy` user was a member of the Docker group.

Docker administration was therefore performed through Ansible privilege escalation:

```bash
ansible homelab -b ...
```

where `-b` enables Ansible `become`.

---

## Lessons Learned

### Docker Compose manages an application

Instead of manually creating every container and network:

```text
docker run
docker network create
docker volume create
```

Compose describes the desired multi-container application in one configuration.

### Compose does not automatically adopt arbitrary existing containers

A manually-created container named:

```text
whoami
```

can conflict with a Compose service requiring the same name.

Existing Docker resources should therefore be inspected before performing cleanup or migrating an application to Compose.

### Docker resources have different lifecycles

Containers, networks, and volumes should not be treated as interchangeable disposable resources.

In this ticket:

```text
old containers       → removed
old empty network    → removed
old persistent volume → preserved
Compose network      → preserved
Compose volume       → preserved
```

### Ansible provides the infrastructure automation layer

The architecture is:

```text
Git repository
      ↓
Ansible
      ↓
Docker Compose
      ↓
Docker application
```

Ansible is responsible for making the deployment reproducible across the homelab nodes.

---

## Ticket Status

**Status: Complete**

Completed:

* Docker Compose configuration
* Ansible Compose deployment
* Multi-container deployment
* Network configuration
* Persistent volume configuration
* Conflict investigation and safe cleanup
* Deployment verification
* Local HTTP verification on node1

### Follow-up

External access to port `8080` was not completed as part of this ticket.

Local HTTP access returned `HTTP/1.1 200 OK`, while direct access through the node's public IP did not complete. This is considered a separate cloud/networking investigation rather than a Docker Compose failure.

That investigation can be addressed later when networking or reverse-proxy work is introduced.
