---
title: Configurazione dell'accesso esterno Google in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Google in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 98857a84238124e75d695242c8d421b9a29f02e7
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396109"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Configurazione dell'accesso esterno Google in ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Nel gennaio 2019 iniziato a Google [arrestare](https://developers.google.com/+/api-shutdown) Google + Accedi e gli sviluppatori devono spostare in un nuovo accesso di Google nel sistema da marzo. ASP.NET Core 2.1 e 2.2 di pacchetti per l'autenticazione di Google verrà aggiornato nel mese di febbraio per contenere le modifiche apportate. Per altre informazioni e soluzioni di attenuazione temporanee per ASP.NET Core, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/6486). Questa esercitazione è stata aggiornata al nuovo processo di installazione.

Questa esercitazione illustra come consentire agli utenti di accedere con il proprio account Google usando il progetto ASP.NET Core 2.2 creato sul [pagina precedente](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Creare un ID di progetto e il client di Console API Google

* Passare a [l'integrazione di Google Sign-nell'app web](https://developers.google.com/identity/sign-in/web/devconsole-project) e selezionare **configurare un progetto**.
* Nel **configurare il client di OAuth** finestra di dialogo, seleziona **server Web**.
* Nel **URI di reindirizzamento autorizzati** casella di immissione di testo, impostare l'URI di reindirizzamento. Ad esempio, `https://localhost:5001/signin-google`.
* Salvare il **ID Client** e **privata del Client**.
* Quando si distribuisce il sito, registrare il nuovo url pubblico dal **Console di Google**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID e ClientSecret

Le impostazioni sensibili, ad esempio Google Store `Client ID` e `Client Secret` con il [Secret Manager](xref:security/app-secrets). Ai fini di questa esercitazione, denominare il token `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret`:

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

È possibile gestire le credenziali di API e l'utilizzo nel [API Console](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Configurare l'autenticazione di Google

Aggiungere il servizio Google `Startup.ConfigureServices`.

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Accedi con Google

* Eseguire l'app e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Google.
* Scegliere il **Google** pulsante, che reindirizza a Google per l'autenticazione.
* Dopo aver immesso le credenziali di Google, si verrà reindirizzati al sito web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Vedere le [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Google. Ciò può essere usato per richiedere informazioni diverse sull'utente.

## <a name="change-the-default-callback-uri"></a>Modificare l'URI di callback predefinito

Il segmento URI `/signin-google` viene impostato come il callback predefinito del provider di autenticazione Google. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Google tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.

## <a name="troubleshooting"></a>Risoluzione dei problemi

* Se non sono presenti eventuali errori di accesso non funziona, passare alla modalità di sviluppo per rendere più semplice eseguire il debug del problema.
* Se non è configurato identità chiamando `services.AddIdentity` nelle `ConfigureServices`, quando si prova a eseguire l'autenticazione dei risultati in *ArgumentException: È necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si ottiene *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Google. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).
* Dopo aver pubblicato l'app in Azure, reimpostare il `ClientSecret` nella Console API di Google.
* Impostare il `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.
