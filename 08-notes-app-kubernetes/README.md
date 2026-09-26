# Notes App on Kubernetes

A hands-on Kubernetes deployment of a Django Notes application with MySQL.

This lab focuses on deploying a containerized application, connecting it to a database through Kubernetes networking, managing configuration with a Secret, exposing the application with a NodePort Service, and verifying the application end-to-end.

> **Learning project:** The original Notes application is based on the public `django-notes-app` project by [LondheShubham153](https://github.com/LondheShubham153/django-notes-app). The Kubernetes deployment and configuration in this directory are part of my hands-on practice.

## Architecture

```text
                     Kubernetes Cluster
                            |
                     +------+------+
                     |             |
              notes-ns namespace   |
                     |             |
          +----------+----------+  |
          |                     |  |
   NodePort Service       MySQL Service
   30001 -> 8000          3306 -> 3306
          |                     |
          v                     v
    Django Notes App          MySQL
      Deployment             Deployment
          |                     |
          +----------+----------+
                     |
              Kubernetes Secret
              database settings
```

## Files

| File | Purpose |
|---|---|
| `namespace.yml` | Creates the `notes-ns` namespace |
| `deployment.yml` | Deploys the Django Notes application |
| `service.yml` | Exposes the application through NodePort `30001` |
| `mysql-deployment.yml` | Deploys MySQL |
| `mysql-service.yml` | Provides internal MySQL connectivity |
| `README.md` | Documentation for this lab |

## Kubernetes Resources

This lab uses:

- Namespace
- Deployment
- MySQL Deployment
- Kubernetes Secret
- ClusterIP Service
- NodePort Service
- Node selector
- Tolerations
- Readiness probes
- Resource requests and limits

## Database Configuration

The Django application reads its database configuration from environment variables:

```text
DB_NAME
DB_USER
DB_PASSWORD
DB_HOST
DB_PORT
```

The Kubernetes Secret provides the database configuration used by the application.

The application connects to MySQL through:

```text
mysql-service:3306
```

Do not commit real database credentials to GitHub.

## Deploy the Application

### 1. Create the Namespace

```bash
kubectl apply -f namespace.yml
```

### 2. Create the Database Secret

Create the Secret separately instead of storing the real password in a YAML file:

```bash
kubectl create secret generic notes-db-secret   -n notes-ns   --from-literal=DB_NAME=test_db   --from-literal=DB_USER=root   --from-literal=DB_PASSWORD=<your-password>   --from-literal=DB_PORT=3306
```

> Replace `<your-password>` with your local learning password. Do not commit the command containing your real password.

### 3. Deploy MySQL

```bash
kubectl apply -f mysql-deployment.yml
kubectl apply -f mysql-service.yml
```

Check MySQL:

```bash
kubectl get deployment,pods,svc -n notes-ns -o wide
```

Wait until the MySQL pod is:

```text
1/1 Running
```

### 4. Deploy the Django Application

```bash
kubectl apply -f deployment.yml
```

### 5. Create the NodePort Service

```bash
kubectl apply -f service.yml
```

Verify:

```bash
kubectl get pods,svc -n notes-ns -o wide
```

The application Service should show:

```text
80:30001/TCP
```

## Database Migration

Run Django migrations inside the application pod:

```bash
kubectl exec -it deployment/notes-dep -n notes-ns -- python manage.py migrate
```

Verify migration status:

```bash
kubectl exec -it deployment/notes-dep -n notes-ns -- python manage.py showmigrations
```

## Verify the Application

Check the application logs:

```bash
kubectl logs deployment/notes-dep -n notes-ns
```

Test the API from the Kubernetes node:

```bash
curl http://localhost:30001/api/notes/
```

Test the application:

```bash
curl http://localhost:30001
```

If the node is reachable from your local machine, open:

```text
http://<NODE-PUBLIC-IP>:30001
```

## Example API Response

After adding notes, the API can return data similar to:

```json
[
  {
    "id": 1,
    "body": "Learn Kubernetes Services",
    "updated": "2026-09-26T10:00:00Z",
    "created": "2026-09-26T10:00:00Z"
  }
]
```

The exact values depend on the notes created during the learning session.

## Useful Verification Commands

```bash
kubectl get namespace notes-ns

kubectl get deployment -n notes-ns

kubectl get pods -n notes-ns -o wide

kubectl get svc -n notes-ns

kubectl get secret notes-db-secret -n notes-ns

kubectl logs deployment/notes-dep -n notes-ns

kubectl logs deployment/mysql-dep -n notes-ns
```

Check the application Service endpoints:

```bash
kubectl get endpoints notes-app-service -n notes-ns
```

## Troubleshooting

### Pod is not starting

```bash
kubectl get pods -n notes-ns
kubectl describe pod <pod-name> -n notes-ns
```

### Check Django logs

```bash
kubectl logs deployment/notes-dep -n notes-ns
```

### Check MySQL logs

```bash
kubectl logs deployment/mysql-dep -n notes-ns
```

### Check the database connection

```bash
kubectl get svc mysql-service -n notes-ns
```

The application should use:

```text
DB_HOST=mysql-service
DB_PORT=3306
```

### If migrations have not been applied

```bash
kubectl exec -it deployment/notes-dep -n notes-ns -- python manage.py migrate
```

## Cleanup

Delete the application resources:

```bash
kubectl delete -f service.yml
kubectl delete -f deployment.yml
kubectl delete -f mysql-service.yml
kubectl delete -f mysql-deployment.yml
```

Delete the Secret:

```bash
kubectl delete secret notes-db-secret -n notes-ns
```

Delete the namespace:

```bash
kubectl delete -f namespace.yml
```

## Important Notes

- This is a Kubernetes learning project, not a production deployment.
- MySQL uses ephemeral container storage in this lab.
- Persistent storage can be added later using a PersistentVolume and PersistentVolumeClaim.
- The NodePort is intended for simple learning and testing.
- The database Secret is created separately so that real credentials do not need to be stored in Git.
- The original Notes application is credited to its public source repository.

## Learning Goal

The goal of this lab is to understand how a multi-container application can be deployed on Kubernetes and connected using Services, Secrets, Deployments, and Kubernetes networking.

