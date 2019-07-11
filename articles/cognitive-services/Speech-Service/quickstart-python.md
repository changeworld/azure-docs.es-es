---
title: 'Inicio rápido: Reconocimiento de voz, Python (servicios de Voz)'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de voz a texto que use el SDK de Voz para Python. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: d6b7cc275fc50fefbe0057620d315d1484c47745
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603009"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-python"></a>Inicio rápido: Reconocimiento de voz con el SDK de Voz para Python

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo se muestra cómo usar los servicios de voz mediante el SDK de voz de Python. Se ilustra cómo reconocer la voz por la entrada del micrófono.

## <a name="prerequisites"></a>Requisitos previos

* Una clave de suscripción de Azure para los servicios de voz. [Obtenga una gratis](get-started.md).
* [Python 3.5 o versiones posteriores](https://www.python.org/downloads/).
* El paquete del SDK de Voz de Python está disponible para estos sistemas operativos:
    * Windows: x64 y x86.
    * Mac: macOS X versión 10.12 o posterior.
    * Linux: Ubuntu 16.04, Ubuntu 18.04, Debian 9 en x64.
* En Linux, ejecute estos comandos para instalar los paquetes necesarios:

  * En Ubuntu:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.0 libasound2
    ```

  * En Debian 9:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.2 libasound2
    ```

* En Windows, necesita [Microsoft Visual C++ Redistributable para Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) para su plataforma.

## <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Este comando instala el paquete de Python desde [PyPI](https://pypi.org/) para el SDK de Voz:

```sh
pip install azure-cognitiveservices-speech
```

## <a name="support-and-updates"></a>Soporte técnico y actualizaciones

Las actualizaciones del paquete de Python del SDK de Voz se distribuirán mediante PyPI y se anunciarán en la página [Notas de la versión](./releasenotes.md).
Si hay disponible una nueva versión, puede actualizarse a ella con el comando `pip install --upgrade azure-cognitiveservices-speech`.
Para comprobar qué versión está instalada actualmente, inspeccione la variable `azure.cognitiveservices.speech.__version__`.

Si tiene un problema o falta una característica, consulte las [opciones de ayuda y soporte técnico](./support.md).

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Creación de una aplicación de Python mediante el SDK de Voz

### <a name="run-the-sample"></a>Ejecución del ejemplo

Puede copiar el [código de ejemplo](#sample-code) de este inicio rápido en un archivo de código fuente `quickstart.py` y ejecutarlo en el IDE o en la consola:

```sh
python quickstart.py
```

También, puede descargar este tutorial de inicio rápido como un cuaderno de [Jupyter](https://jupyter.org) del [repositorio de ejemplos del SDK de Voz](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) y ejecutarlo como un cuaderno.

### <a name="sample-code"></a>Código de ejemplo

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Instalación y uso del SDK de Voz con Visual Studio Code

1. Descargue e instale una versión de 64 bits (3.5 o posterior) de [Python](https://www.python.org/downloads/) en el equipo.
1. Descargue e instale [Visual Studio Code](https://code.visualstudio.com/Download).
1. Abra Visual Studio Code e instale la extensión de Python. Seleccione **File** > **Preferences** > **Extensions** (Archivo > Preferencias > Extensiones) en el menú. Busque **Python**.

   ![Instalación de la extensión de Python](media/sdk/qs-python-vscode-python-extension.png)

1. Cree una carpeta en la que almacenar el proyecto. Por ejemplo, puede usar para ello el Explorador de Windows.
1. En Visual Studio Code, seleccione el icono de **File** (Archivo). A continuación, abra la carpeta que creó.

   ![Abrir una carpeta](media/sdk/qs-python-vscode-python-open-folder.png)

1. Cree un archivo de código fuente de Python `speechsdk.py` mediante la selección del icono de nuevo archivo.

   ![Creación de un archivo](media/sdk/qs-python-vscode-python-newfile.png)

1. Copie, pegue y guarde el [código de Python](#sample-code) en el archivo recién creado.
1. Inserte la información de la suscripción de los servicios de voz.
1. Si se selecciona, se muestra un intérprete de Python en el lado izquierdo de la barra de estado en la parte inferior de la ventana.
   En caso contrario, aparecerá una lista de los intérpretes de Python disponibles. Abra la paleta de comandos (Ctrl+Mayús+P) y escriba **Python: Select Interpreter** (Seleccionar intérprete). Elija un valor apropiado.
1. Puede instalar el paquete de Python del SDK de Voz desde dentro de Visual Studio Code. Hágalo si no está instalado aún para el intérprete de Python seleccionado.
   Para instalar el paquete del SDK de Voz, abra un terminal. Abra de nuevo la paleta de comandos (Ctrl+Mayús+P) y escriba **Terminal: Create New Integrated Terminal** (Crear terminal integrado).
   En el terminal que se abre, escriba el comando `python -m pip install azure-cognitiveservices-speech` o el que sea apropiado para su sistema.
1. Para ejecutar el código de ejemplo, haga clic con el botón derecho en algún lugar dentro del editor. Seleccione **Run Python File in Terminal** (Ejecutar archivo de Python en terminal).
   Diga algunas palabras cuando se le pida. El texto transcrito se muestra poco después.

   ![Ejecución de un ejemplo](media/sdk/qs-python-vscode-python-run.png)

Si tiene problemas para seguir estas instrucciones, consulte el [tutorial de Python para Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial) con información más amplia.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de los ejemplos de Python en GitHub](https://aka.ms/csspeech/samples)
