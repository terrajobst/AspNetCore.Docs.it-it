---
title: Configurazione accesso esterno con ASP.NET Core di Twitter
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Twitter in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: d816ed27898639b0af6896a51ac035d5526c5d29
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814065"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="fea99-103">Configurazione accesso esterno con ASP.NET Core di Twitter</span><span class="sxs-lookup"><span data-stu-id="fea99-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="fea99-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fea99-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fea99-105">In questo esempio viene illustrato come consentire agli utenti [accedere con l'account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un progetto ASP.NET Core 2.2 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="fea99-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="fea99-106">Creare l'app in Twitter</span><span class="sxs-lookup"><span data-stu-id="fea99-106">Create the app in Twitter</span></span>

* <span data-ttu-id="fea99-107">Passare a [ https://apps.twitter.com/ ](https://apps.twitter.com/) ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="fea99-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="fea99-108">Se non si ha già un account Twitter, usare il **[iscriversi adesso](https://twitter.com/signup)** collegamento per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="fea99-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="fea99-109">Toccare **Create New App** e compilare l'applicazione **Name**, **descrizione** e pubblica **sito Web** URI (può essere temporaneo fino a registrare il nome di dominio):</span><span class="sxs-lookup"><span data-stu-id="fea99-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="fea99-110">Immettere l'URI di sviluppo con `/signin-twitter` aggiunto nel **URI di reindirizzamento OAuth validi** campo (ad esempio: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="fea99-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="fea99-111">Lo schema di autenticazione di Twitter configurato più avanti in questo esempio consente di gestire automaticamente le richieste a `/signin-twitter` route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="fea99-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="fea99-112">Il segmento URI `/signin-twitter` viene impostato come il callback predefinito del provider di autenticazione di Twitter.</span><span class="sxs-lookup"><span data-stu-id="fea99-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="fea99-113">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione di Twitter tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.</span><span class="sxs-lookup"><span data-stu-id="fea99-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="fea99-114">Compilare il resto del form e toccare **Crea applicazione Twitter**.</span><span class="sxs-lookup"><span data-stu-id="fea99-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="fea99-115">Vengono visualizzati nuovi dettagli dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="fea99-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="fea99-116">L'archiviazione di chiavi API Consumer di Twitter e il segreto</span><span class="sxs-lookup"><span data-stu-id="fea99-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="fea99-117">Eseguire i comandi seguenti per archiviare in modo sicuro `ClientId` e `ClientSecret` utilizzando [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="fea99-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

<span data-ttu-id="fea99-118">Collegare le impostazioni sensibili, ad esempio Twitter `Consumer Key` e `Consumer Secret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="fea99-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="fea99-119">Ai fini di questo esempio, assegnare un nome token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="fea99-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="fea99-120">Questi token sono reperibile nella **Keys and Access Tokens** scheda dopo aver creato una nuova applicazione Twitter:</span><span class="sxs-lookup"><span data-stu-id="fea99-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="fea99-121">Configurare l'autenticazione di Twitter</span><span class="sxs-lookup"><span data-stu-id="fea99-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="fea99-122">Aggiungere il servizio di Twitter nel `ConfigureServices` nel metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="fea99-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="fea99-123">Vedere le [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Twitter.</span><span class="sxs-lookup"><span data-stu-id="fea99-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="fea99-124">Ciò può essere usato per richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="fea99-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="fea99-125">Accedi con Twitter</span><span class="sxs-lookup"><span data-stu-id="fea99-125">Sign in with Twitter</span></span>

<span data-ttu-id="fea99-126">Eseguire l'app e selezionare **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="fea99-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="fea99-127">Viene visualizzata un'opzione per accedere con Twitter:</span><span class="sxs-lookup"><span data-stu-id="fea99-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="fea99-128">Facendo clic su **Twitter** reindirizza a Twitter per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="fea99-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="fea99-129">Dopo aver immesso le credenziali di Twitter, si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="fea99-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="fea99-130">A questo punto si è connessi con le credenziali di Twitter:</span><span class="sxs-lookup"><span data-stu-id="fea99-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="fea99-131">risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="fea99-131">Troubleshooting</span></span>

* <span data-ttu-id="fea99-132">**ASP.NET Core 2.x solo:** Se Identity non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: È necessario specificare l'opzione 'SignInScheme'* .</span><span class="sxs-lookup"><span data-stu-id="fea99-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="fea99-133">Il modello di progetto usato in questo esempio garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="fea99-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="fea99-134">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="fea99-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="fea99-135">Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.</span><span class="sxs-lookup"><span data-stu-id="fea99-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fea99-136">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fea99-136">Next steps</span></span>

* <span data-ttu-id="fea99-137">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Twitter.</span><span class="sxs-lookup"><span data-stu-id="fea99-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="fea99-138">È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="fea99-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="fea99-139">Dopo aver pubblicato il sito web all'app web di Azure, è consigliabile reimpostare il `ConsumerSecret` del portale per sviluppatori di Twitter.</span><span class="sxs-lookup"><span data-stu-id="fea99-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="fea99-140">Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="fea99-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="fea99-141">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="fea99-141">The configuration system is set up to read keys from environment variables.</span></span>