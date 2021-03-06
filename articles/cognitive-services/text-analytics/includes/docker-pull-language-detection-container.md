---
title: Docker pull para el contenedor de detección de idioma
titleSuffix: Azure Cognitive Services
description: Comando docker pull para el contenedor de detección de idioma
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 95bcb4b424010f63ac1ee4eb02f9e4793647051a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "90906056"
---
#### <a name="docker-pull-for-the-language-detection-container"></a>Docker pull para el contenedor de detección de idioma

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte el contenedor [Detección de idioma](https://go.microsoft.com/fwlink/?linkid=2018759) en Docker Hub.

```
docker pull mcr.microsoft.com/azure-cognitive-services/textanalytics/language:latest
```
