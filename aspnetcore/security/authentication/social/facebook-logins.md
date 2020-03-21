---
title: Configurazione dell'accesso esterno Facebook in ASP.NET Core
author: rick-anderson
description: Esercitazione con esempi di codice che illustrano l'integrazione dell'autenticazione utente dell'account Facebook in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: bb26a27f026e744c7d4925aa2281bf0625fff8a2
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989786"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Configurazione dell'accesso esterno Facebook in ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione con esempi di codice Mostra come consentire agli utenti di accedere con il proprio account Facebook usando un progetto ASP.NET Core 3,0 di esempio creato nella [pagina precedente](xref:security/authentication/social/index). Si inizia creando un ID app Facebook seguendo la [procedura ufficiale](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Creare l'app in Facebook

* Aggiungere il pacchetto NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) al progetto.

* Passare alla pagina dell' [app per sviluppatori Facebook](https://developers.facebook.com/apps/) ed eseguire l'accesso. Se non si ha già un account Facebook, usare il collegamento **Iscriviti a Facebook** nella pagina di accesso per crearne uno.  Dopo aver creato un account Facebook, seguire le istruzioni per registrarsi come sviluppatore di Facebook.

* Dal menu **app personali** selezionare **Crea app** per creare un nuovo ID app.

   ![Facebook per il portale di sviluppatori aperta in Microsoft Edge](index/_static/FBMyApps.png)

* Compilare il modulo e toccare il pulsante **Crea ID app** .

  ![Creare un nuovo ID App](index/_static/FBNewAppId.png)

* Nella scheda nuova app selezionare **Aggiungi un prodotto**.  Nella scheda di **accesso di Facebook** fare clic su **Configura** 

  ![Pagina di installazione del prodotto](index/_static/FBProductSetup.png)

* Viene avviata la procedura guidata **avvio rapido** con **scegliere una piattaforma** come prima pagina. Ignorare la procedura guidata per ora facendo clic sul collegamento **Impostazioni** di **accesso di Facebook** nel menu in basso a sinistra:

  ![Skip Quick Start](index/_static/FBSkipQuickStart.png)

* Viene visualizzata la pagina **impostazioni OAuth client** :

  ![Pagina Impostazioni OAuth client](index/_static/FBOAuthSetup.png)

* Immettere l'URI di sviluppo con */SignIn-Facebook* aggiunto nel campo **Valid URI di reindirizzamento OAuth** (ad esempio: `https://localhost:44320/signin-facebook`). L'autenticazione di Facebook configurata più avanti in questa esercitazione gestirà automaticamente le richieste in */SignIn-Facebook* route per implementare il flusso OAuth.

> [!NOTE]
> L'URI */SignIn-Facebook* viene impostato come callback predefinito del provider di autenticazione di Facebook. È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione di Facebook tramite la proprietà [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) ereditata della classe [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .

* Fare clic su **Save Changes**.

* Nella finestra di spostamento a sinistra fare clic su **impostazioni** > collegamento di **base** .

  In questa pagina prendere nota della `App ID` e della `App Secret`. Nella sezione successiva verranno aggiunti entrambi nell'applicazione ASP.NET Core:

* Quando si distribuisce il sito, è necessario rivedere la pagina di configurazione dell' **account di accesso di Facebook** e registrare un nuovo URI pubblico.

## <a name="store-the-facebook-app-id-and-secret"></a>Archiviare l'ID e il segreto dell'app Facebook

Archiviare le impostazioni riservate, ad esempio l'ID app Facebook e i valori segreti con [gestione Secret](xref:security/app-secrets). Per questo esempio, attenersi alla procedura seguente:

1. Inizializzare il progetto per l'archiviazione segreta in base alle istruzioni riportate in [abilitare l'archiviazione segreta](xref:security/app-secrets#enable-secret-storage).
1. Archiviare le impostazioni sensibili nell'archivio dei segreti locali con le chiavi segrete `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`:

    ```dotnetcli
    dotnet user-secrets set "Authentication:Facebook:AppId" "<app-id>"
    dotnet user-secrets set "Authentication:Facebook:AppSecret" "<app-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-facebook-authentication"></a>Configurare l'autenticazione di Facebook

Aggiungere il servizio Facebook nel metodo `ConfigureServices` nel file *Startup.cs* :

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Vedere le informazioni di riferimento sull'API [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) per altre informazioni sulle opzioni di configurazione supportate dall'autenticazione di Facebook. Opzioni di configurazione possono essere utilizzate per:

* Richiedere informazioni diverse sull'utente.
* Aggiungere gli argomenti stringa di query per personalizzare l'esperienza di accesso.

## <a name="sign-in-with-facebook"></a>Accedi con Facebook

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per l'accesso con Facebook.

![Applicazione Web: utente non autenticato](index/_static/DoneFacebook.png)

Quando si fa clic su **Facebook**, si viene reindirizzati a Facebook per l'autenticazione:

![Pagina di autenticazione di Facebook](index/_static/FBLogin.png)

Indirizzo di posta elettronica e profilo pubblico le richieste di autenticazione di Facebook per impostazione predefinita:

![Schermata di consenso pagina l'autenticazione di Facebook](index/_static/FBLoginDone.png)

Dopo avere immesso le credenziali di Facebook si viene reindirizzati al sito in cui è possibile impostare l'indirizzo di posta elettronica.

A questo punto si è connessi con le credenziali di Facebook:

![Applicazione Web: utente autenticato](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

* **Solo ASP.NET Core 2. x:** Se l'identità non è configurata chiamando `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione determinerà *ArgumentException: è necessario fornire l'opzione ' SignInScheme '* . Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, si ottiene *un'operazione di database non riuscita durante l'elaborazione dell'* errore di richiesta. Toccare **applica migrazioni** per creare il database e aggiornare per continuare a superare l'errore.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Facebook. È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).

* Dopo aver pubblicato il sito Web nell'app Web di Azure, è necessario reimpostare il `AppSecret` nel portale per sviluppatori di Facebook.

* Impostare il `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` come impostazioni dell'applicazione nella portale di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.
