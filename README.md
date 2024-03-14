# 🏠 Homelab

**[Features](#features) • [Topology](#🌐-network-topology) • [Hardware](#⚙️-hardware)**

[![document](https://img.shields.io/website?label=document&logo=gitbook&logoColor=white&style=flat-square&url=https%3A%2F%2Fhomelab.jordanhoare.com)](https://homelab.jordanhoare.com)
[![license](https://img.shields.io/github/license/jordanhoare/homelab?style=flat-square&logo=gnu&logoColor=white)](https://www.gnu.org/licenses/gpl-3.0.html)
[![stars](https://img.shields.io/github/stars/jordanhoare/homelab?logo=github&logoColor=white&color=gold&style=flat-square)](https://github.com/jordanhoare/homelab)


<br>

## 📖 Overview

My homelab serves as a personal learning sandbox and a hub for new tech exploration. Meanwhile, self-hosting applications imbues a sense of ownership over the entire lifecycle of deploying and managing a stack of applications. This process requires me to understand various critical components such as backup plans, security measures, scalability, and simplicity of deployment and upkeep.

My homelab is designed to tolerate at least one failure of a servers. It utilises an Elastic IP (EIP) so that the Kubernetes API (kube-apiserver) will remain accessible if one (or more of the servers) becomes unavailable. 

Given the available hardware and my goals - focusing on the command-line interface tools and embracing [Infrastructure as Code](https://en.wikipedia.org/wiki/Infrastructure_as_code) and [GitOps](https://www.weave.works/technologies/gitops) - I'll be aiming to automate provisioning, operating, and maintainance of my cluster & services with:
- ArgoCD for continuous deployment directly from your Git repository to your k3s cluster, automating application updates and ensuring your cluster's state matches the desired configuration in your version control.
- Ansible for automating the provisioning and configuration of your cluster nodes or any underlying infrastructure changes needed.
- Helm for package management, simplifying the deployment and management of applications on your Kubernetes cluster.


This project utilises 

<br>


## ⚙️ Hardware

- 3 × Lenovo `M910Q 7500T`:
    - CPU: `Intel Core i5-7500T processor @ 2.70GHz`
    - RAM: `8GB`
    - SSD: `256GB`
- 1 × 5 port switch `TP-Link LS105G`

<br>

## 🌐 Network topology

Here's a macroscopic overview of the state of my network, connecting all my devices together, including this lab.

![network](https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/drawings/topology.excalidraw.svg)


<br>

## Considerations

My current homelab setup serves as a foundational learning environment. It's designed with scalability in mind to eventually adopt practices and technologies more commonly seen in production-grade deployments. When transitioning from a homelab to a production-grade Kubernetes environment, there are key differences and scaling concerns to consider. This section acknowledges these while detailing the current homelab setup and planned upgrades.

### Highly available network load balancing
In production environments, HA network load balancing often involves multiple dedicated hardware or software solutions with automatic failover capabilities. My current homelab uses [kube-vip](https://kube-vip.io/) for simplicity and due to hardware constraints, which provides a single-VIP solution with failover to ensure service continuity. While adequate for small-scale setups, this approach lacks the redundancy of more sophisticated configurations such as dual load balancer setups used in larger clusters.

**Behavioral Note**: Traffic distribution methods vary between solutions like kube-vip, which directs traffic to a leader node, and [MetalLB](https://metallb.universe.tf/), which can balance traffic across multiple nodes.

### Highly Available Persistent Storage

Production Kubernetes clusters typically rely on robust, external persistent storage solutions, integrated through CSI drivers (e.g. AWS Elastic Block Store, GCE Persistent Disk, Azure Disk). In contrast, my homelab employs the built-in [etcd](https://docs.k3s.io/datastore/ha-embedded) of k3s for storage, suitable for learning and initial scaling. Future hardware enhancements will allow for the adoption of a dedicated storage system like [Longhorn](https://longhorn.io/), offering improved stability and features such as easy backup and restore, which are crucial for production use.

**Upgrade Path**: The transition from embedded etcd to a more resilient storage solution will be a critical step in maturing the cluster's capability to handle stateful workloads reliably.


### Network Segmentation and Zero-Trust Access
Future network enhancements will focus on implementing network segmentation to isolate the Kubernetes cluster. This isolation helps mitigate risks and is a step towards a zero-trust network model. Implementing VLANs or similar technologies will provide fine-grained control over network traffic, aligning with zero-trust principles by verifying and minimizing access scopes.

### Cloudflare Tunnel Integration
An additional layer of security and accessibility will be considered through [Cloudflare Tunnel](https://www.cloudflare.com/en-au/products/tunnel/). This will create secure and private connections between the cluster and the Cloudflare edge, enabling safe exposure of services without exposing the cluster directly to the public internet. This complements the zero-trust approach and provides secure access to services from anywhere, without VPN overhead.


<br>

## 🔧 Core technology stack

<div class="d-flex">
<table class="table table-white table-borderer border-dark w-auto align-middle">
    <tr>
        <th></th>
        <th>Name</th>
        <th>Description</th>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/ansible.svg"></td>
        <td><a href="https://www.ansible.com">Ansible</a></td>
        <td>Automate OS configuration, external services installation and k3s installation and bootstrapping</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/argocd.svg"></td>
        <td><a href="https://argoproj.github.io/cd">ArgoCD</a></td>
        <td>GitOps tool for deploying applications to Kubernetes</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/cloud-init.svg"></td>
        <td><a href="https://cloudinit.readthedocs.io/en/latest/">Cloud-init</a></td>
        <td>Automate OS initial installation</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/ubuntu.svg"></td>
        <td><a href="https://ubuntu.com/">Ubuntu</a></td>
        <td>Cluster nodes OS</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/k3s.svg"></td>
        <td><a href="https://k3s.io/">K3S</a></td>
        <td>Lightweight distribution of Kubernetes</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/traefik.svg"></td>
        <td><a href="https://traefik.io/">Traefik</a></td>
        <td>Kubernetes Ingress Controller (alternative)</td>
    </tr>   
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/vault.svg"></td>
        <td><a href="https://www.vaultproject.io/">Hashicorp Vault</a></td>
        <td>Secrets Management solution</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/external-secrets.svg"></td>
        <td><a href="https://external-secrets.io/">External Secrets Operator</a></td>
        <td>Sync Kubernetes Secrets from Hashicorp Vault</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/prometheus.svg"></td>
        <td><a href="https://prometheus.io/">Prometheus</a></td>
        <td>Metrics monitoring and alerting</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/grafana.svg"></td>
        <td><a href="https://grafana.com/oss/grafana/">Grafana</a></td>
        <td>Monitoring Dashboards</td>
    </tr>
</table>
</div>

<br>


## ☁️ Cloud dependencies

While most of my infrastructure and workloads are self-hosted I do rely upon the cloud for certain key parts of my setup to avoid having to deal with chicken and the egg scenarios.

<div class="d-flex">
<table class="table table-white table-borderer border-dark w-auto align-middle">
    <tr>
        <th></th>
        <th>Name</th>
        <th>Description</th>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/vault.svg"></td>
        <td><a href="https://www.vaultproject.io/">Hashicorp Vault</a></td>
        <td>Secrets Management solution</td>
    </tr>
    <tr>
        <td><img width="32" src="https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/logos/github.svg"></td>
        <td><a href="https://github.com/">GitHub</a></td>
        <td>Hosting this repository and continuous integration/deployments</td>
    </tr>
</table>
</div>

<br>

## Quick start

### SSH key pairs
```zsh
ssh-keygen -t rsa -b 4096 # ignore if already exists
ssh-copy-id node@192.168.238.137
```

### Bootstrap
```zsh
task ansible:run namespace=bootstrap playbook=initialise
```

### Kubernetes
```zsh
task ansible:run namespace=kubernetes playbook=initialise
```
