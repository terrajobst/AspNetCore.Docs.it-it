---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: Master-Details filtri con un controllo DropDownList (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione vedremo come visualizzare i record master in un controllo DropDownList e i dettagli dell'elemento di elenco selezionato in un controllo GridView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42a6a76b0b05045bed1ada227b7c32a51600b760
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Master-Details filtri con un controllo DropDownList (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) o [Scarica il PDF](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> In questa esercitazione vedremo come visualizzare i record master in un controllo DropDownList e i dettagli dell'elemento di elenco selezionato in un controllo GridView.


## <a name="introduction"></a>Introduzione

Un tipo comune di report è il *master-details report*, in cui inizia il report tramite la visualizzazione di un set di record "master". L'utente può quindi drill-down in uno dei record master, in tal modo la visualizzazione "i dettagli." del record master Master-Details report è la scelta ideale per la visualizzazione delle relazioni uno-a-molti, ad esempio un report che mostra tutte le categorie e quindi consentire all'utente di selezionare una categoria specifica e visualizzare i prodotti associati. Master-details report sono inoltre utili per la visualizzazione di informazioni dettagliate dalle tabelle particolarmente "wide" (quelli che dispone di numerose colonne). Ad esempio, il livello di un report master-details "master" potrebbe mostrare solo il prodotto nome e il prezzo unitario dei prodotti del database e il drill-down in un determinato prodotto permetterebbe di visualizzare i campi prodotto aggiuntive (categoria, fornitore, la quantità per unità, e così via).

Esistono molti modi con cui può essere implementata una relazione master-details. In questa e le esercitazioni successive tre esamineremo una varietà di report master-details. In questa esercitazione si vedrà come visualizzare i record master in un [controllo DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) e i dettagli dell'elemento di elenco selezionato in un controllo GridView. Relazione master-details dell'esercitazione, in particolare, le informazioni di category e product.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Passaggio 1: Visualizzare le categorie in un controllo DropDownList

Il report master-details elencherà le categorie in un controllo DropDownList, con i prodotti della voce di elenco selezionato visualizzati più in basso nella pagina un controllo GridView. La prima attività precedono us, è quindi, per le categorie visualizzate in un controllo DropDownList. Aprire il `FilterByDropDownList.aspx` nella pagina il `Filtering` cartella, trascinare un controllo DropDownList dalla casella degli strumenti nella finestra di progettazione della pagina e impostare il relativo `ID` proprietà `Categories`. Fare clic sul collegamento smart tag del DropDownList Scegli origine dati. Verrà visualizzata la configurazione guidata origine dati.


[![Specificare l'origine dati del DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Figura 1**: specificare l'origine dati del DropDownList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))


Scegliere di aggiungere un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` che richiama la `CategoriesBLL` della classe `GetCategories()` metodo.


[![Aggiungere un nuovo oggetto ObjectDataSource denominato CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**Figura 2**: aggiungere un nuovo denominato ObjectDataSource `CategoriesDataSource` ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))


[![Scegliere di utilizzare la classe CategoriesBLL](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Figura 3**: scegliere di utilizzare il `CategoriesBLL` classe ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))


[![Configurare ObjectDataSource per utilizzare il metodo GetCategories()](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per utilizzare il `GetCategories()` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))


Dopo aver configurato il ObjectDataSource è comunque necessario specificare quale campo dell'origine dati deve essere visualizzato in DropDownList e che uno deve essere associato come valore per l'elemento dell'elenco. Dispone il `CategoryName` campo come la visualizzazione e `CategoryID` come valore per ogni elemento nell'elenco.


[![Avere la visualizzazione DropDownList campo CategoryName e CategoryID utilizzare come valore](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Figura 5**: la visualizzazione DropDownList il `CategoryName` campo e utilizzare `CategoryID` come valore ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))


A questo punto si dispongono di un controllo DropDownList che viene popolato con i record di `Categories` tabella (all eseguita in circa sei secondi). Figura 6 mostra l'avanzamento finora quando viene visualizzato tramite un browser.


[![Un elenco a discesa sono elencate le categorie corrente](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Figura 6**: un elenco a discesa sono elencate le categorie corrente ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Passaggio 2: Aggiunta di prodotti GridView

Quest'ultimo passaggio nel report master-Details è per elencare i prodotti associati alla categoria selezionata. A tale scopo, aggiungere un controllo GridView alla pagina e creare un nuovo oggetto ObjectDataSource denominato `productsDataSource`. Dispone il `productsDataSource` controllo selezionare i dati di `ProductsBLL` della classe `GetProductsByCategoryID(categoryID)` (metodo).


[![Selezionare il metodo GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Figura 7**: selezionare il `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))


Dopo aver scelto questo metodo, la procedura guidata ObjectDataSource richiede us per il valore per il metodo *`categoryID`* parametro. Per utilizzare il valore dell'oggetto selezionato `categories` DropDownList elemento imposta l'origine di parametro al controllo e il ControlID per `Categories`.


[![Impostare il valore di DropDownList categorie categoryID parametro](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Figura 8**: impostare il *`categoryID`* parametro per il valore della `Categories` DropDownList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))


Richiedere qualche istante per l'estrazione di stato di avanzamento in un browser. Durante la prima visita la pagina, tali prodotti appartengono alla categoria selezionata (bibite) vengono visualizzate (come illustrato nella figura 9), ma modifica DropDownList non aggiorna i dati. Infatti, deve verificarsi un postback per il controllo GridView aggiornare. A tale scopo che sono disponibili due opzioni (nessuna delle quali è necessario scrivere codice):

