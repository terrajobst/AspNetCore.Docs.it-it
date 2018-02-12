---
title: La conferma dell'account e Password di ripristino in ASP.NET Core
author: rick-anderson
description: Viene illustrato come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 14c7fdfc1ed8b87aac8ca937298c7da6373bf06d
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="90664-103">La conferma dell'account e il recupero della password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90664-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="90664-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="90664-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="90664-105">In questa esercitazione viene illustrato come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="90664-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="90664-106">Creare un nuovo progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90664-106">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90664-107">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90664-107">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="90664-108">Questo passaggio si applica a Visual Studio in Windows.</span><span class="sxs-lookup"><span data-stu-id="90664-108">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="90664-109">Vedere la sezione successiva per le istruzioni di CLI.</span><span class="sxs-lookup"><span data-stu-id="90664-109">See the next section for CLI instructions.</span></span>

<span data-ttu-id="90664-110">L'esercitazione è necessario Visual Studio 2017 Preview 2 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="90664-110">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="90664-111">In Visual Studio, creare un nuovo progetto applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="90664-111">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="90664-112">Selezionare **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="90664-112">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="90664-113">La figura seguente mostra **.NET Core** selezionata, ma è possibile selezionare **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="90664-113">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="90664-114">Selezionare **Modifica autenticazione** e impostato su **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="90664-114">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="90664-115">Mantenere il valore predefinito **in-app dell'account utente di archivio**.</span><span class="sxs-lookup"><span data-stu-id="90664-115">Keep the default **Store user accounts in-app**.</span></span>

