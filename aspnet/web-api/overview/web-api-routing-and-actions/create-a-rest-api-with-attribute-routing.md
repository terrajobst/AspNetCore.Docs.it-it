---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Creare un'API REST con routing mediante attributi n API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: da3ca8f89f823fcb2c4ab74af6ddf4f61d4e663a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832371"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Creare un'API REST con Routing degli attributi nell'API Web ASP.NET 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

API Web 2 supporta un nuovo tipo di routing, chiamato *routing con attributi*. Per una panoramica generale di routing con attributi, vedere [Routing con attributi nell'API Web 2](attribute-routing-in-web-api-2.md). In questa esercitazione si userà il routing con attributi per creare un'API REST per una raccolta di documentazione. L'API supporterà le azioni seguenti:

| Operazione | URI di esempio |
| --- | --- |
| Ottenere un elenco di tutti i libri. | / api/documentazione |
| Ottenere un libro di ID. | /api/books/1 |
| Ottenere i dettagli di un libro. | /API/books/1/Details |
| Ottenere un elenco di libri in base al genere. | /api/books/fantasy |
| Ottenere un elenco di libri per data di pubblicazione. | /API/books/date/2013-02-16 /api/books/date/2013/02/16 (formato alternativo) |
| Ottenere un elenco di libri di uno specifico autore. | /API/authors/1/books |

Tutti i metodi sono di sola lettura (richieste HTTP GET).

Per il livello di dati, si userà Entity Framework. I record dei libri avrà i campi seguenti:

- Id
- Titolo
- Genre
- Data di pubblicazione
- Prezzo
- Descrizione
- AuthorID (chiave esterna per una tabella Authors)

Per la maggior parte delle richieste, tuttavia, l'API restituirà un subset dei dati (titolo, autore e il genere). Per ottenere il record completo, il client richieste `/api/books/{id}/details`.

## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

Iniziare eseguendo Visual Studio. Dal **File** dal menu **New** e quindi selezionare **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **vuota** modello. In "Aggiungere cartelle e riferimenti per la funzionalità di base", selezionare la **API Web** casella di controllo. Fare clic su **Crea progetto**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Verrà creato un progetto bozza configurato per la funzionalità dell'API Web.

### <a name="domain-models"></a>Modelli di dominio

Successivamente, aggiungere le classi per i modelli di dominio. In Esplora soluzioni fare clic sulla cartella Models. Selezionare **Add**, quindi selezionare **classe**. Assegnare alla classe il nome `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Sostituire il codice in Author.cs con gli elementi seguenti:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

A questo punto aggiungere un'altra classe denominata `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Aggiungere un Controller API Web

In questo passaggio si aggiungerà un controller API Web che usa Entity Framework come livello dati.

Premere CTRL+MAIUSC+B per compilare il progetto. Entity Framework Usa la reflection per individuare le proprietà dei modelli, pertanto è necessario un assembly compilato creare lo schema del database.

In Esplora soluzioni fare clic sulla cartella Controllers. Selezionare **Add**, quindi selezionare **Controller**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

Nel **Add Scaffold** finestra di dialogo, seleziona "Web API 2 Controller con azioni di lettura/scrittura, che usa Entity Framework."

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

Nel **Aggiungi Controller** finestra di dialogo per **nome del Controller**, immettere &quot;BooksController&quot;. Selezionare il &quot;usare le azioni del controller asincrono&quot; casella di controllo. Per la **classe modello**, selezionare &quot;libro&quot;. (Se non viene visualizzato il `Book` classe elencati nell'elenco a discesa, assicurarsi che è stato compilato il progetto.) Fare clic sul pulsante "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Fare clic su **Add** nel **nuovo contesto dati** finestra di dialogo.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Fare clic su **Add** nel **Aggiungi Controller** finestra di dialogo. Lo scaffolding consente di aggiungere una classe denominata `BooksController` che definisce il controller API. Aggiunge anche una classe denominata `BooksAPIContext` nella cartella Models, che definisce il contesto dei dati per Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Seeding del Database

Dal menu Strumenti, selezionare **Gestione pacchetti libreria**, quindi selezionare **Console di gestione pacchetti**.

Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Questo comando crea una cartella Migrations e aggiunge un nuovo file di codice Configuration.cs denominato. Aprire il file e aggiungere il codice seguente per il `Configuration.Seed` (metodo).

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Questi comandi creano un database locale e richiamano il metodo di inizializzazione per popolare il database.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Aggiungere le classi DTO

Se si esegue l'applicazione ora e inviarla una richiesta GET a /api/books/1, la risposta sarà simile al seguente. (Aggiunto rientro per una migliore leggibilità).

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Invece, voglio che questa richiesta per restituire un subset dei campi. Inoltre, voglio che venga restituito il nome dell'autore, anziché l'ID autore. A tale scopo, questo punto, modificheremo i metodi del controller per restituire un *oggetto di trasferimento dati* (DTO) anziché il modello di EF. Un oggetto DTO è un oggetto che può trasportare solo i dati.

In Esplora soluzioni fare clic sul progetto e selezionare **Add** | **nuova cartella**. Denominare la cartella &quot;DTO&quot;. Aggiungere una classe denominata `BookDto` nella cartella oggetti DTO, con la definizione seguente:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Aggiungere un'altra classe denominata `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

A questo punto, aggiornare il `BooksController` classe per restituire `BookDto` istanze. Si userà il [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) metodo al progetto `Book` alle istanze `BookDto` istanze. Ecco il codice aggiornato per la classe controller.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Dopo aver eliminato il `PutBook`, `PostBook`, e `DeleteBook` metodi, perché non sono necessari per questa esercitazione.


Ora se si esegue l'applicazione e richiedere /api/books/1, il corpo della risposta dovrebbe essere simile al seguente:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Aggiungere gli attributi di Route

Successivamente, verrà convertito il controller per usare il routing con attributi. Aggiungere prima di tutto una **RoutePrefix** attributo al controller. Questo attributo definisce i segmenti URI iniziali per tutti i metodi nel controller.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Aggiungere quindi **[Route]** attributi per le azioni del controller, come indicato di seguito:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Il modello di route per ogni metodo del controller è il prefisso più la stringa specificata nel **Route** attributo. Per il `GetBook` metodo, il modello di route include la stringa con parametri &quot;{id: int}&quot;, che corrisponde a se il segmento URI contiene un valore intero.

| Metodo | Modello di route | URI di esempio |
| --- | --- | --- |
| `GetBooks` | "api/documentazione" | `http://localhost/api/books` |
| `GetBook` | "api/books/{id:int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Ottenere i dettagli della Rubrica

Per ottenere i dettagli della Rubrica, il client invia una richiesta GET a `/api/books/{id}/details`, dove *{id}* è l'ID del libro.

Aggiungere il metodo seguente alla classe `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Se si richiedono `/api/books/1/details`, la risposta sarà simile al seguente:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Ottieni documentazione in base al genere

Per ottenere un elenco di libri in genere specifico, il client invia una richiesta GET a `/api/books/genre`, dove *genre* è il nome del genere. ad esempio `/api/books/fantasy`.

Aggiungere il metodo seguente alla `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Di seguito viene definita una route che contiene un parametro di {genre} nel modello URI. Si noti che l'API Web è in grado di distinguere questi due URI e indirizzarle a diversi metodi:

`/api/books/1`

`/api/books/fantasy`

Infatti il `GetBook` metodo include un vincolo che il segmento "id" deve essere un valore integer:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Se si richiede /api/books/fantasy, la risposta sarà simile al seguente:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Ottieni documentazione dall'autore

Per ottenere un elenco di una documentazione per un determinato autore, il client invia una richiesta GET a `/api/authors/id/books`, dove *id* è l'ID dell'autore.

Aggiungere il metodo seguente alla `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

In questo esempio è interessante perché &quot;libri&quot; è considerato una risorsa figlio di &quot;autori&quot;. Questo modello è piuttosto comune per le API RESTful.

La tilde (~) nel modello di route esegue l'override di prefisso della route nel **RoutePrefix** attributo.

## <a name="get-books-by-publication-date"></a>Ottieni documentazione dalla data di pubblicazione

Per ottenere un elenco di libri per data di pubblicazione, il client invia una richiesta GET a `/api/books/date/yyyy-mm-dd`, dove *aaaa-mm-gg* è la data.

Ecco un modo per eseguire questa operazione:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

Il `{pubdate:datetime}` parametro è vincolato in modo che corrisponda un **DateTime** valore. Questo procedimento funziona, ma è effettivamente più permissivo ci piacerebbe. Ad esempio, questi URI corrisponderà anche la route:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Non è niente di sbagliato consentire gli URI. Tuttavia, è possibile limitare la route in un formato specifico mediante l'aggiunta di un vincolo di espressione regolare per il modello di route:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Attualmente solo le date nel formato &quot;aaaa-mm-gg&quot; corrisponderanno. Si noti che non utilizziamo l'espressione regolare per convalidare che abbiamo una data reale. Quando si tenta di convertire il segmento URI in API Web, che viene gestito un **DateTime** istanza. Una data non valida, ad esempio ' 2012-47-99 avrà esito negativo da convertire e il client otterrà un errore 404.

È anche possibile supportare un separatore di barra (`/api/books/date/yyyy/mm/dd`) tramite l'aggiunta di un altro **[Route]** attributo con un'altra espressione regolare.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Sono un sottile ma importante dettaglio di seguito. Il secondo modello di route contiene un carattere jolly (\*) all'inizio del parametro {pubdate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

In questo modo il motore di routing che {pubdate} deve corrispondere il resto dell'URI. Per impostazione predefinita, un parametro di modello corrisponde a un singolo segmento URI. In questo caso, si desidera {pubdate} estendersi su più segmenti URI:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Codice del controller

Ecco il codice completo per la classe BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Riepilogo

Routing con attributi offre maggiore controllo e una maggiore flessibilità quando si progettano gli URI per l'API.
