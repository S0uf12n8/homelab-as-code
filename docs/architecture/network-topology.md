# Network Topology

## Purpose

This document describes the network layout of the homelab environment.

The topology will evolve throughout the project as new services and infrastructure components are introduced. This document represents the intended design and will be updated as the project grows.

---

# Current Topology

At the current stage of the project, two Ubuntu servers are hosted in Oracle Cloud Infrastructure (OCI). The servers are managed remotely from a local development machine using SSH and Ansible.

```
                 Local Development Machine
             (Windows 11 + WSL Ubuntu + Ansible)
                          │
                          │ SSH
                          ▼
                ┌─────────────────────┐
                │ Oracle Cloud (OCI)  │
                └─────────────────────┘
                     │           │
                     │           │
                ┌────▼────┐ ┌────▼────┐
                │ Node 1  │ │ Node 2  │
                │ Ubuntu  │ │ Ubuntu  │
                └─────────┘ └─────────┘
```

---

# Planned Topology

As the project progresses, the infrastructure will evolve into a production-like platform.

```
                     GitHub
                        │
                        │
                 GitHub Actions
                        │
                        ▼
                    Ansible
                        │
        ┌───────────────┴───────────────┐
        │                               │
   Ubuntu Node 1                  Ubuntu Node 2
        │                               │
        └───────────────┬───────────────┘
                        │
                     k3s Cluster
                        │
        ┌───────────────┼────────────────┐
        │               │                │
     Applications   Monitoring      Security
        │               │                │
        ▼               ▼                ▼
      Caddy      Prometheus        Suricata
                 Grafana           Fail2Ban
                 Loki
```

---

# Node Responsibilities

## Local Development Machine

Responsibilities:

- Write infrastructure code
- Execute Ansible playbooks
- Manage Git repository
- Connect to remote servers
- Develop and test automation

---

## Node 1

Planned responsibilities:

- Primary infrastructure node
- Kubernetes control plane
- Core services
- Reverse proxy

---

## Node 2

Planned responsibilities:

- Kubernetes worker
- Application workloads
- Monitoring services
- Security tooling

---

# Communication

Infrastructure management uses SSH.

Ansible communicates with every managed node through SSH using public key authentication.

No agents are installed on managed hosts.

---

# Security Principles

The network follows several security principles:

- SSH key authentication only
- Principle of least privilege
- Firewall enabled on every node
- Automatic security updates
- Centralized monitoring
- Infrastructure managed through code

---

# Future Expansion

The architecture is intentionally modular.

Future improvements may include:

- Additional worker nodes
- Private networking
- Load balancing
- High availability
- Secrets management
- Dedicated monitoring node
- Continuous deployment
- Infrastructure provisioning with OpenTofu

These additions should require minimal changes to the existing automation due to the project's Infrastructure as Code approach.
