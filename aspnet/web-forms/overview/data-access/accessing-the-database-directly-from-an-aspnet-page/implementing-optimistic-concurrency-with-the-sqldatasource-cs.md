---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: Implementazione della concorrenza ottimistica con SqlDataSource (c#) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione è esaminare gli aspetti principali di controllo della concorrenza ottimistica e quindi esplorare come implementarlo mediante il controllo SqlDataSource."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 69ba9e47071956385e96a28372454a3ae93ccc89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Implementazione della concorrenza ottimistica con SqlDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) o [Scarica il PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> In questa esercitazione è esaminare gli aspetti principali di controllo della concorrenza ottimistica e quindi esplorare come implementarlo mediante il controllo SqlDataSource.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente abbiamo esaminato come aggiungere inserimento, aggiornamento ed eliminazione di funzionalità per il controllo SqlDataSource. In breve, per fornire queste funzionalità è necessario specificare il corrispondente `INSERT`, `UPDATE`, o `DELETE` istruzione SQL nel controllo s `InsertCommand`, `UpdateCommand`, o `DeleteCommand` proprietà insieme ai appropriato parametri di `InsertParameters`, `UpdateParameters`, e `DeleteParameters` raccolte. Anche se queste proprietà e raccolte possono essere specificate manualmente, sul pulsante Avanzate di creazione guidata s Configura origine dati offre un genera `INSERT`, `UPDATE`, e `DELETE` istruzioni casella di controllo che crea queste istruzioni in base al `SELECT` istruzione.

Insieme a genera `INSERT`, `UPDATE`, e `DELETE` istruzioni casella di controllo, la finestra di dialogo Opzioni di generazione SQL avanzate include un'opzione di concorrenza ottimistica utilizzare (vedere la figura 1). Se selezionata, il `WHERE` clausole di generato automaticamente `UPDATE` e `DELETE` sono state modificate le istruzioni per eseguire solo l'aggiornamento o eliminazione se t ancora dati database sottostante è stato modificato dall'utente ultimo caricamento dei dati nella griglia.


![È possibile aggiungere il supporto della concorrenza ottimistica da avanzate la finestra di dialogo Opzioni di generazione SQL](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Figura 1**: È possibile aggiungere il supporto della concorrenza ottimistica da avanzate la finestra di dialogo Opzioni di generazione SQL


Nel [implementazione di concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) esercitazione sono state esaminate le nozioni di base del controllo della concorrenza ottimistica e come aggiungerlo a ObjectDataSource. In questa esercitazione viene ritoccare in essentials di controllo della concorrenza ottimistica e quindi esplorare come implementarlo SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Riepilogo della concorrenza ottimistica

Per le applicazioni web che consentono a più utenti simultanei per modificare o eliminare gli stessi dati, esiste la possibilità che un utente potrebbe sovrascrivere accidentalmente modifiche s un'altra. Nel [implementazione di concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) esercitazione fornito nell'esempio seguente:

Si supponga che due utenti, Jisun e Sam, sono stati entrambi visitare una pagina in un'applicazione che consente di aggiornare ed eliminare i prodotti tramite un controllo GridView. Entrambi fare clic sul pulsante Modifica per Chai intorno alla stessa ora. Jisun diventa il nome del prodotto Chai tè e fa clic sul pulsante di aggiornamento. Il risultato è un `UPDATE` istruzione che viene inviato al database, che imposta *tutti* dei campi aggiornabili prodotto s (anche se Jisun aggiornate solo a un campo, `ProductName`). In questo momento, il database ha i valori Chai tè, della categoria Beverages, liquide esotiche il fornitore, e così via per il prodotto specifico. Tuttavia, nella schermata di Sam s GridView Mostra ancora il nome del prodotto nella riga GridView modificabile come Chai. Pochi secondi dopo che le modifiche di Jisun s sono state eseguite, Sam gli aggiornamenti della categoria Condiments e fa clic su Aggiorna. Ciò comporta un `UPDATE` istruzione inviata al database che imposta il nome del prodotto a Chai, il `CategoryID` per l'ID della categoria Condiments corrispondenti e così via. Le modifiche s Jisun al nome del prodotto sono stati sovrascritti.

Figura 2 viene illustrata questa interazione.


[![Quando due utenti simultaneamente aggiornare un Record sono potenziale s s di un utente cambia per sovrascrivere altri elementi](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Figura 2**: quando due utenti simultaneamente aggiornare un Record sono s potenziale per un utente s le modifiche agli altri oggetti di sovrascrittura ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


Per evitare questo scenario unfolding, una forma di [il controllo della concorrenza](http://en.wikipedia.org/wiki/Concurrency_control) deve essere implementato. [Concorrenza ottimistica](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) l'obiettivo di questa esercitazione funziona sul presupposto che mentre possono essere presenti conflitti di concorrenza ogni ora e quindi, si verificano la maggior parte dei casi tali conflitti vinto t. Pertanto, se si verificano un conflitto, controllo della concorrenza ottimistica semplicemente informa l'utente che loro t di modifiche può essere salvato perché un altro utente ha modificato gli stessi dati.

> [!NOTE]
> Per le applicazioni in cui si presuppone che non vi saranno numerosi conflitti di concorrenza o se tali conflitti non sono quindi il controllo della concorrenza pessimistica consente invece. Fare riferimento al [implementazione di concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) esercitazione per una descrizione più dettagliata sul controllo della concorrenza pessimistica.


Controllo della concorrenza ottimistica funziona, verificare che il record da aggiornare o eliminare ha gli stessi valori che aveva all'avvio dell'aggiornamento o eliminazione di processo. Ad esempio, quando si fa clic sul pulsante di modifica in un controllo GridView. modificabile, i record s leggere dal database e visualizzazione dei valori nelle caselle di testo e altri controlli Web. Questi valori originali vengono salvati per il controllo GridView. In seguito, dopo l'utente effettua le proprie modifiche e fa clic sul pulsante di aggiornamento, il `UPDATE` istruzione utilizzata deve prendere in considerazione i valori originali e i nuovi valori e aggiornare solo i record del database sottostante, se i valori originali che l'utente ha iniziato la modifica sono identici a quelli ancora nel database. Figura 3 viene illustrata questa sequenza di eventi.


[![Per Update o Delete abbia esito positivo, i valori originali devono essere uguali per i valori del Database corrente](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Figura 3**: For Update o Delete su esito positivo, l'originale valori deve essere uguale per i valori del Database corrente ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


Sono disponibili diversi approcci per l'implementazione della concorrenza ottimistica (vedere [Peter A. Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [Optmistic concorrenza aggiornamento logica](http://www.eggheadcafe.com/articles/20050719.asp) per una breve panoramica una serie di opzioni). La tecnica utilizzata da SqlDataSource (sia per i dataset tipizzati ADO.NET utilizzato in questo livello di accesso ai dati) aumenta la `WHERE` clausola per includere un confronto di tutti i valori originali. Le operazioni seguenti `UPDATE` istruzione, ad esempio, aggiorna il nome e il prezzo di un prodotto solo se i valori del database corrente sono uguali a quelli che sono stati recuperati inizialmente quando si aggiorna il record in GridView. Il `@ProductName` e `@UnitPrice` parametri contengono i nuovi valori immessi dall'utente, mentre `@original_ProductName` e `@original_UnitPrice` contengono i valori che sono stati originariamente caricati in GridView, quando è stato fatto clic sul pulsante di modifica:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Come si vedrà in questa esercitazione, l'abilitazione di controllo della concorrenza ottimistica con SqlDataSource è semplice come una casella di controllo.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Passaggio 1: Creazione di un SqlDataSource che supporta la concorrenza ottimistica

Aprire il `OptimisticConcurrency.aspx` pagina dal `SqlDataSource` cartella. Trascinare un controllo SqlDataSource dalla casella degli strumenti nella finestra di progettazione, le impostazioni relative `ID` proprietà `ProductsDataSourceWithOptimisticConcurrency`. Fare clic sul collegamento Configura origine dati dallo smart tag s di controllo. Scegliere la prima schermata della procedura guidata, funzionino con la `NORTHWINDConnectionString` e fare clic su Avanti.


[![Scegliere per l'uso con la connessione NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Figura 4**: scegliere di utilizzare il `NORTHWINDConnectionString` ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


Per questo esempio verranno aggiunte GridView che consente agli utenti di modificare il `Products` tabella. Pertanto, la configurazione, la schermata di istruzione Select, scegliere il `Products` tabella dall'elenco a discesa e selezionare il `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colonne, come illustrato nella figura 5.


[![Dalla tabella Products, restituire il ProductID, ProductName, UnitPrice e le colonne non più supportate](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Figura 5**: dal `Products` tabella, per restituire il `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colonne ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


Dopo aver selezionato le colonne, fare clic sul pulsante per visualizzare la finestra di dialogo Opzioni avanzate generazione SQL avanzate. Controllare la generazione `INSERT`, `UPDATE`, e `DELETE` istruzioni e utilizzare le caselle di controllo della concorrenza ottimistica e fare clic su OK (vedere la figura 1 per una schermata). Completare la procedura guidata, fare clic su Avanti, quindi Fine.

Dopo aver completato la configurazione guidata origine dati, è opportuno esaminare il valore risultante `DeleteCommand` e `UpdateCommand` proprietà e `DeleteParameters` e `UpdateParameters` raccolte. Il modo più semplice per eseguire questa operazione è fare clic sulla scheda origine nell'angolo inferiore sinistro per visualizzare la sintassi dichiarativa s di pagina. Sarà possibile trovare un `UpdateCommand` valore:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

Con sette parametri di `UpdateParameters` raccolta:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

Analogamente, il `DeleteCommand` proprietà e `DeleteParameters` raccolta dovrebbe essere simile al seguente:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Oltre a aumento il `WHERE` clausole del `UpdateCommand` e `DeleteCommand` proprietà e aggiungere i parametri aggiuntivi per le raccolte di parametro corrispondente, selezionando l'utilizzo consente di regolare due altre opzione di concorrenza ottimistica proprietà:

- Modifiche di [ `ConflictDetection` proprietà](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) da `OverwriteChanges` (predefinito) per`CompareAllValues`
- Modifiche di [ `OldValuesParameterFormatString` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) da {0} (predefinito) originale\_{0}.

Quando i dati di controllo Web richiama s SqlDataSource `Update()` o `Delete()` metodo passa i valori originali. Se i dispositivi SqlDataSource `ConflictDetection` è impostata su `CompareAllValues`, questi valori originali vengono aggiunti al comando. Il `OldValuesParameterFormatString` proprietà fornisce il modello di denominazione per questi parametri di valore originale. La configurazione guidata origine dati utilizza originale\_{0} e i nomi di ogni parametro originale il `UpdateCommand` e `DeleteCommand` proprietà e `UpdateParameters` e `DeleteParameters` raccolte di conseguenza.

> [!NOTE]
> Poiché è re non utilizza gli oggetti di controllo SqlDataSource inserimento di funzionalità, è possibile rimuovere il `InsertCommand` proprietà e il relativo `InsertParameters` insieme.


## <a name="correctly-handlingnullvalues"></a>Gestione corretta di`NULL`valori

Sfortunatamente, il coefficiente `UPDATE` e `DELETE` eseguire le istruzioni generate automaticamente dalla procedura guidata Configura origine dati quando si utilizza la concorrenza ottimistica *non* funzionano con i record che contengono `NULL` valori. Per visualizzare il motivo, si consideri il nostro s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

Il `UnitPrice` colonna il `Products` tabella può avere `NULL` valori. Se un particolare record include un `NULL` valore per `UnitPrice`, il `WHERE` parte clausola `[UnitPrice] = @original_UnitPrice` verrà *sempre* restituiscono False perché `NULL = NULL` restituisce sempre False. Di conseguenza, i record che contengono `NULL` valori non possono essere modificati o eliminati, come il `UPDATE` e `DELETE` istruzioni `WHERE` clausole vinto t restituiscono tutte le righe da aggiornare o eliminare.

> [!NOTE]
> Questo bug è stato segnalato prima a Microsoft a giugno 2004 in [SqlDataSource genera l'errore SQL le istruzioni non corrette](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) e quest'ultima è pianificata per essere corretti nella prossima versione di ASP.NET.


Per risolvere questo problema, è necessario aggiornare manualmente il `WHERE` clausole in entrambe le `UpdateCommand` e `DeleteCommand` le proprietà per **tutti** colonne che può contenere `NULL` valori. In generale, modificare `[ColumnName] = @original_ColumnName` per:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Questa modifica possono essere eseguite direttamente tramite markup dichiarativo, tramite le opzioni UpdateQuery o DeleteQuery dalla finestra delle proprietà, o tramite l'aggiornamento ed eliminazione di schede nella casella specificare un'istruzione SQL personalizzata o l'opzione di stored procedure nei dati di configurazione Creazione guidata di origine. Nuovamente, questa modifica deve essere eseguita per *ogni* colonna il `UpdateCommand` e `DeleteCommand` s `WHERE` clausola che può contenere `NULL` valori.

Questa applicazione per questo esempio verifica quanto segue modificato `UpdateCommand` e `DeleteCommand` valori:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Passaggio 2: Aggiunta di un controllo GridView con modifica e opzioni di eliminazione

Con SqlDataSource configurato per supportare la concorrenza ottimistica, che è comunque necessario aggiungere un controllo Web di dati che utilizza questo controllo della concorrenza. Consente di s per questa esercitazione, aggiungere un controllo GridView che fornisce sia modifica ed eliminazione automatiche. A tale scopo, trascinare un controllo GridView dalla casella degli strumenti di progettazione e il set relativo `ID` a `Products`. Il GridView s smart tag, associarlo al `ProductsDataSourceWithOptimisticConcurrency` controllo SqlDataSource aggiunto nel passaggio 1. Infine, controllare le opzioni Abilita modifica e abilitare l'eliminazione dallo smart tag.


[![Associare il controllo GridView SqlDataSource e abilitare Modifica ed eliminazione](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Figura 6**: associare il controllo GridView SqlDataSource e abilitare la modifica ed eliminazione ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


Dopo l'aggiunta di GridView, configurare l'aspetto rimuovendo il `ProductID` BoundField, modifica il `ProductName` BoundField s `HeaderText` proprietà al prodotto, aggiornare il `UnitPrice` BoundField in modo che il relativo `HeaderText` proprietà è semplicemente prezzo. Idealmente, d è migliorare l'interfaccia di modifica per un controllo RequiredFieldValidator per includere il `ProductName` valore e un controllo CompareValidator per il `UnitPrice` valore (per verificare il valore numerico formattato correttamente s). Fare riferimento al [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazione per un approfondimento personalizzazione s GridView la modifica di interfaccia.

> [!NOTE]
> Controllo GridView. lo stato di visualizzazione s deve essere abilitato dal momento che i valori originali passati dal controllo GridView SqlDataSource sono archiviati nella visualizzazione stato.


Dopo aver apportato queste modifiche a GridView, il markup dichiarativo GridView e SqlDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Per visualizzare il controllo della concorrenza ottimistica in azione, aprire due finestre del browser e caricare il `OptimisticConcurrency.aspx` pagina in entrambi. Fare clic sul pulsante Modifica per il prodotto prima in entrambi i browser. In un browser, modificare il nome del prodotto e fare clic su Aggiorna. Il browser verrà postback e GridView restituirà relativa modalità di pre-modifica, che mostra il nuovo nome di prodotto per il record appena modificato.

Nella seconda finestra del browser, modificare il prezzo (ma lasciare il nome del prodotto come valore originale) e fare clic su Aggiorna. Durante il postback, griglia restituisce relativa modalità di pre-modifica, ma non viene registrata la modifica al prezzo. Il secondo browser Mostra lo stesso valore prima il nuovo nome del prodotto con il prezzo precedente. Le modifiche apportate nella finestra del browser secondo sono andate perse. Inoltre, le modifiche verranno perse invece in modalità non interattiva, perché si è verificato alcun eccezione o un messaggio che indica che una violazione di concorrenza si è verificato.


[![Nella finestra del Browser secondo le modifiche verranno perse senza avvisare](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Figura 7**: il secondo Browser finestra automaticamente persi di modifiche ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


Il motivo per cui il secondo browser s commit delle non modifiche è stato perché il `UPDATE` istruzione s `WHERE` clausola filtrati tutti i record e pertanto non ha influito sulle righe. Consente di esaminare s il `UPDATE` nuovamente l'istruzione:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

Quando la seconda finestra del browser aggiorna il record, il nome originale del prodotto specificato nella `WHERE` clausola t corrispondenza backup con il nome del prodotto esistente (poiché è stata modificata dal primo browser). Pertanto, l'istruzione `[ProductName] = @original_ProductName` restituisce False e `UPDATE` non influisce sui record.

> [!NOTE]
> Eliminare funziona allo stesso modo. Con due finestre del browser aperti, avviare la modifica di un determinato prodotto con uno, e quindi salvando le modifiche. Dopo aver salvato le modifiche in un browser, fare clic sul pulsante Elimina per lo stesso prodotto in altro. Poiché l'originale don valori t corrispondano nel `DELETE` istruzione s `WHERE` clausola, l'eliminazione automatica ha esito negativo.


Dalla prospettiva s utente finale nella seconda finestra del browser, dopo aver fatto clic sul pulsante Aggiorna della griglia restituisce alla modalità di pre-modifica, ma le modifiche apportate verranno perse. Tuttavia, esiste s alcun feedback visivo soffermeremo loro t le modifiche. In teoria, se un utente s sono modifiche a una violazione di concorrenza, d è informarli e, probabilmente, mantenere la griglia in modalità di modifica. Consente di esaminare come effettuare questa operazione s.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Passaggio 3: Determinare quando si è verificato una violazione di concorrenza

Poiché una violazione della concorrenza rifiuta le modifiche sono state, sarebbe più interessante avvisare l'utente quando si è verificata una violazione di concorrenza. Per generare un avviso all'utente, s consentono di aggiungere un controllo etichetta Web nella parte superiore della pagina denominata `ConcurrencyViolationMessage` cui `Text` proprietà viene visualizzato il messaggio seguente: si è tentato di aggiornare o eliminare un record che è stato aggiornato contemporaneamente da un altro utente. Rivedere le modifiche dell'utente quindi ripetere l'aggiornamento o eliminazione. Impostare il controllo etichetta s `CssClass` proprietà avviso, ovvero una classe CSS definita in `Styles.css` che consente di visualizzare il testo in un tipo di carattere rosso, corsivo, grassetto e grandi dimensioni. Infine, impostare l'etichetta s `Visible` e `EnableViewState` proprietà `false`. Questo consente di nascondere l'etichetta, ad eccezione di solo i postback in cui è impostati in modo esplicito il `Visible` proprietà `true`.


[![Aggiungere un controllo etichetta per la pagina per visualizzare il messaggio di avviso](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Figura 8**: aggiungere un controllo etichetta per la pagina per visualizzare l'avviso ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


Quando si esegue un aggiornamento o eliminazione, i dispositivi di GridView `RowUpdated` e `RowDeleted` gestori eventi attivano dopo che ha eseguito il controllo origine dati di richiesta di aggiornamento o eliminazione. È possibile determinare il numero di righe interessato dall'operazione da questi gestori eventi. Se sono stati interessati zero righe, che si desidera visualizzare il `ConcurrencyViolationMessage` etichetta.

Creare un gestore eventi per entrambi i `RowUpdated` e `RowDeleted` gli eventi e aggiungere il codice seguente:


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

In entrambi i gestori eventi controlliamo il `e.AffectedRows` proprietà e, se è pari a 0, impostare il `ConcurrencyViolationMessage` etichetta s `Visible` proprietà `true`. Nel `RowUpdated` gestore eventi, è anche indicare GridView a rimanere in modalità di modifica impostando il `KeepInEditMode` proprietà su true. In questo modo, è necessario riassociare i dati della griglia in modo che gli altri dati utente s viene caricati l'interfaccia di modifica. Questa operazione viene eseguita chiamando il s GridView `DataBind()` metodo.

Come mostrato nella figura 9, con questi due gestori, viene visualizzato un messaggio molto evidente quando si verifica una violazione della concorrenza.


[![Viene visualizzato un messaggio in caso di una violazione di concorrenza](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Figura 9**: viene visualizzato un messaggio in caso di una violazione della concorrenza ([fare clic per visualizzare l'immagine ingrandita](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>Riepilogo

Quando si crea un'applicazione web in cui simultanea di più, gli utenti possono modificare gli stessi dati, è importante prendere in considerazione le opzioni di controllo della concorrenza. Per impostazione predefinita, i dati ASP.NET che i controlli Web e i controlli origine dati non usano alcun controllo della concorrenza. Come abbiamo visto in questa esercitazione, l'implementazione di controllo della concorrenza ottimistica con SqlDataSource è relativamente semplice e veloce. SqlDataSource gestisce la maggior parte di approfondito per l'aggiunta di ampliate `WHERE` clausole per la generato automaticamente `UPDATE` e `DELETE` istruzioni ma si sono pochi sfumature nella gestione `NULL` valore colonne, come descritto nel Gestione corretta di `NULL` i valori di sezione.

Questa esercitazione si conclude l'esame del SqlDataSource. Le esercitazioni rimanenti restituirà per lavorare con i dati di utilizzo di ObjectDataSource e l'architettura a più livelli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Precedente](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
[Successivo](querying-data-with-the-sqldatasource-control-vb.md)
