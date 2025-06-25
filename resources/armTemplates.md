# Azure Resource Manager - Templates

## Powershell

```powershell
$ pwsh
$ PS> Connect-AzAccount
$ PS> New-AzResourceGroupDeployment -ResourceGroupName arm-grp -TemplateFile resources/arm/arm-variable.json
```
## Template Format

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "",
  "apiProfile": "",
  "parameters": {  },
  "variables": {  },
  "functions": [  ],
  "resources": [  ],
  "outputs": {  }
}
```
## Deployment via Azure Portal

* Search for 'Template deployment'
* Click on 'Deploy from a custom template'
* Create (or paste the json)
* Build with the editor
* Save

## Scripts

Other scripts can be found [here](arm).

## Elements

### Resource id - Extract the id of that specific resource.

```json
"id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', 'my-vnet', 'subnet1')]" 
```

### Resource Group Location

```json
"location": "[resourceGroup().location]",
```

### Variables

```json
 "variables": {
        "resourceLocation" : "Central US"
    },
    "resources": [
        {           
            "name": "storage",
            "type": "Microsoft.Storage/storageAccounts",
            "location":"[variables('resourceLocation')]",

        }
```

## Parameters - If you do not pass at runtime the value(s), then default value is taken.

```json
   "parameters": {
            "storage-sku": {
                "type": "string",
                "defaultValue":"Standard_LRS",
                "allowedValues": [
                    "Standard_LRS","Standard_GRS","Standard_RAGRS"
                ]
            }
    },
    "resources": [
        {           
          ...

            "sku" :{
                "name": "[parameters('storage-sku')]"
            },
        }
            ]
```

Complete file is [here](arm/arm-storage-with-parameters.json)

You can either use the Azure Portal to pass the parameters with another file:

```powershell
$ PS>  New-AzResourceGroupDeployment -ResourceGroupName az-104 -TemplateFile ./resources/arm/arm-storage-with-parameters.json -TemplateParameterFile ./resources/arm/parameter.json
```
### securestring

```json
"parameters": {
        "vmpassword":{
            "type": "securestring",
            "metadata": {
                "description": "Please type the password"
            }
        }
    },
```

### copyIndex

```json
"resources": [
        {
            "name": "[concat('myexamplesa',copyIndex())]",
            ...
            "copy": {
                "name":"storagecopy",
                "count": 3
            }
        }
    ]
```

Complete file is [here](arm/arm-storage-account-copy.json)

### dependsOn

```json
"dependsOn": [
    "[resourceId('Microsoft.Network/networkSecurityGroups', variables('my-network-sg'))]"
],
```

## Parameters file

```powershell
$ New-AzResourceGroupDeployment -ResourceGroupName lab-az-104a-rg -TemplateFile ./resources/arm/arm-storage-account-with-parameters.json  -TemplateParameterFile  ./resources/arm/params-sa.json
```

## Deply VM - Section

```json
         "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2019-datacenter-gensecond",
                        "version": "latest"
                    },
```
offer/sku: :bangbang:

## Various

- BICEP base templates
- API version
- depends on (another resource)
- [Custom Scripts Extension](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/custom-script-windows)


## How to use ARM deployment templates with Azure CLI 
:bangbang:

```shell
$ az deployment group create --resource-group <resource-group-name> --template-file <path-to-template>
```

## Useful Links

[Understand the Structure and Syntax of ARM Template](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/syntax)

[ARM Documentation](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/overview)
