---
title: Proteggere un'app ASP.NET Core Blazor webassembly autonoma con la libreria di autenticazione
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: f9cc2884dcd94c729c45a056ae4327a2c75d34be
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083755"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a>Proteggere un'app ASP.NET Core Blazor webassembly autonoma con la libreria di autenticazione

Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Per creare un'app Blazor webassembly autonoma che usa `Microsoft.AspNetCore.Components.WebAssembly.Authentication` libreria, eseguire il comando seguente in una shell dei comandi:

```dotnetcli
dotnet new blazorwasm -au Individual
```

Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`. Il nome della cartella diventa anche parte del nome del progetto.

In Visual Studio [creare un'app webassembly Blazor](xref:blazor/get-started). Impostare l' **autenticazione** per **singoli account utente** con l'opzione **Archivia account utente in-app** .

## <a name="authentication-package"></a>Pacchetto di autenticazione

Quando si crea un'app per usare account utente singoli, l'app riceve automaticamente un riferimento al pacchetto per il pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication` nel file di progetto dell'app. Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.

Se si aggiunge l'autenticazione a un'app, aggiungere manualmente il pacchetto al file di progetto dell'app:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Sostituire `{VERSION}` nel riferimento al pacchetto precedente con la versione del pacchetto `Microsoft.AspNetCore.Blazor.Templates` illustrato nell'articolo <xref:blazor/get-started>.

## <a name="authentication-service-support"></a>Supporto del servizio di autenticazione

Il supporto per l'autenticazione degli utenti viene registrato nel contenitore del servizio con il metodo di estensione `AddOidcAuthentication` fornito dal pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. Questo metodo configura tutti i servizi necessari per l'interazione dell'app con il provider di identità (IP).

*Program.cs*:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

Il supporto per l'autenticazione per le app autonome è disponibile con Open ID Connect (OIDC). Il metodo `AddOidcAuthentication` accetta un callback per configurare i parametri necessari per autenticare un'app usando OIDC. I valori necessari per la configurazione dell'app possono essere ottenuti dall'IP, ad esempio Google, Microsoft o altri provider conformi a OIDC. Ottenere i valori quando si registra l'app, che in genere si verifica nel portale online.

## <a name="index-page"></a>Pagina di indice

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a>Componente app

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>Componente RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Componente LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Componente di autenticazione

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
