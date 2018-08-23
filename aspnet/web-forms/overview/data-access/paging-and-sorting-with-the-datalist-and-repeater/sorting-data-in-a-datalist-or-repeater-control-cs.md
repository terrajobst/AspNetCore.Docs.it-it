---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Ordinamento dei dati in un controllo DataList o Repeater (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà esaminato come includere l'ordinamento di supporto con DataList e Repeater, nonché come costruire un controllo DataList o Repeater i cui dati possono...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 05fbc51d5341a4d3d634cbbc05c0e66a827b0394
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833954"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Ordinamento dei dati in un controllo DataList o Repeater (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) o [Scarica il PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> In questa esercitazione verrà esaminato come includere l'ordinamento di supporto con DataList e Repeater, nonché come costruire un controllo DataList o Repeater i cui dati possono essere di paging e ordinamento.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](paging-report-data-in-a-datalist-or-repeater-control-cs.md) abbiamo esaminato il modo aggiungere il supporto di paging per un controllo DataList. È stato creato un nuovo metodo nella `ProductsBLL` classe (`GetProductsAsPagedDataSource`) che ha restituito un `PagedDataSource` oggetto. Quando è associato a un controllo DataList o Repeater, DataList o Repeater verranno visualizzati solo la pagina richiesta dei dati. Questa tecnica è simile a quello utilizzato internamente dai controlli GridView, DetailsView e FormView per fornire le funzionalità di paging predefinito incorporato.

Oltre a offrirti supporto per lo spostamento, GridView include anche supporto per l'ordinamento predefiniti. Nessuna delle due DataList e Repeater fornisce funzionalità di ordinamento predefinita; Tuttavia, funzionalità di ordinamento possono essere aggiunti con un po' di codice. In questa esercitazione verrà esaminato come includere l'ordinamento di supporto con DataList e Repeater, nonché come costruire un controllo DataList o Repeater i cui dati possono essere di paging e ordinamento.

## <a name="a-review-of-sorting"></a>Una revisione dell'ordinamento

Come abbiamo visto nel [Paging e ordinamento dei dati del Report](../paging-and-sorting/paging-and-sorting-report-data-cs.md) esercitazione, il controllo GridView è predefiniti supporto per l'ordinamento. Ogni campo di GridView può avere associato un `SortExpression`, che indica il campo dati in base alle quali ordinare i dati. Quando i dispositivi di GridView `AllowSorting` è impostata su `true`, ogni campo GridView con un `SortExpression` valore di proprietà contiene l'intestazione specificata sottoposto a rendering come un controllo LinkButton. Quando un utente fa clic su una particolare intestazione s campo di GridView, si verifica un postback e i dati vengono ordinati in base al campo selezionato s `SortExpression`.

Il controllo GridView è un `SortExpression` proprietà, che archivia il `SortExpression` del campo GridView vengono ordinati i dati. Inoltre, un `SortDirection` proprietà indica se i dati sono da ordinare in ordine crescente o decrescente (se un utente fa clic su un particolare collegamento di intestazione campo s GridView due volte in successione, l'ordinamento è attivato o disattivato).

Quando il controllo GridView è associato al relativo controllo origine dati, passa relativi `SortExpression` e `SortDirection` proprietà ai dati di controllo del codice sorgente. Il controllo origine dati recupera i dati e quindi la Ordina in base a fornito `SortExpression` e `SortDirection` proprietà. Dopo l'ordinamento dei dati, il controllo origine dati restituisce a GridView.

Per replicare questa funzionalità con i controlli DataList o Repeater, è necessario:

- Creare un'interfaccia di ordinamento
- Tenere presente il campo di dati per ordinare in base e se eseguire l'ordinamento in ordine crescente o decrescente
- Indicare a ObjectDataSource per ordinare i dati da un campo dati specifico

Che verranno affrontati queste tre attività nei passaggi 3 e 4. In seguito, verrà esaminato come includere sia il paging e ordinamento di supporto in un controllo DataList o Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Passaggio 2: Visualizzare i prodotti in un controllo Repeater

Prima ci preoccupiamo implementa le funzionalità correlate all'ordinamento, consentire s inizia creando un elenco di prodotti in un controllo Repeater. Iniziare aprendo il `Sorting.aspx` nella pagina di `PagingSortingDataListRepeater` cartella. Aggiungere un controllo Repeater per la pagina web, l'impostazione relativa `ID` proprietà `SortableProducts`. Nello smart tag Repeater s, creare un nuovo oggetto ObjectDataSource denominato `ProductsDataSource` e configurarlo per recuperare i dati di `ProductsBLL` classe s `GetProducts()` (metodo). Selezionare il che opzione (nessuno) dagli elenchi a discesa nelle schede INSERT, UPDATE e DELETE.


[![Creare un ObjectDataSource e configurarlo per usare il metodo GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Figura 1**: creare un ObjectDataSource e configurarlo per usare il `GetProductsAsPagedDataSource()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Figura 2**: impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


A differenza con il DataList, Visual Studio non crea automaticamente un `ItemTemplate` per il controllo Repeater dopo l'associazione a un'origine dati. Inoltre, è necessario aggiungere questo `ItemTemplate` in modo dichiarativo, come lo smart tag di controllo del codice s Repeater manca l'opzione di modifica modelli disponibile in DataList s. S ti permettono di usare lo stesso `ItemTemplate` dall'esercitazione precedente, visualizzare il nome del prodotto s, fornitore e categoria.

Dopo aver aggiunto il `ItemTemplate`, Repeater e ObjectDataSource s markup dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Figura 3 Mostra questa pagina quando viene visualizzato tramite un browser.


[![Viene visualizzato ogni s nome, fornitore e la categoria di prodotto](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Figura 3**: ogni prodotto s nome, fornitore e la categoria viene visualizzata ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Passaggio 3: Che indicano a ObjectDataSource per ordinare i dati

Per ordinare i dati visualizzati nel Repeater, dobbiamo informare ObjectDataSource dell'espressione di ordinamento mediante il quale i dati devono essere ordinati. Prima di ObjectDataSource recupera i relativi dati, viene generato prima di tutto relativi [ `Selecting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), che offre un'opportunità specificare un'espressione di ordinamento. Il `Selecting` gestore dell'evento viene passato un oggetto di tipo [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), che include una proprietà denominata [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). Il `DataSourceSelectArguments` classe è progettata per passare le richieste correlate ai dati da un consumer di dati per il controllo origine dati e include una [ `SortExpression` proprietà](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Per passare informazioni sull'ordinamento dalla pagina ASP.NET a ObjectDataSource, creare un gestore eventi per il `Selecting` evento e usare il codice seguente:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

Il *sortExpression* valore deve essere assegnato il nome del campo dati per ordinare i dati in base (ad esempio ProductName). È presente nessuna proprietà correlate alla direzione di ordinamento, pertanto, se si desidera ordinare i dati in ordine decrescente, aggiungere la stringa DESC per il *sortExpression* valore (ad esempio ProductName DESC).

Procediamo e proviamolo alcuni valori diversi impostati come hardcoded per *sortExpression* e testare i risultati in un browser. Come illustrato nella figura 4, quando si usa ProductName DESC come le *sortExpression*, i prodotti vengono ordinati in base al nome in ordine alfabetico inverso.


[![I prodotti vengono ordinati in base al nome in ordine alfabetico inverso](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Figura 4**: The prodotti vengono ordinati in base al nome in ordine alfabetico inverso ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Passaggio 4: Creazione dell'interfaccia di ordinamento e la memorizzazione di espressione di ordinamento e la direzione

Attivare l'ordinamento di supporto in GridView converte ogni testo dell'intestazione campo ordinabile s in un controllo LinkButton che, quando si fa clic, Ordina i dati di conseguenza. Questo tipo è un'interfaccia di ordinamento senso per il controllo GridView, in cui i dati ben disposti in colonne. Per i controlli DataList e Repeater, tuttavia, è necessaria un'altra interfaccia di ordinamento. Un'interfaccia comune di ordinamento per un elenco di dati (anziché una griglia dei dati), è un elenco di riepilogo a discesa che contiene i campi con cui i dati possono essere ordinati. Lasciare s che implementano tale interfaccia per questa esercitazione.

Aggiungere un controllo DropDownList Web precedente la `SortableProducts` Repeater e set relativo `ID` proprietà `SortBy`. Nella finestra Proprietà fare clic sui puntini di sospensione di `Items` proprietà per visualizzare Editor raccolta di ListItem. Aggiungere `ListItem` s per ordinare i dati per il `ProductName`, `CategoryName`, e `SupplierName` campi. Aggiungere anche un `ListItem` per ordinare i prodotti in base al nome in ordine alfabetico inverso.

Il `ListItem` `Text` proprietà possono essere impostate su qualsiasi valore (ad esempio nome), ma il `Value` devono essere impostate per il nome del campo dati (ad esempio ProductName). Per ordinare i risultati in ordine decrescente, aggiungere la stringa DESC per il nome del campo dati, ad esempio ProductName DESC.


![Aggiungere un elemento ListItem per ognuno dei campi dati ordinabili](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Figura 5**: aggiungere un `ListItem` per ognuno dei campi dati ordinabili


Infine, aggiungere un controllo Web sul pulsante a destra del DropDownList. Impostare relativi `ID` al `RefreshRepeater` e il relativo `Text` proprietà per l'aggiornamento.

Dopo aver creato il `ListItem` s e aggiungere il pulsante di aggiornamento, la sintassi dichiarativa s DropDownList e pulsante dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Con ordinamento DropDownList completo, è quindi necessario aggiornare la s ObjectDataSource `Selecting` gestore dell'evento in modo che utilizzi selezionata `SortBy``ListItem` s `Value` proprietà anziché un'espressione di ordinamento a livello di codice.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

A questo punto, durante la prima visita la pagina dei prodotti verranno inizialmente essere ordinati in base il `ProductName` campo di dati, come s il `SortBy` `ListItem` selezionata per impostazione predefinita (vedere la figura 6). Selezionando una diversa opzione come categoria di ordinamento e fa clic su Aggiorna viene provocato un postback e riordinamento dei dati per il nome di categoria, come illustrato nella figura 7.


[![I prodotti sono inizialmente ordinati in base al nome](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Figura 6**: The prodotti sono inizialmente ordinati in base al nome ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![I prodotti sono ora ordinati per categoria](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Figura 7**: The prodotti sono ora ordinati per categoria ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Facendo clic sul pulsante di aggiornamento, i dati essere automaticamente nuovamente ordinato perché lo stato di visualizzazione Repeater s è stato disabilitato, determinando il Repeater riassociare all'origine dati in ogni postback. Se è stato lasciato il Repeater s lo stato di visualizzazione abilitato, la modifica dell'ordinamento elenco a discesa elenco ha vinto t ha alcun effetto sulla sequenza di ordinamento. Per risolvere questo problema, creare un gestore eventi per s pulsante Refresh `Click` evento e riassociazione di Repeater all'origine dati (chiamando il Repeater s `DataBind()` (metodo)).


## <a name="remembering-the-sort-expression-and-direction"></a>Ricordare l'espressione di ordinamento e la direzione

Durante la creazione di un controllo DataList o Repeater ordinabile in una pagina in cui non ordina correlati possono verificarsi durante i postback, il codice imperativo per s che l'espressione di ordinamento e la direzione mantenuta durante i postback. Si supponga, ad esempio, che viene aggiornato il Repeater in questa esercitazione per includere un pulsante di eliminazione con ogni prodotto. Quando l'utente fa clic sul pulsante Elimina d eseguiamo il codice per eliminare il prodotto selezionato e quindi associare di nuovo i dati al controllo Repeater. Se i dettagli di ordinamento non sono persistenti durante il postback, i dati visualizzati sullo schermo verranno ripristinato l'ordine originale.

Per questa esercitazione, DropDownList in modo implicito Salva l'ordinamento espressione e alla direzione in stato di visualizzazione per noi. Se si stesse usando un'interfaccia di ordinamento diversa uno con, i controlli LinkButton ad esempio, che ha fornito le varie opzioni di ordinamento d occorre prestare attenzione a ricordare l'ordinamento durante i postback. Ad esempio archiviando i parametri di ordinamento nello stato di visualizzazione pagina s, includendo il parametro di ordinamento nella stringa di query, o tramite un'altra tecnica di persistenza dello stato.

Future esempi in questa esercitazione Scopri come rendere permanente l'ordinamento dettagli nello stato di visualizzazione pagina s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Passaggio 5: Aggiunta del supporto per l'ordinamento a un controllo DataList che usa il Paging predefinito

Nel [esercitazione precedente](paging-report-data-in-a-datalist-or-repeater-control-cs.md) abbiamo esaminato come implementare il paging predefinito con un controllo DataList. Let s estendere questo esempio precedente in modo da includere la possibilità di ordinare i dati di paging. Iniziare aprendo il `SortingWithDefaultPaging.aspx` e `Paging.aspx` pagine di `PagingSortingDataListRepeater` cartella. Dal `Paging.aspx` pagina fare clic sul pulsante di origine per visualizzare il markup dichiarativo pagina s. Copiare il testo selezionato (vedere la figura 8) e incollarlo nel markup dichiarativo di `SortingWithDefaultPaging.aspx` tra il `<asp:Content>` tag.


[![Replicare il Markup dichiarativo nel &lt;asp: Content&gt; tag di paging. aspx per SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Figura 8**: replicare Markup dichiarativo nel `<asp:Content>` tag dal `Paging.aspx` al `SortingWithDefaultPaging.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


Dopo aver copiato il codice dichiarativo, copiare i metodi e proprietà nel `Paging.aspx` pagina classe code-behind s per la classe code-behind per `SortingWithDefaultPaging.aspx`. Successivamente, si consiglia di visualizzare il `SortingWithDefaultPaging.aspx` pagina in un browser. Deve presentare la stessa funzionalità e aspetto come `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Miglioramento ProductsBLL per includere un valore predefinito di Paging e ordinamento (metodo)

Nell'esercitazione precedente è stato creato un `GetProductsAsPagedDataSource(pageIndex, pageSize)` metodo nella `ProductsBLL` classe che ha restituito un `PagedDataSource` oggetto. Ciò `PagedDataSource` oggetto è stato popolato con *tutte le* dei prodotti (tramite i dispositivi di livello BLL `GetProducts()` metodo), ma quando è associato a DataList solo i record corrispondente all'oggetto specificato *pageIndex* e *pageSize* sono stati visualizzati i parametri di input.

In precedenza in questa esercitazione è stato aggiunto il supporto dell'ordinamento, specificando l'espressione di ordinamento da ObjectDataSource s `Selecting` gestore dell'evento. Questo funziona bene quando ObjectDataSource viene restituito un oggetto che è possibile ordinare, ad esempio la `ProductsDataTable` restituito dal `GetProducts()` (metodo). Tuttavia, il `PagedDataSource` oggetto restituito dal `GetProductsAsPagedDataSource` (metodo) non supporta l'ordinamento della relativa origine dati interna. In alternativa, è necessario ordinare i risultati restituiti dal `GetProducts()` metodo *prima di* inseriamo nel `PagedDataSource`.

A questo scopo, creare un nuovo metodo nella `ProductsBLL` classe `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Per ordinare le `ProductsDataTable` restituito dal `GetProducts()` metodo, specificare il `Sort` proprietà del relativo valore predefinito `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Il `GetProductsSortedAsPagedDataSource` metodo differisce solo leggermente dal `GetProductsAsPagedDataSource` metodo creato nell'esercitazione precedente. In particolare, `GetProductsSortedAsPagedDataSource` accetta un parametro di input aggiuntivo `sortExpression` e assegna questo valore per il `Sort` proprietà del `ProductDataTable` s `DefaultView`. Poche righe di codice in un secondo momento, il `PagedDataSource` oggetto s DataSource viene assegnato il `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>La chiamata al metodo GetProductsSortedAsPagedDataSource e specificando il valore del parametro di Input SortExpression

Con la `GetProductsSortedAsPagedDataSource` metodo completo, il passaggio successivo consiste nel fornire il valore per questo parametro. Oggetto ObjectDataSource `SortingWithDefaultPaging.aspx` è attualmente configurato per chiamare il `GetProductsAsPagedDataSource` metodo e passa due parametri di input tramite i relativi due `QueryStringParameters`, specificata nel `SelectParameters` raccolta. Questi due `QueryStringParameters` indicano che l'origine per il `GetProductsAsPagedDataSource` metodo s *pageIndex* e *pageSize* parametri provengono dai campi querystring `pageIndex` e `pageSize`.

Aggiornare gli oggetti ObjectDataSource `SelectMethod` proprietà, in modo che si richiama il nuovo `GetProductsSortedAsPagedDataSource` (metodo). Quindi, aggiungere una nuova `QueryStringParameter` in modo che il *sortExpression* parametro di input è accessibile del campo querystring `sortExpression`. Impostare il `QueryStringParameter` s `DefaultValue` a ProductName.

Dopo tali modifiche, il markup dichiarativo di ObjectDataSource s dovrebbe essere simile:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

A questo punto, il `SortingWithDefaultPaging.aspx` pagina verrà ordinare alfabeticamente i relativi risultati dal nome del prodotto (vedere la figura 9). Questo avviene perché, per impostazione predefinita, viene passato un valore di ProductName come le `GetProductsSortedAsPagedDataSource` metodo s *sortExpression* parametro.


[![Per impostazione predefinita, i risultati sono ordinati per ProductName](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Figura 9**: per impostazione predefinita, i risultati vengono ordinati `ProductName` ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Se si aggiunge manualmente un `sortExpression` campo stringa di query, ad esempio `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` verranno ordinati i risultati dall'oggetto specificato `sortExpression`. Tuttavia, ciò `sortExpression` parametro non è incluso nella stringa di query quando si spostano in un'altra pagina di dati. In effetti, facendo clic nella pagina successiva o ultimo pulsanti accetta Microsoft al `Paging.aspx`! Inoltre, ci s attualmente alcun ordinamento non interfaccia. L'unico modo che un utente può modificare l'ordinamento dei dati di paging è manipolando direttamente il parametro querystring.

## <a name="creating-the-sorting-interface"></a>Creazione dell'interfaccia di ordinamento

È necessario innanzitutto aggiornare il `RedirectUser` metodo per inviare all'utente `SortingWithDefaultPaging.aspx` (invece di `Paging.aspx`) e includere il `sortExpression` valore nella stringa di query. Va inoltre aggiunto una sola lettura, a livello di pagina denominato `SortExpression` proprietà. Questa proprietà, simile al `PageIndex` e `PageSize` proprietà creata nell'esercitazione precedente, restituisce il valore della `sortExpression` campo querystring se presente e il valore predefinito (ProductName) in caso contrario.

Attualmente il `RedirectUser` metodo accetta solo un singolo parametro di input l'indice della pagina da visualizzare. Tuttavia, potrebbero esserci casi è necessario reindirizzare l'utente a una determinata pagina di dati usando un'espressione di ordinamento diverso da quali s specificato nella stringa di query. In un momento in cui verrà creato l'interfaccia di ordinamento per questa pagina, che include una serie di controlli Web di pulsante per l'ordinamento dei dati da una colonna specificata. Quando viene selezionato uno di questi pulsanti, si desidera reindirizzare l'utente passando il valore di espressione di ordinamento appropriato. Per fornire questa funzionalità, creare due versioni del `RedirectUser` (metodo). Il primo deve accettare solo l'indice della pagina da visualizzare, mentre il secondo accetta l'espressione di indice e di ordinamento di pagina.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

Nel primo esempio in questa esercitazione, abbiamo creato un'interfaccia di ordinamento usando un controllo DropDownList. Per questo esempio, s ti permettono di usare tre controlli pulsante Web posizionati di sopra di DataList uno per l'ordinamento in base `ProductName`, uno per `CategoryName`e uno per `SupplierName`. Aggiungere i tre controlli pulsante Web, l'impostazione loro `ID` e `Text` proprietà in modo appropriato:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Creare quindi un `Click` gestore eventi per ognuno. I gestori di eventi devono chiamare il `RedirectUser` (metodo), restituzione all'utente per la prima pagina utilizzando l'espressione di ordinamento appropriato.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Durante la prima visita la pagina, i dati sono ordinati per il nome del prodotto in ordine alfabetico (vedere la figura 9). Fare clic sul pulsante Avanti per passare alla seconda pagina di dati, quindi scegliere l'ordinamento dal pulsante di categoria. Questo ci riporta alla prima pagina di dati, ordinati in base al nome di categoria (vedere la figura 10). Analogamente, scegliendo l'ordinamento dal pulsante di fornitore Ordina i dati dal fornitore a partire dalla prima pagina di dati. La scelta di ordinamento viene memorizzata come viene eseguito il paging attraverso i dati. Figura 11 mostra la pagina dopo l'ordinamento in base alla categoria e quindi procedendo alla pagina tredicesima dei dati.


[![I prodotti sono ordinati per categoria](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Figura 10**: The prodotti sono ordinati per categoria ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![L'espressione di ordinamento sono memorizzate quando il Paging attraverso il dati](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Figura 11**: l'espressione di ordinamento sono memorizzate quando il Paging attraverso il dati ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Passaggio 6: Paging personalizzato dei record in un controllo Repeater

Nell'esempio di DataList esaminato nel passaggio 5 pagine tramite i relativi dati tramite la tecnica di paging predefinito inefficiente. Quando il paging attraverso sufficientemente grandi quantità di dati, è fondamentale che essere utilizzato il paging personalizzato. Nel [in modo efficiente il Paging attraverso quantità di dati di grandi dimensioni](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) e [ordinamento personalizzato dei dati di paging](../paging-and-sorting/sorting-custom-paged-data-cs.md) esercitazioni, sono state esaminate le differenze tra predefinito e il paging personalizzato e i metodi creati nel livello BLL per utilizzo personalizzato di paging e ordinamento dei dati di paging personalizzati. In particolare, in queste due esercitazioni precedenti abbiamo aggiunto i tre metodi seguenti per il `ProductsBLL` classe:

- `GetProductsPaged(startRowIndex, maximumRows)` Restituisce un sottoinsieme particolare di record partire *startRowIndex* e non supera *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` Restituisce un sottoinsieme particolare di record ordinati in base ai valori *sortExpression* parametro di input.
- `TotalNumberOfProducts()` fornisce il numero totale di record nel `Products` tabella di database.

Questi metodi sono utilizzabile per pagina e ordinare i dati usando un controllo DataList o Repeater in modo efficiente. Per illustrare questo concetto, consentire s iniziare creando un controllo Repeater con supporto di paging personalizzato; verrà quindi aggiunto funzionalità di ordinamento.

Aprire il `SortingWithCustomPaging.aspx` nella pagina la `PagingSortingDataListRepeater` cartella e aggiungere un controllo Repeater alla pagina di impostazione relativo `ID` proprietà `Products`. Nello smart tag Repeater s, creare un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Per selezionare i dati da configurare il `ProductsBLL` classe s `GetProductsPaged` (metodo).


[![Configurare ObjectDataSource per usare il metodo GetProductsPaged ProductsBLL classe s](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Figura 12**: configurare ObjectDataSource per usare il `ProductsBLL` classe s `GetProductsPaged` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Impostare gli elenchi a discesa nell'aggiornamento, inserimento, eliminare le schede su (nessuno) e quindi fare clic sul pulsante Avanti. La procedura guidata Configura origine dati viene ora richiesto per le origini del `GetProductsPaged` metodo s *startRowIndex* e *maximumRows* parametri di input. In realtà, questi parametri di input vengono ignorati. Al contrario, il *startRowIndex* e *maximumRows* valori verranno passati nel `Arguments` proprietà in s ObjectDataSource `Selecting` gestore eventi, come modo in cui è stato specificato il *sortExpression* in questa demo prima esercitazione s. Pertanto, lasciare l'origine del parametro elenchi a discesa nella procedura guidata impostato su None.


[![Lasciare il Set di origini di parametri su Nessuno](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Figura 13**: lasciare le origini di parametro impostato su None ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Effettuare *non* impostare la s ObjectDataSource `EnablePaging` proprietà `true`. In questo modo ObjectDataSource includere automaticamente il proprio *startRowIndex* e *maximumRows* parametri per il `SelectMethod` elenco parametri esistenti s. Il `EnablePaging` proprietà è utile quando l'associazione personalizzata di paging dei dati a un controllo GridView, DetailsView e FormView perché questi controlli prevedono determinati comportamenti da ObjectDataSource che s è disponibile solo quando `EnablePaging` è di proprietà `true`. Poiché è necessario aggiungere manualmente il supporto di paging per DataList e Repeater, lasciare impostata questa proprietà su `false` (impostazione predefinita), come verrà fanno parte della funzionalità necessarie direttamente all'interno di nostra pagina ASP.NET.


Infine, definire il Repeater s `ItemTemplate` in modo che vengano visualizzati il nome del prodotto s, categoria e fornitore. Dopo tali modifiche, la sintassi dichiarativa s Repeater e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Si consiglia di visitare la pagina tramite un browser e notare che viene restituito alcun record. Infatti è ve ancora per specificare il *startRowIndex* e *maximumRows* i valori dei parametri; pertanto, vengono passati i valori 0 per entrambi. Per specificare questi valori, creare un gestore eventi per gli oggetti ObjectDataSource `Selecting` eventi e impostare questi parametri di valori a livello di codice ai valori impostati come hardcoded pari a 0 e 5, rispettivamente:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Con questa modifica, la pagina, quando viene visualizzato tramite un browser, Mostra i primi cinque prodotti.


[![Vengono visualizzati i primi cinque record](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Figura 14**: vengono visualizzati il primo di cinque record ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> I prodotti elencati nella figura 14 si verificano in base a nome del prodotto perché il `GetProductsPaged` stored procedure che esegue la query di paging personalizzata efficiente Ordina i risultati in base `ProductName`.


Per consentire all'utente di scorrere le pagine, è necessario tenere traccia dell'indice di riga iniziale e il numero massimo di righe e ricordare questi valori durante i postback. Nell'esempio di paging predefinito è stato usato campi querystring in modo permanente di tali valori. per questa demo, consentire s mantenere queste informazioni nello stato di visualizzazione pagina s. Creare le due proprietà seguenti:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Successivamente, aggiornare il codice nel gestore dell'evento quando si seleziona in modo che utilizzi il `StartRowIndex` e `MaximumRows` proprietà anziché i valori hardcoded pari a 0 e 5:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

A questo punto la pagina Mostra ancora solo i primi cinque record. Tuttavia, con queste proprietà sul posto, sono pronti per creare l'interfaccia di paging.

## <a name="adding-the-paging-interface"></a>Aggiunta dell'interfaccia di Paging

"Let" uso il primo stesso, precedente, successivo, paging ultima interfaccia utilizzata per l'esempio di paging predefinito, tra cui Web etichetta controllo che visualizza la pagina di dati viene visualizzato e il numero totale di pagine esistano. Aggiungere i quattro controlli pulsante Web ed etichetta sotto il controllo Repeater.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

A questo punto, creare `Click` gestori eventi per i quattro pulsanti. Quando viene scelto uno di questi pulsanti, è necessario aggiornare il `StartRowIndex` riassociare i dati al controllo Repeater. Il codice per i pulsanti prima, indietro e Avanti è abbastanza semplice, ma per l'ultimo pulsante come si determina l'indice di riga iniziale per l'ultima pagina di dati? Per calcolare l'indice, nonché la possibilità di determinare che se i pulsanti Avanti e ultima devono essere abilitati è necessario conoscere il numero di record in totale è il paging attraverso. È possibile determinare questo chiamando il `ProductsBLL` classe s `TotalNumberOfProducts()` (metodo). S ti permettono di creare una proprietà di sola lettura, a livello di pagina denominata `TotalRowCount` che restituisce i risultati del `TotalNumberOfProducts()` metodo:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Con questa proprietà è ora possibile determinare l'ultimo indice di riga di inizio pagina s. In particolare, è s il risultato dell'intero del `TotalRowCount` meno 1 diviso `MaximumRows`moltiplicato per `MaximumRows`. È possibile scrivere il `Click` gestori eventi per i quattro pulsanti di interfaccia di paging:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Infine, è necessario disabilitare i pulsanti del primo e precedente nell'interfaccia di paging, quando si visualizzano la prima pagina di dati e i pulsanti Avanti e ultimi quando si visualizza l'ultima pagina. A tale scopo, aggiungere il codice seguente al s ObjectDataSource `Selecting` gestore dell'evento:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Dopo aver aggiunto queste `Click` gestori eventi e il codice per abilitare o disabilitare gli elementi di interfaccia di paging in base all'indice di riga iniziale corrente, la pagina di test in un browser. Come illustrato nella figura 15, durante la prima visita la pagina prima e i pulsanti Indietro verranno sono disabilitati. Selezione di Avanti Mostra la seconda pagina di dati, mentre facendo clic su ultima verrà visualizzata la pagina finale (vedere figure 16 e 17). Quando si visualizza l'ultima pagina di dati sono disabilitati i pulsanti Avanti sia l'ultimo.


[![I pulsanti Indietro e ultimo sono disabilitati durante la visualizzazione di pagina prima dei prodotti](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Figura 15**: le precedenti e i pulsanti ultimo sono disabilitati durante la visualizzazione di pagina prima di prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![La seconda pagina di prodotti sono applicato all'intero](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Figura 16**: pagina secondo dei prodotti sono applicato all'intero ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Facendo clic su ultima consente di visualizzare la pagina finale dei dati](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Figura 17**: fare clic su ultima visualizzare la pagina di dati finale ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Passaggio 7: Includa l'ordinamento di supporto con l'oggetto personalizzato del pool di paging Repeater

Ora che è stato implementato il paging personalizzato, sono pronti per includere l'ordinamento supportati. Il `ProductsBLL` classe s `GetProductsPagedAndSorted` metodo ha lo stesso *startRowIndex* e *maximumRows* come parametri di input `GetProductsPaged`, ma consente un'ulteriore  *sortExpression* parametro di input. Usare il `GetProductsPagedAndSorted` metodo `SortingWithCustomPaging.aspx`, è necessario eseguire la procedura seguente:

1. Modificare gli oggetti ObjectDataSource `SelectMethod` proprietà dal `GetProductsPaged` a `GetProductsPagedAndSorted`.
2. Aggiungere un *sortExpression* `Parameter` oggetto ObjectDataSource s `SelectParameters` raccolta.
3. Creare una privata, a livello di pagina `SortExpression` proprietà che rende persistente il valore di postback tramite lo stato di visualizzazione pagina s.
4. Aggiornare gli oggetti ObjectDataSource `Selecting` gestore dell'evento per assegnare gli oggetti ObjectDataSource *sortExpression* parametro il valore del livello di pagina `SortExpression` proprietà.
5. Creare l'interfaccia di ordinamento.

Per iniziare, aggiornare gli oggetti ObjectDataSource `SelectMethod` proprietà e l'aggiunta di un *sortExpression* `Parameter`. Assicurarsi che il *sortExpression* `Parameter` s `Type` viene impostata su `String`. Dopo aver completato le prime due attività, il markup dichiarativo di ObjectDataSource s dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Successivamente, è necessario un livello di pagina `SortExpression` proprietà il cui valore viene serializzato in stato di visualizzazione. Se non è stato impostato alcun valore di espressione di ordinamento, usare ProductName come valore predefinito:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Prima di ObjectDataSource richiama il `GetProductsPagedAndSorted` metodo è necessario impostare la *sortExpression* `Parameter` al valore della `SortExpression` proprietà. Nel `Selecting` gestore dell'evento, aggiungere la riga di codice seguente:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Tutto resta che implementano l'interfaccia di ordinamento. Come è stato fatto nell'ultimo esempio, consentire s hanno l'ordinamento interfaccia implementata usando tre controlli pulsante Web che consentono all'utente di ordinare i risultati dal nome del prodotto, categoria o fornitore.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Creare `Click` gestori eventi per questi tre controlli pulsante. Nell'evento gestore, reimpostare il `StartRowIndex` su 0, viene impostata la `SortExpression` sul valore appropriato, e i dati al controllo Repeater riassociazione:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Che s ne è a esso. Mentre si sono verificati alcuni passaggi per ottenere il paging personalizzato e l'ordinamento implementato, i passaggi sono molto simili a quelli necessari per il paging predefinito. Figura 18 Mostra i prodotti quando si visualizza l'ultima pagina di dati quando ordinato in base alla categoria.


[![Viene visualizzata l'ultima pagina di dati, ordinato in base alla categoria,](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Figura 18**: viene visualizzata l'ultima pagina di dati, ordinato in base alla categoria, ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> Negli esempi precedenti, durante l'ordinamento dal fornitore che suppliername è stato usato come espressione di ordinamento. Tuttavia, per l'implementazione di paging personalizzata, è necessario usare CompanyName. Infatti, la stored procedure responsabile dell'implementazione di paging personalizzato `GetProductsPagedAndSorted` passa l'espressione di ordinamento nel `ROW_NUMBER()` (parola chiave), il `ROW_NUMBER()` (parola chiave) richiede il nome di colonna effettivi anziché un alias. Pertanto, è necessario usare `CompanyName` (il nome della colonna nella `Suppliers` tabella) piuttosto che l'alias usato nel `SELECT` query (`SupplierName`) per l'espressione di ordinamento.


## <a name="summary"></a>Riepilogo

Né il DataList o Repeater offrono il supporto dell'ordinamento predefinito, ma con un po' di codice e un'interfaccia di ordinamento personalizzata, è possibile aggiungere tale funzionalità. Quando si implementa l'ordinamento, ma non di paging, l'espressione di ordinamento può essere specificata tramite il `DataSourceSelectArguments` oggetto passato in oggetti ObjectDataSource `Select` (metodo). Ciò `DataSourceSelectArguments` oggetto s' `SortExpression` proprietà può essere assegnata in s ObjectDataSource `Selecting` gestore dell'evento.

Per aggiungere funzionalità di ordinamento per un controllo DataList o Repeater che fornisce già il supporto di paging, l'approccio più semplice consiste nel personalizzare il livello di logica di Business in modo da includere un metodo che accetta un'espressione di ordinamento. Queste informazioni possono quindi essere passate nel tramite un parametro in s ObjectDataSource `SelectParameters`.

Questa esercitazione si completa l'esame del paging e ordinamento con DataList e Repeater. L'esercitazione successiva e finale verrà esaminate come aggiungere controlli pulsante Web per i modelli di s DataList e Repeater per offrire alcune funzionalità personalizzata, avviata dall'utente su una base per ogni elemento.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata David Suru. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [Successivo](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
