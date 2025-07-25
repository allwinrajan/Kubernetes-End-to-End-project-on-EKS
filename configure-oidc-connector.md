# Configure IAM OIDC Provider for EKS

This guide sets up IAM OIDC provider for an Amazon EKS cluster using AWS CLI and `eksctl`.

## 1. Set the Cluster Name

```bash
export cluster_name=demo-cluster
```

## 2. Get OIDC ID

```bash
oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)
```

## 3. Check if an IAM OIDC Provider Already Exists

```bash
aws iam list-open-id-connect-providers | grep $oidc_id | cut -d "/" -f4
```

If the provider does not exist (i.e., no output from the command above), proceed to the next step.

## 4. Associate OIDC Provider with the EKS Cluster

```bash
eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve
```

## ✅ Notes

- This is required before you can create IAM roles for service accounts (IRSA) in EKS.
- Run this **only once** per cluster if not already associated.
