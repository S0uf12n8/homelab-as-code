# Target Architecture

## Purpose

The goal of this project is to build a complete production-like infrastructure using Infrastructure as Code (IaC). Every component of the platform should be reproducible from source code without requiring manual configuration.

The project is designed as a learning platform to develop practical skills in Linux administration, automation, containerization, Kubernetes, GitOps, observability, and infrastructure security.

The guiding principle of this repository is:

> If it is not stored in Git, it does not exist.

---

# Project Objectives

The platform should demonstrate the complete lifecycle of a modern infrastructure environment.

The final environment will provide:

- Automated Linux provisioning
- Secure server configuration
- Container orchestration
- Infrastructure as Code
- GitOps deployment
- Centralized logging
- Metrics collection
- Security monitoring
- Continuous Integration
- Complete documentation

---

# High-Level Architecture

```
                           GitHub Repository
                                   │
                    Git Push / Pull Requests
                                   │
                                   ▼
                           GitHub Actions
                                   │
                      Validate infrastructure
                                   │
                                   ▼
                              Ansible
                                   │
                 Provision Linux infrastructure
                                   │
         ┌─────────────────────────┴─────────────────────────┐
         │                                                   │
         ▼                                                   ▼
   Ubuntu Node 1                                      Ubuntu Node 2
         │                                                   │
         └─────────────────────────┬─────────────────────────┘
                                   │
                                   ▼
                               k3s Cluster
                                   │
             ┌──────────────┬──────────────┬──────────────┐
             │              │              │
             ▼              ▼              ▼
         Applications    Monitoring     Security
             │              │              │
             ▼              ▼              ▼
        Caddy / Apps   Prometheus     Suricata
                       Grafana
                       Loki
```

---

# Architecture Layers

The project is divided into multiple logical layers.

## Layer 1 — Infrastructure

Responsible for creating and configuring Linux servers.

Technologies:

- Ubuntu Server
- SSH
- Ansible

Responsibilities:

- Package installation
- User management
- SSH configuration
- Firewall
- System hardening

---

## Layer 2 — Container Platform

Responsible for running workloads.

Technologies:

- Docker
- Docker Compose
- k3s

Responsibilities:

- Container execution
- Service deployment
- Internal networking

---

## Layer 3 — GitOps

Responsible for keeping infrastructure synchronized with Git.

Technologies:

- Argo CD

Responsibilities:

- Continuous deployment
- Drift detection
- Automated synchronization

---

## Layer 4 — Observability

Responsible for monitoring the platform.

Technologies:

- Prometheus
- Grafana
- Loki

Responsibilities:

- Metrics
- Dashboards
- Log aggregation

---

## Layer 5 — Security

Responsible for monitoring infrastructure security.

Technologies:

- Suricata
- Fail2Ban
- nftables

Responsibilities:

- Intrusion detection
- Attack visualization
- Network monitoring

---

# Repository Philosophy

This project follows several engineering principles.

## Infrastructure as Code

Every infrastructure change must be reproducible through code.

Manual configuration is avoided whenever possible.

---

## Idempotency

Running the automation multiple times should always produce the same system state.

---

## Version Control

Every infrastructure modification is tracked through Git using feature branches and pull requests.

---

## Documentation

Every completed feature is documented before moving to the next milestone.

The documentation explains:

- Why the feature exists
- How Linux implements it
- How Ansible automates it
- How it was verified

---

# Current Progress

Current phase:

- ✅ Phase 0 — Environment setup
- 🚧 Phase 1 — Linux hardening and automation
- ⏳ Phase 2 — Kubernetes and GitOps
- ⏳ Phase 3 — Observability
- ⏳ Phase 4 — CI/CD and documentation

---

# Long-Term Goal

The final objective is to rebuild the complete infrastructure from a fresh operating system using a minimal number of commands.

The infrastructure should be reproducible, secure, documented, and maintained entirely through Git.
