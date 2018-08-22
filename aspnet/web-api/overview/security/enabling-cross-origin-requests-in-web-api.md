---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Abilitazione di richieste Multiorigine nell'API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Viene illustrato come il supporto di condivisione delle risorse Multiorigine (CORS) nell'API Web ASP.NET.
ms.author: riande
ms.date: 07/15/2014
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: eddf61a4468807f5efd658438c1c27a1d2f9c486
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826768"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="1e0dc-103">Abilitazione di richieste Multiorigine nell'API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="1e0dc-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="1e0dc-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1e0dc-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="1e0dc-105">Protezione del browser impedisce a una pagina web da creare richieste AJAX a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="1e0dc-106">Questa restrizione viene chiamata il *criterio della stessa origine*e impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="1e0dc-107">Tuttavia, in alcuni casi si potrebbe voler consentire ad altri siti di chiamare l'API web.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="1e0dc-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (le origini CORS) è uno standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="1e0dc-109">Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="1e0dc-110">CORS è più sicuri e flessibili rispetto a tecniche precedenti, ad esempio [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="1e0dc-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="1e0dc-111">Questa esercitazione illustra come abilitare CORS nell'applicazione API Web.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1e0dc-112">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="1e0dc-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="1e0dc-113">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="1e0dc-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="1e0dc-114">API Web 2.2</span><span class="sxs-lookup"><span data-stu-id="1e0dc-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="1e0dc-115">Introduzione</span><span class="sxs-lookup"><span data-stu-id="1e0dc-115">Introduction</span></span>

<span data-ttu-id="1e0dc-116">Questa esercitazione illustra che il supporto di CORS nell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="1e0dc-117">Si inizierà creando due progetti ASP.NET – uno chiamato "servizio Web", che ospita un controller API Web, e l'altro denominato "WebClient", che chiama WebService.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="1e0dc-118">Poiché le due applicazioni sono ospitate in domini diversi, una richiesta AJAX da WebClient per servizio Web è una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="1e0dc-119">Che cos'è "Origin stesso"?</span><span class="sxs-lookup"><span data-stu-id="1e0dc-119">What is "Same Origin"?</span></span>

<span data-ttu-id="1e0dc-120">Due URL abbia la stessa origine se dispongono di porte, host e gli schemi identici.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="1e0dc-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="1e0dc-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="1e0dc-122">Questi due URL abbia la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="1e0dc-123">Questi URL sono le entità Origin diversa rispetto al precedente due:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="1e0dc-124">`http://example.net` -Altro dominio</span><span class="sxs-lookup"><span data-stu-id="1e0dc-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="1e0dc-125">`http://example.com:9000/foo.html` -Porta diversa</span><span class="sxs-lookup"><span data-stu-id="1e0dc-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="1e0dc-126">`https://example.com/foo.html` -Schema differente</span><span class="sxs-lookup"><span data-stu-id="1e0dc-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="1e0dc-127">`http://www.example.com/foo.html` -Sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="1e0dc-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="1e0dc-128">Internet Explorer non considera la porta quando si confrontano le entità Origin.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="1e0dc-129">Creare il progetto servizio Web</span><span class="sxs-lookup"><span data-stu-id="1e0dc-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="1e0dc-130">In questa sezione presuppone che si conosca già come creare progetti API Web.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="1e0dc-131">In caso contrario, vedere [Introduzione all'API Web ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1e0dc-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="1e0dc-132">Avviare Visual Studio e creare una nuova **applicazione Web ASP.NET** progetto.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="1e0dc-133">Selezionare il **vuoto** modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-133">Select the **Empty** project template.</span></span> <span data-ttu-id="1e0dc-134">In "Aggiungere cartelle e riferimenti per la funzionalità di base", selezionare la **API Web** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="1e0dc-135">Facoltativamente, selezionare l'opzione "Ospita nel Cloud" per distribuire l'app in Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="1e0dc-136">Microsoft offre l'hosting web gratuito per fino a 10 siti Web in un [account Azure gratuito](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="1e0dc-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="1e0dc-137">Aggiungere un controller API Web denominato `TestController` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="1e0dc-138">È possibile eseguire l'applicazione in locale o distribuirla in Azure.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="1e0dc-139">(Per le schermate contenute in questa esercitazione, distribuito in App Web di servizio App di Azure.) Per verificare che l'API web è operativa, passare a `http://hostname/api/test/`, dove *hostname* è il dominio in cui è distribuita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="1e0dc-140">Il testo di risposta, dovrebbe &quot;OTTIENI: messaggio di prova&quot;.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="1e0dc-141">Creare il progetto WebClient</span><span class="sxs-lookup"><span data-stu-id="1e0dc-141">Create the WebClient Project</span></span>

<span data-ttu-id="1e0dc-142">Creare un altro progetto di applicazione Web ASP.NET e selezionare il **MVC** modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="1e0dc-143">Facoltativamente, selezionare **Modifica autenticazione** > **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="1e0dc-144">L'autenticazione non è necessario per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="1e0dc-145">In Esplora soluzioni aprire il file Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="1e0dc-146">Sostituire il codice in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="1e0dc-147">Per il *serviceUrl* variabile, usare l'URI del servizio Web app.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="1e0dc-148">A questo punto eseguire localmente l'app WebClient o pubblicarla in un altro sito Web.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="1e0dc-149">Scegliendo il pulsante "prova" Invia una richiesta AJAX all'app, servizio Web utilizzando il metodo HTTP elencato nella casella a discesa (GET, POST o PUT).</span><span class="sxs-lookup"><span data-stu-id="1e0dc-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="1e0dc-150">Questo modo è possibile esaminare le diverse richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="1e0dc-151">Al momento, l'app del servizio Web non supportano CORS, in modo che se si fa clic sul pulsante, si otterrà un errore.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="1e0dc-152">Se si osserva il traffico HTTP in uno strumento, ad esempio [Fiddler](http://www.telerik.com/fiddler), si noterà che il browser invia la richiesta GET e la richiesta ha esito positivo, ma la chiamata AJAX restituisce un errore.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="1e0dc-153">È importante comprendere che Criteri di corrispondenza dell'origine non impediscano il browser dal *invio* la richiesta.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="1e0dc-154">Al contrario, impedisce all'applicazione di visualizzare il *risposta*.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="1e0dc-155">Abilitare CORS</span><span class="sxs-lookup"><span data-stu-id="1e0dc-155">Enable CORS</span></span>

<span data-ttu-id="1e0dc-156">A questo punto è possibile abilitare CORS nell'app di servizio Web.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="1e0dc-157">In primo luogo, aggiungere il pacchetto NuGet di CORS.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="1e0dc-158">In Visual Studio dal **degli strumenti** dal menu **Library Package Manager**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-158">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1e0dc-159">Nella finestra della Console di gestione pacchetti, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="1e0dc-160">Questo comando installa il pacchetto più recente e aggiorna tutte le dipendenze, incluse le librerie di API Web di base.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="1e0dc-161">Utente-flag di versione come destinazione una versione specifica.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="1e0dc-162">Il pacchetto CORS richiede Web API 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="1e0dc-163">Aprire il file App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="1e0dc-164">Aggiungere il codice seguente per il **webapiconfig. Register** (metodo).</span><span class="sxs-lookup"><span data-stu-id="1e0dc-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="1e0dc-165">Successivamente, aggiungere il **[EnableCors]** dell'attributo di `TestController` classe:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="1e0dc-166">Per il *le entità Origin* parametro, usare l'URI in cui è distribuita l'applicazione WebClient.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="1e0dc-167">In questo modo le richieste multiorigine da WebClient, mentre comunque impedire tutte le altre richieste tra domini.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="1e0dc-168">In un secondo momento, descriverò i parametri per **[EnableCors]** in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="1e0dc-169">Non includere una barra rovesciata alla fine del *le entità Origin* URL.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="1e0dc-170">Ridistribuire l'applicazione di servizio Web aggiornato.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="1e0dc-171">Non devi aggiornare WebClient.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-171">You don't need to update WebClient.</span></span> <span data-ttu-id="1e0dc-172">A questo punto la richiesta AJAX da WebClient avrà esito positivo.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="1e0dc-173">Sono consentiti tutti i metodi GET, PUT e POST.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="1e0dc-174">Funzionamento della condivisione CORS</span><span class="sxs-lookup"><span data-stu-id="1e0dc-174">How CORS Works</span></span>

<span data-ttu-id="1e0dc-175">In questa sezione viene descritto cosa succede in una richiesta CORS, il livello dei messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="1e0dc-176">È importante comprendere il funzionamento di CORS, in modo che sia possibile configurare il **[EnableCors]** attributo correttamente e risolvere i problemi se ciò non funziona come previsto.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="1e0dc-177">La specifica CORS introduce diverse nuove intestazioni HTTP che attivare richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="1e0dc-178">Se un browser supporta la condivisione CORS, imposta le intestazioni automaticamente per le richieste multiorigine. non è necessario eseguire alcuna operazione speciale nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="1e0dc-179">Di seguito è riportato un esempio di una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="1e0dc-180">L'intestazione "Origin" offre il dominio del sito che effettua la richiesta.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="1e0dc-181">Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="1e0dc-182">Il valore di questa intestazione corrisponde all'intestazione di origine o è il valore del carattere jolly "\*", che significa che è consentita qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="1e0dc-183">Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX non riesce.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="1e0dc-184">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="1e0dc-185">Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'applicazione client.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="1e0dc-186">**Richieste preliminari**</span><span class="sxs-lookup"><span data-stu-id="1e0dc-186">**Preflight Requests**</span></span>

<span data-ttu-id="1e0dc-187">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, definita "richiesta preliminare," prima dell'invio della richiesta effettiva per la risorsa.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="1e0dc-188">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="1e0dc-189">Il metodo di richiesta è GET, HEAD o POST, *e*</span><span class="sxs-lookup"><span data-stu-id="1e0dc-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="1e0dc-190">L'applicazione non imposta le intestazioni di richiesta diversa da Accept, Accept-Language, Content-Language, Content-Type o Last-eventi-ID, *e*</span><span class="sxs-lookup"><span data-stu-id="1e0dc-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="1e0dc-191">L'intestazione Content-Type (se impostato) è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="1e0dc-192">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="1e0dc-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="1e0dc-193">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="1e0dc-193">multipart/form-data</span></span>
    - <span data-ttu-id="1e0dc-194">text/plain</span><span class="sxs-lookup"><span data-stu-id="1e0dc-194">text/plain</span></span>

<span data-ttu-id="1e0dc-195">Si applica la regola sulle intestazioni di richiesta alle intestazioni che l'applicazione imposta chiamando **setRequestHeader** nel **XMLHttpRequest** oggetto.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="1e0dc-196">(Specifica CORS chiama questi "intestazioni di richiesta autore"). La regola non si applica alle intestazioni il *browser* impostare, ad esempio User-Agent, Host o Content-Length.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="1e0dc-197">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="1e0dc-198">La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="1e0dc-199">Include due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-199">It includes two special headers:</span></span>

- <span data-ttu-id="1e0dc-200">Access-Control-Request-Method: Metodo HTTP che verrà usato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="1e0dc-201">Access-Control-Request-Headers: Un elenco di intestazioni di richiesta che il *applicazione* impostare per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="1e0dc-202">(Anche in questo caso, ciò non includono le intestazioni che imposta il browser.)</span><span class="sxs-lookup"><span data-stu-id="1e0dc-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="1e0dc-203">Ecco un esempio di risposta, presupponendo che il server consente la richiesta:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="1e0dc-204">La risposta include un'intestazione Access-Control-Allow-Methods in cui sono elencati i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Consenti-Headers, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="1e0dc-205">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="1e0dc-206">Regole di ambito per [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="1e0dc-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="1e0dc-207">È possibile abilitare CORS per ogni azione, per ogni controller o a livello globale per tutti i controller API Web nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="1e0dc-208">**Per ogni azione**</span><span class="sxs-lookup"><span data-stu-id="1e0dc-208">**Per Action**</span></span>

<span data-ttu-id="1e0dc-209">Per abilitare CORS per una singola azione, impostare il **[EnableCors]** attributo del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="1e0dc-210">L'esempio seguente Abilita CORS per il `GetItem` solo metodo.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="1e0dc-211">**Per ogni Controller**</span><span class="sxs-lookup"><span data-stu-id="1e0dc-211">**Per Controller**</span></span>

<span data-ttu-id="1e0dc-212">Se si imposta **[EnableCors]** nella classe controller, si applica a tutte le azioni nel controller.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="1e0dc-213">Per disabilitare CORS per un'azione, aggiungere il **[DisableCors]** attributo all'azione.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="1e0dc-214">L'esempio seguente Abilita CORS per qualsiasi metodo ad eccezione `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="1e0dc-215">**A livello globale**</span><span class="sxs-lookup"><span data-stu-id="1e0dc-215">**Globally**</span></span>

<span data-ttu-id="1e0dc-216">Per abilitare CORS per tutti i controller API Web nell'applicazione, passare un **EnableCorsAttribute** dell'istanza per il **EnableCors** metodo:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="1e0dc-217">Se si imposta l'attributo più di un ambito, l'ordine di precedenza è:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="1e0dc-218">Operazione</span><span class="sxs-lookup"><span data-stu-id="1e0dc-218">Action</span></span>
2. <span data-ttu-id="1e0dc-219">Controller</span><span class="sxs-lookup"><span data-stu-id="1e0dc-219">Controller</span></span>
3. <span data-ttu-id="1e0dc-220">Global</span><span class="sxs-lookup"><span data-stu-id="1e0dc-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="1e0dc-221">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="1e0dc-221">Set the Allowed Origins</span></span>

<span data-ttu-id="1e0dc-222">Il *le entità Origin* parametro delle **[EnableCors]** attributo specifica quali origini sono consentite per accedere alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="1e0dc-223">Il valore è un elenco delimitato da virgole di origini consentite.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="1e0dc-224">È anche possibile usare il valore del carattere jolly "\*" per consentire le richieste da tutte le entità Origin.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="1e0dc-225">Si consideri con attenzione prima di consentire le richieste provenienti da qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="1e0dc-226">Significa che letteralmente qualsiasi sito Web può eseguire chiamate AJAX all'API Web.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="1e0dc-227">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="1e0dc-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="1e0dc-228">Il *metodi* parametro delle **[EnableCors]** attributo specifica quali metodi HTTP consentiti per accedere alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="1e0dc-229">Per consentire tutti i metodi, usare il valore del carattere jolly "\*".</span><span class="sxs-lookup"><span data-stu-id="1e0dc-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="1e0dc-230">Nell'esempio seguente consente solo le richieste GET e POST.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="1e0dc-231">Impostare le intestazioni di richieste consentite</span><span class="sxs-lookup"><span data-stu-id="1e0dc-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="1e0dc-232">In precedenza è stato descritto come una richiesta preliminare può includere un'intestazione Access-Control-Request-Headers, elenca le intestazioni HTTP impostate dall'applicazione (la cosiddetta "creare le intestazioni delle richieste").</span><span class="sxs-lookup"><span data-stu-id="1e0dc-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="1e0dc-233">Il *intestazioni* parametro delle **[EnableCors]** attributo specifica quali intestazioni di richiesta di autore sono consentite.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="1e0dc-234">Per consentire tutte le intestazioni, impostare *intestazioni* a "\*".</span><span class="sxs-lookup"><span data-stu-id="1e0dc-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="1e0dc-235">A elenco elementi consentiti intestazioni specifiche, impostare *intestazioni* a un elenco delimitato da virgole delle intestazioni consentite:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="1e0dc-236">Tuttavia, i browser non sono completamente coerenti nel modo in cui siano impostate Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="1e0dc-237">Ad esempio Chrome include attualmente "origin"; mentre FireFox non include le intestazioni standard, ad esempio "Accept", anche quando l'applicazione vengono impostati nello script.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="1e0dc-238">Se si imposta *intestazioni* su un valore qualsiasi diverso da "\*", è necessario includere almeno "accept", "content-type" e "origin", oltre a eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="1e0dc-239">Impostare le intestazioni di risposta consentite</span><span class="sxs-lookup"><span data-stu-id="1e0dc-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="1e0dc-240">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="1e0dc-241">Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="1e0dc-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="1e0dc-242">Cache-Control</span></span>
- <span data-ttu-id="1e0dc-243">Content-Language</span><span class="sxs-lookup"><span data-stu-id="1e0dc-243">Content-Language</span></span>
- <span data-ttu-id="1e0dc-244">Content-Type</span><span class="sxs-lookup"><span data-stu-id="1e0dc-244">Content-Type</span></span>
- <span data-ttu-id="1e0dc-245">Alla scadenza</span><span class="sxs-lookup"><span data-stu-id="1e0dc-245">Expires</span></span>
- <span data-ttu-id="1e0dc-246">Ultima modifica</span><span class="sxs-lookup"><span data-stu-id="1e0dc-246">Last-Modified</span></span>
- <span data-ttu-id="1e0dc-247">(Pragma)</span><span class="sxs-lookup"><span data-stu-id="1e0dc-247">Pragma</span></span>

<span data-ttu-id="1e0dc-248">La specifica CORS chiama questi [intestazioni di risposta semplice](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="1e0dc-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="1e0dc-249">Per rendere altre intestazioni disponibili per l'applicazione, impostare il *exposedHeaders* del parametro **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="1e0dc-250">Nell'esempio seguente, il controller `Get` metodo imposta un'intestazione personalizzata denominata "X-Custom-Header".</span><span class="sxs-lookup"><span data-stu-id="1e0dc-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="1e0dc-251">Per impostazione predefinita, il browser non espone questa intestazione in una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="1e0dc-252">Per rendere disponibile l'intestazione, includere "X-Custom-Header" nella *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="1e0dc-253">Passaggio di credenziali in richieste Multiorigine</span><span class="sxs-lookup"><span data-stu-id="1e0dc-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="1e0dc-254">Credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="1e0dc-255">Per impostazione predefinita, il browser invia credenziali con una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="1e0dc-256">Le credenziali includono i cookie, nonché gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="1e0dc-257">Per inviare le credenziali con una richiesta multiorigine, il client deve impostare **XMLHttpRequest.withCredentials** su true.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="1e0dc-258">Usando **XMLHttpRequest** direttamente:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="1e0dc-259">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="1e0dc-260">Inoltre, il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="1e0dc-261">Per consentire le credenziali multiorigine nell'API Web, impostare il **SupportsCredentials** proprietà su true nella **[EnableCors]** attributo:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="1e0dc-262">Se questa proprietà è true, la risposta HTTP includerà un'intestazione di accesso-controllo-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="1e0dc-263">Questa intestazione indica al browser che il server consenta le credenziali per una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="1e0dc-264">Se il browser invia le credenziali, ma la risposta non include un'intestazione di accesso-controllo-Allow-Credentials valida, il browser non espone la risposta all'applicazione e la richiesta AJAX non riesce.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="1e0dc-265">Impostazione cautela **SupportsCredentials** su true, perché significa che un sito Web all'indirizzo di un altro dominio può inviare le credenziali dell'utente ha eseguito l'accesso all'API Web per conto dell'utente, senza che l'utente ne sia consapevole.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="1e0dc-266">La specifica CORS inoltre stabilito che l'impostazione *le entità Origin* al &quot; \* &quot; non è valido se **SupportsCredentials** è true.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="1e0dc-267">Provider criteri CORS personalizzato</span><span class="sxs-lookup"><span data-stu-id="1e0dc-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="1e0dc-268">Il **[EnableCors]** attributo implementa le **ICorsPolicyProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="1e0dc-269">È possibile fornire un'implementazione personalizzata creando una classe che deriva da **attributo** e implementa **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="1e0dc-270">A questo punto è possibile applicare l'attributo in altre posizioni che si inseriscono **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="1e0dc-271">Ad esempio, un provider criteri CORS personalizzato è stato possibile leggere le impostazioni da un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="1e0dc-272">Come alternativa all'uso di attributi, è possibile registrare un' **ICorsPolicyProviderFactory** oggetto creato da **ICorsPolicyProvider** oggetti.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="1e0dc-273">Per impostare il **ICorsPolicyProviderFactory**, chiamare il **SetCorsPolicyProviderFactory** metodo di estensione all'avvio, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="1e0dc-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="1e0dc-274">Supporto del browser</span><span class="sxs-lookup"><span data-stu-id="1e0dc-274">Browser Support</span></span>

<span data-ttu-id="1e0dc-275">Il pacchetto di CORS per l'API Web è una tecnologia lato server.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="1e0dc-276">Il browser dell'utente deve anche supportare CORS.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="1e0dc-277">Fortunatamente, includono le versioni correnti di tutti i principali browser [supporto per CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="1e0dc-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="1e0dc-278">Internet Explorer 8 e Internet Explorer 9 hanno un supporto parziale per la condivisione CORS, utilizzando l'oggetto XDomainRequest legacy anziché XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="1e0dc-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="1e0dc-279">Per altre informazioni, vedere [XDomainRequest - le restrizioni, limitazioni e le soluzioni alternative](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e0dc-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
