---
title: Configurazione dell'accesso esterno all'account Microsoft con ASP.NET Core
author: rick-anderson
description: In questo esempio viene illustrata l'integrazione di account Microsoft autenticazione utente in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 12/4/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: ddaae1a25a1dcf167ffae0f24b480e2cde6aca5b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659796"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="6ea1a-103">Configurazione dell'accesso esterno all'account Microsoft con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ea1a-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="6ea1a-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6ea1a-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6ea1a-105">Questo esempio illustra come consentire agli utenti di accedere con la loro account Microsoft usando il progetto ASP.NET Core 3,0 creato nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="6ea1a-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="6ea1a-106">Creare l'app nel portale per sviluppatori Microsoft</span><span class="sxs-lookup"><span data-stu-id="6ea1a-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="6ea1a-107">Aggiungere il pacchetto NuGet [Microsoft. AspNetCore. Authentication. MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) al progetto.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-107">Add the [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet package to the project.</span></span>
* <span data-ttu-id="6ea1a-108">Passare alla pagina [portale di Azure-registrazioni app](https://go.microsoft.com/fwlink/?linkid=2083908) e creare o accedere a un account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="6ea1a-108">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="6ea1a-109">Se non si dispone di un account Microsoft, selezionare **crearne uno**.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-109">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="6ea1a-110">Dopo aver eseguito l'accesso, si verrà reindirizzati alla pagina **registrazioni app** :</span><span class="sxs-lookup"><span data-stu-id="6ea1a-110">After signing in, you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="6ea1a-111">Seleziona **nuova registrazione**</span><span class="sxs-lookup"><span data-stu-id="6ea1a-111">Select **New registration**</span></span>
* <span data-ttu-id="6ea1a-112">Immettere un **Nome**.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-112">Enter a **Name**.</span></span>
* <span data-ttu-id="6ea1a-113">Selezionare un'opzione per i **tipi di account supportati**.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-113">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="6ea1a-114">In **URI di reindirizzamento**immettere l'URL di sviluppo con `/signin-microsoft` accodato.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-114">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="6ea1a-115">Ad esempio: `https://localhost:5001/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-115">For example, `https://localhost:5001/signin-microsoft`.</span></span> <span data-ttu-id="6ea1a-116">Lo schema di autenticazione Microsoft configurato più avanti in questo esempio gestirà automaticamente le richieste in `/signin-microsoft` route per implementare il flusso OAuth.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-116">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="6ea1a-117">Seleziona **Registro**</span><span class="sxs-lookup"><span data-stu-id="6ea1a-117">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="6ea1a-118">Crea segreto client</span><span class="sxs-lookup"><span data-stu-id="6ea1a-118">Create client secret</span></span>

* <span data-ttu-id="6ea1a-119">Nel riquadro sinistro selezionare **certificati & segreti**.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-119">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="6ea1a-120">In **segreti client**selezionare **nuovo segreto client**</span><span class="sxs-lookup"><span data-stu-id="6ea1a-120">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="6ea1a-121">Aggiungere una descrizione per il segreto client.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-121">Add a description for the client secret.</span></span>
  * <span data-ttu-id="6ea1a-122">Fare clic sul pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-122">Select the **Add** button.</span></span>

* <span data-ttu-id="6ea1a-123">In **segreti client**copiare il valore del segreto client.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-123">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="6ea1a-124">Il `/signin-microsoft` del segmento URI è impostato come callback predefinito del provider di autenticazione Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-124">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="6ea1a-125">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Microsoft tramite la proprietà [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) ereditata della classe [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="6ea1a-125">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="6ea1a-126">Archiviare l'ID client e il segreto client Microsoft</span><span class="sxs-lookup"><span data-stu-id="6ea1a-126">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="6ea1a-127">Eseguire i comandi seguenti per archiviare in modo sicuro `ClientId` e `ClientSecret` utilizzando [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="6ea1a-127">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="6ea1a-128">Collegare le impostazioni sensibili come Microsoft `ClientId` e `ClientSecret` alla configurazione dell'applicazione usando la [gestione dei segreti](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="6ea1a-128">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="6ea1a-129">Ai fini di questo esempio, denominare i token `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-129">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="6ea1a-130">Configurare l'autenticazione dell'account Microsoft</span><span class="sxs-lookup"><span data-stu-id="6ea1a-130">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="6ea1a-131">Aggiungere il servizio account Microsoft alla `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6ea1a-131">Add the Microsoft Account service to the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="6ea1a-132">Per altre informazioni sulle opzioni di configurazione supportate dall'autenticazione dell'account Microsoft, vedere le informazioni di riferimento sull'API [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="6ea1a-132">For more information about configuration options supported by Microsoft Account authentication, see the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference.</span></span> <span data-ttu-id="6ea1a-133">Ciò può essere usato per richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="6ea1a-134">Account Accedi con Microsoft</span><span class="sxs-lookup"><span data-stu-id="6ea1a-134">Sign in with Microsoft Account</span></span>

<span data-ttu-id="6ea1a-135">Eseguire l'app e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-135">Run the app and click **Log in**.</span></span> <span data-ttu-id="6ea1a-136">Viene visualizzata un'opzione per accedere con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-136">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="6ea1a-137">Quando si fa clic su Microsoft, si viene reindirizzati a Microsoft per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-137">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="6ea1a-138">Dopo aver eseguito l'accesso con l'account Microsoft, verrà richiesto di consentire all'app di accedere alle informazioni:</span><span class="sxs-lookup"><span data-stu-id="6ea1a-138">After signing in with your Microsoft Account, you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="6ea1a-139">Toccare **Sì** e si verrà reindirizzati di nuovo al sito Web in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-139">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="6ea1a-140">A questo punto è stato effettuato l'accesso con le credenziali Microsoft:</span><span class="sxs-lookup"><span data-stu-id="6ea1a-140">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="6ea1a-141">risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="6ea1a-141">Troubleshooting</span></span>

* <span data-ttu-id="6ea1a-142">Se il provider di account Microsoft reindirizza l'utente a una pagina di errore di accesso, prendere nota dei parametri della stringa di query titolo e Descrizione dell'errore direttamente dopo il `#` (hashtag) nell'URI.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-142">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="6ea1a-143">Anche se il messaggio di errore sembra indicare un problema con l'autenticazione Microsoft, la causa più comune è l'URI dell'applicazione che non corrisponde ad alcuno degli **URI di reindirizzamento** specificati per la piattaforma **Web** .</span><span class="sxs-lookup"><span data-stu-id="6ea1a-143">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="6ea1a-144">Se l'identità non è configurata chiamando `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione determinerà *ArgumentException: è necessario fornire l'opzione ' SignInScheme '* .</span><span class="sxs-lookup"><span data-stu-id="6ea1a-144">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="6ea1a-145">Il modello di progetto utilizzato in questo esempio garantisce che questa operazione venga eseguita.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-145">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="6ea1a-146">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione dell'* errore di richiesta.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="6ea1a-147">Toccare **applica migrazioni** per creare il database e aggiornare per continuare a superare l'errore.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ea1a-148">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6ea1a-148">Next steps</span></span>

* <span data-ttu-id="6ea1a-149">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-149">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="6ea1a-150">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="6ea1a-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="6ea1a-151">Dopo aver pubblicato il sito Web nell'app Web di Azure, creare un nuovo segreto client nel portale per sviluppatori Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-151">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="6ea1a-152">Impostare il `Authentication:Microsoft:ClientId` e `Authentication:Microsoft:ClientSecret` come impostazioni dell'applicazione nella portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-152">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="6ea1a-153">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="6ea1a-153">The configuration system is set up to read keys from environment variables.</span></span>
