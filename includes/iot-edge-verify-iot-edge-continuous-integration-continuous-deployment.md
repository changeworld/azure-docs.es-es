---
author: v-tcassi
ms.service: iot-edge
ms.topic: include
ms.date: 08/26/2020
ms.author: v-tcassi
ms.openlocfilehash: b5450e4846c3c49c89830ae65c50a95ee0c8d6eb
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104803526"
---
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Comprobación de CI/CD de IoT Edge con las canalizaciones de compilación y de versión

Para desencadenar un trabajo de compilación, puede insertar una confirmación en el repositorio de código fuente o desencadenarlo manualmente. En esta sección, desencadenará manualmente la canalización de CI/CD para probar que funcione. Luego verifique que la implementación se complete correctamente.

1. En el menú del panel izquierdo, seleccione **Canalizaciones** y abra la canalización de compilación que creó al principio de este artículo.

2. Para desencadenar un trabajo de compilación en la canalización de compilación, puede seleccionar el botón **Ejecutar canalización** de la parte superior derecha.

    ![Desencadenar manualmente la canalización de compilación mediante el botón Ejecutar canalización](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/manual-trigger.png)

3. Revise la configuración de **Ejecutar canalización**. Luego, seleccione **Ejecutar**.

    ![Especificar las opciones de ejecución de la canalización y seleccionar Ejecutar](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/run-pipeline-settings.png)

4. Seleccione **Trabajo de agente 1** para ver el progreso de la ejecución. Puede seleccionar el trabajo para revisar los registros de la salida del trabajo. 

    ![Revisar la salida del registro del trabajo](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/view-job-run.png)

5. Si la canalización de compilación se completa correctamente, se desencadena una versión en la fase **dev**. La versión **dev** correcta crea una implementación de IoT Edge en los dispositivos IoT Edge de destino.

    ![Versión de dev](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/pending-approval.png)

6. Haga clic en la fase **dev** para ver los registros de versión.

    ![Registros de versión](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/release-logs.png)

7. Si se ha generado un error en la canalización, empiece por ver los registros. Para ver los registros, vaya al resumen de ejecución de canalización y seleccione el trabajo y la tarea. Si se ha generado un error en una tarea determinada, compruebe los registros de esa tarea. Para obtener instrucciones detalladas sobre cómo configurar y usar registros, consulte [Revisión de registros para diagnosticar problemas de canalización](/azure/devops/pipelines/troubleshooting/review-logs).
