# WordPress on Kubernetes using ArgoCD (GitOps)

## Namespace
test-aman

## Features
- GitOps deployment using ArgoCD
- WordPress + MySQL
- ConfigMap & Secret usage
- Rolling updates
- Readiness & Liveness probes

## Workflow
1. ArgoCD watches this Git repo
2. Any Git change is automatically applied
3. Image version changes trigger rolling updates

## Demo
Change WordPress image tag in GitHub and watch ArgoCD sync automatically.
