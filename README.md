# ☁️ Azure Infrastructure with Terraform

> **Infrastructure as Code (IaC) project using Terraform to provision an Azure Resource Group.**

This repository is a practical Terraform lab built to understand the complete flow of creating Azure infrastructure as code — from writing the Terraform configuration to validating, planning, applying, verifying, and finally destroying the resource.

---

## 🎯 What This Project Does

This project creates **one Azure Resource Group**.

The Resource Group name and Azure region are supplied through Terraform variables.

```text
terraform.tfvars
      │
      ▼
 variables.tf
      │
      ▼
    main.tf
      │
      ▼
 AzureRM Provider
      │
      ▼
 Microsoft Azure
      │
      ▼
 Azure Resource Group
```

### Currently created

```text
Azure
└── Resource Group
    ├── Name     → value from resource_group_name
    └── Location → value from location
```

> This project currently does **not** create a VNet, VM, Storage Account, NSG, or other Azure services.

---

# 🧠 Terraform Flow

The complete flow used in this project is:

```text
1. Write Terraform Configuration
              ↓
2. Configure Variables
              ↓
3. Authenticate with Azure
              ↓
4. terraform init
              ↓
5. terraform fmt
              ↓
6. terraform validate
              ↓
7. terraform plan
              ↓
8. terraform apply
              ↓
9. Verify Resource in Azure
              ↓
10. terraform output
              ↓
11. terraform destroy (when required)
```

---

# 🏗️ Project Architecture

```text
                         Developer
                             │
                             ▼
                    GitHub Repository
                             │
                             ▼
                  Terraform Configuration
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
          ▼                  ▼                  ▼
     provider.tf       variables.tf         main.tf
                              ▲                  │
                              │                  │
                       terraform.tfvars         │
                              │                  │
                              └────────┬─────────┘
                                       ▼
                                Terraform CLI
                                       │
                         ┌─────────────┴─────────────┐
                         ▼                           ▼
                    terraform plan            terraform apply
                                                     │
                                                     ▼
                                               Microsoft Azure
                                                     │
                                                     ▼
                                            Azure Resource Group
```

---

# 📂 Project Structure

```text
azure-terraform-infrastructure/
│
├── .github/
│   └── workflows/
│       └── terraform-ci.yml
│
├── .gitignore
├── .terraform.lock.hcl
├── main.tf
├── output.tf
├── provider.tf
├── variables.tf
├── README.md
│
└── terraform.tfvars        # Local only - not committed
```

---

# 🔍 Understanding the Terraform Files

## 1. `provider.tf`

The provider tells Terraform which cloud platform it needs to communicate with.

This project uses the **AzureRM provider**.

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "5.0.0"
    }
  }
}

provider "azurerm" {
  features {}
}
```

### Simple meaning

```text
Terraform
   ↓
AzureRM Provider
   ↓
Azure
```

Without the AzureRM provider, Terraform would not know how to manage Azure resources.

---

## 2. `variables.tf`

This file defines the inputs required by the project.

```hcl
variable "resource_group_name" {
  description = "Name of the Azure Resource Group"
  type        = string
}

variable "location" {
  description = "Azure region where resources will be created"
  type        = string
}
```

Instead of hardcoding values inside `main.tf`, we define reusable variables.

---

## 3. `terraform.tfvars`

This file supplies the actual values for the variables.

Create this file locally:

```hcl
resource_group_name = "rg-demo"
location            = "eastus"
```

You can use your own values, for example:

```hcl
resource_group_name = "rg-myproject"
location            = "centralindia"
```

### Why is this file local?

`terraform.tfvars` is excluded by `.gitignore`.

That keeps environment-specific values out of the public repository.

---

## 4. `main.tf`

This is where the Azure Resource Group is defined.

```hcl
resource "azurerm_resource_group" "main" {
  name     = var.resource_group_name
  location = var.location
}
```

### How Terraform reads this

```text
terraform.tfvars
       │
       ▼
resource_group_name
location
       │
       ▼
variables.tf
       │
       ▼
main.tf
       │
       ▼
azurerm_resource_group
       │
       ▼
Azure Resource Group
```

---

## 5. `output.tf`

This file tells Terraform which information should be displayed after deployment.

```hcl
output "resource_group_name" {
  description = "Name of the Azure Resource Group"
  value       = azurerm_resource_group.main.name
}

