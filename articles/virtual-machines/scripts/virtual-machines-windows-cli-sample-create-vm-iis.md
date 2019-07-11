---
title: 'Ejemplo de script de la CLI de Azure: instalación de IIS | Microsoft Docs'
description: 'Ejemplo de script de la CLI de Azure: instalación de IIS'
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/28/2017
ms.author: cynthn
ms.openlocfilehash: 6e9e54c164ed3112cddd7f92304eb44ee86abef6
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708260"
---
# <a name="quick-create-a-virtual-machine-with-the-azure-cli"></a>Creación rápida de una máquina virtual con la CLI de Azure

Este script crea una máquina virtual de Azure con Windows Server 2016 y, luego, usa la extensión de scripts personalizados para máquinas virtuales de Azure con el fin de instalar IIS. Una vez ejecutado el script, puedo acceder al sitio web IIS predeterminado en la dirección IP pública de la máquina virtual.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una máquina virtual y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Crea la máquina virtual y la conecta con la tarjeta de red, la red virtual, la subred y el grupo de seguridad de red. Este comando también especifica la imagen de máquina virtual que se usará, y las credenciales administrativas.  |
| [az vm open-port](https://docs.microsoft.com/cli/azure/network/nsg/rule) | Crea una regla de grupo de seguridad de red para permitir el tráfico entrante. En este ejemplo, el puerto 80 está abierto al tráfico HTTP. |
| [azure vm extension set](https://docs.microsoft.com/cli/azure/vm/extension) | Agrega una extensión de máquina virtual a una máquina virtual y la ejecuta. En este ejemplo, se utiliza la extensión de scripts personalizados para instalar IIS.|
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de la CLI de máquina virtual en la [documentación sobre máquinas virtuales Windows de Azure](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
