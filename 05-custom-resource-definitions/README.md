# Custom Resource Definitions

This lab demonstrates how to create and use a **Custom Resource Definition (CRD)** in Kubernetes.

The lab defines a custom Kubernetes resource called `DevOpsLab` and then creates an instance of that resource.

## Files

- `devops-lab-crd.yml` — Defines the `DevOpsLab` Custom Resource Definition.
- `devops-lab.yml` — Creates a `DevOpsLab` custom resource.

## Custom Resource Definition

The CRD uses:

- **Group:** `learning.nitish.dev`
- **Version:** `v1`
- **Kind:** `DevOpsLab`
- **Scope:** Namespaced
- **Plural:** `devopslabs`
- **Short names:** `lab`, `labs`

The custom resource contains:

- `topic`
- `duration`
- `mode`
- `platform`

## Apply the CRD

```bash
kubectl apply -f devops-lab-crd.yml
```

Verify the CRD:

```bash
kubectl get crd devopslabs.learning.nitish.dev
```

Check that Kubernetes recognizes the new resource:

```bash
kubectl api-resources | grep -i devopslab
```

## Create the Custom Resource

```bash
kubectl apply -f devops-lab.yml
```

Verify the created resource:

```bash
kubectl get devopslabs
```

Example:

```text
NAME
devops-learning-lab
```

View the complete resource:

```bash
kubectl get devopslab devops-learning-lab -o yaml
```

## Resources Created

This lab creates the following Kubernetes resources:

| Resource | Name | Purpose |
|---|---|---|
| CustomResourceDefinition | `devopslabs.learning.nitish.dev` | Defines the `DevOpsLab` resource type |
| Custom Resource | `devops-learning-lab` | Creates an instance of the `DevOpsLab` resource |

## Pods

This lab does **not** create any Pods.

A CRD extends the Kubernetes API with a new resource type. It does not automatically create workloads such as Pods, Deployments, or Services.

## Learning Outcome

This lab demonstrates:

- Creating a Custom Resource Definition
- Defining a custom Kubernetes API resource
- Creating a custom resource from the CRD
- Using `kubectl` to inspect custom resources
- Understanding the difference between a CRD and a custom resource

## Resource Flow

```text
devops-lab-crd.yml
        │
        ▼
CustomResourceDefinition
devopslabs.learning.nitish.dev
        │
        │ defines
        ▼
DevOpsLab resource type
        ▲
        │ creates
        │
devops-lab.yml
        │
        ▼
DevOpsLab
devops-learning-lab
```