output "resource_group_location" {
  description = "Location of the Azure Resource Group"
  value       = azurerm_resource_group.main.location
}
```

After deployment, run:

```bash
terraform output
```

---

## 6. `.terraform.lock.hcl`

Terraform creates this file to lock provider dependency information.

It helps keep provider versions and dependency selections consistent.

---

## 7. `.gitignore`

Terraform creates local files that should not be committed to GitHub, such as:

```text
.terraform/
terraform.tfstate
terraform.tfstate.backup
terraform.tfvars
```

The `.gitignore` file prevents these files from being accidentally committed.

---

# 🧰 Prerequisites

Before starting, install:

| Tool | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| Azure CLI | Azure authentication and management |
| Git | Clone and manage the repository |
| VS Code | Edit Terraform files |
| Azure Subscription | Create Azure resources |

### Verify installation

```bash
terraform version
az version
git --version
```

---

# 🔐 Step 1 — Login to Azure

Authenticate using Azure CLI:

```bash
az login
```

A browser window will open. Sign in with your Azure account.

Verify the active account/subscription:

```bash
az account show
```

If you have multiple subscriptions, select the required one:

```bash
az account set --subscription "<subscription-name-or-id>"
```

You can list subscriptions with:

```bash
az account list
```

### Why?

Terraform needs Azure authentication before it can create the Resource Group.

---

# 📥 Step 2 — Clone the Repository

Clone the project:

```bash
git clone https://github.com/rahilarshi2212/azure-terraform-infrastructure.git
```

Move into the project:

```bash
cd azure-terraform-infrastructure
```

Check the files:

```bash
dir
```

Linux/macOS:

```bash
ls
```

You should see files such as:

```text
main.tf
output.tf
provider.tf
variables.tf
README.md
```

---

# 📝 Step 3 — Create `terraform.tfvars`

The public repository does not contain your local `terraform.tfvars`.

Create:

```text
terraform.tfvars
```

Add:

```hcl
resource_group_name = "rg-demo"
location            = "eastus"
```

Replace the values if required.

Example:

```hcl
resource_group_name = "rg-myproject"
location            = "centralindia"
```

---

# ⚙️ Step 4 — Initialize Terraform

Run:

```bash
terraform init
```

### What does it do?

`terraform init` prepares the Terraform working directory and downloads the required provider.

```text
Terraform Configuration
        ↓
terraform init
        ↓
AzureRM Provider
        ↓
Terraform Working Directory Ready
```

Expected result:

```text
Terraform has been successfully initialized!
```

---

# 🧹 Step 5 — Format the Code

Run:

```bash
terraform fmt
```

### What does it do?

It formats Terraform files using Terraform's standard formatting.

You can check formatting without changing files:

```bash
terraform fmt -check
```

---

# ✅ Step 6 — Validate the Configuration

Run:

```bash
terraform validate
```

### What does it do?

It checks whether the Terraform configuration is syntactically and structurally valid.

Expected result:

```text
Success! The configuration is valid.
```

> `terraform validate` checks the configuration. It does not create Azure resources.

---

# 📋 Step 7 — Create a Terraform Plan

Run:

```bash
terraform plan
```

### What does it do?

Terraform compares the desired configuration with its current state and shows the changes it intends to make.

For a fresh deployment, the plan will typically show:

```text
Plan: 1 to add, 0 to change, 0 to destroy.
```

> The exact plan depends on the current Terraform state.

### Important

`terraform plan` **does not create** the resource.

It only previews the changes.

---

# 🚀 Step 8 — Apply the Terraform Configuration

Run:

```bash
terraform apply
```

Terraform will display the planned changes and ask for confirmation.

Type:

```text
yes
```

Terraform will then create the Azure Resource Group.

---

# ☁️ Step 9 — Verify in Azure

Open the Azure Portal.

Go to:

```text
Azure Portal
   ↓
Resource Groups
```

Find the Resource Group whose name you configured in:

```text
terraform.tfvars
```

For example:

```text
rg-demo
```

Verify its Azure region as well.

---

# 📤 Step 10 — Check Terraform Outputs

Run:

```bash
terraform output
```

You should see values for:

```text
resource_group_name
resource_group_location
```

You can also inspect the complete Terraform state view with:

```bash
terraform show
```

---

# 🤖 GitHub Actions — CI Validation

The repository contains:

```text
.github/workflows/terraform-ci.yml
```

The workflow runs on pushes to `main` and Pull Requests targeting `main`.

Its flow is:

```text
GitHub Push / Pull Request
          ↓
    Checkout Code
          ↓
   Setup Terraform
          ↓
 terraform fmt -check
          ↓
 terraform init -backend=false
          ↓
 terraform validate
