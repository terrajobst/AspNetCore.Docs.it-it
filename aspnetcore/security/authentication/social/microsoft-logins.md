---
title: Configurazione accesso esterno dell'Account Microsoft con ASP.NET Core
author: rick-anderson
description: Questo esempio illustra l'integrazione dell'autenticazione di Microsoft account utente in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 2c690e5bd8465806d42091616917cfdd747ef8f0
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815566"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="2608b-103">Configurazione accesso esterno dell'Account Microsoft con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2608b-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="2608b-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2608b-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2608b-105">In questo esempio illustra come consentire agli utenti di accedere con il proprio account Microsoft usando il progetto ASP.NET Core 2.2 creato sul [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2608b-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="2608b-106">Creare l'app nel portale per sviluppatori Microsoft</span><span class="sxs-lookup"><span data-stu-id="2608b-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="2608b-107">Passare il [portale di Azure - registrazioni di App](https://go.microsoft.com/fwlink/?linkid=2083908) pagina e creare o accedere a un account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="2608b-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="2608b-108">Se non hai un account Microsoft, selezionare **crearne uno**.</span><span class="sxs-lookup"><span data-stu-id="2608b-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="2608b-109">Dopo l'accesso si verrà reindirizzati per il **registrazioni per l'App** pagina:</span><span class="sxs-lookup"><span data-stu-id="2608b-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="2608b-110">Selezionare **nuova registrazione**</span><span class="sxs-lookup"><span data-stu-id="2608b-110">Select **New registration**</span></span>
* <span data-ttu-id="2608b-111">Immettere un **nome**.</span><span class="sxs-lookup"><span data-stu-id="2608b-111">Enter a **Name**.</span></span>
* <span data-ttu-id="2608b-112">Selezionare un'opzione per **tipi di account supportati**.</span><span class="sxs-lookup"><span data-stu-id="2608b-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="2608b-113">Sotto **URI di reindirizzamento**, immettere l'URL di sviluppo con `/signin-microsoft` aggiunto.</span><span class="sxs-lookup"><span data-stu-id="2608b-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="2608b-114">Ad esempio `https://localhost:44389/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="2608b-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="2608b-115">Lo schema di autenticazione Microsoft configurato più avanti in questo esempio consente di gestire automaticamente le richieste a `/signin-microsoft` route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="2608b-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="2608b-116">Selezionare **registrare**</span><span class="sxs-lookup"><span data-stu-id="2608b-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="2608b-117">Creare il segreto client</span><span class="sxs-lookup"><span data-stu-id="2608b-117">Create client secret</span></span>

* <span data-ttu-id="2608b-118">Nel riquadro sinistro, selezionare **certificati e i segreti**.</span><span class="sxs-lookup"><span data-stu-id="2608b-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="2608b-119">Sotto **i segreti Client**, selezionare **nuovo segreto client**</span><span class="sxs-lookup"><span data-stu-id="2608b-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="2608b-120">Aggiungere una descrizione per il segreto client.</span><span class="sxs-lookup"><span data-stu-id="2608b-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="2608b-121">Selezionare il **Add** pulsante.</span><span class="sxs-lookup"><span data-stu-id="2608b-121">Select the **Add** button.</span></span>

* <span data-ttu-id="2608b-122">Sotto **i segreti Client**, copiare il valore del segreto client.</span><span class="sxs-lookup"><span data-stu-id="2608b-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="2608b-123">Il segmento URI `/signin-microsoft` viene impostato come il callback predefinito del provider di autenticazione Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2608b-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="2608b-124">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Microsoft tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà del [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="2608b-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="2608b-125">Il segreto client e ID del client Microsoft Store</span><span class="sxs-lookup"><span data-stu-id="2608b-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="2608b-126">Eseguire i comandi seguenti per archiviare in modo sicuro `ClientId` e `ClientSecret` utilizzando [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="2608b-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="2608b-127">Collegare le impostazioni sensibili, ad esempio Microsoft `ClientId` e `ClientSecret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2608b-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="2608b-128">Ai fini di questo esempio, assegnare un nome token `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="2608b-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="2608b-129">Configurare l'autenticazione Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="2608b-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="2608b-130">Aggiungere il servizio Account Microsoft a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2608b-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="2608b-131">Vedere le [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2608b-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="2608b-132">Ciò può essere usato per richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="2608b-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="2608b-133">Accedi con Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="2608b-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="2608b-134">Eseguire il e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="2608b-134">Run the and click **Log in**.</span></span> <span data-ttu-id="2608b-135">Viene visualizzata un'opzione per accedere con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2608b-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="2608b-136">Quando fa clic su Microsoft, si verrà reindirizzati a Microsoft per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2608b-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="2608b-137">Dopo l'accesso con l'Account Microsoft (se non è già stato effettuato l'accesso) verrà richiesto per consentire all'app di accedere alle tue info:</span><span class="sxs-lookup"><span data-stu-id="2608b-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="2608b-138">Toccare **Sì** e si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="2608b-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="2608b-139">A questo punto si è connessi con le credenziali di Microsoft:</span><span class="sxs-lookup"><span data-stu-id="2608b-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="2608b-140">risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="2608b-140">Troubleshooting</span></span>

* <span data-ttu-id="2608b-141">Se il provider dell'Account di Microsoft si viene reindirizzati a una pagina di errore di accesso, si noti l'errore title e description parametri stringa di query che seguono direttamente il `#` (hashtag) nell'Uri.</span><span class="sxs-lookup"><span data-stu-id="2608b-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="2608b-142">Anche se sembra che il messaggio di errore per indicare un problema con l'autenticazione di Microsoft, la causa più comune è l'Uri non corrisponda a uno di applicazione la **Redirect URIs** specificato per il **Web** piattaforma .</span><span class="sxs-lookup"><span data-stu-id="2608b-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="2608b-143">Se Identity non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: È necessario specificare l'opzione 'SignInScheme'* .</span><span class="sxs-lookup"><span data-stu-id="2608b-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="2608b-144">Il modello di progetto usato in questo esempio garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="2608b-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="2608b-145">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="2608b-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="2608b-146">Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.</span><span class="sxs-lookup"><span data-stu-id="2608b-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2608b-147">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="2608b-147">Next steps</span></span>

* <span data-ttu-id="2608b-148">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2608b-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="2608b-149">È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2608b-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="2608b-150">Dopo aver pubblicato il sito web all'app web di Azure, creare un nuovo client i segreti nel portale per sviluppatori Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2608b-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="2608b-151">Impostare il `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="2608b-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="2608b-152">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="2608b-152">The configuration system is set up to read keys from environment variables.</span></span>