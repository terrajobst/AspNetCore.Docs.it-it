---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Abilitazione CRUD Operations in ASP.NET Web API 1 | Documenti Microsoft
author: MikeWasson
description: In questa esercitazione viene illustrato come supportare le operazioni CRUD in un servizio HTTP mediante ASP.NET Web API. Versioni del software utilizzate nell'esercitazione Visual Studio 2012 Web punto di accesso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "29153008"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Abilitazione CRUD Operations in ASP.NET Web API 1
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> In questa esercitazione viene illustrato come supportare le operazioni CRUD in un servizio HTTP mediante ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Visual Studio 2012
> - API Web 1 (funziona anche con l'API Web 2)


È l'acronimo di CRUD &quot;Create, Read, Update e Delete,&quot; quali sono le quattro operazioni di database di base. Molti servizi HTTP modello anche le operazioni CRUD tramite REST o le API REST simile.

In questa esercitazione si creerà un'API per gestire un elenco di prodotti di web molto semplice. Ogni prodotto conterrà un nome, prezzo e la categoria (ad esempio &quot;toys&quot; o &quot;hardware&quot;), nonché un ID prodotto.

Espongono le API di prodotti seguenti metodi.

| Operazione | Metodo HTTP | URI relativo |
| --- | --- | --- |
| Ottenere un elenco di tutti i prodotti | GET | prodotti/api / |
| Ottenere un prodotto in base all'ID | GET | /api/products/*id* |
| Ottenere un prodotto per categoria | GET | /api/products?category=*category* |
| Creare un nuovo prodotto | INSERISCI | prodotti/api / |
| Aggiornamento di un prodotto | PUT | /api/products/*id* |
| Eliminare un prodotto | DELETE | /api/products/*id* |

Si noti che alcuni degli URI includono l'ID prodotto nel percorso. Ad esempio, per ottenere il prodotto il cui ID è 28, il client invia una richiesta GET `http://hostname/api/products/28`.

### <a name="resources"></a>Risorse

I prodotti API definisce l'URI per due tipi di risorse:

| Risorsa | URI |
| --- | --- |
| L'elenco di tutti i prodotti. | prodotti/api / |
| Un singolo prodotto. | /api/products/*id* |

### <a name="methods"></a>Metodi

I quattro principali metodi HTTP (GET, PUT, POST e DELETE) possono essere mappati a operazioni CRUD, come indicato di seguito:

- GET recupera la rappresentazione della risorsa a un URI specificato. GET deve non hanno effetti collaterali sul server.
- PUT consente di aggiornare una risorsa a un URI specificato. PUT anche utilizzabile per creare una nuova risorsa in un URI specificato, se il server consente ai client di specificare di nuovo URI. Per questa esercitazione, l'API non supporta la creazione tramite PUT.
- POST crea una nuova risorsa. Il server assegna l'URI per il nuovo oggetto e restituisce l'URI come parte del messaggio di risposta.
- DELETE elimina una risorsa a un URI specificato.

Nota: Il metodo PUT sostituisce l'entità product intero. Ovvero, il client è previsto per l'invio di una rappresentazione completa del prodotto aggiornato. Se si desidera supportare gli aggiornamenti parziali, il metodo PATCH è preferibile. In questa esercitazione non implementa PATCH.

## <a name="create-a-new-web-api-project"></a>Creare un nuovo progetto di API Web

Iniziare eseguendo Visual Studio e selezionare **nuovo progetto** dal **avviare** pagina. O dal **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#** selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto &quot;ProductStore&quot; e fare clic su **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona **API Web** e fare clic su **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Aggiunta di un modello

Un *modello* è un oggetto che rappresenta i dati nell'applicazione. In ASP.NET Web API, è possibile utilizzare oggetti CLR fortemente tipizzati come modelli e vengono automaticamente serializzati in XML o JSON per il client.

Per l'API ProductStore, costituito da dati di prodotti, verrà quindi creata una nuova classe denominata `Product`.

Se Esplora soluzioni non è visibile, fare clic su di **vista** dal menu **Esplora**. In Esplora soluzioni fare doppio clic su di **modelli** cartella. Selezionare il contesto meny, **Aggiungi**, quindi selezionare **classe**. Nome della classe &quot;prodotto&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Aggiungere le seguenti proprietà per il `Product` classe.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Aggiunta di un Repository

È necessario archiviare una raccolta di prodotti. È consigliabile separare la raccolta dall'implementazione del servizio. In questo modo, è possibile modificare l'archivio di backup senza riscrivere la classe del servizio. Questo tipo di progettazione viene chiamato il *repository* modello. Per iniziare, definire un'interfaccia generica per il repository.

In Esplora soluzioni fare doppio clic su di **modelli** cartella. Selezionare **Aggiungi**, quindi selezionare **nuovo elemento**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il nodo di c#. In c#, selezionare **codice**. Nell'elenco dei modelli di codice, selezionare **interfaccia**. Nome dell'interfaccia &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Aggiungere l'implementazione seguente:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

A questo punto aggiungere un'altra classe nella cartella Models, denominato &quot;ProductRepository.&quot; Questa classe implementerà l'interfaccia `IProductRespository`. Aggiungere l'implementazione seguente:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Il repository mantiene l'elenco nella memoria locale. Questo stato è OK per un'esercitazione, ma in un'applicazione reale, è necessario archiviare esternamente, i dati di un database o nell'archiviazione cloud. Il modello di repository rende più semplice modificare l'implementazione in un secondo momento.

## <a name="adding-a-web-api-controller"></a>Aggiunta di un Controller API Web

Ha familiarità con MVC ASP.NET, quindi si ha già familiarità con i controller di. In ASP.NET Web API, un *controller* è una classe che gestisce le richieste HTTP dal client. La creazione guidata nuovo progetto creato due controller quando creato il progetto. Per visualizzarle, espandere la cartella controller in Esplora soluzioni.

- HomeController è un controller MVC ASP.NET tradizionale. È responsabile per servire le pagine HTML per il sito e non è correlato direttamente l'API Web.
- ValuesController è un controller WebAPI di esempio.

Vado Avanti ed eliminare ValuesController, facendovi in Esplora soluzioni e selezionando **eliminare.** Aggiungere ora un nuovo controller, come segue:

In **Esplora**, fare clic sulla cartella controller. Selezionare **Aggiungi** e quindi selezionare **Controller**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

Nel **Aggiungi Controller** procedura guidata, il nome del controller &quot;ProductsController&quot;. Nel **modello** elenco a discesa, seleziona **Controller API vuoto**. Fare quindi clic su **Aggiungi**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Non è necessario inserire il contollers in una cartella denominata controller. Il nome della cartella non è rilevante. è semplicemente un modo pratico per organizzare i file di origine.


Il **Aggiungi Controller** procedura guidata creerà un file denominato ProductsController.cs nella cartella controller. Se questo file non è già aperto, fare doppio clic sul file per aprirlo. Aggiungere il seguente **utilizzando** istruzione:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Aggiungere un campo che contiene un **IProductRepository** istanza.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> La chiamata `new ProductRepository()` nel controller non è l'approccio migliore, perché collega il controller di una particolare implementazione di `IProductRepository`. Per un approccio migliore, vedere [mediante il Resolver di dipendenza di API Web](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Recupero di una risorsa

L'API ProductStore esporrà diversi &quot;leggere&quot; azioni sotto forma di metodi HTTP GET. Ogni azione corrisponderà a un metodo di `ProductsController` classe.

| Operazione | Metodo HTTP | URI relativo |
| --- | --- | --- |
| Ottenere un elenco di tutti i prodotti | GET | prodotti/api / |
| Ottenere un prodotto in base all'ID | GET | /api/products/*id* |
| Ottenere un prodotto per categoria | GET | /api/products?category=*category* |

Per ottenere l'elenco di tutti i prodotti, aggiungere questo metodo per la `ProductsController` classe:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Il nome del metodo inizia con &quot;ottenere&quot;, pertanto per convenzione viene eseguito il mapping alle richieste GET. Inoltre, poiché il metodo non ha parametri, viene eseguito il mapping a un URI che non contiene un *&quot;id&quot;* segmento del percorso.

Per ottenere un prodotto dall'ID, aggiungere questo metodo per la `ProductsController` classe:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Il nome del metodo inizia anche con &quot;ottenere&quot;, ma il metodo ha un parametro denominato *id*. Questo parametro viene eseguito il mapping per il &quot;id&quot; segmento del percorso URI. Il framework di ASP.NET Web API converte automaticamente l'ID per il tipo di dati corretto (**int**) per il parametro.

Il metodo GetProduct genera un'eccezione di tipo **HttpResponseException** se *id* non è valido. Questa eccezione viene convertita dal framework in un errore 404 (non trovato).

Infine, aggiungere un metodo per trovare i prodotti per categoria:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Se l'URI della richiesta è una stringa di query, API Web tenta di associare i parametri di query ai parametri del metodo del controller. Pertanto, un URI nel formato "/ prodotti api? categoria =*categoria*" verrà eseguito il mapping a questo metodo.

## <a name="creating-a-resource"></a>Creazione di una risorsa

Successivamente, verrà aggiunto un metodo per la `ProductsController` classe per creare un nuovo prodotto. Ecco una semplice implementazione del metodo:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Notare due cose su questo metodo:

- Il nome del metodo inizia con &quot;Post... &quot;. Per creare un nuovo prodotto, il client invia una richiesta HTTP POST.
- Il metodo accetta un parametro di tipo Product. Nell'API Web, i parametri con tipi complessi vengono deserializzati dal corpo della richiesta. Pertanto, è probabile che il client invia una rappresentazione serializzata di un oggetto di prodotto, in formato XML o JSON.

Questa implementazione funzionerà, ma non è ancora completo. Idealmente, si sarebbe la risposta HTTP per includere i seguenti:

- **Codice di risposta:** per impostazione predefinita, il framework Web API imposta il codice di stato risposta su 200 (OK). Tuttavia, in base al protocollo HTTP/1.1, quando una richiesta POST comporta la creazione di una risorsa, il server deve rispondere con stato 201 (creato).
- **Percorso:** quando il server crea una risorsa, deve includere l'URI della nuova risorsa nell'intestazione Location della risposta.

ASP.NET Web API semplifica modificare il messaggio di risposta HTTP. Di seguito è riportata l'implementazione migliorata:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Si noti che il tipo restituito del metodo è ora **HttpResponseMessage**. Restituendo un **HttpResponseMessage** anziché un prodotto, possiamo controllare i dettagli del messaggio di risposta HTTP, incluso il codice di stato e l'intestazione di posizione.

Il **CreateResponse** metodo crea un **HttpResponseMessage** e scrive automaticamente una rappresentazione serializzata dell'oggetto prodotto nel corpo fo il messaggio di risposta.

> [!NOTE]
> In questo esempio non convalida il `Product`. Per informazioni sulla convalida del modello, vedere [convalida del modello in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Aggiornamento di una risorsa

Aggiornamento di un prodotto con PUT è semplice:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Il nome del metodo inizia con &quot;Put... &quot;, pertanto l'API Web a esso corrispondente per le richieste PUT. Il metodo accetta due parametri, l'ID prodotto e il prodotto aggiornato. Il *id* parametro da cui proviene il percorso URI e *prodotto* parametro viene deserializzato dal corpo della richiesta. Per impostazione predefinita, il framework di ASP.NET Web API accetta i tipi di parametro semplici dalla route e i tipi complessi dal corpo della richiesta.

## <a name="deleting-a-resource"></a>Eliminazione di una risorsa

Per eliminare una risorsa, definire un metodo di "Eliminare …".

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Se una richiesta di eliminazione ha esito positivo, restituisce lo stato 200 (OK) con un corpo dell'entità che descrive lo stato; lo stato 202 (accettato) se l'eliminazione non è ancora in sospeso; o di stato 204 (nessun contenuto) senza corpo entità. In questo caso, il `DeleteProduct` metodo ha un `void` il tipo restituito, in modo da ASP.NET Web API converte automaticamente questo in stato codice 204 (nessun contenuto).
