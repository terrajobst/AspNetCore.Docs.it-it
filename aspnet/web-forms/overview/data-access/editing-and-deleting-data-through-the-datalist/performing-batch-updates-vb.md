---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Esecuzione di aggiornamenti Batch (VB) | Documenti Microsoft
author: rick-anderson
description: Informazioni su come creare un completamente modificabile DataList in cui tutti gli elementi sono in modalità di modifica e i cui valori possono essere salvati, facendo clic su un pulsante 'Aggiorna tutto' il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d28a431c2b09de8c46079e888aa191017de4e30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "30892387"
---
<a name="performing-batch-updates-vb"></a>Esecuzione di aggiornamenti Batch (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) o [Scarica il PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Informazioni su come creare un completamente modificabile DataList in cui tutti gli elementi sono in modalità di modifica e i cui valori possono essere salvati, facendo clic sul pulsante "Aggiorna tutte" nella pagina.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) abbiamo esaminato come creare un livello di elemento di DataList. Come standard GridView modificabile, ogni elemento DataList incluso una pulsante Modifica che, quando selezionato, rendere modificabile l'elemento. Mentre questo livello di elemento modifica funziona anche per i dati che viene aggiornati solo occasionalmente, alcuni scenari di casi di utilizzo richiedono all'utente di modificare più record. Se un utente deve modificare numerose di record e fare clic su Modifica, apportare le modifiche desiderate e fare clic su Aggiorna per ciascuno di essi viene forzato, la quantità di facendo clic su può compromettere la produttività. In tali situazioni, un'opzione migliore è di fornire un controllo DataList completamente modificabile, dove *tutti* dei relativi elementi sono in modalità di modifica e i cui valori possono essere modificati, fare clic su un pulsante Aggiorna tutte sulla pagina (vedere la figura 1).