- **Impostare le categorie DropDownList**[proprietà di un postback automatico](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**su True.** (È possibile eseguire questo selezionando l'opzione di attivare un postback automatico smart tag del DropDownList.) Che attiverà un postback ogni volta che il controllo DropDownList selezionata elemento è stato modificato dall'utente. Pertanto, quando l'utente seleziona una nuova categoria da DropDownList verrà insorgere un postback e GridView verrà aggiornato con i prodotti per la categoria selezionata. (Questo è l'approccio che ho utilizzato in questa esercitazione).
- **Aggiungere un controllo pulsante Web accanto DropDownList.** Impostare il relativo `Text` proprietà per l'aggiornamento o un attributo simile. Con questo approccio, l'utente sarà necessario selezionare una nuova categoria e quindi fare clic sul pulsante. Fare clic sul pulsante causerà un postback e aggiorna il GridView per elencare i prodotti della categoria selezionata.

Figure 9 e 10 viene illustrato il report master/dettaglio in azione.


[![Durante la prima visita la pagina, vengono visualizzati i prodotti delle bibite](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Figura 9**: durante la prima visita la pagina, vengono visualizzati i prodotti delle bibite ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))


[![Se si seleziona un nuovo prodotto (prodotto) automaticamente, un PostBack, l'aggiornamento di GridView](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Figura 10**: se si seleziona un nuovo prodotto (prodotto) automaticamente, un PostBack, l'aggiornamento di GridView ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Aggiunta di un elemento di elenco "-consente di scegliere una categoria"

Durante la visita prima la `FilterByDropDownList.aspx` pagina le categorie primo elemento di elenco del DropDownList (bibite) è selezionata per impostazione predefinita, che mostra i prodotti delle bibite in GridView. Anziché con i prodotti della categoria prima, che si può decidere di creare invece un elemento di DropDownList selezionata che è simile al seguente, "-consente di scegliere una categoria,".

Per aggiungere un nuovo elemento elenco DropDownList, passare alla finestra proprietà e fare clic sui puntini di sospensione nel `Items` proprietà. Aggiungere un nuovo elemento di elenco con il `Text` "-consente di scegliere una categoria-" e `Value` `-1`.


[![Aggiungere un - selezionare una categoria - voce di elenco](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Figura 11**: aggiungere un - selezionare una categoria - voce di elenco ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))


In alternativa, è possibile aggiungere la voce di elenco aggiungendovi DropDownList il markup seguente:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Inoltre, è necessario impostare il controllo DropDownList `AppendDataBoundItems` su True perché quando le categorie vengono associate a DropDownList da ObjectDataSource queste sovrascrivono gli elementi dell'elenco aggiunto manualmente se `AppendDataBoundItems` non è True.


![Impostare la proprietà AppendDataBoundItems su True](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Figura 12**: impostare il `AppendDataBoundItems` proprietà su True


Dopo aver apportato queste modifiche, durante la prima visita la pagina è selezionata l'opzione "-consente di scegliere una categoria," e non i prodotti vengono visualizzati.


[![Il caricamento della pagina iniziale prodotti non vengono visualizzati](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Figura 13**: in the iniziale pagina carico No i prodotti vengono visualizzati ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))


Nessun prodotto viene visualizzato quando perché l'elemento di elenco "-consente di scegliere una categoria," è selezionato, infatti, il valore è `-1` e non sono presenti prodotti nel database con un `CategoryID` di `-1`. Se si tratta del comportamento di cui che si desidera la apportate a questo punto! Se, tuttavia, si desidera visualizzare *tutti* delle categorie quando è selezionata la voce di elenco "-consente di scegliere una categoria,", tornare al `ProductsBLL` classe e personalizzare il `GetProductsByCategoryID(categoryID)` metodo in modo che richiama il `GetProducts()` metodo se il valore passato in *`categoryID`* parametro è minore di zero:

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

La tecnica usata in questo esempio è simile all'approccio è utilizzati per visualizzare tutti i fornitori nuovamente il [parametri dichiarativi](../basic-reporting/declarative-parameters-cs.md) dell'esercitazione, anche se per questo esempio si usa un valore di `-1` per indicare che tutti i record devono essere recuperare anziché `null`. In questo modo il *`categoryID`* parametro del `GetProductsByCategoryID(categoryID)` metodo prevede che al valore passato valore intero, mentre nell'esercitazione parametri dichiarativi si stava passando un parametro di input di stringa.

La figura 14 mostra una schermata di `FilterByDropDownList.aspx` quando è selezionata l'opzione "-consente di scegliere una categoria,". In questo caso, tutti i prodotti vengono visualizzati per impostazione predefinita, e l'utente è possibile limitare la visualizzazione scegliendo una categoria specifica.


[![Tutti i prodotti sono ora elencati per impostazione predefinita](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Figura 14**: tutti i prodotti sono ora elencati per impostazione predefinita ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))


## <a name="summary"></a>Riepilogo

Quando si visualizzano dati correlati in modo gerarchico, è spesso utile per presentare i dati di utilizzo dei report master/dettaglio, da cui l'utente può iniziare ad analizzare i dati dalla parte superiore della gerarchia e drill-down nei dettagli. In questa esercitazione sono state esaminate la creazione di un report semplice master-details che mostri i prodotti della categoria selezionata. Questa operazione è stata eseguita utilizzando un controllo DropDownList per l'elenco di categorie e GridView per i prodotti appartenenti alla categoria selezionata.

Nel [esercitazione successiva](master-detail-filtering-with-two-dropdownlists-cs.md) l'utente verrà reindirizzato, inoltre, il passaggio di un'interfaccia DropDownList mediante due controlli DropDownList.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [avanti](master-detail-filtering-with-two-dropdownlists-cs.md)
