---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Master-Details filtri con due controlli DropDownList (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione si espande la relazione master-Details per aggiungere un terzo livello, usando due controlli DropDownList per selezionare il recor padre e l'elemento padre del padre desiderato...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: d971dcb3814dc088202c3a3e4addb03375049ca0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887252"
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Master-Details filtri con due controlli DropDownList (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) o [Scarica il PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> In questa esercitazione si espande la relazione master-Details per aggiungere un terzo livello, usando due controlli DropDownList per selezionare i record padre e l'elemento padre del padre desiderati.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](master-detail-filtering-with-a-dropdownlist-cs.md) abbiamo esaminato come visualizzare un report semplice master/dettagli mediante un unico DropDownList popolato con le categorie e un controllo GridView con i prodotti appartenenti alla categoria selezionata. Questo modello di report funziona bene quando la visualizzazione di record che includono una relazione uno-a-molti e può essere facilmente esteso per lavorare con gli scenari che includono più relazioni uno-a-molti. Ad esempio, un sistema di immissione ordini avrebbe le tabelle che corrispondono ai clienti, ordini e voci dell'ordine. Un determinato cliente può disporre di più ordini con ogni ordine composta da più elementi. Tali dati possono essere presentati all'utente con due controlli DropDownList e un controllo GridView. Il primo DropDownList avrebbe una voce di elenco per ogni cliente nel database con il secondo il contenuto viene ordini effettuati dal cliente selezionato. Un controllo GridView verrà elencati gli elementi di riga dell'ordine selezionato.

Quando il database Northwind include informazioni cliente/ordine canonico nella relativa `Customers`, `Orders`, e `Order Details` , queste tabelle non vengono acquisite nell'architettura. Ciononostante, è comunque possibile descrivere mediante due controlli DropDownList dipendenti. Il primo DropDownList sono elencate le categorie e il secondo i prodotti appartenenti alla categoria selezionata. Un controllo DetailsView verranno quindi elencati i dettagli del prodotto selezionato.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Passaggio 1: Creazione e popolamento DropDownList categorie

Il primo obiettivo consiste nell'aggiungere DropDownList che elenca le categorie. Questi passaggi sono stati esaminati in dettaglio nell'esercitazione precedente, ma sono riepilogati di seguito per motivi di completezza.

Aprire il `MasterDetailsDetails.aspx` nella pagina il `Filtering` cartella, aggiungere un controllo DropDownList alla pagina, imposta il relativo `ID` proprietà `Categories`e fare clic sul collegamento Configura origine dati in smart tag. Scegliere la configurazione guidata origine dati per aggiungere una nuova origine dati.


[![Aggiungere una nuova origine dati per DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Figura 1**: aggiungere una nuova origine dati per DropDownList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


La nuova origine dati, naturalmente, dovrebbe essere ObjectDataSource. Nome nuovo ObjectDataSource `CategoriesDataSource` e richiamare il `CategoriesBLL` dell'oggetto `GetCategories()` metodo.


[![Scegliere di utilizzare la classe CategoriesBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Figura 2**: scegliere di utilizzare il `CategoriesBLL` classe ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![Configurare ObjectDataSource per utilizzare il metodo GetCategories()](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Figura 3**: configurare ObjectDataSource per utilizzare il `GetCategories()` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


Dopo aver configurato il ObjectDataSource è comunque necessario specificare quale campo dell'origine dati deve essere visualizzato nel `Categories` DropDownList e quale deve essere configurato come il valore della voce di elenco. Impostare il `CategoryName` campo come la visualizzazione e `CategoryID` come valore per ogni elemento nell'elenco.


[![Avere la visualizzazione DropDownList campo CategoryName e CategoryID utilizzare come valore](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Figura 4**: la visualizzazione DropDownList il `CategoryName` campo e utilizzare `CategoryID` come valore ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


A questo punto si dispone di un controllo DropDownList (`Categories`) che viene popolata con i record di `Categories` tabella. Quando l'utente sceglie una nuova categoria da DropDownList è opportuno un postback per aggiornare il prodotto DropDownList che si intende creare nel passaggio 2. Pertanto, selezionare l'opzione di attivare un postback automatico dal `categories` smart tag del DropDownList.


[![Abilitare un postback automatico per DropDownList categorie](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Figura 5**: abilitare un postback automatico per il `Categories` DropDownList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Passaggio 2: Visualizzazione dei prodotti della categoria selezionata in un secondo DropDownList

Con il `Categories` DropDownList completato, il passaggio successivo consiste nella visualizzazione di un controllo DropDownList dei prodotti appartenenti alla categoria selezionata. A tale scopo, aggiungere un altro DropDownList denominato `ProductsByCategory`. Come con la `Categories` DropDownList, creare un nuovo oggetto ObjectDataSource per la `ProductsByCategory` DropDownList denominato `ProductsByCategoryDataSource`.


[![Aggiungere una nuova origine dati per ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Figura 6**: aggiungere una nuova origine dati per il `ProductsByCategory` DropDownList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![Creare un nuovo oggetto ObjectDataSource denominato ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Figura 7**: creare un nuovo denominato ObjectDataSource `ProductsByCategoryDataSource` ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


Poiché il `ProductsByCategory` DropDownList esigenze per visualizzare solo i prodotti appartenenti alla categoria selezionata, avere ObjectDataSource richiama il `GetProductsByCategoryID(categoryID)` metodo il `ProductsBLL` oggetto.


[![Scegliere di utilizzare la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Figura 8**: scegliere di utilizzare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![Configurare ObjectDataSource per utilizzare il metodo GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Figura 9**: configurare ObjectDataSource per utilizzare il `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


Nel passaggio finale della procedura guidata è necessario specificare il valore di *`categoryID`* parametro. Assegnare questo parametro per l'elemento selezionato dal `Categories` DropDownList.


[![Effettuare il pull categoryID valore del parametro da DropDownList categorie](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Figura 10**: effettuare il Pull di *`categoryID`* valore del parametro dal `Categories` DropDownList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


Con ObjectDataSource configurato, è comunque per specificare quali campi dell'origine dati vengono utilizzate per la visualizzazione e il valore degli elementi del DropDownList. Visualizzazione di `ProductName` campo e utilizzare il `ProductID` campo come valore.


[![Specificare i campi di origine dati utilizzati per oggetti ListItem del DropDownList testo e le proprietà di valore](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Figura 11**: specificare campi di origine dati utilizzata per il controllo DropDownList `ListItem` s' `Text` e `Value` delle proprietà ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


Con ObjectDataSource e `ProductsByCategory` DropDownList configurato nostra pagina verranno visualizzati due controlli DropDownList: il primo elencherà tutte le categorie mentre il secondo verrà elencati i prodotti appartenenti alla categoria selezionata. Quando l'utente seleziona una nuova categoria dal primo DropDownList, verrà insorgere un postback e verrà riassociati DropDownList secondo, con i prodotti che appartengono alla categoria selezionata. Figure 12 e 13 mostra `MasterDetailsDetails.aspx` nell'azione quando viene visualizzato tramite un browser.


[![Durante la prima visita la pagina, della categoria Beverages è selezionata](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Figura 12**: durante la prima visita la pagina, della categoria Beverages è selezionata ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![Scelta di una categoria diversi Visualizza di prodotti le nuove categorie](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Figura 13**: scegliere di prodotti le nuove categorie diversi Visualizza categoria ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


Attualmente il `productsByCategory` DropDownList, quando vengono modificate, *non* provocato un postback. È tuttavia, sarà necessario un postback al verificarsi di una volta che viene aggiunto un controllo DetailsView per visualizzare i dettagli del prodotto selezionato (passaggio 3). Di conseguenza, verificare la casella di controllo Abilita un postback automatico di `productsByCategory` smart tag del DropDownList.


[![Abilitare la funzionalità di un postback automatico per productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Figura 14**: abilitare la funzionalità di un postback automatico per il `productsByCategory` DropDownList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Passaggio 3: Utilizzo di un controllo DetailsView per visualizzare i dettagli per il prodotto selezionato

Il passaggio finale consiste per visualizzare i dettagli per il prodotto selezionato in un controllo DetailsView. A tale scopo, aggiungere un controllo DetailsView alla pagina, impostare il relativo `ID` proprietà `ProductDetails`e creare un nuovo oggetto ObjectDataSource per essa. Configurare ObjectDataSource per estrarre i dati di `ProductsBLL` della classe `GetProductByProductID(productID)` metodo usando il valore selezionato del `ProductsByCategory` DropDownList per il valore del *`productID`* parametro.


[![Scegliere di utilizzare la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Figura 15**: scegliere di utilizzare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![Configurare ObjectDataSource per utilizzare il metodo GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Figura 16**: configurare ObjectDataSource per utilizzare il `GetProductByProductID(productID)` metodo ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![Effettuare il pull productID valore del parametro da ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Figura 17**: effettuare il Pull di *`productID`* valore del parametro dal `ProductsByCategory` DropDownList ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


È possibile scegliere di visualizzare uno qualsiasi dei campi disponibili nel controllo DetailsView. È stato scelto di rimuovere il `ProductID`, `SupplierID`, e `CategoryID` campi e riordinare e formattati i campi rimanenti. Inoltre, sono deselezionate out di DetailsView `Height` e `Width` proprietà, consentendo di DetailsView espandere la larghezza necessari per una visualizzazione ottimale dei dati anziché è vincolata alle dimensioni specificate. Il markup completo viene visualizzato di seguito:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

È opportuno provare il `MasterDetailsDetails.aspx` pagina in un browser. A prima vista potrebbe sembrare che funzionino nel modo desiderato, ma si verifica un problema complesso. Quando si sceglie una nuova categoria di `ProductsByCategory` DropDownList viene aggiornata per includere i prodotti per la categoria selezionata, ma la `ProductDetails` DetailsView continua a visualizzare le informazioni sul prodotto precedente. DetailsView viene aggiornata quando si sceglie un prodotto diverso per la categoria selezionata. Inoltre, se si prova sufficientemente attentamente, si noterà che se si sceglie continuamente nuove categorie (optando bevande dal `Categories` DropDownList quindi condimenti, quindi dolciumi) ogni altra selezione della categoria comporta il `ProductDetails`DetailsView da aggiornare.

Per facilitare la concretizzare la questo problema, esaminiamo un esempio specifico. Quando si visita prima la pagina è selezionata la categoria delle bevande e i prodotti correlati vengono caricati nel `ProductsByCategory` DropDownList. Chai è il prodotto selezionato e cui vengono visualizzati i dettagli di `ProductDetails` DetailsView, come illustrato nella figura 18.


[![I dettagli del prodotto selezionato vengono visualizzati in un controllo DetailsView.](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Figura 18**: dettagli del prodotto selezionato il vengono visualizzati in un controllo DetailsView ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


Se si modifica la selezione della categoria da bevande a condimenti, si verifica un postback e `ProductsByCategory` DropDownList viene aggiornato di conseguenza, ma controllo DetailsView vengono ancora visualizzati i dettagli per Chai.


[![Dettagli precedentemente selezionata del prodotto sono ancora visualizzati](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Figura 19**: dettagli del prodotto selezionata precedentemente il sono ancora visualizzati ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


Selezione di un nuovo prodotto nell'elenco Aggiorna DetailsView come previsto. Se si seleziona una categoria di nuovo dopo aver modificato il prodotto, non aggiorna il controllo DetailsView nuovamente. Tuttavia, se invece un nuovo prodotto è stata selezionata una nuova categoria, è necessario riavviare DetailsView. Nel mondo andamento qui?

Il problema è un problema di temporizzazione nel ciclo di vita della pagina. Ogni volta che viene richiesta una pagina che procede attraverso una serie di operazioni come il rendering. In uno di questi passaggi ObjectDataSource controlla per verificare se uno dei relativi `SelectParameters` valori sono stati modificati. Se in tal caso, il controllo Web di dati associato a ObjectDataSource sa che è necessario aggiornare la visualizzazione. Ad esempio, quando è selezionata una nuova categoria, il `ProductsByCategoryDataSource` ObjectDataSource rileva che i valori di parametro sono stati modificati e `ProductsByCategory` DropDownList riassocia, ottenere i prodotti per la categoria selezionata.

Il problema che si manifesta in questa situazione è che si verifica il punto di vita che consentono di controllare il ObjectDataSources per i parametri modificati *prima* la riassociazione dei controlli Web di dati associati. Pertanto, quando si seleziona una nuova categoria di `ProductsByCategoryDataSource` ObjectDataSource rileva una modifica nel valore del relativo parametro. Utilizzato da ObjectDataSource il `ProductDetails` DetailsView, tuttavia, non nota tali modifiche poiché il `ProductsByCategory` DropDownList deve ancora essere riassociati. Più avanti nel ciclo di vita di `ProductsByCategory` DropDownList riassocia di ObjectDataSource cattura i prodotti per la categoria selezionata. Mentre il `ProductsByCategory` valore del DropDownList è stata modificata, il `ProductDetails` DetailsView ObjectDataSource ha già eseguito il controllo del valore del parametro; di conseguenza, il controllo DetailsView visualizza i risultati precedenti. Questa interazione viene rappresentata nella figura 20.


[![Il valore ProductsByCategory DropDownList cambia dopo ObjectDataSource di ProductDetails DetailsView verifica la presenza di modifiche](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Figura 20**: il `ProductsByCategory` DropDownList valore modifiche dopo il `ProductDetails` ObjectDataSource verifica DetailsView la presenza di modifiche ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


Per risolvere questo è necessario riassociare in modo esplicito il `ProductDetails` DetailsView dopo il `ProductsByCategory` DropDownList è stato associato. A questo scopo, è possibile chiamare il `ProductDetails` DetailsView `DataBind()` metodo quando il `ProductsByCategory` del DropDownList `DataBound` viene generato l'evento. Aggiungere il seguente codice del gestore eventi per il `MasterDetailsDetails.aspx` classe code-behind della pagina (vedere il "[a livello di codice impostando i valori dei parametri di ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" per una discussione su come aggiungere un gestore eventi):


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Dopo questa chiamata esplicita al `ProductDetails` DetailsView `DataBind()` metodo è stato aggiunto, l'esercitazione funziona come previsto. Figura 21 evidenzia come modificare questo risolto il problema precedente.


[![ProductDetails DetailsView è attivazione dell'esplicitamente aggiornati quando il ProductsByCategory DropDownList dell'evento associato a dati](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Figura 21**: il `ProductDetails` DetailsView è in modo esplicito aggiornati quando il `ProductsByCategory` del DropDownList `DataBound` attivazione dell'evento ([fare clic per visualizzare l'immagine ingrandita](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>Riepilogo

DropDownList funge da un elemento dell'interfaccia utente ideale per i report master/dettaglio in cui vi sia una relazione uno-a-molti tra i record master e di dettaglio. Nell'esercitazione precedente è stato illustrato come usare un singolo DropDownList per filtrare i prodotti visualizzati in base alla categoria selezionata. In questa esercitazione è sostituito GridView dei prodotti con un controllo DropDownList e usato un controllo DetailsView per visualizzare i dettagli del prodotto selezionato. I concetti illustrati in questa esercitazione possono essere esteso facilmente ai modelli di dati che coinvolgono più relazioni uno-a-molti, ad esempio clienti, ordini e ordinare gli articoli. In generale, è sempre possibile aggiungere un controllo DropDownList per ognuna delle entità nelle relazioni uno-a-molti "uno".

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-detail-filtering-with-a-dropdownlist-cs.md)
> [Successivo](master-detail-filtering-across-two-pages-cs.md)
