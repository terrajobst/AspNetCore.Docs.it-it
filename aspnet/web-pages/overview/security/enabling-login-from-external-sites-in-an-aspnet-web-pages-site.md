---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: La registrazione utilizzando siti esterni in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: Questo articolo illustra come eseguire l'accesso al sito di ASP.NET Web Pages (Razor) usando Facebook, Google, Twitter, Yahoo e ad altri siti, vale a dire, come supportare...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: a74b13e9d1ddb5bc02f4ea5184108de5e014ead0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825854"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="c3460-103">La registrazione utilizzando siti esterni in un sito di ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="c3460-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="c3460-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c3460-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c3460-105">Questo articolo illustra come eseguire l'accesso al sito di ASP.NET Web Pages (Razor) usando Facebook, Google, Twitter, Yahoo e ad altri siti, vale a dire, sul supporto di OAuth e OpenID all'interno del sito.</span><span class="sxs-lookup"><span data-stu-id="c3460-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="c3460-106">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="c3460-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="c3460-107">Come abilitare l'accesso da altri siti quando si usa il modello di sito Starter WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c3460-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="c3460-108">Si tratta della funzionalità ASP.NET introdotta nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="c3460-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="c3460-109">Il `OAuthWebSecurity` helper.</span><span class="sxs-lookup"><span data-stu-id="c3460-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c3460-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c3460-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c3460-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="c3460-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="c3460-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="c3460-112">WebMatrix 3</span></span>

