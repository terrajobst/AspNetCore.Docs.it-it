---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Abilitazione di richieste Multiorigine nell'API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Viene illustrato come il supporto di condivisione delle risorse Multiorigine (CORS) nell'API Web ASP.NET.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97a0027194b019b09e220493dcb593e682027fe3
ms.sourcegitcommit: d22b3c23c45a076c4f394a70b1c8df2fbcdf656d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55428447"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="50134-103">Attivare richieste multiorigine nell'API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="50134-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="50134-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="50134-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="50134-105">Protezione del browser impedisce a una pagina web da creare richieste AJAX a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="50134-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="50134-106">Questa restrizione viene chiamata il *criterio della stessa origine*e impedisce a un sito dannoso di leggere dati sensibili da un altro sito.</span><span class="sxs-lookup"><span data-stu-id="50134-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="50134-107">Tuttavia, in alcuni casi si potrebbe voler consentire ad altri siti di chiamare l'API web.</span><span class="sxs-lookup"><span data-stu-id="50134-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="50134-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (le origini CORS) è uno standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine.</span><span class="sxs-lookup"><span data-stu-id="50134-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="50134-109">Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.</span><span class="sxs-lookup"><span data-stu-id="50134-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="50134-110">CORS è più sicuri e flessibili rispetto a tecniche precedenti, ad esempio [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="50134-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="50134-111">Questa esercitazione illustra come abilitare CORS nell'applicazione API Web.</span><span class="sxs-lookup"><span data-stu-id="50134-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="50134-112">Software utilizzato nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="50134-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="50134-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="50134-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="50134-114">API Web 2.2</span><span class="sxs-lookup"><span data-stu-id="50134-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="50134-115">Introduzione</span><span class="sxs-lookup"><span data-stu-id="50134-115">Introduction</span></span>

<span data-ttu-id="50134-116">Questa esercitazione illustra che il supporto di CORS nell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="50134-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="50134-117">Si inizierà creando due progetti ASP.NET – uno chiamato "servizio Web", che ospita un controller API Web, e l'altro denominato "WebClient", che chiama WebService.</span><span class="sxs-lookup"><span data-stu-id="50134-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="50134-118">Poiché le due applicazioni sono ospitate in domini diversi, una richiesta AJAX da WebClient per servizio Web è una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="50134-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="50134-119">Che cos'è "origin stesso"?</span><span class="sxs-lookup"><span data-stu-id="50134-119">What is "same origin"?</span></span>

