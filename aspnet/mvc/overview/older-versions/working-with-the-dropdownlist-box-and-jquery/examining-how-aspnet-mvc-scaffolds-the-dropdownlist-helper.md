---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: "Esaminare la modalità ASP.NET MVC scaffolds del DropDownList Helper | Documenti Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 737773ab424b3ec3b6139b8c238a60ca23de2e69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Esaminare la modalità ASP.NET MVC scaffolds del DropDownList Helper
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

In **Esplora**, fare doppio clic su di *controller* cartella e quindi selezionare **Aggiungi Controller**. Denominare il controller **StoreManagerController**. Impostare le opzioni per il **Aggiungi Controller** finestra di dialogo come illustrato nell'immagine seguente.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Modificare il *StoreManager\Index.cshtml* consente di visualizzare e rimuovere `AlbumArtUrl`. Rimozione di `AlbumArtUrl` semplifichino la presentazione. Il codice completo è illustrato di seguito.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Aprire il *Controllers\StoreManagerController.cs* file e individuare il `Index` metodo. Aggiungere il `OrderBy` clausola in modo gli album verranno ordinati in base al prezzo. Il codice completo è illustrato di seguito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Ordinamento in base al prezzo rende più semplice testare le modifiche al database. Quando si esegue il test della modifica e creare i metodi, è possibile usare un prezzo minimo, pertanto i dati salvati verranno visualizzati per primi.

Aprire il *StoreManager\Edit.cshtml* file. Aggiungere la riga seguente immediatamente dopo il tag della legenda.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Il codice seguente mostra il contesto di questa modifica:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

Il `AlbumId` è necessario apportare modifiche al record di un album.

Premere CTRL+F5 per eseguire l'applicazione. Selezionare questa opzione per il **Admin** collegare, quindi selezionare il **Crea nuovo** collegamento per creare un nuovo album. Verificare che le informazioni è state salvate. Modifica di un album e verificare le modifiche apportate vengono rese persistenti.

### <a name="the-album-schema"></a>Lo Schema di Album

Il `StoreManager` controller creato tramite il meccanismo di scaffolding MVC accedervi CRUD (Create, Read, Update, Delete) gli album nel database dell'archivio musica. Di seguito è riportato lo schema per informazioni:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

Il `Albums` tabella non memorizza il genere album e la descrizione, archivia una chiave esterna per il `Genres` tabella. Il `Genres` tabella contiene in genere nome e descrizione. Analogamente, il `Albums` tabella non contiene il nome di artisti album, ma una chiave esterna per il `Artists` tabella. Il `Artists` tabella contiene il nome dell'artista. Se si esaminano i dati di `Albums` tabella, è possibile visualizzare ogni riga contiene una chiave esterna per il `Genres` tabella e una chiave esterna per il `Artists` tabella. Nell'immagine seguente mostra alcuni dati della tabella dal `Albums` tabella.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Il Tag HTML Select

