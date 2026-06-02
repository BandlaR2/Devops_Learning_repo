# Terraform Infrastructure as Code - Notes

## What is Infrastructure as Code (IaC)?

Infrastructure as Code (IaC) is the process of managing and provisioning cloud infrastructure using code instead of manually creating resources from cloud consoles like AWS or Azure.

Using IaC, we can automate:

* Virtual Machines
* Storage
* Networking
* Databases
* Security Groups
* Kubernetes Clusters

---

# Problems with Manual Infrastructure

Manual infrastructure management causes:

* Human errors
* Inconsistent environments
* Difficult troubleshooting
* Slow deployments
* Poor scalability

Example:
If one engineer manually creates infrastructure and another engineer tries to recreate it later, there may be:

* Missing configurations
* Incorrect security groups
* Wrong networking setup
* Extra or missing resources

This creates inconsistent environments.

---

# Benefits of Infrastructure as Code

## 1. Consistency

Development, Testing, and Production environments remain identical.

## 2. Automation

Infrastructure can be deployed within minutes.

## 3. Version Control

Infrastructure code can be stored in GitHub and tracked like application code.

## 4. Scalability

Terraform can create:

* 10 servers
* 100 servers
* 1000 servers
  using the same configuration.

## 5. Cost Optimization

Unused resources can be destroyed and recreated later automatically.

---

# Imperative vs Declarative Approach

## Imperative Approach

Step-by-step instructions are provided.

Example:

```bash
az group create --name my-rg --location eastus
```

Problem:
If the resource already exists, command fails.

---

## Declarative Approach

We define the final desired state.

Example:

```hcl
resource "azurerm_resource_group" "example" {
  name     = "terraform-rg"
  location = "East US"
}
```

Terraform automatically:

* Creates resources
* Updates resources
* Verifies existing infrastructure
* Maintains consistency

Terraform uses Declarative Infrastructure as Code.

---

# Why Terraform?

## Multi-Cloud Support

Terraform supports:

* AWS
* Azure
* Google Cloud
* Kubernetes
* GitHub
* Datadog
* SaaS Providers
* On-premises Infrastructure

---

# Advantages of Terraform

## 1. Cloud Agnostic

No vendor lock-in.

## 2. Simple HCL Syntax

Terraform uses HashiCorp Configuration Language (HCL).

Example:

```hcl
resource "azurerm_resource_group" "example" {
  name     = "terraform-rg"
  location = "East US"
}
```

---

## 3. State Management

Terraform maintains infrastructure state in:

```bash
terraform.tfstate
```

This helps:

* Track resources
* Prevent duplication
* Enable collaboration

---

## 4. Reusable Modules

Infrastructure components can be reused across projects.

---

## 5. Extensible Architecture

Terraform supports providers for:

* Cloud Platforms
* Databases
* Monitoring Tools
* Networking Services
* SaaS Applications

---

# Terraform Configuration File

Terraform files use:

```bash
.tf
```

Main configuration file:

```bash
main.tf
```

---

# Main Blocks in Terraform

## 1. Provider Block

Defines cloud provider.

Example:

```hcl
provider "azurerm" {
  features {}
}
```

---

## 2. Resource Block

Defines infrastructure resources.

Example:

```hcl
resource "azurerm_resource_group" "example" {
  name     = "terraform-rg"
  location = "East US"
}
```

---

## 3. Output Block

Displays information after deployment.

Example:

```hcl
output "resource_group_name" {
  value = azurerm_resource_group.example.name
}
```

---

# Terraform Workflow

## Step 1 - Write Configuration

Create:

```bash
main.tf
```

---

## Step 2 - Initialize Terraform

```bash
terraform init
```

Purpose:

* Downloads providers
* Initializes backend
* Prepares working directory

---

## Step 3 - Preview Infrastructure

```bash
terraform plan
```

Shows:

* Resources to add
* Modify
* Destroy

---

## Step 4 - Deploy Infrastructure

```bash
terraform apply
```

Creates infrastructure.

---

## Step 5 - Modify Infrastructure

Update `.tf` file and run:

```bash
terraform apply
```

Terraform updates resources.

---

## Step 6 - Destroy Infrastructure

```bash
terraform destroy
```

Deletes all resources.

---

# Azure Authentication for Terraform

## Method 1 - Azure CLI Login

```bash
az login
```

or

```bash
az login --use-device-code
```

---

## Method 2 - Service Principal

Used in:

* CI/CD pipelines
* Enterprise automation
* Production environments

---

# VS Code Extensions for Terraform

## 1. HashiCorp Terraform

Provides:

* Auto completion
* Syntax highlighting

## 2. HashiCorp HCL

Supports Terraform HCL language.

---

# Important Terraform Commands

## Create Project

```bash
mkdir terraform-project
cd terraform-project
```

---

## Create Terraform File

```bash
touch main.tf
```

---

## Initialize

```bash
terraform init
```

---

## Preview Changes

```bash
terraform plan
```

---

## Apply Infrastructure

```bash
terraform apply
```

---

## Destroy Infrastructure

```bash
terraform destroy
```

---

# Interview Summary

Terraform is an Infrastructure as Code tool used to automate cloud infrastructure provisioning using declarative configuration files. It supports multi-cloud environments and enables scalable, repeatable, and version-controlled infrastructure deployment.
