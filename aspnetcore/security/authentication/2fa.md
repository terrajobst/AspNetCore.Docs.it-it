---
title: Autenticazione a due fattori con SMS in ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare l'autenticazione a due fattori (2FA) con un'applicazione ASP.NET Core.
manager: wpickett
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 20f00c2307e140d81e716304c96a143340d934d0
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="78fdf-103">Autenticazione a due fattori con SMS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78fdf-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="78fdf-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [gli sviluppatori di svizzero](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="78fdf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="78fdf-105">Vedere [generazione del codice QR abilitare per le app di autenticazione in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) per ASP.NET Core 2.0 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="78fdf-105">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="78fdf-106">In questa esercitazione viene illustrato come configurare l'autenticazione a due fattori (2FA) tramite SMS.</span><span class="sxs-lookup"><span data-stu-id="78fdf-106">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="78fdf-107">Sono fornite istruzioni per [twilio](https://www.twilio.com/) e [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ma è possibile usare qualsiasi altro provider SMS.</span><span class="sxs-lookup"><span data-stu-id="78fdf-107">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="78fdf-108">Si consiglia di eseguire [la conferma dell'Account e il recupero della Password](xref:security/authentication/accconfirm) prima di iniziare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="78fdf-108">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="78fdf-109">Visualizzazione di [esempio completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="78fdf-109">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="78fdf-110">[Come scaricare](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="78fdf-110">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="78fdf-111">Creare un nuovo progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78fdf-111">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="78fdf-112">Creare una nuova app web ASP.NET Core denominata `Web2FA` con singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="78fdf-112">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="78fdf-113">Seguire le istruzioni in [attivare SSL in un'applicazione ASP.NET Core](xref:security/enforcing-ssl) per impostare e richiedere SSL.</span><span class="sxs-lookup"><span data-stu-id="78fdf-113">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="78fdf-114">Creare un account SMS</span><span class="sxs-lookup"><span data-stu-id="78fdf-114">Create an SMS account</span></span>

<span data-ttu-id="78fdf-115">Creare un account SMS, ad esempio, da [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="78fdf-115">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="78fdf-116">Registrare le credenziali di autenticazione (per twilio: accountSid e authToken, per ASPSMS: userKey specificato e la Password).</span><span class="sxs-lookup"><span data-stu-id="78fdf-116">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="78fdf-117">Capire le credenziali del Provider SMS</span><span class="sxs-lookup"><span data-stu-id="78fdf-117">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="78fdf-118">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="78fdf-118">**Twilio:**</span></span>  
<span data-ttu-id="78fdf-119">Dalla scheda Dashboard dell'account di Twilio, copiare il **SID di Account** e **token di autorizzazione**.</span><span class="sxs-lookup"><span data-stu-id="78fdf-119">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="78fdf-120">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="78fdf-120">**ASPSMS:**</span></span>  
<span data-ttu-id="78fdf-121">Le impostazioni dell'account, passare alla **parametri Userkey** e copiarlo in combinazione con il **Password**.</span><span class="sxs-lookup"><span data-stu-id="78fdf-121">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="78fdf-122">Questi valori con lo strumento di gestione segreto le chiavi in un secondo momento verrà archiviato `SMSAccountIdentification` e `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="78fdf-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="78fdf-123">Specifica l'ID mittente / iniziatore</span><span class="sxs-lookup"><span data-stu-id="78fdf-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="78fdf-124">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="78fdf-124">**Twilio:**</span></span>  
<span data-ttu-id="78fdf-125">Nella scheda numeri, copiare il Twilio **il numero di telefono**.</span><span class="sxs-lookup"><span data-stu-id="78fdf-125">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="78fdf-126">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="78fdf-126">**ASPSMS:**</span></span>  
<span data-ttu-id="78fdf-127">All'interno del Menu dei creatori sbloccare sbloccare una o più mittenti o scegliere un creatore alfanumerico (non supportato da tutte le reti).</span><span class="sxs-lookup"><span data-stu-id="78fdf-127">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="78fdf-128">Questo valore con lo strumento di gestione di chiave privata all'interno della chiave in un secondo momento verrà archiviato `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="78fdf-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="78fdf-129">Fornire le credenziali per il servizio SMS</span><span class="sxs-lookup"><span data-stu-id="78fdf-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="78fdf-130">Si userà il [modello opzioni](xref:fundamentals/configuration/options) per accedere alle impostazioni di account e la chiave utente.</span><span class="sxs-lookup"><span data-stu-id="78fdf-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="78fdf-131">Creare una classe per recuperare la chiave sicura di SMS.</span><span class="sxs-lookup"><span data-stu-id="78fdf-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="78fdf-132">Per questo esempio, il `SMSoptions` classe il *Services/SMSoptions.cs* file.</span><span class="sxs-lookup"><span data-stu-id="78fdf-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="78fdf-133">Impostare il `SMSAccountIdentification`, `SMSAccountPassword` e `SMSAccountFrom` con il [strumento segreto manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="78fdf-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="78fdf-134">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="78fdf-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="78fdf-135">Aggiungere il pacchetto NuGet per il provider SMS.</span><span class="sxs-lookup"><span data-stu-id="78fdf-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="78fdf-136">Dal Package Manager Console (PMC) eseguire:</span><span class="sxs-lookup"><span data-stu-id="78fdf-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="78fdf-137">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="78fdf-137">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="78fdf-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="78fdf-138">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="78fdf-139">Aggiungere il codice nel *Services/MessageServices.cs* file per consentire il SMS.</span><span class="sxs-lookup"><span data-stu-id="78fdf-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="78fdf-140">Utilizzare il Twilio o la sezione ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="78fdf-140">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="78fdf-141">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="78fdf-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="78fdf-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="78fdf-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="78fdf-143">Configurare l'avvio da utilizzare `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="78fdf-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="78fdf-144">Aggiungere `SMSoptions` al contenitore del servizio nel `ConfigureServices` metodo il *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="78fdf-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="78fdf-145">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="78fdf-145">Enable two-factor authentication</span></span>

<span data-ttu-id="78fdf-146">Aprire il *Views/Manage/Index.cshtml* file di visualizzazione Razor e rimuovere il commento caratteri (pertanto nessun tag è commentato).</span><span class="sxs-lookup"><span data-stu-id="78fdf-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="78fdf-147">Eseguire l'accesso con autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="78fdf-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="78fdf-148">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="78fdf-148">Run the app and register a new user</span></span>

![Visualizzazione di registro dell'applicazione Web aperta in Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="78fdf-150">Toccare il nome utente, che attiva il `Index` il metodo di azione nel controller di gestione.</span><span class="sxs-lookup"><span data-stu-id="78fdf-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="78fdf-151">Scegliere il numero di telefono **Aggiungi** collegamento.</span><span class="sxs-lookup"><span data-stu-id="78fdf-151">Then tap the phone number **Add** link.</span></span>

![Gestione visualizzazione](2fa/_static/login2fa2.png)

* <span data-ttu-id="78fdf-153">Aggiungere un numero di telefono che verrà visualizzato il codice di verifica e toccare **invia il codice di verifica**.</span><span class="sxs-lookup"><span data-stu-id="78fdf-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Aggiungi pagina numero di telefono](2fa/_static/login2fa3.png)

* <span data-ttu-id="78fdf-155">Si riceverà un messaggio di testo con il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="78fdf-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="78fdf-156">L'immissione e toccare **invia**</span><span class="sxs-lookup"><span data-stu-id="78fdf-156">Enter it and tap **Submit**</span></span>

![Verificare la pagina del numero di telefono](2fa/_static/login2fa4.png)

<span data-ttu-id="78fdf-158">Se non si ottiene un messaggio di testo, vedere la pagina di log twilio.</span><span class="sxs-lookup"><span data-stu-id="78fdf-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="78fdf-159">La visualizzazione Gestione mostra che il numero di telefono è stato aggiunto correttamente.</span><span class="sxs-lookup"><span data-stu-id="78fdf-159">The Manage view shows your phone number was added successfully.</span></span>

![Gestione visualizzazione](2fa/_static/login2fa5.png)

* <span data-ttu-id="78fdf-161">Toccare **abilitare** per abilitare l'autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="78fdf-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Gestione visualizzazione](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="78fdf-163">Test di autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="78fdf-163">Test two-factor authentication</span></span>

* <span data-ttu-id="78fdf-164">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="78fdf-164">Log off.</span></span>

* <span data-ttu-id="78fdf-165">Accedi.</span><span class="sxs-lookup"><span data-stu-id="78fdf-165">Log in.</span></span>

* <span data-ttu-id="78fdf-166">L'account utente è abilitato l'autenticazione a due fattori, pertanto è necessario specificare il secondo fattore di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="78fdf-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="78fdf-167">In questa esercitazione è stata attivata la verifica telefonica.</span><span class="sxs-lookup"><span data-stu-id="78fdf-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="78fdf-168">I modelli predefiniti consentono inoltre di impostare la posta elettronica come secondo fattore.</span><span class="sxs-lookup"><span data-stu-id="78fdf-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="78fdf-169">È possibile impostare altri fattori secondo per l'autenticazione, ad esempio i codici a matrice.</span><span class="sxs-lookup"><span data-stu-id="78fdf-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="78fdf-170">Toccare **inviare**.</span><span class="sxs-lookup"><span data-stu-id="78fdf-170">Tap **Submit**.</span></span>

![Inviare una visualizzazione di codice di verifica](2fa/_static/login2fa7.png)

* <span data-ttu-id="78fdf-172">Immettere il codice che è visualizzato nel messaggio SMS.</span><span class="sxs-lookup"><span data-stu-id="78fdf-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="78fdf-173">Facendo clic su di **ricordare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso quando si usa il dispositivo e il browser.</span><span class="sxs-lookup"><span data-stu-id="78fdf-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="78fdf-174">Abilitazione 2FA e facendo clic su **ricordare questo browser** offre una protezione avanzata 2FA da utenti malintenzionati che tentano di accedere all'account, purché non hanno accesso al dispositivo.</span><span class="sxs-lookup"><span data-stu-id="78fdf-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="78fdf-175">Tale operazione può essere eseguita su qualsiasi dispositivo privata che vengono utilizzate regolarmente.</span><span class="sxs-lookup"><span data-stu-id="78fdf-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="78fdf-176">Impostando **ricordare questo browser**, ottenere le funzionalità di sicurezza di 2FA dai dispositivi che non si utilizza regolarmente e ottenere il vantaggio nel non dover eseguire 2FA nei propri dispositivi.</span><span class="sxs-lookup"><span data-stu-id="78fdf-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Verificare la visualizzazione](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="78fdf-178">Blocco degli account per la protezione contro gli attacchi di forza bruta</span><span class="sxs-lookup"><span data-stu-id="78fdf-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="78fdf-179">Il blocco dell'account è consigliabile con 2FA.</span><span class="sxs-lookup"><span data-stu-id="78fdf-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="78fdf-180">Una volta che un utente accede tramite un account locale o sociali, ogni tentativo non riuscito in 2FA viene archiviato.</span><span class="sxs-lookup"><span data-stu-id="78fdf-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="78fdf-181">Se i tentativi di accesso non riusciti massimo viene raggiunto, l'utente è bloccato (impostazione predefinita: blocco di 5 minuti dopo i tentativi di accesso non riusciti 5).</span><span class="sxs-lookup"><span data-stu-id="78fdf-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="78fdf-182">Un'autenticazione riuscita Reimposta il numero di tentativi di accesso non riuscito e reimposta l'orologio.</span><span class="sxs-lookup"><span data-stu-id="78fdf-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="78fdf-183">Il numero massimo tentativi di accesso e il periodo di blocco può essere impostato con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) e [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="78fdf-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="78fdf-184">Il seguente consente di configurare il blocco degli account per 10 minuti dopo 10 tentativi di accesso non riusciti:</span><span class="sxs-lookup"><span data-stu-id="78fdf-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="78fdf-185">Verificare che [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) imposta `lockoutOnFailure` a `true`:</span><span class="sxs-lookup"><span data-stu-id="78fdf-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
