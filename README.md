\# Homelab-as-Code



A single Git repository that provisions a small production-like platform from zero — hardened Linux, containers, a k3s cluster managed by GitOps, full observability, and a network security monitoring pipeline (Suricata → Loki → Grafana) as the distinctive workload.



Nothing is done by hand. If it's not in Git, it doesn't exist.



\## Status



\*\*Phase 0 — Setup \& prerequisites\*\* (in progress)



\- \[x] SSH key generated, 2 Oracle Cloud ARM nodes provisioned (node1, node2)

\- \[x] SSH access confirmed to both nodes

\- \[ ] Repo public on GitHub

\- \[ ] Phase 1 — Ansible hardening



\## Architecture (target)



\- 2× Oracle Cloud Ampere A1 ARM instances (Always Free tier), Ubuntu 24.04

\- Ansible for configuration management (Phase 1)

\- k3s + Argo CD for orchestration/GitOps (Phase 2)

\- Prometheus, Grafana, Loki for observability (Phase 3)

\- Suricata IDS pipeline feeding Loki/Grafana as the security-monitoring differentiator (Phase 3)

\- GitHub Actions for CI/CD (Phase 4)



Full roadmap: see project notes (not yet published here).



\## Why this project



Most junior DevOps candidates come from dev backgrounds and are weak on networking. This project inverts that — it demonstrates infrastructure automation, orchestration, and observability skills on top of an existing networking background, differentiated by a real intrusion-detection pipeline rather than a generic app deployment.

