# Kubernetes Ingress Setup

This repository contains the Kubernetes Ingress configuration for routing traffic to different services within a cluster. The Ingress controller used is **Nginx**, and this guide provides steps to install it without Helm.

## Prerequisites

- A running Kubernetes cluster (Minikube, AWS EKS, GKE, etc.)
- `kubectl` installed and configured
- Nginx Ingress Controller installed

## Installing Nginx Ingress Controller (Without Helm)

To deploy the Nginx Ingress Controller manually, follow these steps:

### 1. Apply Mandatory Resources
```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

This command deploys the necessary components, including:
- Ingress Controller Deployment
- Service Account & Cluster Role Bindings
- Service (LoadBalancer or NodePort depending on the cloud provider)

### 2. Verify Ingress Controller Deployment
Run the following command to check if the controller is running:
```sh
kubectl get pods -n ingress-nginx
```
Expected output:
```sh
NAME                                       READY   STATUS    RESTARTS   AGE
ingress-nginx-controller-<id>              1/1     Running   0          2m
```

### 3. Get the External IP
If using a cloud provider, get the external IP assigned to the Ingress controller:
```sh
kubectl get svc -n ingress-nginx
```
If the EXTERNAL-IP is `<pending>`, wait a few minutes and try again.

## Deploying the Ingress Resource

### 1. Apply the Namespace
Ensure the `app` namespace exists before deploying the Ingress resource:
```sh
kubectl create namespace app
```

### 2. Apply the Ingress Configuration
Run the following command to create the Ingress resource:
```sh
kubectl apply -f ingress.yaml
```

### 3. Verify Ingress Configuration
Check if the Ingress resource is created successfully:
```sh
kubectl get ingress -n app
```
Expected output:
```sh
NAME          CLASS    HOSTS         ADDRESS        PORTS   AGE
app-ingress   nginx    example.com   <external-ip>  80      1m
```

## Accessing the Services
Once the Ingress resource is applied, access the services using:
- `http://example.com/nginx` → Routes to `nginx-svc` on port **80**
- `http://example.com/httpd` → Routes to `httpd-svc` on port **9000**
- `http://example.com/tomcat` → Routes to `tomcat-svc` on port **7000**

For local testing, you may need to update your `/etc/hosts` file:
```sh
echo "<external-ip> example.com" | sudo tee -a /etc/hosts
```

## Troubleshooting
- **Ingress Controller not running?**
  ```sh
  kubectl get pods -n ingress-nginx
  ```
  Restart the pod if necessary.
- **External IP is pending?**
  - Wait a few minutes and check again.
  - Ensure your cloud provider supports LoadBalancer services.
- **404 Not Found when accessing services?**
  - Verify that the backend services (`nginx-svc`, `httpd-svc`, `tomcat-svc`) are running.
  - Ensure the paths in `ingress.yaml` match the service names.

## Repository
[GitHub Repository](https://github.com/TusharGayakwad/K8s_Ingress)

---
**Author:** Tushar Gayakwad

