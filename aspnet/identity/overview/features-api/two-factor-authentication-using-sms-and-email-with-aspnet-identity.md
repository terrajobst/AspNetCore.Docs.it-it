---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity | Microsoft Docs
author: HaoK
description: Questa esercitazione illustrerà come configurare l'autenticazione a due fattori (2FA) tramite SMS e posta elettronica. Questo articolo è stato scritto da Rick Anderson ( @RickAndMSFT ), PR....
ms.author: aspnetcontent
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c1f4bd4a3f65d4b7fccd86214fd0ba45c891e390
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807622"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="18ffb-104">Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="18ffb-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="18ffb-105">dal [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="18ffb-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="18ffb-106">Questa esercitazione illustrerà come configurare l'autenticazione a due fattori (2FA) tramite SMS e posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="18ffb-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="18ffb-107">Questo articolo è stato scritto da Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="18ffb-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="18ffb-108">L'esempio di NuGet è stato scritto principalmente da Hao Kung.</span><span class="sxs-lookup"><span data-stu-id="18ffb-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="18ffb-109">In questo argomento illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="18ffb-109">This topic covers the following:</span></span>

- [<span data-ttu-id="18ffb-110">Compilazione dell'esempio di identità</span><span class="sxs-lookup"><span data-stu-id="18ffb-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="18ffb-111">Configurazione di SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="18ffb-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="18ffb-112">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="18ffb-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="18ffb-113">Come registrare un provider di autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="18ffb-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="18ffb-114">Combinare gli account di accesso social e locali</span><span class="sxs-lookup"><span data-stu-id="18ffb-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="18ffb-115">Blocco degli account da attacchi di forza bruta</span><span class="sxs-lookup"><span data-stu-id="18ffb-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="18ffb-116">Compilazione dell'esempio di identità</span><span class="sxs-lookup"><span data-stu-id="18ffb-116">Building the Identity sample</span></span>

<span data-ttu-id="18ffb-117">In questa sezione si userà NuGet per scaricare un esempio che collaboreremo con.</span><span class="sxs-lookup"><span data-stu-id="18ffb-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="18ffb-118">Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="18ffb-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="18ffb-119">Installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="18ffb-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="18ffb-120">Avviso: È necessario installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) per completare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="18ffb-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="18ffb-121">Creare una nuova ***vuoto*** progetto Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="18ffb-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="18ffb-122">Nella Console di gestione pacchetti immettere quanto segue i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="18ffb-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="18ffb-123">In questa esercitazione, useremo [SendGrid](http://sendgrid.com/) per inviare posta elettronica e [Twilio](https://www.twilio.com/) oppure [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) per sms inviato.</span><span class="sxs-lookup"><span data-stu-id="18ffb-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="18ffb-124">Il `Identity.Samples` pacchetto viene installato il codice si userà.</span><span class="sxs-lookup"><span data-stu-id="18ffb-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="18ffb-125">Impostare il [progetto per l'uso di SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="18ffb-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="18ffb-126">*Facoltativo*: seguire le istruzioni nella mio [esercitazione conferma tramite posta elettronica](account-confirmation-and-password-recovery-with-aspnet-identity.md) collegare SendGrid e quindi eseguire l'app e registrare un account di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="18ffb-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="18ffb-127">* Facoltativo: * rimuovere il codice di conferma collegamento tramite posta elettronica demo tratto dall'esempio (il `ViewBag.Link` codice nel controller account.</span><span class="sxs-lookup"><span data-stu-id="18ffb-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="18ffb-128">Vedere le `DisplayEmail` e `ForgotPasswordConfirmation` metodi di azione e visualizzazioni razor).</span><span class="sxs-lookup"><span data-stu-id="18ffb-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="18ffb-129"><em>Facoltativo: * rimuovere il `ViewBag.Status` codice verso i controller di gestione e l'Account e dal *Views\Account\VerifyCode.cshtml</em> e <em>Views\Manage\VerifyPhoneNumber.cshtml</em> visualizzazioni razor.</span><span class="sxs-lookup"><span data-stu-id="18ffb-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="18ffb-130">In alternativa, è possibile mantenere il `ViewBag.Status` display per testare il funzionamento di questa app in locale senza dover associare e inviare messaggi SMS e posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="18ffb-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="18ffb-131">Avviso: Se si modifica una qualsiasi delle impostazioni di sicurezza in questo esempio, le app di produzione saranno necessario eseguire un controllo di sicurezza che effettua chiamate in modo esplicito le modifiche apportate.</span><span class="sxs-lookup"><span data-stu-id="18ffb-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="18ffb-132">Configurazione di SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="18ffb-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="18ffb-133">Questa esercitazione offre istruzioni per l'uso di Twilio o ASPSMS ma è possibile usare qualsiasi altro provider SMS.</span><span class="sxs-lookup"><span data-stu-id="18ffb-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="18ffb-134">**Creazione di un Account utente con un provider SMS**</span><span class="sxs-lookup"><span data-stu-id="18ffb-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="18ffb-135">Creare un [Twilio](https://www.twilio.com/try-twilio) o un' [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span><span class="sxs-lookup"><span data-stu-id="18ffb-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="18ffb-136">**Installare pacchetti aggiuntivi o per aggiungere i riferimenti al servizio**</span><span class="sxs-lookup"><span data-stu-id="18ffb-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="18ffb-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="18ffb-137">Twilio:</span></span>  
   <span data-ttu-id="18ffb-138">Nella Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="18ffb-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="18ffb-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="18ffb-139">ASPSMS:</span></span>  
   <span data-ttu-id="18ffb-140">È necessario aggiungere il riferimento al servizio seguenti:</span><span class="sxs-lookup"><span data-stu-id="18ffb-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="18ffb-141">Indirizzo:</span><span class="sxs-lookup"><span data-stu-id="18ffb-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="18ffb-142">Spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="18ffb-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="18ffb-143">**Scoprire le credenziali utente del Provider SMS**</span><span class="sxs-lookup"><span data-stu-id="18ffb-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="18ffb-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="18ffb-144">Twilio:</span></span>  
   <span data-ttu-id="18ffb-145">Dal **Dashboard** scheda dell'account Twilio, copiare la **SID Account** e **token di autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="18ffb-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="18ffb-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="18ffb-146">ASPSMS:</span></span>  
   <span data-ttu-id="18ffb-147">Da impostazioni dell'account, passare a **Userkey** e copiarlo insieme l'autodefinito **Password**.</span><span class="sxs-lookup"><span data-stu-id="18ffb-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="18ffb-148">Questi valori in un secondo momento verranno archiviati nelle variabili `SMSAccountIdentification` e `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="18ffb-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="18ffb-149">**Specifica l'ID mittente / creatore**</span><span class="sxs-lookup"><span data-stu-id="18ffb-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="18ffb-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="18ffb-150">Twilio:</span></span>  
   <span data-ttu-id="18ffb-151">Dal **numeri** scheda, copiare il numero di telefono di Twilio.</span><span class="sxs-lookup"><span data-stu-id="18ffb-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="18ffb-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="18ffb-152">ASPSMS:</span></span>  
   <span data-ttu-id="18ffb-153">All'interno di **lo sblocco dei creatori** Menu, sbloccare una o più entità di origine o scegliere un creatore alfanumerico (non supportato da tutte le reti).</span><span class="sxs-lookup"><span data-stu-id="18ffb-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="18ffb-154">Questo valore in un secondo momento verrà memorizzata nella variabile `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="18ffb-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="18ffb-155">**Il trasferimento di credenziali del provider SMS in app**</span><span class="sxs-lookup"><span data-stu-id="18ffb-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="18ffb-156">Rendere disponibile l'app per le credenziali e il numero di telefono mittente:</span><span class="sxs-lookup"><span data-stu-id="18ffb-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="18ffb-157">Security - store mai i dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="18ffb-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="18ffb-158">L'account e le credenziali vengono aggiunti al codice precedente per mantenere semplice l'esempio.</span><span class="sxs-lookup"><span data-stu-id="18ffb-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="18ffb-159">Vedere di Jon Atten [ASP.NET MVC: mantenere le impostazioni Private fuori controllo del codice sorgente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="18ffb-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="18ffb-160">**Implementazione di trasferimento dei dati al provider SMS**</span><span class="sxs-lookup"><span data-stu-id="18ffb-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="18ffb-161">Configurare il `SmsService` classe la *App\_Start\IdentityConfig.cs* file.</span><span class="sxs-lookup"><span data-stu-id="18ffb-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="18ffb-162">A seconda del provider SMS usato attivare i **Twilio** o nella **ASPSMS** sezione:</span><span class="sxs-lookup"><span data-stu-id="18ffb-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="18ffb-163">Eseguire l'app e accedere con l'account registrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="18ffb-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="18ffb-164">Fare clic sul proprio ID utente, che si attiva la `Index` metodo di azione `Manage` controller.</span><span class="sxs-lookup"><span data-stu-id="18ffb-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="18ffb-165">Fare clic su Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="18ffb-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="18ffb-166">In pochi secondi si riceverà un messaggio di testo con il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="18ffb-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="18ffb-167">Immetterla e premere **Submit**.</span><span class="sxs-lookup"><span data-stu-id="18ffb-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="18ffb-168">La visualizzazione di gestione mostra che il numero di telefono è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="18ffb-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="18ffb-169">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="18ffb-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="18ffb-170">Il `Index` metodo di azione `Manage` controller imposta il messaggio di stato dell'azione precedente in base e vengono forniti collegamenti per modificare la password locale o aggiungere un account locale.</span><span class="sxs-lookup"><span data-stu-id="18ffb-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="18ffb-171">Il `Index` metodo visualizza inoltre lo stato o le 2FA phone numero, account di accesso esterni, abilitato 2FA e ricordare metodo 2FA per questo browser (descritto più avanti).</span><span class="sxs-lookup"><span data-stu-id="18ffb-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="18ffb-172">Facendo clic sul proprio ID utente (indirizzo di posta elettronica) nella barra del titolo non passare un messaggio.</span><span class="sxs-lookup"><span data-stu-id="18ffb-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="18ffb-173">Facendo clic sui **numero di telefono: rimuovere** collegare passate `Message=RemovePhoneSuccess` come una stringa di query.</span><span class="sxs-lookup"><span data-stu-id="18ffb-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="18ffb-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="18ffb-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="18ffb-175">Il `AddPhoneNumber` metodo di azione consente di visualizzare una finestra di dialogo per immettere un numero di telefono che può ricevere messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="18ffb-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="18ffb-176">Facendo clic sui **Invia codice di verifica** pulsante Invia il numero di telefono per la richiesta HTTP POST `AddPhoneNumber` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="18ffb-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="18ffb-177">Il `GenerateChangePhoneNumberTokenAsync` metodo genera il token di sicurezza che verrà impostato nel messaggio SMS.</span><span class="sxs-lookup"><span data-stu-id="18ffb-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="18ffb-178">Se il servizio SMS è stato configurato, il token viene inviato come stringa &quot;il codice diprotezione è &lt;token&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="18ffb-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="18ffb-179">Il `SmsService.SendAsync` metodo viene chiamato in modo asincrono, quindi viene reindirizzato all'app per la `VerifyPhoneNumber` metodo di azione (che consente di visualizzare la finestra di dialogo seguente), in cui è possibile immettere il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="18ffb-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="18ffb-180">Quando si immette il codice e fare clic su Invia, il codice viene pubblicato per la richiesta HTTP POST `VerifyPhoneNumber` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="18ffb-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="18ffb-181">Il `ChangePhoneNumberAsync` metodo controlla il codice di sicurezza registrate.</span><span class="sxs-lookup"><span data-stu-id="18ffb-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="18ffb-182">Se il codice sia corretto, il numero di telefono viene aggiunto per il `PhoneNumber` campo del `AspNetUsers` tabella.</span><span class="sxs-lookup"><span data-stu-id="18ffb-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="18ffb-183">Se tale chiamata ha esito positivo, il `SignInAsync` viene chiamato il metodo:</span><span class="sxs-lookup"><span data-stu-id="18ffb-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="18ffb-184">Il `isPersistent` parametro determina se la sessione di autenticazione è persistente tra più richieste.</span><span class="sxs-lookup"><span data-stu-id="18ffb-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="18ffb-185">Quando si modifica il profilo di sicurezza, un indicatore di sicurezza verrà generato e archiviato nella `SecurityStamp` campo le *AspNetUsers* tabella.</span><span class="sxs-lookup"><span data-stu-id="18ffb-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="18ffb-186">Si noti che il `SecurityStamp` campo è diverso dal cookie di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="18ffb-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="18ffb-187">Il cookie di sicurezza non verrà memorizzato nel `AspNetUsers` tabella (o qualsiasi altro nel database di identità).</span><span class="sxs-lookup"><span data-stu-id="18ffb-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="18ffb-188">Il token di cookie di sicurezza è autofirmato usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e viene creato con il `UserId, SecurityStamp` e informazioni sull'ora di scadenza.</span><span class="sxs-lookup"><span data-stu-id="18ffb-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="18ffb-189">Il middleware dei cookie controlla il cookie a ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="18ffb-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="18ffb-190">Il `SecurityStampValidator` metodo nella `Startup` classe raggiunge il database e controlla periodicamente, indicatore di sicurezza come specificato con il `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="18ffb-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="18ffb-191">Questo avviene ogni 30 minuti (in questo esempio) solo se non si modifica il profilo di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="18ffb-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="18ffb-192">Per ridurre al minimo trip al database è stato scelto l'intervallo di 30 minuti.</span><span class="sxs-lookup"><span data-stu-id="18ffb-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="18ffb-193">Il `SignInAsync` metodo deve essere chiamato quando si apportano modifiche al profilo di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="18ffb-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="18ffb-194">Quando viene modificato il profilo di sicurezza, il database sia gli aggiornamenti il `SecurityStamp` campo e senza chiamare il `SignInAsync` metodo potrebbe rimanere connessi a *solo* fino all'esecuzione successiva della pipeline OWIN acceda al database (la `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="18ffb-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="18ffb-195">È possibile verificarlo modificando la `SignInAsync` per restituire immediatamente e l'impostazione del cookie `validateInterval` proprietà da 30 minuti su 5 secondi:</span><span class="sxs-lookup"><span data-stu-id="18ffb-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="18ffb-196">Con le modifiche al codice precedente, è possibile modificare il profilo di sicurezza (ad esempio, modificando lo stato del **due fattori abilitati**) e verrà disconnesso entro 5 secondi durante il `SecurityStampValidator.OnValidateIdentity` metodo ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="18ffb-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="18ffb-197">Rimozione del ritorno a capo nel `SignInAsync` metodo, modificare un altro profilo di sicurezza e non verrà disconnesso. Il `SignInAsync` metodo genera un nuovo cookie di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="18ffb-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="18ffb-198">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="18ffb-198">Enable two-factor authentication</span></span>

<span data-ttu-id="18ffb-199">Nell'app di esempio, è necessario usare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA).</span><span class="sxs-lookup"><span data-stu-id="18ffb-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="18ffb-200">Per abilitare 2FA, fare clic sul proprio ID utente (alias di posta elettronica) nella barra di spostamento.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="18ffb-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="18ffb-201">Fare clic su Abilita 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="18ffb-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="18ffb-202">Effettuare la disconnessione, quindi eseguire nuovamente l'accesso.</span><span class="sxs-lookup"><span data-stu-id="18ffb-202">Log out, then log back in.</span></span> <span data-ttu-id="18ffb-203">Se è stato abilitato posta elettronica (vedere la [esercitazione precedente](account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare il SMS o posta elettronica di autenticazione 2fa.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="18ffb-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="18ffb-204">Verrà visualizzata la pagina di verifica codice in cui è possibile immettere il codice (dall'indirizzo di posta elettronica o SMS).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="18ffb-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="18ffb-205">Facendo clic sui **memorizzare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso con tale computer e browser.</span><span class="sxs-lookup"><span data-stu-id="18ffb-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="18ffb-206">Abilitare 2FA e facendo clic sulla **memorizzare questo browser** fornirà protezione 2FA sicuro da utenti malintenzionati che tentano di accedere all'account, fino a quando non hanno accesso al computer in uso.</span><span class="sxs-lookup"><span data-stu-id="18ffb-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="18ffb-207">È possibile farlo in qualsiasi computer privati che vengono utilizzate regolarmente.</span><span class="sxs-lookup"><span data-stu-id="18ffb-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="18ffb-208">Impostando **memorizzare questo browser**, si ottiene le funzionalità di sicurezza di 2FA dai computer non si usa regolarmente, e si ottiene la comodità nel non dover passare attraverso 2FA nei propri computer.</span><span class="sxs-lookup"><span data-stu-id="18ffb-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="18ffb-209">Come registrare un provider di autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="18ffb-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="18ffb-210">Quando si crea un nuovo progetto MVC, il *IdentityConfig.cs* file contiene il codice seguente per registrare un provider di autenticazione a due fattori:</span><span class="sxs-lookup"><span data-stu-id="18ffb-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="18ffb-211">Aggiungere un numero di telefono autenticazione 2fa</span><span class="sxs-lookup"><span data-stu-id="18ffb-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="18ffb-212">Il `AddPhoneNumber` metodo di azione di `Manage` controller genera un token di sicurezza e inviati per il telefono numero che è sono specificato.</span><span class="sxs-lookup"><span data-stu-id="18ffb-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="18ffb-213">Dopo avere inviato il token, reindirizza al `VerifyPhoneNumber` metodo di azione, in cui è possibile immettere il codice per la registrazione SMS autenticazione 2fa.</span><span class="sxs-lookup"><span data-stu-id="18ffb-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="18ffb-214">2FA SMS non viene utilizzato fino a quando non è stato verificato il numero di telefono.</span><span class="sxs-lookup"><span data-stu-id="18ffb-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="18ffb-215">Abilitare 2FA</span><span class="sxs-lookup"><span data-stu-id="18ffb-215">Enabling 2FA</span></span>

<span data-ttu-id="18ffb-216">Il `EnableTFA` 2FA consente a metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="18ffb-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="18ffb-217">Si noti il `SignInAsync` deve essere chiamato perché abilitare 2FA viene apportata una modifica al profilo di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="18ffb-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="18ffb-218">Quando è abilitato 2FA, l'utente dovrà usare 2FA per l'accesso, utilizzando gli approcci 2FA ha registrato (SMS e posta elettronica nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="18ffb-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="18ffb-219">È possibile aggiungere ulteriori provider 2FA, ad esempio i generatori di codice a matrice oppure è possibile scrivere si è proprietari (vedere [tramite Google Authenticator con ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="18ffb-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="18ffb-220">I codici 2FA vengono generati usando [basati sul tempo algoritmo Password monouso](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) e i codici sono validi per sei minuti.</span><span class="sxs-lookup"><span data-stu-id="18ffb-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="18ffb-221">Se si hanno più di sei minuti per immettere il codice, si riceverà un messaggio di errore di codice non valido.</span><span class="sxs-lookup"><span data-stu-id="18ffb-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="18ffb-222">Combinare gli account di accesso social e locali</span><span class="sxs-lookup"><span data-stu-id="18ffb-222">Combine social and local login accounts</span></span>

<span data-ttu-id="18ffb-223">È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="18ffb-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="18ffb-224">Nella sequenza seguente &quot; RickAndMSFT@gmail.com &quot; viene innanzitutto creato un account di accesso locale, ma è possibile creare l'account come log basati su social network in prima, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="18ffb-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="18ffb-225">Fare clic sui **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="18ffb-225">Click on the **Manage** link.</span></span> <span data-ttu-id="18ffb-226">Si noti 0 esterno (account di accesso social) associata all'account.</span><span class="sxs-lookup"><span data-stu-id="18ffb-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="18ffb-227">Fare clic sul collegamento a un altro registro nel servizio e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="18ffb-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="18ffb-228">I due account sono stati combinati, sarà in grado di accedere con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="18ffb-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="18ffb-229">È possibile che gli utenti per aggiungere gli account locali nel caso in cui i log basati su social network in servizio di autenticazione è inattivo oppure più probabile che si è perso l'accesso al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="18ffb-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="18ffb-230">Nell'immagine seguente, Tom è una procedura di accesso basati su social network (che è possibile visualizzare dal **account di accesso esterni: 1** visualizzato sulla pagina).</span><span class="sxs-lookup"><span data-stu-id="18ffb-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="18ffb-231">Facendo clic su **Scegli una password** consente di aggiungere un registro locale in associati con lo stesso account.</span><span class="sxs-lookup"><span data-stu-id="18ffb-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="18ffb-232">Blocco degli account da attacchi di forza bruta</span><span class="sxs-lookup"><span data-stu-id="18ffb-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="18ffb-233">È possibile proteggere gli account nella tua app da attacchi con dizionario, consentendo di blocco dell'utente.</span><span class="sxs-lookup"><span data-stu-id="18ffb-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="18ffb-234">Il codice seguente il `ApplicationUserManager Create` metodo configura blocco out:</span><span class="sxs-lookup"><span data-stu-id="18ffb-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="18ffb-235">Il codice riportato sopra consente di blocco per solo l'autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="18ffb-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="18ffb-236">Sebbene sia possibile attivare è stata bloccata per gli account di accesso modificando `shouldLockout` su true nel `Login` metodo del controller di account, è consigliabile non è stata bloccata per gli account di accesso è attivato perché rende vulnerabili per l'account [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) account di accesso gli attacchi.</span><span class="sxs-lookup"><span data-stu-id="18ffb-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="18ffb-237">Nell'esempio di codice, blocco è disabilitato per l'account amministratore creato nel `ApplicationDbInitializer Seed` metodo:</span><span class="sxs-lookup"><span data-stu-id="18ffb-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="18ffb-238">Che l'utente dispone di un account di posta elettronica convalidato</span><span class="sxs-lookup"><span data-stu-id="18ffb-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="18ffb-239">Il codice seguente richiede un utente di un account di posta elettronica convalidato per poter accedere:</span><span class="sxs-lookup"><span data-stu-id="18ffb-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="18ffb-240">Come le verifiche per requisito 2FA SignInManager</span><span class="sxs-lookup"><span data-stu-id="18ffb-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="18ffb-241">Accesso locale sia basati su social network log di controllo per verificare se è abilitato 2FA.</span><span class="sxs-lookup"><span data-stu-id="18ffb-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="18ffb-242">Se è abilitato 2FA, il `SignInManager` metodo di accesso restituisce `SignInStatus.RequiresVerification`, e l'utente verrà reindirizzato al `SendCode` metodo di azione, in cui è necessario immettere il codice per completare il log in sequenza.</span><span class="sxs-lookup"><span data-stu-id="18ffb-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="18ffb-243">Se l'utente ha RememberMe è nastavit il cookie di utenti locale, il `SignInManager` restituirà `SignInStatus.Success` e non avranno passino 2FA.</span><span class="sxs-lookup"><span data-stu-id="18ffb-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="18ffb-244">Il codice seguente illustra il `SendCode` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="18ffb-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="18ffb-245">Oggetto [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) viene creato con tutti i metodi di 2FA abilitati per l'utente.</span><span class="sxs-lookup"><span data-stu-id="18ffb-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="18ffb-246">Il [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) viene passato per il [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, che consente all'utente di selezionare l'approccio 2FA (in genere tramite posta elettronica e SMS).</span><span class="sxs-lookup"><span data-stu-id="18ffb-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="18ffb-247">Dopo che l'utente registra l'approccio 2FA, il `HTTP POST SendCode` viene chiamato il metodo di azione, il `SignInManager` invia il codice 2FA e l'utente viene reindirizzato al `VerifyCode` metodo di azione in cui è possibile immettere il codice per completare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="18ffb-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="18ffb-248">Blocco 2FA</span><span class="sxs-lookup"><span data-stu-id="18ffb-248">2FA Lockout</span></span>

<span data-ttu-id="18ffb-249">Sebbene sia possibile impostare il blocco degli account in tentativi non riusciti password di account di accesso, tale approccio rende l'account di accesso possibili attacchi [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) dei blocchi.</span><span class="sxs-lookup"><span data-stu-id="18ffb-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="18ffb-250">È consigliabile che usare il blocco dell'account solo con 2FA.</span><span class="sxs-lookup"><span data-stu-id="18ffb-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="18ffb-251">Quando la `ApplicationUserManager` viene creato, il codice di esempio imposta blocco 2FA e `MaxFailedAccessAttemptsBeforeLockout` a cinque.</span><span class="sxs-lookup"><span data-stu-id="18ffb-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="18ffb-252">Una volta che un utente accede (tramite l'account locale o un account di social networking), viene archiviato ogni tentativo non riuscito in 2FA e se viene raggiunto il numero massimo di tentativi, l'utente viene bloccato cinque minuti (è possibile impostare il blocco tempo `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="18ffb-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="18ffb-253">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="18ffb-253">Additional Resources</span></span>

- <span data-ttu-id="18ffb-254">[Risorse consigliate su ASP.NET Identity](../getting-started/aspnet-identity-recommended-resources.md) elenco completo dei blog di identità, video, esercitazioni e great quindi collega.</span><span class="sxs-lookup"><span data-stu-id="18ffb-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="18ffb-255">[App MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) viene inoltre illustrato come aggiungere informazioni sul profilo per la tabella degli utenti.</span><span class="sxs-lookup"><span data-stu-id="18ffb-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="18ffb-256">[ASP.NET MVC e identità 2.0: nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) da John Atten.</span><span class="sxs-lookup"><span data-stu-id="18ffb-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="18ffb-257">Conferma account e recupero della Password con ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="18ffb-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="18ffb-258">Introduzione ad ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="18ffb-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="18ffb-259">[Annuncio della versione RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) di Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="18ffb-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="18ffb-260">[Identità di ASP.NET 2.0: Configurazione di convalida dell'Account e l'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) da John Atten.</span><span class="sxs-lookup"><span data-stu-id="18ffb-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
