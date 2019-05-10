---
title: Configurazione dell'accesso esterno Google in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Google in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 44c79b3279db7946b6d89a726bd3f5acfb5f51af
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895538"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="03958-103">Configurazione dell'accesso esterno Google in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03958-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="03958-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="03958-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="03958-105">[Google + API legacy siano state arrestate al momento della stesura del 7 marzo 2019](https://developers.google.com/+/api-shutdown).</span><span class="sxs-lookup"><span data-stu-id="03958-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="03958-106">Google + Accedi e gli sviluppatori devono spostare in un nuovo accesso di Google nel sistema.</span><span class="sxs-lookup"><span data-stu-id="03958-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="03958-107">I pacchetti ASP.NET Core 2.1 e 2.2 per l'autenticazione di Google hanno aggiornato per contenere le modifiche apportate.</span><span class="sxs-lookup"><span data-stu-id="03958-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="03958-108">Per altre informazioni e soluzioni di attenuazione temporanee per ASP.NET Core, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="03958-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="03958-109">Questa esercitazione è stata aggiornata al nuovo processo di installazione.</span><span class="sxs-lookup"><span data-stu-id="03958-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="03958-110">Questa esercitazione illustra come consentire agli utenti di accedere con il proprio account Google usando il progetto ASP.NET Core 2.2 creato sul [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="03958-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="03958-111">Creare un ID di progetto e il client di Console API Google</span><span class="sxs-lookup"><span data-stu-id="03958-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="03958-112">Passare a [l'integrazione di Google Sign-nell'app web](https://developers.google.com/identity/sign-in/web/devconsole-project) e selezionare **configurare un progetto**.</span><span class="sxs-lookup"><span data-stu-id="03958-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="03958-113">Nel **configurare il client di OAuth** finestra di dialogo, seleziona **server Web**.</span><span class="sxs-lookup"><span data-stu-id="03958-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="03958-114">Nel **URI di reindirizzamento autorizzati** casella di immissione di testo, impostare l'URI di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="03958-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="03958-115">Ad esempio, `https://localhost:5001/signin-google`.</span><span class="sxs-lookup"><span data-stu-id="03958-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="03958-116">Salvare il **ID Client** e **privata del Client**.</span><span class="sxs-lookup"><span data-stu-id="03958-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="03958-117">Quando si distribuisce il sito, registrare il nuovo url pubblico dal **Console di Google**.</span><span class="sxs-lookup"><span data-stu-id="03958-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="03958-118">Store Google ClientID e ClientSecret</span><span class="sxs-lookup"><span data-stu-id="03958-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="03958-119">Le impostazioni sensibili, ad esempio Google Store `Client ID` e `Client Secret` con il [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="03958-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="03958-120">Ai fini di questa esercitazione, denominare il token `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="03958-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="03958-121">È possibile gestire le credenziali di API e l'utilizzo nel [API Console](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="03958-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="03958-122">Configurare l'autenticazione di Google</span><span class="sxs-lookup"><span data-stu-id="03958-122">Configure Google authentication</span></span>

<span data-ttu-id="03958-123">Aggiungere il servizio Google `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="03958-123">Add the Google service to `Startup.ConfigureServices`.</span></span>

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="03958-124">Accedi con Google</span><span class="sxs-lookup"><span data-stu-id="03958-124">Sign in with Google</span></span>

* <span data-ttu-id="03958-125">Eseguire l'app e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="03958-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="03958-126">Viene visualizzata un'opzione per accedere con Google.</span><span class="sxs-lookup"><span data-stu-id="03958-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="03958-127">Scegliere il **Google** pulsante, che reindirizza a Google per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="03958-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="03958-128">Dopo aver immesso le credenziali di Google, si verrà reindirizzati al sito web.</span><span class="sxs-lookup"><span data-stu-id="03958-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="03958-129">Vedere le [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Google.</span><span class="sxs-lookup"><span data-stu-id="03958-129">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="03958-130">Ciò può essere usato per richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="03958-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="03958-131">Modificare l'URI di callback predefinito</span><span class="sxs-lookup"><span data-stu-id="03958-131">Change the default callback URI</span></span>

<span data-ttu-id="03958-132">Il segmento URI `/signin-google` viene impostato come il callback predefinito del provider di autenticazione Google.</span><span class="sxs-lookup"><span data-stu-id="03958-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="03958-133">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Google tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="03958-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="03958-134">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="03958-134">Troubleshooting</span></span>

* <span data-ttu-id="03958-135">Se non sono presenti eventuali errori di accesso non funziona, passare alla modalità di sviluppo per rendere più semplice eseguire il debug del problema.</span><span class="sxs-lookup"><span data-stu-id="03958-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="03958-136">Se non è configurato identità chiamando `services.AddIdentity` nelle `ConfigureServices`, quando si prova a eseguire l'autenticazione dei risultati in *ArgumentException: È necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="03958-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="03958-137">Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="03958-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="03958-138">Se il database del sito non è stato creato applicando la migrazione iniziale, si ottiene *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="03958-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="03958-139">Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.</span><span class="sxs-lookup"><span data-stu-id="03958-139">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="03958-140">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="03958-140">Next steps</span></span>

* <span data-ttu-id="03958-141">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Google.</span><span class="sxs-lookup"><span data-stu-id="03958-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="03958-142">È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="03958-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="03958-143">Dopo aver pubblicato l'app in Azure, reimpostare il `ClientSecret` nella Console API di Google.</span><span class="sxs-lookup"><span data-stu-id="03958-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="03958-144">Impostare il `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="03958-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="03958-145">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="03958-145">The configuration system is set up to read keys from environment variables.</span></span>
