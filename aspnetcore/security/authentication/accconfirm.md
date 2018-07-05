---
title: Conferma account e recupero della password in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803272"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="c18c0-103">Conferma account e recupero della password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c18c0-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="c18c0-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c18c0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="c18c0-105">Questa esercitazione illustra come compilare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="c18c0-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="c18c0-106">Questa esercitazione viene **non** un argomento di inizio.</span><span class="sxs-lookup"><span data-stu-id="c18c0-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="c18c0-107">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="c18c0-107">You should be familiar with:</span></span>

* [<span data-ttu-id="c18c0-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c18c0-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c18c0-109">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="c18c0-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="c18c0-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c18c0-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="c18c0-111">Visualizzare [questo file PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) per le versioni 2.x e ASP.NET Core MVC 1.1.</span><span class="sxs-lookup"><span data-stu-id="c18c0-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c18c0-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c18c0-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="c18c0-113">Creare un nuovo progetto ASP.NET Core con .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c18c0-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c18c0-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="c18c0-115">`--auth Individual` Specifica il modello di progetto di account utente individuali.</span><span class="sxs-lookup"><span data-stu-id="c18c0-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="c18c0-116">In Windows, aggiungere il `-uld` opzione.</span><span class="sxs-lookup"><span data-stu-id="c18c0-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="c18c0-117">Specifica che deve essere usato Local DB invece di SQLite.</span><span class="sxs-lookup"><span data-stu-id="c18c0-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="c18c0-118">Eseguire `new mvc --help` per ottenere assistenza su questo comando.</span><span class="sxs-lookup"><span data-stu-id="c18c0-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c18c0-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c18c0-120">Se si usa il comando o SQLite, eseguire il comando seguente in una finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="c18c0-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="c18c0-121">`--auth Individual` Specifica il modello di progetto di account utente individuali.</span><span class="sxs-lookup"><span data-stu-id="c18c0-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="c18c0-122">In Windows, aggiungere il `-uld` opzione.</span><span class="sxs-lookup"><span data-stu-id="c18c0-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="c18c0-123">Specifica che deve essere usato Local DB invece di SQLite.</span><span class="sxs-lookup"><span data-stu-id="c18c0-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="c18c0-124">Eseguire `new mvc --help` per ottenere assistenza su questo comando.</span><span class="sxs-lookup"><span data-stu-id="c18c0-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="c18c0-125">In alternativa, è possibile creare un nuovo progetto ASP.NET Core con Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c18c0-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="c18c0-126">In Visual Studio, creare una nuova **applicazione Web** progetto.</span><span class="sxs-lookup"><span data-stu-id="c18c0-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="c18c0-127">Selezionare **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="c18c0-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="c18c0-128">**.NET core** sia selezionata nell'immagine seguente, ma è possibile selezionare **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="c18c0-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="c18c0-129">Selezionare **Modifica autenticazione** e impostare **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="c18c0-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="c18c0-130">Mantenere il valore predefinito **nell'app dell'account utente di Store**.</span><span class="sxs-lookup"><span data-stu-id="c18c0-130">Keep the default **Store user accounts in-app**.</span></span>

![Finestra Nuovo progetto, che mostra "Radio di account utente individuali" selezionata](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="c18c0-132">Registrazione di nuovi utenti di test</span><span class="sxs-lookup"><span data-stu-id="c18c0-132">Test new user registration</span></span>

<span data-ttu-id="c18c0-133">Eseguire l'app, selezionare la **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="c18c0-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="c18c0-134">Seguire le istruzioni per eseguire le migrazioni di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c18c0-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="c18c0-135">A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo.</span><span class="sxs-lookup"><span data-stu-id="c18c0-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="c18c0-136">Dopo aver inviato la registrazione, si è connessi all'app.</span><span class="sxs-lookup"><span data-stu-id="c18c0-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="c18c0-137">Più avanti nell'esercitazione, il codice venga aggiornato in modo che i nuovi utenti non è possibile accedere fino alla posta elettronica è stata convalidata.</span><span class="sxs-lookup"><span data-stu-id="c18c0-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="c18c0-138">Visualizzare il database di identità</span><span class="sxs-lookup"><span data-stu-id="c18c0-138">View the Identity database</span></span>

<span data-ttu-id="c18c0-139">Visualizzare [utilizzare SQLite in un progetto ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) per istruzioni su come visualizzare i database SQLite.</span><span class="sxs-lookup"><span data-stu-id="c18c0-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="c18c0-140">Per Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c18c0-140">For Visual Studio:</span></span>

* <span data-ttu-id="c18c0-141">Dal **View** dal menu **Esplora oggetti di SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="c18c0-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="c18c0-142">Passare a **(localdb) MSSQLLocalDB (Server SQL 13)**.</span><span class="sxs-lookup"><span data-stu-id="c18c0-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="c18c0-143">Fare clic su **dbo. AspNetUsers** > **consente di visualizzare dati**:</span><span class="sxs-lookup"><span data-stu-id="c18c0-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu di scelta rapida nella tabella AspNetUsers in Esplora oggetti di SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="c18c0-145">Si noti la tabella `EmailConfirmed` campo è `False`.</span><span class="sxs-lookup"><span data-stu-id="c18c0-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="c18c0-146">Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="c18c0-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="c18c0-147">Fare doppio clic sulla riga e selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="c18c0-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="c18c0-148">L'eliminazione di alias di posta elettronica rende più semplice nella procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="c18c0-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="c18c0-149">La richiesta di HTTPS</span><span class="sxs-lookup"><span data-stu-id="c18c0-149">Require HTTPS</span></span>

<span data-ttu-id="c18c0-150">Visualizzare [Richiedi HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="c18c0-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="c18c0-151">Richiedere conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="c18c0-151">Require email confirmation</span></span>

<span data-ttu-id="c18c0-152">È consigliabile verificare che il messaggio di posta elettronica di una nuova registrazione utente.</span><span class="sxs-lookup"><span data-stu-id="c18c0-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="c18c0-153">Inviare tramite posta elettronica di conferma consente di verificare che non sta rappresentando qualcun altro (vale a dire non hanno registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="c18c0-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="c18c0-154">Si supponga che si ha un forum di discussione e si desidera evitare che "yli@example.com"da registrare come"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="c18c0-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="c18c0-155">Senza conferma tramite posta elettronica, "nolivetto@contoso.com" può ricevere e-mail indesiderate dalla propria app.</span><span class="sxs-lookup"><span data-stu-id="c18c0-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="c18c0-156">Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non l'aveste notato l'errore di ortografia di "yli".</span><span class="sxs-lookup"><span data-stu-id="c18c0-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="c18c0-157">Essi non sarebbe in grado di utilizzare il recupero della password perché l'app non dispone di posta elettronica corretta.</span><span class="sxs-lookup"><span data-stu-id="c18c0-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="c18c0-158">Conferma tramite posta elettronica fornisce solo una protezione limitata dal BOT.</span><span class="sxs-lookup"><span data-stu-id="c18c0-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="c18c0-159">Conferma tramite posta elettronica non fornisce protezione da utenti malintenzionati con numero di account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c18c0-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="c18c0-160">In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che abbiano un messaggio di posta elettronica confermato.</span><span class="sxs-lookup"><span data-stu-id="c18c0-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="c18c0-161">Aggiornamento `ConfigureServices` per richiedere un indirizzo di posta elettronica confermato:</span><span class="sxs-lookup"><span data-stu-id="c18c0-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="c18c0-162">`config.SignIn.RequireConfirmedEmail = true;` impedisce l'accesso fino a quando non viene confermata la posta elettronica agli utenti registrati.</span><span class="sxs-lookup"><span data-stu-id="c18c0-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="c18c0-163">Configurare il provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="c18c0-163">Configure email provider</span></span>

<span data-ttu-id="c18c0-164">In questa esercitazione, SendGrid consente di inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c18c0-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="c18c0-165">È necessario un account SendGrid e una chiave per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c18c0-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="c18c0-166">È possibile usare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c18c0-166">You can use other email providers.</span></span> <span data-ttu-id="c18c0-167">ASP.NET Core 2.x include `System.Net.Mail`, pertanto è possibile inviare tramite posta elettronica dall'app.</span><span class="sxs-lookup"><span data-stu-id="c18c0-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="c18c0-168">È consigliabile che usare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c18c0-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="c18c0-169">SMTP sono difficili da proteggere e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="c18c0-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="c18c0-170">Il [modello di opzioni](xref:fundamentals/configuration/options) viene utilizzato per accedere alle impostazioni account e la chiave dell'utente.</span><span class="sxs-lookup"><span data-stu-id="c18c0-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="c18c0-171">Per altre informazioni, vedere [configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c18c0-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="c18c0-172">Creare una classe per recuperare la chiave di protezione della posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c18c0-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="c18c0-173">Per questo esempio, il `AuthMessageSenderOptions` classe viene creata nel *Services/AuthMessageSenderOptions.cs* file:</span><span class="sxs-lookup"><span data-stu-id="c18c0-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="c18c0-174">Impostare il `SendGridUser` e `SendGridKey` con il [strumento secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="c18c0-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="c18c0-175">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c18c0-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="c18c0-176">In Windows, Secret Manager archivia le coppie di chiavi/valore in una *Secrets* del file nei `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="c18c0-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="c18c0-177">Il contenuto del *Secrets* file non vengono crittografati.</span><span class="sxs-lookup"><span data-stu-id="c18c0-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="c18c0-178">Il *Secrets* file è illustrato di seguito (il `SendGridKey` valore è stato rimosso.)</span><span class="sxs-lookup"><span data-stu-id="c18c0-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="c18c0-179">Configurare l'avvio per usare AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="c18c0-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="c18c0-180">Aggiungere `AuthMessageSenderOptions` al contenitore del servizio alla fine del `ConfigureServices` metodo nella *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="c18c0-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c18c0-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c18c0-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="c18c0-183">Configurare la classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="c18c0-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="c18c0-184">Questa esercitazione illustra come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="c18c0-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="c18c0-185">Installare il `SendGrid` pacchetto NuGet:</span><span class="sxs-lookup"><span data-stu-id="c18c0-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="c18c0-186">Dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="c18c0-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="c18c0-187">Dalla Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c18c0-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="c18c0-188">Visualizzare [inizia gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="c18c0-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="c18c0-189">Configurare SendGrid</span><span class="sxs-lookup"><span data-stu-id="c18c0-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c18c0-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="c18c0-191">Per configurare SendGrid, aggiungere codice simile al seguente nella *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="c18c0-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c18c0-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="c18c0-193">Aggiungere il codice nel *Services/MessageServices.cs* simile al seguente per configurare SendGrid:</span><span class="sxs-lookup"><span data-stu-id="c18c0-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="c18c0-194">Abilitare il ripristino di conferma e la password account</span><span class="sxs-lookup"><span data-stu-id="c18c0-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="c18c0-195">Il modello presenta il codice per il ripristino di conferma e la password di account.</span><span class="sxs-lookup"><span data-stu-id="c18c0-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="c18c0-196">Trovare il `OnPostAsync` nel metodo *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="c18c0-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c18c0-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="c18c0-198">Impedire che gli utenti appena registrati viene connesso automaticamente impostando come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="c18c0-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c18c0-199">Il metodo completo viene visualizzato con la riga modificata evidenziata:</span><span class="sxs-lookup"><span data-stu-id="c18c0-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c18c0-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="c18c0-201">Per abilitare la conferma dell'account, rimuovere il commento nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c18c0-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="c18c0-202">**Nota:** il codice è impedire che un utente appena registrato viene connesso automaticamente impostando come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="c18c0-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c18c0-203">Abilita ripristino password rimuovendo il codice nel `ForgotPassword` azione di *AccountController*:</span><span class="sxs-lookup"><span data-stu-id="c18c0-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="c18c0-204">Rimuovere il commento form *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c18c0-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="c18c0-205">Si potrebbe voler rimuovere il `<p> For more information on how to enable reset password ... </p>` elemento che contiene un collegamento a questo articolo.</span><span class="sxs-lookup"><span data-stu-id="c18c0-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="c18c0-206">Registrare, messaggio di posta elettronica di conferma e reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="c18c0-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="c18c0-207">Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.</span><span class="sxs-lookup"><span data-stu-id="c18c0-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="c18c0-208">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="c18c0-208">Run the app and register a new user</span></span>

  ![Visualizzazione Account registrazione dell'applicazione Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="c18c0-210">Controllare la posta elettronica per il collegamento di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="c18c0-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="c18c0-211">Visualizzare [eseguire il Debug tramite posta elettronica](#debug) se non si riceve il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c18c0-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="c18c0-212">Fare clic sul collegamento per confermare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c18c0-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="c18c0-213">Accedere con l'indirizzo di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="c18c0-213">Log in with your email and password.</span></span>
* <span data-ttu-id="c18c0-214">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="c18c0-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="c18c0-215">Visualizzare la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="c18c0-215">View the manage page</span></span>

<span data-ttu-id="c18c0-216">Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="c18c0-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="c18c0-217">Potrebbe essere necessario espandere la barra di spostamento per visualizzare il nome utente.</span><span class="sxs-lookup"><span data-stu-id="c18c0-217">You might need to expand the navbar to see user name.</span></span>

![Nella barra di spostamento](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c18c0-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c18c0-220">La pagina di gestione viene visualizzata con il **profilo** selezionato della scheda.</span><span class="sxs-lookup"><span data-stu-id="c18c0-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="c18c0-221">Il **messaggio di posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.</span><span class="sxs-lookup"><span data-stu-id="c18c0-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![pagina di gestione](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c18c0-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c18c0-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c18c0-224">Ciò viene indicato più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c18c0-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="c18c0-225">![pagina Gestisci](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="c18c0-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="c18c0-226">Reimpostazione della password di test</span><span class="sxs-lookup"><span data-stu-id="c18c0-226">Test password reset</span></span>

* <span data-ttu-id="c18c0-227">Se si è connessi, selezionare **Logout**.</span><span class="sxs-lookup"><span data-stu-id="c18c0-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="c18c0-228">Selezionare il **accedere** collegamento e selezionare il **password dimenticata?** collegamento.</span><span class="sxs-lookup"><span data-stu-id="c18c0-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="c18c0-229">Immettere l'indirizzo di posta elettronica usato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="c18c0-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="c18c0-230">Viene inviato un messaggio di posta elettronica con un collegamento di reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="c18c0-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="c18c0-231">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="c18c0-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="c18c0-232">Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="c18c0-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="c18c0-233">Eseguire il debug tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="c18c0-233">Debug email</span></span>

<span data-ttu-id="c18c0-234">Se non è possibile ottenere l'indirizzo di posta elettronica funzionante:</span><span class="sxs-lookup"><span data-stu-id="c18c0-234">If you can't get email working:</span></span>

* <span data-ttu-id="c18c0-235">Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="c18c0-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="c18c0-236">Rivedere le [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.</span><span class="sxs-lookup"><span data-stu-id="c18c0-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="c18c0-237">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="c18c0-237">Check your spam folder.</span></span>
* <span data-ttu-id="c18c0-238">Provare un altro alias di posta elettronica in un provider di posta elettronica diverso (Microsoft, Yahoo, Gmail e così via)</span><span class="sxs-lookup"><span data-stu-id="c18c0-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="c18c0-239">Provare a inviare agli account di posta elettronica diverso.</span><span class="sxs-lookup"><span data-stu-id="c18c0-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="c18c0-240">**Una procedura consigliata** consiste nel **non** usare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="c18c0-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="c18c0-241">Se si pubblica l'app in Azure, è possibile impostare i segreti di SendGrid come impostazioni dell'applicazione nel portale di App Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="c18c0-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="c18c0-242">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c18c0-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="c18c0-243">Combinare gli account di accesso social e locali</span><span class="sxs-lookup"><span data-stu-id="c18c0-243">Combine social and local login accounts</span></span>

<span data-ttu-id="c18c0-244">Per completare questa sezione, è innanzitutto necessario abilitare un provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="c18c0-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="c18c0-245">Visualizzare [Facebook, Google e l'autenticazione esterna provider](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c18c0-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="c18c0-246">È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="c18c0-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="c18c0-247">Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso basati su social network, prima di tutto, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="c18c0-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="c18c0-249">Fare clic sui **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="c18c0-249">Click on the **Manage** link.</span></span> <span data-ttu-id="c18c0-250">Si noti 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="c18c0-250">Note the 0 external (social logins) associated with this account.</span></span>

![Gestione visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="c18c0-252">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="c18c0-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="c18c0-253">Nell'immagine seguente, Facebook è il provider di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="c18c0-253">In the following image, Facebook is the external authentication provider:</span></span>

![Gestire la visualizzazione di account di accesso esterni listato Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="c18c0-255">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="c18c0-255">The two accounts have been combined.</span></span> <span data-ttu-id="c18c0-256">Si è in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="c18c0-256">You are able to log on with either account.</span></span> <span data-ttu-id="c18c0-257">È possibile che gli utenti per aggiungere gli account locali nel caso in cui il servizio di autenticazione account di accesso basati su social network è inattivo oppure più probabile che abbia perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="c18c0-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="c18c0-258">Abilitare la conferma dell'account dopo un sito ha utenti</span><span class="sxs-lookup"><span data-stu-id="c18c0-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="c18c0-259">Abilitare la conferma dell'account in un sito con gli utenti per bloccare tutti gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="c18c0-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="c18c0-260">Gli utenti esistenti sono bloccati perché i propri account non confermato.</span><span class="sxs-lookup"><span data-stu-id="c18c0-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="c18c0-261">Per evitare il blocco degli utente esistente, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c18c0-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="c18c0-262">Aggiornare il database per contrassegnare tutti gli utenti esistenti come viene confermata.</span><span class="sxs-lookup"><span data-stu-id="c18c0-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="c18c0-263">Verificare che gli utenti in fase di chiusura.</span><span class="sxs-lookup"><span data-stu-id="c18c0-263">Confirm exiting users.</span></span> <span data-ttu-id="c18c0-264">Ad esempio, batch-trasmissione messaggi di posta elettronica con collegamenti di conferma.</span><span class="sxs-lookup"><span data-stu-id="c18c0-264">For example, batch-send emails with confirmation links.</span></span>
