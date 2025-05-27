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

#### Install kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -sL https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client  # Verify installation
```