---
title: Managed
---

# Managed

- CaaS - Container as a Service
- Serverless
  - K8s + Knative

| vendor        | product   | serverless            |
| ------------- | --------- | --------------------- |
| Alibaba Cloud | ACK       | ASK / ACK Serverless  |
| Amazon        | EKS       | EKS Fargate           |
| Google        | GKE       | GKE Autopilot         |
| Microsoft     | AKS       | AKS Virtual Nodes     |
| Oracle        | OKE       | OKE Virtual Nodes     |
| IBM           | IKS       | IBM Cloud Code Engine |
| Red Hat       | OpenShift | OpenShift Serverless  |
| VMware        | Tanzu     | Cloud Native Runtimes |
| Tencent       | TKE       | TKE Serverless        |

## EKS Fargate

| vCPU | Memory                                   |
| :--- | :--------------------------------------- |
| 0.25 | 0.5 GB, 1 GB, 2 GB                       |
| 0.5  | 1 GB, 2 GB, 3 GB, 4 GB                   |
| 1    | 2 GB, 3 GB, 4 GB, 5 GB, 6 GB, 7 GB, 8 GB |
| 2    | 4 GB - 16 GB (1 GB increments)           |
| 4    | 8 GB - 30 GB (1 GB increments)           |
| 8    | 16 GB - 60 GB (4 GB increments)          |
| 16   | 32 GB - 120 GB (8 GB increments)         |

- https://docs.aws.amazon.com/eks/latest/userguide/fargate-pod-configuration.html
