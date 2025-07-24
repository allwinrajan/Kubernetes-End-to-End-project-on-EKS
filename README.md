# AWS EKS (Amazon Elastic Kubernetes Service) Deployment Guide

This repository serves as a comprehensive and structured guide for setting up and managing Kubernetes clusters on AWS using Amazon EKS. It covers everything from foundational concepts to production-ready deployment strategies, with clear breakdowns and step-by-step instructions.

---

## Table of Contents

1. [Introduction](#introduction)  
2. [Understanding Kubernetes Fundamentals](#understanding-kubernetes-fundamentals)  
   - [1.1 EKS vs. Self-Managed Kubernetes: Pros and Cons](#11-eks-vs-self-managed-kubernetes-pros-and-cons)  
3. [Setting Up Your AWS Environment for EKS](#setting-up-your-aws-environment-for-eks)  
   - [2.1 Creating an AWS Account and IAM Users](#21-creating-an-aws-account-and-iam-users)  
   - [2.2 Configuring the AWS CLI and kubectl](#22-configuring-the-aws-cli-and-kubectl)  
   - [2.3 Preparing Networking and Security Groups for EKS](#23-preparing-networking-and-security-groups-for-eks)  
4. [Launching Your First EKS Cluster](#launching-your-first-eks-cluster)  
5. [Deploying Applications on EKS](#deploying-applications-on-eks)  

---

## Introduction

Amazon Elastic Kubernetes Service (EKS) is a managed Kubernetes service that simplifies the deployment, management, and scalability of containerized applications using Kubernetes on AWS. This guide is designed to assist DevOps engineers, cloud architects, and developers in provisioning robust and secure Kubernetes environments using best practices.

---

## Understanding Kubernetes Fundamentals

### 1.1 EKS vs. Self-Managed Kubernetes: Pros and Cons

#### Amazon EKS (Managed Kubernetes)

**Pros:**

- Managed Control Plane: AWS handles the control plane (API server, etcd, controller manager) with built-in high availability and resilience.
- Automatic Upgrades and Patching: Kubernetes version updates are handled by AWS with minimal manual intervention.
- Scalability: Control plane and worker nodes scale independently, supporting dynamic workloads.
- AWS Integration: Seamless integration with IAM, VPC, CloudWatch, Load Balancers, and ECR.
- Security and Compliance: Meets various compliance standards (e.g., HIPAA, SOC, PCI) with fine-grained IAM control.
- Monitoring and Logging: Built-in integrations with AWS CloudWatch for observability and diagnostics.
- Ecosystem Support: Access to AWS support and continuous improvements from the Kubernetes community.

**Cons:**

- Cost: EKS incurs additional charges for the managed control plane and may be more expensive than self-managed alternatives at scale.
- Less Flexibility: Certain cluster-level configurations are abstracted and not exposed to the user.

#### Self-Managed Kubernetes on EC2

**Pros:**

- Lower Cost Potential: Full control over infrastructure enables use of spot/reserved instances to reduce cost.
- Customizability: Freedom to configure Kubernetes components for specific workloads.
- Early Feature Access: Ability to install cutting-edge Kubernetes releases before they are available on EKS.
- EKS-Compatible: Can still leverage AWS services (e.g., VPC, EBS, ELB).

**Cons:**

- Operational Overhead: Full responsibility for cluster provisioning, updates, and failure recovery.
- Complex Scaling: Manual configuration of control plane and node autoscaling.
- Security Burden: Requires careful implementation of best practices for RBAC, authentication, network policies, and more.
- Lack of Automation: Absence of built-in automation and support found in EKS.

---

## Setting Up Your AWS Environment for EKS

### 2.1 Creating an AWS Account and IAM Users

1. **Create an AWS Account**  
   - Visit https://aws.amazon.com and register a new account.  
   - Provide account details and billing information.

2. **Access the AWS Management Console**  
   - Log in using your root user credentials.

3. **Enable Multi-Factor Authentication (MFA)**  
   - Strongly recommended for root and IAM users.

4. **Create IAM Users and Groups**  
   - Navigate to the IAM console.  
   - Create users with programmatic and/or console access.  
   - Assign permissions via groups or custom policies.

5. **Generate and Store Access Keys**  
   - Securely store Access Key ID and Secret Access Key for programmatic usage.

---

### 2.2 Configuring the AWS CLI and kubectl

**AWS CLI Setup:**

- Install AWS CLI:  
  [Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

- Configure with:  
  ```bash
  aws configure
  ```
  Set access key, region, and default output format.

**kubectl Setup:**

- Install `kubectl`:  
  [Install Guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

- Update kubeconfig:  
  ```bash
  aws eks update-kubeconfig --name <cluster-name> --region <region>
  ```

- Verify cluster access:  
  ```bash
  kubectl get nodes
  ```

---

### 2.3 Preparing Networking and Security Groups for EKS

**1. VPC Setup:**

- Create a custom VPC with:
  - Public and private subnets across multiple AZs.
  - DNS resolution and hostname support enabled.

**2. Security Groups:**

- Create Security Groups for:
  - EKS control plane access
  - Worker node communication
  - Load balancer access

- Define appropriate **inbound rules** (e.g., port 443 for API access, port 22 for SSH)  
- Define **outbound rules** to allow internet access if needed

**3. Internet Gateway (IGW):**

- Create and attach an Internet Gateway to the VPC.  
- Add `0.0.0.0/0` route to the public subnet’s route table pointing to IGW.

**4. IAM Policies and Roles:**

- Create custom IAM policy (or use AWS-managed):
  - `AmazonEKSWorkerNodePolicy`
  - `AmazonEC2ContainerRegistryReadOnly`
  - `AmazonEKS_CNI_Policy`

- Attach policies to the IAM role used by EKS node group.

---

## Launching Your First EKS Cluster

(To be completed)

Includes:

- Console-based EKS cluster creation  
- CLI and IaC (Terraform/CloudFormation) alternatives  
- Bootstrapping worker nodes  
- Cluster authentication and context setup  

---

## Deploying Applications on EKS

(To be completed)

Includes:

- Dockerizing and pushing applications to Amazon ECR  
- Writing Kubernetes manifests (Deployment, Service, ConfigMap, etc.)  
- Helm chart usage  
- Blue/Green and Rolling updates  
- Cluster monitoring and logging integration  

---

## Contributing

Contributions are welcome. Please fork the repository, make changes, and open a pull request. For significant changes, open an issue first to discuss.

---

## License

This project is licensed under the MIT License.

---

## Acknowledgments

This documentation is informed by AWS official guides, CNCF Kubernetes documentation, and enterprise-grade deployment practices.

