---
title: La conferma dell'account e Password di ripristino in ASP.NET Core
author: rick-anderson
description: Viene illustrato come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
keywords: ASP.NET Core, la reimpostazione della password, conferma tramite posta elettronica, protezione
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: aaed75c78a99e59954add959a76a2fd68ea5f3fc
ms.sourcegitcommit: f2fb0b45284e4f8c4a9c422bec790aede7c1f0ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2017
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="c7950-104">La conferma dell'account e il recupero della password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7950-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="c7950-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c7950-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c7950-106">In questa esercitazione viene illustrato come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="c7950-106">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="c7950-107">Creare un nuovo progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7950-107">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7950-108">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="c7950-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c7950-109">Questo passaggio si applica a Visual Studio in Windows.</span><span class="sxs-lookup"><span data-stu-id="c7950-109">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="c7950-110">Vedere la sezione successiva per le istruzioni di CLI.</span><span class="sxs-lookup"><span data-stu-id="c7950-110">See the next section for CLI instructions.</span></span>

<span data-ttu-id="c7950-111">L'esercitazione è necessario Visual Studio 2017 Preview 2 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="c7950-111">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="c7950-112">In Visual Studio, creare un nuovo progetto applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="c7950-112">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="c7950-113">Selezionare **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="c7950-113">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="c7950-114">La figura seguente mostra **.NET Core** selezionata, ma è possibile selezionare **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="c7950-114">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="c7950-115">Selezionare **Modifica autenticazione** e impostato su **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="c7950-115">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="c7950-116">Mantenere il valore predefinito **in-app dell'account utente di archivio**.</span><span class="sxs-lookup"><span data-stu-id="c7950-116">Keep the default **Store user accounts in-app**.</span></span>

