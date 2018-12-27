---
title: Configurazione dell'accesso esterno Facebook in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Facebook in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 12/18/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: 66f895c7c8dcc00d991c0ea57535f2ed56431a77
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735778"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="2da24-103">Configurazione dell'accesso esterno Facebook in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2da24-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="2da24-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2da24-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2da24-105">Questa esercitazione illustra come consentire agli utenti di accedere con il proprio account Facebook usando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2da24-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="2da24-106">Iniziamo creando un ID App Facebook seguendo le [passaggi ufficiali](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="2da24-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="2da24-107">Creare l'app in Facebook</span><span class="sxs-lookup"><span data-stu-id="2da24-107">Create the app in Facebook</span></span>

* <span data-ttu-id="2da24-108">Passare il [app Facebook Developers](https://developers.facebook.com/apps/) pagina ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="2da24-108">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="2da24-109">Se non si ha già un account Facebook, utilizzare il **iscriversi a Facebook** collegamento nella pagina di accesso per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="2da24-109">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="2da24-110">Toccare il **aggiungere una nuova App** pulsante nell'angolo superiore destro per creare un nuovo ID App.</span><span class="sxs-lookup"><span data-stu-id="2da24-110">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook per il portale di sviluppatori aperta in Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="2da24-112">Compilare il modulo e toccare il **Create App ID** pulsante.</span><span class="sxs-lookup"><span data-stu-id="2da24-112">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Creare un nuovo ID App](index/_static/FBNewAppId.png)

* <span data-ttu-id="2da24-114">Nel **selezionare un prodotto** pagina, fare clic su **Set Up** sul **account di accesso di Facebook** carta.</span><span class="sxs-lookup"><span data-stu-id="2da24-114">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![Pagina di installazione del prodotto](index/_static/FBProductSetup.png)

* <span data-ttu-id="2da24-116">Il **Quickstart** verrà avviata la procedura guidata con **Scegli una piattaforma** come prima pagina.</span><span class="sxs-lookup"><span data-stu-id="2da24-116">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="2da24-117">Ignorare la procedura guidata per il momento, fare clic il **impostazioni** collegamento nel menu a sinistra:</span><span class="sxs-lookup"><span data-stu-id="2da24-117">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![Skip Quick Start](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="2da24-119">Viene visualizzata con il **impostazioni OAuth Client** pagina:</span><span class="sxs-lookup"><span data-stu-id="2da24-119">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Pagina Impostazioni OAuth client](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="2da24-121">Immettere l'URI di sviluppo con */signin-facebook* aggiunto nel **URI di reindirizzamento OAuth validi** campo (ad esempio: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="2da24-121">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="2da24-122">L'autenticazione di Facebook configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste al */signin-facebook* route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="2da24-122">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="2da24-123">L'URI */signin-facebook* viene impostato come il callback predefinito del provider di autenticazione di Facebook.</span><span class="sxs-lookup"><span data-stu-id="2da24-123">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="2da24-124">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione di Facebook tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="2da24-124">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="2da24-125">Fare clic su **salvare le modifiche**.</span><span class="sxs-lookup"><span data-stu-id="2da24-125">Click **Save Changes**.</span></span>

* <span data-ttu-id="2da24-126">Fare clic su **le impostazioni** > **base** collegamento nel riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="2da24-126">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="2da24-127">In questa pagina, prendere nota del `App ID` e il `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="2da24-127">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="2da24-128">Nella sezione successiva verranno aggiunti entrambi nell'applicazione ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2da24-128">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="2da24-129">Quando si distribuisce il sito è necessario rivedere le **account di accesso di Facebook** pagina di installazione e registrare un nuovo URI pubblico.</span><span class="sxs-lookup"><span data-stu-id="2da24-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="2da24-130">Store Facebook App ID e segreto dell'App</span><span class="sxs-lookup"><span data-stu-id="2da24-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="2da24-131">Collegare le impostazioni sensibili, ad esempio Facebook `App ID` e `App Secret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2da24-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="2da24-132">Ai fini di questa esercitazione, denominare il token `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="2da24-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="2da24-133">Eseguire i comandi seguenti per archiviare in modo sicuro `App ID` e `App Secret` con Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="2da24-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="2da24-134">Configurare l'autenticazione di Facebook</span><span class="sxs-lookup"><span data-stu-id="2da24-134">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2da24-135">Aggiungere il servizio di Facebook nel `ConfigureServices` metodo di *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="2da24-135">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2da24-136">Installare il [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="2da24-136">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="2da24-137">Per installare questo pacchetto con Visual Studio 2017, pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2da24-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="2da24-138">Per installare con CLI di .NET Core, eseguire il codice seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="2da24-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="2da24-139">Aggiungere il middleware di Facebook nel `Configure` nel metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="2da24-139">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="2da24-140">Vedere le [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Facebook.</span><span class="sxs-lookup"><span data-stu-id="2da24-140">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="2da24-141">Opzioni di configurazione possono essere utilizzate per:</span><span class="sxs-lookup"><span data-stu-id="2da24-141">Configuration options can be used to:</span></span>

* <span data-ttu-id="2da24-142">Richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="2da24-142">Request different information about the user.</span></span>
* <span data-ttu-id="2da24-143">Aggiungere gli argomenti stringa di query per personalizzare l'esperienza di accesso.</span><span class="sxs-lookup"><span data-stu-id="2da24-143">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="2da24-144">Accedi con Facebook</span><span class="sxs-lookup"><span data-stu-id="2da24-144">Sign in with Facebook</span></span>

<span data-ttu-id="2da24-145">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="2da24-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="2da24-146">Viene visualizzata un'opzione per l'accesso con Facebook.</span><span class="sxs-lookup"><span data-stu-id="2da24-146">You see an option to sign in with Facebook.</span></span>

![Applicazione Web: Utente non autenticato](index/_static/DoneFacebook.png)

<span data-ttu-id="2da24-148">Quando fa clic su **Facebook**, si verrà reindirizzati a Facebook per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="2da24-148">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Pagina di autenticazione di Facebook](index/_static/FBLogin.png)

<span data-ttu-id="2da24-150">Indirizzo di posta elettronica e profilo pubblico le richieste di autenticazione di Facebook per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="2da24-150">Facebook authentication requests public profile and email address by default:</span></span>

![Schermata di consenso pagina l'autenticazione di Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="2da24-152">Dopo avere immesso le credenziali di Facebook si viene reindirizzati al sito in cui è possibile impostare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="2da24-152">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="2da24-153">A questo punto si è connessi con le credenziali di Facebook:</span><span class="sxs-lookup"><span data-stu-id="2da24-153">You are now logged in using your Facebook credentials:</span></span>

![Applicazione Web: Autenticata utente](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="2da24-155">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="2da24-155">Troubleshooting</span></span>

* <span data-ttu-id="2da24-156">**ASP.NET Core 2.x solo:** Se Identity non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: È necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="2da24-156">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="2da24-157">Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="2da24-157">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="2da24-158">Se il database del sito non è stato creato applicando la migrazione iniziale, si ottiene *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="2da24-158">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="2da24-159">Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.</span><span class="sxs-lookup"><span data-stu-id="2da24-159">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2da24-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="2da24-160">Next steps</span></span>

* <span data-ttu-id="2da24-161">Aggiungere il [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto NuGet al progetto per scenari avanzati di autenticazione Facebook.</span><span class="sxs-lookup"><span data-stu-id="2da24-161">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to your project for advanced Facebook authentication scenarios.</span></span> <span data-ttu-id="2da24-162">Questo pacchetto non è necessario integrare la funzionalità di accesso esterno di Facebook con l'app.</span><span class="sxs-lookup"><span data-stu-id="2da24-162">This package isn't required to integrate Facebook external login functionality with your app.</span></span> 

* <span data-ttu-id="2da24-163">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Facebook.</span><span class="sxs-lookup"><span data-stu-id="2da24-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="2da24-164">È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2da24-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="2da24-165">Dopo aver pubblicato il sito web all'app web di Azure, è consigliabile reimpostare il `AppSecret` del portale per sviluppatori di Facebook.</span><span class="sxs-lookup"><span data-stu-id="2da24-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="2da24-166">Impostare il `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="2da24-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="2da24-167">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="2da24-167">The configuration system is set up to read keys from environment variables.</span></span>
