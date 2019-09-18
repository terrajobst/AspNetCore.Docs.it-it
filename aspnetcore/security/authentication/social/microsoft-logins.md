---
title: Configurazione dell'accesso esterno all'account Microsoft con ASP.NET Core
author: rick-anderson
description: In questo esempio viene illustrata l'integrazione di account Microsoft autenticazione utente in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 91ace293fd16cd180b3d5c183c637af6db1d08c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082336"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Configurazione dell'accesso esterno all'account Microsoft con ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo esempio illustra come consentire agli utenti di accedere con la loro account Microsoft usando il progetto ASP.NET Core 2,2 creato nella [pagina precedente](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Creare l'app nel portale per sviluppatori Microsoft

* Passare alla pagina [portale di Azure-registrazioni app](https://go.microsoft.com/fwlink/?linkid=2083908) e creare o accedere a un account Microsoft:

Se non si dispone di un account Microsoft, selezionare **crearne uno**. Dopo aver eseguito l'accesso, si verrà reindirizzati alla pagina **registrazioni app** :

* Seleziona **nuova registrazione**
* Immettere un **nome**.
* Selezionare un'opzione per i **tipi di account supportati**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* In **URI di reindirizzamento**immettere l'URL di sviluppo `/signin-microsoft` con accodato. Ad esempio `https://localhost:44389/signin-microsoft`. Lo schema di autenticazione Microsoft configurato più avanti in questo esempio gestirà automaticamente `/signin-microsoft` le richieste in route per implementare il flusso OAuth.
* Seleziona **Registro**

### <a name="create-client-secret"></a>Crea segreto client

* Nel riquadro sinistro selezionare **certificati & segreti**.
* In **segreti client**selezionare **nuovo segreto client**

  * Aggiungere una descrizione per il segreto client.
  * Selezionare il pulsante **Aggiungi** .

* In **segreti client**copiare il valore del segreto client.

> [!NOTE]
> Il segmento `/signin-microsoft` URI viene impostato come callback predefinito del provider di autenticazione Microsoft. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Microsoft tramite la proprietà [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) ereditata della classe [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Archiviare l'ID client e il segreto client Microsoft

Eseguire i comandi seguenti per archiviare `ClientId` e `ClientSecret` in modo sicuro e usare [Secret Manager](xref:security/app-secrets):

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Collegare le impostazioni sensibili come `ClientId` Microsoft `ClientSecret` e alla configurazione dell'applicazione usando la [gestione dei segreti](xref:security/app-secrets). Ai fini di questo esempio, denominare i token `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Configurare l'autenticazione dell'account Microsoft

Aggiungere il servizio account Microsoft a `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Vedere le informazioni di riferimento sull'API [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) per altre informazioni sulle opzioni di configurazione supportate dall'autenticazione dell'account Microsoft. Ciò può essere usato per richiedere informazioni diverse sull'utente.

## <a name="sign-in-with-microsoft-account"></a>Account Accedi con Microsoft

Eseguire e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Microsoft. Quando si fa clic su Microsoft, si viene reindirizzati a Microsoft per l'autenticazione. Dopo aver eseguito l'accesso con l'account Microsoft (se non è già stato effettuato l'accesso), verrà richiesto di consentire all'app di accedere alle informazioni:

Toccare **Sì** e si verrà reindirizzati di nuovo al sito Web in cui è possibile impostare la posta elettronica.

A questo punto è stato effettuato l'accesso con le credenziali Microsoft:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

* Se il provider di account Microsoft reindirizza l'utente a una pagina di errore di accesso, prendere nota dei parametri della stringa di query titolo e descrizione `#` dell'errore direttamente dopo l'hashtag nell'URI.

  Anche se il messaggio di errore sembra indicare un problema con l'autenticazione Microsoft, la causa più comune è l'URI dell'applicazione che non corrisponde ad alcuno degli **URI di reindirizzamento** specificati per la piattaforma **Web** .
* Se l'identità non è configurata chiamando `services.AddIdentity` in `ConfigureServices`, il tentativo di *eseguire l'autenticazione comporterà l'eccezione ArgumentException: È necessario specificare l'opzione ' SignInScheme '* . Il modello di progetto utilizzato in questo esempio garantisce che questa operazione venga eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Microsoft. È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito Web nell'app Web di Azure, creare un nuovo segreto client nel portale per sviluppatori Microsoft.

* Impostare il `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.