# Azure Terraform Infrastructure

> Infrastructure as Code (IaC) project to provision Azure resources using Terraform.

## Overview

This project demonstrates how to use Terraform to provision and manage Microsoft Azure infrastructure using Infrastructure as Code (IaC).

The project uses Terraform variables and a separate `terraform.tfvars` file to keep infrastructure configuration reusable and maintainable.

### What this project demonstrates

- Azure Resource Group provisioning
- Terraform Provider configuration
- Terraform variables
- Environment-specific values using `terraform.tfvars`
- Terraform Plan and Apply workflow
- Git and GitHub version control
- Secure handling of Terraform state and variable files
  ## Architecture

The Terraform workflow used in this project is:

```text
Developer
    |
    v
Terraform Configuration
    |
    +------------------+
    |                  |
    v                  v
variables.tf      terraform.tfvars
    |                  |
    +--------+---------+
             |
             v
           main.tf
             |
             v
      Azure Resource Group
             |
             v
        Microsoft Azure
```

### Workflow

1. `provider.tf` configures the Azure provider.
2. `variables.tf` defines reusable input variables.
3. `terraform.tfvars` provides the actual variable values locally.
4. `main.tf` uses the variables to define the Azure Resource Group.
5. `terraform plan` previews the infrastructure changes.
6. `terraform apply` creates the infrastructure in Azure.

## Technologies Used

| Technology | Purpose |
|------------|---------|
| Terraform | Infrastructure as Code (IaC) |
| Microsoft Azure | Cloud infrastructure platform |
| AzureRM Provider | Terraform-to-Azure integration |
| Git | Version control |
| GitHub | Source code repository |
| PowerShell | Command-line environment |
## Project Structure

```text
azure-terraform-infrastructure/
│
├── .gitignore
├── .terraform.lock.hcl
├── main.tf
├── provider.tf
├── variables.tf
└── terraform.tfvars  # Local only - not committed to GitHub
```

### File Responsibilities

| File | Purpose |
|------|---------|
| `provider.tf` | Configures Terraform and the AzureRM provider |
| `variables.tf` | Defines input variables |
| `terraform.tfvars` | Stores local variable values |
| `main.tf` | Defines Azure resources |
| `.terraform.lock.hcl` | Locks provider dependency versions |
| `.gitignore` | Prevents sensitive/unnecessary files from being committed |

## Prerequisites
Before using this project, make sure the following tools are installed:

- Terraform 
- Azure CLI
- Git
- Visual Studio Code
- An active Microsoft Azure subscription

### Verify Installation

```bash
terraform version
az version
git --version
```

You should also be authenticated with Azure CLI:

```bash
az login
```
## Deployment Steps

### 1. Clone the repository

```bash
git clone https://github.com/rahilarshi2212/azure-terraform-infrastructure.git
cd azure-terraform-infrastructure
```

### 2. Configure Terraform variables

Create a local `terraform.tfvars` file:

```hcl
resource_group_name = "your-resource-group-name"
location            = "your-azure-region"
```

> `terraform.tfvars` is intentionally excluded from Git using `.gitignore`.

### 3. Initialize Terraform

```bash
terraform init
```

### 4. Review the execution plan

```bash
terraform plan
```

### 5. Apply the configuration

```bash
terraform apply
```

Type `yes` when Terraform asks for confirmation.
## Terraform Workflow

Terraform follows a standard workflow to provision infrastructure:

```text
Write Configuration
        |
        v
terraform init
        |
        v
terraform plan
        |
        v
terraform apply
        |
        v
Azure Infrastructure
```

### Terraform Commands

| Command | Purpose |
|---------|---------|
| `terraform init` | Initializes the Terraform working directory and downloads required providers |
| `terraform plan` | Shows the changes Terraform intends to make |
| `terraform apply` | Creates or updates infrastructure |
| `terraform destroy` | Removes infrastructure managed by Terraform |
## Variables

This project uses Terraform input variables to keep the infrastructure configuration reusable.

| Variable | Type | Description | Example |
|----------|------|-------------|---------|
| `resource_group_name` | string | Name of the Azure Resource Group | `rg-example` |
| `location` | string | Azure region for the Resource Group | `eastus` |

The actual values are provided through the local `terraform.tfvars` file, which is excluded from Git using `.gitignore`.
## Resources Created

This project currently provisions the following Azure resource:

| Resource | Terraform Resource Type | Configuration |
|----------|--------------------------|---------------|
| Azure Resource Group | `azurerm_resource_group` | Name and location are provided through variables |

### Current Deployment

- **Resource Group:** `rg-rahil`
- **Azure Region:** `eastus`
- **Terraform Action:** Create

## Cleanup

To remove the Azure infrastructure created by Terraform:

```bash
terraform destroy
```

Review the plan and type `yes` when Terraform asks for confirmation.

> Use `terraform destroy` carefully because it permanently removes resources managed by the Terraform configuration.
## Author


**Rahil Ansari**

Azure Cloud & DevOps Engineer | Terraform | Azure DevOps | GitHub Actions | Infrastructure Automation

- GitHub: [rahilarshi2212](https://github.com/rahilarshi2212)
- LinkedIn: [Rahil Ansari](https://www.linkedin.com/in/ansarirahil)
