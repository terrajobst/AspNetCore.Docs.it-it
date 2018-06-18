---
title: Configurazione dell'account di accesso esterno Account Microsoft con ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione di Microsoft account utente in un'applicazione ASP.NET di base esistente.
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 46973f8a82034bd99a6e6634bbd6da06b1b14f25
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726030"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="63095-103">Configurazione dell'account di accesso esterno Account Microsoft con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="63095-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="63095-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="63095-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="63095-105">In questa esercitazione viene illustrato come consentire agli utenti di accedere con l'account di Microsoft tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="63095-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="63095-106">Creare l'app nel portale per sviluppatori di Microsoft</span><span class="sxs-lookup"><span data-stu-id="63095-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="63095-107">Passare a [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) e creare o accedere a un account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="63095-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Accedere alla finestra di dialogo](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="63095-109">Se non si dispone già di un account Microsoft, tocca  **[crearne uno.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="63095-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="63095-110">Dopo l'accesso, si verrà reindirizzati a **applicazioni personali** pagina:</span><span class="sxs-lookup"><span data-stu-id="63095-110">After signing in you are redirected to **My applications** page:</span></span>

![Portale per sviluppatori Microsoft aperto in Microsoft Edge](index/_static/MicrosoftDev.png)

* <span data-ttu-id="63095-112">Toccare **aggiungere un'app** in alto a destra angolo e immettere il **nome applicazione** e **Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="63095-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Finestra di dialogo Nuova registrazione dell'applicazione](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="63095-114">Ai fini di questa esercitazione, cancellare il **impostazione guidata** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="63095-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="63095-115">Toccare **crea** per continuare al **registrazione** pagina.</span><span class="sxs-lookup"><span data-stu-id="63095-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="63095-116">Fornire un **nome** e prendere nota del valore del **Id applicazione**, utilizzabile come `ClientId` più avanti nell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="63095-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Pagina di registrazione](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="63095-118">Toccare **aggiungere piattaforma** nel **piattaforme** sezione e selezionare il **Web** piattaforma:</span><span class="sxs-lookup"><span data-stu-id="63095-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Finestra di dialogo piattaforma Aggiungi](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="63095-120">Nel nuovo **Web** piattaforma sezione, immettere l'URL di sviluppo con `/signin-microsoft` aggiunto nel **URL di reindirizzamento** campo (ad esempio: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="63095-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="63095-121">Lo schema di autenticazione Microsoft configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel `/signin-microsoft` route per implementare il flusso di OAuth:</span><span class="sxs-lookup"><span data-stu-id="63095-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Sezione relativa alla piattaforma Web](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="63095-123">Il segmento URI `/signin-microsoft` viene impostato come callback predefinito del provider di autenticazione Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63095-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="63095-124">È possibile modificare l'URI di callback predefinito durante la configurazione il middleware di autenticazione Microsoft tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà del [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="63095-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="63095-125">Toccare **Aggiungi URL** per verificare l'URL è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="63095-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="63095-126">Compilare tutte le altre impostazioni applicazione, se necessario e toccare **salvare** nella parte inferiore della pagina per salvare le modifiche alla configurazione di app.</span><span class="sxs-lookup"><span data-stu-id="63095-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="63095-127">Quando si distribuisce il sito è necessario rivedere il **registrazione** pagina e impostare un nuovo URL pubblico.</span><span class="sxs-lookup"><span data-stu-id="63095-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="63095-128">Id applicazione di Microsoft e la Password archiviati</span><span class="sxs-lookup"><span data-stu-id="63095-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="63095-129">Nota il `Application Id` visualizzata sul **registrazione** pagina.</span><span class="sxs-lookup"><span data-stu-id="63095-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="63095-130">Toccare **generare una nuova Password** nel **applicazione segreti** sezione.</span><span class="sxs-lookup"><span data-stu-id="63095-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="63095-131">Verrà visualizzata una casella in cui è possibile copiare la password dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="63095-131">This displays a box where you can copy the application password:</span></span>

![Finestra di dialogo nuova password generata](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="63095-133">Collegare le impostazioni sensibili come Microsoft `Application ID` e `Password` all'applicazione mediante configurazione di [Manager segreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="63095-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="63095-134">Ai fini di questa esercitazione, denominare il token `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="63095-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="63095-135">Configurare l'autenticazione di Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="63095-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="63095-136">Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pacchetto è già installato.</span><span class="sxs-lookup"><span data-stu-id="63095-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="63095-137">Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="63095-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="63095-138">Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="63095-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63095-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63095-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="63095-140">Aggiungere il servizio Account Microsoft nel `ConfigureServices` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="63095-140">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63095-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63095-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="63095-142">Aggiungere il middleware di Account Microsoft nel `Configure` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="63095-142">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="63095-143">Sebbene la terminologia utilizzata nel portale per sviluppatori Microsoft nomi questi token `ApplicationId` e `Password`, è esposto come `ClientId` e `ClientSecret` nell'API di configurazione.</span><span class="sxs-lookup"><span data-stu-id="63095-143">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="63095-144">Vedere il [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63095-144">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="63095-145">Questo può essere usato per richiedere informazioni diverse relative all'utente.</span><span class="sxs-lookup"><span data-stu-id="63095-145">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="63095-146">Accedi con Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="63095-146">Sign in with Microsoft Account</span></span>

<span data-ttu-id="63095-147">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="63095-147">Run your application and click **Log in**.</span></span> <span data-ttu-id="63095-148">Viene visualizzata un'opzione per eseguire l'accesso con Microsoft:</span><span class="sxs-lookup"><span data-stu-id="63095-148">An option to sign in with Microsoft appears:</span></span>

![Log di applicazione nella pagina Web: utente non autenticato](index/_static/DoneMicrosoft.png)

<span data-ttu-id="63095-150">Quando si fa clic su Microsoft, si viene reindirizzati a Microsoft per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="63095-150">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="63095-151">Dopo l'accesso con l'Account Microsoft (se non è già stato effettuato l'accesso) verrà richiesto per consentire all'app di accedere alle tue info:</span><span class="sxs-lookup"><span data-stu-id="63095-151">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Finestra di dialogo autenticazione Microsoft](index/_static/MicrosoftLogin.png)

<span data-ttu-id="63095-153">Toccare **Sì** e si verrà reindirizzati al sito web in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="63095-153">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="63095-154">A questo punto si è connessi utilizzando le credenziali di Microsoft:</span><span class="sxs-lookup"><span data-stu-id="63095-154">You are now logged in using your Microsoft credentials:</span></span>

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="63095-156">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="63095-156">Troubleshooting</span></span>

* <span data-ttu-id="63095-157">Se il provider dell'Account di Microsoft si viene reindirizzati a una pagina di errore di accesso, tenere presente l'errore titolo e descrizione parametri stringa di query direttamente dopo il `#` (hashtag) nell'Uri.</span><span class="sxs-lookup"><span data-stu-id="63095-157">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="63095-158">Anche se sembra che il messaggio di errore indicano un problema con l'autenticazione di Microsoft, la causa più comune è l'Uri non corrisponde a nessuna delle applicazione il **Redirect URIs** specificato per il **Web** piattaforma .</span><span class="sxs-lookup"><span data-stu-id="63095-158">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="63095-159">**ASP.NET Core 2.x solo:** identità se non è configurata tramite la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="63095-159">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="63095-160">Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="63095-160">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="63095-161">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterranno *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="63095-161">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="63095-162">Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.</span><span class="sxs-lookup"><span data-stu-id="63095-162">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63095-163">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="63095-163">Next steps</span></span>

* <span data-ttu-id="63095-164">In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63095-164">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="63095-165">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="63095-165">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="63095-166">Quando si pubblica il sito web all'app web di Azure, è necessario creare un nuovo `Password` nel portale per sviluppatori di Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63095-166">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="63095-167">Impostare il `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="63095-167">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="63095-168">Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="63095-168">The configuration system is set up to read keys from environment variables.</span></span>
