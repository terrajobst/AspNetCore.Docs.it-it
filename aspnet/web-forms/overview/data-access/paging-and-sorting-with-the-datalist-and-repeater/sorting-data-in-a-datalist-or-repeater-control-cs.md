---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Ordinamento dei dati in un controllo DataList o Repeater (c#) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione si esamineranno come includere il supporto in DataList e Repeater ordinamento, nonché come costruire un controllo DataList o Repeater possono i cui dati..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f7ab0df2ebfa24b0928117e683325b158e3aad1c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Ordinamento dei dati in un controllo DataList o Repeater (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) o [Scarica il PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> In questa esercitazione si esamineranno come includere il supporto in DataList e Repeater ordinamento, nonché come costruire un DataList o Repeater i cui dati possono essere di paging e ordinati.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](paging-report-data-in-a-datalist-or-repeater-control-cs.md) abbiamo esaminato come aggiungere il supporto del paging a un controllo DataList. È stato creato un nuovo metodo nella `ProductsBLL` classe (`GetProductsAsPagedDataSource`) che ha restituito un `PagedDataSource` oggetto. Quando è associato a un controllo DataList o Repeater, DataList o Repeater Visualizza la pagina di dati richiesta. Questa tecnica è simile a quella utilizzata internamente dai controlli GridView, DetailsView e FormView per fornire la funzionalità di paging predefinite.

Oltre a offrire il supporto del paging, GridView include anche il supporto per l'ordinamento viene fornita. DataList né Ripetitore fornisce funzionalità di ordinamento predefinita; Tuttavia, funzionalità di ordinamento possono essere aggiunti con un bit di codice. In questa esercitazione si esamineranno come includere il supporto in DataList e Repeater ordinamento, nonché come costruire un DataList o Repeater i cui dati possono essere di paging e ordinati.

## <a name="a-review-of-sorting"></a>Una revisione dell'ordinamento

Come illustrato nel [Paging e ordinamento dei dati del Report](../paging-and-sorting/paging-and-sorting-report-data-cs.md) esercitazione, il controllo GridView fornisce il supporto per l'ordinamento viene fornita. Ogni campo di GridView può avere un oggetto associato `SortExpression`, che indica il campo dei dati da cui si desidera ordinare i dati. Quando i dispositivi di GridView `AllowSorting` è impostata su `true`, ogni campo di GridView con un `SortExpression` valore di proprietà contiene l'intestazione sottoposto a rendering come LinkButton. Quando un utente fa clic su una determinata intestazione s campo di GridView, si verifica un postback e i dati vengono ordinati in base al campo selezionato s `SortExpression`.

Il controllo GridView ha un `SortExpression` anche la proprietà, che archivia il `SortExpression` del campo GridView vengono ordinati i dati. Inoltre, un `SortDirection` proprietà indica se i dati devono essere elencati in ordine crescente o decrescente (se un utente fa clic su un collegamento di intestazione di campo s GridView particolare due volte in successione, l'ordinamento è attivato o disattivato).

Quando il controllo GridView viene associato al relativo controllo origine dati, restituita la `SortExpression` e `SortDirection` proprietà ai dati di controllo del codice sorgente. Recupera i dati di controllo origine dati e quindi la Ordina in base alle fornito `SortExpression` e `SortDirection` proprietà. Dopo l'ordinamento dei dati, il controllo origine dati restituisce a GridView.

Per replicare questa funzionalità con i controlli DataList o Repeater, è necessario:

- Creare un'interfaccia di ordinamento
- Tenere presente il campo dei dati per ordinare in base e se eseguire l'ordinamento in ordine crescente o decrescente
- Indicare ObjectDataSource per ordinare i dati da un determinato campo di dati

Che verranno affrontati queste tre attività nei passaggi 3 e 4. Successivamente, esamineremo come includere sia ordinamento e paging supporto in un controllo DataList o Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Passaggio 2: Visualizzare i prodotti in un ripetitore

Prima di preoccupazione che implementa la funzionalità correlate a ordinamento, consentire s avviare elencando i prodotti in un controllo Repeater. Aprire il `Sorting.aspx` nella pagina di `PagingSortingDataListRepeater` cartella. Aggiungere un controllo Repeater alla pagina web, l'impostazione relativa `ID` proprietà `SortableProducts`. Lo smart tag Ripetitore s, creare un nuovo oggetto ObjectDataSource denominato `ProductsDataSource` e configurarlo per recuperare i dati di `ProductsBLL` classe s `GetProducts()` (metodo). Selezionare il che opzione (nessuno) dagli elenchi a discesa nelle schede delle INSERT, UPDATE e DELETE.


[![Creare un ObjectDataSource e configurarlo per utilizzare il metodo GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Figura 1**: creare un ObjectDataSource e configurarlo per utilizzare il `GetProductsAsPagedDataSource()` metodo ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare le schede su (nessuno)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Figura 2**: impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare le schede su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


A differenza con DataList, Visual Studio non crea automaticamente un `ItemTemplate` per il controllo Repeater dopo l'associazione a un'origine dati. Inoltre, è necessario aggiungere questo `ItemTemplate` in modo dichiarativo, come lo smart tag di controllo s Ripetitore manca l'opzione di modifica modelli trovato in DataList s. S consentono di utilizzare lo stesso `ItemTemplate` dall'esercitazione precedente, che visualizzare il nome del prodotto s, fornitore e categoria.

Dopo aver aggiunto il `ItemTemplate`, Repeater e ObjectDataSource s dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Figura 3 Mostra questa pagina quando viene visualizzato tramite un browser.


[![Viene visualizzato ogni s nome, fornitore e la categoria di prodotto](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Figura 3**: ogni prodotto s, nome, fornitore e la categoria viene visualizzata ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Passaggio 3: Istruzioni ObjectDataSource per ordinare i dati

Per ordinare i dati visualizzati nel Repeater, è necessario informare ObjectDataSource dell'espressione di ordinamento con cui i dati devono essere ordinati. Prima di ObjectDataSource recupera i dati, viene generato prima di tutto il [ `Selecting` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), che consente di specificare un'espressione di ordinamento. Il `Selecting` gestore eventi viene passato un oggetto di tipo [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), che include una proprietà denominata [ `Arguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) di tipo [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.aspx). Il `DataSourceSelectArguments` è progettato per passare le richieste correlate ai dati da un consumer di dati per il controllo origine dati, classe e include un [ `SortExpression` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Per passare informazioni sull'ordinamento dalla pagina ASP.NET per ObjectDataSource, creare un gestore eventi per il `Selecting` eventi e utilizzare il codice seguente:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

Il *sortExpression* valore deve essere assegnato il nome del campo di dati per ordinare i dati (ad esempio ProductName). Nessuna proprietà relative alla direzione di ordinamento, pertanto se si desidera ordinare i dati in ordine decrescente, aggiungere la stringa DESC per il *sortExpression* valore (ad esempio ProductName DESC).

Vado avanti e provare alcuni valori diversi a livello di codice per *sortExpression* e testare i risultati in un browser. Come mostrato nella figura 4, quando si usa DESC ProductName come il *sortExpression*, i prodotti vengono ordinati in base al nome in ordine alfabetico inverso.


[![I prodotti vengono ordinati in base al nome in ordine alfabetico inverso](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Figura 4**: il prodotti vengono ordinati in base al nome in ordine alfabetico inverso ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Passaggio 4: Creazione dell'interfaccia di ordinamento e ricordare l'espressione di ordinamento e la direzione

Accensione di ordinamento di supporto in GridView converte ogni testo dell'intestazione campo ordinabile s in LinkButton che, quando si fa clic, Ordina i dati di conseguenza. Questo tipo è un'interfaccia di ordinamento è significativa per GridView, in cui i dati ben disposti in colonne. Per i controlli DataList e Repeater, tuttavia, è necessaria un'interfaccia di ordinamento diversa. Un'interfaccia comune di ordinamento per un elenco di dati (a differenza di una griglia di dati), è un elenco di riepilogo a discesa che fornisce i campi da cui i dati possono essere ordinati. Consente di implementare tale interfaccia per questa esercitazione s.

Aggiungere un controllo DropDownList Web sopra il `SortableProducts` Repeater e set relativo `ID` proprietà `SortBy`. Dalla finestra delle proprietà, fare clic sui puntini di sospensione nel `Items` proprietà per visualizzare Editor raccolta di ListItem. Aggiungere `ListItem` s per ordinare i dati per il `ProductName`, `CategoryName`, e `SupplierName` campi. Aggiungere anche un `ListItem` per ordinare i prodotti in base al nome in ordine alfabetico inverso.

Il `ListItem` `Text` proprietà possono essere impostate su qualsiasi valore (ad esempio nome), ma la `Value` devono essere impostate per il nome del campo dati (ad esempio ProductName). Per ordinare i risultati in ordine decrescente, aggiungere la stringa DESC al nome del campo dati, ad esempio ProductName DESC.


![Aggiungere un elemento ListItem per ognuno dei campi dati ordinabile](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Figura 5**: aggiungere un `ListItem` per ognuno dei campi dati ordinabile


Infine, aggiungere un controllo Web pulsante a destra di DropDownList. Impostare il relativo `ID` a `RefreshRepeater` e il relativo `Text` proprietà per l'aggiornamento.

Dopo aver creato il `ListItem` s e aggiungere il pulsante di aggiornamento, la sintassi dichiarativa s DropDownList e pulsante dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Con ordinamento DropDownList completato, è quindi necessario aggiornare s ObjectDataSource `Selecting` in modo che utilizzi il gestore dell'evento `SortBy``ListItem` s `Value` proprietà anziché un'espressione di ordinamento a livello di codice.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

A questo punto, durante la prima visita la pagina prodotti saranno inizialmente ordinati in base il `ProductName` campo dei dati, come s il `SortBy` `ListItem` selezionata per impostazione predefinita (vedere Figura 6). Selezionare una diversa opzione come categoria di ordinamento e fare clic su Aggiorna causerà un postback e riordinare i dati dal nome di categoria, come illustrato nella figura 7.


[![I prodotti sono inizialmente ordinati in base al nome](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Figura 6**: il prodotti sono inizialmente ordinati in base al nome ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![I prodotti sono ora ordinati per categoria](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Figura 7**: il prodotti sono ora ordinati per categoria ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Fare clic sul pulsante di aggiornamento, i dati di automaticamente nuovamente ordinato perché lo stato di visualizzazione Ripetitore s è stato disabilitato, causando Ripetitore riassociazione all'origine dati a ogni postback. Se è già lasciato Ripetitore s dello stato di visualizzazione abilitato, modificare l'ordinamento di elenco a discesa elenco vinto t ha alcun effetto sull'ordinamento. Per risolvere questo problema, creare un gestore eventi per s pulsante Aggiorna `Click` riassociazione Ripetitore all'origine dati e evento (chiamando Ripetitore s `DataBind()` (metodo)).


## <a name="remembering-the-sort-expression-and-direction"></a>Ricordare l'espressione di ordinamento e la direzione

Quando si crea un ordinabile DataList o Repeater in una pagina in cui non ordinamento correlati i postback si verifichi, è imperativo s memorizzato l'espressione di ordinamento e la direzione durante i postback. Si supponga, ad esempio, che è stato aggiornato Repeater in questa esercitazione per includere un pulsante Elimina con ogni prodotto. Quando l'utente fa clic sul pulsante Elimina è d eseguire il codice per eliminare il prodotto selezionato e quindi riassociare i dati al controllo Repeater. Se i dettagli di ordinamento non vengono mantenuti durante il postback, i dati visualizzati sullo schermo verranno ripristinato l'ordine originale.

Per questa esercitazione, DropDownList in modo implicito Salva l'ordinamento espressione e la direzione in stato di visualizzazione per noi. Se utilizzato con, LinkButton ad esempio, per le varie opzioni di ordinamento fornita un'interfaccia di ordinamento diversa uno d è necessario prestare attenzione per ricordare all'interno dell'ordinamento durante i postback. Ad esempio archiviando i parametri di ordinamento nello stato di visualizzazione pagina s, includendo il parametro di ordinamento nella stringa di query o tramite un'altra tecnica di persistenza dello stato.

Esempi future di questa esercitazione imparare a rendere persistenti i dettagli dell'ordinamento nello stato di visualizzazione pagina s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Passaggio 5: Aggiunta di supporto per l'ordinamento a un controllo DataList che utilizza il Paging predefinito

Nel [esercitazione precedente](paging-report-data-in-a-datalist-or-repeater-control-cs.md) abbiamo esaminato come implementare il paging predefinito con un controllo DataList. Consente di estendere l'esempio precedente per includere la possibilità di ordinare i dati di paging s. Aprire il `SortingWithDefaultPaging.aspx` e `Paging.aspx` di pagine nel `PagingSortingDataListRepeater` cartella. Dal `Paging.aspx` pagina, fare clic sul pulsante di origine per visualizzare il markup dichiarativo pagina s. Copiare il testo selezionato (vedere la figura 8) e incollarlo nel markup dichiarativo di `SortingWithDefaultPaging.aspx` tra il `<asp:Content>` tag.


[![Replicare il Markup dichiarativo di &lt;asp: Content&gt; tag da paging. aspx a SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Figura 8**: replicare il Markup dichiarativo di `<asp:Content>` tag da `Paging.aspx` a `SortingWithDefaultPaging.aspx` ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


Dopo aver copiato il markup dichiarativo, copiare i metodi e le proprietà di `Paging.aspx` pagina classe code-behind s per la classe code-behind per `SortingWithDefaultPaging.aspx`. Successivamente, è opportuno visualizzare il `SortingWithDefaultPaging.aspx` pagina in un browser. Deve presentare la stessa funzionalità e aspetto `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Miglioramento della ProductsBLL per includere un valore predefinito di metodo di ordinamento e Paging

Nell'esercitazione precedente è stato creato un `GetProductsAsPagedDataSource(pageIndex, pageSize)` metodo il `ProductsBLL` classe che ha restituito un `PagedDataSource` oggetto. Questo `PagedDataSource` oggetto è stato popolato con *tutti* dei prodotti (tramite s BLL `GetProducts()` metodo), ma quando è associato a DataList solo i record corrispondente al valore *pageIndex* e *pageSize* sono stati visualizzati i parametri di input.

In precedenza in questa esercitazione è stato aggiunto il supporto dell'ordinamento specificando l'espressione di ordinamento da ObjectDataSource s `Selecting` gestore dell'evento. Questo processo funziona bene quando ObjectDataSource viene restituito un oggetto che è possibile ordinare, ad esempio il `ProductsDataTable` restituito dal `GetProducts()` metodo. Tuttavia, il `PagedDataSource` oggetto restituito dal `GetProductsAsPagedDataSource` (metodo) non supporta l'ordinamento dell'origine dati interna. In alternativa, è necessario ordinare i risultati restituiti dal `GetProducts()` (metodo) *prima* è stato inserito il `PagedDataSource`.

A tale scopo, creare un nuovo metodo nella `ProductsBLL` (classe), `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Per ordinare il `ProductsDataTable` restituito dal `GetProducts()` (metodo), specificare il `Sort` proprietà del valore predefinito `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Il `GetProductsSortedAsPagedDataSource` metodo differisce solo leggermente dal `GetProductsAsPagedDataSource` metodo creato nell'esercitazione precedente. In particolare, `GetProductsSortedAsPagedDataSource` accetta un parametro di input aggiuntivo `sortExpression` e assegna questo valore per il `Sort` proprietà del `ProductDataTable` s `DefaultView`. Alcune righe di codice in un secondo momento, il `PagedDataSource` oggetto DataSource s viene assegnato il `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>La chiamata al metodo GetProductsSortedAsPagedDataSource e specificando il valore per il parametro di Input SortExpression

Con il `GetProductsSortedAsPagedDataSource` metodo completo, il passaggio successivo consiste nel fornire il valore per questo parametro. ObjectDataSource in `SortingWithDefaultPaging.aspx` è attualmente configurato per chiamare il `GetProductsAsPagedDataSource` metodo e passa due parametri di input tramite i relativi due `QueryStringParameters`, che vengono specificate nel `SelectParameters` insieme. I due `QueryStringParameters` indicano che l'origine per il `GetProductsAsPagedDataSource` metodo s *pageIndex* e *pageSize* parametri provengono dai campi querystring `pageIndex` e `pageSize`.

Aggiornare i dispositivi ObjectDataSource `SelectMethod` proprietà in modo che richiama il nuovo `GetProductsSortedAsPagedDataSource` metodo. Quindi, aggiungere un nuovo `QueryStringParameter` in modo che il *sortExpression* parametro di input è accessibile dal campo querystring `sortExpression`. Impostare il `QueryStringParameter` s `DefaultValue` a ProductName.

Dopo aver apportato queste modifiche, il markup dichiarativo s ObjectDataSource dovrebbe essere simile:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

A questo punto, il `SortingWithDefaultPaging.aspx` pagina permette di ordinare i risultati in ordine alfabetico in base il nome del prodotto (vedere Figura 9). Questo avviene perché, per impostazione predefinita, viene passato un valore di ProductName come il `GetProductsSortedAsPagedDataSource` metodo s *sortExpression* parametro.


[![Per impostazione predefinita, i risultati vengono ordinati in base ProductName](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Figura 9**: per impostazione predefinita, i risultati vengono ordinati in base `ProductName` ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Se si aggiunge manualmente un `sortExpression` campo querystring, ad esempio `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` verranno ordinati i risultati dall'oggetto `sortExpression`. Tuttavia, questo `sortExpression` parametro non è incluso nella stringa di query quando si spostano in un'altra pagina di dati. In effetti, facendo clic nella pagina successiva o ultima pulsanti visualizzata ci `Paging.aspx`! Inoltre, s non esiste attualmente alcun ordinamento non interfaccia. L'unico modo di che un utente può modificare l'ordinamento dei dati di paging è modificando la stringa di query direttamente.

## <a name="creating-the-sorting-interface"></a>La creazione dell'interfaccia di ordinamento

È necessario innanzitutto aggiornare il `RedirectUser` metodo per inviare all'utente di `SortingWithDefaultPaging.aspx` (anziché `Paging.aspx`) e includere il `sortExpression` valore nella stringa di query. Va inoltre aggiunto una sola lettura, a livello di pagina denominata `SortExpression` proprietà. Questa proprietà, come il `PageIndex` e `PageSize` proprietà creata nell'esercitazione precedente, restituisce il valore della `sortExpression` campo querystring se esiste e il valore predefinito (ProductName) in caso contrario.

Attualmente il `RedirectUser` metodo accetta solo un singolo parametro di input l'indice della pagina da visualizzare. Tuttavia, potrebbero esserci orari in cui si desidera reindirizzare l'utente a una particolare pagina di dati utilizzando un'espressione di ordinamento diverso da novità specificato nella stringa di query. In un momento in cui si creerà l'interfaccia di ordinamento per questa pagina, che include una serie di controlli Web di pulsante per l'ordinamento dei dati da una colonna specificata. Quando viene scelto uno di questi pulsanti, di voler reindirizzerà l'utente passa il valore di espressione di ordinamento appropriato. Per fornire questa funzionalità, creare due versioni di `RedirectUser` metodo. Il primo deve accettare solo l'indice della pagina per visualizzare, mentre il secondo accetta l'espressione di ordinamento e di indice di pagina.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

Nel primo esempio in questa esercitazione, è creata un'interfaccia ordinamento utilizzando un DropDownList. Per questo esempio, s consentono di utilizzare tre controlli pulsante Web posizionati di sopra di DataList uno per l'ordinamento da `ProductName`, uno per `CategoryName`e uno per `SupplierName`. Aggiungere i tre controlli Web Button, l'impostazione loro `ID` e `Text` proprietà in modo appropriato:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Creare quindi un `Click` per ogni gestore dell'evento. I gestori eventi devono chiamare il `RedirectUser` metodo, restituendo l'utente alla prima pagina utilizzando l'espressione di ordinamento appropriato.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Durante la prima visita la pagina, i dati sono ordinati in ordine alfabetico dal nome del prodotto (vedere la figura 9). Fare clic sul pulsante Avanti per passare alla seconda pagina di dati e quindi fare clic sull'ordinamento dal pulsante di categoria. In questo ci riporta alla prima pagina di dati, ordinati in base al nome di categoria (vedere la figura 10). Analogamente, fare clic sul tipo di ordinamento dal pulsante fornitore Ordina i dati da fornitore a partire dalla prima pagina di dati. La scelta di ordinamento viene memorizzata come viene eseguito il paging dei dati tramite. Figura 11 mostra la pagina dopo l'ordinamento per categoria e quindi procedendo al tredicesima pagina di dati.


[![I prodotti vengono ordinati per categoria](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Figura 10**: il prodotti vengono ordinati per categoria ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![L'espressione di ordinamento è memorizzate quando il Paging tramite dei dati](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Figura 11**: l'espressione di ordinamento è memorizzate quando il Paging tramite dei dati ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Passaggio 6: Il Paging personalizzato tra i record in un ripetitore

Nell'esempio di DataList esaminato nel passaggio 5 pagine nei relativi dati utilizzando la tecnica di paging predefinito inefficiente. Quando il paging sufficientemente grandi quantità di dati, è fondamentale utilizzato il paging personalizzato. Nel [in modo efficiente il Paging tramite quantità di dati di grandi dimensioni](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) e [ordinamento dei dati di paging personalizzata](../paging-and-sorting/sorting-custom-paged-data-cs.md) esercitazioni, sono state esaminate le differenze tra l'impostazione predefinita e il paging personalizzato e creato i metodi in BLL per utilizzando il paging personalizzato e ordinamento dei dati di paging personalizzati. In particolare, in queste due esercitazioni precedenti abbiamo aggiunto i tre metodi seguenti per la `ProductsBLL` classe:

- `GetProductsPaged(startRowIndex, maximumRows)`Restituisce un sottoinsieme specifico di record a partire da *startRowIndex* e non superiore a *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`Restituisce un sottoinsieme specifico di record ordinati specificato *sortExpression* parametro di input.
- `TotalNumberOfProducts()`fornisce il numero totale di record di `Products` tabella di database.

Questi metodi possono essere utilizzati per pagina in modo efficiente e ordinare i dati di utilizzo di un controllo DataList o Repeater. Per illustrare questo concetto, consentire s inizia creando un controllo Repeater con supporto di paging personalizzato; funzionalità di ordinamento verrà quindi aggiunto.

Aprire il `SortingWithCustomPaging.aspx` nella pagina di `PagingSortingDataListRepeater` cartella e aggiungere un controllo Repeater alla pagina, l'impostazione relativa `ID` proprietà `Products`. Lo smart tag Ripetitore s, creare un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurarlo per selezionare i dati di `ProductsBLL` classe s `GetProductsPaged` metodo.


[![Configurare ObjectDataSource per utilizzare il metodo di classe ProductsBLL s GetProductsPaged](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Figura 12**: configurare ObjectDataSource per utilizzare il `ProductsBLL` classe s `GetProductsPaged` metodo ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Impostare gli elenchi a discesa nell'aggiornamento, inserimento, eliminare le tabulazioni su (nessuno) e quindi fare clic sul pulsante Avanti. La configurazione guidata origine dati è ora richiesto per le origini del `GetProductsPaged` metodo s *startRowIndex* e *maximumRows* i parametri di input. In realtà, questi parametri di input vengono ignorati. Al contrario, il *startRowIndex* e *maximumRows* valori verranno trasmesse nel `Arguments` proprietà in s ObjectDataSource `Selecting` gestore dell'evento, analogamente a come è specificato il *sortExpression* in questa demo prima esercitazione s. Pertanto, lasciare l'origine del parametro elenchi a discesa nella procedura guidata impostato su Nessuno.


[![Lasciare il Set di origini di parametri su Nessuno](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Figura 13**: lasciare le origini di parametro impostato su None ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Eseguire *non* impostare s ObjectDataSource `EnablePaging` proprietà `true`. In questo modo ObjectDataSource includere automaticamente il proprio *startRowIndex* e *maximumRows* parametri per il `SelectMethod` elenco parametri esistenti s. Il `EnablePaging` proprietà è utile quando l'associazione personalizzata di paging dei dati a un controllo GridView, DetailsView o FormView perché questi controlli prevedono determinati comportamenti da ObjectDataSource che s è disponibile solo quando `EnablePaging` proprietà `true`. Poiché è necessario aggiungere manualmente il supporto di paging per il DataList e Repeater, lasciare questa proprietà è impostata su `false` (impostazione predefinita), come si sarà dolci nelle funzionalità necessarie direttamente all'interno di nostra pagina ASP.NET.


Definire infine Ripetitore s `ItemTemplate` in modo che vengono visualizzati il nome del prodotto s, categoria e fornitore. Dopo aver apportato queste modifiche, la sintassi dichiarativa s Repeater e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Richiedere qualche istante per visitare la pagina tramite un browser e notare che viene restituito alcun record. Infatti è ve ancora per specificare il *startRowIndex* e *maximumRows* i valori dei parametri; pertanto, vengono passati valori pari a 0 in per entrambi. Per specificare questi valori, creare un gestore eventi per ObjectDataSource s `Selecting` evento e impostare questi parametri valori a livello di codice per i valori hardcoded pari a 0 e 5, rispettivamente:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Con questa modifica, la pagina, quando viene visualizzato tramite un browser, Mostra i primi cinque prodotti.


[![Vengono visualizzati i primi cinque record](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Nella figura 14**: vengono visualizzati il primo di cinque record ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> I prodotti elencati nella figura 14 risultano essere ordinati in base al nome del prodotto perché il `GetProductsPaged` stored procedure che esegue la query di paging personalizzata efficiente Ordina i risultati in base `ProductName`.


Per consentire all'utente di scorrere le pagine, è necessario tenere traccia dell'indice di riga iniziale e il numero massimo di righe e ricordare questi valori durante i postback. Nell'esempio di paging predefinito è utilizzato i campi querystring per mantenere questi valori; per questa dimostrazione, consentire s mantenere queste informazioni nello stato di visualizzazione pagina s. Creare le due proprietà seguenti:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Aggiornare quindi il codice nel gestore eventi se si seleziona in modo che utilizzi il `StartRowIndex` e `MaximumRows` proprietà anziché i valori hardcoded pari a 0 e 5:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

A questo punto, la pagina viene ancora solo i primi cinque record. Tuttavia, con queste proprietà sul posto, è nuovamente pronto per creare l'interfaccia di paging.

## <a name="adding-the-paging-interface"></a>Aggiunta dell'interfaccia di Paging

S consentono il primo stesso, precedente, successivo, ultimo paging interfaccia utilizzato nell'esempio paging predefinito, inclusi Web etichetta controllo che visualizza la pagina di dati viene visualizzato e il numero totale di pagine esistano. Aggiungere le quattro controlli pulsante Web e l'etichetta sotto il controllo Repeater.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Successivamente, creare `Click` gestori eventi per i quattro pulsanti. Quando viene scelto uno di questi pulsanti, è necessario aggiornare il `StartRowIndex` riassociare i dati al controllo Repeater. Il codice per i pulsanti First, indietro e Avanti è abbastanza semplice, ma per l'ultimo pulsante come si determina l'indice di riga iniziale per l'ultima pagina di dati? Per calcolare l'indice, nonché la possibilità di determinare che se i pulsanti Avanti e ultima devono essere abilitati è necessario conoscere il numero di record in totale è il paging tramite. È possibile determinare questo chiamando il `ProductsBLL` classe s `TotalNumberOfProducts()` metodo. S consentono di creare una proprietà di sola lettura, a livello di pagina denominata `TotalRowCount` che restituisce i risultati del `TotalNumberOfProducts()` metodo:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Con questa proprietà è ora possibile determinare l'indice di riga iniziale pagina s ultimo. In particolare, è s il risultato di tipo integer del `TotalRowCount` meno 1 diviso `MaximumRows`moltiplicato per `MaximumRows`. È ora possibile scrivere il `Click` gestori eventi per i quattro pulsanti di interfaccia di paging:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Infine, è necessario disabilitare i pulsanti del primo e precedente nell'interfaccia di paging quando si visualizzano la prima pagina di dati e i pulsanti Avanti e ultimi quando si visualizza l'ultima pagina. A tale scopo, aggiungere il codice seguente al s ObjectDataSource `Selecting` gestore eventi:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Dopo aver aggiunto queste `Click` i gestori eventi e il codice per abilitare o disabilitare gli elementi dell'interfaccia di paging in base all'indice di riga iniziale corrente, la pagina di prova in un browser. Come illustrato nella figura 15, durante la prima visita la pagina prima e pulsanti precedente verranno disabilitati. Fare clic su Avanti viene illustrata la seconda pagina di dati, mentre facendo clic su ultima visualizzata la pagina finale (vedere 16 cifre e 17). Quando si visualizza l'ultima pagina di dati vengono disabilitati i pulsanti Avanti sia l'ultimo.


[![I pulsanti Indietro e ultimo sono disabilitati quando si visualizzano i prodotti di pagina prima](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Figura 15**: le precedenti e i pulsanti ultimo sono disabilitati quando si visualizza la pagina prima di prodotti ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![La seconda pagina prodotti sono applicato all'intero](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Figura 16**: la seconda pagina di prodotti sono applicato all'intero ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Fare clic su Visualizza ultima pagina finale dei dati](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Figura 17**: facendo clic su ultima consente di visualizzare la pagina finale dei dati ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Passaggio 7: Incluso l'ordinamento di supporto con personalizzata del pool di paging Ripetitore

Ora che è stato implementato il paging personalizzato, il supporto è nuovamente pronto per includere l'ordinamento. Il `ProductsBLL` classe s `GetProductsPagedAndSorted` metodo ha lo stesso *startRowIndex* e *maximumRows* parametri di input `GetProductsPaged`, ma consente un ulteriore  *sortExpression* parametro di input. Utilizzare il `GetProductsPagedAndSorted` metodo `SortingWithCustomPaging.aspx`, è necessario eseguire la procedura seguente:

1. Modificare gli oggetti ObjectDataSource `SelectMethod` proprietà `GetProductsPaged` a `GetProductsPagedAndSorted`.
2. Aggiungere un *sortExpression* `Parameter` oggetto s ObjectDataSource `SelectParameters` insieme.
3. Creare una privata, a livello di pagina `SortExpression` proprietà che rende persistente il valore di postback tramite lo stato di visualizzazione pagina s.
4. Aggiornare i dispositivi ObjectDataSource `Selecting` il gestore eventi per assegnare i dispositivi ObjectDataSource *sortExpression* parametro il valore del livello di pagina `SortExpression` proprietà.
5. Creare l'interfaccia di ordinamento.

Avviare aggiornando il s ObjectDataSource `SelectMethod` proprietà e l'aggiunta di un *sortExpression* `Parameter`. Assicurarsi che il *sortExpression* `Parameter` s `Type` è impostata su `String`. Dopo aver completato le prime due attività, il markup dichiarativo s ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Successivamente, è necessario un livello di pagina `SortExpression` proprietà il cui valore viene serializzato allo stato di visualizzazione. Se non è stato impostato alcun valore di espressione di ordinamento, utilizzare ProductName come il valore predefinito:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Prima di ObjectDataSource richiama il `GetProductsPagedAndSorted` metodo è necessario impostare il *sortExpression* `Parameter` al valore del `SortExpression` proprietà. Nel `Selecting` gestore, aggiungere la riga di codice seguente:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

È comunque necessario implementare l'interfaccia di ordinamento. Come è stato fatto nell'ultimo esempio, consentire s l'ordinamento interfaccia implementata utilizzando tre controlli pulsante Web che consentono all'utente di ordinare i risultati dal fornitore, categoria o il nome del prodotto.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Creare `Click` gestori eventi per i tre controlli pulsante. Nell'evento gestore, reimpostare il `StartRowIndex` su 0, viene impostata la `SortExpression` per il valore appropriato e i dati al controllo Repeater riassociazione:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Sono disponibili tutte le che s è! Mentre ci fosse un numero di passaggi per ottenere il paging personalizzato e l'ordinamento implementato, i passaggi sono molto simili a quelli necessari per il paging predefinito. Figura 18 vengono indicati i prodotti quando si visualizza l'ultima pagina di dati quando viene ordinato in base alla categoria.


[![Viene visualizzata l'ultima pagina di dati, ordinato in base alla categoria,](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Figura 18**: l'ultima pagina di dati, ordinato in base alla categoria, viene visualizzato ([fare clic per visualizzare l'immagine ingrandita](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> Negli esempi precedenti, durante l'ordinamento dal fornitore che NomeFornitore è stata utilizzata come espressione di ordinamento. Tuttavia, per l'implementazione di paging personalizzata, è necessario usare CompanyName. Infatti, la stored procedure responsabile dell'implementazione il paging personalizzato `GetProductsPagedAndSorted` passa l'espressione di ordinamento nel `ROW_NUMBER()` (parola chiave), il `ROW_NUMBER()` (parola chiave) richiede il nome di colonna effettivi anziché un alias. Pertanto, è necessario utilizzare `CompanyName` (il nome della colonna nella `Suppliers` tabella) anziché l'alias usato nella `SELECT` query (`SupplierName`) per l'espressione di ordinamento.


## <a name="summary"></a>Riepilogo

Né il DataList né Ripetitore offrono il supporto dell'ordinamento predefinito, ma con un minimo di codice e un'interfaccia di ordinamento personalizzata, è possibile aggiungere tale funzionalità. Quando si implementa l'ordinamento, ma non di paging, l'espressione di ordinamento può essere specificata tramite il `DataSourceSelectArguments` oggetto passato a s ObjectDataSource `Select` metodo. Questo `DataSourceSelectArguments` oggetto s `SortExpression` proprietà può essere assegnata in s ObjectDataSource `Selecting` gestore dell'evento.

Per aggiungere funzionalità di ordinamento a un controllo DataList o Repeater che già fornisce il supporto di paging, l'approccio più semplice consiste nel personalizzare il livello di logica di Business per includere un metodo che accetta un'espressione di ordinamento. Queste informazioni possono quindi essere passate attraverso un parametro in s ObjectDataSource `SelectParameters`.

In questa esercitazione viene completato l'esame del paging e l'ordinamento con i controlli DataList e Repeater. Questa esercitazione successiva e finale verrà esaminati come aggiungere i controlli pulsante Web per i modelli di s DataList e Repeater per fornire funzionalità personalizzate, avviata dall'utente su una base per ogni elemento.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata David Suru. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
[Successivo](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
