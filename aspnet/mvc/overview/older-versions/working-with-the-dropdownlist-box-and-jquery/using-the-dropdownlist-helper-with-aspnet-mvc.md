---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Utilizzo del DropDownList Helper con ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 600a8ad409d55f0b01eedc8168eaec2b39c88a3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837391"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Utilizzo del DropDownList Helper con ASP.NET MVC
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

Questa esercitazione insegnerà le nozioni di base dell'utilizzo con il [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper e il [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) helper in un'applicazione Web ASP.NET MVC. È possibile utilizzare Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio per seguire l'esercitazione. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:

- [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)

Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Questa esercitazione si presuppone di aver completato la [Introduzione ad ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) esercitazione o la[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) esercitazione o si ha familiarità con lo sviluppo di ASP.NET MVC. Questa esercitazione inizia con un progetto modificato dal [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) esercitazione. È possibile scaricare il progetto iniziale con il collegamento seguente [scaricare la versione c#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Un progetto di Visual Web Developer nell'esercitazione completata il codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Scopo dell'esercitazione

Verranno creati i metodi di azione e le viste che utilizzano le [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) helper per selezionare una categoria. Si utilizzerà inoltre **jQuery** per aggiungere una finestra di dialogo categoria inserimento che può essere usato quando è necessaria una nuova categoria (ad esempio, genere o un artista). Di seguito è riportata una schermata della visualizzazione Create che mostra i collegamenti per aggiungere un genere nuovo e aggiungere un nuovo artista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Competenze

Ecco cosa si apprenderà:

- Come usare il [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) helper per selezionare i dati di categoria.
- Come aggiungere un **jQuery** finestra di dialogo per aggiungere nuove categorie.

### <a name="getting-started"></a>Introduzione

Iniziare scaricando il progetto iniziale con il collegamento seguente, [scaricare](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). In Windows Explorer, fare clic sulla *DDL\_file* file e scegliere Proprietà. Nel **DDL\_proprietà file** nella finestra di dialogo Seleziona Annulla blocco.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Fare clic con il pulsante destro l'istruzione DDL\_file di file, quindi scegliere **Estrai tutto** per decomprimere il file. Aprire il *StartMusicStore.sln* file con Visual Studio 2010 o Visual Web Developer 2010 Express ("Visual Web Developer" o "VWD" breve).

Premere CTRL+F5 per eseguire l'applicazione, quindi scegliere il **Test** collegamento.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Selezionare il **selezionare la categoria di film (semplice)** collegamento. Viene visualizzato un elenco di film tipo selezionare, con commedie il valore selezionato.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Fare clic nel browser e selezionando HTML. Viene visualizzato il codice HTML della pagina. Il codice seguente illustra il codice HTML per l'elemento select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Si noterà che ogni voce nell'elenco di selezione dispone di un valore (0 per l'azione, per drammatico 1, 2 per commedie e 3 per fantascienza) e un nome visualizzato (Action, fiction, commedie e fantascienza). Il codice riportato sopra è HTML standard per un elenco di selezione.

Modificare l'elenco di selezione in drammatico dei comandi e premere il **Submit** pulsante. L'URL nel browser viene `http://localhost:2468/Home/CategoryChosen?MovieType=1` e la pagina viene visualizzata **selezionata: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Aprire il *Controllers\HomeController.cs* del file ed esaminare il `SelectCategory` (metodo).

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

Il [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) helper utilizzata per creare un elenco di selezione HTML è necessario un **IEnumerable&lt;SelectListItem &gt;** , in modo implicito o esplicito. Vale a dire, è possibile passare il **IEnumerable&lt;SelectListItem &gt;**  in modo esplicito il [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) helper oppure è possibile aggiungere il **IEnumerable&lt; SelectListItem &gt;**  per il [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) usando lo stesso nome per il **SelectListItem** come proprietà del modello. Passando il **SelectListItem** in modo implicito e in modo esplicito, vedere la parte successiva dell'esercitazione. Il codice precedente illustra il modo più semplice possibile per creare un **IEnumerable&lt;SelectListItem &gt;**  e popolarlo con testo e valori. Nota il `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) ha la [selezionati](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) proprietà impostata su **true;** ciò causerà l'elenco di selezione sottoposto a rendering mostrare **commedie** come l'elemento selezionato nell'elenco.

