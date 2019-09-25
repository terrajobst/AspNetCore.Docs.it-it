---
title: Autenticazione a due fattori con SMS in ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare l'autenticazione a due fattori (2FA) con un'app ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 68219579be9b7a7b25da6e348054e1ff2015cf5f
ms.sourcegitcommit: e54672f5c493258dc449fac5b98faf47eb123b28
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2019
ms.locfileid: "71248386"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="b797f-103">Autenticazione a due fattori con SMS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b797f-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="b797f-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT) e [Svizzera sviluppatori](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="b797f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="b797f-105">Due factor authentication (2FA) le app di autenticazione, usando un basati sul tempo monouso Password algoritmo (TOTP), sono il settore approccio autenticazione 2fa consigliato.</span><span class="sxs-lookup"><span data-stu-id="b797f-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="b797f-106">2FA usando TOTP è preferibile a SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="b797f-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="b797f-107">Per altre informazioni, vedere [generazione di abilitare la scansione del codice per le app di autenticazione TOTP in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) per ASP.NET Core 2.0 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="b797f-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="b797f-108">Questa esercitazione illustra come configurare l'autenticazione a due fattori (2FA) tramite SMS.</span><span class="sxs-lookup"><span data-stu-id="b797f-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="b797f-109">Le istruzioni sono puramente [twilio](https://www.twilio.com/) e [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ma è possibile usare qualsiasi altro provider SMS.</span><span class="sxs-lookup"><span data-stu-id="b797f-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="b797f-110">È consigliabile svolgere [conferma Account e recupero Password](xref:security/authentication/accconfirm) prima di iniziare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b797f-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="b797f-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="b797f-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="b797f-112">[Come scaricare](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="b797f-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="b797f-113">Creare un nuovo progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b797f-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="b797f-114">Creare una nuova app web ASP.NET Core denominata `Web2FA` con singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="b797f-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="b797f-115">Seguire le istruzioni in <xref:security/enforcing-ssl> per configurare e richiedere HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b797f-115">Follow the instructions in <xref:security/enforcing-ssl> to set up and require HTTPS.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="b797f-116">Creare un account SMS</span><span class="sxs-lookup"><span data-stu-id="b797f-116">Create an SMS account</span></span>

<span data-ttu-id="b797f-117">Creare un account SMS, ad esempio, dal [twilio](https://www.twilio.com/) oppure [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="b797f-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="b797f-118">Registrare le credenziali di autenticazione (per Twilio: accountSid e authToken, per ASPSMS: UserKey e password).</span><span class="sxs-lookup"><span data-stu-id="b797f-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="b797f-119">Scoprire le credenziali del Provider SMS</span><span class="sxs-lookup"><span data-stu-id="b797f-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="b797f-120">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="b797f-120">**Twilio:**</span></span>

<span data-ttu-id="b797f-121">Dalla scheda Dashboard dell'account Twilio copiare il **SID dell'account** e il **token di autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="b797f-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="b797f-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="b797f-122">**ASPSMS:**</span></span>

<span data-ttu-id="b797f-123">Dalle impostazioni dell'account passare a **userKey** e copiarlo insieme alla **password**.</span><span class="sxs-lookup"><span data-stu-id="b797f-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="b797f-124">In un secondo momento verranno archiviati questi valori con lo strumento secret manager tra quelle `SMSAccountIdentification` e `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="b797f-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="b797f-125">Specifica l'ID mittente / creatore</span><span class="sxs-lookup"><span data-stu-id="b797f-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="b797f-126">**Twilio:** Dalla scheda numeri copiare il **numero di telefono**Twilio.</span><span class="sxs-lookup"><span data-stu-id="b797f-126">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="b797f-127">**ASPSMS:** Nel menu Sblocca autori sbloccare uno o più autori o scegliere un originatore alfanumerico (non supportato da tutte le reti).</span><span class="sxs-lookup"><span data-stu-id="b797f-127">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="b797f-128">Questo valore con lo strumento secret manager all'interno della chiave in un secondo momento verranno archiviati `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="b797f-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>

### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="b797f-129">Fornire le credenziali per il servizio SMS</span><span class="sxs-lookup"><span data-stu-id="b797f-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="b797f-130">Si userà il [modello di opzioni](xref:fundamentals/configuration/options) per accedere alle impostazioni e chiave dell'account utente.</span><span class="sxs-lookup"><span data-stu-id="b797f-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

* <span data-ttu-id="b797f-131">Creare una classe per recuperare la chiave sicura di SMS.</span><span class="sxs-lookup"><span data-stu-id="b797f-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="b797f-132">Per questo esempio, il `SMSoptions` classe viene creata nel *Services/SMSoptions.cs* file.</span><span class="sxs-lookup"><span data-stu-id="b797f-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="b797f-133">Impostare il `SMSAccountIdentification`, `SMSAccountPassword` e `SMSAccountFrom` con il [strumento secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b797f-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="b797f-134">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b797f-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* <span data-ttu-id="b797f-135">Aggiungere il pacchetto NuGet per il provider SMS.</span><span class="sxs-lookup"><span data-stu-id="b797f-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="b797f-136">Dal Manager Console pacchetti eseguiti:</span><span class="sxs-lookup"><span data-stu-id="b797f-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="b797f-137">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="b797f-137">**Twilio:**</span></span>

`Install-Package Twilio`

<span data-ttu-id="b797f-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="b797f-138">**ASPSMS:**</span></span>

`Install-Package ASPSMS`

* <span data-ttu-id="b797f-139">Aggiungere il codice nel *Services/MessageServices.cs* file per consentire il SMS.</span><span class="sxs-lookup"><span data-stu-id="b797f-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="b797f-140">Usare la sezione ASPSMS o Twilio:</span><span class="sxs-lookup"><span data-stu-id="b797f-140">Use either the Twilio or the ASPSMS section:</span></span>

<span data-ttu-id="b797f-141">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="b797f-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="b797f-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="b797f-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="b797f-143">Configurare l'avvio da usare `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="b797f-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="b797f-144">Aggiungere `SMSoptions` al contenitore del servizio nel `ConfigureServices` metodo nella *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b797f-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="b797f-145">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="b797f-145">Enable two-factor authentication</span></span>

<span data-ttu-id="b797f-146">Aprire il file di visualizzazione Razor *views/Manage/index. cshtml* e rimuovere i caratteri di commento, in modo che nessun markup sia impostato come commento.</span><span class="sxs-lookup"><span data-stu-id="b797f-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commented out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="b797f-147">Accedere con l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="b797f-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="b797f-148">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="b797f-148">Run the app and register a new user</span></span>

![Visualizzazione Register dell'applicazione Web aperta in Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="b797f-150">Toccare il nome utente, che attiva il `Index` metodo di azione nel controller di gestione.</span><span class="sxs-lookup"><span data-stu-id="b797f-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="b797f-151">Toccare il numero di telefono **Add** collegamento.</span><span class="sxs-lookup"><span data-stu-id="b797f-151">Then tap the phone number **Add** link.</span></span>

![Gestione visualizzazione - toccare il collegamento "Aggiungi"](2fa/_static/login2fa2.png)

* <span data-ttu-id="b797f-153">Aggiungere un numero di telefono che verrà visualizzato il codice di verifica e toccare **Invia codice di verifica**.</span><span class="sxs-lookup"><span data-stu-id="b797f-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Aggiungi pagina il numero di telefono](2fa/_static/login2fa3.png)

* <span data-ttu-id="b797f-155">Si riceverà un messaggio di testo con il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="b797f-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="b797f-156">Immetterla e toccare **Submit**</span><span class="sxs-lookup"><span data-stu-id="b797f-156">Enter it and tap **Submit**</span></span>

![Numero di telefono pagina verifica](2fa/_static/login2fa4.png)

<span data-ttu-id="b797f-158">Se non si riceve un messaggio di testo, vedere pagina log twilio.</span><span class="sxs-lookup"><span data-stu-id="b797f-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="b797f-159">La visualizzazione di gestione mostra che il numero di telefono è stato aggiunto correttamente.</span><span class="sxs-lookup"><span data-stu-id="b797f-159">The Manage view shows your phone number was added successfully.</span></span>

![Gestione visualizzazione: numero di telefono è stato aggiunto](2fa/_static/login2fa5.png)

* <span data-ttu-id="b797f-161">Toccare **abilitare** per abilitare l'autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="b797f-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Gestione visualizzazione - abilitare l'autenticazione a due fattori](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="b797f-163">Test di autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="b797f-163">Test two-factor authentication</span></span>

* <span data-ttu-id="b797f-164">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="b797f-164">Log off.</span></span>

* <span data-ttu-id="b797f-165">Accedi.</span><span class="sxs-lookup"><span data-stu-id="b797f-165">Log in.</span></span>

* <span data-ttu-id="b797f-166">L'account utente è abilitata l'autenticazione a due fattori, pertanto è necessario fornire il secondo fattore di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="b797f-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="b797f-167">In questa esercitazione è stata abilitata la verifica telefonica.</span><span class="sxs-lookup"><span data-stu-id="b797f-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="b797f-168">I modelli predefiniti consentono inoltre di impostare la posta elettronica come secondo fattore.</span><span class="sxs-lookup"><span data-stu-id="b797f-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="b797f-169">È possibile impostare altri fattori secondo per l'autenticazione, ad esempio i codici a matrice.</span><span class="sxs-lookup"><span data-stu-id="b797f-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="b797f-170">Toccare **inviare**.</span><span class="sxs-lookup"><span data-stu-id="b797f-170">Tap **Submit**.</span></span>

![Invio di visualizzazione del codice di verifica](2fa/_static/login2fa7.png)

* <span data-ttu-id="b797f-172">Immettere il codice che si ottiene il messaggio SMS.</span><span class="sxs-lookup"><span data-stu-id="b797f-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="b797f-173">Facendo clic sui **memorizzare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso quando si usa lo stesso dispositivo e browser.</span><span class="sxs-lookup"><span data-stu-id="b797f-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="b797f-174">Abilitare 2FA e facendo clic su **memorizzare questo browser** fornirà protezione 2FA sicuro da utenti malintenzionati che tentano di accedere all'account, fino a quando non hanno accesso al dispositivo.</span><span class="sxs-lookup"><span data-stu-id="b797f-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="b797f-175">È possibile farlo in qualsiasi dispositivo privata che vengono utilizzate regolarmente.</span><span class="sxs-lookup"><span data-stu-id="b797f-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="b797f-176">Impostando **memorizzare questo browser**, si ottiene le funzionalità di sicurezza di 2FA dai dispositivi non si usa regolarmente, e si ottiene la comodità nel non dover passare attraverso 2FA nei propri dispositivi.</span><span class="sxs-lookup"><span data-stu-id="b797f-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Verificare la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="b797f-178">Blocco degli account per la protezione da attacchi di forza bruta</span><span class="sxs-lookup"><span data-stu-id="b797f-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="b797f-179">Blocco degli account è consigliabile 2fa.</span><span class="sxs-lookup"><span data-stu-id="b797f-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="b797f-180">Una volta che un utente accede tramite un account locale o un account di social networking, viene archiviato ogni tentativo non riuscito in 2FA.</span><span class="sxs-lookup"><span data-stu-id="b797f-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="b797f-181">Se viene raggiunto il numero massimo di tentativi di accesso non riusciti, l'utente viene bloccato (impostazione predefinita: blocco di 5 minuti dopo 5 tentativi di accesso non riusciti).</span><span class="sxs-lookup"><span data-stu-id="b797f-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="b797f-182">Un'autenticazione riuscita Reimposta il numero di tentativi di accesso non riuscito e reimposta l'orologio.</span><span class="sxs-lookup"><span data-stu-id="b797f-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="b797f-183">Il numero massimo tentativi di accesso e tempo di blocco può essere impostata con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) e [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="b797f-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="b797f-184">Di seguito consente di configurare il blocco degli account per 10 minuti dopo 10 tentativi di accesso non riusciti:</span><span class="sxs-lookup"><span data-stu-id="b797f-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="b797f-185">Verificare che [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) imposta `lockoutOnFailure` a `true`:</span><span class="sxs-lookup"><span data-stu-id="b797f-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
