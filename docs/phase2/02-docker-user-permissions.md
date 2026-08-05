# Docker User Permissions

## Objective

Allow the existing `deploy` user to interact with Docker without requiring `sudo`.

This playbook ensures that the `deploy` user is a member of the `docker` group while preserving all existing group memberships.

---

## Engineering Problem

By default, Docker communicates through the Unix socket:

```text
/var/run/docker.sock
```

The socket is owned by:

```text
root:docker
```

Only:

- root
- users belonging to the `docker` group

can communicate with the Docker daemon.

Without being a member of the `docker` group, the following command will fail:

```bash
docker ps
```

with a permission denied error.

---

## Linux Concepts

During this ticket, the following Linux concepts were learned:

- Linux groups
- User group membership
- Docker socket permissions
- `usermod`
- Difference between modifying and creating a user

---

## Manual Linux Command

```bash
sudo usermod -aG docker deploy
```

### Command Breakdown

| Command | Meaning |
|----------|---------|
| `usermod` | Modify an existing user |
| `-a` | Append the new group without removing existing groups |
| `-G docker` | Add the user to the `docker` group |
| `deploy` | User being modified |

---

## Why `-a` Matters

Correct:

```bash
sudo usermod -aG docker deploy
```

Incorrect:

```bash
sudo usermod -G docker deploy
```

Without the `-a` option, Linux replaces the user's supplementary groups.

Example:

Before:

```text
deploy
sudo
adm
```

After:

```text
docker
```

The user would lose access to groups such as `sudo`.

---

## Linux to Ansible Mapping

| Linux | Ansible |
|--------|----------|
| `usermod` | `ansible.builtin.user` |
| `deploy` | `name: deploy` |
| `-G docker` | `groups: docker` |
| `-a` | `append: true` |

---

## Ansible Module

| Module | Purpose |
|----------|---------|
| `ansible.builtin.user` | Modify an existing Linux user and manage group membership |

---

## Playbook

```text
playbooks/docker-user.yml
```

---

## Tasks Performed

1. Modify the existing `deploy` user.
2. Add the user to the `docker` group.
3. Preserve all existing group memberships.

---

## Verification Commands

Verify group membership:

```bash
groups deploy
```

or

```bash
id deploy
```

Expected output should include:

```text
docker
```

Verify Docker access:

```bash
docker ps
```

The command should execute successfully without `sudo`.

> **Note:** If the `docker` group was added during the current session, log out and log back in before testing. Group membership changes apply only to new login sessions.

---

## Lessons Learned

- Docker permissions are controlled through Linux group membership.
- The Docker daemon communicates through `/var/run/docker.sock`.
- Users must belong to the `docker` group to interact with Docker without root privileges.
- `append: true` is the Ansible equivalent of the Linux `-a` option.
- This ticket modifies an existing user; it does not create one.

---

## Common Mistakes

❌ Using:

```yaml
name: docker
```

instead of:

```yaml
name: deploy
```

---

❌ Forgetting:

```yaml
append: true
```

---

❌ Using:

```yaml
groups: sudo
```

instead of:

```yaml
groups: docker
```

---

❌ Adding unnecessary parameters such as:

```yaml
shell:
create_home:
```

Those parameters are only required when creating a new user, not when modifying an existing one.

---

## Key Takeaways

- One ticket should have one responsibility.
- Modify existing users instead of recreating them.
- Always preserve existing group memberships using `append: true`.
- Verify changes from Linux, not only from Ansible output.
