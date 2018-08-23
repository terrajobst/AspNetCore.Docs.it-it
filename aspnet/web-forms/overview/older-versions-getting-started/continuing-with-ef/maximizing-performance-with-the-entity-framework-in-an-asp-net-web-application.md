---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Massimizzazione delle prestazioni con Entity Framework 4.0 in un'applicazione Web 4 ASP.NET | Microsoft Docs
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione web di Contoso University specificano che viene creato da Getting Started with serie di esercitazioni in Entity Framework 4.0. POSSO...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 7d7c66289f09179a98e09532172477d5b06c70bd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837114"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Ottimizzazione delle prestazioni con Entity Framework 4.0 in un'applicazione Web 4 ASP.NET
====================
da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web Contoso University specificano che viene creato per il [Introduzione a Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni. Se si non è stato completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) verrebbe creato. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creato dalla serie di esercitazioni complete. Se si hanno domande sulle esercitazioni, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente, è stato illustrato come gestire i conflitti di concorrenza. Questa esercitazione illustra le opzioni per migliorare le prestazioni di un'applicazione web ASP.NET che usa Entity Framework. Si apprenderà diversi metodi per ottimizzare le prestazioni o per la diagnosi dei problemi di prestazioni.

Informazioni visualizzate nelle sezioni seguenti sono probabile che sia utile in una vasta gamma di scenari:

- Caricare in modo efficiente i dati correlati.
- Gestire lo stato di visualizzazione.

Informazioni visualizzate nelle sezioni seguenti potrebbero essere utile se si usano le singole query che presentano problemi di prestazioni:

- Usare il `NoTracking` opzione di merge.
- Precompilare le query LINQ.
- Esaminare i comandi di query inviati al database.

Le informazioni presentate nella sezione seguente sono potenzialmente utile per le applicazioni con i modelli di dati estremamente grandi:

- Pre-generare viste.

> [!NOTE]
> Le prestazioni dell'applicazione Web sono influenzata da molti fattori, tra cui operazioni quali la dimensione dei dati richiesta e risposta, la velocità delle query di database, quante richieste che il server è possibile accodare e rapidità con cui può soddisfare e anche l'efficienza di qualsiasi librerie di script client che si stia usando. Se le prestazioni sono critiche nell'applicazione, o se il test o esperienza mostra che le prestazioni dell'applicazione non sono soddisfacente, è necessario seguire il protocollo normale per ottimizzare le prestazioni. Effettuare una misurazione per determinare dove si verificano i colli di bottiglia delle prestazioni e quindi indirizzare le aree che hanno il maggiore impatto sulle prestazioni complessive dell'applicazione.
> 
> In questo argomento è incentrato principalmente sui modi in cui è potenzialmente utili per migliorare le prestazioni in modo specifico di Entity Framework in ASP.NET. I suggerimenti riportati di seguito sono utili se si determina che l'accesso ai dati sia presente tra i colli di bottiglia delle prestazioni nell'applicazione. Ad eccezione del fatto come indicato, i metodi illustrati in questo argomento non devono essere considerati &quot;procedure consigliate&quot; in generale, molti di essi sono appropriate solo in situazioni eccezionali o a tipi molto specifici indirizzi dei colli di bottiglia delle prestazioni.


Per avviare l'esercitazione, avviare Visual Studio e aprire l'applicazione web di Contoso University specificano che si stava lavorando nell'esercitazione precedente.

## <a name="efficiently-loading-related-data"></a>In modo efficiente il caricamento dei dati correlati

Esistono diversi modi che Entity Framework può caricare i dati correlati nelle proprietà di navigazione di un'entità:

