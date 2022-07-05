# Terraform in Azure 
`note : Am using vitual studio code.`

#### Install Azure CLI

Installation Guide link : [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

`note : macOS`

```bash 
brew update && brew install azure-cli
```

#### Login to azure using VSCode terminal 

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

###### Tip Use `terraform apply -auto-approve`  there is no interation (we dont want to enter yes to apply)

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

Terraform State : [Terraform State Docs](https://www.terraform.io/language/state)

###### Tip Use `terraform show` command for show all detailes.
###### Tip Use `terraform state show <resource name>` command for show all detailes of selected.
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

#### `Note : following steps are used for delete all we created from azure`

Terraform Destroy Docs : [Terraform CLI destroy](https://www.terraform.io/cli/commands/destroy)

#### This will run [`terraform plan`](https://www.terraform.io/cli/commands/plan) in _destroy_ mode, showing you the proposed destroy changes without executing them.
```bash 
mr4dot@machine Terraform-Azure % terraform plan -destroy
azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]
azurerm_virtual_network.mr4dot-vn: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # azurerm_resource_group.mr4dot-rg will be destroyed
  - resource "azurerm_resource_group" "mr4dot-rg" {
      - id       = "/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource" -> null
      - location = "eastasia" -> null
      - name     = "mr4dot-resource" -> null
      - tags     = {
          - "environment" = "ad_tcm"
        } -> null
    }

  # azurerm_virtual_network.mr4dot-vn will be destroyed
  - resource "azurerm_virtual_network" "mr4dot-vn" {
      - address_space           = [
          - "10.10.10.0/24",
        ] -> null
      - dns_servers             = [] -> null
      - flow_timeout_in_minutes = 0 -> null
      - guid                    = "4bf9bc63-0aaa-44df-8761-0d3f7215bc04" -> null
      - id                      = "/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network" -> null
      - location                = "eastasia" -> null
      - name                    = "mr4dot-network" -> null
      - resource_group_name     = "mr4dot-resource" -> null
      - subnet                  = [] -> null
      - tags                    = {
          - "environment" = "ad_tcm"
        } -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
mr4dot@machine Terraform-Azure % 
```
#### After that we can run `terraform apply -destroy`  for destroy all.
```bash 
mr4dot@machine Terraform-Azure % terraform apply -destroy
azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]
azurerm_virtual_network.mr4dot-vn: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # azurerm_resource_group.mr4dot-rg will be destroyed
  - resource "azurerm_resource_group" "mr4dot-rg" {
      - id       = "/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource" -> null
      - location = "eastasia" -> null
      - name     = "mr4dot-resource" -> null
      - tags     = {
          - "environment" = "ad_tcm"
        } -> null
    }

  # azurerm_virtual_network.mr4dot-vn will be destroyed
  - resource "azurerm_virtual_network" "mr4dot-vn" {
      - address_space           = [
          - "10.10.10.0/24",
        ] -> null
      - dns_servers             = [] -> null
      - flow_timeout_in_minutes = 0 -> null
      - guid                    = "4bf9bc63-0aaa-44df-8761-0d3f7215bc04" -> null
      - id                      = "/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network" -> null
      - location                = "eastasia" -> null
      - name                    = "mr4dot-network" -> null
      - resource_group_name     = "mr4dot-resource" -> null
      - subnet                  = [] -> null
      - tags                    = {
          - "environment" = "ad_tcm"
        } -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

azurerm_virtual_network.mr4dot-vn: Destroying... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network]
azurerm_virtual_network.mr4dot-vn: Still destroying... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-...Network/virtualNetworks/mr4dot-network, 10s elapsed]
azurerm_virtual_network.mr4dot-vn: Destruction complete after 13s
azurerm_resource_group.mr4dot-rg: Destroying... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]
azurerm_resource_group.mr4dot-rg: Still destroying... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-...40b83e0/resourceGroups/mr4dot-resource, 10s elapsed]
azurerm_resource_group.mr4dot-rg: Still destroying... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-...40b83e0/resourceGroups/mr4dot-resource, 20s elapsed]
^[[azurerm_resource_group.mr4dot-rg: Still destroying... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-...40b83e0/resourceGroups/mr4dot-resource, 30s elapsed]
^R
azurerm_resource_group.mr4dot-rg: Still destroying... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-...40b83e0/resourceGroups/mr4dot-resource, 40s elapsed]
azurerm_resource_group.mr4dot-rg: Still destroying... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-...40b83e0/resourceGroups/mr4dot-resource, 50s elapsed]
azurerm_resource_group.mr4dot-rg: Still destroying... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-...40b83e0/resourceGroups/mr4dot-resource, 1m0s elapsed]
azurerm_resource_group.mr4dot-rg: Destruction complete after 1m7s

Apply complete! Resources: 0 added, 0 changed, 2 destroyed.
mr4dot@machine Terraform-Azure % 
```

#### To check run : `terraform show`
```bash 
mr4dot@machine Terraform-Azure % terraform show

mr4dot@machine Terraform-Azure % 
```
** The out put is zero so destroy command is worked properly **

#### `Step 5` Setup sub-net 

Azure subnet Terraform Docs : [Azure Sub-net Conf](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/subnet)

#### Run `terraform plan`,`terraform apply -auto-approve`
```hcl 
resource "azurerm_subnet" "mr4dot-sn" {
  name                 = "mr4dot-sn"
  resource_group_name  = azurerm_resource_group.mr4dot-rg.name
  virtual_network_name = azurerm_virtual_network.mr4dot-vn.name
  address_prefixes     = ["10.10.10.0/24"]
}
```
```bash 
mr4dot@machine Terraform-Azure % terraform plan
azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]
azurerm_virtual_network.mr4dot-vn: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_subnet.mr4dot-sn will be created
  + resource "azurerm_subnet" "mr4dot-sn" {
      + address_prefixes                               = [
          + "10.10.10.0/24",
        ]
      + enforce_private_link_endpoint_network_policies = false
      + enforce_private_link_service_network_policies  = false
      + id                                             = (known after apply)
      + name                                           = "mr4dot-sn"
      + resource_group_name                            = "mr4dot-resource"
      + virtual_network_name                           = "mr4dot-network"
    }

Plan: 1 to add, 0 to change, 0 to destroy.

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.

mr4dot@machine Terraform-Azure % terraform apply -auto-approve
azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]
azurerm_virtual_network.mr4dot-vn: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_subnet.mr4dot-sn will be created
  + resource "azurerm_subnet" "mr4dot-sn" {
      + address_prefixes                               = [
          + "10.10.10.0/24",
        ]
      + enforce_private_link_endpoint_network_policies = false
      + enforce_private_link_service_network_policies  = false
      + id                                             = (known after apply)
      + name                                           = "mr4dot-sn"
      + resource_group_name                            = "mr4dot-resource"
      + virtual_network_name                           = "mr4dot-network"
    }

Plan: 1 to add, 0 to change, 0 to destroy.
azurerm_subnet.mr4dot-sn: Creating...
azurerm_subnet.mr4dot-sn: Creation complete after 8s [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network/subnets/mr4dot-sn]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

#### `Step 5` Setup security groups and network rules.  

```hcl 
resource "azurerm_network_security_group" "mr4dot-sg" {
  name                = "mr4dot-sg"
  location            = azurerm_resource_group.mr4dot-rg.location
  resource_group_name = azurerm_resource_group.mr4dot-rg.name
  tags = {
    environment = "ad_tcm"
  }
}
resource "azurerm_network_security_rule" "mr4dot-snr" {
  name                        = "mr4dot-snr"
  priority                    = 100
  direction                   = "Inbound"
  access                      = "Allow"
  protocol                    = "*"
  source_port_range           = "*"
  destination_port_range      = "*"
  source_address_prefix       = "*"
  destination_address_prefix  = "*"
  resource_group_name         = azurerm_resource_group.mr4dot-rg.name
  network_security_group_name = azurerm_network_security_group.mr4dot-sg.name
}
```
##### `Note : From here onwards am not showing the entire out (it is so long) replaced with some ...`
```bash 
mr4dot@machine Terraform-Azure % terraform plan

azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]
azurerm_virtual_network.mr4dot-vn: Refreshing state... 
.
.
out put message ..
.
.
Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.

mr4dot@machine Terraform-Azure % terraform apply -auto-approve

azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]

Terraform will perform the following actions:
.
.
out put message ..
.
.
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
mr4dot@machine Terraform-Azure % 
```
#### `Step 6` Asscociate security groups and network rules to our Subnet
```hcl 
resource "azurerm_subnet_network_security_group_association" "mr4dot-sga" {
  subnet_id                 = azurerm_subnet.mr4dot-sn.id
  network_security_group_id = azurerm_network_security_group.mr4dot-sg.id
}
```
```bash 
mr4dot@machine Terraform-Azure % terraform plan

azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]
.
.
out put message ..
.
.
Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.

