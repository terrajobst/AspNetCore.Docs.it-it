---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: Uso di metodi asincroni in ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET asincrona usando Visual Studio Express 2012 per Web, ovvero una gratuita ve...
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 757b15c34f6fa0078d0bca0dfb38d553bb73809d
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577704"
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Utilizzo di metodi asincroni in ASP.NET MVC 4
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET asincrone usando [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11), ovvero una versione gratuita di Microsoft Visual Studio. È anche possibile usare [Visual Studio 2012](https://www.microsoft.com/visualstudio/11).
> 
> Un esempio completo è disponibile per questa esercitazione su github  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


ASP.NET MVC 4 [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) classe in combinazione [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) consente di scrivere metodi di azione asincroni che restituiscono un oggetto di tipo [attività&lt;ActionResult&gt; ](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). .NET Framework 4 introdotto un concetto di programmazione asincrono detto una [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) e ASP.NET MVC 4 supporta [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Le attività sono rappresentate dal **Task** tipo e i tipi correlati nel [Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) dello spazio dei nomi. .NET Framework 4.5 si basa su questo supporto asincrono con il [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parole chiave che rendono l'utilizzo dei [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) oggetti molto meno complessi precedente approccio asincrono. Il [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) parola chiave è una sintassi abbreviata per indicare che un frammento di codice deve attendere in modo asincrono in altre parti di codice. Il [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parola chiave rappresenta un suggerimento che è possibile usare per contrassegnare i metodi come metodi asincroni basati su attività. La combinazione delle **await**, **async**e il **attività** oggetto rende molto più semplice per la scrittura di codice asincrono in .NET 4.5. Il nuovo modello per i metodi asincroni viene chiamato il *Task-based Asynchronous Pattern* (**toccare**). Questa esercitazione si presuppone una certa conoscenza di programmazione asincrono utilizzando [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parole chiave e il [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) dello spazio dei nomi.

Per altre informazioni sul utilizzando [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parole chiave e il [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) dello spazio dei nomi, vedere i seguenti riferimenti.

- [White paper: Asincronia in .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Domande frequenti su Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programmazione asincrona di Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Come le richieste vengono elaborate dal Pool di Thread

Nel server web, .NET Framework gestisce un pool di thread usati per gestire le richieste ASP.NET. Quando arriva una richiesta, viene inviato un thread dal pool per elaborare la richiesta. Se la richiesta viene elaborata in modo sincrono, il thread che elabora la richiesta è occupato durante la richiesta viene elaborato, e che i thread non può soddisfare un'altra richiesta.   
  
Ciò potrebbe non essere un problema, perché il pool di thread può essere reso grande abbastanza da contenere molti thread occupati. Tuttavia, il numero di thread nel pool di thread è limitato (il valore massimo predefinito per .NET 4.5 è 5.000). Nelle applicazioni di grandi dimensioni con concorrenza elevata di richieste a esecuzione prolungata, tutti i thread disponibili potrebbero essere occupati. Questa condizione è nota come esaurimento dei thread. Quando viene raggiunta questa condizione, il server web mette in coda le richieste. Se la coda di richieste si riempie, il server web rifiuta le richieste con stato HTTP 503 (Server occupato). Il pool di thread CLR presenta limitazioni sul nuovo injection tramite thread. Se la concorrenza è bursty (vale a dire, il sito web può improvvisamente ottenere un numero elevato di richieste) e tutti i thread di richiesta disponibili sono occupati a causa di chiamate di back-end con latenza elevata, la velocità di inserimento thread limitata può rendere l'applicazione risponde basse. Inoltre, ogni nuovo thread aggiunti al pool di thread presenta un overhead (ad esempio, 1 MB di memoria dello stack). Un'applicazione web usando i metodi sincroni chiamate da servizio a latenza elevata in cui il pool di thread aumenta il valore predefinito di .NET 4.5 massimo pari a 5, thread 000 richiederebbe circa 5 GB di memoria rispetto a un'applicazione in grado del servizio lo stesso le richieste usando i metodi asincroni e i thread solo 50. Quando si sta eseguendo operazioni asincrone, non sempre si usa un thread. Ad esempio, quando si effettua una richiesta di servizio web asincrono, ASP.NET non utilizzerà tutti i thread tra i **async** chiamata al metodo e il **await**. Il pool di thread per soddisfare le richieste con una latenza elevata può causare un footprint di memoria di grandi dimensioni e uso non appropriato dell'hardware del server.

## <a name="processing-asynchronous-requests"></a>L'elaborazione asincrona delle richieste

In un'app web che visualizza un numero elevato di richieste simultanee all'avvio o presenta un carico bursty (in cui la concorrenza aumenta improvvisamente), effettua chiamate al servizio web asincrono consente di aumentare la velocità di risposta dell'app. Una richiesta asincrona ha la stessa quantità di tempo per elaborare una richiesta sincrona. Se una richiesta effettua a un servizio web di chiamata che richiede due secondi, la richiesta impiegherà due secondi sia che venga eseguita in modo sincrono o asincrono. Tuttavia durante una chiamata asincrona, non viene impedito un thread di rispondere ad altre richieste mentre si attende il completamento della richiesta prima. Di conseguenza, le richieste asincrone impediscono crescita di pool di thread e di accodamento richiesta quando sono presenti molte richieste simultanee che richiamano operazioni a esecuzione prolungata.

## <a id="ChoosingSyncVasync"></a>  Scelta dei metodi di azione sincroni o asincroni

In questa sezione elenca le linee guida relative all'utilizzo di metodi di azione sincrono o asincrono. Si tratta di semplici linee guida. esaminare ogni applicazione singolarmente per determinare se i metodi asincroni migliorare le prestazioni.

In generale, usare i metodi sincroni per le condizioni seguenti:

- Le operazioni sono semplici o a esecuzione breve.
- La semplicità è più importante dell'efficienza.
- Le operazioni sono principalmente operazioni della CPU anziché operazioni che comportano un sovraccarico di rete o esteso del disco. Usando i metodi di azione asincroni sulle operazioni associate alla CPU non offre alcun vantaggio e comporta un maggiore sovraccarico.

  In generale, usare i metodi asincroni per le condizioni seguenti:

- Si chiamano i servizi che possono essere utilizzati tramite i metodi asincroni, e si usa .NET 4.5 o versione successiva.
- Le operazioni sono associate alla rete o i/O associate ai anziché basate sulla CPU.
- Il parallelismo è più importante della semplicità del codice.
- Si desidera fornire un meccanismo che consente agli utenti di annullare una richiesta a esecuzione prolungata.
- Quando il vantaggio del cambio di thread supera il costo del cambio di contesto. In generale, è necessario rendere un metodo asincrono se il metodo sincrono è in attesa nel thread di richiesta ASP.NET durante l'esecuzione di alcuna operazione. Eseguendo la chiamata asincrona, il thread di richiesta ASP.NET non sia bloccato in attesa del completamento della richiesta di servizio web non esegue alcuna operazione.
- Test indica che le operazioni di blocco sono un collo di bottiglia delle prestazioni del sito e che IIS può soddisfare più richieste tramite i metodi asincroni per queste chiamate di blocco.

  L'esempio scaricabile mostra come utilizzare efficacemente metodi di azione asincroni. L'esempio fornito è stato progettato per fornire una semplice dimostrazione della programmazione asincrona in ASP.NET MVC 4 con .NET 4.5. L'esempio non deve essere un'architettura di riferimento per la programmazione asincrona in ASP.NET MVC. Il programma di esempio chiama [API Web ASP.NET](../../../web-api/index.md) metodi che a sua volta chiamano [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) per simulare le chiamate del servizio web con esecuzione prolungata. La maggior parte delle applicazioni di produzione non mostreranno tali evidenti vantaggi dell'uso di metodi di azione asincroni.   
  
Poche applicazioni richiedono che tutti i metodi di azione asincrona. Spesso, la conversione di alcuni metodi di azione sincroni in metodi asincroni fornisce il migliore aumento dell'efficienza per la quantità di lavoro richiesto.

## <a id="SampleApp"></a>  L'applicazione di esempio

È possibile scaricare l'applicazione di esempio dal [ https://github.com/RickAndMSFT/Async-ASP.NET/ ](https://github.com/RickAndMSFT/Async-ASP.NET) sul [GitHub](https://github.com/) sito. Il repository è costituito da tre progetti:

- *Mvc4Async*: progetto di ASP.NET MVC 4 che contiene il codice usato in questa esercitazione. Effettua chiamate all'API Web per il **WebAPIpgw** servizio.
- *WebAPIpgw*: progetto di API Web ASP.NET MVC 4 che implementa il `Products, Gizmos and Widgets` controller. Fornisce i dati per il *WebAppAsync* progetti e i *Mvc4Async* progetto.
- *WebAppAsync*: progetto di Web Form ASP.NET utilizzato in un'altra esercitazione.

## <a id="GizmosSynch"></a>  Il metodo di azione sincroni Gizmo

 Il codice seguente illustra il `Gizmos` metodo di azione sincrono utilizzato per visualizzare un elenco di gizmo. (In questo articolo una gizmo è un dispositivo meccanico fittizio). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

Il codice seguente illustra il `GetGizmos` metodo del servizio gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

Il `GizmoService GetGizmos` metodo passa un URI a un servizio HTTP API Web ASP.NET che restituisce un elenco di dati gizmo. Il *WebAPIpgw* progetto contiene l'implementazione dell'API Web `gizmos, widget` e `product` controller.  
L'immagine seguente mostra la visualizzazione gizmo dal progetto di esempio.

![Gizmo](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Creazione di un metodo di azione asincrono Gizmo

L'esempio Usa il nuovo [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) parole chiave (disponibile in .NET 4.5 e Visual Studio 2012) per consentire al compilatore di essere responsabile della gestione necessarie per le trasformazioni complesse programmazione asincrona. Il compilatore consente di scrivere codice usando che costrutti di flusso di controllo sincrono del # e il compilatore applica automaticamente le trasformazioni necessarie per usare i callback per evitare di bloccare i thread.

Il codice seguente illustra il `Gizmos` metodo sincrono e il `GizmosAsync` metodo asincrono. Se il browser supporta il [HTML5 `<mark>` elemento](http://www.w3.org/wiki/HTML/Elements/mark), si noterà che le modifiche in `GizmosAsync` nell'evidenziazione di colore giallo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 Le modifiche seguenti sono state applicate per consentire il `GizmosAsync` asincrona.

- Il metodo è contrassegnato con il [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parola chiave, che indica al compilatore di generare callback per parti del corpo e di creare automaticamente un `Task<ActionResult>` restituito.
- &quot;Async&quot; è stato aggiunto al nome del metodo. Aggiungere "Async" non è obbligatorio ma è la convenzione durante la scrittura di metodi asincroni.
- Il tipo restituito è stato modificato da `ActionResult` a `Task<ActionResult>`. Il tipo restituito di `Task<ActionResult>` rappresenta il lavoro in corso e fornisce i chiamanti del metodo con un handle tramite cui si desidera attendere il completamento dell'operazione asincrona. In questo caso, il chiamante è il servizio web. `Task<ActionResult>` rappresenta in corso di lavoro con un risultato `ActionResult.`
- Il [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) è stata applicata la parola chiave per la chiamata al servizio web.
- È stato chiamato l'API del servizio web asincrono (`GetGizmosAsync`).

All'interno del `GetGizmosAsync` metodo corpo di un altro metodo asincrono, `GetGizmosAsync` viene chiamato. `GetGizmosAsync` restituisce immediatamente un `Task<List<Gizmo>>` che alla fine verrà completata quando sono disponibili i dati. Perché non si desidera eseguire altre operazioni fino a quando non sono disponibili i dati gizmo, il codice attende l'attività (utilizzando la **await** parola chiave). È possibile usare la **await** parola chiave solo nei metodi annotati con i **async** (parola chiave).

Il **await** parola chiave non blocca il thread fino al completamento dell'attività. Registra il resto del metodo come callback per l'attività e viene restituito immediatamente. Al termine dell'attività attesa alla fine, verrà richiamare tale callback e quindi riprendere l'esecuzione di destra (metodo) in cui è stata interrotta. Per altre informazioni sull'uso di [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parole chiave e il [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) dello spazio dei nomi, vedere il [async riferimenti](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Il codice seguente illustra il `GetGizmos` e `GetGizmosAsync` metodi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Le modifiche asincrone sono simili a quelle apportate per il **GizmosAsync** sopra. 

- La firma del metodo è stata annotata con il [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parola chiave, per cui è stato modificato il tipo restituito `Task<List<Gizmo>>`, e *Async* è stato aggiunto al nome del metodo.
- Asincrona [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) classe viene usata invece del [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) classe.
- Il [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) è stata applicata la parola chiave per il [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) metodi asincroni.

L'immagine seguente mostra la visualizzazione gizmo asincrona.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

La presentazione di browser dei dati gizmo è identica alla vista creata dalla chiamata sincrona. L'unica differenza è che la versione asincrona potrebbe risultare più efficiente con carichi pesanti.

## <a id="Parallel"></a>  Esecuzione di più operazioni in parallelo

Metodi di azione asincroni hanno un grande vantaggio rispetto ai metodi sincroni quando un'azione deve eseguire diverse operazioni indipendenti. Nell'esempio fornito, il metodo sincrono `PWG`(per i prodotti, i widget e Gizmo) consente di visualizzare i risultati di tre chiamate al servizio web per ottenere un elenco di prodotti, widget ed gizmo. Il [API Web ASP.NET](../../../web-api/index.md) progetto che fornisce questi servizi viene utilizzato [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) per simulare una rete lenta o la latenza di chiamate. Quando il ritardo è impostato su 500 millisecondi, asincrone `PWGasync` metodo accetta un po' oltre 500 millisecondi per completare durante sincroni `PWG` versione subentra 1500 millisecondi. Sincrono `PWG` metodo è illustrato nel codice seguente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Asincrona `PWGasync` metodo è illustrato nel codice seguente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

L'immagine seguente mostra la visualizzazione restituita dalle **PWGasync** (metodo).

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>  Usando un Token di annullamento

I metodi di azione asincroni che restituiscono `Task<ActionResult>`sono annullabile, ovvero che accettano un [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parametro quando ne viene specificato con il [AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) attributo. Il codice seguente illustra il `GizmosCancelAsync` metodo con un timeout di 150 millisecondi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

Il codice seguente viene illustrato l'overload GetGizmosAsync, che accetta un [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parametro.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

Nell'applicazione di esempio fornito, selezionando il *Demo di Token di annullamento* collegare le chiamate di `GizmosCancelAsync` (metodo) e viene illustrato l'annullamento della chiamata asincrona.

## <a id="ServerConfig"></a>  Configurazione del server per chiamate ai servizi Web di latenza elevata concorrenza/alto

Per sfruttare i vantaggi di un'applicazione web asincrona, si potrebbe essere necessario apportare alcune modifiche alla configurazione del server predefinito. Tenere presente durante la configurazione e l'applicazione web asincrona di test di stress quanto segue.

- Windows 7, Windows Vista e tutti i sistemi operativi client Windows è possibile avere un massimo di 10 richieste concomitanti. È necessario un sistema operativo Windows Server per scoprire i vantaggi dei metodi asincroni un carico elevato.
- Registrare .NET 4.5 con IIS da un prompt dei comandi con privilegi elevati:  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i  
  Visualizzare [strumento di registrazione ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Potrebbe essere necessario aumentare la [HTTP. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limite coda dal valore predefinito di 1000 a 5000. Se l'impostazione è troppo bassa, è possibile riscontrare [HTTP. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rifiutare le richieste con stato HTTP 503. Per modificare il limite di coda HTTP. sys:

    - Aprire Gestione IIS e passare al riquadro pool di applicazioni.
    - Fare clic con il pulsante destro sul pool di applicazioni di destinazione e selezionare **impostazioni avanzate**.  
        ![Advanced](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - Nel **impostazioni avanzate** della finestra di dialogo Modifica *lunghezza coda* da 1.000 a 5.000.  
        ![Lunghezza coda](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  Si noti in immagini riportate in precedenza, .NET framework è elencato come v4.0, anche se il pool di applicazioni Usa .NET 4.5. Per comprendere questa discrepanza, vedere gli argomenti seguenti:

    - [Versionamento di .NET e Multi-Targeting - .NET 4.5 è un aggiornamento sul posto per .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Come impostare un'applicazione IIS o a un pool di applicazioni per utilizzare ASP.NET 3.5 anziché 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [Versioni e dipendenze di .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Se l'applicazione utilizza servizi web o System.NET per comunicare con un back-end tramite HTTP si potrebbe essere necessario aumentare la [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elemento. Per le applicazioni ASP.NET, questo è limitato dalla funzionalità di configurazione automatica a 12 volte il numero di CPU. Ciò significa che in una a quattro processori, è possibile avere al massimo 12 \* 4 = 48 connessioni simultanee a un endpoint IP. Poiché questo è legato alla [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), il modo più semplice aumentare `maxconnection` in ASP.NET dell'applicazione consiste nell'impostare [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) a livello di codice in dal `Application_Start` metodo nella *Global. asax* file. Vedere l'esempio di download per un esempio.
- In .NET 4.5, il valore predefinito di 5000 [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) dovrebbe costituire un problema.
