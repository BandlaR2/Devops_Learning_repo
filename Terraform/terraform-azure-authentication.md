# Terraform Azure Authentication and Resource Deployment

## 1. Azure Authentication for Terraform

### Why Authentication is Required

Terraform requires permission to create, update, and delete resources in Azure. Without authentication, Terraform cannot interact with Azure services.

### Authentication Methods

#### Method 1: Azure CLI Authentication

```bash
az login
```

* Uses personal Azure account credentials.
* Suitable for learning and testing.
* Not recommended for production environments.

#### Method 2: Service Principal Authentication (Recommended)

A Service Principal is a special identity created for applications and automation tools.

**Examples:**

* Terraform
* GitHub Actions
* Azure DevOps
* Jenkins

### Why Use a Service Principal?

**Advantages:**

* More secure
* Automated authentication
* Limited permissions
* Supports CI/CD pipelines
* Does not expose personal credentials

---

## 2. Service Principal Creation

### Create Service Principal

```bash
az ad sp create-for-rbac \
--name terraform-sp \
--role Contributor \
--scopes /subscriptions/<subscription-id>
```

### Sample Output

```json
{
  "appId": "xxxxxxxx",
  "password": "xxxxxxxx",
  "tenant": "xxxxxxxx"
}
```

### Credentials Required

| Azure Value     | Terraform Variable |
| --------------- | ------------------ |
| appId           | Client ID          |
| password        | Client Secret      |
| tenant          | Tenant ID          |
| subscription-id | Subscription ID    |

---

## 3. Configure Environment Variables

### Linux / macOS

```bash
export ARM_CLIENT_ID="client-id"
export ARM_CLIENT_SECRET="client-secret"
export ARM_SUBSCRIPTION_ID="subscription-id"
export ARM_TENANT_ID="tenant-id"
```

### Permanent Configuration

Add the above variables to:

```bash
~/.bashrc
```

or

```bash
~/.zshrc
```

Apply changes:

```bash
source ~/.bashrc
```

### Windows PowerShell

```powershell
$env:ARM_CLIENT_ID="client-id"
$env:ARM_CLIENT_SECRET="client-secret"
$env:ARM_SUBSCRIPTION_ID="subscription-id"
$env:ARM_TENANT_ID="tenant-id"
```

---

## 4. Terraform Azure Provider

The provider acts as a bridge between Terraform and Azure.

### Provider Configuration

```terraform
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

### Purpose

* Downloads Azure provider plugins
* Connects Terraform to Azure
* Creates and manages Azure resources

---

## 5. Terraform Workflow

### Step 1: Initialize Terraform

```bash
terraform init
```

**Purpose:**

* Download provider plugins
* Initialize Terraform project

### Step 2: Review Changes

```bash
terraform plan
```

**Purpose:**

* Compare infrastructure changes
* Preview resources before creation

### Step 3: Create Resources

```bash
terraform apply
```

**Purpose:**

* Deploy infrastructure

### Step 4: Delete Resources

```bash
terraform destroy
```

**Purpose:**

* Remove infrastructure

---

## 6. Terraform Registry

Terraform Registry is the official documentation repository for Terraform providers, resources, and modules.

### Contains

* Providers
* Resources
* Modules
* Examples
* Documentation

### Examples

**Azure Storage Account**

Search:

```text
azurerm_storage_account
```

**Azure Linux Virtual Machine**

Search:

```text
azurerm_linux_virtual_machine
```

---

## 7. Azure Resource Group Creation

### Example

```terraform
resource "azurerm_resource_group" "example" {
  name     = "terraform-rg"
  location = "East US"
}
```

### Purpose

A Resource Group acts as a logical container for Azure resources.

---

## 8. Azure Storage Account Creation

### Example

```terraform
resource "azurerm_storage_account" "example" {
  name                     = "terraformstorage001"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

### Purpose

Azure Storage Accounts are used to store:

* Blobs
* Files
* Queues
* Tables

---

## Conclusion

This project demonstrates:

* Azure Authentication using Service Principal
* Terraform Azure Provider Configuration
* Terraform Workflow (Init, Plan, Apply, Destroy)
* Azure Resource Group Creation
* Azure Storage Account Deployment
* Terraform Registry Usage

Terraform enables Infrastructure as Code (IaC), making Azure resource deployment automated, repeatable, and scalable.
