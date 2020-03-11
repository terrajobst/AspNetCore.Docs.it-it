---
title: Configurazione dell'accesso esterno Facebook in ASP.NET Core
author: rick-anderson
description: Esercitazione con esempi di codice che illustrano l'integrazione dell'autenticazione utente dell'account Facebook in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667468"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="213e8-103">Configurazione dell'accesso esterno Facebook in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="213e8-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="213e8-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="213e8-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="213e8-105">Questa esercitazione con esempi di codice Mostra come consentire agli utenti di accedere con il proprio account Facebook usando un progetto ASP.NET Core 3,0 di esempio creato nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="213e8-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="213e8-106">Si inizia creando un ID app Facebook seguendo la [procedura ufficiale](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="213e8-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="213e8-107">Creare l'app in Facebook</span><span class="sxs-lookup"><span data-stu-id="213e8-107">Create the app in Facebook</span></span>

* <span data-ttu-id="213e8-108">Aggiungere il pacchetto NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) al progetto.</span><span class="sxs-lookup"><span data-stu-id="213e8-108">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to the project.</span></span>

* <span data-ttu-id="213e8-109">Passare alla pagina dell' [app per sviluppatori Facebook](https://developers.facebook.com/apps/) ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="213e8-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="213e8-110">Se non si ha già un account Facebook, usare il collegamento **Iscriviti a Facebook** nella pagina di accesso per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="213e8-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>  <span data-ttu-id="213e8-111">Dopo aver creato un account Facebook, seguire le istruzioni per registrarsi come sviluppatore di Facebook.</span><span class="sxs-lookup"><span data-stu-id="213e8-111">Once you have a Facebook account, follow the instructions to register as a Facebook Developer.</span></span>

* <span data-ttu-id="213e8-112">Dal menu **app personali** selezionare **Crea app** per creare un nuovo ID app.</span><span class="sxs-lookup"><span data-stu-id="213e8-112">From the **My Apps** menu select **Create App** to create a new App ID.</span></span>

   ![Facebook per il portale di sviluppatori aperta in Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="213e8-114">Compilare il modulo e toccare il pulsante **Crea ID app** .</span><span class="sxs-lookup"><span data-stu-id="213e8-114">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Creare un nuovo ID App](index/_static/FBNewAppId.png)

* <span data-ttu-id="213e8-116">Nella scheda nuova app selezionare **Aggiungi un prodotto**.</span><span class="sxs-lookup"><span data-stu-id="213e8-116">On the new App card, select **Add a Product**.</span></span>  <span data-ttu-id="213e8-117">Nella scheda di **accesso di Facebook** fare clic su **Configura**</span><span class="sxs-lookup"><span data-stu-id="213e8-117">On the **Facebook Login** card, click **Set Up**</span></span> 

  ![Pagina di installazione del prodotto](index/_static/FBProductSetup.png)

