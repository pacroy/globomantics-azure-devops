# Globomantics Azure DevOps

This is the repository for the exercise in module 5 - "Using Azure DevOps" of the training course [Implementing TYerraform on Microsoft](https://app.pluralsight.com/courses/22882793-d91e-431b-93b7-292a90438a75/table-of-contents) Azure by Ned Bellavance.

## Usage

1. Create Azure Pipelines from [`pipelines/azure-pipelines.yaml`](pipelines/azure-pipelines.yaml). 

2. Create the following environment and set up approval as needed

3. Create variable group `globomantics` with the following variables:

| Name | Description |
|---|---|
| resource_group_name | Resource group name e.g. `vnet-main` |
| sas_token | SAS token of backend storage account e.g. `?sv=2020-08-04&ss=b&srt=sco&sp=rwdlactfx&se=2021-01-01T00:00:00Z&st=2021-12-31T11:59:59Z&spr=https&sig=xxxx` |
| storage_account_name | Backend storage account name |
| sec_client_id | Service principal client ID created from module 3 exercise `3-vnet-peering` |
| sec_client_secret | Service principal client secret |
| sec_principal_id | Service principal (Enterprise App) object ID |
| sec_resource_group | Resource group name created from module 3 exercise `3-vnet-peering` |
| sec_sub_id | Subscription ID used in module 3 exercise `3-vnet-peering` |
| sec_tenant_id | Tenant ID |
| sec_vnet_id | Virtual network resource ID created from module 3 exercise `3-vnet-peering` e.g. `/subscriptions/xxxxxxxx-xxxx-xxxx-xxxxx-xxxxxxxxxxxx/resourceGroups/security/providers/Microsoft.Network/virtualNetworks/security` |
| sec_vnet_name | Virtual network name created from module 3 exercise `3-vnet-peering` |

## Troubleshooting

### AuthorizationFailed

If you see this error in `Terraform Apply` step then try to rerun the job again.

```console
│ Error: Error checking for presence of existing Peering "env_2_sec" (Virtual Network "env-vnet-main" / Resource Group "env-vnet-main"): network.VirtualNetworkPeeringsClient#Get: Failure responding to request: StatusCode=403 -- Original Error: autorest/azure: Service returned an error. Status=403 Code="AuthorizationFailed" Message="The client 'xxxxxxxx-xxxx-xxxx-xxxxx-xxxxxxxxxxxx' with object id 'xxxxxxxx-xxxx-xxxx-xxxxx-xxxxxxxxxxxx' does not have authorization to perform action 'Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read' over scope '/subscriptions/xxxxxxxx-xxxx-xxxx-xxxxx-xxxxxxxxxxxx/resourceGroups/env-vnet-main/providers/Microsoft.Network/virtualNetworks/env-vnet-main/virtualNetworkPeerings/env_2_sec' or the scope is invalid. If access was recently granted, please refresh your credentials."
│ 
│   with azurerm_virtual_network_peering.main,
│   on vnet-peering.tf line 80, in resource "azurerm_virtual_network_peering" "main":
│   80: resource "azurerm_virtual_network_peering" "main" {
│ 
╵
╷
│ Error: network.VirtualNetworkPeeringsClient#CreateOrUpdate: Failure sending request: StatusCode=0 -- Original Error: Code="LinkedAuthorizationFailed" Message="The client 'xxxxxxxx-xxxx-xxxx-xxxxx-xxxxxxxxxxxx' with object id 'xxxxxxxx-xxxx-xxxx-xxxxx-xxxxxxxxxxxx' has permission to perform action 'Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write' on scope '/subscriptions/99813d40-928b-44b2-a969-2c7ea5bd8b96/resourceGroups/security/providers/Microsoft.Network/virtualNetworks/security/virtualNetworkPeerings/sec_2_env'; however, it does not have permission to perform action 'peer/action' on the linked scope(s) '/subscriptions/xxxxxxxx-xxxx-xxxx-xxxxx-xxxxxxxxxxxx/resourceGroups/env-vnet-main/providers/Microsoft.Network/virtualNetworks/env-vnet-main' or the linked scope(s) are invalid."
│ 
│   with azurerm_virtual_network_peering.sec,
│   on vnet-peering.tf line 90, in resource "azurerm_virtual_network_peering" "sec":

```
