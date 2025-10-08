# Block IP in Defender

## Description

This playbook is designed to take IP entities from a Sentinel Incident and add them to the IOC endpoint block list in Microsoft Defender. The block is set to not create an alert by default and is set to not expire. These properties can be changed in the Defender portal once the block has been create. Default settings can be changed in the playbook by adjusting the HTTP post action.

## Prequisites

An admin with Privilege Role Admin or Global Admin will be needed to assign the necessary MDE API permissions to the Managed Identity.

## Quick Deployment

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/


```powershell
# Replace with your managed identity's Object ID
$miObjectID = "<your-managed-identity-object-id>"

# Defender for Endpoint App ID
$appId = "fc780465-2017-40d4-a0c5-307022471b92"

Connect-AzureAD

$app = Get-AzureADServicePrincipal -Filter "AppId eq '$appId'"

$role = $app.AppRoles | Where-Object { $_.Value -eq "Ti.ReadWrite" } | Select-Object -First 1

New-AzureADServiceAppRoleAssignment -Id $role.Id -ObjectId $miObjectID -PrincipalId $miObjectID -ResourceId $app.ObjectId
```
