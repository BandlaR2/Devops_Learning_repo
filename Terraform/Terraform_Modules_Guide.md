# Terraform Modules Guide

## Overview

Terraform Modules are reusable containers for Terraform configurations. They help reduce code duplication, improve maintainability, enforce security standards, and enable teams to deploy infrastructure consistently.

---

## Why Terraform Modules?

Imagine an organization with multiple projects.

Every project requires:

* Virtual Networks (VNet)
* Virtual Machines (VM)
* Storage Accounts

Without modules:

* Teams copy Terraform code.
* Infrastructure becomes inconsistent.
* Security updates require manual changes in every project.
* Maintenance becomes difficult.

Terraform Modules solve this problem by allowing teams to reuse infrastructure code.

---

## What is a Terraform Module?

A Terraform Module is simply a directory containing Terraform configuration files.

Example:

```text
terraform-module/
│
├── main.tf
├── variables.tf
└── outputs.tf
```

A module can be reused across multiple projects.

---

## Module Directory Structure

```text
terraform-module/
│
├── main.tf
│
└── vnet-module/
    ├── main.tf
    ├── variables.tf
    └── outputs.tf
```

---

## Step 1: Create Module Directory

```bash
mkdir terraform-module
cd terraform-module

mkdir vnet-module
cd vnet-module

touch main.tf variables.tf outputs.tf
```

---

## Step 2: Create VNet Module

### main.tf

```hcl
resource "azurerm_virtual_network" "vnet" {
  name                = var.vnet_name
  location            = var.location
  resource_group_name = var.resource_group_name
  address_space       = var.address_space
}
```

---

### variables.tf

```hcl
variable "vnet_name" {
  type = string
}

variable "location" {
  type = string
}

variable "resource_group_name" {
  type = string
}

variable "address_space" {
  type = list(string)
}
```

---

### outputs.tf

```hcl
output "vnet_id" {
  value = azurerm_virtual_network.vnet.id
}

output "vnet_name" {
  value = azurerm_virtual_network.vnet.name
}
```

---

## Step 3: Call Module from Root Project

Create a root-level `main.tf`.

```hcl
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
    }
  }
}

provider "azurerm" {
  features {}
}

module "network" {
  source              = "./vnet-module"

  vnet_name           = "my-vnet"
  location            = "East US"
  resource_group_name = "terraform-rg"
  address_space       = ["10.0.0.0/16"]
}
```

---

## How Modules Work

Root Module:

```text
main.tf
```

calls

```text
vnet-module/
```

Terraform reads:

```text
vnet-module/main.tf
vnet-module/variables.tf
vnet-module/outputs.tf
```

and creates the infrastructure.

---

## Local Modules

Module stored inside project:

```hcl
source = "./vnet-module"
```

Benefits:

* Easy development
* Fast testing
* Good for learning

---

## Remote Modules

Modules can also be stored remotely.

Examples:

* GitHub
* Azure Blob Storage
* AWS S3
* Terraform Registry

Example:

```hcl
source = "git::https://github.com/company/vnet-module.git"
```

Benefits:

* Centralized management
* Version control
* Reusable across teams

---

## Terraform Workflow

Initialize Terraform:

```bash
terraform init
```

Review Changes:

```bash
terraform plan
```

Deploy Infrastructure:

```bash
terraform apply
```

Destroy Infrastructure:

```bash
terraform destroy
```

---

## Benefits of Terraform Modules

### Reduce Code Duplication

Write once and reuse everywhere.

### Easy Maintenance

Update module once.

All projects automatically benefit.

### Security Compliance

Enforce:

* Disk encryption
* Naming standards
* Network policies

### Scalability

Large infrastructures become:

* Organized
* Reusable
* Easy to manage

### Team Collaboration

Different teams can maintain:

* Networking Module
* VM Module
* Storage Module

independently.

---

## Best Practices

### Keep Modules Small

One module should manage one responsibility.

Examples:

```text
vnet-module
vm-module
storage-module
database-module
```

### Use Variables

Avoid hardcoded values.

### Provide Outputs

Expose important resource information.

### Version Control Modules

Store modules in Git repositories.

### Use Remote Modules

For enterprise projects, host modules centrally.

---

## Conclusion

Terraform Modules are the foundation of scalable Infrastructure as Code.

They provide:

* Reusability
* Maintainability
* Security
* Scalability
* Team Collaboration

Using modules allows organizations to standardize cloud infrastructure and simplify deployments across multiple projects.
