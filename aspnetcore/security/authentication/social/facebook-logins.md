---
title: Configurazione dell'accesso esterno Facebook in ASP.NET Core
author: rick-anderson
description: Esercitazione con esempi di codice che illustra l'integrazione di autenticazione dell'utente account Facebook in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 03/04/2019
uid: security/authentication/facebook-logins
ms.openlocfilehash: b69a6f3955d59aaff273a965d8820862e187cd51
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068209"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Configurazione dell'accesso esterno Facebook in ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione con esempi di codice illustra come consentire agli utenti di accedere con il proprio account Facebook usando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index). Iniziamo creando un ID App Facebook seguendo le [passaggi ufficiali](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Creare l'app in Facebook

* Passare il [app Facebook Developers](https://developers.facebook.com/apps/) pagina ed eseguire l'accesso. Se non si ha già un account Facebook, utilizzare il **iscriversi a Facebook** collegamento nella pagina di accesso per crearne uno.

* Toccare il **aggiungere una nuova App** pulsante nell'angolo superiore destro per creare un nuovo ID App.

   ![Facebook per il portale di sviluppatori aperta in Microsoft Edge](index/_static/FBMyApps.png)

* Compilare il modulo e toccare il **Create App ID** pulsante.

  ![Creare un nuovo ID App](index/_static/FBNewAppId.png)

* Nel **selezionare un prodotto** pagina, fare clic su **Set Up** sul **account di accesso di Facebook** carta.

  ![Pagina di installazione del prodotto](index/_static/FBProductSetup.png)

* Il **Quickstart** verrà avviata la procedura guidata con **Scegli una piattaforma** come prima pagina. Ignorare la procedura guidata per il momento, fare clic il **impostazioni** collegamento nel menu a sinistra:

  ![Skip Quick Start](index/_static/FBSkipQuickStart.png)

* Viene visualizzata con il **impostazioni OAuth Client** pagina:

  ![Pagina Impostazioni OAuth client](index/_static/FBOAuthSetup.png)

* Immettere l'URI di sviluppo con */signin-facebook* aggiunto nel **URI di reindirizzamento OAuth validi** campo (ad esempio: `https://localhost:44320/signin-facebook`). L'autenticazione di Facebook configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste al */signin-facebook* route per implementare il flusso di OAuth.

> [!NOTE]
> L'URI */signin-facebook* viene impostato come il callback predefinito del provider di autenticazione di Facebook. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione di Facebook tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) classe.

* Fare clic su **salvare le modifiche**.

* Fare clic su **le impostazioni** > **base** collegamento nel riquadro di spostamento a sinistra.

  In questa pagina, prendere nota del `App ID` e il `App Secret`. Nella sezione successiva verranno aggiunti entrambi nell'applicazione ASP.NET Core:

* Quando si distribuisce il sito è necessario rivedere le **account di accesso di Facebook** pagina di installazione e registrare un nuovo URI pubblico.

## <a name="store-facebook-app-id-and-app-secret"></a>Store Facebook App ID e segreto dell'App

Collegare le impostazioni sensibili, ad esempio Facebook `App ID` e `App Secret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets). Ai fini di questa esercitazione, denominare il token `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Eseguire i comandi seguenti per archiviare in modo sicuro `App ID` e `App Secret` con Secret Manager:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Configurare l'autenticazione di Facebook

::: moniker range=">= aspnetcore-2.0"

Aggiungere il servizio di Facebook nel `ConfigureServices` metodo di *Startup.cs* file:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Installare il [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto.

* Per installare questo pacchetto con Visual Studio 2017, pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.
* Per installare con CLI di .NET Core, eseguire il codice seguente nella directory del progetto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Aggiungere il middleware di Facebook nel `Configure` nel metodo *Startup.cs* file:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

Vedere le [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Facebook. Opzioni di configurazione possono essere utilizzate per:

* Richiedere informazioni diverse sull'utente.
* Aggiungere gli argomenti stringa di query per personalizzare l'esperienza di accesso.

## <a name="sign-in-with-facebook"></a>Accedi con Facebook

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per l'accesso con Facebook.

![Applicazione Web: Utente non autenticato](index/_static/DoneFacebook.png)

Quando fa clic su **Facebook**, si verrà reindirizzati a Facebook per l'autenticazione:

![Pagina di autenticazione di Facebook](index/_static/FBLogin.png)

Indirizzo di posta elettronica e profilo pubblico le richieste di autenticazione di Facebook per impostazione predefinita:

![Schermata di consenso pagina l'autenticazione di Facebook](index/_static/FBLoginDone.png)

Dopo avere immesso le credenziali di Facebook si viene reindirizzati al sito in cui è possibile impostare l'indirizzo di posta elettronica.

A questo punto si è connessi con le credenziali di Facebook:

![Applicazione Web: Autenticata utente](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

* **ASP.NET Core 2.x solo:** Se Identity non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: È necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si ottiene *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Aggiungere il [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto NuGet al progetto per scenari avanzati di autenticazione Facebook. Questo pacchetto non è necessario integrare la funzionalità di accesso esterno di Facebook con l'app. 

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Facebook. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito web all'app web di Azure, è consigliabile reimpostare il `AppSecret` del portale per sviluppatori di Facebook.

* Impostare il `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.