- *Caricamento lazy*. Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati. La prima volta che si tenta di accedere a una proprietà di navigazione, tuttavia, i dati necessari per quest'ultima vengono recuperati automaticamente. Ciò comporta più query inviate al database, ovvero uno per l'entità stessa e uno ogni volta che i dati per l'entità correlati deve essere recuperato. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Caricamento eager*. Quando l'entità viene letta, i dati correlati corrispondenti vengono recuperati insieme ad essa. Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari. Specificare il caricamento eager con il `Include` metodo, come visto in precedenza in queste esercitazioni.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Caricamento esplicito* Ciò è simile a il caricamento lazy, ad eccezione del fatto che recuperare in modo esplicito i dati correlati nel codice. non avviene automaticamente quando si accede a una proprietà di navigazione. Caricare i dati correlati manualmente tramite il `Load` metodo della proprietà di navigazione per le raccolte o si usa il `Load` metodo della proprietà di riferimento per le proprietà che contengono un singolo oggetto. (Ad esempio, si chiama il `PersonReference.Load` per caricare il `Person` proprietà di navigazione di un `Department` entity.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Perché non immediatamente recuperano i valori delle proprietà, il caricamento lazy e il caricamento esplicito sono entrambi noto anche come *caricamento posticipato*.

Il caricamento lazy è il comportamento predefinito per un contesto dell'oggetto che è stato generato dalla finestra di progettazione. Se si apre la *SchoolModel.Designer.cs* file che definisce la classe del contesto di oggetto, sono disponibili tre metodi del costruttore e ognuno di essi include l'istruzione seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

In generale, se si conosce occorre dati correlati per ogni entità durante il caricamento eager, recuperato offre le migliori prestazioni, perché è in genere più efficiente delle query separate per ogni entità recuperata una singola query inviata al database. D'altra parte, se si desidera accedere alle proprietà di navigazione di un'entità solo raramente o solo per un piccolo set di entità, di caricamento lazy o il caricamento esplicito può essere più efficienti, perché il caricamento eager dati recupererebbe più dati è necessario.

In un'applicazione web, il caricamento lazy può essere di scarso valore relativamente comunque, poiché le azioni utente che interessano la necessità di dati correlati vengono eseguite in browser, che è disponibile alcuna connessione al contesto dell'oggetto che viene eseguito il rendering della pagina. D'altra parte, quando si databind un controllo, in genere sapere esattamente quali dati necessarie e pertanto è in genere migliore per scegliere il caricamento eager o il caricamento posticipato basato su ciò che è appropriato in ogni scenario.

Inoltre, un controllo con associazione a dati potrebbe usare un oggetto entità dopo aver eliminato il contesto dell'oggetto. In tal caso, un tentativo di caricamento lazy una proprietà di navigazione avrà esito negativo. Il messaggio di errore visualizzato è chiaro: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

Il `EntityDataSource` controllo Disabilita il caricamento lazy per impostazione predefinita. Per il `ObjectDataSource` controllo che si usi per l'esercitazione corrente (o se il contesto dell'oggetto è accedere dal codice della pagina), esistono diversi modi, è possibile rendere lazy caricamento disabilitato per impostazione predefinita. Quando si crea un'istanza di un contesto dell'oggetto, è possibile disabilitarlo. Ad esempio, è possibile aggiungere la riga seguente al metodo del costruttore del `SchoolRepository` classe:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Per l'applicazione Contoso University, si apporteranno il contesto dell'oggetto disabilitare automaticamente il caricamento lazy in modo che non è necessario impostare ogni volta che viene creata un'istanza di un contesto di questa proprietà.

Aprire il *SchoolModel* modello di dati, fare clic nell'area di progettazione e quindi nel riquadro Proprietà impostare la **il caricamento Lazy abilitata** proprietà `False`. Salvare e chiudere il modello di dati.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>La gestione dello stato di visualizzazione

Per fornire funzionalità di aggiornamento, una pagina web ASP.NET deve archiviare i valori originali delle proprietà di un'entità quando viene eseguito il rendering di una pagina. Durante l'elaborazione del controllo di postback ricreare lo stato originale dell'entità, chiamare l'entità `Attach` metodo prima di applicare le modifiche e chiamando il `SaveChanges` (metodo). Per impostazione predefinita, i controlli dati ASP.NET Web Forms usano lo stato di visualizzazione per archiviare i valori originali. Tuttavia, lo stato di visualizzazione può sulle prestazioni, perché è archiviata in campi nascosti che possono aumentare notevolmente le dimensioni della pagina che viene inviata da e verso il browser.