```

### What does this achieve?

It automatically checks that the Terraform code:

- follows Terraform formatting
- initializes successfully
- passes Terraform validation

### Important

This workflow is **CI validation only**.

It does **not** run:

```text
terraform plan
terraform apply
```

and therefore it does not deploy the Azure Resource Group.

The Azure deployment in this project is performed through the local Terraform workflow.

---

# 🔄 Complete Hands-on Flow

If you want to reproduce this project from a fresh machine, the practical sequence is:

```bash
# 1. Login
az login

# 2. Clone
git clone https://github.com/rahilarshi2212/azure-terraform-infrastructure.git

# 3. Enter project
cd azure-terraform-infrastructure

# 4. Create terraform.tfvars
# Add resource_group_name and location

# 5. Initialize
terraform init

# 6. Format
terraform fmt

# 7. Validate
terraform validate

# 8. Review changes
terraform plan

# 9. Create Azure resource
terraform apply

# 10. View outputs
terraform output
```

---

# 🧠 Remember the Terraform Commands

| Command | Simple meaning |
|---|---|
| `terraform init` | Prepare the Terraform project |
| `terraform fmt` | Format Terraform code |
| `terraform validate` | Check configuration |
| `terraform plan` | Preview changes |
| `terraform apply` | Create/update infrastructure |
| `terraform output` | Show defined outputs |
| `terraform show` | Inspect Terraform state |
| `terraform destroy` | Remove managed infrastructure |

### Easy way to remember

```text
INIT
 ↓
FORMAT
 ↓
VALIDATE
 ↓
PLAN
 ↓
APPLY
 ↓
OUTPUT
 ↓
DESTROY
```

---

# 🧹 Cleanup

When you no longer need the Resource Group created by this Terraform configuration:

```bash
terraform destroy
```

Review the proposed changes and type:

```text
yes
```

Terraform will remove the infrastructure managed by this configuration.

> ⚠️ Use `terraform destroy` carefully. Never run it against an environment unless you understand which resources are managed by that Terraform state.

---

# 🐛 Troubleshooting

## Terraform initialized in an empty directory

If you see:

```text
Terraform initialized in an empty directory!
```

check that you are inside the project directory:

```bash
dir
```

or:

```bash
ls
```

You should see Terraform files such as:

```text
main.tf
provider.tf
variables.tf
```

Then run:

```bash
terraform init
```

---

## Azure authentication problem

Check your Azure login:

```bash
az account show
```

If required:

```bash
az login
```

Then retry:

```bash
terraform plan
```

---

## Terraform says "No changes"

If Terraform says:

```text
No changes.
Your infrastructure matches the configuration.
```

Terraform believes the current infrastructure already matches the configuration and there is nothing new to create or change.

This can be normal after the resource has already been successfully applied.

---

# 📚 What I Learned

This project helped me practice:

- Infrastructure as Code
- Terraform project structure
- AzureRM provider
- Terraform variables
- `terraform.tfvars`
- Terraform resources
- Terraform outputs
- Terraform state
- Terraform CLI workflow
- Azure CLI authentication
- Git
- GitHub
- GitHub Actions
- CI validation

---

# 🚀 Next Terraform Learning Steps

This project is the foundation for more advanced Azure Terraform work.

The next logical improvements are:

```text
Resource Group
      ↓
VNet
      ↓
Subnet
      ↓
NSG
      ↓
Virtual Machine
      ↓
Multiple Resources
      ↓
for_each / count
      ↓
Maps / Lists
      ↓
Modules
      ↓
Remote Backend
      ↓
CI/CD Deployment
```

These are separate learning steps and are **not currently implemented in this repository**.

---

# 👨‍💻 Author

**Rahil Ansari**

Azure Cloud & DevOps Engineer

**Focus Areas**

`Azure` · `Terraform` · `Azure DevOps` · `GitHub Actions` · `Infrastructure Automation`

🔗 [GitHub](https://github.com/rahilarshi2212)

🔗 [LinkedIn](https://www.linkedin.com/in/ansarirahill)
