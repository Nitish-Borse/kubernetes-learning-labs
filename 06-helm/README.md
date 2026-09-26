# Helm

Apache HTTPD Helm chart used for Kubernetes Helm practice.

## Chart features

- Configurable replica count
- Configurable container image
- ServiceAccount
- Service settings
- Optional Ingress
- Optional HTTPRoute
- Liveness/readiness probes
- Optional HPA
- Node selector, affinity and tolerations
- Helm test
- Helper templates

## Commands

```bash
helm lint apache-helm
helm template apache-dev apache-helm
helm install apache-dev apache-helm
helm list
helm test apache-dev
helm uninstall apache-dev
```

The packaged `.tgz` and third-party Helm installation script from the original lab are intentionally excluded; the chart source is the useful portfolio artifact.
