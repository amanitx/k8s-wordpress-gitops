Perfect 👍
Below is a **SINGLE, COMPLETE, END-TO-END `README.md`** that documents **EVERY STEP we followed**, **ALL commands**, and **clear explanations**, exactly as you experienced them.

You can **copy-paste this entire file** into your GitHub repo as `README.md`.
No extra files, no missing steps.

---

# 📘 README.md

```markdown
# Kubernetes WordPress Project using ArgoCD (GitOps)

## Author
Aman Kumar

---

## Project Objective

The goal of this project is to demonstrate **real-world Kubernetes skills** using a **GitOps workflow** with **ArgoCD**.

This project proves hands-on experience with:
- Kubernetes core concepts
- GitOps using ArgoCD
- Application deployment and redeployment
- Health probes (readiness & liveness)
- Resource requests and limits
- MySQL database validation
- Troubleshooting real production issues

This setup is designed to be **manager-review ready** and **interview ready**.

---

## High-Level Architecture

```

GitHub Repository
↓
ArgoCD
↓
Kubernetes Cluster (Private)
↓
WordPress + MySQL

```

- Cluster is **private** (internal IPs only)
- Git is the **single source of truth**
- ArgoCD continuously reconciles Git → Cluster

---

## Namespace Used

```

test-aman

````

All resources are deployed inside this namespace.

---

## Repository Structure

```text
k8s-wordpress-gitops/
├── app/
│   ├── 00-namespace.yaml
│   ├── 01-secret.yaml
│   ├── 02-configmap.yaml
│   ├── 03-mysql.yaml
│   ├── 04-wordpress.yaml
└── README.md
````

---

## Prerequisites

* Kubernetes cluster (already installed)
* kubectl configured
* ArgoCD installed in the cluster
* GitHub repository
* Internet access for pulling public images

---

## STEP 1: Create Namespace

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test-aman
```

**Why**

* Keeps resources isolated
* Follows Kubernetes best practices

---

## STEP 2: Create Secrets (MySQL credentials)

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: wp-secrets
  namespace: test-aman
type: Opaque
stringData:
  MYSQL_ROOT_PASSWORD: root123
  MYSQL_USER: wpuser
  MYSQL_PASSWORD: wppass123
```

**Why**

* Sensitive data should not be hardcoded
* Secrets are base64-encoded and managed securely

---

## STEP 3: Create ConfigMap (Application configuration)

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: wp-config
  namespace: test-aman
data:
  MYSQL_DATABASE: wordpress
  WORDPRESS_DB_HOST: mysql.test-aman.svc.cluster.local
  WORDPRESS_DB_NAME: wordpress
  WORDPRESS_DB_USER: wpuser
```

**Why**

* Separates configuration from code
* Makes application portable

---

## STEP 4: Deploy MySQL (Database)

MySQL is deployed as a **single replica** with a **ClusterIP service**.

**Verification Commands**

```bash
kubectl get pods -n test-aman
kubectl logs deployment/mysql -n test-aman
kubectl get svc mysql -n test-aman
```

**Why**

* MySQL stores WordPress data persistently
* Service provides internal DNS resolution

---

## STEP 5: Deploy WordPress (Application)

### Key Features Implemented

* RollingUpdate strategy
* Resource requests & limits
* Readiness probe
* Safe liveness probe
* Environment variables from ConfigMap & Secret

---

## WordPress Resource Management

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

**Why**

* Requests ensure scheduling
* Limits prevent resource starvation
* Demonstrates production readiness

---

## WordPress Health Probes

```yaml
readinessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 60
  periodSeconds: 10
  timeoutSeconds: 5
  failureThreshold: 6

livenessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 180
  periodSeconds: 30
  timeoutSeconds: 5
  failureThreshold: 6
```

**Why**

* Readiness controls traffic
* Liveness ensures recovery
* Timings are tuned for WordPress behavior

---

## IMPORTANT LESSON: GitOps Behavior

* Manual `kubectl edit` changes are temporary
* ArgoCD always enforces Git state
* All fixes must be done in Git

This was validated during probe troubleshooting.

---

## STEP 6: Install and Access ArgoCD

ArgoCD was installed into the cluster and accessed using:

* NodePort
* Port-forwarding / SSH tunneling (due to private cluster)

### Access ArgoCD

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:80
```

Open:

```
http://localhost:8080
```

---

## STEP 7: Create ArgoCD Application (UI)

### Application Configuration

* Name: `wordpress-test-aman`
* Project: `default`
* Repo URL: GitHub repo
* Path: `app`
* Namespace: `test-aman`
* Sync Policy: Automatic (Self Heal + Prune)

**Result**

* ArgoCD deploys all Kubernetes resources automatically
* Git becomes the single source of truth

---

## STEP 8: Access WordPress Application

```bash
kubectl port-forward svc/wordpress -n test-aman 8081:80
```

Open browser:

```
http://localhost:8081
```

---

## STEP 9: Complete WordPress Setup

* Open WordPress UI
* Complete installation
* Create a Page or Post

This step initializes the database schema.

---

## STEP 10: Verify MySQL Database (CRITICAL VALIDATION)

### Connect to MySQL

```bash
kubectl exec -it deployment/mysql -n test-aman -- mysql -u root -p
```

Password:

```
root123
```

### Verify Page Stored in DB

```sql
USE wordpress;

SELECT ID, post_title, post_type, post_status
FROM wp_posts
ORDER BY post_date DESC
LIMIT 5;
```

**Result**

* Confirms WordPress UI data is stored in MySQL
* Proves persistence across pod restarts

---

## STEP 11: GitOps Rolling Update Demo

### Change Image Version in Git

```yaml
image: wordpress:6.5.3-php8.2-apache
```

Commit & push:

```bash
git commit -m "Update WordPress image version"
git push
```

### Observe ArgoCD

* App becomes OutOfSync
* ArgoCD auto-syncs
* Rolling update occurs with zero downtime

---

## STEP 12: Redeployment Strategy

If issues occur:

* Fix manifests in Git
* Delete Deployment (not namespace)
* ArgoCD redeploys automatically

```bash
kubectl delete deployment wordpress -n test-aman
```

---

## Troubleshooting Learnings

* Health probes must match application behavior
* WordPress requires relaxed probe timings
* GitOps will always override manual changes
* Private clusters require port-forwarding or tunnels

---

## Final Outcome

This project demonstrates:

* Real Kubernetes deployment
* GitOps with ArgoCD
* Resource management
* Health monitoring
* Database persistence
* Rolling updates
* Production-grade troubleshooting

---

## Final Summary Statement

> I built a production-style Kubernetes application using GitOps with ArgoCD, including persistence, health checks, resource management, and automated rolling updates, all driven entirely from Git.

---

## End

```

---

## ✅ You are DONE

This README:
- ✔ Covers **every step we followed**
- ✔ Explains **why** each step exists
- ✔ Shows **real troubleshooting**
- ✔ Is **manager & interview ready**

If you want next:
- Resume bullet points
- Architecture diagram
- Ingress + TLS
- CI/CD with GitHub Actions

Just tell me 👊
```
