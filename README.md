# GKE Infrastructure with Terraform

This repository contains **Terraform configurations** to provision and manage a **Google Kubernetes Engine (GKE)** cluster along with its supporting infrastructure on Google Cloud Platform (GCP).

---

## 📌 Overview

The setup includes:

- **VPC and Subnet** configuration  
- **Router and NAT gateway** for internet access  
- **Firewall rules** for secure traffic control  
- **GKE cluster** provisioning  
- **Node pools** for worker nodes  
- Modularized Terraform code with variables for easy customization  

---

## 📂 Repository Structure

```
terraform/
├── 2-vpc.tf              # VPC network configuration
├── 3-subnet.tf           # Subnet definitions
├── 4-router.tf           # Cloud Router setup
├── 5-nat.tf              # Cloud NAT for outbound internet
├── 6-firewall.tf         # Firewall rules
├── 7-gke.tf              # GKE cluster definition
├── 8-nodepool.tf         # Node pool configuration
├── provider.tf           # Provider setup (Google)
├── variables.tf          # Input variables
├── terraform.tfvars.example # Example variable values
└── outputs.tf (optional) # Outputs from the deployment
```

---

## 🚀 Getting Started

### 1. Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- [Google Cloud SDK](https://cloud.google.com/sdk/docs/install) (`gcloud`) installed and authenticated  
- A **GCP project** with billing enabled  
- A **service account** with permissions for GKE, VPC, and IAM  

Export your service account key:

```bash
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account.json"
```

---

### 2. Initialize Terraform

```bash
cd terraform/
terraform init
```

---

### 3. Configure Variables

Edit `terraform.tfvars` (create from the provided `terraform.tfvars.example`):

```hcl
project_id    = "my-gcp-project"
region        = "us-central1"
zone          = "us-central1-a"

name          = "my-gke-cluster"
repository_id = "my-docker-repo"
```

---

### 4. Plan and Apply

Preview changes:

```bash
terraform plan
```

Apply the configuration:

```bash
terraform apply
```

---

### 5. Access Your Cluster

After successful deployment, configure `kubectl`:

```bash
gcloud container clusters get-credentials gke-cluster-name --region your-region --project your-gcp-project-id
kubectl get nodes
```

---

### 6. Destroy Resources

To remove all created infrastructure:

```bash
terraform destroy
```

---

## ⚙️ Customization

You can adjust:

- **VPC & Subnets** in `2-vpc.tf` and `3-subnet.tf`  
- **Firewall rules** in `6-firewall.tf`  
- **Cluster size, node pools, and machine types** in `7-gke.tf` and `8-nodepool.tf`  

---

## ✅ Best Practices and Plan

- Use **remote backend** (e.g., GCS bucket) for Terraform state  
- Enable **state locking** with Cloud Storage + DynamoDB (if multi-user)  
- Separate **workspaces** for `dev`, `staging`, and `prod`  
- Keep sensitive data (service account keys, secrets) out of Git  
- Refactored into **reusable modules** for better structure and reusability
