---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Gestione della concorrenza con Entity Framework 4.0 in un'applicazione Web 4 ASP.NET | Microsoft Docs
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione web di Contoso University specificano che viene creato da Getting Started with serie di esercitazioni in Entity Framework 4.0. POSSO...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 6b10aca944a322cae252666218aee4d5a2d6ed35
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831593"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Gestione della concorrenza con Entity Framework 4.0 in un'applicazione Web 4 ASP.NET
====================
da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web Contoso University specificano che viene creato per il [Introduzione a Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni. Se si non è stato completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) verrebbe creato. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creato dalla serie di esercitazioni complete. Se si hanno domande sulle esercitazioni, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente si è appreso come ordinare e filtro dei dati tramite il `ObjectDataSource` controllo ed Entity Framework. Questa esercitazione illustra le opzioni per la gestione della concorrenza in un'applicazione web ASP.NET che usa Entity Framework. Si creerà una nuova pagina web dedicato all'aggiornamento le assegnazioni di insegnati office. Gestire i problemi di concorrenza in tale pagina e nella pagina Departments creato in precedenza.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Un conflitto di concorrenza si verifica quando un utente modifica un record e un altro utente modifica lo stesso record prima che la modifica del primo utente venga scritto nel database. Se non si configura Entity Framework per rilevare questi conflitti, che aggiorna il database ultimo sovrascrive le modifiche di altri utenti. In molte applicazioni questo rischio è accettabile, e non è necessario configurare l'applicazione per gestire i conflitti di concorrenza possibili. (Se sono presenti pochi utenti o alcuni aggiornamenti o se non è realmente critico se sovrascrittura di alcune modifiche, il costo di programmazione per la concorrenza può superare i vantaggi.) Se non occorre preoccuparsi di conflitti di concorrenza, è possibile saltare questa esercitazione. le due esercitazioni rimanenti della serie non variano in base gli elementi elaborati in questo.

### <a name="pessimistic-concurrency-locking"></a>Concorrenza pessimistica (blocco)

Se è importante che l'applicazione eviti la perdita accidentale di dati in scenari di concorrenza, un metodo per garantire che ciò accada è l'uso dei blocchi di database. Questa operazione viene definita *concorrenza pessimistica*. Ad esempio, prima di leggere una riga da un database si richiede un blocco per l'accesso di sola lettura o per l'accesso in modalità aggiornamento. Se si blocca una riga per l'accesso di aggiornamento, nessun altro utente può bloccare la riga per l'accesso di sola lettura o di aggiornamento, perché riceverebbe una copia di dati dei quali è in corso la modifica. Se si blocca una riga per l'accesso in sola lettura, anche altri utenti possono bloccarla per l'accesso in sola lettura, ma non per l'aggiornamento.

La gestione dei blocchi presenta alcuni svantaggi. La sua programmazione può risultare complicata. Richiede risorse di gestione di database significativa, e può causare problemi di prestazioni come il numero di utenti di un'applicazione aumenta (vale a dire non è facilmente scalabile). Per questi motivi non tutti i sistemi di gestione di database supportano la concorrenza pessimistica. Entity Framework non fornisce alcun supporto predefinito per questa e in questa esercitazione non illustra come implementarla.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

È l'alternativa alla concorrenza pessimistica *la concorrenza ottimistica*. Nella concorrenza ottimistica si consente che i conflitti di concorrenza si verifichino, quindi si reagisce con le modalità appropriate. Ad esempio, Giorgio esegue la *Department.aspx* pagina, fa clic sul **modificare** il collegamento per il reparto di cronologia e riduce il **Budget** quantità dal 1,000,000.00 $ $ $ 125,000.00. (John amministra un reparto concorrente e desidera liberare denaro per il proprio dipartimento).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Prima di John fa clic su **Update**, Jane esegue la stessa pagina, fa clic il **modificare** collegamento per il reparto di cronologia e quindi le modifiche il **data di inizio** campo a 1/1/10/1/2011 1999. (Jane consente di amministrare il reparto di cronologia e vuole assegnargli anzianità altre).

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John fa clic su **Update** prima di tutto, Jane deve fare clic **Update**. Gli elenchi di ora del browser di Jane il **Budget** amount come $1,000,000.00, ma questo non è corretto perché la quantità è stata modificata da John a $125,000.00.

Alcune delle azioni che è possibile eseguire in questo scenario includono quanto segue:

