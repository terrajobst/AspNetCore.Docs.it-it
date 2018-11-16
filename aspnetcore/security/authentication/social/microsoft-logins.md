---
title: Configurazione accesso esterno dell'Account Microsoft con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione di Microsoft account utente in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 89969370cea66b7b6632f1b0be59e135767c831e
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708400"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="cebbe-103">Configurazione accesso esterno dell'Account Microsoft con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cebbe-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="cebbe-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cebbe-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cebbe-105">Questa esercitazione illustra come consentire agli utenti di accedere con il proprio account Microsoft usando un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="cebbe-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="cebbe-106">Creare l'app nel portale per sviluppatori Microsoft</span><span class="sxs-lookup"><span data-stu-id="cebbe-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="cebbe-107">Passare a [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) e creare o accedere a un account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="cebbe-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![L'accesso nella finestra di dialogo](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="cebbe-109">Se non si ha già un account Microsoft, toccare  **[crearne uno.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="cebbe-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="cebbe-110">Dopo l'accesso si verrà reindirizzati alla **mie applicazioni** pagina:</span><span class="sxs-lookup"><span data-stu-id="cebbe-110">After signing in you are redirected to **My applications** page:</span></span>

![Portale per sviluppatori Microsoft Apri in Microsoft Edge](index/_static/MicrosoftDev.png)

* <span data-ttu-id="cebbe-112">Toccare **aggiungere un'app** in alto a destra nell'angolo e immettere il **nome applicazione** e **Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="cebbe-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Finestra di dialogo Nuova registrazione dell'applicazione](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="cebbe-114">Ai fini di questa esercitazione, cancellare il **installazione guidata** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="cebbe-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="cebbe-115">Toccare **Create** per continuare al **registrazione** pagina.</span><span class="sxs-lookup"><span data-stu-id="cebbe-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="cebbe-116">Fornire un **nome** e prendere nota del valore della **Id applicazione**, che è usato come `ClientId` più avanti nell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="cebbe-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Pagina di registrazione](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="cebbe-118">Toccare **Aggiungi piattaforma** nel **piattaforme** sezione e selezionare il **Web** piattaforma:</span><span class="sxs-lookup"><span data-stu-id="cebbe-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Aggiungi finestra di dialogo piattaforma](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="cebbe-120">Nel nuovo **Web** piattaforma, quindi immettere l'URL di sviluppo con `/signin-microsoft` aggiunto nel **URL di reindirizzamento** campo (ad esempio: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="cebbe-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="cebbe-121">Lo schema di autenticazione Microsoft configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste a `/signin-microsoft` route per implementare il flusso di OAuth:</span><span class="sxs-lookup"><span data-stu-id="cebbe-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Sezione relativa alla piattaforma Web](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="cebbe-123">Il segmento URI `/signin-microsoft` viene impostato come il callback predefinito del provider di autenticazione Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cebbe-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="cebbe-124">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Microsoft tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà del [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="cebbe-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="cebbe-125">Toccare **Aggiungi URL** per assicurarsi che l'URL è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="cebbe-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="cebbe-126">Compilare le altre impostazioni dell'applicazione se necessario e toccare **salvare** nella parte inferiore della pagina per salvare le modifiche alla configurazione delle app.</span><span class="sxs-lookup"><span data-stu-id="cebbe-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="cebbe-127">Quando si distribuisce il sito è necessario rivedere le **registrazione** pagina e impostare un nuovo URL pubblico.</span><span class="sxs-lookup"><span data-stu-id="cebbe-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="cebbe-128">Store Id applicazione di Microsoft e la Password</span><span class="sxs-lookup"><span data-stu-id="cebbe-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="cebbe-129">Si noti il `Application Id` visualizzato nella **registrazione** pagina.</span><span class="sxs-lookup"><span data-stu-id="cebbe-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="cebbe-130">Toccare **Genera nuova Password** nel **i segreti dell'applicazione** sezione.</span><span class="sxs-lookup"><span data-stu-id="cebbe-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="cebbe-131">Verrà visualizzata una finestra in cui è possibile copiare la password dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="cebbe-131">This displays a box where you can copy the application password:</span></span>

![Finestra di dialogo nuova password generata](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="cebbe-133">Collegare le impostazioni sensibili, ad esempio Microsoft `Application ID` e `Password` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="cebbe-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="cebbe-134">Ai fini di questa esercitazione, denominare il token `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="cebbe-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="cebbe-135">Configurare l'autenticazione Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="cebbe-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="cebbe-136">Il modello di progetto usato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pacchetto è già installato.</span><span class="sxs-lookup"><span data-stu-id="cebbe-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="cebbe-137">Per installare questo pacchetto con Visual Studio 2017, pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cebbe-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="cebbe-138">Per installare con CLI di .NET Core, eseguire il codice seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="cebbe-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="cebbe-139">Aggiungere il servizio Account Microsoft nel `ConfigureServices` nel metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="cebbe-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="cebbe-140">Aggiungere il middleware di Account Microsoft nel `Configure` nel metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="cebbe-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

<span data-ttu-id="cebbe-141">Sebbene la terminologia utilizzata nel portale per sviluppatori Microsoft assegna questi token `ApplicationId` e `Password`, è esposta come `ClientId` e `ClientSecret` nell'API di configurazione.</span><span class="sxs-lookup"><span data-stu-id="cebbe-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="cebbe-142">Vedere le [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cebbe-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="cebbe-143">Ciò può essere usato per richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="cebbe-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="cebbe-144">Accedi con Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="cebbe-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="cebbe-145">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="cebbe-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="cebbe-146">Viene visualizzata un'opzione per accedere con Microsoft:</span><span class="sxs-lookup"><span data-stu-id="cebbe-146">An option to sign in with Microsoft appears:</span></span>

![Applicazione del Log nella pagina Web: utente non autenticato](index/_static/DoneMicrosoft.png)

<span data-ttu-id="cebbe-148">Quando fa clic su Microsoft, si verrà reindirizzati a Microsoft per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="cebbe-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="cebbe-149">Dopo l'accesso con l'Account Microsoft (se non è già stato effettuato l'accesso) verrà richiesto per consentire all'app di accedere alle tue info:</span><span class="sxs-lookup"><span data-stu-id="cebbe-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Finestra di dialogo autenticazione Microsoft](index/_static/MicrosoftLogin.png)

<span data-ttu-id="cebbe-151">Toccare **Sì** e si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="cebbe-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="cebbe-152">A questo punto si è connessi con le credenziali di Microsoft:</span><span class="sxs-lookup"><span data-stu-id="cebbe-152">You are now logged in using your Microsoft credentials:</span></span>

![Applicazione Web: utente autenticato](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="cebbe-154">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="cebbe-154">Troubleshooting</span></span>

* <span data-ttu-id="cebbe-155">Se il provider dell'Account di Microsoft si viene reindirizzati a una pagina di errore di accesso, si noti l'errore title e description parametri stringa di query che seguono direttamente il `#` (hashtag) nell'Uri.</span><span class="sxs-lookup"><span data-stu-id="cebbe-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="cebbe-156">Anche se sembra che il messaggio di errore per indicare un problema con l'autenticazione di Microsoft, la causa più comune è l'Uri non corrisponda a uno di applicazione la **Redirect URIs** specificato per il **Web** piattaforma .</span><span class="sxs-lookup"><span data-stu-id="cebbe-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="cebbe-157">**ASP.NET Core 2.x solo:** Identity se non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="cebbe-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="cebbe-158">Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="cebbe-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="cebbe-159">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="cebbe-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="cebbe-160">Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.</span><span class="sxs-lookup"><span data-stu-id="cebbe-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cebbe-161">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="cebbe-161">Next steps</span></span>

* <span data-ttu-id="cebbe-162">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cebbe-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="cebbe-163">È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="cebbe-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="cebbe-164">Dopo aver pubblicato il sito web all'app web di Azure, è necessario creare un nuovo `Password` del portale per sviluppatori Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cebbe-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="cebbe-165">Impostare il `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="cebbe-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="cebbe-166">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="cebbe-166">The configuration system is set up to read keys from environment variables.</span></span>
