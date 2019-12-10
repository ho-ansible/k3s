# Ansible Role: k3s
Minimal distribution of Kubernetes container orchestration system.

+ Downloads `k3s` **binary** from GitHub
  (the installer bash script is not used)
+ Creates **systemd** service from template
+ Saves token from server in inventory **group var**
+ Copies token to node

## Requirements
Debian stable amd64

## Role Variables
+ `k3s_mode` (default: `agent`): either `server` or `agent`
+ `k3s_group` (default: `k3s`): ansible inventory group
  containing all hosts in this k3s cluster (sharing a common token)
+ `k3s_url` (default: https://localhost:6443): how the agent is to
  access the server.
+ `k3s_env` (default: empty dict): additional environment variables
  for k3s (the binary, **not** the installer).
+ `k3s_opts` (default: empty string): additional command-line args.
  In case of conflict, `opts` overrides `env`.
  See [k3s docs](https://rancher.com/docs/k3s/latest/en/installation/install-options/).
+ `k3s_version` (default: latest Github release): which version
  of k3s to install.

## Dependencies
+ ho-ansible.systemd

## License
MIT

## Author Information
Sean Ho, https://github.com/ho-ansible/

This role borrows heavily from the [unofficial k3s ansible role](https://github.com/rancher/k3s/tree/master/contrib/ansible),
with the following changes:
+ Ignore CentOS (sorry!)
+ Focus on amd64 (for now)
+ Consolidate server/node systemd service files into one using a templated env file
+ Pass token in file rather than command line arg
+ Token saved in inventory group var rather than cached fact of a single server
+ Omit copying a kube config file to user's dir
