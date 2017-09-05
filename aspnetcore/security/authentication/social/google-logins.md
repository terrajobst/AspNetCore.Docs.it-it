---
title: Programma di installazione di Google account di accesso esterno in ASP.NET Core
author: rick-anderson
description: Programma di installazione di Google account di accesso esterno in ASP.NET Core
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 8/2/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/google-logins
ms.openlocfilehash: c0bf20437dcdffc447dde3e6405cd6bf55bc8cfd
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-google-authentication-in-aspnet-core"></a>Configurazione dell'autenticazione di Google in ASP.NET Core

<a name=security-authentication-google-logins></a>

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa esercitazione viene illustrato come consentire agli utenti di accedere con l'account di Google + tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](index.md). Iniziamo seguendo il [passaggi ufficiali](https://developers.google.com/identity/sign-in/web/devconsole-project) per creare una nuova app nella Console API Google.

## <a name="create-the-app-in-google-api-console"></a>Creare l'app nella Console API di Google

* Passare a [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) ed eseguire l'accesso. Se si dispone già di un account Google, usare **più opzioni** > **[creare account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  collegamento per creare uno:

![Console di API di Google](index/_static/GoogleConsoleLogin.png)

* Si verrà reindirizzati a **libreria API Manager** pagina:

![Pagina della libreria di gestione API](index/_static/GoogleConsoleSwitchboard.png)

* Toccare **crea** e immettere il **nome progetto**:

![Finestra di dialogo Nuovo progetto](index/_static/GoogleConsoleNewProj.png)

* Dopo avere accettato la finestra di dialogo, si viene reindirizzati alla pagina libreria che consente di scegliere le funzionalità per la nuova app. Trovare **Google + API** nell'elenco e fare clic sul relativo collegamento per aggiungere la funzionalità API:

![Pagina della libreria di gestione API](index/_static/GoogleConsoleChooseApi.png)

* Verrà visualizzata la pagina per l'API appena aggiunto. Toccare **abilitare** aggiungere Google + segno nella funzionalità all'App:

![Pagina di Google + API di gestione API](index/_static/GoogleConsoleEnableApi.png)

* Dopo aver abilitato l'API, toccare **creare credenziali** per configurare i segreti:

![Pagina di Google + API di gestione API](index/_static/GoogleConsoleGoCredentials.png)

* Scegliere:
   * **Google + API**
   * **Server Web (ad esempio, node.js, Tomcat)**, e
   * **Dati utente**:

![Pagina delle credenziali di gestione API: scoprire quali tipo di credenziali necessario pannello](index/_static/GoogleConsoleChooseCred.png)

* Toccare **le credenziali sono necessarie?** che permette di visualizzare il secondo passaggio della configurazione di app, **creare un ID client OAuth 2.0**:

![Pagina delle credenziali di gestione API: creazione di un ID client OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Poiché viene creato un progetto di Google + con una sola funzionalità (accesso), è possibile immettere lo stesso **nome** per l'ID client OAuth 2.0 è utilizzato per il progetto.

* Immettere l'URI di sviluppo con */signin-google* aggiunti al **autorizzato URI di reindirizzamento** campo (ad esempio: `https://localhost:44320/signin-google`). L'autenticazione di Google configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-google* route per implementare il flusso di OAuth.

* Premere TAB per aggiungere il **autorizzato URI di reindirizzamento** voce.

* Toccare **Create client ID**, che permette di visualizzare il terzo passaggio **impostare la schermata di consenso di OAuth 2.0**:

![Pagina delle credenziali di gestione API: impostare la schermata di consenso di OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Immettere il pubblico **indirizzo di posta elettronica** e **nome prodotto** visualizzato per l'app quando Google + richiesto all'utente di accedere. Opzioni aggiuntive sono disponibili in **ulteriori opzioni di personalizzazione**.

* Toccare **continua** per eseguire l'ultimo passaggio, **scaricare le credenziali**:

![Pagina delle credenziali di gestione API: scaricare le credenziali](index/_static/GoogleConsoleFinish.png)

* Toccare **scaricare** per salvare un file JSON con segreti dell'applicazione, e **eseguita** per completare la creazione della nuova app.

* Quando si distribuisce il sito è necessario rivedere il **Google Console** e registrare un nuovo url pubblico.

## <a name="store-google-clientid-and-clientsecret"></a>Archivio Google ClientID e ClientSecret

Collegare le impostazioni sensibili, ad esempio Google `Client ID` e `Client Secret` all'applicazione mediante configurazione di [Manager segreto](../../app-secrets.md). Ai fini di questa esercitazione, denominare il token `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret`.

I valori per questi token, vedere il file JSON scaricato nel passaggio precedente in `web.client_id` e `web.client_secret`.

## <a name="configure-google-authentication"></a>Configurare l'autenticazione di Google

Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) viene installato.

 * Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.
 * Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Aggiungere il servizio Google il `ConfigureServices` metodo *Startup.cs* file:

```csharp
services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

Il `AddAuthentication` metodo deve essere chiamato solo una volta quando si aggiunge più provider di autenticazione. Le chiamate successive a esso hanno la possibilità di cui si esegue l'override di qualsiasi configurati in precedenza [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) proprietà.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Aggiungere il middleware di Google nel `Configure` metodo *Startup.cs* file:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

Vedere il [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione Google. Questo può essere usato per richiedere informazioni diverse relative all'utente.

## <a name="sign-in-with-google"></a>Accedere con Google

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Google:

![Applicazione Web in esecuzione in Microsoft Edge: utente non autenticato](index/_static/DoneGoogle.png)

Quando si fa clic su Google, si verrà reindirizzati a Google per l'autenticazione:

![Finestra di dialogo autenticazione Google](index/_static/GoogleLogin.png)

Dopo aver immesso le credenziali di Google, quindi si verrà reindirizzati al sito web in cui è possibile impostare la posta elettronica.

A questo punto si è connessi utilizzando le credenziali di Google:

![Applicazione Web in esecuzione in Microsoft Edge: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a>Risoluzione dei problemi

* Se si riceve un `403 (Forbidden)` pagina di errore dall'applicazione durante l'esecuzione in modalità di sviluppo o interruzione nel debugger con lo stesso errore, assicurarsi che **Google + API** è stata abilitata nel **libreria gestione API** seguendo i passaggi elencati [precedenti in questa pagina](#create-the-app-in-google-api-console). Se l'accesso non funziona e non vengono visualizzate eventuali errori, passare alla modalità di sviluppo per rendere più semplice eseguire il debug del problema.
* **ASP.NET Core solo 2. x:** identità se non è configurato per la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterranno *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.

## <a name="next-steps"></a>Passaggi successivi

* In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Google. È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](index.md).

* Quando si pubblica il sito web all'app web di Azure, è consigliabile reimpostare il `ClientSecret` nella Console di API di Google.

* Impostare il `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.