<span data-ttu-id="c3460-113">Include il supporto per ASP.NET Web Pages [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provider.</span><span class="sxs-lookup"><span data-stu-id="c3460-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="c3460-114">Con questi provider, è possibile consentire agli utenti di accedere al sito usando le proprie credenziali da Facebook, Twitter, Microsoft e Google.</span><span class="sxs-lookup"><span data-stu-id="c3460-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="c3460-115">Ad esempio, per accedere con un account di Facebook, gli utenti possono semplicemente scegliere un'icona di Facebook, che li reindirizza alla pagina di accesso di Facebook in cui immettere le informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="c3460-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="c3460-116">È quindi possibile associare l'account di accesso di Facebook con il proprio account nel sito.</span><span class="sxs-lookup"><span data-stu-id="c3460-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="c3460-117">Un miglioramento correlato alle funzionalità di appartenenza a pagine Web è che gli utenti possono associare più account di accesso (inclusi gli accessi dai siti di social networking) con un singolo account nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="c3460-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="c3460-118">La seguente immagine illustra la pagina di accesso dal **Starter Site** modello, in cui un utente può scegliere un'icona di Facebook, Twitter, Google o Microsoft per abilitare la registrazione con un account esterno:</span><span class="sxs-lookup"><span data-stu-id="c3460-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![provider esterni](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="c3460-120">È possibile abilitare l'appartenenza a OAuth e OpenID rimuovendo poche righe di codice nel **Starter Site** modello.</span><span class="sxs-lookup"><span data-stu-id="c3460-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="c3460-121">I metodi e proprietà si usano per lavorare con OAuth e OpenID provider è nel `WebMatrix.Security.OAuthWebSecurity` classe.</span><span class="sxs-lookup"><span data-stu-id="c3460-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="c3460-122">Il **Starter Site** modello include un'infrastruttura completa dell'appartenenza, completa di una pagina di accesso, un database di appartenenza e tutto il codice necessario consentire agli utenti di accedere al sito usando le credenziali locali o quelle di un altro sito .</span><span class="sxs-lookup"><span data-stu-id="c3460-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="c3460-123">In questa sezione fornisce un esempio di come consentire agli utenti di accedere da siti esterni a un sito di base di **Starter Site** modello.</span><span class="sxs-lookup"><span data-stu-id="c3460-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="c3460-124">Dopo aver creato un sito di base, tale scopo (dettagli):</span><span class="sxs-lookup"><span data-stu-id="c3460-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="c3460-125">Per i siti che usano un provider OAuth (Facebook, Twitter e Microsoft), si crea un'applicazione nel sito esterno.</span><span class="sxs-lookup"><span data-stu-id="c3460-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="c3460-126">In questo modo le chiavi dell'applicazione che è necessario per richiamare la funzionalità di accesso per tali siti.</span><span class="sxs-lookup"><span data-stu-id="c3460-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="c3460-127">Per i siti che usano un provider di OpenID (Google), non è necessario creare un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3460-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="c3460-128">Per tutti questi siti, è necessario un account per accedere e creare applicazioni per sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="c3460-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c3460-129">Le applicazioni Microsoft accettano solo un URL in tempo reale per un sito Web funzionante, non è possibile utilizzare un URL del sito Web locale per testare gli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="c3460-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="c3460-130">Modificare alcuni file nel sito Web per specificare il provider di autenticazione appropriato e per l'invio di un account di accesso al sito da usare.</span><span class="sxs-lookup"><span data-stu-id="c3460-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="c3460-131">Questo articolo fornisce istruzioni separate per le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="c3460-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="c3460-132">Abilitare gli account di accesso di Google</span><span class="sxs-lookup"><span data-stu-id="c3460-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="c3460-133">Abilitare gli account di accesso di Facebook</span><span class="sxs-lookup"><span data-stu-id="c3460-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="c3460-134">Abilitare gli account di accesso di Twitter</span><span class="sxs-lookup"><span data-stu-id="c3460-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="c3460-135">Abilitare gli account di accesso di Google</span><span class="sxs-lookup"><span data-stu-id="c3460-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="c3460-136">Creare o aprire un sito di ASP.NET Web Pages è basato sul modello di sito Starter WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c3460-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="c3460-137">Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento dalla riga di codice seguente.</span><span class="sxs-lookup"><span data-stu-id="c3460-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="c3460-138">Account di accesso di Google test</span><span class="sxs-lookup"><span data-stu-id="c3460-138">Testing Google login</span></span>

1. <span data-ttu-id="c3460-139">Eseguire la *default. cshtml* page del sito e scegliere il **Accedi** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c3460-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="c3460-140">Nel *account di accesso* nella pagina il **usi un altro servizio per l'accesso** , quindi scegliere il **Google** o **Yahoo** pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="c3460-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="c3460-141">Questo esempio Usa l'account di accesso di Google.</span><span class="sxs-lookup"><span data-stu-id="c3460-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="c3460-142">La pagina web reindirizza la richiesta alla pagina di accesso di Google.</span><span class="sxs-lookup"><span data-stu-id="c3460-142">The web page redirects the request to the Google login page.</span></span>

    ![Accesso Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="c3460-144">Immettere le credenziali per un account Google esistente.</span><span class="sxs-lookup"><span data-stu-id="c3460-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="c3460-145">Se Google viene chiesto se si vuole consentire *Localhost* per usare le informazioni dall'account, fare clic su **Consenti**.</span><span class="sxs-lookup"><span data-stu-id="c3460-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="c3460-146">Il codice Usa il token di Google per l'autenticazione dell'utente e quindi restituisce a questa pagina nel sito Web.</span><span class="sxs-lookup"><span data-stu-id="c3460-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="c3460-147">Questa pagina consente agli utenti di associare i loro account di accesso di Google con un account esistente nel sito Web, o possono registrare un nuovo account nel sito per associare l'account di accesso esterno con.</span><span class="sxs-lookup"><span data-stu-id="c3460-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="c3460-149">Scegliere il **associare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c3460-149">Choose the **Associate** button.</span></span> <span data-ttu-id="c3460-150">Il browser torna alla home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3460-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="c3460-151">Abilitare gli account di accesso di Facebook</span><span class="sxs-lookup"><span data-stu-id="c3460-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="c3460-152">Andare alla [sito degli sviluppatori Facebook](https://developers.facebook.com/apps) (accesso, se non è già stato effettuato).</span><span class="sxs-lookup"><span data-stu-id="c3460-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="c3460-153">Scegliere il **Create New App** pulsante e quindi seguire le istruzioni per assegnare un nome e creare la nuova applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3460-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="c3460-154">Nella sezione **selezionare come si integrerà l'app con Facebook**, scegliere il **sito Web** sezione.</span><span class="sxs-lookup"><span data-stu-id="c3460-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="c3460-155">Immettere il **URL sito** campo con l'URL del sito (ad esempio, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="c3460-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="c3460-156">Il **Domain** campo è facoltativo; è possibile utilizzare questo per fornire l'autenticazione per un intero dominio (ad esempio *example.com*).</span><span class="sxs-lookup"><span data-stu-id="c3460-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="c3460-157">Se si esegue un sito nel computer locale con un URL come `http://localhost:12345` (dove il numero è un numero di porta locale), è possibile aggiungere questo valore per il **URL del sito** campo per il test del sito.</span><span class="sxs-lookup"><span data-stu-id="c3460-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="c3460-158">Tuttavia, ogni volta che il numero di porta delle modifiche al sito locale, dovrai aggiornare il **URL sito** campo dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3460-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="c3460-159">Scegliere il **Save Changes** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c3460-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="c3460-160">Scegliere il **app** scheda nuovo e quindi visualizzare la pagina iniziale per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3460-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="c3460-161">Copia il **App ID** e **segreto App** i valori per l'applicazione e incollarli in un file di testo temporaneo.</span><span class="sxs-lookup"><span data-stu-id="c3460-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="c3460-162">Si passerà questi valori per il provider di Facebook nel codice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="c3460-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="c3460-163">Chiudere il sito per sviluppatori di Facebook.</span><span class="sxs-lookup"><span data-stu-id="c3460-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="c3460-164">A questo punto è apportare modifiche alle due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account di Facebook.</span><span class="sxs-lookup"><span data-stu-id="c3460-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="c3460-165">Creare o aprire un sito di ASP.NET Web Pages è basato sul modello di sito Starter WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c3460-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="c3460-166">Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Facebook.</span><span class="sxs-lookup"><span data-stu-id="c3460-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="c3460-167">Il blocco di codice senza commenti è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c3460-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="c3460-168">Copia il **App ID** valore dall'applicazione Facebook come valore del `appId` parametro (all'interno delle virgolette).</span><span class="sxs-lookup"><span data-stu-id="c3460-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="c3460-169">Copia **segreto App** valore dall'applicazione Facebook come il `appSecret` valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="c3460-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="c3460-170">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="c3460-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="c3460-171">Test di accesso di Facebook</span><span class="sxs-lookup"><span data-stu-id="c3460-171">Testing Facebook login</span></span>

1. <span data-ttu-id="c3460-172">Esecuzione del sito *default. cshtml* pagina, quindi scegliere il **Login** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c3460-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="c3460-173">Nel *account di accesso* nella pagina il **usi un altro servizio per l'accesso** keychains il **Facebook** icona.</span><span class="sxs-lookup"><span data-stu-id="c3460-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="c3460-174">La pagina web reindirizza la richiesta alla pagina di accesso di Facebook.</span><span class="sxs-lookup"><span data-stu-id="c3460-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="c3460-176">Accedere a un account di Facebook.</span><span class="sxs-lookup"><span data-stu-id="c3460-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="c3460-177">Il codice Usa il token di Facebook per l'autenticazione dell'utente e quindi torna in una pagina in cui è possibile associare l'account di accesso di Facebook con account di accesso del sito.</span><span class="sxs-lookup"><span data-stu-id="c3460-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="c3460-178">L'indirizzo di posta elettronica o nome utente viene compilato nel **messaggio di posta elettronica** campo nel form.</span><span class="sxs-lookup"><span data-stu-id="c3460-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="c3460-180">Scegliere il **associare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c3460-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="c3460-181">Il browser torna alla home page e si è connessi.</span><span class="sxs-lookup"><span data-stu-id="c3460-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="c3460-182">Abilitare gli account di accesso di Twitter</span><span class="sxs-lookup"><span data-stu-id="c3460-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="c3460-183">Selezionare il [sito agli sviluppatori di Twitter](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="c3460-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="c3460-184">Scegliere il **creare un'App** collegamento e quindi accedere al sito.</span><span class="sxs-lookup"><span data-stu-id="c3460-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="c3460-185">Nel **creare un'applicazione** formano, compilare il **Name** e **descrizione** campi.</span><span class="sxs-lookup"><span data-stu-id="c3460-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="c3460-186">Nel **sito Web** immettere l'URL del sito (ad esempio, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="c3460-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="c3460-187">Se si sta verificando il sito in locale (tramite un URL come `http://localhost:12345`), Twitter potrebbero non accettare l'URL.</span><span class="sxs-lookup"><span data-stu-id="c3460-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="c3460-188">Tuttavia, potrebbe essere possibile usare l'indirizzo IP di loopback locale (ad esempio `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="c3460-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="c3460-189">Ciò semplifica il processo di test dell'applicazione in locale.</span><span class="sxs-lookup"><span data-stu-id="c3460-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="c3460-190">Tuttavia, ogni volta che viene modificato il numero di porta del sito locale, è necessario aggiornare il **sito Web** campo dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3460-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="c3460-191">Nel **URL di Callback** immettere un URL per la pagina nel sito Web che si desidera che gli utenti a cui tornare dopo la registrazione in Twitter.</span><span class="sxs-lookup"><span data-stu-id="c3460-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="c3460-192">Ad esempio, per inviare gli utenti alla home page del sito Starter (che riconoscerà il relativo stato connesso), immettere lo stesso URL immesso nella **sito Web** campo.</span><span class="sxs-lookup"><span data-stu-id="c3460-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="c3460-193">Accettare le condizioni e scegliere il **Crea applicazione Twitter** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c3460-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="c3460-194">Nel **applicazioni personali** pagina di destinazione scegliere l'applicazione creata.</span><span class="sxs-lookup"><span data-stu-id="c3460-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="c3460-195">Nel **informazioni dettagliate** scheda, scorrere verso il basso e scegliere il **Create My Access Token** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c3460-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="c3460-196">Nel **dettagli** scheda, copiare la **Consumer Key** e **segreto Consumer** i valori per l'applicazione e incollarli in un file di testo temporaneo.</span><span class="sxs-lookup"><span data-stu-id="c3460-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="c3460-197">Questi valori verranno passate al provider di Twitter nel codice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="c3460-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="c3460-198">Uscire dal sito di Twitter.</span><span class="sxs-lookup"><span data-stu-id="c3460-198">Exit the Twitter site.</span></span>

<span data-ttu-id="c3460-199">A questo punto è apportare modifiche alle due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account Twitter.</span><span class="sxs-lookup"><span data-stu-id="c3460-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="c3460-200">Creare o aprire un sito di ASP.NET Web Pages è basato sul modello di sito Starter WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c3460-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="c3460-201">Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Twitter.</span><span class="sxs-lookup"><span data-stu-id="c3460-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="c3460-202">Il blocco di codice senza commenti è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="c3460-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="c3460-203">Copia il **Consumer Key** valore dell'applicazione Twitter come valore del `consumerKey` parametro (all'interno delle virgolette).</span><span class="sxs-lookup"><span data-stu-id="c3460-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="c3460-204">Copia il **Consumer Secret** valore dell'applicazione Twitter come valore del `consumerSecret` parametro.</span><span class="sxs-lookup"><span data-stu-id="c3460-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="c3460-205">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="c3460-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="c3460-206">Test di accesso a Twitter</span><span class="sxs-lookup"><span data-stu-id="c3460-206">Testing Twitter login</span></span>

1. <span data-ttu-id="c3460-207">Eseguire la *default. cshtml* page del sito e scegliere il **Login** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c3460-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="c3460-208">Nel *account di accesso* nella pagina il **usi un altro servizio per l'accesso** keychains il **Twitter** icona.</span><span class="sxs-lookup"><span data-stu-id="c3460-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="c3460-209">La pagina web reindirizza la richiesta a una pagina di accesso di Twitter per l'applicazione creata.</span><span class="sxs-lookup"><span data-stu-id="c3460-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="c3460-211">Accedere a un account Twitter.</span><span class="sxs-lookup"><span data-stu-id="c3460-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="c3460-212">Il codice Usa il token di Twitter per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso con l'account del sito Web.</span><span class="sxs-lookup"><span data-stu-id="c3460-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="c3460-213">L'indirizzo di posta elettronica o nome viene compilato nel **messaggio di posta elettronica** campo nel form.</span><span class="sxs-lookup"><span data-stu-id="c3460-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="c3460-215">Scegliere il **associare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c3460-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="c3460-216">Il browser torna alla home page e si è connessi.</span><span class="sxs-lookup"><span data-stu-id="c3460-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="c3460-217">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c3460-217">Additional Resources</span></span>


- [<span data-ttu-id="c3460-218">Personalizzazione del comportamento a livello di sito</span><span class="sxs-lookup"><span data-stu-id="c3460-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="c3460-219">L'aggiunta della protezione e l'appartenenza a un sito con pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c3460-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
