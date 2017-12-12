---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: Utilizzare i metodi asincroni in ASP.NET MVC 4 | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web ASP.NET MVC asincrona utilizzando Visual Studio Express 2012 per Web, ovvero una ve disponibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 6a0c8dbbd02549757c316807b8e8e64fdfd70123
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Utilizzo di metodi asincroni in ASP.NET MVC 4
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web ASP.NET MVC asincrona utilizzando [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11/en-us), che è una versione gratuita di Microsoft Visual Studio. È inoltre possibile utilizzare [Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us).

> Viene fornito un esempio completo per questa esercitazione su github [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


ASP.NET MVC 4 [Controller](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller(VS.108).aspx) classe in combinazione [.NET 4.5](https://msdn.microsoft.com/en-us/library/w0x726c2(VS.110).aspx) consente di scrivere metodi di azione asincroni che restituiscono un oggetto di tipo [attività&lt;ActionResult&gt; ](https://msdn.microsoft.com/en-us/library/dd321424(VS.110).aspx). .NET Framework 4 introdotto un concetto di programmazione asincrono detto un [attività](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) e supporta ASP.NET MVC 4 [attività](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx). Le attività sono rappresentate dal **attività** tipo e i tipi correlati nel [System.Threading.Tasks](https://msdn.microsoft.com/en-us/library/system.threading.tasks.aspx) dello spazio dei nomi. .NET Framework 4.5 si basa su questo supporto asincrono con il [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) parole chiave che rendono l'utilizzo di [attività](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) oggetti molto meno complessi precedente approccio asincrono. Il [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) la parola chiave è sintattico a sintassi abbreviata per indicare che una parte di codice è necessario attendere in modo asincrono in altre parti di codice. Il [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) parola chiave rappresenta un suggerimento che consente di contrassegnare i metodi come metodi asincroni basati su attività. La combinazione di **await**, **async**e **attività** oggetto rende molto più semplice per la scrittura di codice asincrona in .NET 4.5. Viene chiamato il nuovo modello per i metodi asincroni di *modello asincrono basato su attività* (**toccare**). In questa esercitazione si presuppone che l'utente abbia familiarità con programmazione asincrona [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) parole chiave e il [attività](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) dello spazio dei nomi.

Per ulteriori informazioni sul utilizzando [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) parole chiave e il [attività](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) dello spazio dei nomi, vedere i seguenti riferimenti.

- [White paper: Modalità asincrona in .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Domande frequenti su Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programmazione asincrona di Visual Studio](https://msdn.microsoft.com/en-us/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Come le richieste vengono elaborate dal Pool di Thread

Nel server web, .NET Framework gestisce un pool di thread utilizzati per soddisfare le richieste ASP.NET. Quando arriva una richiesta, viene inviato un thread dal pool per elaborare la richiesta. Se la richiesta viene elaborata in modo sincrono, il thread che elabora la richiesta è occupato durante la richiesta è stato elaborato, e che i thread non può soddisfare un'altra richiesta.   
  
Questo potrebbe non essere un problema, perché possa essere resi grande abbastanza da contenere molti thread occupati pool di thread. Tuttavia, il numero di thread nel pool di thread è limitato (il valore massimo predefinito per .NET 4.5 è 5.000). In applicazioni di grandi dimensioni con concorrenza elevata delle richieste a esecuzione prolungata, tutti i thread disponibili potrebbero essere occupati. Questa condizione è nota come esaurimento dei thread. Quando viene raggiunta questa condizione, il server web le richieste accodate. Se la coda di richieste si riempie, il server web rifiuta le richieste con stato HTTP 503 (Server occupato). Il pool di thread CLR presenta alcune limitazioni in nuovi attacchi injection di thread. Se la concorrenza è bursty (vale a dire, il sito web improvvisamente ottenere un numero elevato di richieste) e tutti i thread di richiesta disponibili sono occupati a causa di chiamate di back-end con latenza elevata, la velocità di inserimento thread limitato è possibile rendere l'applicazione risponde molto ridotte. Inoltre, ogni nuovo thread aggiunti al pool di thread ha un overhead (ad esempio, 1 MB di memoria dello stack). Un'applicazione web tramite i metodi sincroni per chiamate ad alta latenza del servizio in cui il pool di thread aumenta per il valore predefinito di .NET 4.5 massimo pari a 5, thread 000 comporta l'utilizzo di circa 5 GB di memoria rispetto a un'applicazione in grado del servizio lo stesso richiede utilizzando metodi asincroni e thread solo 50. Quando si sta eseguendo operazioni asincrone, non sempre si usa un thread. Ad esempio, quando si effettua una richiesta di servizio web asincroni, ASP.NET non verrà utilizzato alcun thread tra il **async** chiamata al metodo e **await**. Il pool di thread per rispondere alle richieste con una latenza elevata può causare un footprint di grandi quantità di memoria e l'utilizzo di una riduzione dell'hardware del server.

## <a name="processing-asynchronous-requests"></a>L'elaborazione delle richieste asincrone

Nelle applicazioni web che visualizza un numero elevato di richieste simultanee all'avvio o ha un carico bursty (dove la concorrenza aumenta improvvisamente), effettua le chiamate al servizio web asincrona aumenta la velocità di risposta dell'applicazione. Una richiesta asincrona ha la stessa quantità di tempo per elaborare una richiesta sincrona. Ad esempio, se una richiesta effettuata a un servizio web di chiamata che richiede due secondi completare, la richiesta impiegherà due secondi sia che venga eseguita in modo sincrono o asincrono. Durante una chiamata asincrona, tuttavia, un thread non è bloccato da rispondere alle richieste di altri durante l'attesa di completamento della prima richiesta. Pertanto, le richieste asincrone impediscono crescita di pool di thread e di Accodamento messaggi richiesta quando sono presenti molte richieste simultanee che richiamano operazioni a esecuzione prolungata.

## <a id="ChoosingSyncVasync"></a>Scelta dei metodi di azione sincroni o asincroni

In questa sezione elenca le linee guida per l'utilizzo di metodi di azione sincrono o asincrono. Si tratta di semplici linee guida. esaminare ogni applicazione singolarmente per determinare se i metodi asincroni migliorare le prestazioni.

In generale, è possibile utilizzare metodi sincroni per le condizioni seguenti:

- Le operazioni sono semplici o a esecuzione breve.
- La semplicità è più importante dell'efficienza.
- Le operazioni sono principalmente operazioni della CPU anziché operazioni che comportano esteso del disco o carico di rete. Utilizzando i metodi di azione asincroni per le operazioni associate alla CPU non offre alcun vantaggio e comporta un maggiore sovraccarico.

 In generale, è possibile utilizzare metodi asincroni per le condizioni seguenti:

- Si sta chiamando i servizi che possono essere utilizzati tramite i metodi asincroni, e si utilizza .NET 4.5 o versione successiva.
- Le operazioni sono associate alla rete o associate I/O-anziché basate sulla CPU.
- Il parallelismo è più importante della semplicità del codice.
- Si desidera fornire un meccanismo che consente agli utenti di annullare una richiesta a esecuzione prolungata.
- Il vantaggio del cambio del thread out pondera quando il costo del cambio di contesto. In generale, verificare che un metodo asincrono se il metodo sincrono resta in attesa di thread di richiesta ASP.NET durante l'esecuzione di alcuna operazione. Effettuando questa chiamata asincrona, il thread di richiesta ASP.NET non sia bloccato mentre attende il completamento della richiesta di servizio web non esegue alcuna operazione.
- I test mostrano che le operazioni di blocco sono un collo di bottiglia delle prestazioni del sito e che IIS può soddisfare più richieste tramite metodi asincroni per queste chiamate di blocco.

 L'esempio scaricabile mostra come utilizzare efficacemente metodi di azione asincroni. L'esempio fornito è stato progettato per fornire una dimostrazione semplice della programmazione asincrona in ASP.NET MVC 4 con .NET 4.5. L'esempio non deve essere un'architettura di riferimento per la programmazione asincrona in ASP.NET MVC. Il programma di esempio chiama [ASP.NET Web API](../../../web-api/index.md) metodi che a sua volta chiamano [Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096(VS.110).aspx) per simulare chiamate al servizio web con esecuzione prolungata. La maggior parte delle applicazioni di produzione non visualizzerà tale ovvi utilizzando i metodi di azione asincrono.   
  
Alcune applicazioni richiedono tutti i metodi di azione asincrono. Spesso, la conversione di alcuni metodi di azione sincroni in metodi asincroni fornisce il migliore aumento dell'efficienza per la quantità di lavoro richiesto.

## <a id="SampleApp"></a>L'applicazione di esempio

È possibile scaricare l'applicazione di esempio da [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET) sul [GitHub](https://github.com/) sito. Il repository è costituito da tre progetti:

- *Mvc4Async*: progetto di ASP.NET MVC 4 che contiene il codice usato in questa esercitazione. Effettua chiamate API Web per il **WebAPIpgw** servizio.
- *WebAPIpgw*: progetto API Web di ASP.NET MVC 4 che implementa il `Products, Gizmos and Widgets` controller. Fornisce i dati per il *WebAppAsync* progetto e *Mvc4Async* progetto.
- *WebAppAsync*: progetto di Web Form di ASP.NET utilizzato in un'altra esercitazione.

## <a id="GizmosSynch"></a>Il metodo di azione sincrono Gizmos

 Il codice seguente illustra il `Gizmos` metodo di azione sincrono utilizzato per visualizzare un elenco di gizmos. (Per questo articolo, un gizmo è un dispositivo meccanico fittizio). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

Il codice seguente illustra il `GetGizmos` metodo del servizio gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

Il `GizmoService GetGizmos` metodo passa un URI a un servizio HTTP di ASP.NET Web API che restituisce un elenco di dati gizmos. Il *WebAPIpgw* progetto contiene l'implementazione dell'API Web `gizmos, widget` e `product` controller.  
Nella figura seguente mostra la visualizzazione gizmos dal progetto di esempio.

![Gizmos](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Creazione di un metodo di azione asincrono Gizmos

Nell'esempio viene utilizzato il nuovo [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) e [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) parole chiave (disponibile in .NET 4.5 e Visual Studio 2012) per consentire al compilatore di essere responsabile della manutenzione necessari per le trasformazioni complesse programmazione asincrona. Il compilatore consente di scrivere codice utilizzando che i costrutti sincrona il di # controllo di flusso e il compilatore applica automaticamente le trasformazioni necessarie per l'utilizzo di callback per evitare di bloccare i thread.

Il codice seguente illustra il `Gizmos` metodo sincrono e `GizmosAsync` metodo asincrono. Se il browser supporta il [HTML 5 `<mark>` elemento](http://www.w3.org/wiki/HTML/Elements/mark), si noterà che le modifiche in `GizmosAsync` in evidenziazione di colore giallo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 Sono state applicate le seguenti modifiche per consentire il `GizmosAsync` sia asincrona.

- Il metodo è contrassegnato con il [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) (parola chiave), che indica al compilatore di generare i callback per parti del corpo e di creare automaticamente un `Task<ActionResult>` restituito.
- &quot;Async&quot; è stato aggiunto al nome del metodo. Aggiungere "Async" non è obbligatorio ma è la convenzione durante la scrittura di metodi asincroni.
- Il tipo restituito è stato modificato da `ActionResult` a `Task<ActionResult>`. Il tipo restituito di `Task<ActionResult>` rappresenta il lavoro in corso e fornisce i chiamanti del metodo con un handle tramite cui si desidera attendere il completamento dell'operazione asincrona. In questo caso, il chiamante è il servizio web. `Task<ActionResult>`rappresenta in corso di lavoro con un risultato`ActionResult.`
- Il [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) (parola chiave) è stato applicato a una chiamata al servizio web.
- È stata chiamata l'API del servizio web asincrona (`GetGizmosAsync`).

All'interno del `GetGizmosAsync` un altro metodo asincrono, il corpo del metodo `GetGizmosAsync` viene chiamato. `GetGizmosAsync`restituisce immediatamente un `Task<List<Gizmo>>` che infine verrà completata quando i dati sono disponibili. Perché non si desidera eseguire altre operazioni finché non si dispone dei dati gizmo, il codice attende l'attività (usando il **await** parola chiave). È possibile utilizzare il **await** parola chiave solo in metodi annotati con il **async** (parola chiave).

Il **await** parola chiave non blocca il thread fino al completamento dell'attività. Registra il resto del metodo come un callback dell'attività e viene restituito immediatamente. Al termine dell'attività attesa infine, verrà richiamare il callback e quindi riprendere l'esecuzione del diritto di metodo in cui è stata interrotta. Per ulteriori informazioni sull'utilizzo di [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) parole chiave e il [attività](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) dello spazio dei nomi, vedere il [async riferimenti](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Il codice seguente illustra il `GetGizmos` e `GetGizmosAsync` metodi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Le modifiche asincrone sono simili a quelle apportate il **GizmosAsync** sopra. 

- La firma del metodo è stata annotata con il [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) (parola chiave), il tipo restituito è stato modificato in `Task<List<Gizmo>>`, e *Async* è stato aggiunto al nome del metodo.
- Asincrona [HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(VS.110).aspx) classe viene usata invece del [WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient.aspx) classe.
- Il [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) (parola chiave) è stato applicato al [HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(VS.110).aspx) metodi asincroni.

Nella figura seguente mostra la visualizzazione gizmo asincrona.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

La presentazione del browser dei dati gizmos è identica alla visualizzazione creata tramite la chiamata sincrona. L'unica differenza è che la versione asincrona potrebbe essere più efficiente in presenza di carichi.

## <a id="Parallel"></a>Esecuzione di più operazioni in parallelo

Metodi di azione asincroni hanno un vantaggio significativo rispetto ai metodi sincroni quando un'azione deve eseguire diverse operazioni indipendenti. Nell'esempio fornito, il metodo sincrono `PWG`(per i prodotti, widget e Gizmos) consente di visualizzare i risultati di tre chiamate al servizio web per ottenere un elenco di prodotti, widget e gizmos. Il [ASP.NET Web API](../../../web-api/index.md) progetto che fornisce questi servizi utilizza [Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096(VS.110).aspx) per simulare una rete lenta o latenza di chiamate. Quando il ritardo è impostato su 500 millisecondi, asincrona `PWGasync` metodo accetta un po' oltre 500 millisecondi per completare durante sincroni `PWG` versione assume 1500 millisecondi. Sincroni `PWG` metodo è illustrato nel codice seguente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Asincrona `PWGasync` metodo è illustrato nel codice seguente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

La figura seguente mostra la visualizzazione restituita dal **PWGasync** metodo.

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>Utilizzando un Token di annullamento

Metodi di azione asincroni restituzione `Task<ActionResult>`sono annullabile, che accettano un [CancellationToken](https://msdn.microsoft.com/en-us/library/system.threading.cancellationtoken(VS.110).aspx) parametro quando ne viene fornita con il [AsyncTimeout](https://msdn.microsoft.com/en-us/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) attributo. Il codice seguente illustra il `GizmosCancelAsync` metodo con un timeout di 150 millisecondi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

Il codice seguente viene illustrato l'overload GetGizmosAsync, che accetta un [CancellationToken](https://msdn.microsoft.com/en-us/library/system.threading.cancellationtoken(VS.110).aspx) parametro.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

Nell'applicazione di esempio fornito, selezionando il *Demo di Token di annullamento* collegamento chiamate il `GizmosCancelAsync` metodo e viene illustrato l'annullamento della chiamata asincrona.

## <a id="ServerConfig"></a>Configurazione del server per chiamate al servizio Web latenza elevata concorrenza/elevata

Per comprendere i vantaggi di un'applicazione web asincrono, potrebbe essere necessario apportare alcune modifiche alla configurazione del server predefinito. Tenere presente durante la configurazione e l'applicazione web asincrona di test di stress quanto segue.

- Windows 7, Windows Vista e tutti i sistemi operativi client Windows è possibile avere un massimo di 10 richieste simultanee. È necessario un sistema operativo Windows Server per visualizzare i vantaggi dei metodi asincroni un carico elevato.
- Registrare .NET 4.5 con IIS da un prompt dei comandi con privilegi elevati:  
 %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i  
 Vedere [strumento di registrazione IIS ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-us/library/k6h9cz8h.aspx)
- Potrebbe essere necessario aumentare il [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limite della coda da 1.000 a 5.000 il valore predefinito. Se l'impostazione è troppo basso, può vedere [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rifiutare le richieste con stato HTTP 503. Per modificare il limite della coda HTTP. sys:

    - Aprire Gestione IIS e passare al riquadro pool di applicazioni.
    - Fare clic con il pulsante destro sul pool di applicazioni di destinazione e selezionare **impostazioni avanzate**.  
        ![avanzate](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - Nel **impostazioni avanzate** nella finestra di dialogo Modifica *lunghezza coda* da 1.000 a 5.000.  
        ![Lunghezza coda](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
 Si noti nell'immagine precedente, .NET framework è elencato come v 4.0, anche se il pool di applicazioni utilizza .NET 4.5. Per comprendere questa discrepanza, vedere gli argomenti seguenti:

    - [Controllo delle versioni .NET e Multi-Targeting - .NET 4.5 è un aggiornamento sul posto a .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Come impostare un'applicazione IIS o i pool di applicazioni per utilizzare ASP.NET 3.5 anziché 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [Versioni di .NET framework e dipendenze](https://msdn.microsoft.com/en-us/library/bb822049(VS.110).aspx)
- Se l'applicazione utilizza servizi web o System.NET per comunicare con un back-end tramite HTTP si potrebbe essere necessario aumentare il [connectionManagement/maxconnection](https://msdn.microsoft.com/en-us/library/fb6y0fyc(VS.110).aspx) elemento. Per le applicazioni ASP.NET, questa è limitata dalla caratteristica di configurazione automatica per 12 volte il numero di CPU. Ciò significa che un quattro processori, è possibile avere al massimo 12 \* 4 = 48 connessioni simultanee a un punto finale IP. Poiché questo è collegato a [configurazione automatica WLAN](https://msdn.microsoft.com/en-us/library/7w2sway1(VS.110).aspx), il modo più semplice per aumentare `maxconnection` in ASP.NET consiste nell'impostare l'applicazione [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) a livello di codice in dal `Application_Start` metodo il *Global. asax* file. Vedere l'esempio di download per un esempio.
- In .NET 4.5, il valore predefinito di 5000 per [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) dovrebbe costituire un problema.
