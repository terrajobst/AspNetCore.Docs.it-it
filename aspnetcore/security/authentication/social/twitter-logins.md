---
title: Installazione di accesso esterno Twitter con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione di Twitter account utente in un'applicazione ASP.NET di base esistente.
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/twitter-logins
ms.openlocfilehash: 81c19105e4cda932db3302a5df343322fb85abef
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278492"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="e62f2-103">Installazione di accesso esterno Twitter con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e62f2-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="e62f2-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e62f2-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e62f2-105">In questa esercitazione viene illustrato come consentire agli utenti di [accesso con l'account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) utilizzando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="e62f2-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="e62f2-106">Creare l'app in Twitter</span><span class="sxs-lookup"><span data-stu-id="e62f2-106">Create the app in Twitter</span></span>

* <span data-ttu-id="e62f2-107">Passare a [ https://apps.twitter.com/ ](https://apps.twitter.com/) ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e62f2-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="e62f2-108">Se si dispone già di un account Twitter, utilizzare il **[Iscriviti ora](https://twitter.com/signup)** collegamento per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="e62f2-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="e62f2-109">Dopo l'accesso, il **Application Management** viene visualizzata la pagina:</span><span class="sxs-lookup"><span data-stu-id="e62f2-109">After signing in, the **Application Management** page is shown:</span></span>

![Gestione delle applicazioni Twitter aperto in Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="e62f2-111">Toccare **Create New App** e compilare l'applicazione **nome**, **descrizione** pubbliche e **sito Web** (può essere temporanea finché non si URI registrare il nome di dominio):</span><span class="sxs-lookup"><span data-stu-id="e62f2-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Creare una pagina applicazione](index/_static/TwitterCreate.png)

* <span data-ttu-id="e62f2-113">Immettere l'URI di sviluppo con `/signin-twitter` accodati nel **valido OAuth Redirect URIs** campo (ad esempio: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="e62f2-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="e62f2-114">Lo schema di autenticazione Twitter configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel `/signin-twitter` route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="e62f2-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="e62f2-115">Il segmento URI `/signin-twitter` viene impostato come callback predefinito del provider di autenticazione Twitter.</span><span class="sxs-lookup"><span data-stu-id="e62f2-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="e62f2-116">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Twitter tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà del [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.</span><span class="sxs-lookup"><span data-stu-id="e62f2-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="e62f2-117">Compilare il resto del form e toccare **creare un'applicazione Twitter**.</span><span class="sxs-lookup"><span data-stu-id="e62f2-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="e62f2-118">Vengono visualizzati i nuovi dettagli di applicazioni:</span><span class="sxs-lookup"><span data-stu-id="e62f2-118">New application details are displayed:</span></span>

![Scheda Dettagli nella pagina di applicazione](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="e62f2-120">Quando si distribuisce il sito è necessario rivedere il **Application Management** pagina e registrare un nuovo URI pubblico.</span><span class="sxs-lookup"><span data-stu-id="e62f2-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="e62f2-121">ConsumerSecret e l'archiviazione ConsumerKey Twitter</span><span class="sxs-lookup"><span data-stu-id="e62f2-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="e62f2-122">Collegare le impostazioni sensibili come Twitter `Consumer Key` e `Consumer Secret` all'applicazione mediante configurazione di [Manager segreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="e62f2-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="e62f2-123">Ai fini di questa esercitazione, denominare il token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="e62f2-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="e62f2-124">Questi token possono trovarsi nel **chiavi e i token di accesso** scheda dopo aver creato la nuova applicazione Twitter:</span><span class="sxs-lookup"><span data-stu-id="e62f2-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Scheda chiavi e i token di accesso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="e62f2-126">Configurare l'autenticazione di Twitter</span><span class="sxs-lookup"><span data-stu-id="e62f2-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="e62f2-127">Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pacchetto è già installato.</span><span class="sxs-lookup"><span data-stu-id="e62f2-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="e62f2-128">Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e62f2-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="e62f2-129">Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="e62f2-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e62f2-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e62f2-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="e62f2-131">Aggiungere il servizio di Twitter nel `ConfigureServices` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="e62f2-131">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e62f2-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e62f2-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="e62f2-133">Aggiungere il middleware di Twitter nel `Configure` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="e62f2-133">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="e62f2-134">Vedere il [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Twitter.</span><span class="sxs-lookup"><span data-stu-id="e62f2-134">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="e62f2-135">Questo può essere usato per richiedere informazioni diverse relative all'utente.</span><span class="sxs-lookup"><span data-stu-id="e62f2-135">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="e62f2-136">Accedere con Twitter</span><span class="sxs-lookup"><span data-stu-id="e62f2-136">Sign in with Twitter</span></span>

<span data-ttu-id="e62f2-137">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="e62f2-137">Run your application and click **Log in**.</span></span> <span data-ttu-id="e62f2-138">Viene visualizzata un'opzione per accedere con Twitter:</span><span class="sxs-lookup"><span data-stu-id="e62f2-138">An option to sign in with Twitter appears:</span></span>

![Applicazione Web: utente non autenticato](index/_static/DoneTwitter.png)

<span data-ttu-id="e62f2-140">Facendo clic su **Twitter** reindirizza a Twitter per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="e62f2-140">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Pagina di autenticazione di Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="e62f2-142">Dopo aver immesso le credenziali di Twitter, si verrà reindirizzati al sito web in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="e62f2-142">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="e62f2-143">A questo punto si è connessi utilizzando le credenziali di Twitter:</span><span class="sxs-lookup"><span data-stu-id="e62f2-143">You are now logged in using your Twitter credentials:</span></span>

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="e62f2-145">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="e62f2-145">Troubleshooting</span></span>

* <span data-ttu-id="e62f2-146">**ASP.NET Core 2.x solo:** identità se non è configurata tramite la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="e62f2-146">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="e62f2-147">Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="e62f2-147">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="e62f2-148">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterranno *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="e62f2-148">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="e62f2-149">Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.</span><span class="sxs-lookup"><span data-stu-id="e62f2-149">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e62f2-150">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e62f2-150">Next steps</span></span>

* <span data-ttu-id="e62f2-151">In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Twitter.</span><span class="sxs-lookup"><span data-stu-id="e62f2-151">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="e62f2-152">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="e62f2-152">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="e62f2-153">Quando si pubblica il sito web all'app web di Azure, è consigliabile reimpostare il `ConsumerSecret` nel portale per sviluppatori di Twitter.</span><span class="sxs-lookup"><span data-stu-id="e62f2-153">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="e62f2-154">Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e62f2-154">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="e62f2-155">Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="e62f2-155">The configuration system is set up to read keys from environment variables.</span></span>
