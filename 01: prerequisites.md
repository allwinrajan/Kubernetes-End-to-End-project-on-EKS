# Prerequisites for Amazon EKS Setup

Before creating an Amazon EKS cluster, ensure the following tools are installed and configured on your system:

---

## Tools Required

### 1. `kubectl`
A command-line tool for interacting with Kubernetes clusters.

- [Installing or updating kubectl](https://kubernetes.io/docs/tasks/tools/)

---

### 2. `eksctl`
A simple CLI tool for creating and managing EKS clusters. It automates many tasks such as VPC setup and node provisioning.

- [Installing or updating eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)

---

### 3. AWS CLI
Command-line interface for managing AWS services, including EKS.

- [Installing, updating, and uninstalling the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- After installation, configure the CLI using:
  - [Quick configuration with `aws configure`](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config)

---

> Once all the above tools are installed and configured, you're ready to create your EKS cluster.
