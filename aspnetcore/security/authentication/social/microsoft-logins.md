---
title: Configurazione dell'accesso esterno all'account Microsoft con ASP.NET Core
author: rick-anderson
description: In questo esempio viene illustrata l'integrazione di account Microsoft autenticazione utente in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 91ace293fd16cd180b3d5c183c637af6db1d08c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082336"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="cf61f-103">Configurazione dell'accesso esterno all'account Microsoft con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf61f-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="cf61f-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cf61f-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cf61f-105">Questo esempio illustra come consentire agli utenti di accedere con la loro account Microsoft usando il progetto ASP.NET Core 2,2 creato nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="cf61f-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="cf61f-106">Creare l'app nel portale per sviluppatori Microsoft</span><span class="sxs-lookup"><span data-stu-id="cf61f-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="cf61f-107">Passare alla pagina [portale di Azure-registrazioni app](https://go.microsoft.com/fwlink/?linkid=2083908) e creare o accedere a un account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="cf61f-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="cf61f-108">Se non si dispone di un account Microsoft, selezionare **crearne uno**.</span><span class="sxs-lookup"><span data-stu-id="cf61f-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="cf61f-109">Dopo aver eseguito l'accesso, si verrà reindirizzati alla pagina **registrazioni app** :</span><span class="sxs-lookup"><span data-stu-id="cf61f-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="cf61f-110">Seleziona **nuova registrazione**</span><span class="sxs-lookup"><span data-stu-id="cf61f-110">Select **New registration**</span></span>
* <span data-ttu-id="cf61f-111">Immettere un **nome**.</span><span class="sxs-lookup"><span data-stu-id="cf61f-111">Enter a **Name**.</span></span>
* <span data-ttu-id="cf61f-112">Selezionare un'opzione per i **tipi di account supportati**.</span><span class="sxs-lookup"><span data-stu-id="cf61f-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="cf61f-113">In **URI di reindirizzamento**immettere l'URL di sviluppo `/signin-microsoft` con accodato.</span><span class="sxs-lookup"><span data-stu-id="cf61f-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="cf61f-114">Ad esempio `https://localhost:44389/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="cf61f-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="cf61f-115">Lo schema di autenticazione Microsoft configurato più avanti in questo esempio gestirà automaticamente `/signin-microsoft` le richieste in route per implementare il flusso OAuth.</span><span class="sxs-lookup"><span data-stu-id="cf61f-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="cf61f-116">Seleziona **Registro**</span><span class="sxs-lookup"><span data-stu-id="cf61f-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="cf61f-117">Crea segreto client</span><span class="sxs-lookup"><span data-stu-id="cf61f-117">Create client secret</span></span>

* <span data-ttu-id="cf61f-118">Nel riquadro sinistro selezionare **certificati & segreti**.</span><span class="sxs-lookup"><span data-stu-id="cf61f-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="cf61f-119">In **segreti client**selezionare **nuovo segreto client**</span><span class="sxs-lookup"><span data-stu-id="cf61f-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="cf61f-120">Aggiungere una descrizione per il segreto client.</span><span class="sxs-lookup"><span data-stu-id="cf61f-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="cf61f-121">Selezionare il pulsante **Aggiungi** .</span><span class="sxs-lookup"><span data-stu-id="cf61f-121">Select the **Add** button.</span></span>

* <span data-ttu-id="cf61f-122">In **segreti client**copiare il valore del segreto client.</span><span class="sxs-lookup"><span data-stu-id="cf61f-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="cf61f-123">Il segmento `/signin-microsoft` URI viene impostato come callback predefinito del provider di autenticazione Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cf61f-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="cf61f-124">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Microsoft tramite la proprietà [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) ereditata della classe [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="cf61f-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="cf61f-125">Archiviare l'ID client e il segreto client Microsoft</span><span class="sxs-lookup"><span data-stu-id="cf61f-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="cf61f-126">Eseguire i comandi seguenti per archiviare `ClientId` e `ClientSecret` in modo sicuro e usare [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="cf61f-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="cf61f-127">Collegare le impostazioni sensibili come `ClientId` Microsoft `ClientSecret` e alla configurazione dell'applicazione usando la [gestione dei segreti](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="cf61f-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="cf61f-128">Ai fini di questo esempio, denominare i token `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="cf61f-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="cf61f-129">Configurare l'autenticazione dell'account Microsoft</span><span class="sxs-lookup"><span data-stu-id="cf61f-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="cf61f-130">Aggiungere il servizio account Microsoft a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cf61f-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="cf61f-131">Vedere le informazioni di riferimento sull'API [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) per altre informazioni sulle opzioni di configurazione supportate dall'autenticazione dell'account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cf61f-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="cf61f-132">Ciò può essere usato per richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="cf61f-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="cf61f-133">Account Accedi con Microsoft</span><span class="sxs-lookup"><span data-stu-id="cf61f-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="cf61f-134">Eseguire e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="cf61f-134">Run the and click **Log in**.</span></span> <span data-ttu-id="cf61f-135">Viene visualizzata un'opzione per accedere con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cf61f-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="cf61f-136">Quando si fa clic su Microsoft, si viene reindirizzati a Microsoft per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="cf61f-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="cf61f-137">Dopo aver eseguito l'accesso con l'account Microsoft (se non è già stato effettuato l'accesso), verrà richiesto di consentire all'app di accedere alle informazioni:</span><span class="sxs-lookup"><span data-stu-id="cf61f-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="cf61f-138">Toccare **Sì** e si verrà reindirizzati di nuovo al sito Web in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="cf61f-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="cf61f-139">A questo punto è stato effettuato l'accesso con le credenziali Microsoft:</span><span class="sxs-lookup"><span data-stu-id="cf61f-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="cf61f-140">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="cf61f-140">Troubleshooting</span></span>

* <span data-ttu-id="cf61f-141">Se il provider di account Microsoft reindirizza l'utente a una pagina di errore di accesso, prendere nota dei parametri della stringa di query titolo e descrizione `#` dell'errore direttamente dopo l'hashtag nell'URI.</span><span class="sxs-lookup"><span data-stu-id="cf61f-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="cf61f-142">Anche se il messaggio di errore sembra indicare un problema con l'autenticazione Microsoft, la causa più comune è l'URI dell'applicazione che non corrisponde ad alcuno degli **URI di reindirizzamento** specificati per la piattaforma **Web** .</span><span class="sxs-lookup"><span data-stu-id="cf61f-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="cf61f-143">Se l'identità non è configurata chiamando `services.AddIdentity` in `ConfigureServices`, il tentativo di *eseguire l'autenticazione comporterà l'eccezione ArgumentException: È necessario specificare l'opzione ' SignInScheme '* .</span><span class="sxs-lookup"><span data-stu-id="cf61f-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="cf61f-144">Il modello di progetto utilizzato in questo esempio garantisce che questa operazione venga eseguita.</span><span class="sxs-lookup"><span data-stu-id="cf61f-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="cf61f-145">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="cf61f-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="cf61f-146">Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.</span><span class="sxs-lookup"><span data-stu-id="cf61f-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf61f-147">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="cf61f-147">Next steps</span></span>

* <span data-ttu-id="cf61f-148">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cf61f-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="cf61f-149">È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="cf61f-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="cf61f-150">Dopo aver pubblicato il sito Web nell'app Web di Azure, creare un nuovo segreto client nel portale per sviluppatori Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cf61f-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="cf61f-151">Impostare il `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="cf61f-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="cf61f-152">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="cf61f-152">The configuration system is set up to read keys from environment variables.</span></span>