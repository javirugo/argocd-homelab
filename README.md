# Homelab ArgoCD

I use this simple repository to populate my Homelab kubernetes cluster with all tools I usually require.

## Requirements

- Working Kubernetes cluster.
- ArgoCD installed in the "argocd" directory.
- Nginx Ingress Controller installed in the cluter.

## Instructions

Just apply the main apps.yaml in argocd:

```
kubectl apply -f https://raw.githubusercontent.com/javirugo/argocd-homelab/refs/heads/main/apps.yaml
```