Tecniche per la gestione dello stato di visualizzazione o alternative, ad esempio lo stato della sessione non sono univoche per Entity Framework, in modo che questa esercitazione non passi in questo argomento in modo dettagliato. Per altre informazioni vedere i collegamenti alla fine dell'esercitazione.

Tuttavia, la versione 4 di ASP.NET fornisce un nuovo modo di lavorare con lo stato di visualizzazione che è necessario essere consapevoli di tutti gli sviluppatori ASP.NET di applicazioni Web Form: le `ViewStateMode` proprietà. Questa nuova proprietà può essere impostata a livello di pagina o controllo e consente di disabilitare lo stato di visualizzazione per impostazione predefinita per una pagina e abilitarlo solo per i controlli che ne hanno necessità.

Per le applicazioni in cui le prestazioni sono critiche, è consigliabile sempre disabilitare lo stato di visualizzazione a livello di pagina e abilitarlo solo per i controlli che lo richiedono. Le dimensioni dello stato di visualizzazione nelle pagine di Contoso University non sarebbe possibile diminuire notevolmente da questo metodo, ma per verificarne il funzionamento, è possibile usare per la *Instructors.aspx* pagina. Questa pagina contiene molti controlli, tra cui un `Label` controllo che ha disabilitato lo stato di visualizzazione. Nessuno dei controlli in questa pagina effettivamente necessario visualizzare lo stato abilitato. (Il `DataKeyNames` proprietà del `GridView` controllo specifica lo stato che deve essere mantenuto tra i vari postback, ma questi valori vengono mantenuti nello stato di controllo, che non è interessato dal `ViewStateMode` proprietà.)

Il `Page` direttiva e `Label` markup del controllo attualmente è simile al seguente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Apportare le modifiche seguenti:

- Aggiungere `ViewStateMode="Disabled"` per il `Page` direttiva.
- Rimuovere `ViewStateMode="Disabled"` dal `Label` controllo.

Il markup è ora simile al seguente:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Lo stato di visualizzazione è disabilitato per tutti i controlli. Se in un secondo momento si aggiunge un controllo che devono usare lo stato di visualizzazione, è sufficiente è includere il `ViewStateMode="Enabled"` attributo per il controllo.

## <a name="using-the-notracking-merge-option"></a>Utilizzo dell'opzione di unione NoTracking

Quando un contesto dell'oggetto vengono recuperate le righe di database e crea oggetti entità che le rappresentano, per impostazione predefinita tiene inoltre traccia gli oggetti entità utilizzando la gestione dello stato degli oggetti. Questi dati di rilevamento agisce come una cache e viene usati quando si aggiorna un'entità. Perché un'applicazione web ha in genere istanze di contesto di oggetto di breve durata, le query spesso restituiscono dati che non devono essere registrati, perché il contesto dell'oggetto in grado di leggerle verrà eliminato prima di usare una delle entità che legge nuovamente o aggiornano.

In Entity Framework, è possibile specificare se il contesto dell'oggetto tiene traccia degli oggetti entità impostando un *opzione di merge*. È possibile impostare l'opzione di merge per le singole query o per i set di entità. Se è possibile impostare un set di entità, che significa che si sta impostando l'opzione di merge predefinita per tutte le query che vengono creati per il set di entità.

Per l'applicazione Contoso University, rilevamento non è necessario per uno dei set di entità cui si accede da repository, pertanto è possibile impostare l'opzione di merge `NoTracking` per i set di entità quando si crea un'istanza di contesto dell'oggetto della classe di repository. (Si noti che in questa esercitazione, impostare l'opzione di merge non avere un impatto negativo sulle prestazioni dell'applicazione. Il `NoTracking` opzione è probabile che rendere un miglioramento delle prestazioni osservabile solo in determinati scenari con volumi di dati elevati.)

