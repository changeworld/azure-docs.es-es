---
title: 'Encontrar identificadores de objeto de identidad para la autenticación: Azure API for FHIR'
description: En este artículo se explica cómo buscar los identificadores de objetos de identidad necesarios para configurar la autenticación de Azure API for FHIR
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: zxue
ms.openlocfilehash: d65aa4494c7ca5827added3d1159bc6838b2ac7b
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2021
ms.locfileid: "112288710"
---
# <a name="find-identity-object-ids-for-authentication-configuration"></a>Encontrar identificadores de objeto de identidad para la autenticación de autenticación

En este artículo se aprende a buscar los identificadores de objeto de identidad necesarios a la hora de configurar Azure API for FHIR para [usar un inquilino de Active Directory externo o secundario](configure-local-rbac.md) para el plano de datos.

## <a name="find-user-object-id"></a>Búsqueda de identificador de objeto de usuario

Si tiene un usuario con el nombre de usuario `myuser@contoso.com`, puede buscar el `ObjectId` de los usuarios con el siguiente comando de PowerShell:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@contoso.com'").ObjectId
```

O bien, puede usar la CLI de Azure:

```azurecli-interactive
az ad user show --id myuser@contoso.com --query objectId --out tsv
```

## <a name="find-service-principal-object-id"></a>Búsqueda de identificador de objeto de entidad de servicio

Supongamos que ha registrado una [aplicación cliente de servicio](register-service-azure-ad-client-app.md) y desea permitir que este cliente de servicio tenga acceso a Azure API for FHIR, puede encontrar el identificador de objeto para la entidad de servicio de cliente con el siguiente comando de PowerShell:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "AppId eq 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'").ObjectId
```

donde `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX` es el identificador de la aplicación cliente de servicio. Como alternativa, puede usar el `DisplayName` del cliente de servicio:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "DisplayName eq 'testapp'").ObjectId
```

Si usa la CLI de Azure, puede usar:

```azurecli-interactive
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX --query objectId --out tsv
```

## <a name="find-a-security-group-object-id"></a>Búsqueda de un identificador de objeto de grupo de seguridad

Si quiere buscar el identificador de objeto de un grupo de seguridad, puede usar el siguiente comando de PowerShell:

```azurepowershell-interactive
$(Get-AzureADGroup -Filter "DisplayName eq 'mygroup'").ObjectId
```
Donde `mygroup` es el nombre del grupo en el que está interesado.

Si usa la CLI de Azure, puede usar:

```azurecli-interactive
az ad group show --group "mygroup" --query objectId --out tsv
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido a buscar los identificadores de objeto de identidad necesarios a la hora de configurar Azure API for FHIR para usar un inquilino de Azure Active Directory externo o secundario. A continuación obtenga información sobre cómo usar los identificadores de objeto para establecer la configuración de RBAC local:
 
>[!div class="nextstepaction"]
>[Configuración de RBAC local](configure-local-rbac.md)
