---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Batch di eliminazione (VB) | Documenti Microsoft
author: rick-anderson
description: Informazioni su come eliminare più record di database in un'unica operazione. Il Layer dell'interfaccia utente è di ampliare un GridView avanzata creato nella versione precedente tut...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d17ec6ec20be65b3ec9369f1c5a08d5970ec0dd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880115"
---
<a name="batch-deleting-vb"></a>Batch di eliminazione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) o [Scarica il PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Informazioni su come eliminare più record di database in un'unica operazione. Il Layer dell'interfaccia utente è di ampliare un GridView avanzata creata in un'esercitazione precedente. Nel livello di accesso ai dati si eseguono il wrapping di più operazioni di eliminazione in una transazione per garantire che tutte le eliminazioni completata o vengono eseguito il rollback di tutte le eliminazioni.


## <a name="introduction"></a>Introduzione

Il [esercitazione precedente](batch-updating-vb.md) esplorata come creare un batch Modifica interfaccia usando un GridView completamente modificabile. In situazioni in cui gli utenti sono in genere modifica numero di record in una sola volta, un batch di interfaccia di modifica richiederà meno postback e il contesto di mouse da tastiera per commutatori, migliorando l'efficienza di s utente finale. Questa tecnica è allo stesso modo utile per le pagine in cui è comune per gli utenti di eliminare più record in un'unica operazione.

Chi ha utilizzato un client di posta elettronica online ha già familiarità con uno dei più comuni batch l'eliminazione di interfacce: pulsante di una casella di controllo in ogni riga in una griglia con un corrispondente eliminare tutti gli elementi selezionati (vedere la figura 1). In questa esercitazione è piuttosto breve perché è ve già tutte le operazioni di disco rigida nelle esercitazioni precedenti per creare sia l'interfaccia basata sul web e un metodo per eliminare una serie di record come una singola operazione atomica. Nel [aggiunta di una colonna di GridView di caselle di controllo](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) è creato un controllo GridView con una colonna di caselle di controllo e nell'esercitazione di [di wrapping delle modifiche del Database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-vb.md) esercitazione è stato creato un metodo in il livello Business LOGIC che usa una transazione per eliminare un `List<T>` di `ProductID` valori. In questa esercitazione verrà si basano e unire il nostro esperienze precedenti per creare un batch di lavoro, l'eliminazione di esempio.


[![Ogni riga include una casella di controllo](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Figura 1**: ogni riga include una casella di controllo ([fare clic per visualizzare l'immagine ingrandita](batch-deleting-vb/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Passaggio 1: Creazione di Batch di eliminazione di interfaccia

Poiché è già stato creato il batch, l'eliminazione di interfaccia di [aggiunta di una colonna di GridView di caselle di controllo](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) esercitazione, è possibile semplicemente copiare `BatchDelete.aspx` anziché crearlo da zero. Avvia aprendo il `BatchDelete.aspx` nella pagina di `BatchData` cartella e il `CheckBoxField.aspx` nella pagina di `EnhancedGridView` cartella. Dal `CheckBoxField.aspx` pagina, passare alla visualizzazione origine e copiare il codice tra le `<asp:Content>` tag come illustrato nella figura 2.


[![Copiare il Markup dichiarativo di CheckBoxField.aspx negli Appunti](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Figura 2**: copiare il Markup dichiarativo del `CheckBoxField.aspx` negli Appunti ([fare clic per visualizzare l'immagine ingrandita](batch-deleting-vb/_static/image4.png))


Successivamente, passare alla visualizzazione origine in `BatchDelete.aspx` e incollare il contenuto degli Appunti all'interno di `<asp:Content>` tag. Anche copiare e incollare il codice dall'interno la classe code-behind in `CheckBoxField.aspx.vb` all'interno di classe code-behind in `BatchDelete.aspx.vb` (il `DeleteSelectedProducts` pulsante s `Click` gestore dell'evento, il `ToggleCheckState` (metodo) e `Click` gestori eventi per il `CheckAll` e `UncheckAll` pulsanti). Dopo aver copiato il contenuto, la `BatchDelete.aspx` classe code-behind s pagina deve contenere il codice seguente:


[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Dopo aver copiato il markup dichiarativo e codice sorgente, è opportuno testare `BatchDelete.aspx` visualizzandolo tramite un browser. Verrà visualizzato un elenco dei primi dieci prodotti in un controllo GridView con ogni riga Elenca il nome del prodotto s, categoria e prezzo insieme a una casella di controllo di GridView. Dovrebbero essere presenti tre pulsanti: controllare tutti deselezionare tutti i prodotti ed eliminare selezionato. Fare clic sul pulsante Seleziona tutto seleziona tutte le caselle di controllo, mentre tutti deselezionare Cancella tutte le caselle di controllo. Facendo clic di eliminare i prodotti selezionati viene visualizzato un messaggio in cui sono elencati i `ProductID` i valori dei prodotti selezionati, ma non elimina effettivamente i prodotti.


[![L'interfaccia da CheckBoxField.aspx è stata spostata in BatchDeleting.aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Figura 3**: l'interfaccia da `CheckBoxField.aspx` sono state spostate in `BatchDeleting.aspx` ([fare clic per visualizzare l'immagine ingrandita](batch-deleting-vb/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Passaggio 2: Eliminazione dei prodotti selezionati utilizzando transazioni

Con il batch eliminando interfaccia copiata di `BatchDeleting.aspx`tutti che rimane consiste nell'aggiornare il codice in modo che il pulsante Elimina prodotti selezionati Elimina i prodotti selezionati utilizzando il `DeleteProductsWithTransaction` metodo nel `ProductsBLL` classe. Questo metodo, aggiunto nel [di wrapping delle modifiche del Database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-vb.md) esercitazione accetta come input un `List(Of T)` di `ProductID` valori ed elimina ogni corrispondente `ProductID` all'interno dell'ambito di un transazione.

Il `DeleteSelectedProducts` pulsante s `Click` gestore dell'evento attualmente utilizza il seguente `For Each` ciclo per scorrere ogni riga GridView:


[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Per ogni riga, il `ProductSelector` controllo casella di controllo Web a livello di codice a cui fa riferimento. Se è selezionata, la riga s `ProductID` viene recuperato dal `DataKeys` insieme e `DeleteResults` etichetta s `Text` proprietà viene aggiornata per includere un messaggio che indica che la riga è stata selezionata per l'eliminazione.

Il codice precedente non viene eliminato alcun record come la chiamata al `ProductsBLL` classe s `Delete` metodo è impostato come commento. Sono stati questa logica di eliminazione da applicare, il codice di eliminare i prodotti ma non all'interno di un'operazione atomica. Ovvero, se le eliminazioni alcuni prima nella sequenza ha avuto esito positivo, ma una versione più recente non riuscito (probabilmente a causa di una violazione di vincolo di chiave esterna), verrebbe generata un'eccezione ma restano eliminati i prodotti già eliminati.

Per garantire l'atomicità, è necessario usare invece il `ProductsBLL` classe s `DeleteProductsWithTransaction` metodo. Poiché questo metodo accetta un elenco di `ProductID` valori, è necessario innanzitutto compilare questo elenco dalla griglia e quindi passarlo come parametro. È necessario creare un'istanza di un `List(Of T)` di tipo `Integer`. All'interno di `For Each` ciclo è necessario aggiungere i prodotti selezionati `ProductID` valori a questo `List(Of T)`. Dopo il ciclo si `List(Of T)` deve essere passato al `ProductsBLL` classe s `DeleteProductsWithTransaction` metodo. Aggiornamento di `DeleteSelectedProducts` pulsante s `Click` gestore dell'evento con il codice seguente:


[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

Crea il codice aggiornato un `List(Of T)` di tipo `Integer` (`productIDsToDelete`) e la popola con il `ProductID` valori da eliminare. Dopo il `For Each` ciclo, se è presente almeno un prodotto selezionato, il `ProductsBLL` classe s `DeleteProductsWithTransaction` metodo viene chiamato e passato a questo elenco. Il `DeleteResults` viene visualizzata anche l'etichetta e i dati riassociati a GridView (in modo che i record eliminati appena più visualizzati come righe della griglia).

La figura 4 mostra GridView dopo aver selezionato un numero di righe per l'eliminazione. Figura 5 mostra la schermata immediatamente dopo aver scelto il pulsante Elimina prodotti selezionati. Si noti che nella figura 5 di `ProductID` nell'etichetta sotto controllo GridView. vengono visualizzati i valori dei record eliminati e le righe non sono più in GridView.


[![I prodotti selezionati verranno eliminati](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Figura 4**: il selezionati i prodotti verranno eliminati ([fare clic per visualizzare l'immagine ingrandita](batch-deleting-vb/_static/image8.png))


[![I valori di ProductID prodotti eliminato sono elencati sotto il controllo GridView.](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Figura 5**: il prodotti eliminato `ProductID` i valori sono elencati sotto il controllo GridView. ([fare clic per visualizzare l'immagine ingrandita](batch-deleting-vb/_static/image10.png))


> [!NOTE]
> Per testare il `DeleteProductsWithTransaction` atomicità metodo s, aggiungere manualmente una voce per un prodotto di `Order Details` tabella e quindi si tenta di eliminare tale prodotto (insieme ad altri utenti). Si riceverà una violazione di vincolo di chiave esterna quando si tenta di eliminare il prodotto con un ordine associato, ma si noti come le altre operazioni di eliminazione prodotti selezionati viene eseguito il rollback.


## <a name="summary"></a>Riepilogo

Creazione di un batch di eliminazione interfaccia prevede l'aggiunta di un controllo GridView con una colonna di caselle di controllo e un pulsante che, quando selezionato, verranno eliminate tutte le righe selezionate come una singola operazione atomica. In questa esercitazione tale interfaccia è compilata da riunire lavoro svolto in due esercitazioni precedenti, [aggiunta di una colonna di GridView di caselle di controllo](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) e [di wrapping delle modifiche del Database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-vb.md). Nella prima esercitazione è stato creato un GridView con una colonna di caselle di controllo e in quest'ultimo è implementato un metodo in BLL che, quando viene passato un `List(Of T)` di `ProductID` valori, sono stati eliminati nell'ambito di una transazione.

Nella prossima esercitazione si creerà un'interfaccia per l'esecuzione di inserimenti batch.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Hilton Giesenow e Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](batch-updating-vb.md)
> [Successivo](batch-inserting-vb.md)
