---
title: Configurazione accesso esterno di Twitter con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Twitter in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 49db8b921fde169380ca284f46e535786b2b8a30
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735804"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Configurazione accesso esterno di Twitter con ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra come consentire agli utenti di [accedere con l'account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Creare l'app in Twitter

* Passare a [ https://apps.twitter.com/ ](https://apps.twitter.com/) ed eseguire l'accesso. Se non si ha già un account Twitter, usare il **[iscriversi adesso](https://twitter.com/signup)** collegamento per crearne uno. Dopo l'accesso, il **Gestione applicazioni** pagina viene visualizzata:

  ![Gestione delle applicazioni Twitter Apri in Microsoft Edge](index/_static/TwitterAppManage.png)

* Toccare **Create New App** e compilare l'applicazione **Name**, **descrizione** e pubblica **sito Web** URI (può essere temporaneo fino a registrare il nome di dominio):

  ![Creare una pagina applicazione](index/_static/TwitterCreate.png)

* Immettere l'URI di sviluppo con `/signin-twitter` aggiunto nel **URI di reindirizzamento OAuth validi** campo (ad esempio: `https://localhost:44320/signin-twitter`). Lo schema di autenticazione di Twitter configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste a `/signin-twitter` route per implementare il flusso di OAuth.

  > [!NOTE]
  > Il segmento URI `/signin-twitter` viene impostato come il callback predefinito del provider di autenticazione di Twitter. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione di Twitter tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.

* Compilare il resto del form e toccare **Crea applicazione Twitter**. Vengono visualizzati nuovi dettagli dell'applicazione:

  ![Scheda Dettagli nella pagina di applicazione](index/_static/TwitterAppDetails.png)

* Quando si distribuisce il sito è necessario rivedere le **Gestione applicazioni** pagina e registrare un nuovo URI pubblico.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>ConsumerSecret e l'archiviazione ConsumerKey Twitter

Collegare le impostazioni sensibili, ad esempio Twitter `Consumer Key` e `Consumer Secret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets). Ai fini di questa esercitazione, denominare il token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.

Questi token sono reperibile nella **Keys and Access Tokens** scheda dopo aver creato la nuova applicazione Twitter:

![Scheda chiavi e token di accesso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Configurare l'autenticazione di Twitter

Il modello di progetto usato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pacchetto è già installato.

* Per installare questo pacchetto con Visual Studio 2017, pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.
* Per installare con CLI di .NET Core, eseguire il codice seguente nella directory del progetto:

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

Aggiungere il servizio di Twitter nel `ConfigureServices` nel metodo *Startup.cs* file:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aggiungere il middleware di Twitter nel `Configure` nel metodo *Startup.cs* file:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

Vedere le [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Twitter. Ciò può essere usato per richiedere informazioni diverse sull'utente.

## <a name="sign-in-with-twitter"></a>Accedi con Twitter

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Twitter:

![Applicazione Web: Utente non autenticato](index/_static/DoneTwitter.png)

Facendo clic su **Twitter** reindirizza a Twitter per l'autenticazione:

![Pagina di autenticazione di Twitter](index/_static/TwitterLogin.png)

Dopo aver immesso le credenziali di Twitter, si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.

A questo punto si è connessi con le credenziali di Twitter:

![Applicazione Web: Autenticata utente](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

* **ASP.NET Core 2.x solo:** Se Identity non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: È necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Twitter. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito web all'app web di Azure, è consigliabile reimpostare il `ConsumerSecret` del portale per sviluppatori di Twitter.

* Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.
