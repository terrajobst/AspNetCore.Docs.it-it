---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Utilizzo del DropDownList Helper con ASP.NET MVC | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 21373deeded801c5cea9e89f6dac0f3542a55ca5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Utilizzo del DropDownList Helper con ASP.NET MVC
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

In questa esercitazione verrà fornite le nozioni di base dell'utilizzo di [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper e [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) helper in un'applicazione Web ASP.NET MVC. È possibile utilizzare Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio per eseguire l'esercitazione. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:

- [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)

Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). In questa esercitazione si presuppone di aver completato il [Intro to ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) esercitazione o[ASP.NET MVC negozio](../mvc-music-store/mvc-music-store-part-1.md) esercitazione o si ha familiarità con lo sviluppo di ASP.NET MVC. In questa esercitazione inizia con un progetto modificato dal [ASP.NET MVC negozio](../mvc-music-store/mvc-music-store-part-1.md) esercitazione. È possibile scaricare il progetto di avvio con il seguente collegamento [scaricare la versione c#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Un progetto di Visual Web Developer nell'esercitazione completato il codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Cosa Compilerai

Si creeranno i metodi di azione e le viste che utilizzano il [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) helper per selezionare una categoria. Si utilizzerà inoltre **jQuery** per aggiungere una finestra di dialogo categoria insert che può essere usato quando è necessaria una nuova categoria (ad esempio artista o genere). Di seguito viene riportata una schermata di visualizzazione di creazione che illustra i collegamenti per aggiungere un nuovo genere e aggiungere un nuovo artista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Si apprenderà competenze

Ecco cosa si apprenderà:

- Come utilizzare il [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) helper per selezionare i dati della categoria.
- Come aggiungere un **jQuery** finestra di dialogo per aggiungere nuove categorie.

### <a name="getting-started"></a>Introduzione

Per iniziare, scaricare il progetto di avvio con il seguente collegamento [scaricare](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). In Esplora risorse, fare clic di *DDL\_Starter.zip* file e selezionare proprietà. Nel **DDL\_Starter.zip proprietà** nella finestra di dialogo Seleziona Annulla blocco.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Fare clic con il pulsante destro DDL\_Starter.zip file e selezionare **Estrai tutto** per decomprimere il file. Aprire il *StartMusicStore.sln* file con Visual Web Developer 2010 Express ("Visual Web Developer" o "VWD" forma abbreviata) o Visual Studio 2010.

Premere CTRL + F5 per eseguire l'applicazione e fare clic su di **Test** collegamento.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Selezionare il **selezionare la categoria di film (semplice)** collegamento. Viene visualizzato un elenco di film tipo selezionare, con comici il valore selezionato.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Fare clic con il browser e selezionare Visualizza origine. Viene visualizzato il codice HTML della pagina. Il codice riportato di seguito viene illustrato il codice HTML per l'elemento select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

È possibile visualizzare ogni elemento nell'elenco di selezione di un valore (0 per l'azione, 1 per serie, 2 per comici e 3 per fantascienza) e un nome visualizzato (azione, serie, comici e fantascienza). Il codice sopra riportato è HTML standard per un elenco di selezione.

Modificare l'elenco di selezione in serie e premere il **Invia** pulsante. L'URL nel browser è `http://localhost:2468/Home/CategoryChosen?MovieType=1` e Visualizza la pagina **selezionati: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Aprire il *controllers\homecontroller.cs.* file ed esaminare il `SelectCategory` metodo.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

