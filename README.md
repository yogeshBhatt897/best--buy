# Project Assignment #2: Cloud-Native App Best-Buy Application

This repository contains the source code, documentation, and deployment files for the Best Buy demo cloud-native application. The application follows a microservices architecture and incorporates AI capabilities for product descriptions and image generation.

## Application Architecture

### Application Table

| Service              | Description                                                       | Notes                           |
| -------------------- | ----------------------------------------------------------------- | ------------------------------- |
| **Store-Front**      | Customer-facing app for browsing and placing orders              | -                               |
| **Store-Admin**      | Employee-facing app for managing products and viewing orders.    | -                               |
| **Order-Service**    | Handles order creation and sends data to the managed order queue. | Uses RabbitMQ for messaging.    |
| **Product-Service**  | Handles CRUD operations for product data.                         | -                               |
| **Makeline-Service** | Processes and completes orders by reading from the order queue.   | -                               |
| **AI-Service**       | Generates product descriptions and images.                        | Uses GPT-4 and DALL-E-3 models. |
| **Database**         | MongoDB for persisting order and product data.                    | -                               |

### Diagram

![Best Buy Application Architecture](image.png)

### Architecture Overview

- **Store-Front**: React.js application deployed as a microservice to allow customers to browse products and place orders.
- **Store-Admin**: Admin portal for employees to manage inventory and view customer orders.
- **Order-Service**: Processes incoming orders and pushes them to Azure Service Bus.
- **Product-Service**: Manages product data, interfacing with the database and the AI service.
- **Makeline-Service**: Pulls order details from Azure Service Bus, processes them, and updates order status.
- **AI-Service**: Integrates with OpenAI APIs to generate product descriptions and images dynamically.
- **MongoDB**: Used as a database for storing product and order information.

## Prerequisites

- **Azure account** for Azure Service Bus and Azure Kubernetes Service (AKS).
- **Docker** installed and configured.
- **kubectl CLI** installed and configured for your AKS cluster.
- Access to **OpenAI APIs** for GPT-4 and DALL-E.

## Steps

1. Clone the Repository

```bash
git clone https://github.com/yogeshBhatt897/best--buy.git
cd best-buy-app
```

2. Build and Push Docker Images:
```bash
For each microservice, navigate to its directory and run:

docker build -t yogeshbhatt0199/<service-name>:laatest .

docker push yogeshbhatt0199/<service-name>:latest
```
3. Setup Azure Resources
```bash
Resource group name: assignment

Region: East US.

Click Review + Create and then Create.
```

4. Create AKS Clusters

- Select **agentpool**. This nodes will have the controlplane.
        - Set **node size** to `D2as_v4`.
        - **Scale method**: `Manual`
        - **Node count**: `1`
        - Click `update`
     - Click on **Add node pool**:
        - **Node pool name**: `workernodes`.
        - **Mode**: `User` 
        - Set **node size** to `D2as_v3`.
        - **Scale method**: `Manual`
        - **Node count**: `2`
        - Click `add`
   - Click **Review + Create**, and then **Create**. The deployment will take a few minutes.

**Deploy to Kubernetes** Apply the deployment YAML files:
```
kubectl apply -f Deployment Files/
 ```

4. Verify Deployment:
   Ensure all pods are running
```
kubectl get pods
```
5. Deploy Microservices

Apply the deployment YAML files for each microservice:

```
kubectl apply -f Deployment_Files/store-front.yaml -n best-buy-app
kubectl apply -f Deployment_Files/store-admin.yaml -n best-buy-app
kubectl apply -f Deployment_Files/order-service.yaml -n best-buy-app
kubectl apply -f Deployment_Files/product-service.yaml -n best-buy-app
kubectl apply -f Deployment_Files/makeline-service.yaml -n best-buy-app
kubectl apply -f Deployment_Files/ai-service.yaml -n best-buy-app

```
### Issues and Limitations
1. **Azure Service Bus Integration**
   - Replacing RabbitMQ required significant code for message handling.
2. **Azure AI Service**
   - Facing issue while implementing ai service.
  

3. **Docker Image Optimization**:
   - Large image sizes affected deployment time.
   - Resolved with multi-stage builds and minimal base images.

4. **CI/CD Pipeline Setup**:
   - Automation for builds and deployments posed initial challenges.
   - Overcame using GitHub Actions workflows.

---


## Repositories
This table lists the GitHub repositories for each of the microservices in the project:

| **Service**        | **Repository Link**                          |
|--------------------|----------------------------------------------|
| Store-Front        | https://github.com/yogeshBhatt897/store-front-l8.git |
| Order-Service      | https://github.com/yogeshBhatt897/order-service-l8.git |
| Product-Service    | https://github.com/yogeshBhatt897/product-service-l8.git |
| Makeline-Service   | https://github.com/yogeshBhatt897/makeline-service-l8.git |
| Store-Admin        | https://github.com/yogeshBhatt897/store-admin-l8.git |
| AI-Service         | https://github.com/yogeshBhatt897/ai-service-l8.git |

## Docker Images
This table lists the docker images for each of the microservices in the project:

| **Service**        | **Docker Image Link**                          |
|--------------------|----------------------------------------------|
| Store-Front        | https://hub.docker.com/repository/docker/yogeshbhatt0199/store-front1/general |
| Order-Service      | https://hub.docker.com/repository/docker/yogeshbhatt0199/order-service/general|
| Product-Service    | https://hub.docker.com/repository/docker/yogeshbhatt0199/product-service1/general |
| Makeline-Service   | https://hub.docker.com/repository/docker/yogeshbhatt0199/makeline-service/general |
| Store-Admin        | https://hub.docker.com/repository/docker/yogeshbhatt0199/store-admin1/general|
| AI-Service         | https://hub.docker.com/repository/docker/yogeshbhatt0199/ai-service/general |


## Demo Video:
(https://algonquinlivecom-my.sharepoint.com/:v:/g/personal/bhat0199_algonquinlive_com/Ec2aY3T2AcBAvYMLOV5J2AEBBgeJ-0jCcgYBa1z64G9Wrw?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=UaJGwz)
