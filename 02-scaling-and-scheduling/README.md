# Scaling and Scheduling

Hands-on Kubernetes practice covering autoscaling, scheduling, resource management, node affinity, taints and tolerations, and container health probes.

## Topics Covered

- Horizontal Pod Autoscaler (HPA)
- Vertical Pod Autoscaler (VPA)
- Liveness probes
- Readiness probes
- Startup probes
- Node affinity
- Taints and tolerations
- Resource requests and limits
- ResourceQuota

## Directory Structure

```text
02-scaling-and-scheduling/
├── HPA/
├── probes/
├── node-affinity.yml
├── taints-and-tolerations.yml
├── resource_quotas_and_limits.yml
├── vpa-deployment.yml
├── vpa.yml
└── README.md
```

## 1. Horizontal Pod Autoscaler (HPA)

HPA automatically adjusts the number of pod replicas based on resource utilization such as CPU.

The HPA examples require the Kubernetes Metrics Server to be available in the cluster.

Check HPA:

```bash
kubectl get hpa
kubectl describe hpa <hpa-name>
```

## 2. Vertical Pod Autoscaler (VPA)

This example demonstrates VPA resource recommendations for a Kubernetes Deployment.

Files:

- `vpa-deployment.yml` - creates the Deployment targeted by VPA
- `vpa.yml` - configures the VPA

### Apply

First create the Deployment:

```bash
kubectl apply -f vpa-deployment.yml
```

Then create the VPA:

```bash
kubectl apply -f vpa.yml
```

### Check

```bash
kubectl get deployment vpa-demo
kubectl get pods -l app=vpa-demo
kubectl get vpa
kubectl describe vpa vpa-demo
```

The VPA example uses:

```yaml
updateMode: "Off"
```

This means VPA provides resource recommendations without automatically changing the pod resources.

The VPA components and CRDs must be installed in the Kubernetes cluster before applying `vpa.yml`.

## 3. Probes

Kubernetes health probes are used to determine whether a container is healthy and ready to receive traffic.

This lab includes examples of:

- Liveness probes
- Readiness probes
- Startup probes
- HTTP probes
- TCP probes
- Exec probes
- Probe configuration settings

Examples are available in the `probes/` directory.

## 4. Node Affinity

Node affinity allows pods to be scheduled onto nodes that match specific labels.

File:

```text
node-affinity.yml
```

Useful command:

```bash
kubectl get nodes --show-labels
```

## 5. Taints and Tolerations

Taints prevent pods from being scheduled onto specific nodes unless the pod has a matching toleration.

File:

```text
taints-and-tolerations.yml
```

Useful commands:

```bash
kubectl describe nodes
kubectl get pods -o wide
```

## 6. Resource Requests, Limits and ResourceQuota

This example demonstrates both container-level resource management and namespace-level resource quotas.

File:

```text
resource_quotas_and_limits.yml
```

The manifest creates:

- A dedicated namespace
- A ResourceQuota for CPU, memory and pod count
- An NGINX Deployment with resource requests and limits

### Apply

```bash
kubectl apply -f resource_quotas_and_limits.yml
```

### Check the ResourceQuota

```bash
kubectl get resourcequota -n resource-demo
kubectl describe resourcequota resource-quota -n resource-demo
```

### Check the Deployment and Pods

```bash
kubectl get deployment -n resource-demo
kubectl get pods -n resource-demo
```

Resource requests and limits control resource allocation for individual containers, while ResourceQuota limits total resource consumption within a namespace.

## Prerequisites

These labs may require:

- Kubernetes cluster
- `kubectl`
- Metrics Server for HPA examples
- Vertical Pod Autoscaler for VPA examples
- Basic Linux command-line knowledge

The labs were practiced on a Kubernetes cluster running on AWS EC2 using kubeadm and containerd.

## Validation

Before committing the manifests to GitHub, validate YAML syntax and Kubernetes resources:

```bash
kubectl apply --dry-run=client -f vpa-deployment.yml
kubectl apply --dry-run=client -f vpa.yml
kubectl apply --dry-run=client -f resource_quotas_and_limits.yml
```

For other manifests:

```bash
kubectl apply --dry-run=client -f <file>.yml
```

## Cleanup

For the VPA example:

```bash
kubectl delete -f vpa.yml
kubectl delete -f vpa-deployment.yml
```

For the ResourceQuota and resource limits example:

```bash
kubectl delete -f resource_quotas_and_limits.yml
```

## Learning Goal

The goal of this lab is to build practical understanding of Kubernetes scaling, scheduling, resource management, and container health checks through hands-on manifests and commands.
