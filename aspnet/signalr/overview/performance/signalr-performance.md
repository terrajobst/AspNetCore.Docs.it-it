---
uid: signalr/overview/performance/signalr-performance
title: Prestazioni di SignalR | Documenti Microsoft
author: pfletcher
description: Prestazioni di SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: dec2602e47fbcb838643a506a7e3feebda9d9c81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance"></a>Prestazioni di SignalR
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> In questo argomento viene descritto come progettare per misurare e migliorare le prestazioni in un'applicazione di SignalR.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


Per una presentazione recente sulle prestazioni di SignalR e scalabilità, vedere [scala Web in tempo reale con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Considerazioni sulla progettazione](#design)
- [Ottimizzazione delle prestazioni del server di SignalR](#tuning)
- [Risoluzione dei problemi di prestazioni](#troubleshooting)
- [Utilizzando i contatori delle prestazioni di SignalR](#perfcounters)
- [Con altri contatori di prestazioni](#othercounters)
- [Altre risorse](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Considerazioni sulla progettazione

In questa sezione descrive i modelli che possono essere implementate durante la progettazione di un'applicazione di SignalR, per garantire che le prestazioni non viene risentono generare traffico di rete non necessarie.

### <a name="throttling-message-frequency"></a>La limitazione della frequenza di messaggio

Anche in un'applicazione che invia messaggi a una frequenza elevata (ad esempio un'applicazione di giochi in tempo reale), la maggior parte delle applicazioni non necessario inviare più messaggi al secondo. Per ridurre la quantità di traffico che genera ciascun client, è possibile implementare un ciclo di messaggi che code e invia i messaggi non più di frequente un corrispettivo fisso (ovvero, fino a un determinato numero di messaggi verranno inviati al secondo, se sono presenti messaggi in quel momento in terval invio). Per un'applicazione di esempio che limita i messaggi per un determinato tasso (da client e server), vedere [in tempo reale ad alta frequenza con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Riduzione delle dimensioni di messaggio

È possibile ridurre le dimensioni di un messaggio SignalR riducendo le dimensioni degli oggetti serializzati. Nel codice server, se si invia un oggetto che contiene le proprietà che non devono essere trasmessi, evitare tali proprietà venga serializzato utilizzando il `JsonIgnore` attributo. I nomi delle proprietà vengono archiviati anche nel messaggio. i nomi delle proprietà possono essere abbreviati utilizzando il `JsonProperty` attributo. Esempio di codice seguente viene illustrato come escludere una proprietà vengano inviate al client e su come ridurre i nomi delle proprietà:

**Codice server .NET che illustra l'attributo JsonIgnore per escludere i dati vengano inviati al client e l'attributo JsonProperty per ridurre la dimensione del messaggio**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Per mantenere la leggibilità / manutenibilità nel codice client, i nomi di proprietà abbreviato possono essere rimappato a umane descrittivo nomi dopo aver ricevuto il messaggio. Esempio di codice riportato di seguito viene illustrato uno dei modi di modifica del mapping di nomi abbreviati per più lunghi, definendo un contratto di messaggio (mapping) e l'utilizzo di `reMap` funzione per applicare il contratto per la classe messaggio ottimizzata:

**Codice JavaScript sul lato client che esegue la abbreviato i nomi delle proprietà di nomi leggibili**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Nomi possono essere abbreviati in messaggi dal client al server, anche con lo stesso metodo.

Ridurre il footprint di memoria (ovvero, la quantità di memoria utilizzata per il messaggio) del messaggio di oggetto può inoltre migliorare le prestazioni. Ad esempio, se l'intervallo completo di un `int` non è necessaria una `short` o `byte` è invece possibile utilizzare.

Poiché i messaggi vengono archiviati nel bus di messaggi nella memoria del server, riducendo le dimensioni dei messaggi può anche risolvere i problemi di memoria server.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Ottimizzazione delle prestazioni del server di SignalR

Le impostazioni di configurazione seguente consente di ottimizzare il server per migliorare le prestazioni in un'applicazione di SignalR. Per informazioni generali su come migliorare le prestazioni in un'applicazione ASP.NET, vedere [miglioramento delle prestazioni di ASP.NET](https://msdn.microsoft.com/en-us/library/ff647787.aspx).

**Impostazioni di configurazione di SignalR**

- **DefaultMessageBufferSize**: per impostazione predefinita, SignalR mantiene 1000 messaggi in memoria per l'hub per ogni connessione. Se vengono utilizzati i messaggi di grandi dimensioni, questo può creare problemi di memoria che possono essere ridotta mediante la riduzione di questo valore. Questa impostazione può essere definita `Application_Start` gestore dell'evento in un'applicazione ASP.NET o nel `Configuration` metodo di una classe di avvio OWIN in un'applicazione indipendente. L'esempio seguente viene illustrato come ridurre questo valore per ridurre il footprint di memoria dell'applicazione per ridurre la quantità di memoria del server utilizzata:

    **Codice server .NET in Startup.cs per la relativa riduzione delle dimensioni predefinite del buffer del messaggio**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Impostazioni di configurazione di IIS**

- **Numero massimo di richieste simultanee per ogni applicazione**: aumentando il numero di simultanee IIS richieste aumenterà risorse server disponibili per gestire le richieste. Il valore predefinito è 5000; Per aumentare questa impostazione, eseguire i comandi seguenti in un prompt dei comandi con privilegi elevati:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: il numero massimo di richieste HTTP. sys che inserisce in coda per il pool di applicazioni. Quando la coda è piena, le nuove richieste ricevono una risposta 503 "Servizio non disponibile". Il valore predefinito è 1000.

    Ridurre la lunghezza della coda per il processo di lavoro nel pool di applicazioni che ospita l'applicazione verrà risparmiare risorse di memoria. Per ulteriori informazioni, vedere [gestione, ottimizzazione e la configurazione di pool di applicazioni](https://technet.microsoft.com/en-us/library/cc745955.aspx).

**Impostazioni di configurazione ASP.NET**

In questa sezione include le impostazioni di configurazione che possono essere impostate nella `aspnet.config` file. Questo file si trova in uno dei due percorsi, a seconda della piattaforma:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Impostazioni di ASP.NET che possono migliorare le prestazioni di SignalR includono quanto segue:

- **Numero massimo di richieste simultaneo per CPU**: aumentare questa impostazione può ridurre i colli di bottiglia delle prestazioni. Per aumentare questa impostazione, aggiungere la seguente impostazione di configurazione per il `aspnet.config` file:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limite coda richieste**: quando il numero totale di connessioni supera le `maxConcurrentRequestsPerCPU` impostazione, ASP.NET inizierà la limitazione delle richieste effettuate utilizzando una coda. Per aumentare le dimensioni della coda, è possibile aumentare la `requestQueueLimit` impostazione. A tale scopo, aggiungere la seguente impostazione di configurazione per il `processModel` nodo `config/machine.config` (anziché `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Risoluzione dei problemi di prestazioni

In questa sezione viene descritto come individuare eventuali colli di bottiglia nell'applicazione.

### <a name="verifying-that-websocket-is-being-used"></a>Verifica che sia in uso WebSocket

SignalR è possibile utilizzare una varietà di trasporti per la comunicazione tra client e server, WebSocket offre un vantaggio significativo delle prestazioni, mentre deve essere utilizzato se supporta client e server. Per determinare se il client e il server soddisfa i requisiti per WebSocket, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports). Per determinare il trasporto utilizzato nell'applicazione, è possibile utilizzare gli strumenti di sviluppo di browser e i registri per il trasporto è stata utilizzata per la connessione. Per informazioni sull'uso degli strumenti di sviluppo del browser in Internet Explorer e Chrome, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Utilizzando i contatori delle prestazioni di SignalR

In questa sezione viene descritto come abilitare e usare i contatori delle prestazioni di SignalR, vedere il `Microsoft.AspNet.SignalR.Utils` pacchetto.

### <a name="installing-signalrexe"></a>L'installazione signalr.exe

I contatori delle prestazioni possono essere aggiunti al server tramite l'utilità SignalR.exe. Per installare questa utilità, seguire questi passaggi:

1. Nell'applicazione Visual Studio, selezionare **strumenti**, **Gestione pacchetti libreria**, **Gestisci pacchetti NuGet per la soluzione...**
2. Cercare **signalr.utils**e scegliere Installa.

    ![](signalr-performance/_static/image1.png)
3. Accettare il contratto di licenza per installare il pacchetto.
4. Verrà installato SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>L'installazione di contatori delle prestazioni con SignalR.exe

Per installare i contatori delle prestazioni di SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il parametro seguente:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Per rimuovere i contatori delle prestazioni di SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il parametro seguente:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Contatori delle prestazioni di SignalR

Il pacchetto di utilità installa i contatori delle prestazioni seguenti. I contatori "Total" misurano il numero di eventi dopo l'ultimo pool di applicazioni o server riavviare.

**Metriche di connessione**

Le metriche seguenti misurano gli eventi di durata connessione che si verificano. Per ulteriori informazioni, vedere [comprensione e gestione degli eventi di durata connessione](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Connessioni**
- **Connessioni riconnesse**
- **Connessioni disconnesso**
- **Connessioni correnti**

**Metrica messaggio**

Le metriche seguenti misurano il traffico di messaggi generato da SignalR.

- **Totale messaggi ricevuti di connessione**
- **Totale messaggi connessione inviati**
- **Connessione messaggi ricevuti/Sec**
- **Connessione messaggi inviati al secondo**

**Metriche del bus messaggi**

Le metriche seguenti misurano il traffico tramite il bus di messaggi SignalR interno, la coda in cui vengono inseriti tutti i messaggi SignalR in ingresso e in uscita. Un messaggio è **pubblicato** quando viene inviato o trasmettere. Oggetto **sottoscrittore** in questo contesto è una sottoscrizione nel bus di messaggi, questo deve essere uguale al numero di client più il server stesso. Un **lavoro allocato** è un componente che invia i dati per le connessioni attive; un **lavoro occupato** è quello che sta inviando attivamente un messaggio.

- **Bus di messaggi ricevuti totale dei messaggi**
- **Bus di messaggi messaggi ricevuti/Sec**
- **Bus di messaggi pubblicati totale**
- **Bus di messaggi messaggi pubblicati/Sec**
- **Messaggio Bus sottoscrizione corrente**
- **Totale di sottoscrittori Bus di messaggi**
- **I sottoscrittori di Bus di messaggi al secondo**
- **Bus di messaggi allocati worker**
- **Processi di lavoro occupati Bus messaggio**
- **Messaggio Bus argomenti corrente**

**Metrica di errore**

Le metriche seguenti misurano gli errori generati dal traffico di messaggi SignalR. **Risoluzione di hub** si verificano errori quando un hub o un metodo dell'hub non può essere risolto. **Chiamata dell'hub** gli errori sono le eccezioni generate durante la chiamata di un metodo dell'hub. **Trasporto** gli errori di connessione generata durante una richiesta o risposta HTTP.

- **Gli errori: Totale di tutti**
- **Errori: All/Sec**
- **Errori: Totale risoluzione di Hub**
- **Errori: Risoluzione Hub/Sec**
- **Errori: Totale chiamata di Hub**
- **Errori: Chiamata di Hub/Sec**
- **Errori: Totale messaggi trasporto**
- **: Errori di trasporto/Sec**

<a id="scaleout_metrics"></a>

**Metriche di scalabilità orizzontale**

Le metriche seguenti misurano il traffico e gli errori generati dal provider di scalabilità orizzontale. Oggetto **flusso** in questo contesto è un'unità di scala utilizzata dal provider di scalabilità orizzontale; si tratta di una tabella se viene utilizzato SQL Server, un argomento se viene utilizzato il Bus di servizio e una sottoscrizione se viene usato Redis. Ogni flusso garantisce ordinato operazioni di lettura e scrittura; un singolo flusso è un potenziale collo di bottiglia scala, pertanto può essere aumentato il numero di flussi per ridurre il collo di bottiglia. Se si utilizzano più flussi, SignalR distribuirà automaticamente i messaggi (partizione) tra i flussi in modo da garantire che i messaggi inviati da qualsiasi connessione specifica sono in ordine.

Il [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) permette di controllare la lunghezza della coda di invio di scalabilità orizzontale gestita da SignalR. Verrà impostata su un valore maggiore di 0 inseriranno tutti i messaggi in una coda di trasmissione da inviare al backplane di messaggistica configurato uno alla volta. Se le dimensioni della coda sono superiore alla lunghezza configurata, le chiamate successive a inviare verrà non riescono immediatamente con un [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) fino a quando il numero di messaggi nella coda è minore dell'impostazione nuovamente. Accodamento messaggi sono disattivata per impostazione predefinita perché i ripiani posteriori di implementato delle hanno in genere il proprio flusso di controllo o Accodamento messaggi sul posto. Nel caso di SQL Server, è il pool di connessioni in modo efficace limita il numero delle trasmissioni accade in qualsiasi momento.

Per impostazione predefinita, viene utilizzato un solo flusso per SQL Server e di Redis, cinque flussi vengono utilizzati per il Bus di servizio e accodamento è disabilitata, ma queste impostazioni possono essere modificate tramite la configurazione sul Server SQL e Bus di servizio:

**Codice server .NET per la configurazione di lunghezza nella tabella di conteggio e coda per backplane di SQL Server**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**Codice server .NET per la configurazione di lunghezza coda e di conteggio di argomento per backplane Bus di servizio**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

Oggetto **Buffering** flusso è entrato in uno stato di errore; quando il flusso è in stato di errore, tutti i messaggi inviati al backplane avrà esito negativo immediatamente fino a quando il flusso non è in errore. Il **lunghezza coda di invio** è il numero di messaggi che sono state registrate ma non ancora inviati.

- **Bus di messaggi di scalabilità orizzontale dei messaggi ricevuti/Sec**
- **Totale di flussi di scalabilità orizzontale**
- **Apertura di flussi di scalabilità orizzontale**
- **Flussi di scalabilità orizzontale memorizzazione nel buffer**
- **Totale errori di scalabilità orizzontale**
- **Errori di scalabilità orizzontale al secondo**
- **Lunghezza coda di trasmissione di scalabilità orizzontale**

Per ulteriori informazioni su ciò che misurano le questi contatori, vedere [SignalR Scaleout con Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Con altri contatori di prestazioni

Contatori delle prestazioni seguenti possono essere utili per monitorare le prestazioni dell'applicazione.

**Memoria**

- Memoria CLR .NET\\byte in tutti gli heap (per w3wp)

**ASP.NET 2.0**

- Net\richieste correnti
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- Tempo del processore Information\Processor

**TCP/IP**

- TCPv6 le connessioni stabilite
- TCPv4 le connessioni stabilite

**Servizio Web**

- Web\connessioni
- Connessioni Service\Maximum Web

**Threading**

- .NET Common Language Runtime blocca thread e\\thread logici attuali
- .NET Common Language Runtime blocca thread e\\thread fisici attuali

<a id="otherresources"></a>

## <a name="other-resources"></a>Altre risorse

Per ulteriori informazioni sul monitoraggio e ottimizzazione delle prestazioni di ASP.NET, vedere gli argomenti seguenti:

- [Cenni preliminari sulle prestazioni di ASP.NET](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)
- [ASP.NET Thread Usage on IIS 6.0, IIS 7.5 e IIS 7.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; elemento (impostazioni Web)](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
