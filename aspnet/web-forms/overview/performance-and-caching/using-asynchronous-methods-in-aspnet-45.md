---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Utilizzare i metodi asincroni in ASP.NET 4.5 | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET asincrona utilizzando Visual Studio Express 2012 per Web, è disponibile gratuitamente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 839cfc39188a91b6674465b8ff8fe51804033295
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890099"
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Utilizzare i metodi asincroni in ASP.NET 4.5
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET asincrona utilizzando [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11), che è una versione gratuita di Microsoft Visual Studio. È inoltre possibile utilizzare [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). Nelle sezioni seguenti sono inclusi in questa esercitazione.
> 
> - [Modo in cui le richieste vengono elaborate dal Pool di Thread](#HowRequestsProcessedByTP)
> - [Scelta dei metodi sincroni o asincroni](#ChoosingSyncVasync)
> - [L'applicazione di esempio](#SampleApp)
> - [La pagina sincrono Gizmos](#GizmosSynch)
> - [Creazione di una pagina Gizmos asincrona](#CreatingAsynchGizmos)
> - [Esecuzione di più operazioni in parallelo](#Parallel)
> - [Utilizzando un Token di annullamento](#CancelToken)
> - [Configurazione del server per chiamate al servizio Web latenza elevata concorrenza elevata](#ServerConfig)
> 
> Viene fornito un esempio completo per questa esercitazione  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) Nel [GitHub](https://github.com/) sito.


Le pagine Web ASP.NET 4.5 in combinazione [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) consente di registrare metodi asincroni che restituiscono un oggetto di tipo [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). .NET Framework 4 introdotto un concetto di programmazione asincrono detto un [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) e ASP.NET 4.5 supporta [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Le attività sono rappresentate dal **attività** tipo e i tipi correlati nel [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) dello spazio dei nomi. .NET Framework 4.5 si basa su questo supporto asincrono con il [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parole chiave che rendono l'utilizzo di [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) oggetti molto meno complessi precedente approccio asincrono. Il [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) la parola chiave è sintattico a sintassi abbreviata per indicare che una parte di codice è necessario attendere in modo asincrono in altre parti di codice. Il [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parola chiave rappresenta un suggerimento che consente di contrassegnare i metodi come metodi asincroni basati su attività. La combinazione di **await**, **async**e **attività** oggetto rende molto più semplice per la scrittura di codice asincrona in .NET 4.5. Viene chiamato il nuovo modello per i metodi asincroni di *modello asincrono basato su attività* (**toccare**). In questa esercitazione si presuppone che l'utente abbia familiarità con programmazione asincrona [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parole chiave e il [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) dello spazio dei nomi.

Per ulteriori informazioni sul utilizzando [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parole chiave e il [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) dello spazio dei nomi, vedere i seguenti riferimenti.

- [White paper: La modalità asincrona in .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Domande frequenti su Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programmazione asincrona di Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Modo in cui le richieste vengono elaborate dal Pool di Thread

Nel server web, .NET Framework gestisce un pool di thread utilizzati per soddisfare le richieste ASP.NET. Quando arriva una richiesta, viene inviato un thread dal pool per elaborare la richiesta. Se la richiesta viene elaborata in modo sincrono, il thread che elabora la richiesta è occupato durante la richiesta è stato elaborato, e che i thread non può soddisfare un'altra richiesta.   
  
Questo potrebbe non essere un problema, perché possa essere resi grande abbastanza da contenere molti thread occupati pool di thread. Tuttavia, il numero di thread nel pool di thread è limitato (il valore massimo predefinito per .NET 4.5 è 5.000). In applicazioni di grandi dimensioni con concorrenza elevata delle richieste a esecuzione prolungata, tutti i thread disponibili potrebbero essere occupati. Questa condizione è nota come esaurimento dei thread. Quando viene raggiunta questa condizione, il server web le richieste accodate. Se la coda di richieste si riempie, il server web rifiuta le richieste con stato HTTP 503 (Server occupato). Il pool di thread CLR presenta alcune limitazioni in nuovi attacchi injection di thread. Se la concorrenza è bursty (vale a dire, il sito web improvvisamente ottenere un numero elevato di richieste) e tutti i thread di richiesta disponibili sono occupati a causa di chiamate di back-end con latenza elevata, la velocità di inserimento thread limitato è possibile rendere l'applicazione risponde molto ridotte. Inoltre, ogni nuovo thread aggiunti al pool di thread ha un overhead (ad esempio, 1 MB di memoria dello stack). Un'applicazione web tramite i metodi sincroni per chiamate ad alta latenza del servizio in cui il pool di thread aumenta per il valore predefinito di .NET 4.5 massimo pari a 5, thread 000 comporta l'utilizzo di circa 5 GB di memoria rispetto a un'applicazione in grado del servizio lo stesso richiede utilizzando metodi asincroni e thread solo 50. Quando si sta eseguendo operazioni asincrone, non sempre si usa un thread. Ad esempio, quando si effettua una richiesta di servizio web asincroni, ASP.NET non verrà utilizzato alcun thread tra il **async** chiamata al metodo e **await**. Il pool di thread per rispondere alle richieste con una latenza elevata può causare un footprint di grandi quantità di memoria e l'utilizzo di una riduzione dell'hardware del server.

## <a name="processing-asynchronous-requests"></a>L'elaborazione delle richieste asincrone

Nelle applicazioni web che un numero di richieste simultanee all'avvio o ha un carico bursty (dove la concorrenza aumenta improvvisamente), effettua chiamate al servizio web asincrona aumenta la velocità di risposta dell'applicazione. Una richiesta asincrona ha la stessa quantità di tempo per elaborare una richiesta sincrona. Ad esempio, se una richiesta effettuata a un servizio web di chiamata che richiede due secondi completare, la richiesta impiegherà due secondi sia che venga eseguita in modo sincrono o asincrono. Durante una chiamata asincrona, tuttavia, un thread non è bloccato da rispondere alle richieste di altri durante l'attesa di completamento della prima richiesta. Pertanto, le richieste asincrone impediscono crescita di pool di thread e di Accodamento messaggi richiesta quando sono presenti molte richieste simultanee che richiamano operazioni a esecuzione prolungata.

## <a id="ChoosingSyncVasync"></a>  Scelta dei metodi sincroni o asincroni

In questa sezione elenca le linee guida per l'utilizzo di metodi sincroni o asincroni. Si tratta di semplici linee guida. esaminare ogni applicazione singolarmente per determinare se i metodi asincroni migliorare le prestazioni.

In generale, è possibile utilizzare metodi sincroni per le condizioni seguenti:

- Le operazioni sono semplici o a esecuzione breve.
- La semplicità è più importante dell'efficienza.
- Le operazioni sono principalmente operazioni della CPU anziché operazioni che comportano esteso del disco o carico di rete. Utilizzare i metodi asincroni per le operazioni associate alla CPU non offre alcun vantaggio e comporta un maggiore sovraccarico.

  In generale, è possibile utilizzare metodi asincroni per le condizioni seguenti:

- Si sta chiamando i servizi che possono essere utilizzati tramite i metodi asincroni, e si utilizza .NET 4.5 o versione successiva.
- Le operazioni sono associate alla rete o associate I/O-anziché basate sulla CPU.
- Il parallelismo è più importante della semplicità del codice.
- Si desidera fornire un meccanismo che consente agli utenti di annullare una richiesta a esecuzione prolungata.
- Il vantaggio del cambio del thread out pondera quando il costo del cambio di contesto. In generale, verificare che un metodo asincrono se il metodo sincrono blocca il thread di richiesta ASP.NET durante l'esecuzione di alcuna operazione. Effettuando questa chiamata asincrona, il thread di richiesta ASP.NET non è bloccato mentre attende il completamento della richiesta di servizio web non esegue alcuna operazione.
- I test mostrano che le operazioni di blocco sono un collo di bottiglia delle prestazioni del sito e che IIS può soddisfare più richieste tramite metodi asincroni per queste chiamate di blocco.

  L'esempio scaricabile mostra come usare i metodi asincroni in modo efficace. L'esempio fornito è stato progettato per fornire una dimostrazione semplice della programmazione asincrona in ASP.NET 4.5. L'esempio non deve essere un'architettura di riferimento per la programmazione asincrona in ASP.NET. Il programma di esempio chiama [ASP.NET Web API](../../../web-api/index.md) metodi che a sua volta chiamano [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) per simulare chiamate al servizio web con esecuzione prolungata. La maggior parte delle applicazioni di produzione non verranno visualizzati tali vantaggi evidente per utilizzare i metodi asincroni.   
  
Alcune applicazioni richiedono tutti i metodi siano asincroni. Spesso, la conversione di alcuni metodi sincroni in metodi asincroni fornisce il migliore aumento dell'efficienza per la quantità di lavoro richiesto.

## <a id="SampleApp"></a>  L'applicazione di esempio

È possibile scaricare l'applicazione di esempio dal [ https://github.com/RickAndMSFT/Async-ASP.NET ](https://github.com/RickAndMSFT/Async-ASP.NET) sul [GitHub](https://github.com/) sito. Il repository è costituito da tre progetti:

- *WebAppAsync*: progetto di Web Form ASP.NET che utilizza l'API Web **WebAPIpwg** servizio. La maggior parte del codice per questa esercitazione è da questo progetto.
- *WebAPIpgw*: progetto API Web di ASP.NET MVC 4 che implementa il `Products, Gizmos and Widgets` controller. Fornisce i dati per il *WebAppAsync* progetto e *Mvc4Async* progetto.
- *Mvc4Async*: progetto di ASP.NET MVC 4 che contiene il codice usato in un'altra esercitazione. Effettua chiamate API Web per il **WebAPIpwg** servizio.

## <a id="GizmosSynch"></a>  La pagina sincrono Gizmos

 Il codice seguente illustra il `Page_Load` metodo sincrono che consente di visualizzare un elenco di gizmos. (Per questo articolo, un gizmo è un dispositivo meccanico fittizio). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Il codice seguente illustra il `GetGizmos` metodo del servizio gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

Il `GizmoService GetGizmos` metodo passa un URI a un servizio HTTP di ASP.NET Web API che restituisce un elenco di dati gizmos. Il *WebAPIpgw* progetto contiene l'implementazione dell'API Web `gizmos, widget` e `product` controller.  
La figura seguente mostra la pagina gizmos dal progetto di esempio.

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Creazione di una pagina Gizmos asincrona

Nell'esempio viene utilizzato il nuovo [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) parole chiave (disponibile in .NET 4.5 e Visual Studio 2012) per consentire al compilatore di essere responsabile della manutenzione necessari per le trasformazioni complesse programmazione asincrona. Il compilatore consente di scrivere codice utilizzando che i costrutti sincrona il di # controllo di flusso e il compilatore applica automaticamente le trasformazioni necessarie per l'utilizzo di callback per evitare di bloccare i thread.

Le pagine asincrone ASP.NET devono includere il [pagina](https://msdn.microsoft.com/library/ydy4x04a.aspx) direttiva con la `Async` attributo impostato su "true". Il codice seguente illustra il [pagina](https://msdn.microsoft.com/library/ydy4x04a.aspx) direttiva con la `Async` attributo impostato su "true" per il *GizmosAsync.aspx* pagina.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Il codice seguente illustra il `Gizmos` sincrono `Page_Load` (metodo) e `GizmosAsync` pagina asincrona. Se il browser supporta il [HTML 5 &lt;contrassegnare&gt; elemento](http://www.w3.org/wiki/HTML/Elements/mark), si noterà che le modifiche in `GizmosAsync` in evidenziazione di colore giallo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

La versione asincrona:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Sono state applicate le seguenti modifiche per consentire il `GizmosAsync` pagina essere asincrono.

- Il [pagina](https://msdn.microsoft.com/library/ydy4x04a.aspx) deve avere il `Async` attributo impostato su "true".
- Il `RegisterAsyncTask` metodo viene utilizzato per registrare un'attività asincrona che contiene il codice che viene eseguito in modo asincrono.
- Il nuovo `GetGizmosSvcAsync` metodo è contrassegnato con il [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) (parola chiave), che indica al compilatore di generare i callback per parti del corpo e di creare automaticamente un `Task` restituito.
- &quot;Async&quot; è stato aggiunto al nome del metodo asincrono. Aggiungere "Async" non è obbligatorio ma è la convenzione durante la scrittura di metodi asincroni.
- Il tipo restituito del nuovo `GetGizmosSvcAsync` metodo `Task`. Il tipo restituito di `Task` rappresenta il lavoro in corso e fornisce i chiamanti del metodo con un handle tramite cui si desidera attendere il completamento dell'operazione asincrona.
- Il [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (parola chiave) è stato applicato a una chiamata al servizio web.
- È stata chiamata l'API del servizio web asincrona (`GetGizmosAsync`).

All'interno del `GetGizmosSvcAsync` un altro metodo asincrono, il corpo del metodo `GetGizmosAsync` viene chiamato. `GetGizmosAsync` restituisce immediatamente un `Task<List<Gizmo>>` che infine verrà completata quando i dati sono disponibili. Perché non si desidera eseguire altre operazioni finché non si dispone dei dati gizmo, il codice attende l'attività (usando il **await** parola chiave). È possibile utilizzare il **await** parola chiave solo in metodi annotati con il **async** (parola chiave).

Il **await** parola chiave non blocca il thread fino al completamento dell'attività. Registra il resto del metodo come un callback dell'attività e viene restituito immediatamente. Al termine dell'attività attesa infine, verrà richiamare il callback e quindi riprendere l'esecuzione del diritto di metodo in cui è stata interrotta. Per ulteriori informazioni sull'utilizzo di [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) parole chiave e il [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) dello spazio dei nomi, vedere il [async riferimenti](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Il codice seguente illustra il `GetGizmos` e `GetGizmosAsync` metodi.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Le modifiche asincrone sono simili a quelle apportate il **GizmosAsync** sopra. 

- La firma del metodo è stata annotata con il [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) (parola chiave), il tipo restituito è stato modificato in `Task<List<Gizmo>>`, e *Async* è stato aggiunto al nome del metodo.
- Asincrona [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) classe viene utilizzata invece sincroni [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) classe.
- Il [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (parola chiave) è stato applicato al [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) metodo asincrono.

Nella figura seguente mostra la visualizzazione gizmo asincrona.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

La presentazione del browser dei dati gizmos è identica alla visualizzazione creata tramite la chiamata sincrona. L'unica differenza è che la versione asincrona potrebbe essere più efficiente in presenza di carichi.

## <a name="registerasynctask-notes"></a>Note RegisterAsyncTask

Metodi agganciato con `RegisterAsyncTask` verrà eseguito immediatamente dopo [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). È anche possibile utilizzare gli eventi di pagina void async direttamente, come illustrato nel codice seguente:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Lo svantaggio di async void eventi è che gli sviluppatori non dispone più il controllo completo durante l'esecuzione degli eventi. Ad esempio, se un con estensione aspx e un. Definire master `Page_Load` eventi e uno o entrambi gli elementi sono asincroni, l'ordine di esecuzione non può essere garantito. Lo stesso ordine indeterminiate per gestori eventi non (ad esempio `async void Button_Click` ) si applica. Per la maggior parte degli sviluppatori deve essere accettabile, ma gli utenti che richiedono il controllo completo sull'ordine di esecuzione devono utilizzare solo API come `RegisterAsyncTask` che utilizzano metodi che restituiscono un oggetto Task.

## <a id="Parallel"></a>  Esecuzione di più operazioni in parallelo

Quando un'azione deve eseguire diverse operazioni indipendenti, i metodi asincroni presentano un vantaggio significativo rispetto ai metodi sincroni. Nell'esempio fornito, la pagina sincrona *PWG.aspx*(per i prodotti, widget e Gizmos) consente di visualizzare i risultati di tre chiamate al servizio web per ottenere un elenco di prodotti, widget e gizmos. Il [ASP.NET Web API](../../../web-api/index.md) progetto che fornisce questi servizi utilizza [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) per simulare una rete lenta o latenza di chiamate. Quando il ritardo è impostato su 500 millisecondi, asincrona *PWGasync.aspx* pagina richiede un po' oltre 500 millisecondi per completare durante sincroni `PWG` versione assume 1500 millisecondi. Sincroni *PWG.aspx* pagina è illustrata nel codice seguente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Asincrona `PWGasync` code-behind è illustrato di seguito.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

La figura seguente mostra la visualizzazione restituita da asincrona *PWGasync.aspx* pagina.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>  Utilizzando un Token di annullamento

I metodi asincroni che restituiscono `Task`sono annullabile, che accettano un [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parametro quando ne viene fornita con il `AsyncTimeout` attributo del [pagina](https://msdn.microsoft.com/library/ydy4x04a.aspx) direttiva. Il codice seguente illustra il *GizmosCancelAsync.aspx* pagina con un timeout di secondo.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Il codice seguente illustra il *GizmosCancelAsync.aspx.cs* file.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

Nell'applicazione di esempio fornito, selezionando il *GizmosCancelAsync* collegare le chiamate di *GizmosCancelAsync.aspx* pagina e viene illustrato l'annullamento (dal timeout) della chiamata asincrona. Poiché il tempo di ritardo è all'interno di un intervallo casuale, potrebbe essere necessario aggiornare la pagina di un paio di volte per ottenere il messaggio di errore di timeout.

## <a id="ServerConfig"></a>  Configurazione del server per chiamate al servizio Web latenza elevata concorrenza elevata

Per comprendere i vantaggi di un'applicazione web asincrono, potrebbe essere necessario apportare alcune modifiche alla configurazione del server predefinito. Tenere presente durante la configurazione e l'applicazione web asincrona di test di stress quanto segue.

- Windows 7, Windows Vista, Windows 8 e tutti i sistemi operativi client Windows è possibile avere un massimo di 10 richieste simultanee. È necessario un sistema operativo Windows Server per visualizzare i vantaggi dei metodi asincroni un carico elevato.
- Registrare .NET 4.5 con IIS prompt dei comandi con privilegi elevati tramite il comando seguente:  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
  Vedere [strumento di registrazione IIS ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Potrebbe essere necessario aumentare il [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limite della coda da 1.000 a 5.000 il valore predefinito. Se l'impostazione è troppo basso, può vedere [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rifiutare le richieste con stato HTTP 503. Per modificare il limite della coda HTTP. sys:

    - Aprire Gestione IIS e passare al riquadro pool di applicazioni.
    - Fare clic con il pulsante destro sul pool di applicazioni di destinazione e selezionare **impostazioni avanzate**.  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - Nel **impostazioni avanzate** nella finestra di dialogo Modifica *lunghezza coda* da 1.000 a 5.000.  
        ![Lunghezza coda](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Si noti nell'immagine precedente, .NET framework è elencato come v 4.0, anche se il pool di applicazioni utilizza .NET 4.5. Per comprendere questa discrepanza, vedere gli argomenti seguenti:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Se l'applicazione utilizza servizi web o System.NET per comunicare con un back-end tramite HTTP si potrebbe essere necessario aumentare il [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elemento. Per le applicazioni ASP.NET, questa è limitata dalla caratteristica di configurazione automatica per 12 volte il numero di CPU. Ciò significa che un quattro processori, è possibile avere al massimo 12 \* 4 = 48 connessioni simultanee a un punto finale IP. Poiché questo è collegato a [configurazione automatica WLAN](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), il modo più semplice per aumentare `maxconnection` in ASP.NET consiste nell'impostare l'applicazione [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) a livello di codice in dal `Application_Start` metodo il *Global. asax* file. Vedere l'esempio di download per un esempio.
- In .NET 4.5, il valore predefinito di 5000 per [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) dovrebbe costituire un problema.

## <a name="contributors"></a>Contributors

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Blog di Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
