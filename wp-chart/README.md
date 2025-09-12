# WordPress Helm Chart with MySQL

This Helm chart deploys **WordPress** with a **MySQL** database on Kubernetes.  
It includes instructions to initialize the database and user for WordPress.

---

## Prerequisites

- Kubernetes cluster up and running.
- Helm installed.
- `kubectl` configured to access your cluster.

---

## Helm Installation

1. Navigate to your chart directory:

```bash
cd path/to/wp-chart
Install the Helm chart:

```bash
create namespace wp-app
helm install my-wordpress . -n wp-app
Release name: my-wordpress

Namespace: wp-app

Deleting and Reinstalling Helm Release
helm uninstall my-wordpress -n wp-app
kubectl delete pvc -n wp-app --all
helm install my-wordpress . -n wp-app

exec into container 
    kubectl exec -it <mysql-pod-name> -n wp-app -- mysql -u root -p'root123'

Run the SQL commands if the database has not created

CREATE DATABASE IF NOT EXISTS wordpress;
CREATE USER 'wpuser'@'%' IDENTIFIED BY 'wppassword123';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'%';
FLUSH PRIVILEGES;


Access wordpress

 http://<NodeIP>:<NodePort>
