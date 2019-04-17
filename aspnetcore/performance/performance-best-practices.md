---
title: Procedure consigliate ASP.NET Core le prestazioni
author: mjrousos
description: Suggerimenti per migliorare le prestazioni nelle App ASP.NET Core ed evitare problemi comuni di prestazioni.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/13/2019
uid: performance/performance-best-practices
ms.openlocfilehash: 095db38cf3102f6e18930efdbbaeeb90dffad8af
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614448"
---
# <a name="aspnet-core-performance-best-practices"></a>Procedure consigliate ASP.NET Core le prestazioni

Da [Mike Rousos](https://github.com/mjrousos)

In questo argomento vengono fornite linee guida per le prestazioni le procedure consigliate con ASP.NET Core.

<a name="hot"></a>

In questo documento, un *percorso del codice critico* viene definito come un percorso di codice che viene chiamato frequentemente e in cui si è gran parte del tempo di esecuzione. I percorsi del codice critico limitano in genere app scalabilità e prestazioni.

## <a name="cache-aggressively"></a>Memorizzare nella cache in modo aggressivo

La memorizzazione nella cache viene trattata in diverse parti di questo documento. Per altre informazioni, vedere <xref:performance/caching/response>.

## <a name="avoid-blocking-calls"></a>Evitare di bloccare le chiamate

Le app ASP.NET Core devono essere progettate per elaborare simultaneamente un numero di richieste. Le API asincrone consentono un piccolo pool di thread per gestire migliaia di richieste simultanee da non in attesa di bloccare le chiamate. Anziché in attesa di completamento di un'attività sincrona a esecuzione prolungata, il thread può funzionare su un'altra richiesta.

Un problema di prestazioni comuni nelle App ASP.NET Core sta bloccando le chiamate che può essere asincrone. Molte chiamate di blocco sincrone causare [esaurimento delle risorse del Pool di Thread](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) e ridotto i tempi di risposta.

**No**:

* Bloccare l'esecuzione asincrona chiamando [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) oppure [Task. Result](/dotnet/api/system.threading.tasks.task-1.result).
* Acquisire i blocchi nei percorsi di codice comune. Le app ASP.NET Core sono più efficienti quando progettate per eseguire il codice in parallelo.

**Do**:

* Rendere [hot percorsi del codice](#hot) asincrona.
* Chiamare le API di operazioni a esecuzione prolungata e accesso ai dati in modo asincrono.
* Rendere controller/Razor azioni relative alla pagina asincrona. L'intero stack di chiamate è asincrona per trarre vantaggio dalla [async/await](/dotnet/csharp/programming-guide/concepts/async/) modelli.

Un profiler, ad esempio [PerfView](https://github.com/Microsoft/perfview), può essere utilizzato per trovare i thread spesso aggiunti al [Pool di Thread](/windows/desktop/procthread/thread-pool). Il `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` evento indica che un thread aggiunti al pool di thread. <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a>Ridurre al minimo le allocazioni di oggetti di grandi dimensioni

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core -->
Il [.NET Core garbage collector](/dotnet/standard/garbage-collection/) gestisce l'allocazione e rilascio di memoria automaticamente nelle App ASP.NET Core. Garbage collection automatica in genere significa che gli sviluppatori non dovranno preoccuparsi di come o quando viene liberata memoria. Tuttavia, la pulizia pulirà gli oggetti richiede tempo di CPU, in modo che gli sviluppatori dovrebbero ridurre al minimo di allocazione oggetti nel [hot percorsi del codice](#hot). Operazione di Garbage collection è molto dispendiosa in oggetti di grandi dimensioni (byte > 85 K). Oggetti di grandi dimensioni vengono archiviati nel [heap oggetti grandi](/dotnet/standard/garbage-collection/large-object-heap) e richiede una procedura completa (generazione 2) la pulizia di garbage collection. A differenza di generazione 0 e le raccolte di generazione 1, una raccolta di generazione 2 richiede una sospensione temporanea di esecuzione di app. Frequenti allocazione e deallocazione di oggetti di grandi dimensioni può causare prestazioni non coerente.

Indicazioni:

* **Eseguire operazioni** prendere in considerazione la memorizzazione nella cache di oggetti di grandi dimensioni usati di frequente. La memorizzazione nella cache di oggetti di grandi dimensioni impedisce le allocazioni costosa.
* **Effettuare** pool di buffer tramite un [ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1) per archiviare le matrici di grandi dimensioni.
* **Non li** allocare oggetti di grandi dimensioni molte e di breve durati nel [hot percorsi del codice](#hot).

Problemi di memoria, ad esempio precedente, possono essere diagnosticati esaminando statistiche di garbage collection (GC) nella [PerfView](https://github.com/Microsoft/perfview) ed esaminando i:

* Tempo del Garbage Collector pausa.
* Quale percentuale del tempo del processore viene impiegata in garbage collection.
* Quante operazioni di garbage collection sono generazione 0, 1 e 2.

Per altre informazioni, vedere [Garbage Collection e prestazioni](/dotnet/standard/garbage-collection/performance).

## <a name="optimize-data-access"></a>Ottimizzare l'accesso ai dati

Le interazioni con un archivio dati e altri servizi remoti sono spesso le parti più lente di un'app ASP.NET Core. La lettura e scrittura dei dati in modo efficiente è essenziale per assicurare prestazioni ottimali.

Indicazioni:

* **Eseguire operazioni** chiamare tutte le API di accesso dati in modo asincrono.
* **No** recuperare più dati del necessario. Scrivere query per restituire solo i dati che sono necessari per la richiesta HTTP corrente.
* **Eseguire operazioni** prendere in considerazione la memorizzazione nella cache spesso ottenuto accesso ai dati recuperati da un database o un servizio remoto, se i dati non aggiornati leggermente sono accettabili. A seconda dello scenario, usare una [MemoryCache](xref:performance/caching/memory) o una [DistributedCache](xref:performance/caching/distributed). Per altre informazioni, vedere <xref:performance/caching/response>.
* **Eseguire operazioni** ridurre al minimo i round trip di rete. L'obiettivo consiste nel recuperare i dati necessari in una singola chiamata anziché diverse chiamate.
* **Effettuare** usano [query senza registrazione](/ef/core/querying/tracking#no-tracking-queries) in Entity Framework Core quando si accede a dati per scopi di sola lettura. EF Core può restituire i risultati della query senza registrazione in modo più efficiente.
* **Effettuare** filtro e query di aggregazione LINQ (con `.Where`, `.Select`, o `.Sum` istruzioni, ad esempio) in modo che il filtro viene eseguito dal database.
* **Eseguire operazioni** prendere in considerazione che EF Core risolve alcuni operatori di query sul client, con conseguente esecuzione di query inefficiente. Per altre informazioni, vedere [problemi di prestazioni di valutazione Client](/ef/core/querying/client-eval#client-evaluation-performance-issues).
* **No** utilizzare query di proiezione per raccolte, che possono comportare l'esecuzione di "N + 1" query SQL. Per altre informazioni, vedere [ottimizzazione di sottoquery correlate](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).

Visualizzare [EF ad alte prestazioni](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) per approcci che possono migliorare le prestazioni nelle App a scalabilità elevata:

* [Pooling DbContext](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [Query compilate in modo esplicito](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

Si consiglia di misurare l'impatto dei precedenti approcci ad alte prestazioni prima di eseguire il commit della codebase. La complessità aggiuntiva delle query compilate non può giustificare il miglioramento delle prestazioni.

Query problemi possono essere rilevati esaminando il tempo impiegato per l'accesso ai dati con [Application Insights](/azure/application-insights/app-insights-overview) o con gli strumenti di profilatura. La maggior parte dei database anche dispongono di statistiche relative alle query eseguite di frequente.

## <a name="pool-http-connections-with-httpclientfactory"></a>Connessioni di pool HTTP con HttpClientFactory

Sebbene [HttpClient](/dotnet/api/system.net.http.httpclient) implementa il `IDisposable` interfaccia, è progettato per il riutilizzo. Chiuso `HttpClient` istanze lasciata aperta in socket di `TIME_WAIT` dello stato per un breve periodo di tempo. Se un percorso di codice che crea ed Elimina `HttpClient` oggetti viene spesso usato, l'app può esaurire socket disponibile. [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) è stato introdotto in ASP.NET Core 2.1 come una soluzione al problema. Gestisce le connessioni HTTP del pool di connessioni per ottimizzare le prestazioni e affidabilità.

Indicazioni:

* **Non li** creare ed eliminare risorse `HttpClient` istanze direttamente.
* **Effettuare** usano [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) recuperare `HttpClient` istanze. Per altre informazioni, vedere [HttpClientFactory uso per implementare le richieste HTTP resiliente](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).

## <a name="keep-common-code-paths-fast"></a>Mantenere comuni percorsi del codice veloci

Si vuole che tutto il codice da chiamare in modo rapido, spesso i percorsi del codice sono i più importanti per ottimizzare:

* I componenti middleware nella pipeline di elaborazione richieste dell'app, in particolare middleware eseguire nelle prime fasi della pipeline. Questi componenti hanno un notevole impatto sulle prestazioni.
* Codice che viene eseguito per ogni richiesta o più volte per ogni richiesta. Ad esempio, la registrazione personalizzata, gestori di autorizzazione o l'inizializzazione dei servizi temporanei.

Indicazioni:

* **No** usare i componenti middleware personalizzato con attività a esecuzione prolungata.
* **Effettuare** usare, ad esempio, strumenti di profilatura delle prestazioni [strumenti di diagnostica di Visual Studio](/visualstudio/profiling/profiling-feature-tour) o [PerfView](https://github.com/Microsoft/perfview)), per identificare [hot percorsi del codice](#hot).

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>Completare con esecuzione prolungata attività di fuori delle richieste HTTP

La maggior parte delle richieste a un'app ASP.NET Core possono essere gestite da un controller o un modello di pagina chiama i servizi necessari e restituire una risposta HTTP. Per alcune richieste che comportano attività con esecuzione prolungata, è preferibile rendere il processo di intera richiesta-risposta asincrono.

Indicazioni:

* **No** attendere il completamento come parte dell'elaborazione della richiesta HTTP comune delle attività con esecuzione prolungata.
* **Effettuare** considerare la gestione delle richieste a esecuzione prolungata con [servizi in background](xref:fundamentals/host/hosted-services) o all'esterno di processo con un [funzioni di Azure](/azure/azure-functions/). Completamento di lavoro out-of-process è particolarmente utile per le attività a elevato utilizzo della CPU.
* **Effettuare** usare le opzioni di comunicazione in tempo reale, ad esempio [SignalR](xref:signalr/introduction), per comunicare con i client in modo asincrono.

## <a name="minify-client-assets"></a>Minimizzare gli asset client

Le app ASP.NET Core con front-end complessi spesso servono molti JavaScript, CSS o i file di immagine. Possono migliorare le prestazioni delle richieste di caricamento iniziale:

* Creazione di bundle, che combina più file in un unico.
* Minimizzazione, che riduce le dimensioni dei file tramite la rimozione di spazi vuoti e commenti.

Indicazioni:

* **Effettuare** usare ASP.NET Core [supporto integrato](xref:client-side/bundling-and-minification) per la creazione di bundle e minimizzazione di risorse di client.
* **Effettuare** considerare altri strumenti di terze parti, ad esempio [Gulp](xref:client-side/using-gulp) oppure [Webpack](https://webpack.js.org/) per la gestione di asset client complessi.

## <a name="compress-responses"></a>Compressione delle risposte

 Riduzione delle dimensioni della risposta in genere aumenta la velocità di risposta di un'app, spesso notevolmente. Un modo per ridurre le dimensioni dei payload è per la compressione delle risposte di un'app. Per altre informazioni, vedere [compressione delle risposte](xref:performance/response-compression).

## <a name="use-the-latest-aspnet-core-release"></a>Usare la versione più recente di ASP.NET Core

Ogni nuova versione di ASP.NET Core include miglioramenti delle prestazioni. Le ottimizzazioni in .NET Core e ASP.NET Core significa che le versioni più recenti che in genere le versioni precedenti. Ad esempio, .NET Core 2.1 aggiunto il supporto per espressioni regolari compilate e apprezzato [ `Span<T>` ](https://msdn.microsoft.com/magazine/mt814808.aspx). Supporto di ASP.NET Core 2.2 aggiunto per HTTP/2. Se le prestazioni sono una priorità, prendere in considerazione l'aggiornamento alla versione corrente di ASP.NET Core.

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a>Ridurre al minimo le eccezioni

Le eccezioni devono essere rare. Generare e rilevare eccezioni è lento rispetto alla altri modelli di flusso di codice. Per questo motivo, le eccezioni non devono essere usate per controllare il flusso di programma normale.

Indicazioni:

* **Non li** Usa la generazione o il rilevamento delle eccezioni come mezzo per il flusso di programma normale, soprattutto nelle [hot percorsi del codice](#hot).
* **Eseguire operazioni** includere la logica nell'app per rilevare e gestire le condizioni che potrebbero causare un'eccezione.
* **Eseguire operazioni** generare o intercettare le eccezioni per le condizioni insolite o impreviste.

Strumenti di diagnostica di App, ad esempio Application Insights, possono contribuire a identificare le eccezioni più comuni in un'app che possono influire sulle prestazioni.
