---
title: Conferma dell'account e recupero della password in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare un'app ASP.NET Core con la conferma della posta elettronica e la reimpostazione della password.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 49d3d214fd64edc5b17df2df929ddc3c2af47ede
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829270"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="24a58-103">Conferma dell'account e recupero della password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24a58-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="24a58-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant)e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="24a58-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="24a58-105">Questa esercitazione illustra come compilare un'app ASP.NET Core con la conferma della posta elettronica e la reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="24a58-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="24a58-106">Questa esercitazione **non** è un argomento iniziale.</span><span class="sxs-lookup"><span data-stu-id="24a58-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="24a58-107">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="24a58-107">You should be familiar with:</span></span>

* [<span data-ttu-id="24a58-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24a58-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="24a58-109">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="24a58-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="24a58-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="24a58-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="24a58-111">Vedere [questo file PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) per la versione ASP.NET Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="24a58-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="24a58-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="24a58-112">Prerequisites</span></span>

[<span data-ttu-id="24a58-113">.NET Core 3,0 SDK o versione successiva</span><span class="sxs-lookup"><span data-stu-id="24a58-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="24a58-114">Creare e testare un'app Web con autenticazione</span><span class="sxs-lookup"><span data-stu-id="24a58-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="24a58-115">Eseguire i comandi seguenti per creare un'app Web con l'autenticazione di.</span><span class="sxs-lookup"><span data-stu-id="24a58-115">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="24a58-116">Eseguire l'app, selezionare il collegamento **Register** e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="24a58-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="24a58-117">Una volta effettuata la registrazione, si verrà reindirizzati alla pagina a `/Identity/Account/RegisterConfirmation` contenente un collegamento per simulare la conferma della posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="24a58-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="24a58-118">Selezionare il collegamento `Click here to confirm your account`.</span><span class="sxs-lookup"><span data-stu-id="24a58-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="24a58-119">Selezionare il collegamento di **accesso** e accedere con le stesse credenziali.</span><span class="sxs-lookup"><span data-stu-id="24a58-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="24a58-120">Selezionare il collegamento `Hello YourEmail@provider.com!`, che reindirizza all'`/Identity/Account/Manage/PersonalData` pagina.</span><span class="sxs-lookup"><span data-stu-id="24a58-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="24a58-121">Selezionare la scheda **Personal Data (dati personali** ) a sinistra e quindi fare clic su **Delete (Elimina**).</span><span class="sxs-lookup"><span data-stu-id="24a58-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="24a58-122">Configurare un provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-122">Configure an email provider</span></span>

<span data-ttu-id="24a58-123">In questa esercitazione viene usato [SendGrid](https://sendgrid.com) per inviare messaggi di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="24a58-124">Per inviare messaggi di posta elettronica sono necessari un account e una chiave di SendGrid.</span><span class="sxs-lookup"><span data-stu-id="24a58-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="24a58-125">È possibile usare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-125">You can use other email providers.</span></span> <span data-ttu-id="24a58-126">È consigliabile usare SendGrid o un altro servizio di posta elettronica per inviare messaggi di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="24a58-127">SMTP è difficile da proteggere e configurare correttamente.</span><span class="sxs-lookup"><span data-stu-id="24a58-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="24a58-128">Creare una classe per recuperare la chiave di posta elettronica sicura.</span><span class="sxs-lookup"><span data-stu-id="24a58-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="24a58-129">Per questo esempio, creare *Servizi/AuthMessageSenderOptions. cs*:</span><span class="sxs-lookup"><span data-stu-id="24a58-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="24a58-130">Configurare i segreti utente di SendGrid</span><span class="sxs-lookup"><span data-stu-id="24a58-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="24a58-131">Impostare il `SendGridUser` e `SendGridKey` con lo [strumento di gestione dei segreti](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="24a58-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="24a58-132">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="24a58-132">For example:</span></span>

```dotnetcli
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="24a58-133">In Windows, gestione Secret archivia le coppie chiave/valore in un file *Secrets. JSON* nella directory `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="24a58-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="24a58-134">Il contenuto del file *Secrets. JSON* non è crittografato.</span><span class="sxs-lookup"><span data-stu-id="24a58-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="24a58-135">Il markup seguente mostra il file *Secrets. JSON* .</span><span class="sxs-lookup"><span data-stu-id="24a58-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="24a58-136">Il valore `SendGridKey` è stato rimosso.</span><span class="sxs-lookup"><span data-stu-id="24a58-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="24a58-137">Per ulteriori informazioni, vedere il [modello](xref:fundamentals/configuration/options) e la [configurazione](xref:fundamentals/configuration/index)delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="24a58-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="24a58-138">Installare SendGrid</span><span class="sxs-lookup"><span data-stu-id="24a58-138">Install SendGrid</span></span>

<span data-ttu-id="24a58-139">Questa esercitazione illustra come aggiungere notifiche tramite posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare messaggi di posta elettronica usando SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="24a58-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="24a58-140">Installare il pacchetto NuGet `SendGrid`:</span><span class="sxs-lookup"><span data-stu-id="24a58-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24a58-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24a58-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="24a58-142">Dalla console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="24a58-142">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="24a58-143">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="24a58-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="24a58-144">Dalla console di immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="24a58-144">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="24a58-145">Per una registrazione gratuita per un account SendGrid gratuito, vedere [Introduzione a SendGrid](https://sendgrid.com/free/) .</span><span class="sxs-lookup"><span data-stu-id="24a58-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="24a58-146">Implementare IEmailSender</span><span class="sxs-lookup"><span data-stu-id="24a58-146">Implement IEmailSender</span></span>

<span data-ttu-id="24a58-147">Per implementare `IEmailSender`, creare *Services/EmailSender. cs* con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="24a58-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="24a58-148">Configurare l'avvio per il supporto della posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-148">Configure startup to support email</span></span>

<span data-ttu-id="24a58-149">Aggiungere il codice seguente al metodo `ConfigureServices` nel file *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="24a58-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="24a58-150">Aggiungere `EmailSender` come servizio temporaneo.</span><span class="sxs-lookup"><span data-stu-id="24a58-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="24a58-151">Registrare l'istanza di configurazione `AuthMessageSenderOptions`.</span><span class="sxs-lookup"><span data-stu-id="24a58-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="24a58-152">Registrare, confermare la posta elettronica e reimpostare la password</span><span class="sxs-lookup"><span data-stu-id="24a58-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="24a58-153">Eseguire l'app Web e testare la conferma dell'account e il flusso di recupero della password.</span><span class="sxs-lookup"><span data-stu-id="24a58-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="24a58-154">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="24a58-154">Run the app and register a new user</span></span>
* <span data-ttu-id="24a58-155">Controllare la posta elettronica per il collegamento di conferma dell'account.</span><span class="sxs-lookup"><span data-stu-id="24a58-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="24a58-156">Se non si riceve il messaggio di posta elettronica, vedere [debug](#debug) della posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="24a58-157">Fare clic sul collegamento per confermare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="24a58-158">Accedere con l'indirizzo di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="24a58-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="24a58-159">Uscire,</span><span class="sxs-lookup"><span data-stu-id="24a58-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="24a58-160">Testare la reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="24a58-160">Test password reset</span></span>

* <span data-ttu-id="24a58-161">Se è stato eseguito l'accesso, selezionare **Disconnetti**.</span><span class="sxs-lookup"><span data-stu-id="24a58-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="24a58-162">Selezionare il collegamento **Accedi** e selezionare il collegamento **password dimenticata?** .</span><span class="sxs-lookup"><span data-stu-id="24a58-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="24a58-163">Immettere il messaggio di posta elettronica usato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="24a58-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="24a58-164">Viene inviato un messaggio di posta elettronica con un collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="24a58-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="24a58-165">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="24a58-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="24a58-166">Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="24a58-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="24a58-167">Modificare il timeout dell'attività e del messaggio di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-167">Change email and activity timeout</span></span>

<span data-ttu-id="24a58-168">Il timeout di inattività predefinito è di 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="24a58-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="24a58-169">Il codice seguente imposta il timeout di inattività su 5 giorni:</span><span class="sxs-lookup"><span data-stu-id="24a58-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="24a58-170">Modifica tutte le durate dei token di protezione dati</span><span class="sxs-lookup"><span data-stu-id="24a58-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="24a58-171">Il codice seguente modifica il periodo di timeout di tutti i token di protezione dati in 3 ore:</span><span class="sxs-lookup"><span data-stu-id="24a58-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="24a58-172">I token utente predefiniti Identity (vedere [AspNetCore/src/Identity/Extensions. core/src/TokenOptions. cs](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) hanno un timeout di un [giorno](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="24a58-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="24a58-173">Modificare la durata del token di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-173">Change the email token lifespan</span></span>

<span data-ttu-id="24a58-174">La durata del token predefinita dei [token utente di identità](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) è [un giorno](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="24a58-174">The default token lifespan of [the Identity user tokens](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="24a58-175">Questa sezione illustra come modificare la durata del token di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="24a58-176">Aggiungere un [DataProtectorTokenProvider personalizzato\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) e <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="24a58-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="24a58-177">Aggiungere il provider personalizzato al contenitore del servizio:</span><span class="sxs-lookup"><span data-stu-id="24a58-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="24a58-178">Invia di nuovo la conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-178">Resend email confirmation</span></span>

<span data-ttu-id="24a58-179">Vedere [il problema in GitHub](https://github.com/dotnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="24a58-179">See [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="24a58-180">Posta elettronica di debug</span><span class="sxs-lookup"><span data-stu-id="24a58-180">Debug email</span></span>

<span data-ttu-id="24a58-181">Se non è possibile ottenere la posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="24a58-181">If you can't get email working:</span></span>

* <span data-ttu-id="24a58-182">Impostare un punto di interruzione in `EmailSender.Execute` per verificare che venga chiamato `SendGridClient.SendEmailAsync`.</span><span class="sxs-lookup"><span data-stu-id="24a58-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="24a58-183">Creare un' [app console per inviare messaggi di posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) usando codice simile per `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="24a58-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="24a58-184">Esaminare la pagina [attività posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) .</span><span class="sxs-lookup"><span data-stu-id="24a58-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="24a58-185">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="24a58-185">Check your spam folder.</span></span>
* <span data-ttu-id="24a58-186">Prova un altro alias di posta elettronica in un provider di posta elettronica diverso (Microsoft, Yahoo, Gmail e così via)</span><span class="sxs-lookup"><span data-stu-id="24a58-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="24a58-187">Provare a inviare a diversi account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="24a58-188">**Una procedura di sicurezza consigliata** consiste nel **non** usare i segreti di produzione per test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="24a58-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="24a58-189">Se si pubblica l'app in Azure, impostare i segreti SendGrid come impostazioni dell'applicazione nel portale dell'app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="24a58-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="24a58-190">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="24a58-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="24a58-191">Combinare account di accesso di social networking e locali</span><span class="sxs-lookup"><span data-stu-id="24a58-191">Combine social and local login accounts</span></span>

<span data-ttu-id="24a58-192">Per completare questa sezione, è necessario innanzitutto abilitare un provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="24a58-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="24a58-193">Vedere [l'autenticazione di Facebook, Google e del provider esterno](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="24a58-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="24a58-194">È possibile combinare account locali e di social networking facendo clic sul collegamento di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="24a58-195">Nella sequenza seguente "RickAndMSFT@gmail.com" viene innanzitutto creato come account di accesso locale; Tuttavia, è possibile creare prima l'account come account di accesso di social networking, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="24a58-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="24a58-197">Fare clic sul collegamento **Gestisci** .</span><span class="sxs-lookup"><span data-stu-id="24a58-197">Click on the **Manage** link.</span></span> <span data-ttu-id="24a58-198">Si notino gli account di accesso di social network (0) esterni associati a questo account.</span><span class="sxs-lookup"><span data-stu-id="24a58-198">Note the 0 external (social logins) associated with this account.</span></span>

![Gestisci visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="24a58-200">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste dell'app.</span><span class="sxs-lookup"><span data-stu-id="24a58-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="24a58-201">Nell'immagine seguente, Facebook è il provider di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="24a58-201">In the following image, Facebook is the external authentication provider:</span></span>

![Gestire gli account di accesso esterni visualizzare l'elenco di Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="24a58-203">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="24a58-203">The two accounts have been combined.</span></span> <span data-ttu-id="24a58-204">È possibile accedere con uno dei due account.</span><span class="sxs-lookup"><span data-stu-id="24a58-204">You are able to sign in with either account.</span></span> <span data-ttu-id="24a58-205">È possibile che gli utenti aggiungano gli account locali nel caso in cui il servizio di autenticazione per l'accesso di social networking sia inattivo o più probabilmente abbiano perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="24a58-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="24a58-206">Abilitare la conferma dell'account dopo che un sito ha utenti</span><span class="sxs-lookup"><span data-stu-id="24a58-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="24a58-207">L'abilitazione della conferma dell'account in un sito con utenti blocca tutti gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="24a58-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="24a58-208">Gli utenti esistenti sono bloccati perché i relativi account non sono confermati.</span><span class="sxs-lookup"><span data-stu-id="24a58-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="24a58-209">Per aggirare il blocco utente esistente, utilizzare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="24a58-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="24a58-210">Aggiornare il database per contrassegnare tutti gli utenti esistenti come confermati.</span><span class="sxs-lookup"><span data-stu-id="24a58-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="24a58-211">Verificare gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="24a58-211">Confirm existing users.</span></span> <span data-ttu-id="24a58-212">Ad esempio, invio in batch di messaggi di posta elettronica con collegamenti di conferma.</span><span class="sxs-lookup"><span data-stu-id="24a58-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="24a58-213">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="24a58-213">Prerequisites</span></span>

[<span data-ttu-id="24a58-214">.NET Core 2,2 SDK o versione successiva</span><span class="sxs-lookup"><span data-stu-id="24a58-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="24a58-215">Creare un'app Web e un'identità di impalcatura</span><span class="sxs-lookup"><span data-stu-id="24a58-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="24a58-216">Eseguire i comandi seguenti per creare un'app Web con l'autenticazione di.</span><span class="sxs-lookup"><span data-stu-id="24a58-216">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="24a58-217">Testare la registrazione di un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="24a58-217">Test new user registration</span></span>

<span data-ttu-id="24a58-218">Eseguire l'app, selezionare il collegamento **Register** e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="24a58-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="24a58-219">A questo punto, l'unica convalida sul messaggio di posta elettronica è con l'attributo [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) .</span><span class="sxs-lookup"><span data-stu-id="24a58-219">At this point, the only validation on the email is with the [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="24a58-220">Dopo aver inviato la registrazione, si è connessi all'app.</span><span class="sxs-lookup"><span data-stu-id="24a58-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="24a58-221">Più avanti nell'esercitazione, il codice viene aggiornato in modo che i nuovi utenti non possano accedere fino a quando non vengono convalidati i messaggi di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="24a58-222">Si noti che il campo `EmailConfirmed` della tabella è `False`.</span><span class="sxs-lookup"><span data-stu-id="24a58-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="24a58-223">Potrebbe essere necessario usare nuovamente questo messaggio di posta elettronica nel passaggio successivo quando l'app invia un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="24a58-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="24a58-224">Fare clic con il pulsante destro del mouse sulla riga e scegliere **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="24a58-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="24a58-225">L'eliminazione dell'alias di posta elettronica rende più semplice nei passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="24a58-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="24a58-226">Richiedi conferma posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-226">Require email confirmation</span></span>

<span data-ttu-id="24a58-227">È consigliabile confermare il messaggio di posta elettronica di una nuova registrazione utente.</span><span class="sxs-lookup"><span data-stu-id="24a58-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="24a58-228">La conferma tramite posta elettronica consente di verificare che non stiano eseguendo la rappresentazione di un altro utente, ovvero che non sono stati registrati con il messaggio di posta elettronica di un altro utente.</span><span class="sxs-lookup"><span data-stu-id="24a58-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="24a58-229">Si supponga di avere un forum di discussione e di voler impedire che "yli@example.com" si registri come "nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="24a58-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="24a58-230">Senza la conferma della posta elettronica, "nolivetto@contoso.com" potrebbe ricevere un messaggio di posta elettronica indesiderato dall'app.</span><span class="sxs-lookup"><span data-stu-id="24a58-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="24a58-231">Si supponga che l'utente sia stato accidentalmente registrato come "ylo@example.com" e non abbia notato l'errore di ortografia di "Yli".</span><span class="sxs-lookup"><span data-stu-id="24a58-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="24a58-232">Non saranno in grado di usare il ripristino della password perché l'app non ha la posta elettronica corretta.</span><span class="sxs-lookup"><span data-stu-id="24a58-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="24a58-233">La conferma tramite posta elettronica garantisce una protezione limitata dai bot.</span><span class="sxs-lookup"><span data-stu-id="24a58-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="24a58-234">La conferma tramite posta elettronica non fornisce protezione da utenti malintenzionati con molti account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="24a58-235">In genere si vuole impedire ai nuovi utenti di inviare dati al sito Web prima di avere un messaggio di posta elettronica confermato.</span><span class="sxs-lookup"><span data-stu-id="24a58-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="24a58-236">Aggiornare `Startup.ConfigureServices` per richiedere un messaggio di posta elettronica confermato:</span><span class="sxs-lookup"><span data-stu-id="24a58-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="24a58-237">`config.SignIn.RequireConfirmedEmail = true;` impedisce agli utenti registrati di accedere fino a quando non viene confermata la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="24a58-238">Configurare il provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-238">Configure email provider</span></span>

<span data-ttu-id="24a58-239">In questa esercitazione viene usato [SendGrid](https://sendgrid.com) per inviare messaggi di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="24a58-240">Per inviare messaggi di posta elettronica sono necessari un account e una chiave di SendGrid.</span><span class="sxs-lookup"><span data-stu-id="24a58-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="24a58-241">È possibile usare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-241">You can use other email providers.</span></span> <span data-ttu-id="24a58-242">ASP.NET Core 2. x include `System.Net.Mail`, che consente di inviare messaggi di posta elettronica dall'app.</span><span class="sxs-lookup"><span data-stu-id="24a58-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="24a58-243">È consigliabile usare SendGrid o un altro servizio di posta elettronica per inviare messaggi di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="24a58-244">SMTP è difficile da proteggere e configurare correttamente.</span><span class="sxs-lookup"><span data-stu-id="24a58-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="24a58-245">Creare una classe per recuperare la chiave di posta elettronica sicura.</span><span class="sxs-lookup"><span data-stu-id="24a58-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="24a58-246">Per questo esempio, creare *Servizi/AuthMessageSenderOptions. cs*:</span><span class="sxs-lookup"><span data-stu-id="24a58-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="24a58-247">Configurare i segreti utente di SendGrid</span><span class="sxs-lookup"><span data-stu-id="24a58-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="24a58-248">Impostare il `SendGridUser` e `SendGridKey` con lo [strumento di gestione dei segreti](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="24a58-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="24a58-249">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="24a58-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="24a58-250">In Windows, gestione Secret archivia le coppie chiave/valore in un file *Secrets. JSON* nella directory `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="24a58-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="24a58-251">Il contenuto del file *Secrets. JSON* non è crittografato.</span><span class="sxs-lookup"><span data-stu-id="24a58-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="24a58-252">Il markup seguente mostra il file *Secrets. JSON* .</span><span class="sxs-lookup"><span data-stu-id="24a58-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="24a58-253">Il valore `SendGridKey` è stato rimosso.</span><span class="sxs-lookup"><span data-stu-id="24a58-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="24a58-254">Per ulteriori informazioni, vedere il [modello](xref:fundamentals/configuration/options) e la [configurazione](xref:fundamentals/configuration/index)delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="24a58-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="24a58-255">Installare SendGrid</span><span class="sxs-lookup"><span data-stu-id="24a58-255">Install SendGrid</span></span>

<span data-ttu-id="24a58-256">Questa esercitazione illustra come aggiungere notifiche tramite posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare messaggi di posta elettronica usando SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="24a58-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="24a58-257">Installare il pacchetto NuGet `SendGrid`:</span><span class="sxs-lookup"><span data-stu-id="24a58-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24a58-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24a58-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="24a58-259">Dalla console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="24a58-259">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="24a58-260">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="24a58-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="24a58-261">Dalla console di immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="24a58-261">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="24a58-262">Per una registrazione gratuita per un account SendGrid gratuito, vedere [Introduzione a SendGrid](https://sendgrid.com/free/) .</span><span class="sxs-lookup"><span data-stu-id="24a58-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="24a58-263">Implementare IEmailSender</span><span class="sxs-lookup"><span data-stu-id="24a58-263">Implement IEmailSender</span></span>

<span data-ttu-id="24a58-264">Per implementare `IEmailSender`, creare *Services/EmailSender. cs* con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="24a58-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="24a58-265">Configurare l'avvio per il supporto della posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-265">Configure startup to support email</span></span>

<span data-ttu-id="24a58-266">Aggiungere il codice seguente al metodo `ConfigureServices` nel file *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="24a58-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="24a58-267">Aggiungere `EmailSender` come servizio temporaneo.</span><span class="sxs-lookup"><span data-stu-id="24a58-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="24a58-268">Registrare l'istanza di configurazione `AuthMessageSenderOptions`.</span><span class="sxs-lookup"><span data-stu-id="24a58-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="24a58-269">Abilitare la conferma dell'account e il recupero della password</span><span class="sxs-lookup"><span data-stu-id="24a58-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="24a58-270">Il modello include il codice per la conferma dell'account e il recupero della password.</span><span class="sxs-lookup"><span data-stu-id="24a58-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="24a58-271">Trovare il metodo `OnPostAsync` in *areas/Identity/Pages/account/Register. cshtml. cs*.</span><span class="sxs-lookup"><span data-stu-id="24a58-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="24a58-272">Impedire l'accesso automatico degli utenti appena registrati impostando come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="24a58-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="24a58-273">Il metodo completo viene visualizzato con la riga modificata evidenziata:</span><span class="sxs-lookup"><span data-stu-id="24a58-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="24a58-274">Registrare, confermare la posta elettronica e reimpostare la password</span><span class="sxs-lookup"><span data-stu-id="24a58-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="24a58-275">Eseguire l'app Web e testare la conferma dell'account e il flusso di recupero della password.</span><span class="sxs-lookup"><span data-stu-id="24a58-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="24a58-276">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="24a58-276">Run the app and register a new user</span></span>
* <span data-ttu-id="24a58-277">Controllare la posta elettronica per il collegamento di conferma dell'account.</span><span class="sxs-lookup"><span data-stu-id="24a58-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="24a58-278">Se non si riceve il messaggio di posta elettronica, vedere [debug](#debug) della posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="24a58-279">Fare clic sul collegamento per confermare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="24a58-280">Accedere con l'indirizzo di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="24a58-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="24a58-281">Uscire,</span><span class="sxs-lookup"><span data-stu-id="24a58-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="24a58-282">Visualizzare la pagina Gestisci</span><span class="sxs-lookup"><span data-stu-id="24a58-282">View the manage page</span></span>

<span data-ttu-id="24a58-283">Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="24a58-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="24a58-284">La pagina Gestisci viene visualizzata con la scheda **profilo** selezionata.</span><span class="sxs-lookup"><span data-stu-id="24a58-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="24a58-285">Viene visualizzata una casella di controllo che indica che il messaggio di **posta elettronica è** stato confermato.</span><span class="sxs-lookup"><span data-stu-id="24a58-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="24a58-286">Testare la reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="24a58-286">Test password reset</span></span>

* <span data-ttu-id="24a58-287">Se è stato eseguito l'accesso, selezionare **Disconnetti**.</span><span class="sxs-lookup"><span data-stu-id="24a58-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="24a58-288">Selezionare il collegamento **Accedi** e selezionare il collegamento **password dimenticata?** .</span><span class="sxs-lookup"><span data-stu-id="24a58-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="24a58-289">Immettere il messaggio di posta elettronica usato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="24a58-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="24a58-290">Viene inviato un messaggio di posta elettronica con un collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="24a58-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="24a58-291">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="24a58-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="24a58-292">Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="24a58-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="24a58-293">Modificare il timeout dell'attività e del messaggio di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-293">Change email and activity timeout</span></span>

<span data-ttu-id="24a58-294">Il timeout di inattività predefinito è di 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="24a58-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="24a58-295">Il codice seguente imposta il timeout di inattività su 5 giorni:</span><span class="sxs-lookup"><span data-stu-id="24a58-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="24a58-296">Modifica tutte le durate dei token di protezione dati</span><span class="sxs-lookup"><span data-stu-id="24a58-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="24a58-297">Il codice seguente modifica il periodo di timeout di tutti i token di protezione dati in 3 ore:</span><span class="sxs-lookup"><span data-stu-id="24a58-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="24a58-298">I token utente predefiniti Identity (vedere [AspNetCore/src/Identity/Extensions. core/src/TokenOptions. cs](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) hanno un timeout di un [giorno](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="24a58-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="24a58-299">Modificare la durata del token di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-299">Change the email token lifespan</span></span>

<span data-ttu-id="24a58-300">La durata del token predefinita dei [token utente di identità](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) è [un giorno](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="24a58-300">The default token lifespan of [the Identity user tokens](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/dotnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="24a58-301">Questa sezione illustra come modificare la durata del token di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="24a58-302">Aggiungere un [DataProtectorTokenProvider personalizzato\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) e <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="24a58-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="24a58-303">Aggiungere il provider personalizzato al contenitore del servizio:</span><span class="sxs-lookup"><span data-stu-id="24a58-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="24a58-304">Invia di nuovo la conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="24a58-304">Resend email confirmation</span></span>

<span data-ttu-id="24a58-305">Vedere [il problema in GitHub](https://github.com/dotnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="24a58-305">See [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="24a58-306">Posta elettronica di debug</span><span class="sxs-lookup"><span data-stu-id="24a58-306">Debug email</span></span>

<span data-ttu-id="24a58-307">Se non è possibile ottenere la posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="24a58-307">If you can't get email working:</span></span>

* <span data-ttu-id="24a58-308">Impostare un punto di interruzione in `EmailSender.Execute` per verificare che venga chiamato `SendGridClient.SendEmailAsync`.</span><span class="sxs-lookup"><span data-stu-id="24a58-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="24a58-309">Creare un' [app console per inviare messaggi di posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) usando codice simile per `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="24a58-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="24a58-310">Esaminare la pagina [attività posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) .</span><span class="sxs-lookup"><span data-stu-id="24a58-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="24a58-311">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="24a58-311">Check your spam folder.</span></span>
* <span data-ttu-id="24a58-312">Prova un altro alias di posta elettronica in un provider di posta elettronica diverso (Microsoft, Yahoo, Gmail e così via)</span><span class="sxs-lookup"><span data-stu-id="24a58-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="24a58-313">Provare a inviare a diversi account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="24a58-314">**Una procedura di sicurezza consigliata** consiste nel **non** usare i segreti di produzione per test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="24a58-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="24a58-315">Se si pubblica l'app in Azure, è possibile impostare SendGrid Secrets come impostazioni dell'applicazione nel portale dell'app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="24a58-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="24a58-316">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="24a58-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="24a58-317">Combinare account di accesso di social networking e locali</span><span class="sxs-lookup"><span data-stu-id="24a58-317">Combine social and local login accounts</span></span>

<span data-ttu-id="24a58-318">Per completare questa sezione, è necessario innanzitutto abilitare un provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="24a58-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="24a58-319">Vedere [l'autenticazione di Facebook, Google e del provider esterno](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="24a58-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="24a58-320">È possibile combinare account locali e di social networking facendo clic sul collegamento di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="24a58-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="24a58-321">Nella sequenza seguente "RickAndMSFT@gmail.com" viene innanzitutto creato come account di accesso locale; Tuttavia, è possibile creare prima l'account come account di accesso di social networking, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="24a58-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="24a58-323">Fare clic sul collegamento **Gestisci** .</span><span class="sxs-lookup"><span data-stu-id="24a58-323">Click on the **Manage** link.</span></span> <span data-ttu-id="24a58-324">Si notino gli account di accesso di social network (0) esterni associati a questo account.</span><span class="sxs-lookup"><span data-stu-id="24a58-324">Note the 0 external (social logins) associated with this account.</span></span>

![Gestisci visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="24a58-326">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste dell'app.</span><span class="sxs-lookup"><span data-stu-id="24a58-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="24a58-327">Nell'immagine seguente, Facebook è il provider di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="24a58-327">In the following image, Facebook is the external authentication provider:</span></span>

![Gestire gli account di accesso esterni visualizzare l'elenco di Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="24a58-329">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="24a58-329">The two accounts have been combined.</span></span> <span data-ttu-id="24a58-330">È possibile accedere con uno dei due account.</span><span class="sxs-lookup"><span data-stu-id="24a58-330">You are able to sign in with either account.</span></span> <span data-ttu-id="24a58-331">È possibile che gli utenti aggiungano gli account locali nel caso in cui il servizio di autenticazione per l'accesso di social networking sia inattivo o più probabilmente abbiano perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="24a58-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="24a58-332">Abilitare la conferma dell'account dopo che un sito ha utenti</span><span class="sxs-lookup"><span data-stu-id="24a58-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="24a58-333">L'abilitazione della conferma dell'account in un sito con utenti blocca tutti gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="24a58-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="24a58-334">Gli utenti esistenti sono bloccati perché i relativi account non sono confermati.</span><span class="sxs-lookup"><span data-stu-id="24a58-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="24a58-335">Per aggirare il blocco utente esistente, utilizzare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="24a58-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="24a58-336">Aggiornare il database per contrassegnare tutti gli utenti esistenti come confermati.</span><span class="sxs-lookup"><span data-stu-id="24a58-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="24a58-337">Verificare gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="24a58-337">Confirm existing users.</span></span> <span data-ttu-id="24a58-338">Ad esempio, invio in batch di messaggi di posta elettronica con collegamenti di conferma.</span><span class="sxs-lookup"><span data-stu-id="24a58-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