Nella cartella di DAL, aprire il *SchoolRepository.cs* file e aggiungere un metodo del costruttore che imposta l'opzione di merge per i set di entità che accede al repository:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Query LINQ in fase di pre-compilazione

La prima volta che Entity Framework esegue una query Entity SQL all'interno di tutta la durata di un determinato `ObjectContext` istanza, sono necessari alcuni minuti per compilare la query. Il risultato della compilazione viene memorizzato nella cache, il che significa che nelle successive esecuzioni della query sono molto più rapida. Le query LINQ seguono un modello simile, ad eccezione del fatto che parte del lavoro necessario per compilare la query viene eseguita ogni volta che viene eseguita la query. In altre parole, per le query LINQ, per impostazione predefinita non tutti i risultati della compilazione vengono memorizzati nella cache.

Se si dispone di una query LINQ che si prevede di eseguire più volte nella vita di un contesto dell'oggetto, è possibile scrivere codice che fa in modo che tutti i risultati della compilazione da memorizzare nella cache la prima volta che viene eseguita la query LINQ.

A scopo illustrativo, puoi farlo per due `Get` metodi nel `SchoolRepository` (classe), uno dei quali non accetta alcun parametro (il `GetInstructorNames` metodo) e uno che richiede un parametro (il `GetDepartmentsByAdministrator` (metodo)). Questi metodi restano immutati ora non è necessario essere compilato perché non sono le query LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Tuttavia, in modo che è possibile provare le query compilate, sarà procedere come se si fosse stata scritta come le query LINQ seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

È possibile modificare il codice in questi metodi per che cosa ha illustrato in precedenza ed eseguire l'applicazione per verificarne il funzionamento prima di continuare. Ma le istruzioni seguenti passare subito alla creazione di versioni pre-compilate di essi.

Creare un file di classe nel *DAL* cartella, denominarla *SchoolEntities.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Questo codice crea una classe parziale che estende la classe del contesto di oggetto generato automaticamente. La classe parziale include due query LINQ compilate usando le `Compile` metodo del `CompiledQuery` classe. Crea anche i metodi che è possibile usare per chiamare le query. Salvare e chiudere il file.

Successivamente, nella *SchoolRepository.cs*, modificare l'oggetto esistente `GetInstructorNames` e `GetDepartmentsByAdministrator` metodi nel repository di classe in modo che chiamano le query compilate:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Eseguire la *Departments.aspx* pagina per verificarne il funzionamento come in precedenza. Il `GetInstructorNames` metodo viene chiamato per popolare l'elenco a discesa amministratore e il `GetDepartmentsByAdministrator` viene chiamato quando si fa clic **Update** per verificare che nessun insegnante sia un amministratore di più reparto.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

È stato query precompilate nell'applicazione di Contoso University solo per informazioni su come eseguire questa operazione, non perché misurabile le prestazioni. Pre-compilazione di query LINQ di aggiungere un livello di complessità al codice, assicurarsi che farlo solo per le query che rappresentano effettivamente i colli di bottiglia nell'applicazione.

## <a name="examining-queries-sent-to-the-database"></a>Esaminare le query inviate al Database

Quando si esaminano i problemi di prestazioni, in alcuni casi è utile conoscere l'esatti comandi SQL che invia al database di Entity Framework. Se si lavora con un `IQueryable` dell'oggetto, un modo per eseguire questa operazione consiste nell'usare il `ToTraceString` (metodo).

Nelle *SchoolRepository.cs*, modificare il codice il `GetDepartmentsByName` metodo in base all'esempio seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

Il `departments` variabile necessario eseguire il cast a un `ObjectQuery` digitare solo perché il `Where` metodo alla fine della riga precedente crea un `IQueryable` ; dell'oggetto senza il `Where` metodo, il cast non sarebbe necessario.

