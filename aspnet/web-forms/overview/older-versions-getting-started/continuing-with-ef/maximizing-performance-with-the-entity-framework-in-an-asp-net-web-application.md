---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Ottimizzare le prestazioni con Entity Framework 4.0 in un'applicazione Web 4 ASP.NET | Documenti Microsoft
author: tdykstra
description: Questa serie di esercitazioni compila nell'applicazione web di Contoso University creato da introduttiva la serie di esercitazioni di Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 9e257f5061f6bdf14ad0776ff6385fb526d6dcb1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Ottimizzazione delle prestazioni con Entity Framework 4.0 in un'applicazione Web 4 ASP.NET
====================
Da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web di Contoso University creando il [introduzione di Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni. Se non ha completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) che consente di creare. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creati tramite la serie di esercitazioni completo. Nel caso di problemi con le esercitazioni, è possibile registrarli per il [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente, è stato illustrato come gestire i conflitti di concorrenza. Questa esercitazione vengono illustrate le opzioni per migliorare le prestazioni di un'applicazione web ASP.NET che usa Entity Framework. Si apprenderà diversi metodi per ottimizzare le prestazioni o per la diagnosi dei problemi di prestazioni.

Informazioni fornite nelle sezioni seguenti sono probabile che sia utile in una vasta gamma di scenari:

- Caricare i dati correlati in modo efficiente.
- Gestire lo stato di visualizzazione.

Informazioni fornite nelle sezioni seguenti potrebbero essere utile se si dispone di singole query che presentano problemi di prestazioni:

- Utilizzare il `NoTracking` merge (opzione).
- La precompilazione le query LINQ.
- Esaminare i comandi di query inviati al database.

Informazioni presentate nella sezione seguente sono potenzialmente utile per le applicazioni che dispongono di modelli molto grandi quantità di dati:

- Pregenerare le visualizzazioni.

> [!NOTE]
> Le prestazioni dell'applicazione Web dipende da numerosi fattori, inclusi elementi quali le dimensioni dei dati di richiesta e risposta, la velocità delle query di database, il numero di richieste che server possibile mettere in coda e rapidità con cui può essere utilizzato e anche l'efficienza di qualsiasi è possibile utilizzare librerie di script client. Se le prestazioni sono critiche nell'applicazione in uso o se il test o l'esperienza mostra che le prestazioni dell'applicazione non sono soddisfacente, è necessario seguire il protocollo normale per l'ottimizzazione delle prestazioni. Misure per determinare dove si verificano colli di bottiglia delle prestazioni e risolvere le aree che hanno il maggiore impatto sulle prestazioni complessive dell'applicazione.
> 
> In questo argomento è incentrato soprattutto sulla modalità in cui è potenzialmente possibile migliorare le prestazioni in particolare di Entity Framework di ASP.NET. I suggerimenti riportati di seguito sono utili se si determina che l'accesso ai dati è uno dei colli di bottiglia delle prestazioni dell'applicazione. La differenza di quanto indicato, i metodi illustrati in questo argomento non devono essere considerata come &quot;procedure consigliate&quot; in generale, molte di esse sono appropriati solo in situazioni eccezionali o per tipi specifici di indirizzo di colli di bottiglia delle prestazioni.


Per avviare l'esercitazione, avviare Visual Studio e aprire l'applicazione web di Contoso università che si stava lavorando nell'esercitazione precedente.

## <a name="efficiently-loading-related-data"></a>In modo efficiente il caricamento dei dati correlati

Esistono diversi modi di Entity Framework può caricare i dati correlati alle proprietà di navigazione di un'entità:

- *Caricamento lazy*. Durante la lettura di entità, non recuperare i dati correlati. La prima volta che si tenta di accedere a una proprietà di navigazione, viene tuttavia recuperati automaticamente i dati necessari per la proprietà di navigazione. Di conseguenza, più query inviate al database, ovvero uno per l'entità stessa e uno ogni volta che i dati per l'entità correlati deve essere recuperato. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Caricamento eager*. Durante la lettura di entità, i dati correlati vengono recuperati con essa. Ciò comporta in genere una query singola join che recupera tutti i dati necessari. Per specificare il caricamento non differito, utilizzare il `Include` (metodo), come visto già in queste esercitazioni.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Caricamento esplicito*. È simile al caricamento lazy, ad eccezione del fatto che vengano recuperate in modo esplicito i dati correlati nel codice. non avviene automaticamente quando si accede a una proprietà di navigazione. Caricare i dati correlati manualmente tramite il `Load` metodo della proprietà di navigazione per le raccolte oppure utilizzare il `Load` metodo della proprietà di riferimento per le proprietà che contengono un singolo oggetto. (Ad esempio, si chiama il `PersonReference.Load` per caricare il `Person` proprietà di navigazione di un `Department` entità.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Perché non immediatamente recuperano i valori delle proprietà, il caricamento differito e il caricamento esplicito sono entrambi noto anche come *il caricamento posticipato*.

Caricamento lazy è il comportamento predefinito per un contesto dell'oggetto che è stato generato dalla finestra di progettazione. Se si apre il *SchoolModel.Designer.cs* file che definisce la classe del contesto di oggetto, sono disponibili tre metodi del costruttore e ognuno di essi include l'istruzione seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

In generale, se si conosce sono necessari i dati correlati per ogni entità caricamento eager, recuperato offre le migliori prestazioni, perché una singola query inviate al database è in genere più efficiente delle query separate per ogni entità recuperati. D'altra parte, se si desidera accedere alle proprietà di navigazione di un'entità solo raramente o solo per un piccolo set di entità, il caricamento lazy o caricamento esplicito può essere più efficiente, perché il caricamento non differito dovrà recuperare più dati è necessario.

In un'applicazione web, il caricamento lazy può essere di relativamente scarso valore comunque, perché le azioni dell'utente che interessano la necessità di dati correlati vengono eseguite in browser che dispone di alcuna connessione al contesto dell'oggetto a cui il rendering della pagina. D'altra parte, quando si databind un controllo, è in genere conoscere quali i dati necessari e pertanto è in genere migliore per scegliere caricamento eager o il caricamento posticipato in base alle quali è appropriato in ogni scenario.

Inoltre, un controllo con associazione a dati potrebbe utilizzare un oggetto entità dopo aver eliminato il contesto dell'oggetto. In tal caso, un tentativo di caricamento lazy una proprietà di navigazione avrà esito negativo. Il messaggio di errore visualizzato è ovvia:&quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

Il `EntityDataSource` controllo Disabilita il caricamento lazy per impostazione predefinita. Per il `ObjectDataSource` controllo che si sta utilizzando per l'esercitazione corrente (o per accedere al contesto dell'oggetto codice della pagina), esistono diversi modi per rendere lazy caricamento disabilitato per impostazione predefinita. Quando si crea un'istanza di un contesto dell'oggetto, è possibile disabilitarla. Ad esempio, è possibile aggiungere la riga seguente al metodo del costruttore del `SchoolRepository` classe:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Per l'applicazione Contoso University, ti contesto dell'oggetto automaticamente disabilitare il caricamento lazy in modo che questa proprietà non è necessario impostare ogni volta che viene creata un'istanza di un contesto.

Aprire il *SchoolModel* modello di dati, fare clic nell'area di progettazione e quindi nel riquadro Proprietà impostare il **durante il caricamento Lazy abilitato** proprietà `False`. Salvare e chiudere il modello di dati.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Gestione dello stato di visualizzazione

Per fornire funzionalità di aggiornamento, quando la pagina viene visualizzata una pagina web ASP.NET necessario archiviare i valori originali delle proprietà di un'entità. Durante l'elaborazione del controllo di postback ricreare lo stato originale dell'entità, chiamare l'entità `Attach` metodo prima di applicare le modifiche e la chiamata di `SaveChanges` metodo. Per impostazione predefinita, i controlli Web Form ASP.NET dati utilizzano lo stato di visualizzazione per archiviare i valori originali. Tuttavia, lo stato di visualizzazione può sulle prestazioni, perché è archiviata nei campi nascosti che consente di aumentare notevolmente le dimensioni della pagina che viene inviata al e dal browser.

Tecniche per la gestione dello stato di visualizzazione o alternative, ad esempio lo stato della sessione, non sono univoche a Entity Framework, pertanto in questa esercitazione non entra in questo argomento. Per ulteriori informazioni, vedere i collegamenti alla fine dell'esercitazione.

Tuttavia, la versione 4 di ASP.NET fornisce un nuovo modo di lavorare con lo stato di visualizzazione che ogni sviluppatore ASP.NET di applicazioni Web Form da tenere in considerazione: la `ViewStateMode` proprietà. Questa nuova proprietà può essere impostata a livello di pagina o controllo e consente di disabilitare lo stato di visualizzazione per impostazione predefinita per una pagina e abilitarlo solo per i controlli che ne hanno necessità.

Per le applicazioni in cui le prestazioni sono critiche, una procedura consigliata è sempre disabilitare lo stato di visualizzazione a livello di pagina e abilitarlo solo per i controlli che lo richiedono. Le dimensioni dello stato di visualizzazione nelle pagine della University Contoso non sarebbe possibile diminuire sostanzialmente da questo metodo, ma per verificarne il funzionamento, è possibile farlo il *Instructors.aspx* pagina. Questa pagina contiene molti controlli, ad esempio, un `Label` controllo che ha disabilitato lo stato di visualizzazione. Nessuno dei controlli in questa pagina effettivamente necessario visualizzare lo stato abilitato. (Il `DataKeyNames` proprietà del `GridView` controllo specifica lo stato che deve essere mantenuto tra i vari postback, ma questi valori vengono mantenuti nello stato di controllo, non viene influenzato dal `ViewStateMode` proprietà.)

Il `Page` direttiva e `Label` markup del controllo attualmente simile all'esempio seguente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Apportare le modifiche seguenti:

- Aggiungere `ViewStateMode="Disabled"` per il `Page` direttiva.
- Rimuovere `ViewStateMode="Disabled"` dal `Label` controllo.

Il codice è ora simile all'esempio seguente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Lo stato di visualizzazione è disabilitato per tutti i controlli. Se in un secondo momento, si aggiunge un controllo che devono utilizzare lo stato di visualizzazione, è sufficiente è includono il `ViewStateMode="Enabled"` attributo per tale controllo.

## <a name="using-the-notracking-merge-option"></a>Utilizzando l'opzione di unione NoTracking

Quando un contesto dell'oggetto recupera le righe di database e crea gli oggetti di entità che li rappresentano, per impostazione predefinita tiene traccia anche tali oggetti entità utilizzando la gestione dello stato degli oggetti. Questi dati di rilevamento funge da cache e viene utilizzati quando si aggiorna un'entità. Perché un'applicazione web è in genere le istanze di contesto di oggetto di breve durata, le query spesso restituiscono dati che non devono essere registrate, perché verrà eliminato prima di una delle entità che legge nuovamente il contesto dell'oggetto che vengono letti o aggiornati.

In Entity Framework, è possibile specificare se il contesto dell'oggetto tiene traccia degli oggetti di entità impostando un *opzione di unione*. È possibile impostare l'opzione di merge per le singole query o per set di entità. Se si imposta la proprietà per un set di entità, che significa che si sta impostando l'opzione di unione predefinita per tutte le query che vengono creati per il set di entità.

Per l'applicazione Contoso University, rilevamento non è necessaria per uno dei set di entità a cui si accede dal repository, pertanto è possibile impostare l'opzione di merge `NoTracking` per i set di entità quando si crea un'istanza del contesto dell'oggetto nella classe del repository. (Si noti che in questa esercitazione, impostare l'opzione di unione avrà un impatto negativo sulle prestazioni dell'applicazione. Di `NoTracking` è probabile che un miglioramento delle prestazioni observable solo in determinati scenari di alto volume di dati.)

Nella cartella di DAL, aprire il *SchoolRepository.cs* file e aggiungere un metodo del costruttore che imposta l'opzione di merge per set di entità che accede a repository:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Query LINQ pre-compilazione

La prima volta che Entity Framework viene eseguita una query Entity SQL nella durata di un determinato `ObjectContext` istanza, comporta un tempo per compilare la query. Il risultato della compilazione viene memorizzato nella cache, il che significa che le successive esecuzioni della query sono molto più rapidamente. Le query LINQ seguono un modello simile, ad eccezione del fatto che alcune delle operazioni necessarie per compilare la query viene eseguita ogni volta che viene eseguita la query. In altre parole, per le query LINQ, per impostazione predefinita tutti i risultati della compilazione vengono memorizzati nella cache.

Se si dispone di una query LINQ che si prevede di eseguire ripetutamente in tutta la durata di un contesto dell'oggetto, è possibile scrivere codice che fa sì che tutti i risultati della compilazione da memorizzare nella cache la prima volta che viene eseguita la query LINQ.

Ad esempio, questo scopo per due `Get` metodi il `SchoolRepository` (classe), uno dei quali non accetta parametri (il `GetInstructorNames` metodo) e uno che richiedono un parametro (il `GetDepartmentsByAdministrator` (metodo)). Questi metodi restano immutati ora effettivamente non devono essere compilati perché non sono le query LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Tuttavia, in modo che è possibile provare le query compilate, sarà procedere come se questi fosse stata scritta come le query LINQ seguenti:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Che cos'è illustrato in precedenza ed eseguire l'applicazione per verificare che il funzionamento prima di continuare, è possibile modificare il codice in questi metodi. Tuttavia, le istruzioni seguenti subito a creare versioni pre-compilate.

Creare un file di classe nel *DAL* cartella, il nome *SchoolEntities.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Questo codice crea una classe parziale che estende la classe di contesto di oggetto generato automaticamente. La classe parziale include due query LINQ compilate utilizzando il `Compile` metodo la `CompiledQuery` classe. Crea inoltre metodi che è possibile utilizzare per chiamare le query. Salvare e chiudere il file.

Successivamente, nel *SchoolRepository.cs*, modificare esistente `GetInstructorNames` e `GetDepartmentsByAdministrator` metodi nel repository di classe in modo che chiamano le query compilate:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Eseguire il *Departments.aspx* pagina per verificare se funziona come in precedenza. Il `GetInstructorNames` metodo viene chiamato per popolare l'elenco a discesa, amministratore e `GetDepartmentsByAdministrator` metodo viene chiamato quando si fa clic su **aggiornamento** per verificare che nessun istruttore sia un amministratore di più reparto.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

È stato pre-compilate le query nell'applicazione Contoso University solo per informazioni su come eseguire l'operazione, non perché sarebbe per migliorare le prestazioni. Pre-compilazione di query LINQ di aggiungere un livello di complessità al codice, quindi assicurarsi che farlo solo per le query che rappresentano effettivamente eventuali colli di bottiglia nell'applicazione.

## <a name="examining-queries-sent-to-the-database"></a>Esaminare le query inviate al Database

Quando si sta verificando problemi di prestazioni, talvolta è utile conoscere l'esatti comandi SQL che invia al database di Entity Framework. Se si utilizza un `IQueryable` oggetto, un modo per eseguire questa operazione consiste nell'utilizzare il `ToTraceString` metodo.

In *SchoolRepository.cs*, modificare il codice di `GetDepartmentsByName` metodo affinché corrisponda all'esempio seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

Il `departments` variabile deve essere impostata un `ObjectQuery` digitare solo perché il `Where` metodo alla fine della riga precedente crea un `IQueryable` ; dell'oggetto senza il `Where` (metodo), il cast non è necessario.

Impostare un punto di interruzione di `return` di riga e quindi eseguire il *Departments.aspx* pagina nel debugger. Quando si raggiunge il punto di interruzione, esaminare il `commandText` variabile il **variabili locali** finestra e utilizzare il Visualizzatore di testo (lente di ingrandimento nel **valore** colonna) per visualizzarne il valore nel **Visualizzatore testo** finestra. È possibile visualizzare l'intero comando SQL risultante da questo codice:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

In alternativa, la funzionalità di IntelliTrace in Visual Studio Ultimate consente di visualizzare comandi SQL generati da Entity Framework che non richiede di modificare il codice o persino impostare un punto di interruzione.

> [!NOTE]
> È possibile eseguire le procedure seguenti solo se si dispone di Visual Studio Ultimate.


Ripristinare il codice originale il `GetDepartmentsByName` (metodo) e quindi eseguire il *Departments.aspx* pagina nel debugger.

In Visual Studio, selezionare il **Debug** menu, quindi **IntelliTrace**e quindi **eventi IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Nel **IntelliTrace** finestra, fare clic su **Interrompi tutto**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Il **IntelliTrace** finestra viene visualizzato un elenco di eventi recenti:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Fare clic su di **ADO.NET** riga. Espande per mostrare il testo del comando:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

È possibile copiare la stringa di testo intero comando negli Appunti dal **variabili locali** finestra.

Si supponga che stava lavorando con un database con più tabelle, relazioni e colonne più semplici `School` database. È possibile che una query che consente di raccogliere tutte le informazioni necessarie in una singola `Select` istruzione che contiene più `Join` clausole diventa troppo complesso per lavorare in modo efficiente. In questo caso è possibile passare da eager durante il caricamento per il caricamento esplicito per semplificare la query.

Ad esempio, provare a modificare il codice di `GetDepartmentsByName` metodo *SchoolRepository.cs*. Attualmente in metodo dispone di una query di oggetto che ha `Include` metodi per il `Person` e `Courses` le proprietà di navigazione. Sostituire il `return` istruzione con il codice che esegue il caricamento esplicito, come illustrato nell'esempio seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Eseguire il *Departments.aspx* pagina nel debugger e controllare il **IntelliTrace** finestra nuovamente durante la precedenza. A questo punto, in cui era presente una singola query prima di, vedere una lunga sequenza di essi.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Fare clic sul primo **ADO.NET** riga per vedere cosa è successo a query complesse è visualizzato in precedenza.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

La query da reparti è diventata una semplice `Select` query senza `Join` clausola, ma è seguita da query separata che recuperano i corsi correlati e un amministratore, tramite un set di due query per ogni reparto restituito dall'originale query.

> [!NOTE]
> Se si lascia lazy l'abilitazione del caricamento, il modello viene visualizzato in questo caso, con la stessa query ripetuta più volte, potrebbe comportare il caricamento lazy. Un modello che si desidera evitare in genere è il caricamento lazy dati correlati per ogni riga della tabella primaria. A meno che non aver verificato che una query singola join è troppo complessa per essere efficiente, in genere sarebbe in grado di migliorare le prestazioni in tali casi modificando la query principale per l'utilizzo di caricamento eager.


## <a name="pre-generating-views"></a>Pre-la generazione di viste

Quando un `ObjectContext` oggetto viene prima creato un nuovo dominio applicazione, Entity Framework genera un set di classi utilizzate per accedere al database. Queste classi vengono chiamate *viste*, e se si dispone di un modello di dati molto grandi, generazione di queste visualizzazioni possono ritardare la risposta del sito web per la prima richiesta per una pagina dopo l'inizializzazione di un nuovo dominio applicazione. È possibile ridurre il ritardo prima richiesta mediante la creazione di visualizzazioni in fase di compilazione anziché in fase di esecuzione.

> [!NOTE]
> Se l'applicazione non dispone di un modello di dati molto grandi o se dispone di un modello di dati di grandi dimensioni, ma non è necessario che un problema di prestazioni che interessa solo la prima richiesta di pagina dopo IIS viene riciclato, è possibile ignorare questa sezione. Visualizzazione di creazione non avviene ogni volta che si crea un'istanza un `ObjectContext` dell'oggetto, poiché le viste vengono memorizzati nella cache nel dominio dell'applicazione. Pertanto, a meno che non si è spesso riciclo dell'applicazione in IIS, può costituire un vantaggio poche richieste di pagine da visualizzazioni pregenerate.


È possibile pregenerare viste utilizzando la *EdmGen.exe* strumento da riga di comando o tramite un *Toolkit di trasformazione del modello di testo* modello (T4). In questa esercitazione si userà un modello T4.

Nel *DAL* cartella, aggiungere un file utilizzando il **modello di testo** modello (è nel **generale** nodo il **modelli installati** elenco), e denominarlo *SchoolModel.Views.tt*. Sostituire il codice esistente nel file con il codice seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Questo codice genera visualizzazioni per un *edmx* file che si trova nella stessa cartella del modello con lo stesso nome del file del modello. Ad esempio, se il file di modello è denominato *SchoolModel.Views.tt*, verrà cercato un file di modello di dati denominato *SchoolModel*.

Salvare il file, quindi il file in **Esplora** e selezionare **Esegui strumento personalizzato**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio genera un file di codice che crea le visualizzazioni, denominato *SchoolModel.Views.cs* in base al modello. (Si può notare che il file di codice viene generato anche prima di selezionare **Esegui strumento personalizzato**, non appena si salva il file di modello.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

È ora possibile eseguire l'applicazione e verificare se funziona come in precedenza.

Per ulteriori informazioni sulle viste generate in precedenza, vedere le risorse seguenti:

- [Procedura: generare in anticipo le visualizzazioni per migliorare le prestazioni di Query](https://msdn.microsoft.com/en-us/library/bb896240.aspx) nel sito web MSDN. Viene illustrato come utilizzare il `EdmGen.exe` strumento da riga di comando per pregenerare le visualizzazioni.
- [L'isolamento delle prestazioni con viste a precompilata/Pre-generated in Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) sul blog di Windows Server AppFabric Team di consulenza clienti.

Introduzione a migliorare le prestazioni in un'applicazione web ASP.NET che usa Entity Framework è stata completata. Per altre informazioni, vedere le seguenti risorse:

- [Considerazioni sulle prestazioni (Entity Framework)](https://msdn.microsoft.com/en-us/library/cc853327.aspx) nel sito web MSDN.
- [Post correlati alle prestazioni nel blog del Team di Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF opzioni di unione e query compilate](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Post di blog che spiega i comportamenti imprevisti delle query compilate e unione Opzioni, ad esempio `NoTracking`. Se si prevede di usare le query compilate o modificare le impostazioni di opzione di unione nell'applicazione, prima leggere.
- [Entità correlate Framework post nel blog del Team di consulenza clienti di modellazione e dati](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Include i post su query compilate e tramite il Profiler di Visual Studio 2010 per individuare i problemi di prestazioni.
- [Thread del forum Entity Framework con suggerimenti per migliorare le prestazioni delle query estremamente complesse](https://social.msdn.microsoft.com/Forums/en-US/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Suggerimenti per la gestione dello stato di ASP.NET](https://msdn.microsoft.com/en-us/library/z1hkazw7.aspx).
- [Utilizzo di Entity Framework e ObjectDataSource: il Paging personalizzato](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Post di blog che si basa sull'applicazione ContosoUniversity creati in queste esercitazioni viene illustrato come implementare il paging nel *Departments.aspx* pagina.

L'esercitazione successiva esamina alcuni importanti miglioramenti apportati a Entity Framework introdotte nella versione 4.

>[!div class="step-by-step"]
[Precedente](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
[Successivo](what-s-new-in-the-entity-framework-4.md)
