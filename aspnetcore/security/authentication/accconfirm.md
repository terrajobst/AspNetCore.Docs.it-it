---
title: Conferma account e recupero della password in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 3ca6d014245bb2a9bc4b1c90285f47eec7cefe84
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655472"
---
::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="3faed-103">Visualizzare [questo file PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) per ASP.NET Core 1.1 e versione 2.1.</span><span class="sxs-lookup"><span data-stu-id="3faed-103">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="3faed-104">Conferma account e recupero della password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3faed-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="3faed-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="3faed-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="3faed-106">Questa esercitazione illustra come compilare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="3faed-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="3faed-107">Questa esercitazione viene **non** un argomento di inizio.</span><span class="sxs-lookup"><span data-stu-id="3faed-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="3faed-108">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="3faed-108">You should be familiar with:</span></span>

* [<span data-ttu-id="3faed-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3faed-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="3faed-110">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="3faed-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="3faed-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3faed-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="3faed-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3faed-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="3faed-113">Creare un'app web ed eseguire lo scaffolding di identità</span><span class="sxs-lookup"><span data-stu-id="3faed-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3faed-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3faed-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="3faed-115">In Visual Studio, creare una nuova **applicazione Web** progetto denominato **WebPWrecover**.</span><span class="sxs-lookup"><span data-stu-id="3faed-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="3faed-116">Selezionare **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="3faed-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="3faed-117">Mantenere il valore predefinito **Authentication** impostata su **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="3faed-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="3faed-118">L'autenticazione viene aggiunto nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="3faed-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="3faed-119">Nel passaggio successivo:</span><span class="sxs-lookup"><span data-stu-id="3faed-119">In the next step:</span></span>

* <span data-ttu-id="3faed-120">Impostare la pagina di layout *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3faed-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="3faed-121">Selezionare *Account/Register*</span><span class="sxs-lookup"><span data-stu-id="3faed-121">Select *Account/Register*</span></span>
* <span data-ttu-id="3faed-122">Creare un nuovo **alla classe contesto dati**</span><span class="sxs-lookup"><span data-stu-id="3faed-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3faed-123">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="3faed-123">.NET Core CLI</span></span>](#tab/netcore-cli)

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

<span data-ttu-id="3faed-124">Eseguire `dotnet aspnet-codegenerator identity --help` per ottenere informazioni sullo strumento di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="3faed-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="3faed-125">Seguire le istruzioni in [abilitare l'autenticazione](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="3faed-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="3faed-126">Aggiungere `app.UseAuthentication();` a `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="3faed-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="3faed-127">Aggiungere `<partial name="_LoginPartial" />` nel file di layout.</span><span class="sxs-lookup"><span data-stu-id="3faed-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="3faed-128">Registrazione di nuovi utenti di test</span><span class="sxs-lookup"><span data-stu-id="3faed-128">Test new user registration</span></span>

<span data-ttu-id="3faed-129">Eseguire l'app, selezionare la **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="3faed-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="3faed-130">A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo.</span><span class="sxs-lookup"><span data-stu-id="3faed-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="3faed-131">Dopo aver inviato la registrazione, si è connessi all'app.</span><span class="sxs-lookup"><span data-stu-id="3faed-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="3faed-132">Più avanti nell'esercitazione, il codice venga aggiornato in modo che i nuovi utenti non possono accedere fino a quando non viene convalidata la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3faed-132">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="3faed-133">Si noti la tabella `EmailConfirmed` campo è `False`.</span><span class="sxs-lookup"><span data-stu-id="3faed-133">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="3faed-134">Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="3faed-134">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="3faed-135">Fare doppio clic sulla riga e selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="3faed-135">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="3faed-136">L'eliminazione di alias di posta elettronica rende più semplice nella procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="3faed-136">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="3faed-137">Richiedere conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="3faed-137">Require email confirmation</span></span>

<span data-ttu-id="3faed-138">È consigliabile verificare che il messaggio di posta elettronica di una nuova registrazione utente.</span><span class="sxs-lookup"><span data-stu-id="3faed-138">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="3faed-139">Inviare tramite posta elettronica di conferma consente di verificare che non sta rappresentando qualcun altro (vale a dire non hanno registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="3faed-139">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="3faed-140">Si supponga che si ha un forum di discussione e si desidera evitare che "yli@example.com"da registrare come"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="3faed-140">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="3faed-141">Senza conferma tramite posta elettronica, "nolivetto@contoso.com" può ricevere e-mail indesiderate dalla propria app.</span><span class="sxs-lookup"><span data-stu-id="3faed-141">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="3faed-142">Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non l'aveste notato l'errore di ortografia di "yli".</span><span class="sxs-lookup"><span data-stu-id="3faed-142">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="3faed-143">Essi non sarebbe in grado di utilizzare il recupero della password perché l'app non dispone di posta elettronica corretta.</span><span class="sxs-lookup"><span data-stu-id="3faed-143">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="3faed-144">Conferma tramite posta elettronica fornisce una protezione limitata dal BOT.</span><span class="sxs-lookup"><span data-stu-id="3faed-144">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="3faed-145">Conferma tramite posta elettronica non fornisce protezione da utenti malintenzionati con numero di account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3faed-145">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="3faed-146">In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che abbiano un messaggio di posta elettronica confermato.</span><span class="sxs-lookup"><span data-stu-id="3faed-146">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="3faed-147">Update *Areas/Identity/IdentityHostingStartup.cs* per richiedere un indirizzo di posta elettronica confermato:</span><span class="sxs-lookup"><span data-stu-id="3faed-147">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="3faed-148">`config.SignIn.RequireConfirmedEmail = true;` impedisce l'accesso fino a quando non viene confermata la posta elettronica agli utenti registrati.</span><span class="sxs-lookup"><span data-stu-id="3faed-148">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="3faed-149">Configurare il provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="3faed-149">Configure email provider</span></span>

<span data-ttu-id="3faed-150">In questa esercitazione [SendGrid](https://sendgrid.com) viene usato per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3faed-150">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="3faed-151">È necessario un account SendGrid e una chiave per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3faed-151">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="3faed-152">È possibile usare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3faed-152">You can use other email providers.</span></span> <span data-ttu-id="3faed-153">ASP.NET Core 2.x include `System.Net.Mail`, pertanto è possibile inviare tramite posta elettronica dall'app.</span><span class="sxs-lookup"><span data-stu-id="3faed-153">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="3faed-154">È consigliabile che usare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3faed-154">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="3faed-155">SMTP sono difficili da proteggere e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="3faed-155">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="3faed-156">Creare una classe per recuperare la chiave di protezione della posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3faed-156">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="3faed-157">Per questo esempio, creare *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="3faed-157">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="3faed-158">Configurare i segreti utente di SendGrid</span><span class="sxs-lookup"><span data-stu-id="3faed-158">Configure SendGrid user secrets</span></span>

<span data-ttu-id="3faed-159">Aggiungere un valore univoco `<UserSecretsId>` valore per il `<PropertyGroup>` elemento del file di progetto:</span><span class="sxs-lookup"><span data-stu-id="3faed-159">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="3faed-160">Impostare il `SendGridUser` e `SendGridKey` con il [strumento secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3faed-160">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="3faed-161">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3faed-161">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="3faed-162">In Windows, Secret Manager archivia le coppie di chiavi/valore in una *Secrets* del file nei `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="3faed-162">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="3faed-163">Il contenuto del *Secrets* file non vengono crittografati.</span><span class="sxs-lookup"><span data-stu-id="3faed-163">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="3faed-164">Il *Secrets* file è illustrato di seguito (il `SendGridKey` valore è stato rimosso.)</span><span class="sxs-lookup"><span data-stu-id="3faed-164">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
<span data-ttu-id="3faed-165">Per altre informazioni, vedere la [modello di opzioni](xref:fundamentals/configuration/options) e [configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3faed-165">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="3faed-166">Installare SendGrid</span><span class="sxs-lookup"><span data-stu-id="3faed-166">Install SendGrid</span></span>

<span data-ttu-id="3faed-167">Questa esercitazione illustra come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="3faed-167">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="3faed-168">Installare il `SendGrid` pacchetto NuGet:</span><span class="sxs-lookup"><span data-stu-id="3faed-168">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3faed-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3faed-169">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="3faed-170">Dalla Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="3faed-170">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3faed-171">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="3faed-171">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="3faed-172">Dalla console, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="3faed-172">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="3faed-173">Visualizzare [inizia gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="3faed-173">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="3faed-174">Implementare IEmailSender</span><span class="sxs-lookup"><span data-stu-id="3faed-174">Implement IEmailSender</span></span>

<span data-ttu-id="3faed-175">L'implementazione `IEmailSender`, creare *Services/EmailSender.cs* con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3faed-175">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="3faed-176">Configurare l'avvio per il supporto tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="3faed-176">Configure startup to support email</span></span>

<span data-ttu-id="3faed-177">Aggiungere il codice seguente per il `ConfigureServices` metodo nella *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="3faed-177">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="3faed-178">Aggiungere `EmailSender` come un servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="3faed-178">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="3faed-179">Registrare il `AuthMessageSenderOptions` istanza di configurazione.</span><span class="sxs-lookup"><span data-stu-id="3faed-179">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="3faed-180">Abilitare il ripristino di conferma e la password account</span><span class="sxs-lookup"><span data-stu-id="3faed-180">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="3faed-181">Il modello presenta il codice per il ripristino di conferma e la password di account.</span><span class="sxs-lookup"><span data-stu-id="3faed-181">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="3faed-182">Trovare il `OnPostAsync` nel metodo *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="3faed-182">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="3faed-183">Impedire che gli utenti appena registrati viene connesso automaticamente impostando come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="3faed-183">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3faed-184">Il metodo completo viene visualizzato con la riga modificata evidenziata:</span><span class="sxs-lookup"><span data-stu-id="3faed-184">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="3faed-185">Registrare, messaggio di posta elettronica di conferma e reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="3faed-185">Register, confirm email, and reset password</span></span>

<span data-ttu-id="3faed-186">Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.</span><span class="sxs-lookup"><span data-stu-id="3faed-186">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="3faed-187">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="3faed-187">Run the app and register a new user</span></span>

  ![Visualizzazione Account registrazione dell'applicazione Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="3faed-189">Controllare la posta elettronica per il collegamento di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="3faed-189">Check your email for the account confirmation link.</span></span> <span data-ttu-id="3faed-190">Visualizzare [eseguire il Debug tramite posta elettronica](#debug) se non si riceve il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3faed-190">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="3faed-191">Fare clic sul collegamento per confermare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3faed-191">Click the link to confirm your email.</span></span>
* <span data-ttu-id="3faed-192">Accedere con l'indirizzo di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="3faed-192">Log in with your email and password.</span></span>
* <span data-ttu-id="3faed-193">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="3faed-193">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="3faed-194">Visualizzare la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="3faed-194">View the manage page</span></span>

<span data-ttu-id="3faed-195">Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="3faed-195">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="3faed-196">Potrebbe essere necessario espandere la barra di spostamento per visualizzare il nome utente.</span><span class="sxs-lookup"><span data-stu-id="3faed-196">You might need to expand the navbar to see user name.</span></span>

![Nella barra di spostamento](accconfirm/_static/x.png)

<span data-ttu-id="3faed-198">La pagina di gestione viene visualizzata con il **profilo** selezionato della scheda.</span><span class="sxs-lookup"><span data-stu-id="3faed-198">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="3faed-199">Il **messaggio di posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.</span><span class="sxs-lookup"><span data-stu-id="3faed-199">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="3faed-200">Reimpostazione della password di test</span><span class="sxs-lookup"><span data-stu-id="3faed-200">Test password reset</span></span>

* <span data-ttu-id="3faed-201">Se si è connessi, selezionare **Logout**.</span><span class="sxs-lookup"><span data-stu-id="3faed-201">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="3faed-202">Selezionare il **accedere** collegamento e selezionare il **password dimenticata?** collegamento.</span><span class="sxs-lookup"><span data-stu-id="3faed-202">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="3faed-203">Immettere l'indirizzo di posta elettronica usato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="3faed-203">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="3faed-204">Viene inviato un messaggio di posta elettronica con un collegamento di reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="3faed-204">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="3faed-205">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="3faed-205">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="3faed-206">Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="3faed-206">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="3faed-207">Eseguire il debug tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="3faed-207">Debug email</span></span>

<span data-ttu-id="3faed-208">Se non è possibile ottenere l'indirizzo di posta elettronica funzionante:</span><span class="sxs-lookup"><span data-stu-id="3faed-208">If you can't get email working:</span></span>

* <span data-ttu-id="3faed-209">Impostare un punto di interruzione `EmailSender.Execute` per verificare `SendGridClient.SendEmailAsync` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="3faed-209">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="3faed-210">Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) usando codice simile a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="3faed-210">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="3faed-211">Rivedere le [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.</span><span class="sxs-lookup"><span data-stu-id="3faed-211">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="3faed-212">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="3faed-212">Check your spam folder.</span></span>
* <span data-ttu-id="3faed-213">Provare un altro alias di posta elettronica in un provider di posta elettronica diverso (Microsoft, Yahoo, Gmail e così via)</span><span class="sxs-lookup"><span data-stu-id="3faed-213">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="3faed-214">Provare a inviare agli account di posta elettronica diverso.</span><span class="sxs-lookup"><span data-stu-id="3faed-214">Try sending to different email accounts.</span></span>

<span data-ttu-id="3faed-215">**Una procedura consigliata** consiste nel **non** usare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="3faed-215">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="3faed-216">Se si pubblica l'app in Azure, è possibile impostare i segreti di SendGrid come impostazioni dell'applicazione nel portale di App Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="3faed-216">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="3faed-217">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="3faed-217">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="3faed-218">Combinare gli account di accesso social e locali</span><span class="sxs-lookup"><span data-stu-id="3faed-218">Combine social and local login accounts</span></span>

<span data-ttu-id="3faed-219">Per completare questa sezione, è innanzitutto necessario abilitare un provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="3faed-219">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="3faed-220">Visualizzare [Facebook, Google e l'autenticazione esterna provider](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3faed-220">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="3faed-221">È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="3faed-221">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="3faed-222">Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso basati su social network, prima di tutto, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="3faed-222">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="3faed-224">Fare clic sui **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="3faed-224">Click on the **Manage** link.</span></span> <span data-ttu-id="3faed-225">Si noti 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="3faed-225">Note the 0 external (social logins) associated with this account.</span></span>

![Gestione visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="3faed-227">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="3faed-227">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="3faed-228">Nell'immagine seguente, Facebook è il provider di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="3faed-228">In the following image, Facebook is the external authentication provider:</span></span>

![Gestire la visualizzazione di account di accesso esterni listato Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="3faed-230">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="3faed-230">The two accounts have been combined.</span></span> <span data-ttu-id="3faed-231">Si è in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="3faed-231">You are able to log on with either account.</span></span> <span data-ttu-id="3faed-232">È possibile che gli utenti per aggiungere gli account locali nel caso in cui il servizio di autenticazione account di accesso basati su social network è inattivo oppure più probabile che abbia perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="3faed-232">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="3faed-233">Abilitare la conferma dell'account dopo un sito ha utenti</span><span class="sxs-lookup"><span data-stu-id="3faed-233">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="3faed-234">Abilitare la conferma dell'account in un sito con gli utenti per bloccare tutti gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="3faed-234">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="3faed-235">Gli utenti esistenti sono bloccati perché i propri account non confermato.</span><span class="sxs-lookup"><span data-stu-id="3faed-235">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="3faed-236">Per evitare il blocco degli utente esistente, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="3faed-236">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="3faed-237">Aggiornare il database per contrassegnare tutti gli utenti esistenti come viene confermata.</span><span class="sxs-lookup"><span data-stu-id="3faed-237">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="3faed-238">Verificare che gli utenti in fase di chiusura.</span><span class="sxs-lookup"><span data-stu-id="3faed-238">Confirm exiting users.</span></span> <span data-ttu-id="3faed-239">Ad esempio, batch-trasmissione messaggi di posta elettronica con collegamenti di conferma.</span><span class="sxs-lookup"><span data-stu-id="3faed-239">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
