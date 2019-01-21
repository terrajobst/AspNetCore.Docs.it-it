---
title: Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x tramite OAuth 2.0 con provider di autenticazione esterni.
ms.author: riande
ms.custom: mvc
ms.date: 1/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: 48dd8b772234ff18158423a36ed1716102bc2f31
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396142"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="4f46e-103">Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f46e-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="4f46e-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4f46e-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4f46e-105">Questa esercitazione illustra come compilare un'app ASP.NET Core 2.2 che consente agli utenti di accedere tramite OAuth 2.0 con credenziali di provider di autenticazione esterni.</span><span class="sxs-lookup"><span data-stu-id="4f46e-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="4f46e-106">I provider di [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), e [Microsoft](xref:security/authentication/microsoft-logins) vengono trattati nelle sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="4f46e-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="4f46e-107">Altri provider sono disponibili nei pacchetti di terze parti, ad esempio [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="4f46e-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Icone di social media per Facebook, Twitter, Google + e Windows](index/_static/social.png)

<span data-ttu-id="4f46e-109">Consentire agli utenti di accedere con le credenziali esistenti è utile per gli utenti e sposta molte delle complessità di gestione del processo di accesso in una terza parte.</span><span class="sxs-lookup"><span data-stu-id="4f46e-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="4f46e-110">Per degli esempi di come gli account di accesso ai social possano risultare utili per la conversione del traffico e del cliente, vedere dei case study da [Facebook](https://www.facebook.com/unsupportedbrowser) e [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="4f46e-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="4f46e-111">Creare un nuovo progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f46e-111">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="4f46e-112">In Visual Studio 2017 creare un nuovo progetto dalla pagina iniziale o scegliendo **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="4f46e-112">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="4f46e-113">Selezionare il modello **Applicazione Web ASP.NET Core** disponibile nella categoria **Visual C#** > **.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="4f46e-113">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>
* <span data-ttu-id="4f46e-114">Selezionare **Modifica autenticazione** e impostare l'autenticazione su **Account utente individuali**.</span><span class="sxs-lookup"><span data-stu-id="4f46e-114">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="4f46e-115">Applicare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="4f46e-115">Apply migrations</span></span>

* <span data-ttu-id="4f46e-116">Eseguire l'app e selezionare il collegamento **Registra**.</span><span class="sxs-lookup"><span data-stu-id="4f46e-116">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="4f46e-117">Immettere l'indirizzo di posta elettronica e la password per il nuovo account, quindi selezionare **Registra**.</span><span class="sxs-lookup"><span data-stu-id="4f46e-117">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="4f46e-118">Seguire le istruzioni per applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="4f46e-118">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="4f46e-119">Usare SecretManager per archiviare i token assegnati dai provider di accesso</span><span class="sxs-lookup"><span data-stu-id="4f46e-119">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="4f46e-120">I provider di accesso ai social assegnano i token **ID applicazione** e **Segreto dell'applicazione** durante il processo di registrazione.</span><span class="sxs-lookup"><span data-stu-id="4f46e-120">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="4f46e-121">La denominazione esatta dei token varia a seconda del provider.</span><span class="sxs-lookup"><span data-stu-id="4f46e-121">The exact token names vary by provider.</span></span> <span data-ttu-id="4f46e-122">Questi token rappresentano le credenziali usate dall'app per accedere alle API.</span><span class="sxs-lookup"><span data-stu-id="4f46e-122">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="4f46e-123">I token costituiscono i "segreti" che è possibile collegare alla configurazione dell'app usando [Secret Manager](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="4f46e-123">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="4f46e-124">Secret Manager è un'alternativa che offre maggior sicurezza rispetto alla memorizzazione dei token in un file di configurazione, ad esempio *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4f46e-124">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4f46e-125">Secret Manager è progettato esclusivamente per lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4f46e-125">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="4f46e-126">È possibile memorizzare e proteggere i segreti relativi al test e alla produzione di Azure usando il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="4f46e-126">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="4f46e-127">Seguire la procedura descritta nell'argomento [Archiviazione sicura di segreti dell'app durante lo sviluppo di ASP.NET Core](xref:security/app-secrets) per archiviare i token assegnati dai singoli provider di accesso elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="4f46e-127">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="4f46e-128">Impostare i provider di accesso necessari per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="4f46e-128">Setup login providers required by your application</span></span>

<span data-ttu-id="4f46e-129">Usare gli argomenti seguenti per configurare l'applicazione per usare i rispettivi provider:</span><span class="sxs-lookup"><span data-stu-id="4f46e-129">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="4f46e-130">Istruzioni di [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="4f46e-130">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="4f46e-131">Istruzioni di [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="4f46e-131">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="4f46e-132">Istruzioni di [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="4f46e-132">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="4f46e-133">Istruzioni di [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="4f46e-133">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="4f46e-134">Istruzioni di [altri provider](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="4f46e-134">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="4f46e-135">Impostare facoltativamente la password</span><span class="sxs-lookup"><span data-stu-id="4f46e-135">Optionally set password</span></span>

<span data-ttu-id="4f46e-136">Quando ci si registra con un provider di accesso esterno, non si dispone di una password registrata con l'app.</span><span class="sxs-lookup"><span data-stu-id="4f46e-136">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="4f46e-137">Questo evita la creazione e la memorizzazione di una password per il sito, ma crea anche una dipendenza dal provider di accesso esterno.</span><span class="sxs-lookup"><span data-stu-id="4f46e-137">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="4f46e-138">Se il provider di accesso esterno non è disponibile, non si potrà accedere al sito web.</span><span class="sxs-lookup"><span data-stu-id="4f46e-138">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="4f46e-139">Per creare una password e accedere usando la posta elettronica impostata durante il processo di accesso con provider esterni:</span><span class="sxs-lookup"><span data-stu-id="4f46e-139">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="4f46e-140">Selezionare il collegamento **Salve &lt;alias di posta elettronica&gt;** nell'angolo superiore destro per passare alla vista **Gestione**.</span><span class="sxs-lookup"><span data-stu-id="4f46e-140">Select the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Vista Gestione dell'applicazione Web](index/_static/pass1a.png)

* <span data-ttu-id="4f46e-142">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4f46e-142">Select **Create**</span></span>

![Impostare la pagina della password](index/_static/pass2a.png)

* <span data-ttu-id="4f46e-144">Impostare una password valida in modo da usarla per accedere con la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="4f46e-144">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f46e-145">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4f46e-145">Next steps</span></span>

* <span data-ttu-id="4f46e-146">Questo articolo ha introdotto l'autenticazione esterna e ha illustrato i prerequisiti necessari per aggiungere gli account di accesso esterni all'app di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f46e-146">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="4f46e-147">Pagine di riferimento specifico del provider per configurare gli account di accesso per i provider richiesti dall'app.</span><span class="sxs-lookup"><span data-stu-id="4f46e-147">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="4f46e-148">Può essere opportuno salvare in modo permanente dati aggiuntivi sull'utente e il relativo accesso e i token di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="4f46e-148">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="4f46e-149">Per ulteriori informazioni, vedere <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="4f46e-149">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
