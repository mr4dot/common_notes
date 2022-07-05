# Terraform in Azure 
`note : Am using vitual studio code.`

#### Install Azure CLI

Installation Guide link : [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

`note : macOS`

```bash 
brew update && brew install azure-cli
```

#### Login to azure using vitual code terminal 

```bash 
mr4dot@machine ~ % az login 

'default browser will popup and login in that'

if any issue follow the below steps

mr4dot@machine ~ % az login --use-device-code 

'you will get one link and one code follow the link in your browser and login azure account and paste the code you got !'

after login you can check with 

mr4dot@machine ~ % az account show
{
  "environmentName": "AzureCloud",
  "homeTenantId": "5d50341a-4f77-4d01-888a-77134832f9cb",
  "id": "1f2a6ed1-90je-411a-b4cf-28b5b40b83re0",
  "isDefault": true,
  "managedByTenants": [],
  "name": "Azure subscription 1",
  "state": "Enabled",
  "tenantId": "5d50241e-4f77-4d01-888a-76793832f9cb",
  "user": {
    "name": "0xk4slr@gmail.com",
    "type": "user"
  }
}
mr4dot@machine ~ % 
```

#### Install Terraform Extension for VSCode

** Go to extensions and search terraform and install **

![[Pasted image 20220704143534.png]]

### Now all set to go.

###### Tip Use `terraform fmt` for clean and auto intend your code
```bash 
mr4dot@machine Terraform-Azure % terraform fmt
main.tf
mr4dot@machine Terraform-Azure % 
```
#### Terraform Basic Terminal Commands

#### `Step 1` Create a  working directory 
```hcl
step 1 : create a folder in your computer and open that using vscode
step 2 : create new file name : main.tf
step 3 : write you code
```
#### `Step 2` Setup  Azure Provider

Docs azure provider : [Azure provider](https://meet.google.com/awe-wczq-ymw)

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=3.0.0"
    }
  }
}

# Configure the Microsoft Azure Provider
provider "azurerm" {
  features {}
}
```
`note : save the code`

The `terraform init` command is used to initialize a working directory containing Terraform configuration files.
**

```bash 
mr4dot@machine Terraform-Azure %  terraform init

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/azurerm from the dependency lock file
- Using previously-installed hashicorp/azurerm v3.0.0

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
mr4dot@machine Terraform-Azure % 
```

#### `Step 3` Setup resource group 
```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=3.0.0"
    }
  }
}

provider "azurerm" {
  features {}
}
resource "azurerm_resource_group" "mr4dot-rg" {
  name     = "mr4dot-rg"
  location = "East Asia"
  tags ={
    "environment" = "ad_tcm"
    }
}
```
`note : save the code`

The `terraform plan` command creates an execution plan, which lets you preview the changes that Terraform plans to make to your infrastructure.

```bash 
mr4dot@machine Terraform-Azure % terraform plan

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found no differences, so no changes are needed.
mr4dot@machine Terraform-Azure % terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_resource_group.mr4dot-rg will be created
  + resource "azurerm_resource_group" "mr4dot-rg" {
      + id       = (known after apply)
      + location = "eastasia"
      + name     = "mr4dot-rg"
      + tags     = {
          + "environment" = "ad_tcm"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
```

#### `Step 4` Apply the plan

The `terraform apply` command executes the actions proposed in a Terraform plan.

```hcl
mr4dot@machine Terraform-Azure % terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_resource_group.mr4dot-rg will be created
  + resource "azurerm_resource_group" "mr4dot-rg" {
      + id       = (known after apply)
      + location = "eastasia"
      + name     = "mr4dot-rg"
      + tags     = {
          + "environment" = "ad_tcm"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

azurerm_resource_group.mr4dot-rg: Creating...
azurerm_resource_group.mr4dot-rg: Creation complete after 2s [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-rg]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
mr4dot@machine Terraform-Azure % 
```

#### `Step 4` Setup network
`note : from here only showing added part of the code`
```hcl 
resource "azurerm_virtual_network" "mr4dot-vn" {
  name                = "mr4dot-network"
  resource_group_name = azurerm_resource_group.mr4dot-rg.name
  location            = azurerm_resource_group.mr4dot-rg.location
  address_space       = ["10.10.10.0/24"]

  tags = {
    environment = "ad_tcm"
  }
}
```
```bash 
terraform plan 
azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_virtual_network.mr4dot-vn will be created
  + resource "azurerm_virtual_network" "mr4dot-vn" {
      + address_space       = [
          + "10.10.10.0/24",
        ]
      + dns_servers         = (known after apply)
      + guid                = (known after apply)
      + id                  = (known after apply)
      + location            = "eastasia"
      + name                = "mr4dot-network"
      + resource_group_name = "mr4dot-resource"
      + subnet              = (known after apply)
      + tags                = {
          + "environment" = "ad_tcm"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

──────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
mr4dot@machine Terraform-Azure % terraform apply
azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_virtual_network.mr4dot-vn will be created
mr4dot@machine Terraform-Azure % 
```
#### Screenshots in portal
![[Pasted image 20220704200834.png]]
![[Pasted image 20220704200812.png]]

###### Tip : Use `terraform state list`  command is used to list resources within a Terraform state.

```bash 
mr4dot@machine Terraform-Azure % terraform state list
azurerm_resource_group.mr4dot-rg
azurerm_virtual_network.mr4dot-vn
mr4dot@machine Terraform-Azure % 
```
###### Tip Use `terraform state show <resource name>` command for show all detailes of selected 
```bash 
mr4dot@machine Terraform-Azure % terraform state list
azurerm_resource_group.mr4dot-rg
azurerm_virtual_network.mr4dot-vn
mr4dot@machine Terraform-Azure % terraform state show azurerm_virtual_network.mr4dot-vn
# azurerm_virtual_network.mr4dot-vn:
resource "azurerm_virtual_network" "mr4dot-vn" {
    address_space           = [
        "10.10.10.0/24",
    ]
    dns_servers             = []
    flow_timeout_in_minutes = 0
    guid                    = "4bf9bc63-0aaa-44df-8761-0d3f7215bc04"
    id                      = "/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network"
    location                = "eastasia"
    name                    = "mr4dot-network"
    resource_group_name     = "mr4dot-resource"
    subnet                  = []
    tags                    = {
        "environment" = "ad_tcm"
    }
}
mr4dot@machine Terraform-Azure % 
```

