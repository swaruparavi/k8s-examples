# Day 5 – Ingress Controller, RBAC, and NetworkPolicy

This document explains the key Kubernetes concepts of **Ingress Controller**, **Role-Based Access Control (RBAC)**, and **NetworkPolicy**, including why they are used and how to implement them.

The files: **ingress.yaml**, **networkpolicy.yaml**, **rbac.yaml**

---

## 1️⃣ Ingress Controller -- ingress.yaml

### What It Is
- An **Ingress Controller** manages external HTTP/HTTPS access to Kubernetes services.
- It watches for `Ingress` resources and configures a reverse proxy (like NGINX) to route traffic to the appropriate service inside the cluster.

### Why We Need It
- Services of type `NodePort` or `LoadBalancer` can expose applications but are harder to manage at scale.
- Ingress provides **centralized routing**, **path/host-based routing**, and **TLS termination**.

### Key Components
- **Ingress Controller**: The pod/deployment (e.g., NGINX, HAProxy, Traefik) that processes ingress rules.
- **Ingress Resource**: Defines rules (host, path, backend service) for routing.

### Steps to Implement
1. Deploy NGINX Ingress Controller (via Helm or manifest).
   Kubectl:
      kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.0/deploy/static/provider/kind/deploy.yaml
   
   Helm:

   ```
     helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
     helm repo update

    # Install the ingress-nginx controller in the namespace 'ingress-nginx'
    helm install ingress-nginx ingress-nginx/ingress-nginx \
    --namespace ingress-nginx --create-namespace
    ```

2. Create a Service and Deployment (e.g., WordPress) inside your namespace.
3. Create an Ingress resource specifying:
   - Host (e.g., `wordpress.local`)
   - Backend service name and port.
4. Update local DNS (`/etc/hosts` or Windows hosts file) to map `wordpress.local` to the cluster IP.

### Debugging Tips
```bash
kubectl describe ingress <name>
kubectl logs -n ingress-nginx <controller-pod>
kubectl get endpoints <service>
```

## Role-Based Access Control (RBAC)  -- rbac.yaml
 
    RBAC controls who can perform what actions on Kubernetes resources.

    It uses Roles/ClusterRoles and RoleBindings/ClusterRoleBindings to define permissions.

### Why We Need It

Enhances security by granting least privilege.

Prevents unauthorized access to sensitive resources.

### Debugging Tips:
   kubectl auth can-i get pods --as=pod-reader


## NetworkPolicy -- networkpolicy.yaml

### What It Is

A NetworkPolicy defines how pods are allowed to communicate with each other and with external endpoints.

It works with a compatible CNI (Container Network Interface) like Calico, Cilium, or Weave.

### Why We Need It

By default, all pods can talk to all others.

NetworkPolicy allows you to restrict traffic to meet security/compliance needs.

### Types of Rules

Ingress Rules: Control incoming traffic to pods.

Egress Rules: Control outgoing traffic from pods.

### Debugging Tips:

kubectl describe networkpolicy <name>
kubectl run -it test --image=busybox sh
wget mysql-service:3306


 **Debugging Scenarios**:

 1. Ingress not accessible
DNS resolves (ping works) but site times out.
1️⃣ Check if Ingress Controller pods are running (kubectl get pods -n ingress-nginx).
2️⃣ kubectl describe ingress – check events for backend errors.
3️⃣ Ensure ingress.class matches controller.
4️⃣ Use kubectl logs -n ingress-nginx <controller-pod> to inspect NGINX logs.
5️⃣ Check service endpoints (kubectl get endpoints <svc>).

 2. Pod cannot talk to another service after applying NetworkPolicy
Application errors like connection refused.
1️⃣ kubectl describe networkpolicy <policy> – check podSelector and namespaceSelector.
2️⃣ Ensure ingress/egress rules allow required traffic.
3️⃣ Run kubectl run -it test --image=busybox sh inside same namespace and try wget <svc>:<port>.