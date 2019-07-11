---
title: 'Ejemplo de script de la CLI de Azure: cifrado de una máquina virtual Linux | Microsoft Docs'
description: 'Ejemplo de script de la CLI de Azure: cifrado de una máquina virtual Linux'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 16bbd4031c851a950af0f3c0fe98ebdd24b183df
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709431"
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>Cifrado de una máquina virtual Linux en Azure

Este script crea un almacén Azure Key Vault seguro, claves de cifrado, una entidad de servicio de Azure Active Directory y una máquina virtual (VM) Linux. La máquina virtual, a continuación, se cifra mediante la clave de cifrado del almacén de claves y las credenciales de la entidad de servicio.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, un almacén Azure Key Vault, una entidad de servicio, una máquina virtual y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault) | Crea un almacén Azure Key Vault para almacenar los datos seguros, como las claves de cifrado. |
| [az keyvault key create](https://docs.microsoft.com/cli/azure/keyvault/key) | Crea una clave de cifrado en el almacén Key Vault. |
| [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp) | Crea una entidad de servicio de Azure Active Directory para autenticar y controlar el acceso a las claves de cifrado de forma segura. |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault) | Establece permisos en el almacén Key Vault para conceder acceso a la entidad de servicio a las claves de cifrado. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Crea la máquina virtual y la conecta con la tarjeta de red, la red virtual, la subred y el NSG. Este comando también especifica la imagen de máquina virtual que se usará, y las credenciales administrativas.  |
| [az vm encryption enable](https://docs.microsoft.com/cli/azure/vm/encryption) | Habilita el cifrado en una máquina virtual usando las credenciales de la entidad de servicio y la clave de cifrado. |
| [az vm encryption show](https://docs.microsoft.com/cli/azure/vm/encryption) | Muestra el estado del proceso de cifrado de la máquina virtual. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de la CLI de máquina virtual en la [documentación sobre máquinas virtuales Linux de Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
