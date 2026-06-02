
# Terraform Learning Notes

## Overview

This repository contains Terraform concepts and hands-on examples covering:

* Terraform Variables
* Dynamic Variables
* Terraform Modules
* Built-in Functions
* Conditional Expressions
* Azure Authentication
* Azure Resource Deployment
* Best Practices

---

# 1. Terraform Variables

## Why Variables?

Hardcoding values inside `main.tf` creates several problems:

* Not reusable
* Difficult to maintain
* Prone to human errors
* Difficult to scale across environments

### Example (Bad Practice)

```hcl
size           = "Standard_B1s"
admin_username = "azureuser"
admin_password = "Password@123"
```

---

## Solution: Variables

Create a separate file:

```text
variables.tf
```

Example:

```hcl
variable "vm_size" {
  description = "Azure VM Size"
  type        = string
  default     = "Standard_B1s"
}
```

Use in `main.tf`:

```hcl
size = var.vm_size
```

---

## Variable Types

### Input Variables

Used to pass values into Terraform.

Example:

```hcl
variable "location" {
  type = string
}
```

### Output Variables

Used to retrieve values after deployment.

Example:

```hcl
output "vm_private_ip" {
  value = azurerm_linux_virtual_machine.example.private_ip_address
}
```

---

## Project Structure

```text
terraform-project/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
└── terraform.tfvars
```

---

# 2. Dynamic Variables

## Problem

Creating separate copies of Terraform code for Dev, Test, and Prod is not recommended.

Bad Example:

```text
dev-vm/
prod-vm/
test-vm/
```

This creates duplicate code.

---

## Terraform tfvars Files

Development:

```text
dev.tfvars
```

Production:

```text
prod.tfvars
```

Testing:

```text
test.tfvars
```

Example:

```hcl
location = "West Europe"
vm_size  = "Standard_B1s"
```

---

## Apply Environment Configuration

Development

```bash
terraform apply -var-file=env/dev.tfvars
```

Production

```bash
terraform apply -var-file=env/prod.tfvars
```

Testing

```bash
terraform apply -var-file=env/test.tfvars
```

---

## Recommended Folder Structure

```text
terraform-project/
│
├── main.tf
├── variables.tf
├── outputs.tf
│
└── env/
    ├── dev.tfvars
    ├── test.tfvars
    └── prod.tfvars
```

---

# 3. Terraform Modules

## What Problem Do Modules Solve?

Without modules:

* Teams copy Terraform code
* Code duplication
* Maintenance becomes difficult
* Inconsistent security configurations

---

## What Is a Module?

A Terraform Module is simply a directory containing Terraform files.

Example:

```text
modules/
│
├── vnet/
├── vm/
└── storage/
```

---

## Benefits

### Reduce Code Duplication

Write once.

Reuse everywhere.

---

### Easier Maintenance

Update module once.

All teams benefit.

---

### Improved Security

Security policies can be enforced centrally.

Example:

* Disk encryption
* Naming standards
* Network rules

---

### Better Scalability

Infrastructure becomes:

* Organized
* Reusable
* Easy to manage

---

# 4. Terraform Built-In Functions

Terraform provides built-in functions for:

* String operations
* Numeric calculations
* Collection handling
* Date and time operations
* Encoding functions
* File system operations

---

## String Functions

### lower()

```hcl
lower("Terraform")
```

Output:

```text
terraform
```

---

### upper()

```hcl
upper("terraform")
```

Output:

```text
TERRAFORM
```

---

### format()

```hcl
format("%s-rg", lower(var.project_name))
```

Output:

```text
terraformlab-rg
```

---

### join()

```hcl
join("-", ["terraformlab", "subnet"])
```

Output:

```text
terraformlab-subnet
```

---

## Numeric Functions

### max()

```hcl
max(10,20,30)
```

Output:

```text
30
```

---

### min()

```hcl
min(10,20,30)
```

Output:

```text
10
```

---

## Terraform Console

