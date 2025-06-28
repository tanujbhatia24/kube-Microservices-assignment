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
![image](https://github.com/user-attachments/assets/6c759d3c-dbbc-4f31-a2b1-213d184efa30)<br>
- Product service snaphsot<br>
<img width="384" alt="image" src="https://github.com/user-attachments/assets/00876c46-4e12-46c7-a6d5-b1d60fc69a8c" /><br>
![image](https://github.com/user-attachments/assets/84aa3374-79ad-4a58-abe4-cdc2f1362de7)<br>
- User service snaphsot<br>
<img width="407" alt="image" src="https://github.com/user-attachments/assets/8d751267-9b8c-45d3-b53f-25891d8bbc46" /><br>
<img width="464" alt="image" src="https://github.com/user-attachments/assets/29b6a93f-6a61-4128-a7cb-d6f5331698cd" /><br>
- Gateway service snaphsot<br>
![image](https://github.com/user-attachments/assets/e0ad6740-7b81-4358-b6f5-4f4d1ff3c551)<br>
![image](https://github.com/user-attachments/assets/f7f69de1-5aaa-41b5-876d-bdeccdd61d20)<br>
- Order service snaphsot<br>
![image](https://github.com/user-attachments/assets/517f7f87-673d-473b-a238-b74a56ba1850)<br>
![image](https://github.com/user-attachments/assets/c80d58e8-f394-4538-be71-2eebad732daa)<br>
- Inter-service Communication (Inside Pod) snapshot<br>
![image](https://github.com/user-attachments/assets/395f78ef-0857-487f-8340-615e3e996bb7)<br>
- (Optional) Ingress Snapshot<br>
![image](https://github.com/user-attachments/assets/3d45525b-d2f4-4e55-8ac4-ef37d0739cfc)<br>
![image](https://github.com/user-attachments/assets/afa47776-ae81-456e-98a8-954b07370977)<br>
---

## Author & Credits
- Assignment by Tanuj Bhatia<br>
- Container registry: tanujbhatia24
---
