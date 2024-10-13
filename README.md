# CI/CD to deploy Go application on EKS

This project uses Terraform, Kubernetes, and Github Actions to deploy a Go application on AWS EKS using CI/CD. Here is how you can set it up:

## Prerequisites

Before you begin, you will need to have the following:

- Docker installed on your machine.
- Terraform installed on your machine.
- kubectl installed on your machine.
- AWS CLI installed and configured on your machine.
- Base64 tool for encoding files.
- An AWS account.
- A GitHub account.

## Directory Structure

- `terraform/`: This directory contains the Terraform configuration files to build EKS and ECR on AWS.
- `k8s/`: This directory contains Kubernetes manifest files to deploy the Go application along with the database.
- `Go-app/`: This directory contains the Go application and the Dockerfile.
- `.github/workflows/`: This directory contains the GitHub Actions CI/CD workflow configurations.

## Setup Steps

### Step 1: AWS Setup

Firstly, make sure you have configured your AWS CLI with the appropriate AWS account credentials.

### Step 2: Terraform Setup

Navigate to the `terraform/` directory and initialize Terraform:

```bash
cd terraform
terraform init
```

Plan the Terraform changes:

```bash
terraform plan
```

If everything looks good, apply the changes:

```bash
terraform apply
```

### Step 3: Kubernetes Setup

After EKS is set up, you should configure kubectl to interact with the EKS cluster. You can get the kubeconfig file by following the instructions provided by AWS.

```
aws eks update-kubeconfig --name <CLUSTER_NAME> --region <REGION> --kubeconfig <FILE_NAME>
```

After you have your kubeconfig file, encode it in base64:

```bash
base64 /path/to/your/kubeconfig > <NEW_FILE_NAME>
```

### Step 4: GitHub Secrets Setup

Navigate to your GitHub repository and go to "Settings" -> "Secrets and variables" -> "Actions ". Here you need to add the following secrets:

- `AWS_ACCESS_KEY_ID`: The AWS access key ID for your account.
- `AWS_SECRET_ACCESS_KEY`: The AWS secret access key for your account.
- `KUBECONFIG`: The base64 encoded kubeconfig file from step 3.

### Step 5: CI/CD Setup

GitHub Actions is used for continuous integration and deployment. The workflows are automatically triggered when a push or pull request is made to the main branch. The `ci.yml` workflow builds the Docker image and pushes it to ECR. The `cd.yml` workflow deploys the application to the EKS cluster using the Kubernetes manifests.

`NOTE:` Make sure to update the `Image` from the `k8s/app.yml` file with the `ECR_URI` from your `AWS`

## Running the Application

After completing the setup, you can make changes to the Go application in the `Go-app/` directory and push your changes. GitHub Actions will automatically build, push, and deploy your changes.

Make sure to check the "Actions" tab in your GitHub repository to see the status of your workflows.

## Cleanup

To clean up your AWS resources, navigate to the `terraform/` directory and destroy the Terraform resources:

```bash
terraform destroy
```
"# action-test" 