<span data-ttu-id="50134-120">Due URL abbia la stessa origine se dispongono di porte, host e gli schemi identici.</span><span class="sxs-lookup"><span data-stu-id="50134-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="50134-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="50134-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="50134-122">Questi due URL abbia la stessa origine:</span><span class="sxs-lookup"><span data-stu-id="50134-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="50134-123">Questi URL sono le entità Origin diversa rispetto al precedente due:</span><span class="sxs-lookup"><span data-stu-id="50134-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="50134-124">`http://example.net` -Altro dominio</span><span class="sxs-lookup"><span data-stu-id="50134-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="50134-125">`http://example.com:9000/foo.html` -Porta diversa</span><span class="sxs-lookup"><span data-stu-id="50134-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="50134-126">`https://example.com/foo.html` -Schema differente</span><span class="sxs-lookup"><span data-stu-id="50134-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="50134-127">`http://www.example.com/foo.html` -Sottodominio diverso</span><span class="sxs-lookup"><span data-stu-id="50134-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="50134-128">Internet Explorer non considera la porta quando si confrontano le entità Origin.</span><span class="sxs-lookup"><span data-stu-id="50134-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="50134-129">Creare il progetto servizio Web</span><span class="sxs-lookup"><span data-stu-id="50134-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="50134-130">In questa sezione presuppone che si conosca già come creare progetti API Web.</span><span class="sxs-lookup"><span data-stu-id="50134-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="50134-131">In caso contrario, vedere [Introduzione all'API Web ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="50134-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="50134-132">Avviare Visual Studio e creare una nuova **applicazione Web ASP.NET (.NET Framework)** progetto.</span><span class="sxs-lookup"><span data-stu-id="50134-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="50134-133">Nel **nuova applicazione Web ASP.NET** finestra di dialogo, seleziona la **vuota** modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="50134-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="50134-134">Sotto **aggiungere le cartelle e riferimenti per di base**, selezionare la **API Web** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="50134-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![Finestra Nuovo progetto ASP.NET in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="50134-136">Aggiungere un controller API Web denominato `TestController` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="50134-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="50134-137">È possibile eseguire l'applicazione in locale o distribuirla in Azure.</span><span class="sxs-lookup"><span data-stu-id="50134-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="50134-138">(Per le schermate contenute in questa esercitazione, distribuire l'app per App Web di servizio App di Azure.) Per verificare che l'API web è operativa, passare a `http://hostname/api/test/`, dove *hostname* è il dominio in cui è distribuita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="50134-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="50134-139">Si dovrebbe vedere il testo di risposta, &quot;ottenere: Messaggio di prova&quot;.</span><span class="sxs-lookup"><span data-stu-id="50134-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Messaggio di prova di Web browser che mostra](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="50134-141">Creare il progetto WebClient</span><span class="sxs-lookup"><span data-stu-id="50134-141">Create the WebClient project</span></span>

1. <span data-ttu-id="50134-142">Creare un'altra **applicazione Web ASP.NET (.NET Framework)** del progetto e selezionare il **MVC** modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="50134-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="50134-143">Facoltativamente, selezionare **Modifica autenticazione** > **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="50134-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="50134-144">L'autenticazione non è necessario per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="50134-144">You don't need authentication for this tutorial.</span></span>

   ![Modello MVC nella finestra di dialogo Nuovo progetto ASP.NET in Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="50134-146">Nelle **Esplora soluzioni**, aprire il file *Views/Home/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="50134-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="50134-147">Sostituire il codice in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="50134-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="50134-148">Per il *serviceUrl* variabile, usare l'URI del servizio Web app.</span><span class="sxs-lookup"><span data-stu-id="50134-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="50134-149">Eseguire localmente l'app WebClient o pubblicarla in un altro sito Web.</span><span class="sxs-lookup"><span data-stu-id="50134-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="50134-150">Quando si sceglie il pulsante "prova", viene inviata una richiesta AJAX per l'app di servizio Web utilizzando il metodo HTTP elencato nella casella a discesa (GET, POST o PUT).</span><span class="sxs-lookup"><span data-stu-id="50134-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="50134-151">Ciò consente di esaminare le diverse richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="50134-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="50134-152">Attualmente, l'app del servizio Web non supportano CORS, in modo che se si fa clic sul pulsante, si otterrà un errore.</span><span class="sxs-lookup"><span data-stu-id="50134-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

!["Prova" errore nel browser](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="50134-154">Se si osserva il traffico HTTP in uno strumento, ad esempio [Fiddler](https://www.telerik.com/fiddler), si noterà che il browser invia la richiesta GET e la richiesta ha esito positivo, ma la chiamata AJAX restituisce un errore.</span><span class="sxs-lookup"><span data-stu-id="50134-154">If you watch the HTTP traffic in a tool like [Fiddler](https://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="50134-155">È importante comprendere che Criteri di corrispondenza dell'origine non impediscano il browser dal *invio* la richiesta.</span><span class="sxs-lookup"><span data-stu-id="50134-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="50134-156">Al contrario, impedisce all'applicazione di visualizzare il *risposta*.</span><span class="sxs-lookup"><span data-stu-id="50134-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Debugger web Fiddler che mostra le richieste web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="50134-158">Abilitare CORS</span><span class="sxs-lookup"><span data-stu-id="50134-158">Enable CORS</span></span>

<span data-ttu-id="50134-159">A questo punto è possibile abilitare CORS nell'app di servizio Web.</span><span class="sxs-lookup"><span data-stu-id="50134-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="50134-160">In primo luogo, aggiungere il pacchetto NuGet di CORS.</span><span class="sxs-lookup"><span data-stu-id="50134-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="50134-161">In Visual Studio dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="50134-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="50134-162">Nella finestra della Console di gestione pacchetti, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="50134-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="50134-163">Questo comando installa il pacchetto più recente e aggiorna tutte le dipendenze, incluse le librerie di API Web di base.</span><span class="sxs-lookup"><span data-stu-id="50134-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="50134-164">Usare il `-Version` flag a una versione specifica.</span><span class="sxs-lookup"><span data-stu-id="50134-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="50134-165">Il pacchetto CORS richiede Web API 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="50134-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="50134-166">Aprire il file *App\_Start/WebApiConfig.cs*.</span><span class="sxs-lookup"><span data-stu-id="50134-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="50134-167">Aggiungere il codice seguente per il **webapiconfig. Register** metodo:</span><span class="sxs-lookup"><span data-stu-id="50134-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="50134-168">Successivamente, aggiungere il **[EnableCors]** dell'attributo di `TestController` classe:</span><span class="sxs-lookup"><span data-stu-id="50134-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="50134-169">Per il *le entità Origin* parametro, usare l'URI in cui è distribuita l'applicazione WebClient.</span><span class="sxs-lookup"><span data-stu-id="50134-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="50134-170">In questo modo le richieste multiorigine da WebClient, mentre comunque impedire tutte le altre richieste tra domini.</span><span class="sxs-lookup"><span data-stu-id="50134-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="50134-171">In un secondo momento, descriverò i parametri per **[EnableCors]** in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="50134-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="50134-172">Non includere una barra rovesciata alla fine del *le entità Origin* URL.</span><span class="sxs-lookup"><span data-stu-id="50134-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="50134-173">Ridistribuire l'applicazione di servizio Web aggiornato.</span><span class="sxs-lookup"><span data-stu-id="50134-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="50134-174">Non devi aggiornare WebClient.</span><span class="sxs-lookup"><span data-stu-id="50134-174">You don't need to update WebClient.</span></span> <span data-ttu-id="50134-175">A questo punto la richiesta AJAX da WebClient avrà esito positivo.</span><span class="sxs-lookup"><span data-stu-id="50134-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="50134-176">Sono consentiti tutti i metodi GET, PUT e POST.</span><span class="sxs-lookup"><span data-stu-id="50134-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Messaggio di test riuscito di Web browser che mostra](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="50134-178">Funzionamento della condivisione CORS</span><span class="sxs-lookup"><span data-stu-id="50134-178">How CORS Works</span></span>

<span data-ttu-id="50134-179">In questa sezione viene descritto cosa succede in una richiesta CORS, il livello dei messaggi HTTP.</span><span class="sxs-lookup"><span data-stu-id="50134-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="50134-180">È importante comprendere il funzionamento di CORS, in modo che sia possibile configurare il **[EnableCors]** attributo correttamente e risolvere i problemi se ciò non funziona come previsto.</span><span class="sxs-lookup"><span data-stu-id="50134-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="50134-181">La specifica CORS introduce diverse nuove intestazioni HTTP che attivare richieste multiorigine.</span><span class="sxs-lookup"><span data-stu-id="50134-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="50134-182">Se un browser supporta la condivisione CORS, imposta le intestazioni automaticamente per le richieste multiorigine. non è necessario eseguire alcuna operazione speciale nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="50134-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="50134-183">Di seguito è riportato un esempio di una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="50134-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="50134-184">L'intestazione "Origin" offre il dominio del sito che effettua la richiesta.</span><span class="sxs-lookup"><span data-stu-id="50134-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="50134-185">Se il server consente la richiesta, imposta l'intestazione Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="50134-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="50134-186">Il valore di questa intestazione corrisponde all'intestazione di origine o è il valore del carattere jolly "\*", che significa che è consentita qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="50134-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="50134-187">Se la risposta non include l'intestazione Access-Control-Allow-Origin, la richiesta AJAX non riesce.</span><span class="sxs-lookup"><span data-stu-id="50134-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="50134-188">In particolare, il browser non consente la richiesta.</span><span class="sxs-lookup"><span data-stu-id="50134-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="50134-189">Anche se il server restituisce una risposta con esito positivo, il browser non rende la risposta disponibili per l'applicazione client.</span><span class="sxs-lookup"><span data-stu-id="50134-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="50134-190">**Richieste preliminari**</span><span class="sxs-lookup"><span data-stu-id="50134-190">**Preflight Requests**</span></span>

<span data-ttu-id="50134-191">Per alcune richieste CORS, il browser invia una richiesta aggiuntiva, definita "richiesta preliminare," prima dell'invio della richiesta effettiva per la risorsa.</span><span class="sxs-lookup"><span data-stu-id="50134-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="50134-192">Il browser può ignorare la richiesta preliminare se vengono soddisfatte le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="50134-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="50134-193">Il metodo di richiesta è GET, HEAD o POST, *e*</span><span class="sxs-lookup"><span data-stu-id="50134-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="50134-194">L'applicazione non imposta le intestazioni di richiesta diversa da Accept, Accept-Language, Content-Language, Content-Type o Last-eventi-ID, *e*</span><span class="sxs-lookup"><span data-stu-id="50134-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="50134-195">L'intestazione Content-Type (se impostato) è uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="50134-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="50134-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="50134-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="50134-197">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="50134-197">multipart/form-data</span></span>
    - <span data-ttu-id="50134-198">text/plain</span><span class="sxs-lookup"><span data-stu-id="50134-198">text/plain</span></span>

<span data-ttu-id="50134-199">Si applica la regola sulle intestazioni di richiesta alle intestazioni che l'applicazione imposta chiamando **setRequestHeader** nel **XMLHttpRequest** oggetto.</span><span class="sxs-lookup"><span data-stu-id="50134-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="50134-200">(Specifica CORS chiama questi "intestazioni di richiesta autore"). La regola non si applica alle intestazioni il *browser* impostare, ad esempio User-Agent, Host o Content-Length.</span><span class="sxs-lookup"><span data-stu-id="50134-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="50134-201">Di seguito è riportato un esempio di una richiesta preliminare:</span><span class="sxs-lookup"><span data-stu-id="50134-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="50134-202">La richiesta preliminare viene utilizzato il metodo OPTIONS HTTP.</span><span class="sxs-lookup"><span data-stu-id="50134-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="50134-203">Include due intestazioni speciali:</span><span class="sxs-lookup"><span data-stu-id="50134-203">It includes two special headers:</span></span>

- <span data-ttu-id="50134-204">Access-Control-Request-Method: Il metodo HTTP che verrà usato per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="50134-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="50134-205">Access-Control-Request-Headers: Un elenco di intestazioni di richiesta che il *applicazione* impostare per la richiesta effettiva.</span><span class="sxs-lookup"><span data-stu-id="50134-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="50134-206">(Anche in questo caso, ciò non includono le intestazioni che imposta il browser.)</span><span class="sxs-lookup"><span data-stu-id="50134-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="50134-207">Ecco un esempio di risposta, presupponendo che il server consente la richiesta:</span><span class="sxs-lookup"><span data-stu-id="50134-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="50134-208">La risposta include un'intestazione Access-Control-Allow-Methods in cui sono elencati i metodi consentiti e, facoltativamente, un'intestazione Access-Control-Consenti-Headers, che elenca le intestazioni consentite.</span><span class="sxs-lookup"><span data-stu-id="50134-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="50134-209">Se la richiesta preliminare ha esito positivo, il browser invia la richiesta effettiva, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="50134-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<span data-ttu-id="50134-210">Strumenti usati comunemente per testare gli endpoint con le richieste preliminari opzioni (ad esempio, [Fiddler](https://www.telerik.com/fiddler) e [Postman](https://www.getpostman.com/)) non inviare le intestazioni di opzioni obbligatorie per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="50134-210">Tools commonly used to test endpoints with preflight OPTIONS requests (for example, [Fiddler](https://www.telerik.com/fiddler) and [Postman](https://www.getpostman.com/)) don't send the required OPTIONS headers by default.</span></span> <span data-ttu-id="50134-211">Verificare che il `Access-Control-Request-Method` e `Access-Control-Request-Headers` intestazioni vengono inviate con la richiesta e intestazioni di opzioni di raggiungere l'app tramite IIS.</span><span class="sxs-lookup"><span data-stu-id="50134-211">Confirm that the `Access-Control-Request-Method` and `Access-Control-Request-Headers` headers are sent with the request and that OPTIONS headers reach the app through IIS.</span></span>

<span data-ttu-id="50134-212">Per configurare IIS per consentire a un'app ASP.NET ricevere e gestire le richieste di opzione, aggiungere la seguente configurazione dell'app *Web. config* del file nei `<system.webServer><handlers>` sezione:</span><span class="sxs-lookup"><span data-stu-id="50134-212">To configure IIS to allow an ASP.NET app to receive and handle OPTION requests, add the following configuration to the app's *web.config* file in the `<system.webServer><handlers>` section:</span></span>

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

<span data-ttu-id="50134-213">La rimozione di `OPTIONSVerbHandler` impedisce a IIS di gestione delle richieste di opzioni.</span><span class="sxs-lookup"><span data-stu-id="50134-213">The removal of `OPTIONSVerbHandler` prevents IIS from handling OPTIONS requests.</span></span> <span data-ttu-id="50134-214">La sostituzione di `ExtensionlessUrlHandler-Integrated-4.0` consente le richieste di opzioni raggiungere l'app perché la registrazione del modulo predefinito consente solo le richieste GET, HEAD, POST e DEBUG con gli URL senza estensione.</span><span class="sxs-lookup"><span data-stu-id="50134-214">The replacement of `ExtensionlessUrlHandler-Integrated-4.0` allows OPTIONS requests to reach the app because the default module registration only allows GET, HEAD, POST, and DEBUG requests with extensionless URLs.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="50134-215">Regole di ambito per [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="50134-215">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="50134-216">È possibile abilitare CORS per ogni azione, per ogni controller o a livello globale per tutti i controller API Web nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="50134-216">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="50134-217">**Per ogni azione**</span><span class="sxs-lookup"><span data-stu-id="50134-217">**Per Action**</span></span>

<span data-ttu-id="50134-218">Per abilitare CORS per una singola azione, impostare il **[EnableCors]** attributo del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="50134-218">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="50134-219">L'esempio seguente Abilita CORS per il `GetItem` solo metodo.</span><span class="sxs-lookup"><span data-stu-id="50134-219">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="50134-220">**Per ogni Controller**</span><span class="sxs-lookup"><span data-stu-id="50134-220">**Per Controller**</span></span>

<span data-ttu-id="50134-221">Se si imposta **[EnableCors]** nella classe controller, si applica a tutte le azioni nel controller.</span><span class="sxs-lookup"><span data-stu-id="50134-221">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="50134-222">Per disabilitare CORS per un'azione, aggiungere il **[DisableCors]** attributo all'azione.</span><span class="sxs-lookup"><span data-stu-id="50134-222">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="50134-223">L'esempio seguente Abilita CORS per qualsiasi metodo ad eccezione `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="50134-223">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="50134-224">**A livello globale**</span><span class="sxs-lookup"><span data-stu-id="50134-224">**Globally**</span></span>

<span data-ttu-id="50134-225">Per abilitare CORS per tutti i controller API Web nell'applicazione, passare un **EnableCorsAttribute** dell'istanza per il **EnableCors** metodo:</span><span class="sxs-lookup"><span data-stu-id="50134-225">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="50134-226">Se si imposta l'attributo più di un ambito, l'ordine di precedenza è:</span><span class="sxs-lookup"><span data-stu-id="50134-226">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="50134-227">Operazione</span><span class="sxs-lookup"><span data-stu-id="50134-227">Action</span></span>
2. <span data-ttu-id="50134-228">Controller</span><span class="sxs-lookup"><span data-stu-id="50134-228">Controller</span></span>
3. <span data-ttu-id="50134-229">Global</span><span class="sxs-lookup"><span data-stu-id="50134-229">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="50134-230">Impostare le origini consentite</span><span class="sxs-lookup"><span data-stu-id="50134-230">Set the allowed origins</span></span>

<span data-ttu-id="50134-231">Il *le entità Origin* parametro delle **[EnableCors]** attributo specifica quali origini sono consentite per accedere alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="50134-231">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="50134-232">Il valore è un elenco delimitato da virgole di origini consentite.</span><span class="sxs-lookup"><span data-stu-id="50134-232">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="50134-233">È anche possibile usare il valore del carattere jolly "\*" per consentire le richieste da tutte le entità Origin.</span><span class="sxs-lookup"><span data-stu-id="50134-233">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="50134-234">Si consideri con attenzione prima di consentire le richieste provenienti da qualsiasi origine.</span><span class="sxs-lookup"><span data-stu-id="50134-234">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="50134-235">Significa che letteralmente qualsiasi sito Web può eseguire chiamate AJAX all'API Web.</span><span class="sxs-lookup"><span data-stu-id="50134-235">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="50134-236">Impostare i metodi HTTP consentiti</span><span class="sxs-lookup"><span data-stu-id="50134-236">Set the allowed HTTP methods</span></span>

<span data-ttu-id="50134-237">Il *metodi* parametro delle **[EnableCors]** attributo specifica quali metodi HTTP consentiti per accedere alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="50134-237">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="50134-238">Per consentire tutti i metodi, usare il valore del carattere jolly "\*".</span><span class="sxs-lookup"><span data-stu-id="50134-238">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="50134-239">Nell'esempio seguente consente solo le richieste GET e POST.</span><span class="sxs-lookup"><span data-stu-id="50134-239">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="50134-240">Impostare le intestazioni di richieste consentite</span><span class="sxs-lookup"><span data-stu-id="50134-240">Set the allowed request headers</span></span>

<span data-ttu-id="50134-241">Questo articolo ha illustrato in precedenza come una richiesta preliminare può includere un'intestazione Access-Control-Request-Headers, elenca le intestazioni HTTP impostate dall'applicazione (la cosiddetta "creare le intestazioni delle richieste").</span><span class="sxs-lookup"><span data-stu-id="50134-241">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="50134-242">Il *intestazioni* parametro delle **[EnableCors]** attributo specifica quali intestazioni di richiesta di autore sono consentite.</span><span class="sxs-lookup"><span data-stu-id="50134-242">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="50134-243">Per consentire tutte le intestazioni, impostare *intestazioni* a "\*".</span><span class="sxs-lookup"><span data-stu-id="50134-243">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="50134-244">A elenco elementi consentiti intestazioni specifiche, impostare *intestazioni* a un elenco delimitato da virgole delle intestazioni consentite:</span><span class="sxs-lookup"><span data-stu-id="50134-244">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="50134-245">Tuttavia, i browser non sono completamente coerenti nel modo in cui siano impostate Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="50134-245">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="50134-246">Ad esempio Chrome include attualmente "origin".</span><span class="sxs-lookup"><span data-stu-id="50134-246">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="50134-247">FireFox non include le intestazioni standard, ad esempio "Accept", anche quando l'applicazione vengono impostati nello script.</span><span class="sxs-lookup"><span data-stu-id="50134-247">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="50134-248">Se si imposta *intestazioni* su un valore qualsiasi diverso da "\*", è necessario includere almeno "accept", "content-type" e "origin", oltre a eventuali intestazioni personalizzate che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="50134-248">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="50134-249">Impostare le intestazioni di risposta consentite</span><span class="sxs-lookup"><span data-stu-id="50134-249">Set the allowed response headers</span></span>

<span data-ttu-id="50134-250">Per impostazione predefinita, il browser non espone tutte le intestazioni di risposta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="50134-250">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="50134-251">Le intestazioni di risposta che sono disponibili per impostazione predefinita sono:</span><span class="sxs-lookup"><span data-stu-id="50134-251">The response headers that are available by default are:</span></span>

- <span data-ttu-id="50134-252">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="50134-252">Cache-Control</span></span>
- <span data-ttu-id="50134-253">Content-Language</span><span class="sxs-lookup"><span data-stu-id="50134-253">Content-Language</span></span>
- <span data-ttu-id="50134-254">Content-Type</span><span class="sxs-lookup"><span data-stu-id="50134-254">Content-Type</span></span>
- <span data-ttu-id="50134-255">Alla scadenza</span><span class="sxs-lookup"><span data-stu-id="50134-255">Expires</span></span>
- <span data-ttu-id="50134-256">Ultima modifica</span><span class="sxs-lookup"><span data-stu-id="50134-256">Last-Modified</span></span>
- <span data-ttu-id="50134-257">Pragma</span><span class="sxs-lookup"><span data-stu-id="50134-257">Pragma</span></span>

<span data-ttu-id="50134-258">La specifica CORS chiama questi [intestazioni di risposta semplice](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="50134-258">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="50134-259">Per rendere altre intestazioni disponibili per l'applicazione, impostare il *exposedHeaders* del parametro **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="50134-259">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="50134-260">Nell'esempio seguente, il controller `Get` metodo imposta un'intestazione personalizzata denominata "X-Custom-Header".</span><span class="sxs-lookup"><span data-stu-id="50134-260">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="50134-261">Per impostazione predefinita, il browser non espone questa intestazione in una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="50134-261">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="50134-262">Per rendere disponibile l'intestazione, includere "X-Custom-Header" nella *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="50134-262">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="50134-263">Passare le credenziali in richieste multiorigine</span><span class="sxs-lookup"><span data-stu-id="50134-263">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="50134-264">Credenziali richiedono una gestione speciale in una richiesta CORS.</span><span class="sxs-lookup"><span data-stu-id="50134-264">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="50134-265">Per impostazione predefinita, il browser invia credenziali con una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="50134-265">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="50134-266">Le credenziali includono i cookie, nonché gli schemi di autenticazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="50134-266">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="50134-267">Per inviare le credenziali con una richiesta multiorigine, il client deve impostare **XMLHttpRequest.withCredentials** su true.</span><span class="sxs-lookup"><span data-stu-id="50134-267">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="50134-268">Usando **XMLHttpRequest** direttamente:</span><span class="sxs-lookup"><span data-stu-id="50134-268">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="50134-269">In jQuery:</span><span class="sxs-lookup"><span data-stu-id="50134-269">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="50134-270">Inoltre, il server deve consentire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="50134-270">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="50134-271">Per consentire le credenziali multiorigine nell'API Web, impostare il **SupportsCredentials** proprietà su true nella **[EnableCors]** attributo:</span><span class="sxs-lookup"><span data-stu-id="50134-271">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="50134-272">Se questa proprietà è true, la risposta HTTP includerà un'intestazione di accesso-controllo-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="50134-272">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="50134-273">Questa intestazione indica al browser che il server consenta le credenziali per una richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="50134-273">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="50134-274">Se il browser invia le credenziali, ma la risposta non include un'intestazione di accesso-controllo-Allow-Credentials valida, il browser non espone la risposta all'applicazione e la richiesta AJAX non riesce.</span><span class="sxs-lookup"><span data-stu-id="50134-274">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="50134-275">Prestare particolare attenzione impostazione **SupportsCredentials** su true, perché significa che un sito Web all'indirizzo di un altro dominio può inviare le credenziali dell'utente ha eseguito l'accesso all'API Web per conto dell'utente, senza che l'utente ne sia consapevole.</span><span class="sxs-lookup"><span data-stu-id="50134-275">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="50134-276">La specifica CORS inoltre stabilito che l'impostazione *le entità Origin* al &quot; \* &quot; non è valido se **SupportsCredentials** è true.</span><span class="sxs-lookup"><span data-stu-id="50134-276">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="50134-277">Provider criteri CORS personalizzati</span><span class="sxs-lookup"><span data-stu-id="50134-277">Custom CORS policy providers</span></span>

<span data-ttu-id="50134-278">Il **[EnableCors]** attributo implementa le **ICorsPolicyProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="50134-278">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="50134-279">È possibile fornire un'implementazione personalizzata creando una classe che deriva da **attributo** e implementa **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="50134-279">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="50134-280">A questo punto è possibile applicare l'attributo in altre posizioni che si inseriscono **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="50134-280">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="50134-281">Ad esempio, un provider criteri CORS personalizzato è stato possibile leggere le impostazioni da un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="50134-281">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="50134-282">Come alternativa all'uso di attributi, è possibile registrare un' **ICorsPolicyProviderFactory** oggetto creato da **ICorsPolicyProvider** oggetti.</span><span class="sxs-lookup"><span data-stu-id="50134-282">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="50134-283">Per impostare il **ICorsPolicyProviderFactory**, chiamare il **SetCorsPolicyProviderFactory** metodo di estensione all'avvio, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="50134-283">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="50134-284">Supporto browser</span><span class="sxs-lookup"><span data-stu-id="50134-284">Browser support</span></span>

<span data-ttu-id="50134-285">Il pacchetto di CORS per l'API Web è una tecnologia lato server.</span><span class="sxs-lookup"><span data-stu-id="50134-285">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="50134-286">Il browser dell'utente deve anche supportare CORS.</span><span class="sxs-lookup"><span data-stu-id="50134-286">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="50134-287">Fortunatamente, includono le versioni correnti di tutti i principali browser [supporto per CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="50134-287">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
