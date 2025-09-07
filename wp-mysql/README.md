# Day 3 – WordPress + MySQL with ConfigMap, PV & PVC

## 1. Objective

Introduce persistence and configuration management in Kubernetes using:

- **PersistentVolumes (PV) + PersistentVolumeClaims (PVC)**
- **ConfigMaps** for storing paths and settings
- **resources: request and limits** 

---

## 2. Folder Structure

wp-mysql/
├── mysql-pv.yml
├── mysql-pvc.yml
├── mysql-deployment.yml
├── wordpress-configMap.yml
├── wordpress-deployment.yml
├── wordpress-svc.yml
├── wp-namespace.yml
└── README.md

---

## 3. Steps

### 3.1 ConfigMap and secret

- File: `wordpress-configMap.yml`  
- Purpose: Defines base storage path or settings for WordPress

- File: `mysql-secret.yml`  
- Purpose: Defines the secret for mysql db

---

### 3.2 PersistentVolume + PersistentVolumeClaim

- Files: `mysql-pv.yml`, `mysql-pvc.yml`  
- **PV**: Maps to a local `hostPath` directory (Windows path under Docker Desktop)  
- **PVC**: Requested by MySQL pod to persist `/var/lib/mysql`

---

### 3.3 Resoruces: Requests and Limits
    requests define the minimum resources a pod gets, ensuring stable scheduling. Limits cap the max resources a pod can consume, preventing noisy neighbor

```text
Node total capacity
+----------------------------------------+
| CPU: 2000m (2 cores)                   |
| Memory: 4Gi                             |
|                                        |
|  +------------------------------+      |
|  | Pod A                        |      |
|  | requests: 250m CPU, 256Mi    |      |
|  | limits: 500m CPU, 512Mi      |      |
|  +------------------------------+      |
|                                        |
|  +------------------------------+      |
|  | Pod B                        |      |
|  | requests: 500m CPU, 512Mi    |      |
|  | limits: 1 CPU, 1Gi           |      |
|  +------------------------------+      |
|                                        |
|  Remaining free resources:             |
|  CPU: 1250m, Memory: 3Gi               |
+----------------------------------------+
Requests = minimum guaranteed resources

Limits = maximum allowed resources

3.4 MySQL Deployment + Service

Mounts PVC to /var/lib/mysql

Ensures MySQL data persists even if pod restarts

3.5 WordPress Deployment + Service

Connects to MySQL using ConfigMap + Secret

Liveness and Readiness Probes ensure pods are healthy and ready before serving traffic

3.6 Verify

    kubectl get pvc -n practice
    kubectl get pv -n practice
    kubectl get pods -n practice

Access WordPress

    Open in browser:

    http://localhost:<nodePort>
