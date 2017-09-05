---
title: Core di ASP.NET MVC con Entity Framework Core, 8 - concorrenza - 10
author: tdykstra
description: "In questa esercitazione viene illustrato come gestire i conflitti quando più utenti aggiornano la stessa entità nello stesso momento."
keywords: Concorrenza di ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 15e79e15-bda5-441d-80c7-8032a2628605
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 4c402aee195d6614733be71c9c422e33553ad646
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2017
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a>Gestione dei conflitti di concorrenza - EF Core con l'esercitazione di base di ASP.NET MVC (8 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).

Nelle esercitazioni precedenti, è stato descritto come aggiornare i dati. In questa esercitazione viene illustrato come gestire i conflitti quando più utenti aggiornano la stessa entità nello stesso momento.

Si creeranno le pagine web che utilizzano l'entità Department e gestire gli errori di concorrenza. Le illustrazioni seguenti mostrano le pagine di modifica e l'eliminazione, inclusi alcuni messaggi che vengono visualizzati se si verifica un conflitto di concorrenza.

![Pagina Modifica reparto](concurrency/_static/edit-error.png)

![Pagina di eliminazione di reparto](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Un conflitto di concorrenza si verifica quando un utente visualizza dati di un'entità per modificarlo, mentre un altro utente aggiorna i dati dell'entità stessa prima che la modifica del primo utente venga scritto nel database. Se non si abilita il rilevamento dei conflitti, ultimo utente che aggiorna il database sovrascrive le modifiche dell'utente. In molte applicazioni, questo rischio è accettabile: se sono presenti alcuni utenti o alcuni aggiornamenti o se non è realmente critico se alcune modifiche vengono sovrascritte, il costo di programmazione per la concorrenza potrebbe annullare il vantaggio. In tal caso, non è necessario configurare l'applicazione per gestire i conflitti di concorrenza.

### <a name="pessimistic-concurrency-locking"></a>Concorrenza pessimistica (blocco)

Se l'applicazione è necessario evitare la perdita accidentale dei dati in scenari di concorrenza, un modo per eseguire questa operazione è necessario utilizzare blocchi di database. Si tratta della concorrenza pessimistica. Ad esempio, prima di leggere una riga da un database, si richiedono il blocco di sola lettura o per l'accesso per l'aggiornamento. Se si blocca una riga per l'accesso per l'aggiornamento, altri utenti non sono consentiti per bloccare la riga di sola lettura o aggiornare l'accesso, poiché si potrebbe ottenere una copia dei dati che sono in corso di modifica. Se si blocca una riga per l'accesso in sola lettura, altri utenti anche possibile bloccarla per l'accesso in sola lettura, ma non per l'aggiornamento.

La gestione dei blocchi presenta gli svantaggi. Può risultare difficile al programma. Richiede risorse di gestione di database significativa, e può causare problemi di prestazioni come il numero di utenti di un'applicazione aumenta. Per questi motivi, non tutti i sistemi di gestione di database supportano la concorrenza pessimistica. Entity Framework Core non fornisce alcun supporto predefinito per il e questa esercitazione non mostra come implementarlo.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

L'alternativa alla concorrenza pessimistica è della concorrenza ottimistica. Concorrenza ottimistica significa consentire i conflitti di concorrenza consente di eseguire e quindi reazione in modo appropriato in questo caso. Ad esempio, Jane visita la pagina Modifica reparto e l'importo di Budget per il reparto inglese 350,000.00 $ diventa $0,00.

![La modifica di budget per 0](concurrency/_static/change-budget.png)

Prima di Jane seleziona **salvare**, John visita la pagina stessa e il campo Data di inizio da 1/9/2007 a 1/9/2013.

![Modifica la data di inizio a 2013](concurrency/_static/change-date.png)

Jane seleziona **salvare** prima e vede utente modifica quando il browser torna alla pagina di indice.

![Budget modificato a zero](concurrency/_static/budget-zero.png)

Quindi seleziona John **salvare** in una pagina di modifica che mostra ancora un budget di $350,000.00. Le operazioni successive è determinata dalla modalità di gestione i conflitti di concorrenza.

Alcune opzioni includono:

* È possibile tenere traccia di quali proprietà di un utente ha modificato e aggiornare solo le colonne corrispondenti nel database.

     Nello scenario di esempio, non dati andrebbero persi, perché sono state aggiornate diverse proprietà per i due utenti. La volta successiva che un utente che accede al dipartimento in lingua inglese, noteranno le modifiche di Jane sia di John - una data di inizio del 1/9/2013 e un budget di zero dollari. Questo metodo di aggiornamento può ridurre il numero di conflitti che potrebbero comportare la perdita di dati, ma è possibile evitare la perdita di dati se vengono apportate modifiche competizione per la stessa proprietà di un'entità. Se Entity Framework funziona allo stesso modo dipende dalla modalità di implementazione del codice di aggiornamento. È spesso poco pratico in un'applicazione web, perché possono essere necessarie gestire grandi quantità di stato per tenere traccia di tutti i valori originali di proprietà per un'entità, nonché i nuovi valori. Gestione di grandi quantità di stato in termini di prestazioni dell'applicazione perché richiede risorse di server o deve essere incluso nella pagina web stessa (ad esempio, nei campi nascosti) o in un cookie.

* È possibile consentire la modifica di John sovrascrivere la modifica di Jane.

     La volta successiva che un utente che accede al reparto in lingua inglese, verranno visualizzate 1/9/2013 e il valore di $350,000.00 ripristinato. Si tratta di un *prevalenza del Client* o *ultimo in Wins* scenario. (Tutti i valori dal client hanno la precedenza su ciò che si trova nell'archivio dati). Come accennato nell'introduzione di questa sezione, se non si configura questo codice per la gestione della concorrenza, verrà eseguita automaticamente.

* È possibile impedire la modifica di John vengano aggiornati nel database.

     In genere, verrà visualizzato un messaggio di errore, quest'ultimo Mostra lo stato corrente dei dati e consentirgli di riapplicare le modifiche se Alessandro desidera renderli. Si tratta di un *archivio Wins* scenario. (I valori dell'archivio dati hanno la precedenza sui valori inviati dal client). Lo scenario Wins archivio viene implementato in questa esercitazione. Questo metodo assicura che modifiche non vengono sovrascritti senza un utente da ricevere un avviso se ciò che accade.

### <a name="detecting-concurrency-conflicts"></a>Il rilevamento dei conflitti di concorrenza

È possibile risolvere i conflitti gestendo `DbConcurrencyException` eccezioni generate Entity Framework. Per sapere quando generano queste eccezioni, Entity Framework deve essere in grado di rilevare i conflitti. Pertanto, è necessario configurare il database e il modello di dati in modo appropriato. Di seguito sono elencate alcune opzioni per abilitare il rilevamento dei conflitti:

* Nella tabella di database, includere una colonna di rilevamento che può essere utilizzata per determinare quando è stata modificata una riga. È quindi possibile configurare Entity Framework per includere tale colonna nel Where clausola di aggiornamento di SQL o comandi Delete.

     Il tipo di dati della colonna di rilevamento viene in genere `rowversion`. Il `rowversion` valore è un numero sequenziale che sia stato incrementato ogni volta che viene aggiornata la riga. In un comando Update o Delete, Where clausola include il valore originale della colonna di rilevamento (la versione originale). Se la riga aggiornata è stata modificata da un altro utente, il valore di `rowversion` colonna è diverso dal valore originale, pertanto l'istruzione Update o Delete non è possibile trovare la riga da aggiornare a causa di Where clausola. Quando trova Entity Framework che nessuna riga è stata aggiornata mediante l'aggiornamento o eliminazione di comando (ovvero, quando il numero di righe interessate è pari a zero), che vengono interpretati come un conflitto di concorrenza.

* Configurare Entity Framework per includere i valori originali di ogni colonna nella tabella Where clausola di comandi Update e Delete.

     Come prima opzione, qualsiasi elemento nella riga è stato modificato dopo la riga prima di tutto è stato letto, Where clausola non restituisce una riga da aggiornare, che Entity Framework viene interpretato come un conflitto di concorrenza. Per le tabelle di database con molte colonne, questo approccio può comportare grandi Where, clausole e possono richiedere mantenere grandi quantità di stato. Come notato in precedenza, la gestione di grandi quantità di stato può sulle prestazioni dell'applicazione. Pertanto questo approccio è in genere sconsigliato, e non è il metodo utilizzato in questa esercitazione.

     Se si desidera implementare questo approccio alla concorrenza, è necessario contrassegnare tutte le proprietà chiave non primaria dell'entità a cui si desidera tenere traccia di concorrenza per aggiungendo il `ConcurrencyCheck` attributo. Tale modifica consente a Entity Framework includere tutte le colonne nella clausola SQL Where delle istruzioni Update e Delete.

Nella parte restante di questa esercitazione si aggiungerà un `rowversion` proprietà di rilevamento per l'entità Department, creare un controller e visualizzazioni e test per verificare che tutto funzioni correttamente.

## <a name="add-a-tracking-property-to-the-department-entity"></a>Aggiungere una proprietà di rilevamento all'entità reparto

In *Models/Department.cs*, aggiungere una proprietà di rilevamento denominata RowVersion:

[!code-csharp[Principale](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

Il `Timestamp` attributo specifica che questa colonna sarà inclusa nelle Where clausola dei comandi di aggiornamento ed eliminazione inviate al database. L'attributo viene chiamato `Timestamp` perché le versioni precedenti di SQL Server utilizzato un database SQL `timestamp` il tipo di dati prima di SQL `rowversion` sostituito. Il tipo .NET per `rowversion` è una matrice di byte.

Se si preferisce utilizzare l'API fluent, è possibile utilizzare il `IsConcurrencyToken` metodo (in *Data/SchoolContext.cs*) per specificare la proprietà di rilevamento, come illustrato nell'esempio seguente:

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

Tramite l'aggiunta di una proprietà è stato modificato il modello di database, pertanto è necessario eseguire la migrazione di un altro.

Salvare le modifiche e compilare il progetto e quindi immettere i comandi seguenti nella finestra di comando:

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>Creare un controller di reparti e viste

Eseguire lo scaffolding di un controller di reparti e le visualizzazioni come fatto in precedenza per i docenti, corsi e studenti.

![Lo scaffolding reparto](concurrency/_static/add-departments-controller.png)

Nel *DepartmentsController.cs* file, modificare tutte le occorrenze di "FirstMidName" in "FullName" in modo che gli elenchi di riepilogo a discesa amministratore reparto conterrà il nome completo del istruttore anziché solo l'ultimo nome.

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>Aggiornare la visualizzazione dell'indice reparti

Il motore di scaffolding creata una colonna RowVersion nella vista dell'indice, ma il campo non deve essere visualizzato.

Sostituire il codice in *Views/Departments/Index.cshtml* con il codice seguente.

[!code-html[Principale](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

Questo viene modificato il titolo "Servizi", Elimina la colonna RowVersion e Mostra il nome completo anziché il nome per l'amministratore.

## <a name="update-the-edit-methods-in-the-departments-controller"></a>I metodi di modifica nel controller i reparti di aggiornamento

In entrambi i HttpGet `Edit` (metodo) e `Details` metodo, aggiungere `AsNoTracking`. Nel HttpGet `Edit` metodo, aggiungere il caricamento immediato per l'amministratore.

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

Sostituire il codice esistente per HttpPost `Edit` metodo con il codice seguente:

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

Il codice inizia la lettura del reparto da aggiornare. Se il `SingleOrDefaultAsync` metodo restituisce null, il reparto è stato eliminato da un altro utente. In questo caso il codice Usa i valori di modulo registrato per creare un'entità di reparto, in modo che la pagina di modifica può essere visualizzata con un messaggio di errore. In alternativa, non è necessario creare nuovamente l'entità department se si visualizza un messaggio di errore senza rivisualizzazione i campi di reparto.

La vista archivia originale `RowVersion` valore in un campo nascosto e questo metodo riceve il valore nel `rowVersion` parametro. Prima di chiamare `SaveChanges`, è necessario inserire che originale `RowVersion` nel valore della proprietà di `OriginalValues` raccolta per l'entità.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Quando Entity Framework viene creato un comando SQL UPDATE, il comando, includerà una clausola WHERE che esegue la ricerca di una riga con l'originale `RowVersion` valore. Se nessuna riga è interessata dal comando di aggiornamento (righe di non avere originale `RowVersion` valore), Entity Framework genera un `DbUpdateConcurrencyException` eccezione.

Il codice nel blocco catch per tale eccezione Ottiene l'entità Department interessato con i valori aggiornati dal `Entries` proprietà per l'oggetto eccezione.

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

Il `Entries` raccolta avrà uno `EntityEntry` oggetto.  È possibile utilizzare tale oggetto per ottenere i nuovi valori immessi dall'utente e i valori del database corrente.

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

Il codice aggiunge un messaggio di errore personalizzato per ogni colonna con valori di database diversi da quali immessi dall'utente dal menu Modifica pagina (un solo campo è riportato per brevità).

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Infine, il codice imposta il `RowVersion` valore il `departmentToUpdate` per il nuovo valore recuperato dal database. Questa nuova `RowVersion` valore verrà archiviato nel campo nascosto quando la modifica di pagina viene visualizzata e la successiva ora l'utente fa clic **salvare**, solo gli errori di concorrenza che si verificano dopo la visualizzazione della pagina di modifica verrà rilevata.

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

Il `ModelState.Remove` istruzione è necessaria perché `ModelState` è il vecchio `RowVersion` valore. Nella visualizzazione, il `ModelState` valore per un campo ha la precedenza sui valori di proprietà del modello quando sono presenti entrambe.

## <a name="update-the-department-edit-view"></a>Aggiornare la visualizzazione di modifica di reparto

In *Views/Departments/Edit.cshtml*, apportare le modifiche seguenti:

* Rimuovere il `<div>` elemento che è stata scaffolding per il `RowVersion` campo.

* Aggiungere un campo nascosto per salvare il `RowVersion` valore della proprietà, subito dopo il campo nascosto per il `DepartmentID` proprietà.

* Aggiungere un'opzione "Selezionare amministratore" per l'elenco a discesa.

[!code-html[Principale](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>Verificare i conflitti di concorrenza presente nella pagina di modifica

Esecuzione del sito e fare clic per passare alla pagina di indice reparti i reparti.

Fare doppio clic su di **modifica** collegamento ipertestuale per il reparto in lingua inglese e selezionare **aperto in una nuova scheda**, quindi fare clic su di **modifica** collegamento ipertestuale per il reparto in lingua inglese. Le schede del due browser ora visualizzano le stesse informazioni.

Modificare un campo nella prima scheda del browser e fare clic su **salvare**.

![Reparto Modifica pagina 1 dopo la modifica](concurrency/_static/edit-after-change-1.png)

Il browser viene mostrata la pagina di indice con il valore modificato.

Modificare un campo nella seconda scheda del browser.

![Reparto Modifica pagina 2 dopo la modifica](concurrency/_static/edit-after-change-2.png)

Fare clic su **Salva**. Viene visualizzato un messaggio di errore:

![Messaggio di errore di pagina Modifica reparto](concurrency/_static/edit-error.png)

Fare clic su **salvare** nuovamente. Il valore che immesso nella seconda scheda browser viene salvato. Vengono visualizzati i valori salvati quando viene visualizzata la pagina di indice.

## <a name="update-the-delete-page"></a>Aggiornare la pagina di eliminazione

Per la pagina di eliminazione, Entity Framework rileva i conflitti di concorrenza causati da un utente in caso contrario la modifica del reparto in modo simile. Quando il HttpGet `Delete` metodo visualizza la conferma, la visualizzazione include originale `RowVersion` valore in un campo nascosto. Valore ottenuto viene quindi disponibili per il HttpPost `Delete` metodo che viene chiamato quando l'utente conferma l'eliminazione. Quando Entity Framework viene creato il comando SQL DELETE, include una clausola WHERE con originale `RowVersion` valore. Se i risultati del comando in zero righe interessato (che indica la riga è stata modificata dopo che è stata visualizzata la pagina di conferma eliminazione), viene generata un'eccezione di concorrenza e il HttpGet `Delete` metodo viene chiamato con un errore flag è impostato su true per visualizzare nuovamente il pagina di conferma con un messaggio di errore. È anche possibile che siano stati interessati zero righe, perché la riga è stata eliminata da un altro utente, pertanto in questo caso non viene visualizzato alcun messaggio di errore.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>I metodi di eliminazione nel controller i reparti di aggiornamento

In *DepartmentsController.cs*, sostituire il HttpGet `Delete` metodo con il codice seguente:

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

Il metodo accetta un parametro facoltativo che indica se la pagina viene nuovamente visualizzata dopo un errore di concorrenza. Se questo flag è true e il reparto specificato non esiste, è stata eliminata da un altro utente. In tal caso, il codice reindirizza alla pagina di indice.  Se questo flag è true e il reparto esiste, è stato modificato da un altro utente. In tal caso, il codice invia un messaggio di errore per la visualizzazione con `ViewData`.  

Sostituire il codice in HttpPost `Delete` metodo (denominato `DeleteConfirmed`) con il codice seguente:

[!code-csharp[Principale](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

Nel codice scaffolding che è stato sostituito solo, questo metodo è accettato solo un ID di record:


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Questo parametro è stato modificato a un'istanza di entità reparto creata dal gestore di associazione del modello. Ciò consente di accedere EF al valore della proprietà RowVersion oltre alla chiave del record.

```csharp
public async Task<IActionResult> Delete(Department department)
```

È stato modificato il nome del metodo di azione da `DeleteConfirmed` a `Delete`. Il codice di supporto temporaneo utilizzato il nome `DeleteConfirmed` per fornire il metodo HttpPost una firma univoca. (Common Language Runtime richiede metodi di overload per disporre di parametri di metodo diverso). Ora che le firme sono univoche, è possibile utilizzare la convenzione MVC e utilizzare lo stesso nome per i metodi di eliminazione HttpPost e HttpGet.

Se il reparto è già stato eliminato, il `AnyAsync` metodo restituisce false e l'applicazione appena torna al metodo di indice.

Se viene rilevato un errore di concorrenza, il codice viene visualizzata nuovamente la pagina di conferma eliminazione e fornisce un flag che indica che si visualizza un messaggio di errore di concorrenza.

### <a name="update-the-delete-view"></a>Aggiornare la visualizzazione di eliminazione

In *Views/Departments/Delete.cshtml*, sostituire il codice di supporto temporaneo con il seguente codice che aggiunge un campo di messaggio di errore e i campi nascosti per le proprietà DepartmentID e RowVersion. Le modifiche sono evidenziate.

[!code-html[Principale](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

In questo modo le modifiche seguenti:

* Aggiunge un messaggio di errore tra il `h2` e `h3` intestazioni.

* Sostituisce LastName con FullName nel **amministratore** campo.

* Rimuove il campo RowVersion.

* Aggiunge un campo nascosto per il `RowVersion` proprietà.

Eseguire la pagina di indice reparti. Fare clic il **eliminare** collegamento ipertestuale per il reparto in lingua inglese e selezionare **aperto in una nuova scheda**, fare clic su nella prima scheda le **modifica** collegamento ipertestuale per il reparto in lingua inglese.

Nella prima finestra, modificare uno dei valori e fare clic su **salvare**:

![Pagina Modifica reparto dopo la modifica prima dell'eliminazione](concurrency/_static/edit-after-change-for-delete.png)

Nella seconda scheda, fare clic su **eliminare**. Viene visualizzato il messaggio di errore di concorrenza e i valori di reparto vengono aggiornati con i dati attualmente il database.

![Pagina di conferma eliminazione reparto con errore di concorrenza](concurrency/_static/delete-error.png)

Se si fa clic **eliminare** nuovamente, si viene reindirizzati alla pagina di indice, che indica che il reparto è stato eliminato.

## <a name="update-details-and-create-views"></a>L'aggiornamento dei dettagli e creare visualizzazioni

Facoltativamente, è possibile pulire il codice scaffolding nei dettagli e creare visualizzazioni.

Sostituire il codice in *Views/Departments/Details.cshtml* per eliminare la colonna RowVersion e visualizzare il nome completo dell'amministratore.

[!code-html[Principale](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Sostituire il codice in *Views/Departments/Create.cshtml* da aggiungere all'elenco di riepilogo a discesa selezionare l'opzione.

[!code-html[Principale](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>Riepilogo

Introduzione alla gestione di conflitti di concorrenza è stata completata. Per ulteriori informazioni su come gestire la concorrenza in Entity Framework Core, vedere [i conflitti di concorrenza](https://docs.microsoft.com/ef/core/saving/concurrency). L'esercitazione successiva viene illustrato come implementare l'ereditarietà tabella per gerarchia per le entità Instructor e Student.

>[!div class="step-by-step"]
[Precedente](update-related-data.md)
[Successivo](inheritance.md)  
