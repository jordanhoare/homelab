# Ansible

## Prerequisites

### Unix environment
WSL on windows

### Ansible
A UNIX environment: https://docs.ansible.com/ansible/latest/installation_guide/index.html

### Python environemnt
The `netaddr` Python library needs to be installed on the control machine

### Kubernetes CLI
Install [kubectl](https://kubernetes.io/docs/tasks/tools/).

### SSH key pairs
Generate an RSA SSH key pair.
```zsh
ssh-keygen -t rsa -b 4096
```

To enable passwordless SSH authentication, you need to copy the public key (id_rsa.pub) to each managed node.
```zsh
ssh-copy-id node@192.168.238.137
```

## Get started

### Install some requirements for ansible
```zsh
ansible-galaxy install -r ./ansible/requirements.yml
```

## References

- https://github.com/techno-tim/k3s-ansible