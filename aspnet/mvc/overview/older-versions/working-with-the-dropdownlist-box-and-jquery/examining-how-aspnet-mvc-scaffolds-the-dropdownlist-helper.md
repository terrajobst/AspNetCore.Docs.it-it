---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Esaminare la modalità di scaffolding del DropDownList Helper ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 8e407d5426ed93388b3c6f7bdb125be5356c002c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837130"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Esaminare la modalità di scaffolding del DropDownList Helper ASP.NET MVC
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

Nella **Esplora soluzioni**, fare doppio clic il *controller* cartella e quindi selezionare **Aggiungi Controller**. Denominare il controller **StoreManagerController**. Impostare le opzioni per la **Aggiungi Controller** finestra di dialogo come illustrato nell'immagine seguente.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Modificare il *StoreManager\Index.cshtml* consente di visualizzare e rimuovere `AlbumArtUrl`. Rimozione di `AlbumArtUrl` semplifichino la presentazione. Il codice completo è illustrato di seguito.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Aprire il *Controllers\StoreManagerController.cs* del file e trovare il `Index` (metodo). Aggiungere il `OrderBy` clausola in modo che gli album verranno ordinati in base al prezzo. Il codice completo è illustrato di seguito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

L'ordinamento in base al prezzo renderà più facile testare le modifiche al database. Quando si sta testando la modifica e creazione di metodi, è possibile usare un prezzo ridotto in modo che i dati salvati verranno visualizzati per primi.

Aprire il *StoreManager\Edit.cshtml* file. Aggiungere la riga seguente subito dopo il tag della legenda.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Il codice seguente mostra il contesto di questa modifica:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

Il `AlbumId` è necessario apportare modifiche al record di un album.

Premere CTRL+F5 per eseguire l'applicazione. Selezionare questa opzione per la **Admin** collegare, quindi selezionare la **Crea nuovo** collegamento per creare un nuovo album. Verificare che le informazioni di album è state salvate. Modifica di un album e verificare le modifiche apportate vengono mantenute.

### <a name="the-album-schema"></a>Lo Schema di Album

Il `StoreManager` controller creato dal meccanismo di scaffolding di MVC consente l'accesso CRUD (Create, Read, Update, Delete) a album nel database di music store. Di seguito è riportato lo schema per le informazioni di album:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

Il `Albums` tabella non vengono archiviati il genere di album e la descrizione, archivia una chiave esterna per il `Genres` tabella. Il `Genres` tabella contiene il genere nome e descrizione. Analogamente, il `Albums` la tabella non contiene il nome e gli artisti album invece una chiave esterna per il `Artists` tabella. Il `Artists` tabella contiene il nome dell'artista. Se si esamina i dati nella `Albums` tabella, è possibile visualizzare ogni riga contiene una chiave esterna per il `Genres` tabella e una chiave esterna per il `Artists` tabella. L'immagine seguente mostra alcuni dati della tabella dal `Albums` tabella.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Il Tag di selezione HTML

