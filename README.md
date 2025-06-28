# Microservices Deployment on Kubernetes with Minikube
This project demonstrates how to deploy a microservices-based Node.js application using Kubernetes and Minikube. The application includes the following services:

- **User Service** (Port 3000)
- **Product Service** (Port 3001)
- **Order Service** (Port 3002)
- **Gateway Service** (Port 3003)
---

## Prerequisites
- Docker
- Minikube
- kubectl
- Docker Hub account (or another container registry)
---

## Step-by-Step Instructions
### 1. Build & Push Docker Images
For each microservice (`user-service`, `product-service`, `order-service`, `gateway-service`), navigate to the folder and run:
```bash
docker build -t your-dockerhub-username/<service-name>:latest .
docker push your-dockerhub-username/<service-name>:latest
```
Example:
```bash
cd user-service
docker build -t your-dockerhub-username/user-service:latest .
docker push your-dockerhub-username/user-service:latest
```
Repeat for each service.

### 2. Start Minikube
```bash
minikube start --memory=4096 --cpus=2
```
(Optional for Ingress):
```bash
minikube addons enable ingress
```

### 3. Deploy Services and Deployments
From the root directory:
```bash
kubectl apply -f deployments/
kubectl apply -f services/
```
Check status:
```bash
kubectl get pods
kubectl get services
```

### 4. Testing Services
A. Port-forward Service
```bash
kubectl port-forward svc/servicename PORT:SVC PORT
```
Then open your browser:
```bash
http://localhost:PORT/
```
EXAMPLE
Gateway Service
```bash
kubectl port-forward svc/gateway-service 3003:3003
```
open your browser and hit "http://localhost:3003/"

B. Inter-service Communication (Inside Pod)
```bash
kubectl exec -it <some-pod-name> -- sh
curl http://user-service:3000/health
curl http://product-service:3001/health
```

### 5. (Optional) Ingress Setup
If you attempted the bonus task:
```bash
kubectl apply -f ingress/
minikube tunnel
```
Then add this to your /etc/hosts file:
```bash
127.0.0.1 microservices.local
```
Test
```bash
http://microservices.local/api/users
```
---

## Troubleshooting Tips
Pod CrashLoopBackOff: Check logs with kubectl logs <pod-name><br>
Image Pull Error: Ensure your image name is correct and pushed to Docker Hub<br>
Service Unreachable: Verify correct port and service name in your requests<br>
---

## Screenshots
Docker images snapshot<br>
<img width="755" alt="image" src="https://github.com/user-attachments/assets/f45d4cee-0b35-44f0-afdd-0a71a9346ad5" /><br>
Minikube running snapshot<br>
<img width="746" alt="image" src="https://github.com/user-attachments/assets/2da2071a-4255-4846-b92c-b9714454634a" /><br>

---

## Author & Credits
Assignment by Tanuj Bhatia
Container registry: tanujbhatia24
---