[![Ogni elemento in un DataList modificabile può essere modificato](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Figura 1**: ogni elemento in un DataList modificabile può essere modificato ([fare clic per visualizzare l'immagine ingrandita](performing-batch-updates-vb/_static/image3.png))


In questa esercitazione si esamineranno come permettere agli utenti di aggiornare le informazioni sull'indirizzo di fornitori utilizzando un DataList completamente modificabile.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Passaggio 1: Creare l'interfaccia utente modificabile in ItemTemplate s DataList

Nell'esercitazione precedente, in cui è la creazione di un controllo DataList standard, a livello di elemento modificabile, abbiamo utilizzato due modelli:

- `ItemTemplate` contiene l'interfaccia utente di sola lettura (i controlli Web di etichetta per la visualizzazione di ogni elemento name s prodotto e prezzo).
- `EditItemTemplate` contiene l'interfaccia utente in modalità di modifica (i due controlli TextBox Web).

DataList s `EditItemIndex` proprietà stabilisce cosa `DataListItem` (se presente) viene eseguito il rendering tramite il `EditItemTemplate`. In particolare, il `DataListItem` cui `ItemIndex` valore corrisponde a DataList s `EditItemIndex` viene eseguito il rendering di proprietà utilizzando il `EditItemTemplate`. Questo modello funziona bene quando è possibile modificare solo un elemento a un ora, ma divide in due cade durante la creazione di un DataList completamente modificabile.

Per un controllo DataList completamente modificabile, desideriamo *tutti* del `DataListItem` s per il rendering tramite l'interfaccia modificabile. Il modo più semplice per eseguire questa operazione consiste nel definire l'interfaccia modificabile nel `ItemTemplate`. Per modificare le informazioni sull'indirizzo di fornitori, l'interfaccia modificabile contiene il nome del fornitore come testo e caselle di testo per l'indirizzo, città e i valori di paese.

Aprire il `BatchUpdate.aspx` pagina, aggiungere un controllo DataList e impostare il relativo `ID` proprietà `Suppliers`. Smart tag DataList s, scegliere di aggiungere un nuovo controllo ObjectDataSource denominato `SuppliersDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Figura 2**: creare un nuovo denominato ObjectDataSource `SuppliersDataSource` ([fare clic per visualizzare l'immagine ingrandita](performing-batch-updates-vb/_static/image6.png))


Configurare ObjectDataSource per recuperare dati tramite il `SuppliersBLL` classe s `GetSuppliers()` (metodo) (vedere la figura 3). Con l'esercitazione precedente, piuttosto che l'aggiornamento delle informazioni del fornitore tramite ObjectDataSource, è necessario utilizzare direttamente il livello di logica di Business. Pertanto, impostare l'elenco a discesa su (nessuno) nella scheda aggiornamento (vedere la figura 4).


[![Recuperare informazioni sul fornitore tramite il metodo GetSuppliers()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Figura 3**: recuperare informazioni sul fornitore utilizzando il `GetSuppliers()` metodo ([fare clic per visualizzare l'immagine ingrandita](performing-batch-updates-vb/_static/image9.png))


[![Impostazione dell'elenco di riepilogo a discesa su (nessuno) nella scheda dell'aggiornamento](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Figura 4**: impostare l'elenco a discesa su (nessuno) nella scheda aggiornamento ([fare clic per visualizzare l'immagine ingrandita](performing-batch-updates-vb/_static/image12.png))


Dopo aver completato la procedura guidata, Visual Studio genera automaticamente DataList s `ItemTemplate` per visualizzare ogni campo di dati restituito dall'origine dati in un controllo etichetta Web. È necessario modificare il modello in modo che fornisce l'interfaccia di modifica. Il `ItemTemplate` può essere personalizzato tramite la finestra di progettazione utilizzando l'opzione di modifica modelli dallo smart tag DataList s o direttamente tramite la sintassi dichiarativa.

Richiedere qualche istante per creare un'interfaccia di modifica che visualizza il nome del fornitore s come testo, ma include caselle di testo per il fornitore s indirizzo, città e i valori di paese. Dopo aver apportato queste modifiche, la sintassi dichiarativa s della pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Come con l'esercitazione precedente DataList in questa esercitazione deve essere attivato lo stato di visualizzazione.


Nel `ItemTemplate` si m utilizzando due nuove classi CSS, `SupplierPropertyLabel` e `SupplierPropertyValue`, che sono state aggiunte per la `Styles.css` classe e configurato per utilizzare le stesse impostazioni di stile il `ProductPropertyLabel` e `ProductPropertyValue` classi CSS.


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Dopo aver apportato queste modifiche, visitare la pagina tramite un browser. Come illustrato nella figura 5, ogni elemento DataList Visualizza il nome del fornitore come testo e usato nelle caselle di testo per visualizzare l'indirizzo, città e paese.


[![Ogni fornitore in DataList è modificabile](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Figura 5**: ogni fornitore in DataList è modificabile ([fare clic per visualizzare l'immagine ingrandita](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Passaggio 2: Aggiunta di un aggiornamento pulsante tutto

Mentre ogni fornitore nella figura 5 con indirizzo, città e i campi del paese visualizzati in una casella di testo, attualmente non disponibile alcun pulsante di aggiornamento. Invece di un pulsante Aggiorna per ogni elemento, con DataList completamente modificabile è in genere un solo pulsante Aggiorna tutte sulla pagina che, quando si fa clic, Aggiorna *tutti* dei record di DataList. Per questa esercitazione, consentire s aggiungere due pulsanti Aggiorna tutto - uno nella parte superiore della pagina e l'altro nella parte inferiore (anche se facendo clic su uno dei pulsanti avrà lo stesso effetto).

Inizio aggiungendo un controllo pulsante Web di sopra del DataList e impostare il relativo `ID` proprietà `UpdateAll1`. Successivamente, aggiungere il secondo controllo pulsante Web sotto il controllo DataList, l'impostazione relativa `ID` a `UpdateAll2`. Impostare il `Text` le proprietà per i due pulsanti per l'aggiornamento tutti. Infine, creare i gestori eventi per entrambi i pulsanti `Click` eventi. Invece di duplicare la logica di aggiornamento in ognuno dei gestori eventi, s consentono di effettuare il refactoring di quella logica a un terzo metodo, `UpdateAllSupplierAddresses`, con i gestori di eventi semplicemente richiamare questo metodo terzo.


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Figura 6 mostra la pagina dopo l'aggiornamento a tutti i pulsanti sono stati aggiunti.


[![Due aggiornamento tutti i pulsanti sono stati aggiunti alla pagina](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Figura 6**: due aggiornamento tutti i pulsanti sono stati aggiunti alla pagina ([fare clic per visualizzare l'immagine ingrandita](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Passaggio 3: Aggiornamento di tutte le informazioni di indirizzo fornitori

Con tutti gli elementi DataList s visualizzare l'interfaccia di modifica e l'aggiunta di aggiornare tutti i pulsanti, che rimane sta scrivendo il codice per eseguire l'aggiornamento batch. In particolare, è necessario scorrere gli elementi DataList s e chiamata di `SuppliersBLL` classe s `UpdateSupplierAddress` metodo per ognuno di essi.

La raccolta di `DataListItem` istanze di tale struttura DataList saranno accessibili tramite DataList s [ `Items` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Con un riferimento a un `DataListItem`, è possibile acquisire corrispondente `SupplierID` dal `DataKeys` raccolta e a livello di programmazione TextBox Web controlli all'interno di riferimento di `ItemTemplate` come illustrato nel codice seguente:


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Quando l'utente fa clic su uno dei pulsanti, Aggiorna tutte le `UpdateAllSupplierAddresses` metodo scorre ogni `DataListItem` nel `Suppliers` DataList e chiama il `SuppliersBLL` classe s `UpdateSupplierAddress` , passando i valori corrispondenti. Un valore non è stato immesso per indirizzo, città o paese passa è un valore di `Nothing` a `UpdateSupplierAddress` (anziché una stringa vuota), che comporta un database `NULL` per i campi di record s sottostante.

> [!NOTE]
> In alternativa, si desidera aggiungere un controllo etichetta Web stato alla pagina che fornisce un messaggio di conferma dopo l'aggiornamento batch viene eseguito.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>L'aggiornamento solo gli indirizzi che sono stati modificati

L'algoritmo di aggiornamento batch utilizzato per le chiamate di questa esercitazione il `UpdateSupplierAddress` metodo per *ogni* fornitore in DataList, indipendentemente dal fatto che le relative informazioni di indirizzo sono state modificate. Mentre tale blind aggiornamenti non in genere un problema di prestazioni, se si stanno controllo modifica alla tabella di database può causare superflue dei record. Ad esempio, se si utilizzano trigger per registrare tutti `UPDATE` s per il `Suppliers` tabella a una tabella di controllo, ogni volta che un utente fa clic sul pulsante Aggiorna tutto verrà creato un nuovo record di controllo per ogni fornitore nel sistema, indipendentemente dal fatto se l'utente rese qualsiasi modifiche.

Le classi DataTable di ADO.NET e DataAdapter sono progettate per supportare gli aggiornamenti di batch in cui i record di nuovi, eliminati e modificati solo comporta le comunicazioni di database. Ogni riga nella DataTable dispone un [ `RowState` proprietà](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) che indica se la riga è stata aggiunta alla tabella di dati eliminato da quest'ultimo, modifica, o non subisce modifiche. Quando un oggetto DataTable inizialmente è popolato, tutte le righe contrassegnate subisce modifiche. Modifica del valore di una o più colonne s riga la riga viene contrassegnata come modificata.

Nel `SuppliersBLL` classe è aggiornare le informazioni sull'indirizzo specificato fornitore s mediante la lettura prima del record singolo fornitore in un `SuppliersDataTable` e quindi impostare il `Address`, `City`, e `Country` i valori di colonna utilizzando il codice seguente:


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Questo codice assegna gestire i passato in indirizzo, città, paese valori e per il `SuppliersRow` nel `SuppliersDataTable` indipendentemente dal fatto che i valori sono stati modificati. Tali modifiche causano il `SuppliersRow` s `RowState` proprietà sarà contrassegnata come modificata. Quando s il livello di accesso ai dati `Update` metodo viene chiamato, viene rilevato che il `SupplierRow` è stato modificato e pertanto invia un `UPDATE` comando nel database.

Si supponga, tuttavia, che il codice è stato aggiunto a questo metodo per assegnare solo l'indirizzo passato, city e valori di paese se differiscono dal `SuppliersRow` valori esistenti s. Nel caso in cui l'indirizzo, città e paese sono come i dati esistenti, non verrà apportata alcuna modifica e `SupplierRow` s `RowState` rimarrà invariata contrassegnato come di sinistra. Il risultato finale è che quando i dispositivi DAL `Update` metodo viene chiamato, verrà effettuata alcuna chiamata di database perché il `SuppliersRow` non è stato modificato.

Per rendere effettiva questa modifica, sostituire le istruzioni che ciecamente assegnare l'indirizzo passato, città, paese i valori e con il codice seguente:


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Con questo aggiunto codice, i dispositivi DAL `Update` metodo invia un `UPDATE` istruzione per il database solo i record i cui valori relativi a indirizzi sono stati modificati.

In alternativa, è possibile tenere traccia del se sono presenti eventuali differenze tra i campi indirizzo passato e i dati del database e, se non sono presenti, ignora semplicemente la chiamata a s DAL `Update` metodo. Questo approccio funziona anche se si stanno utilizzando il database di metodo, diretto trascorsi da t DB dirette del metodo non è un `SuppliersRow` istanza il cui `RowState` può essere controllato per determinare se una chiamata al database è effettivamente necessario.

> [!NOTE]
> Ogni volta che il `UpdateSupplierAddress` metodo viene richiamato, viene eseguita una chiamata al database per recuperare informazioni sui record aggiornato. Quindi, se sono state apportate modifiche nei dati, viene eseguita un'altra chiamata al database per aggiornare la riga della tabella. Questo flusso di lavoro può essere ottimizzata mediante la creazione di un `UpdateSupplierAddress` overload del metodo che accetta un `EmployeesDataTable` istanza che dispone di *tutti* delle modifiche del `BatchUpdate.aspx` pagina. Quindi, è possibile apportare una chiamata al database per ottenere tutti i record di `Suppliers` tabella. Quindi è stato possibile enumerare i due set di risultati ed è stato possibile aggiornare solo i record in cui sono state apportate modifiche.


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come creare un controllo DataList completamente modificabile, consentendo all'utente di modificare rapidamente le informazioni sull'indirizzo per più fornitori. È stato avviato mediante la definizione di interfaccia per la modifica di un controllo casella di testo Web per il fornitore s indirizzo, città e i valori di paese in DataList s `ItemTemplate`. Successivamente, abbiamo aggiunto aggiornamento tutti i pulsanti sopra e sotto il controllo DataList. Dopo che un utente è apportate le modifiche e fa clic su uno dei pulsanti, Aggiorna tutte le `DataListItem` s vengono enumerati e una chiamata al `SuppliersBLL` classe s `UpdateSupplierAddress` metodo.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Zack Jones e Ken Pespisa. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [Successivo](handling-bll-and-dal-level-exceptions-vb.md)
