# 🏠 Homelab

**[Notes](#Notes) • [Topology](#🌐-network-topology) • [Hardware](#⚙️-hardware)**

[![documentation](https://img.shields.io/website?label=document&logo=gitbook&logoColor=white&style=flat-square&url=https://jordanhoare.github.io/homelab/)](https://jordanhoare.github.io/homelab/)
[![stars](https://img.shields.io/github/stars/jordanhoare/homelab?logo=github&logoColor=white&color=gold&style=flat-square)](https://github.com/jordanhoare/homelab)

<br>

## 📖 Overview

This is a mono repository for my home infrastructure and Kubernetes cluster. I aim to adhere to Infrastructure as Code (IaC) and GitOps practices with toolings from [Ansible](https://www.ansible.com/), [HashiCorp](https://www.hashicorp.com/), [Kubernetes](https://kubernetes.io/), [Helm](https://github.com/helm/helm), [Kustomize](https://kustomize.io/), [ArgoCD](https://github.com/argoproj/argo-cd), [Renovate](https://github.com/renovatebot/renovate), and [GitHub Actions](https://github.com/features/actions).

My homelab serves as a personal learning sandbox and a hub for new tech exploration. Meanwhile, self-hosting applications imbues a sense of ownership over the entire lifecycle of deploying and managing a stack of applications. This process requires me to understand various critical components such as backup plans, security measures, scalability, and simplicity of deployment and upkeep.

<br>

## 🤝 Principles

> [!NOTE]
>
> As much as possible I try to keep documentation for any decisions I make, or troubles I run into.. You can check out my notes related to my approach, design considerations and ideas, [here](https://jordanhoare.github.io/homelab/).

While developing my homelab I have a few principles that underpin my decisions:

- All applications and infrastructure (outside of my **cloud dependencies**) are run entirely on k3s. I have no VM's running auxiliary services such as DNS or storage
- I don't rely on persistent storage or stateful sets (at this moment in time)
- The cluster should be designed to tolerate at least one failure of a servers. 


<br>

### 📁 Directories

This Git repository contains the following directories:

```zsh
📁 homelab
├── 📁 ansible
│   ├── 📁 bootstrap
│   ├── 📁 kubernetes
│   └── 📁 storage
└── 📁 kubernetes
    └── 📁 ...
```

<br>

## ⚙️ Hardware


- 3 × Lenovo `M910Q 7500T`:
  - CPU: `Intel Core i5-7500T processor @ 2.70GHz`
  - RAM: `8GB`
  - SSD: `256GB`
- 1 × 5 port switch `TP-Link LS105G`

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

## 🔧 Core technology stack
All of these applications are run entirely on the k3s cluster.

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

## 🌐 Network topology

Here's a macroscopic overview of the state of my network, connecting all my devices together, including this lab.

![network](https://raw.githubusercontent.com/jordanhoare/homelab/main/docs/src/assets/drawings/topology.excalidraw.svg)