mr4dot@machine Terraform-Azure % terraform apply -auto-approve

azurerm_resource_group.mr4dot-rg: Refreshing state... [id=/subscriptions/1f2a6ed1-90fe-411a-b4cf-28b5b40b83e0/resourceGroups/mr4dot-resource]
.
.
out put message ..
.
.
resource/providers/Microsoft.Network/virtualNetworks/mr4dot-network/subnets/mr4dot-sn]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
mr4dot@machine Terraform-Azure %
```

#### `Step 7`  Make public IP Static/Dynamic

Azure Public IP Static Doc [azurerm_public_ip](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/public_ip) 

```hcl
resource "azurerm_public_ip" "mr4dot-ip" {
  name                = "mr4dot-ip"
  resource_group_name = azurerm_resource_group.mr4dot-rg.name
  location            = azurerm_resource_group.mr4dot-rg.location
  allocation_method   = "Static"

  tags = {
    environment = "ad_tcm"
  }
  ```
```bash
mr4dot@machine Terraform-Azure % terraform plan
.
.
out put message ..
.
.
Plan: 1 to add, 0 to change, 0 to destroy.

mr4dot@machine Terraform-Azure % terraform apply -auto-approve
.
.
out put message ..
.
.
Plan: 1 to add, 0 to change, 0 to destroy.
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
mr4dot@machine Terraform-Azure % terraform plan            
```

#### `Step 8`  Config network interface.
Azure Network Interface Docs : [azurerm_network_interface](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_interface)

```hcl
resource "azurerm_network_interface" "mr4dot-nic" {
  name                = "mr4dot-nic"
  location            = azurerm_resource_group.mr4dot-rg.location
  resource_group_name = azurerm_resource_group.mr4dot-rg.name
  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.mr4dot-sn.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id          = azurerm_public_ip.mr4dot-ip.id
  }
  tags = {
    environment = "ad_tcm"
  }
}
```
```bash 
mr4dot@machine Terraform-Azure % terraform plan
.
.
out put message ..
.
.
Plan: 1 to add, 0 to change, 0 to destroy.

mr4dot@machine Terraform-Azure % terraform apply -auto-approve
.
.
out put message ..
.
.
Plan: 1 to add, 0 to change, 0 to destroy.
.
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
mr4dot@machine Terraform-Azure % 
```
