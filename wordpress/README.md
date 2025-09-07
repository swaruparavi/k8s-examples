Secrets (mysql-secret.yaml)  -- Stores MySQL root password securely.

MySQL Deployment + Service (mysql-deployment.yaml, mysql-svc.yaml)  -- Runs a MySQL 8.0 pod.

    Exposes it internally via ClusterIP service.

WordPress Deployment + Service (wordpress-deployment.yaml, wordpress-svc.yaml)

    Connects to MySQL using env variables (DB host, password from secret).

    Exposed using NodePort so WordPress is accessible via browser.

Verify

kubectl get pods -n practice
kubectl get svc -n practice