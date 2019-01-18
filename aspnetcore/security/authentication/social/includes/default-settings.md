---
ms.openlocfilehash: a7be92adffc06ac0f25b84ea15d1a8c4896f4f9d
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54397077"
---
<!--Don't update this for 2.2, use the 2.2 version --> La chiamata a [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) Configura le impostazioni di schema predefinito. Il [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) overload set il [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) proprietà. Il [AddAuthentication (azione&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) overload consente di configurare le opzioni di autenticazione, che possono essere utilizzate per impostare gli schemi di autenticazione predefinito per scopi diversi. Le chiamate successive a `AddAuthentication` override configurato in precedenza [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) proprietà.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) metodi di estensione che registra un gestore di autenticazione potrebbero lze volat pouze jednou per ogni schema di autenticazione. Sono disponibili overload che consentono di configurare le proprietà dello schema, nome dello schema e il nome visualizzato.
