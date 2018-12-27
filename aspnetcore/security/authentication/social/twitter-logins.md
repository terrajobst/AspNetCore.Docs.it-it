---
title: Configurazione accesso esterno di Twitter con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Twitter in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 49db8b921fde169380ca284f46e535786b2b8a30
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735804"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="22efe-103">Configurazione accesso esterno di Twitter con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22efe-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="22efe-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="22efe-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="22efe-105">Questa esercitazione illustra come consentire agli utenti di [accedere con l'account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="22efe-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="22efe-106">Creare l'app in Twitter</span><span class="sxs-lookup"><span data-stu-id="22efe-106">Create the app in Twitter</span></span>

* <span data-ttu-id="22efe-107">Passare a [ https://apps.twitter.com/ ](https://apps.twitter.com/) ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="22efe-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="22efe-108">Se non si ha già un account Twitter, usare il **[iscriversi adesso](https://twitter.com/signup)** collegamento per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="22efe-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="22efe-109">Dopo l'accesso, il **Gestione applicazioni** pagina viene visualizzata:</span><span class="sxs-lookup"><span data-stu-id="22efe-109">After signing in, the **Application Management** page is shown:</span></span>

  ![Gestione delle applicazioni Twitter Apri in Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="22efe-111">Toccare **Create New App** e compilare l'applicazione **Name**, **descrizione** e pubblica **sito Web** URI (può essere temporaneo fino a registrare il nome di dominio):</span><span class="sxs-lookup"><span data-stu-id="22efe-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

  ![Creare una pagina applicazione](index/_static/TwitterCreate.png)

* <span data-ttu-id="22efe-113">Immettere l'URI di sviluppo con `/signin-twitter` aggiunto nel **URI di reindirizzamento OAuth validi** campo (ad esempio: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="22efe-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="22efe-114">Lo schema di autenticazione di Twitter configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste a `/signin-twitter` route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="22efe-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="22efe-115">Il segmento URI `/signin-twitter` viene impostato come il callback predefinito del provider di autenticazione di Twitter.</span><span class="sxs-lookup"><span data-stu-id="22efe-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="22efe-116">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione di Twitter tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.</span><span class="sxs-lookup"><span data-stu-id="22efe-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="22efe-117">Compilare il resto del form e toccare **Crea applicazione Twitter**.</span><span class="sxs-lookup"><span data-stu-id="22efe-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="22efe-118">Vengono visualizzati nuovi dettagli dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="22efe-118">New application details are displayed:</span></span>

  ![Scheda Dettagli nella pagina di applicazione](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="22efe-120">Quando si distribuisce il sito è necessario rivedere le **Gestione applicazioni** pagina e registrare un nuovo URI pubblico.</span><span class="sxs-lookup"><span data-stu-id="22efe-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="22efe-121">ConsumerSecret e l'archiviazione ConsumerKey Twitter</span><span class="sxs-lookup"><span data-stu-id="22efe-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="22efe-122">Collegare le impostazioni sensibili, ad esempio Twitter `Consumer Key` e `Consumer Secret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="22efe-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="22efe-123">Ai fini di questa esercitazione, denominare il token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="22efe-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="22efe-124">Questi token sono reperibile nella **Keys and Access Tokens** scheda dopo aver creato la nuova applicazione Twitter:</span><span class="sxs-lookup"><span data-stu-id="22efe-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Scheda chiavi e token di accesso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="22efe-126">Configurare l'autenticazione di Twitter</span><span class="sxs-lookup"><span data-stu-id="22efe-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="22efe-127">Il modello di progetto usato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pacchetto è già installato.</span><span class="sxs-lookup"><span data-stu-id="22efe-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="22efe-128">Per installare questo pacchetto con Visual Studio 2017, pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="22efe-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="22efe-129">Per installare con CLI di .NET Core, eseguire il codice seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="22efe-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="22efe-130">Aggiungere il servizio di Twitter nel `ConfigureServices` nel metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="22efe-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="22efe-131">Aggiungere il middleware di Twitter nel `Configure` nel metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="22efe-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

<span data-ttu-id="22efe-132">Vedere le [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Twitter.</span><span class="sxs-lookup"><span data-stu-id="22efe-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="22efe-133">Ciò può essere usato per richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="22efe-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="22efe-134">Accedi con Twitter</span><span class="sxs-lookup"><span data-stu-id="22efe-134">Sign in with Twitter</span></span>

<span data-ttu-id="22efe-135">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="22efe-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="22efe-136">Viene visualizzata un'opzione per accedere con Twitter:</span><span class="sxs-lookup"><span data-stu-id="22efe-136">An option to sign in with Twitter appears:</span></span>

![Applicazione Web: Utente non autenticato](index/_static/DoneTwitter.png)

<span data-ttu-id="22efe-138">Facendo clic su **Twitter** reindirizza a Twitter per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="22efe-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Pagina di autenticazione di Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="22efe-140">Dopo aver immesso le credenziali di Twitter, si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="22efe-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="22efe-141">A questo punto si è connessi con le credenziali di Twitter:</span><span class="sxs-lookup"><span data-stu-id="22efe-141">You are now logged in using your Twitter credentials:</span></span>

![Applicazione Web: Autenticata utente](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="22efe-143">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="22efe-143">Troubleshooting</span></span>

* <span data-ttu-id="22efe-144">**ASP.NET Core 2.x solo:** Se Identity non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: È necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="22efe-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="22efe-145">Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="22efe-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="22efe-146">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="22efe-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="22efe-147">Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.</span><span class="sxs-lookup"><span data-stu-id="22efe-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22efe-148">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="22efe-148">Next steps</span></span>

* <span data-ttu-id="22efe-149">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Twitter.</span><span class="sxs-lookup"><span data-stu-id="22efe-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="22efe-150">È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="22efe-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="22efe-151">Dopo aver pubblicato il sito web all'app web di Azure, è consigliabile reimpostare il `ConsumerSecret` del portale per sviluppatori di Twitter.</span><span class="sxs-lookup"><span data-stu-id="22efe-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="22efe-152">Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="22efe-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="22efe-153">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="22efe-153">The configuration system is set up to read keys from environment variables.</span></span>
