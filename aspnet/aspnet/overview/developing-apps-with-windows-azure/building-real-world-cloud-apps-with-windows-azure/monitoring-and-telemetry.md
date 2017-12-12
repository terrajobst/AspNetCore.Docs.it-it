---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitoraggio e telemetria (creazione di applicazioni con Azure Cloud del mondo reale) | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: dfb0158ec05c890ecf80571d95b22d8c791ba7fc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitoraggio e telemetria (creazione di applicazioni Cloud reale in Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


Molti utenti si basano sui clienti per informarli quando l'applicazione è inattivo. Che non sono in effetti una procedura consigliata in qualsiasi luogo e in particolare, non nel cloud. Non c'è garanzia di notifica rapida e quando ricevere notifiche, spesso ottenere dati minimi o fuorvianti su cosa è successo. Con i dati di telemetria buona e i sistemi di registrazione che è possibile tenere cosa succede con l'app e quando un elemento andare errati sapere immediatamente e delle informazioni sulla risoluzione dei problemi utili per lavorare con.

## <a name="buy-or-rent-a-telemetry-solution"></a>Acquistare o noleggiare una soluzione di telemetria

> [!NOTE]
> Questo articolo è stato scritto prima [Application Insights](https://azure.microsoft.com/services/application-insights/) è stato rilasciato. Application Insights è la scelta migliore per le soluzioni di telemetria in Azure. Vedere [configurare Application Insights per il sito Web ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) per ulteriori informazioni.


Una delle operazioni che ottima sull'ambiente cloud è che è molto semplice di acquistare o noleggiare possibile vittoria. Dati di telemetria è riportato un esempio. Senza un notevole sforzo è possibile ottenere un sistema di telemetria veramente accurati verso l'alto e in esecuzione, molto costoso. Esistono una serie di grande partner che si integrano con Azure e alcune non sono disponibili livelli, è possibile ottenere dati di telemetria base per nulla. Ecco solo alcuni di quelli attualmente disponibili in Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

A partire da marzo 2015, [Microsoft Application Insights per Visual Studio Online](https://azure.microsoft.com/en-us/documentation/articles/app-insights-get-started/) non è ancora rilasciati, ma è disponibile in anteprima per provare. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) include anche funzionalità di monitoraggio.

Verrà esaminato rapidamente impostazione New Relic per mostrare come semplice può essere possibile utilizzare un sistema di telemetria.

Nel portale di gestione di Azure, effettuare l'iscrizione per il servizio. Fare clic su **New**, quindi fare clic su **archivio**. Il **scegliere un componente aggiuntivo** viene visualizzata la finestra di dialogo. Scorrere verso il basso e fare clic su **New Relic**.

![Scegliere un componente aggiuntivo](monitoring-and-telemetry/_static/image1.png)

Fare clic sulla freccia a destra e scegliere il livello di servizio desiderato. Per questa dimostrazione si userà il livello gratuito.

![Componente aggiuntivo di personalizzazione](monitoring-and-telemetry/_static/image2.png)

Fare clic sulla freccia destra e conferma "acquisto" New Relic ora visualizzata come un componente aggiuntivo nel portale.

![Acquisto di revisione](monitoring-and-telemetry/_static/image3.png)

![Nuovo componente aggiuntivo Relic nel portale di gestione](monitoring-and-telemetry/_static/image4.png)

Fare clic su **le informazioni di connessione**e copiare il codice di licenza.

![Informazioni di connessione](monitoring-and-telemetry/_static/image5.png)

Passare al **configura** scheda per impostare l'app web nel portale di **il monitoraggio delle prestazioni** a **componente aggiuntivo**e impostare il **componente aggiuntivo scegliere** elenco a discesa per **New Relic**. Quindi fare clic su **salvare**.

![New Relic nella scheda Configura](monitoring-and-telemetry/_static/image6.png)

In Visual Studio, installare il pacchetto NuGet Relic nuova nell'app.

![Analitica sviluppatore nella scheda Configura](monitoring-and-telemetry/_static/image7.png)

Distribuire l'app in Azure e iniziare a usarlo. Creare alcune Correggi attività per fornire alcune attività per New Relic per il monitoraggio.

Quindi tornare al **New Relic** nella pagina di **componenti aggiuntivi** scheda del portale e fare clic su **Gestisci**. Il portale Invia a portale di gestione di New Relic, con accesso single sign-on per l'autenticazione non è necessario immettere nuovamente le credenziali. La pagina Panoramica viene visualizzata un'ampia gamma di statistiche sulle prestazioni. (Fare clic sull'immagine per vedere le dimensioni della pagina panoramica completa).

[![Nuova scheda Monitoraggio Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Di seguito sono riportate alcune delle statistiche è possibile vedere:

- Tempo medio di risposta in momenti diversi del giorno.

    ![Tempo di risposta](monitoring-and-telemetry/_static/image10.png)
- Velocità effettiva (in numero di richieste al minuto) in momenti diversi del giorno.

    ![Velocità effettiva](monitoring-and-telemetry/_static/image11.png)
- Tempo CPU del server impiegato per la gestione delle richieste HTTP diverse.

    ![Tempi di transazione Web](monitoring-and-telemetry/_static/image12.png)
- Tempo CPU trascorso in diverse parti del codice dell'applicazione:

    ![Dettagli traccia](monitoring-and-telemetry/_static/image13.png)
- Statistiche sulle prestazioni cronologiche.

    ![Cronologia delle prestazioni](monitoring-and-telemetry/_static/image14.png)
- Il servizio è stato chiamate ai servizi esterni, ad esempio il servizio Blob e le statistiche sull'affidabile e reattiva.

    ![Servizi esterni](monitoring-and-telemetry/_static/image15.png)

    ![Servizi esterni](monitoring-and-telemetry/_static/image16.png)

    ![Servizio esterno](monitoring-and-telemetry/_static/image17.png)
- Informazioni sulla posizione nel mondo o nell'app web Usa il traffico di provenienza.

    ![Geografia](monitoring-and-telemetry/_static/image18.png)

È inoltre possibile impostare report ed eventi. Ad esempio, è possibile ad esempio un avvio di visualizzare gli errori, inviare un messaggio di posta elettronica personale di supporto di avviso per il problema.

![Report](monitoring-and-telemetry/_static/image19.png)

New Relic è solo un esempio di un sistema di telemetria. è possibile ottenere tutte queste da altri servizi. Il vantaggio del cloud è che, senza dover scrivere alcun codice e per minime o nessuna expense è improvvisamente possibile ottenere molte più informazioni su come viene usata l'applicazione e ciò che effettivamente si verificano i clienti.

<a id="log"></a>
## <a name="log-for-insight"></a>Log per informazioni dettagliate

Un pacchetto di dati di telemetria è un buon punto di partenza, ma è comunque necessario instrumentare il proprio codice. Il servizio di telemetria indica quando si è verificato un problema e indica ciò che si verificano i clienti, ma potrebbe non produrre molta approfondite andamento nel codice.

Non si desidera disporre in remoto a un server di produzione per visualizzare tutte le app. Che potrebbero essere pratico quando si dispone di un server, ma cosa accade se è stato ridotto a centinaia di server e non si conoscono quali in remoto in cui è necessario? La registrazione deve fornire informazioni sufficienti che è possibile utilizzare remoto nel server di produzione per analizzare ed eseguire il debug dei problemi. Deve essere registrazione informazioni sufficienti in modo che è possibile isolare problemi esclusivamente tramite i registri.

### <a name="log-in-production"></a>Log nell'ambiente di produzione

Molti utenti attivare la traccia nell'ambiente di produzione solo quando si è verificato un problema e che si desidera eseguire il debug. Questo può introdurre un ritardo significativo tra il momento in cui che si è conoscenza di un problema e l'ora in cui che si ottiene informazioni sulla risoluzione dei problemi utili. E le informazioni ottenute potrebbero non essere utile per gli errori intermittenti.

È consigliabile nell'ambiente cloud in cui l'archiviazione è economica è lasciare sempre l'accesso nell'ambiente di produzione. In questo modo, quando si verificano errori già sono registrati e si dispone di dati cronologici che consentono analizzare i problemi che sviluppano nel tempo o si verificano regolarmente in momenti diversi. È possibile automatizzare un processo di eliminazione per eliminare i log precedenti, ma si noterà che è più costoso impostare tale processo rispetto al mantenimento dei registri.

Le spese correlate all'aggiunta di registrazione sono piuttosto semplice rispetto alla quantità di tempo e denaro, che è possibile salvare tutte le informazioni necessarie già disponibili quando si verificano problemi con risoluzione dei problemi. Quindi quando un utente, indica che avevano un errore casuale momento circa 8:00 notte precedente, ma non ricordare l'errore, è possibile trovare facilmente out Qual era il problema.

Per meno di $4 un mese in cui è possibile mantenere scorte 50 gigabyte dei log e l'impatto sulle prestazioni di registrazione è piuttosto semplice, purché si mantenere un aspetto importante: per evitare colli di bottiglia delle prestazioni, verificare che la libreria di registrazione è asincrona.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Differenziare i log che informano dai registri che richiedono un'azione

I registri sono concepiti per informare (è necessario sapere qualcosa) o ACT (è necessario utilizzare le operazioni da eseguire). Prestare attenzione solo scrivere i log di ACT per problemi che richiedono effettivamente una persona o un processo automatizzato per eseguire azioni. I log di ACT troppi creerà il rumore, che richiedono troppo lavoro per esplorare in tutti i problemi di originale. Evitando se i log di ACT verrà automaticamente avviato un'azione, ad esempio l'invio di posta elettronica al personale di supporto, consentendo di essere attivata da un singolo problema migliaia di tali azioni.

Traccia System. Diagnostics .NET, i registri possono essere assegnati come livello di errore, avviso, informazioni e Debug/Verbose. È possibile differenziare ACT dai registri INFORM riservare il livello di errore per i log di ACT e utilizzando i livelli inferiori per informare i registri.

![Livelli di registrazione](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurare i livelli di registrazione in fase di esecuzione

Sebbene sia utile disporre di accesso sempre nell'ambiente di produzione, un'altra procedura consigliata consiste nell'implementare un framework di registrazione che consente di modificare in fase di esecuzione il livello di dettaglio che ci si connette, senza ridistribuire o il riavvio dell'applicazione. Ad esempio quando si utilizza la funzionalità di traccia in `System.Diagnostics` è possibile creare l'errore, avviso, informazioni e Debug/dettagliato vengono registrati. Si consiglia di errore, avviso, sempre l'accesso e registra informazioni nell'ambiente di produzione, è opportuno essere in grado di aggiungere in modo dinamico la registrazione di Debug/dettagliato per la risoluzione dei problemi nel caso per caso.

App Web nel servizio App di Azure con un supporto predefinito per la scrittura `System.Diagnostics` i log per il file system, archiviazione tabelle o nell'archiviazione Blob. È possibile selezionare diversi livelli di registrazione per ogni destinazione di archiviazione e, è possibile modificare il livello di registrazione in tempo reale senza riavviare l'applicazione. Il supporto di archiviazione Blob, è facile eseguire [HDInsight](https://docs.microsoft.com/azure/hdinsight/) processi di analisi nel log di applicazione, poiché HDInsight sia in grado di lavorare direttamente con archiviazione Blob.

### <a name="log-exceptions"></a>Eccezioni di log

Non è sufficiente inserire *eccezione. ToString ()* nel codice di registrazione. Che esclude le informazioni contestuali. Nel caso di errori SQL, lascia il numero di errore SQL. Per tutte le eccezioni, includono informazioni sul contesto, l'eccezione stessa e le eccezioni interne per assicurarsi che si desidera fornire tutti gli elementi che saranno necessari per la risoluzione dei problemi. Informazioni sul contesto, ad esempio, potrebbe includere il nome del server, un identificatore di transazione e un nome utente (ma non la password o alcun dato segreto!).

Se si utilizzano ogni sviluppatore deve nel modo giusto con la registrazione di eccezione, alcuni di essi non. Per assicurarsi che si ottiene il diritto modo ogni ora, generare eccezioni in un'interfaccia logger: passare l'oggetto eccezione per la classe logger e registrare correttamente i dati di eccezione nella classe logger.

### <a name="log-calls-to-services"></a>Registro chiamate ai servizi

Si consiglia di scrivere un log ogni volta che l'app chiama un servizio, se si tratta di un database o un'API REST o qualsiasi altro servizio esterno. Includere nei file registro non solo un'indicazione di esito positivo o negativo ma il tempo impiegato ogni richiesta. Nell'ambiente cloud si noterà spesso problemi relativi ai rallentamenti anziché interruzioni completate. Un elemento che accetta in genere di 10 millisecondi improvvisamente potrebbe avviare l'esecuzione di un secondo. Quando un utente, indica che l'app è lenta, che si desidera essere in grado di analizzare in New Relic o qualsiasi servizio di telemetria aver e convalidare l'esperienza e si desidera essere in grado di analizzare sono file di log per approfondire i dettagli del motivo per cui è lenta.

### <a name="use-an-ilogger-interface"></a>Utilizzare un'interfaccia ILogger

Che cos'è consigliabile eseguire quando si crea un'applicazione di produzione consiste nel creare una semplice *ILogger* interfaccia e utilizzare alcuni metodi in essa contenuti. Questo rende facile modificare l'implementazione di registrazione in un secondo momento e non deve eseguire tutto il codice per eseguire l'operazione. È possibile utilizzare il `System.Diagnostics.Trace` classe nell'intera app Correggi, ma invece viene utilizzato dietro le quinte in una classe di registrazione che implementa *ILogger*, e si rende *ILogger* chiamate in tutto l'app.

In questo modo, se si desidera effettuare la registrazione più dettagliato, è possibile sostituire [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) con qualsiasi meccanismo di registrazione desiderato. Ad esempio, man mano che aumenta l'app è possibile decidere che si desidera utilizzare un pacchetto di registrazione più completo, ad esempio [NLog](http://nlog-project.org/) o [blocco applicazione per la registrazione della libreria Enterprise](https://msdn.microsoft.com/en-us/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) è un altro framework di registrazione comuni ma non esegue la registrazione asincrona.)

Uno dei possibili motivi per utilizzare un framework come NLog è quello di facilitare suddividere l'output di registrazione in archivi dati con volumi elevati e di alto valore separato. Che consente di archiviare in modo efficiente volumi elevati di dati di informare che non è necessario eseguire query veloce, mantenendo l'accesso rapido ai dati di ACT.

### <a name="semantic-logging"></a>Registrazione semantica

Per eseguire la registrazione in grado di produrre più utili informazioni di diagnostica in modo relativamente nuovo, vedere [Enterprise Library semantica registrazione applicazione blocco (sezione)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Usa sonoro [Event Tracing for Windows](https://msdn.microsoft.com/en-us/library/windows/desktop/bb968803.aspx) (ETW) e [EventSource](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracing.eventsource.aspx) supporto in .NET 4.5 che consente di creare più log delle eccezioni strutturato e query. Definire un metodo diverso per ogni tipo di evento che si accede, che consente di personalizzare le informazioni che si scrive. Ad esempio, per registrare un errore di Database SQL è possibile chiamare un `LogSQLDatabaseError` metodo. Per questo tipo di eccezione, si conosce che un'informazione chiave è il numero di errore, pertanto è possibile includere un parametro di numero di errore nella firma del metodo e registrare il numero di errore come un campo del record di log che è scrivere separato. Poiché il numero è un campo separato è possibile in modo più semplice e affidabile ottenere report basati sui numeri di errore SQL che è possibile se si sono stati concatenazione solo il numero di errore in una stringa di messaggio.

## <a name="logging-in-the-fix-it-app"></a>Registrazione di correzione, app

### <a name="the-ilogger-interface"></a>L'interfaccia ILogger

Ecco il *ILogger* interfaccia nell'app Correggi.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Questi metodi consentono di scrivere i log agli stessi quattro livelli supportati da *System. Diagnostics*. I metodi TraceApi sono per la registrazione delle chiamate del servizio esterno con informazioni sulla latenza. È anche possibile aggiungere un set di metodi per il livello di Debug/Verbose.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>L'implementazione di Logger dell'interfaccia ILogger

L'implementazione dell'interfaccia è un'operazione estremamente semplice. In pratica solo chiama degli standard *System. Diagnostics* metodi. Il frammento di codice seguente illustra tutte e tre i metodi di informazioni e uno degli altri.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Chiamata dei metodi di ILogger

Ogni volta codice nell'app Correggi rileva un'eccezione, chiama un *ILogger* metodo per registrare i dettagli dell'eccezione. E ogni volta che effettua una chiamata al database, il servizio Blob o un'API REST, avvia un cronometro prima della chiamata, arresta il cronometro quando viene restituito, il servizio e registra il tempo trascorso insieme alle informazioni sull'esito positivo o negativo.

Si noti che il messaggio di log include il nome della classe e il nome del metodo. È consigliabile assicurarsi che i messaggi di log di identificare quale parte del codice dell'applicazione ha scritto li.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

A questo punto per ogni volta che l'app Correggi ha effettuato una chiamata al Database SQL, è possibile visualizzare la chiamata, il metodo che lo hanno chiamato, ed esattamente il tempo richiesto.

![Query di Database SQL nei log](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Se visualizzano i passare attraverso i registri, si noterà che il tempo delle chiamate di database è variabile. Che informazioni potrebbero essere utili: poiché l'applicazione registra tutto ciò è possibile analizzare le tendenze cronologiche in modalità esegue il servizio di database nel corso del tempo. Ad esempio, un servizio potrebbe essere rapido la maggior parte dei casi ma potrebbero non riuscire a richieste o risposte potrebbero rallentare in determinati orari del giorno.

È possibile eseguire la stessa operazione per il servizio Blob: per ogni volta che l'applicazione carica un nuovo file, è un log e si può vedere esattamente il tempo impiegato per caricare ogni file.

![Log di caricamento BLOB](monitoring-and-telemetry/_static/image23.png)

Si tratta di un paio altre righe di codice per scrivere ogni volta che si chiama un servizio e l'ora ogni volta che un utente è indicato che si è verificato un problema, si conosce esattamente il problema animale, se è verificato un errore, o anche se è stato appena esecuzione lenta. È possibile individuare l'origine del problema senza dover remoto in un server o attivare la registrazione dopo l'errore si verifica e speriamo di crearlo nuovamente.

## <a name="dependency-injection-in-the-fix-it-app"></a>Inserimento di dipendenze di correzione, app

Ci si potrebbe chiedere come costruttore repository nell'esempio illustrato in precedenza Ottiene il logger di implementazione dell'interfaccia:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Per associare l'interfaccia per l'implementazione, l'app Usa [inserimento di dipendenze](http://en.wikipedia.org/wiki/Dependency_injection)(DI) con [AutoFac](http://autofac.org/). Il modello DI consente di utilizzare un oggetto basato su un'interfaccia in numerose posizioni in tutto il codice solo specificare, in un'unica posizione, l'implementazione che viene utilizzato quando viene creata un'istanza dell'interfaccia. Questo rende più semplice modificare l'implementazione: potrebbe ad esempio, si desidera sostituire il logger di System. Diagnostics con un logger NLog. In alternativa, per i test automatizzati, potresti sostituire una versione fittizia del logger.

L'applicazione Correggi utilizza DI tutti i repository e tutti i controller. I costruttori delle classi controller ottenere un *ITaskRepository* interfaccia allo stesso modo, il repository Ottiene un'interfaccia di logger:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

L'app Usa la libreria DI AutoFac per fornire automaticamente *TaskRepository* e *Logger* istanze per questi costruttori.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Questo codice indica fondamentalmente che necessita di un costruttore in qualsiasi punto un *ILogger* l'interfaccia, passare in un'istanza del *Logger* (classe), e ogni volta che è necessario un *IFixItTaskRepository*l'interfaccia, passare a un'istanza di *FixItTaskRepository* classe.

[AutoFac](http://autofac.org/) è uno dei molti framework di inserimento di dipendenza che è possibile utilizzare. È un diffuso altro [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), che è consigliata e supportata da Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Supporto per la registrazione incorporata in Azure

Azure supporta i seguenti tipi di [registrazione per le app Web in Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Funzionalità di traccia System. Diagnostics (è possibile attivare e disattivare e impostare i livelli in tempo reale senza dover riavviare il sito).
- Eventi di Windows.
- Log di IIS (HTTP/FREB).

Azure supporta i seguenti tipi di [registrazione in servizi Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Funzionalità di traccia System. Diagnostics.
- Contatori delle prestazioni.
- Eventi di Windows.
- Log di IIS (HTTP/FREB).
- Monitoraggio directory personalizzata.

L'app Correggi utilizza analisi System. Diagnostics. È sufficiente eseguire per abilitare la registrazione in un'applicazione web di System. Diagnostics è capovolge un'opzione nel portale o chiamare l'API REST. Nel portale, fare clic su di **configurazione** scheda per il sito e scorrere verso il basso per visualizzare il **Application Diagnostics** sezione. È possibile attivare o disattivare la registrazione e selezionare il livello di registrazione desiderato. È possibile avere Azure scrivere i log nel file System o in un account di archiviazione.

![Diagnostica di App e di diagnostica del sito nella scheda Configura](monitoring-and-telemetry/_static/image24.png)

Dopo aver abilitato la registrazione in Azure, è possibile visualizzare i log disponibili nella finestra di Output di Visual Studio al momento della creazione.

![Menu di log di streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu di log di streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

È anche possibile scrittura all'account di archiviazione dei log e di visualizzazione con uno strumento che è possibile accedere al servizio di tabella di archiviazione di Azure, ad esempio **Esplora Server** in Visual Studio o [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Log in Esplora Server](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Riepilogo

È molto semplice implementare un sistema di telemetria della casella, instrumentare il proprio codice di registrazione e configurare la registrazione in Azure. E, nel caso di problemi di produzione, la combinazione di un sistema di telemetria e i log personalizzati consentono di risolvere rapidamente i problemi prima che diventino problemi principali per i clienti.

Nel [capitolo successivo](transient-fault-handling.md) verrà esaminato come gestire gli errori temporanei in modo che non diventino problemi di produzione da analizzare.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse.

Documentazione principalmente sulla telemetria di:

- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Vedere strumentazione e dati di telemetria sulla, istruzioni di controllo del servizio, il monitoraggio dell'integrità dell'Endpoint criterio e il criterio di riconfigurazione di Runtime.
- [Centesimi di compressione nel Cloud: abilitazione di nuove Relic monitoraggio delle prestazioni nei siti Web di Azure](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx). White paper Mark Simms e Michael Thomassy. Vedere la sezione di dati di telemetria e diagnostica.
- [Sviluppo di prossima generazione con Application Insights](https://msdn.microsoft.com/en-us/magazine/dn683794.aspx). Articolo di MSDN Magazine.

Documentazione principalmente sulla registrazione:

- [Blocco applicazione di semantica di registrazione (sezione)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie presenta nel caso di registrazione semantica con sonoro.
- [Creazione di log significativo e strutturati con la registrazione semantica](https://channel9.msdn.com/Events/Build/2013/3-336). (Video) Julian Dominguez presenta nel caso di registrazione semantica con sonoro.
- [EF6 Registrazione SQL – parte 1: registrazione semplice](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers viene illustrato come registrare query eseguita da Entity Framework 6, Entity Framework.
- [Resilienza di connessione e comando di intercettazione con Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quarto in una serie di esercitazioni nove parti, viene illustrato come utilizzare la funzionalità di intercettazione di Entity Framework 6 comando per registrare i comandi SQL inviati al database da Entity Framework.
- [Migliorare la registrazione tramite c# attributi informativi sul chiamante 5.0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Come registrare il nome del metodo chiamante in modo semplice senza impostare come hardcoded, in valori letterali o tramite reflection eseguire questa operazione manualmente.

Documentazione principalmente sulla risoluzione dei problemi:

- [Risoluzione dei problemi di Azure &amp; debug blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools – la diagnostica utilità utilizzati dal Team di supporto per sviluppatori di Azure](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Introduce e fornisce un collegamento di download per uno strumento utilizzato in una macchina virtuale di Azure per scaricare ed eseguire un'ampia gamma di strumenti di diagnostica e monitoraggio. È utile quando è necessario diagnosticare un problema per una determinata macchina virtuale.
- [Risolvere i problemi relativi a un'app web nel servizio App di Azure con Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Un'esercitazione dettagliata per iniziare a usare System. Diagnostics traccia e debug remoto.

Video

- [Operatore alternativo: Compilazione di servizi Cloud resilienti e scalabili](https://channel9.msdn.com/Series/FailSafe). Serie di nove parti Ulrich Homann, Marc Mercuri e Mark Simms. Presenta i concetti generali e i principi architetturali in modo molto accessibile e interessano con storie ricavate dall'esperienza di Microsoft Team (CAT, Customer Advisory) con i clienti effettivi. Episodi 4 e 9 sono sul monitoraggio e dati di telemetria. Episodio 9 include una panoramica di monitoraggio dei servizi MetricsHub, AppDynamics, New Relic e PagerDuty.
- [Compilazione Big: Apprese dai clienti di Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms comunica sulla progettazione per errore e la strumentazione di tutti gli elementi. Simile alla serie di operatore alternativo ma consente di spostarsi in dettaglio procedure.

Nell'esempio di codice:

- [Sviluppo di servizi in Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio creata con il Team di consulenza clienti di Microsoft Azure. Viene illustrato sia dati di telemetria e procedure consigliate di registrazione, come descritto negli articoli seguenti. L'esempio implementa la registrazione delle applicazioni tramite [NLog](http://nlog-project.org/). Per la documentazione correlata, vedere il [serie di quattro articoli wiki di TechNet sui dati di telemetria e la registrazione](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

>[!div class="step-by-step"]
[Precedente](design-to-survive-failures.md)
[Successivo](transient-fault-handling.md)