- È possibile tenere traccia della proprietà che un utente ha modificato e aggiornare solo le colonne corrispondenti nel database. Nello scenario dell'esempio non si perde nessun dato, perché i due utenti hanno aggiornato proprietà diverse. La volta successiva che un utente torna a visualizzare il reparto di cronologia, essi visualizzeranno 1/1/1999 e $125,000.00. 

    Questo è il comportamento predefinito in Entity Framework e può ridurre notevolmente il numero di conflitti che potrebbero causare la perdita di dati. Tuttavia, questo comportamento non evita la perdita di dati se vengono apportate modifiche concorrenti alla stessa proprietà di un'entità. Inoltre, questo comportamento non è sempre possibile; Quando si esegue il mapping di stored procedure a un tipo di entità, tutte le proprietà di un'entità vengono aggiornate quando vengono apportate modifiche all'entità nel database.
- È possibile consentire la modifica di Jane sovrascrivere modifica di John. Dopo che Jane fa clic su **Update**, il **Budget** quantità viene reimpostato $1,000,000.00. Questo scenario è detto *Priorità client* o *Last in Wins* (Priorità ultimo accesso). (Valori del client hanno la precedenza su ciò che si trova nell'archivio dati).
- È possibile impedire modifiche di Jane venga aggiornato nel database. In genere, si potrebbe visualizzare un messaggio di errore relativa Mostra lo stato corrente dei dati e consentirle di immettere nuovamente le proprie modifiche se Anna vuole renderli. È possibile automatizzare il processo di ulteriormente salvando l'input e relativa fornendo un'opportunità per associarlo nuovamente senza che sia necessario immetterlo. Questo scenario è detto *Store Wins* (Priorità archivio). I valori dell'archivio dati hanno la precedenza sui valori inoltrati dal client.

### <a name="detecting-concurrency-conflicts"></a>Il rilevamento dei conflitti di concorrenza

In Entity Framework, è possibile risolvere i conflitti gestendo `OptimisticConcurrencyException` le eccezioni che Entity Framework genera un'eccezione. Per determinare quando generare queste eccezioni, Entity Framework deve essere in grado di rilevare i conflitti. Pertanto è necessario configurare il database e il modello di dati in modo appropriato. Di seguito sono elencate alcune opzioni per abilitare il rilevamento dei conflitti:

- Nel database, includere una colonna di tabella che può essere utilizzata per determinare quando è stata modificata una riga. È quindi possibile configurare Entity Framework per includere tale colonna i `Where` clausola SQL `Update` o `Delete` comandi.

    Lo scopo del `Timestamp` colonna il `OfficeAssignment` tabella.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Il tipo di dati di `Timestamp` colonna è l'acronimo `Timestamp`. Tuttavia, la colonna non contiene effettivamente un valore data o ora. Al contrario, il valore è un numero sequenziale che viene incrementato ogni volta che viene aggiornata la riga. In un' `Update` oppure `Delete` comando, il `Where` clausola include dell'originale `Timestamp` valore. Se la riga aggiornata è stata modificata da un altro utente, il valore in `Timestamp` è diverso dal valore originale, in modo che il `Where` clausola viene restituita alcuna riga da aggiornare. Quando Entity Framework rileva che nessuna riga è stata aggiornata dall'oggetto corrente `Update` o `Delete` comando (ovvero, quando il numero di righe interessate è pari a zero), interpreta questo fatto come un conflitto di concorrenza.
- Configurare Entity Framework per includere i valori originali di ogni colonna nella tabella di `Where` della clausola `Update` e `Delete` comandi.

    Come la prima opzione, qualsiasi elemento nella riga è stato modificato dopo la riga è stato letto prima di tutto, il `Where` clausola non restituisce una riga da aggiornare, che Entity Framework interpreta come un conflitto di concorrenza. Questo metodo è altrettanto efficace come usando un `Timestamp` campo, ma può risultare inefficiente. Per le tabelle di database con molte colonne, ciò può comportare grandi `Where` clausole, e in un'applicazione web possibile richiede la gestione di grandi quantità di stato. Gestione di grandi quantità di stato può influire sulle prestazioni dell'applicazione perché richiede risorse di server (ad esempio, lo stato della sessione) oppure deve essere incluso nella pagina web stessa (ad esempio, lo stato di visualizzazione).

In questa esercitazione si aggiungerà la gestione di conflitti di concorrenza ottimistica per un'entità che non dispone di una proprietà di rilevamento (il `Department` entità) e per un'entità che dispongono di una proprietà di rilevamento (la `OfficeAssignment` entità).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Gestione della concorrenza ottimistica senza una proprietà di rilevamento

Per implementare la concorrenza ottimistica per il `Department` entità, che non ha un rilevamento (`Timestamp`) proprietà, si completeranno le attività seguenti:

- Modificare il modello di dati per abilitare il rilevamento per la concorrenza `Department` entità.
- Nel `SchoolRepository` classe, la gestione delle eccezioni di concorrenza nel `SaveChanges` (metodo).
- Nel *Departments.aspx* pagina, la gestione delle eccezioni di concorrenza tramite la visualizzazione di un messaggio all'utente di avviso che tali modifiche hanno avuto esito positivo. L'utente può quindi visualizzare i valori correnti e ripetere le modifiche se sono ancora necessari.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Abilitazione della concorrenza nel modello di dati di rilevamento

In Visual Studio, aprire l'applicazione web di Contoso University specificano che si stava lavorando nell'esercitazione precedente di questa serie.

Open *SchoolModel*e in Progettazione modelli di dati, fare doppio clic il `Name` proprietà nel `Department` entità e quindi fare clic su **proprietà**. Nel **delle proprietà** finestra Modifica il `ConcurrencyMode` proprietà `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Eseguire la stessa operazione per le altre proprietà di scalare non-primary-key (`Budget`, `StartDate`, e `Administrator`.) (È Impossibile eseguire questa operazione per le proprietà di navigazione.) Specifica che ogni volta che Entity Framework genera una `Update` o `Delete` comando SQL per aggiornare le `Department` entità nel database, queste colonne (con i valori originali) devono essere incluso nel `Where` clausola. Se viene trovata alcuna riga quando la `Update` o `Delete` esecuzione del comando, Entity Framework genera un'eccezione di concorrenza ottimistica.

Salvare e chiudere il modello di dati.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Gestione delle eccezioni di concorrenza in DAL

Aprire *SchoolRepository.cs* e aggiungere quanto segue `using` istruzione per il `System.Data` dello spazio dei nomi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Aggiungere la seguente nuovo `SaveChanges` metodo che gestisce le eccezioni di concorrenza ottimistica:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Se un errore di concorrenza si verifica quando viene chiamato questo metodo, i valori delle proprietà dell'entità in memoria vengono sostituiti con i valori presenti nel database. Viene nuovamente generata l'eccezione di concorrenza in modo che la pagina web possa gestirla.

Nel `DeleteDepartment` e `UpdateDepartment` metodi, sostituire la chiamata a esistenti `context.SaveChanges()` con una chiamata a `SaveChanges()` per poter richiamare il nuovo metodo.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Gestione delle eccezioni di concorrenza nel livello di presentazione

Aprire *Departments.aspx* e aggiungere un' `OnDeleted="DepartmentsObjectDataSource_Deleted"` attributo di `DepartmentsObjectDataSource` controllo. Il tag di apertura per il controllo sarà ora simile all'esempio seguente.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Nel `DepartmentsGridView` controllare, specificare tutte le colonne della tabella nel `DataKeyNames` attributo, come illustrato nell'esempio seguente. Si noti che verrà creato visualizzazioni molto grande campi di stato, è questo il motivo motivi per cui Usa un campo di rilevamento sono in genere il modo migliore per rilevare i conflitti di concorrenza.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Aprire *Departments.aspx.cs* e aggiungere quanto segue `using` istruzione per il `System.Data` dello spazio dei nomi:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Aggiungere il nuovo metodo seguente, che verrà chiamato del controllo origine dati `Updated` e `Deleted` gestori eventi per la gestione delle eccezioni di concorrenza:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Questo codice verifica il tipo di eccezione e, se è un'eccezione di concorrenza, il codice crea dinamicamente una `CustomValidator` controllo che a sua volta viene visualizzato un messaggio nel `ValidationSummary` controllo.

Il nuovo metodo da chiamare il `Updated` gestore evento aggiunti in precedenza. Inoltre, creare un nuovo `Deleted` gestore dell'evento che chiama il metodo di stesso (ma non esegue altre operazioni):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Test della concorrenza ottimistica nella pagina di reparti

Eseguire la *Departments.aspx* pagina.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Fare clic su **modifica** in una riga e modificare il valore di **Budget** colonna. (Tenere presente che è possibile modificare solo i record creati per questa esercitazione, perché l'oggetto esistente `School` record del database contengono alcuni dati non validi. Il record per il reparto di economia è sicuro per sperimentare.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Aprire una nuova finestra del browser ed eseguire nuovamente la pagina (copia l'URL dalla casella indirizzo prima della finestra del browser per la seconda finestra del browser).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Fare clic su **Edit** nella stessa riga è modificato in precedenza e modificare le **Budget** valore a un elemento diverso.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Nella seconda finestra del browser, fare clic su **Update**. Il **Budget** quantità viene modificata correttamente in questo nuovo valore.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Nella prima finestra del browser, fare clic su **Update**. L'aggiornamento non riesce. Il **Budget** quantità viene nuovamente visualizzata usando il valore impostato nella seconda finestra del browser e viene visualizzato un messaggio di errore.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Gestione della concorrenza ottimistica tramite una proprietà di rilevamento

Per gestire la concorrenza ottimistica per un'entità che dispone di una proprietà di rilevamento, si completeranno le attività seguenti:

- Aggiungere stored procedure al modello di dati per gestire `OfficeAssignment` entità. (Proprietà di rilevamento e le stored procedure non devono necessariamente essere usati insieme, essi sono semplicemente raggruppati insieme qui a scopo illustrativo).
- Aggiungere metodi CRUD DAL e BLL per `OfficeAssignment` entità, incluso il codice per gestire le eccezioni di concorrenza ottimistica nel DAL.
- Creare una pagina web le assegnazioni di ufficio.
- Test della concorrenza ottimistica nella nuova pagina web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Aggiunta di OfficeAssignment Stored procedure al modello di dati

Aprire il *SchoolModel* in Progettazione modelli di file, fare doppio clic nell'area di progettazione e fare clic su **Aggiorna modello da Database**. Nel **Add** scheda della finestra di **Scegli oggetti di Database** finestra di dialogo, espandere **Stored procedure** e selezionare i tre `OfficeAssignment` stored procedure (vedere la screenshot seguente), quindi fare clic su **fine**. (Queste stored procedure erano già presenti nel database quando si è scaricata o creata usando uno script.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Fare doppio clic il `OfficeAssignment` entità e selezionare **Mapping Stored Procedure**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Impostare il **inserire**, **Update**, e **Elimina** funzioni da usare le relative stored procedure. Per il `OrigTimestamp` parametro del `Update` impostare il **proprietà** al `Timestamp` e selezionare il **utilizza Original Value** opzione.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Quando Entity Framework chiama il `UpdateOfficeAssignment` stored procedure, passerà il valore originale del `Timestamp` colonna il `OrigTimestamp` parametro. La stored procedure utilizza questo parametro nel relativo `Where` clausola:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

La stored procedure consente di selezionare anche il nuovo valore della `Timestamp` colonna dopo l'aggiornamento in modo che Entity Framework è possibile mantenere il `OfficeAssignment` entità a cui è in memoria sincronizzati con la riga del database corrispondente.

(Si noti che la stored procedure per l'eliminazione di un'assegnazione di ufficio non ha un `OrigTimestamp` parametro. Per questo motivo, Entity Framework non è possibile verificare che un'entità non è stata modificata prima di eliminarlo.)

Salvare e chiudere il modello di dati.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Aggiunta di metodi OfficeAssignment a DAL

Aprire *ISchoolRepository.cs* e aggiungere i seguenti metodi CRUD per la `OfficeAssignment` set di entità:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Aggiungere i nuovi metodi seguenti per *SchoolRepository.cs*. Nel `UpdateOfficeAssignment` metodo, si chiama locale `SaveChanges` invece del metodo `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Nel progetto di test, aprire *MockSchoolRepository.cs* e aggiungere quanto segue `OfficeAssignment` insieme e metodi CRUD a esso. (Il repository fittizio deve implementare l'interfaccia del repository o la soluzione non verrà compilato.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Aggiunta di metodi OfficeAssignment per il livello BLL

Nel progetto principale, aprire *SchoolBL.cs* e aggiungere i seguenti metodi CRUD per i `OfficeAssignment` del set di entità ad esso:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Creazione di una pagina Web OfficeAssignments

Creare una nuova pagina web che usa il *Site. master* pagina master e denominarlo *OfficeAssignments.aspx*. Aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Si noti che nel `DataKeyNames` dell'attributo, il markup consente di specificare il `Timestamp` proprietà, nonché la chiave del record (`InstructorID`). Specifica delle proprietà nel `DataKeyNames` attributo fa sì che il controllo di salvarli in stato di controllo (che è simile allo stato di visualizzazione) in modo che i valori originali sono disponibili durante l'elaborazione del postback.

Se non è stato salvato il `Timestamp` valore, Entity Framework non avrà it per il `Where` clausola SQL `Update` comando. Di conseguenza nulla verrà individuato da aggiornare. Di conseguenza, Entity Framework genera un'eccezione di concorrenza ottimistica ogni volta che un `OfficeAssignment` entità viene aggiornato.

Aprire *OfficeAssignments.aspx.cs* e aggiungere quanto segue `using` istruzione per il livello di accesso ai dati:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Aggiungere il codice seguente `Page_Init` metodo, che abilita la funzionalità di Dynamic Data. Aggiungere anche il gestore seguente per il `ObjectDataSource` del controllo `Updated` eventi per verificare la presenza di errori di concorrenza:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Test della concorrenza ottimistica nella pagina OfficeAssignments

Eseguire la *OfficeAssignments.aspx* pagina.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Fare clic su **modifica** in una riga e modificare il valore di **percorso** colonna.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Aprire una nuova finestra del browser ed eseguire nuovamente la pagina (copia l'URL dalla finestra del browser prima alla seconda finestra del browser).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Fare clic su **Edit** nella stessa riga è modificato in precedenza e modificare le **posizione** valore a un elemento diverso.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Nella seconda finestra del browser, fare clic su **Update**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Passare alla finestra del browser prima e fare clic su **Update**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Viene visualizzato un messaggio di errore e il **posizione** valore è stato aggiornato per mostrare il valore è stata modificata da per la seconda finestra del browser.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Gestione della concorrenza con il controllo EntityDataSource

Il `EntityDataSource` controllo include una logica incorporata che riconosce le impostazioni di concorrenza nel modello di dati e gli handle di aggiornano e di conseguenza le operazioni di eliminazione. Tuttavia, come con tutte le eccezioni, è necessario gestire `OptimisticConcurrencyException` eccezioni autonomamente per fornire un messaggio di errore descrittivo.

Successivamente, si configurerà il *Courses.aspx* pagina (che usa un `EntityDataSource` controllo) per consentire l'aggiornamento e le operazioni di eliminazione e per visualizzare un messaggio di errore se si verifica un conflitto di concorrenza. Il `Course` entità non dispone di una colonna, per il rilevamento, quindi si utilizzerà il metodo di stesso che è stato fatto con la concorrenza di `Department` entità: rilevare valori di tutte le proprietà non chiave.

Aprire il *SchoolModel* file. Per le proprietà non chiave del `Course` entity (`Title`, `Credits`, e `DepartmentID`), impostare il **modalità di concorrenza** proprietà `Fixed`. Quindi salvare e chiudere il modello di dati.

Aprire il *Courses.aspx* pagina e apportare le modifiche seguenti:

- Nel `CoursesEntityDataSource` controllo, aggiungere `EnableUpdate="true"` e `EnableDelete="true"` attributi. Il tag di apertura per il controllo è ora simile al seguente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- Nel `CoursesGridView` controllare, modificare il `DataKeyNames` al valore dell'attributo `"CourseID,Title,Credits,DepartmentID"`. Quindi aggiungere una `CommandField` elemento per il `Columns` elemento che mostra **modificare** e **Elimina** pulsanti (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). Il `GridView` controllo sarà ora simile all'esempio seguente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Eseguire la pagina e creare una situazione di conflitto, come descritto in precedenza nella pagina di reparti. Eseguire la pagina in due finestre del browser, fare clic su **modifica** nella stessa riga in ogni finestra e apportare una modifica diversi in ognuno di essi. Fare clic su **Update** in una finestra e quindi fare clic su **Update** in altra finestra. Quando fa clic su **Update** la seconda volta, viene visualizzata la pagina di errore che risulta da un'eccezione non gestita della concorrenza.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Gestire questo errore in modo molto simile alla modalità di gestione per il `ObjectDataSource` controllo. Aprire il *Courses.aspx* pagina e nel `CoursesEntityDataSource` (controllo), specificano i gestori per il `Deleted` e `Updated` eventi. Il tag di apertura del controllo è ora simile al seguente:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Prima di `CoursesGridView` controllo, aggiungere il codice seguente `ValidationSummary` controllo:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

Nella *Courses.aspx.cs*, aggiungere un `using` istruzione per il `System.Data` dello spazio dei nomi, aggiungere un metodo che verifica per le eccezioni di concorrenza e aggiungere i gestori per il `EntityDataSource` del controllo `Updated` e`Deleted`gestori eventi. Il codice avrà un aspetto simile al seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

L'unica differenza tra questo codice ed è stato fatto per la `ObjectDataSource` controllo è che in questo caso l'eccezione di concorrenza è nel `Exception` proprietà dell'oggetto argomenti evento invece che nel caso di eccezione `InnerException` proprietà.

Eseguire la pagina e creare di nuovo un conflitto di concorrenza. Questa volta verrà visualizzato un messaggio di errore:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Questo argomento completa l'introduzione alla gestione dei conflitti di concorrenza. L'esercitazione successiva fornirà indicazioni su come migliorare le prestazioni in un'applicazione web che usa Entity Framework.

> [!div class="step-by-step"]
> [Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Successivo](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
