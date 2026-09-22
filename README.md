<h1>Microservices Deployment on AWS EKS</h1>



<h2>Description</h2>
This project demonstrates the transformation of a food-ordering application from a monolithic architecture into a containerized microservices architecture using Amazon EKS and Kubernetes.


<h2>The application consists of:</h2>

- <b>Ruby frontend service</b> 
- <b>NodeJS backend service</b>
- <b>Crystals backend service</b>

The solution uses Kubernetes deployments, services, replicas, and an AWS load balancer to provide application availability, networking, and horizontal scalability.
<br />


<h2>Components</h2>

- <b>Amazon EKS</b>
- <b>Amazon EC2 worker nodes</b>
- <b>AWS IAM</b>
- <b>Docker/container imagesHub</b>
- <b>Kubernetes Deployment</b>
- <b>Pods</b>
- <b>Kubernetes Services</b>
- <b> Replicas
- <b>AWS Load Balancer</b>
- <b>Kubectl</b>
- <b>AWS CLI</b>




<h2>Implementation:</h2>


1. ##  AWS CLI
   
- <b>aws configure
- <b>Set the AWS region to:
us-east-1

2. ## Create EKS cluster
Created an Amazon EKS cluster in the US East (N. Virginia) region to provide managed Kubernetes orchestration for the food-ordering microservices application.

Configure kubectl

Connect the local Kubernetes configuration to the EKS cluster:
- <b>aws eks --region us-east-1 update-kubeconfig --name ample-eks-microservices</b>

- <b>Verify the cluster:</b>

 kubectl get nodes

 <img width="814" height="409" alt="image" src="https://github.com/user-attachments/assets/adf92671-b7fd-4a0c-8216-cc7ff79ba42f" />

 IAM role for worker nodes was created

 
3. ## Create EKS Worker Nodes
   
An EKS node group was configured with multiple EC2 worker nodes to support application availability and workload distribution. 

<img width="1108" height="606" alt="image" src="https://github.com/user-attachments/assets/083cf2da-7158-4d01-abcb-57ed095f99ab" />
Three worker nodes were created to have multiple places where Kubernetes can schedule pods.


<h2>Microservices:</h2>

## Frontend
The Ruby-based frontend is deployed as an independent Kubernetes Deployment and exposed through a Kubernetes LoadBalancer Service.

## NodeJS Backend
The NodeJS backend is deployed independently using its own Kubernetes Deployment and Service.

## Crystal Backend
The Crystal backend is deployed independently using its own Kubernetes Deployment and Service.

## Deploy NodeJS Backend
cd ecsdemo-nodejs/kubernetes

- <b> apply -f deployment.yaml
- <b> apply -f service.yaml



## Deploy Crystal Backend
cd ecsdemo-crystal/kubernetes

- <b> kubectl apply -f deployment.yaml
- <b> kubectl apply -f service.yaml



## Deploy Frontend
cd ecsdemo-frontend/kubernetes

- <b> apply -f deployment.yaml
- <b> apply -f service.yaml

## Verify
- <b> Kubectl get deployment
- <b> Kubectl get service

## Scale the Application

<img width="1028" height="284" alt="image" src="https://github.com/user-attachments/assets/33f0e7e0-ddfc-4f36-9862-9c13890ee713" />

- <b> Scale the NodeJS backend:
kubectl scale deployment ecsdemo-nodejs --replicas=3

- <b> Scale the Crystal backend:
kubectl scale deployment ecsdemo-crystal --replicas=3

- <b> Scale the frontend:
kubectl scale deployment ecsdemo-frontend --replicas=3

## Project Outcome

Designed and deployed a containerized food-ordering application on Amazon EKS using Kubernetes. Configured application services, external load balancing, multiple replicas, and horizontal scaling to demonstrate a scalable microservices architecture.


---

## Architecture diagram


                       ┌──────────────────┐
                       │     INTERNET     │
                       └────────┬─────────┘
                                │
                                ▼
                    ┌──────────────────────┐
                    │   AWS LOAD BALANCER  │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │      AMAZON EKS      │
                    ample-eks-microservices       │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Ruby Frontend Srvice     │
                    │            │
                    └──────────┬───────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
                 ▼                           ▼
       ┌──────────────────┐       ┌──────────────────┐
       │ NODEJS Backend   │       │ Crystal Backend │
       └────────┬─────────┘       └────────┬─────────┘
                │                          │
          ┌─────┼─────┐              ┌─────┼─────┐
          ▼     ▼     ▼              ▼     ▼     ▼
         POD   POD   POD            POD   POD   POD
