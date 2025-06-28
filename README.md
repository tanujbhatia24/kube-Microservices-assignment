# Microservices Dockerization Project

This project contains Dockerized Node.js microservices orchestrated with Docker Compose. It includes:

- **User Service** (Port `3000`)
- **Product Service** (Port `3001`)
- **Order Service** (Port `3002`)
- **Gateway Service** (Port `3003`)

---

## Project Structure
```
Microservices
├── user-service/
│ └── Dockerfile
├── product-service/
│ └── Dockerfile
├── order-service/
│ └── Dockerfile
├── gateway-service/
│ └── Dockerfile
├── docker-compose.yml
└── README.md
└── READMEinitial.md
```
---

## Setup Instructions

### 1. Clone the Repository.

```bash
git clone <your-forked-repo-url>
```
### 2. You need to fix the package.json with below code.
Troubleshoot - there's no scripts section, so when Docker runs npm start, it doesn't know what to do.
```bash
{
  "name": "microservice",
  "version": "1.0.0",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.17.1",
    "axios": "^0.24.0"
  }
}
```

### 3. Create Dockerfile for each micorservice individually. Use below for your reference.
```bash
FROM node:22
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3002
CMD [ "npm", "start" ]
```

### 4. Build and Run the images individually for each microservice.
```bash

#For User Service
docker build . -t users-service:v1.0
docker run -d -p 3000:3000 users-service:v1.0

#For Products Service
docker build . -t product-service:v1.0
docker run -d -p 3001:3001 products-service:v1.0

#For Orders Service
docker build . -t order-service:v1.0
docker run -d -p 3002:3002 orders-service:v1.0

#For Gateway Service
docker build . -t gateway-service:v1.0
docker run -d -p 3003:3003 gateway-service:v1.0
```

### 5. Create docker-compose.yml file. Use below for your reference.
```bash
version: '3.8'

services:
  users-service:
    build: ./user-service
    container_name: users-service_container
    ports:
      - '3000:3000'

  products-service:
    build: ./product-service
    container_name: products-service_container
    ports:
      - '3001:3001'

  orders-service:
    build: ./order-service
    container_name: orders-service_container
    ports:
      - '3002:3002'
    depends_on:
      - users-service

  gateway-service:
    build: ./gateway-service
    container_name: gateway-service_container
    ports:
      - '3003:3003'

networks:
  default:
    name: microservice-network
```

### 6. Build and Run the image docker-compose.
```bash
docker compose build
docker compose up -d
```

## Valdation Snapshots.

1. Docker Individual image each microservice.<br>
![image](https://github.com/user-attachments/assets/772ff546-a35a-47e0-951c-26447b7fe7f4)<br>

2. Docker-Compose image & container for combine microservice.<br>
![image](https://github.com/user-attachments/assets/7ae3d8d4-54d8-482c-95a4-109cf8d75554)<br>
![image](https://github.com/user-attachments/assets/f681b319-74ed-4d97-bffa-2d4a4b8d277f)<br>

3. users-service<br>
![image](https://github.com/user-attachments/assets/47fccb13-80e2-4d8e-aae7-94209538706f)<br>

4. products-service<br>
![image](https://github.com/user-attachments/assets/959217ac-59e5-4aa2-a1fb-076df4ceff3c)<br>

5. orders-service<br>
![image](https://github.com/user-attachments/assets/1ff0501c-9ae2-4d90-9691-a8bd78939050)<br>
![image](https://github.com/user-attachments/assets/5d85c45d-dd19-4ad8-a948-96c69a9a156e)<br>

6. gateway-service<br>
![image](https://github.com/user-attachments/assets/a2e9e9e1-6be7-4bfe-8cba-9e8ea6999a19)<br>


