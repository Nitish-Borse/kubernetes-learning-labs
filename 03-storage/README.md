# Kubernetes Storage

This section contains hands-on Kubernetes practice for working with
configuration and persistent storage.

## Topics Covered

- ConfigMap
- PersistentVolume (PV)
- PersistentVolumeClaim (PVC)
- MySQL Service
- MySQL StatefulSet
- Static storage using `hostPath`
- StatefulSet `volumeClaimTemplates`

## Storage Examples

### 1. Generic Persistent Storage

The following files demonstrate a basic PersistentVolume and
PersistentVolumeClaim:

- `persistent_volume.yml`
- `persistent_volume_claim.yml`

The PV uses the `local-storage` storage class and a local
`hostPath`.

### 2. MySQL Stateful Storage

The MySQL example demonstrates persistent storage with a StatefulSet:

- `mysql_stateful_set.yml`
- `mysql_service.yml`
- `mysql_pv.yml`
- `configmap.yml`
- `mysql-secret-example.yml`

The MySQL StatefulSet uses `volumeClaimTemplates` to create storage
for the MySQL data directory.

## Secret

`mysql-secret-example.yml` contains only a dummy password value for
learning purposes.

Never commit real passwords, API keys, tokens, AWS credentials, or
other sensitive information to GitHub.

For real deployments, create Kubernetes Secrets securely and manage
credentials using an appropriate secret-management solution.

## Important Note

The storage examples in this directory use `hostPath` for learning
purposes. `hostPath` storage is tied to the Kubernetes node where the
data is stored and should not be treated as a production storage
solution.

## Learning Goal

The goal of this section is to practice how Kubernetes applications
can use ConfigMaps, Secrets, PersistentVolumes, PersistentVolumeClaims,
and StatefulSets for stateful workloads.
