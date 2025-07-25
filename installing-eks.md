# Install EKS on Fargate

> ⚠️ **Note:** Please ensure all [prerequisites](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-prereqs.html) are met before proceeding.

## 🚀 Create an EKS Cluster with Fargate

This command creates an EKS cluster named `demo-cluster` in the `us-east-1` region using AWS Fargate.

```bash
eksctl create cluster --name demo-cluster --region us-east-1 --fargate
```

- Automatically provisions the control plane and configures Fargate as the default compute environment.
- No EC2 nodes are provisioned.
- Ideal for serverless workloads and quick testing environments.

## 🧹 Delete the EKS Cluster

Use the following command to delete the EKS cluster:

```bash
eksctl delete cluster --name demo-cluster --region us-east-1
```

- Deletes all resources associated with the cluster.
- Clean-up may take a few minutes depending on provisioned services.

> ✅ Recommended to verify resources using the AWS Console or CLI after deletion.
