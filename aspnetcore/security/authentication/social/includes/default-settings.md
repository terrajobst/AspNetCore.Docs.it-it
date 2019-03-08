---
ms.openlocfilehash: 2fe12027e7a5233cf01e6c412f7ee536d479facd
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665779"
---
<!--Don't update this for 2.2, use the 2.2 version --> La chiamata a [AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) Configura le impostazioni di schema predefinito. Il [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) overload set il [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) proprietà. Il [AddAuthentication (azione&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) overload consente di configurare le opzioni di autenticazione, che possono essere utilizzate per impostare gli schemi di autenticazione predefinito per scopi diversi. Le chiamate successive a `AddAuthentication` override configurato in precedenza [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) proprietà.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) metodi di estensione che registra un gestore di autenticazione potrebbero lze volat pouze jednou per ogni schema di autenticazione. Sono disponibili overload che consentono di configurare le proprietà dello schema, nome dello schema e il nome visualizzato.