Impostare un punto di interruzione per il `return` riga e quindi eseguire il *Departments.aspx* pagina nel debugger. Quando si raggiunge il punto di interruzione, esaminare i `commandText` di una variabile nel **variabili locali** finestra e utilizzare il Visualizzatore di testo (la lente di ingrandimento nel **valore** colonna) per visualizzarne il valore nel **Visualizzatore testo** finestra. È possibile visualizzare l'intero comando SQL risultante da questo codice:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

In alternativa, la funzionalità di IntelliTrace in Visual Studio Ultimate fornisce un modo per visualizzare comandi SQL generati da Entity Framework che non richiede di modificare il codice o persino impostare un punto di interruzione.

> [!NOTE]
> È possibile eseguire le procedure seguenti solo se si dispone di Visual Studio Ultimate.


Ripristinare il codice originale di `GetDepartmentsByName` (metodo) e quindi eseguire il *Departments.aspx* pagina nel debugger.

In Visual Studio, selezionare la **Debug** menu, quindi **IntelliTrace**e quindi **eventi IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Nel **IntelliTrace** finestra, fare clic su **Interrompi tutto**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Il **IntelliTrace** finestra viene visualizzato un elenco di eventi recenti:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Scegliere il **ADO.NET** riga. Espande per mostrare il testo del comando:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

È possibile copiare la stringa di testo intero comando negli Appunti dal **variabili locali** finestra.

Si supponga che stava lavorando con un database con altre tabelle, relazioni e di colonne maggiore il semplice `School` database. Si potrebbe rilevare che una query che consente di raccogliere tutte le informazioni è necessario in un unico `Select` istruzione che contiene più `Join` clausole diventa troppo complesso per lavorare in modo efficiente. In tal caso è possibile passare dal caricamento per il caricamento esplicito per semplificare la query eager.

Ad esempio, provare a modificare il codice nel `GetDepartmentsByName` nel metodo *SchoolRepository.cs*. Attualmente, in quanto metodo si dispone di una query di oggetto che dispone `Include` metodi per il `Person` e `Courses` le proprietà di navigazione. Sostituire il `return` istruzione con il codice che esegue il caricamento esplicito, come illustrato nell'esempio seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Eseguire la *Departments.aspx* pagina nel debugger e controllare le **IntelliTrace** finestra nuovamente come fatto in precedenza. A questo punto, in cui era presente una singola query prima, noterete una lunga sequenza di essi.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Fare clic sul primo **ADO.NET** riga per vedere cosa è successo a query complesse è visualizzato in precedenza.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

La query partendo da reparti è diventato un semplice `Select` eseguire una query senza `Join` clausola, ma è seguito da query separate che recuperano i corsi correlati e un amministratore, usando un set di due query per ogni reparto restituito da originale query.

> [!NOTE]
> Se si esce dalla modalità differita potrebbe causare l'abilitazione del caricamento, il modello riportato di seguito, con la stessa query ripetute più volte, il caricamento lazy. Un modello che si desidera evitare in genere è il caricamento lazy dei dati correlati per ogni riga della tabella primaria. A meno che non aver verificato che una query join singola è troppo complessa per essere efficace, in genere sarà in grado di migliorare le prestazioni in questi casi, modificare la query principale per usare il caricamento eager.


## <a name="pre-generating-views"></a>Pregenerazione di visualizzazioni

Quando un `ObjectContext` oggetto viene inizialmente creato in un nuovo dominio applicazione, Entity Framework genera un set di classi che lo usa per accedere al database. Queste classi vengono chiamate *viste*, e se si dispone di un modello di dati molto grandi, generazione di queste visualizzazioni possono ritardare la risposta del sito web per la prima richiesta per una pagina dopo l'inizializzazione di un nuovo dominio applicazione. È possibile ridurre questo ritardo prima richiesta mediante la creazione di visualizzazioni in fase di compilazione anziché in fase di esecuzione.

