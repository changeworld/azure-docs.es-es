---
title: 'Inicio rápido: Traducción de voz, C# (.NET Core Windows)'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, creará una sencilla aplicación de .NET Core para capturar la voz del usuario, traducirla a otro idioma y mostrar el texto en la línea de comandos. Esta guía está diseñada para usuarios de Windows.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 8ab76a786f11677f5d8f0715b257bd2fb04e50bf
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67602806"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-net-core"></a>Inicio rápido: Traducción de voz con Speech SDK para .NET Core

También hay inicios rápidos disponibles para la [conversión de voz en texto ](quickstart-csharp-dotnet-windows.md) y [de texto en voz](quickstart-text-to-speech-dotnetcore.md).

En este inicio rápido, creará una sencilla aplicación de .NET Core que captura la voz del usuario procedente del micrófono del equipo, traduce el contenido de voz y transcribe el texto traducido en la línea de comandos en tiempo real. La aplicación está diseñada para ejecutarse en Windows de 64 bits y se ha creado con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017.

Para obtener una lista completa de los idiomas disponibles para la traducción de voz, consulte la [compatibilidad con idiomas](language-support.md).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [SDK de .NET Core](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra `Program.cs` y reemplace todo el código que contiene por lo siguiente.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-dotnetcore/helloworld/Program.cs#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Además, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a su suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana de consola que le pide decir algo. Diga una oración o frase en inglés. Lo que diga se transmitirá al servicio Voz, se traducirá y se transcribirá como texto que aparecerá en la misma ventana.

    ![Captura de pantalla de la salida de la consola después de una traducción correcta](media/sdk/qs-translate-csharp-dotnetcore-windows-output.png "Console output after successful translation")


## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales como, por ejemplo, leer voz desde un archivo de audio y proporcionar una salida del texto traducido como voz sintetizada, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