![Finestra Nuovo progetto con "Singoli account utente opzione"](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90664-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90664-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="90664-118">L'esercitazione è necessario Visual Studio 2017 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="90664-118">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="90664-119">In Visual Studio, creare un nuovo progetto applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="90664-119">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="90664-120">Selezionare **Modifica autenticazione** e impostato su **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="90664-120">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![Finestra Nuovo progetto con "Singoli account utente opzione"](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="90664-122">Creazione del progetto CLI di .NET core per macOS e Linux</span><span class="sxs-lookup"><span data-stu-id="90664-122">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="90664-123">Se si utilizza l'interfaccia CLI o SQLite, eseguire le operazioni seguenti in una finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="90664-123">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="90664-124">`--auth Individual`Specifica il modello di singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="90664-124">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="90664-125">In Windows, aggiungere il `-uld` opzione.</span><span class="sxs-lookup"><span data-stu-id="90664-125">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="90664-126">Il `-uld` opzione Crea una stringa di connessione del database locale anziché un database SQLite.</span><span class="sxs-lookup"><span data-stu-id="90664-126">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="90664-127">Eseguire `new mvc --help` per ottenere assistenza su questo comando.</span><span class="sxs-lookup"><span data-stu-id="90664-127">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="90664-128">Verifica registrazione nuovo utente</span><span class="sxs-lookup"><span data-stu-id="90664-128">Test new user registration</span></span>

<span data-ttu-id="90664-129">Eseguire l'app, selezionare il **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="90664-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="90664-130">Seguire le istruzioni per eseguire le migrazioni di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="90664-130">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="90664-131">A questo punto, la convalida sola nel messaggio di posta elettronica è con il [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo.</span><span class="sxs-lookup"><span data-stu-id="90664-131">At this  point, the only validation on the email is with the [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="90664-132">Dopo aver inviato la registrazione, si è connessi all'app.</span><span class="sxs-lookup"><span data-stu-id="90664-132">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="90664-133">Più avanti nell'esercitazione, verranno modificate in questo modo nuovi utenti non possono accedere finché non viene convalidata la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-133">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="90664-134">Visualizzare il database di identità</span><span class="sxs-lookup"><span data-stu-id="90664-134">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="90664-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="90664-135">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="90664-136">Dal **vista** dal menu **Esplora oggetti di SQL Server** (sillaba SSOX).</span><span class="sxs-lookup"><span data-stu-id="90664-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="90664-137">Passare a **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="90664-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="90664-138">Fare clic su **dbo. AspNetUsers** > **visualizzare dati**:</span><span class="sxs-lookup"><span data-stu-id="90664-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu di scelta rapida per la tabella AspNetUsers in Esplora oggetti di SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="90664-140">Si noti il `EmailConfirmed` campo `False`.</span><span class="sxs-lookup"><span data-stu-id="90664-140">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="90664-141">Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="90664-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="90664-142">Fare doppio clic sulla riga e selezionare **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="90664-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="90664-143">Eliminare il messaggio di posta elettronica alias ora renderà più facile nei passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="90664-143">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="90664-144">SQLite</span><span class="sxs-lookup"><span data-stu-id="90664-144">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="90664-145">Vedere [utilizzo di SQLite in un progetto ASP.NET MVC Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) per istruzioni su come visualizzare il database di SQLite.</span><span class="sxs-lookup"><span data-stu-id="90664-145">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="90664-146">Richiedere SSL e l'installazione di IIS Express per SSL</span><span class="sxs-lookup"><span data-stu-id="90664-146">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="90664-147">Vedere [applicazione SSL](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="90664-147">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="90664-148">Richiedi conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="90664-148">Require email confirmation</span></span>

<span data-ttu-id="90664-149">È consigliabile verificare il messaggio di posta elettronica di una nuova registrazione utente per verificare che non viene rappresentata un'altra persona (vale a dire, è ancora stato registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="90664-149">It's a best practice to confirm the email of a new user registration to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="90664-150">Si supponga che si dispone di un forum di discussione e si desidera impedire "yli@example.com"tramite la registrazione come"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="90664-150">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="90664-151">Senza conferma tramite posta elettronica, "nolivetto@contoso.com" Impossibile ricevere la posta elettronica indesiderato dall'app.</span><span class="sxs-lookup"><span data-stu-id="90664-151">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="90664-152">Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non veniva rilevato l'errore di ortografia di "yli", sarebbe possibile usare il recupero della password, perché l'app non dispone di posta elettronica corretta.</span><span class="sxs-lookup"><span data-stu-id="90664-152">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="90664-153">Conferma tramite posta elettronica fornisce a livello di protezione limitato da BOT e non offre protezione da spamming determinato che dispongono di molti alias di posta elettronica di lavoro che possono utilizzare per registrare.</span><span class="sxs-lookup"><span data-stu-id="90664-153">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="90664-154">In genere si desidera impedire agli utenti di nuovo di tutti i dati al sito web di registrazione prima che sia una conferma tramite posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="90664-155">Aggiornamento `ConfigureServices` per richiedere una conferma tramite posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="90664-155">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90664-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90664-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90664-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90664-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="90664-158">La riga precedente impedisce registrati in corso la registrazione fino a quando non viene confermata la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-158">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="90664-159">Tuttavia, tale riga non impedisca nuovi utenti viene effettuato l'accesso dopo cui registrare.</span><span class="sxs-lookup"><span data-stu-id="90664-159">However, that line doesn't prevent new users from being logged in after they register.</span></span> <span data-ttu-id="90664-160">Il codice predefinito l'accesso utente dopo che la registrazione.</span><span class="sxs-lookup"><span data-stu-id="90664-160">The default code logs in a user after they register.</span></span> <span data-ttu-id="90664-161">Dopo l'accesso, non sarà in grado di accedere di nuovo fino a quando non vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="90664-161">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="90664-162">Più avanti in questa esercitazione verranno modificate sono l'utente di codice in modo appena registrato **non** effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="90664-162">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="90664-163">Configurare i provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="90664-163">Configure email provider</span></span>

<span data-ttu-id="90664-164">In questa esercitazione, SendGrid viene utilizzato per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="90664-165">È necessario un account SendGrid e una chiave per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="90664-166">È possibile utilizzare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-166">You can use other email providers.</span></span> <span data-ttu-id="90664-167">ASP.NET Core 2. x include `System.Net.Mail`, che consente di inviare posta elettronica dalla tua app.</span><span class="sxs-lookup"><span data-stu-id="90664-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="90664-168">È consigliabile che utilizzare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="90664-169">SMTP è difficile da proteggere e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="90664-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="90664-170">Il [modello opzioni](xref:fundamentals/configuration/options) viene utilizzato per accedere alle impostazioni di account e la chiave utente.</span><span class="sxs-lookup"><span data-stu-id="90664-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="90664-171">Per ulteriori informazioni, vedere [configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="90664-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="90664-172">Creare una classe per recuperare la chiave di proteggere la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="90664-173">Per questo esempio, il `AuthMessageSenderOptions` classe il *Services/AuthMessageSenderOptions.cs* file.</span><span class="sxs-lookup"><span data-stu-id="90664-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="90664-174">Impostare il `SendGridUser` e `SendGridKey` con il [strumento segreto manager](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="90664-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="90664-175">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="90664-175">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="90664-176">In Windows, gestione Secret archivia le coppie di chiavi/valore in un *secrets.json* file nella directory %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.</span><span class="sxs-lookup"><span data-stu-id="90664-176">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="90664-177">Il contenuto del *secrets.json* file non vengono crittografati.</span><span class="sxs-lookup"><span data-stu-id="90664-177">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="90664-178">Il *secrets.json* file è illustrato di seguito (il `SendGridKey` valore è stato rimosso.)</span><span class="sxs-lookup"><span data-stu-id="90664-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="90664-179">Configurare l'avvio per l'utilizzo di AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="90664-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="90664-180">Aggiungere `AuthMessageSenderOptions` al contenitore del servizio alla fine del `ConfigureServices` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="90664-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90664-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90664-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90664-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90664-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="90664-183">Configurare la classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="90664-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="90664-184">In questa esercitazione viene illustrato come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="90664-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="90664-185">Installare il `SendGrid` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="90664-185">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="90664-186">Dalla Console di gestione pacchetti, immettere quanto segue il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="90664-186">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="90664-187">Vedere [iniziare gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account di SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="90664-187">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="90664-188">Configurare SendGrid</span><span class="sxs-lookup"><span data-stu-id="90664-188">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90664-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90664-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="90664-190">Aggiungere il codice nel *Services/EmailSender.cs* simile al seguente per configurare SendGrid:</span><span class="sxs-lookup"><span data-stu-id="90664-190">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90664-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90664-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="90664-192">Aggiungere il codice nel *Services/MessageServices.cs* simile al seguente per configurare SendGrid:</span><span class="sxs-lookup"><span data-stu-id="90664-192">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="90664-193">Abilitare il ripristino di conferma e la password di account</span><span class="sxs-lookup"><span data-stu-id="90664-193">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="90664-194">Il modello presenta il codice per il ripristino di conferma e la password di account.</span><span class="sxs-lookup"><span data-stu-id="90664-194">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="90664-195">Trovare il `[HttpPost] Register` metodo il *AccountController.cs* file.</span><span class="sxs-lookup"><span data-stu-id="90664-195">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90664-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90664-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="90664-197">Impedire che i nuovi utenti registrati automaticamente l'accesso da impostare come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="90664-197">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="90664-198">Il metodo completo viene visualizzato con la riga modificata evidenziata:</span><span class="sxs-lookup"><span data-stu-id="90664-198">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

<span data-ttu-id="90664-199">Nota: Il codice precedente avrà esito negativo se si implementa `IEmailSender` e inviare un messaggio di testo normale.</span><span class="sxs-lookup"><span data-stu-id="90664-199">Note: The previous code will fail if you implement `IEmailSender` and send a plain text email.</span></span> <span data-ttu-id="90664-200">Vedere [questo problema](https://github.com/aspnet/Home/issues/2152) per ulteriori informazioni e una soluzione alternativa.</span><span class="sxs-lookup"><span data-stu-id="90664-200">See [this issue](https://github.com/aspnet/Home/issues/2152) for more information and a workaround.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90664-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90664-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="90664-202">Rimuovere il commento il codice per consentire la conferma dell'account.</span><span class="sxs-lookup"><span data-stu-id="90664-202">Uncomment the code to enable account confirmation.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="90664-203">Nota: È stiamo anche impedire un utente appena registrato automaticamente l'accesso da impostare come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="90664-203">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="90664-204">Abilita ripristino password rimuovendo il codice di `ForgotPassword` azione nel *Controllers/AccountController.cs* file.</span><span class="sxs-lookup"><span data-stu-id="90664-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="90664-205">Rimuovere il commento modulo *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="90664-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="90664-206">È possibile rimuovere il `<p> For more information on how to enable reset password ... </p>` elemento che contiene un collegamento a questo articolo.</span><span class="sxs-lookup"><span data-stu-id="90664-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="90664-207">Registrare, posta elettronica di conferma e reimpostare la password</span><span class="sxs-lookup"><span data-stu-id="90664-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="90664-208">Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.</span><span class="sxs-lookup"><span data-stu-id="90664-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="90664-209">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="90664-209">Run the app and register a new user</span></span>

 ![Visualizzazione di registrare Account dell'applicazione Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="90664-211">Controllare la posta elettronica per il collegamento di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="90664-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="90664-212">Vedere [Debug posta elettronica](#debug) se non si ottiene il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="90664-213">Fare clic sul collegamento per confermare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="90664-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="90664-214">Accedere con il messaggio di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="90664-214">Log in with your email and password.</span></span>
* <span data-ttu-id="90664-215">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="90664-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="90664-216">Visualizzare la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="90664-216">View the manage page</span></span>

<span data-ttu-id="90664-217">Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="90664-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="90664-218">Potrebbe essere necessario espandere la barra di spostamento per visualizzare il nome utente.</span><span class="sxs-lookup"><span data-stu-id="90664-218">You might need to expand the navbar to see user name.</span></span>

![barra di spostamento](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90664-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90664-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="90664-221">Verrà visualizzata la pagina di gestione con il **profilo** scheda selezionata.</span><span class="sxs-lookup"><span data-stu-id="90664-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="90664-222">Il **posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.</span><span class="sxs-lookup"><span data-stu-id="90664-222">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![pagina Gestisci](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90664-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90664-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="90664-225">Su questa pagina verrà descritto più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="90664-225">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="90664-226">![pagina Gestisci](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="90664-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="90664-227">Reimpostazione della password di test</span><span class="sxs-lookup"><span data-stu-id="90664-227">Test password reset</span></span>

* <span data-ttu-id="90664-228">Se si è connessi, selezionare **Logout**.</span><span class="sxs-lookup"><span data-stu-id="90664-228">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="90664-229">Selezionare il **Accedi** collegamento e selezionare il **password dimenticata?** collegamento.</span><span class="sxs-lookup"><span data-stu-id="90664-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="90664-230">Immettere il messaggio di posta elettronica che è utilizzato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="90664-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="90664-231">Verrà inviato un messaggio di posta elettronica con un collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="90664-231">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="90664-232">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="90664-232">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="90664-233">Dopo che la password è stata reimpostata, può eseguire l'accesso con il messaggio di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="90664-233">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="90664-234">Eseguire il debug di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="90664-234">Debug email</span></span>

<span data-ttu-id="90664-235">Se non è possibile ottenere l'utilizzo di posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="90664-235">If you can't get email working:</span></span>

* <span data-ttu-id="90664-236">Esaminare il [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.</span><span class="sxs-lookup"><span data-stu-id="90664-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="90664-237">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="90664-237">Check your spam folder.</span></span>
* <span data-ttu-id="90664-238">Provare un altro alias di posta elettronica su un provider diverso posta elettronica (Microsoft, Yahoo, Gmail, ecc.)</span><span class="sxs-lookup"><span data-stu-id="90664-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="90664-239">Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="90664-239">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="90664-240">Provare a inviare agli account di posta elettronica diverso.</span><span class="sxs-lookup"><span data-stu-id="90664-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="90664-241">**Nota:** è una procedura consigliata di non utilizzare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="90664-241">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="90664-242">Se si pubblica l'app in Azure, è possibile impostare i segreti SendGrid come impostazioni dell'applicazione nel portale di Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="90664-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="90664-243">Il sistema di configurazione è configurato per la lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="90664-243">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="90664-244">Impedire l'accesso al momento della registrazione</span><span class="sxs-lookup"><span data-stu-id="90664-244">Prevent login at registration</span></span>

<span data-ttu-id="90664-245">Con i modelli di correnti, quando un utente ha completato il modulo di registrazione, si è connessi (autenticato).</span><span class="sxs-lookup"><span data-stu-id="90664-245">With the current templates, once a user completes the registration form, they're logged in (authenticated).</span></span> <span data-ttu-id="90664-246">In genere, è necessario confermare la posta elettronica prima di registrarle.</span><span class="sxs-lookup"><span data-stu-id="90664-246">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="90664-247">Nella sezione seguente si modificherà il codice in modo da richiedere nuovi utenti dispongano di un messaggio di posta elettronica di conferma prima che si è connessi.</span><span class="sxs-lookup"><span data-stu-id="90664-247">In the section below, we will modify the code to require new users have a confirmed email before they're logged in.</span></span> <span data-ttu-id="90664-248">Aggiornamento di `[HttpPost] Login` azione nel *AccountController.cs* file con le seguenti modifiche evidenziate.</span><span class="sxs-lookup"><span data-stu-id="90664-248">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

<span data-ttu-id="90664-249">**Nota:** è una procedura consigliata di non utilizzare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="90664-249">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="90664-250">Se si pubblica l'app in Azure, è possibile impostare i segreti SendGrid come impostazioni dell'applicazione nel portale di Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="90664-250">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="90664-251">Il sistema di configurazione è configurato per la lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="90664-251">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="90664-252">Combinare gli account di accesso social networking e locale</span><span class="sxs-lookup"><span data-stu-id="90664-252">Combine social and local login accounts</span></span>

<span data-ttu-id="90664-253">Nota: In questa sezione si applica solo a ASP.NET di base 1. x.</span><span class="sxs-lookup"><span data-stu-id="90664-253">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="90664-254">Per ASP.NET Core 2. x, vedere [questo](https://github.com/aspnet/Docs/issues/3753) problema.</span><span class="sxs-lookup"><span data-stu-id="90664-254">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="90664-255">Per completare questa sezione, è innanzitutto necessario abilitare i provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="90664-255">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="90664-256">Vedere [Abilitazione autenticazione con Facebook, Google e altri provider esterni](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="90664-256">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="90664-257">È possibile combinare gli account locali e sociali facendo clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="90664-257">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="90664-258">Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso social prima, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="90664-258">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="90664-260">Fare clic su di **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="90664-260">Click on the **Manage** link.</span></span> <span data-ttu-id="90664-261">Si noti il valore 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="90664-261">Note the 0 external (social logins) associated with this account.</span></span>

![Gestione visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="90664-263">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="90664-263">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="90664-264">Nell'immagine seguente, Facebook è il provider di autenticazione esterni:</span><span class="sxs-lookup"><span data-stu-id="90664-264">In the image below, Facebook is the external authentication provider:</span></span>

![Gestire l'account di accesso esterni di visualizzazione elenco Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="90664-266">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="90664-266">The two accounts have been combined.</span></span> <span data-ttu-id="90664-267">Sarà in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="90664-267">You will be able to log on with either account.</span></span> <span data-ttu-id="90664-268">Si consiglia agli utenti di aggiungere gli account locali nel caso in cui il log di social networking nel servizio di autenticazione non è attivo, o più probabile hai più accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="90664-268">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they've lost access to their social account.</span></span>
