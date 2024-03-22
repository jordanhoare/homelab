# ArgoCD

**The decision between setting up GitOps immediately or installing my ingress controllers first** was a hurdle. Given my aim is to automate as much as possible from a clean state of my cluster,  I decided to configure GitOps early in the process. 

Configuring GitOps with ArgoCD right after my cluster is bootstrapped makes a lot of sense. I can manage almost every aspect of my cluster through GitOps, including the deployment of my ingress controllers. This approach aligns with the GitOps philosophy of having a single source of truth (my Git repository) that dictates the desired state of my cluster, including networking components like ingress controllers.

## Bootstrapping ArgoCD with Kustomize

When bootstrapping ArgoCD using kubectl and Kustomize:

Kustomize: Define my ArgoCD setup in Kustomize. This includes customizing ArgoCD components, such as the server, repo server, and application controller, according to my requirements.

Apply configuration: Use kubectl apply -k to apply my Kustomize configuration to my cluster. This step will create or update the ArgoCD resources in my cluster based on the definitions in my Kustomize configuration.

Idempotency: If you re-run kubectl apply -k with the same Kustomize configuration, Kubernetes will compare the desired state in my Kustomize configuration with the current state of the resources in the cluster. If there are no changes, Kubernetes won't perform any updates, maintaining idempotency.