# UFW Firewall Configuration

## Objective

Configure a host-based firewall using UFW to provide an additional security layer for all managed Ubuntu servers.

## Implementation

The Ansible playbook performs the following tasks:

- Installs the UFW package.
- Allows OpenSSH connections.
- Sets the default incoming policy to `deny`.
- Sets the default outgoing policy to `allow`.
- Enables the firewall.

## Verification

The following verification steps were completed:

- Syntax check passed.
- Playbook executed successfully on all hosts.
- Verified UFW status using:

```bash
sudo ufw status verbose
```

Output confirmed:

- Firewall active.
- Default incoming policy: deny.
- Default outgoing policy: allow.
- OpenSSH allowed.

The playbook was executed a second time and returned:

```
changed=0
```

confirming idempotency.

## Problems Encountered

### Safe execution

Before enabling the firewall, two SSH sessions were kept open to prevent accidental lockout if firewall rules were misconfigured.

### Execution order

The firewall was configured in the following order:

1. Install UFW.
2. Allow OpenSSH.
3. Configure default policies.
4. Enable UFW.

This prevents locking administrators out of the server.

## Lessons Learned

- A host firewall provides defense in depth alongside cloud security groups.
- The order of firewall configuration is critical.
- Always verify SSH connectivity after enabling a firewall.
- Idempotency confirms the playbook only changes the system when necessary.
