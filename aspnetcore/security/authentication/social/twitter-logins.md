---
title: Configurazione dell'accesso esterno a Twitter con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione utente dell'account Twitter in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 4710c033018710ce3620f8d7221ae2253b2c0b69
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172514"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="02723-103">Configurazione dell'accesso esterno a Twitter con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02723-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="02723-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="02723-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="02723-105">Questo esempio illustra come consentire agli utenti di [accedere con il proprio account Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un progetto ASP.NET Core 3,0 di esempio creato nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="02723-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="02723-106">Creare l'app in Twitter</span><span class="sxs-lookup"><span data-stu-id="02723-106">Create the app in Twitter</span></span>

* <span data-ttu-id="02723-107">Aggiungere il pacchetto NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) al progetto.</span><span class="sxs-lookup"><span data-stu-id="02723-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="02723-108">Passare a [https://apps.twitter.com/](https://apps.twitter.com/) ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="02723-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="02723-109">Se non si ha già un account Twitter, usare il collegamento **[Iscriviti ora](https://twitter.com/signup)** per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="02723-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="02723-110">Selezionare **Create an app** (Crea un'app).</span><span class="sxs-lookup"><span data-stu-id="02723-110">Select **Create an app**.</span></span> <span data-ttu-id="02723-111">Compilare il **nome dell'app**, la **Descrizione dell'applicazione** e l'URI del **sito Web** pubblico. questa operazione può essere temporanea fino a quando non si registra il nome di dominio:</span><span class="sxs-lookup"><span data-stu-id="02723-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="02723-112">Selezionare la casella accanto a **Abilita accesso con Twitter**</span><span class="sxs-lookup"><span data-stu-id="02723-112">Check the box next to **Enable Sign in with Twitter**</span></span>

* <span data-ttu-id="02723-113">Per impostazione predefinita, Microsoft. AspNetCore. Identity richiede che gli utenti dispongano di un indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="02723-113">Microsoft.AspNetCore.Identity requires users to have an email address by default.</span></span> <span data-ttu-id="02723-114">Passare alla scheda **autorizzazioni** , fare clic sul pulsante **modifica** e selezionare la casella accanto a **Richiedi indirizzo di posta elettronica dagli utenti**.</span><span class="sxs-lookup"><span data-stu-id="02723-114">Go to the **Permissions** tab, click the **Edit** button and check the box next to **Request email address from users**.</span></span>

* <span data-ttu-id="02723-115">Immettere l'URI di sviluppo con `/signin-twitter` accodato nel campo **URL di callback** (ad esempio: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="02723-115">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="02723-116">Lo schema di autenticazione di Twitter configurato più avanti in questo esempio gestirà automaticamente le richieste in `/signin-twitter` route per implementare il flusso OAuth.</span><span class="sxs-lookup"><span data-stu-id="02723-116">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="02723-117">Il `/signin-twitter` del segmento URI è impostato come callback predefinito del provider di autenticazione Twitter.</span><span class="sxs-lookup"><span data-stu-id="02723-117">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="02723-118">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Twitter tramite la proprietà [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) ereditata della classe [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="02723-118">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="02723-119">Compilare il resto del modulo e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="02723-119">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="02723-120">Vengono visualizzati i dettagli della nuova applicazione:</span><span class="sxs-lookup"><span data-stu-id="02723-120">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="02723-121">Archiviazione della chiave e del segreto API del consumer Twitter</span><span class="sxs-lookup"><span data-stu-id="02723-121">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="02723-122">Eseguire i comandi seguenti per archiviare in modo sicuro `ClientId` e `ClientSecret` utilizzando [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="02723-122">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="02723-123">Collegare le impostazioni riservate, ad esempio Twitter `Consumer Key` e `Consumer Secret` alla configurazione dell'applicazione usando la [gestione dei segreti](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="02723-123">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="02723-124">Ai fini di questo esempio, denominare i token `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="02723-124">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="02723-125">Questi token si trovano nella scheda **chiavi e token di accesso** dopo la creazione di una nuova applicazione Twitter:</span><span class="sxs-lookup"><span data-stu-id="02723-125">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="02723-126">Configurare l'autenticazione di Twitter</span><span class="sxs-lookup"><span data-stu-id="02723-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="02723-127">Aggiungere il servizio Twitter nel metodo `ConfigureServices` nel file *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="02723-127">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="02723-128">Vedere le informazioni di riferimento sull'API [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) per altre informazioni sulle opzioni di configurazione supportate dall'autenticazione Twitter.</span><span class="sxs-lookup"><span data-stu-id="02723-128">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="02723-129">Ciò può essere usato per richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="02723-129">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="02723-130">Accedi con Twitter</span><span class="sxs-lookup"><span data-stu-id="02723-130">Sign in with Twitter</span></span>

<span data-ttu-id="02723-131">Eseguire l'app e selezionare **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="02723-131">Run the app and select **Log in**.</span></span> <span data-ttu-id="02723-132">Viene visualizzata un'opzione per accedere con Twitter:</span><span class="sxs-lookup"><span data-stu-id="02723-132">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="02723-133">Clic su **Twitter** reindirizza a Twitter per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="02723-133">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="02723-134">Dopo aver immesso le credenziali di Twitter, viene reindirizzato di nuovo al sito Web in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="02723-134">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="02723-135">A questo punto è stato effettuato l'accesso con le credenziali di Twitter:</span><span class="sxs-lookup"><span data-stu-id="02723-135">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="02723-136">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="02723-136">Troubleshooting</span></span>

* <span data-ttu-id="02723-137">**Solo ASP.NET Core 2. x:** Se l'identità non è configurata chiamando `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione determinerà *ArgumentException: è necessario fornire l'opzione ' SignInScheme '* .</span><span class="sxs-lookup"><span data-stu-id="02723-137">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="02723-138">Il modello di progetto utilizzato in questo esempio garantisce che questa operazione venga eseguita.</span><span class="sxs-lookup"><span data-stu-id="02723-138">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="02723-139">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione dell'* errore di richiesta.</span><span class="sxs-lookup"><span data-stu-id="02723-139">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="02723-140">Toccare **applica migrazioni** per creare il database e aggiornare per continuare a superare l'errore.</span><span class="sxs-lookup"><span data-stu-id="02723-140">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02723-141">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="02723-141">Next steps</span></span>

* <span data-ttu-id="02723-142">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Twitter.</span><span class="sxs-lookup"><span data-stu-id="02723-142">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="02723-143">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="02723-143">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="02723-144">Dopo aver pubblicato il sito Web nell'app Web di Azure, è necessario reimpostare il `ConsumerSecret` nel portale per sviluppatori di Twitter.</span><span class="sxs-lookup"><span data-stu-id="02723-144">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="02723-145">Impostare il `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` come impostazioni dell'applicazione nella portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="02723-145">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="02723-146">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="02723-146">The configuration system is set up to read keys from environment variables.</span></span>