Il **IEnumerable&lt;SelectListItem &gt;**  creato sopra viene aggiunto per il [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con il nome MovieType. Come si passano le **IEnumerable&lt;SelectListItem &gt;**  in modo implicito al [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) helper illustrato di seguito.

Aprire il *Views\Home\SelectCategory.cshtml* file ed esaminare il markup.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Nella terza riga, impostiamo il layout su Views/Shared/\_semplice\_layout. cshtml, che è una versione semplificata del file di layout standard. È scopo di mantenere la visualizzazione e il rendering HTML semplice.

In questo esempio si intende modificare non lo stato dell'applicazione, pertanto si invieranno i dati usando un **HTTP GET**, non **HTTP POST**. Vedere la sezione W3C [rapido elenco di controllo per la scelta di HTTP GET o POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Poiché stiamo non apportare modifiche all'applicazione e il modulo di registrazione, utilizziamo il [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) overload che consente di specificare il metodo di azione, il metodo controller e il modulo (**HTTP POST** o**HTTP GET**). In genere le visualizzazioni contengono i [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) overload che non accetta parametri. La versione del parametro non viene impostato su registrazione i dati del modulo alla versione POST di stesso metodo di azione e controller.

Nella riga seguente

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

passa un argomento stringa per il **DropDownList** helper. Questa stringa, "MovieType" in questo esempio, comporta due operazioni:

- Fornisce la chiave per la **DropDownList** helper per trovare una **IEnumerable&lt;SelectListItem &gt;**  nel **ViewBag**.
- Si è associato a dati per l'elemento del form MovieType. Se il metodo submit **HTTP GET**, `MovieType` sarà una stringa di query. Se il metodo submit **HTTP POST**, `MovieType` verranno aggiunti nel corpo del messaggio. L'immagine seguente mostra la stringa di query con il valore 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Il codice seguente illustra il `CategoryChosen` il form è stato inviato al metodo.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Tornare alla pagina di prova e selezionare il **HTML SelectList** collegamento. La pagina HTML esegue il rendering di un elemento select simile alla pagina di test di MVC ASP.NET semplice. Fare clic con il pulsante destro della finestra del browser e selezionare **Visualizza origine**. Il markup HTML per l'elenco di selezione è essenzialmente identico. Pagina di prova il codice HTML, funziona come il metodo di azione MVC ASP.NET e visualizzazione che abbiamo testato in precedenza.

### <a name="improving-the-movie-select-list-with-enums"></a>Migliorare l'elenco di selezione di film con enumerazioni senza ambito

Se le categorie dell'applicazione sono fissi e non cambia, è possibile sfruttare i vantaggi delle enumerazioni per rendere il codice più affidabile e più semplice da estendere. Quando si aggiunge una nuova categoria, viene generato il valore della categoria corretta. L'evita gli errori di copia e Incolla quando si aggiunge una nuova categoria senza dimenticare di aggiornare il valore della categoria.

Aprire il *Controllers\HomeController.cs* file ed esaminare il codice seguente:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

Il [enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` acquisisce i tipi di quattro film. Il `SetViewBagMovieType` metodo crea il **IEnumerable&lt;SelectListItem &gt;**  dal `eMovieCategories` **enum**e imposta il `Selected` proprietà dal `selectedMovie` parametro. Il `SelectCategoryEnum` metodo di azione utilizza la stessa visualizzazione di `SelectCategory` metodo di azione.

Passare alla pagina di Test e fare clic su di `Select Movie Category (Enum)` collegamento. Questa volta, anziché un valore (number) viene visualizzato, viene visualizzata una stringa che rappresenta l'enumerazione.

### <a name="posting-enum-values"></a>I valori di enumerazione di registrazione

Moduli HTML sono in genere usati per inviare dati al server. Il codice seguente illustra il `HTTP GET` e `HTTP POST` le versioni del `SelectCategoryEnumPost` (metodo).

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Passando un `eMovieCategories` enum per il `POST` metodo, è possibile estrarre il valore enum sia la stringa di enumerazione. Eseguire l'esempio e passare alla pagina di Test. Fare clic su di `Select Movie Category(Enum Post)` collegamento. Selezionare un tipo di film e quindi fare clic sul pulsante Invia. La visualizzazione Mostra il valore sia il nome del tipo di film.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Creazione di un elemento Select sezione più

Il [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) helper HTML esegue il rendering HTML `<select>` elemento con la `multiple` attributo, che consente agli utenti di effettuare più selezioni. Passare al collegamento di Test e quindi selezionare il **paese selezionare Multi** collegamento. Il rendering dell'interfaccia utente consente di selezionare più paesi. Nell'immagine seguente, vengono selezionati in Canada e Cina.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Analisi del codice MultiSelectCountry

Esaminare il codice seguente il *Controllers\HomeController.cs* file.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

Il `GetCountries` metodo crea un elenco di paesi, quindi lo passa al `MultiSelectList` costruttore. Il `MultiSelectList` overload del costruttore utilizzata nel `GetCountries` metodo precedente accetta quattro parametri:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *gli elementi*: un' [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenente gli elementi nell'elenco. Nell'esempio precedente, l'elenco di paesi.
2. *dataValueField*: il nome della proprietà nel **IEnumerable** elenco che contiene il valore. Nell'esempio precedente, il `ID` proprietà.
3. *dataTextField*: il nome della proprietà nel **IEnumerable** elenco che contiene le informazioni da visualizzare. Nell'esempio precedente, il `name` proprietà.
4. *selectedValues*: l'elenco di valori selezionati.

Nell'esempio precedente, il `MultiSelectCountry` metodo passa un `null` valore per i paesi selezionati, in modo da alcun paese non sono selezionati quando viene visualizzata l'interfaccia utente. Il codice seguente viene illustrato il markup Razor per eseguire il rendering di `MultiSelectCountry` visualizzazione.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

L'helper HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) metodo usato sopra accettano due parametri, il nome della proprietà per l'associazione del modello e il [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) contenenti i valori e selezionare le opzioni. Il `ViewBag.YouSelected` codice sopra riportato viene utilizzato per visualizzare i valori dei paesi selezionato quando si invia il form. Esaminare l'overload del metodo HTTP POST il `MultiSelectCountry` (metodo).

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

Il `ViewBag.YouSelected` proprietà dinamica contiene paesi selezionati, ottenuti per il `Countries` voce nella raccolta di form. In questa versione al metodo GetCountries viene passato un elenco di paesi selezionati, pertanto quando il `MultiSelectCountry` verrà aperta la visualizzazione, vengono selezionati i paesi selezionati nell'interfaccia utente.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Effettua una selezionare elemento descrittivo con il componente aggiuntivo jQuery Harvest scelto plug-in

Le raccolto [scelto](http://harvesthq.github.com/chosen/) plug-in jQuery possono essere aggiunti a un elemento HTML &lt;seleziona&gt; elemento per creare un utente descrittivo dell'interfaccia utente. Le immagini seguenti illustrano il Harvest [scelto](http://harvesthq.github.com/chosen/) plug-in jQuery con `MultiSelectCountry` vista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

Nelle due immagini seguenti, **Canada** sia selezionata.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Nell'immagine precedente, in Canada sia selezionato e contiene un **x** è possibile fare clic per rimuovere la selezione. L'immagine seguente mostra in Canada, Cina, e il Giappone selezionato.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Associazione di plug-in jQuery Harvest scelto

La sezione seguente è più semplice da seguire se si ha familiarità con jQuery. Se è mai stata usata prima di jQuery, è possibile provare una delle esercitazioni di jQuery seguente.

- [Funzionamento di jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) da [John Resig](http://ejohn.org/)
- [Introduzione a jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) da [Jörn Zaefferer](http://bassistance.de/)
- [Esempi di jQuery Live](http://codylindley.com/blogstuff/js/jquery/#) da [Cody Lindley](http://codylindley.com/)

Il plug-in scelto è incluso in starter e progetti di esempio completo che accompagna questa esercitazione. Solo per questa esercitazione è necessario usare jQuery per associarlo con l'interfaccia utente. Per usare il plug-in jQuery Harvest scelto in un progetto ASP.NET MVC, è necessario:

1. Scarica plug-in selezionato dal [github](https://github.com/harvesthq/chosen/). Questo passaggio è stato eseguito automaticamente.
2. Aggiungere la cartella selezionata per il progetto ASP.NET MVC. Aggiungere gli asset dal plug-in scelto che è stato scaricato nel passaggio precedente della cartella scelta. Questo passaggio è stato eseguito automaticamente.
3. Collegare il plug-in selezionato per il **DropDownList** oppure **ListBox** helper HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Hook del plug-in scelte per la visualizzazione MultiSelectCountry.

Aprire il *Views\Home\MultiSelectCountry.cshtml* file e aggiungere un' `htmlAttributes` parametro per il `Html.ListBox`. Il parametro verrà aggiunto contiene un nome di classe per l'elenco di selezione (`@class = "chzn-select"`). Il codice completo è illustrato di seguito:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Nel codice precedente, che stiamo aggiungendo l'attributo HTML e il valore dell'attributo `class = "chzn-select"`. Il \@ carattere classe precedente ha nulla a che fare con il motore di visualizzazione Razor. `class` è un [parola chiave c#](https://msdn.microsoft.com/library/x53a06bb.aspx). Parole chiave c# non possono essere utilizzate come identificatori a meno che non includano \@ come prefisso. Nell'esempio precedente, `@class` è un identificatore valido, ma **classe** non perché **classe** è una parola chiave.

Aggiungere i riferimenti per il *Chosen/chosen.jquery.js* e *Chosen/chosen.css* file. Il *Chosen/chosen.jquery.js* e implementa il livello funzionale del plug-nella scelta. Il *Chosen/chosen.css* file offre l'applicazione di stili. Aggiungere questi riferimenti a fondo le *Views\Home\MultiSelectCountry.cshtml* file. Il codice seguente mostra come fare riferimento il plug-in selezionato.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Attivare il plug-in selezionato utilizzando il nome della classe utilizzato nel **Html.ListBox** codice. Nell'esempio precedente, è il nome della classe `chzn-select`. Aggiungere la riga seguente alla fine della *Views\Home\MultiSelectCountry.cshtml* file di visualizzazione. Questa riga consente di attivare il plug-in selezionato.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

La riga seguente è la sintassi per chiamare la funzione di pronto jQuery, che seleziona l'elemento DOM con nome di classe `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Sottoposto a incapsulamento set restituito dalla chiamata precedente si applica al metodo scelto (`.chosen();`), che esegue il plug-in selezionato.

Il codice seguente illustra il completate *Views\Home\MultiSelectCountry.cshtml* file di visualizzazione.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Eseguire l'applicazione e passare al `MultiSelectCountry` vista. Provare ad aggiungere e l'eliminazione di paesi. Download di esempio fornito contiene anche un `MultiCountryVM` metodo e vista che implementa la funzionalità MultiSelectCountry tramite una visualizzazione del modello anziché un **ViewBag**.

Nella sezione successiva si vedrà come il meccanismo di scaffolding di ASP.NET MVC funziona con il **DropDownList** helper.

> [!div class="step-by-step"]
> [avanti](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