Il codice HTML `<select>` elemento (creato dal codice HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) viene utilizzato per visualizzare un elenco completo dei valori (ad esempio l'elenco di generi). Per i moduli di modifica, quando il valore corrente è noto, l'elenco di selezione può visualizzare il valore corrente. È stato illustrato questo precedentemente quando si imposta il valore selezionato **comici**. Elenco di selezione è ideale per la visualizzazione di categoria o dati di chiave esterna. Il `<select>` elemento per la chiave esterna Genre viene visualizzato l'elenco di nomi di genere possibili, ma quando si salva il modulo la proprietà Genre viene aggiornata con Genre valore della chiave esterna, non il nome visualizzato genere. Nell'immagine seguente, il genere selezionato è **Disco** e artista **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Esame di MVC ASP.NET scaffolding codice

Aprire il *Controllers\StoreManagerController.cs* file e individuare il `HTTP GET Create` metodo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

Il `Create` metodo aggiunge due [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) oggetti per il `ViewBag`, uno per contenere le informazioni di genre e uno per contenere le informazioni artista. Il [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) overload del costruttore utilizzato in precedenza accetta tre argomenti:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *elementi*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenenti gli elementi nell'elenco. Nell'esempio precedente, l'elenco di generi restituito dal `db.Genres`.
2. *dataValueField*: il nome della proprietà di **IEnumerable** elenco che contiene il valore della chiave. Nell'esempio precedente, `GenreId` e `ArtistId`.
3. *dataTextField*: il nome della proprietà di **IEnumerable** elenco che contiene le informazioni da visualizzare. In entrambi artisti e la tabella genre il `name` campo viene usato.

Aprire il *Views\StoreManager\Create.cshtml* file ed esaminare il `Html.DropDownList` markup di supporto per il campo genre.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

La prima riga indica che la visualizzazione di creazione accetta un `Album` modello. Nel `Create` metodo illustrato sopra, è stato passato alcun modello, in modo che la vista ottenga un **null** `Album` modello. A questo punto viene creato un nuovo album pertanto non `Album` dati.

Il [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload illustrato in precedenza prende il nome del campo da associare al modello. Viene inoltre utilizzato il nome specificato per cercare un **ViewBag** oggetto che contiene un [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) oggetto. Utilizza questo overload, verrà richiesto di nome di **ViewBag SelectList** oggetto `GenreId`. Il secondo parametro (`String.Empty`) è il testo da visualizzare quando è selezionato alcun elemento. Questo è esattamente quello desiderato quando si crea un nuovo album. Se è possibile rimuovere il secondo parametro e utilizzato nel codice seguente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Elenco di selezione predefinito verrà impostato per il primo elemento o Rock in questo esempio.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Esaminare il `HTTP POST Create` metodo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Questo overload del metodo di `Create` metodo accetta un `album` oggetto, creato dal sistema di associazione del modello MVC ASP.NET dai valori di modulo registrato. Quando si invia un nuovo album, se lo stato del modello è valido e non sono presenti errori di database, il database viene aggiunto il nuovo album. Nella figura seguente illustra la creazione di un nuovo album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

È possibile utilizzare il [strumento fiddler](http://www.fiddler2.com/fiddler2/) per esaminare i valori del form inseriti tale associazione di modelli di MVC ASP.NET utilizza per creare l'oggetto album.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>La creazione di ViewBag SelectList refactoring

Entrambi i `Edit` metodi e `HTTP POST Create` metodo sono identico codice per impostare il **SelectList** nel **ViewBag**. In spirito [secca](http://en.wikipedia.org/wiki/Don't_repeat_yourself), abbiamo questo codice di eseguire il refactoring. Ci accerteremo utilizzo di questo codice sottoposto a refactoring in un secondo momento.

Creare un nuovo metodo per aggiungere un genere e artista **SelectList** per il **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Sostituire le due righe impostando il `ViewBag` in ogni il `Create` e `Edit` metodi con una chiamata al `SetGenreArtistViewBag` metodo. Il codice completo è illustrato di seguito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Creare un nuovo album e modificare un album per verificare le modifiche.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Passare in modo esplicito il SelectList DropDownList

Le viste di creare e modificare create per l'utilizzo di scaffolding di ASP.NET MVC seguente **DropDownList** overload:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

Il `DropDownList` di seguito è riportato il markup per la visualizzazione di creazione.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Perché il `ViewBag` proprietà per il `SelectList` denominato `GenreId`, **DropDownList** helper utilizzerà il `GenreId` **SelectList** nel **ViewBag** . Nell'esempio seguente **DropDownList** overload, la `SelectList` viene passato in modo esplicito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Aprire il *Views\StoreManager\Edit.cshtml* file e modificare il **DropDownList** chiamata per passare in modo esplicito il **SelectList**, utilizzando l'overload del precedente. Questa operazione per la categoria di genere. Il codice completo è illustrato di seguito:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Eseguire l'applicazione e fare clic su di **Admin** collegare, quindi passare a un album Jazz e selezionare il **modifica** collegamento.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Anziché mostrare Jazz come il genere attualmente selezionato, viene visualizzato Rock. Quando l'argomento di stringa (la proprietà da associare) e **SelectList** oggetto con lo stesso nome, il valore selezionato non viene utilizzato. Quando è presente alcun valore selezionato fornito, browser predefinito per il primo elemento di **SelectList**(ovvero **Rock** nell'esempio precedente). Si tratta di una limitazione nota del **DropDownList** helper.

Aprire il *Controllers\StoreManagerController.cs* file e modificare il **SelectList** oggetto nomi `Genres` e `Artists`. Il codice completo è illustrato di seguito:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

I nomi di generi e artisti sono nomi migliori per le categorie, in quanto contengono solo l'ID di ogni categoria. Abbiamo fatto in precedenza il refactoring dei pagamenti. Anziché modificare il **ViewBag** in quattro metodi, le modifiche sono state isolate per il `SetGenreArtistViewBag` metodo.

Modifica il **DropDownList** chiamare nella creazione e modifica delle viste per usare la nuova **SelectList** nomi. Di seguito è riportato il nuovo tag per la visualizzazione di modifica:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Visualizzazione di creazione richiede una stringa vuota per evitare che venga visualizzato il primo elemento di SelectList.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Creare un nuovo album e modificare un album per verificare le modifiche. Testare il codice di modifica selezionando un album con un genere diverso da Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Con un modello di visualizzazione del DropDownList Helper

Creare una nuova classe nella cartella ViewModel denominata `AlbumSelectListViewModel`. Sostituire il codice di `AlbumSelectListViewModel` classe con le operazioni seguenti:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

Il `AlbumSelectListViewModel` costruttore accetta un album, un elenco degli artisti e generi e crea un oggetto contenente album e un `SelectList` per generi e artisti.

Compilare il progetto pertanto `AlbumSelectListViewModel` è disponibile quando si crea una vista nel passaggio successivo.

Aggiungere un `EditVM` metodo il `StoreManagerController`. Il codice completo è illustrato di seguito.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Fare clic destro `AlbumSelectListViewModel`selezionare **risolvere**, quindi **utilizzando MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

In alternativa, è possibile aggiungere la seguente istruzione using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Fare clic destro `EditVM` e selezionare **Aggiungi visualizzazione**. Utilizzare le opzioni riportate di seguito.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Selezionare **Aggiungi**, quindi sostituire il contenuto del *Views\StoreManager\EditVM.cshtml* file con le operazioni seguenti:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

Il `EditVM` markup è molto simile all'originale `Edit` markup con le eccezioni seguenti.

- Le proprietà del modello di `Edit` visualizzati sono nel formato `model.property`(ad esempio, `model.Title` ). Le proprietà del modello di `EditVm` visualizzati sono nel formato `model.Album.property`(ad esempio, `model.Album.Title`). Ciò accade perché il `EditVM` visualizzazione viene passato un contenitore un `Album`, non un `Album` come nel `Edit` visualizzazione.
- Il **DropDownList** secondo parametro viene fornito dal modello di visualizzazione, non il **ViewBag**.
- Il **BeginForm** helper nel `EditVM` vista in modo esplicito il postback al `Edit` metodo di azione. Registrazione di eseguire il backup per il `Edit` azione, non è necessario scrivere un `HTTP POST EditVM` azione e riutilizzare il `HTTP POST` `Edit` azione.

Eseguire l'applicazione e modificare un album. Modificare l'URL da utilizzare `EditVM`. Modificare un campo e premere il **salvare** per verificare il funzionamento del codice.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Scelta dell'approccio è necessario utilizzare?

Tutti e tre gli approcci illustrati sono acceptible. Molti sviluppatori preferiscono explictily passare il `SelectList` per il `DropDownList` utilizzando il `ViewBag`. Questo approccio presenta il vantaggio di offrendo la possibilità di utilizzare un nome più appropriato per la raccolta. Un'avvertenza è è possibile assegnare il nome di `ViewBag SelectList` lo stesso nome della proprietà del modello dell'oggetto.

Alcuni sviluppatori preferiscono l'approccio ViewModel. Altri prendere in considerazione il più dettagliato markup e codice HTML generato del ViewModel approccio basato su uno svantaggio.

In questa sezione aver acquisito tre approcci per l'utilizzo di **DropDownList** con i dati di categoria. Nella sezione successiva, vi mostreremo come aggiungere una nuova categoria.

>[!div class="step-by-step"]
[Precedente](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Successivo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
