# SRE CI/CD & Kubernetes Auto Scaling Platform

A production-inspired hands-on DevOps/SRE project demonstrating automated CI/CD, containerization, Kubernetes deployment, load balancing, monitoring, and application auto scaling on AWS.

## Project Overview

This project implements an automated application delivery workflow using:

- GitHub
- Jenkins
- Docker
- Amazon ECR
- Amazon EKS
- Kubernetes
- AWS Application Load Balancer
- Horizontal Pod Autoscaler (HPA)
- Kubernetes Metrics Server
- AWS CloudWatch

The objective was to build and validate an end-to-end CI/CD workflow where a code change automatically reaches the Kubernetes environment without manual deployment.

## Architecture

```text
Developer
    |
    v
GitHub Repository
    |
    | Webhook
    v
Jenkins Pipeline
    |
    +--> Verify Source Code
    |
    +--> Build Docker Image
    |
    +--> Login to Amazon ECR
    |
    +--> Tag Docker Image
    |
    +--> Push Image to ECR
    |
    v
Amazon EKS
    |
    v
Kubernetes Deployment
    |
    v
Application Pods
    |
    v
ClusterIP Service
    |
    v
Ingress
    |
    v
AWS Application Load Balancer
    |
    v
Users / Browser

        HPA
         |
         +--> Min Pods: 2
         +--> Max Pods: 5
         +--> CPU Target: 50%
```

## CI/CD Pipeline

A GitHub push automatically triggers Jenkins through a webhook.

The Jenkins pipeline performs:

1. Checkout source code
2. Verify source code
3. Build Docker image
4. Authenticate with Amazon ECR
5. Tag Docker image
6. Push image to Amazon ECR
7. Apply Kubernetes manifests
8. Deploy the current image to Amazon EKS
9. Verify Kubernetes rollout
10. Execute post-deployment actions

Each Jenkins build number is used as a Docker image tag for traceability.

## Kubernetes Resources

The application uses the following Kubernetes manifests:

```text
k8s/
├── deployment.yaml
├── service.yaml
├── ingressclass.yaml
├── ingress.yaml
└── hpa.yaml
```

### Deployment

The application runs with a minimum of two replicas.

### Service

A ClusterIP service provides internal connectivity to the application Pods.

### Ingress

AWS Load Balancer Controller processes the Kubernetes Ingress resource and exposes the application through an internet-facing AWS Application Load Balancer.

### Horizontal Pod Autoscaler

HPA configuration:

```text
Minimum Replicas : 2
Maximum Replicas : 5
CPU Target       : 50%
```

Metrics Server provides CPU utilization metrics to Kubernetes HPA.

## Repository Structure

```text
sre-cicd-autoscaling-project/
│
├── app/
│   └── index.html
│
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingressclass.yaml
│   ├── ingress.yaml
│   └── hpa.yaml
│
├── Dockerfile
├── Jenkinsfile
├── .gitignore
└── README.md
```

## High Availability

The final environment used:

- 2 EKS worker nodes
- 2 application replicas
- Kubernetes rolling deployments
- AWS Application Load Balancer
- HPA scaling from 2 to 5 Pods

This provides redundancy at both the compute and application layers.

## Monitoring & Validation

The environment was validated using:

```bash
kubectl get nodes
kubectl get pods -A
kubectl get deployment
kubectl get service
kubectl get ingress
kubectl get hpa
kubectl top nodes
kubectl top pods
```

Final HPA status:

```text
TARGETS       MINPODS   MAXPODS   REPLICAS
cpu: 2%/50%   2         5         2
```

Final application validation:

```text
HTTP Status: 200
Response Time: 0.008554s
```

## Final CI/CD Validation

For the final test, the application version was changed and pushed to GitHub.

The complete automated flow executed successfully:

```text
Code Change
   ↓
Git Push
   ↓
GitHub Webhook
   ↓
Jenkins Build #12
   ↓
Docker Image Build
   ↓
Amazon ECR
   ↓
Amazon EKS
   ↓
Kubernetes Rolling Update
   ↓
Ingress / AWS ALB
   ↓
Browser
```

Final deployed version:

```text
Application Version: 2.2.0
Application Status: Healthy
```

Jenkins Build #12 completed successfully with all pipeline stages green.

## Troubleshooting Performed

Several practical issues were identified and resolved during implementation:

- EKS worker node connectivity issue
- Kubernetes Pod capacity limitation
- EKS node group scaling
- Jenkins-to-EKS access configuration
- AWS Load Balancer Controller IAM/OIDC integration
- AWS Load Balancer Controller VPC discovery issue
- Missing Kubernetes IngressClass
- ALB DNS propagation/resolution issue
- Kubernetes rollout validation

These troubleshooting scenarios provided practical experience with real-world SRE and Kubernetes operations.

## Key Skills Demonstrated

- CI/CD Automation
- Jenkins Pipeline
- GitHub Webhooks
- Docker
- Amazon ECR
- Amazon EKS
- Kubernetes
- AWS Load Balancer Controller
- Application Load Balancing
- Horizontal Pod Autoscaling
- Kubernetes Metrics
- IAM / OIDC Integration
- Linux / Bash
- Monitoring & Health Validation
- Production Troubleshooting

## Final Result

The complete application delivery workflow was successfully implemented and validated:

```text
GitHub
   → Jenkins
   → Docker
   → Amazon ECR
   → Amazon EKS
   → Kubernetes Deployment
   → Service
   → Ingress
   → AWS ALB
   → Browser
```

**Final Status: Successfully Implemented, Automated, Validated and Cleaned Up.**

> Note: AWS lab resources were deleted after successful validation to avoid unnecessary cloud costs.