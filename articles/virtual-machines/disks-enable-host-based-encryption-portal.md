---
title: Habilitación del cifrado de un extremo a otro mediante el cifrado en host con discos administrados - Azure Portal
description: Use el cifrado en el host para habilitar el cifrado de un extremo a otro en los discos administrados de Azure mediante Azure Portal.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 08/24/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: cdb22805e2e68893d3883272b66c2cfac13c807e
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "104721875"
---
# <a name="use-the-azure-portal-to-enable-end-to-end-encryption-using-encryption-at-host"></a>Use Azure Portal para habilitar el cifrado de un extremo a otro mediante el cifrado en host

Cuando se habilita el cifrado en el host, los datos almacenados en el host de máquina virtual se cifran en reposo y se transmiten cifrados al servido Storage. Para obtener información conceptual sobre el cifrado en el host, así como otros tipos de cifrado de disco administrado, vea:

* Linux: [cifrado en el host; cifrado de un extremo a otro de los datos de la máquina virtual](./disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data)

* Windows: [cifrado en el host; cifrado de un extremo a otro de los datos de la máquina virtual](./disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data)

## <a name="restrictions"></a>Restricciones

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]


### <a name="supported-vm-sizes"></a>Tamaños de máquinas virtuales que se admiten

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

## <a name="prerequisites"></a>Requisitos previos

Debe habilitar la característica para su suscripción antes de usar la propiedad EncryptionAtHost para su VM/VMSS. Siga los pasos que se indican a continuación para habilitar la característica para su suscripción:

1. **Portal de Azure**: Seleccione el icono de Cloud Shell en [Azure Portal](https://portal.azure.com):

    ![Icono para iniciar Cloud Shell desde Azure Portal](../Cloud-Shell/media/overview/portal-launch-icon.png)
    
2.  Ejecute el siguiente comando para registrar la característica para su suscripción

    ```powershell
     Register-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace "Microsoft.Compute" 
    ```

3.  Compruebe que el estado de registro es Registrado (tarda unos minutos) mediante el comando siguiente antes de probar la característica.

    ```powershell
     Get-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace "Microsoft.Compute"  
    ```


Inicie sesión en Azure Portal con el [vínculo proporcionado](https://aka.ms/diskencryptionupdates).

> [!IMPORTANT]
> Debe usar el [vínculo proporcionado](https://aka.ms/diskencryptionupdates) para tener acceso a Azure Portal. El cifrado en el host no está visible actualmente en Azure Portal público sin usar el vínculo.

### <a name="create-an-azure-key-vault-and-disk-encryption-set"></a>Creación de un almacén de Azure Key Vault y conjunto de cifrado de disco

Una vez habilitada la característica, deberá configurar un almacén de Azure Key Vault y un conjunto de cifrado de disco, si no lo ha hecho aún.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-portal](../../includes/virtual-machines-disks-encryption-create-key-vault-portal.md)]

## <a name="deploy-a-vm"></a>Implementación de una máquina virtual

Debe implementar una máquina virtual nueva para habilitar el cifrado en el host, no se puede habilitar en las máquinas virtuales existentes.

1. Busque **Máquinas virtuales** y seleccione **+Agregar** para crear una VM.
1. Cree una máquina virtual nueva, seleccione una región adecuada y un tamaño de máquina virtual compatible.
1. Rellene los otros valores de la hoja **Básico** como desee y, luego, vaya a la hoja **Discos**.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-basic-blade.png" alt-text="Captura de pantalla de la hoja Básico de la creación de máquinas virtuales, se resaltan la región y el tamaño de la máquina virtual.":::

1. En la hoja **Discos**, seleccione **Sí** para **Cifrado en el host**.
1. Realice las selecciones restantes como desee.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-disk-blade.png" alt-text="Captura de pantalla de la hoja Discos de la creación de máquinas virtuales, se resalta el cifrado en el host.":::

1. Finalice el proceso de implementación de la máquina virtual y realice las selecciones que se adapten a su entorno.

Ahora que ha implementado una máquina virtual con cifrado en el host habilitado, todos sus discos asociados se cifrarán con el cifrado en el host.

## <a name="next-steps"></a>Pasos siguientes

[Ejemplos de plantillas de Azure Resource Manager](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/EncryptionAtHost)
