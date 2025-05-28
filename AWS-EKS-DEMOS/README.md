# Amazon EKS (Elastic Kubernetes Service)

Amazon Elastic Kubernetes Service (EKS) is a managed Kubernetes service that allows you to run Kubernetes without installing and operating your own control plane or nodes. EKS integrates with AWS networking and security services, making it easier to build secure and scalable applications using Kubernetes.

## Features of AWS EKS
- **Fully managed control plane**: EKS automatically manages availability and scalability of the Kubernetes control plane.
- **AWS Fargate support**: Enables serverless compute for containers.
- **IAM integration**: Secure access controls using AWS Identity and Access Management (IAM).
- **Built-in observability**: Supports logging and monitoring using AWS CloudWatch and AWS X-Ray.
- **VPC networking**: Ensures high-performance networking with AWS Virtual Private Cloud (VPC).
- **High availability**: EKS is deployed across multiple availability zones.

## Installing `eksctl` on Linux (Ubuntu)
`eksctl` is a command-line tool that simplifies the management of EKS clusters.

### Prerequisites
Before installing `eksctl`, ensure the following tools are installed:
1. **AWS CLI** (Command Line Interface)
2. **kubectl** (Kubernetes command-line tool)

### Steps to Install AWS CLI and kubectl on Linux (Ubuntu)

#### Install AWS CLI
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version  # Verify installation

```
OR 
Run the following snap install command for the AWS CLI.
```bash

$ snap install aws-cli --classic

$ aws --version

```
#### Install kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -sL https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client  # Verify installation
```

#### Install eksctl

```bash
curl -sL "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar xz
sudo mv eksctl /usr/local/bin/
eksctl version  # Verify installation

```
### Next Steps
#### Configure AWS CLI using

```bash
$ aws configure
```
And follow the instructions

# 📘 eksctl Command Reference

This document provides a list of the most commonly used `eksctl` commands for managing Amazon EKS (Elastic Kubernetes Service) clusters.

---

## 🛠️ Cluster Management

### ✅ Create a Cluster
```bash
eksctl create cluster --name <cluster-name> --region <region> --nodes <count>
```
Creates an EKS cluster with the specified name, region, and number of worker nodes.

### ❌ Delete a Cluster
```bash
eksctl delete cluster --name <cluster-name> --region <region>
```
Deletes the specified EKS cluster and all associated resources.

---

## 👤 Node Group Management

### ➕ Create a Node Group
```bash
eksctl create nodegroup --cluster <cluster-name> --name <nodegroup-name> --region <region> --nodes <count>
```
Adds a managed node group to an existing cluster.

### ➖ Delete a Node Group
```bash
eksctl delete nodegroup --cluster <cluster-name> --name <nodegroup-name> --region <region>
```
Deletes a specific node group from a cluster.

---

## 🔐 IAM & Access

### 🔗 Create IAM Identity Mapping
```bash
eksctl create iamidentitymapping --cluster <cluster-name> --arn <IAM-ARN> --username <username> --group <group>
```
Grants an IAM user or role access to the EKS cluster by mapping to a Kubernetes user/group.

---

## 📦 Add-ons & Resources

### 📋 List Add-ons
```bash
eksctl get addons --cluster <cluster-name> --region <region>
```
Lists available or installed EKS add-ons (like VPC CNI, CoreDNS, kube-proxy).

### 🧩 Install an Add-on
```bash
eksctl create addon --name <addon-name> --cluster <cluster-name> --region <region>
```
Installs an EKS managed add-on.

### 🗑️ Delete an Add-on
```bash
eksctl delete addon --name <addon-name> --cluster <cluster-name> --region <region>
```
Removes an EKS managed add-on from the cluster.

---

## 🧪 For Cluster Info & Troubleshooting

### 🔍 Get Cluster Info
```bash
eksctl get cluster --region <region>
```
Displays basic information about existing clusters in a region.

### 📄 To Get Nodegroups
```bash
eksctl get nodegroup --cluster <cluster-name> --region <region>
```
Lists all node groups in the specified cluster.

---

## ⚙️ Advanced Usage

### 📝 Generate kubeconfig
```bash
eksctl utils write-kubeconfig --cluster <cluster-name> --region <region>
```
Writes or updates your kubeconfig to use the specified EKS cluster.

### 📁 Create Cluster from Config File
```bash
eksctl create cluster -f cluster-config.yaml
```
Creates a cluster based on a declarative YAML configuration file.

### 🧹 To Delete Cluster from Config File
```bash
eksctl delete cluster -f cluster-config.yaml
```
Deletes a cluster defined in a YAML configuration file.

---

## 🧰 Helpful Flags

- `--verbose 4` — Enables verbose logging for debugging.  
- `--dry-run` — Simulates command without executing any changes.

---

## 📚 Resources

- [eksctl Official Documentation](https://eksctl.io/)
- [Amazon EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)

---