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

### Kubectl
Copy your kube config locally:

```zsh
scp node@192.168.238.138:~/.kube/config ~/.kube/config
kubectl get nodes
```

### Kube-vip (load balancer)

Fetch the logs of the first kube-vip pod found in the kube-system namespace (extracting the pod name with grep and awk).

```zsh
kubectl logs $(kubectl get pods -n kube-system | grep kube-vip | awk '{print $1}') -n kube-system
```
