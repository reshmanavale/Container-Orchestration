# Microservices Kubernetes Deployment

This project demonstrates deploying a microservices architecture on Kubernetes using Minikube. The application consists of four services: User, Product, Order, and Gateway services.

## Prerequisites

- Docker installed
- Minikube installed
- kubectl installed

## Minikube Setup Instructions

1. Start Minikube with adequate resources:

```bash
minikube start --driver=docker --cpus=2 --memory=4g
```


![image](https://github.com/user-attachments/assets/933fd54f-550e-49d3-84b3-18f8c5d1293f)

2. Enable the Ingress controller:

```bash
minikube addons enable ingress
```

![image](https://github.com/user-attachments/assets/e63dc16d-8998-4ae7-8d04-f157daee51ad)


## Building the Docker Images

If you need to build the Docker images locally:

```bash
# Navigate to each service directory and build the image
docker build -t your-registry/user-service:latest ./user-service
docker build -t your-registry/product-service:latest ./product-service
docker build -t your-registry/order-service:latest ./order-service
docker build -t your-registry/gateway-service:latest ./gateway-service
```
Since we have already Build it ealier, we'll use those : 




## Deployment Steps

1. Apply all deployment configurations:

```bash
kubectl apply -f deployments/
```

2. Apply all service configurations:

```bash
kubectl apply -f services/
```


Port Forward and Test the services : 
![Screenshot 2025-04-12 125146](https://github.com/user-attachments/assets/b93f73c8-0474-4bc7-910f-7432f55530a6)

![Screenshot 2025-04-12 135237](https://github.com/user-attachments/assets/bc6e98fb-9d44-48af-8627-74aa1b61976d)

![image](https://github.com/user-attachments/assets/cddaa658-fccc-4f97-a3a7-3d27b1e5aa8c)

![image](https://github.com/user-attachments/assets/bbd171ef-72e9-473b-8300-5170da901b79)

![image](https://github.com/user-attachments/assets/b6d51c81-3a8f-4ad8-8a13-eb90046ac781)



4. Apply the Ingress configuration:

```bash
kubectl apply -f ingress/
```

5. Verify all resources are created correctly:

```bash
kubectl get deployments
kubectl get services
kubectl get pods
kubectl get ingress
```

## Testing Procedures

1. Add the hostname to your local hosts file:

```bash
echo "$(minikube ip) microservices.local" | sudo tee -a /etc/hosts
```

2. Access the services via the Ingress:

```bash
# User Service
curl http://microservices.local/api/users

# Product Service
curl http://microservices.local/api/products

# Order Service
curl http://microservices.local/api/orders

# Gateway Service (default route)
curl http://microservices.local/
```

3. Verify internal service communication:

```bash
# Get a pod name
kubectl get pods

# Execute a shell in the gateway pod
kubectl exec -it <gateway-pod-name> -- /bin/sh

# From inside the pod, test communication with other services
curl http://user-service:3000/users
curl http://product-service:3001/products
curl http://order-service:3002/orders
```

## Troubleshooting Guide

### Common Issues and Solutions

1. **Pods are not running:**
   - Check pod status: `kubectl get pods`
   - Check detailed pod information: `kubectl describe pod <pod-name>`
   - Check pod logs: `kubectl logs <pod-name>`

2. **Services are not accessible:**
   - Verify service endpoints: `kubectl get endpoints`
   - Test service inside the cluster: `kubectl run -i --tty --rm debug --image=busybox --restart=Never -- wget -O- <service-name>:<port>/`

3. **Ingress is not routing correctly:**
   - Check Ingress status: `kubectl describe ingress microservices-ingress`
   - Verify Ingress controller is running: `kubectl get pods -n kube-system`
   - Check Ingress controller logs: `kubectl logs -n kube-system <ingress-controller-pod-name>`

4. **DNS issues:**
   - Verify DNS resolution: `kubectl run -i --tty --rm debug --image=busybox --restart=Never -- nslookup <service-name>`

5. **Resource constraints:**
   - Check resource usage: `kubectl top pods`
   - Increase Minikube resources if needed: `minikube stop && minikube start --driver=docker --cpus=4 --memory=8g`

### If All Else Fails

Reset your Minikube cluster:

```bash
minikube delete
minikube start --driver=docker --cpus=2 --memory=4g
```
