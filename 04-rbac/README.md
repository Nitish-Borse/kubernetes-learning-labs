# Kubernetes RBAC

Demonstrates namespace-scoped authorization using:

- Namespace
- ServiceAccount
- Role
- RoleBinding

The Role grants selected access to Pods, Services and Deployments in
`apache-ns`.

## Apply the RBAC resources

```bash
kubectl apply -f namespace.yml
kubectl apply -f service-account.yml
kubectl apply -f roles.yml
kubectl apply -f role-binding.yml
```

## Verify RBAC resources

```bash
kubectl get serviceaccount -n apache-ns
kubectl get role -n apache-ns
kubectl get rolebinding -n apache-ns
```

## Test permissions

Check an allowed permission:

```bash
kubectl auth can-i get pods \
  --as=system:serviceaccount:apache-ns:apache-user \
  -n apache-ns
```

Check another allowed permission:

```bash
kubectl auth can-i delete pods \
  --as=system:serviceaccount:apache-ns:apache-user \
  -n apache-ns
```

Check a permission that is not granted by the Role:

```bash
kubectl auth can-i get secrets \
  --as=system:serviceaccount:apache-ns:apache-user \
  -n apache-ns
```

Expected results:

```text
get pods       → yes
delete pods    → yes
get secrets    → no
```

## Important Note

Authentication and authorization are separate concerns.

The ServiceAccount identifies the workload identity, while the Role
defines the permissions and the RoleBinding connects the identity to
those permissions.

