# Kubernetes Dashboard

Hands-on practice with Kubernetes Dashboard authentication using a ServiceAccount and ClusterRoleBinding.

> ⚠️ **Learning example only:** This configuration grants the `dashboard-admin` ServiceAccount the built-in `cluster-admin` role. It is intended for Kubernetes learning and practice, not as a least-privilege production authentication configuration.

## What I Practiced

- Kubernetes Dashboard installation
- ServiceAccounts
- ClusterRoleBinding
- Dashboard authentication using a ServiceAccount token
- Accessing the Dashboard through Kubernetes port forwarding
- Basic RBAC concepts

## Files

| File | Description |
|---|---|
| `dashboard-admin-user.yml` | Creates the `dashboard-admin` ServiceAccount and binds it to the `cluster-admin` ClusterRole |

## Dashboard Installation

### Current Installation Method

Current Kubernetes Dashboard releases use **Helm-based installation**. Manifest-based installation is no longer supported from Dashboard v7.0.0 onward.

Add the Kubernetes Dashboard Helm repository:

```bash
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
```

Install the Dashboard:

```bash
helm upgrade --install kubernetes-dashboard \
  kubernetes-dashboard/kubernetes-dashboard \
  --create-namespace \
  --namespace kubernetes-dashboard
```

Check the Dashboard resources:

```bash
kubectl get pods -n kubernetes-dashboard
kubectl get svc -n kubernetes-dashboard
```

### Installation Method Used in This Lab

This lab was originally practiced using **Kubernetes Dashboard v2.7.0** with the manifest-based installation method:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

This command is kept here as a **historical learning reference** for the method used during my practice. It should not be treated as the current installation method.

After installation, verify the Dashboard pods:

```bash
kubectl get pods -n kubernetes-dashboard
```

## Create the Dashboard Admin ServiceAccount

Apply the RBAC configuration:

```bash
kubectl apply -f dashboard-admin-user.yml
```

Verify the ServiceAccount:

```bash
kubectl get serviceaccount -n kubernetes-dashboard
```

Verify the ClusterRoleBinding:

```bash
kubectl get clusterrolebinding dashboard-admin
```

## Generate a Login Token

Generate a token for the learning session:

```bash
kubectl -n kubernetes-dashboard create token dashboard-admin
```

Use the generated token only for the local learning session.

> ⚠️ **Security:** Never commit Kubernetes tokens or screenshots containing tokens to GitHub.

## Access the Dashboard

For the current Helm-based Dashboard installation, port-forward the Dashboard's Kong proxy service:

```bash
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
```

Then open:

```text
https://localhost:8443
```

Use the generated ServiceAccount token when prompted for authentication.

### Accessing a Dashboard on a Remote EC2 Cluster

If the Kubernetes cluster is running on a remote EC2 instance, create an SSH tunnel from your local machine:

```bash
ssh -i "your-key.pem" -L 8443:localhost:8443 ubuntu@<MASTER-PUBLIC-IP>
```

Then open:

```text
https://localhost:8443
```

The SSH tunnel keeps the Dashboard port accessible through the local machine without exposing the Dashboard directly to the public internet.

## Verify Resources

Useful commands for checking the Dashboard installation:

```bash
kubectl get pods -n kubernetes-dashboard
kubectl get services -n kubernetes-dashboard
kubectl get serviceaccount -n kubernetes-dashboard
kubectl get clusterrolebinding dashboard-admin
```

## Security Note

This lab uses the `cluster-admin` ClusterRole to simplify Dashboard authentication during practice.

For real environments:

- Follow least-privilege RBAC.
- Do not use this ServiceAccount configuration as a production authentication design.
- Never expose the Kubernetes Dashboard directly to the public internet without appropriate security controls.
- Never commit Kubernetes tokens, AWS credentials, private keys, kubeconfig files, or real secrets to GitHub.

## Learning Goal

The goal of this lab is to understand how Kubernetes ServiceAccounts and RBAC can be used for authentication and authorization when accessing the Kubernetes Dashboard.

This lab also helped me understand the difference between an older manifest-based Dashboard installation and the current Helm-based installation approach.

