---
title: Configurazione dell'accesso esterno Google in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Google in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082479"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Configurazione dell'accesso esterno Google in ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[Le API di Google + legacy sono state arrestate a partire dal 7 marzo 2019](https://developers.google.com/+/api-shutdown). L'accesso a Google + e gli sviluppatori devono passare a un nuovo sistema di accesso Google. Per soddisfare le modifiche, è necessario aggiornare i pacchetti ASP.NET Core 2,1 e 2,2 per l'autenticazione Google. Per ulteriori informazioni e mitigazioni temporanee per ASP.NET Core, vedere [questo problema di GitHub](https://github.com/aspnet/AspNetCore/issues/6486). Questa esercitazione è stata aggiornata con il nuovo processo di installazione.

Questa esercitazione illustra come consentire agli utenti di accedere con il proprio account Google usando il progetto ASP.NET Core 2,2 creato nella [pagina precedente](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Creare un progetto console e un ID client di Google API

* Passare all'articolo relativo all' [integrazione di Google Sign-in nell'app Web](https://developers.google.com/identity/sign-in/web/devconsole-project) e selezionare **configura un progetto**.
* Nella finestra di dialogo **Configura client OAuth** selezionare **server Web**.
* Nella casella voce di testo **URI di reindirizzamento autorizzato** impostare l'URI di reindirizzamento. Ad esempio: `https://localhost:5001/signin-google`
* Salvare l' **ID client** e il **segreto client**.
* Quando si distribuisce il sito, registrare il nuovo URL pubblico dalla **console di Google**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID e ClientSecret

Archiviare le impostazioni riservate, ad `Client ID` esempio `Client Secret` Google e la [gestione dei segreti](xref:security/app-secrets). Ai fini di questa esercitazione, denominare i token `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret`:

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

È possibile gestire le credenziali e l'utilizzo dell'API nella [console API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Configurare l'autenticazione di Google

Aggiungere il servizio Google a `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Accedi con Google

* Eseguire l'app e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Google.
* Fare clic sul pulsante **Google** , che reindirizza a Google per l'autenticazione.
* Dopo aver immesso le credenziali Google, viene reindirizzato di nuovo al sito Web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Per altre <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> informazioni sulle opzioni di configurazione supportate da Google Authentication, vedere le informazioni di riferimento sulle API. Ciò può essere usato per richiedere informazioni diverse sull'utente.

## <a name="change-the-default-callback-uri"></a>Modificare l'URI di callback predefinito

Il segmento URI `/signin-google` viene impostato come il callback predefinito del provider di autenticazione Google. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Google tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.

## <a name="troubleshooting"></a>Risoluzione dei problemi

* Se l'accesso non funziona e non si ricevono errori, passare alla modalità di sviluppo per semplificare il debug del problema.
* Se l'identità non è configurata chiamando `services.AddIdentity` in `ConfigureServices`, il tentativo di autenticare i risultati in *ArgumentException: È necessario specificare l'opzione ' SignInScheme '* . Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si ottiene *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Selezionare **applica migrazioni** per creare il database e aggiornare la pagina per continuare a superare l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Google. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).
* Dopo aver pubblicato l'app in Azure, reimpostare `ClientSecret` la nella console dell'API Google.
* Impostare il `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.
