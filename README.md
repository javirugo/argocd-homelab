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

### Vault External Secrets

This requires first to create a Secret in the "data" namespace with a valid Vault token:

```
apiVersion: v1
kind: Secret
metadata:
  name: vault-token
  namespace: data
type: Opaque
stringData:
  token: VAULT_TOKEN_HERE
```
