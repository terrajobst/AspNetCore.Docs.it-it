---
title: Configurazione dell'accesso esterno Google in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Google in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: e5deda5d521643e3155be00f4630a86c6a82575c
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121531"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Configurazione dell'accesso esterno Google in ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra come consentire agli utenti di accedere con il proprio account Google + tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index). Iniziare seguendo la [passaggi ufficiali](https://developers.google.com/identity/sign-in/web/devconsole-project) per creare una nuova app nella Console API di Google.

## <a name="create-the-app-in-google-api-console"></a>Creare l'app nella Console API Google

* Passare a [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) ed eseguire l'accesso. Se non si ha già un account Google, usare **altre opzioni** > **[creare account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  collegamento per crearne uno:

![Console di Google API](index/_static/GoogleConsoleLogin.png)

* Si verrà reindirizzati alla **libreria di gestione API** pagina:

![Nella pagina libreria di gestione API di destinazione](index/_static/GoogleConsoleSwitchboard.png)

* Toccare **Create** e immettere il **nome del progetto**:

![Finestra di dialogo Nuovo progetto](index/_static/GoogleConsoleNewProj.png)

* Dopo aver accettato la finestra di dialogo, si verrà reindirizzati alla pagina di libreria che consente di scegliere le funzionalità per la nuova app. Trovare **Google + API** nell'elenco e fare clic sul relativo collegamento per aggiungere la funzionalità API:

![Cercare "Google + API" nella pagina della libreria di gestione API](index/_static/GoogleConsoleChooseApi.png)

* Verrà visualizzata la pagina relativa all'API appena aggiunto. Toccare **abilitare** aggiungere Google + segno nella funzionalità all'App:

![Nella pagina Google + API di gestione API di destinazione](index/_static/GoogleConsoleEnableApi.png)

* Dopo l'abilitazione dell'API, toccare **creare le credenziali** per configurare i segreti:

![Creare credenziali pulsante nella pagina di gestione API di Google + API](index/_static/GoogleConsoleGoCredentials.png)

* Scegliere:
  * **Google + API**
  * **Server Web (ad esempio Node. js, Tomcat)**, e
  * **I dati utente**:

![Pagina delle credenziali di gestione API: informazioni su quali tipi di credenziali è necessario pannello](index/_static/GoogleConsoleChooseCred.png)

* Toccare **quali credenziali è necessario?** che consente di visualizzare il secondo passaggio della configurazione dell'app, **creare un ID client OAuth 2.0**:

![Pagina delle credenziali di gestione API: creazione di un ID client OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Perché si sta creando un progetto Google + con una sola funzionalità (Accedi), è possibile immettere lo stesso **nome** per l'ID client OAuth 2.0 come quella utilizzate per il progetto.

* Immettere l'URI di sviluppo con `/signin-google` aggiunto nel **URI di reindirizzamento autorizzati** campo (ad esempio: `https://localhost:44320/signin-google`). L'autenticazione di Google configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste a `/signin-google` route per implementare il flusso di OAuth.

> [!NOTE]
> Il segmento URI `/signin-google` viene impostato come il callback predefinito del provider di autenticazione Google. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Google tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.

* Premere TAB per aggiungere il **URI di reindirizzamento autorizzati** voce.

* Toccare **creare ID client**, che consente di visualizzare il terzo passaggio **configurare la schermata di consenso di OAuth 2.0**:

![Pagina delle credenziali di gestione API: configurare la schermata di consenso di OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Immettere il pubblico **indirizzo di posta elettronica** e il **il nome del prodotto** visualizzato per l'app quando Google + richiede all'utente di accedere. Sono disponibili con opzioni aggiuntive **altre opzioni di personalizzazione**.

* Toccare **continuazione** per procedere all'ultimo passaggio **scaricare le credenziali**:

![Pagina delle credenziali di gestione API: scaricare le credenziali](index/_static/GoogleConsoleFinish.png)

* Toccare **scaricare** per salvare un file JSON con i segreti dell'applicazione, e **eseguita** per completare la creazione della nuova app.

* Quando si distribuisce il sito è necessario rivedere le **Console di Google** e registrare un nuovo url pubblico.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID e ClientSecret

Collegare le impostazioni sensibili, ad esempio Google `Client ID` e `Client Secret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets). Ai fini di questa esercitazione, denominare il token `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret`.

I valori per questi token sono reperibili nel file JSON scaricato nel passaggio precedente sotto `web.client_id` e `web.client_secret`.

## <a name="configure-google-authentication"></a>Configurare l'autenticazione di Google

::: moniker range=">= aspnetcore-2.0"

Aggiungere il servizio Google nel `ConfigureServices` nel metodo *Startup.cs* file:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Il modello di progetto usato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) installazione del pacchetto.

* Per installare questo pacchetto con Visual Studio 2017, pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.
* Per installare con CLI di .NET Core, eseguire il codice seguente nella directory del progetto:

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Aggiungere il middleware di Google nel `Configure` nel metodo *Startup.cs* file:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

Vedere le [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Google. Ciò può essere usato per richiedere informazioni diverse sull'utente.

## <a name="sign-in-with-google"></a>Accedi con Google

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Google:

![Applicazione Web in esecuzione in Microsoft Edge: utente non autenticato](index/_static/DoneGoogle.png)

Quando fa clic su Google, si verrà reindirizzati a Google per l'autenticazione:

![Finestra di dialogo autenticazione Google](index/_static/GoogleLogin.png)

Dopo aver immesso le credenziali di Google, quindi si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.

A questo punto si è connessi con le credenziali di Google:

![Applicazione Web in esecuzione in Microsoft Edge: utente autenticato](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

* Se si riceve un `403 (Forbidden)` pagina di errore dalla propria app quando è in esecuzione in modalità di sviluppo o inserire un'interruzione nel debugger con lo stesso errore, verificare che **Google + API** è stata abilitata nel **libreriadigestioneAPI** seguendo i passaggi elencati [precedenti in questa pagina](#create-the-app-in-google-api-console). Se non sono presenti eventuali errori di accesso non funziona, passare alla modalità di sviluppo per rendere più semplice eseguire il debug del problema.
* **ASP.NET Core 2.x solo:** Identity se non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Google. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito web all'app web di Azure, è consigliabile reimpostare il `ClientSecret` nella Console API di Google.

* Impostare il `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.
