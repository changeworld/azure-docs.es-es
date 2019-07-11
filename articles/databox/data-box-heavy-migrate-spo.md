---
title: Uso de Azure Data Box Heavy para migrar el contenido del recurso compartido de archivos a SharePoint Online | Microsoft Docs
description: Uso de este tutorial para aprender a migrar el contenido del recurso compartido de archivos a SharePoint Online mediante Azure Data Box Heavy
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: d74539ec1de8f503b0d0e423adf6273d1422fed5
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592332"
---
# <a name="use-the-azure-data-box-heavy-to-migrate-your-file-share-content-to-sharepoint-online"></a>Uso de Azure Data Box Heavy para migrar el contenido del recurso compartido de archivos a SharePoint Online

Utilice Azure Data Box Heavy y la Herramienta de migración de SharePoint (SPMT) para migrar fácilmente el contenido del recurso compartido de archivos a SharePoint Online y OneDrive. Al utilizar Data Box Heavy, puede quitar la dependencia del vínculo de red de área extensa (WAN) para transferir los datos.

Microsoft Azure Data Box es un servicio que le permite solicitar un dispositivo desde Microsoft Azure Portal. Después puede copiar terabytes de datos desde los servidores al dispositivo. Después de enviarlo a Microsoft, los datos se copian en Azure. Según el tamaño de los datos que se van a transferir, puede elegir entre:

