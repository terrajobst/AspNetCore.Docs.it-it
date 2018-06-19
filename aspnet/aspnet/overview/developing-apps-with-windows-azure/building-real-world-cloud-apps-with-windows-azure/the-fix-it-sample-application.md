---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Appendice: per risolvere il problema applicazione di esempio (creazione di applicazioni Cloud del mondo reale con Azure) | Documenti Microsoft'
author: MikeWasson
description: Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 9a1fa36b34c4783b101bb27bc6931241e9251e10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876475"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Appendice: per risolvere il problema applicazione di esempio (creazione di applicazioni Cloud del mondo reale con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Scaricare la correzione progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


Questa appendice per le app Cloud mondo reale compilazione con Azure e-book contiene le sezioni seguenti forniscono informazioni aggiuntive sul Correggi applicazione di esempio che è possibile scaricare:

- [Problemi noti](#knownissues)
- [Procedure consigliate](#bestpractices)
- [Come eseguire l'app da Visual Studio nel computer locale](#run-in-vs)
- [Come distribuire l'app di base per le app Web di servizio App di Azure utilizzando gli script di Windows PowerShell](#deploybase)
- [Risoluzione dei problemi di script di Windows PowerShell](#troubleshooting)
- [Come distribuire l'app con la coda di elaborazione per App Web di servizio App di Azure e un servizio Cloud di Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemi noti

Correggi app è stato sviluppato per illustrare il modo più semplice possibile alcuni dei modelli presentati in questa e-book. Tuttavia, poiché l'e-book sulla compilazione di applicazioni reali, il codice Correggi è sottoposto a una revisione e il processo di test simile a ciò che facciamo per software rilasciati. È stato rilevato un numero di problemi e, come per qualsiasi applicazione reale, alcuni di essi è fisso e alcuni di essi è posticipata a una versione successiva.

Nell'elenco seguente include i problemi che devono essere risolti in un'applicazione di produzione, ma per uno dei motivi o un'altra che si è deciso di indirizzo nella versione iniziale dell'applicazione di esempio Correggi non.

### <a name="security"></a>Sicurezza

- Verificare che non è possibile assegnare un'attività a un proprietario inesistente.
- Verificare che è possibile solo visualizzare e modificare attività creati o assegnati all'utente.
- Utilizzo di HTTPS per pagine di accesso e i cookie di autenticazione.
- Specificare un limite di tempo per i cookie di autenticazione.

### <a name="input-validation"></a>Convalida dell'input

In generale, si farebbe un'app di produzione più input convalida rispetto all'app correggere. Ad esempio, le dimensioni dell'immagine immagine di dimensioni di file consentite per il caricamento deve essere limitato.

### <a name="administrator-functionality"></a>Funzionalità di amministratore

Un amministratore deve essere in grado di modificare la proprietà delle attività esistenti. Ad esempio, l'autore di un'attività potrebbe lasciano la società, lasciando nessun utente con autorizzazioni sufficienti per gestire l'attività, a meno che non è abilitato l'accesso amministrativo.

### <a name="queue-message-processing"></a>Elaborazione messaggio della coda

Messaggio della coda di elaborazione nell'app Correggi è stato progettato per essere semplice per illustrare il modello basate su coda di lavoro con una quantità minima di codice. Questo codice semplice non è sufficiente per un'applicazione di produzione effettivo.

- Il codice non garantisce che ogni messaggio della coda verrà elaborato al massimo una volta. Quando viene visualizzato un messaggio dalla coda, si verifica un periodo di timeout durante il quale il messaggio è visibile in altri listener di coda. Se il timeout scade prima che il messaggio viene eliminato, il messaggio diventa nuovamente visibile. Pertanto, se un'istanza del ruolo worker impiega molto tempo l'elaborazione di un messaggio, è teoricamente possibile per lo stesso messaggio di elaborare due volte, risultante in un'attività duplicata nel database. Per ulteriori informazioni, vedere [mediante le code di archiviazione Azure](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La logica di polling della coda potrebbe essere più conveniente, suddividendo in batch il recupero dei messaggi. Ogni volta che si chiama [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), ha un costo delle transazioni. In alternativa, è possibile chiamare [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (si noti il plurale '), che ottiene più messaggi in una singola transazione. I costi di transazione per le code di archiviazione di Azure sono molto bassi, pertanto l'impatto sui costi non è significativo nella maggior parte degli scenari.
- Una stretta ciclo nel codice di elaborazione dei messaggi della coda causa l'affinità di CPU, che non vengano utilizzate in modo efficiente le macchine virtuali multicore. Per eseguire diverse attività asincrone in parallelo una progettazione migliore utilizzerà parallelismo delle attività.
- Elaborazione dei messaggi della coda è la gestione delle eccezioni solo rudimentali. Ad esempio, il codice di non gestire [messaggi non elaborabili](https://msdn.microsoft.com/library/ms789028.aspx). (Quando l'elaborazione del messaggio provoca un'eccezione, è necessario registrare l'errore ed eliminare il messaggio, o il ruolo di lavoro tenterà di nuovo l'elaborazione e il ciclo continua per un periodo illimitato.)

### <a name="sql-queries-are-unbounded"></a>Query SQL non sono associate

Correggere il codice corrente verrà posizionato alcun limite le query per le pagine di indice potrebbero restituire il numero di righe. Se il database viene inserito un volume elevato di attività, le dimensioni degli elenchi risultante ricevuto potrebbe causare problemi di prestazioni. La soluzione consiste nell'implementare il paging. Per un esempio, vedere [ordinamento, filtro e il Paging con Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Visualizzazione di modelli consigliati

L'app Correggi utilizza la classe di entità FixItTask per passare informazioni tra il controller e la visualizzazione. Una procedura consigliata consiste nell'utilizzare i modelli di visualizzazione. Il modello di dominio (ad esempio, la classe di entità FixItTask) si basa su ciò che è necessaria per la persistenza dei dati, mentre un modello di visualizzazione può essere progettato per la presentazione dei dati. Per ulteriori informazioni, vedere [12 ASP.NET MVC consigliate](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Blob dell'immagine protetto consigliato

Gli archivi di app Correggi caricare immagini come public, vale a dire che tutti gli utenti che consente di trovare l'URL può accedere le immagini. Impossibile proteggere le immagini anziché pubblico.

### <a name="no-powershell-automation-scripts-for-queues"></a>Script di automazione non PowerShell per le code

Gli script di automazione di PowerShell di esempio sono stati scritti solo per la versione di base di Correggi eseguito interamente in App Web di servizio App di Azure. Gli script è non fornito per la configurazione e distribuzione dell'app web più di un ambiente del servizio Cloud necessari per l'elaborazione di coda.

### <a name="special-handling-for-html-codes-in-user-input"></a>Gestione speciale per i codici HTML nell'input utente

ASP.NET impedisce automaticamente molti modi in cui gli utenti malintenzionati potrebbero tentare attacchi cross-site scripting immettendo script nelle caselle di testo di input utente. E di MVC `DisplayFor` helper utilizzata per visualizzare l'attività titoli e rileva automaticamente i valori di codifica in HTML che viene inviato al browser. Ma in un'app di produzione è consigliabile adottare misure aggiuntive. Per ulteriori informazioni, vedere [richiesta di convalida in ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Procedure consigliate

Di seguito sono alcuni problemi che sono stati risolti dopo essere stato individuato nella revisione del codice e il test della versione originale dell'app Correggi. Alcune sono causate dal codificatore originale non è necessario tenere traccia di una particolare procedura consigliata, alcune semplicemente perché il codice è stato scritto rapidamente e non è stato progettato per il software rilasciato. Ci stiamo elenco qui i problemi nel caso in cui si è verificato un acquisito da questa revisione e test che potrebbero essere utili ad altri utenti che sono anche lo sviluppo di applicazioni web.

### <a name="dispose-the-database-repository"></a>Eliminare il repository di database

Il `FixItTaskRepository` classe deve eliminare Entity Framework `DbContext` istanza. Abbiamo implementando `IDisposable` nel `FixItTaskRepository` classe:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Si noti che AutoFac eliminerà automaticamente il `FixItTaskRepository` dell'istanza, in modo che non è necessario eliminarlo in modo esplicito.

Un'altra opzione consiste nel rimuovere il `DbContext` variabile membro da `FixItTaskRepository`, invece di creare una variabile locale `DbContext` variabile all'interno di ogni metodo di repository, all'interno di un `using` istruzione. Ad esempio:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrarsi singleton di conseguenza DI

Poiché solo un'istanza del `PhotoService` classe e `Logger` classe è necessaria, queste classi devono essere [registrato come singole istanze per l'inserimento di dipendenze](https://code.google.com/p/autofac/wiki/InstanceScope) in *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Sicurezza: Non visualizzare i dettagli dell'errore per gli utenti

L'app originale Correggi non dispone di una pagina di errore generico e solo consentire tutte le bolle eccezioni fino all'interfaccia utente, pertanto potrebbe causare alcune eccezioni, ad esempio errori di connessione del database in una traccia di stack completo viene visualizzata nel browser. Informazioni dettagliate sull'errore talvolta possono favorire attacchi da utenti malintenzionati. La soluzione consiste nel registrare i dettagli dell'eccezione e visualizzare una pagina di errore all'utente che non include i dettagli dell'errore. Correggi app è stata già registrazione e per visualizzare una pagina di errore, sono aggiunte `<customErrors mode=On>` nel file Web. config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Per impostazione predefinita in questo modo *Views\Shared\Error.cshtml* da visualizzare per gli errori. È possibile personalizzare *Error.cshtml* o creare la propria visualizzazione pagina di errore e aggiungere un `defaultRedirect` attributo. È anche possibile specificare le pagine di errore diversi per errori specifici.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Protezione: Consenti solo un'attività essere modificato dall'autore

La pagina di indice del Dashboard vengono mostrate solo le attività create dall'utente connesso, ma un utente malintenzionato potrebbe creare un URL con un ID attività dell'utente. Aggiunta del codice in *DashboardController.cs* per restituire un errore 404 in tal caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Non accettare le eccezioni

L'app originale Correggi appena ha restituito null dopo la registrazione di un'eccezione derivante da una query SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Questo potrebbe renderlo anteprima per l'utente come se la query è stata completata, ma semplicemente non restituisce alcuna riga. È necessario rigenerare l'eccezione dopo il rilevamento e la registrazione:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Intercettare tutte le eccezioni nei ruoli di lavoro

Eventuali eccezioni non gestite in un ruolo di lavoro causerà la macchina virtuale di essere riciclati, pertanto si desidera eseguire il wrapping di tutti gli elementi non in un blocco try-catch e gestire tutte le eccezioni.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Specificare la lunghezza per le proprietà della stringa nelle classi di entità

Per visualizzare il codice semplice, la versione originale dell'app Correggi non sono state specificate le lunghezze dei campi dell'entità FixItTask e pertanto sono state definite come varchar (max) nel database. Di conseguenza, l'interfaccia utente accetterebbero quasi tutte le quantità di input. Specifica i limiti di set di lunghezze applicati entrambi all'utente di input nella pagina web e dimensioni di colonna nel database:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Contrassegnare i membri privati come di sola lettura quando essi non sono previste modifiche

Ad esempio, nel `DashboardController` un'istanza della classe `FixItTaskRepository` viene creato e non è prevista la modifica, pertanto è definita come [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Utilizzare l'elenco. Any () invece di elenco. Count () &gt; 0

Se si è rilevante se uno o più elementi in un elenco di adattare i criteri specificati, utilizzare il [qualsiasi](https://msdn.microsoft.com/library/bb534972.aspx) (metodo), poiché viene restituito non appena viene trovato un elemento che corrispondono ai criteri, mentre il `Count` metodo deve sempre eseguire l'iterazione tramite ogni elemento. Il Dashboard *cshtml* file di origine include il codice seguente:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

È stato modificato, a questo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generare gli URL nelle visualizzazioni MVC utilizzo di helper MVC

Per il **crearla una correzione** pulsante nella home page, Correggi app rigido codificato di un elemento ancoraggio:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Per i collegamenti di visualizzazione/azione simile al seguente si consiglia di utilizzare il [Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) helper HTML, ad esempio:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Utilizzare Task.Delay anziché Sleep nel ruolo di lavoro

Inserisce il nuovo progetto modello `Thread.Sleep` nell'esempio di codice per un ruolo di lavoro, ma a causa del thread può causare il pool di thread per generare il thread non necessari aggiuntivi. È possibile evitare che utilizzando [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) invece.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evitare async void

Se un metodo asincrono non deve necessariamente restituire un valore, restituire un `Task` tipo anziché `void`.

Questo esempio è tratto la `FixItQueueManager` classe: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

È consigliabile utilizzare `async void` solo per i gestori di eventi di primo livello. Se si definisce un metodo come `async void`, il chiamante non è possibile **await** il metodo o di intercettare le eccezioni, il metodo genera un'eccezione. Per ulteriori informazioni, vedere [procedure consigliate nella programmazione asincrona](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Utilizzare un token di annullamento per interrompere il ciclo di ruolo worker

In genere, il **eseguire** metodo su un ruolo di lavoro contiene un ciclo infinito. Quando il ruolo di lavoro viene arrestato, la [Roleentrypoint](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) metodo viene chiamato. Utilizzare questo metodo per annullare il lavoro che viene eseguito all'interno di **eseguire** (metodo) e uscita normalmente. In caso contrario, il processo potrebbe essere terminato all'interno di un'operazione.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Rifiutare esplicitamente procedura analisi MIME automatica

In alcuni casi, Internet Explorer restituisce un tipo MIME diverso rispetto al tipo specificato dal server web. Ad esempio, se Internet Explorer rileva contenuto HTML in un file recapitato con l'intestazione della risposta HTTP Content-Type: text/plain, Internet Explorer determina che il contenuto deve essere eseguito il rendering in formato HTML. Sfortunatamente, questo "analisi MIME" possono inoltre comportare problemi di sicurezza per i server che ospitano contenuto non attendibile. Per risolvere questo problema, Internet Explorer 8, ha un numero di modifiche al codice di determinazione dei tipi MIME e consente agli sviluppatori di applicazioni [rifiutare esplicitamente l'analisi MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). È stato aggiunto il seguente codice per il *Web. config* file.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Abilitare l'aggregazione e riduzione

Quando Visual Studio crea un nuovo progetto web, bundling and minification dei file JavaScript non è abilitato per impostazione predefinita. Una riga di codice è stato aggiunto in BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Impostare un timeout di scadenza per i cookie di autenticazione

Per impostazione predefinita, i cookie di autenticazione scadono entro due settimane. Un tempo più breve è più sicuro. È possibile modificare questa impostazione in *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Come eseguire l'app da Visual Studio nel computer locale

Esistono due modi per eseguire l'app Fix:

- Eseguire l'applicazione di base che scrive le nuove attività direttamente al database SQL.
- Eseguire l'applicazione utilizzando una coda e un servizio back-end per creare le attività. Il modello di coda è descritto nel capitolo [basate su coda di lavoro modello](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Eseguire l'applicazione di base

1. Installare [Visual Studio 2013 o Visual Studio 2013 Express per Web](https://www.visualstudio.com/downloads).
2. Installare il [Azure SDK per .NET per Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Scaricare il file con estensione zip di [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. In Esplora File, fare clic con il file zip e fare clic su proprietà e nella finestra Proprietà fare clic su Annulla blocco.
5. Decomprimere il file.
6. Fare doppio clic sul file con estensione sln per avviare Visual Studio.
7. Dal menu Strumenti, fare clic su Gestione pacchetti libreria, quindi Console Gestione pacchetti.
8. In Package Manager Console (PMC), fare clic su Ripristina.
9. Uscire da Visual Studio.
10. Avviare il [emulatore di archiviazione Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Riavviare Visual Studio, aprire il file di soluzione che è stata chiusa nel passaggio precedente.
12. Verificare che il progetto FixIt è impostato come progetto di avvio e quindi premere CTRL + F5 per eseguire il progetto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Eseguire l'applicazione con l'elaborazione di coda

1. Seguire le istruzioni per [eseguire l'applicazione di base](#runbase), quindi chiudere il browser e si chiude Visual Studio.
2. Avviare Visual Studio con privilegi di amministratore. (Verrà utilizzata l'emulatore di calcolo di Azure, e che richiede privilegi di amministratore).
3. Nell'applicazione *Web. config* file nel *MyFixIt* progetto (il progetto web), modificare il valore di `appSettings/UseQueues` su "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Se il [emulatore di archiviazione Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) non è ancora in esecuzione, avviarlo di nuovo.
5. Eseguire il progetto web FixIt e il progetto MyFixItCloudService simultaneamente.

    Con Visual Studio 2013:

   1. Premere F5 per eseguire il progetto FixIt.
   2. In **Esplora**, fare clic sul progetto MyFixItCloudService e quindi fare clic su **Debug** -- **Avvia nuova istanza**.

      Utilizzo di Visual Studio 2013 Express per Web:

   3. In Esplora soluzioni, la soluzione FixIt e scegliere **proprietà**.
   4. Selezionare **più progetti di avvio**...
   5. Nel **azione** elenco a discesa in MyFixIt e MyFixItCloudService, selezionare **avviare**.
   6. Fare clic su **OK**.
   7. Premere F5 per eseguire entrambi i progetti.

      Quando si esegue il progetto MyFixItCloudService, Visual Studio avvia l'emulatore di calcolo di Azure. A seconda della configurazione del firewall, potrebbe essere necessario consentire l'emulatore attraverso il firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Come distribuire l'app di base per le app Web di servizio App di Azure utilizzando gli script di Windows PowerShell

Per illustrare il [automatizzare tutti gli elementi](automate-everything.md) modello, l'app Correggi viene fornito con script per impostare un ambiente in Azure e distribuire il progetto nel nuovo ambiente. Le istruzioni seguenti illustrano come utilizzare gli script.

Se si desidera eseguire in Azure senza l'utilizzo delle code e delle modifiche, per eseguire in locale con le code, assicurarsi di che impostare il valore di appSetting UseQueues reimpostata su false prima di procedere con le istruzioni seguenti.

Queste istruzioni presuppongono che è già stato scaricato ed eseguire la soluzione Correggi in locale, e di disporre di Azure l'account o dispone di una sottoscrizione di Azure che si è autorizzati a gestire.

1. Installare il **Azure PowerShell** console. Per istruzioni, vedere [come installare e configurare Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Questa console personalizzata è configurata per funzionare con la sottoscrizione di Azure. Cui è installato il modulo di Azure di *programmi* directory e viene importato automaticamente in ogni uso della console di PowerShell di Azure.

    Se si preferisce utilizzare un programma host diverso, ad esempio, Windows PowerShell ISE, assicurarsi di utilizzare il [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet per importare il modulo di Azure oppure utilizzare un comando nel modulo Azure per attivare l'importazione automatica del modulo.
2. Avviare PowerShell di Azure con il **Esegui come amministratore** opzione.
3. Eseguire il [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet per impostare i criteri di esecuzione di Azure PowerShell `RemoteSigned`. Immettere **Y** (per Sì) per completare la modifica dei criteri.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Questa impostazione consente di eseguire gli script locali che non sono firmati digitalmente. (È anche possibile impostare i criteri di esecuzione `Unrestricted`, che comporta la necessità per il passaggio di sblocco in un secondo momento, ma questa operazione è sconsigliata per motivi di sicurezza.)
4. Eseguire il `Add-AzureAccount` cmdlet per impostare PowerShell con le credenziali per l'account.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Queste credenziali scadono dopo un periodo di tempo ed è necessario eseguire di nuovo il `Add-AzureAccount` cmdlet. Come viene scritto l'e-book, il limite di tempo prima della scadono delle credenziali è 12 ore.
5. Se si dispone di più sottoscrizioni, usare il cmdlet Select-AzureSubscription per specificare la sottoscrizione a cui in che si desidera creare l'ambiente di test.
6. Importare un certificato di gestione per la stessa sottoscrizione di Azure tramite il `Get-AzurePublishSettingsFile` e `Import-AzurePublishSettingsFile` cmdlet. Il primo di questi cmdlet scarica un file di certificato e nella seconda è specificare il percorso del file per l'importazione. > [!IMPORTANT]
   > Mantenere il file scaricato in un percorso sicuro o eliminarlo al termine, perché contiene un certificato che può essere usato per gestire i servizi di Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Il certificato viene utilizzato per una chiamata API REST che rileva l'indirizzo IP del computer di sviluppo per impostare una regola del firewall nel server di Database SQL.
7. Eseguire il [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (alias sono `cd`, `chdir`, e `sl`) per passare alla directory che contiene gli script. (E si trovano nel *automazione* della cartella di soluzione Correggi.) Se uno dei nomi di directory contiene spazi, racchiudere tra virgolette il percorso. Ad esempio, per individuare il `c:\Sample Apps\FixIt\Automation` directory è possibile immettere il comando seguente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Per consentire a Windows PowerShell eseguire questi script, utilizzare il [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet. (Gli script sono bloccati perché sono stati scaricati da Internet).

    > [!WARNING]
    > Sicurezza - prima di eseguire `Unblock-File` su qualsiasi script o file eseguibile, aprire il file in blocco note, esaminare i comandi e verificare che non contengano codice dannoso.

    Ad esempio, il comando seguente esegue il `Unblock-File` cmdlet su tutti gli script nella directory corrente.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Per creare l'app web per la base (Nessuna coda di elaborazione) correggerlo app, eseguire lo script di creazione ambiente.

    Obbligatorio `Name` parametro specifica il nome del database e viene usato anche per l'account di archiviazione che consente di creare lo script. Il nome deve essere globalmente univoco all'interno del dominio azurewebsites.net. Se si specifica un nome che non è univoco, ad esempio Fixit o Test (o anche come nell'esempio, fixitdemo), il `New-AzureWebsite` cmdlet ha esito negativo con un errore interno che viene segnalato un conflitto. Lo script converte il nome in tutte le lettere minuscole per rispettare i requisiti dei nomi per le applicazioni web, gli account di archiviazione e database.

    Obbligatorio `SqlDatabasePassword` parametro specifica la password per l'account amministratore che verrà creato per il Database SQL. Non includere i caratteri XML speciali nella password (&amp; &lt; &gt; ;). Si tratta di una limitazione del modo in cui gli script sono stati scritti, non una limitazione di Azure.

    Ad esempio, se si desidera creare un'app web denominata "fixitdemo" e utilizzare una password di amministratore di SQL Server di "Passw0rd1", è possibile immettere il comando seguente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Il nome deve essere univoco nel dominio azurewebsites.net e la password deve soddisfare i requisiti del Database SQL per la complessità delle password. (L'esempio Passw0rd1 soddisfa i requisiti).

    Si noti che il comando inizia con ". \". Per evitare che malintenzionati esecuzione di script, Windows PowerShell richiede di specificare il percorso completo del file di script quando si esegue uno script. È possibile utilizzare un punto per indicare la directory corrente (".\") In alternativa, specificare il percorso completo, ad esempio:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Per ulteriori informazioni sullo script, utilizzare il `Get-Help` cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    È possibile utilizzare il `Detailed`, `Full`, `Parameters`, e `Examples` parametri del cmdlet Get-Help per filtrare la Guida che viene restituita.

    Se lo script genera errori o errori, ad esempio "New-AzureWebsite: chiamare Set-AzureSubscription e Select-AzureSubscription first," si potrebbe non avere completato la configurazione di Azure PowerShell.

    Al termine dell'esecuzione dello script, è possibile utilizzare il portale di gestione di Azure per visualizzare le risorse che sono state create, come illustrato nella [automatizzare tutti gli elementi](automate-everything.md) capitolo.
10. Per distribuire il progetto FixIt nel nuovo ambiente di Azure, utilizzare il *AzureWebsite.ps1* script. Ad esempio:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Quando viene eseguita la distribuzione, il browser viene visualizzata con Correggi in esecuzione in Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Risoluzione dei problemi di script di Windows PowerShell

Gli errori più comuni durante l'esecuzione di questi script sono correlati alle autorizzazioni. Assicurarsi che `Add-AzureAccount` e `Import-AzurePublishSettingsFile` hanno avuto esito positivo e utilizzato per la sottoscrizione di Azure stesso. Anche se `Add-AzureAccount` è stata corretta potrebbe essere necessario eseguirlo nuovamente. Le autorizzazioni aggiunte da `Add-AzureAccount` scadono entro 12 ore.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Riferimento a un oggetto non impostato su un'istanza di oggetto.

Se lo script restituisce errori, ad esempio "Riferimento all'oggetto non è impostata su un'istanza di un oggetto," che significa che Windows PowerShell non è possibile trovare un oggetto processo (si tratta di un'eccezione di riferimento null), eseguire il `Add-AzureAccount` cmdlet e riprovare a eseguire lo script.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: Il server ha rilevato un errore interno.

Il `New-AzureWebsite` cmdlet restituisce un errore interno quando il nome non è univoco nel dominio azurewebsites.net. Per risolvere l'errore, utilizzare un valore diverso per il nome, ovvero nel parametro Name di *New AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Riavviare lo script

Se è necessario riavviare il *New AzureWebsiteEnv.ps1* script perché non è riuscito prima che il messaggio "Script è stato completato", si desidera eliminare le risorse che lo script creato prima dell'arresto. Ad esempio, se lo script è già stato creato l'app web ContosoFixItDemo e si esegue nuovamente lo script con lo stesso nome, lo script avrà esito negativo perché il nome è in uso.

Per determinare quali risorse lo script creato prima dell'arresto, utilizzare i cmdlet seguenti:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Per eseguire questo cmdlet, inviare tramite pipe il nome del server di database `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Per eliminare queste risorse, utilizzare i comandi seguenti. Si noti che se si elimina il server di database, si elimina automaticamente i database associati al server.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Come distribuire l'app con la coda di elaborazione per App Web di servizio App di Azure e un servizio Cloud di Azure

Per abilitare le code, apportare la modifica seguente nel file MyFixIt\Web.config. In `appSettings`, modificare il valore di `UseQueues` su "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Distribuire quindi l'applicazione MVC a un'app web in Azure App Service, come descritto [precedenti](#deploybase).

Successivamente, creare un nuovo servizio cloud di Azure. Gli script inclusi con l'app Correggi non creare o distribuire il servizio cloud, è necessario utilizzare il portale di Azure per questo oggetto. Nel portale, fare clic su **New** -- **calcolo** – **servizio Cloud** -- **creazione rapida**e quindi immettere un URL e una posizione del data center. Utilizzare lo stesso data center in cui è distribuita l'app web.

![](the-fix-it-sample-application/_static/image1.png)

Prima di poter distribuire il servizio cloud, è necessario aggiornare alcuni dei file di configurazione.

In MyFixIt.WorkerRoler\app.config, in `connectionStrings`, sostituire il valore del `appdb` stringa di connessione con la stringa di connessione effettiva per il Database SQL. È possibile ottenere la stringa di connessione dal portale. Nel portale, fare clic su **database SQL** - **appdb** - **stringhe di connessione di Database SQL di visualizzazione per ADO .net, ODBC, PHP e JDBC**. La stringa di connessione ADO.NET di copiare e incollare il valore nel file app. config. Sostituire "{il\_password\_qui}" con la password del database. (Presupponendo che vengano utilizzati gli script di distribuzione dell'app MVC, è specificata la password del database nel `SqlDatabasePassword` parametro di script.)

Il risultato dovrebbe essere simile al seguente:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Nello stesso file, MyFixIt.WorkerRoler\app.config in `appSettings`, sostituire i due valori di segnaposto per l'account di archiviazione di Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

È possibile ottenere la chiave di accesso dal portale. Vedere [come gestire gli account di archiviazione](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

In MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, sostituire i segnaposto con valori di due stesso per l'account di archiviazione di Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

A questo punto si è pronti per distribuire il servizio cloud. In Esplora soluzioni, fare clic sul progetto MyFixItCloudService e selezionare **pubblica**. Per ulteriori informazioni, vedere "[distribuire l'applicazione in Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", che è in parte 2 di [questa esercitazione](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Precedente](more-patterns-and-guidance.md)
