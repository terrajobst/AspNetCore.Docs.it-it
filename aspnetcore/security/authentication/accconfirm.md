---
title: Conferma account e recupero della password in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
ms.author: riande
ms.date: 3/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 05efb75d26558702c88e87d191a780371034282c
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841475"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="761da-103">Conferma account e recupero della password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="761da-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="761da-104">Visualizzare [questo file PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) per ASP.NET Core 1.1 e versione 2.1.</span><span class="sxs-lookup"><span data-stu-id="761da-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="761da-105">Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="761da-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="761da-106">Questa esercitazione illustra come compilare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="761da-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="761da-107">Questa esercitazione viene **non** un argomento di inizio.</span><span class="sxs-lookup"><span data-stu-id="761da-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="761da-108">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="761da-108">You should be familiar with:</span></span>

* [<span data-ttu-id="761da-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="761da-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="761da-110">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="761da-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="761da-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="761da-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="761da-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="761da-112">Prerequisites</span></span>

[<span data-ttu-id="761da-113">.NET core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="761da-113">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="761da-114">Creare un'app web ed eseguire lo scaffolding di identità</span><span class="sxs-lookup"><span data-stu-id="761da-114">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="761da-115">Eseguire i comandi seguenti per creare un'app web con l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="761da-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="761da-116">Registrazione di nuovi utenti di test</span><span class="sxs-lookup"><span data-stu-id="761da-116">Test new user registration</span></span>

<span data-ttu-id="761da-117">Eseguire l'app, selezionare la **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="761da-117">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="761da-118">A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo.</span><span class="sxs-lookup"><span data-stu-id="761da-118">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="761da-119">Dopo aver inviato la registrazione, si è connessi all'app.</span><span class="sxs-lookup"><span data-stu-id="761da-119">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="761da-120">Più avanti nell'esercitazione, il codice venga aggiornato in modo che i nuovi utenti non riesci ad accedere fino a quando non viene convalidata la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-120">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="761da-121">Si noti la tabella `EmailConfirmed` campo è `False`.</span><span class="sxs-lookup"><span data-stu-id="761da-121">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="761da-122">Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="761da-122">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="761da-123">Fare doppio clic sulla riga e selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="761da-123">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="761da-124">L'eliminazione di alias di posta elettronica rende più semplice nella procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="761da-124">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="761da-125">Richiedere conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="761da-125">Require email confirmation</span></span>

<span data-ttu-id="761da-126">È consigliabile verificare che il messaggio di posta elettronica di una nuova registrazione utente.</span><span class="sxs-lookup"><span data-stu-id="761da-126">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="761da-127">Inviare tramite posta elettronica di conferma consente di verificare che non sta rappresentando qualcun altro (vale a dire non hanno registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="761da-127">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="761da-128">Si supponga che si ha un forum di discussione e si desidera evitare che "yli@example.com"da registrare come"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="761da-128">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="761da-129">Senza conferma tramite posta elettronica, "nolivetto@contoso.com" può ricevere e-mail indesiderate dalla propria app.</span><span class="sxs-lookup"><span data-stu-id="761da-129">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="761da-130">Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non l'aveste notato l'errore di ortografia di "yli".</span><span class="sxs-lookup"><span data-stu-id="761da-130">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="761da-131">Essi non sarebbe in grado di utilizzare il recupero della password perché l'app non dispone di posta elettronica corretta.</span><span class="sxs-lookup"><span data-stu-id="761da-131">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="761da-132">Conferma tramite posta elettronica fornisce una protezione limitata dal BOT.</span><span class="sxs-lookup"><span data-stu-id="761da-132">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="761da-133">Conferma tramite posta elettronica non fornisce protezione da utenti malintenzionati con numero di account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-133">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="761da-134">In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che abbiano un messaggio di posta elettronica confermato.</span><span class="sxs-lookup"><span data-stu-id="761da-134">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="761da-135">Aggiornamento `Startup.ConfigureServices` per richiedere un indirizzo di posta elettronica confermato:</span><span class="sxs-lookup"><span data-stu-id="761da-135">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="761da-136">`config.SignIn.RequireConfirmedEmail = true;` impedisce l'accesso fino a quando non viene confermata la posta elettronica agli utenti registrati.</span><span class="sxs-lookup"><span data-stu-id="761da-136">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="761da-137">Configurare il provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="761da-137">Configure email provider</span></span>

<span data-ttu-id="761da-138">In questa esercitazione [SendGrid](https://sendgrid.com) viene usato per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-138">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="761da-139">È necessario un account SendGrid e una chiave per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-139">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="761da-140">È possibile usare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-140">You can use other email providers.</span></span> <span data-ttu-id="761da-141">ASP.NET Core 2.x include `System.Net.Mail`, pertanto è possibile inviare tramite posta elettronica dall'app.</span><span class="sxs-lookup"><span data-stu-id="761da-141">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="761da-142">È consigliabile che usare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-142">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="761da-143">SMTP sono difficili da proteggere e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="761da-143">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="761da-144">Creare una classe per recuperare la chiave di protezione della posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-144">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="761da-145">Per questo esempio, creare *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="761da-145">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="761da-146">Configurare i segreti utente di SendGrid</span><span class="sxs-lookup"><span data-stu-id="761da-146">Configure SendGrid user secrets</span></span>

<span data-ttu-id="761da-147">Impostare il `SendGridUser` e `SendGridKey` con il [strumento secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="761da-147">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="761da-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="761da-148">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="761da-149">In Windows, Secret Manager archivia le coppie di chiavi/valore in una *Secrets* del file nei `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="761da-149">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="761da-150">Il contenuto del *Secrets* file non vengono crittografati.</span><span class="sxs-lookup"><span data-stu-id="761da-150">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="761da-151">Il markup seguente mostra le *Secrets* file.</span><span class="sxs-lookup"><span data-stu-id="761da-151">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="761da-152">Il `SendGridKey` valore è stato rimosso.</span><span class="sxs-lookup"><span data-stu-id="761da-152">The `SendGridKey` value has been removed.</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
<span data-ttu-id="761da-153">Per altre informazioni, vedere la [modello di opzioni](xref:fundamentals/configuration/options) e [configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="761da-153">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="761da-154">Installare SendGrid</span><span class="sxs-lookup"><span data-stu-id="761da-154">Install SendGrid</span></span>

<span data-ttu-id="761da-155">Questa esercitazione illustra come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="761da-155">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="761da-156">Installare il `SendGrid` pacchetto NuGet:</span><span class="sxs-lookup"><span data-stu-id="761da-156">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="761da-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="761da-157">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="761da-158">Dalla Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="761da-158">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="761da-159">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="761da-159">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="761da-160">Dalla console, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="761da-160">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="761da-161">Visualizzare [inizia gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="761da-161">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="761da-162">Implementare IEmailSender</span><span class="sxs-lookup"><span data-stu-id="761da-162">Implement IEmailSender</span></span>

<span data-ttu-id="761da-163">L'implementazione `IEmailSender`, creare *Services/EmailSender.cs* con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="761da-163">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="761da-164">Configurare l'avvio per il supporto tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="761da-164">Configure startup to support email</span></span>

<span data-ttu-id="761da-165">Aggiungere il codice seguente per il `ConfigureServices` metodo nella *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="761da-165">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="761da-166">Aggiungere `EmailSender` come un temporaneo del servizio.</span><span class="sxs-lookup"><span data-stu-id="761da-166">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="761da-167">Registrare il `AuthMessageSenderOptions` istanza di configurazione.</span><span class="sxs-lookup"><span data-stu-id="761da-167">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="761da-168">Abilitare il ripristino di conferma e la password account</span><span class="sxs-lookup"><span data-stu-id="761da-168">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="761da-169">Il modello presenta il codice per il ripristino di conferma e la password di account.</span><span class="sxs-lookup"><span data-stu-id="761da-169">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="761da-170">Trovare il `OnPostAsync` nel metodo *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="761da-170">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="761da-171">Impedire agli utenti appena registrati di riconnessi automaticamente impostando come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="761da-171">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="761da-172">Il metodo completo viene visualizzato con la riga modificata evidenziata:</span><span class="sxs-lookup"><span data-stu-id="761da-172">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="761da-173">Registrare, messaggio di posta elettronica di conferma e reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="761da-173">Register, confirm email, and reset password</span></span>

<span data-ttu-id="761da-174">Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.</span><span class="sxs-lookup"><span data-stu-id="761da-174">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="761da-175">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="761da-175">Run the app and register a new user</span></span>
* <span data-ttu-id="761da-176">Controllare la posta elettronica per il collegamento di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="761da-176">Check your email for the account confirmation link.</span></span> <span data-ttu-id="761da-177">Visualizzare [eseguire il Debug tramite posta elettronica](#debug) se non si riceve il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-177">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="761da-178">Fare clic sul collegamento per confermare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-178">Click the link to confirm your email.</span></span>
* <span data-ttu-id="761da-179">Accedere con l'indirizzo di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="761da-179">Sign in with your email and password.</span></span>
* <span data-ttu-id="761da-180">Disconnettersi.</span><span class="sxs-lookup"><span data-stu-id="761da-180">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="761da-181">Visualizzare la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="761da-181">View the manage page</span></span>

<span data-ttu-id="761da-182">Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="761da-182">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="761da-183">La pagina di gestione viene visualizzata con il **profilo** selezionato della scheda.</span><span class="sxs-lookup"><span data-stu-id="761da-183">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="761da-184">Il **messaggio di posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.</span><span class="sxs-lookup"><span data-stu-id="761da-184">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="761da-185">Reimpostazione della password di test</span><span class="sxs-lookup"><span data-stu-id="761da-185">Test password reset</span></span>

* <span data-ttu-id="761da-186">Se è connessi, selezionare **Logout**.</span><span class="sxs-lookup"><span data-stu-id="761da-186">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="761da-187">Selezionare il **accedere** collegamento e selezionare il **password dimenticata?** collegamento.</span><span class="sxs-lookup"><span data-stu-id="761da-187">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="761da-188">Immettere l'indirizzo di posta elettronica usato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="761da-188">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="761da-189">Viene inviato un messaggio di posta elettronica con un collegamento di reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="761da-189">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="761da-190">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="761da-190">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="761da-191">Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="761da-191">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="761da-192">Modifica timeout messaggio di posta elettronica e l'attività</span><span class="sxs-lookup"><span data-stu-id="761da-192">Change email and activity timeout</span></span>

<span data-ttu-id="761da-193">Il timeout di inattività predefinito è 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="761da-193">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="761da-194">Il codice seguente imposta il timeout di inattività da 5 giorni:</span><span class="sxs-lookup"><span data-stu-id="761da-194">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="761da-195">Modificare tutti i dati protection token durata di validità superiore</span><span class="sxs-lookup"><span data-stu-id="761da-195">Change all data protection token lifespans</span></span>

<span data-ttu-id="761da-196">Il codice seguente modifica tutti periodo di timeout i token di protezione dati a 3 ore:</span><span class="sxs-lookup"><span data-stu-id="761da-196">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="761da-197">Incorporato in token dell'identità utente (vedere [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) hanno un [timeout di un giorno](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="761da-197">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="761da-198">Modificare la durata di token di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="761da-198">Change the email token lifespan</span></span>

<span data-ttu-id="761da-199">Il ciclo di vita di token predefinito dei [i token utente di identità](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) viene [giorno](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="761da-199">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="761da-200">In questa sezione viene illustrato come modificare la durata di token di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="761da-200">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="761da-201">Aggiungere una classe personalizzata [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) e <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="761da-201">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="761da-202">Aggiungere il provider personalizzato al contenitore del servizio:</span><span class="sxs-lookup"><span data-stu-id="761da-202">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="761da-203">Inviare di nuovo conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="761da-203">Resend email confirmation</span></span>

<span data-ttu-id="761da-204">Visualizzare [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="761da-204">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>
### <a name="debug-email"></a><span data-ttu-id="761da-205">Eseguire il debug tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="761da-205">Debug email</span></span>

<span data-ttu-id="761da-206">Se non è possibile ottenere l'indirizzo di posta elettronica funzionante:</span><span class="sxs-lookup"><span data-stu-id="761da-206">If you can't get email working:</span></span>

* <span data-ttu-id="761da-207">Impostare un punto di interruzione `EmailSender.Execute` per verificare `SendGridClient.SendEmailAsync` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="761da-207">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="761da-208">Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) usando codice simile a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="761da-208">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="761da-209">Rivedere le [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.</span><span class="sxs-lookup"><span data-stu-id="761da-209">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="761da-210">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="761da-210">Check your spam folder.</span></span>
* <span data-ttu-id="761da-211">Provare un altro alias di posta elettronica in un provider di posta elettronica diverso (Microsoft, Yahoo, Gmail e così via)</span><span class="sxs-lookup"><span data-stu-id="761da-211">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="761da-212">Provare a inviare agli account di posta elettronica diverso.</span><span class="sxs-lookup"><span data-stu-id="761da-212">Try sending to different email accounts.</span></span>

<span data-ttu-id="761da-213">**Una procedura consigliata** consiste nel **non** usare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="761da-213">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="761da-214">Se si pubblica l'app in Azure, è possibile impostare i segreti di SendGrid come impostazioni dell'applicazione nel portale di App Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="761da-214">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="761da-215">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="761da-215">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="761da-216">Combinare gli account di accesso social e locali</span><span class="sxs-lookup"><span data-stu-id="761da-216">Combine social and local login accounts</span></span>

<span data-ttu-id="761da-217">Per completare questa sezione, è innanzitutto necessario abilitare un provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="761da-217">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="761da-218">Visualizzare [Facebook, Google e l'autenticazione esterna provider](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="761da-218">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="761da-219">È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="761da-219">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="761da-220">Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso basati su social network, prima di tutto, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="761da-220">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="761da-222">Fare clic sui **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="761da-222">Click on the **Manage** link.</span></span> <span data-ttu-id="761da-223">Si noti 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="761da-223">Note the 0 external (social logins) associated with this account.</span></span>

![Gestione visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="761da-225">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="761da-225">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="761da-226">Nell'immagine seguente, Facebook è il provider di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="761da-226">In the following image, Facebook is the external authentication provider:</span></span>

![Gestire la visualizzazione di account di accesso esterni listato Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="761da-228">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="761da-228">The two accounts have been combined.</span></span> <span data-ttu-id="761da-229">Si è in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="761da-229">You are able to sign in with either account.</span></span> <span data-ttu-id="761da-230">È possibile che gli utenti per aggiungere gli account locali nel caso in cui il servizio di autenticazione account di accesso basati su social network è inattivo oppure più probabile che abbia perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="761da-230">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="761da-231">Abilitare la conferma dell'account dopo un sito ha utenti</span><span class="sxs-lookup"><span data-stu-id="761da-231">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="761da-232">Abilitare la conferma dell'account in un sito con gli utenti per bloccare tutti gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="761da-232">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="761da-233">Gli utenti esistenti sono bloccati perché i propri account non confermato.</span><span class="sxs-lookup"><span data-stu-id="761da-233">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="761da-234">Per evitare il blocco degli utente esistente, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="761da-234">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="761da-235">Aggiornare il database per contrassegnare tutti gli utenti esistenti come viene confermata.</span><span class="sxs-lookup"><span data-stu-id="761da-235">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="761da-236">Verificare che gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="761da-236">Confirm existing users.</span></span> <span data-ttu-id="761da-237">Ad esempio, batch-trasmissione messaggi di posta elettronica con collegamenti di conferma.</span><span class="sxs-lookup"><span data-stu-id="761da-237">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
