---
title: Configurazione di account di accesso esterno Facebook in ASP.NET Core
author: rick-anderson
description: Configurazione di account di accesso esterno Facebook in ASP.NET Core
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2b478ce38e98977a7c52e9317b5bc6fa0d6624b7
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-facebook-authentication"></a>Configurazione dell'autenticazione di Facebook

<a name=security-authentication-facebook-logins></a>

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa esercitazione viene illustrato come consentire agli utenti di accedere con il proprio account Facebook tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](index.md). Viene innanzitutto creato un ID App Facebook seguendo il [passaggi ufficiali](https://www.facebook.com/unsupportedbrowser).

## <a name="create-the-app-in-facebook"></a>Creare l'app in Facebook

*  Passare il [Facebook per gli sviluppatori](https://www.facebook.com/unsupportedbrowser) pagina ed eseguire l'accesso. Se si dispone già di un account Facebook, utilizzare il **iscriversi a Facebook** collegamento nella pagina di accesso per crearne uno.

* Toccare il **creare App** pulsante nell'angolo superiore destro per creare un nuovo ID di App.

   ![Facebook per portale sviluppatori aprire in Microsoft Edge](index/_static/FBMyApps.png)

* Compilare il modulo e toccare il **Crea ID App** pulsante.

   ![Creare un nuovo ID di App](index/_static/FBNewAppId.png)

* Quando verrà visualizzata **selezionare un prodotto** prompt dei comandi, fare clic su **Set Up** sul **account Facebook** scheda.

   ![Pagina di installazione del prodotto](index/_static/FBProductSetup.png)

* Il **delle Guide rapide** guidata consentirà di avviare con **scegliere una piattaforma** come la prima pagina. Fare clic su ignorare la procedura guidata per il momento di **impostazioni** collegamento nel menu a sinistra:

   ![Guida introduttiva a Skip](index/_static/FBSkipQuickStart.png)

* Viene visualizzata la **impostazioni OAuth Client** pagina:

![Pagina Impostazioni OAuth client](index/_static/FBOAuthSetup.png)

* Immettere l'URI di sviluppo con */signin-facebook* aggiunti al **URI di reindirizzamento di OAuth validi** campo (ad esempio: `https://localhost:44320/signin-facebook`). L'autenticazione di Facebook configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-facebook* route per implementare il flusso di OAuth.

* Fare clic su **salvare modifiche**.

* Fare clic su di **Dashboard** collegamento nel riquadro di spostamento a sinistra. 

    In questa pagina, annotare il `App ID` e `App Secret`. Nella sezione successiva verranno aggiunti entrambi nell'applicazione ASP.NET di base:

   ![Dashboard dello sviluppatore di Facebook](index/_static/FBDashboard.png)

* Quando si distribuisce il sito è necessario rivedere il **account Facebook** pagina di installazione e registrare un nuovo URI pubblico.

## <a name="store-facebook-app-id-and-app-secret"></a>Archiviare ID App Facebook e segreto dell'applicazione

Collegare le impostazioni sensibili, ad esempio Facebook `App ID` e `App Secret` all'applicazione mediante configurazione di [Manager segreto](xref:security/app-secrets). Ai fini di questa esercitazione, denominare il token `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.

## <a name="configure-facebook-authentication"></a>Configurare l'autenticazione di Facebook

Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto è già installato.

* Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.
* Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aggiungere il servizio di Facebook nel `ConfigureServices` metodo il *Startup.cs* file:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aggiungere il middleware di Facebook nel `Configure` metodo *Startup.cs* file:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

Vedere il [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Facebook. Opzioni di configurazione possono essere utilizzate per:

* Richiedere informazioni diverse relative all'utente.
* Aggiungere gli argomenti di stringa di query per personalizzare l'esperienza di accesso.

## <a name="sign-in-with-facebook"></a>Accedere con Facebook

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Facebook.

![Applicazione Web: utente non autenticato](index/_static/DoneFacebook.png)

Facendo clic sul **Facebook**, si verrà reindirizzati a Facebook per l'autenticazione:

![Pagina di autenticazione di Facebook](index/_static/FBLogin.png)

Indirizzo di posta elettronica e profilo pubblico le richieste di autenticazione di Facebook per impostazione predefinita:

![Pagina di autenticazione di Facebook](index/_static/FBLoginDone.png)

Dopo aver immesso le credenziali di Facebook, che si verrà reindirizzati al sito in cui è possibile impostare la posta elettronica.

A questo punto si è connessi utilizzando le credenziali di Facebook:

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a>Risoluzione dei problemi

* **ASP.NET Core solo 2. x:** identità se non è configurato per la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, è possibile ottenere *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.

## <a name="next-steps"></a>Passaggi successivi

* In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Facebook. È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](index.md).

* Quando si pubblica il sito web all'app web di Azure, è consigliabile reimpostare il `AppSecret` nel portale per sviluppatori di Facebook.

* Impostare il `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.
