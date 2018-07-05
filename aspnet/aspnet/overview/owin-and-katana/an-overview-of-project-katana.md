---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Una panoramica del progetto Katana | Microsoft Docs
author: howarddierking
description: Il Framework ASP.NET ormai da oltre dieci anni, e la piattaforma è abilitato lo sviluppo di innumerevoli siti Web e servizi. Come applicazione Web...
ms.author: aspnetcontent
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: a4c7d6cb5dc40ad68280a87acd89f60106a260a8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803094"
---
<a name="an-overview-of-project-katana"></a>Una panoramica del progetto Katana
====================
da [Howard Dierking](https://github.com/howarddierking)

> Il Framework ASP.NET ormai da oltre dieci anni, e la piattaforma è abilitato lo sviluppo di innumerevoli siti Web e servizi. Come si sono evolute strategie di sviluppo di applicazioni Web, il framework è stato in grado di evolvere nel passaggio con tecnologie quali ASP.NET MVC e API Web ASP.NET. Come lo sviluppo di applicazioni Web richiede il passaggio successivo evolutivo nel mondo del cloud computing, proiettare [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) fornisce un set di componenti per le applicazioni ASP.NET, consentendo loro di essere flessibile e portabile, sottostante caricamento leggero e garantiscono prestazioni migliori, in altri termini, progetti [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) cloud consente di ottimizzare le applicazioni ASP.NET.


## <a name="why-katana--why-now"></a>Il motivo per cui Katana: perché ora?

 Indipendentemente dal fatto che uno è illustrare un prodotto di framework o degli utenti finali per gli sviluppatori, è importante comprendere le motivazioni sottostante per la creazione del prodotto – e la parte che include informazioni relative all'identità il prodotto è stato creato per. ASP.NET è stato originariamente creato con due clienti presente.   
  
**Il primo gruppo di clienti era classici sviluppatori ASP hanno.** Al momento, ASP era una delle principali tecnologie per la creazione dinamica, basati sui dati siti Web e applicazioni da incrocio markup e script lato server. Il runtime ASP forniti script sul lato server con un set di oggetti estratti gli aspetti fondamentali del protocollo HTTP sottostante e il server Web e fornito l'accesso ad altri servizi di tale gestione dello stato sessione e applicazione, memorizzare nella cache e così via. Sebbene siano strumenti potenti, le applicazioni ASP classiche è diventata una sfida per la gestione con l'aumento di dimensioni e complessità. Si tratta principalmente a causa della mancanza di struttura trovato in ambienti associati la duplicazione del codice risultante dall'interfoliazione del markup e codice di script. Per poter sfruttare al meglio le potenzialità di ASP classico durante la risoluzione di alcuni dei problemi, ASP.NET hanno sfruttato i vantaggi dell'organizzazione codice fornita dai linguaggi orientate a oggetti di .NET Framework mantenendo anche il modello di programmazione lato server per quali ASP classico è stata estesa agli sviluppatori abituati.

**Il secondo gruppo di clienti di destinazione per ASP.NET è stata agli sviluppatori di applicazioni di business di Windows.** A differenza dei classici sviluppatori ASP, che sono abituati a scrivere il markup HTML e il codice per generare il markup HTML più, gli sviluppatori di Windows Form (ad esempio, gli sviluppatori di VB6 prima li) abituati a una fase di progettazione che include un'area di disegno e un set completo di utente controlli dell'interfaccia. La prima versione di ASP.NET – noto anche come "Web Forms" fornito un'esperienza di fase di progettazione simile insieme a un modello di eventi sul lato server per i componenti dell'interfaccia utente e un set di funzionalità dell'infrastruttura (ad esempio ViewState) per creare un'esperienza di sviluppo senza problemi tra client e la programmazione lato server. Web Form nascosta in modo efficace natura apolide del Web con un modello di eventi con stato che fosse familiare agli sviluppatori di Windows Form.

### <a name="challenges-raised-by-the-historical-model"></a>Sfide generate dal modello cronologico

**Il risultato finale era un runtime maturo, ricco e modello di programmazione per gli sviluppatori.** Tuttavia, con cui-ricchezza della funzionalità ha introdotto due sfide significative. In primo luogo, il framework è stata **monolitica**, con le unità in modo logico diversi di funzionalità risultare strettamente accoppiati nello stesso assembly DLL (ad esempio, gli oggetti di base HTTP con il framework di Web Form). In secondo luogo, ASP.NET è stata inclusa come parte del più grande .NET Framework, che significava che il **tempo tra le versioni: nell'ordine di anni.** Ciò rendeva difficile per ASP.NET per restare al passo con tutte le modifiche apportate in rapida evoluzione lo sviluppo Web. Infine, è stato accoppiato DLL stessa in modi diversi per una specifica opzione di hosting Web: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Fasi evolutive: ASP.NET MVC e API Web ASP.NET

E un numero elevato di modifiche è stato in corso nello sviluppo Web. Sono state sviluppate applicazioni Web sempre più come una serie di piccole dimensioni, con stato attivo componenti piuttosto che come Framework di grandi dimensioni. È stata aumentando il numero di componenti, nonché la frequenza con cui sono stati rilasciati con una frequenza più veloce che mai. Era chiaro che tenere il passo con il Web richiederebbero infrastrutture per ottenere più mirato e limitato, disaccoppiati anziché più grandi e più ricche di funzionalità, pertanto il **team ASP.NET ha impiegato diverse fasi evolutive abilitare ASP.NET come una famiglia di componenti Web modulari anziché un unico framework**.

Una delle prime modifiche è stato il crescente popolarità del noto modello di progettazione model-view-controller (MVC) grazie al framework di sviluppo Web quali Ruby on Rails. Questo stile di creazione di applicazioni Web assegnato allo sviluppatore un maggiore controllo sulla markup dell'applicazione mantenendo al tempo stesso la separazione della logica di business e markup, che era uno dei punti vendita iniziali per ASP.NET. Per soddisfare la richiesta per questo stile di sviluppo di applicazioni Web, Microsoft ha avuto l'opportunità per posizionarsi migliori per il futuro da **lo sviluppo di ASP.NET MVC fuori banda** (e non incluso in .NET Framework). ASP.NET MVC è stata rilasciata come download indipendente. Il team di progettazione ha offerto la flessibilità necessaria per distribuire gli aggiornamenti molto più frequentemente di quanto è stato possibile in precedenza.

Un altro cambiamento importante nello sviluppo di applicazioni Web è stata lo spostamento da pagine Web dinamiche, generati dal server al markup iniziali statico con sezioni dinamiche della pagina generata da uno script lato client che comunicano **con back-end API Web tramite Le richieste AJAX**. Questo cambiamento architettonico aiutato sospingere l'aumento delle API Web e lo sviluppo del framework API Web ASP.NET. Come nel caso di ASP.NET MVC, la versione dell'API Web ASP.NET fornito un'altra opportunità per sviluppare ulteriormente come un framework più modulare ASP.NET. Il team tecnico hanno sfruttato i vantaggi delle opportunità e **compilati API Web ASP.NET in modo che lo ha senza dipendenze in uno dei tipi di framework di core disponibili in DLL**. Questa opzione abilitata due cose: innanzitutto, lo scopo quello che ASP.NET Web API è stato possibile evolvere in modo completamente autonomo (e è stato possibile continuare a eseguire l'iterazione rapidamente perché viene fornito tramite NuGet). In secondo luogo, poiché si sono verificati senza dipendenze esterne a DLL e conseguenza, nessuna dipendenza da IIS, ASP.NET Web API inclusa la possibilità di eseguire in un host personalizzato (ad esempio, un'applicazione console del servizio di Windows, e così via.)

### <a name="the-future-a-nimble-framework"></a>Il futuro: Un Framework Agile

Disaccoppiamento di componenti di framework uno da altro e quindi rilasciarli in NuGet, potrebbero ora frameworks **eseguire l'iterazione in modo più rapido e più in modo indipendente**. Inoltre, la potenza e la flessibilità della funzionalità self-hosting dell'API Web si è rivelata molto interessante per gli sviluppatori che pensano un **piccoli e leggero host** per i propri servizi. Ha dimostrato così interessante, infatti, che gli altri framework voleva anche questa funzionalità e questo viene rilevata una nuova richiesta di verifica in quanto ogni framework è stato eseguito nel relativo processo host in un proprio indirizzo di base e deve essere gestito (avvio, arresto e così via) in modo indipendente. Un'applicazione Web moderna in genere supporta l'uso del file statico, la generazione di pagine dinamiche, API Web e più recentemente real-time e notifiche push. È previsto che ognuno di questi servizi debba essere eseguito e gestito in modo indipendente non è stato semplicemente realistico.

Ciò che è stato necessario era una singola astrazione di hosting che consentono agli sviluppatori di comporre un'applicazione da un'ampia gamma di diversi componenti e Framework e quindi eseguire tale applicazione in un host di supporto.

## <a name="the-open-web-interface-for-net-owin"></a>L'interfaccia Web aperta per .NET (OWIN)

 Lasciati guidare dai vantaggi a tale scopo [Rack](http://rack.github.io/) nella community di Ruby, diversi membri della community .NET hanno deciso di creare un'astrazione tra i server Web e componenti del framework. Due obiettivi di progettazione per l'astrazione di OWIN sono state che è stato semplice e che lo ha richiesto il minor numero di possibili dipendenze in altri tipi di framework. Questi due obiettivi garantire:

- Nuovi componenti può essere sviluppati più facilmente e utilizzati.
- Le applicazioni è stato possibile eseguire più facilmente il porting tra gli host e potenzialmente intero piattaforme/sistemi operativi.

L'astrazione risulta è costituito da due elementi principali. Il primo è il dizionario dell'ambiente. Questa struttura di dati è responsabile per l'archiviazione dello stato necessario per l'elaborazione di una richiesta HTTP e risposta, nonché qualsiasi stato server pertinente. Il dizionario dell'ambiente viene definito come segue:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Un server Web compatibile con OWIN è responsabile del popolamento con dati, ad esempio i flussi del corpo e le raccolte di intestazioni per una richiesta HTTP e una risposta il dizionario dell'ambiente. È quindi responsabilità dei componenti dell'applicazione o framework per compilare o aggiornare il dizionario con i valori aggiuntivi e scrivere il flusso del corpo della risposta.

Oltre a specificare il tipo per il dizionario dell'ambiente, la specifica di OWIN definisce un elenco di coppie chiave-valore dizionario principale. Ad esempio, nella tabella seguente mostra le chiavi del dizionario necessari per una richiesta HTTP:

| Nome della chiave | Valore descrizione |
| --- | --- |
| `"owin.RequestBody"` | Stream con il corpo della richiesta, se presente. Stream può essere usato come segnaposto se non vi è alcun corpo della richiesta. Visualizzare [corpo della richiesta](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Un `IDictionary<string, string[]>` delle intestazioni di richiesta. Visualizzare [intestazioni](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Oggetto `string` che contiene il metodo di richiesta HTTP della richiesta (ad esempio `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Oggetto `string` contenente il percorso della richiesta. Il percorso deve essere relativo alla "radice" del delegato dell'applicazione; visualizzare [percorsi](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Oggetto `string` che contiene la parte del percorso di richiesta corrispondente a "radice" del delegato dell'applicazione; vedere [percorsi](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Oggetto `string` che contiene il nome del protocollo e la versione (ad esempio `"HTTP/1.0"` o `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Oggetto `string` contenente il componente stringa di query dell'URI, della richiesta HTTP senza il prefisso "?" (ad esempio, `"foo=bar&baz=quux"`). Il valore può essere una stringa vuota. |
| `"owin.RequestScheme"` | Oggetto `string` contenente lo schema URI utilizzato per la richiesta (ad esempio `"http"`, `"https"`); vedere [schema URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Il secondo elemento chiave di OWIN è il delegato dell'applicazione. Si tratta di una firma della funzione che funge da interfaccia primaria tra tutti i componenti in un'applicazione OWIN. La definizione per il delegato dell'applicazione è come segue:

`Func<IDictionary<string, object>, Task>;`

Il delegato dell'applicazione, è semplicemente un'implementazione del tipo delegato Func in cui la funzione accetta il dizionario dell'ambiente come input e restituisce un'attività. Questa progettazione ha diverse implicazioni per gli sviluppatori:

- Sono presenti un numero molto ridotto di dipendenze dei tipi necessari per poter scrivere i componenti OWIN. In questo modo aumentano l'accessibilità di OWIN per gli sviluppatori.
- La progettazione asincrona consente l'astrazione efficienza con la gestione delle risorse di elaborazione, in particolare in altre operazioni con utilizzo intensivo dei / o.
- Poiché il delegato dell'applicazione è un'unità atomica di esecuzione e poiché il dizionario dell'ambiente viene trasportato in un parametro nel delegato, i componenti OWIN possano essere facilmente concatenati insieme per creare l'elaborazione pipeline HTTP complesse.

Da una prospettiva di implementazione, OWIN è una specifica ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). L'obiettivo del modello non deve essere il framework Web successivo, ma piuttosto una specifica per l'interagiscono tra i framework Web e server Web.

Se è stato esaminato [OWIN](http://owin.org/) oppure [Katana](https://github.com/aspnet/AspNetKatana/wiki), è inoltre possibile aver notato il [pacchetto NuGet di Owin](http://nuget.org/packages/Owin) e Owin.dll. Questa libreria contiene un'unica interfaccia [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), che formalizza e consente di codificare la sequenza di avvio descritto in [sezione 4](http://owin.org/html/owin.html#4-application-startup) delle specifiche OWIN. Sebbene non sia necessario per compilare i server OWIN, il [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) interfaccia fornisce un punto di riferimento concreto e viene usato dai componenti del progetto Katana.

## <a name="project-katana"></a>Progetto Katana

Mentre entrambi i [OWIN](http://owin.org/html/owin.html) specifica e *Owin.dll* sono di proprietà e Comunità eseguire gli sforzi open source, il [Katana](https://github.com/aspnet/AspNetKatana/wiki) progetto rappresenta il set di OWIN componenti che, sebbene ancora open source e sono compilati e rilasciati da Microsoft. Questi componenti includono, ad esempio componenti dell'infrastruttura, ad esempio gli host e server, oltre a componenti funzionali, ad esempio componenti dell'autenticazione e le associazioni ai frameworks [SignalR](../../../signalr/index.md) e [Web ASP.NET API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Il progetto ha i seguenti tre obiettivi di alto livelli: 

- **Portabile** – componenti devono essere in grado di essere facilmente sostituiti per i componenti di nuovo appena diventano disponibili. Ciò include tutti i tipi di componenti, dal framework per il server e l'host. L'implicazione di questo obiettivo è che Framework di terze parti possono essere eseguiti senza problemi nei server Microsoft mentre Microsoft Framework possono essere potenzialmente eseguiti su host e server di terze parti.
- **Modulare/flessibile**: a differenza di molti Framework che includono una varietà di funzionalità che sono attivati per impostazione predefinita, i componenti del progetto Katana devono essere più circoscritto e mirato, fornendo un controllo allo sviluppatore di applicazioni per determinare quali componenti utilizzare dall'applicazione.
- **Lightweight/ad alte prestazioni/scalabile** : suddividendo il concetto tradizionale di un framework in un set di piccole dimensioni, con stato attivo di componenti che vengono aggiunti in modo esplicito dallo sviluppatore dell'applicazione, un'applicazione Katana risulta può utilizzare un numero inferiore di elaborazione le risorse e di conseguenza, gestire un carico maggiore, rispetto ad altri tipi di server e i Framework. Come i requisiti dell'applicazione richiedono altre funzionalità dall'infrastruttura sottostante, quelli possono essere aggiunti alla pipeline OWIN, ma che deve essere una decisione esplicita da parte dello sviluppatore dell'applicazione. Inoltre, il sostituibilità dei componenti di livello inferiore significa che appena diventano disponibili, nuovi server ad alte prestazioni può essere introdotta senza problemi per migliorare le prestazioni delle applicazioni OWIN senza interrompere le applicazioni.

## <a name="getting-started-with-katana-components"></a>Introduzione a componenti Katana

Quando è stato introdotto per primo, un aspetto del [Node. js](http://nodejs.org/) framework che ha tracciato immediatamente l'attenzione è stata la semplicità con cui uno è stato possibile creare ed eseguire un server Web. Se sono stati inseriti in un frame in luce dell'obiettivi Katana [Node. js](http://nodejs.org/), uno potrebbe riportarli affermando che Katana offre molti dei vantaggi offerti [Node. js](http://nodejs.org/) (e i Framework, ad esempio,) senza imporre agli sviluppatori di eliminare gli tutto ciò che sa sullo sviluppo di applicazioni Web ASP.NET. Per questa istruzione restituire true, Introduzione al Katana project dovrebbe essere altrettanto semplice in natura [Node. js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Creazione di "Hello World!"

Una differenza rilevante tra lo sviluppo di JavaScript e .NET è il presenza o assenza di un compilatore. Di conseguenza, il punto di partenza per un semplice server Katana è un progetto di Visual Studio. Tuttavia, è possibile iniziare con la minima dei tipi di progetto: l'applicazione Web ASP.NET vuota.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Successivamente, verrà installato il [systemweb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) pacchetto NuGet nel progetto. Questo pacchetto fornisce un server OWIN che viene eseguito nella pipeline delle richieste ASP.NET. È disponibile nel [raccolta NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) e può essere installato usando la finestra di dialogo Gestione pacchetti Visual Studio o la console di gestione pacchetti con il comando seguente:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Installazione di `Microsoft.Owin.Host.SystemWeb` del pacchetto installerà alcuni pacchetti aggiuntivi come dipendenze. Uno di tali dipendenze è `Microsoft.Owin`, una libreria che offre diversi tipi di helper e metodi per lo sviluppo delle applicazioni OWIN. È possibile usare questi tipi di scrivere rapidamente il server di "hello world" seguente.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

È possibile eseguire il server Web molto semplice con Visual Studio **F5** di comandi e include il supporto completo per il debug.

## <a name="switching-hosts"></a>Cambio di host

Per impostazione predefinita, il precedente esempio di "hello world" viene eseguito nella pipeline delle richieste ASP.NET, che usa System. Web nel contesto di IIS. Ciò può da solo aggiungere un notevole valore perché ci permette di usufruire della flessibilità componibilità di una pipeline OWIN con le funzionalità di gestione e la maturità complessiva di IIS. Tuttavia, potrebbero esserci casi in cui non sono necessari i vantaggi forniti da IIS e il desiderio è per un host più piccolo e più leggero. Che cos'è necessaria, quindi, per eseguire la semplice server Web di fuori di IIS e System. Web?

Per illustrare l'obiettivo di portabilità, lo spostamento da un host del server Web a una riga di comando richiede aggiungendo semplicemente le dipendenze di server e host nuova cartella di output del progetto e quindi avviare l'host. In questo esempio verranno ospitati i server Web in un host di Katana denominato `OwinHost.exe` e utilizzerà il server basati su Katana HttpListener. Analogamente ad altri componenti di Katana, questi viene acquisiti da NuGet usando il comando seguente:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Dalla riga di comando, è possibile quindi passare alla cartella radice del progetto ed eseguire semplicemente il `OwinHost.exe` (che è stato installato nella cartella degli strumenti del relativo pacchetto NuGet corrispondente). Per impostazione predefinita, `OwinHost.exe` è configurato per cercare il server basati su HttpListener e, pertanto non è necessaria alcuna configurazione aggiuntiva. Spostamenti all'interno di un Web browser per `http://localhost:5000/` indica che l'applicazione in esecuzione tramite la console.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architettura di Katana

 L'architettura dei componenti Katana divide un'applicazione in quattro livelli logici, come illustrato di seguito: *host, server, middleware* e *applicazione*. L'architettura dei componenti è inserito in modo che le implementazioni di questi livelli possono essere facilmente sostituite, in molti casi, senza richiedere la ricompilazione dell'applicazione.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 L'host è responsabile per:

- La gestione del processo sottostante.
- Orchestrare il flusso di lavoro che comporta la selezione di un server e la creazione di una pipeline OWIN tramite le richieste verranno gestite.

  Al momento, esistono 3 opzioni di hosting primarie per le applicazioni basate su Katana:  
  
**Accoppiato**: Usa i tipi di HttpModule e HttpHandler standard, le pipeline OWIN eseguibili in IIS come parte di un flusso di richieste ASP.NET. Supporto per l'hosting di ASP.NET è abilitata per l'installazione del pacchetto Microsoft.AspNet.Host.SystemWeb NuGet in un progetto di applicazione Web. Inoltre, poiché IIS funge da un host e un server, la distinzione di server/host OWIN è conflated nel pacchetto NuGet, vale a dire che se si usa l'host SystemWeb, uno sviluppatore non può sostituire un'implementazione del server alternativo.  
  
**Host personalizzato**: Katana la suite di componenti offre agli sviluppatori la possibilità di ospitare applicazioni nel proprio processo personalizzato, se è un'applicazione console del servizio di Windows, e così via. Questa funzionalità è simile alla funzionalità self-hosting fornita dall'API Web. L'esempio seguente illustra un host personalizzato del codice dell'API Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

La configurazione Self-hosting di un'applicazione Katana è simile:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Una differenza rilevante tra gli esempi di hosting indipendente dell'API Web e Katana è che il codice di configurazione di API Web è presente nell'esempio Katana self-hosting. Per consentire la portabilità sia componibilità, Katana separa il codice che avvia il server dal codice che consente di configurare la pipeline di elaborazione delle richieste. Il codice che consente di configurare Web API, quindi è contenuto nella classe di avvio, viene inoltre specificato come parametro di tipo in WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

La classe di avvio verrà descritta più dettagliatamente più avanti nell'articolo. Tuttavia, il codice necessario per avviare un Katana processo self-hosting Cerca sorprendentemente simile al codice che si stia utilizzando attualmente nelle applicazioni di hosting indipendente dell'API Web ASP.NET.

**OwinHost.exe**: mentre alcune desidera scrivere un processo personalizzato per eseguire applicazioni Katana Web, molti vuole semplicemente avviare un eseguibile predefinito che è possibile avviare un server ed eseguire l'applicazione. In questo scenario include la suite di componenti Katana `OwinHost.exe`. Quando eseguito dall'interno directory radice del progetto, questo file eseguibile verrà avviare un server (utilizza il server di HttpListener per impostazione predefinita) e usare le convenzioni per trovare ed eseguire la classe di avvio dell'utente. Per un controllo più granulare, il file eseguibile fornisce una serie di parametri della riga di comando aggiuntivi.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Server

 Mentre l'host è responsabile dell'avvio e la gestione di processo entro il quale viene eseguita l'applicazione, la responsabilità del server è per aprire un socket di rete, attendere le richieste e inviare loro attraverso la pipeline di componenti OWIN specificati dall'utente (come si è già notato, questa pipeline è specificata nella classe di avvio dello sviluppatore dell'applicazione). Attualmente, il progetto Katana include due implementazioni di server: 

- **Systemweb**: come accennato in precedenza, IIS in concerto con la pipeline ASP.NET funge da un host e un server. Pertanto, quando si sceglie questa opzione di hosting, IIS gestisce i problemi a livello di host, ad esempio l'attivazione del processo sia è in ascolto delle richieste HTTP. Per le applicazioni Web ASP.NET, quindi invia le richieste alla pipeline ASP.NET. L'host Katana SystemWeb registra un ASP.NET HttpModule e HttpHandler per intercettare le richieste che passano attraverso la pipeline HTTP e inviarli tramite la pipeline OWIN specificato dall'utente.
- **Microsoft.Owin.Host.HttpListener**: come indica il nome, questo server Katana Usa classi di HttpListener di .NET Framework per aprire un socket e inviare le richieste in una pipeline OWIN specificati dallo sviluppatore. È attualmente la selezione di server predefinito per OwinHost.exe sia il Katana hosting indipendente dell'API.

## <a name="middlewareframework"></a>Middleware/framework

 Come accennato in precedenza, quando il server accetta una richiesta da un client, è responsabile per passarlo tramite una pipeline di componenti OWIN, che sono specificati dal codice di avvio per gli sviluppatori. Questi componenti della pipeline sono note come middleware.  
 A un livello molto semplice, un componente del middleware OWIN deve semplicemente implementare il delegato dell'applicazione OWIN, in modo che sia possibile chiamare.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Tuttavia, per semplificare lo sviluppo e la composizione dei componenti middleware, Katana supporta un numero limitato di convenzioni e i tipi di supporto per i componenti middleware. La più comune di questi è il `OwinMiddleware` classe. Un componente middleware personalizzato creato utilizzando questa classe avrebbe un aspetto simile al seguente: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Questa classe deriva da `OwinMiddleware`, implementa un costruttore che accetta un'istanza di middleware successivo nella pipeline come uno dei relativi argomenti e quindi lo passa al costruttore di base. Specificati argomenti aggiuntivi utilizzati per configurare il middleware vengono dichiarati come parametri del costruttore dopo il parametro middleware successivo.   
  
In fase di esecuzione, viene eseguito il middleware tramite sottoposto a override `Invoke` (metodo). Questo metodo accetta un singolo argomento di tipo `OwinContext`. Questo oggetto di contesto viene fornito per il `Microsoft.Owin` pacchetto NuGet descritto in precedenza e fornisce accesso fortemente tipizzato al dizionario di richiesta, risposta e l'ambiente, insieme ad alcuni tipi di supporto aggiuntivi.   
  
La classe del middleware può essere facilmente aggiunti alla pipeline OWIN nel codice di avvio dell'applicazione come indicato di seguito:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Poiché l'infrastruttura di Katana crea semplicemente una pipeline di componenti del middleware OWIN e perché i componenti sufficiente supportare il delegato dell'applicazione per partecipare alla pipeline, i componenti middleware possono variare in complessità semplice logger da intero Framework come ASP.NET, API Web, oppure [SignalR](../../../signalr/index.md). Ad esempio, l'aggiunta di API Web ASP.NET alla pipeline OWIN precedente richiede aggiungendo il seguente codice di avvio:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

L'infrastruttura di Katana compilerà la pipeline dei componenti middleware in base all'ordine in cui sono stati aggiunti all'oggetto IAppBuilder nel metodo di configurazione. In questo esempio, quindi, LoggerMiddleware può gestire tutte le richieste che passano attraverso la pipeline, indipendentemente dalla modalità di gestione in definitiva queste richieste. Ciò consente di realizzare scenari potenti in cui un componente del middleware (ad esempio, un componente di autenticazione) possa elaborare le richieste per una pipeline che include più componenti e Framework (ad esempio, API Web ASP.NET SignalR e un server di file statici).
 
## <a name="applications"></a>Applicazioni

Come illustrato negli esempi precedenti, OWIN e Katana project dovrebbe non essere considerati come un nuovo modello di programmazione dell'applicazione, ma piuttosto come un'astrazione per separare i modelli di programmazione dell'applicazione e i Framework da server e infrastruttura di hosting. Ad esempio, quando si compilano applicazioni API Web, il framework per sviluppatori continuerà a usare il framework API Web ASP.NET, indipendentemente dal fatto se l'applicazione viene eseguita in una pipeline OWIN tramite i componenti dal progetto Katana. Di un'unica posizione in cui saranno visibili allo sviluppatore di applicazioni basate su OWIN codice sarà il codice di avvio dell'applicazione, in cui lo sviluppatore compone la pipeline OWIN. Nel codice di avvio, lo sviluppatore registrerà una serie di istruzioni UseXx, in genere uno per ogni componente del middleware che elaborerà le richieste in ingresso. Questa esperienza avrà lo stesso effetto la registrazione di moduli HTTP nel mondo System. Web corrente. In genere, un middleware framework più grande, ad esempio API Web ASP.NET oppure [SignalR](../../../signalr/index.md) verrà registrato alla fine della pipeline. I componenti middleware a montaggio incrociato, ad esempio quelli per l'autenticazione o la memorizzazione nella cache, vengono registrati a livello generale verso l'inizio della pipeline in modo che si elaborerà le richieste per tutti i Framework e i componenti registrati in un secondo momento nella pipeline. Questa separazione dei componenti middleware uno da altro e verso i componenti dell'infrastruttura sottostante consente i componenti di evolversi a velocità crescenti diversi, garantendo che tutto il sistema rimane stabile.

## <a name="components--nuget-packages"></a>Componenti, ovvero i pacchetti NuGet

Ad esempio molte librerie corrente e Framework, i componenti del progetto Katana vengono recapitati come un set di pacchetti NuGet. Per la prossima versione 2.0, il grafico delle dipendenze di pacchetto Katana è simile al seguente. (Fare clic sull'immagine per visualizzarla completamente).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Quasi ogni pacchetto nel progetto Katana dipende, direttamente o indirettamente, il pacchetto di Owin. Come probabilmente si ricorderà che questo è il pacchetto che contiene l'interfaccia di IAppBuilder, che fornisce un'implementazione concreta della sequenza di avvio dell'applicazione descritta nella sezione 4 di alle specifiche OWIN. Inoltre, molti dei pacchetti dipendono owin, che fornisce un set di tipi di helper per l'uso di richieste e risposte HTTP. Il resto del pacchetto può essere classificato come hosting dei pacchetti di infrastruttura (server o host) o middleware. I pacchetti e dipendenze esterne al progetto Katana vengono visualizzate in arancione.

L'infrastruttura di hosting per Katana 2.0 include sia il SystemWeb e i server basati su HttpListener, il pacchetto OwinHost per l'esecuzione delle applicazioni OWIN tramite OwinHost.exe e del pacchetto di owin per la modalità self-hosting OWIN applicazioni in un host personalizzato (ad esempio, applicazione console del servizio di Windows, e così via.)

Katana 2.0, i componenti middleware sono principalmente interessati al fornisce diversi metodi di autenticazione. È disponibile un componente del middleware aggiuntive per la diagnostica, che abilita il supporto per una pagina di avvio e di errore. Man mano che aumenta OWIN nell'astrazione di hosting standard de facto, l'ecosistema dei componenti middleware, sia quelle sviluppate da Microsoft e terze parti, aumenterà anche in numero.

## <a name="conclusion"></a>Conclusione

 Dall'inizio, l'obiettivo del progetto Katana non è stata per creare e imporre in tal modo gli sviluppatori per informazioni su un altro framework Web. Piuttosto, l'obiettivo è stata un'astrazione per consentire agli sviluppatori di applicazioni Web .NET più scelta che in precedenza è stato possibile creare. Suddividendo i livelli logici di uno stack di applicazioni Web tipiche in un set di componenti sostituibili, al Katana project consente ai componenti in tutto lo stack per migliorare alla tariffa di senso per tali componenti. Mediante la generazione di tutti i componenti per l'astrazione di OWIN semplice, Katana Abilita i Framework e la portabilità delle applicazioni basato su essi in un'ampia gamma di host e server diversi. Inserendo lo sviluppatore nel controllo dello stack, Katana assicura che lo sviluppatore rende la scelta finale sulla modalità caricamento leggero o ricco di funzionalità devono essere proprio stack Web.  
  

## <a name="for-more-information-about-katana"></a>Per altre informazioni su Katana

- Il Katana project su GitHub: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Video: [Katana Project - OWIN per ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), da Howard Dierking.

## <a name="acknowledgements"></a>Riconoscimenti

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick è una scrittrice senior di programmazione per Microsoft che si occupa in Azure e MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
