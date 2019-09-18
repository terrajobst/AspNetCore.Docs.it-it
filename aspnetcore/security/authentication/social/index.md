---
title: Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x tramite OAuth 2.0 con provider di autenticazione esterni.
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2019
uid: security/authentication/social/index
ms.openlocfilehash: edaf9eeaf02879b2f7816bab0eb373a7de640c05
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082511"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="6a9a5-103">Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a9a5-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="6a9a5-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6a9a5-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6a9a5-105">Questa esercitazione illustra come compilare un'app ASP.NET Core 2.2 che consente agli utenti di eseguire l'accesso tramite OAuth 2.0 con credenziali di provider di autenticazione esterni.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="6a9a5-106">I provider di [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), e [Microsoft](xref:security/authentication/microsoft-logins) vengono trattati nelle sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="6a9a5-107">Altri provider sono disponibili nei pacchetti di terze parti, ad esempio [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="6a9a5-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Icone di social media per Facebook, Twitter, Google + e Windows](index/_static/social.png)

<span data-ttu-id="6a9a5-109">Il fatto di consentire agli utenti l'accesso con le proprie credenziali esistenti:</span><span class="sxs-lookup"><span data-stu-id="6a9a5-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="6a9a5-110">È pratico per gli utenti.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-110">Is convenient for the users.</span></span>
* <span data-ttu-id="6a9a5-111">Trasferisce a terzi molti aspetti complicati del processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="6a9a5-112">Per degli esempi di come gli account di accesso ai social possano risultare utili per la conversione del traffico e del cliente, vedere dei case study da [Facebook](https://www.facebook.com/unsupportedbrowser) e [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="6a9a5-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="6a9a5-113">Creare un nuovo progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a9a5-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a9a5-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a9a5-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a9a5-115">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-115">Create a new project.</span></span>
* <span data-ttu-id="6a9a5-116">Selezionare **Applicazione Web ASP.NET Core** e **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-116">Select **ASP.NET Core Web Application** and **Next**.</span></span>
* <span data-ttu-id="6a9a5-117">Specificare un **Nome progetto** e confermare o modificare il **Percorso**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-117">Provide a **Project name** and confirm or change the **Location**.</span></span> <span data-ttu-id="6a9a5-118">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-118">Select **Create**.</span></span>
* <span data-ttu-id="6a9a5-119">Selezionare **ASP.NET Core 2.2** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-119">Select **ASP.NET Core 2.2** in the drop down.</span></span> <span data-ttu-id="6a9a5-120">Selezionare **Applicazione Web** nell'elenco dei modelli.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-120">Select **Web Application** in the template list.</span></span>
* <span data-ttu-id="6a9a5-121">In **Autenticazione** selezionare **Modifica** e impostare l'autenticazione su **Account utente individuali**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-121">Under **Authentication**, select **Change** and set the authentication to **Individual User Accounts**.</span></span> <span data-ttu-id="6a9a5-122">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-122">Select **OK**.</span></span>
* <span data-ttu-id="6a9a5-123">Nella finestra **Crea una nuova applicazione Web ASP.NET Core** selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-123">In the **Create a new ASP.NET Core Web Application** window, select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a9a5-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a9a5-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6a9a5-125">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6a9a5-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="6a9a5-126">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-126">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="6a9a5-127">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="6a9a5-127">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="6a9a5-128">Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *WebApp1*.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-128">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="6a9a5-129">`-uld` usa LocalDB invece di SQLite.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-129">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="6a9a5-130">Omettere `-uld` per usare SQLite.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-130">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="6a9a5-131">`-au Individual` crea il codice per l'autenticazione individuale.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-131">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="6a9a5-132">Il comando `code` apre la cartella *WebApp1* in una nuova istanza di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-132">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

* <span data-ttu-id="6a9a5-133">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'WebApp1'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="6a9a5-133">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span> <span data-ttu-id="6a9a5-134">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-134">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a9a5-135">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="6a9a5-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6a9a5-136">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-136">Select **File** > **New Solution**.</span></span>
* <span data-ttu-id="6a9a5-137">Selezionare **.NET Core** > **App** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-137">Select **.NET Core** > **App** in the sidebar.</span></span> <span data-ttu-id="6a9a5-138">Selezionare il modello **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-138">Select the **Web Application** template.</span></span> <span data-ttu-id="6a9a5-139">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-139">Select **Next**.</span></span>
* <span data-ttu-id="6a9a5-140">Impostare l'elenco a discesa **Framework di destinazione** su **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-140">Set the **Target Framework** drop down to **.NET Core 2.2**.</span></span> <span data-ttu-id="6a9a5-141">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-141">Select **Next**.</span></span>
* <span data-ttu-id="6a9a5-142">Specificare un **Nome progetto**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-142">Provide a **Project Name**.</span></span> <span data-ttu-id="6a9a5-143">Confermare o modificare il **Percorso**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-143">Confirm or change the **Location**.</span></span> <span data-ttu-id="6a9a5-144">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-144">Select **Create**.</span></span>

---

## <a name="apply-migrations"></a><span data-ttu-id="6a9a5-145">Applicare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="6a9a5-145">Apply migrations</span></span>

* <span data-ttu-id="6a9a5-146">Eseguire l'app e selezionare il collegamento **Registra**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-146">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="6a9a5-147">Immettere l'indirizzo di posta elettronica e la password per il nuovo account, quindi selezionare **Registra**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-147">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="6a9a5-148">Seguire le istruzioni per applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-148">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="6a9a5-149">Usare SecretManager per archiviare i token assegnati dai provider di accesso</span><span class="sxs-lookup"><span data-stu-id="6a9a5-149">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="6a9a5-150">I provider di accesso ai social assegnano i token **ID applicazione** e **Segreto dell'applicazione** durante il processo di registrazione.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-150">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="6a9a5-151">La denominazione esatta dei token varia a seconda del provider.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-151">The exact token names vary by provider.</span></span> <span data-ttu-id="6a9a5-152">Questi token rappresentano le credenziali usate dall'app per accedere alle API.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-152">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="6a9a5-153">I token costituiscono i "segreti" che è possibile collegare alla configurazione dell'app usando [Secret Manager](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="6a9a5-153">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="6a9a5-154">Secret Manager è un'alternativa che offre maggior sicurezza rispetto alla memorizzazione dei token in un file di configurazione, ad esempio *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-154">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a9a5-155">Secret Manager è progettato esclusivamente per lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-155">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="6a9a5-156">È possibile memorizzare e proteggere i segreti relativi al test e alla produzione di Azure usando il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="6a9a5-156">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="6a9a5-157">Seguire la procedura descritta nell'argomento [Archiviazione sicura di segreti dell'app durante lo sviluppo di ASP.NET Core](xref:security/app-secrets) per archiviare i token assegnati dai singoli provider di accesso elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-157">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="6a9a5-158">Impostare i provider di accesso necessari per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="6a9a5-158">Setup login providers required by your application</span></span>

