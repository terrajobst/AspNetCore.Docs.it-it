---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity | Documenti Microsoft
author: HaoK
description: In questa esercitazione Mostra come configurare l'autenticazione a due fattori (2FA) tramite posta elettronica e SMS. In questo articolo è stato scritto dal Rick Anderson ( @RickAndMSFT ), PR....
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="b1ac5-104">Autenticazione a due fattori tramite SMS e posta elettronica con identità di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b1ac5-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="b1ac5-105">dal [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="b1ac5-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="b1ac5-106">In questa esercitazione Mostra come configurare l'autenticazione a due fattori (2FA) tramite posta elettronica e SMS.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="b1ac5-107">In questo articolo è stato scritto dal Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="b1ac5-108">L'esempio di NuGet è stata scritta principalmente da Hao Kung.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="b1ac5-109">Questo argomento vengono illustrate le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-109">This topic covers the following:</span></span>

- [<span data-ttu-id="b1ac5-110">Compilazione dell'esempio di identità</span><span class="sxs-lookup"><span data-stu-id="b1ac5-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="b1ac5-111">Impostare SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="b1ac5-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="b1ac5-112">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="b1ac5-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="b1ac5-113">Come registrare un provider di autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="b1ac5-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="b1ac5-114">Combinare gli account di accesso social networking e locale</span><span class="sxs-lookup"><span data-stu-id="b1ac5-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="b1ac5-115">Blocco degli account da attacchi di forza bruta</span><span class="sxs-lookup"><span data-stu-id="b1ac5-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="b1ac5-116">Compilazione dell'esempio di identità</span><span class="sxs-lookup"><span data-stu-id="b1ac5-116">Building the Identity sample</span></span>

<span data-ttu-id="b1ac5-117">In questa sezione si userà NuGet per scaricare un esempio che si opererà con.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="b1ac5-118">Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="b1ac5-119">Installazione di Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="b1ac5-120">Avviso: È necessario installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) per completare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="b1ac5-121">Creare un nuovo ***vuoto*** progetto Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="b1ac5-122">Nella Console di gestione pacchetti, immettere quanto segue i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="b1ac5-123">In questa esercitazione si userà [SendGrid](http://sendgrid.com/) per inviare posta elettronica e [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) per collaudo sms.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="b1ac5-124">Il `Identity.Samples` pacchetto viene installato il codice che verrà utilizzato con.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="b1ac5-125">Impostare il [progetto utilizzare SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="b1ac5-126">*Parametro facoltativo*: seguire le istruzioni in my [esercitazione conferma tramite posta elettronica](account-confirmation-and-password-recovery-with-aspnet-identity.md) collegare SendGrid e quindi eseguire l'app e registrare un account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="b1ac5-127">* Facoltativo: * rimuovere il codice di conferma demo collegamento e-mail tratto dall'esempio (il `ViewBag.Link` codice nel controller di account.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="b1ac5-128">Vedere il `DisplayEmail` e `ForgotPasswordConfirmation` metodi di azione e visualizzazioni razor).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="b1ac5-129"><em>Facoltativo: * rimuovere il `ViewBag.Status` codice verso i controller di gestione e l'Account e dal *Views\Account\VerifyCode.cshtml</em> e <em>Views\Manage\VerifyPhoneNumber.cshtml</em> visualizzazioni razor.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="b1ac5-130">In alternativa, è possibile mantenere il `ViewBag.Status` visualizzato per verificare come questa app funziona in locale senza dover collegare e inviare posta elettronica e messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="b1ac5-131">Avviso: Se si modifica una qualsiasi delle impostazioni di sicurezza in questo esempio, le applicazioni di produzione saranno necessario eseguire un controllo di sicurezza che chiama in modo esplicito le modifiche apportate.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="b1ac5-132">Impostare SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="b1ac5-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="b1ac5-133">In questa esercitazione vengono fornite istruzioni per l'utilizzo di Twilio o ASPSMS ma è possibile usare qualsiasi altro provider SMS.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="b1ac5-134">**Creazione di un Account utente con un provider SMS**</span><span class="sxs-lookup"><span data-stu-id="b1ac5-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="b1ac5-135">Creare un [Twilio](https://www.twilio.com/try-twilio) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="b1ac5-136">**Installazione di pacchetti aggiuntivi o l'aggiunta di riferimenti al servizio**</span><span class="sxs-lookup"><span data-stu-id="b1ac5-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="b1ac5-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-137">Twilio:</span></span>  
   <span data-ttu-id="b1ac5-138">Nella Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="b1ac5-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-139">ASPSMS:</span></span>  
   <span data-ttu-id="b1ac5-140">È necessario aggiungere il riferimento al servizio seguente:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="b1ac5-141">Indirizzo:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="b1ac5-142">Spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="b1ac5-143">**Capire le credenziali utente del Provider SMS**</span><span class="sxs-lookup"><span data-stu-id="b1ac5-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="b1ac5-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-144">Twilio:</span></span>  
   <span data-ttu-id="b1ac5-145">Dal **Dashboard** scheda dell'account di Twilio, copia il **SID di Account** e **token di autorizzazione**.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="b1ac5-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-146">ASPSMS:</span></span>  
   <span data-ttu-id="b1ac5-147">Le impostazioni dell'account, passare alla **parametri Userkey** e copiarlo insieme il Self-definito **Password**.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="b1ac5-148">Questi valori in un secondo momento verrà archiviato nella variabile di `SMSAccountIdentification` e `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="b1ac5-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="b1ac5-149">**Specifica l'ID mittente / iniziatore**</span><span class="sxs-lookup"><span data-stu-id="b1ac5-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="b1ac5-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-150">Twilio:</span></span>  
   <span data-ttu-id="b1ac5-151">Dal **numeri** scheda, copiare il numero di telefono di Twilio.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="b1ac5-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-152">ASPSMS:</span></span>  
   <span data-ttu-id="b1ac5-153">All'interno di **dei creatori di sbloccare** Menu, sbloccare una o più mittenti o scegliere un creatore alfanumerico (non supportato da tutte le reti).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="b1ac5-154">Questo valore in un secondo momento verrà archiviato nella variabile `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="b1ac5-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="b1ac5-155">**Il trasferimento di credenziali del provider SMS in app**</span><span class="sxs-lookup"><span data-stu-id="b1ac5-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="b1ac5-156">Rendere disponibile per l'app le credenziali e il numero di telefono mittente:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="b1ac5-157">Sicurezza - archivio mai i dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="b1ac5-158">L'account e le credenziali vengono aggiunti al codice precedente per semplificare il codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="b1ac5-159">Vedere di Jon Atten [ASP.NET MVC: mantenere privato Out di impostazioni di controllo del codice sorgente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="b1ac5-160">**Implementazione del trasferimento dei dati al provider SMS**</span><span class="sxs-lookup"><span data-stu-id="b1ac5-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="b1ac5-161">Configurare il `SmsService` classe il *App\_Start\IdentityConfig.cs* file.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="b1ac5-162">A seconda del provider SMS utilizzato attivare il **Twilio** o **ASPSMS** sezione:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="b1ac5-163">Eseguire l'app e accedere con l'account che è stato registrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="b1ac5-164">Fare clic sull'ID utente, che attiva il `Index` metodo di azione `Manage` controller.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="b1ac5-165">Fare clic su Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="b1ac5-166">In pochi secondi si otterrà un messaggio di testo con il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="b1ac5-167">L'immissione e premere **Invia**.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="b1ac5-168">La visualizzazione Gestione mostra che il numero di telefono è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="b1ac5-169">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="b1ac5-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="b1ac5-170">Il `Index` metodo di azione `Manage` controller imposta il messaggio di stato in base l'azione precedente e vengono forniti i collegamenti per modificare la password locale o aggiungere un account locale.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="b1ac5-171">Il `Index` metodo visualizza inoltre lo stato o il 2FA phone numero, l'account di accesso esterni, 2FA abilitato e ricordare 2FA metodo per il browser (illustrato più avanti).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="b1ac5-172">Facendo clic sull'ID utente (posta elettronica) nella barra del titolo non passare un messaggio.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="b1ac5-173">Facendo clic su di **numero di telefono: rimuovere** collegare passate `Message=RemovePhoneSuccess` come una stringa di query.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="b1ac5-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="b1ac5-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="b1ac5-175">Il `AddPhoneNumber` metodo di azione consente di visualizzare una finestra di dialogo per immettere un numero di telefono che può ricevere i messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="b1ac5-176">Facendo clic su di **invia il codice di verifica** pulsante Invia il numero di telefono per HTTP POST `AddPhoneNumber` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="b1ac5-177">Il `GenerateChangePhoneNumberTokenAsync` metodo genera il token di sicurezza che verrà impostato nel messaggio SMS.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="b1ac5-178">Se il servizio SMS è stato configurato, il token viene inviato come stringa &quot;il codice di sicurezza è &lt;token&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="b1ac5-179">Il `SmsService.SendAsync` metodo viene chiamato in modo asincrono, quindi l'app viene reindirizzato il `VerifyPhoneNumber` metodo di azione (che consente di visualizzare la finestra di dialogo seguente), in cui è possibile immettere il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="b1ac5-180">Dopo aver immesso il codice e fare clic su Invia, viene inserito il codice HTTP POST `VerifyPhoneNumber` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="b1ac5-181">Il `ChangePhoneNumberAsync` metodo controlla il codice di sicurezza registrato.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="b1ac5-182">Se il codice è corretto, il numero di telefono viene aggiunto per il `PhoneNumber` campo il `AspNetUsers` tabella.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="b1ac5-183">Se la chiamata ha esito positivo, il `SignInAsync` metodo viene chiamato:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="b1ac5-184">Il `isPersistent` parametro determina se la sessione di autenticazione è persistente in più richieste.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="b1ac5-185">Quando si modifica il profilo di sicurezza, un indicatore di sicurezza verrà generato e archiviato nel `SecurityStamp` campo il *AspNetUsers* tabella.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="b1ac5-186">Si noti che il `SecurityStamp` campo è diverso dal cookie di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="b1ac5-187">Il cookie di sicurezza non verrà memorizzato nel `AspNetUsers` tabella (o qualsiasi altro nel database di identità).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="b1ac5-188">Il token del cookie di sicurezza è autofirmato usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e viene creato con il `UserId, SecurityStamp` e informazioni sull'ora di scadenza.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="b1ac5-189">Middleware del cookie controlla il cookie a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="b1ac5-190">Il `SecurityStampValidator` metodo il `Startup` classe raggiungono il database e controlla periodicamente, indicatore di sicurezza come specificato con il `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="b1ac5-191">Questo errore si verifica solo ogni 30 minuti (in questo esempio) a meno che non si modifica il profilo di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="b1ac5-192">L'intervallo di 30 minuti è stato scelto per ridurre al minimo trip al database.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="b1ac5-193">Il `SignInAsync` (metodo) deve essere chiamato quando si apportano modifiche al profilo di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="b1ac5-194">Quando viene modificato il profilo di sicurezza, il database è aggiornamenti il `SecurityStamp` campo e senza chiamare il `SignInAsync` metodo è necessario rimanere connessi a *solo* fino alla successiva esecuzione della pipeline OWIN accede al database (il `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="b1ac5-195">È possibile testare questo modificando il `SignInAsync` per restituire immediatamente e l'impostazione del cookie `validateInterval` proprietà da 30 minuti a 5 secondi:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="b1ac5-196">Con le modifiche al codice precedente, è possibile modificare il profilo di sicurezza (ad esempio, modificando lo stato di **due fattori abilitata**) e verrà eseguito 5 secondi quando il `SecurityStampValidator.OnValidateIdentity` metodo ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="b1ac5-197">Rimuovere il ritorno a capo nel `SignInAsync` (metodo), modificare un altro profilo di sicurezza e non verranno registrate. Il `SignInAsync` metodo genera un nuovo cookie di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="b1ac5-198">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="b1ac5-198">Enable two-factor authentication</span></span>

<span data-ttu-id="b1ac5-199">Nell'applicazione di esempio, è necessario utilizzare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="b1ac5-200">Per abilitare 2FA, fare clic sull'ID utente (alias di posta elettronica) nella barra di spostamento.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="b1ac5-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="b1ac5-201">Fare clic su Abilita 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="b1ac5-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="b1ac5-202">Effettuare la disconnessione, quindi eseguire nuovamente l'accesso.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-202">Log out, then log back in.</span></span> <span data-ttu-id="b1ac5-203">Se è stato abilitato posta elettronica (vedere il [esercitazione precedente](account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare il SMS o posta elettronica per 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="b1ac5-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="b1ac5-204">Verrà visualizzata la pagina di verifica di codice in cui è possibile immettere il codice (da posta elettronica o SMS).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="b1ac5-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="b1ac5-205">Facendo clic su di **ricordare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso con tale computer e il browser.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="b1ac5-206">Abilitazione 2FA e facendo clic su di **ricordare questo browser** offre una protezione avanzata 2FA da utenti malintenzionati che tentano di accedere all'account, purché non hanno accesso al computer.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="b1ac5-207">È possibile farlo in qualsiasi computer privati che vengono utilizzate regolarmente.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="b1ac5-208">Impostando **ricordare questo browser**, ottenere le funzionalità di sicurezza di 2FA dai computer non si utilizza regolarmente e ottenere il vantaggio nel non dover eseguire 2FA nei propri computer.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="b1ac5-209">Come registrare un provider di autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="b1ac5-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="b1ac5-210">Quando si crea un nuovo progetto MVC, il *IdentityConfig.cs* file contiene il codice seguente per registrare un provider di autenticazione a due fattori:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="b1ac5-211">Aggiungere un numero di telefono per 2FA</span><span class="sxs-lookup"><span data-stu-id="b1ac5-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="b1ac5-212">Il `AddPhoneNumber` metodo di azione di `Manage` controller genera un token di sicurezza ed invia a telefono numero che è stato fornito.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="b1ac5-213">Dopo l'invio del token, reindirizza il `VerifyPhoneNumber` metodo di azione, in cui è possibile immettere il codice per registrare SMS per 2FA.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="b1ac5-214">2FA SMS non viene utilizzato fino a quando non significa che il numero di telefono.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="b1ac5-215">Abilitazione 2FA</span><span class="sxs-lookup"><span data-stu-id="b1ac5-215">Enabling 2FA</span></span>

<span data-ttu-id="b1ac5-216">Il `EnableTFA` 2FA consente di metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="b1ac5-217">Si noti il `SignInAsync` deve essere chiamato perché 2FA attiva è una modifica al profilo di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="b1ac5-218">Quando 2FA è abilitata, l'utente dovrà utilizzare 2FA per l'accesso, utilizzando gli approcci 2FA registrazione (SMS e posta elettronica di esempio).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="b1ac5-219">È possibile aggiungere ulteriori 2FA provider, ad esempio i generatori di codice a matrice o è possibile scrivere si è proprietari (vedere [utilizzando Google Authenticator con ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="b1ac5-220">I codici 2FA vengono generati utilizzando [basati sul tempo algoritmo Password monouso](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) e codici sono validi per sei minuti.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="b1ac5-221">Se si accettano più di sei minuti per immettere il codice, si otterrà un messaggio di errore del codice non valido.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="b1ac5-222">Combinare gli account di accesso social networking e locale</span><span class="sxs-lookup"><span data-stu-id="b1ac5-222">Combine social and local login accounts</span></span>

<span data-ttu-id="b1ac5-223">È possibile combinare gli account locali e sociali facendo clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="b1ac5-224">Nella sequenza seguente &quot; RickAndMSFT@gmail.com &quot; viene innanzitutto creato un account di accesso locale, ma è possibile creare l'account come un accesso social prima, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="b1ac5-225">Fare clic su di **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-225">Click on the **Manage** link.</span></span> <span data-ttu-id="b1ac5-226">Si noti il valore 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="b1ac5-227">Fare clic sul collegamento a un altro registro nel servizio e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="b1ac5-228">I due account sono stati combinati, sarà possibile eseguire l'accesso con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="b1ac5-229">Si consiglia agli utenti di aggiungere gli account locali nel caso in cui il log di social networking nel servizio di autenticazione è inattivo o più probabilmente è più possibile accedere al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="b1ac5-230">Nella figura seguente, Tom è una procedura di accesso social (che si può vedere dal **account di accesso esterni: 1** visualizzato nella pagina).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="b1ac5-231">Facendo clic su **Scegli una password** consente di aggiungere un registro locale in associato lo stesso account.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="b1ac5-232">Blocco degli account da attacchi di forza bruta</span><span class="sxs-lookup"><span data-stu-id="b1ac5-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="b1ac5-233">È possibile proteggere gli account dell'app da attacchi con dizionario, consentendo di blocco dell'utente.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="b1ac5-234">Nell'esempio di codice il `ApplicationUserManager Create` metodo configura blocco:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="b1ac5-235">Il codice precedente consente di blocco per solo l'autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="b1ac5-236">Sebbene sia possibile attivare un blocco per gli account di accesso modificando `shouldLockout` su true nel `Login` metodo del controller di account, si suggerisce di non abilitare blocco per gli account di accesso perché rende vulnerabili per l'account [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) account di accesso attacchi.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="b1ac5-237">Nell'esempio di codice, blocco è disabilitato per l'account amministratore creato nel `ApplicationDbInitializer Seed` metodo:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="b1ac5-238">Che l'utente dispone di un account di posta elettronica convalidato</span><span class="sxs-lookup"><span data-stu-id="b1ac5-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="b1ac5-239">Il codice seguente richiede all'utente di disporre di un account di posta elettronica convalidato prima di poter accedere:</span><span class="sxs-lookup"><span data-stu-id="b1ac5-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="b1ac5-240">Come requisito 2FA controlla SignInManager</span><span class="sxs-lookup"><span data-stu-id="b1ac5-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="b1ac5-241">Accesso locale sia social Accedi per verificare se 2FA è abilitata.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="b1ac5-242">Se 2FA è abilitata, il `SignInManager` metodo di accesso restituisce `SignInStatus.RequiresVerification`, e l'utente verrà reindirizzato al `SendCode` metodo di azione, in cui è necessario immettere il codice per completare il log in sequenza.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="b1ac5-243">Se l'utente ha RememberMe è impostato sul cookie di utenti locale, il `SignInManager` restituirà `SignInStatus.Success` e non avranno passino 2FA.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="b1ac5-244">Il codice seguente illustra il `SendCode` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="b1ac5-245">Oggetto [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) viene creato con tutti i metodi di 2FA abilitati per l'utente.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="b1ac5-246">Il [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) viene passato per il [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, che consente all'utente di selezionare l'approccio 2FA (in genere di posta elettronica e SMS).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="b1ac5-247">Una volta che l'utente invia l'approccio 2FA, il `HTTP POST SendCode` viene chiamato il metodo di azione, il `SignInManager` invia il codice 2FA e l'utente viene reindirizzato al `VerifyCode` metodo di azione in cui possono inserire il codice per completare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="b1ac5-248">Blocco 2FA</span><span class="sxs-lookup"><span data-stu-id="b1ac5-248">2FA Lockout</span></span>

<span data-ttu-id="b1ac5-249">Anche se è possibile impostare il blocco degli account per i tentativi non riusciti password di account di accesso, tale approccio rende sensibili per l'account di accesso [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) blocchi.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="b1ac5-250">È consigliabile che utilizzare il blocco degli account solo con 2FA.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="b1ac5-251">Quando il `ApplicationUserManager` viene creato, il codice di esempio imposta blocco 2FA e `MaxFailedAccessAttemptsBeforeLockout` a cinque.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="b1ac5-252">Una volta che un utente accede (tramite un account locale o sociali), ogni tentativo non riuscito in 2FA viene archiviato e se viene raggiunto il numero massimo di tentativi, l'utente è bloccato per cinque minuti (è possibile impostare il blocco ora con `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="b1ac5-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="b1ac5-253">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b1ac5-253">Additional Resources</span></span>

- <span data-ttu-id="b1ac5-254">[ASP.NET Identity consigliato risorse](../getting-started/aspnet-identity-recommended-resources.md) elenco completo delle identità blog, video, esercitazioni e great così collega.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="b1ac5-255">[App di MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) viene inoltre illustrato come aggiungere informazioni sul profilo alla tabella users.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="b1ac5-256">[ASP.NET MVC e identità 2.0: nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) da John Atten.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="b1ac5-257">La conferma dell'account e il recupero della Password con identità di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b1ac5-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="b1ac5-258">Introduzione ad ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="b1ac5-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="b1ac5-259">[Annuncio RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) da Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="b1ac5-260">[Identità di ASP.NET 2.0: Impostazione di convalida dell'Account e l'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) da John Atten.</span><span class="sxs-lookup"><span data-stu-id="b1ac5-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