Il codice HTML `<select>` elemento (creato dal codice HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) consente di visualizzare un elenco completo di valori (ad esempio l'elenco dei generi). Per la modifica di moduli, quando si conosce il valore corrente, l'elenco di selezione può visualizzare il valore corrente. Abbiamo visto questo in precedenza quando si imposta il valore selezionato **commedie**. Elenco di selezione è ideale per la visualizzazione dei dati di chiave esterna o categoria. Il `<select>` (elemento) per la chiave esterna Genre viene visualizzato l'elenco di nomi di sottogeneri possibili, ma quando si salva il form proprietà Genre viene aggiornata con genere valore della chiave esterna, non il nome visualizzato genre. Nell'immagine seguente, il genere selezionato viene **Disco** ed è l'artista **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Analisi di MVC ASP.NET codice sottoposto a scaffolding

Aprire il *Controllers\StoreManagerController.cs* del file e trovare il `HTTP GET Create` (metodo).

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

Il `Create` metodo aggiunge due [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) oggetti per il `ViewBag`, uno per contenere le informazioni al genere e uno per contenere le informazioni di artista. Il [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) overload del costruttore utilizzata in precedenza accetta tre argomenti:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *gli elementi*: un' [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenente gli elementi nell'elenco. Nell'esempio precedente, l'elenco dei generi restituito da `db.Genres`.
2. *dataValueField*: il nome della proprietà nel **IEnumerable** elenco che contiene il valore della chiave. Nell'esempio precedente, `GenreId` e `ArtistId`.
3. *dataTextField*: il nome della proprietà nel **IEnumerable** elenco che contiene le informazioni da visualizzare. Tabella genre, sia agli artisti il `name` campo viene usato.

Aprire il *Views\StoreManager\Create.cshtml* del file ed esaminare il `Html.DropDownList` markup dell'helper per il campo genre.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

La prima riga indica che la visualizzazione di creazione accetta un `Album` modello. Nel `Create` metodo illustrato in precedenza, è stato passato alcun modello, in modo che la vista ottenga un **null** `Album` modello. A questo punto verrà creato un nuovo album pertanto non ne esistono `Album` i relativi dati.

Il [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload illustrato in precedenza accetta il nome del campo da associare al modello. Questo nome viene inoltre utilizzato per cercare un **ViewBag** oggetto contenente un [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) oggetto. Utilizza questo overload, viene richiesto di nome il **ViewBag SelectList** oggetto `GenreId`. Il secondo parametro (`String.Empty`) è il testo da visualizzare quando è selezionato alcun elemento. Questo è esattamente ciò che vogliamo quando si crea un nuovo album. Se è rimosso il secondo parametro e utilizzato nel codice seguente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Elenco di selezione sarebbe per impostazione predefinita il primo elemento o Rock nel nostro esempio.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Esaminando il `HTTP POST Create` (metodo).

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Questo overload del metodo di `Create` metodo accetta un `album` oggetto, creato tramite il sistema di associazione di modelli ASP.NET MVC dai valori di modulo registrato. Quando si invia un nuovo album, se lo stato del modello è valido e non siano presenti errori di database, il nuovo album viene aggiunto il database. L'immagine seguente illustra la creazione di un nuovo album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

È possibile usare la [strumento fiddler](http://www.fiddler2.com/fiddler2/) per esaminare i valori di modulo viene utilizzato tale associazione di modelli ASP.NET MVC per creare l'oggetto di album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>La creazione di ViewBag SelectList di refactoring

Entrambi i `Edit` metodi e la `HTTP POST Create` metodo avere codice identico per configurare il **SelectList** nel **ViewBag**. Nello spirito dei [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), abbiamo questo codice di eseguire il refactoring. Ci assicureremo uso di questo codice sottoposto a refactoring in un secondo momento.

Creare un nuovo metodo per aggiungere un genere e artista **SelectList** per il **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Sostituire le due righe impostando il `ViewBag` in ognuna delle `Create` e `Edit` metodi con una chiamata al `SetGenreArtistViewBag` (metodo). Il codice completo è illustrato di seguito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Creare un nuovo album e modificare un album per verificare che le modifiche funzionino.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Passando esplicitamente il SelectList a DropDownList

Le visualizzazioni di creazione e modifica creato mediante l'utilizzo di scaffolding di ASP.NET MVC seguente **DropDownList** overload:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

Il `DropDownList` markup per la visualizzazione di creazione è illustrato di seguito.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Poiché il `ViewBag` proprietà per il `SelectList` denominato `GenreId`, il **DropDownList** helper userà il `GenreId` **SelectList** nel **ViewBag** . Nell'esempio seguente **DropDownList** eseguire l'overload, il `SelectList` viene passato in modo esplicito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Aprire il *Views\StoreManager\Edit.cshtml* di file e modificare le **DropDownList** chiamata passare in modo esplicito il **SelectList**, usando l'overload precedente. Eseguire questa operazione per la categoria al genere. Il codice completo è illustrato di seguito:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Eseguire l'applicazione e fare clic sui **Admin** collegare, quindi passare a un album Jazz e selezionare il **modificare** collegamento.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Anziché mostrare Jazz come il genere attualmente selezionato, viene visualizzato Rock. Quando l'argomento della stringa, la proprietà da associare e il **SelectList** oggetto hanno lo stesso nome, il valore selezionato non viene utilizzato. Quando non è specificato alcun valore selezionato, i browser per impostazione predefinita al primo elemento nel **SelectList**(ovvero **Rock** nell'esempio precedente). Si tratta di una limitazione nota del **DropDownList** helper.

Aprire il *Controllers\StoreManagerController.cs* file e modificare le **SelectList** nomi dell'oggetto `Genres` e `Artists`. Il codice completo è illustrato di seguito:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

I nomi generi e gli artisti sono nomi migliori per le categorie, che contengono solo l'ID di ogni categoria. Il refactoring che è stato fatto in precedenza dato buoni risultati. Anziché modificare il **ViewBag** nei quattro metodi, le modifiche sono state isolata e prevede la `SetGenreArtistViewBag` (metodo).

Modifica il **DropDownList** chiamare nel creare e modificare le viste per utilizzare le nuove **SelectList** nomi. Di seguito è riportato il nuovo tag per la visualizzazione di modifica:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Visualizzazione di creazione richiede una stringa vuota per evitare che il primo elemento di SelectList venga visualizzato.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Creare un nuovo album e modificare un album per verificare che le modifiche funzionino. Testare il codice di modifica selezionando un album con un genere diversi da Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Con un modello di visualizzazione del DropDownList Helper

Creare una nuova classe nella cartella ViewModel denominata `AlbumSelectListViewModel`. Sostituire il codice nel `AlbumSelectListViewModel` classe con quanto segue:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

Il `AlbumSelectListViewModel` costruttore accetta un album, un elenco degli artisti e generi e crea un oggetto che contiene il album e un `SelectList` per generi e gli artisti.

Compilare il progetto in modo che il `AlbumSelectListViewModel` è disponibile quando si crea una vista nel passaggio successivo.

Aggiungere un `EditVM` metodo di `StoreManagerController`. Il codice completo è illustrato di seguito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Fare clic destro `AlbumSelectListViewModel`, selezionare **risolvere**, quindi **usando MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

In alternativa, è possibile aggiungere la seguente istruzione using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Fare clic destro `EditVM` e selezionare **Aggiungi visualizzazione**. Usare le opzioni riportate di seguito.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Selezionare **Add**, quindi sostituire il contenuto delle *Views\StoreManager\EditVM.cshtml* file con il codice seguente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

Il `EditVM` markup è molto simile all'istanza originale `Edit` markup con le eccezioni seguenti.

- Le proprietà del modello i `Edit` visualizzazione hanno la forma `model.property`(ad esempio, `model.Title` ). Le proprietà del modello i `EditVm` visualizzazione hanno la forma `model.Album.property`(ad esempio, `model.Album.Title`). Infatti il `EditVM` view viene passata a un contenitore un `Album`, non un `Album` come mostrato nel `Edit` visualizzazione.
- Il **DropDownList** secondo parametro viene fornito dal modello di visualizzazione, non il **ViewBag**.
- Il **BeginForm** helper nel `EditVM` vista postback in modo esplicito al `Edit` metodo di azione. Mediante la registrazione di eseguire il backup per il `Edit` azione, non è necessario scrivere un' `HTTP POST EditVM` azione e può riusare la `HTTP POST` `Edit` azione.

Eseguire l'applicazione e modificare un album. Modificare l'URL da utilizzare `EditVM`. Modificare un campo e premere il **salvare** pulsante per verificare il funzionamento del codice.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>L'approccio è opportuno utilizzare?

Tutti i tre approcci illustrati sono possono essere. Molti sviluppatori preferiscono explictily pass il `SelectList` per il `DropDownList` usando il `ViewBag`. Questo approccio offre l'ulteriore vantaggio di contemporaneamente la flessibilità dell'uso di un nome più appropriato per la raccolta. Un'avvertenza è è possibile assegnare un nome di `ViewBag SelectList` lo stesso nome di proprietà del modello dell'oggetto.

Alcuni sviluppatori preferiscono l'approccio ViewModel. Altri prendere in considerazione il markup più dettagliato e HTML dell'approccio ViewModel generato uno svantaggio.

In questa sezione è emerso tre approcci per l'utilizzo di **DropDownList** con i dati delle categorie. Nella sezione successiva, ecco come aggiungere una nuova categoria.

> [!div class="step-by-step"]
> [Precedente](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Successivo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
