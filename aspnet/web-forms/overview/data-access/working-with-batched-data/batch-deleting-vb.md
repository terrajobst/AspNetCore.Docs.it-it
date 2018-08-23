---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Batch di eliminazione (VB) | Microsoft Docs
author: rick-anderson
description: Informazioni su come eliminare più record di database in un'unica operazione. Nel livello di interfaccia utente si basano un GridView avanzato creati in un precedente tut...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: ec3560f31f2def1801f9d09b3b583de6cdabc834
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834850"
---
<a name="batch-deleting-vb"></a>Eliminazione batch (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) o [Scarica il PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Informazioni su come eliminare più record di database in un'unica operazione. Nel livello di interfaccia utente si basano un GridView avanzato creati in un'esercitazione precedente. Nel livello di accesso ai dati è eseguire il wrapping di più operazioni di eliminazione in una transazione per garantire che tutte le eliminazioni di esito positivo o vengano eseguito il rollback di tutte le eliminazioni.


## <a name="introduction"></a>Introduzione

Il [esercitazione precedente](batch-updating-vb.md) esaminato come creare un batch Modifica interfaccia utilizzando un controllo GridView interamente modificabili. In situazioni in cui gli utenti sono in genere modifica numero di record in una sola volta, un batch di interfaccia di modifica richiederà meno i postback e il contesto di tastiera per mouse commutatori, migliorando l'efficienza di s utente finale. Questa tecnica è utile in modo analogo per le pagine in cui è comune per gli utenti di eliminare il numero di record in un'unica operazione.

Chiunque abbia usato un client di posta elettronica online ha già familiarità con uno dei batch di più comune l'eliminazione di interfacce: pulsante di una casella di controllo in ogni riga in una griglia con una corrispondente eliminare tutti gli elementi selezionati (vedere la figura 1). Questa esercitazione è piuttosto breve perché è stato già fatto tutto il lavoro difficile nelle esercitazioni precedenti nella creazione sia l'interfaccia basata sul web e un metodo per eliminare una serie di record di una singola operazione atomica. Nel [aggiunta di una colonna GridView di caselle di controllo](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) esercitazione viene creato un controllo GridView con una colonna di caselle di controllo e nel [wrapping delle modifiche al Database in una transazione](wrapping-database-modifications-within-a-transaction-vb.md) esercitazione è stato creato un metodo in il livello BLL che usa una transazione per eliminare un `List<T>` di `ProductID` valori. In questa esercitazione verranno si basano e nostre esperienze precedenti per creare un batch di lavoro l'eliminazione di esempio di tipo merge.


[![Ogni riga include una casella di controllo](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Figura 1**: ogni riga include una casella di controllo ([fare clic per visualizzare l'immagine con dimensioni normali](batch-deleting-vb/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Passaggio 1: Creazione di Batch di eliminazione dell'interfaccia

Poiché è già stato creato l'eliminazione di interfaccia in batch le [aggiunta di una colonna GridView di caselle di controllo](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) esercitazione, è possibile semplicemente copiarlo `BatchDelete.aspx` anziché crearlo da zero. Iniziare aprendo il `BatchDelete.aspx` nella pagina la `BatchData` cartella e il `CheckBoxField.aspx` nella pagina di `EnhancedGridView` cartella. Dal `CheckBoxField.aspx` pagina, passare alla visualizzazione origine e copiare il codice tra il `<asp:Content>` tag come illustrato nella figura 2.


[![Copiare il Markup dichiarativo di CheckBoxField.aspx negli Appunti](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Figura 2**: Markup dichiarativo di copiare `CheckBoxField.aspx` negli Appunti ([fare clic per visualizzare l'immagine con dimensioni normali](batch-deleting-vb/_static/image4.png))


Successivamente, passare alla visualizzazione origine nella `BatchDelete.aspx` e incollare il contenuto degli Appunti all'interno di `<asp:Content>` tag. Copiare e incollare il codice all'interno della classe code-behind in anche `CheckBoxField.aspx.vb` all'interno della classe code-behind in `BatchDelete.aspx.vb` (il `DeleteSelectedProducts` pulsante s `Click` gestore eventi, il `ToggleCheckState` metodo e il `Click` gestori eventi per il `CheckAll` e `UncheckAll` pulsanti). Dopo aver copiato questo contenuto, il `BatchDelete.aspx` classe code-behind pagina s deve contenere il codice seguente:


[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Dopo aver copiato il markup dichiarativo e codice sorgente, si consiglia di testare `BatchDelete.aspx` visualizzandolo tramite un browser. Verrà visualizzato un elenco dei primi dieci prodotti in un controllo GridView con ogni riga Elenca il nome del prodotto s, categoria e prezzo insieme a una casella di controllo di GridView. Dovrebbero essere presenti tre pulsanti: controllare tutti deselezionare tutti i prodotti ed eliminare selezionata. Facendo clic sul pulsante Seleziona tutto consente di selezionare tutte le caselle di controllo, mentre tutte deselezionare Cancella tutte le caselle di controllo. Facendo clic su Elimina prodotti selezionati viene visualizzato un messaggio in cui sono elencati i `ProductID` i valori dei prodotti selezionati, ma non elimina effettivamente i prodotti.


[![L'interfaccia da CheckBoxField.aspx è stata spostata in BatchDeleting.aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Figura 3**: l'interfaccia da `CheckBoxField.aspx` è stata spostata `BatchDeleting.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](batch-deleting-vb/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Passaggio 2: Eliminare i prodotti selezionati utilizzando transazioni

Con il batch di eliminazione dell'interfaccia è stata copiata in `BatchDeleting.aspx`, resta per aggiornare il codice in modo che il pulsante di eliminare i prodotti selezionati Elimina i prodotti selezionati tramite i `DeleteProductsWithTransaction` metodo nel `ProductsBLL` classe. Questo metodo, aggiunto nel [di wrapping delle modifiche al Database in una transazione](wrapping-database-modifications-within-a-transaction-vb.md) esercitazione accetta come input un `List(Of T)` dei `ProductID` valori ed elimina ogni corrispondente `ProductID` all'interno dell'ambito di un transazione.

Il `DeleteSelectedProducts` pulsante s `Click` gestore dell'evento attualmente utilizza il seguente `For Each` ciclo per scorrere ogni riga GridView:


[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Per ogni riga, il `ProductSelector` controllo casella di controllo Web a livello di codice viene fatto riferimento. Se è selezionata, la riga s `ProductID` viene recuperato dal `DataKeys` raccolta e il `DeleteResults` etichetta s `Text` proprietà viene aggiornata per includere un messaggio che indica che la riga è stata selezionata per l'eliminazione.

Il codice sopra riportato non elimina effettivamente i record come la chiamata ai `ProductsBLL` classe s `Delete` metodo viene impostata come commento. Sono stati questa logica di eliminazione da applicare, il codice eliminerebbe i prodotti, ma non all'interno di un'operazione atomica. Vale a dire, se le eliminazioni di alcuni prima nella sequenza ha avuto esito positivo, ma una versione più recente non riuscito (forse a causa di una violazione di vincolo di chiave esterna), verrebbe generata un'eccezione mentre rimangono eliminati i prodotti già eliminati.

Per garantire l'atomicità, è necessario usare invece i `ProductsBLL` classe s `DeleteProductsWithTransaction` (metodo). Poiché questo metodo accetta un elenco di `ProductID` valori, è necessario innanzitutto compilare questo elenco dalla griglia e quindi passarlo come parametro. Viene innanzitutto creata un'istanza di un `List(Of T)` di tipo `Integer`. All'interno di `For Each` ciclo è necessario aggiungere prodotti selezionati `ProductID` valori a questo `List(Of T)`. Dopo il ciclo ciò `List(Of T)` deve essere passato per il `ProductsBLL` classe s `DeleteProductsWithTransaction` (metodo). Aggiorna il `DeleteSelectedProducts` pulsante s `Click` gestore dell'evento con il codice seguente:


[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

Crea il codice aggiornato un `List(Of T)` typu `Integer` (`productIDsToDelete`) e la popola con i `ProductID` valori da eliminare. Dopo il `For Each` ciclo, se è presente almeno un prodotto selezionato, il `ProductsBLL` classe s `DeleteProductsWithTransaction` metodo viene chiamato e passato questo elenco. Il `DeleteResults` etichetta viene inoltre visualizzato e i dati riassociati a GridView (in modo che il record appena eliminata non saranno più visualizzati come righe nella griglia).

Figura 4 illustra il controllo GridView dopo aver selezionato un numero di righe per l'eliminazione. Figura 5 mostra la schermata immediatamente dopo che è stato fatto clic sul pulsante Elimina i prodotti selezionati. Si noti che nella figura 5 il `ProductID` valori del record eliminati vengono visualizzati nell'etichetta sotto il controllo GridView e tali righe non sono più in GridView.


[![Verranno eliminati i prodotti selezionati](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Figura 4**: The selezionato prodotti verranno eliminati ([fare clic per visualizzare l'immagine con dimensioni normali](batch-deleting-vb/_static/image8.png))


[![I valori di ProductID prodotti eliminati vengono elencate di sotto di GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Figura 5**: di prodotti eliminati `ProductID` i valori sono elencati sotto GridView ([fare clic per visualizzare l'immagine con dimensioni normali](batch-deleting-vb/_static/image10.png))


> [!NOTE]
> Per testare la `DeleteProductsWithTransaction` atomicità metodo s, aggiungere manualmente una voce per un prodotto nel `Order Details` di tabella, quindi tentare di eliminare tale prodotto (insieme ad altri utenti). Si riceverà una violazione di vincolo di chiave esterna quando si prova a eliminare il prodotto con un ordine associato, ma si noti come il rollback all'eliminazione di altri prodotti selezionati.


## <a name="summary"></a>Riepilogo

Creazione di un batch di eliminazione dell'interfaccia prevede l'aggiunta di un controllo GridView con una colonna di caselle di controllo e controllo Web un pulsante che, quando selezionato, verranno eliminate tutte le righe selezionate come una singola operazione atomica. In questa esercitazione è stato creato questo tipo di interfaccia da riunire a lavorare in due esercitazioni precedenti [aggiunta di una colonna GridView di caselle di controllo](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) e [wrapping delle modifiche al Database in una transazione](wrapping-database-modifications-within-a-transaction-vb.md). Nella prima esercitazione viene creato un controllo GridView con una colonna di caselle di controllo e in quest'ultimo è implementato un metodo nel livello BLL che, quando viene passato un `List(Of T)` di `ProductID` li eliminati i valori, tutto all'interno dell'ambito di una transazione.

Nella prossima esercitazione si creerà un'interfaccia per l'esecuzione di inserimenti batch.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Hilton Giesenow e Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](batch-updating-vb.md)
> [Successivo](batch-inserting-vb.md)
