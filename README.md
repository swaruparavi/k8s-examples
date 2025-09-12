# Kubernetes Examples

This repository offers a structured, day-wise learning journey through Kubernetes, from basic deployments to more advanced concepts like persistence and configuration management.
---

##  Learning Path

### ** Day 1 – Nginx Deployment**
- Deploys a simple Nginx Pod using a `Deployment`.
- Exposes it through a `Service` and `NodePort`.
- Access via `http://localhost:<nodePort>`.
- [See details →](./nginx/README.md)

### ** Day 2 – WordPress + MySQL (Basic)**
- Deploys MySQL with a Secret for the root password.
- Deploys WordPress linked to the MySQL backend.
- Exposes WordPress via `NodePort`.
- [See details →](./wordpress/README.md)

### ** Day 3 – WordPress + MySQL with PVC & ConfigMap**
- Adds a `PersistentVolume` and `PersistentVolumeClaim` for durable data storage.
- Uses a `ConfigMap` and `Secret` for configuration and credentials.
- Introduces liveness/readiness probes for reliability.
- [See details →](./wp-mysql/README.md)

### ** Day 4 – WordPress Helm Chart with MySQL**
- An **Ingress Controller** manages external HTTP/HTTPS access to Kubernetes services..
- RBAC controls who can perform what actions on Kubernetes resources
- A NetworkPolicy defines how pods are allowed to communicate with each other and with external endpoints
- [See details →](./wp-chart/README.md)
- 
### ** Day 5 – WordPress + MySQL with PVC & ConfigMap , Ingress Controller , RBAC and NetworkPolicy**
- An **Ingress Controller** manages external HTTP/HTTPS access to Kubernetes services..
- RBAC controls who can perform what actions on Kubernetes resources
- A NetworkPolicy defines how pods are allowed to communicate with each other and with external endpoints
- [See details →](./wp-chart/README2.md)

---

##  Requirements

- Docker Desktop (with Kubernetes enabled)
- `kubectl` CLI installed
- Git

--- 
   cd k8s-examples

