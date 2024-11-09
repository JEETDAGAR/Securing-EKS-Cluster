# Secure Amazon EKS Cluster with KMS Encryption and CloudWatch Logging

This repository contains the configuration files and steps to set up a secure Amazon Elastic Kubernetes Service (EKS) cluster using `eksctl`. Key security features include:

- **KMS (Key Management Service) Encryption** for Kubernetes secrets
- **CloudWatch Logging** for detailed observability of cluster activity
- **Private Subnets** for network isolation
- **Multi-OS Node Groups** for both Windows and Linux workloads

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [1. Create a KMS Key](#1-create-a-kms-key)
  - [2. Configure IAM Role for Windows Nodes](#2-configure-iam-role-for-windows-nodes)
  - [3. Create the Cluster Configuration](#3-create-the-cluster-configuration)
  - [4. Deploy the EKS Cluster](#4-deploy-the-eks-cluster)
  - [5. Validate KMS Encryption and CloudWatch Logging](#5-validate-kms-encryption-and-cloudwatch-logging)
  - [6. Deploy a Sample Pod Using the Secret](#6-deploy-a-sample-pod-using-the-secret)
- [Conclusion](#conclusion)
- [License](#license)

## Overview

This guide demonstrates how to create a highly secure EKS cluster using `eksctl`, with a focus on security and observability. The key security aspects include envelope encryption for secrets, network isolation with private subnets, and comprehensive CloudWatch logging for cluster monitoring.

## Prerequisites

- **AWS CLI**: Installed and configured with the necessary permissions
- **eksctl**: Version 1.29 or higher
- **kubectl**: Installed for managing Kubernetes resources

## Getting Started

### 1. Create a KMS Key

To enable encryption for Kubernetes secrets at rest, create a KMS key using the following command:

```bash
aws kms create-key --description "EKS Secrets Encryption Key"
```

Save the KMS key ARN, as it will be used in the `cluster.yaml` configuration file.

### 2. Configure IAM Role for Windows Nodes

Create an IAM role for Windows nodes. This role provides necessary permissions for Windows workloads in EKS.

### 3. Create the Cluster Configuration

Define the EKS cluster configuration in `cluster.yaml` with KMS encryption, CloudWatch logging, private subnets, and multi-OS node groups. An example configuration is provided in this repository.

### 4. Deploy the EKS Cluster

Run the following command to create the EKS cluster:

```bash
eksctl create cluster -f cluster.yaml
```

### 5. Validate KMS Encryption and CloudWatch Logging

- **KMS Encryption**: Create a Kubernetes secret and check AWS CloudTrail for "Encrypt" events to confirm encryption.
- **CloudWatch Logging**: Verify that log groups for API, audit, authenticator, and other cluster logs are created in CloudWatch.

### 6. Deploy a Sample Pod Using the Secret

To test the secret encryption, deploy a sample pod that references the encrypted secret:

```yaml
# pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
      env:
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: username
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: password
```

Apply the pod configuration:

```bash
kubectl apply -f pod.yaml
```

## Conclusion

In this guide, we've set up a secure EKS cluster with:

- **KMS Envelope Encryption** for Kubernetes secrets
- **CloudWatch Logging** for comprehensive observability
- **Private Subnets** for network isolation
- **Multi-OS Node Groups** with specific IAM roles

This configuration provides a production-ready EKS setup that is secure, scalable, and well-monitored.
## **Contributing**

If you find any issues or have suggestions for improvements, feel free to open an issue or submit a pull request. **Contributions are always welcome!**

## **Connect**

If you have any questions or want to discuss about DevOps, Kubernetes, Cloud or anything else, feel free to reach out!

**LinkedIn**: https://www.linkedin.com/in/jeet-dagar-0463621b7/