Il [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) helper utilizzata per creare un elenco di selezione HTML richiede un **IEnumerable&lt;SelectListItem &gt;** , in modo esplicito o implicito. Vale a dire, è possibile passare il **IEnumerable&lt;SelectListItem &gt;**  in modo esplicito il [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) helper oppure è possibile aggiungere il **IEnumerable&lt; SelectListItem &gt;**  per il [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) utilizzando lo stesso nome per il **SelectListItem** come la proprietà del modello. Passando il **SelectListItem** fa in modo implicito e in modo esplicito nella parte successiva dell'esercitazione. Il codice precedente viene illustrato il modo più semplice possibile creare un **IEnumerable&lt;SelectListItem &gt;**  e popolarlo con testo e valori. Nota il `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) ha il [selezionati](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) proprietà impostata su **true;** in questo modo, l'elenco di selezione viene eseguito il rendering mostrare **comici** come l'elemento selezionato nell'elenco.

Il **IEnumerable&lt;SelectListItem &gt;**  creato sopra viene aggiunto per il [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con il nome MovieType. Si tratta di come viene passato il **IEnumerable&lt;SelectListItem &gt;**  in modo implicito al [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) helper illustrato di seguito.

Aprire il *Views\Home\SelectCategory.cshtml* file ed esaminare il markup.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Nella terza riga, è impostato il layout per Views/Shared/\_semplice\_cshtml, che è una versione semplificata del file di layout standard. È utile per mantenere la visualizzazione e il rendering HTML semplice.

In questo esempio si sta modificando non lo stato dell'applicazione, pertanto si invia i dati utilizzando un **HTTP GET**, non **HTTP POST**. Vedere la sezione W3C [un elenco di controllo rapido per la scelta di HTTP GET o POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Poiché stiamo non apportare modifiche all'applicazione e il modulo di registrazione, utilizziamo la [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) overload che consente di specificare il metodo di azione, il metodo di controller e il modulo (**HTTP POST** o **HTTP GET**). In genere viste contengono il [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) overload che non accetta parametri. La versione di parametro non predefinito per i dati del form per la versione POST dello stesso metodo di azione e il controller di registrazione.

Nella riga seguente

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

passa un argomento di stringa per il **DropDownList** helper. Questa stringa, "MovieType" in questo esempio, eseguite due operazioni:

- Fornisce la chiave per la **DropDownList** helper per trovare un **IEnumerable&lt;SelectListItem &gt;**  nel **ViewBag**.
- Si è associato a dati per l'elemento form MovieType. Se il metodo di invio è **HTTP GET**, `MovieType` sarà una stringa di query. Se il metodo di invio è **HTTP POST**, `MovieType` verrà aggiunto nel corpo del messaggio. La figura seguente mostra la stringa di query con il valore di 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Il codice seguente illustra il `CategoryChosen` il form è stato inviato al metodo.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Tornare alla pagina di prova e selezionare il **HTML SelectList** collegamento. La pagina HTML esegue il rendering di un elemento select simile alla pagina di test ASP.NET MVC semplice. Fare clic con il pulsante destro della finestra del browser e selezionare **Visualizza origine**. Il markup HTML per l'elenco di selezione è essenzialmente identico. Pagina di prova HTML, e funziona come il metodo di azione MVC ASP.NET e visualizzazione testati in precedenza.

### <a name="improving-the-movie-select-list-with-enums"></a>Migliorare l'elenco di selezione film con enumerazioni

Se le categorie dell'applicazione sono fissi e non cambia, è possibile usufruire delle enumerazioni per rendere il codice più affidabile e più semplice da estendere. Quando si aggiunge una nuova categoria, viene generato il valore della categoria corretta. Il consente di evitare errori di copia e Incolla quando si aggiunge una nuova categoria ma dimentica di aggiornare il valore della categoria.

Aprire il *controllers\homecontroller.cs.* file ed esaminare il codice seguente:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

Il [enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` acquisisce i tipi di quattro film. Il `SetViewBagMovieType` metodo crea il **IEnumerable&lt;SelectListItem &gt;**  dal `eMovieCategories` **enum**e imposta il `Selected` proprietà il `selectedMovie` parametro. Il `SelectCategoryEnum` metodo di azione utilizza la stessa vista come la `SelectCategory` metodo di azione.

Passare alla pagina di prova e fare clic su di `Select Movie Category (Enum)` collegamento. Questa volta, anziché un valore (numero) viene visualizzato, viene visualizzata una stringa che rappresenta l'enumerazione.

### <a name="posting-enum-values"></a>I valori di enumerazione di registrazione

Form HTML vengono in genere utilizzati per inviare dati al server. Il codice seguente illustra il `HTTP GET` e `HTTP POST` versioni il `SelectCategoryEnumPost` metodo.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Passando un `eMovieCategories` enum per il `POST` (metodo), è possibile estrarre il valore enum sia la stringa di enum. Eseguire l'esempio e passare alla pagina di prova. Fare clic su di `Select Movie Category(Enum Post)` collegamento. Selezionare un tipo di film e quindi fare clic sul pulsante Invia. La visualizzazione Mostra il valore sia il nome del tipo di film.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Creazione di un elemento Select più di sezione

Il [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) helper HTML esegue il rendering HTML `<select>` elemento con la `multiple` attributo, che consente agli utenti di effettuare più selezioni. Passare al collegamento di Test, quindi selezionare il **Multi selezionare paese** collegamento. Il rendering dell'interfaccia utente consente di selezionare più paesi. Nell'immagine seguente, vengono selezionati il Canada e cinese.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Analisi del codice MultiSelectCountry

Esaminare il codice seguente dal *controllers\homecontroller.cs.* file.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

Il `GetCountries` metodo crea un elenco di paesi, quindi lo passa al `MultiSelectList` costruttore. Il `MultiSelectList` overload del costruttore utilizzato nel `GetCountries` dei metodi descritti sopra accetta quattro parametri:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *gli elementi*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenenti gli elementi nell'elenco. Nell'esempio precedente, l'elenco di paesi.
2. *dataValueField*: il nome della proprietà di **IEnumerable** elenco che contiene il valore. Nell'esempio precedente, il `ID` proprietà.
3. *dataTextField*: il nome della proprietà di **IEnumerable** elenco contenente le informazioni da visualizzare. Nell'esempio precedente, il `name` proprietà.
4. *selectedValues*: l'elenco dei valori selezionati.

Nell'esempio precedente, il `MultiSelectCountry` metodo passa una `null` valore per i paesi selezionati, in modo che nessun paesi sono selezionati, verrà visualizzata l'interfaccia utente. Il codice seguente viene illustrato il markup Razor usato per eseguire il rendering di `MultiSelectCountry` vista.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

L'helper HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) metodo usato sopra accettano due parametri, il nome della proprietà per l'associazione del modello e [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) contenente selezionare le opzioni e i valori. Il `ViewBag.YouSelected` codice precedente viene utilizzato per visualizzare i valori dei paesi selezionato quando si invia il form. Esaminare l'overload del metodo HTTP POST il `MultiSelectCountry` metodo.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

Il `ViewBag.YouSelected` proprietà dinamica contiene paesi selezionati, ottenuti per il `Countries` voce della raccolta di form. In questa versione GetCountries viene passato un elenco di paesi selezionati, quando il `MultiSelectCountry` viene visualizzato, vengono selezionati i paesi selezionati nell'interfaccia utente.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Effettua una selezionare elemento descrittivo con il plug-in di jQuery raccolto scelto

Raccolto [scelto](http://harvesthq.github.com/chosen/) plug-in di jQuery possono essere aggiunti a un elemento HTML &lt;selezionare&gt; elemento per creare un utente descrittivo dell'interfaccia utente. Immagini riportate di seguito viene illustrato il raccolto [scelto](http://harvesthq.github.com/chosen/) plug-in di jQuery con `MultiSelectCountry` visualizzazione.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

In due immagini riportate di seguito, **Canada** è selezionata.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Nell'immagine precedente, è selezionato il Canada e contiene un **x** è possibile fare clic per rimuovere la selezione. L'immagine seguente mostra il Canada, Cina, e Giappone selezionato.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Associazione di jQuery raccolto scelto plug-in

Nella sezione seguente è più facile da seguire se si ha familiarità con jQuery. Se non si è mai usato jQuery prima, si desidera provare una delle seguenti esercitazioni jQuery.

- [Funzionamento di jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) da [John Resig](http://ejohn.org/)
- [Guida introduttiva a jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) da [Jörn Zaefferer](http://bassistance.de/)
- [Esempi di jQuery Live](http://codylindley.com/blogstuff/js/jquery/#) da [Cody Lindley](http://codylindley.com/)

Il plug-in selezionato è incluso nell'avvio e i progetti di esempio completo che accompagnano questa esercitazione. Solo per questa esercitazione è necessario utilizzare jQuery per associarlo con l'interfaccia utente. Per utilizzare il plug-in di jQuery scelto raccolto in un progetto ASP.NET MVC, è necessario:

1. Scaricare i plug-in selezionato da [github](https://github.com/harvesthq/chosen/). Questo passaggio è stato eseguito automaticamente.
2. Aggiungere la cartella scelta per il progetto ASP.NET MVC. Aggiungere le risorse dal plug-in selezionato che è stato scaricato nel passaggio precedente per la cartella scelta. Questo passaggio è stato eseguito automaticamente.
3. Collegare il plug-in selezionato per il **DropDownList** o **ListBox** helper HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Associazione di plug-in per la visualizzazione MultiSelectCountry scelto.

Aprire il *Views\Home\MultiSelectCountry.cshtml* file e aggiungere un `htmlAttributes` parametro per il `Html.ListBox`. Il parametro verrà aggiunto contiene un nome di classe per l'elenco di selezione (`@class = "chzn-select"`). Il codice completo è illustrato di seguito:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Nel codice precedente, si sta aggiungendo l'attributo HTML e il valore dell'attributo `class = "chzn-select"`. Il carattere @ precedente classe non ha nulla a che fare con il motore di visualizzazione Razor. `class` è un [parola chiave c#](https://msdn.microsoft.com/library/x53a06bb.aspx). Parole chiave c# non possono essere utilizzate come identificatori a meno che non includano @ come prefisso. Nell'esempio precedente, `@class` è un identificatore valido ma **classe** non **classe** è una parola chiave.

Aggiungere riferimenti al *Chosen/chosen.jquery.js* e *Chosen/chosen.css* file. Il *Chosen/chosen.jquery.js* e implementa il livello funzionale del plug-in selezionato. Il *Chosen/chosen.css* file fornisce gli stili. Aggiungere questi riferimenti a fondo il *Views\Home\MultiSelectCountry.cshtml* file. Il codice seguente viene illustrato come fare riferimento il plug-in selezionato.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Attivare il plug-in selezionato utilizzando il nome di classe utilizzato nel **Html.ListBox** codice. Nell'esempio precedente, è il nome della classe `chzn-select`. Aggiungere la riga seguente alla fine del *Views\Home\MultiSelectCountry.cshtml* file della vista. Questa riga consente di attivare il plug-in selezionato.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

La riga seguente è riportata la sintassi per chiamare la funzione di pronto jQuery, che consente di selezionare l'elemento DOM con il nome classe `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Si applica il wrapping set restituito dalla chiamata precedente al metodo scelto (`.chosen();`), che associa il plug-in selezionato.

Nel codice seguente viene completato *Views\Home\MultiSelectCountry.cshtml* file della vista.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Eseguire l'applicazione e passare il `MultiSelectCountry` visualizzazione. Provare ad aggiungere e l'eliminazione di paesi. Download di esempio fornito contiene inoltre un `MultiCountryVM` (metodo) e visualizzazione che implementa la funzionalità MultiSelectCountry utilizzando una vista del modello anziché un **ViewBag**.

Nella sezione successiva verrà visualizzato come il meccanismo di scaffolding di ASP.NET MVC funziona con il **DropDownList** helper.

> [!div class="step-by-step"]
> [avanti](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
