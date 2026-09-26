# Kubernetes Learning Labs

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#kubernetes-learning-labs)

Hands-on Kubernetes practice covering workloads, scheduling, storage, RBAC, CRDs, Helm, probes, autoscaling, and application deployment.

This repository contains Kubernetes manifests and notes from my hands-on learning and practice.

## What I Practiced

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#what-i-practiced)

- Pods and workload controllers
- Deployments, ReplicaSets, DaemonSets, StatefulSets, Jobs and CronJobs
- HPA and VPA
- Node affinity
- Taints and tolerations
- Resource requests, limits and quotas
- Liveness, readiness and startup probes
- PersistentVolumes and PersistentVolumeClaims
- ConfigMaps
- Stateful workloads
- RBAC with Roles, RoleBindings and ServiceAccounts
- Custom Resource Definitions (CRDs)
- Helm charts and Helm templating
- Kubernetes Dashboard access
- Application deployment using Kubernetes Services

## Repository Structure

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#repository-structure)

| **DirectoryTopics**               |                                                                      |
| --------------------------------- | -------------------------------------------------------------------- |
| `01-pod-types/`                   | Deployment, ReplicaSet, DaemonSet, StatefulSet, Job, CronJob         |
| `02-scaling-and-scheduling/`      | HPA, VPA, probes, node affinity, taints/tolerations, resources       |
| `03-storage/`                     | PV, PVC, ConfigMap, StatefulSet, MySQL storage examples              |
| `04-rbac/`                        | ServiceAccount, Role, RoleBinding and RBAC authorization             |
| `05-custom-resource-definitions/` | CustomResourceDefinition and Custom Resource                         |
| `06-helm/`                        | Apache Helm chart, templates, Service, Ingress, HPA and Helm test    |
| `07-kubernetes-dashboard/`        | Dashboard access and RBAC example                                    |
| `08-notes-app-kubernetes/`        | Django Notes App, MySQL, Secrets, Services and Kubernetes deployment |

## Helm

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#helm)

The Apache chart demonstrates:

- `Chart.yaml`
- `values.yaml`
- Deployment template
- Service template
- ServiceAccount
- Ingress
- HTTPRoute
- HPA
- Helm test
- Reusable helper templates

Validate the chart with:

```
helm lint 06-helm/apache-helm
helm template demo 06-helm/apache-helm
```

**svg**

## Notes Application

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#notes-application)

The `08-notes-app-kubernetes/` lab demonstrates deploying a Django Notes application with MySQL on Kubernetes using:

- Namespace
- Django Deployment
- MySQL Deployment
- Kubernetes Secret
- ClusterIP Service
- NodePort Service
- Node selector and tolerations
- Readiness probes
- Resource requests and limits
- Automatic Django database migrations

The Django application listens on port `8000`, while the Kubernetes Service exposes it through NodePort `30001`.

The Django application connects to MySQL through the Kubernetes Service:

```
mysql-service:3306

```

**svg**

The application used in this lab is based on the public [django-notes-app](https://github.com/LondheShubham153/django-notes-app) project by LondheShubham153. The focus of this lab is containerization and Kubernetes deployment rather than original application development.

> Note: MySQL uses ephemeral container storage in this lab. Persistent storage can be added later using a PersistentVolume and PersistentVolumeClaim.

## Screenshots

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#screenshots)

### Kubernetes Cluster

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#kubernetes-cluster)

[Kubernetes Cluster](https://github.com/Nitish-Borse/kubernetes-learning-labs/blob/main/screenshots/cluster-nodes.png) ([image](https://github.com/Nitish-Borse/kubernetes-learning-labs/raw/main/screenshots/cluster-nodes.png))

### Kubernetes Workloads

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#kubernetes-workloads)

[Kubernetes Workloads](https://github.com/Nitish-Borse/kubernetes-learning-labs/blob/main/screenshots/workloads.png) ([image](https://github.com/Nitish-Borse/kubernetes-learning-labs/raw/main/screenshots/workloads.png))

### HPA

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#hpa)

[Horizontal Pod Autoscaler](https://github.com/Nitish-Borse/kubernetes-learning-labs/blob/main/screenshots/hpa.png) ([image](https://github.com/Nitish-Borse/kubernetes-learning-labs/raw/main/screenshots/hpa.png))

### Helm

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#helm-1)

[Helm](https://github.com/Nitish-Borse/kubernetes-learning-labs/blob/main/screenshots/helm.png) ([image](https://github.com/Nitish-Borse/kubernetes-learning-labs/raw/main/screenshots/helm.png))

### Notes Application

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#notes-application-1)

[Notes Application](https://github.com/Nitish-Borse/kubernetes-learning-labs/blob/main/screenshots/notes-app.png) ([image](https://github.com/Nitish-Borse/kubernetes-learning-labs/raw/main/screenshots/notes-app.png))

## Useful Commands

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#useful-commands)

```
kubectl get nodes
kubectl get pods -A
kubectl get deployments -A
kubectl get services -A

kubectl apply -f <manifest>.yml
kubectl get -f <manifest>.yml
kubectl delete -f <manifest>.yml

kubectl describe pod <pod-name>
kubectl logs <pod-name>

helm lint 06-helm/apache-helm
helm template demo 06-helm/apache-helm
helm install demo 06-helm/apache-helm
helm test demo
helm uninstall demo
```

**svg**

## Validation

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#validation)

For regular Kubernetes manifests:

```
kubectl apply --dry-run=client -f <manifest>.yml
```

**svg**

For Helm:

```
helm lint 06-helm/apache-helm
helm template demo 06-helm/apache-helm
```

**svg**

Helm files under `06-helm/apache-helm/templates/` are templates and should be validated through Helm rather than as standalone YAML.

## Security

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#security)

Never commit:

- AWS credentials
- kubeconfig files
- private keys
- `.pem` files
- `.env` files
- real passwords or tokens

The MySQL Secret in `03-storage/mysql-secret-example.yml` contains only the dummy value `change-me` for learning purposes. It is not a real credential.

Never replace the example value with a real password before committing the repository.

## Learning Goal

[svg](https://github.com/Nitish-Borse/kubernetes-learning-labs#learning-goal)

The goal of this repository is to demonstrate practical Kubernetes learning through focused labs, manifests, and a small application deployment rather than a collection of copied notes.

