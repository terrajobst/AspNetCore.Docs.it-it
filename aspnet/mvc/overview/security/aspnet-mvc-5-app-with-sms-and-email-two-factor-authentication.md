---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: App ASP.NET MVC 5 con SMS e l'autenticazione a due fattori di posta elettronica | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione viene illustrato come compilare un'app web ASP.NET MVC 5 con autenticazione a due fattori. È necessario completare l'applicazione web crea un sicuro ASP.NET MVC 5 con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: db57b8fe44f41d65d27964f45e0884138629f92b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="161ca-104">App ASP.NET MVC 5 con SMS e l'autenticazione a due fattori di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="161ca-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="161ca-105">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="161ca-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="161ca-106">In questa esercitazione viene illustrato come compilare un'app web ASP.NET MVC 5 con autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="161ca-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="161ca-107">È consigliabile completare [creare un'app web di ASP.NET MVC 5 sicura con l'accesso, la reimpostazione di posta elettronica di conferma e la password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima di procedere.</span><span class="sxs-lookup"><span data-stu-id="161ca-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="161ca-108">È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="161ca-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="161ca-109">Il download contiene funzioni di supporto di debug che consentono di testare conferma tramite posta elettronica e SMS senza dover configurare un messaggio di posta elettronica o il provider SMS.</span><span class="sxs-lookup"><span data-stu-id="161ca-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="161ca-110">In questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="161ca-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="161ca-111">Creare un'applicazione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="161ca-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="161ca-112">Impostare SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="161ca-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="161ca-113">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="161ca-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="161ca-114">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="161ca-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="161ca-115">Creare un'applicazione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="161ca-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="161ca-116">Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="161ca-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="161ca-117">Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="161ca-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="161ca-118">Avviso: È necessario completare [creare un'app web di ASP.NET MVC 5 sicura con l'accesso, la reimpostazione di posta elettronica di conferma e la password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima di procedere.</span><span class="sxs-lookup"><span data-stu-id="161ca-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="161ca-119">È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="161ca-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="161ca-120">Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC.</span><span class="sxs-lookup"><span data-stu-id="161ca-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="161ca-121">Web Form supporta anche ASP.NET Identity, pertanto è possibile attenersi alla procedura simile in un'app di web form.</span><span class="sxs-lookup"><span data-stu-id="161ca-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="161ca-122">Lasciare l'autenticazione predefinita come **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="161ca-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="161ca-123">Se si desidera ospitare l'applicazione in Azure, lasciare selezionata la casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="161ca-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="161ca-124">Più avanti nell'esercitazione si distribuirà in Azure.</span><span class="sxs-lookup"><span data-stu-id="161ca-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="161ca-125">È possibile [aprire un account Azure, gratuitamente](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="161ca-125">You can [open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="161ca-126">Impostare il [progetto utilizzare SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="161ca-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="161ca-127">Impostare SMS per l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="161ca-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="161ca-128">In questa esercitazione vengono fornite istruzioni per l'utilizzo di Twilio o ASPSMS ma è possibile usare qualsiasi altro provider SMS.</span><span class="sxs-lookup"><span data-stu-id="161ca-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="161ca-129">**Creazione di un Account utente con un provider SMS**</span><span class="sxs-lookup"><span data-stu-id="161ca-129">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="161ca-130">Creare un [Twilio](https://www.twilio.com/try-twilio) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span><span class="sxs-lookup"><span data-stu-id="161ca-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="161ca-131">**Installazione di pacchetti aggiuntivi o l'aggiunta di riferimenti al servizio**</span><span class="sxs-lookup"><span data-stu-id="161ca-131">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="161ca-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="161ca-132">Twilio:</span></span>  
 <span data-ttu-id="161ca-133">Nella Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="161ca-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="161ca-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="161ca-134">ASPSMS:</span></span>  
 <span data-ttu-id="161ca-135">È necessario aggiungere il riferimento al servizio seguente:</span><span class="sxs-lookup"><span data-stu-id="161ca-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 <span data-ttu-id="161ca-136">Indirizzo:</span><span class="sxs-lookup"><span data-stu-id="161ca-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="161ca-137">Spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="161ca-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="161ca-138">**Capire le credenziali utente del Provider SMS**</span><span class="sxs-lookup"><span data-stu-id="161ca-138">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="161ca-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="161ca-139">Twilio:</span></span>  
 <span data-ttu-id="161ca-140">Dal **Dashboard** scheda dell'account di Twilio, copia il **SID di Account** e **token di autorizzazione**.</span><span class="sxs-lookup"><span data-stu-id="161ca-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="161ca-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="161ca-141">ASPSMS:</span></span>  
 <span data-ttu-id="161ca-142">Le impostazioni dell'account, passare alla **parametri Userkey** e copiarlo insieme il Self-definito **Password**.</span><span class="sxs-lookup"><span data-stu-id="161ca-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="161ca-143">Questi valori in un secondo momento verrà archiviato il *Web. config* file all'interno di chiavi `"SMSAccountIdentification"` e `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="161ca-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="161ca-144">**Specifica l'ID mittente / iniziatore**</span><span class="sxs-lookup"><span data-stu-id="161ca-144">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="161ca-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="161ca-145">Twilio:</span></span>  
 <span data-ttu-id="161ca-146">Dal **numeri** scheda, copiare il numero di telefono di Twilio.</span><span class="sxs-lookup"><span data-stu-id="161ca-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="161ca-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="161ca-147">ASPSMS:</span></span>  
 <span data-ttu-id="161ca-148">All'interno di **dei creatori di sbloccare** Menu, sbloccare una o più mittenti o scegliere un creatore alfanumerico (non supportato da tutte le reti).</span><span class="sxs-lookup"><span data-stu-id="161ca-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="161ca-149">Questo valore in un secondo momento verrà archiviato il *Web. config* file all'interno della chiave `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="161ca-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="161ca-150">**Il trasferimento delle credenziali del provider SMS in app**</span><span class="sxs-lookup"><span data-stu-id="161ca-150">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="161ca-151">Le credenziali e il numero di telefono mittente rendere disponibile l'app.</span><span class="sxs-lookup"><span data-stu-id="161ca-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="161ca-152">Per semplificare le operazioni, questi valori in verrà archiviato il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="161ca-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="161ca-153">Quando si distribuisce in Azure, che è possibile archiviare i valori in modo sicuro nel **impostazioni app** scheda Configura sezione sul sito web.</span><span class="sxs-lookup"><span data-stu-id="161ca-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="161ca-154">Sicurezza - archivio mai i dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="161ca-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="161ca-155">L'account e le credenziali vengono aggiunti al codice precedente per semplificare il codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="161ca-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="161ca-156">Vedere [procedure consigliate per la distribuzione delle password e altri dati sensibili su ASP.NET e in Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="161ca-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="161ca-157">**Implementazione di trasferimento dei dati al provider SMS**</span><span class="sxs-lookup"><span data-stu-id="161ca-157">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="161ca-158">Configurare il `SmsService` classe il *App\_Start\IdentityConfig.cs* file.</span><span class="sxs-lookup"><span data-stu-id="161ca-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="161ca-159">A seconda del provider SMS utilizzato attivare il **Twilio** o **ASPSMS** sezione:</span><span class="sxs-lookup"><span data-stu-id="161ca-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="161ca-160">Aggiornamento di *Views\Manage\Index.cshtml* visualizzazione Razor: (Nota: non è sufficiente rimuovere i commenti nel codice esistente, utilizzare il codice riportato di seguito.)</span><span class="sxs-lookup"><span data-stu-id="161ca-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="161ca-161">Verificare il `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` metodi di azione nel `ManageController` hanno il[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attributo:</span><span class="sxs-lookup"><span data-stu-id="161ca-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="161ca-162">Eseguire l'app e accedere con l'account che è stato registrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="161ca-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="161ca-163">Fare clic sull'ID utente, che attiva il `Index` metodo di azione `Manage` controller.</span><span class="sxs-lookup"><span data-stu-id="161ca-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="161ca-164">Fare clic su Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="161ca-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="161ca-165">Il `AddPhoneNumber` metodo di azione consente di visualizzare una finestra di dialogo per immettere un numero di telefono che può ricevere i messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="161ca-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="161ca-166">In pochi secondi si otterrà un messaggio di testo con il codice di verifica.</span><span class="sxs-lookup"><span data-stu-id="161ca-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="161ca-167">L'immissione e premere **Invia**.</span><span class="sxs-lookup"><span data-stu-id="161ca-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="161ca-168">La visualizzazione Gestione mostra che il numero di telefono è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="161ca-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="161ca-169">Abilitare l'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="161ca-169">Enable two-factor authentication</span></span>

<span data-ttu-id="161ca-170">Nell'app modello generato, è necessario utilizzare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA).</span><span class="sxs-lookup"><span data-stu-id="161ca-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="161ca-171">Per abilitare 2FA, fare clic sull'ID utente (alias di posta elettronica) nella barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="161ca-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="161ca-172">Fare clic su Abilita 2FA.</span><span class="sxs-lookup"><span data-stu-id="161ca-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="161ca-173">Disconnessione, quindi accedere di nuovo.</span><span class="sxs-lookup"><span data-stu-id="161ca-173">Logout, then log back in.</span></span> <span data-ttu-id="161ca-174">Se è stato abilitato posta elettronica (vedere il [esercitazione precedente](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare il SMS o posta elettronica per 2FA.</span><span class="sxs-lookup"><span data-stu-id="161ca-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="161ca-175">Verrà visualizzata la pagina di verifica di codice in cui è possibile immettere il codice (da posta elettronica o SMS).</span><span class="sxs-lookup"><span data-stu-id="161ca-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="161ca-176">Facendo clic su di **ricordare questo browser** casella di controllo sarà esente dalla necessità di eseguire l'accesso quando si utilizza il browser e dispositivi in cui è stata selezionata la casella 2FA.</span><span class="sxs-lookup"><span data-stu-id="161ca-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="161ca-177">Fino a quando gli utenti malintenzionati non possono accedere al dispositivo, abilitazione 2FA e facendo clic su di **ricordare questo browser** fornirà accedere facilmente la password di un unico passaggio, conservando al tempo protezione 2FA complesse per tutti gli accessi dai dispositivi non trusted.</span><span class="sxs-lookup"><span data-stu-id="161ca-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="161ca-178">Tale operazione può essere eseguita su qualsiasi dispositivo privata che vengono utilizzate regolarmente.</span><span class="sxs-lookup"><span data-stu-id="161ca-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="161ca-179">Questa esercitazione viene fornita una rapida introduzione all'abilitazione 2FA in una nuova applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="161ca-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="161ca-180">L'esercitazione [autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) vengono inseriti nel dettaglio in code-behind dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="161ca-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="161ca-181">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="161ca-181">Additional Resources</span></span>

- <span data-ttu-id="161ca-182">[Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra dettagli sull'autenticazione a due fattori</span><span class="sxs-lookup"><span data-stu-id="161ca-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="161ca-183">Collegamenti a ASP.NET Identity consigliato risorse</span><span class="sxs-lookup"><span data-stu-id="161ca-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="161ca-184">[Account di conferma e il recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) vengono descritti in dettaglio in Conferma password di ripristino e account.</span><span class="sxs-lookup"><span data-stu-id="161ca-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="161ca-185">[App di MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) in questa esercitazione viene illustrato come scrivere un'applicazione ASP.NET MVC 5 con autorizzazione di Facebook e Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="161ca-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="161ca-186">Viene inoltre illustrato come aggiungere ulteriori dati al database di identità.</span><span class="sxs-lookup"><span data-stu-id="161ca-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="161ca-187">[Distribuire un'app sicura ASP.NET MVC con appartenenza, OAuth e il Database SQL Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="161ca-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="161ca-188">In questa esercitazione aggiunge distribuzione di Azure, come proteggere le app con i ruoli, come utilizzare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="161ca-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="161ca-189">Creazione di un'app di Google per OAuth 2 e l'applicazione di connessione al progetto</span><span class="sxs-lookup"><span data-stu-id="161ca-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="161ca-190">Creazione di app in Facebook e l'applicazione di connessione al progetto</span><span class="sxs-lookup"><span data-stu-id="161ca-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="161ca-191">Impostazione di SSL nel progetto</span><span class="sxs-lookup"><span data-stu-id="161ca-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
