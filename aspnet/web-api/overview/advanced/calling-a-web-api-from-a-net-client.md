---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chiamare un'API Web da un Client .NET (c#) | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8fcc5e7c6bc39f961931589128a7a5863482aa4e
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/07/2017
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="7de0a-102">Chiamare un'API Web da un Client .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="7de0a-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="7de0a-103">da [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7de0a-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="7de0a-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="7de0a-104">Download Completed Project</span></span>](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

<span data-ttu-id="7de0a-105">In questa esercitazione viene illustrato come chiamare un'API web da un'applicazione .NET, utilizzando [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="7de0a-105">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="7de0a-106">In questa esercitazione, un'app client è scritto che utilizza l'API web seguente:</span><span class="sxs-lookup"><span data-stu-id="7de0a-106">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="7de0a-107">Azione</span><span class="sxs-lookup"><span data-stu-id="7de0a-107">Action</span></span> | <span data-ttu-id="7de0a-108">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="7de0a-108">HTTP method</span></span> | <span data-ttu-id="7de0a-109">URI relativo</span><span class="sxs-lookup"><span data-stu-id="7de0a-109">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7de0a-110">Ottenere un prodotto in base all'ID</span><span class="sxs-lookup"><span data-stu-id="7de0a-110">Get a product by ID</span></span> | <span data-ttu-id="7de0a-111">GET</span><span class="sxs-lookup"><span data-stu-id="7de0a-111">GET</span></span> | <span data-ttu-id="7de0a-112">/API/prodotti/*id*</span><span class="sxs-lookup"><span data-stu-id="7de0a-112">/api/products/*id*</span></span> |
| <span data-ttu-id="7de0a-113">Creare un nuovo prodotto</span><span class="sxs-lookup"><span data-stu-id="7de0a-113">Create a new product</span></span> | <span data-ttu-id="7de0a-114">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="7de0a-114">POST</span></span> | <span data-ttu-id="7de0a-115">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="7de0a-115">/api/products</span></span> |
| <span data-ttu-id="7de0a-116">Aggiornamento di un prodotto</span><span class="sxs-lookup"><span data-stu-id="7de0a-116">Update a product</span></span> | <span data-ttu-id="7de0a-117">PUT</span><span class="sxs-lookup"><span data-stu-id="7de0a-117">PUT</span></span> | <span data-ttu-id="7de0a-118">/API/prodotti/*id*</span><span class="sxs-lookup"><span data-stu-id="7de0a-118">/api/products/*id*</span></span> |
| <span data-ttu-id="7de0a-119">Eliminare un prodotto</span><span class="sxs-lookup"><span data-stu-id="7de0a-119">Delete a product</span></span> | <span data-ttu-id="7de0a-120">DELETE</span><span class="sxs-lookup"><span data-stu-id="7de0a-120">DELETE</span></span> | <span data-ttu-id="7de0a-121">/API/prodotti/*id*</span><span class="sxs-lookup"><span data-stu-id="7de0a-121">/api/products/*id*</span></span> |

<span data-ttu-id="7de0a-122">Per informazioni su come implementare questa API con ASP.NET Web API, vedere [la creazione di un'API Web che supporta le operazioni CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="7de0a-122">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="7de0a-123">Per semplicità, l'applicazione client in questa esercitazione è un'applicazione console di Windows.</span><span class="sxs-lookup"><span data-stu-id="7de0a-123">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="7de0a-124">**HttpClient** è supportata anche per le app di Windows Phone e Windows Store.</span><span class="sxs-lookup"><span data-stu-id="7de0a-124">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="7de0a-125">Per ulteriori informazioni, vedere [scrittura di codice di Client Web API per più piattaforme con le librerie portabili](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="7de0a-125">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="7de0a-126">Creare l'applicazione Console</span><span class="sxs-lookup"><span data-stu-id="7de0a-126">Create the Console Application</span></span>

<span data-ttu-id="7de0a-127">In Visual Studio, creare una nuova app console di Windows denominata **HttpClientSample** e incollare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7de0a-127">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="7de0a-128">Il codice precedente è l'applicazione client completa.</span><span class="sxs-lookup"><span data-stu-id="7de0a-128">The preceding code is the complete client app.</span></span>

<span data-ttu-id="7de0a-129">`RunAsync`viene eseguito e si blocca fino al completamento.</span><span class="sxs-lookup"><span data-stu-id="7de0a-129">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="7de0a-130">La maggior parte delle **HttpClient** metodi sono async, poiché eseguono i/o rete.</span><span class="sxs-lookup"><span data-stu-id="7de0a-130">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="7de0a-131">Tutte le attività asincrone vengono eseguite all'interno di `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="7de0a-131">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="7de0a-132">In genere un'app non blocca il thread principale, ma questa app non consente alcuna interazione.</span><span class="sxs-lookup"><span data-stu-id="7de0a-132">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="7de0a-133">Installare le librerie Client per le API Web</span><span class="sxs-lookup"><span data-stu-id="7de0a-133">Install the Web API Client Libraries</span></span>

<span data-ttu-id="7de0a-134">Usare Gestione pacchetti NuGet per installare il pacchetto di librerie Client di API Web.</span><span class="sxs-lookup"><span data-stu-id="7de0a-134">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="7de0a-135">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="7de0a-135">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="7de0a-136">In Package Manager Console (PMC), digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="7de0a-136">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="7de0a-137">Il comando precedente consente di aggiungere i pacchetti NuGet seguenti al progetto:</span><span class="sxs-lookup"><span data-stu-id="7de0a-137">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="7de0a-138">Webapi</span><span class="sxs-lookup"><span data-stu-id="7de0a-138">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="7de0a-139">Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="7de0a-139">Newtonsoft.Json</span></span>

<span data-ttu-id="7de0a-140">Json.NET è un framework JSON ad alte prestazioni comune per .NET.</span><span class="sxs-lookup"><span data-stu-id="7de0a-140">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="7de0a-141">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="7de0a-141">Add a Model Class</span></span>

<span data-ttu-id="7de0a-142">Esaminare la `Product` classe:</span><span class="sxs-lookup"><span data-stu-id="7de0a-142">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="7de0a-143">Questa classe corrisponde al modello di dati utilizzato per l'API web.</span><span class="sxs-lookup"><span data-stu-id="7de0a-143">This class matches the data model used by the web API.</span></span> <span data-ttu-id="7de0a-144">Un'app è possibile utilizzare **HttpClient** per leggere un `Product` istanza da una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="7de0a-144">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="7de0a-145">L'app non dispone di scrivere codice la deserializzazione.</span><span class="sxs-lookup"><span data-stu-id="7de0a-145">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="7de0a-146">Creare e inizializzare HttpClient</span><span class="sxs-lookup"><span data-stu-id="7de0a-146">Create and Initialize HttpClient</span></span>

<span data-ttu-id="7de0a-147">Esaminare il metodo statico **HttpClient** proprietà:</span><span class="sxs-lookup"><span data-stu-id="7de0a-147">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="7de0a-148">**HttpClient** è possibile creare istanze di una volta e riutilizzate per tutta la durata di un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7de0a-148">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="7de0a-149">Le condizioni seguenti possono causare **SocketException** errori:</span><span class="sxs-lookup"><span data-stu-id="7de0a-149">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="7de0a-150">Creazione di un nuovo **HttpClient** istanza per richiesta.</span><span class="sxs-lookup"><span data-stu-id="7de0a-150">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="7de0a-151">Server con carico elevato.</span><span class="sxs-lookup"><span data-stu-id="7de0a-151">Server under heavy load.</span></span>

<span data-ttu-id="7de0a-152">Creazione di un nuovo **HttpClient** istanza per richiesta può esaurire il socket disponibili.</span><span class="sxs-lookup"><span data-stu-id="7de0a-152">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="7de0a-153">Nell'esempio di codice consente di inizializzare il **HttpClient** istanza:</span><span class="sxs-lookup"><span data-stu-id="7de0a-153">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="7de0a-154">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="7de0a-154">The preceding code:</span></span>

* <span data-ttu-id="7de0a-155">Imposta l'URI di base per le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="7de0a-155">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="7de0a-156">Modificare il numero di porta per la porta utilizzata per l'applicazione server.</span><span class="sxs-lookup"><span data-stu-id="7de0a-156">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="7de0a-157">L'app non funzionerà a meno che non viene utilizzata la porta per l'applicazione server.</span><span class="sxs-lookup"><span data-stu-id="7de0a-157">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="7de0a-158">Imposta l'intestazione Accept su "application/json".</span><span class="sxs-lookup"><span data-stu-id="7de0a-158">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="7de0a-159">L'impostazione di questa intestazione indica al server di inviare i dati in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="7de0a-159">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="7de0a-160">Inviare una richiesta GET per recuperare una risorsa</span><span class="sxs-lookup"><span data-stu-id="7de0a-160">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="7de0a-161">Il codice seguente viene inviata una richiesta GET per un prodotto:</span><span class="sxs-lookup"><span data-stu-id="7de0a-161">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="7de0a-162">Il **GetAsync** metodo invia la richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="7de0a-162">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="7de0a-163">Al termine, il metodo restituisce un **HttpResponseMessage** contenente la risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="7de0a-163">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="7de0a-164">Se il codice di stato nella risposta è un codice di esito positivo, il corpo della risposta contiene la rappresentazione JSON di un prodotto.</span><span class="sxs-lookup"><span data-stu-id="7de0a-164">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="7de0a-165">Chiamare **ReadAsAsync** per deserializzare il payload JSON per un `Product` istanza.</span><span class="sxs-lookup"><span data-stu-id="7de0a-165">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="7de0a-166">Il **ReadAsAsync** metodo è asincrono, perché il corpo della risposta può essere arbitrariamente grande.</span><span class="sxs-lookup"><span data-stu-id="7de0a-166">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="7de0a-167">**HttpClient** non genera un'eccezione quando la risposta HTTP contiene un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="7de0a-167">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="7de0a-168">Al contrario, il **IsSuccessStatusCode** proprietà **false** se lo stato è un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="7de0a-168">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="7de0a-169">Se si preferisce gestire codici di errore HTTP come eccezioni, chiamare [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) nell'oggetto della risposta.</span><span class="sxs-lookup"><span data-stu-id="7de0a-169">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="7de0a-170">`EnsureSuccessStatusCode`genera un'eccezione se il codice di stato non è compreso nell'intervallo di 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="7de0a-170">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="7de0a-171">Si noti che **HttpClient** possono essere generate eccezioni per altri motivi &mdash; ad esempio, se la richiesta scade.</span><span class="sxs-lookup"><span data-stu-id="7de0a-171">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="7de0a-172">Formattatori di Media Type da deserializzare</span><span class="sxs-lookup"><span data-stu-id="7de0a-172">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="7de0a-173">Quando **ReadAsAsync** viene chiamata senza parametri, viene utilizzato un set predefinito di *formattatori di media* per leggere il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="7de0a-173">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="7de0a-174">I formattatori predefinita supportano JSON, XML e dati con codifica url Form.</span><span class="sxs-lookup"><span data-stu-id="7de0a-174">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="7de0a-175">Anziché utilizzare i formattatori predefinito, è possibile fornire un elenco di formattatori dal **ReadAsAsync** metodo.</span><span class="sxs-lookup"><span data-stu-id="7de0a-175">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="7de0a-176">Utilizzando un un elenco di formattatori è utile se si dispone di un formattatore di media type personalizzato:</span><span class="sxs-lookup"><span data-stu-id="7de0a-176">Using a a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="7de0a-177">Per ulteriori informazioni, vedere [formattatori di Media in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="7de0a-177">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="7de0a-178">Inviare una richiesta POST per creare una risorsa</span><span class="sxs-lookup"><span data-stu-id="7de0a-178">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="7de0a-179">Il codice seguente invia una richiesta POST che contiene un `Product` istanza nel formato JSON:</span><span class="sxs-lookup"><span data-stu-id="7de0a-179">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="7de0a-180">Il **PostAsJsonAsync** metodo:</span><span class="sxs-lookup"><span data-stu-id="7de0a-180">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="7de0a-181">Serializza un oggetto in JSON.</span><span class="sxs-lookup"><span data-stu-id="7de0a-181">Serializes an object to JSON.</span></span>
* <span data-ttu-id="7de0a-182">Invia il payload JSON in una richiesta POST.</span><span class="sxs-lookup"><span data-stu-id="7de0a-182">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="7de0a-183">Se la richiesta ha esito positivo:</span><span class="sxs-lookup"><span data-stu-id="7de0a-183">If the request succeeds:</span></span>

* <span data-ttu-id="7de0a-184">Deve restituire una risposta 201 (creato).</span><span class="sxs-lookup"><span data-stu-id="7de0a-184">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="7de0a-185">La risposta deve includere l'URL delle risorse create nell'intestazione Location.</span><span class="sxs-lookup"><span data-stu-id="7de0a-185">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="7de0a-186">Inviare una richiesta PUT per aggiornare una risorsa</span><span class="sxs-lookup"><span data-stu-id="7de0a-186">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="7de0a-187">Il codice seguente viene inviata una richiesta PUT per aggiornare un prodotto:</span><span class="sxs-lookup"><span data-stu-id="7de0a-187">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="7de0a-188">Il **PutAsJsonAsync** metodo opera come **PostAsJsonAsync**, ad eccezione del fatto che viene inviata una richiesta PUT anziché POST.</span><span class="sxs-lookup"><span data-stu-id="7de0a-188">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="7de0a-189">L'invio di una richiesta di eliminazione per eliminare una risorsa</span><span class="sxs-lookup"><span data-stu-id="7de0a-189">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="7de0a-190">Il codice seguente viene inviata una richiesta di eliminazione per eliminare un prodotto:</span><span class="sxs-lookup"><span data-stu-id="7de0a-190">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="7de0a-191">Ad esempio GET, una richiesta di eliminazione non dispone di un corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="7de0a-191">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="7de0a-192">Non è necessario specificare il formato JSON o XML con l'istruzione DELETE.</span><span class="sxs-lookup"><span data-stu-id="7de0a-192">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="7de0a-193">Testare l'esempio</span><span class="sxs-lookup"><span data-stu-id="7de0a-193">Test the sample</span></span>

<span data-ttu-id="7de0a-194">Per testare l'app client:</span><span class="sxs-lookup"><span data-stu-id="7de0a-194">To test the client app:</span></span>

1. <span data-ttu-id="7de0a-195">[Scaricare](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) ed eseguire l'applicazione server.</span><span class="sxs-lookup"><span data-stu-id="7de0a-195">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) and run the server app.</span></span> <span data-ttu-id="7de0a-196">[Istruzioni di download](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="7de0a-196">[Download instructions](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="7de0a-197">Verificare il che funzionamento dell'applicazione server.</span><span class="sxs-lookup"><span data-stu-id="7de0a-197">Verify the server app is working.</span></span> <span data-ttu-id="7de0a-198">Per exaxmple, `http://localhost:64195/api/products` deve restituire un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="7de0a-198">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="7de0a-199">Impostare l'URI di base per le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="7de0a-199">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="7de0a-200">Modificare il numero di porta per la porta utilizzata per l'applicazione server.</span><span class="sxs-lookup"><span data-stu-id="7de0a-200">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="7de0a-201">Eseguire l'applicazione client.</span><span class="sxs-lookup"><span data-stu-id="7de0a-201">Run the client app.</span></span> <span data-ttu-id="7de0a-202">Viene generato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="7de0a-202">The following output is produced:</span></span>

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
