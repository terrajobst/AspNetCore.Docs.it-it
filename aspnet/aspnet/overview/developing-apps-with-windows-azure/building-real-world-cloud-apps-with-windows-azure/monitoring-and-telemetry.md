---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitoraggio e telemetria (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: aspnetcontent
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: e71970e0ec3a10d464c56e0eed11ff97e9580a26
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822063"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitoraggio e telemetria (creazione di App Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


Molte persone si basano sui clienti per informarli quando l'applicazione è inattiva. Che non è realmente una procedura consigliata ovunque e in particolare, non nel cloud. Non c'è garanzia di notifica rapida e quando si riceva una notifica, si ottengono spesso dati minima o ingannevoli su cosa è successo. Con è possibile controllare cosa sta succedendo con l'app e quando qualcosa di sistemi di registrazione e telemetria efficaci, andare individuare subito eventuali problemi e avere informazioni utili per lavorare con.

## <a name="buy-or-rent-a-telemetry-solution"></a>Acquista o noleggiare una soluzione di telemetria

> [!NOTE]
> Questo articolo è stato scritto prima [Application Insights](https://azure.microsoft.com/services/application-insights/) è stata rilasciata. Application Insights è la scelta migliore per le soluzioni di dati di telemetria in Azure. Visualizzare [configurazione di Application Insights per il sito Web ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) per altre informazioni.


Uno degli aspetti che è ideale sull'ambiente cloud è che è molto facile da acquistare o noleggiare verso vittoria. I dati di telemetria è riportato un esempio. Senza un notevole sforzo è possibile ottenere un sistema di telemetria davvero efficaci verso l'alto e in esecuzione, in modo molto conveniente. Esistono una serie di ottimo partner che si integrano con Azure e alcune non sono i livelli gratuiti, quindi è possibile ottenere i dati di telemetria di base per nulla. Ecco solo alcuni di quelli attualmente disponibili in Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

A partire da marzo 2015 [Microsoft Application Insights per Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) non è ancora rilasciata, ma è disponibile in anteprima per provare. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) include anche funzionalità di monitoraggio.

Verrà illustrato rapidamente con la configurazione di New Relic per mostrare quanto può essere facile da utilizzare un sistema di telemetria.

Nel portale di gestione di Azure, effettuare l'iscrizione per il servizio. Fare clic su **New**, quindi fare clic su **Store**. Il **scegliere un componente aggiuntivo** verrà visualizzata la finestra di dialogo. Scorrere verso il basso e fare clic su **New Relic**.

![Scegliere un componente aggiuntivo](monitoring-and-telemetry/_static/image1.png)

Fare clic sulla freccia destra e scegliere il livello di servizio desiderato. Per questa dimostrazione verrà usato il livello gratuito.

![Personalizza componente aggiuntivo](monitoring-and-telemetry/_static/image2.png)

Fare clic sulla freccia a destra, confermare la "acquisto" e New Relic a questo punto viene visualizzato come un componente aggiuntivo nel portale.

![Rivedi acquisto](monitoring-and-telemetry/_static/image3.png)

![Nuovo componente aggiuntivo Relic nel portale di gestione](monitoring-and-telemetry/_static/image4.png)

Fare clic su **le informazioni di connessione**e copiare la chiave di licenza.

![Informazioni di connessione](monitoring-and-telemetry/_static/image5.png)

Andare alla **configura** scheda per impostare l'app web nel portale di **Performance Monitoring** a **componente aggiuntivo**e impostare il **scegliere componenti aggiuntivi** elenco a discesa per **New Relic**. Quindi fare clic su **salvare**.

![New Relic nella scheda Configura](monitoring-and-telemetry/_static/image6.png)

In Visual Studio, installare il pacchetto NuGet di New Relic nell'app.

![Analitica per gli sviluppatori nella scheda Configura](monitoring-and-telemetry/_static/image7.png)

Distribuire l'app in Azure e iniziare a usarlo. Creare alcune Fix It attività per fornire alcune attività per New Relic monitorare.

Quindi tornare al **New Relic** pagina il **componenti aggiuntivi** scheda del portale e fare clic su **Gestisci**. Il portale invia all'utente per il portale di gestione di New Relic, con accesso single sign-on per l'autenticazione in modo da non dover immettere nuovamente le credenziali. La pagina Panoramica offre un'ampia gamma di statistiche sulle prestazioni. (Fare clic sull'immagine per visualizzare le dimensioni della pagina overview completi).

