# Set up

Ubuntu 20.04

It is recommended to turn off ufw (uncomplicated firewall):

ufw disable

If you wish to keep ufw enabled, by default, the following rules are required:

If you plan on achieving high availability with embedded etcd, server nodes must be accessible to each other on ports 2379 and 2380.

This are the instructions to quickly deploy Kuberentes Pi-cluster using the following tools:

cloud-init: to automate initial OS installation/configuration on each node of the cluster
Ansible: to automatically configure cluster nodes, install and configure external services (DNS, DHCP, Firewall, S3 Storage server, Hashicorp Vautl) install K3S, and bootstraping cluster through installation and configuration of ArgoCD
Argo CD: to automatically deploy Applications to Kuberenetes cluster from manifest files in Git repository.


Protocol	Port	Source	Destination	Description
TCP	2379-2380	Servers	Servers	Required only for HA with embedded etcd
TCP	6443	Agents	Servers	K3s supervisor and Kubernetes API Server
TCP	10250	All nodes	All nodes	Kubelet metrics
ufw allow 6443/tcp #apiserver
ufw allow from 10.42.0.0/16 to any #pods
ufw allow from 10.43.0.0/16 to any #services


Typically, all outbound traffic is allowed.