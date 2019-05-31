# Ansible Role: k3s
Minimal distribution of Kubernetes container orchestration system.

Uses k3s' own install script to setup the default configuration and systemd service.

## Requirements
Only tested on Debian stable, for now.

## Role Variables
+ `k3s_role` (default: `master`): whether host is to be a Kubernetes
  master or `agent`.
+ `k3s_env` (default: none): extra environment variables passed to the
  install script, e.g., `INSTALL_K3S_EXEC="--disable-agent"`
  to exclude host from set of worker nodes.
+ `k3s_version` (default: from latest Github release): which version
  of k3s to install.
+ `k3s_install_tmp` (default: /tmp/k3s_install.tmp): where to save the
  k3s install script (just for installation, not the binary).

### Role Variables for Agents
+ `k3s_master`: inventory name referring to master node.
  The master stores the authentication token as a host fact.
+ `k3s_url` (default: https://localhost:6443): how the agent is to
  access the master.

## Dependencies
None.

## Example Playbook

```
- hosts: k3s
  roles:
    - { role: ho-ansible.k3s }
```

## License
MIT

## Author Information
Sean Ho, https://github.com/ho-ansible/
