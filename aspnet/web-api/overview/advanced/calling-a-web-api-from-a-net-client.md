---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chiamare un'API Web da un Client .NET (c#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 434515692a53c0939652b643b080cea9f2102564
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370918"
---
<a name="call-a-web-api-from-a-net-client-c"></a>Chiamare un'API Web da un Client .NET (c#)
====================
dal [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[Download progetto completato](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Istruzioni per il download](/aspnet/core/tutorials/#how-to-download-a-sample). 

Questa esercitazione illustra come chiamare un'API web da un'applicazione .NET, usando [HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

In questa esercitazione viene scritta un'app client che utilizza l'API web seguente:

| Operazione | Metodo HTTP | URI relativo |
| --- | --- | --- |
| Ottenere un prodotto base all'ID | GET | /api/products/*id* |
| Creare un nuovo prodotto | INSERISCI | prodotti/api / |
| Aggiornare un prodotto | PUT | /api/products/*id* |
| Eliminare un prodotto | DELETE | /api/products/*id* |

Per informazioni su come implementare questa API con l'API Web ASP.NET, vedere [creazione di un'API Web che supporta le operazioni CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Per semplicità, l'applicazione client in questa esercitazione è un'applicazione console di Windows. **HttpClient** è supportata anche per le app di Windows Phone e Windows Store. Per altre informazioni, vedere [scrittura di codice di Client API Web per più piattaforme usando le librerie portabili](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Creare l'applicazione Console

In Visual Studio, creare una nuova app console Windows denominata **HttpClientSample** e incollare il codice seguente:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Il codice precedente è l'app client completa.

`RunAsync` viene eseguito e si blocca fino al completamento. La maggior parte degli **HttpClient** metodi sono asincroni, poiché eseguono i/o rete. Tutte le attività asincrone vengono eseguite all'interno di `RunAsync`. In genere un'app non blocca il thread principale, ma questa app non consente alcuna interazione.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Installare le librerie Client dell'API Web

Usare Gestione pacchetti NuGet per installare il pacchetto di librerie Client API Web.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**. In Package Manager Console (PMC), digitare il comando seguente:

`Install-Package Microsoft.AspNet.WebApi.Client`

Il comando precedente consente di aggiungere i pacchetti NuGet seguenti al progetto:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET è un framework JSON ad alte prestazioni più diffusi per .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Aggiungere una classe modello

Esaminare il `Product` classe:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Questa classe corrisponde al modello di dati usato dall'API web. Un'app può usare **HttpClient** per leggere un `Product` istanza da una risposta HTTP. L'app non deve scrivere alcun codice di deserializzazione.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Creare e inizializzare HttpClient

Esaminare il metodo statico **HttpClient** proprietà:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** è destinato a essere creata un'istanza di una volta e riutilizzate per tutta la durata di un'applicazione. Le condizioni seguenti possono comportare **SocketException** errori:

* Creazione di una nuova **HttpClient** istanza per ogni richiesta.
* Server con un carico pesante.

Creazione di una nuova **HttpClient** istanza per ogni richiesta può esaurire i socket disponibili.

Il codice seguente consente di inizializzare il **HttpClient** istanza:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Il codice precedente:

* Imposta l'URI di base per le richieste HTTP. Modificare il numero di porta per la porta utilizzata per l'app server. L'app non funzionerà a meno che non viene utilizzata la porta per l'app server.
* Imposta l'intestazione Accept su "application/json". L'impostazione di questa intestazione indica al server di inviare i dati in formato JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Inviare una richiesta GET per recuperare una risorsa

Il codice seguente invia una richiesta GET per un prodotto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

Il **GetAsync** metodo invia la richiesta HTTP GET. Al termine, il metodo restituisce un **HttpResponseMessage** che contiene la risposta HTTP. Se il codice di stato nella risposta è un codice di riuscita, il corpo della risposta contiene la rappresentazione JSON di un prodotto. Chiamare **ReadAsAsync** deserializzare il payload JSON per un `Product` istanza. Il **ReadAsAsync** metodo è asincrono, poiché il corpo della risposta può essere arbitrariamente grande.

**HttpClient** non genera un'eccezione quando la risposta HTTP contiene un codice di errore. Al contrario, il **IsSuccessStatusCode** è di proprietà **false** se lo stato è un codice di errore. Se si preferisce gestire i codici di errore HTTP come eccezioni, chiamare [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) nell'oggetto della risposta. `EnsureSuccessStatusCode` genera un'eccezione se il codice di stato non è compreso nell'intervallo di 200&ndash;299. Si noti che **HttpClient** può generare eccezioni per altri motivi &mdash; ad esempio, se la richiesta scade.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formattatori di Media Type da deserializzare

Quando **ReadAsAsync** viene chiamata senza parametri, viene utilizzato un set predefinito di *formattatori di media* per leggere il corpo della risposta. I formattatori predefiniti supportano JSON, XML e i dati codificati negli url di Form.

Invece di usare i formattatori predefiniti, è possibile fornire un elenco di formattatori per la **ReadAsAsync** (metodo).  Usando un elenco di formattatori è utile se si dispone di un formattatore di media type personalizzato:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Per altre informazioni, vedere [formattatori di Media in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Inviare una richiesta POST per creare una risorsa

Il codice seguente invia una richiesta POST contenente un `Product` istanza nel formato JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

Il **PostAsJsonAsync** metodo:

* Serializza un oggetto in JSON.
* Invia il payload JSON in una richiesta POST.

Se la richiesta ha esito positivo:

* Deve restituire una risposta 201 (creato).
* La risposta deve includere l'URL delle risorse create nell'intestazione Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Invia una richiesta PUT per aggiornare una risorsa

Il codice seguente invia una richiesta PUT per aggiornare un prodotto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

Il **PutAsJsonAsync** metodo è paragonabile **PostAsJsonAsync**, ad eccezione del fatto che viene inviata una richiesta PUT anziché POST.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Invia una richiesta DELETE per eliminare una risorsa

Il codice seguente invia una richiesta DELETE per eliminare un prodotto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Ad esempio GET, una richiesta di eliminazione non è un corpo della richiesta. Non è necessario specificare il formato JSON o XML con l'istruzione DELETE.

## <a name="test-the-sample"></a>Testare il codice di esempio

Per testare l'app client:

1. [Scaricare](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) ed eseguire l'app server. [Istruzioni per il download](/aspnet/core/tutorials/#how-to-download-a-sample). Verificare il che funzionamento dell'app server. Per exaxmple, `http://localhost:64195/api/products` deve restituire un elenco di prodotti.
2. Impostare l'URI di base per le richieste HTTP. Modificare il numero di porta per la porta utilizzata per l'app server.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Eseguire l'app client. Viene generato l'output seguente:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
