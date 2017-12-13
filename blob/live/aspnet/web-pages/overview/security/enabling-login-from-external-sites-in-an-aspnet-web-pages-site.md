---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Accesso tramite siti esterni in un Web ASP.NET di pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene illustrato come accedere al sito pagine Web ASP.NET (Razor) utilizzando Google, Facebook, Twitter, Yahoo e altri siti, vale a dire il supporto di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="b5c68-103">Registrazione tramite siti esterni in un sito Web di ASP.NET di pagine (Razor)</span><span class="sxs-lookup"><span data-stu-id="b5c68-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="b5c68-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b5c68-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b5c68-105">In questo articolo viene illustrato come accedere al sito pagine Web ASP.NET (Razor) utilizzando Google, Facebook, Twitter, Yahoo e altri siti, vale a dire il supporto di OAuth e OpenID nel sito.</span><span class="sxs-lookup"><span data-stu-id="b5c68-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="b5c68-106">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b5c68-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b5c68-107">Come abilitare l'account di accesso da altri siti quando si utilizza il modello Starter Site di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="b5c68-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="b5c68-108">Si tratta della funzionalità ASP.NET introdotta nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="b5c68-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="b5c68-109">Il `OAuthWebSecurity` helper.</span><span class="sxs-lookup"><span data-stu-id="b5c68-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b5c68-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b5c68-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b5c68-111">Pagine Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="b5c68-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="b5c68-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="b5c68-112">WebMatrix 3</span></span>

