# Useful commands

## Load balancers

### Kube-vip

Fetch the logs of the first kube-vip pod found in the kube-system namespace (extracting the pod name with grep and awk).

```zsh
kubectl logs $(kubectl get pods -n kube-system | grep kube-vip | awk '{print $1}') -n kube-system
```

## ArgoCD commands

### Login remotely to argocd (without a load balancer)
```zsh
kubectl port-forward svc/argocd-server -n argocd 8080:443

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && argocd login localhost:8080 --port-forward-namespace argocd --username admin --password $(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
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