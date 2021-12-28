# azure_arm_cookbook

## Blank Base

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources" : []
}
```

## Blank Storage Account

```json
{
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "2019-04-01",
  "name": "st-name",
  "location": "UK South",
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "StorageV2"
}
```

## Blank Storage Container
```json
{
    "type": "blobServices/containers",
    "apiVersion": "2021-02-01",
    "name": "default/sc-name",
    "dependsOn" : [
        "ParentStorageAccountsName"
    ]
}
```

## SQL Server
```json
{
    "type": "Microsoft.Sql/servers",
        "apiVersion": "2015-05-01-preview",
        "name": "[parameters('databaseServerName')]",
        "location": "[parameters('location')]",
        "properties": {
            "administratorLogin": "[parameters('adminUser')]",
            "administratorLoginPassword": "[parameters('adminPassword')]",
            "version": "12.0"
        },
        "resources": [
        {
            "type": "firewallrules",
            "apiVersion": "2015-05-01-preview",
            "name": "AllowAllAzureIps",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[parameters('databaseServerName')]"
            ],
            "properties": {
                "startIpAddress": "0.0.0.0",
                "endIpAddress": "0.0.0.0"
            }
        }
        ]
}

```

## Generate Psuedo-Random String Seeded By UTC time
```json
{
    "parameters": {
        "randomSeed" : {
            "type" : "string",
            "defaultValue" : "[utcNow()]"
        }
    },
    "variables": {
        "pass": {
            "value": "[concat('FT12!', substring(uniqueString(parameters('randomSeed')),0,7))]"
        }
    }
}
```

## Azure Container Images ( ACI ) - Echo Container
```json
{
    "type": "Microsoft.ContainerInstance/containerGroups",
    "apiVersion": "2021-09-01",
    "name": "echo",
    "location": "[resourceGroup().location]",
    "properties": {
        "sku": "Standard",
        "containers": [
            {
                "name": "echo",
                "properties": {
                    "image": "ealen/echo-server",
                    "ports": [
                        {
                            "protocol": "TCP",
                            "port": 80
                        }
                    ],
                    "resources": {
                        "requests": {
                            "memoryInGB": 1,
                            "cpu": 1
                        }
                    }
                }
            }
        ],
        "restartPolicy": "OnFailure",
        "ipAddress": {
            "ports": [
                {
                    "protocol": "TCP",
                    "port": 80
                }
            ],
            "type": "Public"
        },
        "osType": "Linux"
    }
}
```
