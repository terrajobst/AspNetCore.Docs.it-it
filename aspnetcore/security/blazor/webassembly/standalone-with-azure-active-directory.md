---
title: Proteggere un'app ASP.NET Core Blazor webassembly autonomamente con Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: 76bcac29d86a236e56c0eaea24a694c4845ecbcf
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083748"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a>Proteggere un'app ASP.NET Core Blazor webassembly autonomamente con Azure Active Directory

Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Per creare un'app Blazor webassembly autonoma che usa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) per l'autenticazione:

[Creare un'applicazione Web e un tenant di AAD](/azure/active-directory/develop/v2-overview):

Registrare un'app AAD nell'area **Azure Active Directory** > **registrazioni app** del portale di Azure:

1. Specificare un **nome** per l'app (ad esempio, **Blazor AAD client**).
1. Scegliere i **tipi di conto supportati**. È possibile selezionare gli **account in questa directory organizzativa solo** per questa esperienza.
1. Lasciare l'elenco a discesa **URI di reindirizzamento** impostato su **Web**e specificare un uri di reindirizzamento di `https://localhost:5001/authentication/login-callback`.
1. Disabilitare le **autorizzazioni** > la casella **di controllo Concedi a OpenID e offline_access le autorizzazioni** .
1. Selezionare **Registra**.

In **Authentication** > **configurazioni della piattaforma** > **Web**:

1. Verificare che sia presente l' **URI di reindirizzamento** del `https://localhost:5001/authentication/login-callback`.
1. Per **concessione implicita**, selezionare le caselle di controllo per i token di **accesso** e i **token ID**.
1. Per questa esperienza sono accettabili le impostazioni predefinite rimanenti per l'app.
1. Selezionare il pulsante **Salva**.

Registrare le seguenti informazioni:

* ID applicazione (ID client) (ad esempio, `11111111-1111-1111-1111-111111111111`)
* ID directory (ID tenant) (ad esempio, `22222222-2222-2222-2222-222222222222`)

Sostituire i segnaposto nel comando seguente con le informazioni registrate in precedenza ed eseguire il comando in una shell dei comandi:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`. Il nome della cartella diventa anche parte del nome del progetto.

## <a name="authentication-package"></a>Pacchetto di autenticazione

Quando viene creata un'app per usare gli account aziendali o dell'Istituto di istruzione (`SingleOrg`), l'app riceve automaticamente un riferimento al pacchetto per [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.

Se si aggiunge l'autenticazione a un'app, aggiungere manualmente il pacchetto al file di progetto dell'app:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Sostituire `{VERSION}` nel riferimento al pacchetto precedente con la versione del pacchetto `Microsoft.AspNetCore.Blazor.Templates` illustrato nell'articolo <xref:blazor/get-started>.

Il pacchetto di `Microsoft.Authentication.WebAssembly.Msal` aggiunge in modo transitivo il pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication` all'app.

## <a name="authentication-service-support"></a>Supporto del servizio di autenticazione

Il supporto per l'autenticazione degli utenti viene registrato nel contenitore del servizio con il metodo di estensione `AddMsalAuthentication` fornito dal pacchetto di `Microsoft.Authentication.WebAssembly.Msal`. Questo metodo configura tutti i servizi necessari per l'interazione dell'app con il provider di identità (IP).

*Program.cs*:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

Il metodo `AddMsalAuthentication` accetta un callback per configurare i parametri necessari per autenticare un'app. Quando si registra l'app, è possibile ottenere i valori necessari per la configurazione dell'app dalla configurazione di AAD del portale di Azure.

Il modello di Blazor webassembly non configura automaticamente l'app per richiedere un token di accesso per un'API protetta. Per eseguire il provisioning di un token come parte del flusso di accesso, aggiungere l'ambito agli ambiti dei token di accesso predefiniti della `MsalProviderOptions`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> L'ambito del token di accesso predefinito deve essere nel formato `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`, ad esempio `11111111-1111-1111-1111-111111111111/API.Access`. Se per l'impostazione dell'ambito vengono forniti uno schema o uno schema e un host, come illustrato nel portale di Azure, l' *app client* genera un'eccezione non gestita quando riceve una risposta *401 non autorizzata* dall'app per le *API del server*.

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

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authentication/azure-active-directory/index>
