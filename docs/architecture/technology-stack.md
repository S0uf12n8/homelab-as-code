# Technology Stack

## Purpose

This document describes the technologies used throughout the project and explains why each one was selected.

The objective is not to use every available tool, but to build a practical, production-like environment while understanding the purpose of every component.

---

# Core Operating System

## Ubuntu Server

### Purpose

Ubuntu Server provides the operating system for every managed node.

### Why Ubuntu?

- Stable Long-Term Support (LTS) releases
- Excellent documentation
- Large community
- Native support from Ansible
- Widely used in production environments

---

# Version Control

## Git

### Purpose

Track every infrastructure change.

### Why Git?

- Complete history of every modification
- Easy collaboration
- Branch-based development
- Rollback capability
- Industry standard

---

## GitHub

### Purpose

Remote repository hosting and collaboration.

### Planned Uses

- Source code hosting
- Pull requests
- Issue tracking
- GitHub Actions
- Release tags
- Project documentation

---

# Automation

## Ansible

### Purpose

Automate server provisioning and configuration.

### Why Ansible?

- Agentless architecture
- SSH-based communication
- Human-readable YAML syntax
- Idempotent execution
- Large ecosystem of official modules

### Responsibilities

- Package installation
- User management
- SSH configuration
- Firewall configuration
- Docker installation
- Service deployment

---

# Container Platform

## Docker

### Purpose

Run applications inside isolated containers.

### Why Docker?

- Consistent deployments
- Lightweight virtualization
- Easy service management
- Industry standard

---

## Docker Compose

### Purpose

Manage multi-container applications.

### Planned Uses

- Reverse proxy
- Monitoring stack
- Utility services

---

# Container Orchestration

## k3s

### Purpose

Provide a lightweight Kubernetes cluster.

### Why k3s?

- Designed for resource-constrained environments
- Fully Kubernetes compatible
- Simple installation
- Production-ready
- Ideal for homelabs

---

# GitOps

## Argo CD

### Purpose

Continuously synchronize Kubernetes with Git.

### Why Argo CD?

- Declarative deployments
- Automatic synchronization
- Drift detection
- Easy rollback

---

# Reverse Proxy

## Caddy

### Purpose

Expose web services securely.

### Why Caddy?

- Automatic HTTPS
- Simple configuration
- Fast deployment
- Excellent for learning

---

# Monitoring

## Prometheus

### Purpose

Collect infrastructure metrics.

### Responsibilities

- CPU monitoring
- Memory monitoring
- Disk usage
- Node health
- Kubernetes metrics

---

## Grafana

### Purpose

Visualize collected metrics.

### Responsibilities

- Dashboards
- Infrastructure overview
- Performance analysis
- Alert visualization

---

## Loki

### Purpose

Centralize logs from servers and containers.

### Responsibilities

- Log aggregation
- Log searching
- Troubleshooting
- Security investigation

---

# Security

## Suricata

### Purpose

Network Intrusion Detection System (IDS).

### Why Suricata?

This project focuses on combining DevOps with networking and security.

Suricata provides:

- Deep packet inspection
- Threat detection
- Network visibility
- Security event generation

These events will later be visualized in Grafana using Loki.

---

## nftables

### Purpose

Linux firewall.

### Responsibilities

- Packet filtering
- Network segmentation
- Access control

---

## Fail2Ban

### Purpose

Protect services against brute-force attacks.

### Responsibilities

- Detect repeated authentication failures
- Automatically ban malicious IP addresses

---

# Infrastructure Monitoring

## Lynis

### Purpose

Perform security auditing on Linux systems.

### Responsibilities

- Security assessment
- Hardening recommendations
- Compliance checks
- Before/after comparisons

Lynis will be executed before and after the hardening process to measure improvements.

---

# Continuous Integration

## GitHub Actions

### Purpose

Automate repository validation.

### Planned Responsibilities

- Ansible linting
- Infrastructure validation
- Future automated testing
- Deployment workflows

---

# Documentation

## Markdown

Project documentation is written entirely in Markdown.

### Why Markdown?

- Human-readable
- Version-controlled
- Native GitHub support
- Lightweight
- Easy to maintain

---

# Design Principles

The technologies selected for this project follow several principles:

- Open source whenever possible
- Production relevance
- Active community support
- Strong documentation
- Widely adopted in the industry
- Suitable for learning real-world DevOps practices

Technology choices may evolve throughout the project as new requirements emerge or better solutions are identified.
