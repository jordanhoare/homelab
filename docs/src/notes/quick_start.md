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

### Create ArgoCD...
```zsh
kubectl apply -k ./kubernetes/argocd
kubectl get pods -n argocd

```

### Delete ArgoCD
```zsh
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.7.2/manifests/install.yaml

kubectl delete all,secret,configmap,role,rolebinding,clusterrole,clusterrolebinding,serviceaccount -n argocd --selector app.kubernetes.io/part-of=argocd

kubectl delete all --all -n argocd
kubectl delete secret --all -n argocd
kubectl delete configmap --all -n argocd
kubectl delete role --all -n argocd
kubectl delete rolebinding --all -n argocd
kubectl delete clusterrole,clusterrolebinding -n argocd --selector=app.kubernetes.io/part-of=argocd
kubectl delete serviceaccount --all -n argocd

kubectl delete ns argocd
```