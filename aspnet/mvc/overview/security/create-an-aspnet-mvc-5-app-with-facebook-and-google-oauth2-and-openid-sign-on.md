---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: MVC 5 di creare App con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di accedere tramite OAuth 2.0 con le credenziali di un autenti esterni...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 8a9528f76b0166175f950543b4b8a7250bdf5100
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388766"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="da336-103">Creare un'App ASP.NET MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#)</span><span class="sxs-lookup"><span data-stu-id="da336-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="da336-104">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="da336-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="da336-105">Questa esercitazione illustra come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di accedere tramite [OAuth 2.0](http://oauth.net/2/) con le credenziali di un provider di autenticazione esterni, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google.</span><span class="sxs-lookup"><span data-stu-id="da336-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="da336-106">Per semplicità, questa esercitazione è incentrata sull'uso di credenziali da Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="da336-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="da336-107">Abilitazione di queste credenziali nei siti web offre un vantaggio significativo perché milioni di utenti hanno già account con questi provider esterni.</span><span class="sxs-lookup"><span data-stu-id="da336-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="da336-108">Questi utenti potrebbero essere più inclini a effettuare l'iscrizione per il sito se non dispongono creare e tenere presente un nuovo insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="da336-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="da336-109">Vedere anche [app ASP.NET MVC 5 con SMS e posta elettronica l'autenticazione a due fattori](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="da336-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="da336-110">L'esercitazione illustra anche come aggiungere dati del profilo dell'utente e come usare l'API di appartenenza per aggiungere i ruoli.</span><span class="sxs-lookup"><span data-stu-id="da336-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="da336-111">Questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (me, seguire in Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="da336-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="da336-112">Introduzione</span><span class="sxs-lookup"><span data-stu-id="da336-112">Getting Started</span></span>

