---
title: Configurazione dell'accesso esterno a Twitter con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione utente dell'account Twitter in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 4710c033018710ce3620f8d7221ae2253b2c0b69
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665928"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Configurazione dell'accesso esterno a Twitter con ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo esempio illustra come consentire agli utenti di [accedere con il proprio account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un progetto ASP.NET Core 3,0 di esempio creato nella [pagina precedente](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Creare l'app in Twitter

* Aggiungere il pacchetto NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) al progetto.

* Passare a [https://apps.twitter.com/](https://apps.twitter.com/) ed eseguire l'accesso. Se non si ha già un account Twitter, usare il collegamento **[Iscriviti ora](https://twitter.com/signup)** per crearne uno.

* Selezionare **Create an app** (Crea un'app). Compilare il **nome dell'app**, la **Descrizione dell'applicazione** e l'URI del **sito Web** pubblico. questa operazione può essere temporanea fino a quando non si registra il nome di dominio:

* Selezionare la casella accanto a **Abilita accesso con Twitter**

* Per impostazione predefinita, Microsoft. AspNetCore. Identity richiede che gli utenti dispongano di un indirizzo di posta elettronica. Passare alla scheda **autorizzazioni** , fare clic sul pulsante **modifica** e selezionare la casella accanto a **Richiedi indirizzo di posta elettronica dagli utenti**.

* Immettere l'URI di sviluppo con `/signin-twitter` accodato nel campo **URL di callback** (ad esempio: `https://webapp128.azurewebsites.net/signin-twitter`). Lo schema di autenticazione di Twitter configurato più avanti in questo esempio gestirà automaticamente le richieste in `/signin-twitter` route per implementare il flusso OAuth.

  > [!NOTE]
  > Il `/signin-twitter` del segmento URI è impostato come callback predefinito del provider di autenticazione Twitter. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Twitter tramite la proprietà [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) ereditata della classe [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .

* Compilare il resto del modulo e selezionare **Crea**. Vengono visualizzati i dettagli della nuova applicazione:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Archiviazione della chiave e del segreto API del consumer Twitter

Eseguire i comandi seguenti per archiviare in modo sicuro `ClientId` e `ClientSecret` utilizzando [Secret Manager](xref:security/app-secrets):

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Collegare le impostazioni riservate, ad esempio Twitter `Consumer Key` e `Consumer Secret` alla configurazione dell'applicazione usando la [gestione dei segreti](xref:security/app-secrets). Ai fini di questo esempio, denominare i token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.

Questi token si trovano nella scheda **chiavi e token di accesso** dopo la creazione di una nuova applicazione Twitter:

## <a name="configure-twitter-authentication"></a>Configurare l'autenticazione di Twitter

Aggiungere il servizio Twitter nel metodo `ConfigureServices` nel file *Startup.cs* :

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

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

* **Solo ASP.NET Core 2. x:** Se l'identità non è configurata chiamando `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione determinerà *ArgumentException: è necessario fornire l'opzione ' SignInScheme '* . Il modello di progetto utilizzato in questo esempio garantisce che questa operazione venga eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione dell'* errore di richiesta. Toccare **applica migrazioni** per creare il database e aggiornare per continuare a superare l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Twitter. È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito Web nell'app Web di Azure, è necessario reimpostare il `ConsumerSecret` nel portale per sviluppatori di Twitter.

* Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nella portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.
