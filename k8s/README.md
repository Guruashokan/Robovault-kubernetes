# RoboVault Kubernetes Deployment

Standard Kubernetes manifests for RoboVault.

## Components

- MongoDB StatefulSet with persistent volume claim
- Spring Boot backend Deployment and ClusterIP Service
- Nginx frontend Deployment and ClusterIP Service
- Dedicated robovault namespace
- Kubernetes Secret references for credentials
- Health probes and resource requests/limits

## Prerequisites

- A working Kubernetes cluster
- kubectl configured for that cluster
- A default StorageClass that supports ReadWriteOnce PVCs
- Access to the published Docker Hub images

## Deployment

1. Create the namespace:

   kubectl apply -f namespace.yaml

2. Create the required secret in the robovault namespace. It must contain:
   - MONGO_USERNAME
   - MONGO_PASSWORD
   - JWT_SECRET

   Do not commit actual credentials to Git.

3. Deploy the resources:

   kubectl apply -k .

4. Inspect resources:

   kubectl get all -n robovault
   kubectl get pvc -n robovault
   kubectl get pods -n robovault

5. Access the frontend through port forwarding:

   kubectl port-forward service/frontend 8080:80 -n robovault

   Open http://localhost:8080

## Important

The MongoDB PVC provides persistence for this Kubernetes deployment.
It does not automatically import data from Docker Compose volumes or
robovault-local-db.json.

The frontend Service is ClusterIP. External access requires port
forwarding, an Ingress controller, or a suitable LoadBalancer setup.

The application images must be rebuilt and published whenever the
application source changes.