<span data-ttu-id="da336-113">Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="da336-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="da336-114">Installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="da336-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="da336-115">Per informazioni su Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! e altro ancora, vedere questo [progetto di esempio](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="da336-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="da336-116">È necessario installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva per usare Google OAuth 2 e per eseguire il debug in locale senza eventuali avvisi SSL.</span><span class="sxs-lookup"><span data-stu-id="da336-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="da336-117">Fare clic su **nuovo progetto** dalle **avviare** pagina oppure è possibile usare il menu e selezionare **File**e quindi **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="da336-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="da336-118">Creazione della prima applicazione</span><span class="sxs-lookup"><span data-stu-id="da336-118">Creating Your First Application</span></span>

<span data-ttu-id="da336-119">Fare clic su **nuovo progetto**, quindi selezionare **Visual c#** a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="da336-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="da336-120">Denominare il progetto "MvcAuth" e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="da336-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="da336-121">Nel **nuovo progetto ASP.NET** finestra di dialogo, fare clic su **MVC**.</span><span class="sxs-lookup"><span data-stu-id="da336-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="da336-122">Se l'autenticazione non è **account utente individuali**, fare clic sui **Modifica autenticazione** e selezionare **account utente individuali**.</span><span class="sxs-lookup"><span data-stu-id="da336-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="da336-123">Controllando **ospita nel cloud**, l'app sarà molto facile da ospitare in Azure.</span><span class="sxs-lookup"><span data-stu-id="da336-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="da336-124">Se è stato selezionato **ospita nel cloud**, completare la finestra di dialogo Configura.</span><span class="sxs-lookup"><span data-stu-id="da336-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="da336-125">Usare NuGet per aggiornare il middleware OWIN più recente</span><span class="sxs-lookup"><span data-stu-id="da336-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="da336-126">Usare Gestione pacchetti NuGet per aggiornare il [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="da336-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="da336-127">Selezionare **aggiornamenti** nel menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="da336-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="da336-128">È possibile fare clic sui **Aggiorna tutte** pulsante oppure è possibile cercare solo i pacchetti OWIN (illustrati nella figura seguente):</span><span class="sxs-lookup"><span data-stu-id="da336-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="da336-129">Nell'immagine seguente, vengono visualizzati solo i pacchetti OWIN:</span><span class="sxs-lookup"><span data-stu-id="da336-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="da336-130">Dal pacchetto Manager Console (console di gestione pacchetti), è possibile immettere il `Update-Package` comando, che aggiornerà tutti i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="da336-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="da336-131">Premere **F5** oppure **CTRL+F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da336-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="da336-132">Nell'immagine seguente, il numero di porta è 1234.</span><span class="sxs-lookup"><span data-stu-id="da336-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="da336-133">Quando si esegue l'applicazione, si noterà un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="da336-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="da336-134">A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di spostamento per visualizzare il **Home**, **sulle**, **contatto**, **registrare**e **accedere** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="da336-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="da336-135">Configurare SSL nel progetto</span><span class="sxs-lookup"><span data-stu-id="da336-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="da336-136">Per connettersi ai provider di autenticazione, ad esempio Google e Facebook, è necessario configurare IIS Express per utilizzare SSL.</span><span class="sxs-lookup"><span data-stu-id="da336-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="da336-137">È importante continuare a usare SSL dopo l'accesso e non rilasciare nuovamente a HTTP, il cookie di accesso è semplicemente come segreto come nome utente e password sia senza utilizzare SSL inviato in testo non crittografato attraverso la rete.</span><span class="sxs-lookup"><span data-stu-id="da336-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="da336-138">Inoltre, è stato così già compiuto il tempo necessario per eseguire l'handshake e proteggere il canale, ovvero la maggior parte delle caratteristiche che rendono più lento rispetto a HTTP a HTTPS, prima dell'esecuzione della pipeline MVC, pertanto, reindirizzando a HTTP dopo che si è connessi non rendere la richiesta corrente o futuro richieste è molto più veloce.</span><span class="sxs-lookup"><span data-stu-id="da336-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="da336-139">Nelle **Esplora soluzioni**, fare clic sui **MvcAuth** progetto.</span><span class="sxs-lookup"><span data-stu-id="da336-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="da336-140">Premere il tasto F4 per visualizzare le proprietà del progetto.</span><span class="sxs-lookup"><span data-stu-id="da336-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="da336-141">In alternativa, scegliere il **View** menu è possibile selezionare **finestra proprietà**.</span><span class="sxs-lookup"><span data-stu-id="da336-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="da336-142">Change **SSL abilitato** su True.</span><span class="sxs-lookup"><span data-stu-id="da336-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="da336-143">Copiare l'URL SSL (che sarà `https://localhost:44300/` a meno che non creati altri progetti SSL).</span><span class="sxs-lookup"><span data-stu-id="da336-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="da336-144">Nella **Esplora soluzioni**, fare clic il **MvcAuth** del progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="da336-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="da336-145">Selezionare il **Web** scheda e quindi incollare l'URL SSL nel **Url progetto** casella.</span><span class="sxs-lookup"><span data-stu-id="da336-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="da336-146">Salvare il file (CTRL + S).</span><span class="sxs-lookup"><span data-stu-id="da336-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="da336-147">È necessario l'URL per configurare le app di autenticazione di Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="da336-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="da336-148">Aggiungere il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) dell'attributo di `Home` controller in modo da richiedere tutte le richieste devono utilizzare HTTPS.</span><span class="sxs-lookup"><span data-stu-id="da336-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="da336-149">Un approccio più sicuro consiste nell'aggiungere il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtro all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da336-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="da336-150">Vedere la sezione &quot;proteggere l'applicazione con SSL e l'attributo autorizzare&quot; nella mio tutoral [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio App di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="da336-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="da336-151">Di seguito è riportata una parte del controller Home.</span><span class="sxs-lookup"><span data-stu-id="da336-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="da336-152">Premere CTRL+F5 per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da336-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="da336-153">Se il certificato è stato installato in precedenza, è possibile ignorare il resto di questa sezione e passare a [creazione di un'app Google per OAuth 2 e ci si connette l'app per il progetto](#goog), in caso contrario, seguire le istruzioni per considerare attendibile autofirmato certificato IIS Express è generato.</span><span class="sxs-lookup"><span data-stu-id="da336-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="da336-154">Leggere il **avviso di sicurezza** finestra di dialogo e quindi fare clic su **Yes** se si desidera installare il certificato che rappresenta localhost.</span><span class="sxs-lookup"><span data-stu-id="da336-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="da336-155">Internet Explorer mostra il *Home* pagina e nessun avviso SSL.</span><span class="sxs-lookup"><span data-stu-id="da336-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="da336-156">Google Chrome, inoltre, accetta il certificato e visualizzerà contenuto HTTPS senza alcun avviso.</span><span class="sxs-lookup"><span data-stu-id="da336-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="da336-157">Firefox Usa un proprio archivio certificati, in modo che verrà visualizzato un avviso.</span><span class="sxs-lookup"><span data-stu-id="da336-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="da336-158">Per l'applicazione è possibile scegliere l'opzione **dichiaro di aver compreso i rischi**.</span><span class="sxs-lookup"><span data-stu-id="da336-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="da336-159">Creazione di un'app Google per OAuth 2 e ci si connette l'app al progetto</span><span class="sxs-lookup"><span data-stu-id="da336-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="da336-160">Per istruzioni di Google OAuth corrente, vedere [configurazione di Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="da336-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="da336-161">Passare il [Google Developers Console](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="da336-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="da336-162">Se è stato ancora creato un progetto prima di, selezionare **credenziali** nella scheda a sinistra e quindi selezionare **crea**.</span><span class="sxs-lookup"><span data-stu-id="da336-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="da336-163">Nella scheda di sinistra, fare clic su **credenziali**.</span><span class="sxs-lookup"><span data-stu-id="da336-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="da336-164">Fare clic su **creare le credenziali** quindi **ID client OAuth**.</span><span class="sxs-lookup"><span data-stu-id="da336-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="da336-165">Nel **Create Client ID** finestra di dialogo, mantenere il valore predefinito **applicazione Web** per il tipo di applicazione.</span><span class="sxs-lookup"><span data-stu-id="da336-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="da336-166">Impostare il **JavaScript autorizzate** origini per l'URL SSL descritti in precedenza (`https://localhost:44300/` a meno che non creati altri progetti SSL)</span><span class="sxs-lookup"><span data-stu-id="da336-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="da336-167">Impostare il **Authorized redirect URI** per:</span><span class="sxs-lookup"><span data-stu-id="da336-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="da336-168">Scegliere la voce di menu schermata consenso OAuth, quindi impostare il nome di prodotto e indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="da336-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="da336-169">Dopo aver completato il modulo di fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="da336-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="da336-170">Scegliere la voce di menu della libreria, cercare **Google + API**, fare clic su di esso, quindi premere Enable.</span><span class="sxs-lookup"><span data-stu-id="da336-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="da336-171">L'immagine seguente mostra le API abilitate.</span><span class="sxs-lookup"><span data-stu-id="da336-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="da336-172">Da Gestione API Google API, visitare il **credenziali** pressione di tab per ottenere il **ID Client**.</span><span class="sxs-lookup"><span data-stu-id="da336-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="da336-173">Download per salvare un file JSON con i segreti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da336-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="da336-174">Copiare e incollare il **ClientId** e **ClientSecret** nel `UseGoogleAuthentication` trovato nel metodo il *Startup.Auth.cs* del file nel *App_Start* cartella.</span><span class="sxs-lookup"><span data-stu-id="da336-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="da336-175">Il **ClientId** e **ClientSecret** valori riportati di seguito sono esempi e non funzionano.</span><span class="sxs-lookup"><span data-stu-id="da336-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="da336-176">Security - store mai i dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="da336-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="da336-177">L'account e le credenziali vengono aggiunti al codice precedente per mantenere semplice l'esempio.</span><span class="sxs-lookup"><span data-stu-id="da336-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="da336-178">Visualizzare [procedure consigliate per la distribuzione delle password e altri dati sensibili in ASP.NET e servizio App di Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="da336-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="da336-179">Premere **CTRL+F5** per compilare ed eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da336-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="da336-180">Scegliere il **Accedi** collegamento.</span><span class="sxs-lookup"><span data-stu-id="da336-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="da336-181">Sotto **usare un altro servizio per accedere**, fare clic su **Google**.</span><span class="sxs-lookup"><span data-stu-id="da336-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="da336-182">Se non è disponibile uno dei passaggi precedenti, si otterrà un errore HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="da336-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="da336-183">Eseguire nuovamente la procedura precedente.</span><span class="sxs-lookup"><span data-stu-id="da336-183">Recheck your steps above.</span></span> <span data-ttu-id="da336-184">Se non è disponibile un'impostazione obbligatoria (ad esempio **nome del prodotto**), aggiungere l'elemento manca e salvare; può richiedere alcuni minuti per l'autenticazione funzioni.</span><span class="sxs-lookup"><span data-stu-id="da336-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="da336-185">Si verrà reindirizzati al sito di Google in cui vengono immesse le credenziali.</span><span class="sxs-lookup"><span data-stu-id="da336-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="da336-186">Dopo aver immesso le credenziali, verrà richiesto di concedere le autorizzazioni per l'applicazione web che appena creato:</span><span class="sxs-lookup"><span data-stu-id="da336-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="da336-187">Fare clic su **accettano**.</span><span class="sxs-lookup"><span data-stu-id="da336-187">Click **Accept**.</span></span> <span data-ttu-id="da336-188">Verrà reindirizzati al **registrare** pagina dell'applicazione MvcAuth in cui è possibile registrare l'account Google.</span><span class="sxs-lookup"><span data-stu-id="da336-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="da336-189">È possibile scegliere di modificare il nome di registrazione di posta elettronica locale usato per l'account Gmail, ma in genere si vuole mantenere l'alias di posta elettronica predefinito (vale a dire quello usato per l'autenticazione).</span><span class="sxs-lookup"><span data-stu-id="da336-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="da336-190">Fare clic su **Register**.</span><span class="sxs-lookup"><span data-stu-id="da336-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="da336-191">Creazione dell'app in Facebook e ci si connette l'app al progetto</span><span class="sxs-lookup"><span data-stu-id="da336-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="da336-192">Per istruzioni di autenticazione Facebook OAuth2 corrente, vedere [l'autenticazione di configurazione di Facebook](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="da336-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="da336-193">Per l'autenticazione OAuth2 di Facebook, è necessario copiare nel progetto alcune impostazioni da un'applicazione che si crea in Facebook.</span><span class="sxs-lookup"><span data-stu-id="da336-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="da336-194">Nel browser, passare a [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) ed eseguire l'accesso, immettere le credenziali di Facebook.</span><span class="sxs-lookup"><span data-stu-id="da336-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="da336-195">Se già non sono registrati come sviluppatore Facebook, fare clic su **registrarsi come sviluppatore** e seguire le istruzioni per registrare.</span><span class="sxs-lookup"><span data-stu-id="da336-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="da336-196">Nel **Apps** scheda, fare clic su **Crea nuova App**.</span><span class="sxs-lookup"><span data-stu-id="da336-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Crea nuova app](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="da336-198">Immettere un **nome dell'App** e **categoria**, quindi fare clic su **crea App**.</span><span class="sxs-lookup"><span data-stu-id="da336-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="da336-199">Il <strong>App Namespace</strong> è la parte dell'URL che l'App userà per accedere all'applicazione di Facebook per l'autenticazione (ad esempio, https\://apps.facebook.com/{App Namespace}).</span><span class="sxs-lookup"><span data-stu-id="da336-199">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https\://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="da336-200">Se non si specifica un <strong>App Namespace</strong>, il <strong>App ID</strong> verrà usato per l'URL.</span><span class="sxs-lookup"><span data-stu-id="da336-200">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="da336-201">Il <strong>App ID</strong> è un numero lungo generati dal sistema che verrà visualizzato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="da336-201">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Finestra di dialogo Nuova App crea](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="da336-203">Inviare il controllo di sicurezza standard.</span><span class="sxs-lookup"><span data-stu-id="da336-203">Submit the standard security check.</span></span>

    ![Controllo di sicurezza](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="da336-205">Selezionare **le impostazioni** per la barra di menu a sinistra![ Barra dei menu per gli sviluppatori di Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="da336-205">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="da336-206">Nel **base** sezione Impostazioni della pagina selezionare **Aggiungi piattaforma** per specificare che si sta aggiungendo un'applicazione del sito Web.</span><span class="sxs-lookup"><span data-stu-id="da336-206">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="da336-207">![Impostazioni di base](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="da336-207">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="da336-208">Selezionare **sito Web** tra le opzioni di piattaforma.</span><span class="sxs-lookup"><span data-stu-id="da336-208">Select **Website** from the platform choices.</span></span>  
  
    ![Scelte di piattaforma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="da336-210">Prendere nota del **App ID** e il **segreto dell'App** in modo che sia possibile aggiungere entrambi nell'applicazione MVC più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="da336-210">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="da336-211">Inoltre, aggiungere l'URL del sito (`https://localhost:44300/`) per testare l'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="da336-211">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="da336-212">Aggiungere anche un **Contact Email**.</span><span class="sxs-lookup"><span data-stu-id="da336-212">Also, add a **Contact Email**.</span></span> <span data-ttu-id="da336-213">Quindi, selezionare **Save Changes**.</span><span class="sxs-lookup"><span data-stu-id="da336-213">Then, select **Save Changes**.</span></span>   

    ![Pagina dei dettagli di un'applicazione di base](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="da336-215">Si noti che solo sarà in grado di eseguire l'autenticazione usando l'alias di posta elettronica che è stato registrato.</span><span class="sxs-lookup"><span data-stu-id="da336-215">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="da336-216">Non sarà in grado di registrare altri utenti e account di test.</span><span class="sxs-lookup"><span data-stu-id="da336-216">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="da336-217">È possibile concedere altro accesso di account di Facebook all'applicazione su Facebook **ruolo di sviluppatore** scheda.</span><span class="sxs-lookup"><span data-stu-id="da336-217">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="da336-218">In Visual Studio, aprire *App\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="da336-218">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="da336-219">Copiare e incollare il **AppId** e **segreto dell'App** nel `UseFacebookAuthentication` (metodo).</span><span class="sxs-lookup"><span data-stu-id="da336-219">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="da336-220">Il **AppId** e **segreto App** valori riportati di seguito sono esempi e non funzionerà.</span><span class="sxs-lookup"><span data-stu-id="da336-220">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="da336-221">Fare clic su **salvare le modifiche**.</span><span class="sxs-lookup"><span data-stu-id="da336-221">Click **Save Changes**.</span></span>
13. <span data-ttu-id="da336-222">Premere **CTRL+F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da336-222">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="da336-223">Selezionare **Accedi** per visualizzare la pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="da336-223">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="da336-224">Fare clic su **Facebook** sotto **usi un altro servizio per l'accesso.**</span><span class="sxs-lookup"><span data-stu-id="da336-224">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="da336-225">Immettere le credenziali di Facebook.</span><span class="sxs-lookup"><span data-stu-id="da336-225">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="da336-226">Verrà richiesto di concedere l'autorizzazione per l'applicazione accedere ai profili pubblici e liste di amici.</span><span class="sxs-lookup"><span data-stu-id="da336-226">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Dettagli dell'app per Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="da336-228">Si è ora connessi.</span><span class="sxs-lookup"><span data-stu-id="da336-228">You are now logged in.</span></span> <span data-ttu-id="da336-229">È ora possibile registrare questo account con l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da336-229">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="da336-230">Quando si registra, viene aggiunta una voce per il *utenti* tabella del database delle appartenenze.</span><span class="sxs-lookup"><span data-stu-id="da336-230">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="da336-231">Esaminare i dati di appartenenza</span><span class="sxs-lookup"><span data-stu-id="da336-231">Examine the Membership Data</span></span>

<span data-ttu-id="da336-232">Nel **View** menu, fare clic su **Esplora Server**.</span><span class="sxs-lookup"><span data-stu-id="da336-232">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="da336-233">Espandere **DefaultConnection (MvcAuth)**, espandere **tabelle**, fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.</span><span class="sxs-lookup"><span data-stu-id="da336-233">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dati della tabella aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="da336-235">Aggiunta di dati di profilo per la classe utente</span><span class="sxs-lookup"><span data-stu-id="da336-235">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="da336-236">In questa sezione si aggiungeranno data di nascita e città natale ai dati utente durante la registrazione, come illustrato nell'immagine seguente.</span><span class="sxs-lookup"><span data-stu-id="da336-236">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg con città natale e Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="da336-238">Aprire il *Models\IdentityModels.cs* file e aggiungere le proprietà di Town (città) casa e data di nascita:</span><span class="sxs-lookup"><span data-stu-id="da336-238">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="da336-239">Aprire il *Models\AccountViewModels.cs* file e il set di proprietà Town (città) data e l'home page in nascita `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="da336-239">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="da336-240">Aprire il *Controllers\AccountController.cs* del file e aggiungere il codice per town home page e data di nascita nel `ExternalLoginConfirmation` metodo di azione come mostrato:</span><span class="sxs-lookup"><span data-stu-id="da336-240">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="da336-241">Aggiungere la data di nascita e città natale per il *Views\Account\ExternalLoginConfirmation.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="da336-241">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="da336-242">In modo che anche in questo caso è possibile registrare l'account di Facebook con l'applicazione e verificare che la nuova data di nascita e le informazioni sul profilo città Natale, è possibile aggiungere, eliminare il database di appartenenza.</span><span class="sxs-lookup"><span data-stu-id="da336-242">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="da336-243">Dal **Esplora soluzioni**, fare clic sui **Mostra tutti i file** icona, quindi fare clic destro *Add\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;mdf* e fare clic su **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="da336-243">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="da336-244">Dal **degli strumenti** menu, fare clic su **Gestione pacchetti NuGet**, quindi fare clic su **Package Manager Console** (PMC).</span><span class="sxs-lookup"><span data-stu-id="da336-244">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="da336-245">Immettere i comandi seguenti nella console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="da336-245">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="da336-246">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="da336-246">Enable-Migrations</span></span>
2. <span data-ttu-id="da336-247">Add-Migration Init</span><span class="sxs-lookup"><span data-stu-id="da336-247">Add-Migration Init</span></span>
3. <span data-ttu-id="da336-248">Update-Database</span><span class="sxs-lookup"><span data-stu-id="da336-248">Update-Database</span></span>

<span data-ttu-id="da336-249">Eseguire l'applicazione e usare FaceBook e Google per accedere e registrare alcuni utenti.</span><span class="sxs-lookup"><span data-stu-id="da336-249">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="da336-250">Esaminare i dati di appartenenza</span><span class="sxs-lookup"><span data-stu-id="da336-250">Examine the Membership Data</span></span>

<span data-ttu-id="da336-251">Nel **View** menu, fare clic su **Esplora Server**.</span><span class="sxs-lookup"><span data-stu-id="da336-251">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="da336-252">Fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.</span><span class="sxs-lookup"><span data-stu-id="da336-252">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="da336-253">Il `HomeTown` e `BirthDate` vengono visualizzati i campi seguenti.</span><span class="sxs-lookup"><span data-stu-id="da336-253">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="da336-254">La disconnessione all'App e accedere con un altro Account</span><span class="sxs-lookup"><span data-stu-id="da336-254">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="da336-255">Se si accede all'App con Facebook e quindi effettuare la disconnessione e provare ad accedere nuovamente con un altro account di Facebook (usando lo stesso browser), verrà immediatamente registrato all'account di Facebook precedente che è stato usato.</span><span class="sxs-lookup"><span data-stu-id="da336-255">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="da336-256">Per usare un altro account, è necessario passare a Facebook ed effettuare la disconnessione in Facebook.</span><span class="sxs-lookup"><span data-stu-id="da336-256">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="da336-257">La stessa regola si applica a qualsiasi altro 3rd provider di autenticazione terze parti.</span><span class="sxs-lookup"><span data-stu-id="da336-257">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="da336-258">In alternativa, è possibile accedere con un altro account usando un browser diverso.</span><span class="sxs-lookup"><span data-stu-id="da336-258">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da336-259">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="da336-259">Next Steps</span></span>

<span data-ttu-id="da336-260">Visualizzare [Introducing i provider di sicurezza Yahoo e OAuth di LinkedIn per OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) da Jerrie Pelser per le istruzioni di Yahoo e LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="da336-260">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="da336-261">Vedere del Jerrie insomma, pulsanti di accesso per ASP.NET MVC 5 ottenere i pulsanti per abilitare l'accesso social.</span><span class="sxs-lookup"><span data-stu-id="da336-261">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="da336-262">Seguire l'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio App di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), che continua in questa esercitazione e viene illustrato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="da336-262">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="da336-263">Come distribuire l'app di Azure.</span><span class="sxs-lookup"><span data-stu-id="da336-263">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="da336-264">Come proteggere l'app con i ruoli si.</span><span class="sxs-lookup"><span data-stu-id="da336-264">How to secure you app with roles.</span></span>
3. <span data-ttu-id="da336-265">Come proteggere l'app con il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [autorizza](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtri.</span><span class="sxs-lookup"><span data-stu-id="da336-265">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="da336-266">Come usare l'API di appartenenza per aggiungere utenti e ruoli.</span><span class="sxs-lookup"><span data-stu-id="da336-266">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="da336-267">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare.</span><span class="sxs-lookup"><span data-stu-id="da336-267">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="da336-268">È anche possibile richiedere nuovi argomenti in [Mostra Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="da336-268">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="da336-269">È anche possibile richiedere e votare nuove funzionalità per essere aggiunto ad ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="da336-269">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="da336-270">Ad esempio, è possibile votare per uno strumento a [creare e gestire utenti e ruoli.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="da336-270">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="da336-271">Per una valida spiegazione del funzionamento di servizi di autenticazione esterno di ASP.NET, vedere di Robert McMurray [servizi di autenticazione esterni](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="da336-271">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="da336-272">L'articolo di Robert anche descrive in dettaglio nell'abilitazione dell'autenticazione di Microsoft e Twitter.</span><span class="sxs-lookup"><span data-stu-id="da336-272">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="da336-273">Tom Dykstra Kothari [esercitazione su EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) descrive l'uso con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="da336-273">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
