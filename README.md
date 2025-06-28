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
### 1. Create Dockerfile for each micorservice individually. Use below for your reference.
```bash
FROM node:22
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3002
CMD [ "npm", "start" ]
```
Repeat for each service.

### 2. Build & Push Docker Images
For each microservice (`user-service`, `product-service`, `order-service`, `gateway-service`), navigate to the folder and run:
```bash
cd .\Microservices\<service-folder>
docker build -t your-dockerhub-username\<service-name>:latest .
docker push your-dockerhub-username/<service-name>:latest
```
Example:
```bash
cd .\Microservices\user-service
docker build -t your-dockerhub-username/user-service:latest .
docker push your-dockerhub-username/user-service:latest
```
Repeat for each service.

### 3. Start Minikube
```bash
minikube start
```
(Optional for Ingress):
```bash
minikube addons enable ingress
```

### 4. Deploy Services and Deployments
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

### 5. Testing Services
- A. Port-forward Service
```bash
kubectl port-forward svc/servicename PORT:SVCPORT
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

- B. Inter-service Communication (Inside Pod)
```bash
kubectl exec -it <some-pod-name> -- sh
curl http://user-service:3000/health
curl http://product-service:3001/health
```

### 6. (Optional) Ingress Setup
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

## Troubleshooting Tips<br>
- Pod CrashLoopBackOff: Check logs with kubectl logs <pod-name><br>
- Image Pull Error: Ensure your image name is correct and pushed to Docker Hub<br>
- Service Unreachable: Verify correct port and service name in your requests<br>
---

## Screenshots
- Docker images snapshot<br>
<img width="661" alt="image" src="https://github.com/user-attachments/assets/cf6f0901-0084-4b4d-9575-2ca0fe924fd8" /><br>
- Minikube running snapshot<br>
<img width="746" alt="image" src="https://github.com/user-attachments/assets/2da2071a-4255-4846-b92c-b9714454634a" /><br>
- Applying deployment snapshot<br>
<img width="376" alt="image" src="https://github.com/user-attachments/assets/927780c9-8a59-449c-b94d-0f8a56e41248" /><br>
- Applying services snapshot<br>
<img width="349" alt="image" src="https://github.com/user-attachments/assets/040694c8-882c-4871-bb39-dc03b304f245" /><br>
- kubectl services status snapshot<br>
<img width="368" alt="image" src="https://github.com/user-attachments/assets/29885cb6-76b7-43e3-9bbd-1295a4cafd2b" /><br>
- kubectl pods status snapshot<br>
- product service snaphsot<br>
<img width="384" alt="image" src="https://github.com/user-attachments/assets/00876c46-4e12-46c7-a6d5-b1d60fc69a8c" /><br>
- user service snaphsot<br>
- gateway service snaphsot<br>
- order service snaphsot<br>
---

## Author & Credits
- Assignment by Tanuj Bhatia<br>
- Container registry: tanujbhatia24
---
