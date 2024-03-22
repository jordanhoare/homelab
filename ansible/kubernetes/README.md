# Kubernetes



### Initialisation

From the root directory:
```zsh
task ansible:run namespace=kubernetes playbook=initialise
```

## Testing your cluster

### Ping the server
Copy your kube config locally:
```zsh
ping 192.168.x.x # specified in the config.yml
ssh node@192.168.238.139
```

### Kubectl
Copy your kube config locally:
```zsh
scp node@192.168.238.139:~/.kube/config ~/.kube/config
kubectl get nodes
```

### Kube-vip (load balancer)

Fetch the logs of the first kube-vip pod found in the kube-system namespace (extracting the pod name with grep and awk).

```zsh
kubectl logs $(kubectl get pods -n kube-system | grep kube-vip | awk '{print $1}') -n kube-system
```
