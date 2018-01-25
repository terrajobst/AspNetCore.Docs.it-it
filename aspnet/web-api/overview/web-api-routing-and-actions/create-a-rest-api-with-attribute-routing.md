---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Creare un'API REST con l'attributo Routing in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: c1d0b3e1644ef7f9ebb4be74c3fdf3df90cf3537
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Creare un'API REST con attributo Routing in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

Web API 2 supporta un nuovo tipo di routing, denominata *attributo routing*. Per una panoramica generale di routing degli attributi, vedere [Routing degli attributi in Web API 2](attribute-routing-in-web-api-2.md). In questa esercitazione si utilizzerà il routing di attributo per creare un'API REST per una raccolta di documentazione. L'API offrirà supporto per le azioni seguenti:

| Operazione | URI di esempio |
| --- | --- |
| Ottenere un elenco di tutti i libri. | /api/books |
| Ottenere un libro di ID. | /api/books/1 |
| Ottenere i dettagli di un libro. | /api/books/1/details |
| Ottenere un elenco di libri per genere. | /api/books/fantasy |
| Ottenere un elenco di libri per data di pubblicazione. | /API/books/date/2013-02-16 /api/books/date/2013/02/16 (modulo alternativo) |
| Ottenere un elenco di libri da un particolare autore. | /api/authors/1/books |

Tutti i metodi sono di sola lettura (richieste HTTP GET).

Per il livello dati, si userà Entity Framework. Record della Rubrica saranno disponibili i seguenti campi:

- Id
- Titolo
- Genere
- Data di pubblicazione
- Prezzo
- Descrizione
- AuthorID (chiave esterna a una tabella di autori)

Per la maggior parte delle richieste, tuttavia, l'API restituirà un sottoinsieme di dati (titolo, autore e genere). Per ottenere il record, il client richieste `/api/books/{id}/details`.

## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

Iniziare eseguendo Visual Studio. Dal **File** dal menu **New** e quindi selezionare **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#**selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **vuoto** modello. In "Aggiungere cartelle e i riferimenti per core", selezionare il **API Web** casella di controllo. Fare clic su **creare progetto**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Verrà creato un progetto di base che è configurato per la funzionalità API Web.

### <a name="domain-models"></a>Modelli di dominio

Successivamente, aggiungere le classi per i modelli di dominio. In Esplora soluzioni, fare clic sulla cartella Models. Selezionare **Aggiungi**, quindi selezionare **classe**. Assegnare alla classe il nome `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Sostituire il codice in Author.cs con gli elementi seguenti:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

A questo punto aggiungere un'altra classe denominata `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Aggiungere un Controller API Web

In questo passaggio verrà aggiunto un controller API Web che usa Entity Framework del livello dati.

Premere CTRL+MAIUSC+B per compilare il progetto. Entity Framework utilizza la reflection per individuare le proprietà dei modelli, pertanto richiede un assembly compilato creare lo schema del database.

In Esplora soluzioni, fare clic sulla cartella controller. Selezionare **Aggiungi**, quindi selezionare **Controller**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

Nel **aggiungere lo scaffolding** finestra di dialogo, seleziona "Web API 2 Controller con azioni di lettura/scrittura, mediante Entity Framework."

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

Nel **Aggiungi Controller** finestra di dialogo per **nome Controller**, immettere &quot;BooksController&quot;. Selezionare il &quot;utilizzare azioni asincrone del controller&quot; casella di controllo. Per **classe modello**selezionare &quot;Book&quot;. (Se non viene visualizzato il `Book` classe elencati nella casella di riepilogo, verificare che il progetto è stato generato.) Fare clic sul pulsante "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Fare clic su **Aggiungi** nel **nuovo contesto dati** finestra di dialogo.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Fare clic su **Aggiungi** nel **Aggiungi Controller** finestra di dialogo. Lo scaffolding consente di aggiungere una classe denominata `BooksController` che definisce il controller API. Aggiunge inoltre una classe denominata `BooksAPIContext` nella cartella Models, che definisce il contesto dei dati per Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Valore di inizializzazione del Database

Dal menu Strumenti, selezionare **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.

Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Questo comando crea una cartella di migrazioni e aggiunge un nuovo file di codice denominato Configuration.cs. Aprire il file e aggiungere il codice seguente per il `Configuration.Seed` metodo.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Questi comandi creano un database locale e richiamano il metodo di inizializzazione per popolare il database.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Aggiungere le classi DTO

Se si esegue ora l'applicazione e inviarla una richiesta GET a /api/books/1, la risposta è simile al seguente. (Aggiunto rientro per migliorare la leggibilità).

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Al contrario, si desidera la richiesta per restituire un subset dei campi. Inoltre, si desidera restituire il nome dell'autore, anziché l'ID autore. A tale scopo, verrà modificata i metodi di controller per restituire un *oggetto di trasferimento dati* (DTO) anziché il modello di Entity Framework. Un DTO è un oggetto che può trasportare solo dati.

