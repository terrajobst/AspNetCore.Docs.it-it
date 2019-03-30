---
title: Configurazione accesso esterno dell'Account Microsoft con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione di Microsoft account utente in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1733d049d6752c24d7749b5b5ae2a4b866492358
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751001"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Configurazione accesso esterno dell'Account Microsoft con ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra come consentire agli utenti di accedere con il proprio account Microsoft usando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Creare l'app nel portale per sviluppatori Microsoft

* Passare a [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) e creare o accedere a un account Microsoft:

![L'accesso nella finestra di dialogo](index/_static/MicrosoftDevLogin.png)

Se non si ha già un account Microsoft, toccare  **[crearne uno.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Dopo l'accesso si verrà reindirizzati alla **mie applicazioni** pagina:

![Portale per sviluppatori Microsoft Apri in Microsoft Edge](index/_static/MicrosoftDev.png)

* Toccare **aggiungere un'app** in alto a destra nell'angolo e immettere il **nome applicazione** e **Contact Email**:

![Finestra di dialogo Nuova registrazione dell'applicazione](index/_static/MicrosoftDevAppCreate.png)

* Ai fini di questa esercitazione, cancellare il **installazione guidata** casella di controllo.

* Toccare **Create** per continuare al **registrazione** pagina. Fornire un **nome** e prendere nota del valore della **Id applicazione**, che è usato come `ClientId` più avanti nell'esercitazione:

![Pagina di registrazione](index/_static/MicrosoftDevAppReg.png)

* Toccare **Aggiungi piattaforma** nel **piattaforme** sezione e selezionare il **Web** piattaforma:

![Aggiungi finestra di dialogo piattaforma](index/_static/MicrosoftDevAppPlatform.png)

* Nel nuovo **Web** piattaforma, quindi immettere l'URL di sviluppo con `/signin-microsoft` aggiunto nel **URL di reindirizzamento** campo (ad esempio: `https://localhost:44320/signin-microsoft`). Lo schema di autenticazione Microsoft configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste a `/signin-microsoft` route per implementare il flusso di OAuth:

![Sezione relativa alla piattaforma Web](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> Il segmento URI `/signin-microsoft` viene impostato come il callback predefinito del provider di autenticazione Microsoft. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Microsoft tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà del [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) classe.

* Toccare **Aggiungi URL** per assicurarsi che l'URL è stato aggiunto.

* Compilare le altre impostazioni dell'applicazione se necessario e toccare **salvare** nella parte inferiore della pagina per salvare le modifiche alla configurazione delle app.

* Quando si distribuisce il sito è necessario rivedere le **registrazione** pagina e impostare un nuovo URL pubblico.

## <a name="store-microsoft-application-id-and-password"></a>Store Id applicazione di Microsoft e la Password

* Si noti il `Application Id` visualizzato nella **registrazione** pagina.

* Toccare **Genera nuova Password** nel **i segreti dell'applicazione** sezione. Verrà visualizzata una finestra in cui è possibile copiare la password dell'applicazione:

![Finestra di dialogo nuova password generata](index/_static/MicrosoftDevPassword.png)

Collegare le impostazioni sensibili, ad esempio Microsoft `Application ID` e `Password` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets). Ai fini di questa esercitazione, denominare il token `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Configurare l'autenticazione Account Microsoft

Il modello di progetto usato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pacchetto è già installato.

* Per installare questo pacchetto con Visual Studio 2017, pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.
* Per installare con CLI di .NET Core, eseguire il codice seguente nella directory del progetto:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

Aggiungere il servizio Account Microsoft nel `ConfigureServices` nel metodo *Startup.cs* file:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aggiungere il middleware di Account Microsoft nel `Configure` nel metodo *Startup.cs* file:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

Sebbene la terminologia utilizzata nel portale per sviluppatori Microsoft assegna questi token `ApplicationId` e `Password`, è esposta come `ClientId` e `ClientSecret` nell'API di configurazione.

Vedere le [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Account Microsoft. Ciò può essere usato per richiedere informazioni diverse sull'utente.

## <a name="sign-in-with-microsoft-account"></a>Accedi con Account Microsoft

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Microsoft:

![Applicazione del Log nella pagina Web: Utente non autenticato](index/_static/DoneMicrosoft.png)

Quando fa clic su Microsoft, si verrà reindirizzati a Microsoft per l'autenticazione. Dopo l'accesso con l'Account Microsoft (se non è già stato effettuato l'accesso) verrà richiesto per consentire all'app di accedere alle tue info:

![Finestra di dialogo autenticazione Microsoft](index/_static/MicrosoftLogin.png)

Toccare **Sì** e si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.

A questo punto si è connessi con le credenziali di Microsoft:

![Applicazione Web: Autenticata utente](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

* Se il provider dell'Account di Microsoft si viene reindirizzati a una pagina di errore di accesso, si noti l'errore title e description parametri stringa di query che seguono direttamente il `#` (hashtag) nell'Uri.

  Anche se sembra che il messaggio di errore per indicare un problema con l'autenticazione di Microsoft, la causa più comune è l'Uri non corrisponda a uno di applicazione la **Redirect URIs** specificato per il **Web** piattaforma .
* **ASP.NET Core 2.x solo:** Se Identity non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: È necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Microsoft. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito web all'app web di Azure, è necessario creare un nuovo `Password` del portale per sviluppatori Microsoft.

* Impostare il `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.
