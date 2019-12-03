# Ansible Role: k3s
Minimal distribution of Kubernetes container orchestration system.

+ Downloads `k3s` binary from GitHub
+ Creates systemd service from template
+ Saves token from server in hostvar
+ Copies token from server hostvar to node

## Requirements
Debian stable amd64

## Role Variables
+ `k3s_servers` (default: `k3s_server`): ansible inventory group
  containing hosts that are **servers** rather than worker **nodes**.
+ `k3s_env` (default: empty): dict of additional environment variables
  for k3s (the binary, **not** the installer).
  See [k3s docs](https://rancher.com/docs/k3s/latest/en/installation/install-options/).
+ `k3s_version` (default: latest Github release): which version
  of k3s to install.
+ `k3s_url` (default: https://localhost:6443): how the agent is to
  access the server.

## Dependencies
None.

## License
MIT

## Author Information
Sean Ho, https://github.com/ho-ansible/

This role borrows heavily from the [unofficial k3s ansible role](https://github.com/rancher/k3s/tree/master/contrib/ansible):
+ Ignore CentOS (sorry!)
+ Focus on amd64 (for now)
+ Consolidate server/node systemd service files into one using a templated env file
+ Pass token in file rather than command line arg
+ Omit copying a kube config file to user's dir
