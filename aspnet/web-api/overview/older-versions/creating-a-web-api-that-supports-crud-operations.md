---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Abilitare le operazioni CRUD nell'API Web ASP.NET 1 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP tramite l'API Web ASP.NET. Versioni del software utilizzate nell'esercitazione Visual Studio 2012 Web punto di accesso...
ms.author: riande
ms.date: 01/28/2012
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: ba061b26b8527e447f25f6046057542a54f989a8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826315"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Abilitare le operazioni CRUD nell'API Web ASP.NET 1
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Questa esercitazione illustra come supportare le operazioni CRUD in un servizio HTTP tramite l'API Web ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Visual Studio 2012
> - API Web 1 (funziona anche con l'API Web 2)


CRUD è l'acronimo &quot;Create, Read, Update e Delete,&quot; quali sono le quattro operazioni di base dei database. Molti servizi HTTP anche modellare le operazioni CRUD tramite REST o l'API simili a REST.

In questa esercitazione si compilerà un'API per gestire un elenco di prodotti web molto semplice. Ogni prodotto conterrà un nome, prezzo e la categoria (ad esempio &quot;toys&quot; oppure &quot;hardware&quot;), oltre a un ID prodotto.

I prodotti API esporrà i metodi in seguito.

| Operazione | Metodo HTTP | URI relativo |
| --- | --- | --- |
| Ottenere un elenco di tutti i prodotti | GET | prodotti/api / |
| Ottenere un prodotto base all'ID | GET | /api/products/*id* |
| Ottenere un prodotto per categoria | GET | /api/products?category=*category* |
| Creare un nuovo prodotto | INSERISCI | prodotti/api / |
| Aggiornare un prodotto | PUT | /api/products/*id* |
| Eliminare un prodotto | DELETE | /api/products/*id* |

Si noti che alcuni degli URI includono l'ID prodotto nel percorso. Ad esempio, per ottenere il prodotto il cui ID è 28, il client invia una richiesta GET `http://hostname/api/products/28`.

### <a name="resources"></a>Risorse

I prodotti API definisce gli URI per due tipi di risorse:

| Risorsa | URI |
| --- | --- |
| L'elenco di tutti i prodotti. | prodotti/api / |
| Un singolo prodotto. | /api/products/*id* |

### <a name="methods"></a>Metodi

I quattro principali metodi HTTP (GET, PUT, POST e DELETE) possono essere mappati a operazioni CRUD, come indicato di seguito:

- GET recupera la rappresentazione della risorsa a un URI specificato. GET deve non hanno effetti collaterali sul server.
- PUT consente di aggiornare una risorsa a un URI specificato. PUT è anche utilizzabile per creare una nuova risorsa a un URI specificato, se il server consente ai client di specificare nuovi URI. Per questa esercitazione, l'API non supporterà la creazione tramite PUT.
- POST crea una nuova risorsa. Il server assegna l'URI per il nuovo oggetto e restituisce questo URI come parte del messaggio di risposta.
- DELETE elimina una risorsa a un URI specificato.

Nota: Il metodo PUT sostituisce l'entità product intero. Vale a dire, il client è previsto per l'invio di una rappresentazione completa del prodotto aggiornato. Se si desidera supportare gli aggiornamenti parziali, il metodo PATCH è preferito. Questa esercitazione può neimplementuje metodu PATCH.

## <a name="create-a-new-web-api-project"></a>Creare un nuovo progetto API Web

Iniziare eseguendo Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina. E viceversa, il **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto &quot;ProductStore&quot; e fare clic su **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona **API Web** e fare clic su **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Aggiunta di un modello

Un *modello* è un oggetto che rappresenta i dati nell'applicazione. Nell'API Web ASP.NET, è possibile usare oggetti CLR fortemente tipizzati come modelli e si viene automaticamente serializzate in XML o JSON per il client.

Per l'API ProductStore, i dati costituita da prodotti, verrà quindi creata una nuova classe denominata `Product`.

Se Esplora soluzioni non è già visibile, scegliere il **View** menu e selezionare **Esplora soluzioni**. In Esplora soluzioni fare doppio clic il **modelli** cartella. Meny il contesto, selezionare **Add**, quindi selezionare **classe**. Denominare la classe &quot;prodotto&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Aggiungere le seguenti proprietà per il `Product` classe.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Aggiunta di un Repository

È necessario archiviare un insieme di prodotti. È un'ottima idea separare la raccolta dall'implementazione del servizio. In questo modo, è possibile modificare l'archivio di backup senza riscrivere la classe del servizio. Questo tipo di progettazione viene chiamato il *repository* pattern. Iniziare definendo un'interfaccia generica per il repository.

In Esplora soluzioni fare doppio clic il **modelli** cartella. Selezionare **Add**, quindi selezionare **nuovo elemento**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il nodo c#. In c#, selezionare **codice**. Nell'elenco dei modelli di codice, selezionare **interfaccia**. Denominare l'interfaccia &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Aggiungere l'implementazione seguente:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Aggiungere ora un'altra classe nella cartella Models, denominato &quot;ProductRepository.&quot; Questa classe implementerà l'interfaccia `IProductRespository`. Aggiungere l'implementazione seguente:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Il repository mantiene l'elenco in memoria locale. Questo è accettabile per un'esercitazione, ma in un'applicazione reale, è necessario archiviare esternamente, i dati di un database o nell'archiviazione cloud. Lo schema repository renderà più facile modificare l'implementazione in un secondo momento.

## <a name="adding-a-web-api-controller"></a>Aggiunta di un Controller API Web

Se l'utente abbia familiarità con ASP.NET MVC, quindi si ha familiarità con i controller. Nell'API Web ASP.NET, un *controller* è una classe che gestisce le richieste HTTP dal client. La procedura guidata nuovo progetto creato due controller durante la creazione del progetto. Per visualizzarle, espandere la cartella controller in Esplora soluzioni.

- HomeController è un controller MVC ASP.NET tradizionale. È responsabile per la gestione delle pagine HTML per il sito e non è correlato direttamente l'API Web.
- ValuesController è un controller API Web di esempio.

Andare avanti ed eliminare ValuesController, facendo clic sul file in Esplora soluzioni e selezionando **eliminare.** Aggiungere ora un nuovo controller, come indicato di seguito:

Nelle **Esplora soluzioni**, fare clic sulla cartella Controllers. Selezionare **Add** e quindi selezionare **Controller**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

Nel **Aggiungi Controller** procedura guidata, nome del controller &quot;ProductsController&quot;. Nel **modello** elenco a discesa, seleziona **Controller API vuoto**. Fare quindi clic su **Aggiungi**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Non è necessario inserire il contollers in una cartella denominata controller. Il nome della cartella non è rilevante. è semplicemente un modo pratico per organizzare i file di origine.


Il **Aggiungi Controller** procedura guidata creerà un file denominato ProductsController.cs nella cartella controller. Se non è già aperto questo file, fare doppio clic sul file per aprirlo. Aggiungere il codice seguente **usando** istruzione:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Aggiungere un campo che contiene un' **IProductRepository** istanza.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> La chiamata `new ProductRepository()` nel controller non è la migliore struttura, in quanto si unisce il controller a una particolare implementazione di `IProductRepository`. Per un approccio migliore, vedere [utilizzando il Resolver di dipendenza di API Web](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Recupero di una risorsa

L'API ProductStore esporrà diversi &quot;leggere&quot; azioni sotto forma di metodi HTTP GET. Ogni azione corrisponderà a un metodo nel `ProductsController` classe.

| Operazione | Metodo HTTP | URI relativo |
| --- | --- | --- |
| Ottenere un elenco di tutti i prodotti | GET | prodotti/api / |
| Ottenere un prodotto base all'ID | GET | /api/products/*id* |
| Ottenere un prodotto per categoria | GET | /api/products?category=*category* |

Per ottenere l'elenco di tutti i prodotti, aggiungere questo metodo per il `ProductsController` classe:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Il nome del metodo inizia con &quot;ottenere&quot;, in modo che per convenzione viene eseguito il mapping alle richieste GET. Inoltre, poiché il metodo non ha parametri, esegue il mapping a un URI che non contiene un' *&quot;id&quot;* segmento nel percorso.

Per ottenere un prodotto dall'ID, aggiungere questo metodo per il `ProductsController` classe:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Questo nome di metodo inoltre inizia con &quot;ottenere&quot;, ma il metodo ha un parametro denominato *id*. Questo parametro viene eseguito il mapping per il &quot;id&quot; segmento del percorso URI. Il framework API Web ASP.NET converte automaticamente l'ID per il tipo di dati corretto (**int**) per il parametro.

Il metodo GetProduct genera un'eccezione di tipo **HttpResponseException** se *id* non è valido. Questa eccezione verrà convertita dal framework in un errore 404 (non trovato).

Infine, aggiungere un metodo per trovare i prodotti per categoria:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Se l'URI della richiesta è una stringa di query, API Web cerca la corrispondenza con i parametri di query ai parametri nel metodo del controller. Pertanto, un URI nel formato "api/prodotti? categoria =*categoria*" verrà eseguito il mapping a questo metodo.

## <a name="creating-a-resource"></a>Creazione di una risorsa

Successivamente, verrà aggiunto un metodo per il `ProductsController` classe per creare un nuovo prodotto. Ecco una semplice implementazione del metodo:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Tenere presente due aspetti di questo metodo:

- Il nome del metodo inizia con &quot;Post... &quot;. Per creare un nuovo prodotto, il client invia una richiesta HTTP POST.
- Il metodo accetta un parametro di tipo Product. Nell'API Web, i parametri con tipi complessi vengono deserializzati dal corpo della richiesta. È quindi necessario che il client invia una rappresentazione serializzata di un oggetto di prodotto, in formato XML o JSON.

Questa implementazione funzionerà, ma non è ancora completo. In teoria, vorremmo la risposta HTTP da includere quanto segue:

- **Codice di risposta:** per impostazione predefinita, il framework API Web imposta il codice di stato risposta 200 (OK). Tuttavia, secondo il protocollo HTTP/1.1, quando una richiesta POST comporta la creazione di una risorsa, il server deve rispondere con stato 201 (creato).
- **Percorso:** quando il server crea una risorsa, deve includere l'URI della nuova risorsa nell'intestazione Location della risposta.

API Web ASP.NET semplifica modificare il messaggio di risposta HTTP. Ecco l'implementazione migliorata:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Si noti che il tipo restituito del metodo è ora **HttpResponseMessage**. Tramite la restituzione di un **HttpResponseMessage** invece di un prodotto, possiamo controllare i dettagli del messaggio di risposta HTTP, tra cui il codice di stato e l'intestazione Location.

Il **CreateResponse** metodo crea un' **HttpResponseMessage** e automaticamente scrive una rappresentazione serializzata dell'oggetto prodotto nel corpo fo il messaggio di risposta.

> [!NOTE]
> In questo esempio non convalida il `Product`. Per informazioni sulla convalida del modello, vedere [convalida del modello in API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Aggiornamento di una risorsa

L'aggiornamento di un prodotto di PUT è semplice:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Il nome del metodo inizia con &quot;inserito... &quot;, in modo che l'API Web corrisponde alle richieste PUT. Il metodo accetta due parametri, l'ID prodotto e il prodotto aggiornato. Il *id* parametro viene estratto dal percorso URI e il *prodotto* parametro viene deserializzato dal corpo della richiesta. Per impostazione predefinita, il framework API Web ASP.NET accetta i tipi di parametri semplici dalla route e i tipi complessi dal corpo della richiesta.

## <a name="deleting-a-resource"></a>L'eliminazione di una risorsa

Per eliminare una risorsa memorizzata in una, definire un metodo di "Eliminazione in corso".

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Se una richiesta di eliminazione ha esito positivo, con un corpo di entità che descrive lo stato; può restituire lo stato 200 (OK) lo stato 202 (accettato) se l'eliminazione è ancora in sospeso; o di stato 204 (nessun contenuto) senza corpo entità. In questo caso, il `DeleteProduct` metodo presenta un `void` tipo restituito, in modo che l'API Web ASP.NET converte automaticamente in stato codice 204 (nessun contenuto).
