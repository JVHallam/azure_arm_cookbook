# azure_arm_cookbook

## Blank Base

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
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
