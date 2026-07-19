Here's a comprehensive comparison between **Terraform**, **Azure ARM Templates**, and **Azure Bicep**.

| Feature                   | Terraform                                | Azure ARM Templates      | Azure Bicep                  |
| ------------------------- | ---------------------------------------- | ------------------------ | ---------------------------- |
| **Vendor**                | HashiCorp                                | Microsoft                | Microsoft                    |
| **Language**              | HCL (HashiCorp Configuration Language)   | JSON                     | Bicep DSL                    |
| **Cloud Support**         | Multi-cloud (Azure, AWS, GCP, OCI, etc.) | Azure Only               | Azure Only                   |
| **Readability**           | ⭐⭐⭐⭐⭐ Easy                               | ⭐ Difficult              | ⭐⭐⭐⭐⭐ Very Easy              |
| **Learning Curve**        | Medium                                   | High                     | Low                          |
| **File Extension**        | `.tf`                                    | `.json`                  | `.bicep`                     |
| **State File**            | Yes (`terraform.tfstate`)                | No                       | No                           |
| **Deployment Engine**     | Terraform Engine                         | Azure Resource Manager   | Azure Resource Manager       |
| **Dependency Management** | Automatic                                | Manual (`dependsOn`)     | Automatic                    |
| **Modules**               | Yes                                      | Linked/Nested Templates  | Yes                          |
| **Loops & Conditions**    | Excellent                                | Complex                  | Simple                       |
| **Rollback**              | Manual                                   | Limited                  | Limited                      |
| **Drift Detection**       | Yes (`terraform plan`)                   | No                       | No                           |
| **Cross-Cloud Support**   | Yes                                      | No                       | No                           |
| **Community Modules**     | Huge Terraform Registry                  | Limited                  | Azure Verified Modules (AVM) |
| **Best For**              | Enterprise Multi-Cloud                   | Legacy Azure Deployments | Modern Azure Deployments     |

---

# Architecture

```text
                Infrastructure as Code

        ┌─────────────┬─────────────┬─────────────┐
        │ Terraform   │ ARM Template│   Bicep     │
        └─────────────┴─────────────┴─────────────┘

               ↓              ↓             ↓

        Terraform CLI     ARM Engine    Bicep CLI
               ↓              ↑             ↓
               └──────── Azure Resource Manager ───────┘
                               ↓
                      Azure Resources
```

---

# Language Comparison

## Terraform

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "DemoRG"
  location = "East US"
}
```

---

## ARM Template

```json
{
  "type": "Microsoft.Resources/resourceGroups",
  "apiVersion": "2024-01-01",
  "name": "DemoRG",
  "location": "East US"
}
```

---

## Bicep

```bicep
resource rg 'Microsoft.Resources/resourceGroups@2024-01-01' = {
  name: 'DemoRG'
  location: 'East US'
}
```

---

# File Structure

## Terraform

```
terraform-project
│
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── providers.tf
└── modules/
```

---

## ARM

```
arm-project
│
├── azuredeploy.json
├── azuredeploy.parameters.json
```

---

## Bicep

```
bicep-project
│
├── main.bicep
├── parameters.bicepparam
├── modules/
│     ├── storage.bicep
│     └── vm.bicep
```

---

# Deployment Commands

## Terraform

```bash
terraform init

terraform validate

terraform plan

terraform apply

terraform destroy
```

---

## ARM

```bash
az deployment group create \
  --resource-group MyRG \
  --template-file azuredeploy.json \
  --parameters azuredeploy.parameters.json
```

Delete deployment history

```bash
az deployment group delete \
  --resource-group MyRG \
  --name deploymentName
```

Delete Resource Group

```bash
az group delete \
  --name MyRG \
  --yes
```

---

## Bicep

```bash
az deployment group create \
  --resource-group MyRG \
  --template-file main.bicep \
  --parameters parameters.bicepparam
```

Delete deployment

```bash
az deployment group delete \
  --resource-group MyRG \
  --name deploymentName
```

Delete Resource Group

```bash
az group delete \
  --name MyRG \
  --yes
```

---

# State Management

## Terraform

```text
main.tf
      ↓
terraform apply
      ↓
terraform.tfstate
      ↓
Tracks Every Resource
```

Advantages:

* Knows what exists
* Detects drift
* Plans changes before deployment

---

## ARM & Bicep

```text
main.bicep
      ↓
Azure Resource Manager
      ↓
Azure Resources
```

No state file is maintained. Azure Resource Manager determines the desired state during deployment.

---

# Modules

## Terraform

```hcl
module "storage" {
  source = "./modules/storage"
}
```

---

## Bicep

```bicep
module storage './modules/storage.bicep' = {
  name: 'storageDeploy'
}
```

---

## ARM

Uses nested or linked templates.

---

# Advantages

## Terraform

* Multi-cloud
* Large ecosystem
* State management
* Drift detection
* Excellent modules
* Mature community

### Disadvantages

* State file management
* Remote backend required for teams
* Slightly slower for Azure-only deployments

---

## ARM

* Native Azure support
* No additional tools
* Stable
* Complete Azure feature coverage

### Disadvantages

* Verbose JSON
* Difficult to read and maintain
* Complex syntax

---

## Bicep

* Native Azure language
* Very readable
* Compiles to ARM JSON
* Automatic dependency handling
* Excellent IntelliSense in VS Code
* No state file

### Disadvantages

* Azure only
* No built-in drift detection
* No multi-cloud support

---

# Interview Questions

| Question              | Terraform | ARM              | Bicep     |
| --------------------- | --------- | ---------------- | --------- |
| Multi-cloud?          | ✅ Yes     | ❌ No             | ❌ No      |
| State File?           | ✅ Yes     | ❌ No             | ❌ No      |
| Deployment Engine     | Terraform | ARM              | ARM       |
| Language              | HCL       | JSON             | Bicep DSL |
| Best Readability      | ⭐⭐⭐⭐⭐     | ⭐                | ⭐⭐⭐⭐⭐     |
| Microsoft Recommended | No        | Legacy           | ✅ Yes     |
| Dependency Handling   | Automatic | `dependsOn`      | Automatic |
| Modules               | Yes       | Nested Templates | Yes       |

---

# When to Use Which?

| Scenario                                          | Recommended Tool                    |
| ------------------------------------------------- | ----------------------------------- |
| Azure-only projects                               | ✅ Azure Bicep                       |
| AWS + Azure + GCP                                 | ✅ Terraform                         |
| Existing ARM template maintenance                 | ✅ ARM Templates                     |
| New Azure Infrastructure                          | ✅ Azure Bicep                       |
| Enterprise multi-cloud platform                   | ✅ Terraform                         |
| Azure Landing Zones with multiple cloud providers | ✅ Terraform                         |
| Learning Azure IaC                                | ✅ Azure Bicep first, then Terraform |

---

# Quick Decision Guide

```text
                 Need Multi-Cloud?
                      │
          ┌───────────┴───────────┐
          │                       │
        Yes                      No
          │                       │
    Use Terraform         Azure Only?
                                  │
                     ┌────────────┴────────────┐
                     │                         │
                New Project              Existing ARM
                     │                         │
               Use Bicep             Continue with ARM
```

## Summary

* **Terraform**: Best for multi-cloud environments, advanced state management, and enterprise-scale infrastructure automation.
* **Azure ARM Templates**: Native Azure JSON templates, primarily maintained for backward compatibility and legacy deployments.
* **Azure Bicep**: Microsoft's recommended Infrastructure as Code language for Azure today. It offers a cleaner syntax, native Azure integration, and is ideal for new Azure projects.
