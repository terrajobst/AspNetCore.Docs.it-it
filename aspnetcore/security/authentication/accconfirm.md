---
title: Conferma account e recupero della password in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 802ba446af04df6a35ac73187ad693b8ec80c654
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814844"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="87e21-103">Conferma account e recupero della password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="87e21-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="87e21-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="87e21-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="87e21-105">Questa esercitazione illustra come compilare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="87e21-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="87e21-106">Questa esercitazione viene **non** un argomento di inizio.</span><span class="sxs-lookup"><span data-stu-id="87e21-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="87e21-107">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="87e21-107">You should be familiar with:</span></span>

* [<span data-ttu-id="87e21-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="87e21-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="87e21-109">autenticazione</span><span class="sxs-lookup"><span data-stu-id="87e21-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="87e21-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="87e21-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="87e21-111">Visualizzare [questo file PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) per la versione di ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="87e21-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="87e21-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="87e21-112">Prerequisites</span></span>

[<span data-ttu-id="87e21-113">.NET core SDK 3.0 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="87e21-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="87e21-114">Creare e testare un'app web con l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="87e21-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="87e21-115">Eseguire i comandi seguenti per creare un'app web con l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="87e21-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="87e21-116">Eseguire l'app, selezionare la **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="87e21-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="87e21-117">Una volta registrato, si verrà reindirizzati al a `/Identity/Account/RegisterConfirmation` pagina che contiene un collegamento per simulare conferma tramite posta elettronica:</span><span class="sxs-lookup"><span data-stu-id="87e21-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="87e21-118">Selezionare il `Click here to confirm your account` collegamento.</span><span class="sxs-lookup"><span data-stu-id="87e21-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="87e21-119">Selezionare il **account di accesso** collegamento e l'accesso con le stesse credenziali.</span><span class="sxs-lookup"><span data-stu-id="87e21-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="87e21-120">Selezionare il `Hello YourEmail@provider.com!` collegamento che reindirizza l'utente al `/Identity/Account/Manage/PersonalData` pagina.</span><span class="sxs-lookup"><span data-stu-id="87e21-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="87e21-121">Selezionare il **i dati personali** scheda a sinistra e quindi selezionare **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="87e21-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="87e21-122">Configurare un provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-122">Configure an email provider</span></span>

<span data-ttu-id="87e21-123">In questa esercitazione [SendGrid](https://sendgrid.com) viene usato per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="87e21-124">È necessario un account SendGrid e una chiave per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="87e21-125">È possibile usare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-125">You can use other email providers.</span></span> <span data-ttu-id="87e21-126">È consigliabile che usare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="87e21-127">SMTP sono difficili da proteggere e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="87e21-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="87e21-128">Creare una classe per recuperare la chiave di protezione della posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="87e21-129">Per questo esempio, creare *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="87e21-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="87e21-130">Configurare i segreti utente di SendGrid</span><span class="sxs-lookup"><span data-stu-id="87e21-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="87e21-131">Impostare il `SendGridUser` e `SendGridKey` con il [strumento secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="87e21-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="87e21-132">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="87e21-132">For example:</span></span>

```console
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="87e21-133">In Windows, Secret Manager archivia le coppie di chiavi/valore in una *Secrets* del file nei `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="87e21-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="87e21-134">Il contenuto del *Secrets* file non vengono crittografati.</span><span class="sxs-lookup"><span data-stu-id="87e21-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="87e21-135">Il markup seguente mostra le *Secrets* file.</span><span class="sxs-lookup"><span data-stu-id="87e21-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="87e21-136">Il `SendGridKey` valore è stato rimosso.</span><span class="sxs-lookup"><span data-stu-id="87e21-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="87e21-137">Per altre informazioni, vedere la [modello di opzioni](xref:fundamentals/configuration/options) e [configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="87e21-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="87e21-138">Installare SendGrid</span><span class="sxs-lookup"><span data-stu-id="87e21-138">Install SendGrid</span></span>

<span data-ttu-id="87e21-139">Questa esercitazione illustra come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="87e21-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="87e21-140">Installare il `SendGrid` pacchetto NuGet:</span><span class="sxs-lookup"><span data-stu-id="87e21-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87e21-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87e21-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="87e21-142">Dalla Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="87e21-142">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="87e21-143">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="87e21-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="87e21-144">Dalla console, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="87e21-144">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="87e21-145">Visualizzare [inizia gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="87e21-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="87e21-146">Implementare IEmailSender</span><span class="sxs-lookup"><span data-stu-id="87e21-146">Implement IEmailSender</span></span>

<span data-ttu-id="87e21-147">L'implementazione `IEmailSender`, creare *Services/EmailSender.cs* con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="87e21-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="87e21-148">Configurare l'avvio per il supporto tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-148">Configure startup to support email</span></span>

<span data-ttu-id="87e21-149">Aggiungere il codice seguente per il `ConfigureServices` metodo nella *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="87e21-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="87e21-150">Aggiungere `EmailSender` come un temporaneo del servizio.</span><span class="sxs-lookup"><span data-stu-id="87e21-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="87e21-151">Registrare il `AuthMessageSenderOptions` istanza di configurazione.</span><span class="sxs-lookup"><span data-stu-id="87e21-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="87e21-152">Registrare, messaggio di posta elettronica di conferma e reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="87e21-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="87e21-153">Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.</span><span class="sxs-lookup"><span data-stu-id="87e21-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="87e21-154">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="87e21-154">Run the app and register a new user</span></span>
* <span data-ttu-id="87e21-155">Controllare la posta elettronica per il collegamento di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="87e21-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="87e21-156">Visualizzare [eseguire il Debug tramite posta elettronica](#debug) se non si riceve il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="87e21-157">Fare clic sul collegamento per confermare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="87e21-158">Accedere con l'indirizzo di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="87e21-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="87e21-159">Uscire,</span><span class="sxs-lookup"><span data-stu-id="87e21-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="87e21-160">Reimpostazione della password di test</span><span class="sxs-lookup"><span data-stu-id="87e21-160">Test password reset</span></span>

* <span data-ttu-id="87e21-161">Se è connessi, selezionare **Logout**.</span><span class="sxs-lookup"><span data-stu-id="87e21-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="87e21-162">Selezionare il **accedere** collegamento e selezionare il **password dimenticata?** collegamento.</span><span class="sxs-lookup"><span data-stu-id="87e21-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="87e21-163">Immettere l'indirizzo di posta elettronica usato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="87e21-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="87e21-164">Viene inviato un messaggio di posta elettronica con un collegamento di reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="87e21-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="87e21-165">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="87e21-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="87e21-166">Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="87e21-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="87e21-167">Modifica timeout messaggio di posta elettronica e l'attività</span><span class="sxs-lookup"><span data-stu-id="87e21-167">Change email and activity timeout</span></span>

<span data-ttu-id="87e21-168">Il timeout di inattività predefinito è 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="87e21-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="87e21-169">Il codice seguente imposta il timeout di inattività da 5 giorni:</span><span class="sxs-lookup"><span data-stu-id="87e21-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="87e21-170">Modificare tutti i dati protection token durata di validità superiore</span><span class="sxs-lookup"><span data-stu-id="87e21-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="87e21-171">Il codice seguente modifica tutti periodo di timeout i token di protezione dati a 3 ore:</span><span class="sxs-lookup"><span data-stu-id="87e21-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="87e21-172">Incorporato in token dell'identità utente (vedere [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) hanno un [timeout di un giorno](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="87e21-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="87e21-173">Modificare la durata di token di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-173">Change the email token lifespan</span></span>

<span data-ttu-id="87e21-174">Il ciclo di vita di token predefinito dei [i token utente di identità](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) viene [giorno](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="87e21-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="87e21-175">In questa sezione viene illustrato come modificare la durata di token di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="87e21-176">Aggiungere una classe personalizzata [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) e <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="87e21-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="87e21-177">Aggiungere il provider personalizzato al contenitore del servizio:</span><span class="sxs-lookup"><span data-stu-id="87e21-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="87e21-178">Inviare di nuovo conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-178">Resend email confirmation</span></span>

<span data-ttu-id="87e21-179">Visualizzare [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="87e21-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="87e21-180">Eseguire il debug tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-180">Debug email</span></span>

<span data-ttu-id="87e21-181">Se non è possibile ottenere l'indirizzo di posta elettronica funzionante:</span><span class="sxs-lookup"><span data-stu-id="87e21-181">If you can't get email working:</span></span>

* <span data-ttu-id="87e21-182">Impostare un punto di interruzione `EmailSender.Execute` per verificare `SendGridClient.SendEmailAsync` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="87e21-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="87e21-183">Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) usando codice simile a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="87e21-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="87e21-184">Rivedere le [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.</span><span class="sxs-lookup"><span data-stu-id="87e21-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="87e21-185">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="87e21-185">Check your spam folder.</span></span>
* <span data-ttu-id="87e21-186">Provare un altro alias di posta elettronica in un provider di posta elettronica diverso (Microsoft, Yahoo, Gmail e così via)</span><span class="sxs-lookup"><span data-stu-id="87e21-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="87e21-187">Provare a inviare agli account di posta elettronica diverso.</span><span class="sxs-lookup"><span data-stu-id="87e21-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="87e21-188">**Una procedura consigliata** consiste nel **non** usare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="87e21-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="87e21-189">Se si pubblica l'app in Azure, impostare i segreti di SendGrid come impostazioni dell'applicazione nel portale di App Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="87e21-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="87e21-190">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="87e21-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="87e21-191">Combinare gli account di accesso social e locali</span><span class="sxs-lookup"><span data-stu-id="87e21-191">Combine social and local login accounts</span></span>

<span data-ttu-id="87e21-192">Per completare questa sezione, è innanzitutto necessario abilitare un provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="87e21-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="87e21-193">Visualizzare [Facebook, Google e l'autenticazione esterna provider](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="87e21-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="87e21-194">È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="87e21-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="87e21-195">Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso basati su social network, prima di tutto, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="87e21-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="87e21-197">Fare clic sui **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="87e21-197">Click on the **Manage** link.</span></span> <span data-ttu-id="87e21-198">Si noti 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="87e21-198">Note the 0 external (social logins) associated with this account.</span></span>

![Gestione visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="87e21-200">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="87e21-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="87e21-201">Nell'immagine seguente, Facebook è il provider di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="87e21-201">In the following image, Facebook is the external authentication provider:</span></span>

![Gestire la visualizzazione di account di accesso esterni listato Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="87e21-203">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="87e21-203">The two accounts have been combined.</span></span> <span data-ttu-id="87e21-204">Si è in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="87e21-204">You are able to sign in with either account.</span></span> <span data-ttu-id="87e21-205">È possibile che gli utenti per aggiungere gli account locali nel caso in cui il servizio di autenticazione account di accesso basati su social network è inattivo oppure più probabile che abbia perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="87e21-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="87e21-206">Abilitare la conferma dell'account dopo un sito ha utenti</span><span class="sxs-lookup"><span data-stu-id="87e21-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="87e21-207">Abilitare la conferma dell'account in un sito con gli utenti per bloccare tutti gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="87e21-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="87e21-208">Gli utenti esistenti sono bloccati perché i propri account non confermato.</span><span class="sxs-lookup"><span data-stu-id="87e21-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="87e21-209">Per evitare il blocco degli utente esistente, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="87e21-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="87e21-210">Aggiornare il database per contrassegnare tutti gli utenti esistenti come viene confermata.</span><span class="sxs-lookup"><span data-stu-id="87e21-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="87e21-211">Verificare che gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="87e21-211">Confirm existing users.</span></span> <span data-ttu-id="87e21-212">Ad esempio, batch-trasmissione messaggi di posta elettronica con collegamenti di conferma.</span><span class="sxs-lookup"><span data-stu-id="87e21-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="87e21-213">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="87e21-213">Prerequisites</span></span>

[<span data-ttu-id="87e21-214">.NET core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="87e21-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="87e21-215">Creare un'app web ed eseguire lo scaffolding di identità</span><span class="sxs-lookup"><span data-stu-id="87e21-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="87e21-216">Eseguire i comandi seguenti per creare un'app web con l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="87e21-216">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="87e21-217">Registrazione di nuovi utenti di test</span><span class="sxs-lookup"><span data-stu-id="87e21-217">Test new user registration</span></span>

<span data-ttu-id="87e21-218">Eseguire l'app, selezionare la **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="87e21-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="87e21-219">A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo.</span><span class="sxs-lookup"><span data-stu-id="87e21-219">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="87e21-220">Dopo aver inviato la registrazione, si è connessi all'app.</span><span class="sxs-lookup"><span data-stu-id="87e21-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="87e21-221">Più avanti nell'esercitazione, il codice venga aggiornato in modo che i nuovi utenti non riesci ad accedere fino a quando non viene convalidata la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="87e21-222">Si noti la tabella `EmailConfirmed` campo è `False`.</span><span class="sxs-lookup"><span data-stu-id="87e21-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="87e21-223">Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="87e21-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="87e21-224">Fare doppio clic sulla riga e selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="87e21-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="87e21-225">L'eliminazione di alias di posta elettronica rende più semplice nella procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="87e21-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="87e21-226">Richiedere conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-226">Require email confirmation</span></span>

<span data-ttu-id="87e21-227">È consigliabile verificare che il messaggio di posta elettronica di una nuova registrazione utente.</span><span class="sxs-lookup"><span data-stu-id="87e21-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="87e21-228">Inviare tramite posta elettronica di conferma consente di verificare che non sta rappresentando qualcun altro (vale a dire non hanno registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="87e21-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="87e21-229">Si supponga che si ha un forum di discussione e si desidera evitare che "yli@example.com"da registrare come"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="87e21-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="87e21-230">Senza conferma tramite posta elettronica, "nolivetto@contoso.com" può ricevere e-mail indesiderate dalla propria app.</span><span class="sxs-lookup"><span data-stu-id="87e21-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="87e21-231">Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non l'aveste notato l'errore di ortografia di "yli".</span><span class="sxs-lookup"><span data-stu-id="87e21-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="87e21-232">Essi non sarebbe in grado di utilizzare il recupero della password perché l'app non dispone di posta elettronica corretta.</span><span class="sxs-lookup"><span data-stu-id="87e21-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="87e21-233">Conferma tramite posta elettronica fornisce una protezione limitata dal BOT.</span><span class="sxs-lookup"><span data-stu-id="87e21-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="87e21-234">Conferma tramite posta elettronica non fornisce protezione da utenti malintenzionati con numero di account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="87e21-235">In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che abbiano un messaggio di posta elettronica confermato.</span><span class="sxs-lookup"><span data-stu-id="87e21-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="87e21-236">Aggiornamento `Startup.ConfigureServices` per richiedere un indirizzo di posta elettronica confermato:</span><span class="sxs-lookup"><span data-stu-id="87e21-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="87e21-237">`config.SignIn.RequireConfirmedEmail = true;` impedisce l'accesso fino a quando non viene confermata la posta elettronica agli utenti registrati.</span><span class="sxs-lookup"><span data-stu-id="87e21-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="87e21-238">Configurare il provider di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-238">Configure email provider</span></span>

<span data-ttu-id="87e21-239">In questa esercitazione [SendGrid](https://sendgrid.com) viene usato per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="87e21-240">È necessario un account SendGrid e una chiave per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="87e21-241">È possibile usare altri provider di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-241">You can use other email providers.</span></span> <span data-ttu-id="87e21-242">ASP.NET Core 2.x include `System.Net.Mail`, pertanto è possibile inviare tramite posta elettronica dall'app.</span><span class="sxs-lookup"><span data-stu-id="87e21-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="87e21-243">È consigliabile che usare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="87e21-244">SMTP sono difficili da proteggere e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="87e21-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="87e21-245">Creare una classe per recuperare la chiave di protezione della posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="87e21-246">Per questo esempio, creare *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="87e21-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="87e21-247">Configurare i segreti utente di SendGrid</span><span class="sxs-lookup"><span data-stu-id="87e21-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="87e21-248">Impostare il `SendGridUser` e `SendGridKey` con il [strumento secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="87e21-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="87e21-249">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="87e21-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="87e21-250">In Windows, Secret Manager archivia le coppie di chiavi/valore in una *Secrets* del file nei `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="87e21-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="87e21-251">Il contenuto del *Secrets* file non vengono crittografati.</span><span class="sxs-lookup"><span data-stu-id="87e21-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="87e21-252">Il markup seguente mostra le *Secrets* file.</span><span class="sxs-lookup"><span data-stu-id="87e21-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="87e21-253">Il `SendGridKey` valore è stato rimosso.</span><span class="sxs-lookup"><span data-stu-id="87e21-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="87e21-254">Per altre informazioni, vedere la [modello di opzioni](xref:fundamentals/configuration/options) e [configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="87e21-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="87e21-255">Installare SendGrid</span><span class="sxs-lookup"><span data-stu-id="87e21-255">Install SendGrid</span></span>

<span data-ttu-id="87e21-256">Questa esercitazione illustra come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.</span><span class="sxs-lookup"><span data-stu-id="87e21-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="87e21-257">Installare il `SendGrid` pacchetto NuGet:</span><span class="sxs-lookup"><span data-stu-id="87e21-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87e21-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87e21-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="87e21-259">Dalla Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="87e21-259">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="87e21-260">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="87e21-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="87e21-261">Dalla console, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="87e21-261">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="87e21-262">Visualizzare [inizia gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="87e21-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="87e21-263">Implementare IEmailSender</span><span class="sxs-lookup"><span data-stu-id="87e21-263">Implement IEmailSender</span></span>

<span data-ttu-id="87e21-264">L'implementazione `IEmailSender`, creare *Services/EmailSender.cs* con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="87e21-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="87e21-265">Configurare l'avvio per il supporto tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-265">Configure startup to support email</span></span>

<span data-ttu-id="87e21-266">Aggiungere il codice seguente per il `ConfigureServices` metodo nella *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="87e21-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="87e21-267">Aggiungere `EmailSender` come un temporaneo del servizio.</span><span class="sxs-lookup"><span data-stu-id="87e21-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="87e21-268">Registrare il `AuthMessageSenderOptions` istanza di configurazione.</span><span class="sxs-lookup"><span data-stu-id="87e21-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="87e21-269">Abilitare il ripristino di conferma e la password account</span><span class="sxs-lookup"><span data-stu-id="87e21-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="87e21-270">Il modello presenta il codice per il ripristino di conferma e la password di account.</span><span class="sxs-lookup"><span data-stu-id="87e21-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="87e21-271">Trovare il `OnPostAsync` nel metodo *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="87e21-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="87e21-272">Impedire agli utenti appena registrati di riconnessi automaticamente impostando come commento la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="87e21-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="87e21-273">Il metodo completo viene visualizzato con la riga modificata evidenziata:</span><span class="sxs-lookup"><span data-stu-id="87e21-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="87e21-274">Registrare, messaggio di posta elettronica di conferma e reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="87e21-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="87e21-275">Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.</span><span class="sxs-lookup"><span data-stu-id="87e21-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="87e21-276">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="87e21-276">Run the app and register a new user</span></span>
* <span data-ttu-id="87e21-277">Controllare la posta elettronica per il collegamento di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="87e21-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="87e21-278">Visualizzare [eseguire il Debug tramite posta elettronica](#debug) se non si riceve il messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="87e21-279">Fare clic sul collegamento per confermare l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="87e21-280">Accedere con l'indirizzo di posta elettronica e la password.</span><span class="sxs-lookup"><span data-stu-id="87e21-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="87e21-281">Uscire,</span><span class="sxs-lookup"><span data-stu-id="87e21-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="87e21-282">Visualizzare la pagina di gestione</span><span class="sxs-lookup"><span data-stu-id="87e21-282">View the manage page</span></span>

<span data-ttu-id="87e21-283">Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="87e21-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="87e21-284">La pagina di gestione viene visualizzata con il **profilo** selezionato della scheda.</span><span class="sxs-lookup"><span data-stu-id="87e21-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="87e21-285">Il **messaggio di posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.</span><span class="sxs-lookup"><span data-stu-id="87e21-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="87e21-286">Reimpostazione della password di test</span><span class="sxs-lookup"><span data-stu-id="87e21-286">Test password reset</span></span>

* <span data-ttu-id="87e21-287">Se è connessi, selezionare **Logout**.</span><span class="sxs-lookup"><span data-stu-id="87e21-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="87e21-288">Selezionare il **accedere** collegamento e selezionare il **password dimenticata?** collegamento.</span><span class="sxs-lookup"><span data-stu-id="87e21-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="87e21-289">Immettere l'indirizzo di posta elettronica usato per registrare l'account.</span><span class="sxs-lookup"><span data-stu-id="87e21-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="87e21-290">Viene inviato un messaggio di posta elettronica con un collegamento di reimpostazione della password.</span><span class="sxs-lookup"><span data-stu-id="87e21-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="87e21-291">Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="87e21-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="87e21-292">Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.</span><span class="sxs-lookup"><span data-stu-id="87e21-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="87e21-293">Modifica timeout messaggio di posta elettronica e l'attività</span><span class="sxs-lookup"><span data-stu-id="87e21-293">Change email and activity timeout</span></span>

<span data-ttu-id="87e21-294">Il timeout di inattività predefinito è 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="87e21-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="87e21-295">Il codice seguente imposta il timeout di inattività da 5 giorni:</span><span class="sxs-lookup"><span data-stu-id="87e21-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="87e21-296">Modificare tutti i dati protection token durata di validità superiore</span><span class="sxs-lookup"><span data-stu-id="87e21-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="87e21-297">Il codice seguente modifica tutti periodo di timeout i token di protezione dati a 3 ore:</span><span class="sxs-lookup"><span data-stu-id="87e21-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="87e21-298">Incorporato in token dell'identità utente (vedere [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) hanno un [timeout di un giorno](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="87e21-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="87e21-299">Modificare la durata di token di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-299">Change the email token lifespan</span></span>

<span data-ttu-id="87e21-300">Il ciclo di vita di token predefinito dei [i token utente di identità](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) viene [giorno](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="87e21-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="87e21-301">In questa sezione viene illustrato come modificare la durata di token di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="87e21-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="87e21-302">Aggiungere una classe personalizzata [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) e <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="87e21-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="87e21-303">Aggiungere il provider personalizzato al contenitore del servizio:</span><span class="sxs-lookup"><span data-stu-id="87e21-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="87e21-304">Inviare di nuovo conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-304">Resend email confirmation</span></span>

<span data-ttu-id="87e21-305">Visualizzare [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="87e21-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="87e21-306">Eseguire il debug tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="87e21-306">Debug email</span></span>

<span data-ttu-id="87e21-307">Se non è possibile ottenere l'indirizzo di posta elettronica funzionante:</span><span class="sxs-lookup"><span data-stu-id="87e21-307">If you can't get email working:</span></span>

* <span data-ttu-id="87e21-308">Impostare un punto di interruzione `EmailSender.Execute` per verificare `SendGridClient.SendEmailAsync` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="87e21-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="87e21-309">Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) usando codice simile a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="87e21-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="87e21-310">Rivedere le [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.</span><span class="sxs-lookup"><span data-stu-id="87e21-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="87e21-311">Controllare la cartella della posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="87e21-311">Check your spam folder.</span></span>
* <span data-ttu-id="87e21-312">Provare un altro alias di posta elettronica in un provider di posta elettronica diverso (Microsoft, Yahoo, Gmail e così via)</span><span class="sxs-lookup"><span data-stu-id="87e21-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="87e21-313">Provare a inviare agli account di posta elettronica diverso.</span><span class="sxs-lookup"><span data-stu-id="87e21-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="87e21-314">**Una procedura consigliata** consiste nel **non** usare i segreti di produzione nel test e sviluppo.</span><span class="sxs-lookup"><span data-stu-id="87e21-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="87e21-315">Se si pubblica l'app in Azure, è possibile impostare i segreti di SendGrid come impostazioni dell'applicazione nel portale di App Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="87e21-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="87e21-316">Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="87e21-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="87e21-317">Combinare gli account di accesso social e locali</span><span class="sxs-lookup"><span data-stu-id="87e21-317">Combine social and local login accounts</span></span>

<span data-ttu-id="87e21-318">Per completare questa sezione, è innanzitutto necessario abilitare un provider di autenticazione esterno.</span><span class="sxs-lookup"><span data-stu-id="87e21-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="87e21-319">Visualizzare [Facebook, Google e l'autenticazione esterna provider](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="87e21-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="87e21-320">È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="87e21-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="87e21-321">Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso basati su social network, prima di tutto, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="87e21-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

<span data-ttu-id="87e21-323">Fare clic sui **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="87e21-323">Click on the **Manage** link.</span></span> <span data-ttu-id="87e21-324">Si noti 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="87e21-324">Note the 0 external (social logins) associated with this account.</span></span>

![Gestione visualizzazione](accconfirm/_static/manage.png)

<span data-ttu-id="87e21-326">Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="87e21-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="87e21-327">Nell'immagine seguente, Facebook è il provider di autenticazione esterno:</span><span class="sxs-lookup"><span data-stu-id="87e21-327">In the following image, Facebook is the external authentication provider:</span></span>

![Gestire la visualizzazione di account di accesso esterni listato Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="87e21-329">I due account sono stati combinati.</span><span class="sxs-lookup"><span data-stu-id="87e21-329">The two accounts have been combined.</span></span> <span data-ttu-id="87e21-330">Si è in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="87e21-330">You are able to sign in with either account.</span></span> <span data-ttu-id="87e21-331">È possibile che gli utenti per aggiungere gli account locali nel caso in cui il servizio di autenticazione account di accesso basati su social network è inattivo oppure più probabile che abbia perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="87e21-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="87e21-332">Abilitare la conferma dell'account dopo un sito ha utenti</span><span class="sxs-lookup"><span data-stu-id="87e21-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="87e21-333">Abilitare la conferma dell'account in un sito con gli utenti per bloccare tutti gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="87e21-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="87e21-334">Gli utenti esistenti sono bloccati perché i propri account non confermato.</span><span class="sxs-lookup"><span data-stu-id="87e21-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="87e21-335">Per evitare il blocco degli utente esistente, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="87e21-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="87e21-336">Aggiornare il database per contrassegnare tutti gli utenti esistenti come viene confermata.</span><span class="sxs-lookup"><span data-stu-id="87e21-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="87e21-337">Verificare che gli utenti esistenti.</span><span class="sxs-lookup"><span data-stu-id="87e21-337">Confirm existing users.</span></span> <span data-ttu-id="87e21-338">Ad esempio, batch-trasmissione messaggi di posta elettronica con collegamenti di conferma.</span><span class="sxs-lookup"><span data-stu-id="87e21-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
