---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: La gestione della concorrenza con Entity Framework 4.0 in un'applicazione Web 4 ASP.NET | Documenti Microsoft
author: tdykstra
description: Questa serie di esercitazioni compila nell'applicazione web di Contoso University creato da introduttiva la serie di esercitazioni di Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 7bdcf610458631749531ed1279d27e90572f0371
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>La gestione della concorrenza con Entity Framework 4.0 in un'applicazione Web 4 ASP.NET
====================
Da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web di Contoso University creando il [introduzione di Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni. Se non ha completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) che consente di creare. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creati tramite la serie di esercitazioni completo. Nel caso di problemi con le esercitazioni, è possibile registrarli per il [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente è stato descritto come eseguire l'ordinamento e filtro dei dati utilizzando il `ObjectDataSource` controllo e Entity Framework. Questa esercitazione vengono illustrate le opzioni per la gestione della concorrenza in un'applicazione web ASP.NET che usa Entity Framework. Si creerà una nuova pagina web dedicato all'aggiornamento delle assegnazioni di office istruttore. Gestire problemi di concorrenza in tale pagina e nella pagina reparti creato in precedenza.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Quando un utente modifica un record e un altro utente modifica lo stesso record prima che la modifica del primo utente venga scritto nel database, si verifica un conflitto di concorrenza. Se non si imposta Entity Framework per rilevare questi conflitti, ultimo utente che aggiorna il database sovrascrive le modifiche dell'utente. In molte applicazioni, questo rischio è accettabile, e non è necessario configurare l'applicazione per gestire i conflitti di concorrenza possibili. (Se sono presenti alcuni utenti o alcuni aggiornamenti o se non è realmente critico se alcune modifiche vengono sovrascritte, il costo di programmazione per la concorrenza potrebbe annullare il vantaggio.) Se non è necessario preoccuparsi i conflitti di concorrenza, è possibile ignorare questa esercitazione; le esercitazioni rimanenti due serie non dipendono gli elementi elaborati in questo oggetto.

### <a name="pessimistic-concurrency-locking"></a>Concorrenza pessimistica (blocco)

Se l'applicazione è necessario evitare la perdita accidentale dei dati in scenari di concorrenza, un modo per eseguire questa operazione è necessario utilizzare blocchi di database. Si tratta di *concorrenza pessimistica*. Ad esempio, prima di leggere una riga da un database, si richiedono il blocco di sola lettura o per l'accesso per l'aggiornamento. Se si blocca una riga per l'accesso per l'aggiornamento, altri utenti non sono consentiti per bloccare la riga di sola lettura o aggiornare l'accesso, poiché si potrebbe ottenere una copia dei dati che sono in corso di modifica. Se si blocca una riga per l'accesso in sola lettura, altri utenti anche possibile bloccarla per l'accesso in sola lettura, ma non per l'aggiornamento.

Gestione dei blocchi presenta alcuni svantaggi. Può risultare difficile al programma. Richiede risorse di gestione di database significativa, e come il numero di utenti di un'applicazione può causare problemi di prestazioni aumenta (vale a dire non è facilmente scalabile). Per questi motivi, non tutti i sistemi di gestione di database supportano la concorrenza pessimistica. Entity Framework è disponibile alcun supporto predefinito per il e, in questa esercitazione non mostra come implementarlo.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

L'alternativa alla concorrenza pessimistica è *la concorrenza ottimistica*. Concorrenza ottimistica significa consentire i conflitti di concorrenza consente di eseguire e quindi reazione in modo appropriato in questo caso. Ad esempio, Giorgio esegue la *Department.aspx* pagina, fa clic sul **modifica** collegamento per il reparto di cronologia e riduce il **Budget** importo di $ $1,000,000.00 125,000.00. (Giorgio amministra un reparto concorrente e desidera liberare money per il proprio reparto).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Prima di John fa clic su **aggiornamento**, Jane viene eseguito nella stessa pagina, fa clic il **modifica** collegamento per il reparto di cronologia e le modifiche di **data di inizio** campo a 1/1/1/10/2011 1999. (Jane Amministra il reparto di cronologia e desidera assegnargli altre anzianità).

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

Seleziona John **aggiornamento** in primo luogo, quindi fa clic Jane **aggiornamento**. Gli elenchi di ora browser di Jane il **Budget** importo di $1,000,000.00, ma questo non è corretto perché è cambiata la quantità di John in $125,000.00.

Alcune delle azioni da eseguire in questo scenario includono quanto segue:

- È possibile tenere traccia di quali proprietà di un utente ha modificato e aggiornare solo le colonne corrispondenti nel database. Nello scenario di esempio, non dati andrebbero persi, perché sono state aggiornate diverse proprietà per i due utenti. La volta successiva che un utente che accede al reparto di cronologia, visualizzeranno 1/1/1999 e $125,000.00. 

    Questo è il comportamento predefinito in Entity Framework e consente pertanto di ridurre il numero di conflitti che potrebbero comportare la perdita di dati. Tuttavia, questo comportamento non evita la perdita di dati se vengono apportate modifiche competizione per la stessa proprietà di un'entità. Inoltre, questo comportamento non è sempre possibile. Quando si esegue il mapping di stored procedure a un tipo di entità, le proprietà di un'entità vengono aggiornate quando vengono apportate modifiche all'entità nel database.
- È possibile consentire la modifica di Jane sovrascrivere la modifica di John. Dopo che fa clic Jane **aggiornamento**, **Budget** quantità torna a essere $1,000,000.00. Si tratta di un *prevalenza del Client* o *ultimo in Wins* scenario. (I valori del client hanno la precedenza su ciò che si trova nell'archivio dati).
- È possibile impedire la modifica di Jane vengano aggiornati nel database. In genere, verrebbe visualizzato un messaggio di errore, Mostra lo stato corrente dei dati e consente di immettere nuovamente le proprie modifiche se desiderate in modo da renderle. È possibile automatizzare ulteriormente il processo di salvataggio proprio input e fornendo utente la possibilità di riapplicarlo senza dover immettere di nuovo il. Si tratta di un *archivio Wins* scenario. (I valori dell'archivio dati hanno la precedenza sui valori inviati dal client).

### <a name="detecting-concurrency-conflicts"></a>Il rilevamento dei conflitti di concorrenza

In Entity Framework, è possibile risolvere conflitti gestendo `OptimisticConcurrencyException` eccezioni generate Entity Framework. Per sapere quando generano queste eccezioni, Entity Framework deve essere in grado di rilevare i conflitti. Pertanto, è necessario configurare il database e il modello di dati in modo appropriato. Di seguito sono elencate alcune opzioni per abilitare il rilevamento dei conflitti:

- Nel database, includere una colonna di tabella che può essere utilizzata per determinare quando è stata modificata una riga. È quindi possibile configurare Entity Framework per includere la colonna di `Where` clausola SQL `Update` o `Delete` comandi.

    Che è lo scopo del `Timestamp` colonna il `OfficeAssignment` tabella.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Il tipo di dati di `Timestamp` colonna viene anche chiamata `Timestamp`. Tuttavia, la colonna non effettivamente contiene un valore di data o ora. Al contrario, il valore è un numero sequenziale che sia stato incrementato ogni volta che viene aggiornata la riga. In un `Update` o `Delete` comando, il `Where` clausola include originale `Timestamp` valore. Se la riga aggiornata è stata modificata da un altro utente, il valore in `Timestamp` è diverso dal valore originale, pertanto la `Where` clausola viene restituita alcuna riga da aggiornare. Quando Entity Framework rileva che non le righe sono state aggiornate dall'oggetto corrente `Update` o `Delete` comando (ovvero, quando il numero di righe interessate è pari a zero), che interpreta come un conflitto di concorrenza.
- Configurare Entity Framework per includere i valori originali di ogni colonna nella tabella di `Where` clausola di `Update` e `Delete` comandi.

    Come prima opzione, qualsiasi elemento nella riga è stato modificato dopo la riga prima di tutto è stato letto, il `Where` clausola non restituisce una riga da aggiornare, che Entity Framework viene interpretato come un conflitto di concorrenza. Questo metodo è altrettanto efficace come utilizzare un `Timestamp` campo, ma può risultare inefficiente. Per le tabelle di database con molte colonne, può permettere di dimensioni molto grandi `Where` clausole, e in un'applicazione web può richiedere mantenere grandi quantità di stato. Gestione di grandi quantità di stato può influire sulle prestazioni di applicazioni perché richiede risorse del server (ad esempio, lo stato della sessione) o deve essere incluso nella pagina web stessa (ad esempio, lo stato di visualizzazione).

In questa esercitazione si aggiungerà la gestione di conflitti di concorrenza ottimistica per un'entità che non dispone di una proprietà di rilevamento (il `Department` entità) e per un'entità che dispongono di una proprietà di rilevamento (il `OfficeAssignment` entità).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>La gestione della concorrenza ottimistica senza una proprietà di rilevamento

La concorrenza ottimistica per implementare il `Department` entità, che non include un rilevamento (`Timestamp`) proprietà, si completeranno le attività seguenti:

- Modificare il modello di dati per abilitare il rilevamento di concorrenza `Department` entità.
- Nel `SchoolRepository` classe, la gestione delle eccezioni di concorrenza nel `SaveChanges` metodo.
- Nel *Departments.aspx* pagina, la gestione delle eccezioni di concorrenza visualizzando un messaggio all'utente di avviso che tali modifiche sono state non riusciti. L'utente può quindi visualizzare i valori correnti e ripetere le modifiche se sono ancora necessari.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Abilitazione di concorrenza nel modello di dati di rilevamento

In Visual Studio, aprire l'applicazione web di Contoso università che si stava lavorando nell'esercitazione precedente di questa serie.

Aprire *SchoolModel*e in Progettazione modelli di dati, fare doppio clic su di `Name` proprietà il `Department` entità e quindi fare clic su **proprietà**. Nel **proprietà** finestra, modifica il `ConcurrencyMode` proprietà `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Eseguire la stessa operazione per le altre proprietà chiave non primaria scalare (`Budget`, `StartDate`, e `Administrator`.) (È non consentita per le proprietà di navigazione.) Specifica che ogni volta che Entity Framework genera un `Update` o `Delete` comando SQL per aggiornare il `Department` entità nel database, queste colonne (con i valori originali) devono essere incluso nel `Where` clausola. Se viene trovata alcuna riga quando il `Update` o `Delete` comando viene eseguito, Entity Framework genera un'eccezione di concorrenza ottimistica.

Salvare e chiudere il modello di dati.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Gestione delle eccezioni di concorrenza in DAL

Aprire *SchoolRepository.cs* e aggiungere le seguenti `using` istruzione per il `System.Data` dello spazio dei nomi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Aggiungere il seguente nuovo `SaveChanges` metodo che gestisce le eccezioni di concorrenza ottimistica:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Se si verifica un errore di concorrenza quando questo metodo viene chiamato, i valori delle proprietà dell'entità in memoria vengono sostituiti con i valori presenti nel database. In modo che la pagina web può essere gestita, viene nuovamente generata l'eccezione di concorrenza.

Nel `DeleteDepartment` e `UpdateDepartment` metodi, sostituire la chiamata esistente a `context.SaveChanges()` con una chiamata a `SaveChanges()` per richiamare il nuovo metodo.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Gestione delle eccezioni di concorrenza nel livello di presentazione

Aprire *Departments.aspx* e aggiungere un `OnDeleted="DepartmentsObjectDataSource_Deleted"` attributo la `DepartmentsObjectDataSource` controllo. Il tag di apertura per il controllo sarà ora simile all'esempio seguente.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Nel `DepartmentsGridView` controllare, specificare le colonne della tabella di `DataKeyNames` attributo, come illustrato nell'esempio seguente. Si noti che verrà creato grande Visualizza i campi di stato, che è uno dei motivi motivo per cui si usa un campo di rilevamento sono in genere il modo migliore per rilevare i conflitti di concorrenza.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Aprire *Departments.aspx.cs* e aggiungere le seguenti `using` istruzione per il `System.Data` dello spazio dei nomi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Aggiungere il seguente nuovo metodo, verrà chiamato dal controllo origine dati `Updated` e `Deleted` gestori eventi per la gestione delle eccezioni di concorrenza:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Questo codice controlla il tipo di eccezione, e se si tratta di un'eccezione di concorrenza, il codice crea dinamicamente un `CustomValidator` controllo che a sua volta visualizza un messaggio di `ValidationSummary` controllo.

Il nuovo metodo da chiamare il `Updated` gestore dell'evento aggiunto in precedenza. Inoltre, creare un nuovo `Deleted` gestore eventi che chiama il metodo stesso (ma non esegue alcuna operazione else):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Test della concorrenza ottimistica nella pagina reparti

Eseguire il *Departments.aspx* pagina.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Fare clic su **modifica** in una riga e modificare il valore di **Budget** colonna. (Tenere presente che è possibile modificare solo i record creati per questa esercitazione, poiché esistente `School` i record del database contengono alcuni dati non validi. Il record per il reparto di economia è sicuro per sperimentare.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Aprire una nuova finestra del browser ed eseguire nuovamente la pagina (copia l'URL nella casella indirizzo prima della finestra del browser per la seconda finestra del browser).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Fare clic su **modifica** nella stessa riga modificata in precedenza e modificare il **Budget** valore a un elemento diverso.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Nella seconda finestra del browser, fare clic su **aggiornamento**. Il **Budget** quantità viene modificata per questo nuovo valore.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Nella prima finestra del browser, fare clic su **aggiornamento**. L'aggiornamento ha esito negativo. Il **Budget** quantità viene nuovamente visualizzata utilizzando il valore è impostato nella seconda finestra del browser e viene visualizzato un messaggio di errore.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>La gestione della concorrenza ottimistica con una proprietà di rilevamento

Per gestire la concorrenza ottimistica per un'entità che dispone di una proprietà di rilevamento, si completeranno le attività seguenti:

- Aggiunta di stored procedure per il modello di dati per gestire `OfficeAssignment` entità. (Stored procedure e le proprietà di rilevamento non devono essere utilizzate insieme, essi sono semplicemente raggruppati insieme qui a scopo illustrativo.)
- Aggiungere metodi CRUD DAL e il livello Business LOGIC per `OfficeAssignment` entità, incluso il codice per gestire le eccezioni di concorrenza ottimistica in DAL.
- Creare una pagina web le assegnazioni di office.
- Testare la concorrenza ottimistica nella nuova pagina web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Aggiunta di OfficeAssignment Stored procedure per il modello di dati

Aprire il *SchoolModel* file in Progettazione modelli, fare doppio clic nell'area di progettazione e fare clic su **il modello di aggiornamento dal Database**. Nel **Aggiungi** scheda della finestra il **Seleziona oggetti di Database** finestra di dialogo espandere **Stored procedure** e selezionare i tre `OfficeAssignment` stored procedure (vedere il schermata di seguito), quindi fare clic su **fine**. (Queste stored procedure sono stati già nel database quando è stato scaricato o creato tramite uno script.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Fare doppio clic su di `OfficeAssignment` entità e selezionare **Stored Procedure Mapping**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Impostare il **inserire**, **aggiornamento**, e **eliminare** funzioni da utilizzare le relative stored procedure. Per il `OrigTimestamp` parametro del `Update` funzione, impostare il **proprietà** a `Timestamp` e selezionare il **Usa il valore originale** opzione.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Quando Entity Framework chiama la `UpdateOfficeAssignment` stored procedure, l'applicazione superi il valore originale del `Timestamp` colonna il `OrigTimestamp` parametro. La stored procedure utilizza questo parametro nel relativo `Where` clausola:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

La stored procedure seleziona anche il nuovo valore della `Timestamp` colonna dopo l'aggiornamento in modo che Entity Framework consente di mantenere il `OfficeAssignment` entità presente in memoria la sincronizzazione con la riga corrispondente di database.

(Si noti che la stored procedure per l'eliminazione di un'assegnazione di office non ha un `OrigTimestamp` parametro. Per questo motivo, Entity Framework non è possibile verificare che un'entità non è stata modificata prima dell'eliminazione.)

Salvare e chiudere il modello di dati.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Aggiunta di metodi OfficeAssignment a DAL

Aprire *ISchoolRepository.cs* e aggiungere i seguenti metodi CRUD per il `OfficeAssignment` set di entità:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Aggiungere i seguenti nuovi metodi per *SchoolRepository.cs*. Nel `UpdateOfficeAssignment` , è chiamata al metodo locali `SaveChanges` anziché `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Nel progetto di test, aprire *MockSchoolRepository.cs* e aggiungere le seguenti `OfficeAssignment` insieme e metodi CRUD a esso. (Il repository fittizio deve implementare l'interfaccia del repository, o la soluzione non verrà compilato).

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Aggiunta di metodi OfficeAssignment al livello Business Logic

Il progetto principale, aprire *SchoolBL.cs* e aggiungere i seguenti metodi CRUD per il `OfficeAssignment` del set di entità a esso:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Creazione di una pagina Web OfficeAssignments

Creare una nuova pagina web che utilizza il *Site. master* pagina master e denominarlo *OfficeAssignments.aspx*. Aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Si noti che il `DataKeyNames` attributo, il markup specifica il `Timestamp` proprietà, nonché la chiave del record (`InstructorID`). Specifica le proprietà di `DataKeyNames` attributo, il controllo devono essere salvati in stato di controllo (che è simile allo stato di visualizzazione) in modo che i valori originali sono disponibili durante l'elaborazione di postback.

Se non è stato salvato il `Timestamp` valore, Entity Framework non dispongono per il `Where` clausola dell'istruzione SQL `Update` comando. Di conseguenza, non sarebbe trovare da aggiornare. Di conseguenza, Entity Framework genera un'eccezione di concorrenza ottimistica ogni volta un `OfficeAssignment` entità viene aggiornato.

Aprire *OfficeAssignments.aspx.cs* e aggiungere le seguenti `using` istruzione per il livello di accesso ai dati:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Aggiungere il seguente `Page_Init` metodo, che abilita la funzionalità di Dynamic Data. Inoltre, aggiungere il seguente gestore per il `ObjectDataSource` del controllo `Updated` evento per verificare la presenza di errori di concorrenza:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Test della concorrenza ottimistica nella pagina OfficeAssignments

Eseguire il *OfficeAssignments.aspx* pagina.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Fare clic su **modifica** in una riga e modificare il valore di **percorso** colonna.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Aprire una nuova finestra del browser ed eseguire nuovamente la pagina (copia l'URL dalla finestra del browser prima la seconda finestra del browser).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Fare clic su **modifica** nella stessa riga modificata in precedenza e modificare il **percorso** valore a un elemento diverso.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Nella seconda finestra del browser, fare clic su **aggiornamento**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Passa alla prima finestra del browser e fare clic su **aggiornamento**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Viene visualizzato un messaggio di errore e **percorso** valore è stato aggiornato per mostrare il valore è stato modificato nella seconda finestra del browser.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>La gestione della concorrenza con il controllo EntityDataSource

Il `EntityDataSource` controllo include una logica incorporata che riconosce le impostazioni di concorrenza nel modello di dati e gli handle di aggiornamento e di conseguenza le operazioni di eliminazione. Tuttavia, come con tutte le eccezioni, è necessario gestire `OptimisticConcurrencyException` eccezioni manualmente per fornire un messaggio di errore descrittivi.

Passaggio successivo configurerà il *Courses.aspx* pagina (che usa un `EntityDataSource` controllo) per consentire l'aggiornamento e le operazioni di eliminazione e per visualizzare un messaggio di errore se si verifica un conflitto di concorrenza. Il `Course` entità non dispone di una concorrenza rilevamento colonna, quindi si utilizzerà lo stesso metodo che è stato eseguito con il `Department` entità: traccia i valori di tutte le proprietà non chiave.

Aprire il *SchoolModel* file. Per le proprietà non chiave del `Course` entità (`Title`, `Credits`, e `DepartmentID`), impostare il **modalità di concorrenza** proprietà `Fixed`. Successivamente, salvare e chiudere il modello di dati.

Aprire il *Courses.aspx* pagina e apportare le modifiche seguenti:

- Nel `CoursesEntityDataSource` di controllo, aggiungere `EnableUpdate="true"` e `EnableDelete="true"` gli attributi. Il tag di apertura per il controllo è ora simile all'esempio seguente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- Nel `CoursesGridView` controllare, modificare il `DataKeyNames` valore di attributo `"CourseID,Title,Credits,DepartmentID"`. Quindi aggiungere un `CommandField` elemento per il `Columns` elemento che mostra **modifica** e **eliminare** pulsanti (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). Il `GridView` controllo ora simile all'esempio seguente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Eseguire la pagina e creare una situazione di conflitto, come descritto in precedenza nella pagina reparti. Eseguire la pagina in due finestre del browser, fare clic su **modifica** nella stessa riga in ogni finestra e apportare una modifica diversa in ognuno di essi. Fare clic su **aggiornamento** in una finestra e quindi fare clic su **aggiornamento** in altra finestra. Quando fa clic su **aggiornamento** la seconda volta, viene visualizzata la pagina di errore risultante da un'eccezione non gestita di concorrenza.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Gestire l'errore in modo molto simile alla modalità di gestione per il `ObjectDataSource` controllo. Aprire il *Courses.aspx* pagina e nel `CoursesEntityDataSource` (controllo), specificare i gestori per il `Deleted` e `Updated` eventi. Il tag di apertura del controllo è ora simile all'esempio seguente:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Prima di `CoursesGridView` di controllo, aggiungere il seguente `ValidationSummary` controllo:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

In *Courses.aspx.cs*, aggiungere un `using` istruzione per il `System.Data` dello spazio dei nomi, aggiungere un metodo che verifica le eccezioni di concorrenza e aggiungere i gestori per il `EntityDataSource` del controllo `Updated` e `Deleted`gestori. Il codice risulterà simile al seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

L'unica differenza tra il codice e cosa è stato fatto il `ObjectDataSource` controllo è che in questo caso l'eccezione di concorrenza nel `Exception` proprietà dell'oggetto evento argomenti piuttosto che in tale eccezione `InnerException` proprietà.

Eseguire la pagina e creare nuovamente un conflitto di concorrenza. Questa fase viene visualizzato un messaggio di errore:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Introduzione alla gestione di conflitti di concorrenza è stata completata. L'esercitazione successiva forniranno indicazioni su come migliorare le prestazioni in un'applicazione web che usa Entity Framework.

>[!div class="step-by-step"]
[Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
[Successivo](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
