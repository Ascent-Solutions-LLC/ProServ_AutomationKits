# Isolate Device in MDE

## Description

This playbook will use device entities from Sentinel/MDE generated incidents and isolate those devices using MDE. This is accomplished via the Windows Defender Advanced Threat Protection (WDATP) API. The playbook utilizes the System-Assigned Managed Identity to interact with the incident, retrieve the entities, submit the isolation in MDE, and update the incident with a comment. This playbook will require specific permissions which will be addressed in the requirements section.

## Requirements

MDE API permissions are going to require a user with Global Admin (GA) or Privileged Role Admin permissions. A Azure CLI powershell script is available below to deploy those rules. This will be done after the deployment of the playbook. Appropriate permissions to assign the RBAC permssions of Sentinel Responder to the managed identity will also be required.

## Deployment


```powershell
$MIGuid = '<Enter your managed identity guid here>' 

$MI = Get-AzureADServicePrincipal -ObjectId $MIGuid 

$MDEAppId = 'fc780465-2017-40d4-a0c5-307022471b92' 

$PermissionName = 'Machine.Isolate' 

$MDEServicePrincipal = Get-AzureADServicePrincipal -Filter 'appId eq '$MDEAppId'' 

$AppRole = $MDEServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains 'Application'} 

New-AzureAdServiceAppRoleAssignment -ObjectId $MI.ObjectId -PrincipalId $MI.ObjectId ` -ResourceId $MDEServicePrincipal.ObjectId -Id $AppRole.Id
```
