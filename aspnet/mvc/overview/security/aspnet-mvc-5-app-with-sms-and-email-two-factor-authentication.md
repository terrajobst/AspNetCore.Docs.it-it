---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: App ASP.NET MVC 5 con SMS e posta elettronica l'autenticazione a due fattori | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come compilare un'app web ASP.NET MVC 5 con autenticazione a due fattori. È necessario completare l'app web crea un ASP.NET MVC 5 sicura con...
ms.author: aspnetcontent
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4fd0091effcf2cc0517da91922981e49ef0eef5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803207"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="76a13-104">App ASP.NET MVC 5 con SMS e posta elettronica l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="76a13-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="76a13-105">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="76a13-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="76a13-106">Questa esercitazione illustra come compilare un'app web ASP.NET MVC 5 con autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="76a13-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="76a13-107">È consigliabile completare [creare un'app web ASP.NET MVC 5 sicura con accesso, messaggio di posta elettronica conferma e reimpostazione della password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima di procedere.</span><span class="sxs-lookup"><span data-stu-id="76a13-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="76a13-108">È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="76a13-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="76a13-109">Il download contiene gli helper di debug che consentono di testare conferma tramite posta elettronica e SMS senza dover configurare un indirizzo di posta elettronica o il provider SMS.</span><span class="sxs-lookup"><span data-stu-id="76a13-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="76a13-110">Questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="76a13-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="76a13-111">Creare un'app ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="76a13-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="76a13-112">Configurazione di SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="76a13-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="76a13-113">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="76a13-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="76a13-114">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="76a13-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="76a13-115">Creare un'app ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="76a13-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="76a13-116">Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="76a13-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="76a13-117">Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="76a13-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="76a13-118">Avviso: È consigliabile completare [creare un'app web ASP.NET MVC 5 sicura con accesso, messaggio di posta elettronica conferma e reimpostazione della password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima di procedere.</span><span class="sxs-lookup"><span data-stu-id="76a13-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="76a13-119">È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="76a13-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="76a13-120">Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC.</span><span class="sxs-lookup"><span data-stu-id="76a13-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="76a13-121">Web Form supporta inoltre ASP.NET Identity, pertanto è possibile seguire la procedura in un'applicazione web form.</span><span class="sxs-lookup"><span data-stu-id="76a13-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="76a13-122">Lasciare l'autenticazione predefinita come **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="76a13-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="76a13-123">Se vuoi ospitare l'app in Azure, lasciare selezionata la casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="76a13-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="76a13-124">Più avanti nell'esercitazione verrà distribuita in Azure.</span><span class="sxs-lookup"><span data-stu-id="76a13-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="76a13-125">È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="76a13-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="76a13-126">Impostare il [progetto per l'uso di SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="76a13-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="76a13-127">Configurazione di SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="76a13-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="76a13-128">Questa esercitazione offre istruzioni per l'uso di Twilio o ASPSMS ma è possibile usare qualsiasi altro provider SMS.</span><span class="sxs-lookup"><span data-stu-id="76a13-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="76a13-129">**Creazione di un Account utente con un provider SMS**</span><span class="sxs-lookup"><span data-stu-id="76a13-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="76a13-130">Creare un [Twilio](https://www.twilio.com/try-twilio) o un' [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span><span class="sxs-lookup"><span data-stu-id="76a13-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="76a13-131">**Installare pacchetti aggiuntivi o per aggiungere i riferimenti al servizio**</span><span class="sxs-lookup"><span data-stu-id="76a13-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="76a13-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="76a13-132">Twilio:</span></span>  
   <span data-ttu-id="76a13-133">Nella Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="76a13-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="76a13-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="76a13-134">ASPSMS:</span></span>  
   <span data-ttu-id="76a13-135">È necessario aggiungere il riferimento al servizio seguenti:</span><span class="sxs-lookup"><span data-stu-id="76a13-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="76a13-136">Indirizzo:</span><span class="sxs-lookup"><span data-stu-id="76a13-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="76a13-137">Spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="76a13-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="76a13-138">**Scoprire le credenziali utente del Provider SMS**</span><span class="sxs-lookup"><span data-stu-id="76a13-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="76a13-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="76a13-139">Twilio:</span></span>  
   <span data-ttu-id="76a13-140">Dal **Dashboard** scheda dell'account Twilio, copiare la **SID Account** e **token di autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="76a13-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="76a13-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="76a13-141">ASPSMS:</span></span>  
   <span data-ttu-id="76a13-142">Da impostazioni dell'account, passare a **Userkey** e copiarlo insieme l'autodefinito **Password**.</span><span class="sxs-lookup"><span data-stu-id="76a13-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="76a13-143">In un secondo momento verranno archiviati i valori nel *Web. config* file tra quelle `"SMSAccountIdentification"` e `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="76a13-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="76a13-144">**Specifica l'ID mittente / creatore**</span><span class="sxs-lookup"><span data-stu-id="76a13-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="76a13-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="76a13-145">Twilio:</span></span>  
   <span data-ttu-id="76a13-146">Dal **numeri** scheda, copiare il numero di telefono di Twilio.</span><span class="sxs-lookup"><span data-stu-id="76a13-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="76a13-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="76a13-147">ASPSMS:</span></span>  
   <span data-ttu-id="76a13-148">All'interno di **lo sblocco dei creatori** Menu, sbloccare una o più entità di origine o scegliere un creatore alfanumerico (non supportato da tutte le reti).</span><span class="sxs-lookup"><span data-stu-id="76a13-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="76a13-149">In un secondo momento si archivierà questo valore nel *Web. config* file all'interno della chiave `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="76a13-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="76a13-150">**Il trasferimento di credenziali del provider SMS in app**</span><span class="sxs-lookup"><span data-stu-id="76a13-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="76a13-151">Rendere disponibile l'app per le credenziali e il numero di telefono mittente.</span><span class="sxs-lookup"><span data-stu-id="76a13-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="76a13-152">Per semplificare le operazioni verranno archiviati i valori nel *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="76a13-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="76a13-153">Quando si distribuisce in Azure, è possibile archiviare i valori in modo sicuro nel **le impostazioni dell'app** scheda Configura sezione sul sito web.</span><span class="sxs-lookup"><span data-stu-id="76a13-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="76a13-154">Security - store mai i dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="76a13-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="76a13-155">L'account e le credenziali vengono aggiunti al codice precedente per mantenere semplice l'esempio.</span><span class="sxs-lookup"><span data-stu-id="76a13-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="76a13-156">Visualizzare [procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="76a13-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="76a13-157">**Implementazione di trasferimento dei dati al provider SMS**</span><span class="sxs-lookup"><span data-stu-id="76a13-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="76a13-158">Configurare il `SmsService` classe la *App\_Start\IdentityConfig.cs* file.</span><span class="sxs-lookup"><span data-stu-id="76a13-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="76a13-159">A seconda del provider SMS usato attivare i **Twilio** o nella **ASPSMS** sezione:</span><span class="sxs-lookup"><span data-stu-id="76a13-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="76a13-160">Aggiorna il *Views\Manage\Index.cshtml* visualizzazione Razor: (Nota: non è sufficiente rimuovere i commenti nel codice esistente, usare il codice seguente.)</span><span class="sxs-lookup"><span data-stu-id="76a13-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="76a13-161">Verificare i `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` metodi di azione nel `ManageController` hanno il[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attributo:</span><span class="sxs-lookup"><span data-stu-id="76a13-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="76a13-162">Eseguire l'app e accedere con l'account registrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="76a13-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="76a13-163">Fare clic sul proprio ID utente, che si attiva la `Index` metodo di azione `Manage` controller.</span><span class="sxs-lookup"><span data-stu-id="76a13-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="76a13-164">Fare clic su Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="76a13-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="76a13-165">Il `AddPhoneNumber` metodo di azione consente di visualizzare una finestra di dialogo per immettere un numero di telefono che può ricevere messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="76a13-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="76a13-166">In pochi secondi si riceverà un messaggio di testo con il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="76a13-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="76a13-167">Immetterla e premere **Submit**.</span><span class="sxs-lookup"><span data-stu-id="76a13-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="76a13-168">La visualizzazione di gestione mostra che il numero di telefono è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="76a13-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="76a13-169">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="76a13-169">Enable two-factor authentication</span></span>

<span data-ttu-id="76a13-170">Nell'app modello generato, è necessario usare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA).</span><span class="sxs-lookup"><span data-stu-id="76a13-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="76a13-171">Per abilitare 2FA, fare clic sul proprio ID utente (alias di posta elettronica) nella barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="76a13-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="76a13-172">Fare clic su Abilita 2FA.</span><span class="sxs-lookup"><span data-stu-id="76a13-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="76a13-173">Punto di disconnessione, quindi accedere di nuovo.</span><span class="sxs-lookup"><span data-stu-id="76a13-173">Logout, then log back in.</span></span> <span data-ttu-id="76a13-174">Se è stato abilitato posta elettronica (vedere la [esercitazione precedente](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare il SMS o posta elettronica di autenticazione 2fa.</span><span class="sxs-lookup"><span data-stu-id="76a13-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="76a13-175">Verrà visualizzata la pagina di verifica codice in cui è possibile immettere il codice (dall'indirizzo di posta elettronica o SMS).</span><span class="sxs-lookup"><span data-stu-id="76a13-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="76a13-176">Facendo clic sui **memorizzare questo browser** casella di controllo sarà esente dalla necessità di usare 2FA per l'accesso quando si usa il browser e dispositivi in cui è stata selezionata la casella.</span><span class="sxs-lookup"><span data-stu-id="76a13-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="76a13-177">Fino a quando gli utenti malintenzionati non è possibile ottenere l'accesso al dispositivo, abilitare 2FA e facendo clic sui **memorizzare questo browser** fornirà pratico accesso tramite password di un unico passaggio, mantenendo comunque la protezione avanzata 2FA per tutti gli accessi dai dispositivi non attendibili.</span><span class="sxs-lookup"><span data-stu-id="76a13-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="76a13-178">È possibile farlo in qualsiasi dispositivo privata che vengono utilizzate regolarmente.</span><span class="sxs-lookup"><span data-stu-id="76a13-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="76a13-179">Questa esercitazione offre un'introduzione rapida abilitare 2FA in una nuova app ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="76a13-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="76a13-180">L'esercitazione [autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) descrive in dettaglio il codice dietro il codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="76a13-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="76a13-181">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="76a13-181">Additional Resources</span></span>

- <span data-ttu-id="76a13-182">[Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) descrive in dettaglio l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="76a13-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="76a13-183">Risorse consigliate su collegamenti ad ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="76a13-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="76a13-184">[Conferma dell'account e recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) descrivono in maggiore dettaglio nella conferma account e di ripristino della password.</span><span class="sxs-lookup"><span data-stu-id="76a13-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="76a13-185">[App MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) questa esercitazione illustra come scrivere un'app ASP.NET MVC 5 con Facebook e Google OAuth 2 l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="76a13-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="76a13-186">Viene inoltre illustrato come aggiungere dati aggiuntivi al database di identità.</span><span class="sxs-lookup"><span data-stu-id="76a13-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="76a13-187">[Distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e Database SQL di Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="76a13-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="76a13-188">Questa esercitazione aggiunge una distribuzione di Azure, come proteggere l'app con i ruoli, come usare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="76a13-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="76a13-189">Creazione di un'app Google per OAuth 2 e ci si connette l'app al progetto</span><span class="sxs-lookup"><span data-stu-id="76a13-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="76a13-190">Creazione dell'app in Facebook e ci si connette l'app al progetto</span><span class="sxs-lookup"><span data-stu-id="76a13-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="76a13-191">Configurare SSL nel progetto</span><span class="sxs-lookup"><span data-stu-id="76a13-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
