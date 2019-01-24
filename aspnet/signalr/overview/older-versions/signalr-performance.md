---
uid: signalr/overview/older-versions/signalr-performance
title: Prestazioni di SignalR (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: Prestazioni di SignalR
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4158cb055088f3da752020e577007ffe80856b60
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837728"
---
<a name="signalr-performance-signalr-1x"></a>SignalR Performance (SignalR 1.x)
====================
da [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene descritto come progettare per misurare e migliorare le prestazioni in un'applicazione di SignalR.


Per una presentazione recenti sulle prestazioni di SignalR e scalabilità, vedere [scala Web in tempo reale con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Considerazioni di progettazione](#design)
- [Ottimizzazione del server di SignalR per le prestazioni](#tuning)
- [Risoluzione dei problemi di prestazioni](#troubleshooting)
- [Usando i contatori delle prestazioni di SignalR](#perfcounters)
- [Uso di altri contatori delle prestazioni](#othercounters)
- [Altre risorse](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Considerazioni di progettazione

In questa sezione vengono descritti i modelli che possono essere implementati durante la progettazione di un'applicazione di SignalR, per garantire che le prestazioni non sia in corso ostacolata generando traffico di rete superfluo.

### <a name="throttling-message-frequency"></a>La limitazione della frequenza di messaggio

Anche in un'applicazione che invia i messaggi con una frequenza elevata (ad esempio un'applicazione di gioco in tempo reale), la maggior parte delle applicazioni non necessario inviare più messaggi al secondo. Per ridurre la quantità di traffico che ogni client che genera l'errore, un ciclo di messaggi può essere implementato che le code e invia i messaggi spesso non più di una tariffa fissa (vale a dire, fino a un certo numero di messaggi verranno inviati al secondo, se sono presenti messaggi in quel momento in tervallo invio). Per un'applicazione di esempio che limita i messaggi a una velocità determinata (da client e server), vedere [messaggistica ad alta frequenza con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Riduzione delle dimensioni dei messaggi

È possibile ridurre le dimensioni di un messaggio SignalR riducendo le dimensioni degli oggetti serializzati. Nel codice server, se si sta inviando un oggetto che contiene le proprietà che non devono essere trasmessi, impedire che tali proprietà viene serializzata utilizzando il `JsonIgnore` attributo. I nomi delle proprietà vengono archiviati anche nel messaggio. i nomi delle proprietà possono essere abbreviati utilizzando il `JsonProperty` attributo. Esempio di codice seguente viene illustrato come escludere una proprietà che vengano inviati al client e come abbreviare i nomi delle proprietà:

**Codice del server .NET che dimostra l'attributo JsonIgnore per escludere dati che vengano inviati al client e l'attributo JsonProperty per ridurre la dimensione del messaggio**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Per mantenere la leggibilità / manutenibilità del codice client, i nomi delle proprietà abbreviato può essere rimappato a Converto nomi dopo viene ricevuto il messaggio. Esempio di codice seguente viene illustrato uno dei possibili modi di modifica del mapping di nomi abbreviati per quelle più lunghe, definendo un contratto di messaggio (mapping) e l'uso di `reMap` funzione da applicare il contratto per la classe messaggio ottimizzata:

**Codice JavaScript lato client che esegue un nuovo mapping abbreviato i nomi delle proprietà ai nomi leggibili**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

I nomi possono essere abbreviati nei messaggi dal client al server, anche con lo stesso metodo.

Ridurre il footprint di memoria (vale a dire, la quantità di memoria usata per il messaggio) del messaggio di oggetto può anche migliorare le prestazioni. Ad esempio, se la gamma completa di un `int` non è necessaria, una `short` o `byte` è invece possibile usare.

Poiché i messaggi vengono archiviati nel bus di messaggi in memoria del server, riducendo le dimensioni dei messaggi può anche risolvere i problemi di memoria di server.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Ottimizzazione del server di SignalR per le prestazioni

Le impostazioni di configurazione seguente sono utilizzabile per ottimizzare il server per migliorare le prestazioni in un'applicazione di SignalR. Per informazioni generali su come migliorare le prestazioni in un'applicazione ASP.NET, vedere [miglioramento delle prestazioni ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**Impostazioni di configurazione di SignalR**

- **DefaultMessageBufferSize**: Per impostazione predefinita, SignalR mantiene 1000 messaggi in memoria per ogni hub per ogni connessione. Se vengono utilizzati messaggi di grandi dimensioni, ciò può creare problemi di memoria che possono essere mitigati mediante la riduzione di questo valore. Questa impostazione può essere impostata `Application_Start` gestore dell'evento in un'applicazione ASP.NET o nel `Configuration` metodo di una classe di avvio OWIN in un'applicazione self-hosted. L'esempio seguente viene illustrato come ridurre questo valore per ridurre il footprint di memoria dell'applicazione per ridurre la quantità di memoria del server usata:

    **Codice server .NET in Global. asax per la riduzione delle dimensioni del buffer di messaggio predefinita**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Impostazioni di configurazione di IIS**

- **Numero massimo di richieste simultanee per ogni applicazione**: L'aumento del numero di IIS simultanee richieste aumenterà le risorse server disponibili per soddisfare le richieste. Il valore predefinito è 5000; Per aumentare questa impostazione, eseguire i comandi seguenti in un prompt dei comandi con privilegi elevati:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**Impostazioni di configurazione ASP.NET**

In questa sezione include le impostazioni di configurazione che possono essere impostate nella `aspnet.config` file. Questo file si trova in uno dei due percorsi, a seconda della piattaforma:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Impostazioni di ASP.NET che possono migliorare le prestazioni di SignalR includono quanto segue:

- **Numero massimo di richieste simultaneo per CPU**: Aumentare questa impostazione può ridurre i colli di bottiglia delle prestazioni. Per aumentare questa impostazione, aggiungere la seguente impostazione di configurazione per il `aspnet.config` file:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limite coda richieste**: Quando il numero totale di connessioni supera le `maxConcurrentRequestsPerCPU` impostazione, ASP.NET inizierà a limitazione delle richieste tramite una coda. Per aumentare le dimensioni della coda, è possibile aumentare il `requestQueueLimit` impostazione. A tale scopo, aggiungere la seguente impostazione di configurazione per il `processModel` nodo `config/machine.config` (anziché `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Risoluzione dei problemi di prestazioni

In questa sezione vengono descritti i modi per trovare i colli di bottiglia nell'applicazione.

### <a name="verifying-that-websocket-is-being-used"></a>Verifica che venga usato WebSocket

Sebbene SignalR è possibile usare un'ampia gamma di trasporti per le comunicazioni tra client e server, WebSocket presenta un vantaggio significativo delle prestazioni e deve essere utilizzata se il client e il server lo supportano. Per determinare se il client e il server soddisfa i requisiti per WebSocket, vedere [trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports). Per determinare quali il trasporto utilizzato nell'applicazione, è possibile usare gli strumenti di sviluppo del browser ed esaminare i registri per verificare quali il trasporto viene usato per la connessione. Per informazioni sull'uso degli strumenti di sviluppo del browser in Internet Explorer e Chrome, vedere [trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Usando i contatori delle prestazioni di SignalR

Questa sezione descrive come abilitare e usare contatori delle prestazioni di SignalR, disponibili nel `Microsoft.AspNet.SignalR.Utils` pacchetto.

### <a name="installing-signalrexe"></a>Installazione signalr.exe

Contatori delle prestazioni possono essere aggiunti al server usando un'utilità denominata SignalR.exe. Per installare questa utilità, seguire questa procedura:

1. In Visual Studio, selezionare **strumenti di** > **Gestione pacchetti NuGet** > **Gestisci pacchetti NuGet per soluzione**
2. Cercare **signalr.utils**e scegliere Installa.

    ![](signalr-performance/_static/image1.png)
3. Accettare il contratto di licenza per installare il pacchetto.
4. Sarà installato SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Installazione dei contatori delle prestazioni con SignalR.exe

Per installare i contatori delle prestazioni di SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il parametro seguente:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Per rimuovere i contatori delle prestazioni di SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il parametro seguente:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Contatori delle prestazioni di SignalR

Il pacchetto di utilità installa i contatori delle prestazioni seguenti. I contatori "Totale" misurano il numero di eventi dopo l'ultimo pool di applicazioni o server riavviare.

**Metriche relative alla connessione**

Le metriche seguenti misurano gli eventi di durata connessione che si verificano. Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Connessioni connesse**
- **Connessioni riconnesse**
- **Le connessioni Disonnected**
- **Connessioni correnti**

**Metrica messaggio**

Le seguenti metriche misurano il traffico di messaggi generato da SignalR.

- **Totale messaggi ricevuti connessione**
- **Totale inviati messaggi di connessione**
- **Connessione dei messaggi ricevuti/Sec**
- **Connessione dei messaggi inviati/Sec**

**Metriche del bus di messaggi**

Le seguenti metriche misurano il traffico attraverso il bus di messaggi SignalR interno, la coda in cui vengono inseriti tutti i messaggi SignalR in ingresso e in uscita. È un messaggio **Published** quando viene inviato o trasmettere. Oggetto **sottoscrittore** in questo contesto è una sottoscrizione del bus di messaggi, questo deve essere uguale al numero di client più il server stesso. Un' **ruolo di lavoro allocati** è un componente che invia i dati per le connessioni attive; un **ruolo di lavoro occupati** è quello che sta inviando attivamente un messaggio.

- **Totale ricevuti messaggi di Bus di messaggi**
- **Messaggio Bus di messaggi ricevuti/Sec**
- **I messaggi del Bus di messaggi pubblicati totale**
- **Messaggio Bus di messaggi pubblicati/Sec**
- **Messaggio del Bus sottoscrittori corrente**
- **Totale di sottoscrizione del Bus di messaggi**
- **I server di sottoscrizione del Bus di messaggi/Sec**
- **Bus di messaggi allocati i ruoli di lavoro**
- **Messaggio del Bus thread di lavoro occupati**
- **Corrente di argomenti del Bus di messaggi**

**Metriche di errore**

Le metriche seguenti misurano gli errori generati dal traffico di messaggi SignalR. **Risoluzione dell'hub** si verificano errori quando un hub o un metodo dell'hub non può essere risolto. **Chiamata dell'hub** gli errori sono le eccezioni generate durante la chiamata di un metodo dell'hub. **Trasporto** gli errori di connessione generata un'eccezione durante una richiesta o risposta HTTP.

- **Errori: Tutti i totali**
- **Errori: All/Sec**
- **Errori: Totale di risoluzione dell'hub**
- **Errori: Risoluzione dell'hub al secondo**
- **Errori: Hub Invocation Total**
- **Errori: Chiamata dell'hub al secondo**
- **Errori: Totale trasporto**
- **Errori: Messaggi trasporto/Sec**

**Metrica di scalabilità orizzontale**

Le seguenti metriche misurano il traffico e gli errori generati dal provider di scalabilità orizzontale. Oggetto **Stream** in questo contesto è un'unità di scala utilizzata dal provider di scalabilità orizzontale; si tratta di una tabella se viene utilizzato SQL Server, un argomento se si usa il Bus di servizio e una sottoscrizione se Redis è usato. Per impostazione predefinita, viene usato un solo flusso, ma questo valore può essere aumentato mediante la configurazione su SQL Server e del Bus di servizio. Oggetto **Buffering** flusso è quello che è entrato in uno stato di errore; quando il flusso è in stato di errore, tutti i messaggi inviati la backplane non riuscirà finché il flusso non è più eventi di errore. Il **lunghezza coda di invio** è il numero di messaggi che sono stati registrati ma non ancora inviati.

- **Bus di messaggi con scalabilità orizzontale dei messaggi ricevuti/Sec**
- **Totale flussi con scalabilità orizzontale**
- **Scalabilità orizzontale trasmette aperto**
- **Memorizzazione nel buffer i flussi di scalabilità orizzontale**
- **Totale errori di scalabilità orizzontale**
- **Errori di scalabilità orizzontale/Sec**
- **Lunghezza coda di invii di scalabilità orizzontale**

Per altre informazioni su ciò che siano misurando questi contatori, vedere [SignalR Scaleout con il Bus di servizio di Azure](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Uso di altri contatori delle prestazioni

Contatori delle prestazioni seguenti possono rivelarsi utili per monitorare le prestazioni dell'applicazione.

**Memoria**

- Memoria CLR .NET # byte in tutti gli heap (per w3wp)

**ASP.NET 2.0**

- ASP.NET\Requests Current
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- Tempo processore Information\Processor

**TCP/IP**

- TCPv6/connessioni stabilite
- TCPv4/connessioni stabilite

**Servizio Web**

- Connessioni Web servizio web\connessioni correnti
- Connessioni Service\Maximum Web

**Threading**

- LocksAndThreads CLR .NET\# thread logici attuali
- .NET CLR LocksAnd thread\# thread fisici attuali

<a id="otherresources"></a>

## <a name="other-resources"></a>Altre risorse

Per altre informazioni sul monitoraggio e ottimizzazione delle prestazioni di ASP.NET, vedere gli argomenti seguenti:

- [Cenni preliminari sulle prestazioni di ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [ASP.NET Thread Usage on IIS 7.0, IIS 7.5 e IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; (impostazioni Web)](https://msdn.microsoft.com/library/dd560842.aspx)
