---
title: Configurazione dell'account di accesso esterno Account Microsoft con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione di Microsoft account utente in un'applicazione ASP.NET di base esistente.
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: aabbbe66aee8c8b93140bcc4181b432017cec1d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Configurazione dell'account di accesso esterno Account Microsoft con ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa esercitazione viene illustrato come consentire agli utenti di accedere con l'account di Microsoft tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Creare l'app nel portale per sviluppatori di Microsoft

* Passare a [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) e creare o accedere a un account Microsoft:

![Accedere alla finestra di dialogo](index/_static/MicrosoftDevLogin.png)

Se non si dispone già di un account Microsoft, tocca  **[crearne uno.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Dopo l'accesso, si verrà reindirizzati a **applicazioni personali** pagina:

![Portale per sviluppatori Microsoft aperto in Microsoft Edge](index/_static/MicrosoftDev.png)

* Toccare **aggiungere un'app** in alto a destra angolo e immettere il **nome applicazione** e **Contact Email**:

![Finestra di dialogo Nuova registrazione dell'applicazione](index/_static/MicrosoftDevAppCreate.png)

* Ai fini di questa esercitazione, cancellare il **impostazione guidata** casella di controllo.

* Toccare **crea** per continuare al **registrazione** pagina. Fornire un **nome** e prendere nota del valore del **Id applicazione**, utilizzabile come `ClientId` più avanti nell'esercitazione:

![Pagina di registrazione](index/_static/MicrosoftDevAppReg.png)

* Toccare **aggiungere piattaforma** nel **piattaforme** sezione e selezionare il **Web** piattaforma:

![Finestra di dialogo piattaforma Aggiungi](index/_static/MicrosoftDevAppPlatform.png)

* Nel nuovo **Web** piattaforma, quindi immettere l'URL di sviluppo con */signin-microsoft* aggiunti al **URL di reindirizzamento** campo (ad esempio: `https://localhost:44320/signin-microsoft`). Lo schema di autenticazione Microsoft configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-microsoft* route per implementare il flusso di OAuth:

![Sezione relativa alla piattaforma Web](index/_static/MicrosoftRedirectUri.png)

* Toccare **Aggiungi URL** per verificare l'URL è stato aggiunto.

* Compilare tutte le altre impostazioni applicazione, se necessario e toccare **salvare** nella parte inferiore della pagina per salvare le modifiche alla configurazione di app.

* Quando si distribuisce il sito è necessario rivedere il **registrazione** pagina e impostare un nuovo URL pubblico.

## <a name="store-microsoft-application-id-and-password"></a>Id applicazione di Microsoft e la Password archiviati

* Nota il `Application Id` visualizzata sul **registrazione** pagina.

* Toccare **generare una nuova Password** nel **applicazione segreti** sezione. Verrà visualizzata una casella in cui è possibile copiare la password dell'applicazione:

![Finestra di dialogo nuova password generata](index/_static/MicrosoftDevPassword.png)

Collegare le impostazioni sensibili come Microsoft `Application ID` e `Password` all'applicazione mediante configurazione di [Manager segreto](xref:security/app-secrets). Ai fini di questa esercitazione, denominare il token `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Configurare l'autenticazione di Account Microsoft

Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pacchetto è già installato.

* Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.
* Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Aggiungere il servizio Account Microsoft nel `ConfigureServices` metodo *Startup.cs* file:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Aggiungere il middleware di Account Microsoft nel `Configure` metodo *Startup.cs* file:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

* * *
Sebbene la terminologia utilizzata nel portale per sviluppatori Microsoft nomi questi token `ApplicationId` e `Password`, è esposto come `ClientId` e `ClientSecret` nell'API di configurazione.

Vedere il [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Account Microsoft. Questo può essere usato per richiedere informazioni diverse relative all'utente.

## <a name="sign-in-with-microsoft-account"></a>Accedi con Account Microsoft

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per eseguire l'accesso con Microsoft:

![Log di applicazione nella pagina Web: utente non autenticato](index/_static/DoneMicrosoft.png)

Quando si fa clic su Microsoft, si viene reindirizzati a Microsoft per l'autenticazione. Dopo l'accesso con l'Account Microsoft (se non è già stato effettuato l'accesso) verrà richiesto per consentire all'app di accedere alle tue info:

![Finestra di dialogo autenticazione Microsoft](index/_static/MicrosoftLogin.png)

Toccare **Sì** e si verrà reindirizzati al sito web in cui è possibile impostare la posta elettronica.

A questo punto si è connessi utilizzando le credenziali di Microsoft:

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a>Risoluzione dei problemi

* Se il provider dell'Account di Microsoft si viene reindirizzati a una pagina di errore di accesso, tenere presente l'errore titolo e descrizione parametri stringa di query direttamente dopo il `#` (hashtag) nell'Uri.

  Anche se sembra che il messaggio di errore indicano un problema con l'autenticazione di Microsoft, la causa più comune è l'Uri non corrisponde a nessuna delle applicazione il **Redirect URIs** specificato per il **Web** piattaforma .
* **ASP.NET Core 2.x solo:** identità se non è configurata tramite la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterranno *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.

## <a name="next-steps"></a>Passaggi successivi

* In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Microsoft. È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).

* Quando si pubblica il sito web all'app web di Azure, è necessario creare un nuovo `Password` nel portale per sviluppatori di Microsoft.

* Impostare il `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.
