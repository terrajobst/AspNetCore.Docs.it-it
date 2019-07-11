---
title: Configurazione accesso esterno dell'Account Microsoft con ASP.NET Core
author: rick-anderson
description: Questo esempio illustra l'integrazione dell'autenticazione di Microsoft account utente in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 2c690e5bd8465806d42091616917cfdd747ef8f0
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815566"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Configurazione accesso esterno dell'Account Microsoft con ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

In questo esempio illustra come consentire agli utenti di accedere con il proprio account Microsoft usando il progetto ASP.NET Core 2.2 creato sul [pagina precedente](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Creare l'app nel portale per sviluppatori Microsoft

* Passare il [portale di Azure - registrazioni di App](https://go.microsoft.com/fwlink/?linkid=2083908) pagina e creare o accedere a un account Microsoft:

Se non hai un account Microsoft, selezionare **crearne uno**. Dopo l'accesso si verrà reindirizzati per il **registrazioni per l'App** pagina:

* Selezionare **nuova registrazione**
* Immettere un **nome**.
* Selezionare un'opzione per **tipi di account supportati**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* Sotto **URI di reindirizzamento**, immettere l'URL di sviluppo con `/signin-microsoft` aggiunto. Ad esempio `https://localhost:44389/signin-microsoft`. Lo schema di autenticazione Microsoft configurato più avanti in questo esempio consente di gestire automaticamente le richieste a `/signin-microsoft` route per implementare il flusso di OAuth.
* Selezionare **registrare**

### <a name="create-client-secret"></a>Creare il segreto client

* Nel riquadro sinistro, selezionare **certificati e i segreti**.
* Sotto **i segreti Client**, selezionare **nuovo segreto client**

  * Aggiungere una descrizione per il segreto client.
  * Selezionare il **Add** pulsante.

* Sotto **i segreti Client**, copiare il valore del segreto client.

> [!NOTE]
> Il segmento URI `/signin-microsoft` viene impostato come il callback predefinito del provider di autenticazione Microsoft. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Microsoft tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà del [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) classe.

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Il segreto client e ID del client Microsoft Store

Eseguire i comandi seguenti per archiviare in modo sicuro `ClientId` e `ClientSecret` utilizzando [Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Collegare le impostazioni sensibili, ad esempio Microsoft `ClientId` e `ClientSecret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets). Ai fini di questo esempio, assegnare un nome token `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Configurare l'autenticazione Account Microsoft

Aggiungere il servizio Account Microsoft a `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Vedere le [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Account Microsoft. Ciò può essere usato per richiedere informazioni diverse sull'utente.

## <a name="sign-in-with-microsoft-account"></a>Accedi con Account Microsoft

Eseguire il e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Microsoft. Quando fa clic su Microsoft, si verrà reindirizzati a Microsoft per l'autenticazione. Dopo l'accesso con l'Account Microsoft (se non è già stato effettuato l'accesso) verrà richiesto per consentire all'app di accedere alle tue info:

Toccare **Sì** e si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.

A questo punto si è connessi con le credenziali di Microsoft:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>risoluzione dei problemi

* Se il provider dell'Account di Microsoft si viene reindirizzati a una pagina di errore di accesso, si noti l'errore title e description parametri stringa di query che seguono direttamente il `#` (hashtag) nell'Uri.

  Anche se sembra che il messaggio di errore per indicare un problema con l'autenticazione di Microsoft, la causa più comune è l'Uri non corrisponda a uno di applicazione la **Redirect URIs** specificato per il **Web** piattaforma .
* Se Identity non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: È necessario specificare l'opzione 'SignInScheme'* . Il modello di progetto usato in questo esempio garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Microsoft. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito web all'app web di Azure, creare un nuovo client i segreti nel portale per sviluppatori Microsoft.

* Impostare il `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.