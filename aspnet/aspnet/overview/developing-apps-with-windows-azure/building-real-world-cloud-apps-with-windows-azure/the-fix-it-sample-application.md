---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Appendice: La correzione applicazione di esempio (compilazione di App per Cloud funzionanti con Azure) | Microsoft Docs'
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: baf46a87155e6368d9a81c5c5b777a491117d7b6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835209"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Appendice: La correzione applicazione di esempio (compilazione di App per Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Scaricare la correzione del progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


In questa appendice per la creazione Real World di App Cloud con Azure e-book contiene le sezioni seguenti che forniscono informazioni aggiuntive sulle Fix It applicazione di esempio che è possibile scaricare:

- [Problemi noti](#knownissues)
- [Procedure consigliate](#bestpractices)
- [Come eseguire l'app da Visual Studio nel computer locale](#run-in-vs)
- [Come distribuire l'app di base di App Web di servizio App di Azure usando gli script di Windows PowerShell](#deploybase)
- [Risoluzione dei problemi di script di Windows PowerShell](#troubleshooting)
- [Come distribuire l'app con la coda di elaborazione per App Web di servizio App di Azure e un servizio Cloud di Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemi noti

L'app Fix It è stata originariamente sviluppata per illustrare il modo più semplice possibile alcuni modelli presentati in questo e-book. Tuttavia, poiché l'e-book sulla compilazione di App reali, il codice Fix It è sottoposta a una revisione e il processo di test simile a quella eseguita per il software. È stato rilevato un numero di problemi e come con qualsiasi applicazione reale, alcune di esse è stato risolto e alcune di esse è posticipata a una versione successiva.

Nell'elenco seguente include i problemi che devono essere svolte in un'applicazione di produzione, ma per un motivo o un altro che abbiamo deciso di non indirizzo nella versione iniziale dell'applicazione di esempio Fix It.

### <a name="security"></a>Sicurezza

- Assicurarsi che non è possibile assegnare un'attività a un proprietario inesistente.
- Assicurarsi che è possibile solo visualizzare e modificare attività creati o assegnati all'utente.
- Uso di HTTPS per le pagine di accesso e i cookie di autenticazione.
- Specificare un limite di tempo per i cookie di autenticazione.

### <a name="input-validation"></a>Convalida dell'input

In generale, eseguire un'app di produzione più convalida dell'input da quella dell'app Fix It. Ad esempio, le dimensioni dell'immagine / image dimensioni di file consentite per il caricamento deve essere limitato.

### <a name="administrator-functionality"></a>Funzionalità amministratore

Un amministratore deve essere in grado di modificare la proprietà delle attività esistenti. Ad esempio, il creatore di un'attività potrebbe lascia l'azienda, lasciando nessuno con l'autorità per mantenere l'attività a meno che non è abilitato l'accesso amministrativo.

### <a name="queue-message-processing"></a>Elaborazione messaggio della coda

Messaggio della coda di elaborazione nell'app Fix It è stato progettato per essere semplice per illustrare il modello di lavoro incentrato sulle code con una quantità minima di codice. Questo semplice codice non sia sufficiente per un'applicazione di produzione reale.

- Il codice non garantisce che ogni messaggio della coda verrà elaborato più di una volta. Quando si riceve un messaggio dalla coda, è previsto un periodo di timeout, durante il quale il messaggio rimane invisibile in altri listener della coda. Se il timeout scade prima che il messaggio viene eliminato, il messaggio diventa nuovamente visibile. Pertanto, se un'istanza del ruolo worker trascorre molto tempo l'elaborazione di un messaggio, è teoricamente possibile per lo stesso messaggio a venire elaborati due volte, risultante in un'attività duplicata nel database. Per altre informazioni su questo problema, vedere [usando le code di archiviazione di Azure](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La logica di polling della coda può essere più conveniente, suddividendo in batch il recupero di messaggi. Ogni volta che si chiama [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), è previsto un costo delle transazioni. In alternativa, è possibile chiamare [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (si noti il plurale '), che ottiene più messaggi in una singola transazione. Il costo delle transazioni per le code di archiviazione di Azure è molto basso, pertanto l'impatto sui costi non è significativo nella maggior parte degli scenari.
- Il ciclo ridotto nel codice di elaborazione dei messaggi di coda provoca affinità di CPU, che non usa in modo efficiente le macchine virtuali multicore. Per eseguire diverse attività asincrone in parallelo, una progettazione migliore utilizzerà parallelismo delle attività.
- Elaborazione dei messaggi della coda è la gestione delle eccezioni solo rudimentali. Ad esempio, il codice non gestisce [messaggi non elaborabili](https://msdn.microsoft.com/library/ms789028.aspx). (Quando l'elaborazione del messaggio provoca un'eccezione, è necessario registrare l'errore ed eliminare il messaggio, o il ruolo di lavoro tenterà di nuovo l'elaborazione e il ciclo continua per un periodo illimitato.)

### <a name="sql-queries-are-unbounded"></a>Query SQL non sono associate

Fix It codice corrente non inserisce alcun limite sul numero di righe di query per le pagine dell'indice potrebbero essere restituiti. Se il database viene inserito un volume elevato di attività, le dimensioni degli elenchi risultante ricevuto potrebbe causare problemi di prestazioni. La soluzione consiste nell'implementare il paging. Per un esempio, vedere [ordinamento, filtro e Paging con Entity Framework in un'applicazione ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Visualizzazione di modelli consigliati

L'app Fix It Usa la classe di entità FixItTask passare informazioni tra i controller e la visualizzazione. Una procedura consigliata consiste nell'usare i modelli di visualizzazione. Il modello di dominio (ad esempio, la classe di entità FixItTask) si basa su quella necessaria per la persistenza dei dati, mentre un modello di visualizzazione può essere progettato per la presentazione dei dati. Per altre informazioni, vedere [12 ASP.NET MVC consigliate](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Blob dell'immagine protetto consigliato

Gli archivi di app Fix It immagini come pubblici, vale a dire che chiunque individui l'URL può accedere alle immagini caricate. Le immagini che possa essere protetto invece di pubblico.

### <a name="no-powershell-automation-scripts-for-queues"></a>Nessuno script di automazione di PowerShell per le code

Gli script di automazione di PowerShell di esempio sono stati scritti solo per la versione di base di Fix It che viene eseguito interamente in App Web di servizio App di Azure. Non include gli script per l'installazione e distribuzione per l'app web e ambiente del servizio Cloud necessari per l'elaborazione della coda.

### <a name="special-handling-for-html-codes-in-user-input"></a>Gestione speciale per i codici HTML negli input dell'utente

ASP.NET impedisce automaticamente molti modi in cui gli utenti malintenzionati potrebbero tentare attacchi di scripting intersito immettendo lo script nelle caselle di testo di input utente. E MVC `DisplayFor` helper utilizzata per visualizzare l'attività i titoli e note automaticamente i valori di codifica in HTML inviato al browser. Ma in un'app di produzione è possibile adottare misure aggiuntive. Per altre informazioni, vedere [richiesta la convalida in ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Procedure consigliate

Di seguito sono alcuni problemi risolti dopo essere stati individuati nella revisione del codice e il test della versione originale dell'app Fix It. Alcuni sono stati causati dall'autore originale non ne sia consapevole consigliata specifico, alcuni semplicemente perché il codice è stato scritto rapidamente e non è stato progettato per il software. Abbiamo stiamo elenco qui i problemi nel caso in cui c'è qualcosa che abbiamo imparato da questa revisione e il test che può risultare utile per altri utenti che sono anche lo sviluppo di App web.

### <a name="dispose-the-database-repository"></a>Eliminare il repository di database

Il `FixItTaskRepository` classe deve eliminare Entity Framework `DbContext` istanza. Questo viene fatto implementando `IDisposable` nella `FixItTaskRepository` classe:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Si noti che AutoFac eliminerà automaticamente il `FixItTaskRepository` dell'istanza, pertanto non è necessario eliminarlo in modo esplicito.

Un'altra opzione consiste nel rimuovere la `DbContext` variabile membro dal `FixItTaskRepository`ed è preferibile creare una variabile locale `DbContext` variabile all'interno di ogni metodo di repository, all'interno di un `using` istruzione. Ad esempio:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrare i singleton con stato di conseguenza l'inserimento delle dipendenze

Poiché solo un'istanza del `PhotoService` classe e `Logger` classe è necessaria, queste classi devono essere [registrato come singole istanze di inserimento delle dipendenze](https://code.google.com/p/autofac/wiki/InstanceScope) in *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Sicurezza: Non visualizzare i dettagli dell'errore per gli utenti

L'app originale Fix It non dispone di una pagina di errore generico e lasciare tutte le bolle eccezioni fino all'interfaccia utente, in modo che potrebbe causare alcune eccezioni, ad esempio errori di connessione del database in una traccia dello stack completo viene visualizzata nel browser. Informazioni dettagliate sull'errore in alcuni casi può facilitare gli attacchi da utenti malintenzionati. La soluzione consiste nel registrare i dettagli dell'eccezione e visualizzare una pagina di errore all'utente che non include i dettagli dell'errore. L'app Fix It è stata già la registrazione e per visualizzare una pagina di errore, abbiamo aggiunto `<customErrors mode=On>` nel file Web. config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Per impostazione predefinita in questo modo *Views\Shared\Error.cshtml* da visualizzare per gli errori. È possibile personalizzare *Error.cshtml* oppure creare la propria visualizzazione di pagina di errore e aggiungere un `defaultRedirect` attributo. È anche possibile specificare le pagine di errore diversi per errori specifici.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Sicurezza: consentire solo un'attività da modificare dal creatore

La pagina di indice del Dashboard Mostra solo le attività create dall'utente connesso, ma un utente malintenzionato è stato possibile creare un URL con un ID attività dell'utente. È stato aggiunto codice nel *DashboardController.cs* per restituire un errore 404 in questo caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Non inghiottire eccezioni

L'app originale Fix It è appena rientrato null dopo la registrazione di un'eccezione che ha generato da una query SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Questo potrebbe renderlo un aspetto all'utente come se la query è stata completata ma semplicemente non ha restituito alcuna riga. Soluzione consiste nel generare nuovamente l'eccezione dopo il rilevamento e la registrazione:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Intercettare tutte le eccezioni nei ruoli di lavoro

Eccezioni non gestite in un ruolo di lavoro provocherà la macchina virtuale di essere riciclati, pertanto si desidera eseguire il wrapping di tutti gli elementi non in un blocco try-catch e gestire tutte le eccezioni.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Specificare la lunghezza per le proprietà della stringa nelle classi di entità

Per visualizzare codice semplice, la versione originale dell'app Fix It non ha specificato le lunghezze dei campi dell'entità FixItTask e di conseguenza sono state definite come varchar (max) nel database. Di conseguenza, l'interfaccia utente accetterebbero quasi qualsiasi quantità di input. Limiti dei set di lunghezze specificando che si applicano sia all'utente di input nella pagina web e le dimensioni di colonna nel database:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Contrassegnare i membri privati come di sola lettura quando non sono previste per modificare

Ad esempio, nelle `DashboardController` un'istanza della classe `FixItTaskRepository` viene creato e non è prevista la modifica, in modo che è stato definito come [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Usare l'elenco. Any () invece di elenco. Count () &gt; 0

Se si è interessati se uno o più elementi in un elenco soddisfano i criteri specificati, utilizzare il [eventuali](https://msdn.microsoft.com/library/bb534972.aspx) metodo, perché restituisce, non appena viene trovato un elemento corrispondono ai criteri, mentre il `Count` metodo deve sempre eseguire l'iterazione tramite ogni elemento. Il Dashboard *index. cshtml* file conteneva in origine questo codice:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Si modificato in:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generare gli URL nelle visualizzazioni MVC usando gli helper MVC

Per il **crearlo una correzione** pulsante nella home page, Fix It app rigido codificato un elemento anchor:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Per i collegamenti di visualizzazione/azione simile al seguente è preferibile usare il [Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) helper HTML, ad esempio:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Usare Task.Delay invece di thread. Sleep nel ruolo di lavoro

Inserisce il modello nuovo-progetto `Thread.Sleep` nell'esempio di codice per un ruolo di lavoro, ma che causa il thread allo stato di sospensione può causare il pool di thread per generare il thread aggiuntivi non necessari. È possibile evitare che usando [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) invece.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evitare di async void

Se un metodo asincrono non è necessaria restituire un valore, restituisce un `Task` tipo anziché `void`.

Questo esempio si trova il `FixItQueueManager` classe: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

È consigliabile usare `async void` solo per i gestori di eventi di primo livello. Se si definisce un metodo come `async void`, il chiamante non è possibile **await** il metodo o rilevare eventuali eccezioni generate dal metodo. Per altre informazioni, vedere [procedure consigliate nella programmazione asincrona](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Usare un token di annullamento per interrompere il ciclo di ruolo worker

In genere, il **eseguire** metodo su un ruolo di lavoro contiene un ciclo infinito. Quando il ruolo di lavoro viene arrestato, il [Roleentrypoint](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) viene chiamato il metodo. È consigliabile usare questo metodo per annullare il lavoro che viene eseguito all'interno di **eseguiti** (metodo) e uscita normalmente. In caso contrario, il processo potrebbe essere terminato un'operazione in corso.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Rifiutare esplicitamente la procedura di analisi MIME automatica

In alcuni casi, Internet Explorer indica un tipo MIME diverso dal tipo specificato dal server web. Ad esempio, se Internet Explorer rileva contenuto HTML in un file recapitato con l'intestazione della risposta HTTP Content-Type: text/plain, Internet Explorer determina che il contenuto deve essere eseguito il rendering come HTML. Sfortunatamente, questo "analisi MIME" possono causare problemi di sicurezza per i server che ospitano contenuto non attendibile. Per evitare questo problema, Internet Explorer 8 ha apportato alcune modifiche al codice di determinazione dei tipi MIME e consente agli sviluppatori di applicazioni [rifiutare esplicitamente l'analisi MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Il codice seguente è stato aggiunto per il *Web. config* file.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Abilitare la creazione di bundle e minimizzazione

Quando Visual Studio crea un nuovo progetto web, bundling and minification dei file JavaScript non è abilitato per impostazione predefinita. Abbiamo aggiunto una riga di codice in BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Impostare un timeout di scadenza per i cookie di autenticazione

Per impostazione predefinita, i cookie di autenticazione scadono entro due settimane. Un tempo più breve è più sicuro. È possibile modificare questa impostazione nella finestra *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Come eseguire l'app da Visual Studio nel computer locale

Esistono due modi per eseguire l'app Fix It:

- Eseguire l'applicazione di base che scrive le nuove attività direttamente al database SQL.
- Eseguire l'applicazione usando una coda oltre a un servizio back-end per creare le attività. Il modello di accodamento è descritto nel capitolo [modello di lavoro incentrato sulle code](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Eseguire l'applicazione di base

1. Installare [Visual Studio 2013 o Visual Studio 2013 Express per Web](https://www.visualstudio.com/downloads).
2. Installare il [Azure SDK per .NET per Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Scaricare il file con estensione zip dal [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. In Esplora File fare clic con il file con estensione zip e fare clic su proprietà e nella finestra Proprietà fare clic su Annulla blocco.
5. Decomprimere il file.
6. Fare doppio clic sul file con estensione sln per avviare Visual Studio.
7. Dal menu Strumenti, fare clic su Gestione pacchetti libreria e quindi Console di gestione pacchetti.
8. In Package Manager Console (PMC), fare clic su Ripristina.
9. Uscire da Visual Studio.
10. Avviare il [emulatore di archiviazione di Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Riavviare Visual Studio, aprire il file della soluzione che è stata chiusa nel passaggio precedente.
12. Assicurarsi che il progetto FixIt sia impostato come progetto di avvio e quindi premere CTRL+F5 per eseguire il progetto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Eseguire l'applicazione con l'elaborazione della coda

1. Seguire le istruzioni per il [eseguire l'applicazione di base](#runbase), quindi chiudere il browser e chiudere Visual Studio.
2. Avviare Visual Studio con privilegi di amministratore. (Sarà possibile usare l'emulatore di calcolo di Azure, e che richiede privilegi di amministratore).
3. Nell'applicazione *Web. config* del file nei *MyFixIt* project (progetto web), modificare il valore di `appSettings/UseQueues` su "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Se il [emulatore di archiviazione di Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) non è ancora in esecuzione, avviarlo di nuovo.
5. Eseguire il progetto web FixIt e il progetto MyFixItCloudService contemporaneamente.

    Con Visual Studio 2013:

   1. Premere F5 per eseguire il progetto FixIt.
   2. Nelle **Esplora soluzioni**, fare clic sul progetto MyFixItCloudService e quindi fare clic su **Debug** -- **Avvia nuova istanza**.

      Uso di Visual Studio 2013 Express per Web:

   3. In Esplora soluzioni fare doppio clic la soluzione FixIt e selezionare **proprietà**.
   4. Selezionare **progetti di avvio multipli**...
   5. Nel **azione** elenco a discesa in MyFixIt e MyFixItCloudService, selezionare **avviare**.
   6. Fare clic su **OK**.
   7. Premere F5 per eseguire entrambi i progetti.

      Quando si esegue il progetto MyFixItCloudService, Visual Studio avvia l'emulatore di calcolo di Azure. A seconda della configurazione di firewall, è necessario consentire all'emulatore attraverso il firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Come distribuire l'app di base di App Web di servizio App di Azure usando gli script di Windows PowerShell

Per illustrare la [automatizzare tutto](automate-everything.md) modello, l'app Fix It viene fornito con gli script che configurare un ambiente di Azure e distribuire il progetto nel nuovo ambiente. Le istruzioni seguenti illustrano come usare gli script.

Se si desidera eseguire in Azure senza usare le code e sono state apportate le modifiche per eseguire localmente con le code, assicurarsi che impostare il valore di appSetting UseQueues reimpostarla su false prima di procedere con le istruzioni seguenti.

Queste istruzioni presuppongono che hai già scaricato ed eseguire la soluzione Fix It in locale, e che si disponga di un'istanza di Azure dell'account o avere una sottoscrizione di Azure che si è autorizzati a gestire.

1. Installare il **Azure PowerShell** console. Per istruzioni, vedere [come installare e configurare Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Questa console personalizzata è configurata per funzionare con la sottoscrizione di Azure. Il modulo di Azure viene installato nel *Program Files* directory e viene importato automaticamente in ogni uso della console di Azure PowerShell.

    Se si preferisce lavorare in un programma host diverso, ad esempio Windows PowerShell ISE, assicurarsi di usare la [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet per importare il modulo di Azure o usare un comando nel modulo di Azure per attivare l'importazione automatica del modulo.
2. Avviare Azure PowerShell con il **Esegui come amministratore** opzione.
3. Eseguire la [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet di impostare i criteri di esecuzione di Azure PowerShell `RemoteSigned`. Immettere **Y** (per Sì) per completare la modifica dei criteri.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Questa impostazione consente di eseguire gli script locali che non sono firmati digitalmente. (È anche possibile impostare i criteri di esecuzione su `Unrestricted`, che avrebbe eliminato la necessità per il passaggio Sblocca in un secondo momento, ma questa operazione è sconsigliata per motivi di sicurezza.)
4. Eseguire il `Add-AzureAccount` cmdlet configurare PowerShell con le credenziali per l'account.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Queste credenziali scadono dopo un periodo di tempo ed è necessario eseguire nuovamente il `Add-AzureAccount` cmdlet. Come viene scritto questo e-book, il limite di tempo prima della scadono delle credenziali è 12 ore.
5. Se si hanno più sottoscrizioni, usare il cmdlet Select-AzureSubscription per specificare la sottoscrizione che si desidera creare l'ambiente di test in.
6. Importare un certificato di gestione per la stessa sottoscrizione di Azure usando il `Get-AzurePublishSettingsFile` e `Import-AzurePublishSettingsFile` cmdlet. Il primo di questi cmdlet scarica un file di certificato e nel secondo si è specificare il percorso del file per importarlo. > [!IMPORTANT]
   > Conservare il file scaricato in un luogo sicuro oppure eliminarlo al termine, perché contiene un certificato che può essere utilizzato per gestire i servizi di Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Il certificato viene usato per una chiamata all'API REST che consente di rilevare l'indirizzo IP del computer di sviluppo per impostare una regola del firewall nel server di Database SQL.
7. Eseguire la [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (alias sono `cd`, `chdir`, e `sl`) per passare alla directory che contiene gli script. (Che si trovano nel *automazione* della cartella di soluzione Fix It.) Inserire il percorso tra virgolette se uno dei nomi di directory contiene spazi. Ad esempio, per passare al `c:\Sample Apps\FixIt\Automation` directory è possibile immettere il comando seguente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Per consentire a Windows PowerShell per eseguire questi script, usare il [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet. (Gli script sono bloccati perché sono state scaricate da Internet).

    > [!WARNING]
    > Security - prima dell'esecuzione `Unblock-File` in qualsiasi script o file eseguibile, aprire il file nel blocco note, esaminare i comandi e verificare che non contengono codice dannoso.

    Ad esempio, il comando seguente esegue il `Unblock-File` cmdlet in tutti gli script nella directory corrente.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Per creare l'app web per la base (Nessuna coda di elaborazione) correggerlo app, eseguire lo script di creazione ambiente.

    Obbligatorio `Name` parametro specifica il nome del database e viene usato anche per l'account di archiviazione create dallo script. Il nome deve essere globalmente univoco all'interno del dominio azurewebsites.net. Se si specifica un nome che non è univoco, ad esempio Fixit o Test (o persino come nell'esempio, fixitdemo), il `New-AzureWebsite` cmdlet ha esito negativo con un errore interno che segnala un conflitto. Lo script converte il nome in lettere minuscole per rispettare i requisiti di nome per le app web, gli account di archiviazione e database.

    Obbligatorio `SqlDatabasePassword` parametro specifica la password per l'account amministratore che verrà creato per il Database SQL. Non includere i caratteri XML speciali nella password (&amp; &lt; &gt; ;). Si tratta di una limitazione del modo in cui gli script sono stati scritti, non una limitazione di Azure.

    Ad esempio, se si desidera creare un'app web denominata "fixitdemo" e usare una password di amministratore di SQL Server di "Passw0rd1", è possibile immettere il comando seguente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Il nome deve essere univoco nel dominio azurewebsites.net, e la password deve soddisfare i requisiti del Database SQL per la complessità delle password. (L'esempio Passw0rd1 soddisfa i requisiti).

    Si noti che il comando inizia con ". \". Per evitare che malintenzionato esecuzione degli script, Windows PowerShell richiede di specificare il percorso completo del file di script quando si esegue uno script. È possibile usare un punto per indicare la directory corrente (".\") In alternativa, specificare il percorso completo, ad esempio:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Per altre informazioni sullo script, usare il `Get-Help` cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    È possibile usare la `Detailed`, `Full`, `Parameters`, e `Examples` parametri del cmdlet Get-Help per filtrare la Guida che viene restituita.

    Se lo script ha esito negativo o genera errori, ad esempio "New-AzureWebsite: chiamare Set-AzureSubscription e Select-AzureSubscription first," si potrebbe non avere completato la configurazione di Azure PowerShell.

    Al termine dell'esecuzione dello script, è possibile usare il portale di gestione di Azure per visualizzare le risorse che sono state create, come illustrato nel [automatizzare tutto](automate-everything.md) capitolo.
10. Per distribuire il progetto FixIt nel nuovo ambiente di Azure, usare il *AzureWebsite.ps1* script. Ad esempio:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Quando viene eseguita la distribuzione, il browser si apre con Fix It in esecuzione in Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Risoluzione dei problemi di script di Windows PowerShell

Gli errori più comuni rilevati durante l'esecuzione di questi script sono correlati alle autorizzazioni. Verificare che l'opzione `Add-AzureAccount` e `Import-AzurePublishSettingsFile` hanno avuto esito positivo e utilizzata per la stessa sottoscrizione di Azure. Anche se `Add-AzureAccount` era riuscita potrebbe essere necessario eseguirlo nuovamente. Le autorizzazioni aggiunte da `Add-AzureAccount` scadono dopo 12 ore.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Riferimento a un oggetto non impostato su un'istanza di oggetto.

Se lo script restituisce errori, ad esempio "Riferimento oggetto non impostato su un'istanza di un oggetto," che significa che Windows PowerShell non è possibile trovare un oggetto processo (si tratta di un'eccezione con riferimento null), eseguire il `Add-AzureAccount` cmdlet e provare di nuovo lo script.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: Il server ha rilevato un errore interno.

Il `New-AzureWebsite` cmdlet restituisce un errore interno quando il nome non è univoco nel dominio azurewebsites.net. Per risolvere l'errore, utilizzare un valore diverso per il nome, ovvero nel parametro Name del *New-AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Il riavvio dello script

Se è necessario riavviare il *New-AzureWebsiteEnv.ps1* script perché non è riuscito prima che quest ' ultimo stampato il messaggio "Lo Script sia completo", è possibile eliminare le risorse che lo script creato prima dell'arresto. Ad esempio, se lo script è già stato creato l'app web di ContosoFixItDemo e si esegue nuovamente lo script con lo stesso nome, lo script avrà esito negativo perché il nome è in uso.

Per determinare le risorse che lo script creato prima dell'arresto, usare i cmdlet seguenti:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Per eseguire questo cmdlet, inviare tramite pipe il nome del server di database per `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Per eliminare queste risorse, usare i comandi seguenti. Si noti che se si elimina il server di database, vengono automaticamente eliminati i database associati al server.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Come distribuire l'app con la coda di elaborazione per App Web di servizio App di Azure e un servizio Cloud di Azure

Per abilitare le code, apportare la modifica seguente nel file MyFixIt\Web.config. Sotto `appSettings`, modificare il valore di `UseQueues` su "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Distribuire quindi l'applicazione MVC in un'app web nel servizio App di Azure, come descritto [precedenti](#deploybase).

Successivamente, creare un nuovo servizio cloud di Azure. Gli script inclusi con l'app Fix It non creare o distribuire il servizio cloud, pertanto è necessario usare il portale di Azure per questo oggetto. Nel portale, fare clic su **New** -- **calcolo** – **servizio Cloud** -- **creazione rapida**e quindi immettere un URL e una posizione del data center. Uso dello stesso data center in cui è distribuita l'app web.

![](the-fix-it-sample-application/_static/image1.png)

Prima di poter distribuire il servizio cloud, è necessario aggiornare alcuni dei file di configurazione.

In MyFixIt.WorkerRoler\app.config, sotto `connectionStrings`, sostituire il valore del `appdb` stringa di connessione con la stringa di connessione effettiva per il Database SQL. È possibile ottenere la stringa di connessione dal portale. Nel portale, fare clic su **database SQL** - **appdb** - **stringhe di connessione del Database SQL di visualizzazione per ADO .net, ODBC, PHP e JDBC**. Copiare la stringa di connessione ADO.NET e incollare il valore nel file app. config. Sostituire "{il\_password\_qui}" con la password del database. (Supponendo che gli script usato per distribuire l'app MVC, è specificata la password del database nel `SqlDatabasePassword` parametro di script.)

Il risultato dovrebbe essere simile al seguente:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Nello stesso file, MyFixIt.WorkerRoler\app.config sotto `appSettings`, sostituire i due valori di segnaposto per l'account di archiviazione di Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

È possibile ottenere la chiave di accesso dal portale. Visualizzare [come gestire gli account di archiviazione](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

In MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, sostituire gli stessi due valori di segnaposto per l'account di archiviazione di Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

A questo punto si è pronti per distribuire il servizio cloud. In Esplora soluzioni, fare clic sul progetto MyFixItCloudService e selezionare **pubblica**. Per altre informazioni, vedere "[distribuire l'applicazione in Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", ovvero nella parte 2 del [in questa esercitazione](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Precedente](more-patterns-and-guidance.md)
