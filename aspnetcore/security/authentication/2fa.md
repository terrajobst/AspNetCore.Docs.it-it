---
title: Autenticazione a due fattori con SMS
author: rick-anderson
description: Viene illustrato come configurare l'autenticazione a due fattori (2FA) con ASP.NET Core
keywords: ASP.NET Core, SMS, l'autenticazione, 2FA, autenticazione a due fattori, autenticazione a due fattori
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 77404de2f367cb12ba25433899198b69f9e5a7f2
ms.sourcegitcommit: 9a22c64759a7285ba788a37039bea5fe95f45f21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2017
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="26f09-104">Autenticazione a due fattori con SMS</span><span class="sxs-lookup"><span data-stu-id="26f09-104">Two-factor authentication with SMS</span></span>

<span data-ttu-id="26f09-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [gli sviluppatori di svizzero](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="26f09-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="26f09-106">In questa esercitazione si applica a ASP.NET Core solo 1. x.</span><span class="sxs-lookup"><span data-stu-id="26f09-106">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="26f09-107">Vedere [la generazione di codice a matrice di attivazione per le app di autenticazione in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) per componenti di base di ASP.NET 2.0 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="26f09-107">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="26f09-108">In questa esercitazione viene illustrato come configurare l'autenticazione a due fattori (2FA) tramite SMS.</span><span class="sxs-lookup"><span data-stu-id="26f09-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="26f09-109">Sono fornite istruzioni per [twilio](https://www.twilio.com/) e [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ma è possibile usare qualsiasi altro provider SMS.</span><span class="sxs-lookup"><span data-stu-id="26f09-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="26f09-110">Si consiglia di eseguire [la conferma dell'Account e il recupero della Password](accconfirm.md) prima di iniziare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="26f09-110">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="26f09-111">Visualizzazione di [esempio completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="26f09-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="26f09-112">[Come scaricare](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="26f09-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="26f09-113">Creare un nuovo progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26f09-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="26f09-114">Creare una nuova app web ASP.NET Core denominata `Web2FA` con singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="26f09-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="26f09-115">Seguire le istruzioni in [applicazione SSL in un'applicazione ASP.NET Core](xref:security/enforcing-ssl) per impostare e richiedere SSL.</span><span class="sxs-lookup"><span data-stu-id="26f09-115">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="26f09-116">Creare un account SMS</span><span class="sxs-lookup"><span data-stu-id="26f09-116">Create an SMS account</span></span>

<span data-ttu-id="26f09-117">Creare un account SMS, ad esempio, da [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="26f09-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="26f09-118">Registrare le credenziali di autenticazione (per twilio: accountSid e authToken, per ASPSMS: userKey specificato e la Password).</span><span class="sxs-lookup"><span data-stu-id="26f09-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="26f09-119">Capire le credenziali del Provider SMS</span><span class="sxs-lookup"><span data-stu-id="26f09-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="26f09-120">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="26f09-120">**Twilio:**</span></span>  
<span data-ttu-id="26f09-121">Dalla scheda Dashboard dell'account di Twilio, copiare il **SID di Account** e **token di autorizzazione**.</span><span class="sxs-lookup"><span data-stu-id="26f09-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="26f09-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="26f09-122">**ASPSMS:**</span></span>  
<span data-ttu-id="26f09-123">Le impostazioni dell'account, passare alla **parametri Userkey** e copiarlo in combinazione con il **Password**.</span><span class="sxs-lookup"><span data-stu-id="26f09-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="26f09-124">Questi valori con lo strumento di gestione segreto le chiavi in un secondo momento verrà archiviato `SMSAccountIdentification` e `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="26f09-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="26f09-125">Specifica l'ID mittente / iniziatore</span><span class="sxs-lookup"><span data-stu-id="26f09-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="26f09-126">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="26f09-126">**Twilio:**</span></span>  
<span data-ttu-id="26f09-127">Nella scheda numeri, copiare il Twilio **il numero di telefono**.</span><span class="sxs-lookup"><span data-stu-id="26f09-127">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="26f09-128">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="26f09-128">**ASPSMS:**</span></span>  
<span data-ttu-id="26f09-129">All'interno del Menu dei creatori sbloccare sbloccare una o più mittenti o scegliere un creatore alfanumerico (non supportato da tutte le reti).</span><span class="sxs-lookup"><span data-stu-id="26f09-129">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="26f09-130">Questo valore con lo strumento di gestione di chiave privata all'interno della chiave in un secondo momento verrà archiviato `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="26f09-130">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="26f09-131">Fornire le credenziali per il servizio SMS</span><span class="sxs-lookup"><span data-stu-id="26f09-131">Provide credentials for the SMS service</span></span>

<span data-ttu-id="26f09-132">Si userà il [modello opzioni](xref:fundamentals/configuration#options-config-objects) per accedere alle impostazioni di account e la chiave utente.</span><span class="sxs-lookup"><span data-stu-id="26f09-132">We'll use the [Options pattern](xref:fundamentals/configuration#options-config-objects) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="26f09-133">Creare una classe per recuperare la chiave sicura di SMS.</span><span class="sxs-lookup"><span data-stu-id="26f09-133">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="26f09-134">Per questo esempio, il `SMSoptions` classe il *Services/SMSoptions.cs* file.</span><span class="sxs-lookup"><span data-stu-id="26f09-134">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

<span data-ttu-id="26f09-135">[!code-csharp[Principale](2fa/sample/Web2FA/Services/SMSoptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="26f09-135">[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]</span></span>

<span data-ttu-id="26f09-136">Impostare il `SMSAccountIdentification`, `SMSAccountPassword` e `SMSAccountFrom` con il [strumento segreto manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="26f09-136">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="26f09-137">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="26f09-137">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="26f09-138">Aggiungere il pacchetto NuGet per il provider SMS.</span><span class="sxs-lookup"><span data-stu-id="26f09-138">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="26f09-139">Dal Package Manager Console (PMC) eseguire:</span><span class="sxs-lookup"><span data-stu-id="26f09-139">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="26f09-140">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="26f09-140">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="26f09-141">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="26f09-141">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="26f09-142">Aggiungere il codice nel *Services/MessageServices.cs* file per consentire il SMS.</span><span class="sxs-lookup"><span data-stu-id="26f09-142">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="26f09-143">Utilizzare il Twilio o la sezione ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="26f09-143">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="26f09-144">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="26f09-144">**Twilio:**</span></span>  
<span data-ttu-id="26f09-145">[!code-csharp[Principale](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="26f09-145">[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="26f09-146">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="26f09-146">**ASPSMS:**</span></span>  
<span data-ttu-id="26f09-147">[!code-csharp[Principale](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="26f09-147">[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="26f09-148">Configurare l'avvio da utilizzare`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="26f09-148">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="26f09-149">Aggiungere `SMSoptions` al contenitore del servizio nel `ConfigureServices` metodo il *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="26f09-149">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

<span data-ttu-id="26f09-150">[!code-csharp[Principale](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="26f09-150">[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]</span></span>

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="26f09-151">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="26f09-151">Enable two-factor authentication</span></span>

<span data-ttu-id="26f09-152">Aprire il *Views/Manage/Index.cshtml* file di visualizzazione Razor e rimuovere il commento caratteri (pertanto nessun tag è commentato).</span><span class="sxs-lookup"><span data-stu-id="26f09-152">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="26f09-153">Eseguire l'accesso con autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="26f09-153">Log in with two-factor authentication</span></span>

* <span data-ttu-id="26f09-154">Eseguire l'app e registrare un nuovo utente</span><span class="sxs-lookup"><span data-stu-id="26f09-154">Run the app and register a new user</span></span>

![Visualizzazione di registro dell'applicazione Web aperta in Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="26f09-156">Toccare il nome utente, che attiva il `Index` il metodo di azione nel controller di gestione.</span><span class="sxs-lookup"><span data-stu-id="26f09-156">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="26f09-157">Scegliere il numero di telefono **Aggiungi** collegamento.</span><span class="sxs-lookup"><span data-stu-id="26f09-157">Then tap the phone number **Add** link.</span></span>

![Gestione visualizzazione](2fa/_static/login2fa2.png)

* <span data-ttu-id="26f09-159">Aggiungere un numero di telefono che verrà visualizzato il codice di verifica e toccare **invia il codice di verifica**.</span><span class="sxs-lookup"><span data-stu-id="26f09-159">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Aggiungi pagina numero di telefono](2fa/_static/login2fa3.png)

* <span data-ttu-id="26f09-161">Si riceverà un messaggio di testo con il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="26f09-161">You will get a text message with the verification code.</span></span> <span data-ttu-id="26f09-162">L'immissione e toccare **invia**</span><span class="sxs-lookup"><span data-stu-id="26f09-162">Enter it and tap **Submit**</span></span>

![Verificare la pagina del numero di telefono](2fa/_static/login2fa4.png)

<span data-ttu-id="26f09-164">Se non si ottiene un messaggio di testo, vedere la pagina di log twilio.</span><span class="sxs-lookup"><span data-stu-id="26f09-164">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="26f09-165">La visualizzazione Gestione mostra che il numero di telefono è stato aggiunto correttamente.</span><span class="sxs-lookup"><span data-stu-id="26f09-165">The Manage view shows your phone number was added successfully.</span></span>

![Gestione visualizzazione](2fa/_static/login2fa5.png)

* <span data-ttu-id="26f09-167">Toccare **abilitare** per abilitare l'autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="26f09-167">Tap **Enable** to enable two-factor authentication.</span></span>

![Gestione visualizzazione](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="26f09-169">Test di autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="26f09-169">Test two-factor authentication</span></span>

* <span data-ttu-id="26f09-170">Esegue la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="26f09-170">Log off.</span></span>

* <span data-ttu-id="26f09-171">Accedi.</span><span class="sxs-lookup"><span data-stu-id="26f09-171">Log in.</span></span>

* <span data-ttu-id="26f09-172">L'account utente è abilitato l'autenticazione a due fattori, pertanto è necessario specificare il secondo fattore di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="26f09-172">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="26f09-173">In questa esercitazione è stata attivata la verifica telefonica.</span><span class="sxs-lookup"><span data-stu-id="26f09-173">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="26f09-174">I modelli predefiniti consentono inoltre di impostare la posta elettronica come secondo fattore.</span><span class="sxs-lookup"><span data-stu-id="26f09-174">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="26f09-175">È possibile impostare altri fattori secondo per l'autenticazione, ad esempio i codici a matrice.</span><span class="sxs-lookup"><span data-stu-id="26f09-175">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="26f09-176">Toccare **inviare**.</span><span class="sxs-lookup"><span data-stu-id="26f09-176">Tap **Submit**.</span></span>

![Inviare una visualizzazione di codice di verifica](2fa/_static/login2fa7.png)

* <span data-ttu-id="26f09-178">Immettere il codice che è visualizzato nel messaggio SMS.</span><span class="sxs-lookup"><span data-stu-id="26f09-178">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="26f09-179">Facendo clic su di **ricordare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso quando si usa il dispositivo e il browser.</span><span class="sxs-lookup"><span data-stu-id="26f09-179">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="26f09-180">Abilitazione 2FA e facendo clic su **ricordare questo browser** offre una protezione avanzata 2FA da utenti malintenzionati che tentano di accedere all'account, purché non hanno accesso al dispositivo.</span><span class="sxs-lookup"><span data-stu-id="26f09-180">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="26f09-181">Tale operazione può essere eseguita su qualsiasi dispositivo privata che vengono utilizzate regolarmente.</span><span class="sxs-lookup"><span data-stu-id="26f09-181">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="26f09-182">Impostando **ricordare questo browser**, ottenere le funzionalità di sicurezza di 2FA dai dispositivi che non si utilizza regolarmente e ottenere il vantaggio nel non dover eseguire 2FA nei propri dispositivi.</span><span class="sxs-lookup"><span data-stu-id="26f09-182">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Verificare la visualizzazione](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="26f09-184">Blocco degli account per la protezione contro gli attacchi di forza bruta</span><span class="sxs-lookup"><span data-stu-id="26f09-184">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="26f09-185">È consigliabile che utilizzare il blocco degli account con 2FA.</span><span class="sxs-lookup"><span data-stu-id="26f09-185">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="26f09-186">Una volta che un utente accede (tramite un account locale o sociali), ogni tentativo non riuscito in 2FA viene archiviato e se viene raggiunto il numero massimo di tentativi (valore predefinito è 5), l'utente è bloccato per cinque minuti (è possibile impostare il blocco ora con `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="26f09-186">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="26f09-187">Gli elementi seguenti configurano Account venga bloccato per 10 minuti dopo 10 tentativi non riusciti.</span><span class="sxs-lookup"><span data-stu-id="26f09-187">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

<span data-ttu-id="26f09-188">[!code-csharp[Principale](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]</span><span class="sxs-lookup"><span data-stu-id="26f09-188">[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]</span></span> 