> [!NOTE]
> Se l'applicazione non dispone di un modello di dati molto grandi, o se dispone di un modello di dati di grandi dimensioni, ma non si è preoccupati per un problema di prestazioni che interessa solo la prima richiesta di pagina dopo IIS viene riciclato, è possibile ignorare questa sezione. Visualizzazione creazione non avviene ogni volta che crea un'istanza di un `ObjectContext` dell'oggetto, poiché le visualizzazioni vengono memorizzati nella cache nel dominio dell'applicazione. Pertanto, a meno che non si sta riciclo frequente l'applicazione in IIS, un numero molto ridotto di richieste di pagine possono trarre vantaggio dalle visualizzazioni pregenerate.


È possibile pre-generare viste tramite il *EdmGen.exe* dello strumento da riga di comando o tramite un *Toolkit di trasformazione di modelli di testo* modello (T4). In questa esercitazione si userà un modello T4.

Nel *DAL* cartella, aggiungere un file con il **modello di testo** modello (è sotto la **generale** nodo il **modelli installati** elenco), e denominarlo *SchoolModel.Views.tt*. Sostituire il codice esistente nel file con il codice seguente:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Questo codice genera visualizzazioni per un *edmx* file che si trova nella stessa cartella del modello e che ha lo stesso nome del file del modello. Ad esempio, se il file di modello è denominato *SchoolModel.Views.tt*, verrà cercato un file di modello di dati denominato *SchoolModel*.

Salvare il file, quindi fare clic sul file in **Esplora soluzioni** e selezionare **Esegui strumento personalizzato**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio genera un file di codice che crea le visualizzazioni, chiamata ScriptHelpers *SchoolModel.Views.cs* basato sul modello. (Si può notare che il file di codice viene generato anche prima di selezionare **Esegui strumento personalizzato**, non appena si salva il file di modello.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

È ora possibile eseguire l'applicazione e verificarne il funzionamento come in precedenza.

Per altre informazioni sulle visualizzazioni pregenerate, vedere le risorse seguenti:

- [Procedura: pre-generare viste per migliorare le prestazioni delle Query](https://msdn.microsoft.com/library/bb896240.aspx) nel sito web MSDN. Viene illustrato come utilizzare il `EdmGen.exe` strumento da riga di comando per pregenerare le visualizzazioni.
- [Isolamento delle prestazioni con viste a precompilata/Pre-generate in anticipo in Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) sul blog di Windows Server AppFabric Customer Advisory Team.

In questo argomento completa l'introduzione di miglioramento delle prestazioni in un'applicazione web ASP.NET che usa Entity Framework. Per altre informazioni, vedere le seguenti risorse:

- [Considerazioni sulle prestazioni (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) nel sito web MSDN.
- [Post relativi alle prestazioni nel blog del Team di Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Entity Framework opzioni di unione e query compilate](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Post di blog che spiega i comportamenti imprevisti delle query compilate e unione, ad esempio opzioni `NoTracking`. Se si prevede di usare le query compilate o manipolare le impostazioni dell'opzione di unione nell'applicazione, leggere questo passaggio.
- [Entity Framework correlati post nel blog del Team di consulenza clienti di modellazione e dati](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Include gli invii in query compilate e usando il Profiler di 2010 Visual Studio per individuare problemi di prestazioni.
- [Thread del forum Entity Framework con suggerimenti sul miglioramento delle prestazioni delle query estremamente complesse](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Indicazioni per la gestione dello stato di ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Utilizzo di Entity Framework e ObjectDataSource: Paging personalizzato](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Post di blog che si basa sull'applicazione ContosoUniversity creato in queste esercitazioni illustrano come implementare il paging nel *Departments.aspx* pagina.

L'esercitazione successiva illustra alcuni importanti miglioramenti apportati a Entity Framework che sono una novità della versione 4.

> [!div class="step-by-step"]
> [Precedente](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Successivo](what-s-new-in-the-entity-framework-4.md)
