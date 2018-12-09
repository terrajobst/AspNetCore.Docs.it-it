---
title: Configurazione dell'accesso esterno Google in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione di autenticazione dell'utente account Google in un'app ASP.NET Core esistente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: e5deda5d521643e3155be00f4630a86c6a82575c
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121531"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="aa463-103">Configurazione dell'accesso esterno Google in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa463-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="aa463-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aa463-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aa463-105">Questa esercitazione illustra come consentire agli utenti di accedere con il proprio account Google + tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="aa463-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="aa463-106">Iniziare seguendo la [passaggi ufficiali](https://developers.google.com/identity/sign-in/web/devconsole-project) per creare una nuova app nella Console API di Google.</span><span class="sxs-lookup"><span data-stu-id="aa463-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="aa463-107">Creare l'app nella Console API Google</span><span class="sxs-lookup"><span data-stu-id="aa463-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="aa463-108">Passare a [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="aa463-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="aa463-109">Se non si ha già un account Google, usare **altre opzioni** > **[creare account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  collegamento per crearne uno:</span><span class="sxs-lookup"><span data-stu-id="aa463-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Console di Google API](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="aa463-111">Si verrà reindirizzati alla **libreria di gestione API** pagina:</span><span class="sxs-lookup"><span data-stu-id="aa463-111">You are redirected to **API Manager Library** page:</span></span>

![Nella pagina libreria di gestione API di destinazione](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="aa463-113">Toccare **Create** e immettere il **nome del progetto**:</span><span class="sxs-lookup"><span data-stu-id="aa463-113">Tap **Create** and enter your **Project name**:</span></span>

![Finestra di dialogo Nuovo progetto](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="aa463-115">Dopo aver accettato la finestra di dialogo, si verrà reindirizzati alla pagina di libreria che consente di scegliere le funzionalità per la nuova app.</span><span class="sxs-lookup"><span data-stu-id="aa463-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="aa463-116">Trovare **Google + API** nell'elenco e fare clic sul relativo collegamento per aggiungere la funzionalità API:</span><span class="sxs-lookup"><span data-stu-id="aa463-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Cercare "Google + API" nella pagina della libreria di gestione API](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="aa463-118">Verrà visualizzata la pagina relativa all'API appena aggiunto.</span><span class="sxs-lookup"><span data-stu-id="aa463-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="aa463-119">Toccare **abilitare** aggiungere Google + segno nella funzionalità all'App:</span><span class="sxs-lookup"><span data-stu-id="aa463-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Nella pagina Google + API di gestione API di destinazione](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="aa463-121">Dopo l'abilitazione dell'API, toccare **creare le credenziali** per configurare i segreti:</span><span class="sxs-lookup"><span data-stu-id="aa463-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Creare credenziali pulsante nella pagina di gestione API di Google + API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="aa463-123">Scegliere:</span><span class="sxs-lookup"><span data-stu-id="aa463-123">Choose:</span></span>
  * <span data-ttu-id="aa463-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="aa463-124">**Google+ API**</span></span>
  * <span data-ttu-id="aa463-125">**Server Web (ad esempio Node. js, Tomcat)**, e</span><span class="sxs-lookup"><span data-stu-id="aa463-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
  * <span data-ttu-id="aa463-126">**I dati utente**:</span><span class="sxs-lookup"><span data-stu-id="aa463-126">**User data**:</span></span>

![Pagina delle credenziali di gestione API: informazioni su quali tipi di credenziali è necessario pannello](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="aa463-128">Toccare **quali credenziali è necessario?** che consente di visualizzare il secondo passaggio della configurazione dell'app, **creare un ID client OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="aa463-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Pagina delle credenziali di gestione API: creazione di un ID client OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="aa463-130">Perché si sta creando un progetto Google + con una sola funzionalità (Accedi), è possibile immettere lo stesso **nome** per l'ID client OAuth 2.0 come quella utilizzate per il progetto.</span><span class="sxs-lookup"><span data-stu-id="aa463-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="aa463-131">Immettere l'URI di sviluppo con `/signin-google` aggiunto nel **URI di reindirizzamento autorizzati** campo (ad esempio: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="aa463-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="aa463-132">L'autenticazione di Google configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste a `/signin-google` route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="aa463-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="aa463-133">Il segmento URI `/signin-google` viene impostato come il callback predefinito del provider di autenticazione Google.</span><span class="sxs-lookup"><span data-stu-id="aa463-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="aa463-134">È possibile modificare l'URI di callback predefinito durante la configurazione del middleware di autenticazione Google tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà delle [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="aa463-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="aa463-135">Premere TAB per aggiungere il **URI di reindirizzamento autorizzati** voce.</span><span class="sxs-lookup"><span data-stu-id="aa463-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="aa463-136">Toccare **creare ID client**, che consente di visualizzare il terzo passaggio **configurare la schermata di consenso di OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="aa463-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Pagina delle credenziali di gestione API: configurare la schermata di consenso di OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="aa463-138">Immettere il pubblico **indirizzo di posta elettronica** e il **il nome del prodotto** visualizzato per l'app quando Google + richiede all'utente di accedere.</span><span class="sxs-lookup"><span data-stu-id="aa463-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="aa463-139">Sono disponibili con opzioni aggiuntive **altre opzioni di personalizzazione**.</span><span class="sxs-lookup"><span data-stu-id="aa463-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="aa463-140">Toccare **continuazione** per procedere all'ultimo passaggio **scaricare le credenziali**:</span><span class="sxs-lookup"><span data-stu-id="aa463-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Pagina delle credenziali di gestione API: scaricare le credenziali](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="aa463-142">Toccare **scaricare** per salvare un file JSON con i segreti dell'applicazione, e **eseguita** per completare la creazione della nuova app.</span><span class="sxs-lookup"><span data-stu-id="aa463-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="aa463-143">Quando si distribuisce il sito è necessario rivedere le **Console di Google** e registrare un nuovo url pubblico.</span><span class="sxs-lookup"><span data-stu-id="aa463-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="aa463-144">Store Google ClientID e ClientSecret</span><span class="sxs-lookup"><span data-stu-id="aa463-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="aa463-145">Collegare le impostazioni sensibili, ad esempio Google `Client ID` e `Client Secret` alla configurazione dell'applicazione usando la [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="aa463-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="aa463-146">Ai fini di questa esercitazione, denominare il token `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="aa463-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="aa463-147">I valori per questi token sono reperibili nel file JSON scaricato nel passaggio precedente sotto `web.client_id` e `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="aa463-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="aa463-148">Configurare l'autenticazione di Google</span><span class="sxs-lookup"><span data-stu-id="aa463-148">Configure Google Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa463-149">Aggiungere il servizio Google nel `ConfigureServices` nel metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="aa463-149">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="aa463-150">Il modello di progetto usato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) installazione del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="aa463-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="aa463-151">Per installare questo pacchetto con Visual Studio 2017, pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="aa463-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="aa463-152">Per installare con CLI di .NET Core, eseguire il codice seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="aa463-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="aa463-153">Aggiungere il middleware di Google nel `Configure` nel metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="aa463-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

<span data-ttu-id="aa463-154">Vedere le [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Google.</span><span class="sxs-lookup"><span data-stu-id="aa463-154">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="aa463-155">Ciò può essere usato per richiedere informazioni diverse sull'utente.</span><span class="sxs-lookup"><span data-stu-id="aa463-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="aa463-156">Accedi con Google</span><span class="sxs-lookup"><span data-stu-id="aa463-156">Sign in with Google</span></span>

<span data-ttu-id="aa463-157">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="aa463-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="aa463-158">Viene visualizzata un'opzione per accedere con Google:</span><span class="sxs-lookup"><span data-stu-id="aa463-158">An option to sign in with Google appears:</span></span>

![Applicazione Web in esecuzione in Microsoft Edge: utente non autenticato](index/_static/DoneGoogle.png)

<span data-ttu-id="aa463-160">Quando fa clic su Google, si verrà reindirizzati a Google per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="aa463-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Finestra di dialogo autenticazione Google](index/_static/GoogleLogin.png)

<span data-ttu-id="aa463-162">Dopo aver immesso le credenziali di Google, quindi si verrà reindirizzati al sito web in cui è possibile impostare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="aa463-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="aa463-163">A questo punto si è connessi con le credenziali di Google:</span><span class="sxs-lookup"><span data-stu-id="aa463-163">You are now logged in using your Google credentials:</span></span>

![Applicazione Web in esecuzione in Microsoft Edge: utente autenticato](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="aa463-165">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="aa463-165">Troubleshooting</span></span>

* <span data-ttu-id="aa463-166">Se si riceve un `403 (Forbidden)` pagina di errore dalla propria app quando è in esecuzione in modalità di sviluppo o inserire un'interruzione nel debugger con lo stesso errore, verificare che **Google + API** è stata abilitata nel **libreriadigestioneAPI** seguendo i passaggi elencati [precedenti in questa pagina](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="aa463-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="aa463-167">Se non sono presenti eventuali errori di accesso non funziona, passare alla modalità di sviluppo per rendere più semplice eseguire il debug del problema.</span><span class="sxs-lookup"><span data-stu-id="aa463-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="aa463-168">**ASP.NET Core 2.x solo:** Identity se non è configurato tramite la chiamata `services.AddIdentity` nel `ConfigureServices`, tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="aa463-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="aa463-169">Il modello di progetto usato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="aa463-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="aa463-170">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterrà *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="aa463-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="aa463-171">Toccare **applicare le migrazioni** per creare il database e di aggiornamento per continuare oltre l'errore.</span><span class="sxs-lookup"><span data-stu-id="aa463-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa463-172">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="aa463-172">Next steps</span></span>

* <span data-ttu-id="aa463-173">Questo articolo ha illustrato come è possibile eseguire l'autenticazione con Google.</span><span class="sxs-lookup"><span data-stu-id="aa463-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="aa463-174">È possibile seguire un approccio simile per l'autenticazione con altri provider di servizi elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="aa463-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="aa463-175">Dopo aver pubblicato il sito web all'app web di Azure, è consigliabile reimpostare il `ClientSecret` nella Console API di Google.</span><span class="sxs-lookup"><span data-stu-id="aa463-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="aa463-176">Impostare il `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="aa463-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="aa463-177">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="aa463-177">The configuration system is set up to read keys from environment variables.</span></span>