<span data-ttu-id="6a9a5-159">Usare gli argomenti seguenti per configurare l'applicazione per usare i rispettivi provider:</span><span class="sxs-lookup"><span data-stu-id="6a9a5-159">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="6a9a5-160">Istruzioni di [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="6a9a5-160">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="6a9a5-161">Istruzioni di [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="6a9a5-161">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="6a9a5-162">Istruzioni di [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="6a9a5-162">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="6a9a5-163">Istruzioni di [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="6a9a5-163">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="6a9a5-164">Istruzioni di [altri provider](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="6a9a5-164">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="6a9a5-165">Impostare facoltativamente la password</span><span class="sxs-lookup"><span data-stu-id="6a9a5-165">Optionally set password</span></span>

<span data-ttu-id="6a9a5-166">Quando ci si registra con un provider di accesso esterno, non si dispone di una password registrata con l'app.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-166">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="6a9a5-167">Questo evita la creazione e la memorizzazione di una password per il sito, ma crea anche una dipendenza dal provider di accesso esterno.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-167">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="6a9a5-168">Se il provider di accesso esterno non è disponibile, non si potrà accedere al sito web.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-168">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="6a9a5-169">Per creare una password e accedere usando la posta elettronica impostata durante il processo di accesso con provider esterni:</span><span class="sxs-lookup"><span data-stu-id="6a9a5-169">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="6a9a5-170">Selezionare il collegamento **Salve &lt;alias di posta elettronica&gt;** nell'angolo superiore destro per passare alla vista **Gestione**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-170">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Vista Gestione dell'applicazione Web](index/_static/pass1a.png)

* <span data-ttu-id="6a9a5-172">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-172">Select **Create**</span></span>

![Impostare la pagina della password](index/_static/pass2a.png)

* <span data-ttu-id="6a9a5-174">Impostare una password valida in modo da usarla per accedere con la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-174">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a9a5-175">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6a9a5-175">Next steps</span></span>

* <span data-ttu-id="6a9a5-176">Questo articolo ha introdotto l'autenticazione esterna e ha illustrato i prerequisiti necessari per aggiungere gli account di accesso esterni all'app di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-176">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="6a9a5-177">Pagine di riferimento specifico del provider per configurare gli account di accesso per i provider richiesti dall'app.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-177">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="6a9a5-178">Può essere opportuno salvare in modo permanente dati aggiuntivi sull'utente e il relativo accesso e i token di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-178">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="6a9a5-179">Per altre informazioni, vedere <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="6a9a5-179">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
