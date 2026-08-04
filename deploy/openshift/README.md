# Deploying RVTMA on OpenShift

## Quick Deploy

```bash
oc apply -f deploy/openshift/rvtma.yaml
oc get route rvtma -n rvtma -o jsonpath='{.spec.host}'
```

## What's Included (rvtma.yaml)

Single manifest with all resources:
- Namespace `rvtma`
- Deployment with 1 replica, resource limits, health probes
- ClusterIP Service on port 8080
- Route with TLS edge termination

## OpenShift Compatibility

The container image runs as non-root (OpenShift `restricted` SCC):
- Nginx listens on port **8080**
- PID and temp files use `/tmp/`
- No ConfigMap override needed

## PDF Generation

PDF generation works via the `/api/generate-pdf` endpoint.
The deployment limits memory at 2Gi. For large PDFs (100+ pages), consider increasing the limit.

## Cleanup

```bash
oc delete namespace rvtma
```
