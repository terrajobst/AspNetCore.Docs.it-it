---
title: Abilitazione dell'autenticazione con Facebook, Google e altri provider esterni
author: rick-anderson
description: Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x tramite OAuth 2.0 con provider di autenticazione esterni.
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/social/index
ms.openlocfilehash: 475656b03b0d1a3e79e78e6cf3816f091ccd640b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="6b58c-103">Abilitazione dell'autenticazione con Facebook, Google e altri provider esterni</span><span class="sxs-lookup"><span data-stu-id="6b58c-103">Enabling authentication using Facebook, Google, and other external providers</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="6b58c-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6b58c-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6b58c-105">Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x che consente agli utenti di accedere tramite OAuth 2.0 con le credenziali dai provider di autenticazione esterni.</span><span class="sxs-lookup"><span data-stu-id="6b58c-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="6b58c-106">I provider di [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), e [Microsoft](microsoft-logins.md) vengono trattati nelle sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="6b58c-106">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="6b58c-107">Altri provider sono disponibili nei pacchetti di terze parti, ad esempio [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="6b58c-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Icone di social media per Facebook, Twitter, Google + e Windows](index/_static/social.png)

<span data-ttu-id="6b58c-109">Consentire agli utenti di accedere con le credenziali esistenti è utile per gli utenti e sposta molte delle complessità di gestione del processo di accesso in una terza parte.</span><span class="sxs-lookup"><span data-stu-id="6b58c-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="6b58c-110">Per degli esempi di come gli account di accesso ai social possano risultare utili per la conversione del traffico e del cliente, vedere dei case study da [Facebook](https://www.facebook.com/unsupportedbrowser) e [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="6b58c-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="6b58c-111">Nota: i pacchetti qui presentati riassumono una notevole dose di complessità del flusso di autenticazione OAuth, ma comprendere i dettagli potrebbe essere necessario per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="6b58c-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="6b58c-112">Sono disponibili molte risorse; ad esempio, vedere [Introduzione a OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) o [Comprensione di OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="6b58c-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="6b58c-113">Alcuni problemi possono essere risolti esaminando il [Codice sorgente di ASP.NET Core per i pacchetti di provider](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="6b58c-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="6b58c-114">Creare un nuovo progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b58c-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="6b58c-115">In Visual Studio 2017 creare un nuovo progetto dalla Pagina iniziale o tramite **File > Nuovo > Progetto**.</span><span class="sxs-lookup"><span data-stu-id="6b58c-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="6b58c-116">Selezionare il modello **Applicazione Web di ASP.NET Core** disponibile nella categoria **Visual C# > .NET Core**:</span><span class="sxs-lookup"><span data-stu-id="6b58c-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Finestra di dialogo Nuovo progetto](index/_static/new-project.png)

* <span data-ttu-id="6b58c-118">Toccare **Applicazione Web** e verificare che l'**Autenticazione** sia impostata su **Account utente individuali**:</span><span class="sxs-lookup"><span data-stu-id="6b58c-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Finestra di dialogo Nuova Applicazione Web](index/_static/select-project.png)

<span data-ttu-id="6b58c-120">Nota: questa esercitazione si applica alla versione di ASP.NET Core 2.0 SDK che può essere selezionata nella parte superiore della procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="6b58c-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="6b58c-121">Applicare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="6b58c-121">Apply migrations</span></span>

* <span data-ttu-id="6b58c-122">Eseguire l'app e selezionare il collegamento **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="6b58c-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="6b58c-123">Selezionare il collegamento **Esegui registrazione come nuovo utente**.</span><span class="sxs-lookup"><span data-stu-id="6b58c-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="6b58c-124">Immettere l'indirizzo di posta elettronica e la password per il nuovo account, quindi selezionare **Registra**.</span><span class="sxs-lookup"><span data-stu-id="6b58c-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="6b58c-125">Seguire le istruzioni per applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="6b58c-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="6b58c-126">Richiedere SSL</span><span class="sxs-lookup"><span data-stu-id="6b58c-126">Require SSL</span></span>

<span data-ttu-id="6b58c-127">OAuth 2.0 richiede l'uso di SSL per l'autenticazione tramite il protocollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6b58c-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="6b58c-128">Nota: i progetti creati tramite i modelli di progetto **Applicazione Web** o **API Web** per ASP.NET Core 2.x vengono configurati automaticamente per attivare SSL e avviarlo con l'URL https se l'opzione **Account utente singoli** è stata selezionata l'opzione su **Modifica finestra di dialogo di autenticazione** nella procedura guidata del progetto, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="6b58c-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="6b58c-129">Richiedere SSL sul sito seguendo la procedura descritta nell'argomento [Applicazione SSL in un'app ASP.NET Core](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="6b58c-129">Require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="6b58c-130">Usare SecretManager per archiviare i token assegnati dai provider di accesso</span><span class="sxs-lookup"><span data-stu-id="6b58c-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="6b58c-131">I provider di accesso ai social assegnano i token **ID applicazione** e **Segreto dell'applicazione** durante il processo di registrazione (la denominazione esatta varia in base al provider).</span><span class="sxs-lookup"><span data-stu-id="6b58c-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="6b58c-132">Questi valori sono di fatto il *nome utente* e la *password* che l'applicazione usa per accedere alle API e costituiscono i "segreti" che possono essere collegati a una configurazione dell'applicazione con l'aiuto di **Manager segreto** invece di archiviarli direttamente nei file di configurazione o di impostarli come hard-coded.</span><span class="sxs-lookup"><span data-stu-id="6b58c-132">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="6b58c-133">Seguire la procedura nell'argomento [Archiviazione sicura di segreti dell'app durante lo sviluppo di ASP.NET Core](xref:security/app-secrets) in modo da poter archiviare i token assegnati da ciascun provider di accesso indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="6b58c-133">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="6b58c-134">Impostare i provider di accesso necessari per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="6b58c-134">Setup login providers required by your application</span></span>

<span data-ttu-id="6b58c-135">Usare gli argomenti seguenti per configurare l'applicazione per usare i rispettivi provider:</span><span class="sxs-lookup"><span data-stu-id="6b58c-135">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="6b58c-136">Istruzioni di [Facebook](facebook-logins.md)</span><span class="sxs-lookup"><span data-stu-id="6b58c-136">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="6b58c-137">Istruzioni di [Twitter](twitter-logins.md)</span><span class="sxs-lookup"><span data-stu-id="6b58c-137">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="6b58c-138">Istruzioni di [Google](google-logins.md)</span><span class="sxs-lookup"><span data-stu-id="6b58c-138">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="6b58c-139">Istruzioni di [Microsoft](microsoft-logins.md)</span><span class="sxs-lookup"><span data-stu-id="6b58c-139">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="6b58c-140">Istruzioni di [altri provider](other-logins.md)</span><span class="sxs-lookup"><span data-stu-id="6b58c-140">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="6b58c-141">Impostare facoltativamente la password</span><span class="sxs-lookup"><span data-stu-id="6b58c-141">Optionally set password</span></span>

<span data-ttu-id="6b58c-142">Quando ci si registra con un provider di accesso esterno, non si dispone di una password registrata con l'app.</span><span class="sxs-lookup"><span data-stu-id="6b58c-142">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="6b58c-143">Questo evita la creazione e la memorizzazione di una password per il sito, ma crea anche una dipendenza dal provider di accesso esterno.</span><span class="sxs-lookup"><span data-stu-id="6b58c-143">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="6b58c-144">Se il provider di accesso esterno non è disponibile, non si potrà accedere al sito web.</span><span class="sxs-lookup"><span data-stu-id="6b58c-144">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="6b58c-145">Per creare una password e accedere usando la posta elettronica impostata durante il processo di accesso con provider esterni:</span><span class="sxs-lookup"><span data-stu-id="6b58c-145">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="6b58c-146">Toccare il collegamento **Hello <email alias>** nell'angolo superiore destro per passare alla vista **Gestione**.</span><span class="sxs-lookup"><span data-stu-id="6b58c-146">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Vista Gestione dell'applicazione Web](index/_static/pass1a.png)

* <span data-ttu-id="6b58c-148">Toccare **Crea**</span><span class="sxs-lookup"><span data-stu-id="6b58c-148">Tap **Create**</span></span>

![Impostare la pagina della password](index/_static/pass2a.png)

* <span data-ttu-id="6b58c-150">Impostare una password valida in modo da usarla per accedere con la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="6b58c-150">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b58c-151">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6b58c-151">Next steps</span></span>

* <span data-ttu-id="6b58c-152">Questo articolo ha introdotto l'autenticazione esterna e ha illustrato i prerequisiti necessari per aggiungere gli account di accesso esterni all'app di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6b58c-152">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="6b58c-153">Pagine di riferimento specifico del provider per configurare gli account di accesso per i provider richiesti dall'app.</span><span class="sxs-lookup"><span data-stu-id="6b58c-153">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
