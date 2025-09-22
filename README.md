# Homelab ArgoCD

I use this simple repository to populate my Homelab kubernetes cluster with all tools I usually require.

It deploys:

- Base k8s objects (such as namespaces, an ingress for argocd, some secrets, a cert-manager clusterissuer, etc.)
- Gitea
- Hasicorp's Vault (single instance, no dev-mode)
- MySQL Operator and a InnoDB cluster

## Requirements

- Working Kubernetes cluster.
- ArgoCD installed in the "argocd" directory.
- Nginx Ingress Controller installed in the cluter.
- Cert-Manager installed in the cluster (a clusterissuer will be managed by argocd).
  - cert-manager will use a cloudflare dns01 solver using letsencrypt. I have a free cloudflare account for managing my galluman.com zone, you can change that to whatever you want to use for DNS.

Once everything starts to deploy, it will be required to follow some steps:

1. Init and unseal Vault (and setup authentication, roles, etc.).
2. Create all secrets Vault (see all externalsecrets in this repo).
3. Create a secret in the build, data and cert-manager namespaces for the vault token:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: vault-token
  namespace: data
type: Opaque
stringData:
  token: VAULT_TOKEN_HERE
```

## Instructions

Just apply the main apps.yaml in argocd:

```bash
kubectl apply -f https://raw.githubusercontent.com/javirugo/argocd-homelab/refs/heads/main/apps.yaml
```