- [Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview) con 35 TB de capacidad utilizable por solicitud para los conjuntos de datos pequeños y medianos.
- [Data Box](https://docs.microsoft.com/azure/databox/data-box-overview) con 80 TB de capacidad utilizable por dispositivo para conjuntos de datos medianos y grandes.
- [Data Box Heavy](https://docs.microsoft.com/azure/databox/data-box-heavy-overview) con 770 TB de capacidad utilizable por dispositivo para grandes conjuntos de datos.

En este artículo se habla específicamente sobre cómo usar Data Box Heavy para migrar el contenido del recurso compartido de archivos a SharePoint Online.

## <a name="requirements-and-costs"></a>Requisitos y costos

### <a name="for-data-box-heavy"></a>Para Data Box Heavy

- Data Box Heavy solo está disponible para Contratos Enterprise (EA), el Proveedor de soluciones en la nube (CSP) o las ofertas de patrocinio de Azure. Si su suscripción no se encuentra entre ninguno de los tipos anteriores, póngase en contacto con el Soporte técnico de Microsoft para actualizar la suscripción o consulte los [precios de la suscripción de Azure](https://azure.microsoft.com/pricing/).
- Se cobra por usar Data Box Heavy. No se olvide de revisar el [precio de Data Box Heavy](https://azure.microsoft.com/pricing/details/databox/heavy/).


### <a name="for-sharepoint-online"></a>Para SharePoint Online

- Revise los [requisitos mínimos para la Herramienta de migración de SharePoint (SPMT)](https://docs.microsoft.com/sharepointmigration/how-to-use-the-sharepoint-migration-tool).

## <a name="workflow-overview"></a>Introducción al flujo de trabajo

Este flujo de trabajo requiere que realice pasos en Azure Data Box Heavy así como en SharePoint Online.
Los pasos siguientes se refieren a Azure Data Box Heavy.

1. Solicite Azure Data Box Heavy.
2. Reciba y configure el dispositivo.
3. Copie los datos del recurso compartido de archivos local en la carpeta de Azure Files del dispositivo.
4. Una vez completada la copia, devuelva el dispositivo según las instrucciones.
5. Espere a que los datos se carguen completamente en Azure.

Los pasos siguientes se refieren a SharePoint Online.

6. Cree una máquina virtual en Azure Portal y monte el recurso compartido de archivos de Azure en él.
7. Instale la herramienta SPMT en la máquina virtual de Azure.
8. Ejecute la herramienta SPMT con el recurso compartido de archivos de Azure como *origen*.
9. Complete los pasos finales de la herramienta.
10. Compruebe y confirme los datos.

## <a name="use-data-box-heavy-to-copy-data"></a>Uso de Data Box Heavy para copiar datos

Siga estos pasos para copiar datos a Data Box Heavy.

1. [Solicite su instancia de Data Box Heavy](data-box-heavy-deploy-ordered.md).
2. Después de recibir Data Box Heavy, [configúrelo](data-box-heavy-deploy-set-up.md). Deberá conectar los cables y configurar los nodos en el dispositivo.
3. [Copie los datos a Azure Data Box Heavy](data-box-heavy-deploy-copy-data.md). Al copiar, asegúrese de que:

    - Use solo la carpeta *AzureFile* en Data Box Heavy para copiar los datos. Esto se debe a que quiere que los datos terminen en un recurso compartido de archivos de Azure, no en blobs en bloques o en páginas.
    - Copie los archivos en una subcarpeta de la carpeta *AzureFile*. Una subcarpeta dentro de *AzureFile* crea un recurso compartido de archivos. Los archivos que se copien directamente en la carpeta *AzureFile* presentan errores y se cargan como blobs en bloques. Este es el archivo compartido que montará en la máquina virtual en el siguiente paso.
    - Copie los datos en ambos nodos de la instancia de Data Box Heavy.
3. Ejecute [Preparación para el envío](data-box-heavy-deploy-picked-up.md#prepare-to-ship) en el dispositivo. Una correcta preparación para el envío garantiza la carga correcta de los archivos en Azure.
4. [Devuelva el dispositivo](data-box-heavy-deploy-picked-up.md#ship-data-box-heavy-back).
5. [Compruebe la carga de datos en Azure](data-box-heavy-deploy-picked-up.md#verify-data-upload-to-azure).

## <a name="use-spmt-to-migrate-data"></a>Uso de SPMT para migrar datos

Después de recibir la confirmación del equipo de datos de Azure de que la copia de datos se ha completado, ahora puede proceder a migrar los datos a SharePoint Online.

Para optimizar el rendimiento y la conectividad, se recomienda crear una máquina virtual de Azure.

1. Inicie sesión en Azure Portal y, a continuación, [cree una máquina virtual](../virtual-machines/windows/quick-create-portal.md).
2. [Monte el recurso compartido de archivos de Azure en la máquina virtual](../storage/files/storage-how-to-use-files-windows.md#mount-the-azure-file-share-with-file-explorer).
3. [Descargue la Herramienta de migración de SharePoint](https://spmtreleasescus.blob.core.windows.net/install/default.htm) e instálela en la máquina virtual de Azure.
4. Inicie la Herramienta de migración de SharePoint. Haga clic en **Iniciar sesión** y escriba su nombre de usuario y la contraseña de Office 365.
5. Cuando se le pregunte **¿Dónde están los datos?** , seleccione **Recurso compartido de archivos**. Escriba la ruta de acceso al recurso compartido de archivos de Azure donde se encuentran los datos.
6. Siga las instrucciones siguientes de la forma habitual, incluida la ubicación de destino. Para más información, vaya a [Uso de la herramienta de migración de SharePoint](https://docs.microsoft.com/sharepointmigration/how-to-use-the-sharepoint-migration-tool).

> [!IMPORTANT]
> - La velocidad a la que los datos se ingieren en SharePoint Online se ve afectada por varios factores, independientemente de si ya tiene los datos en Azure. Entender estos factores le ayudará a planear y maximizar la eficacia de la migración.  Para más información, vaya a [Velocidad de migración de SharePoint Online y OneDrive](/sharepointmigration/sharepoint-online-and-onedrive-migration-speed).
> - Existe un riesgo de perder los permisos existentes en los archivos al migrar los datos a SharePoint Online. También puede perder algunos metadatos, como *Created by* (Creado por) y *Date modified by* (Fecha de modificación por).

## <a name="next-steps"></a>Pasos siguientes

[Solicitar la instancia de Data Box Heavy](./data-box-heavy-deploy-ordered.md)