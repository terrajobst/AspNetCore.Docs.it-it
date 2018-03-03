---
title: Autenticazione a due fattori con SMS
author: rick-anderson
description: Viene illustrato come configurare l'autenticazione a due fattori (2FA) con ASP.NET Core
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 721c4c20234c7232b509a0cff444538c2cfeb166
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="82a0e-103">Autenticazione a due fattori con SMS</span><span class="sxs-lookup"><span data-stu-id="82a0e-103">Two-factor authentication with SMS</span></span>

<span data-ttu-id="82a0e-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [gli sviluppatori di svizzero](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="82a0e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="82a0e-105">In questa esercitazione si applica a ASP.NET Core solo 1. x.</span><span class="sxs-lookup"><span data-stu-id="82a0e-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="82a0e-106">Vedere [la generazione di codice a matrice di attivazione per le app di autenticazione in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) per componenti di base di ASP.NET 2.0 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="82a0e-106">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="82a0e-107">In questa esercitazione viene illustrato come configurare l'autenticazione a due fattori (2FA) tramite SMS.</span><span class="sxs-lookup"><span data-stu-id="82a0e-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="82a0e-108">Sono fornite istruzioni per [twilio](https://www.twilio.com/) e [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ma è possibile usare qualsiasi altro provider SMS.</span><span class="sxs-lookup"><span data-stu-id="82a0e-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="82a0e-109">Si consiglia di eseguire [la conferma dell'Account e il recupero della Password](accconfirm.md) prima di iniziare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="82a0e-109">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="82a0e-110">Visualizzazione di [esempio completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="82a0e-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="82a0e-111">[Come scaricare](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="82a0e-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="82a0e-112">Creare un nuovo progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82a0e-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="82a0e-113">Creare una nuova app web ASP.NET Core denominata `Web2FA` con singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="82a0e-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="82a0e-114">Seguire le istruzioni in [applicazione SSL in un'applicazione ASP.NET Core](xref:security/enforcing-ssl) per impostare e richiedere SSL.</span><span class="sxs-lookup"><span data-stu-id="82a0e-114">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="82a0e-115">Creare un account SMS</span><span class="sxs-lookup"><span data-stu-id="82a0e-115">Create an SMS account</span></span>

<span data-ttu-id="82a0e-116">Creare un account SMS, ad esempio, da [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="82a0e-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="82a0e-117">Registrare le credenziali di autenticazione (per twilio: accountSid e authToken, per ASPSMS: userKey specificato e la Password).</span><span class="sxs-lookup"><span data-stu-id="82a0e-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="82a0e-118">Capire le credenziali del Provider SMS</span><span class="sxs-lookup"><span data-stu-id="82a0e-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="82a0e-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="82a0e-119">**Twilio:**</span></span>  
<span data-ttu-id="82a0e-120">Dalla scheda Dashboard dell'account di Twilio, copiare il **SID di Account** e **token di autorizzazione**.</span><span class="sxs-lookup"><span data-stu-id="82a0e-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="82a0e-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="82a0e-121">**ASPSMS:**</span></span>  
<span data-ttu-id="82a0e-122">Le impostazioni dell'account, passare alla **parametri Userkey** e copiarlo in combinazione con il **Password**.</span><span class="sxs-lookup"><span data-stu-id="82a0e-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="82a0e-123">Questi valori con lo strumento di gestione segreto le chiavi in un secondo momento verrà archiviato `SMSAccountIdentification` e `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="82a0e-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="82a0e-124">Specifica l'ID mittente / iniziatore</span><span class="sxs-lookup"><span data-stu-id="82a0e-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="82a0e-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="82a0e-125">**Twilio:**</span></span>  
<span data-ttu-id="82a0e-126">Nella scheda numeri, copiare il Twilio **il numero di telefono**.</span><span class="sxs-lookup"><span data-stu-id="82a0e-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="82a0e-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="82a0e-127">**ASPSMS:**</span></span>  
<span data-ttu-id="82a0e-128">All'interno del Menu dei creatori sbloccare sbloccare una o più mittenti o scegliere un creatore alfanumerico (non supportato da tutte le reti).</span><span class="sxs-lookup"><span data-stu-id="82a0e-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="82a0e-129">Questo valore con lo strumento di gestione di chiave privata all'interno della chiave in un secondo momento verrà archiviato `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="82a0e-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="82a0e-130">Fornire le credenziali per il servizio SMS</span><span class="sxs-lookup"><span data-stu-id="82a0e-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="82a0e-131">Si userà il [modello opzioni](xref:fundamentals/configuration/options) per accedere alle impostazioni di account e la chiave utente.</span><span class="sxs-lookup"><span data-stu-id="82a0e-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="82a0e-132">Creare una classe per recuperare la chiave sicura di SMS.</span><span class="sxs-lookup"><span data-stu-id="82a0e-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="82a0e-133">Per questo esempio, il `SMSoptions` classe il *Services/SMSoptions.cs* file.</span><span class="sxs-lookup"><span data-stu-id="82a0e-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="82a0e-134">Impostare il `SMSAccountIdentification`, `SMSAccountPassword` e `SMSAccountFrom` con il [strumento segreto manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="82a0e-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="82a0e-135">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="82a0e-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="82a0e-136">Aggiungere il pacchetto NuGet per il provider SMS.</span><span class="sxs-lookup"><span data-stu-id="82a0e-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="82a0e-137">Dal Package Manager Console (PMC) eseguire:</span><span class="sxs-lookup"><span data-stu-id="82a0e-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="82a0e-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="82a0e-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="82a0e-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="82a0e-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="82a0e-140">Aggiungere il codice nel *Services/MessageServices.cs* file per consentire il SMS.</span><span class="sxs-lookup"><span data-stu-id="82a0e-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="82a0e-141">Utilizzare il Twilio o la sezione ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="82a0e-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="82a0e-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="82a0e-142">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="82a0e-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="82a0e-143">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="82a0e-144">Configurare l'avvio da utilizzare `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="82a0e-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="82a0e-145">Aggiungere `SMSoptions` al contenitore del servizio nel `ConfigureServices` metodo il *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="82a0e-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="82a0e-146">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="82a0e-146">Enable two-factor authentication</span></span>

<span data-ttu-id="82a0e-147">Aprire il *Views/Manage/Index.cshtml* file di visualizzazione Razor e rimuovere il commento caratteri (pertanto nessun tag è commentato).</span><span class="sxs-lookup"><span data-stu-id="82a0e-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="82a0e-148">Eseguire l'accesso con autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="82a0e-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="82a0e-149">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="82a0e-149">Run the app and register a new user</span></span>

![Visualizzazione di registro dell'applicazione Web aperta in Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="82a0e-151">Toccare il nome utente, che attiva il `Index` il metodo di azione nel controller di gestione.</span><span class="sxs-lookup"><span data-stu-id="82a0e-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="82a0e-152">Scegliere il numero di telefono **Aggiungi** collegamento.</span><span class="sxs-lookup"><span data-stu-id="82a0e-152">Then tap the phone number **Add** link.</span></span>

![Gestione visualizzazione](2fa/_static/login2fa2.png)

* <span data-ttu-id="82a0e-154">Aggiungere un numero di telefono che verrà visualizzato il codice di verifica e toccare **invia il codice di verifica**.</span><span class="sxs-lookup"><span data-stu-id="82a0e-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Aggiungi pagina numero di telefono](2fa/_static/login2fa3.png)

* <span data-ttu-id="82a0e-156">Si riceverà un messaggio di testo con il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="82a0e-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="82a0e-157">L'immissione e toccare **invia**</span><span class="sxs-lookup"><span data-stu-id="82a0e-157">Enter it and tap **Submit**</span></span>

![Verificare la pagina del numero di telefono](2fa/_static/login2fa4.png)

<span data-ttu-id="82a0e-159">Se non si ottiene un messaggio di testo, vedere la pagina di log twilio.</span><span class="sxs-lookup"><span data-stu-id="82a0e-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="82a0e-160">La visualizzazione Gestione mostra che il numero di telefono è stato aggiunto correttamente.</span><span class="sxs-lookup"><span data-stu-id="82a0e-160">The Manage view shows your phone number was added successfully.</span></span>

![Gestione visualizzazione](2fa/_static/login2fa5.png)

* <span data-ttu-id="82a0e-162">Toccare **abilitare** per abilitare l'autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="82a0e-162">Tap **Enable** to enable two-factor authentication.</span></span>

![Gestione visualizzazione](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="82a0e-164">Test di autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="82a0e-164">Test two-factor authentication</span></span>

* <span data-ttu-id="82a0e-165">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="82a0e-165">Log off.</span></span>

* <span data-ttu-id="82a0e-166">Accedi.</span><span class="sxs-lookup"><span data-stu-id="82a0e-166">Log in.</span></span>

* <span data-ttu-id="82a0e-167">L'account utente è abilitato l'autenticazione a due fattori, pertanto è necessario specificare il secondo fattore di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="82a0e-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="82a0e-168">In questa esercitazione è stata attivata la verifica telefonica.</span><span class="sxs-lookup"><span data-stu-id="82a0e-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="82a0e-169">I modelli predefiniti consentono inoltre di impostare la posta elettronica come secondo fattore.</span><span class="sxs-lookup"><span data-stu-id="82a0e-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="82a0e-170">È possibile impostare altri fattori secondo per l'autenticazione, ad esempio i codici a matrice.</span><span class="sxs-lookup"><span data-stu-id="82a0e-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="82a0e-171">Toccare **inviare**.</span><span class="sxs-lookup"><span data-stu-id="82a0e-171">Tap **Submit**.</span></span>

![Inviare una visualizzazione di codice di verifica](2fa/_static/login2fa7.png)

* <span data-ttu-id="82a0e-173">Immettere il codice che è visualizzato nel messaggio SMS.</span><span class="sxs-lookup"><span data-stu-id="82a0e-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="82a0e-174">Facendo clic su di **ricordare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso quando si usa il dispositivo e il browser.</span><span class="sxs-lookup"><span data-stu-id="82a0e-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="82a0e-175">Abilitazione 2FA e facendo clic su **ricordare questo browser** offre una protezione avanzata 2FA da utenti malintenzionati che tentano di accedere all'account, purché non hanno accesso al dispositivo.</span><span class="sxs-lookup"><span data-stu-id="82a0e-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="82a0e-176">Tale operazione può essere eseguita su qualsiasi dispositivo privata che vengono utilizzate regolarmente.</span><span class="sxs-lookup"><span data-stu-id="82a0e-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="82a0e-177">Impostando **ricordare questo browser**, ottenere le funzionalità di sicurezza di 2FA dai dispositivi che non si utilizza regolarmente e ottenere il vantaggio nel non dover eseguire 2FA nei propri dispositivi.</span><span class="sxs-lookup"><span data-stu-id="82a0e-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Verificare la visualizzazione](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="82a0e-179">Blocco degli account per la protezione contro gli attacchi di forza bruta</span><span class="sxs-lookup"><span data-stu-id="82a0e-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="82a0e-180">È consigliabile che utilizzare il blocco degli account con 2FA.</span><span class="sxs-lookup"><span data-stu-id="82a0e-180">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="82a0e-181">Una volta che un utente accede (tramite un account locale o sociali), ogni tentativo non riuscito in 2FA viene archiviato e se viene raggiunto il numero massimo di tentativi (valore predefinito è 5), l'utente è bloccato per cinque minuti (è possibile impostare il blocco ora con `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="82a0e-181">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="82a0e-182">Gli elementi seguenti configurano Account venga bloccato per 10 minuti dopo 10 tentativi non riusciti.</span><span class="sxs-lookup"><span data-stu-id="82a0e-182">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
