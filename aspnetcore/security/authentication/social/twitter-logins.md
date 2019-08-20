---
title: Configurazione dell'accesso esterno a Twitter con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione utente dell'account Twitter in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 6b6fa3e50f602a92fec9112ac3ba43583de33a70
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994275"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Configurazione dell'accesso esterno a Twitter con ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo esempio illustra come consentire agli utenti di [accedere con il proprio account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un progetto ASP.NET Core 2,2 di esempio creato nella [pagina precedente](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Creare l'app in Twitter

* Passare a [ https://apps.twitter.com/ ](https://apps.twitter.com/) ed eseguire l'accesso. Se non si ha già un account Twitter, usare il collegamento **[Iscriviti ora](https://twitter.com/signup)** per crearne uno.

* Toccare **Crea nuova app** e compilare il **nome**dell'applicazione, la **Descrizione** e l'URI del **sito Web** pubblico. questa operazione può essere temporanea fino a quando non si registra il nome di dominio:

* Immettere l'URI di sviluppo `/signin-twitter` con accodato nel campo **validi URI di reindirizzamento OAuth** (ad esempio `https://webapp128.azurewebsites.net/signin-twitter`:). Lo schema di autenticazione di Twitter configurato più avanti in questo esempio gestirà `/signin-twitter` automaticamente le richieste in route per implementare il flusso OAuth.

  > [!NOTE]
  > Il segmento `/signin-twitter` URI viene impostato come callback predefinito del provider di autenticazione Twitter. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Twitter tramite la proprietà [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) ereditata della classe [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .

* Compilare il resto del modulo e toccare crea l' **applicazione Twitter**. Vengono visualizzati i dettagli della nuova applicazione:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Archiviazione della chiave e del segreto API del consumer Twitter

Eseguire i comandi seguenti per archiviare `ClientId` e `ClientSecret` in modo sicuro e usare [Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Collegare le impostazioni sensibili come `Consumer Key` Twitter `Consumer Secret` e alla configurazione dell'applicazione usando la [gestione dei segreti](xref:security/app-secrets). Ai fini di questo esempio, denominare i token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.

Questi token si trovano nella scheda **chiavi e token di accesso** dopo la creazione di una nuova applicazione Twitter:

## <a name="configure-twitter-authentication"></a>Configurare l'autenticazione di Twitter

Aggiungere il servizio Twitter nel `ConfigureServices` metodo nel file *Startup.cs* :

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Vedere le informazioni di riferimento sull'API [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) per altre informazioni sulle opzioni di configurazione supportate dall'autenticazione Twitter. Ciò può essere usato per richiedere informazioni diverse sull'utente.

## <a name="sign-in-with-twitter"></a>Accedi con Twitter

Eseguire l'app e selezionare **Accedi**. Viene visualizzata un'opzione per accedere con Twitter:

Clic su **Twitter** reindirizza a Twitter per l'autenticazione:

Dopo aver immesso le credenziali di Twitter, viene reindirizzato di nuovo al sito Web in cui è possibile impostare la posta elettronica.

A questo punto è stato effettuato l'accesso con le credenziali di Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>risoluzione dei problemi

* **Solo ASP.NET Core 2. x:** Se l'identità non è configurata chiamando `services.AddIdentity` in `ConfigureServices`, il tentativo di *eseguire l'autenticazione comporterà l'eccezione ArgumentException: È necessario specificare l'opzione ' SignInScheme '* . Il modello di progetto utilizzato in questo esempio garantisce che questa operazione venga eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Twitter. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito Web nell'app Web di Azure, è necessario reimpostare il `ConsumerSecret` nel portale per sviluppatori di Twitter.

* Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.