* <span data-ttu-id="213e8-119">Viene avviata la procedura guidata **avvio rapido** con **scegliere una piattaforma** come prima pagina.</span><span class="sxs-lookup"><span data-stu-id="213e8-119">The **Quickstart** wizard launches with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="213e8-120">Ignorare la procedura guidata per ora facendo clic sul collegamento **Impostazioni** di **accesso di Facebook** nel menu in basso a sinistra:</span><span class="sxs-lookup"><span data-stu-id="213e8-120">Bypass the wizard for now by clicking the **FaceBook Login** **Settings** link in the menu on the lower left:</span></span>

  ![Skip Quick Start](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="213e8-122">Viene visualizzata la pagina **impostazioni OAuth client** :</span><span class="sxs-lookup"><span data-stu-id="213e8-122">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Pagina Impostazioni OAuth client](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="213e8-124">Immettere l'URI di sviluppo con */SignIn-Facebook* aggiunto nel campo **Valid URI di reindirizzamento OAuth** (ad esempio: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="213e8-124">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="213e8-125">L'autenticazione di Facebook configurata più avanti in questa esercitazione gestirà automaticamente le richieste in */SignIn-Facebook* route per implementare il flusso OAuth.</span><span class="sxs-lookup"><span data-stu-id="213e8-125">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="213e8-126">L'URI */SignIn-Facebook* viene impostato come callback predefinito del provider di autenticazione di Facebook.</span><span class="sxs-lookup"><span data-stu-id="213e8-126">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="213e8-127">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione di Facebook tramite la proprietà [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) ereditata della classe [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .</span><span class="sxs-lookup"><span data-stu-id="213e8-127">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="213e8-128">Fare clic su **Salva modifiche**.</span><span class="sxs-lookup"><span data-stu-id="213e8-128">Click **Save Changes**.</span></span>

* <span data-ttu-id="213e8-129">Nella finestra di spostamento a sinistra fare clic su **impostazioni** > collegamento di **base** .</span><span class="sxs-lookup"><span data-stu-id="213e8-129">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="213e8-130">In questa pagina prendere nota della `App ID` e della `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="213e8-130">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="213e8-131">Nella sezione successiva verranno aggiunti entrambi nell'applicazione ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="213e8-131">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="213e8-132">Quando si distribuisce il sito, è necessario rivedere la pagina di configurazione dell' **account di accesso di Facebook** e registrare un nuovo URI pubblico.</span><span class="sxs-lookup"><span data-stu-id="213e8-132">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="213e8-133">Store Facebook App ID e segreto dell'App</span><span class="sxs-lookup"><span data-stu-id="213e8-133">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="213e8-134">Collegare le impostazioni sensibili come Facebook `App ID` e `App Secret` alla configurazione dell'applicazione usando la [gestione dei segreti](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="213e8-134">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="213e8-135">Ai fini di questa esercitazione, denominare i token `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="213e8-135">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="213e8-136">Eseguire i comandi seguenti per archiviare in modo sicuro `App ID` e `App Secret` utilizzando Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="213e8-136">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="213e8-137">Configurare l'autenticazione di Facebook</span><span class="sxs-lookup"><span data-stu-id="213e8-137">Configure Facebook Authentication</span></span>

<span data-ttu-id="213e8-138">Aggiungere il servizio Facebook nel metodo `ConfigureServices` nel file *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="213e8-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="213e8-139">Vedere le informazioni di riferimento sull'API [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) per altre informazioni sulle opzioni di configurazione supportate dall'autenticazione di Facebook.</span><span class="sxs-lookup"><span data-stu-id="213e8-139">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="213e8-140">Opzioni di configurazione possono essere utilizzate per:</span><span class="sxs-lookup"><span data-stu-id="213e8-140">Configuration options can be used to:</span></span>

* <span data-ttu-id="213e8-141">Richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="213e8-141">Request different information about the user.</span></span>
* <span data-ttu-id="213e8-142">Aggiungere gli argomenti stringa di query per personalizzare l'esperienza di accesso.</span><span class="sxs-lookup"><span data-stu-id="213e8-142">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="213e8-143">Accedi con Facebook</span><span class="sxs-lookup"><span data-stu-id="213e8-143">Sign in with Facebook</span></span>

<span data-ttu-id="213e8-144">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="213e8-144">Run your application and click **Log in**.</span></span> <span data-ttu-id="213e8-145">Viene visualizzata un'opzione per l'accesso con Facebook.</span><span class="sxs-lookup"><span data-stu-id="213e8-145">You see an option to sign in with Facebook.</span></span>

![Applicazione Web: utente non autenticato](index/_static/DoneFacebook.png)

<span data-ttu-id="213e8-147">Quando si fa clic su **Facebook**, si viene reindirizzati a Facebook per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="213e8-147">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Pagina di autenticazione di Facebook](index/_static/FBLogin.png)

<span data-ttu-id="213e8-149">Indirizzo di posta elettronica e profilo pubblico le richieste di autenticazione di Facebook per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="213e8-149">Facebook authentication requests public profile and email address by default:</span></span>

![Schermata di consenso pagina l'autenticazione di Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="213e8-151">Dopo avere immesso le credenziali di Facebook si viene reindirizzati al sito in cui è possibile impostare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="213e8-151">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="213e8-152">A questo punto si è connessi con le credenziali di Facebook:</span><span class="sxs-lookup"><span data-stu-id="213e8-152">You are now logged in using your Facebook credentials:</span></span>

![Applicazione Web: utente autenticato](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="213e8-154">risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="213e8-154">Troubleshooting</span></span>

* <span data-ttu-id="213e8-155">**Solo ASP.NET Core 2. x:** Se l'identità non è configurata chiamando `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione determinerà *ArgumentException: è necessario fornire l'opzione ' SignInScheme '* .</span><span class="sxs-lookup"><span data-stu-id="213e8-155">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="213e8-156">Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="213e8-156">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="213e8-157">Se il database del sito non è stato creato applicando la migrazione iniziale, si ottiene *un'operazione di database non riuscita durante l'elaborazione dell'* errore di richiesta.</span><span class="sxs-lookup"><span data-stu-id="213e8-157">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="213e8-158">Toccare **applica migrazioni** per creare il database e aggiornare per continuare a superare l'errore.</span><span class="sxs-lookup"><span data-stu-id="213e8-158">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="213e8-159">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="213e8-159">Next steps</span></span>

* <span data-ttu-id="213e8-160">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Facebook.</span><span class="sxs-lookup"><span data-stu-id="213e8-160">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="213e8-161">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="213e8-161">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="213e8-162">Dopo aver pubblicato il sito Web nell'app Web di Azure, è necessario reimpostare il `AppSecret` nel portale per sviluppatori di Facebook.</span><span class="sxs-lookup"><span data-stu-id="213e8-162">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="213e8-163">Impostare il `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` come impostazioni dell'applicazione nella portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="213e8-163">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="213e8-164">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="213e8-164">The configuration system is set up to read keys from environment variables.</span></span>
