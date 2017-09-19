---
title: Programma di installazione di Google account di accesso esterno in ASP.NET Core
author: rick-anderson
description: Programma di installazione di Google account di accesso esterno in ASP.NET Core
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/google-logins
ms.openlocfilehash: 7e37a8af4ae5a957483fa5f4a89ea4e8999a3d1d
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="configuring-google-authentication-in-aspnet-core"></a><span data-ttu-id="7e79e-104">Configurazione dell'autenticazione di Google in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e79e-104">Configuring Google authentication in ASP.NET Core</span></span>

<a name=security-authentication-google-logins></a>

<span data-ttu-id="7e79e-105">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7e79e-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7e79e-106">In questa esercitazione viene illustrato come consentire agli utenti di accedere con l'account di Google + tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](index.md).</span><span class="sxs-lookup"><span data-stu-id="7e79e-106">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="7e79e-107">Iniziamo seguendo il [passaggi ufficiali](https://developers.google.com/identity/sign-in/web/devconsole-project) per creare una nuova app nella Console API Google.</span><span class="sxs-lookup"><span data-stu-id="7e79e-107">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="7e79e-108">Creare l'app nella Console API di Google</span><span class="sxs-lookup"><span data-stu-id="7e79e-108">Create the app in Google API Console</span></span>

