---
title: Add or remove role assignments using Azure RBAC and the REST API
description: Learn how to grant access to Azure resources for users, groups, service principals, or managed identities using Azure role-based access control (RBAC) and the REST API.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''

ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind

---
# Add or remove role assignments using Azure RBAC and the REST API

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] This article describes how to assign roles using the REST API.

## Prerequisites

To add or remove role assignments, you must have:

- `Microsoft.Authorization/roleAssignments/write` and `Microsoft.Authorization/roleAssignments/delete` permissions, such as [User Access Administrator](built-in-roles.md#user-access-administrator) or [Owner](built-in-roles.md#owner)

## Add a role assignment

In RBAC, to grant access, you add a role assignment. To add a role assignment, use the [Role Assignments - Create](/rest/api/authorization/roleassignments/create) REST API and specify the security principal, role definition, and scope. To call this API, you must have access to the `Microsoft.Authorization/roleAssignments/write` operation. Of the built-in roles, only [Owner](built-in-roles.md#owner) and [User Access Administrator](built-in-roles.md#user-access-administrator) are granted access to this operation.

1. Use the [Role Definitions - List](/rest/api/authorization/roledefinitions/list) REST API or see [Built-in roles](built-in-roles.md) to get the identifier for the role definition you want to assign.

1. Use a GUID tool to generate a unique identifier that will be used for the role assignment identifier. The identifier has the format: `00000000-0000-0000-0000-000000000000`

1. Start with the following request and body:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```

1. Within the URI, replace *{scope}* with the scope for the role assignment.

    | Scope | Type |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Management group |
    | `subscriptions/{subscriptionId1}` | Subscription |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | Resource |

1. Replace *{roleAssignmentName}* with the GUID identifier of the role assignment.

1. Within the request body, replace *{scope}* with the scope for the role assignment.

    | Scope | Type |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Management group |
    | `subscriptions/{subscriptionId1}` | Subscription |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | Resource |

1. Replace *{roleDefinitionId}* with the role definition identifier.

1. Replace *{principalId}* with the object identifier of the user, group, or service principal that will be assigned the role.

## Remove a role assignment

In RBAC, to remove access, you remove a role assignment. To remove a role assignment, use the [Role Assignments - Delete](/rest/api/authorization/roleassignments/delete) REST API. To call this API, you must have access to the `Microsoft.Authorization/roleAssignments/delete` operation. Of the built-in roles, only [Owner](built-in-roles.md#owner) and [User Access Administrator](built-in-roles.md#user-access-administrator) are granted access to this operation.

1. Get the role assignment identifier (GUID). This identifier is returned when you first create the role assignment or you can get it by listing the role assignments.

1. Start with the following request:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. Within the URI, replace *{scope}* with the scope for removing the role assignment.

    | Scope | Type |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Management group |
    | `subscriptions/{subscriptionId1}` | Subscription |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | Resource |

1. Replace *{roleAssignmentName}* with the GUID identifier of the role assignment.

## Next steps

- [List role assignments using Azure RBAC and the REST API](role-assignments-list-rest.md)
- [Deploy resources with Resource Manager templates and Resource Manager REST API](../azure-resource-manager/templates/deploy-rest.md)
- [Azure REST API Reference](/rest/api/azure/)
- [Create custom roles for Azure resources using the REST API](custom-roles-rest.md)
