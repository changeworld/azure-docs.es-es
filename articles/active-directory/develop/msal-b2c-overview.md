---
title: Uso de MSAL.js con Azure AD B2C
titleSuffix: Microsoft identity platform
description: La biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) permite que las aplicaciones trabajen con Azure AD B2C y adquieran tokens para llamar a API web protegidas. Estas API web pueden ser Microsoft Graph, otras API de Microsoft, API web de terceros o su propia API web.
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 06/05/2020
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev devx-track-js
ms.openlocfilehash: e11ed0d284e89a9e5f406aade2147b52d139f0ad
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/10/2021
ms.locfileid: "111953539"
---
# <a name="use-the-microsoft-authentication-library-for-javascript-to-work-with-azure-ad-b2c"></a>Uso de la biblioteca de autenticación de Microsoft para JavaScript a fin de trabajar con Azure AD B2C

La [biblioteca de autenticación de Microsoft para JavaScript (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js) permite que los desarrolladores de JavaScript autentiquen a los usuarios con identidades sociales y locales mediante [Azure Active Directory B2C](../../active-directory-b2c/overview.md) (Azure AD B2C).

Al usar Azure AD B2C como un servicio de administración de identidades, puede personalizar y controlar la manera en que los clientes se registran, inician sesión y administran sus perfiles al usar las aplicaciones. Azure AD B2C también permite personalizar la marca y la interfaz de usuario que muestra la aplicación durante el proceso de autenticación.

## <a name="supported-app-types-and-scenarios"></a>Escenarios y tipos de aplicaciones admitidos

MSAL.js permite que las [aplicaciones de página única](../../active-directory-b2c/application-types.md#single-page-applications) inicien la sesión de los usuarios con Azure AD B2C mediante el [flujo de código de autorización con concesión de PKCE](../../active-directory-b2c/authorization-code-flow.md). Con MSAL.js y Azure AD B2C:

- Los usuarios **pueden** autenticarse con sus identidades locales y sociales.
- Los usuarios **pueden** tener autorización para acceder a los recursos protegidos de Azure AD B2C (pero no a los recursos protegidos de Azure AD).
- Los usuarios **no pueden** obtener tokens para las API de Microsoft (por ejemplo, MS Graph API) mediante [permisos delegados](/azure/active-directory/develop/v2-permissions-and-consent#permission-types).
- Los usuarios con privilegios de administrador **pueden** obtener tokens para las API de Microsoft (por ejemplo, MS Graph API) mediante [permisos delegados](/azure/active-directory/develop/v2-permissions-and-consent#permission-types).

Para obtener más información, consulte [Trabajo con Azure AD B2C](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/working-with-b2c.md).

## <a name="next-steps"></a>Pasos siguientes

Siga el tutorial acerca de cómo:

- [Iniciar la sesión de los usuarios con Azure AD B2C en una aplicación de página única](../../active-directory-b2c/tutorial-single-page-app.md)
- [Llamar a una API web protegida de Azure AD B2C](../../active-directory-b2c/tutorial-single-page-app-webapi.md)