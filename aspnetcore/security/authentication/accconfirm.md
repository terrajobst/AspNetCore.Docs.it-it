---
title: Conferma account e recupero della password in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 8e175cd19ca4a9de1e7cf6b330b3d82f309b6501
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063338"
---
::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="ebc55-103">Visualizzare [questo file PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) per ASP.NET Core 1.1 e versione 2.1.</span><span class="sxs-lookup"><span data-stu-id="ebc55-103">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="ebc55-104">Conferma account e recupero della password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc55-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="ebc55-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="ebc55-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="ebc55-106">Questa esercitazione illustra come compilare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="ebc55-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="ebc55-107">Questa esercitazione viene **non** un argomento di inizio.</span><span class="sxs-lookup"><span data-stu-id="ebc55-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="ebc55-108">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="ebc55-108">You should be familiar with:</span></span>

* [<span data-ttu-id="ebc55-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc55-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="ebc55-110">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="ebc55-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="ebc55-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ebc55-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="ebc55-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ebc55-112">Prerequisites</span></span>

<span data-ttu-id="ebc55-113">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="ebc55-113">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span></span>

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="ebc55-114">Creare un'app web ed eseguire lo scaffolding di identità</span><span class="sxs-lookup"><span data-stu-id="ebc55-114">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ebc55-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebc55-115">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="ebc55-116">In Visual Studio, creare una nuova **applicazione Web** progetto.</span><span class="sxs-lookup"><span data-stu-id="ebc55-116">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="ebc55-117">Selezionare **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="ebc55-117">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="ebc55-118">Mantenere il valore predefinito **Authentication** impostata su **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="ebc55-118">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="ebc55-119">L'autenticazione viene aggiunto nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="ebc55-119">Authentication is added in the next step.</span></span>

<span data-ttu-id="ebc55-120">Nel passaggio successivo:</span><span class="sxs-lookup"><span data-stu-id="ebc55-120">In the next step:</span></span>

* <span data-ttu-id="ebc55-121">Impostare la pagina di layout *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ebc55-121">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="ebc55-122">Selezionare *Account/Register*</span><span class="sxs-lookup"><span data-stu-id="ebc55-122">Select *Account/Register*</span></span>
* <span data-ttu-id="ebc55-123">Creare un nuovo **alla classe contesto dati**</span><span class="sxs-lookup"><span data-stu-id="ebc55-123">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ebc55-124">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc55-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

<span data-ttu-id="ebc55-125">Eseguire `dotnet aspnet-codegenerator identity --help` per ottenere informazioni sullo strumento di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="ebc55-125">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="ebc55-126">Seguire le istruzioni in [abilitare l'autenticazione](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="ebc55-126">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="ebc55-127">Aggiungere `app.UseAuthentication();` a `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="ebc55-127">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="ebc55-128">Aggiungere `<partial name="_LoginPartial" />` nel file di layout.</span><span class="sxs-lookup"><span data-stu-id="ebc55-128">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="ebc55-129">Registrazione di nuovi utenti di test</span><span class="sxs-lookup"><span data-stu-id="ebc55-129">Test new user registration</span></span>

<span data-ttu-id="ebc55-130">Eseguire l'app, selezionare la **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="ebc55-130">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="ebc55-131">A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo.</span><span class="sxs-lookup"><span data-stu-id="ebc55-131">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="ebc55-132">Dopo aver inviato la registrazione, si è connessi all'app.</span><span class="sxs-lookup"><span data-stu-id="ebc55-132">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="ebc55-133">Più avanti nell'esercitazione, il codice venga aggiornato in modo che i nuovi utenti non possono accedere fino a quando non viene convalidata la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ebc55-133">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="ebc55-134">Visualizzare il database di identità</span><span class="sxs-lookup"><span data-stu-id="ebc55-134">View the Identity database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ebc55-135">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebc55-135">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="ebc55-136">Dal **View** dal menu **Esplora oggetti di SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="ebc55-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="ebc55-137">Passare a **(localdb) MSSQLLocalDB (Server SQL 13)**.</span><span class="sxs-lookup"><span data-stu-id="ebc55-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="ebc55-138">Fare clic su **dbo. AspNetUsers** > **consente di visualizzare dati**:</span><span class="sxs-lookup"><span data-stu-id="ebc55-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu di scelta rapida nella tabella AspNetUsers in Esplora oggetti di SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="ebc55-140">Si noti la tabella `EmailConfirmed` campo è `False`.</span><span class="sxs-lookup"><span data-stu-id="ebc55-140">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="ebc55-141">Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="ebc55-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="ebc55-142">Fare doppio clic sulla riga e selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="ebc55-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="ebc55-143">L'eliminazione di alias di posta elettronica rende più semplice nella procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="ebc55-143">Deleting the email alias makes it easier in the following steps.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ebc55-144">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc55-144">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ebc55-145">Visualizzare [utilizzare SQLite in un progetto ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) per istruzioni su come visualizzare i database SQLite.</span><span class="sxs-lookup"><span data-stu-id="ebc55-145">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

------

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="ebc55-146">Richiedere conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="ebc55-146">Require email confirmation</span></span>

<span data-ttu-id="ebc55-147">È consigliabile verificare che il messaggio di posta elettronica di una nuova registrazione utente.</span><span class="sxs-lookup"><span data-stu-id="ebc55-147">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="ebc55-148">Inviare tramite posta elettronica di conferma consente di verificare che non sta rappresentando qualcun altro (vale a dire non hanno registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="ebc55-148">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="ebc55-149">Si supponga che si ha un forum di discussione e si desidera evitare che "yli@example.com"da registrare come"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="ebc55-149">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="ebc55-150">Senza conferma tramite posta elettronica, "nolivetto@contoso.com" può ricevere e-mail indesiderate dalla propria app.</span><span class="sxs-lookup"><span data-stu-id="ebc55-150">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="ebc55-151">Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non l'aveste notato l'errore di ortografia di "yli".</span><span class="sxs-lookup"><span data-stu-id="ebc55-151">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="ebc55-152">Essi non sarebbe in grado di utilizzare il recupero della password perché l'app non dispone di posta elettronica corretta.</span><span class="sxs-lookup"><span data-stu-id="ebc55-152">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="ebc55-153">Conferma tramite posta elettronica fornisce una protezione limitata dal BOT.</span><span class="sxs-lookup"><span data-stu-id="ebc55-153">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="ebc55-154">Conferma tramite posta elettronica non fornisce protezione da utenti malintenzionati con numero di account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ebc55-154">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="ebc55-155">In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che abbiano un messaggio di posta elettronica confermato.</span><span class="sxs-lookup"><span data-stu-id="ebc55-155">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="ebc55-156">Update *Areas/Identity/IdentityHostingStartup.cs* per richiedere un indirizzo di posta elettronica confermato:</span><span class="sxs-lookup"><span data-stu-id="ebc55-156">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="ebc55-157">`config.SignIn.RequireConfirmedEmail = true;` impedisce l'accesso fino a quando non viene confermata la posta elettronica agli utenti registrati.</span><span class="sxs-lookup"><span data-stu-id="ebc55-157">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="ebc55-158">Configurare il provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="ebc55-158">Configure email provider</span></span>

<span data-ttu-id="ebc55-159">In questa esercitazione [SendGrid](https://sendgrid.com) viene usato per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ebc55-159">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="ebc55-160">È necessario un account SendGrid e una chiave per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ebc55-160">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="ebc55-161">È possibile usare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ebc55-161">You can use other email providers.</span></span> <span data-ttu-id="ebc55-162">ASP.NET Core 2.x include `System.Net.Mail`, pertanto è possibile inviare tramite posta elettronica dall'app.</span><span class="sxs-lookup"><span data-stu-id="ebc55-162">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="ebc55-163">È consigliabile che usare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ebc55-163">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="ebc55-164">SMTP sono difficili da proteggere e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="ebc55-164">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="ebc55-165">Il [modello di opzioni](xref:fundamentals/configuration/options) viene utilizzato per accedere alle impostazioni account e la chiave dell'utente.</span><span class="sxs-lookup"><span data-stu-id="ebc55-165">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="ebc55-166">Per altre informazioni, vedere [configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="ebc55-166">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="ebc55-167">Creare una classe per recuperare la chiave di protezione della posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ebc55-167">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="ebc55-168">Per questo esempio, creare *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="ebc55-168">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="ebc55-169">Configurare i segreti utente di SendGrid</span><span class="sxs-lookup"><span data-stu-id="ebc55-169">Configure SendGrid user secrets</span></span>

<span data-ttu-id="ebc55-170">Aggiungere un valore univoco `<UserSecretsId>` valore per il `<PropertyGroup>` elemento del file di progetto:</span><span class="sxs-lookup"><span data-stu-id="ebc55-170">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="ebc55-171">Impostare il `SendGridUser` e `SendGridKey` con il [strumento secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="ebc55-171">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="ebc55-172">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ebc55-172">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="ebc55-173">In Windows, Secret Manager archivia le coppie di chiavi/valore in una *Secrets* del file nei `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="ebc55-173">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="ebc55-174">Il contenuto del *Secrets* file non vengono crittografati.</span><span class="sxs-lookup"><span data-stu-id="ebc55-174">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="ebc55-175">Il *Secrets* file è illustrato di seguito (il `SendGridKey` valore è stato rimosso.)</span><span class="sxs-lookup"><span data-stu-id="ebc55-175">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="install-sendgrid"></a><span data-ttu-id="ebc55-176">Installare SendGrid</span><span class="sxs-lookup"><span data-stu-id="ebc55-176">Install SendGrid</span></span>

<span data-ttu-id="ebc55-177">Questa esercitazione illustra come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="ebc55-177">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="ebc55-178">Installare il `SendGrid` pacchetto NuGet:</span><span class="sxs-lookup"><span data-stu-id="ebc55-178">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ebc55-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebc55-179">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="ebc55-180">Dalla Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ebc55-180">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ebc55-181">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc55-181">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ebc55-182">Dalla console, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ebc55-182">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="ebc55-183">Visualizzare [inizia gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="ebc55-183">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="ebc55-184">Implementare IEmailSender</span><span class="sxs-lookup"><span data-stu-id="ebc55-184">Implement IEmailSender</span></span>

<span data-ttu-id="ebc55-185">L'implementazione `IEmailSender`, creare *Services/EmailSender.cs* con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ebc55-185">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="ebc55-186">Configurare l'avvio per il supporto tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="ebc55-186">Configure startup to support email</span></span>

<span data-ttu-id="ebc55-187">Aggiungere il codice seguente per il `ConfigureServices` metodo nella *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="ebc55-187">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="ebc55-188">Aggiungere `EmailSender` come un servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="ebc55-188">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="ebc55-189">Registrare il `AuthMessageSenderOptions` istanza di configurazione.</span><span class="sxs-lookup"><span data-stu-id="ebc55-189">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="ebc55-190">Abilitare il ripristino di conferma e la password account</span><span class="sxs-lookup"><span data-stu-id="ebc55-190">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="ebc55-191">Il modello presenta il codice per il ripristino di conferma e la password di account.</span><span class="sxs-lookup"><span data-stu-id="ebc55-191">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="ebc55-192">Trovare il `OnPostAsync` nel metodo *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="ebc55-192">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="ebc55-193">Impedire che gli utenti appena registrati viene connesso automaticamente impostando come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="ebc55-193">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="ebc55-194">Il metodo completo viene visualizzato con la riga modificata evidenziata:</span><span class="sxs-lookup"><span data-stu-id="ebc55-194">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="ebc55-195">Registrare, messaggio di posta elettronica di conferma e reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="ebc55-195">Register, confirm email, and reset password</span></span>

<span data-ttu-id="ebc55-196">Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.</span><span class="sxs-lookup"><span data-stu-id="ebc55-196">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="ebc55-197">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="ebc55-197">Run the app and register a new user</span></span>

  ![Visualizzazione Account registrazione dell'applicazione Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="ebc55-199">Controllare la posta elettronica per il collegamento di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="ebc55-199">Check your email for the account confirmation link.</span></span> <span data-ttu-id="ebc55-200">Visualizzare [eseguire il Debug tramite posta elettronica](#debug) se non si riceve il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ebc55-200">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="ebc55-201">Fare clic sul collegamento per confermare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="ebc55-201">Click the link to confirm your email.</span></span>
* <span data-ttu-id="ebc55-202">Accedere con l'indirizzo di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="ebc55-202">Log in with your email and password.</span></span>
* <span data-ttu-id="ebc55-203">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="ebc55-203">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="ebc55-204">Visualizzare la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="ebc55-204">View the manage page</span></span>

<span data-ttu-id="ebc55-205">Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="ebc55-205">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="ebc55-206">Potrebbe essere necessario espandere la barra di spostamento per visualizzare il nome utente.</span><span class="sxs-lookup"><span data-stu-id="ebc55-206">You might need to expand the navbar to see user name.</span></span>

![Nella barra di spostamento](accconfirm/_static/x.png)

<span data-ttu-id="ebc55-208">La pagina di gestione viene visualizzata con il **profilo** selezionato della scheda.</span><span class="sxs-lookup"><span data-stu-id="ebc55-208">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="ebc55-209">Il **messaggio di posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.</span><span class="sxs-lookup"><span data-stu-id="ebc55-209">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="ebc55-210">Reimpostazione della password di test</span><span class="sxs-lookup"><span data-stu-id="ebc55-210">Test password reset</span></span>

* <span data-ttu-id="ebc55-211">Se si è connessi, selezionare **Logout**.</span><span class="sxs-lookup"><span data-stu-id="ebc55-211">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="ebc55-212">Selezionare il **accedere** collegamento e selezionare il **password dimenticata?** collegamento.</span><span class="sxs-lookup"><span data-stu-id="ebc55-212">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="ebc55-213">Immettere l'indirizzo di posta elettronica usato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="ebc55-213">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="ebc55-214">Viene inviato un messaggio di posta elettronica con un collegamento di reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="ebc55-214">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="ebc55-215">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="ebc55-215">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="ebc55-216">Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="ebc55-216">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="ebc55-217">Eseguire il debug tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="ebc55-217">Debug email</span></span>

<span data-ttu-id="ebc55-218">Se non è possibile ottenere l'indirizzo di posta elettronica funzionante:</span><span class="sxs-lookup"><span data-stu-id="ebc55-218">If you can't get email working:</span></span>

* <span data-ttu-id="ebc55-219">Impostare un punto di interruzione `EmailSender.Execute` per verificare `SendGridClient.SendEmailAsync` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="ebc55-219">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="ebc55-220">Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) usando codice simile a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="ebc55-220">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="ebc55-221">Rivedere le [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.</span><span class="sxs-lookup"><span data-stu-id="ebc55-221">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="ebc55-222">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="ebc55-222">Check your spam folder.</span></span>
* <span data-ttu-id="ebc55-223">Provare un altro alias di posta elettronica in un provider di posta elettronica diverso (Microsoft, Yahoo, Gmail e così via)</span><span class="sxs-lookup"><span data-stu-id="ebc55-223">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="ebc55-224">Provare a inviare agli account di posta elettronica diverso.</span><span class="sxs-lookup"><span data-stu-id="ebc55-224">Try sending to different email accounts.</span></span>

<span data-ttu-id="ebc55-225">**Una procedura consigliata** consiste nel **non** usare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="ebc55-225">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="ebc55-226">Se si pubblica l'app in Azure, è possibile impostare i segreti di SendGrid come impostazioni dell'applicazione nel portale di App Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="ebc55-226">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="ebc55-227">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="ebc55-227">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="ebc55-228">Combinare gli account di accesso social e locali</span><span class="sxs-lookup"><span data-stu-id="ebc55-228">Combine social and local login accounts</span></span>

<span data-ttu-id="ebc55-229">Per completare questa sezione, è innanzitutto necessario abilitare un provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="ebc55-229">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="ebc55-230">Visualizzare [Facebook, Google e l'autenticazione esterna provider](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ebc55-230">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="ebc55-231">È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="ebc55-231">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="ebc55-232">Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso basati su social network, prima di tutto, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="ebc55-232">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="ebc55-234">Fare clic sui **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="ebc55-234">Click on the **Manage** link.</span></span> <span data-ttu-id="ebc55-235">Si noti 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="ebc55-235">Note the 0 external (social logins) associated with this account.</span></span>

![Gestione visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="ebc55-237">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="ebc55-237">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="ebc55-238">Nell'immagine seguente, Facebook è il provider di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="ebc55-238">In the following image, Facebook is the external authentication provider:</span></span>

![Gestire la visualizzazione di account di accesso esterni listato Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="ebc55-240">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="ebc55-240">The two accounts have been combined.</span></span> <span data-ttu-id="ebc55-241">Si è in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="ebc55-241">You are able to log on with either account.</span></span> <span data-ttu-id="ebc55-242">È possibile che gli utenti per aggiungere gli account locali nel caso in cui il servizio di autenticazione account di accesso basati su social network è inattivo oppure più probabile che abbia perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="ebc55-242">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="ebc55-243">Abilitare la conferma dell'account dopo un sito ha utenti</span><span class="sxs-lookup"><span data-stu-id="ebc55-243">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="ebc55-244">Abilitare la conferma dell'account in un sito con gli utenti per bloccare tutti gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="ebc55-244">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="ebc55-245">Gli utenti esistenti sono bloccati perché i propri account non confermato.</span><span class="sxs-lookup"><span data-stu-id="ebc55-245">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="ebc55-246">Per evitare il blocco degli utente esistente, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="ebc55-246">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="ebc55-247">Aggiornare il database per contrassegnare tutti gli utenti esistenti come viene confermata.</span><span class="sxs-lookup"><span data-stu-id="ebc55-247">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="ebc55-248">Verificare che gli utenti in fase di chiusura.</span><span class="sxs-lookup"><span data-stu-id="ebc55-248">Confirm exiting users.</span></span> <span data-ttu-id="ebc55-249">Ad esempio, batch-trasmissione messaggi di posta elettronica con collegamenti di conferma.</span><span class="sxs-lookup"><span data-stu-id="ebc55-249">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end