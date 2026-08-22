# Homelab-as-Code

> A production-inspired homelab built with Infrastructure as Code to learn and demonstrate modern DevOps practices.

## Project Goal

This repository documents my journey from Linux system administration to a fully automated infrastructure platform.

The objective is to provision and manage servers entirely through code, gradually evolving into a production-like environment featuring:

- Linux hardening with Ansible
- Containerized workloads with Docker
- Kubernetes (k3s)
- GitOps with Argo CD
- Observability with Prometheus, Grafana and Loki
- Network security monitoring with Suricata
- CI/CD with GitHub Actions

Every change is version-controlled. If it's not in Git, it doesn't exist.

---

## Current Status

### ✅ Phase 0 – Environment Setup

**Completed:**

### ✅ Phase 1 – Linux Hardening

**Completed:**

Implemented:

- Base packages
- Chrony time synchronization
- Linux user management
- SSH public key deployment
- Passwordless sudo
- SSH hardening
- UFW firewall
- Automatic security updates
- Fail2Ban protection

**Phase 1 Status:** ✅ Complete

### 🚧 Phase 2 – Docker Platform

**Completed:**

- Docker Engine installation
- Docker user permissions
- First container deployment
- Docker container lifecycle and logs
- Docker volume management
- Docker network management
- Multi-container deployment
- Docker Compose deployment
- Ansible automation of Docker Compose
- Nginx reverse proxy
- Docker container health checks
- Deployment and configuration verification

**Phase 2 Status:** ✅ Complete

### ✅ Phase 3 – CI/CD

**Completed:**

- GitHub Actions CI workflow
- Ansible syntax validation in CI
- Ansible-lint validation in CI
- Docker image build in CI
- Multi-tag Docker images using Git commit SHA
- GitHub Container Registry (GHCR)
- ARM64 Docker image builds for node2
- Automated SSH access from GitHub Actions to node2
- Ansible-based Docker deployment
- Deployment using the exact Git commit SHA
- End-to-end CI/CD verification

**Phase 3 Status:** ✅ Complete
---

## Target Architecture

```
GitHub
   │
Ansible
   │
┌───────────────┐
│ Oracle Cloud  │
├──────┬────────┤
│Node 1│Node 2  │
└──┬───┴───┬────┘
   │       │
 Docker / k3s
      │
   Argo CD
      │
Prometheus + Loki + Grafana
      │
 Suricata IDS
```

---

## Repository Structure

```
homelab-as-code/
├── docs/
├── inventory/
├── playbooks/
├── ansible.cfg
└── README.md
```

---

## Roadmap

- [x] Phase 0 — Environment setup
- [x] Phase 1 — Linux hardening with Ansible
- [x] Phase 2 — Docker & k3s
- [x] Phase 3 — GitOps
- [ ] Phase 4 — Observability
- [ ] Phase 5 — Security monitoring
- [ ] Phase 6 — CI/CD

---

## Technologies

- Ubuntu Server
- Oracle Cloud
- Ansible
- Docker
- Kubernetes (k3s)
- Argo CD
- Prometheus
- Grafana
- Loki
- Suricata
- GitHub Actions

---
## Ansible Workflow

The project follows an incremental Infrastructure-as-Code workflow:

1. Provision and prepare the infrastructure.
2. Establish SSH-based access.
3. Configure systems using Ansible.
4. Apply security hardening.
5. Deploy and manage services.
6. Document and verify changes.

All infrastructure changes should be reproducible through Ansible whenever practical.
---
## Philosophy

This project is focused on learning engineering practices rather than simply deploying software. Every component is built incrementally, documented, reproducible, and managed as Infrastructure as Code.
Manual configuration is avoided whenever possible to ensure the infrastructure remains reproducible