* <span data-ttu-id="7e79e-109">Passare a [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="7e79e-109">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="7e79e-110">Se si dispone già di un account Google, usare **più opzioni** > **[creare account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api) ** collegamento per creare uno:</span><span class="sxs-lookup"><span data-stu-id="7e79e-110">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Console di API di Google](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="7e79e-112">Si verrà reindirizzati a **libreria API Manager** pagina:</span><span class="sxs-lookup"><span data-stu-id="7e79e-112">You are redirected to **API Manager Library** page:</span></span>

![Pagina della libreria di gestione API](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="7e79e-114">Toccare **crea** e immettere il **nome progetto**:</span><span class="sxs-lookup"><span data-stu-id="7e79e-114">Tap **Create** and enter your **Project name**:</span></span>

![Finestra di dialogo Nuovo progetto](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="7e79e-116">Dopo avere accettato la finestra di dialogo, si viene reindirizzati alla pagina libreria che consente di scegliere le funzionalità per la nuova app.</span><span class="sxs-lookup"><span data-stu-id="7e79e-116">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="7e79e-117">Trovare **Google + API** nell'elenco e fare clic sul relativo collegamento per aggiungere la funzionalità API:</span><span class="sxs-lookup"><span data-stu-id="7e79e-117">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Pagina della libreria di gestione API](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="7e79e-119">Verrà visualizzata la pagina per l'API appena aggiunto.</span><span class="sxs-lookup"><span data-stu-id="7e79e-119">The page for the newly added API is displayed.</span></span> <span data-ttu-id="7e79e-120">Toccare **abilitare** aggiungere Google + segno nella funzionalità all'App:</span><span class="sxs-lookup"><span data-stu-id="7e79e-120">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Pagina di Google + API di gestione API](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="7e79e-122">Dopo aver abilitato l'API, toccare **creare credenziali** per configurare i segreti:</span><span class="sxs-lookup"><span data-stu-id="7e79e-122">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Pagina di Google + API di gestione API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="7e79e-124">Scegliere:</span><span class="sxs-lookup"><span data-stu-id="7e79e-124">Choose:</span></span>
   * <span data-ttu-id="7e79e-125">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="7e79e-125">**Google+ API**</span></span>
   * <span data-ttu-id="7e79e-126">**Server Web (ad esempio, node.js, Tomcat)**, e</span><span class="sxs-lookup"><span data-stu-id="7e79e-126">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="7e79e-127">**Dati utente**:</span><span class="sxs-lookup"><span data-stu-id="7e79e-127">**User data**:</span></span>

![Pagina delle credenziali di gestione API: scoprire quali tipo di credenziali necessario pannello](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="7e79e-129">Toccare **le credenziali sono necessarie?** che permette di visualizzare il secondo passaggio della configurazione di app, **creare un ID client OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="7e79e-129">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Pagina delle credenziali di gestione API: creazione di un ID client OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="7e79e-131">Poiché viene creato un progetto di Google + con una sola funzionalità (accesso), è possibile immettere lo stesso **nome** per l'ID client OAuth 2.0 è utilizzato per il progetto.</span><span class="sxs-lookup"><span data-stu-id="7e79e-131">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="7e79e-132">Immettere l'URI di sviluppo con */signin-google* aggiunti al **autorizzato URI di reindirizzamento** campo (ad esempio: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="7e79e-132">Enter your development URI with */signin-google* appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="7e79e-133">L'autenticazione di Google configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-google* route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="7e79e-133">The Google authentication configured later in this tutorial will automatically handle requests at */signin-google* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="7e79e-134">Premere TAB per aggiungere il **autorizzato URI di reindirizzamento** voce.</span><span class="sxs-lookup"><span data-stu-id="7e79e-134">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="7e79e-135">Toccare **Create client ID**, che permette di visualizzare il terzo passaggio **impostare la schermata di consenso di OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="7e79e-135">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Pagina delle credenziali di gestione API: impostare la schermata di consenso di OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="7e79e-137">Immettere il pubblico **indirizzo di posta elettronica** e **nome prodotto** visualizzato per l'app quando Google + richiesto all'utente di accedere.</span><span class="sxs-lookup"><span data-stu-id="7e79e-137">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="7e79e-138">Opzioni aggiuntive sono disponibili in **ulteriori opzioni di personalizzazione**.</span><span class="sxs-lookup"><span data-stu-id="7e79e-138">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="7e79e-139">Toccare **continua** per eseguire l'ultimo passaggio, **scaricare le credenziali**:</span><span class="sxs-lookup"><span data-stu-id="7e79e-139">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Pagina delle credenziali di gestione API: scaricare le credenziali](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="7e79e-141">Toccare **scaricare** per salvare un file JSON con segreti dell'applicazione, e **eseguita** per completare la creazione della nuova app.</span><span class="sxs-lookup"><span data-stu-id="7e79e-141">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="7e79e-142">Quando si distribuisce il sito è necessario rivedere il **Google Console** e registrare un nuovo url pubblico.</span><span class="sxs-lookup"><span data-stu-id="7e79e-142">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="7e79e-143">Archivio Google ClientID e ClientSecret</span><span class="sxs-lookup"><span data-stu-id="7e79e-143">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="7e79e-144">Collegare le impostazioni sensibili, ad esempio Google `Client ID` e `Client Secret` all'applicazione mediante configurazione di [Manager segreto](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="7e79e-144">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="7e79e-145">Ai fini di questa esercitazione, denominare il token `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="7e79e-145">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="7e79e-146">I valori per questi token, vedere il file JSON scaricato nel passaggio precedente in `web.client_id` e `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="7e79e-146">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="7e79e-147">Configurare l'autenticazione di Google</span><span class="sxs-lookup"><span data-stu-id="7e79e-147">Configure Google Authentication</span></span>

<span data-ttu-id="7e79e-148">Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) viene installato.</span><span class="sxs-lookup"><span data-stu-id="7e79e-148">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

 * <span data-ttu-id="7e79e-149">Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7e79e-149">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
 * <span data-ttu-id="7e79e-150">Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="7e79e-150">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e79e-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7e79e-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7e79e-152">Aggiungere il servizio Google il `ConfigureServices` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="7e79e-152">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

<span data-ttu-id="7e79e-153">Il `AddAuthentication` metodo deve essere chiamato solo una volta quando si aggiunge più provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="7e79e-153">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="7e79e-154">Le chiamate successive a esso hanno la possibilità di cui si esegue l'override di qualsiasi configurati in precedenza [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) proprietà.</span><span class="sxs-lookup"><span data-stu-id="7e79e-154">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e79e-155">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7e79e-155">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7e79e-156">Aggiungere il middleware di Google nel `Configure` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="7e79e-156">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="7e79e-157">Vedere il [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione Google.</span><span class="sxs-lookup"><span data-stu-id="7e79e-157">See the [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="7e79e-158">Questo può essere usato per richiedere informazioni diverse relative all'utente.</span><span class="sxs-lookup"><span data-stu-id="7e79e-158">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="7e79e-159">Accedere con Google</span><span class="sxs-lookup"><span data-stu-id="7e79e-159">Sign in with Google</span></span>

<span data-ttu-id="7e79e-160">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="7e79e-160">Run your application and click **Log in**.</span></span> <span data-ttu-id="7e79e-161">Viene visualizzata un'opzione per accedere con Google:</span><span class="sxs-lookup"><span data-stu-id="7e79e-161">An option to sign in with Google appears:</span></span>

![Applicazione Web in esecuzione in Microsoft Edge: utente non autenticato](index/_static/DoneGoogle.png)

<span data-ttu-id="7e79e-163">Quando si fa clic su Google, si verrà reindirizzati a Google per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="7e79e-163">When you click on Google, you are redirected to Google for authentication:</span></span>

![Finestra di dialogo autenticazione Google](index/_static/GoogleLogin.png)

<span data-ttu-id="7e79e-165">Dopo aver immesso le credenziali di Google, quindi si verrà reindirizzati al sito web in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="7e79e-165">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="7e79e-166">A questo punto si è connessi utilizzando le credenziali di Google:</span><span class="sxs-lookup"><span data-stu-id="7e79e-166">You are now logged in using your Google credentials:</span></span>

![Applicazione Web in esecuzione in Microsoft Edge: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="7e79e-168">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="7e79e-168">Troubleshooting</span></span>

* <span data-ttu-id="7e79e-169">Se si riceve un `403 (Forbidden)` pagina di errore dall'applicazione durante l'esecuzione in modalità di sviluppo o interruzione nel debugger con lo stesso errore, assicurarsi che **Google + API** è stata abilitata nel **libreria gestione API** seguendo i passaggi elencati [precedenti in questa pagina](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="7e79e-169">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="7e79e-170">Se l'accesso non funziona e non vengono visualizzate eventuali errori, passare alla modalità di sviluppo per rendere più semplice eseguire il debug del problema.</span><span class="sxs-lookup"><span data-stu-id="7e79e-170">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="7e79e-171">**ASP.NET Core solo 2. x:** identità se non è configurato per la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="7e79e-171">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="7e79e-172">Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="7e79e-172">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="7e79e-173">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterranno *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="7e79e-173">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="7e79e-174">Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.</span><span class="sxs-lookup"><span data-stu-id="7e79e-174">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e79e-175">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="7e79e-175">Next steps</span></span>

* <span data-ttu-id="7e79e-176">In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Google.</span><span class="sxs-lookup"><span data-stu-id="7e79e-176">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="7e79e-177">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](index.md).</span><span class="sxs-lookup"><span data-stu-id="7e79e-177">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="7e79e-178">Quando si pubblica il sito web all'app web di Azure, è consigliabile reimpostare il `ClientSecret` nella Console di API di Google.</span><span class="sxs-lookup"><span data-stu-id="7e79e-178">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="7e79e-179">Impostare il `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="7e79e-179">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="7e79e-180">Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="7e79e-180">The configuration system is set up to read keys from environment variables.</span></span>
