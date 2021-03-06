---
title: Página de información general o panel principal de Azure Security Center
description: Obtenga información sobre las características de la página de información general de Security Center.
author: memildin
ms.author: memildin
ms.date: 03/04/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 768d69b626a870910d6734e1a1ddc29871f96afb
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102099784"
---
# <a name="azure-security-centers-overview-page"></a>Página de información general de Azure Security Center

Al abrir Azure Security Center, la primera página que aparece es la página de información general. 

Este panel interactivo proporciona una vista unificada de la posición de seguridad de las cargas de trabajo de la nube híbrida. Además, muestra alertas de seguridad, información de cobertura y mucho más.

Puede seleccionar cualquier elemento de la página para obtener información más detallada.

:::image type="content" source="media/overview-page/overview.png" alt-text="Página de información general de Security Center":::

## <a name="features-of-the-overview-page"></a>Características de la página de información general

:::image type="content" source="media/overview-page/top-bar-of-overview.png" alt-text="Barra superior de la página de información general de Security Center":::

La **barra de menús superior** ofrece:
- **Suscripciones**: puede ver y filtrar la lista de suscripciones seleccionando este botón. Security Center ajustará la pantalla para reflejar la postura de seguridad de las suscripciones seleccionadas.
- **Novedades**: abre las [notas de la versión](release-notes.md) para que pueda mantenerse al día de nuevas características, correcciones de errores y funcionalidad en desuso.
- **Números de alto nivel** para las cuentas de nube conectadas, para mostrar el contexto de la información en los iconos principales siguientes. También se muestran el número de recursos evaluados, las recomendaciones activas y las alertas de seguridad. Seleccione el número de recursos evaluados para acceder al [inventario de recursos](asset-inventory.md). Obtenga más información sobre la conexión de las [cuentas de AWS](quickstart-onboard-aws.md) y los [proyectos de GCP](quickstart-onboard-gcp.md).


En el centro de la página, hay **cuatro iconos centrales**, y cada uno de ellos está vinculado a un panel dedicado para obtener más información:
- **Puntuación de seguridad**: Security Center evalúa continuamente los recursos, las suscripciones y la organización en busca de problemas de seguridad. A continuación, agrega todos los resultados a una sola puntuación para que pueda conocer de un vistazo la situación de la seguridad actual: cuanto mayor sea la puntuación, menor será el nivel de riesgo identificado. [Más información](secure-score-security-controls.md).
- **Cumplimiento normativo**: Security Center proporciona información detallada acerca de su estado de cumplimiento basada en la evaluación continua de su entorno de Azure. Security Center analiza los factores de riesgo en su entorno de nube híbrida según los procedimientos recomendados de seguridad. Estas valoraciones se asignan a los controles de cumplimiento desde un conjunto de estándares compatible. [Más información](security-center-compliance-dashboard.md).
- **Azure Defender**: es la plataforma de protección de cargas de trabajo en la nube (CWPP) integrada en Security Center para la protección avanzada e inteligente de las cargas de trabajo híbridas y de Azure. El icono muestra la cobertura de los recursos conectados (para las suscripciones seleccionadas actualmente) y las alertas recientes, codificadas con colores según la gravedad. [Más información](azure-defender.md).
- **Firewall Manager**: este icono muestra el estado de los concentradores y redes desde [Azure Firewall Manager](../firewall-manager/overview.md). 


En el panel de **Insights** se ofrecen elementos personalizados para su entorno, incluidos los siguientes:
- Recursos más atacados
- Los controles de seguridad que tienen el máximo potencial de aumentar la puntuación de seguridad.
- Recomendaciones activas con la mayoría de los recursos afectados
- Entradas de blog recientes de expertos en Azure Security Center

## <a name="next-steps"></a>Pasos siguientes

En esta página se presentó la página de información general de Security Center. Para obtener información relacionada, consulte:

- [Explore y administre los recursos con las herramientas de administración e inventario de recursos.](asset-inventory.md)
- [Puntuación de seguridad de Azure Security Center](secure-score-security-controls.md)