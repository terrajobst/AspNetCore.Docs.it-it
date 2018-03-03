---
title: La conferma dell'account e Password di ripristino in ASP.NET Core
author: rick-anderson
description: Informazioni su come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b236b4e5d3a4fa7212453f2aec209d145f5f5e32
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="3ef83-103">La conferma dell'account e il recupero della password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ef83-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="3ef83-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="3ef83-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="3ef83-105">In questa esercitazione viene illustrato come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="3ef83-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="3ef83-106">Questa esercitazione è **non** un argomento di inizio.</span><span class="sxs-lookup"><span data-stu-id="3ef83-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="3ef83-107">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="3ef83-107">You should be familiar with:</span></span>

* [<span data-ttu-id="3ef83-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ef83-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="3ef83-109">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="3ef83-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="3ef83-110">Conferma account e recupero password</span><span class="sxs-lookup"><span data-stu-id="3ef83-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="3ef83-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3ef83-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="3ef83-112">Vedere [questo file PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) per le versioni 1.1 di MVC ASP.NET Core e 2. x.</span><span class="sxs-lookup"><span data-stu-id="3ef83-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ef83-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3ef83-113">Prerequisites</span></span>

<span data-ttu-id="3ef83-114">[.NET core 2.1.4 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="3ef83-114">[.NET Core 2.1.4 SDK](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="3ef83-115">Creare un nuovo progetto ASP.NET Core con l'interfaccia CLI di .NET Core</span><span class="sxs-lookup"><span data-stu-id="3ef83-115">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ef83-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* <span data-ttu-id="3ef83-117">`--auth Individual` Specifica il modello di progetto di singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="3ef83-117">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="3ef83-118">In Windows, aggiungere il `-uld` opzione.</span><span class="sxs-lookup"><span data-stu-id="3ef83-118">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="3ef83-119">Specifica di che utilizzare LocalDB anziché SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ef83-119">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="3ef83-120">Eseguire `new mvc --help` per ottenere assistenza su questo comando.</span><span class="sxs-lookup"><span data-stu-id="3ef83-120">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ef83-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ef83-122">Se si utilizza l'interfaccia CLI o SQLite, eseguire le operazioni seguenti in una finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="3ef83-122">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="3ef83-123">`--auth Individual` Specifica il modello di progetto di singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="3ef83-123">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="3ef83-124">In Windows, aggiungere il `-uld` opzione.</span><span class="sxs-lookup"><span data-stu-id="3ef83-124">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="3ef83-125">Specifica di che utilizzare LocalDB anziché SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ef83-125">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="3ef83-126">Eseguire `new mvc --help` per ottenere assistenza su questo comando.</span><span class="sxs-lookup"><span data-stu-id="3ef83-126">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="3ef83-127">In alternativa, è possibile creare un nuovo progetto ASP.NET Core con Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="3ef83-127">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="3ef83-128">In Visual Studio, creare un nuovo **applicazione Web** progetto.</span><span class="sxs-lookup"><span data-stu-id="3ef83-128">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="3ef83-129">Selezionare **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="3ef83-129">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="3ef83-130">**.NET core** sia selezionato nell'immagine seguente, ma è possibile selezionare **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="3ef83-130">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="3ef83-131">Selezionare **Modifica autenticazione** e impostato su **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="3ef83-131">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="3ef83-132">Mantenere il valore predefinito **in-app dell'account utente di archivio**.</span><span class="sxs-lookup"><span data-stu-id="3ef83-132">Keep the default **Store user accounts in-app**.</span></span>

![Finestra Nuovo progetto con "Singoli account utente opzione"](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="3ef83-134">Verifica registrazione nuovo utente</span><span class="sxs-lookup"><span data-stu-id="3ef83-134">Test new user registration</span></span>

<span data-ttu-id="3ef83-135">Eseguire l'app, selezionare il **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="3ef83-135">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="3ef83-136">Seguire le istruzioni per eseguire le migrazioni di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3ef83-136">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="3ef83-137">A questo punto, la convalida sola nel messaggio di posta elettronica è con il [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo.</span><span class="sxs-lookup"><span data-stu-id="3ef83-137">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="3ef83-138">Dopo aver inviato la registrazione, si è connessi all'app.</span><span class="sxs-lookup"><span data-stu-id="3ef83-138">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="3ef83-139">Più avanti nell'esercitazione, il codice viene aggiornato in modo non possono accedere nuovi utenti fino a quando non è stata convalidata alla posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-139">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="3ef83-140">Visualizzare il database di identità</span><span class="sxs-lookup"><span data-stu-id="3ef83-140">View the Identity database</span></span>

<span data-ttu-id="3ef83-141">Vedere [utilizzo di SQLite in un progetto ASP.NET MVC Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) per istruzioni su come visualizzare il database di SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ef83-141">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="3ef83-142">Per Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="3ef83-142">For Visual Studio:</span></span>

* <span data-ttu-id="3ef83-143">Dal **vista** dal menu **Esplora oggetti di SQL Server** (sillaba SSOX).</span><span class="sxs-lookup"><span data-stu-id="3ef83-143">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="3ef83-144">Passare a **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="3ef83-144">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="3ef83-145">Fare clic su **dbo. AspNetUsers** > **visualizzare dati**:</span><span class="sxs-lookup"><span data-stu-id="3ef83-145">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu di scelta rapida per la tabella AspNetUsers in Esplora oggetti di SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="3ef83-147">Si noti la tabella `EmailConfirmed` campo `False`.</span><span class="sxs-lookup"><span data-stu-id="3ef83-147">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="3ef83-148">Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="3ef83-148">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="3ef83-149">Fare doppio clic sulla riga e selezionare **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="3ef83-149">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="3ef83-150">Eliminare l'alias di posta elettronica rende più semplice nei passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="3ef83-150">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="3ef83-151">Utilizzo di HTTPS</span><span class="sxs-lookup"><span data-stu-id="3ef83-151">Require HTTPS</span></span>

<span data-ttu-id="3ef83-152">Vedere [richiedono HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="3ef83-152">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="3ef83-153">Richiedi conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="3ef83-153">Require email confirmation</span></span>

<span data-ttu-id="3ef83-154">È consigliabile verificare il messaggio di posta elettronica di una nuova registrazione utente.</span><span class="sxs-lookup"><span data-stu-id="3ef83-154">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="3ef83-155">Posta elettronica di conferma è utile per verificare che non viene rappresentata un'altra persona (vale a dire, è ancora stato registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="3ef83-155">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="3ef83-156">Si supponga che si dispone di un forum di discussione e si desidera impedire "yli@example.com"tramite la registrazione come"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="3ef83-156">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="3ef83-157">Senza conferma tramite posta elettronica, "nolivetto@contoso.com" Impossibile ricevere il messaggio di posta elettronica indesiderato dall'app.</span><span class="sxs-lookup"><span data-stu-id="3ef83-157">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="3ef83-158">Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non veniva rilevato l'errore di ortografia di "yli".</span><span class="sxs-lookup"><span data-stu-id="3ef83-158">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="3ef83-159">Essi non potranno utilizzare il recupero della password in quanto l'app non dispone di posta elettronica corretta.</span><span class="sxs-lookup"><span data-stu-id="3ef83-159">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="3ef83-160">Conferma tramite posta elettronica fornisce solo protezione limitata da BOT.</span><span class="sxs-lookup"><span data-stu-id="3ef83-160">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="3ef83-161">Conferma tramite posta elettronica non offre protezione da utenti malintenzionati con numero di account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-161">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="3ef83-162">In genere si desidera impedire agli utenti di nuovo di tutti i dati al sito web di registrazione prima che sia una conferma tramite posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-162">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="3ef83-163">Aggiornamento `ConfigureServices` per richiedere una conferma tramite posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="3ef83-163">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="3ef83-164">`config.SignIn.RequireConfirmedEmail = true;` impedisce l'accesso fino a quando non viene confermata la posta elettronica agli utenti registrati.</span><span class="sxs-lookup"><span data-stu-id="3ef83-164">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="3ef83-165">Configurare i provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="3ef83-165">Configure email provider</span></span>

<span data-ttu-id="3ef83-166">In questa esercitazione, SendGrid viene utilizzato per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-166">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="3ef83-167">È necessario un account SendGrid e una chiave per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-167">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="3ef83-168">È possibile utilizzare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-168">You can use other email providers.</span></span> <span data-ttu-id="3ef83-169">ASP.NET Core 2. x include `System.Net.Mail`, che consente di inviare posta elettronica dalla tua app.</span><span class="sxs-lookup"><span data-stu-id="3ef83-169">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="3ef83-170">È consigliabile che utilizzare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-170">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="3ef83-171">SMTP è difficile da proteggere e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="3ef83-171">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="3ef83-172">Il [modello opzioni](xref:fundamentals/configuration/options) viene utilizzato per accedere alle impostazioni di account e la chiave utente.</span><span class="sxs-lookup"><span data-stu-id="3ef83-172">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="3ef83-173">Per ulteriori informazioni, vedere [configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3ef83-173">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="3ef83-174">Creare una classe per recuperare la chiave di proteggere la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="3ef83-175">Per questo esempio, il `AuthMessageSenderOptions` classe il *Services/AuthMessageSenderOptions.cs* file:</span><span class="sxs-lookup"><span data-stu-id="3ef83-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="3ef83-176">Impostare il `SendGridUser` e `SendGridKey` con il [strumento segreto manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3ef83-176">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="3ef83-177">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3ef83-177">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="3ef83-178">In Windows, gestione Secret archivia coppie di chiavi/valore in un *secrets.json* file nel `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="3ef83-178">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="3ef83-179">Il contenuto del *secrets.json* file non vengono crittografati.</span><span class="sxs-lookup"><span data-stu-id="3ef83-179">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="3ef83-180">Il *secrets.json* file è illustrato di seguito (il `SendGridKey` valore è stato rimosso.)</span><span class="sxs-lookup"><span data-stu-id="3ef83-180">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="3ef83-181">Configurare l'avvio per l'utilizzo di AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="3ef83-181">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="3ef83-182">Aggiungere `AuthMessageSenderOptions` al contenitore del servizio alla fine del `ConfigureServices` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="3ef83-182">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ef83-183">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-183">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ef83-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="3ef83-185">Configurare la classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="3ef83-185">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="3ef83-186">In questa esercitazione viene illustrato come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="3ef83-186">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="3ef83-187">Installare il `SendGrid` pacchetto NuGet:</span><span class="sxs-lookup"><span data-stu-id="3ef83-187">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="3ef83-188">Dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="3ef83-188">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="3ef83-189">Dalla Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="3ef83-189">From the Package Manager Console, enter the following command:</span></span>

 `Install-Package SendGrid`

<span data-ttu-id="3ef83-190">Vedere [iniziare gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account di SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="3ef83-190">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="3ef83-191">Configurare SendGrid</span><span class="sxs-lookup"><span data-stu-id="3ef83-191">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ef83-192">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-192">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3ef83-193">Per configurare SendGrid, aggiungere codice analogo al seguente nella *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="3ef83-193">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ef83-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="3ef83-195">Aggiungere il codice nel *Services/MessageServices.cs* simile al seguente per configurare SendGrid:</span><span class="sxs-lookup"><span data-stu-id="3ef83-195">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="3ef83-196">Abilitare il ripristino di conferma e la password di account</span><span class="sxs-lookup"><span data-stu-id="3ef83-196">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="3ef83-197">Il modello presenta il codice per il ripristino di conferma e la password di account.</span><span class="sxs-lookup"><span data-stu-id="3ef83-197">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="3ef83-198">Trovare il `OnPostAsync` metodo *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="3ef83-198">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ef83-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3ef83-200">Impedire che i nuovi utenti registrati automaticamente l'accesso da impostare come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="3ef83-200">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3ef83-201">Il metodo completo viene visualizzato con la riga modificata evidenziata:</span><span class="sxs-lookup"><span data-stu-id="3ef83-201">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ef83-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ef83-203">Per abilitare la conferma dell'account, rimuovere il commento nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3ef83-203">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="3ef83-204">**Nota:** il codice è impedire che un utente appena registrato viene connesso automaticamente da impostare come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="3ef83-204">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="3ef83-205">Abilita ripristino password rimuovendo il codice di `ForgotPassword` azione di *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="3ef83-205">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="3ef83-206">Rimuovere il commento modulo *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3ef83-206">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="3ef83-207">È possibile rimuovere il `<p> For more information on how to enable reset password ... </p>` elemento che contiene un collegamento a questo articolo.</span><span class="sxs-lookup"><span data-stu-id="3ef83-207">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="3ef83-208">Registrare, posta elettronica di conferma e reimpostare la password</span><span class="sxs-lookup"><span data-stu-id="3ef83-208">Register, confirm email, and reset password</span></span>

<span data-ttu-id="3ef83-209">Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.</span><span class="sxs-lookup"><span data-stu-id="3ef83-209">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="3ef83-210">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="3ef83-210">Run the app and register a new user</span></span>

 ![Visualizzazione di registrare Account dell'applicazione Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="3ef83-212">Controllare la posta elettronica per il collegamento di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="3ef83-212">Check your email for the account confirmation link.</span></span> <span data-ttu-id="3ef83-213">Vedere [Debug posta elettronica](#debug) se non si ottiene il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-213">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="3ef83-214">Fare clic sul collegamento per confermare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="3ef83-214">Click the link to confirm your email.</span></span>
* <span data-ttu-id="3ef83-215">Accedere con il messaggio di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="3ef83-215">Log in with your email and password.</span></span>
* <span data-ttu-id="3ef83-216">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="3ef83-216">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="3ef83-217">Visualizzare la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="3ef83-217">View the manage page</span></span>

<span data-ttu-id="3ef83-218">Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="3ef83-218">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="3ef83-219">Potrebbe essere necessario espandere la barra di spostamento per visualizzare il nome utente.</span><span class="sxs-lookup"><span data-stu-id="3ef83-219">You might need to expand the navbar to see user name.</span></span>

![barra di spostamento](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ef83-221">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-221">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3ef83-222">Verrà visualizzata la pagina di gestione con il **profilo** scheda selezionata.</span><span class="sxs-lookup"><span data-stu-id="3ef83-222">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="3ef83-223">Il **posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.</span><span class="sxs-lookup"><span data-stu-id="3ef83-223">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![pagina Gestisci](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ef83-225">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ef83-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ef83-226">Questo è riportato più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="3ef83-226">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="3ef83-227">![pagina Gestisci](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="3ef83-227">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="3ef83-228">Reimpostazione della password di test</span><span class="sxs-lookup"><span data-stu-id="3ef83-228">Test password reset</span></span>

* <span data-ttu-id="3ef83-229">Se si è connessi, selezionare **Logout**.</span><span class="sxs-lookup"><span data-stu-id="3ef83-229">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="3ef83-230">Selezionare il **Accedi** collegamento e selezionare il **password dimenticata?** collegamento.</span><span class="sxs-lookup"><span data-stu-id="3ef83-230">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="3ef83-231">Immettere il messaggio di posta elettronica che è utilizzato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="3ef83-231">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="3ef83-232">Viene inviato un messaggio di posta elettronica con un collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="3ef83-232">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="3ef83-233">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="3ef83-233">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="3ef83-234">Dopo che la password è stata reimpostata correttamente, è possibile accedere con il messaggio di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="3ef83-234">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="3ef83-235">Eseguire il debug di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="3ef83-235">Debug email</span></span>

<span data-ttu-id="3ef83-236">Se non è possibile ottenere l'utilizzo di posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="3ef83-236">If you can't get email working:</span></span>

* <span data-ttu-id="3ef83-237">Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="3ef83-237">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="3ef83-238">Esaminare il [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.</span><span class="sxs-lookup"><span data-stu-id="3ef83-238">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="3ef83-239">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="3ef83-239">Check your spam folder.</span></span>
* <span data-ttu-id="3ef83-240">Provare un altro alias di posta elettronica su un provider diverso posta elettronica (Microsoft, Yahoo, Gmail, ecc.)</span><span class="sxs-lookup"><span data-stu-id="3ef83-240">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="3ef83-241">Provare a inviare agli account di posta elettronica diverso.</span><span class="sxs-lookup"><span data-stu-id="3ef83-241">Try sending to different email accounts.</span></span>

<span data-ttu-id="3ef83-242">**Una procedura consigliata** è **non** Usa i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="3ef83-242">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="3ef83-243">Se si pubblica l'app in Azure, è possibile impostare i segreti SendGrid come impostazioni dell'applicazione nel portale di Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="3ef83-243">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="3ef83-244">Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="3ef83-244">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="3ef83-245">Combinare gli account di accesso social networking e locale</span><span class="sxs-lookup"><span data-stu-id="3ef83-245">Combine social and local login accounts</span></span>

<span data-ttu-id="3ef83-246">Per completare questa sezione, è innanzitutto necessario abilitare i provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="3ef83-246">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="3ef83-247">Vedere [Facebook, Google, autenticazione basata su provider esterni e](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3ef83-247">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="3ef83-248">È possibile combinare gli account locali e sociali facendo clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="3ef83-248">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="3ef83-249">Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso social prima, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="3ef83-249">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="3ef83-251">Fare clic su di **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="3ef83-251">Click on the **Manage** link.</span></span> <span data-ttu-id="3ef83-252">Si noti il valore 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="3ef83-252">Note the 0 external (social logins) associated with this account.</span></span>

![Gestione visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="3ef83-254">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="3ef83-254">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="3ef83-255">Nella figura seguente, Facebook è il provider di autenticazione esterni:</span><span class="sxs-lookup"><span data-stu-id="3ef83-255">In the following image, Facebook is the external authentication provider:</span></span>

![Gestire l'account di accesso esterni di visualizzazione elenco Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="3ef83-257">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="3ef83-257">The two accounts have been combined.</span></span> <span data-ttu-id="3ef83-258">Si è in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="3ef83-258">You are able to log on with either account.</span></span> <span data-ttu-id="3ef83-259">Si consiglia agli utenti di aggiungere gli account locali nel caso in cui il servizio di autenticazione account di accesso social è inattivo o più probabile hai più accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="3ef83-259">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="3ef83-260">Abilitare la conferma dell'account dopo che un sito con gli utenti</span><span class="sxs-lookup"><span data-stu-id="3ef83-260">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="3ef83-261">Consentire la conferma dell'account in un sito con utenti Blocca tutti gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="3ef83-261">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="3ef83-262">Gli utenti esistenti vengono bloccati perché gli account non sono confermati.</span><span class="sxs-lookup"><span data-stu-id="3ef83-262">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="3ef83-263">Per risolvere l'uscita di blocco dell'utente, utilizzare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="3ef83-263">To work around exiting user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="3ef83-264">Aggiornare il database per contrassegnare tutti gli utenti esistenti come viene confermata</span><span class="sxs-lookup"><span data-stu-id="3ef83-264">Update the database to mark all existing users as being confirmed</span></span>
* <span data-ttu-id="3ef83-265">Verificare che gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="3ef83-265">Confirm exiting users.</span></span> <span data-ttu-id="3ef83-266">Ad esempio, batch-trasmissione messaggi di posta elettronica con i collegamenti di conferma.</span><span class="sxs-lookup"><span data-stu-id="3ef83-266">For example, batch-send emails with confirmation links.</span></span>
