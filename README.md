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

### ✅ Completed

- Oracle Cloud Free Tier environment
- SSH key authentication
- Initial Ansible project structure
- Base package installation
- Time synchronization (Chrony)
- Linux user management

### 🚧 In Progress

- SSH key deployment with Ansible
- Linux hardening

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
inventory/
playbooks/
docs/
README.md
```

---

## Roadmap

- [x] Phase 0 — Environment setup
- [ ] Phase 1 — Linux hardening with Ansible
- [ ] Phase 2 — Docker & k3s
- [ ] Phase 3 — GitOps
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

## Philosophy

This project is focused on learning engineering practices rather than simply deploying software. Every component is built incrementally, documented, reproducible, and managed as Infrastructure as Code.
