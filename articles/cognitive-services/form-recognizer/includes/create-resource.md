---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: a37087c90b6c9b3629402c7a8c2fa5861e46ae9a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592529"
---
Cuando se le conceda acceso para usar Form Recognizer, recibirá un correo electrónico de bienvenida con varios vínculos y recursos. Use el vínculo de "Azure Portal" de dicho mensaje para abrir Azure Portal y crear un recurso de Form Recognizer. En el panel **Crear**, proporcione la siguiente información:

|    |    |
|--|--|
| **Nombre** | Un nombre descriptivo para su recurso. Se recomienda usar un nombre descriptivo, como *MyNameFormRecognizer*. |
| **Suscripción** | Seleccione la suscripción de Azure a la que se le ha concedido acceso. |
| **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. |
| **Plan de tarifa** | El costo del recurso depende el plan de tarifa elegido y del uso. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Grupos de recursos** | [Grupo de recursos de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) que contendrá su recurso. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

> [!IMPORTANT]
> Normalmente, al crear un recurso de Cognitive Services en Azure Portal, tiene la opción de crear una clave de suscripción para varios servicios (que se usa en varias instancias de Cognitive Services) o una clave de suscripción para un solo servicio (que se usa solo con una instancia específica de Cognitive Services). Sin embargo, dado que Form Recognizer se encuentra en versión preliminar, no se incluye en la suscripción para varios servicios, y no puede crear la suscripción para un solo servicio a menos que use el vínculo proporcionado en el correo electrónico de bienvenida.

Cuando el recurso de Form Recognizer termine la implementación, búsquelo y selecciónelo en la lista **Todos los recursos** del portal. A continuación, seleccione la pestaña **Claves** para ver las claves de la suscripción. Cualquiera de las claves dará a la aplicación acceso al recurso. Copie el valor de **CLAVE 1**. La usará en la sección siguiente.