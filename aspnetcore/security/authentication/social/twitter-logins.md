---
title: Configurazione accesso esterno con ASP.NET Core di Twitter
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Twitter in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: d816ed27898639b0af6896a51ac035d5526c5d29
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814065"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Configurazione accesso esterno con ASP.NET Core di Twitter

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

In questo esempio viene illustrato come consentire agli utenti [accedere con l'account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un progetto ASP.NET Core 2.2 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Creare l'app in Twitter

* Passare a [ https://apps.twitter.com/ ](https://apps.twitter.com/) ed eseguire l'accesso. Se non si ha già un account Twitter, usare il **[iscriversi adesso](https://twitter.com/signup)** collegamento per crearne uno.

* Toccare **Create New App** e compilare l'applicazione **Name**, **descrizione** e pubblica **sito Web** URI (può essere temporaneo fino a registrare il nome di dominio):

* Immettere l'URI di sviluppo con `/signin-twitter` aggiunto nel **URI di reindirizzamento OAuth validi** campo (ad esempio: `https://webapp128.azurewebsites.net/signin-twitter`). Lo schema di autenticazione di Twitter configurato più avanti in questo esempio consente di gestire automaticamente le richieste a `/signin-twitter` route per implementare il flusso di OAuth.

  > [!NOTE]
  > Il segmento URI `/signin-twitter` viene impostato come il callback predefinito del provider di autenticazione di Twitter. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione di Twitter tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.

* Compilare il resto del form e toccare **Crea applicazione Twitter**. Vengono visualizzati nuovi dettagli dell'applicazione:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>L'archiviazione di chiavi API Consumer di Twitter e il segreto

Eseguire i comandi seguenti per archiviare in modo sicuro `ClientId` e `ClientSecret` utilizzando [Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

Collegare le impostazioni sensibili, ad esempio Twitter `Consumer Key` e `Consumer Secret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets). Ai fini di questo esempio, assegnare un nome token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.

Questi token sono reperibile nella **Keys and Access Tokens** scheda dopo aver creato una nuova applicazione Twitter:

## <a name="configure-twitter-authentication"></a>Configurare l'autenticazione di Twitter

Aggiungere il servizio di Twitter nel `ConfigureServices` nel metodo *Startup.cs* file:

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Vedere le [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Twitter. Ciò può essere usato per richiedere informazioni diverse sull'utente.

## <a name="sign-in-with-twitter"></a>Accedi con Twitter

Eseguire l'app e selezionare **Accedi**. Viene visualizzata un'opzione per accedere con Twitter:

Facendo clic su **Twitter** reindirizza a Twitter per l'autenticazione:

Dopo aver immesso le credenziali di Twitter, si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.

A questo punto si è connessi con le credenziali di Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>risoluzione dei problemi

* **ASP.NET Core 2.x solo:** Se Identity non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: È necessario specificare l'opzione 'SignInScheme'* . Il modello di progetto usato in questo esempio garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Twitter. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito web all'app web di Azure, è consigliabile reimpostare il `ConsumerSecret` del portale per sviluppatori di Twitter.

* Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.