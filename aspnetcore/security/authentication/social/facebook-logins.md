---
title: Configurazione dell'accesso esterno Facebook in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Facebook in un'app ASP.NET Core esistente.
ms.author: riande
ms.date: 08/01/2017
uid: security/authentication/facebook-logins
ms.openlocfilehash: 3ba6fe7785afa268e54e6032f1963c1867f6bb27
ms.sourcegitcommit: 74c09caec8992635825b45b7f065f871d33c077a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42634809"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="a5280-103">Configurazione dell'accesso esterno Facebook in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5280-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="a5280-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a5280-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5280-105">Questa esercitazione illustra come consentire agli utenti di accedere con il proprio account Facebook usando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a5280-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="a5280-106">Richiede l'autenticazione di Facebook le [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="a5280-106">Facebook authentication requires the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package.</span></span> <span data-ttu-id="a5280-107">Iniziamo creando un ID App Facebook seguendo le [passaggi ufficiali](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="a5280-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="a5280-108">Creare l'app in Facebook</span><span class="sxs-lookup"><span data-stu-id="a5280-108">Create the app in Facebook</span></span>

* <span data-ttu-id="a5280-109">Passare il [app Facebook Developers](https://developers.facebook.com/apps/) pagina ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="a5280-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="a5280-110">Se non si ha già un account Facebook, utilizzare il **iscriversi a Facebook** collegamento nella pagina di accesso per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="a5280-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="a5280-111">Toccare il **aggiungere una nuova App** pulsante nell'angolo superiore destro per creare un nuovo ID App.</span><span class="sxs-lookup"><span data-stu-id="a5280-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook per il portale di sviluppatori aperta in Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="a5280-113">Compilare il modulo e toccare il **Create App ID** pulsante.</span><span class="sxs-lookup"><span data-stu-id="a5280-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Creare un nuovo ID App](index/_static/FBNewAppId.png)

* <span data-ttu-id="a5280-115">Nel **selezionare un prodotto** pagina, fare clic su **Set Up** sul **account di accesso di Facebook** carta.</span><span class="sxs-lookup"><span data-stu-id="a5280-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![Pagina di installazione del prodotto](index/_static/FBProductSetup.png)

* <span data-ttu-id="a5280-117">Il **Quickstart** verrà avviata la procedura guidata con **Scegli una piattaforma** come prima pagina.</span><span class="sxs-lookup"><span data-stu-id="a5280-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="a5280-118">Ignorare la procedura guidata per il momento, fare clic il **impostazioni** collegamento nel menu a sinistra:</span><span class="sxs-lookup"><span data-stu-id="a5280-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Skip Quick Start](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="a5280-120">Viene visualizzata con il **impostazioni OAuth Client** pagina:</span><span class="sxs-lookup"><span data-stu-id="a5280-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Pagina Impostazioni OAuth client](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="a5280-122">Immettere l'URI di sviluppo con */signin-facebook* aggiunto nel **URI di reindirizzamento OAuth validi** campo (ad esempio: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="a5280-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="a5280-123">L'autenticazione di Facebook configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste al */signin-facebook* route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="a5280-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="a5280-124">L'URI */signin-facebook* viene impostato come il callback predefinito del provider di autenticazione di Facebook.</span><span class="sxs-lookup"><span data-stu-id="a5280-124">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="a5280-125">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione di Facebook tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="a5280-125">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="a5280-126">Fare clic su **salvare le modifiche**.</span><span class="sxs-lookup"><span data-stu-id="a5280-126">Click **Save Changes**.</span></span>

* <span data-ttu-id="a5280-127">Fare clic su **Impostazioni > base** collegamento nel riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="a5280-127">Click **Settings > Basic** link in the left navigation.</span></span> 

    <span data-ttu-id="a5280-128">In questa pagina, prendere nota del `App ID` e il `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="a5280-128">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="a5280-129">Nella sezione successiva verranno aggiunti entrambi nell'applicazione ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a5280-129">You will add both into your ASP.NET Core application in the next section:</span></span>


* <span data-ttu-id="a5280-130">Quando si distribuisce il sito è necessario rivedere le **account di accesso di Facebook** pagina di installazione e registrare un nuovo URI pubblico.</span><span class="sxs-lookup"><span data-stu-id="a5280-130">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="a5280-131">Store Facebook App ID e segreto dell'App</span><span class="sxs-lookup"><span data-stu-id="a5280-131">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="a5280-132">Collegare le impostazioni sensibili, ad esempio Facebook `App ID` e `App Secret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a5280-132">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="a5280-133">Ai fini di questa esercitazione, denominare il token `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="a5280-133">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="a5280-134">Eseguire i comandi seguenti per archiviare in modo sicuro `App ID` e `App Secret` con Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="a5280-134">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="a5280-135">Configurare l'autenticazione di Facebook</span><span class="sxs-lookup"><span data-stu-id="a5280-135">Configure Facebook Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5280-136">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5280-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a5280-137">Aggiungere il servizio di Facebook nel `ConfigureServices` metodo di *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="a5280-137">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5280-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5280-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a5280-139">Installare il [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="a5280-139">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="a5280-140">Per installare questo pacchetto con Visual Studio 2017, pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a5280-140">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="a5280-141">Per installare con CLI di .NET Core, eseguire il codice seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="a5280-141">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="a5280-142">Aggiungere il middleware di Facebook nel `Configure` nel metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="a5280-142">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="a5280-143">Vedere le [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Facebook.</span><span class="sxs-lookup"><span data-stu-id="a5280-143">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="a5280-144">Opzioni di configurazione possono essere utilizzate per:</span><span class="sxs-lookup"><span data-stu-id="a5280-144">Configuration options can be used to:</span></span>

* <span data-ttu-id="a5280-145">Richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="a5280-145">Request different information about the user.</span></span>
* <span data-ttu-id="a5280-146">Aggiungere gli argomenti stringa di query per personalizzare l'esperienza di accesso.</span><span class="sxs-lookup"><span data-stu-id="a5280-146">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="a5280-147">Accedi con Facebook</span><span class="sxs-lookup"><span data-stu-id="a5280-147">Sign in with Facebook</span></span>

<span data-ttu-id="a5280-148">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="a5280-148">Run your application and click **Log in**.</span></span> <span data-ttu-id="a5280-149">Viene visualizzata un'opzione per l'accesso con Facebook.</span><span class="sxs-lookup"><span data-stu-id="a5280-149">You see an option to sign in with Facebook.</span></span>

![Applicazione Web: utente non autenticato](index/_static/DoneFacebook.png)

<span data-ttu-id="a5280-151">Quando fa clic su **Facebook**, si verrà reindirizzati a Facebook per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="a5280-151">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Pagina di autenticazione di Facebook](index/_static/FBLogin.png)

<span data-ttu-id="a5280-153">Indirizzo di posta elettronica e profilo pubblico le richieste di autenticazione di Facebook per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="a5280-153">Facebook authentication requests public profile and email address by default:</span></span>

![Pagina di autenticazione di Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="a5280-155">Dopo avere immesso le credenziali di Facebook si viene reindirizzati al sito in cui è possibile impostare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="a5280-155">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="a5280-156">A questo punto si è connessi con le credenziali di Facebook:</span><span class="sxs-lookup"><span data-stu-id="a5280-156">You are now logged in using your Facebook credentials:</span></span>

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="a5280-158">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="a5280-158">Troubleshooting</span></span>

* <span data-ttu-id="a5280-159">**ASP.NET Core 2.x solo:** Identity se non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="a5280-159">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="a5280-160">Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="a5280-160">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="a5280-161">Se il database del sito non è stato creato applicando la migrazione iniziale, si ottiene *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="a5280-161">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="a5280-162">Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.</span><span class="sxs-lookup"><span data-stu-id="a5280-162">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5280-163">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a5280-163">Next steps</span></span>

* <span data-ttu-id="a5280-164">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Facebook.</span><span class="sxs-lookup"><span data-stu-id="a5280-164">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="a5280-165">È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a5280-165">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="a5280-166">Dopo aver pubblicato il sito web all'app web di Azure, è consigliabile reimpostare il `AppSecret` del portale per sviluppatori di Facebook.</span><span class="sxs-lookup"><span data-stu-id="a5280-166">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="a5280-167">Impostare il `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="a5280-167">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="a5280-168">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="a5280-168">The configuration system is set up to read keys from environment variables.</span></span>
