Secrets (mysql-secret.yaml)  -- Stores MySQL root password securely.

MySQL Deployment + Service (mysql-deployment.yaml, mysql-svc.yaml)  -- Runs a MySQL 8.0 pod.

    Exposes it internally via ClusterIP service.

WordPress Deployment + Service (wordpress-deployment.yaml, wordpress-svc.yaml)

    Connects to MySQL using env variables (DB host, password from secret).

    Exposed using NodePort so WordPress is accessible via browser.

Verify

kubectl get pods -n practice
kubectl get svc -n practice


**Debugging Scenraios**

Scenario:		
1. Pod CrashLoopBackOff	
  Symptoms:  kubectl get pods shows CrashLoopBackOff.	
  Debugging Steps:
    1️⃣ kubectl describe pod <pod> – check Events for OOMKilled, ImagePullBackOff, or readiness failures.
    2️⃣ kubectl logs <pod> – inspect container logs.
    3️⃣ Verify resource requests/limits in YAML.
    4️⃣ If OOMKilled → increase memory limits.
    5️⃣ If ImagePullBackOff → check image name/tag, registry credentials.

2. Service not reachable	
   Symptoms:  kubectl get svc shows service, but app not accessible.
   Debugging Steps:	
    1️⃣ kubectl get endpoints <svc> – ensure pods have valid endpoints.
    2️⃣ Check pod labels match service selector.
    3️⃣ Use kubectl port-forward svc/<svc> <port> to test connectivity.
    4️⃣ Curl inside cluster (kubectl run -it --rm test --image=busybox sh).

3. NodePort unreachable externally	
Symptoms: NodePort service exposed but curl fails from host.	
Debugging Steps:
    1️⃣ Check firewall rules (Windows/Mac Docker Desktop).
    2️⃣ Verify cluster node IP and correct nodePort.
    3️⃣ If on cloud (EKS/GKE), check security group or firewall rules.