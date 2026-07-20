# Repository Structure

## Purpose

This document describes the organization of the repository and the purpose of each directory.

The project is structured to separate infrastructure code, documentation, inventories, and automation into logical components. As the project grows, this organization helps maintain readability, scalability, and reproducibility.

---

# Current Repository Layout

```
homelab-as-code/
├── docs/
├── inventory/
├── playbooks/
├── ansible.cfg
├── README.md
└── site.yml
```

---

# Directory Overview

## docs/

Contains all project documentation.

Documentation is organized by topic and project phase rather than by date.

Examples include:

- Architecture
- Phase documentation
- Troubleshooting guides
- Reference material

---

## inventory/

Contains the Ansible inventory.

The inventory defines all managed infrastructure, including servers, groups, variables, and connection information.

Example:

```
inventory/
├── hosts.yml
└── group_vars/
```

The inventory allows Ansible to know:

- Which servers exist
- How to connect to them
- Which variables belong to each group

---

## playbooks/

Contains Ansible playbooks.

Each playbook automates a specific infrastructure task.

Examples:

- Base package installation
- Time synchronization
- User management
- SSH configuration
- Firewall configuration

As the project evolves, these playbooks will be refactored into reusable Ansible roles.

---

## ansible.cfg

The Ansible configuration file.

This file defines project-wide behavior, including:

- Inventory location
- SSH configuration
- Default options

Keeping the configuration inside the repository ensures every contributor uses the same Ansible settings.

---

## site.yml

The main Ansible entry point.

Instead of executing individual playbooks manually, `site.yml` will include every role required to build a complete server.

The long-term objective is:

```bash
ansible-playbook site.yml
```

A single command should fully provision a fresh Linux server.

---

## README.md

The public entry point of the project.

The README explains:

- Project objectives
- Architecture
- Technologies
- Roadmap
- Current progress

It is intended for visitors, recruiters, and collaborators.

Detailed technical explanations are stored inside the `docs/` directory.

---

# Branch Strategy

Development follows a feature-branch workflow.

Examples:

```
main

feature/base-packages
feature/user-management
feature/ssh-key-management
feature/firewall
feature/docker
```

Each branch focuses on a single feature and is merged into `main` after testing.

Tags are created to mark significant project milestones.

---

# Documentation Strategy

Every completed feature should include documentation before it is merged into the main branch.

Each document should answer the following questions:

1. What problem is being solved?
2. Why is the feature important?
3. How would Linux implement it manually?
4. How does Ansible automate it?
5. How was the configuration verified?

This approach keeps the documentation synchronized with the implementation.

---

# Long-Term Repository Structure

The repository will gradually evolve into the following structure:

```
homelab-as-code/
├── docs/
├── inventory/
├── playbooks/
├── roles/
├── group_vars/
├── host_vars/
├── templates/
├── files/
├── handlers/
├── vars/
├── ansible.cfg
├── site.yml
└── README.md
```

The introduction of Ansible roles will improve modularity and make the infrastructure easier to maintain as additional services are added.