In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi** | **nuova cartella**. Denominare la cartella &quot;DTO&quot;. Aggiungere una classe denominata `BookDto` nella cartella DTO, con la definizione seguente:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Aggiungere un'altra classe denominata `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Aggiornare quindi il `BooksController` classe per restituire `BookDto` istanze. Si userà il [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) metodo progetto `Book` istanze per `BookDto` istanze. Di seguito è riportato il codice aggiornato per la classe controller.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> È stato eliminato il `PutBook`, `PostBook`, e `DeleteBook` metodi, perché non sono necessari per questa esercitazione.


Se si esegue l'applicazione e richiedere /api/books/1, il corpo della risposta deve essere simile a questo:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Aggiungere gli attributi delle Route

Successivamente, verrà convertito il controller per l'utilizzo di routing degli attributi. Aggiungere innanzitutto un **RoutePrefix** attributo nel controller. Questo attributo definisce i segmenti URI iniziali per tutti i metodi in questo controller.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Aggiungere quindi **[Route]** attributi per le azioni del controller, come indicato di seguito:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Il modello di route per ogni metodo del controller è il prefisso più la stringa specificata nel **Route** attributo. Per il `GetBook` metodo, il modello di route include la stringa con parametri &quot;{id: int}&quot;, che corrisponde se il segmento URI contiene un valore intero.

| Metodo | Modello di route | URI di esempio |
| --- | --- | --- |
| `GetBooks` | "documentazione/api" | `http://localhost/api/books` |
| `GetBook` | "api/books/{id:int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Ottenere i dettagli della Rubrica

Per ottenere i dettagli di libro, il client invia una richiesta GET al `/api/books/{id}/details`, dove *{id}* è l'ID del libro.

Aggiungere il metodo seguente alla classe `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Se si richiede `/api/books/1/details`, la risposta è simile al seguente:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Ottenere documentazione per genere

Per ottenere un elenco di libri un genere specifico, il client invia una richiesta GET al `/api/books/genre`, dove *genre* è il nome del genere. ad esempio `/get/books/fantasy`.

Aggiungere il seguente metodo alla `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Di seguito viene definita la una route contenente un parametro di {genre} nel modello URI. Si noti che l'API Web è in grado di distinguere questi due URI e li indirizzano a metodi diversi:

`/api/books/1`

`/api/books/fantasy`

Ciò accade perché il `GetBook` metodo include un vincolo che il segmento "id" deve essere un valore integer:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Se si richiede /api/books/fantasy, la risposta simile alla seguente:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Ottenere i libri di autore

Per ottenere un elenco di una documentazione per un determinato autore, il client invia una richiesta GET al `/api/authors/id/books`, dove *id* è l'ID dell'autore.

Aggiungere il seguente metodo alla `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

In questo esempio è interessante poiché &quot;documentazione&quot; è considerato una risorsa figlio di &quot;autori&quot;. Questo modello è piuttosto comune nelle API REST.

Tilde (~) nel modello di route esegue l'override di prefisso della route nel **RoutePrefix** attributo.

## <a name="get-books-by-publication-date"></a>Ottenere documentazione per data di pubblicazione

Per ottenere un elenco di libri per data di pubblicazione, il client invia una richiesta GET al `/api/books/date/yyyy-mm-dd`, dove *aaaa-mm-gg* Data.

Ecco un modo per eseguire questa operazione:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

Il `{pubdate:datetime}` parametro è vincolato in modo che corrisponda un **DateTime** valore. Questo procedimento funziona, ma è molto più permissivo di Microsoft è interessata. Ad esempio, gli URI corrisponderà anche la route:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Non è verificato un errore con consentire gli URI. Tuttavia, è possibile limitare la route in un formato specifico, aggiungere un vincolo di espressione regolare per il modello di route:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Solo le date nel formato &quot;aaaa-mm-gg&quot; corrisponderanno. Si noti che non viene utilizzata l'espressione regolare per convalidare che si è arrivati a una data reale. Che avviene quando l'API Web tenta di convertire il segmento URI in un **DateTime** istanza. Una data non valida, ad esempio "2012-47-99 avrà esito negativo da convertire e il client riceverà un errore 404.

È anche possibile supportare un separatore di barra (`/api/books/date/yyyy/mm/dd`) mediante l'aggiunta di un altro **[Route]** attributo con un'altra espressione regolare.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

È un dettaglio sottile ma importante qui. Il secondo modello di route è un carattere jolly (\*) all'inizio del parametro {pubdate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

In questo modo il motore di routing che {pubdate} deve corrispondere il resto dell'URI. Per impostazione predefinita, un parametro di modello corrisponde a un singolo segmento URI. In questo caso, si vuole {pubdate} estendersi su più segmenti URI:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Codice del controller

Di seguito è riportato il codice completo per la classe BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Riepilogo

Attributo di routing consente che un maggiore controllo e una maggiore flessibilità quando si progetta l'URI per l'API.
