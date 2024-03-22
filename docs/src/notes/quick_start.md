## Quick start

### SSH key pairs
```zsh
ssh-keygen -t rsa -b 4096 # ignore if already exists
ssh-copy-id node@192.168.238.139
```

### Bootstrap
```zsh
task ansible:run namespace=bootstrap playbook=initialise
```

### Kubernetes
```zsh
task ansible:run namespace=kubernetes playbook=initialise
```

### Kubectl
Copy your kube config locally:

```zsh
scp node@192.168.238.139:~/.kube/config ~/.kube/config
```

### Create ArgoCD...
```zsh
kubectl apply -k ./kubernetes/argocd
```
