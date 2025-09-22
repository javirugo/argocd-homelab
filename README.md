# Homelab ArgoCD

I use this simple repository to populate my Homelab kubernetes cluster with all tools I usually require.

It deploys:

- Base k8s objects (such as namespaces, an ingress for argocd, some secrets, a cert-manager clusterissuer, etc.)
- Gitea
- Hasicorp's Vault (single instance, no dev-mode, persistant file storage...)
- MySQL Operator and an InnoDB cluster for the Homelab tools (for gitea for example).

## Requirements

- Working Kubernetes cluster.
- ArgoCD installed in the "argocd" directory.
- Nginx Ingress Controller installed in the cluter.
- Cert-Manager installed in the cluster (a clusterissuer will be managed by argocd).
  - cert-manager will use a cloudflare dns01 solver using letsencrypt. I have a free cloudflare account for managing my galluman.com zone, you can change that to whatever you want to use.

Once everything starts to deploy, some steps are required:

1. Init and unseal Vault (and setup authentication, roles, etc as you prefer).
2. Create some Vault secrets (see all externalsecrets in this repo).
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

See the Requirements section...

## Notes

I use microk8s in my homelab server that makes it so easy to have a basic k8s cluster with "addons" for argocd, cert-manager and nginx-ingress-controller. If you are using a different setup (kubedm created cluster, cloud managed or whatever) you will need to take care of the Requisites described above and probably change a few things in this repository.

All ingresses are currently using my domain: galluman.com. Please change them to use your own domain.
