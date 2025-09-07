1. Objective

    Introduce persistence and configuration management in Kubernetes using:

    PersistentVolumes (PV) + PersistentVolumeClaims (PVC)

    ConfigMaps for storing paths and settings.

2. Steps

ConfigMap (wordpress-configMap.yaml) --- Defines base storage path or settings for MySQL/WordPress.

PersistentVolume + PersistentVolumeClaim (mysql-pv.yaml, mysql-pvc.yaml)

PV: Maps to a local hostPath directory (Windows path under Docker Desktop).

PVC: Requested by MySQL pod to persist /var/lib/mysql.

MySQL Deployment with PVC and MySQL Service

Mounts PVC to /var/lib/mysql.

Ensures MySQL data persists even if pod restarts.

WordPress Deployment and wordpress Service

Liveness and Readiness Probes

Ensure pods are healthy and ready before serving traffic.

Verify

kubectl get pvc -n practice
kubectl get pv -n practice
kubectl get pods -n practice

Access wordpress Service (http://localhost:<nodePort>) 