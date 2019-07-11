---
title: 'Inicio rápido: Reconocimiento de voz, Objective-C (Speech Service)'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en Objective-C para iOS mediante el SDK de Voz.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 55b442b74847ccbc0dcc944eada7b33ccd368bed
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605059"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en Objective-C para iOS mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, aprenderá a crear una aplicación iOS en Objective-C mediante el SDK de Voz de Cognitive Services para transcribir voz a texto desde el micrófono o desde un archivo con audio grabado.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, presentamos una lista de requisitos previos:

* Una [clave de suscripción](get-started.md) para Speech Service
* Una máquina macOS con [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) o posterior
* El destino establecido en la versión de iOS 9.3 o posterior

## <a name="get-the-speech-sdk-for-ios"></a>Obtención del SDK de Voz para iOS

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

La versión actual del SDK de Speech de Cognitive Services es `1.6.0`.

En la actualidad, el SDK de Voz de Cognitive Services para iOS se distribuye como marco de Cocoa.
Se puede descargar [aquí](https://aka.ms/csspeech/iosbinary). Descargue el archivo en el directorio principal.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode

Inicie Xcode y haga clic en **Archivo** > **Nuevo** > **Proyecto** para iniciar un proyecto nuevo.
En el cuadro de diálogo de selección de plantilla, elija la plantilla "Aplicación de vista única (iOS)".

En los cuadros de diálogo que aparecen después, realice las selecciones siguientes:

1. Cuadro de diálogo Opciones del proyecto
    1. Escriba un nombre para la aplicación de inicio rápido, por ejemplo `helloworld`.
    1. Escriba un nombre e identificador de la organización adecuados, si ya tiene una cuenta de desarrollador de Apple. Con fines de prueba, puede elegir cualquier nombre, como `testorg`. Para firmar la aplicación, necesita un perfil de aprovisionamiento adecuado. Consulte el [sitio para desarrolladores de Apple](https://developer.apple.com/) para más información.
    1. Asegúrese de elegir Objective-C como lenguaje para el proyecto.
    1. Desactive todas las casillas de las pruebas y los datos principales.
    ![Configuración del proyecto](media/sdk/qs-objectivec-project-settings.png)
1. Selección del directorio del proyecto
    1. Elija el directorio principal para incluir el proyecto. Se crea un directorio `helloworld` en el directorio principal que contiene todos los archivos del proyecto de Xcode.
    1. Deshabilite la creación de un repositorio de Git para este proyecto de ejemplo.
    1. Ajuste las rutas de acceso para el SDK en *Configuración del proyecto*.
        1. En la pestaña **General**, en el encabezado **Binarios incrustados**, agregue la biblioteca del SDK como un marco de trabajo: **Add embedded binaries (Agregar archivos binarios incrustado)s** > **Add other... (Agregar otro...)** > Vaya a su directorio y elija el archivo `MicrosoftCognitiveServicesSpeech.framework`. Como resultado, se agrega automáticamente la biblioteca del SDK al encabezado **Linked Framework and Libraries** (Marcos y bibliotecas vinculados).
        ![Marco de trabajo agregado](media/sdk/qs-objectivec-framework.png)
        1. Vaya a la pestaña **Configuración de compilación** y active la configuración **Todos**.
        1. Agregue el directorio `$(SRCROOT)/..` a *Rutas de búsqueda de marco* bajo el encabezado **Rutas de búsqueda**.
        ![Opción Ruta de búsqueda de marco](media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Configuración de la interfaz de usuario

La aplicación de ejemplo tendrá una interfaz de usuario muy simple: dos botones para iniciar el reconocimiento de voz desde un archivo o desde la entrada del micrófono y una etiqueta de texto para mostrar el resultado.
La interfaz de usuario se configura en la parte `Main.storyboard` del proyecto.
Para abrir la vista XML del guion gráfico, haga clic con el botón derecho en la entrada `Main.storyboard` del árbol del proyecto y seleccione **Abrir como...**  > **Código fuente**.
Reemplace el código XML generado automáticamente por este código:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Para descargar el [archivo wav de ejemplo](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) haga clic con el botón derecho en el vínculo y seleccione **Guardar destino como...** . Agregue el archivo wav al proyecto como un recurso arrastrándolo desde una ventana de Finder al nivel raíz de la vista del proyecto.
   Haga clic en **Finalizar** en el siguiente cuadro de diálogo sin cambiar la configuración.
1. Reemplace el contenido del archivo `ViewController.m` generado automáticamente por lo siguiente:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.
1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).
1. Agregue la solicitud de acceso de micrófono. Haga clic con el botón derecho en la entrada `Info.plist` del árbol del proyecto y seleccione **Open As...**  > **Código fuente**(Abrir como...> Código fuente). Agregue las líneas siguientes en la sección `<dict>` y, después, guarde el archivo.
    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="building-and-running-the-sample"></a>Compilación y ejecución del ejemplo

1. Haga visible la salida de depuración (**Ver** > **Área de depuración** > **Activar consola**).
1. Elija el simulador de iOS o un dispositivo iOS conectado a la máquina de desarrollo como destino para la aplicación en la lista del menú **Producto** > **Destino**.
1. Para compilar y ejecutar el código de ejemplo en el simulador de iOS seleccione **Producto** > **Ejecutar** en el menú o haga clic en el botón **Reproducir**.
1. Tras hacer clic en el botón "Recognize (File)" (Reconocer [Archivo]) de la aplicación, debería ver el contenido del archivo de audio "What's the weather like?" (¿Qué tiempo hace?). en la parte inferior de la pantalla.

   ![Aplicación de iOS simulada](media/sdk/qs-objectivec-simulated-app.png)

1. Después haga clic en el botón "Recognize (Microphone)" (Reconocer [Micrófono]) de la aplicación y diga algunas palabras; debería ver el texto que ha dicho en la parte inferior de la pantalla.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Objective-C en GitHub](https://aka.ms/csspeech/samples)
