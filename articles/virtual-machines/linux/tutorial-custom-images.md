---
title: 'Tutorial: Creación de imágenes de máquina virtual personalizadas con la CLI de Azure | Microsoft Docs'
description: En este tutorial, aprenderá a usar la CLI de Azure para crear una imagen de máquina virtual personalizada en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 080d1d68c2f2fb757bb51471b91a807b2f00fa2d
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709520"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-the-azure-cli"></a>Tutorial: Creación de una imagen personalizada de una máquina virtual de Azure con la CLI de Azure

Las imágenes personalizadas son como las imágenes de Marketplace, pero las puede crear usted mismo. Las imágenes personalizadas pueden usarse para configuraciones de arranque como la carga previa de aplicaciones, configuraciones de aplicaciones y otras configuraciones del sistema operativo. En este tutorial, creará su propia imagen personalizada de una máquina virtual de Azure. Aprenderá a:

> [!div class="checklist"]
> * Desaprovisionar y generalizar máquinas virtuales
> * Crear una imagen personalizada
> * Crear una imagen personalizada a partir de una máquina virtual
> * Enumerar todas las imágenes en su suscripción
> * Eliminar una imagen

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, en este tutorial es preciso que ejecute la CLI de Azure de la versión 2.0.30, u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="before-you-begin"></a>Antes de empezar

Los siguientes pasos explican cómo tomar una máquina virtual existente y convertirla en una imagen personalizada reutilizable que puede usar para crear nuevas instancias de máquinas virtuales.

Para completar el ejemplo de este tutorial, debe tener una máquina virtual. Si es necesario, este [script de ejemplo](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) puede crear una. Al trabajar en el tutorial, reemplace los nombres de grupo de recursos y máquina virtual cuando proceda.

## <a name="create-a-custom-image"></a>Crear una imagen personalizada

Para crear una imagen de una máquina virtual, debe preparar la máquina virtual, desaprovisionando, desasignando y marcando después la máquina virtual de origen como generalizada. Una vez que se ha preparado la máquina virtual, puede crear una imagen.

### <a name="deprovision-the-vm"></a>Desaprovisionar la máquina virtual 

El desaprovisionamiento generaliza la máquina virtual mediante la eliminación de información específica de la máquina. Esta generalización hace posible implementar muchas máquinas virtuales a partir de una sola imagen. Durante el desaprovisionamiento, el nombre de host se restablece a *localhost.localdomain*. También se eliminan las claves de host SSH, las configuraciones de servidor de nombres, la contraseña raíz y las concesiones DHCP almacenadas en caché.

Para desaprovisionar la máquina virtual, use el agente de máquina virtual de Azure (waagent). El agente de máquina virtual de Azure está instalado en la máquina virtual y administra el aprovisionamiento y la interacción con el controlador de tejido de Azure. Consulte la [Guía de usuario del Agente de Linux de Azure](../extensions/agent-linux.md) para más información.

Conéctese a la máquina virtual mediante SSH y ejecute el comando para desaprovisionar la máquina virtual. Con el argumento `+user` también se elimina la última cuenta de usuario aprovisionada y los datos asociados. Reemplace la dirección IP de ejemplo con la dirección IP pública de la máquina virtual.

SSH a la máquina virtual
```bash
ssh azureuser@52.174.34.95
```
Desaprovisione la máquina virtual.

```bash
sudo waagent -deprovision+user -force
```
Cierre la sesión SSH.

```bash
exit
```

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Desasignar y marcar la máquina virtual como generalizada

Para crear una imagen, es necesario desasignar la máquina virtual. Desasigne la máquina virtual mediante [az vm deallocate](/cli//azure/vm). 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

Por último, establezca el estado de la máquina virtual como generalizado con [az vm generalize](/cli//azure/vm) para que la plataforma Azure lo sepa. Solo se puede crear una imagen de una máquina virtual generalizada.
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-the-image"></a>Crear la imagen

Ahora puede crear una imagen de la máquina virtual con [az image create](/cli//azure/image). En el ejemplo siguiente se crea una imagen denominada *myImage* a partir de la máquina virtual llamada *myVM*.
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a>Creación de máquinas virtuales a partir de la imagen

Ahora que tiene una imagen, puede crear una o varias máquinas virtuales de la imagen mediante [az vm create](/cli/azure/vm). En el ejemplo siguiente se crea una máquina virtual denominada *myVMfromImage* a partir de la máquina virtual llamada *myImage*.

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>Administración de imágenes 

Estos son algunos ejemplos de tareas de administración de imágenes comunes y cómo realizarlas mediante la CLI de Azure.

Una lista de todas las imágenes por nombre en un formato de tabla.

```azurecli-interactive 
az image list \
    --resource-group myResourceGroup
```

Elimine una imagen. Este ejemplo elimina la imagen con el nombre *myOldImage* de *myResourceGroup*.

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado una imagen de máquina virtual personalizada. Ha aprendido a:

> [!div class="checklist"]
> * Desaprovisionar y generalizar máquinas virtuales
> * Crear una imagen personalizada
> * Crear una imagen personalizada a partir de una máquina virtual
> * Enumerar todas las imágenes en su suscripción
> * Eliminar una imagen

Avance al siguiente tutorial para obtener información sobre máquinas virtuales de alta disponibilidad.

> [!div class="nextstepaction"]
> [Creación de máquinas virtuales de alta disponibilidad](tutorial-availability-sets.md).

