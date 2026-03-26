# Crossplane Multi-Cloud Infrastructure

This repository provisions cloud resources on AWS and Azure using [Crossplane](https://crossplane.io/), a Kubernetes-native control plane for infrastructure management.

## Prerequisites
```
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane crossplane-stable/crossplane \
 --namespace crossplane-system \
 --create-namespace
```

You will need to map these values like so for the following configuration:
- appID is the clientId
- password is the clientSecret
- tenant is the tenantId
```
az ad sp create-for-rbac \
 --name "crossplane-provider" \
 --role Contributor \
 --scopes /subscriptions/Your_Subscription_ID
```

## Repository Structure

| File | Description |
|------|-------------|
| `providers.yaml` | Installs the AWS S3 and Azure Storage Crossplane providers |
| `aws_provider.yaml` | AWS credentials secret and `ProviderConfig` |
| `az_provider.yaml` | Azure credentials secret and `ProviderConfig` |
| `resources.yaml` | Cloud resources to provision (S3 bucket, Azure Resource Group) |

## Usage

### 1. Install Providers

Apply the provider packages to your cluster:

```bash
kubectl apply -f providers.yaml
```

This installs:
- `upbound/provider-aws-s3:v1.16.0`
- `upbound/provider-azure-storage:v1.9.0`

Wait for providers to become healthy before proceeding:

```bash
kubectl get providers
```

### 2. Configure AWS Credentials

Edit `aws_provider.yaml` and replace the placeholder values with your AWS credentials, then apply:

```bash
kubectl apply -f aws_provider.yaml
```

Required values:
- `aws_access_key_id`
- `aws_secret_access_key`
- `aws_session_token` (optional)

### 3. Configure Azure Credentials

Edit `az_provider.yaml` and populate the JSON credentials block with your service principal values, then apply:

```bash
kubectl apply -f az_provider.yaml
```

Required values:
- `clientId`
- `clientSecret`
- `tenantId`
- `subscriptionId`

### 4. Provision Resources

```bash
kubectl apply -f resources.yaml
```

This creates:
- **AWS S3 Bucket** — `my-lens-crossplane-bucket` in `us-east-1`
- **Azure Resource Group** — `my-crossplane-rg` in `East US`

Monitor provisioning status:

```bash
kubectl get buckets
kubectl get resourcegroups
```
