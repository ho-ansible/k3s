# Ansible Role: k3s
Minimal distribution of Kubernetes container orchestration system.

+ Downloads `k3s` **binary** from GitHub
  (the installer bash script is not used)
+ Creates **systemd** service from template
+ Saves token from server in inventory **group var**
+ Copies token to node

Consequently, it's best first to apply this role for the k3s server node
so it can generate and save the token, then apply the role for the k3s 
agents using the saved token.

To manage the cluster, copy `/etc/rancher/k3s/k3s.yaml` to your local
workstation as `~/.kube/config`, and install kubectl (e.g., using the
k3s binary).

## Requirements
Debian stable amd64

## Role Variables
+ `k3s_mode` (default: `agent`): either `server` or `agent`
+ `k3s_group` (default: `k3s`): ansible inventory group
  containing all hosts in this k3s cluster (sharing a common token)
+ `k3s_url` (default: none): API endpoint, e.g., `https://server:6443`.
  Leave unset for server.
  If using HA etcd, start first server with this empty, e.g., `-e k3s_url=`,
  then after the URL is resolvable by another server, restart with URL set.
+ `k3s_labels`: list of `key=value` pairs to set as immutable node labels
+ `k3s_taints`: list of values to set as immutable node taints
+ `k3s_config`: dict of config options for `/etc/rancher/k3s/config.yaml`
+ `k3s_interface` (default: `ansible_default_ipv4.interface`): 
  Firewall rules allowing cluster-internal traffic are enabled for this 
  interface.
+ `k3s_ip` (default: follows `k3s_interface`): Address on above interface.
+ `k3s_external_ip`: address for NodePort etc.
+ `k3s_resolv_conf`: contents of custom `resolv.conf` for kubelet
+ `k3s_kubelet_conf`: contents of custom config file for kubelet
+ `k3s_iptables`: list of additional firewall rules
+ `k3s_version` (default: latest Github release): which version to install.

### Server Variables
The following role variables only apply to k3s servers / control nodes:

+ `k3s_tls_san`: list of SNI domain names to add to TLS cert
+ `k3s_disable`: list of services prepackaged with k3s to disable

### Managed Variables
+ `k3s_token`: auto-generated if not present, and stored in a group var
  for the entire `k3s_group`.

## Playbooks
+ `main.yml`: apply role
  + `k3s_rollout_size`: sets `serial` number of hosts (default all) to run at a time.
+ `uninstall.yml`: remove. Run this before removing config from inventory.
  Does not stop pods or remove `/var/lib/{rancher,kubelet,cni}`.

## Dependencies
+ [ho-ansible.systemd](https://github.com/ho-ansible/systemd)

## License
+ Ansible role licensed [MIT](LICENSE)

## Author Information
+ Ansible role by [Sean Ho](https://github.com/ho-ansible/)

## History
This role borrows heavily from the [unofficial k3s ansible role](https://github.com/rancher/k3s/tree/master/contrib/ansible),
with the following changes:
+ Ignore CentOS (sorry!)
+ Focus on amd64 (for now)
+ Consolidate server/node systemd service files into one using a templated env file
+ Pass token in file rather than command line arg
+ Token saved in inventory group var rather than cached fact of a single server
+ Omit copying a kube config file to user's dir