<span data-ttu-id="b5c68-113">Include il supporto per ASP.NET Web Pages [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provider.</span><span class="sxs-lookup"><span data-stu-id="b5c68-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="b5c68-114">Utilizzando questi provider, è possibile consentire agli utenti l'accesso al sito utilizzando le credenziali esistenti da Facebook, Twitter, Microsoft e Google.</span><span class="sxs-lookup"><span data-stu-id="b5c68-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="b5c68-115">Ad esempio, per accedere con un account Facebook, gli utenti possono solo scegliere un'icona di Facebook, che li reindirizza alla pagina di accesso di Facebook in cui inserire le informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="b5c68-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="b5c68-116">È quindi possibile associare l'account di accesso di Facebook con il proprio account nel sito.</span><span class="sxs-lookup"><span data-stu-id="b5c68-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="b5c68-117">Una funzionalità avanzata correlata alle funzionalità di appartenenza a pagine Web è che gli utenti possono associare più account di accesso (inclusi gli accessi dai siti di social networking) con un singolo account nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="b5c68-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="b5c68-118">La seguente immagine illustra la pagina account di accesso di **Starter Site** modello, in cui un utente può scegliere un'icona di Facebook, Twitter, Google o Microsoft per abilitare l'accesso con un account esterno:</span><span class="sxs-lookup"><span data-stu-id="b5c68-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![provider esterni](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="b5c68-120">È possibile abilitare l'appartenenza OAuth e OpenID rimuovendo poche righe di codice il **Starter Site** modello.</span><span class="sxs-lookup"><span data-stu-id="b5c68-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="b5c68-121">I metodi e proprietà consentono di lavorare con OAuth e i provider OpenID sono nella `WebMatrix.Security.OAuthWebSecurity` classe.</span><span class="sxs-lookup"><span data-stu-id="b5c68-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="b5c68-122">Il **Starter Site** modello include un'infrastruttura completa dell'appartenenza, completo di una pagina di accesso, un database di appartenenza e tutto il codice necessario consentire agli utenti l'accesso al sito usando le credenziali locali o da un altro sito .</span><span class="sxs-lookup"><span data-stu-id="b5c68-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="b5c68-123">In questa sezione viene fornito un esempio di come consentire agli utenti di accedere da siti esterni a un sito che si basa sul **Starter Site** modello.</span><span class="sxs-lookup"><span data-stu-id="b5c68-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="b5c68-124">Dopo aver creato un sito di partenza, tale scopo (dettagli):</span><span class="sxs-lookup"><span data-stu-id="b5c68-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="b5c68-125">Per i siti che usano un provider OAuth (Facebook, Twitter e Microsoft), si crea un'applicazione nel sito esterno.</span><span class="sxs-lookup"><span data-stu-id="b5c68-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="b5c68-126">In questo modo le chiavi dell'applicazione che è necessario per richiamare la funzionalità di accesso per tali siti.</span><span class="sxs-lookup"><span data-stu-id="b5c68-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="b5c68-127">Per i siti che usano un provider OpenID (Google), non si dispone creare un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b5c68-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="b5c68-128">Per tutti i siti, è necessario disporre di un account per accedere e creare applicazioni dello sviluppatore.</span><span class="sxs-lookup"><span data-stu-id="b5c68-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5c68-129">Applicazioni Microsoft accettano solo un URL in tempo reale per un sito Web di lavoro, non è possibile utilizzare un URL del sito Web locale per il test degli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="b5c68-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="b5c68-130">Modifica di alcuni file nel sito Web per specificare il provider di autenticazione appropriato e per l'invio di un account di accesso al sito che si desidera utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b5c68-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="b5c68-131">Questo articolo fornisce istruzioni separate per le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="b5c68-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="b5c68-132">Abilitare gli account di accesso di Google</span><span class="sxs-lookup"><span data-stu-id="b5c68-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="b5c68-133">Abilitare gli account di accesso di Facebook</span><span class="sxs-lookup"><span data-stu-id="b5c68-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="b5c68-134">Abilitazione di account di accesso Twitter</span><span class="sxs-lookup"><span data-stu-id="b5c68-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="b5c68-135">Abilitare gli account di accesso di Google</span><span class="sxs-lookup"><span data-stu-id="b5c68-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="b5c68-136">Creare o aprire un sito ASP.NET Web Pages basato sul modello Starter Site di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="b5c68-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="b5c68-137">Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento la riga di codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b5c68-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="b5c68-138">Account di accesso Google di test</span><span class="sxs-lookup"><span data-stu-id="b5c68-138">Testing Google login</span></span>

1. <span data-ttu-id="b5c68-139">Eseguire il *cshtml* pagina del sito e scegliere il **Accedi** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b5c68-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="b5c68-140">Nel *account di accesso* nella pagina di **utilizzare un altro servizio per l'accesso** sezione, scegliere il **Google** o **Yahoo** pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="b5c68-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="b5c68-141">Questo esempio viene utilizzato l'account di accesso di Google.</span><span class="sxs-lookup"><span data-stu-id="b5c68-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="b5c68-142">La pagina web reindirizza la richiesta alla pagina di accesso di Google.</span><span class="sxs-lookup"><span data-stu-id="b5c68-142">The web page redirects the request to the Google login page.</span></span>

    ![Accesso di Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="b5c68-144">Immettere le credenziali per un account esistente di Google.</span><span class="sxs-lookup"><span data-stu-id="b5c68-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="b5c68-145">Se Google viene chiesto se si desidera consentire *Localhost* per utilizzare le informazioni dell'account, fare clic su **Consenti**.</span><span class="sxs-lookup"><span data-stu-id="b5c68-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="b5c68-146">Il codice Usa il token di Google per autenticare l'utente e torna a questa pagina nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="b5c68-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="b5c68-147">Questa pagina consente agli utenti di associare i relativi account di accesso di Google a un account esistente nel sito Web o cui possono registrare un nuovo account del sito per associare l'account di accesso esterno con.</span><span class="sxs-lookup"><span data-stu-id="b5c68-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="b5c68-149">Scegliere il **associare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b5c68-149">Choose the **Associate** button.</span></span> <span data-ttu-id="b5c68-150">Il browser restituisce alla pagina iniziale dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b5c68-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="b5c68-151">Abilitare gli account di accesso di Facebook</span><span class="sxs-lookup"><span data-stu-id="b5c68-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="b5c68-152">Passare al [sito agli sviluppatori di Facebook](https://developers.facebook.com/apps) (accedere se non è già connesso).</span><span class="sxs-lookup"><span data-stu-id="b5c68-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="b5c68-153">Scegliere il **Create New App** pulsante e quindi seguire le istruzioni per assegnare un nome e creare la nuova applicazione.</span><span class="sxs-lookup"><span data-stu-id="b5c68-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="b5c68-154">Nella sezione **selezionare come permetterà di combinare l'app con Facebook**, scegliere il **sito Web** sezione.</span><span class="sxs-lookup"><span data-stu-id="b5c68-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="b5c68-155">Compilare il **URL del sito** campo con l'URL del sito (ad esempio, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="b5c68-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="b5c68-156">Il **dominio** campo è facoltativo; è possibile utilizzare questo metodo per fornire l'autenticazione per un intero dominio (ad esempio *example.com*).</span><span class="sxs-lookup"><span data-stu-id="b5c68-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="b5c68-157">Se si esegue un sito nel computer locale con un URL come `http://localhost:12345` (dove il numero è un numero di porta locale), è possibile aggiungere questo valore per il **URL del sito** campo per il test del sito.</span><span class="sxs-lookup"><span data-stu-id="b5c68-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="b5c68-158">Tuttavia, ogni volta che il numero di porta delle modifiche del sito locale, sarà necessario aggiornare il **URL del sito** campo dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b5c68-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="b5c68-159">Scegliere il **Salva modifiche** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b5c68-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="b5c68-160">Scegliere il **app** scheda nuovo e quindi visualizzare la pagina iniziale per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b5c68-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="b5c68-161">Copia il **ID App** e **segreto dell'App** valori per l'applicazione e incollarli in un file di testo temporaneo.</span><span class="sxs-lookup"><span data-stu-id="b5c68-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="b5c68-162">Questi valori verrà passato al provider di Facebook nel codice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="b5c68-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="b5c68-163">Chiudere il sito per sviluppatori di Facebook.</span><span class="sxs-lookup"><span data-stu-id="b5c68-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="b5c68-164">Ora è apportare modifiche a due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account Facebook.</span><span class="sxs-lookup"><span data-stu-id="b5c68-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="b5c68-165">Creare o aprire un sito ASP.NET Web Pages basato sul modello Starter Site di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="b5c68-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="b5c68-166">Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Facebook.</span><span class="sxs-lookup"><span data-stu-id="b5c68-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="b5c68-167">Il blocco di codice senza commenti è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b5c68-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="b5c68-168">Copia il **ID App** valore dall'applicazione Facebook come il valore di `appId` parametro (racchiuso tra virgolette).</span><span class="sxs-lookup"><span data-stu-id="b5c68-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="b5c68-169">Copia **segreto dell'App** valore dall'applicazione Facebook come il `appSecret` valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="b5c68-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="b5c68-170">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="b5c68-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="b5c68-171">Test di accesso di Facebook</span><span class="sxs-lookup"><span data-stu-id="b5c68-171">Testing Facebook login</span></span>

1. <span data-ttu-id="b5c68-172">Eseguire il sito *cshtml* pagina e selezionare il **accesso** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b5c68-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="b5c68-173">Nel *account di accesso* nella pagina di **utilizzare un altro servizio per l'accesso** , scegliere il **Facebook** icona.</span><span class="sxs-lookup"><span data-stu-id="b5c68-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="b5c68-174">La pagina web reindirizza la richiesta alla pagina di accesso Facebook.</span><span class="sxs-lookup"><span data-stu-id="b5c68-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="b5c68-176">Accedere a un account Facebook.</span><span class="sxs-lookup"><span data-stu-id="b5c68-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="b5c68-177">Il codice Usa il token di Facebook per autenticare l'utente e quindi restituisce a una pagina in cui è possibile associare l'account di accesso di Facebook con account di accesso del sito.</span><span class="sxs-lookup"><span data-stu-id="b5c68-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="b5c68-178">L'indirizzo di posta elettronica o nome utente verrà inserita nel **posta elettronica** campo nel form.</span><span class="sxs-lookup"><span data-stu-id="b5c68-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="b5c68-180">Scegliere il **associare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b5c68-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="b5c68-181">Il browser restituisce alla home page e si è connessi.</span><span class="sxs-lookup"><span data-stu-id="b5c68-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="b5c68-182">Abilitazione di account di accesso Twitter</span><span class="sxs-lookup"><span data-stu-id="b5c68-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="b5c68-183">Individuare il [sito agli sviluppatori di Twitter](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="b5c68-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="b5c68-184">Scegliere il **creare un'App** collegamento e quindi accedere al sito.</span><span class="sxs-lookup"><span data-stu-id="b5c68-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="b5c68-185">Nel **creare un'applicazione** modulo, compilare il **nome** e **descrizione** campi.</span><span class="sxs-lookup"><span data-stu-id="b5c68-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="b5c68-186">Nel **sito Web** immettere l'URL del sito (ad esempio, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="b5c68-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="b5c68-187">Se si sta testando il sito locale (tramite un URL come `http://localhost:12345`), Twitter potrebbero non accettare l'URL.</span><span class="sxs-lookup"><span data-stu-id="b5c68-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="b5c68-188">Tuttavia, potrebbe essere in grado di utilizzare l'indirizzo IP di loopback locale (ad esempio `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="b5c68-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="b5c68-189">Questa operazione semplifica il processo di test dell'applicazione in locale.</span><span class="sxs-lookup"><span data-stu-id="b5c68-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="b5c68-190">Tuttavia, ogni volta che viene modificato il numero di porta del sito locale, è necessario aggiornare il **sito Web** campo dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b5c68-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="b5c68-191">Nel **URL Callback** immettere un URL per la pagina nel sito Web che si desidera che gli utenti restituire dopo la registrazione in Twitter.</span><span class="sxs-lookup"><span data-stu-id="b5c68-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="b5c68-192">Ad esempio, per inviare gli utenti alla home page del sito Starter (che verrà riconosciuto loro stato di accesso), immettere lo stesso URL immesso nel **sito Web** campo.</span><span class="sxs-lookup"><span data-stu-id="b5c68-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="b5c68-193">Accettare le condizioni e scegliere il **creare un'applicazione Twitter** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b5c68-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="b5c68-194">Nel **applicazioni personali** pagina di destinazione scegliere l'applicazione creata.</span><span class="sxs-lookup"><span data-stu-id="b5c68-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="b5c68-195">Nel **dettagli** scheda, scorrere verso il basso e scegliere il **creare risorse del Token di accesso** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b5c68-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="b5c68-196">Nel **dettagli** scheda, copiare il **chiave Consumer** e **segreto del cliente** i valori per l'applicazione e incollarli in un file di testo temporaneo.</span><span class="sxs-lookup"><span data-stu-id="b5c68-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="b5c68-197">Questi valori verrà passato al provider di Twitter nel codice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="b5c68-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="b5c68-198">Chiudere il sito di Twitter.</span><span class="sxs-lookup"><span data-stu-id="b5c68-198">Exit the Twitter site.</span></span>

<span data-ttu-id="b5c68-199">Ora è apportare modifiche a due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account Twitter.</span><span class="sxs-lookup"><span data-stu-id="b5c68-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="b5c68-200">Creare o aprire un sito ASP.NET Web Pages basato sul modello Starter Site di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="b5c68-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="b5c68-201">Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Twitter.</span><span class="sxs-lookup"><span data-stu-id="b5c68-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="b5c68-202">Il blocco di codice senza commenti è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b5c68-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="b5c68-203">Copia il **chiave Consumer** valore dall'applicazione Twitter come valore della `consumerKey` parametro (racchiuso tra virgolette).</span><span class="sxs-lookup"><span data-stu-id="b5c68-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="b5c68-204">Copia il **segreto del cliente** l'applicazione di Twitter come valore del valore di `consumerSecret` parametro.</span><span class="sxs-lookup"><span data-stu-id="b5c68-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="b5c68-205">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="b5c68-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="b5c68-206">Test di account di accesso Twitter</span><span class="sxs-lookup"><span data-stu-id="b5c68-206">Testing Twitter login</span></span>

1. <span data-ttu-id="b5c68-207">Eseguire il *cshtml* pagina del sito e scegliere il **accesso** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b5c68-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="b5c68-208">Nel *account di accesso* nella pagina di **utilizzare un altro servizio per l'accesso** , scegliere il **Twitter** icona.</span><span class="sxs-lookup"><span data-stu-id="b5c68-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="b5c68-209">La pagina web reindirizza la richiesta a una pagina di accesso Twitter per l'applicazione creata.</span><span class="sxs-lookup"><span data-stu-id="b5c68-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="b5c68-211">Accedere a un account Twitter.</span><span class="sxs-lookup"><span data-stu-id="b5c68-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="b5c68-212">Il codice Usa il token di Twitter per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso con l'account del sito Web.</span><span class="sxs-lookup"><span data-stu-id="b5c68-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="b5c68-213">Il nome o indirizzo di posta verrà inserita nel **posta elettronica** campo nel form.</span><span class="sxs-lookup"><span data-stu-id="b5c68-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="b5c68-215">Scegliere il **associare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b5c68-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="b5c68-216">Il browser restituisce alla home page e si è connessi.</span><span class="sxs-lookup"><span data-stu-id="b5c68-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b5c68-217">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b5c68-217">Additional Resources</span></span>


- [<span data-ttu-id="b5c68-218">Personalizzazione del comportamento a livello di sito</span><span class="sxs-lookup"><span data-stu-id="b5c68-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="b5c68-219">Aggiunta della protezione e l'appartenenza a un sito di pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b5c68-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
