---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: Implementazione della concorrenza ottimistica con SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione è esaminare i passaggi fondamentali del controllo della concorrenza ottimistica e quindi esplorare come implementarla mediante il controllo SqlDataSource.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: f2590e8e7712d719eb89403ef839f03066a93d2b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835819"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Implementazione della concorrenza ottimistica con SqlDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) o [Scarica il PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> In questa esercitazione è esaminare i passaggi fondamentali del controllo della concorrenza ottimistica e quindi esplorare come implementarla mediante il controllo SqlDataSource.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente abbiamo esaminato come aggiungere inserimento, aggiornamento ed eliminazione di funzionalità per il controllo SqlDataSource. In breve, per fornire queste funzionalità sono necessarie per specificare il corrispondente `INSERT`, `UPDATE`, o `DELETE` istruzione SQL nel controllo s `InsertCommand`, `UpdateCommand`, o `DeleteCommand` proprietà, insieme alle appropriato i parametri in di `InsertParameters`, `UpdateParameters`, e `DeleteParameters` raccolte. Anche se queste proprietà e le raccolte possono essere specificate manualmente, il pulsante Avanzate di creazione guidata s Configura origine dati offre una generazione `INSERT`, `UPDATE`, e `DELETE` istruzioni casella di controllo che crea automaticamente tali istruzioni in base il `SELECT` istruzione.

E genera `INSERT`, `UPDATE`, e `DELETE` istruzioni casella di controllo di finestra di dialogo Opzioni avanzate generazione SQL è inclusa un'opzione di concorrenza ottimistica utilizzare (vedere la figura 1). Se selezionata, il `WHERE` clausole in quello generato automaticamente `UPDATE` e `DELETE` istruzioni sono state modificate per eseguire solo l'update o delete se t non abbia dati del database sottostante è stato modificato dall'utente ultimo caricamento dei dati nella griglia.


