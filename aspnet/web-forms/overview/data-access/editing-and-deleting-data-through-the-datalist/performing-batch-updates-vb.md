---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Esecuzione di aggiornamenti Batch (VB) | Microsoft Docs
author: rick-anderson
description: Informazioni su come creare un completamente modificabile DataList in cui tutti i relativi elementi sono in modalità di modifica e i cui valori possono essere salvati facendo clic su un pulsante "Aggiorna tutto" il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: a877bcc5b26965d59065aa17959643b66a5c4484
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388493"
---
<a name="performing-batch-updates-vb"></a>Esecuzione di aggiornamenti Batch (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) o [Scarica il PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Informazioni su come creare un completamente modificabile DataList in cui tutti i relativi elementi sono in modalità di modifica e i cui valori possono essere salvati, fare clic su un pulsante "Aggiorna tutto" nella pagina.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) abbiamo esaminato come creare un livello di elemento di DataList. Ad esempio GridView modificabile standard, ogni elemento di DataList incluso una modifica sul pulsante che, quando selezionato, renderebbero l'elemento modificabile. Anche se questo elemento a livello di modifica è ottimale per i dati che viene aggiornati solo in alcuni casi, alcuni scenari dei casi d'uso richiedono all'utente di modificare più record. Se un utente deve modificare decine di record e viene forzato a fare clic su Modifica, apportare le modifiche desiderate e fare clic su Aggiorna per ognuno di essi, la quantità di facendo clic può compromettere la propria produttività. In tali situazioni, un'opzione migliore consiste nel fornire un controllo DataList completamente modificabile, dove *tutti* dei relativi elementi sono in modalità di modifica e i cui valori possono essere modificati, fare clic su un pulsante Aggiorna tutte sulla pagina (vedere la figura 1).


