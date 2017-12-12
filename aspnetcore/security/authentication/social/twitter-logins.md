---
title: Il programma di installazione di accesso esterno a Twitter
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione di Twitter account utente in un'applicazione ASP.NET di base esistente.
keywords: ASP.NET Core, Twitter, account di accesso, autenticazione
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 6751b34b42007cffa9ee92ee49170564b8eac997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-twitter-authentication"></a><span data-ttu-id="ab151-104">Configurazione dell'autenticazione di Twitter</span><span class="sxs-lookup"><span data-stu-id="ab151-104">Configuring Twitter authentication</span></span>

<span data-ttu-id="ab151-105">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ab151-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ab151-106">In questa esercitazione viene illustrato come consentire agli utenti di [accesso con l'account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) utilizzando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](index.md).</span><span class="sxs-lookup"><span data-stu-id="ab151-106">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="ab151-107">Creare l'app in Twitter</span><span class="sxs-lookup"><span data-stu-id="ab151-107">Create the app in Twitter</span></span>

* <span data-ttu-id="ab151-108">Passare a [https://apps.twitter.com/](https://apps.twitter.com/) ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="ab151-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="ab151-109">Se si dispone già di un account Twitter, utilizzare il  **[Iscriviti ora](https://twitter.com/signup)**  collegamento per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="ab151-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="ab151-110">Dopo l'accesso, il **Application Management** viene visualizzata la pagina:</span><span class="sxs-lookup"><span data-stu-id="ab151-110">After signing in, the **Application Management** page is shown:</span></span>

![Gestione delle applicazioni Twitter aperto in Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="ab151-112">Toccare **Create New App** e compilare l'applicazione **nome**, **descrizione** pubbliche e **sito Web** (può essere temporanea finché non si URI registrare il nome di dominio):</span><span class="sxs-lookup"><span data-stu-id="ab151-112">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Creare una pagina applicazione](index/_static/TwitterCreate.png)

* <span data-ttu-id="ab151-114">Immettere l'URI di sviluppo con */signin-twitter* aggiunti al **URI di reindirizzamento di OAuth validi** campo (ad esempio: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="ab151-114">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="ab151-115">Lo schema di autenticazione Twitter configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-twitter* route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="ab151-115">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="ab151-116">Compilare il resto del form e toccare **creare un'applicazione Twitter**.</span><span class="sxs-lookup"><span data-stu-id="ab151-116">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="ab151-117">Vengono visualizzati i nuovi dettagli di applicazioni:</span><span class="sxs-lookup"><span data-stu-id="ab151-117">New application details are displayed:</span></span>

![Scheda Dettagli nella pagina di applicazione](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="ab151-119">Quando si distribuisce il sito è necessario rivedere il **Application Management** pagina e registrare un nuovo URI pubblico.</span><span class="sxs-lookup"><span data-stu-id="ab151-119">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="ab151-120">ConsumerSecret e l'archiviazione ConsumerKey Twitter</span><span class="sxs-lookup"><span data-stu-id="ab151-120">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="ab151-121">Collegare le impostazioni sensibili come Twitter `Consumer Key` e `Consumer Secret` all'applicazione mediante configurazione di [Manager segreto](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="ab151-121">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="ab151-122">Ai fini di questa esercitazione, denominare il token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="ab151-122">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="ab151-123">Questi token possono trovarsi nel **chiavi e i token di accesso** scheda dopo aver creato la nuova applicazione Twitter:</span><span class="sxs-lookup"><span data-stu-id="ab151-123">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Scheda chiavi e i token di accesso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="ab151-125">Configurare l'autenticazione di Twitter</span><span class="sxs-lookup"><span data-stu-id="ab151-125">Configure Twitter Authentication</span></span>

<span data-ttu-id="ab151-126">Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pacchetto è già installato.</span><span class="sxs-lookup"><span data-stu-id="ab151-126">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="ab151-127">Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ab151-127">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="ab151-128">Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="ab151-128">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab151-129">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ab151-129">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ab151-130">Aggiungere il servizio di Twitter nel `ConfigureServices` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="ab151-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab151-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ab151-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ab151-132">Aggiungere il middleware di Twitter nel `Configure` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="ab151-132">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="ab151-133">Vedere il [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Twitter.</span><span class="sxs-lookup"><span data-stu-id="ab151-133">See the [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="ab151-134">Questo può essere usato per richiedere informazioni diverse relative all'utente.</span><span class="sxs-lookup"><span data-stu-id="ab151-134">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="ab151-135">Accedere con Twitter</span><span class="sxs-lookup"><span data-stu-id="ab151-135">Sign in with Twitter</span></span>

<span data-ttu-id="ab151-136">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="ab151-136">Run your application and click **Log in**.</span></span> <span data-ttu-id="ab151-137">Viene visualizzata un'opzione per accedere con Twitter:</span><span class="sxs-lookup"><span data-stu-id="ab151-137">An option to sign in with Twitter appears:</span></span>

![Applicazione Web: utente non autenticato](index/_static/DoneTwitter.png)

<span data-ttu-id="ab151-139">Facendo clic su **Twitter** reindirizza a Twitter per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="ab151-139">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Pagina di autenticazione di Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="ab151-141">Dopo aver immesso le credenziali di Twitter, si verrà reindirizzati al sito web in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ab151-141">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="ab151-142">A questo punto si è connessi utilizzando le credenziali di Twitter:</span><span class="sxs-lookup"><span data-stu-id="ab151-142">You are now logged in using your Twitter credentials:</span></span>

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="ab151-144">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="ab151-144">Troubleshooting</span></span>

* <span data-ttu-id="ab151-145">**ASP.NET Core solo 2. x:** identità se non è configurato per la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="ab151-145">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="ab151-146">Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="ab151-146">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="ab151-147">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterranno *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="ab151-147">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="ab151-148">Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.</span><span class="sxs-lookup"><span data-stu-id="ab151-148">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab151-149">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="ab151-149">Next steps</span></span>

* <span data-ttu-id="ab151-150">In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Twitter.</span><span class="sxs-lookup"><span data-stu-id="ab151-150">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="ab151-151">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](index.md).</span><span class="sxs-lookup"><span data-stu-id="ab151-151">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="ab151-152">Quando si pubblica il sito web all'app web di Azure, è consigliabile reimpostare il `ConsumerSecret` nel portale per sviluppatori di Twitter.</span><span class="sxs-lookup"><span data-stu-id="ab151-152">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="ab151-153">Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="ab151-153">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="ab151-154">Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="ab151-154">The configuration system is set up to read keys from environment variables.</span></span>
