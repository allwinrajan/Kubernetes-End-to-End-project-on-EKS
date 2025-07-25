# 2048 App Deployment with ALB Ingress on AWS EKS (Fargate)

This guide walks you through deploying the classic 2048 game on an Amazon EKS cluster using Fargate, with Ingress support through AWS Load Balancer Controller (ALB).

---

## ✅ Prerequisites

Before proceeding, ensure the following:

- You have an existing **EKS cluster** named `demo-cluster`.
- The **AWS Load Balancer Controller** is installed and configured.
- `kubectl` is configured to access your EKS cluster.
- `eksctl` is installed and accessible in your terminal.
- Region: `us-east-1`

---

## 🚀 Step 1: Create a Fargate Profile

Run the following command to create a Fargate profile specifically for the 2048 application:

```bash
eksctl create fargateprofile \
    --cluster demo-cluster \
    --region us-east-1 \
    --name alb-sample-app \
    --namespace game-2048
```

This profile ensures that all pods deployed in the `game-2048` namespace run on AWS Fargate.

---

## 🚀 Step 2: Deploy the 2048 App with Ingress

Apply the Kubernetes manifests that define the 2048 Deployment, Service, and Ingress using the command below:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml
```

This manifest includes:
- **Deployment**: Deploys the 2048 game container.
- **Service**: Exposes the app internally within the cluster.
- **Ingress**: Configures an ALB to route traffic to the Service.

---

## ✅ Verification

To verify the deployment:
1. Check pod status:
   ```bash
   kubectl get pods -n game-2048
   ```
2. Check Ingress:
   ```bash
   kubectl get ingress -n game-2048
   ```

You should see an ALB address in the `ADDRESS` field of the Ingress. Visit it in a browser to access the 2048 game.

---

## 🧠 Notes

- This setup uses **AWS Fargate**, which abstracts the need to manage worker nodes.
- Ensure your Ingress annotations match the requirements of the AWS Load Balancer Controller.
- ALB resources are automatically provisioned by the controller based on Ingress definitions.

---

## 📁 Reference

- [AWS Load Balancer Controller GitHub](https://github.com/kubernetes-sigs/aws-load-balancer-controller)
- [2048 App Example](https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml)

