# Ansible Verification

After running an Ansible playbook, verify that the target system reached the
desired state instead of relying only on the playbook output.

## Basic Checks

Check Ansible connectivity:

```bash
ansible all -m ping
