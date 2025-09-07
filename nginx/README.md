1. Create a Namespace

    kubectl create namespace practice

2. Apply Manifests

    kubectl apply -f <file-name>.yml -n practice


    Deployment (nginx-deployment.yml)  -- Deploys an Nginx pod with replica management.

    Service (nginx-service.yml) -- Exposes Nginx inside the cluster.

    NodePort (nginx-nodeport.yml)

3. (Alternative) Create NodePort via CLI

    Instead of YAML, you can directly expose the deployment:

    kubectl expose deployment nginx-deployment \
    --type=NodePort \
    --port=80 \
    --name=nginx-nodeport \
    -n practice

4. Access the Service
    http://localhost:<nodePort>

5. Verify Resources
    kubectl get pods -n practice
    kubectl get svc -n practice
