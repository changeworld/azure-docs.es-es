---
title: Application Insights, C#
titleSuffix: Azure Cognitive Services
description: En este tutorial se agrega información de bot y Language Understanding al almacenamiento de datos de telemetría de Application Insights.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/16/2019
ms.author: diberry
ms.openlocfilehash: 720352403fd5f5937669f9838f3974cb0d3f8797
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657804"
---
# <a name="add-luis-results-to-application-insights-from-a-bot-in-c"></a>Adición de resultados de LUIS a Application Insights desde un bot en C#

En este tutorial se agrega información de bot y Language Understanding al almacenamiento de datos de telemetría de [Application Insights](https://azure.microsoft.com/services/application-insights/). Una vez que tengas los datos, puedes consultarlos con el lenguaje Kusto o Power BI para analizar y agregar intenciones y entidades de la expresión en tiempo real, así como para informar sobre ellas. Este análisis le ayuda a determinar si debe agregar o editar las intenciones y las entidades de la aplicación de LUIS.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Capturar datos de bot y Language Understanding en Application Insights
> * Consultar datos de Language Understanding en Application Insights

## <a name="prerequisites"></a>Requisitos previos

* Un bot de Azure Bot Service, creado con Application Insights habilitado.
* Código de bot descargado del **[tutorial](luis-csharp-tutorial-bf-v4.md)** de bot anterior. 
* [Bot Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

Todo el código de este tutorial está disponible en el [repositorio Azure-Samples de Language Understanding de GitHub](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-csharp-bot-johnsmith-src-telemetry). 

## <a name="add-application-insights-to-web-app-bot-project"></a>Agregar Application Insights a un proyecto de bot de aplicación web

En la actualidad, el servicio de Application Insights que se usa en este bot de aplicación web recopila telemetría de estado general para el bot. No recopila información sobre LUIS. 

Para capturar la información de LUIS, el bot de aplicación web necesita que el paquete NuGet **[Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** esté instalado y configurado.  

1. Desde Visual Studio, agrega la dependencia a la solución. En el **Explorador de soluciones**, haz clic con el botón derecho en el nombre del proyecto y selecciona **Administrar paquetes NuGet…** . En el Administrador de paquetes NuGet se muestra una lista de los paquetes instalados. 
1. Selecciona **Examinar** y, luego, busca **Microsoft.ApplicationInsights**.
1. Instala el paquete. 

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Captura y envío de los resultados de consulta de LUIS a Application Insights

1. Abre el archivo `LuisHelper.cs` y reemplaza el contenido por el código siguiente. El método **LogToApplicationInsights** captura los datos del bot y de LUIS y los envía a Application Insights como un evento de seguimiento denominado `LUIS`.

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.AI.Luis;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    using Microsoft.ApplicationInsights;
    using System.Collections.Generic;
    
    namespace Microsoft.BotBuilderSamples
    {
        public static class LuisHelper
        {
            public static async Task<BookingDetails> ExecuteLuisQuery(IConfiguration configuration, ILogger logger, ITurnContext turnContext, CancellationToken cancellationToken)
            {
                var bookingDetails = new BookingDetails();
    
                try
                {
                    // Create the LUIS settings from configuration.
                    var luisApplication = new LuisApplication(
                        configuration["LuisAppId"],
                        configuration["LuisAPIKey"],
                        "https://" + configuration["LuisAPIHostName"]
                    );
    
                    var recognizer = new LuisRecognizer(luisApplication);
    
                    // The actual call to LUIS
                    var recognizerResult = await recognizer.RecognizeAsync(turnContext, cancellationToken);
    
                    LuisHelper.LogToApplicationInsights(configuration, turnContext, recognizerResult);
    
                    var (intent, score) = recognizerResult.GetTopScoringIntent();
                    if (intent == "Book_flight")
                    {
                        // We need to get the result from the LUIS JSON which at every level returns an array.
                        bookingDetails.Destination = recognizerResult.Entities["To"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
                        bookingDetails.Origin = recognizerResult.Entities["From"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
    
                        // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                        // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                        bookingDetails.TravelDate = recognizerResult.Entities["datetime"]?.FirstOrDefault()?["timex"]?.FirstOrDefault()?.ToString().Split('T')[0];
                    }
                }
                catch (Exception e)
                {
                    logger.LogWarning($"LUIS Exception: {e.Message} Check your LUIS configuration.");
                }
    
                return bookingDetails;
            }
            public static void LogToApplicationInsights(IConfiguration configuration, ITurnContext turnContext, RecognizerResult result)
            {
                // Create Application Insights object
                TelemetryClient telemetry = new TelemetryClient();
    
                // Set Application Insights Instrumentation Key from App Settings
                telemetry.InstrumentationKey = configuration["BotDevAppInsightsKey"];
    
                // Collect information to send to Application Insights
                Dictionary<string, string> logProperties = new Dictionary<string, string>();
    
                logProperties.Add("BotConversation", turnContext.Activity.Conversation.Name);
                logProperties.Add("Bot_userId", turnContext.Activity.Conversation.Id);
    
                logProperties.Add("LUIS_query", result.Text);
                logProperties.Add("LUIS_topScoringIntent_Name", result.GetTopScoringIntent().intent);
                logProperties.Add("LUIS_topScoringIntentScore", result.GetTopScoringIntent().score.ToString());
    
    
                // Add entities to collected information
                int i = 1;
                if (result.Entities.Count > 0)
                {
                    foreach (var item in result.Entities)
                    {
                        logProperties.Add("LUIS_entities_" + i++ + "_" + item.Key, item.Value.ToString());
                    }
                }
    
                // Send to Application Insights
                telemetry.TrackTrace("LUIS", ApplicationInsights.DataContracts.SeverityLevel.Information, logProperties);
    
            }
        }
    }
    ```

## <a name="add-application-insights-instrumentation-key"></a>Agregar la clave de instrumentación de Application Insights 

Para agregar datos a Application Insights, necesitas la clave de instrumentación.

1. En un explorador, en [Azure Portal](https://portal.azure.com), busca el recurso**Application Insights** del bot. El nombre tendrá la mayor parte del nombre del bot, luego caracteres aleatorios al final del nombre, como `luis-csharp-bot-johnsmithxqowom`. 
1. En el recurso de Application Insights, en la página **Información general**, copia la **Clave de instrumentación**.
1. En Visual Studio, abre el archivo **appsettings.json** en la raíz del proyecto de bot. En este archivo se encuentran todas las variables de entorno.
1. Agrega una nueva variable `BotDevAppInsightsKey` con el valor de la clave de instrumentación. El valor debe estar entre comillas. 

## <a name="build-and-start-the-bot"></a>Compilar e iniciar el bot

1. En Visual Studio, compila y ejecuta el bot. 
1. Inicia el emulador de bots y abre el bot. Este [paso](luis-csharp-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) se proporciona en el tutorial anterior.

1. Haz una pregunte al bot. Este [paso](luis-csharp-tutorial-bf-v4.md#ask-bot-a-question-for-the-book-flight-intent) se proporciona en el tutorial anterior.

## <a name="view-luis-entries-in-application-insights"></a>Visualización de las entradas de LUIS en Application Insights

Abra Application Insights para ver las entradas de LUIS. Los datos pueden tardar unos minutos en aparecer en Application Insights.

1. En [Azure Portal](https://portal.azure.com), abre el recurso Application Insights del bot. 
1. Cuando se abra el recurso, selecciona **Buscar** y busca todos los datos en los últimos **30 minutos** con el tipo de evento **Seguimiento**. Selecciona el seguimiento denominado **LUIS**. 
1. La información del bot y de LUIS está disponible en **Propiedades personalizadas**. 

    ![Revisar las propiedades personalizadas de LUIS almacenadas en Application Insights](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-csharp.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Consultar en Application Insights de la intención, puntuación y expresión
Application Insights ofrece la capacidad de consultar los datos con el lenguaje [Kusto](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview#what-language-do-log-queries-use), así como exportarlos a [Power BI](https://powerbi.microsoft.com). 

1. Selecciona **Log (Analytics)** (Registro [análisis]). Se abre una ventana nueva con una ventana de consulta en la parte superior y una ventana de tabla de datos por debajo. Si ha usado bases de datos antes, esta disposición le resultará familiar. La consulta representa los datos filtrados anteriores. La columna **CustomDimensions** tiene la información del bot y de LUIS.
1. Para extraer la intención, puntuación y expresión superiores, agregue lo siguiente justo encima de la última línea (la línea `|top...`) en la ventana de consulta:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent_Name)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

1. Ejecute la consulta. Las nuevas columnas de intención superior, puntuación y expresión están disponibles. Selecciona la columna topIntent para ordenar.

Obtén más información sobre el [lenguaje de consultas Kusto](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) o [la exportación de los datos a Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Más información sobre Bot Framework

Obtenga más información sobre [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Pasos siguientes

Otra información que es posible que quiera agregar a los datos de Application Insights incluye el id. de la aplicación, el Id. de versión, la última fecha de cambio del modelo, la última fecha de entrenamiento y la última fecha de publicación. Estos valores se pueden obtener de la dirección URL del punto de conexión (los identificadores de la aplicación y la versión), o bien de una llamada a la API de creación, que después se establecen en la configuración del bot de aplicación web y se extraen desde allí.  

Si usa la misma suscripción de punto de conexión para más de una aplicación de LUIS, también debe incluir el identificador de suscripción y una propiedad que indique que es una clave compartida.

> [!div class="nextstepaction"]
> [Más información sobre las expresiones de ejemplo](luis-how-to-add-example-utterances.md)