[![Ogni elemento in un controllo DataList modificabile completamente può essere modificato.](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Figura 1**: può essere modificato ogni elemento in un controllo DataList modificabile completamente ([fare clic per visualizzare l'immagine con dimensioni normali](performing-batch-updates-vb/_static/image3.png))


In questa esercitazione verrà esaminato come consentire agli utenti di aggiornare informazioni sull'indirizzo di fornitori utilizzando un controllo DataList interamente modificabili.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Passaggio 1: Creare l'interfaccia utente modificabili di ItemTemplate s DataList

Nell'esercitazione precedente, in cui è la creazione di un controllo DataList standard, a livello di elemento modificabile, venivano utilizzati due modelli:

- `ItemTemplate` contenuto dell'interfaccia utente di sola lettura (i controlli Web di etichetta per la visualizzazione di ogni s nome prodotto e prezzo).
- `EditItemTemplate` contiene l'interfaccia utente in modalità di modifica (i due controlli TextBox Web).

S DataList `EditItemIndex` proprietà stabilisce cosa `DataListItem` (se presente) viene eseguito il rendering tramite il `EditItemTemplate`. In particolare, il `DataListItem` la cui `ItemIndex` valore corrisponde a DataList s `EditItemIndex` viene eseguito il rendering di proprietà utilizzando il `EditItemTemplate`. Questo modello funziona bene quando solo un elemento può essere modificato in un momento, ma distanti tra loro non rientra durante la creazione di un controllo DataList interamente modificabili.

Per un controllo DataList completamente modificabile, vogliamo *tutte* del `DataListItem` s per il rendering usando l'interfaccia modificabile. Il modo più semplice per eseguire questa operazione consiste nel definire l'interfaccia modificabile nel `ItemTemplate`. Per modificare le informazioni sull'indirizzo di fornitori, l'interfaccia modificabile contiene il nome del fornitore come testo e quindi nelle caselle di testo per l'indirizzo, città e valori del paese.

Iniziare aprendo il `BatchUpdate.aspx` pagina, aggiungere un controllo DataList e impostare relativi `ID` proprietà `Suppliers`. Nello smart tag, DataList s scegliere di aggiungere un nuovo controllo ObjectDataSource denominato `SuppliersDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Figura 2**: creare un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](performing-batch-updates-vb/_static/image6.png))


Configurare ObjectDataSource per recuperare i dati usando il `SuppliersBLL` classe s `GetSuppliers()` (metodo) (vedere la figura 3). Come con l'esercitazione precedente anziché aggiornare le informazioni del fornitore tramite ObjectDataSource, sarà lavoriamo direttamente con il livello di logica di Business. Pertanto, impostare l'elenco a discesa su (nessuno) nella scheda aggiornamenti (vedere la figura 4).


[![Recuperare le informazioni sul fornitore utilizzando il metodo GetSuppliers()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Figura 3**: recuperare informazioni sul fornitore utilizzando i `GetSuppliers()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](performing-batch-updates-vb/_static/image9.png))


[![Impostare l'elenco a discesa su (nessuno) nella scheda aggiornamenti](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Figura 4**: impostare l'elenco a discesa su (nessuno) nella scheda aggiornamenti ([fare clic per visualizzare l'immagine con dimensioni normali](performing-batch-updates-vb/_static/image12.png))


Dopo aver completato la procedura guidata, Visual Studio genera automaticamente il controllo DataList s `ItemTemplate` per visualizzare ogni campo di dati restituito dall'origine dati in un controllo etichetta Web. È necessario modificare il modello in modo che fornisca invece l'interfaccia di modifica. Il `ItemTemplate` può essere personalizzato tramite la finestra di progettazione utilizzando l'opzione di modifica modelli dello smart tag DataList s o direttamente tramite la sintassi dichiarativa.

Si consiglia di creare un'interfaccia di modifica che visualizza il nome del fornitore s come testo, ma include caselle di testo per il fornitore s indirizzo, città e valori del paese. Dopo aver apportato queste modifiche, la sintassi dichiarativa per s pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Come con l'esercitazione precedente, DataList in questa esercitazione è necessario il proprio stato abilitato.


Nel `ItemTemplate` ho m usando due nuove classi CSS, `SupplierPropertyLabel` e `SupplierPropertyValue`, che sono state aggiunte ad il `Styles.css` classe e configurato per usare le stesse impostazioni di stile del `ProductPropertyLabel` e `ProductPropertyValue` classi CSS.


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Dopo aver apportato queste modifiche, visita questa pagina tramite un browser. Come illustrato nella figura 5, ogni elemento DataList Visualizza il nome del fornitore come testo e utilizzata nelle caselle di testo per visualizzare l'indirizzo, città e paese.


[![Ogni fornitore in DataList è modificabile](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Figura 5**: ogni fornitore in DataList è modificabile ([fare clic per visualizzare l'immagine con dimensioni normali](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Passaggio 2: Aggiunta di un aggiornamento pulsante tutto

Mentre ogni fornitore nella figura 5 ha relativo indirizzo, città e paese campi visualizzati in una casella di testo, non è attualmente alcun pulsante di aggiornamento disponibile. Evitando che un pulsante Aggiorna per ogni elemento, con DataList completamente modificabile è in genere un singolo pulsante Aggiorna tutto nella pagina che, quando si fa clic, viene aggiornato *tutti* dei record di controllo DataList. Per questa esercitazione, lasciare s aggiungere due pulsanti Aggiorna tutto - uno nella parte superiore della pagina e l'altro nella parte inferiore (sebbene facendo clic su dei pulsanti avrà lo stesso effetto).

Avvio tramite l'aggiunta di un controllo Web Button di sopra di DataList e set relativo `ID` proprietà `UpdateAll1`. Successivamente, aggiungere il secondo controllo Web Button di sotto di DataList, l'impostazione relativa `ID` a `UpdateAll2`. Impostare il `Text` le proprietà per i due pulsanti a All Update. Infine, creare gestori eventi per entrambi i pulsanti `Click` gli eventi. Invece di duplicare la logica di aggiornamento in ciascuno dei gestori di eventi, ti permettono di s, effettuare il refactoring che per la logica a un terzo metodo `UpdateAllSupplierAddresses`, con i gestori di eventi è sufficiente richiamare questo metodo terzo.


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Figura 6 mostra la pagina dopo l'aggiornamento tutti i pulsanti sono stati aggiunti.


[![Due aggiornamento tutti i pulsanti sono stati aggiunti alla pagina](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Figura 6**: due aggiornamento tutti i pulsanti sono stati aggiunti alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Passaggio 3: Aggiornamento di tutte le informazioni di indirizzo Suppliers

Con tutti gli elementi s DataList visualizzare l'interfaccia di modifica e l'aggiunta di aggiornare tutti i pulsanti, che rimane sta scrivendo il codice per eseguire l'aggiornamento batch. In particolare, è necessario eseguire un ciclo tramite le voci di DataList s e una chiamata di `SuppliersBLL` classe s `UpdateSupplierAddress` metodo per ognuno di essi.

La raccolta di `DataListItem` istanze di tale struttura DataList sono accessibili tramite DataList s [ `Items` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Con un riferimento a un `DataListItem`, è possibile recuperare il corrispondente `SupplierID` dal `DataKeys` raccolta e a livello di programmazione riferimento Web nella casella di testo controlli all'interno di `ItemTemplate` come illustrato nel codice seguente:


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Quando l'utente fa clic su uno dei pulsanti, Aggiorna tutte le `UpdateAllSupplierAddresses` metodo scorre ogni `DataListItem` nel `Suppliers` DataList e chiama il `SuppliersBLL` classe s `UpdateSupplierAddress` , passando i valori corrispondenti. Un valore per l'indirizzo, città o paese passa immesso non è un valore di `Nothing` al `UpdateSupplierAddress` (anziché a una stringa vuota), in modo da un database `NULL` per i campi di record s sottostante.

> [!NOTE]
> Come ulteriore miglioramento, è possibile aggiungere uno stato controllo etichetta Web alla pagina che fornisce un messaggio di conferma dopo l'aggiornamento batch viene eseguito.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>L'aggiornamento solo gli indirizzi che sono stati modificati

L'algoritmo di aggiornamento batch utilizzato per le chiamate di questa esercitazione il `UpdateSupplierAddress` metodo per *ogni* fornitore in DataList, indipendentemente dal fatto che le informazioni sull'indirizzo è stati modificati. Mentre tale blind Aggiorna t sono in genere un problema di prestazioni, se si ri il controllo viene modificata alla tabella di database può causare ai record superfluo. Ad esempio, se si utilizzano trigger per registrare tutti `UPDATE` s per il `Suppliers` tabella a una tabella di controllo, ogni volta che un utente fa clic sul pulsante Aggiorna tutto verrà creato un nuovo record di controllo per ogni fornitore nel sistema, indipendentemente dal fatto che l'utente eseguita ogni modifiche.

Le classi DataTable di ADO.NET e DataAdapter sono progettate per supportare gli aggiornamenti in batch in cui solo modificati, eliminati e nuovi record risultante in tutte le comunicazioni del database. Ogni riga nella DataTable ha un [ `RowState` proprietà](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) che indica se la riga è stato aggiunto alla tabella di dati, da quest'ultimo, modificati, eliminato o rimane invariata. Quando un oggetto DataTable inizialmente è popolato, tutte le righe sono contrassegnate invariate. Modifica del valore di una o più colonne s riga la riga viene contrassegnata come modificata.

Nel `SuppliersBLL` classe le informazioni sull'indirizzo specificato supplier s viene aggiornato da leggere prima l'articolo nel record unico fornitore in un `SuppliersDataTable` e quindi impostare il `Address`, `City`, e `Country` i valori di colonna usando il codice seguente:


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Gestire questo codice viene assegnato l'indirizzo passato, città e valori del paese per la `SuppliersRow` nella `SuppliersDataTable` indipendentemente dal fatto o meno i valori sono stati modificati. Tali modifiche causano il `SuppliersRow` s `RowState` proprietà deve essere contrassegnato come modificato. Quando il livello di accesso ai dati di s `Update` viene chiamato il metodo, vede che il `SupplierRow` è stato modificato e pertanto invia un `UPDATE` comando nel database.

Si supponga, tuttavia, che è stato aggiunto codice a questo metodo solo assegnare l'indirizzo passato, città e paese valori se sono diversi dal `SuppliersRow` i valori esistenti di s. Nel caso in cui l'indirizzo, città e paese sono le stesse i dati esistenti, non sarà possibile apportare modifiche e il `SupplierRow` s `RowState` resteranno inalterati contrassegnati come left. Il risultato è che quando i dispositivi DAL `Update` viene chiamato il metodo, poiché non sarà necessario eseguire alcuna chiamata al database di `SuppliersRow` non è stato modificato.

Per applicare questa modifica, sostituire le istruzioni che alla cieca assegnare l'indirizzo passato, città e valori del paese con il codice seguente:


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Con questo codice, i dispositivi DAL aggiunto `Update` metodo invia un `UPDATE` istruzione per il database solo i record i cui valori correlate agli indirizzi sono stati modificati.

In alternativa, è possibile tenere traccia degli se sono presenti eventuali differenze tra i campi indirizzo passato e i dati del database e, se non vi sono, ignorare semplicemente la chiamata a s DAL `Update` (metodo). Questo approccio funziona bene se metodo, si stia usando il database diretto poiché passato t DB metodo diretto non è un `SuppliersRow` istanza cui `RowState` può essere controllato per determinare se una chiamata al database è effettivamente necessario.

> [!NOTE]
> Ogni volta che il `UpdateSupplierAddress` metodo viene richiamato, viene eseguita una chiamata al database per recuperare informazioni su del record aggiornato. Quindi, se sono state apportate modifiche nei dati, viene eseguita un'altra chiamata al database per aggiornare la riga della tabella. Questo flusso di lavoro può essere ottimizzata mediante la creazione di un' `UpdateSupplierAddress` overload del metodo che accetta un `EmployeesDataTable` istanza che ha *tutti* delle modifiche dal `BatchUpdate.aspx` pagina. Quindi, potrebbe verificare una chiamata al database per ottenere tutti i record dal `Suppliers` tabella. I due set di risultati può quindi essere enumerati ed è stato possibile aggiornare solo i record in cui sono state apportate modifiche.


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come creare un controllo DataList completamente modificabile, che consente all'utente di modificare rapidamente le informazioni sull'indirizzo per diversi fornitori. Abbiamo iniziato con la definizione di interfaccia per la modifica di un controllo TextBox Web per l'indirizzo di fornitore, città e valori del paese in DataList s `ItemTemplate`. Successivamente, abbiamo aggiunto aggiornamento tutti i pulsanti sopra e sotto il controllo DataList. Dopo che un utente ha effettuato le modifiche e fatto clic su uno dei pulsanti Aggiorna tutto, il `DataListItem` s vengono enumerati e una chiamata per il `SuppliersBLL` classe s `UpdateSupplierAddress` metodo è costituito.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Zack Jones e Ken Pespisa. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [Successivo](handling-bll-and-dal-level-exceptions-vb.md)