![È possibile aggiungere il supporto della concorrenza ottimistica da avanzate finestra di dialogo Opzioni di generazione SQL](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Figura 1**: È possibile aggiungere il supporto della concorrenza ottimistica da avanzate finestra di dialogo Opzioni di generazione SQL


Nel [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) esercitazione sono state esaminate le nozioni di base del controllo della concorrenza ottimistica e come aggiungerlo a ObjectDataSource. In questa esercitazione verranno ritoccare su concetti essenziali del controllo della concorrenza ottimistica e quindi esplorare come implementarla utilizzando SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Un riepilogo della concorrenza ottimistica

Per le applicazioni web che consentono a più utenti simultanei per modificare o eliminare gli stessi dati, esiste una possibilità che un utente potrebbe sovrascrivere accidentalmente modifiche s un'altra. Nel [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) esercitazione ho fornito nell'esempio seguente:

Si supponga che due utenti, Jisun e Sam, sono stati entrambi che visitano una pagina in un'applicazione che consente di aggiornare ed eliminare i prodotti tramite un controllo GridView. Entrambi fare clic sul pulsante Modifica per Chai intorno alla stessa ora. Jisun diventa il nome del prodotto tè Chai e fa clic sul pulsante di aggiornamento. Il risultato è un' `UPDATE` istruzione che viene inviato al database, che imposta *tutte* dei campi aggiornabili prodotto s (anche se Jisun aggiornate solo a un campo, `ProductName`). In questo momento, il database presenta i valori Chai Tea, la categoria Beverages, il fornitore originali, e così via per liquidi questa particolare prodotto. Tuttavia, il controllo GridView nella schermata di Sam s Mostra ancora il nome del prodotto nella riga GridView modificabile come Chai. Pochi secondi dopo che sono state salvate, le modifiche s Jisun Sam gli aggiornamenti della categoria Condiments e fa clic su Aggiorna. Il risultato è un' `UPDATE` istruzione inviata al database che consente di impostare il nome del prodotto a Chai compare, il `CategoryID` per l'ID della categoria Condiments corrispondenti e così via. Modifiche di Jisun s per il nome del prodotto sono stati sovrascritti.

Figura 2 illustra questa interazione.


[![Quando due utenti aggiornano contemporaneamente un Record sono s potenziale per un utente s cambia aspetto per sovrascrivere l'altri spazi dei nomi](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Figura 2**: quando due utenti simultaneamente aggiornare un Record sono s potenziale per un utente s le modifiche al sovrascrittura di altri spazi dei nomi ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


Per impedire che unfolding, una forma di questo scenario [controllo della concorrenza](http://en.wikipedia.org/wiki/Concurrency_control) devono essere implementati. [La concorrenza ottimistica](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) l'obiettivo di questa esercitazione funziona sul presupposto che possono essere anche i conflitti di concorrenza ogni ora e quindi, si verificano la maggior parte dei casi questi conflitti ha vinto t. Pertanto, se si verificano un conflitto, controllo della concorrenza ottimistica semplicemente informa l'utente che le modifiche Impossibile essere salvato perché un altro utente ha modificato gli stessi dati.

> [!NOTE]
> Per le applicazioni in cui si presuppone che siano presenti molti conflitti di concorrenza o se tali conflitti non vengono tollerabili, quindi il controllo della concorrenza pessimistica è utilizzabile in alternativa. Fare riferimento al [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) esercitazione per una discussione più approfondita sul controllo della concorrenza pessimistica.


Controllo della concorrenza ottimistica funzionamento verificando che il record da aggiornare o eliminare dispone degli stessi valori si blocca durante l'aggiornamento o eliminazione di processo avviato. Ad esempio, quando si fa clic sul pulsante Modifica in un controllo GridView modificabile, i record s leggere dal database e visualizzazione dei valori nelle caselle di testo e altri controlli di Web. Questi valori originali vengono salvati per il controllo GridView. Successivamente, dopo che l'utente effettua le proprie modifiche e fa clic sul pulsante di aggiornamento, il `UPDATE` istruzione utilizzata deve prendere in considerazione i valori originali e i nuovi valori e aggiornare solo i record del database sottostante, se i valori originali che l'utente ha iniziato la modifica sono identici a quelli ancora nel database. Figura 3 viene illustrata questa sequenza di eventi.


[![Per Update o Delete abbia esito positivo, i valori originali devono essere uguali per i valori del Database corrente](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Figura 3**: per l'aggiornamento o eliminazione a hanno esito positivo, l'originale valori deve essere uguale per i valori del Database corrente ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


Esistono vari approcci all'implementazione della concorrenza ottimistica (vedere [Peter A. Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [per la logica di aggiornamento di concorrenza di Optmistic](http://www.eggheadcafe.com/articles/20050719.asp) per un rapido sguardo una serie di opzioni). La tecnica utilizzata da SqlDataSource (sia da ADO.NET tipizzata set di dati usati nel nostro livello di accesso ai dati) aumenta la `WHERE` clausola per includere un confronto di tutti i valori originali. Nell'esempio `UPDATE` istruzione, ad esempio, aggiorna il nome e il prezzo di un prodotto solo se sono uguali ai valori originariamente recuperati quando si aggiorna il record in GridView i valori del database corrente. Il `@ProductName` e `@UnitPrice` i parametri contengono i nuovi valori immessi dall'utente, mentre `@original_ProductName` e `@original_UnitPrice` contengono i valori che originariamente venivano caricati in GridView quando è stato fatto clic sul pulsante di modifica:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Come sarà illustrato in questa esercitazione, l'abilitazione di controllo della concorrenza ottimistica con SqlDataSource è semplice come selezionare una casella di controllo.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Passaggio 1: Creazione di un SqlDataSource che supporta la concorrenza ottimistica

Iniziare aprendo il `OptimisticConcurrency.aspx` dalla pagina di `SqlDataSource` cartella. Trascinare un controllo SqlDataSource dalla casella degli strumenti nella finestra di progettazione, le impostazioni relative `ID` proprietà `ProductsDataSourceWithOptimisticConcurrency`. Successivamente, fare clic sul collegamento Configura origine dati nello smart tag controllo s. Dalla schermata prima della procedura guidata, scegliere di lavorare con i `NORTHWINDConnectionString` e fare clic su Avanti.


[![Scegliere di lavoro con il NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Figura 4**: scegliere di lavoro con il `NORTHWINDConnectionString` ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


In questo esempio verranno aggiunte GridView che consente agli utenti di modificare il `Products` tabella. Pertanto, configura la schermata di istruzione Select, scegliere il `Products` tabella nell'elenco a discesa e selezionare il `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colonne, come illustrato nella figura 5.


[![Dalla tabella Products e restituire il ProductID, ProductName, UnitPrice e le colonne non più supportate](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Figura 5**: dal `Products` tabella, per restituire le `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colonne ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


Dopo aver selezionato le colonne, fare clic sul pulsante Avanzate per visualizzare la finestra di dialogo Opzioni di generazione SQL avanzate. Controllare la generazione `INSERT`, `UPDATE`, e `DELETE` istruzioni e usare le caselle di controllo della concorrenza ottimistica e fare clic su OK (vedere la figura 1 per visualizzare uno screenshot). Completare la procedura guidata, fare clic su Avanti e quindi finire.

Dopo aver completato la procedura guidata Configura origine dati, si consiglia di esaminare l'oggetto risultante `DeleteCommand` e `UpdateCommand` delle proprietà e il `DeleteParameters` e `UpdateParameters` raccolte. Il modo più semplice per eseguire questa operazione deve fare clic sulla scheda origine nell'angolo inferiore sinistro per visualizzare la sintassi dichiarativa s di pagina. Qui troverai un `UpdateCommand` pari a:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

I sette parametri in di `UpdateParameters` raccolta:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

Analogamente, il `DeleteCommand` proprietà e `DeleteParameters` raccolta dovrebbe essere simile al seguente:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Oltre all'aumento di `WHERE` clausole del `UpdateCommand` e `DeleteCommand` proprietà e aggiungendo i parametri aggiuntivi per le raccolte di parametri corrispondente, selezionando l'utilizzo consente di regolare gli altri due opzione di concorrenza ottimistica proprietà:

- Modifiche i [ `ConflictDetection` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) da `OverwriteChanges` (predefinito) per `CompareAllValues`
- Modifiche i [ `OldValuesParameterFormatString` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) dalla {0} (predefinito) a originale\_ {0} .

Quando i controllo Web per dati richiama s SqlDataSource `Update()` o `Delete()` (metodo), passa i valori originali. Se la s SqlDataSource `ConflictDetection` è impostata su `CompareAllValues`, questi valori originali vengono aggiunti al comando. Il `OldValuesParameterFormatString` proprietà fornisce il modello di denominazione utilizzato per questi parametri di valore originale. La procedura guidata Configura origine dati Usa originale\_ {0} e i nomi di ogni parametro originale nel `UpdateCommand` e `DeleteCommand` le proprietà e `UpdateParameters` e `DeleteParameters` raccolte di conseguenza.

> [!NOTE]
> Poiché si ri non usa gli oggetti controllo SqlDataSource inserimento di funzionalità, è possibile rimuovere il `InsertCommand` proprietà e i relativi `InsertParameters` raccolta.


## <a name="correctly-handlingnullvalues"></a>Corretta gestione`NULL`valori

Sfortunatamente, l'elemento aumentata `UPDATE` e `DELETE` eseguire le istruzioni generate automaticamente dalla procedura guidata Configura origine dati quando si usa la concorrenza ottimistica *non* funzionano con i record che contengono `NULL` valori. Per comprendere l'importanza, prendere in considerazione la s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

Il `UnitPrice` colonna il `Products` tabella può includere `NULL` valori. Se un particolare record ha un `NULL` valore per `UnitPrice`, il `WHERE` parte clausola `[UnitPrice] = @original_UnitPrice` verrà *sempre* restituiscono False perché `NULL = NULL` restituisce sempre False. Di conseguenza, i record che contengono `NULL` valori non possono essere modificati o eliminati, come i `UPDATE` e `DELETE` istruzioni `WHERE` clausole ha vinto t restituiscono tutte le righe da aggiornare o eliminare.

> [!NOTE]
> Questo bug è stata innanzitutto segnalato a Microsoft nel giugno del 2004 nella [SqlDataSource genera SQL le istruzioni non corrette](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) e quest'ultima è pianificata per essere corretto nella prossima versione di ASP.NET.


Per risolvere questo problema, è necessario aggiornare manualmente il `WHERE` clausole in entrambe le `UpdateCommand` e `DeleteCommand` le proprietà per **tutti i** colonne che possono avere `NULL` valori. In generale, modificare `[ColumnName] = @original_ColumnName` per:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Questa modifica possono essere impostati direttamente tramite markup dichiarativo, tramite le opzioni UpdateQuery o DeleteQuery dalla finestra proprietà o l'aggiornamento ed eliminare le schede nella specifica di un'istruzione SQL personalizzata o opzione della stored procedure nei dati di configurazione Creazione guidata origine. Anche in questo caso, è necessario effettuare questa modifica *ogni* colonna il `UpdateCommand` e `DeleteCommand` s `WHERE` clausola che può contenere `NULL` valori.

L'applicazione di questa all'esempio precedente restituisce il seguente modificata `UpdateCommand` e `DeleteCommand` valori:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Passaggio 2: Aggiunta di un controllo GridView con modifica e opzioni di eliminazione

Con SqlDataSource configurato per supportare la concorrenza ottimistica, resta che aggiungere un controllo Web per dati alla pagina che utilizza questo controllo della concorrenza. Aggiungere un controllo GridView che offre di modifica e funzionalità di eliminazione per questa esercitazione, lasciare che s. A tale scopo, trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione e set relativo `ID` a `Products`. GridView s nello smart tag, associarlo al `ProductsDataSourceWithOptimisticConcurrency` controllo SqlDataSource aggiunto nel passaggio 1. Infine, controllare le opzioni Abilita modifica e abilitare l'eliminazione dello smart tag.


[![Associare il controllo GridView a SqlDataSource e abilitare la modifica ed eliminazione](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Figura 6**: associare il controllo GridView a SqlDataSource e Abilita modifica ed eliminazione ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


Dopo aver aggiunto il controllo GridView, configurare l'aspetto del controllo tramite la rimozione il `ProductID` BoundField, modificare il `ProductName` BoundField s `HeaderText` proprietà al prodotto e l'aggiornamento il `UnitPrice` BoundField in modo che relativo `HeaderText` proprietà è è sufficiente prezzo. In teoria, è il d migliora l'interfaccia di modifica per includere un RequiredFieldValidator per il `ProductName` valore e un controllo CompareValidator per i `UnitPrice` valore (per assicurarsi che un valore numerico formattato correttamente s). Fare riferimento al [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazione per un'analisi più approfondita personalizzazione s GridView interfaccia di modifica.

> [!NOTE]
> GridView lo stato di visualizzazione s deve essere abilitato dal momento che i valori originali passati dal GridView e SqlDataSource sono archiviate nella vista stato.


Dopo aver apportato queste modifiche a GridView, il markup dichiarativo GridView e SqlDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Per visualizzare il controllo della concorrenza ottimistica in azione, aprire due finestre del browser e caricare il `OptimisticConcurrency.aspx` pagina in entrambi. Fare clic sui pulsanti di modifica per il primo prodotto in entrambi i browser. In un browser, modificare il nome del prodotto e fare clic su Aggiorna. Il browser verrà postback e GridView restituirà alla modalità di pre-modifica, che mostra il nuovo nome di prodotto per il record appena modificato.

Nella seconda finestra del browser, modificare il prezzo (ma lasciare il nome del prodotto come relativo valore originale) e fare clic su Aggiorna. Durante il postback, la griglia restituisce alla modalità di pre-modifica, ma la modifica per il prezzo non viene registrata. Il secondo browser visualizza lo stesso valore di quella del primo il nuovo nome del prodotto con prezzo precedente. Le modifiche apportate nella seconda finestra del browser sono stati perse. Inoltre, le modifiche sono andate perse piuttosto in modalità non interattiva, perché si è verificato alcun eccezione o un messaggio che indica che una violazione della concorrenza si è verificato.


[![Le modifiche nella seconda finestra del Browser sono stati perse in modo invisibile](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Figura 7**: The modifiche la seconda Browser finestra sono state automaticamente perso ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


Il motivo per cui il secondo browser s commit delle non modifiche è stato perché il `UPDATE` istruzione s `WHERE` clausola filtrati tutti i record e pertanto non ha modificato alcuna riga. Let s esaminare il `UPDATE` nuovamente l'istruzione:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

Quando la seconda finestra del browser aggiorna il record, il nome del prodotto originale specificato nel `WHERE` clausola l t match backup con il nome del prodotto esistente (poiché è stato modificato dal primo browser). Pertanto, l'istruzione `[ProductName] = @original_ProductName` restituisce False e il `UPDATE` non avrà effetto su qualsiasi record.

> [!NOTE]
> Eliminare funziona allo stesso modo. Con due finestre del browser aperte, avviare la modifica di un prodotto specifico con uno e quindi salvando le modifiche. Dopo aver salvato le modifiche in un browser, fare clic sul pulsante Elimina per lo stesso prodotto in altro. Poiché l'originale don valori t corrispondere nel `DELETE` istruzione s `WHERE` clausola, l'eliminazione automatica ha esito negativo.


Dalla prospettiva di s dell'utente finale nella seconda finestra del browser, dopo aver fatto clic sul pulsante Aggiorna della griglia restituisce alla modalità di pre-modifica, ma le modifiche apportate verranno perse. Tuttavia, qui s alcun feedback visivo che osserva le t le modifiche. In teoria, se le modifiche di un utente s vengono perse a una violazione della concorrenza, è il d avvisali e, forse, mantenere la griglia in modalità di modifica. Let s esaminare come eseguire questa operazione.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Passaggio 3: Determinare quando si è verificata una violazione di concorrenza

Poiché una violazione della concorrenza rifiuta le modifiche sono state, sarebbe utile avvisare l'utente quando si è verificata una violazione della concorrenza. Per generare un avviso all'utente, s ti permettono di aggiungere un controllo Web etichetta nella parte superiore della pagina denominata `ConcurrencyViolationMessage` cui `Text` proprietà Visualizza il messaggio seguente: si è tentato di aggiornare o eliminare un record che è stato aggiornato contemporaneamente da un altro utente. Rivedere le altre modifiche dell'utente quindi Ripeti l'aggiornamento o eliminare. Impostare il controllo etichetta 1!s `CssClass` proprietà ad avviso, ovvero una classe CSS definita in `Styles.css` che visualizza il testo in un tipo di carattere rosso, corsivo, grassetto e grandi dimensioni. Infine, impostare l'etichetta 1!s `Visible` e `EnableViewState` delle proprietà per `false`. In modo da nascondere l'etichetta tranne solo tali postback in cui è impostati in modo esplicito relativo `Visible` proprietà `true`.


[![Aggiungere un controllo etichetta per la pagina per visualizzare il messaggio di avviso](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Figura 8**: aggiungere un controllo etichetta per la pagina per visualizzare il messaggio di avviso ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


Quando si esegue un aggiornamento o eliminazione, la s GridView `RowUpdated` e `RowDeleted` gestori eventi vengono attivati dopo che il controllo origine dati ha eseguito l'aggiornamento richiesto o l'eliminazione. È possibile determinare il numero di righe sono stato interessato dall'operazione di da questi gestori eventi. Se sono stati interessati zero righe, vorrei che vengano visualizzati i `ConcurrencyViolationMessage` etichetta.

Creare un gestore eventi per entrambi i `RowUpdated` e `RowDeleted` eventi e aggiungere il codice seguente:


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

In entrambi i gestori eventi controlliamo il `e.AffectedRows` proprietà e, se uguale a 0, impostare il `ConcurrencyViolationMessage` etichetta s `Visible` proprietà `true`. Nel `RowUpdated` gestore eventi, è anche istruire il controllo GridView a rimanere in modalità di modifica impostando il `KeepInEditMode` proprietà su true. In questo modo, è necessario riassociare i dati nella griglia in modo che gli altri dati utente s viene caricati l'interfaccia di modifica. Questa operazione viene eseguita chiamando il s GridView `DataBind()` (metodo).

Come illustrato nella figura 9, con questi due gestori di eventi, viene visualizzato un messaggio molto evidente ogni volta che si verifica una violazione della concorrenza.


[![Viene visualizzato un messaggio in caso di una violazione della concorrenza](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Figura 9**: viene visualizzato un messaggio in caso di una violazione della concorrenza ([fare clic per visualizzare l'immagine con dimensioni normali](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>Riepilogo

Quando si crea un'applicazione web in cui simultanea di più, gli utenti possono modificare gli stessi dati, è importante prendere in considerazione le opzioni di controllo di concorrenza. Per impostazione predefinita, i dati ASP.NET che Web controlli e controlli origine dati non usano alcun controllo della concorrenza. Come abbiamo visto in questa esercitazione, che implementa il controllo della concorrenza ottimistica con SqlDataSource è relativamente semplice e rapido. SqlDataSource gestisce la maggior parte delle attività per l'aggiunta di ampliata `WHERE` clausole a quello generato automaticamente `UPDATE` e `DELETE` istruzioni ma vi sono alcune sottigliezze nella gestione degli `NULL` valore colonne, come descritto nel Corretta gestione `NULL` sezione i valori.

Questa esercitazione è terminata l'esame del SqlDataSource. Le esercitazioni rimanenti restituirà all'utilizzo dei dati di utilizzo di ObjectDataSource e architettura a più livelli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [Successivo](querying-data-with-the-sqldatasource-control-vb.md)
