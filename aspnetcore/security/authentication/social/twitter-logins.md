---
title: Il programma di installazione di accesso esterno a Twitter
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 87be0bdd4637cff609a4908b303a13272656e2a4
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-twitter-authentication"></a>Configurazione dell'autenticazione di Twitter

<a name=security-authentication-twitter-logins></a>

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa esercitazione viene illustrato come consentire agli utenti di [accesso con l'account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) utilizzando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](index.md).

## <a name="create-the-app-in-twitter"></a>Creare l'app in Twitter

* Passare a [https://apps.twitter.com/](https://apps.twitter.com/) ed eseguire l'accesso. Se si dispone già di un account Twitter, utilizzare il  **[Iscriviti ora](https://twitter.com/signup)**  collegamento per crearne uno. Dopo l'accesso, il **Application Management** viene visualizzata la pagina:

![Gestione delle applicazioni Twitter aperto in Microsoft Edge](index/_static/TwitterAppManage.png)

* Toccare **Create New App** e compilare l'applicazione **nome**, **descrizione** pubbliche e **sito Web** (può essere temporanea finché non si URI registrare il nome di dominio):

![Creare una pagina applicazione](index/_static/TwitterCreate.png)

* Immettere l'URI di sviluppo con */signin-twitter* aggiunti al **URI di reindirizzamento di OAuth validi** campo (ad esempio: `https://localhost:44320/signin-twitter`). Lo schema di autenticazione Twitter configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-twitter* route per implementare il flusso di OAuth.

* Compilare il resto del form e toccare **creare un'applicazione Twitter**. Vengono visualizzati i nuovi dettagli di applicazioni:

![Scheda Dettagli nella pagina di applicazione](index/_static/TwitterAppDetails.png)

* Quando si distribuisce il sito è necessario rivedere il **Application Management** pagina e registrare un nuovo URI pubblico.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>ConsumerSecret e l'archiviazione ConsumerKey Twitter

Collegare le impostazioni sensibili come Twitter `Consumer Key` e `Consumer Secret` all'applicazione mediante configurazione di [Manager segreto](../../app-secrets.md). Ai fini di questa esercitazione, denominare il token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.

Questi token possono trovarsi nel **chiavi e i token di accesso** scheda dopo aver creato la nuova applicazione Twitter:

![Scheda chiavi e i token di accesso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Configurare l'autenticazione di Twitter

Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pacchetto è già installato.

* Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.
* Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aggiungere il servizio di Twitter nel `ConfigureServices` metodo *Startup.cs* file:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aggiungere il middleware di Twitter nel `Configure` metodo *Startup.cs* file:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

Vedere il [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Twitter. Questo può essere usato per richiedere informazioni diverse relative all'utente.

## <a name="sign-in-with-twitter"></a>Accedere con Twitter

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Twitter:

![Applicazione Web: utente non autenticato](index/_static/DoneTwitter.png)

Facendo clic su **Twitter** reindirizza a Twitter per l'autenticazione:

![Pagina di autenticazione di Twitter](index/_static/TwitterLogin.png)

Dopo aver immesso le credenziali di Twitter, si verrà reindirizzati al sito web in cui è possibile impostare la posta elettronica.

A questo punto si è connessi utilizzando le credenziali di Twitter:

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a>Risoluzione dei problemi

* **ASP.NET Core solo 2. x:** identità se non è configurato per la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterranno *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.

## <a name="next-steps"></a>Passaggi successivi

* In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Twitter. È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](index.md).

* Quando si pubblica il sito web all'app web di Azure, è consigliabile reimpostare il `ConsumerSecret` nel portale per sviluppatori di Twitter.

* Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.
