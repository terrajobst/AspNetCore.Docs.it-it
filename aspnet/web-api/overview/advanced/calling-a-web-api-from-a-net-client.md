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
<a name="call-a-web-api-from-a-net-client-c"></a>Chiamare un'API Web da un Client .NET (c#)
====================
da [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[Scaricare il progetto completato](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

In questa esercitazione viene illustrato come chiamare un'API web da un'applicazione .NET, utilizzando [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)

In questa esercitazione, un'app client è scritto che utilizza l'API web seguente:

| Azione | Metodo HTTP | URI relativo |
| --- | --- | --- |
| Ottenere un prodotto in base all'ID | GET | /API/prodotti/*id* |
| Creare un nuovo prodotto | INSERISCI | prodotti/api / |
| Aggiornamento di un prodotto | PUT | /API/prodotti/*id* |
| Eliminare un prodotto | DELETE | /API/prodotti/*id* |

Per informazioni su come implementare questa API con ASP.NET Web API, vedere [la creazione di un'API Web che supporta le operazioni CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Per semplicità, l'applicazione client in questa esercitazione è un'applicazione console di Windows. **HttpClient** è supportata anche per le app di Windows Phone e Windows Store. Per ulteriori informazioni, vedere [scrittura di codice di Client Web API per più piattaforme con le librerie portabili](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Creare l'applicazione Console

In Visual Studio, creare una nuova app console di Windows denominata **HttpClientSample** e incollare il codice seguente:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Il codice precedente è l'applicazione client completa.

`RunAsync`viene eseguito e si blocca fino al completamento. La maggior parte delle **HttpClient** metodi sono async, poiché eseguono i/o rete. Tutte le attività asincrone vengono eseguite all'interno di `RunAsync`. In genere un'app non blocca il thread principale, ma questa app non consente alcuna interazione.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Installare le librerie Client per le API Web

Usare Gestione pacchetti NuGet per installare il pacchetto di librerie Client di API Web.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**. In Package Manager Console (PMC), digitare il comando seguente:

`Install-Package Microsoft.AspNet.WebApi.Client`

Il comando precedente consente di aggiungere i pacchetti NuGet seguenti al progetto:

* Webapi
* Newtonsoft. JSON

Json.NET è un framework JSON ad alte prestazioni comune per .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Aggiungere una classe modello

Esaminare la `Product` classe:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Questa classe corrisponde al modello di dati utilizzato per l'API web. Un'app è possibile utilizzare **HttpClient** per leggere un `Product` istanza da una risposta HTTP. L'app non dispone di scrivere codice la deserializzazione.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Creare e inizializzare HttpClient

Esaminare il metodo statico **HttpClient** proprietà:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** è possibile creare istanze di una volta e riutilizzate per tutta la durata di un'applicazione. Le condizioni seguenti possono causare **SocketException** errori:

* Creazione di un nuovo **HttpClient** istanza per richiesta.
* Server con carico elevato.

Creazione di un nuovo **HttpClient** istanza per richiesta può esaurire il socket disponibili.

Nell'esempio di codice consente di inizializzare il **HttpClient** istanza:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Il codice precedente:

* Imposta l'URI di base per le richieste HTTP. Modificare il numero di porta per la porta utilizzata per l'applicazione server. L'app non funzionerà a meno che non viene utilizzata la porta per l'applicazione server.
* Imposta l'intestazione Accept su "application/json". L'impostazione di questa intestazione indica al server di inviare i dati in formato JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Inviare una richiesta GET per recuperare una risorsa

Il codice seguente viene inviata una richiesta GET per un prodotto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

Il **GetAsync** metodo invia la richiesta HTTP GET. Al termine, il metodo restituisce un **HttpResponseMessage** contenente la risposta HTTP. Se il codice di stato nella risposta è un codice di esito positivo, il corpo della risposta contiene la rappresentazione JSON di un prodotto. Chiamare **ReadAsAsync** per deserializzare il payload JSON per un `Product` istanza. Il **ReadAsAsync** metodo è asincrono, perché il corpo della risposta può essere arbitrariamente grande.

**HttpClient** non genera un'eccezione quando la risposta HTTP contiene un codice di errore. Al contrario, il **IsSuccessStatusCode** proprietà **false** se lo stato è un codice di errore. Se si preferisce gestire codici di errore HTTP come eccezioni, chiamare [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) nell'oggetto della risposta. `EnsureSuccessStatusCode`genera un'eccezione se il codice di stato non è compreso nell'intervallo di 200&ndash;299. Si noti che **HttpClient** possono essere generate eccezioni per altri motivi &mdash; ad esempio, se la richiesta scade.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formattatori di Media Type da deserializzare

Quando **ReadAsAsync** viene chiamata senza parametri, viene utilizzato un set predefinito di *formattatori di media* per leggere il corpo della risposta. I formattatori predefinita supportano JSON, XML e dati con codifica url Form.

Anziché utilizzare i formattatori predefinito, è possibile fornire un elenco di formattatori dal **ReadAsAsync** metodo.  Utilizzando un un elenco di formattatori è utile se si dispone di un formattatore di media type personalizzato:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Per ulteriori informazioni, vedere [formattatori di Media in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Inviare una richiesta POST per creare una risorsa

Il codice seguente invia una richiesta POST che contiene un `Product` istanza nel formato JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

Il **PostAsJsonAsync** metodo:

* Serializza un oggetto in JSON.
* Invia il payload JSON in una richiesta POST.

Se la richiesta ha esito positivo:

* Deve restituire una risposta 201 (creato).
* La risposta deve includere l'URL delle risorse create nell'intestazione Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Inviare una richiesta PUT per aggiornare una risorsa

Il codice seguente viene inviata una richiesta PUT per aggiornare un prodotto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

Il **PutAsJsonAsync** metodo opera come **PostAsJsonAsync**, ad eccezione del fatto che viene inviata una richiesta PUT anziché POST.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>L'invio di una richiesta di eliminazione per eliminare una risorsa

Il codice seguente viene inviata una richiesta di eliminazione per eliminare un prodotto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Ad esempio GET, una richiesta di eliminazione non dispone di un corpo della richiesta. Non è necessario specificare il formato JSON o XML con l'istruzione DELETE.

## <a name="test-the-sample"></a>Testare l'esempio

Per testare l'app client:

1. [Scaricare](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) ed eseguire l'applicazione server. [Istruzioni di download](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample). Verificare il che funzionamento dell'applicazione server. Per exaxmple, `http://localhost:64195/api/products` deve restituire un elenco di prodotti.
2. Impostare l'URI di base per le richieste HTTP. Modificare il numero di porta per la porta utilizzata per l'applicazione server.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Eseguire l'applicazione client. Viene generato l'output seguente:

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
