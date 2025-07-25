# 2048 App - Deploy with ALB Ingress on AWS EKS

This guide walks you through deploying the 2048 sample application on Amazon EKS using Fargate and configuring ALB (AWS Application Load Balancer) Ingress with the AWS Load Balancer Controller.

---

## Step 1: Create a Fargate Profile

Ensure your EKS cluster is already created before proceeding.

```bash
eksctl create fargateprofile \
    --cluster demo-cluster \
    --region us-east-1 \
    --name alb-sample-app \
    --namespace game-2048
```

---

## Step 2: Deploy the 2048 App, Service, and Ingress

Apply the deployment, service, and ingress configuration using kubectl:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml
```

---

## Step 3: Setup AWS Load Balancer (ALB) Controller Add-on

### 3.1 Download IAM Policy

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
```

### 3.2 Create IAM Policy

```bash
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
```

### 3.3 Create IAM Role for ALB Controller

Replace `<your-cluster-name>` and `<your-aws-account-id>` accordingly:

```bash
eksctl create iamserviceaccount \
  --cluster=<your-cluster-name> \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<your-aws-account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
```

---

## Step 4: Deploy AWS Load Balancer Controller Using Helm

### 4.1 Add Helm Repository

```bash
helm repo add eks https://aws.github.io/eks-charts
```

### 4.2 Update Helm Repositories

```bash
helm repo update eks
```

### 4.3 Install the Controller

Replace the placeholders: `<your-cluster-name>`, `<your-region>`, and `<your-vpc-id>`.

```bash
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system \
  --set clusterName=<your-cluster-name> \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=<your-region> \
  --set vpcId=<your-vpc-id>
```

---

## Step 5: Verify the Controller is Running

```bash
kubectl get deployment -n kube-system aws-load-balancer-controller
```

---

## Notes

- Make sure your EKS version supports the ALB controller version you are installing.
- Ensure IAM roles and Kubernetes service accounts are properly linked.
- The Ingress resource in `2048_full.yaml` will automatically provision an ALB with DNS when everything is configured correctly.

---

## References

- [AWS Load Balancer Controller Documentation](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)
- [2048 Game Deployment Example](https://github.com/kubernetes-sigs/aws-load-balancer-controller/tree/main/docs/examples/2048)