![Finestra Nuovo progetto con "Singoli account utente opzione"](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7950-118">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="c7950-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c7950-119">L'esercitazione è necessario Visual Studio 2017 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="c7950-119">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="c7950-120">In Visual Studio, creare un nuovo progetto applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="c7950-120">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="c7950-121">Selezionare **Modifica autenticazione** e impostato su **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="c7950-121">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![Finestra Nuovo progetto con "Singoli account utente opzione"](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="c7950-123">Creazione del progetto CLI di .NET core per macOS e Linux</span><span class="sxs-lookup"><span data-stu-id="c7950-123">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="c7950-124">Se si utilizza l'interfaccia CLI o SQLite, eseguire le operazioni seguenti in una finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="c7950-124">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="c7950-125">`--auth Individual`Specifica il modello di singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="c7950-125">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="c7950-126">In Windows, aggiungere il `-uld` opzione.</span><span class="sxs-lookup"><span data-stu-id="c7950-126">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="c7950-127">Il `-uld` opzione Crea una stringa di connessione del database locale anziché un database SQLite.</span><span class="sxs-lookup"><span data-stu-id="c7950-127">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="c7950-128">Eseguire `new mvc --help` per ottenere assistenza su questo comando.</span><span class="sxs-lookup"><span data-stu-id="c7950-128">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="c7950-129">Verifica registrazione nuovo utente</span><span class="sxs-lookup"><span data-stu-id="c7950-129">Test new user registration</span></span>

<span data-ttu-id="c7950-130">Eseguire l'app, selezionare il **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="c7950-130">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="c7950-131">Seguire le istruzioni per eseguire le migrazioni di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c7950-131">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="c7950-132">A questo punto, la convalida sola nel messaggio di posta elettronica è con il [[EmailAddress]](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attributo.</span><span class="sxs-lookup"><span data-stu-id="c7950-132">At this  point, the only validation on the email is with the [[EmailAddress]](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span> <span data-ttu-id="c7950-133">Dopo aver inviato la registrazione, si è connessi all'app.</span><span class="sxs-lookup"><span data-stu-id="c7950-133">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="c7950-134">Più avanti nell'esercitazione, verranno modificate in questo modo nuovi utenti non possono accedere finché non viene convalidata la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-134">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="c7950-135">Visualizzare il database di identità</span><span class="sxs-lookup"><span data-stu-id="c7950-135">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="c7950-136">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c7950-136">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="c7950-137">Dal **vista** dal menu **Esplora oggetti di SQL Server** (sillaba SSOX).</span><span class="sxs-lookup"><span data-stu-id="c7950-137">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="c7950-138">Passare a **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="c7950-138">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="c7950-139">Fare clic su **dbo. AspNetUsers** > **visualizzare dati**:</span><span class="sxs-lookup"><span data-stu-id="c7950-139">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu di scelta rapida per la tabella AspNetUsers in Esplora oggetti di SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="c7950-141">Si noti il `EmailConfirmed` campo `False`.</span><span class="sxs-lookup"><span data-stu-id="c7950-141">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="c7950-142">Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="c7950-142">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="c7950-143">Fare doppio clic sulla riga e selezionare **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="c7950-143">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="c7950-144">Eliminare il messaggio di posta elettronica alias ora renderà più facile nei passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="c7950-144">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="c7950-145">SQLite</span><span class="sxs-lookup"><span data-stu-id="c7950-145">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="c7950-146">Vedere [utilizzo di SQLite in un progetto ASP.NET MVC Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) per istruzioni su come visualizzare il database di SQLite.</span><span class="sxs-lookup"><span data-stu-id="c7950-146">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="c7950-147">Richiedere SSL e l'installazione di IIS Express per SSL</span><span class="sxs-lookup"><span data-stu-id="c7950-147">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="c7950-148">Vedere [applicazione SSL](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="c7950-148">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="c7950-149">Richiedi conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="c7950-149">Require email confirmation</span></span>

<span data-ttu-id="c7950-150">È consigliabile verificare il messaggio di posta elettronica di una nuova registrazione utente per verificare che non rappresentano un altro utente (vale a dire, è ancora stato registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="c7950-150">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="c7950-151">Si supponga che si dispone di un forum di discussione e si desidera impedire "yli@example.com"tramite la registrazione come"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="c7950-151">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="c7950-152">Senza conferma tramite posta elettronica, "nolivetto@contoso.com" Impossibile ricevere la posta elettronica indesiderato dall'app.</span><span class="sxs-lookup"><span data-stu-id="c7950-152">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="c7950-153">Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non veniva rilevato l'errore di ortografia di "yli", sarebbe possibile usare il recupero della password, perché l'app non dispone di posta elettronica corretta.</span><span class="sxs-lookup"><span data-stu-id="c7950-153">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="c7950-154">Conferma tramite posta elettronica fornisce a livello di protezione limitato da BOT e non offre protezione da spamming determinato che dispongono di molti alias di posta elettronica di lavoro che possono utilizzare per registrare.</span><span class="sxs-lookup"><span data-stu-id="c7950-154">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="c7950-155">In genere si desidera impedire agli utenti di nuovo di tutti i dati al sito web di registrazione prima che sia una conferma tramite posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-155">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="c7950-156">Aggiornamento `ConfigureServices` per richiedere una conferma tramite posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="c7950-156">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7950-157">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="c7950-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c7950-158">[!code-csharp[Principale](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]</span><span class="sxs-lookup"><span data-stu-id="c7950-158">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]</span></span>


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7950-159">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="c7950-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c7950-160">[!code-csharp[Principale](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]</span><span class="sxs-lookup"><span data-stu-id="c7950-160">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]</span></span>

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="c7950-161">La riga precedente impedisce registrati in corso la registrazione fino a quando non viene confermata la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-161">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="c7950-162">Tuttavia, tale riga non impedire nuovi utenti viene effettuato l'accesso dopo cui registrare.</span><span class="sxs-lookup"><span data-stu-id="c7950-162">However, that line does not prevent new users from being logged in after they register.</span></span> <span data-ttu-id="c7950-163">Il codice predefinito l'accesso utente dopo che la registrazione.</span><span class="sxs-lookup"><span data-stu-id="c7950-163">The default code logs in a user after they register.</span></span> <span data-ttu-id="c7950-164">Dopo l'accesso, non sarà in grado di accedere di nuovo fino a quando non vengono registrati.</span><span class="sxs-lookup"><span data-stu-id="c7950-164">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="c7950-165">Più avanti in questa esercitazione verranno modificate sono l'utente di codice in modo appena registrato **non** effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="c7950-165">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="c7950-166">Configurare i provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="c7950-166">Configure email provider</span></span>

<span data-ttu-id="c7950-167">In questa esercitazione, SendGrid viene utilizzato per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-167">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="c7950-168">È necessario un account SendGrid e una chiave per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-168">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="c7950-169">È possibile utilizzare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-169">You can use other email providers.</span></span> <span data-ttu-id="c7950-170">ASP.NET Core 2. x include `System.Net.Mail`, che consente di inviare posta elettronica dalla tua app.</span><span class="sxs-lookup"><span data-stu-id="c7950-170">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="c7950-171">È consigliabile che utilizzare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-171">We recommend you use SendGrid or another email service to send email.</span></span>

<span data-ttu-id="c7950-172">Il [modello opzioni](xref:fundamentals/configuration#options-config-objects) viene utilizzato per accedere alle impostazioni di account e la chiave utente.</span><span class="sxs-lookup"><span data-stu-id="c7950-172">The [Options pattern](xref:fundamentals/configuration#options-config-objects) is used to access the user account and key settings.</span></span> <span data-ttu-id="c7950-173">Per ulteriori informazioni, vedere [configurazione](xref:fundamentals/configuration#fundamentals-configuration).</span><span class="sxs-lookup"><span data-stu-id="c7950-173">For more information, see [configuration](xref:fundamentals/configuration#fundamentals-configuration).</span></span>

<span data-ttu-id="c7950-174">Creare una classe per recuperare la chiave di proteggere la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="c7950-175">Per questo esempio, il `AuthMessageSenderOptions` classe il *Services/AuthMessageSenderOptions.cs* file.</span><span class="sxs-lookup"><span data-stu-id="c7950-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

<span data-ttu-id="c7950-176">[!code-csharp[Principale](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c7950-176">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]</span></span>

<span data-ttu-id="c7950-177">Impostare il `SendGridUser` e `SendGridKey` con il [strumento segreto manager](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="c7950-177">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="c7950-178">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7950-178">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="c7950-179">In Windows, gestione Secret archivia le coppie di chiavi/valore in un *secrets.json* file nella directory %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.</span><span class="sxs-lookup"><span data-stu-id="c7950-179">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="c7950-180">Il contenuto del *secrets.json* file non vengono crittografati.</span><span class="sxs-lookup"><span data-stu-id="c7950-180">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="c7950-181">Il *secrets.json* file è illustrato di seguito (il `SendGridKey` valore è stato rimosso.)</span><span class="sxs-lookup"><span data-stu-id="c7950-181">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="c7950-182">Configurare l'avvio per l'utilizzo di AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="c7950-182">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="c7950-183">Aggiungere `AuthMessageSenderOptions` al contenitore del servizio alla fine del `ConfigureServices` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="c7950-183">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7950-184">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="c7950-184">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c7950-185">[!code-csharp[Principale](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]</span><span class="sxs-lookup"><span data-stu-id="c7950-185">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7950-186">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="c7950-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
<span data-ttu-id="c7950-187">[!code-csharp[Principale](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]</span><span class="sxs-lookup"><span data-stu-id="c7950-187">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]</span></span>

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="c7950-188">Configurare la classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="c7950-188">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="c7950-189">In questa esercitazione viene illustrato come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="c7950-189">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="c7950-190">Installare il `SendGrid` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="c7950-190">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="c7950-191">Dalla Console di gestione pacchetti, immettere quanto segue il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c7950-191">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="c7950-192">Vedere [iniziare gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account di SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="c7950-192">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="c7950-193">Configurare SendGrid</span><span class="sxs-lookup"><span data-stu-id="c7950-193">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7950-194">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="c7950-194">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="c7950-195">Aggiungere il codice nel *Services/EmailSender.cs* simile al seguente per configurare SendGrid:</span><span class="sxs-lookup"><span data-stu-id="c7950-195">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

<span data-ttu-id="c7950-196">[!code-csharp[Principale](accconfirm/sample/WebPW/Services/EmailSender.cs)]</span><span class="sxs-lookup"><span data-stu-id="c7950-196">[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]</span></span>


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7950-197">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="c7950-197">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="c7950-198">Aggiungere il codice nel *Services/MessageServices.cs* simile al seguente per configurare SendGrid:</span><span class="sxs-lookup"><span data-stu-id="c7950-198">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

<span data-ttu-id="c7950-199">[!code-csharp[Principale](accconfirm/sample/WebApp1/Services/MessageServices.cs)]</span><span class="sxs-lookup"><span data-stu-id="c7950-199">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]</span></span>

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="c7950-200">Abilitare il ripristino di conferma e la password di account</span><span class="sxs-lookup"><span data-stu-id="c7950-200">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="c7950-201">Il modello presenta il codice per il ripristino di conferma e la password di account.</span><span class="sxs-lookup"><span data-stu-id="c7950-201">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="c7950-202">Trovare il `[HttpPost] Register` metodo il *AccountController.cs* file.</span><span class="sxs-lookup"><span data-stu-id="c7950-202">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7950-203">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="c7950-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c7950-204">Impedire che i nuovi utenti registrati automaticamente l'accesso da impostare come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="c7950-204">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c7950-205">Il metodo completo viene visualizzato con la riga modificata evidenziata:</span><span class="sxs-lookup"><span data-stu-id="c7950-205">The complete method is shown with the changed line highlighted:</span></span>

<span data-ttu-id="c7950-206">[!code-csharp[Principale](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]</span><span class="sxs-lookup"><span data-stu-id="c7950-206">[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7950-207">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="c7950-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c7950-208">Rimuovere il commento il codice per consentire la conferma dell'account.</span><span class="sxs-lookup"><span data-stu-id="c7950-208">Uncomment the code to enable account confirmation.</span></span>

<span data-ttu-id="c7950-209">[!code-csharp[Principale](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]</span><span class="sxs-lookup"><span data-stu-id="c7950-209">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]</span></span>

<span data-ttu-id="c7950-210">Nota: È stiamo anche impedire un utente appena registrato automaticamente l'accesso da impostare come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="c7950-210">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c7950-211">Abilita ripristino password rimuovendo il codice di `ForgotPassword` azione nel *Controllers/AccountController.cs* file.</span><span class="sxs-lookup"><span data-stu-id="c7950-211">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

<span data-ttu-id="c7950-212">[!code-csharp[Principale](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]</span><span class="sxs-lookup"><span data-stu-id="c7950-212">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]</span></span>

<span data-ttu-id="c7950-213">Rimuovere il commento modulo *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c7950-213">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="c7950-214">È possibile rimuovere il `<p> For more information on how to enable reset password ... </p>` elemento che contiene un collegamento a questo articolo.</span><span class="sxs-lookup"><span data-stu-id="c7950-214">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

<span data-ttu-id="c7950-215">[!code-html[Principale](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]</span><span class="sxs-lookup"><span data-stu-id="c7950-215">[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]</span></span>

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="c7950-216">Registrare, posta elettronica di conferma e reimpostare la password</span><span class="sxs-lookup"><span data-stu-id="c7950-216">Register, confirm email, and reset password</span></span>

<span data-ttu-id="c7950-217">Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.</span><span class="sxs-lookup"><span data-stu-id="c7950-217">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="c7950-218">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="c7950-218">Run the app and register a new user</span></span>

 ![Visualizzazione di registrare Account dell'applicazione Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="c7950-220">Controllare la posta elettronica per il collegamento di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="c7950-220">Check your email for the account confirmation link.</span></span> <span data-ttu-id="c7950-221">Vedere [Debug posta elettronica](#debug) se non si ottiene il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-221">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="c7950-222">Fare clic sul collegamento per confermare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c7950-222">Click the link to confirm your email.</span></span>
* <span data-ttu-id="c7950-223">Accedere con il messaggio di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="c7950-223">Log in with your email and password.</span></span>
* <span data-ttu-id="c7950-224">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="c7950-224">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="c7950-225">Visualizzare la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="c7950-225">View the manage page</span></span>

<span data-ttu-id="c7950-226">Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="c7950-226">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="c7950-227">Potrebbe essere necessario espandere la barra di spostamento per visualizzare il nome utente.</span><span class="sxs-lookup"><span data-stu-id="c7950-227">You might need to expand the navbar to see user name.</span></span>

![barra di spostamento](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c7950-229">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="c7950-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c7950-230">Verrà visualizzata la pagina di gestione con il **profilo** scheda selezionata.</span><span class="sxs-lookup"><span data-stu-id="c7950-230">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="c7950-231">Il **posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.</span><span class="sxs-lookup"><span data-stu-id="c7950-231">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![pagina Gestisci](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c7950-233">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="c7950-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c7950-234">Su questa pagina verrà descritto più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c7950-234">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="c7950-235">![pagina Gestisci](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="c7950-235">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="c7950-236">Reimpostazione della password di test</span><span class="sxs-lookup"><span data-stu-id="c7950-236">Test password reset</span></span>

* <span data-ttu-id="c7950-237">Se si è connessi, selezionare **Logout**.</span><span class="sxs-lookup"><span data-stu-id="c7950-237">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="c7950-238">Selezionare il **Accedi** collegamento e selezionare il **password dimenticata?** collegamento.</span><span class="sxs-lookup"><span data-stu-id="c7950-238">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="c7950-239">Immettere il messaggio di posta elettronica che è utilizzato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="c7950-239">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="c7950-240">Verrà inviato un messaggio di posta elettronica con un collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="c7950-240">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="c7950-241">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="c7950-241">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="c7950-242">Dopo che la password è stata reimpostata, può eseguire l'accesso con il messaggio di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="c7950-242">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="c7950-243">Eseguire il debug di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="c7950-243">Debug email</span></span>

<span data-ttu-id="c7950-244">Se non è possibile ottenere l'utilizzo di posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="c7950-244">If you can't get email working:</span></span>

* <span data-ttu-id="c7950-245">Esaminare il [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.</span><span class="sxs-lookup"><span data-stu-id="c7950-245">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="c7950-246">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="c7950-246">Check your spam folder.</span></span>
* <span data-ttu-id="c7950-247">Provare un altro alias di posta elettronica su un provider diverso posta elettronica (Microsoft, Yahoo, Gmail, ecc.)</span><span class="sxs-lookup"><span data-stu-id="c7950-247">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="c7950-248">Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="c7950-248">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="c7950-249">Provare a inviare agli account di posta elettronica diverso.</span><span class="sxs-lookup"><span data-stu-id="c7950-249">Try sending to different email accounts.</span></span>

<span data-ttu-id="c7950-250">**Nota:** è una procedura consigliata di non utilizzare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="c7950-250">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="c7950-251">Se si pubblica l'app in Azure, è possibile impostare i segreti SendGrid come impostazioni dell'applicazione nel portale di Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="c7950-251">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="c7950-252">Il sistema di configurazione è configurato per la lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c7950-252">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="c7950-253">Impedire l'accesso al momento della registrazione</span><span class="sxs-lookup"><span data-stu-id="c7950-253">Prevent login at registration</span></span>

<span data-ttu-id="c7950-254">Con i modelli di correnti, quando un utente ha completato il modulo di registrazione, vengono registrate nel (autenticato).</span><span class="sxs-lookup"><span data-stu-id="c7950-254">With the current templates, once a user completes the registration form, they are logged in (authenticated).</span></span> <span data-ttu-id="c7950-255">In genere, è necessario confermare la posta elettronica prima di registrarle.</span><span class="sxs-lookup"><span data-stu-id="c7950-255">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="c7950-256">Nella sezione seguente si modificherà il codice in modo da richiedere nuovi utenti dispongano di un messaggio di posta elettronica di conferma prima di vengono registrate nel.</span><span class="sxs-lookup"><span data-stu-id="c7950-256">In the section below, we will modify the code to require new users have a confirmed email before they are logged in.</span></span> <span data-ttu-id="c7950-257">Aggiornamento di `[HttpPost] Login` azione nel *AccountController.cs* file con le seguenti modifiche evidenziate.</span><span class="sxs-lookup"><span data-stu-id="c7950-257">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

<span data-ttu-id="c7950-258">[!code-csharp[Principale](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]</span><span class="sxs-lookup"><span data-stu-id="c7950-258">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]</span></span>

<span data-ttu-id="c7950-259">**Nota:** è una procedura consigliata di non utilizzare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="c7950-259">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="c7950-260">Se si pubblica l'app in Azure, è possibile impostare i segreti SendGrid come impostazioni dell'applicazione nel portale di Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="c7950-260">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="c7950-261">Il sistema di configurazione è configurato per la lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c7950-261">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="c7950-262">Combinare gli account di accesso social networking e locale</span><span class="sxs-lookup"><span data-stu-id="c7950-262">Combine social and local login accounts</span></span>

<span data-ttu-id="c7950-263">Nota: In questa sezione si applica solo a ASP.NET di base 1. x.</span><span class="sxs-lookup"><span data-stu-id="c7950-263">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="c7950-264">Per ASP.NET Core 2. x, vedere [questo](https://github.com/aspnet/Docs/issues/3753) problema.</span><span class="sxs-lookup"><span data-stu-id="c7950-264">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="c7950-265">Per completare questa sezione, è innanzitutto necessario abilitare i provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="c7950-265">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="c7950-266">Vedere [Abilitazione autenticazione con Facebook, Google e altri provider esterni](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="c7950-266">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="c7950-267">È possibile combinare gli account locali e sociali facendo clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="c7950-267">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="c7950-268">Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso social prima, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="c7950-268">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="c7950-270">Fare clic su di **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="c7950-270">Click on the **Manage** link.</span></span> <span data-ttu-id="c7950-271">Si noti il valore 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="c7950-271">Note the 0 external (social logins) associated with this account.</span></span>

![Gestione visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="c7950-273">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="c7950-273">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="c7950-274">Nell'immagine seguente, Facebook è il provider di autenticazione esterni:</span><span class="sxs-lookup"><span data-stu-id="c7950-274">In the image below, Facebook is the external authentication provider:</span></span>

![Gestire l'account di accesso esterni di visualizzazione elenco Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="c7950-276">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="c7950-276">The two accounts have been combined.</span></span> <span data-ttu-id="c7950-277">Sarà in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="c7950-277">You will be able to log on with either account.</span></span> <span data-ttu-id="c7950-278">Si consiglia agli utenti di aggiungere gli account locali nel caso in cui il log di social networking nel servizio di autenticazione è inattivo o più probabilmente è più possibile accedere al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="c7950-278">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>