[![Nuova scheda Monitoraggio Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Di seguito sono elencate alcune delle statistiche è possibile visualizzare:

- Tempo medio di risposta a diverse ore del giorno.

    ![Tempo di risposta](monitoring-and-telemetry/_static/image10.png)
- Velocità effettiva (in numero di richieste al minuto) a diverse ore del giorno.

    ![Velocità effettiva](monitoring-and-telemetry/_static/image11.png)
- Tempo di CPU del server impiegato per la gestione delle richieste HTTP diverse.

    ![Tempi di transazione Web](monitoring-and-telemetry/_static/image12.png)
- Tempo di CPU impiegato in diverse parti del codice dell'applicazione:

    ![Dettagli traccia](monitoring-and-telemetry/_static/image13.png)
- Statistiche sulle prestazioni cronologiche.

    ![Prestazioni cronologiche](monitoring-and-telemetry/_static/image14.png)
- Le chiamate ai servizi esterni, ad esempio il servizio Blob e le statistiche relative alle modalità affidabile e reattivo il servizio è stato.

    ![Servizi esterni](monitoring-and-telemetry/_static/image15.png)

    ![Servizi esterni](monitoring-and-telemetry/_static/image16.png)

    ![Servizio esterno](monitoring-and-telemetry/_static/image17.png)
- Informazioni su dove in tutto il mondo o nell'app web Usa il traffico di provenienza.

    ![Geografia](monitoring-and-telemetry/_static/image18.png)

È anche possibile impostare report ed eventi. Ad esempio, è possibile ad esempio ogniqualvolta si iniziare a vedere gli errori, inviare un messaggio di posta elettronica al personale di supporto degli avvisi per il problema.

![Report](monitoring-and-telemetry/_static/image19.png)

New Relic è solo un esempio di un sistema di telemetria; è possibile ottenere tutto questo da anche altri servizi. Il vantaggio del cloud è che senza dover scrivere alcun codice e per minime o Nessuna spesa, è improvvisamente possibile ottenere molte più informazioni sull'uso dell'applicazione e ciò che effettivamente si verificano i tuoi clienti.

<a id="log"></a>
## <a name="log-for-insight"></a>Log per informazioni dettagliate

Un pacchetto di dati di telemetria è un buon primo passo, ma è comunque necessario instrumentare il proprio codice. Indica il servizio di telemetria quando si verifica un problema e indica ciò che si verificano i clienti, ma potrebbe non produrre una grande quantità di informazioni relative a cosa sta succedendo nel codice.

Non si desidera avere in remoto un server di produzione per visualizzare tutte le app. Che potrebbe essere pratico dopo aver ottenuto un server, ma cosa accade se è stato ridimensionato a centinaia di server e non si conoscono quali è necessario accedere in remoto? La registrazione deve fornire informazioni sufficienti che non hai mai in remoto i server di produzione per analizzare ed eseguire il debug dei problemi. Si devono creare i log informazioni sufficienti in modo che è possibile isolare i problemi esclusivamente tramite i log.

### <a name="log-in-production"></a>Accedere in fase di produzione

Molte persone attivare la traccia nell'ambiente di produzione solo quando si è verificato un problema e che si desidera eseguire il debug. Questa operazione può introdurre un ritardo significativo tra il momento che si verifica un problema e l'ora che è ottenere utili informazioni sulla risoluzione dei problemi su di esso. E le informazioni che ottengono potrebbero non essere utile per gli errori intermittenti.

È consigliabile nell'ambiente cloud in cui l'archiviazione è economica è lasciare sempre l'accesso nell'ambiente di produzione. In questo modo, quando si verificano errori si sono già disponibili registrati e sono disponibili dati cronologici che possono aiutare è analizzare i problemi che sviluppano nel tempo o si verificano regolarmente in momenti diversi. È possibile automatizzare un processo di eliminazione per eliminare i vecchi log, ma si noterà che è più costoso configurare un processo di questo tipo anziché a conservare i log.

Il costo aggiuntivo di registrazione è semplice rispetto alla quantità di risoluzione dei problemi di tempo e denaro, che è possibile salvare facendo in modo che tutte le informazioni necessarie già disponibile quando si verificano problemi. Quindi quando un utente indica che avevano un errore casuale a qualche minuto circa 8.00 notte scorsa, ma non ricorda l'errore, è possibile facilmente scoprire qual è il problema.

Per meno di $4 per un mese è possibile mantenere d' parte 50 gigabyte di registri e l'impatto sulle prestazioni della registrazione è semplice, purché si tenere a mente che, per evitare colli di bottiglia delle prestazioni, assicurarsi che la libreria di registrazione è asincrona.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Distinguere i log che indicano a dai registri che richiedono un'azione

I log sono concepiti per INFORM (voglio sapere qualcosa) o ACT (voglio eseguire un'operazione). Prestare attenzione solo scrivere i log di ACT per i problemi che richiedono effettivamente una persona o un processo automatizzato per eseguire azioni. I log di ACT troppi creerà rumore, richiedere una quantità eccessiva lavoro richiedeva di tutto per individuare problemi autentici. E se i log di ACT attivano automaticamente un'azione, ad esempio l'invio di posta elettronica al personale di supporto, evitare di lasciare che migliaia di tali azioni essere attivato da un singolo problema.

Traccia di .NET System. Diagnostics, i log possono essere assegnati a livello di errore, avviso, informazioni e Debug/Verbose. È possibile distinguere ACT dai registri INFORM riservando il livello di errore per i log di ACT e utilizzando i livelli più bassi per i log INFORM.

![Livelli di registrazione](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurare i livelli di registrazione in fase di esecuzione

Sebbene sia utile per avere sempre la registrazione nell'ambiente di produzione, un'altra procedura consigliata consiste nell'implementare un framework di registrazione che consente di regolare in fase di esecuzione il livello di dettaglio che si accede, senza ridistribuire o il riavvio dell'applicazione. Ad esempio quando si usa la funzionalità di traccia in `System.Diagnostics` è possibile creare l'errore, avviso, informazioni e i log di Debug/Verbose. È consigliabile sempre accesso Error, Warning, Info log nell'ambiente di produzione e devi essere in grado di aggiungere in modo dinamico la registrazione di Debug/dettagliato per la risoluzione dei problemi nel caso per caso.

Le app Web nel servizio App di Azure includono supporto incorporato per la scrittura `System.Diagnostics` i log per il file system, archiviazione tabelle o archiviazione Blob. È possibile selezionare diversi livelli di registrazione per ogni destinazione di archiviazione ed è possibile modificare il livello di registrazione in tempo reale senza dover riavviare l'applicazione. Il supporto di archiviazione Blob, è facile eseguire [HDInsight](https://docs.microsoft.com/azure/hdinsight/) dei processi di analisi nei log di applicazione, poiché HDInsight in grado di lavorare direttamente con l'archiviazione Blob.

### <a name="log-exceptions"></a>Registrare eccezioni

Non è sufficiente inserire *eccezione. Metodo ToString ()* del codice di registrazione. Che esclude le informazioni contestuali. In caso di errori SQL, lascia il numero di errore SQL. Per tutte le eccezioni, includere le informazioni sul contesto, l'eccezione stessa ed eccezioni interne per assicurarsi che vengano forniti tutti gli elementi che saranno necessari per la risoluzione dei problemi. Informazioni sul contesto potrebbe includere ad esempio, il nome del server, un identificatore di transazione e un nome utente (ma non la password o segreti).

Se ci si affida a fare la cosa giusta con la registrazione delle eccezione ogni sviluppatore, non sarà alcuni di essi. Per garantire che viene completato il diritto modo ogni volta che, a destra nell'interfaccia del logger di gestione delle eccezioni di compilazione: passare l'oggetto eccezione per la classe logger e registrare correttamente i dati dell'eccezione nella classe logger.

### <a name="log-calls-to-services"></a>Registro chiamate ai servizi

Si consiglia di scrivere un log ogni volta che l'app effettua una chiamata a un servizio, si tratti di un database o un'API REST o qualsiasi servizio esterno. Includere nei log non solo un'indicazione di esito positivo o errore ma il tempo impiegato di ogni richiesta. Nell'ambiente cloud si noteranno spesso problemi correlati a rallentamenti anziché a interruzioni completate. Qualcosa che prende in genere 10 millisecondi improvvisamente può avviare l'esecuzione di un secondo. Quando un utente indica che l'app è lenta, potrebbe essere in grado di analizzare in New Relic o qualsiasi servizio di telemetria, si dispone e convalidare l'esperienza e quindi si desidera essere in grado di analizzare siano file di log per approfondire i dettagli del motivo per cui è lenta.

### <a name="use-an-ilogger-interface"></a>Usare un'interfaccia ILogger

Che cos'è consigliabile scegliere questo quando si crea un'applicazione di produzione consiste nel creare una semplice *ILogger* interfaccia e utilizzare soltanto quello di alcuni metodi in esso. Questo rende facile da modificare l'implementazione della registrazione in un secondo momento e non deve eseguire tutto il codice per eseguire questa operazione. È possibile usare la `System.Diagnostics.Trace` classe nell'intera app Fix It, ma invece utilizziamo, dietro le quinte in una classe di registrazione che implementa *ILogger*, ed effettuerà *ILogger* metodo chiama in tutto l'app.

In questo modo, se si vuole usare rendere più completa, la registrazione è possibile sostituire [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) con qualsiasi meccanismo di registrazione desiderato. Ad esempio, man mano che aumenta l'app è possibile decidere che si desidera usare, ad esempio un pacchetto di registrazione più completo [NLog](http://nlog-project.org/) oppure [blocco applicazione per la registrazione della libreria Enterprise](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) è un altro framework di registrazione diffusi ma non esegue la registrazione asincrona.)

Uno dei motivi possibili per l'uso di un framework, come NLog consiste nel facilitare suddividere la registrazione di output in archivi dati separati di volume elevato e a valore elevato. Che consente di archiviare in modo efficiente volumi elevati di dati INFORM che non è necessario eseguire query rapide su, conservando l'accesso rapido ai dati di ACT.

### <a name="semantic-logging"></a>Registrazione semantica

Per un modo relativamente nuovo per eseguire la registrazione in grado di produrre informazioni di diagnostica più utili, vedere [Enterprise Library registrazione semantica Application Block (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Usa SLAB [traccia eventi per Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) e [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) supportare in .NET 4.5 che consentono di creare più log strutturati e queryable. Si definisce un metodo diverso per ogni tipo di evento che si accede, che consente di personalizzare le informazioni che si scrive. Ad esempio, per registrare un errore di Database SQL è possibile chiamare un `LogSQLDatabaseError` (metodo). Per questo tipo di eccezione, sai che un'informazione chiave è il numero di errore, pertanto è possibile includere un parametro di numeri di errore nella firma del metodo e prendere nota del numero di errore come campo distinto del record di log che si scrive. Poiché il numero è in un campo separato in modo più semplice e affidabile, potrai report basati sui numeri di errore SQL rispetto a quanto potresti se si sono stati concatenazione semplicemente il numero di errore in una stringa di messaggio.

## <a name="logging-in-the-fix-it-app"></a>La registrazione di correzione, app

### <a name="the-ilogger-interface"></a>L'interfaccia ILogger

Di seguito è riportato il *ILogger* interfaccia nell'app Fix It.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Questi metodi consentono di scrivere i log agli stessi quattro livelli supportati da *System. Diagnostics*. I metodi TraceApi sono per la registrazione delle chiamate ai servizi esterni con informazioni sulla latenza. È anche possibile aggiungere un set di metodi per il livello di Debug/Verbose.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>L'implementazione del Logger dell'interfaccia ILogger

L'implementazione dell'interfaccia è davvero semplice. In pratica semplicemente chiama nello standard *System. Diagnostics* metodi. Il frammento seguente illustra tutti e tre i metodi di informazioni e uno per ciascuno degli altri.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>La chiamata ai metodi ILogger

Ogni volta codice nell'app Fix It rileva un'eccezione, chiama un' *ILogger* metodo per registrare i dettagli dell'eccezione. E ogni volta che effettua una chiamata al database, servizio Blob o un'API REST, viene avviato un cronometro prima della chiamata, interrompe il cronometro quando termina, il servizio e registra il tempo trascorso e le informazioni sull'esito positivo o negativo.

Si noti che il messaggio di log include il nome della classe e il nome di metodo. È buona norma assicurarsi che i messaggi di log di identificare quale parte del codice dell'applicazione veniva scritto.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

A questo punto per ogni volta che l'app Fix It ha effettuato una chiamata al Database SQL, è possibile visualizzare la chiamata, il metodo che chiamato, ed esattamente come il tempo necessario.

![Query nei log del Database SQL](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Se si passare esplorando tramite i log, si noterà che il tempo delle chiamate al database è variabile. Che informazioni potrebbero essere utili: poiché l'app Registra tutto ciò è possibile analizzare le tendenze cronologiche nel modo in cui sta eseguendo il servizio di database nel corso del tempo. Ad esempio, un servizio potrebbe essere veloce nella maggior parte dei casi, ma richieste potrebbero non riuscire o risposte potrebbe risultare rallentata in determinati momenti del giorno.

È possibile eseguire la stessa operazione per il servizio Blob, ogni volta che l'app consente di caricare un nuovo file, vi è un log e si può vedere esattamente il tempo impiegato per caricare ogni file.

![Log di caricamento BLOB](monitoring-and-telemetry/_static/image23.png)

È solo un paio di righe aggiuntive di codice per scrivere ogni volta che si chiama un servizio, e ora ogni volta che qualcuno dice che si è verificato un problema, si conosce esattamente il problema riscontrato, se è verificato un errore o anche se è stato appena esecuzione lenta. È possibile individuare l'origine del problema senza dover accedere in un server remoto o attivare la registrazione dopo l'errore si verifica e sperare di crearlo nuovamente.

## <a name="dependency-injection-in-the-fix-it-app"></a>Inserimento di dipendenze di correzione, app

Ci si potrebbe chiedere come costruttore repository di esempio illustrato in precedenza Ottiene il logger di implementazione dell'interfaccia:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Per collegare l'interfaccia per l'implementazione dell'app Usa [inserimento delle dipendenze](http://en.wikipedia.org/wiki/Dependency_injection)(inserimento delle dipendenze) con [AutoFac](http://autofac.org/). L'inserimento delle dipendenze consente di usare un oggetto basato su un'interfaccia in numerose posizioni in tutto il codice e avere solo specificare, in un'unica posizione, l'implementazione che viene utilizzata quando viene creata un'istanza dell'interfaccia. Questo rende più semplice modificare l'implementazione: ad esempio, voler sostituita da un logger NLog il logger di System. Diagnostics. In alternativa, per i test automatizzati è possibile sostituire una versione fittizia del logger.

L'applicazione Fix It Usa l'inserimento delle dipendenze in tutti i repository e tutti i controller. I costruttori delle classi controller ottenere un' *ITaskRepository* interfaccia allo stesso modo il repository Ottiene un'interfaccia del logger:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

L'app Usa la libreria di AutoFac l'inserimento delle dipendenze per fornire automaticamente *TaskRepository* e *Logger* istanze per questi costruttori.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Questo codice indica fondamentalmente che necessita di un costruttore in un punto qualsiasi un' *ILogger* l'interfaccia, passare un'istanza del *Logger* (classe), e ogni volta che è necessario un *IFixItTaskRepository*l'interfaccia, passare in un'istanza di *FixItTaskRepository* classe.

[AutoFac](http://autofac.org/) è uno dei molti framework di inserimento delle dipendenze che è possibile usare. È un altro più diffusi [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), che è consigliata e supportata da Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Supporto per la registrazione predefinita in Azure

Azure supporta i tipi seguenti di [la registrazione per le app Web nel servizio App di Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Analisi di System. Diagnostics (è possibile attivare e disattivare e impostare i livelli in tempo reale senza dover riavviare il sito).
- Eventi di Windows.
- Log di IIS (HTTP/FREB).

Azure supporta i tipi seguenti di [la registrazione nei servizi Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Traccia System. Diagnostics.
- Contatori delle prestazioni.
- Eventi di Windows.
- Log di IIS (HTTP/FREB).
- Monitoraggio di directory personalizzata.

L'app Fix It Usa analisi System. Diagnostics. È sufficiente eseguire per abilitare la registrazione in un'app web di System. Diagnostics è capovolgere un'opzione nel portale o chiamare l'API REST. Nel portale, fare clic sui **Configuration** scheda per il sito e scorrere verso il basso per visualizzare i **Application Diagnostics** sezione. È possibile attivare o disattivare la registrazione e selezionare il livello di registrazione desiderato. È possibile che Azure scriva i log nel file System o in un account di archiviazione.

![Diagnostica dell'App e diagnostica del sito nella scheda Configura](monitoring-and-telemetry/_static/image24.png)

Dopo aver abilitato la registrazione in Azure, è possibile visualizzare i log nella finestra di Output di Visual Studio al momento della creazione.

![Menu di scelta dei log di streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu di scelta dei log di streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

È anche possibile avere scrittura all'account di archiviazione dei log e visualizzare con qualsiasi strumento che è possibile accedere al servizio di archiviazione tabelle di Azure, ad esempio **Esplora Server** in Visual Studio oppure [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Log in Esplora Server](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Riepilogo

È davvero semplice implementare un sistema di telemetria di out-of-the-box, instrumentare la registrazione nel proprio codice e configurare la registrazione in Azure. E quando si verificano problemi di produzione, la combinazione di un sistema di telemetria e i log personalizzati consentono di risolvere rapidamente i problemi prima che diventino problemi principali per i clienti.

Nel [capitolo successivo](transient-fault-handling.md) esamineremo come gestire gli errori temporanei, in modo che non diventino problemi di produzione da analizzare.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse.

Documentazione principalmente sui dati di telemetria:

- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere indicazioni sulla strumentazione e i dati di telemetria sulle, indicazioni sulla misurazione del servizio, il modello di monitoraggio Endpoint di integrità e modello di riconfigurazione di Runtime.
- [Penny avvicinamento nel Cloud: abilitazione di nuove Relic Performance Monitoring nei siti Web Azure](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper Mark Simms e Michael Thomassy. Vedere la sezione di dati di telemetria e diagnostica.
- [Sviluppo di nuova generazione con Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Articolo di MSDN Magazine.

Documentazione principalmente sulla registrazione:

- [Blocco applicazione per la registrazione semantica (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie presenta nel caso di registrazione semantica con SLAB.
- [Creazione di log strutturati e significative con la registrazione semantica](https://channel9.msdn.com/Events/Build/2013/3-336). (Video) Julian Dominguez presenta nel caso di registrazione semantica con SLAB.
- [EF6 Registrazione SQL – parte 1: registrazione semplice](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers Mostra come registrare le query eseguite da Entity Framework 6, Entity Framework.
- [Resilienza della connessione e intercettazione dei comandi con Entity Framework in un'applicazione ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). La quarta in una serie di esercitazioni nove parti illustra come usare la funzionalità di intercettazione del comando Entity Framework 6 per registrare i comandi SQL inviati al database da Entity Framework.
- [Migliorare la registrazione usando c# attributi informativi sul chiamante 5.0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Come registrare in modo semplice il nome del metodo chiamante senza hardcoded in valori letterali o usare la reflection per crearlo manualmente.

Documentazione principalmente sulla risoluzione dei problemi:

- [Azure Troubleshooting &amp; debug blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools – la diagnostica utilità utilizzata dal Team di supporto per gli sviluppatori Azure](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Introduce e fornisce un collegamento di download per uno strumento che sono utilizzabili in una VM di Azure per scaricare ed eseguire un'ampia gamma di strumenti di diagnostica e monitoraggio. È utile quando è necessario diagnosticare un problema in una determinata VM.
- [Risolvere i problemi di un'app web nel servizio App di Azure con Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Un'esercitazione dettagliata per la Guida introduttiva a System. Diagnostics traccia e debug remoto.

Video

- [Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti Marc Mercuri, Ulrich Homann e Mark Simms. Presenta i concetti generali e principi architetturali in modo estremamente accessibile e interessano, con storie ricavate dall'esperienza di Microsoft Customer Advisory Team (CAT) con i clienti effettivi. Episodi 4 e 9 sono sul monitoraggio e telemetria. Episodio 9 include una panoramica del monitoraggio dei servizi MetricsHub AppDynamics, New Relic e PagerDuty.
- [Compilazione Big: Lezioni apprese dai clienti di Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms illustra la progettazione di errore e strumentazione di tutti gli elementi. Simile alla serie di tipo operatore alternativo, ma passa ad analizzare altri dettagli sulle procedure.

Esempio di codice:

- [Sviluppo di servizi in Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio creata per il Team di consulenza clienti di Microsoft Azure. Viene illustrato sia i dati di telemetria e procedure consigliate di registrazione, come illustrato negli articoli seguenti. L'esempio implementa la registrazione delle applicazioni usando [NLog](http://nlog-project.org/). Per la documentazione correlata, vedere la [serie di quattro articoli di wiki di TechNet sulla registrazione e telemetria](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Precedente](design-to-survive-failures.md)
> [Successivo](transient-fault-handling.md)
