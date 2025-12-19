Perfect 👍
Below is a **clean, structured, copy-paste–friendly LIST OF ALL COMMANDS** that were used (or are relevant) to **build, access, operate, troubleshoot, and demonstrate** your **ArgoCD + Kubernetes + WordPress GitOps project**.

You can **directly paste this section into your README** under something like
👉 **“Complete Command Reference”**.

---

# 📌 Complete Command Reference

**(ArgoCD + Kubernetes + WordPress GitOps Project)**

---

## 🔹 1. Kubernetes Cluster & Context Verification

```bash
kubectl version --client
kubectl cluster-info
kubectl get nodes -o wide
kubectl config current-context
kubectl config view --minify
```

---

## 🔹 2. Namespace Commands

```bash
kubectl get ns
kubectl create ns test-aman
kubectl get ns test-aman
```

---

## 🔹 3. ArgoCD Installation & Verification

### Install ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Verify ArgoCD Pods

```bash
kubectl get pods -n argocd
kubectl get svc -n argocd
kubectl get deployments -n argocd
```

---

## 🔹 4. Get ArgoCD Admin Password

```bash
kubectl get secret argocd-initial-admin-secret -n argocd \
  -o jsonpath="{.data.password}" | base64 -d
```

Username:

```
admin
```

---

## 🔹 5. Access ArgoCD UI

### Port-forward (recommended for private cluster)

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:80
```

Open in browser:

```
http://localhost:8080
```

---

## 🔹 6. ArgoCD Application Verification

```bash
kubectl get applications -n argocd
kubectl describe application wordpress-test-aman -n argocd
```

---

## 🔹 7. Apply Kubernetes Manifests (Initial / Non-GitOps)

> ⚠️ After ArgoCD is configured, **do NOT use kubectl apply** for app resources.

```bash
kubectl apply -f app/00-namespace.yaml
kubectl apply -f app/01-secret.yaml
kubectl apply -f app/02-configmap.yaml
kubectl apply -f app/03-mysql.yaml
kubectl apply -f app/04-wordpress.yaml
```

---

## 🔹 8. Check Deployments, Pods, Services

```bash
kubectl get all -n test-aman
kubectl get pods -n test-aman
kubectl get svc -n test-aman
kubectl get deployments -n test-aman
```

---

## 🔹 9. WordPress Access Commands

### Port-forward WordPress

```bash
kubectl port-forward svc/wordpress -n test-aman 8081:80
```

Open:

```
http://localhost:8081
```

---

## 🔹 10. MySQL Verification Commands

### Check MySQL Pod

```bash
kubectl get pods -n test-aman
kubectl logs deployment/mysql -n test-aman
```

### Connect to MySQL

```bash
kubectl exec -it deployment/mysql -n test-aman -- mysql -u root -p
```

Password:

```
root123
```

### Verify WordPress Data in DB

```sql
USE wordpress;

SHOW TABLES;

SELECT ID, post_title, post_type, post_status
FROM wp_posts
ORDER BY post_date DESC
LIMIT 10;
```

---

## 🔹 11. Resource & Health Probe Checks

```bash
kubectl describe pod <wordpress-pod-name> -n test-aman
kubectl describe deployment wordpress -n test-aman
kubectl top pods -n test-aman
kubectl top nodes
```

---

## 🔹 12. Rolling Update Commands (GitOps)

### Change image in Git (example)

```yaml
image: wordpress:6.5.3-php8.2-apache
```

### Commit & Push

```bash
git status
git add .
git commit -m "Update WordPress image version"
git push
```

### Observe rollout

```bash
kubectl rollout status deployment/wordpress -n test-aman
kubectl get pods -n test-aman -w
```

---

## 🔹 13. ReplicaSet & Rollout History

```bash
kubectl get replicaset -n test-aman
kubectl rollout history deployment/wordpress -n test-aman
```

### Rollback Deployment

```bash
kubectl rollout undo deployment/wordpress -n test-aman
```

---

## 🔹 14. Scaling Commands

```bash
kubectl scale deployment wordpress -n test-aman --replicas=2
kubectl scale deployment wordpress -n test-aman --replicas=3
kubectl get pods -n test-aman
```

---

## 🔹 15. Redeployment Commands (GitOps Safe)

### Delete Deployment (ArgoCD recreates it)

```bash
kubectl delete deployment wordpress -n test-aman
```

### Restart Deployment

```bash
kubectl rollout restart deployment wordpress -n test-aman
```

---

## 🔹 16. Troubleshooting Commands

```bash
kubectl describe pod <pod-name> -n test-aman
kubectl logs <pod-name> -n test-aman
kubectl get events -n test-aman
kubectl get endpoints -n test-aman
kubectl exec -it <pod-name> -n test-aman -- bash
```

---

## 🔹 17. GitOps Validation Commands

```bash
kubectl diff -f app/04-wordpress.yaml
kubectl get applications -n argocd
```

---

## 🔹 18. Cleanup (Optional)

```bash
kubectl delete deployment wordpress -n test-aman
kubectl delete deployment mysql -n test-aman
kubectl delete ns test-aman
```

---

## ✅ Final Notes (Important)

* `kubectl edit` is **temporary** in GitOps
* GitHub is the **source of truth**
* ArgoCD **reconciles automatically**
* ReplicaSets are created for **every update**
* Only ReplicaSet with `DESIRED > 0` is active

---

## 🎯 One-Line Manager Summary

> “This project uses ArgoCD to manage Kubernetes deployments via GitOps, including secure access, rolling updates, health checks, scaling, and database persistence.”

---

If you want next, I can:

* Convert this into a **cheat-sheet PDF**
* Add **ArgoCD CLI (`argocd`) commands**
* Create **resume bullets**
* Add **HPA commands**

Just tell me 👊