Practice functions:

```bash
terraform console
```

Examples:

```hcl
upper("terraform")
lower("TERRAFORM")
replace("Terraform","Terra","Cloud")
```

---

# 5. Conditional Expressions

## What Is a Conditional Expression?

Also called:

* Ternary Operator
* Inline If-Else

Syntax:

```hcl
condition ? true_value : false_value
```

---

## Example

```hcl
age >= 18 ? "Adult" : "Minor"
```

---

## Terraform Example

Create Public IP only for Dev Environment:

```hcl
count = var.environment == "prod" ? 0 : 1
```

Explanation:

* Prod → No Public IP
* Dev → Public IP Created

---

## VM Size Based on Environment

```hcl
size = var.environment == "prod" ?
"Standard_D4s_v3" :
"Standard_B1s"
```

Result:

* Dev → Smaller VM
* Prod → Larger VM

---

# 6. Azure Authentication for Terraform

## Why Authentication?

Terraform requires Azure permissions to:

* Create resources
* Update resources
* Delete resources

---

## Authentication Methods

### Azure CLI

```bash
az login
```

Suitable for:

* Learning
* Testing

Not recommended for production.

---

### Service Principal (Recommended)

Used by:

* Terraform
* GitHub Actions
* Azure DevOps
* Jenkins

---

## Create Service Principal

```bash
az ad sp create-for-rbac \
--name terraform-sp \
--role Contributor \
--scopes /subscriptions/<subscription-id>
```

Output:

```json
{
  "appId": "...",
  "password": "...",
  "tenant": "..."
}
```

---

## Configure Environment Variables

Linux/macOS

```bash
export ARM_CLIENT_ID=""
export ARM_CLIENT_SECRET=""
export ARM_SUBSCRIPTION_ID=""
export ARM_TENANT_ID=""
```

Windows PowerShell

```powershell
$env:ARM_CLIENT_ID=""
$env:ARM_CLIENT_SECRET=""
$env:ARM_SUBSCRIPTION_ID=""
$env:ARM_TENANT_ID=""
```

---

# 7. Terraform Azure Provider

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.0"
    }
  }
}

provider "azurerm" {
  features {}
}
```

---

# 8. Terraform Workflow

## Initialize

```bash
terraform init
```

Purpose:

* Download providers
* Initialize project

---

## Plan

```bash
terraform plan
```

Purpose:

* Review changes
* Preview resources

---

## Apply

```bash
terraform apply
```

Purpose:

* Deploy infrastructure

---

## Destroy

```bash
terraform destroy
```

Purpose:

* Remove infrastructure

---

# 9. Azure Resource Group

```hcl
resource "azurerm_resource_group" "example" {
  name     = "terraform-rg"
  location = "East US"
}
```

Purpose:

Logical container for Azure resources.

---

# 10. Azure Storage Account

```hcl
resource "azurerm_storage_account" "example" {
  name                     = "terraformstorage001"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

Used for:

* Blobs
* Files
* Queues
* Tables

---

# Best Practices

### Variables

* Keep all inputs in `variables.tf`
* Use descriptive names
* Avoid hardcoded values

### Security

* Never store passwords in code
* Use Azure Key Vault
* Use Service Principals

### Environment Management

Use:

```text
env/dev.tfvars
env/test.tfvars
env/prod.tfvars
```

### Modules

Create reusable modules for:

* VNet
* VM
* Storage
* Databases

### Naming Standards

Use meaningful names:

```text
terraformlab-rg
terraformlab-vnet
terraformlab-vm
terraformlab-subnet
```

---

# Conclusion

Terraform enables Infrastructure as Code (IaC) by providing:

* Reusable Infrastructure
* Environment Management
* Modular Design
* Secure Deployments
* Automated Provisioning
* Scalable Cloud Infrastructure

Using Variables, Modules, Built-in Functions, and Conditional Expressions helps create maintainable, reusable, and production-ready Terraform projects.

