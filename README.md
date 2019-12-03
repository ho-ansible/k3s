# Ansible Role: k3s
Minimal distribution of Kubernetes container orchestration system.

Only the `k3s` binary is downloaded from GitHub.

## Requirements
Debian stable amd64

## Role Variables
+ `k3s_servers` (default: `k3s_server`): ansible inventory group
  containing hosts that are **servers** rather than worker **nodes**.
+ `k3s_opts` (default: none): dict of additional options to pass to k3s
  on startup.  See [k3s docs](https://rancher.com/docs/k3s/latest/en/installation/install-options/).
+ `k3s_version` (default: from latest Github release): which version
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
+ Consolidate server/node systemd service files into one using a templated env file
+ Pass token in file rather than command line arg
+ Omit copying a kube config file to user's dir
