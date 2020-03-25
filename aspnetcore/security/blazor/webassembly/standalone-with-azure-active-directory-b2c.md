---
title: Proteggere un'app ASP.NET Core Blazor webassembly autonomamente con Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: bb03ef1e6d216cfc06e2b91919c64f92f2ef634e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219272"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a>Proteggere un'app ASP.NET Core Blazor webassembly autonomamente con Azure Active Directory B2C

Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Per creare un'app Blazor webassembly autonoma che usa [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) per l'autenticazione:

1. Per creare un tenant e registrare un'app Web nel portale di Azure, seguire le istruzioni riportate negli argomenti seguenti:

   * [Creare un tenant AAD B2C](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; registrare le informazioni seguenti:

     1 \. AAD B2C istanza (ad esempio, `https://contoso.b2clogin.com/`, che include la barra finale)<br>
     2 \. AAD B2C dominio tenant, ad esempio `contoso.onmicrosoft.com`

   * [Registrare un'applicazione web](/azure/active-directory-b2c/tutorial-register-applications) &ndash; effettuare le selezioni seguenti durante la registrazione dell'app:

     1 \. Impostare **app Web/API Web** su **Sì**.<br>
     2 \. Impostare **Consenti flusso implicito** su **Sì**.<br>
     3 \. Aggiungere un **URL di risposta** di `https://localhost:5001/authentication/login-callback`.

     Registrare l'ID applicazione (ID client), ad esempio `11111111-1111-1111-1111-111111111111`.

   * [Creare flussi utente](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; creare un flusso utente di iscrizione e accesso.

     Selezionare come minimo l'attributo **Application claims** > **Display Name** user per popolare il `context.User.Identity.Name` nel componente `LoginDisplay` (*Shared/LoginDisplay. Razor*).

     Registrare il nome del flusso utente di iscrizione e accesso creato per l'app, ad esempio `B2C_1_signupsignin`.

1. Sostituire i segnaposto nel comando seguente con le informazioni registrate in precedenza ed eseguire il comando in una shell dei comandi:

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`. Il nome della cartella diventa anche parte del nome del progetto.

## <a name="authentication-package"></a>Pacchetto di autenticazione

Quando viene creata un'app per usare un singolo account B2C (`IndividualB2C`), l'app riceve automaticamente un riferimento al pacchetto per [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.

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
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

Il metodo `AddMsalAuthentication` accetta un callback per configurare i parametri necessari per autenticare un'app. Quando si registra l'app, è possibile ottenere i valori necessari per la configurazione dell'app dalla configurazione di AAD del portale di Azure.

Il modello di Blazor webassembly non configura automaticamente l'app per richiedere un token di accesso per un'API protetta. Per eseguire il provisioning di un token come parte del flusso di accesso, aggiungere l'ambito agli ambiti dei token di accesso predefiniti della `MsalProviderOptions`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a>Pagina di indice

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

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

* <xref:security/authentication/azure-ad-b2c>
* [Esercitazione: Creare un tenant di Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant)
