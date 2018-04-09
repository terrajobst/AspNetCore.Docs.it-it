---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Una panoramica del progetto Katana | Documenti Microsoft
author: howarddierking
description: Il Framework di ASP.NET è stato rilasciato per oltre 10 anni e la piattaforma è abilitato lo sviluppo di numerosi siti Web e servizi. Come applicazione Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 3c2bcbbc6e506af759f6d77af17d015278cc0bdf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="an-overview-of-project-katana"></a>Una panoramica del progetto Katana
====================
da [Howard Dierking](https://github.com/howarddierking)

> Il Framework di ASP.NET è stato rilasciato per oltre 10 anni e la piattaforma è abilitato lo sviluppo di numerosi siti Web e servizi. Come hanno strategie di sviluppo di applicazioni Web, il framework è stato in grado di sviluppare nel passaggio con tecnologie quali ASP.NET MVC e ASP.NET Web API. Progetto di sviluppo di applicazioni Web richiede il passaggio successivo evolutivo nell'ambiente di cloud computing, [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) fornisce il set di componenti per le applicazioni ASP.NET, che potranno essere portabile e flessibile sottostante leggere e garantiscono prestazioni migliori: in altri termini, progetti [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) cloud consente di ottimizzare le applicazioni ASP.NET.


## <a name="why-katana--why-now"></a>Motivo per cui Katana: perché ora?

 Indipendentemente dal fatto che uno è illustrato un prodotto per sviluppatori di framework o l'utente finale, è importante comprendere le motivazioni sottostante per la creazione del prodotto: e la parte che include sapendo che il prodotto è stato creato per. ASP.NET è stato originariamente creato con due clienti presente.   
  
**Il primo gruppo di clienti è stato sviluppatori ASP classici.** Al momento, ASP è una delle tecnologie per la creazione dinamica, basati sui dati applicazioni e siti Web tramite incrocio markup e lo script sul lato server primarie. Il runtime ASP fornito script sul lato server con un set di oggetti estratti gli aspetti principali del protocollo HTTP sottostante e del server Web e fornito l'accesso a ulteriori servizi di tale gestione dello stato sessione e dell'applicazione, memorizzare nella cache e così via. Mentre potente, le applicazioni ASP classiche è diventato difficile gestire come vengono aumentate le dimensioni e complessità. Questo è in gran parte a causa della mancanza di struttura presente negli ambienti associati la duplicazione del codice risultanti da interfoliazione di codice e codice di script. Per sfruttare appieno le potenzialità di ASP classico durante la risoluzione di alcuni dei relativi problemi, ASP.NET volevano dell'organizzazione codice fornita dai linguaggi orientata agli oggetti di .NET Framework mantenendo anche il modello di programmazione sul lato server per quali ASP classico è stata estesa gli sviluppatori abituati.

**Il secondo gruppo di clienti di destinazione per ASP.NET è stata gli sviluppatori di applicazioni di business di Windows.** A differenza degli sviluppatori ASP classici, che sono abituati alla scrittura di markup HTML e il codice per generare il markup HTML più, gli sviluppatori di Windows Form (ad esempio, gli sviluppatori VB6 precedute) abituati a una fase di progettazione che include un'area di disegno e un set completo di utente controlli dell'interfaccia. La prima versione di ASP.NET, noto anche come "Web Form" fornito un simile fase di progettazione insieme a un modello di eventi sul lato server per i componenti dell'interfaccia utente e un set di funzionalità dell'infrastruttura (ad esempio ViewState) per creare un'esperienza di sviluppo tra client e programmazione sul lato server. Web form nascosto in modo efficace l'assenza del sito Web in un modello di eventi con stato che è stato già noto agli sviluppatori di Windows Form.

### <a name="challenges-raised-by-the-historical-model"></a>Problemi generati dal modello cronologico

**Il risultato è un modello di programmazione per sviluppatori e di runtime maturo ricco di funzionalità.** Tuttavia, con cui ricchezza di funzionalità proviene alcuni problemi rilevanti. In primo luogo, il framework era **monolitico**, con le unità diverse in modo logico di funzionalità da strettamente nello stesso assembly System.Web.dll (ad esempio, gli oggetti di base HTTP con il framework di Web Form). In secondo luogo, ASP.NET è stata inclusa come parte di dimensioni maggiori .NET Framework, che ha il **tempo tra le versioni: nell'ordine di anni.** Questo rende difficile per tenere il passo con tutte le modifiche eseguite in rapida evoluzione lo sviluppo Web ASP.NET. Infine, System.Web.dll stessa è stata associata in diversi modi per un'opzione di hosting Web specifico: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Passaggi evolutivi: ASP.NET MVC e ASP.NET Web API

E un numero elevato di modifiche è stato in corso di sviluppo Web. Sono state sviluppate applicazioni Web sempre più come una serie di piccole dimensioni, con stato attivo componenti anziché il framework di grandi dimensioni. Il numero di componenti, nonché la frequenza con cui sono state rilasciate è aumentato a una velocità superiore mai. È chiaro che tenere il passo con Web richiederebbe Framework per ottenere più piccoli, separati e più mirati anziché più grandi e più ricco di funzionalità, pertanto il **ASP.NET team ha impiegato diverse operazioni evolutivi abilitare ASP.NET come una famiglia di componenti Web collegabili anziché un unico framework**.

Una delle modifiche anticipate è l'aumento della diffusione del noto modello di progettazione model-view-controller (MVC) grazie al framework di sviluppo Web come Ruby su Guide. Questo stile di compilazione di applicazioni Web assegnato allo sviluppatore maggiore controllo sul codice dell'applicazione mantenendo al tempo stesso la separazione della logica di business e markup, che è stato uno dei punti vendita iniziali per ASP.NET. Per soddisfare la richiesta per questo stile di sviluppo di applicazioni Web, Microsoft ha la possibilità di posizionarsi migliori per il futuro da **lo sviluppo di ASP.NET MVC fuori banda** (e non incluso in .NET Framework). ASP.NET MVC è stato rilasciato come download indipendente. Il team di progettazione ha fornito la flessibilità necessaria per distribuire gli aggiornamenti molto più frequentemente di quanto è stato possibile in precedenza.

Un altro cambiamento importante nello sviluppo di applicazioni Web è stato il turno dalle pagine Web dinamiche, generato dal server al markup iniziale statico con sezioni dinamiche della pagina generati da script sul lato client comunicano **con back-end Web API tramite Le richieste AJAX**. Il cambiamento di architettura consentito sospingere l'avvento di API Web e lo sviluppo del framework ASP.NET Web API. Come nel caso di ASP.NET MVC, la versione di ASP.NET Web API fornito un'altra possibilità di sviluppare ulteriormente come più modulare framework ASP.NET. Il team tecnico ha richiesto l'opportunità e **generati API Web ASP.NET in modo che non ha alcuna dipendenza in uno dei tipi di framework core disponibili in System.Web.dll**. Questa opzione abilitata due cose: in primo luogo, in genere che ASP.NET Web API Impossibile evolvere in modo completamente indipendente (e grado di continuare a scorrere rapidamente perché viene recapitato tramite NuGet). In secondo luogo, perché non erano senza dipendenze esterne a System.Web.dll e, di conseguenza alcuna dipendenza da IIS, ASP.NET Web API inclusa la possibilità di eseguire in un host personalizzato (ad esempio, un'applicazione console del servizio di Windows, e così via.)

### <a name="the-future-a-nimble-framework"></a>Il futuro: Un Framework di facile utilizzo

Separazione dei componenti di framework uno da altro e quindi rilasciarlo su NuGet, Framework potrebbero ora **eseguire l'iterazione in modo più rapido e più in modo indipendente**. Inoltre, la potenza e flessibilità di funzionalità self-hosting dell'API Web dimostrato molto interessante per gli sviluppatori che pensano un **host piccolo e leggero** per i servizi. Ha dimostrato in modo interessante, infatti, che gli altri framework voleva anche questa funzionalità e questo viene rilevata una nuova richiesta di verifica in quanto ogni framework è stato eseguito in un processo host il proprio indirizzo di base e deve essere gestito (avvio, arresto e così via) in modo indipendente. In genere, un'applicazione Web moderna supporta più recente reale-tempo/notifiche push, la generazione della pagina dinamica, API Web e gestione di file statici. È previsto che ognuno di questi servizi debba essere eseguito e gestito in modo indipendente non è stato semplicemente realistico.

Cosa è stato necessario è un'astrazione di hosting singola che consentono allo sviluppatore di creare un'applicazione da un'ampia gamma di diversi componenti e Framework e quindi eseguire tale applicazione in un host di supporto.

## <a name="the-open-web-interface-for-net-owin"></a>L'interfaccia Web aperta per .NET (OWIN)

 Ispirato i vantaggi scopo [Rack](http://rack.github.io/) della community di Ruby, impostare diversi membri della community .NET per creare un'astrazione tra i server Web e componenti .NET framework. Due obiettivi di progettazione per l'astrazione OWIN sono stati che si tratta di semplice e impiegato il minor numero di possibili dipendenze in altri tipi di framework. Questi due obiettivi garantire:

- Nuovi componenti Impossibile più facilmente sviluppati e utilizzati.
- Le applicazioni può essere trasferite più facilmente tra gli host e potenzialmente intera piattaforme/sistemi operativi.

L'astrazione risulta è costituito da due elementi principali. Il primo è il dizionario dell'ambiente. Questa struttura di dati è responsabile per archiviare lo stato necessario per l'elaborazione di una richiesta HTTP e risposta, nonché qualsiasi stato server pertinenti. Il dizionario dell'ambiente viene definito come segue:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Un server Web compatibile OWIN è responsabile del popolamento con dati, ad esempio le raccolte di intestazione per una richiesta e risposta HTTP e i flussi di corpo il dizionario dell'ambiente. È responsabilità dei componenti dell'applicazione o framework per compilare o aggiornare il dizionario con valori aggiuntivi e scrivere il flusso del corpo della risposta.

Oltre a specificare il tipo per il dizionario dell'ambiente, la specifica di OWIN definisce un elenco di coppie chiave-valore di dizionario core. Ad esempio, nella tabella seguente mostra le chiavi del dizionario necessari per una richiesta HTTP:

| Nome della chiave | Descrizione del valore |
| --- | --- |
| `"owin.RequestBody"` | Un flusso con il corpo della richiesta, se presente. Stream può essere usato come segnaposto se è presente alcun corpo della richiesta. Vedere [corpo della richiesta](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Un `IDictionary<string, string[]>` delle intestazioni di richiesta. Vedere [intestazioni](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Oggetto `string` contenente il metodo di richiesta HTTP della richiesta (ad esempio, `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Oggetto `string` contenente il percorso della richiesta. Il percorso deve essere relativo alla "radice" del delegato di applicazione; vedere [percorsi](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Oggetto `string` contenente la parte del percorso di richiesta corrispondente a "root" del delegato di applicazione; vedere [percorsi](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Oggetto `string` contenente il nome del protocollo e la versione (ad esempio `"HTTP/1.0"` o `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Oggetto `string` contenente il componente stringa di query dell'URI, della richiesta HTTP senza il prefisso "?" (ad esempio, `"foo=bar&baz=quux"`). Il valore può essere una stringa vuota. |
| `"owin.RequestScheme"` | Oggetto `string` contenente lo schema URI utilizzato per la richiesta (ad esempio, `"http"`, `"https"`); vedere [schema URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Il secondo elemento chiave di OWIN è il delegato dell'applicazione. Si tratta di una firma della funzione che funge da interfaccia primaria tra tutti i componenti in un'applicazione OWIN. La definizione per il delegato dell'applicazione è il seguente:

`Func<IDictionary<string, object>, Task>;`

Il delegato dell'applicazione, è semplicemente un'implementazione del tipo delegato Func in cui la funzione accetta il dizionario dell'ambiente come input e restituisce un'attività. Questa progettazione ha diverse implicazioni per gli sviluppatori:

- Esistono un numero molto ridotto delle dipendenze di tipo necessario per scrivere componenti OWIN. In questo modo aumentano l'accessibilità di OWIN per gli sviluppatori.
- La progettazione asincrona consente l'astrazione di efficienza con la gestione delle risorse di elaborazione, in particolare in altre operazioni con utilizzo intensivo dei / o.
- Poiché il delegato dell'applicazione è un'unità atomica di esecuzione e poiché il dizionario dell'ambiente viene eseguito come un parametro nel delegato, componenti OWIN possono essere facilmente concatenati insieme per creare complesse HTTP pipeline di elaborazione.

Da una prospettiva di implementazione, OWIN è una specifica ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). L'obiettivo non è deve essere il framework di Web Avanti, ma piuttosto una specifica per l'interagiscono di Framework Web e server Web.

Se è stato esaminato [OWIN](http://owin.org/) o [Katana](https://github.com/aspnet/AspNetKatana/wiki), si può anche notare il [pacchetto Owin NuGet](http://nuget.org/packages/Owin) e Owin.dll. Questa raccolta contiene una singola interfaccia [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), formalizza che codifica la sequenza di avvio descritto in [sezione 4](http://owin.org/html/owin.html#4-application-startup) della specifica OWIN. Sebbene non sia necessario per compilare i server OWIN, il [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) interfaccia fornisce un punto di riferimento concreta e viene utilizzato dai componenti del progetto Katana.

## <a name="project-katana"></a>Progetto Katana

Considerando che sia il [OWIN](http://owin.org/html/owin.html) specifica e *Owin.dll* sono proprietà dell'azienda e Comunità eseguire attività di origine aperto, il [Katana](https://github.com/aspnet/AspNetKatana/wiki) progetto rappresenta il set di OWIN componenti che, sebbene origine ancora aperto, vengono compilati e rilasciati da Microsoft. Questi componenti includono, ad esempio componenti dell'infrastruttura, ad esempio host e server, nonché a componenti funzionali, ad esempio componenti di autenticazione e le associazioni ai framework [SignalR](../../../signalr/index.md) e [Web ASP.NET API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Il progetto presenta gli obiettivi di alto livelli tre seguenti: 

- **Portabile** – componenti devono essere in grado di essere facilmente sostituito con nuovi componenti appena diventano disponibili. Sono inclusi tutti i tipi di componenti, dal framework per il server host. L'implicazione di questo obiettivo è che il framework di terze parti può facilmente eseguiti sui server Microsoft mentre Framework Microsoft possono essere potenzialmente eseguiti su host e server di terze parti.
- **Modulare/flessibile**: a differenza di molti Framework che includono numerose funzionalità che sono attivate per impostazione predefinita, i componenti di progetto Katana devono essere piccole e con lo stato attivo, che fornisce controllo allo sviluppatore di applicazioni per determinare quali componenti utilizzare nella propria applicazione.
- **Lightweight/ad alte prestazioni/scalabile** – suddividendo la nozione di un framework tradizionale in un set di piccola, con stato attivo di componenti che vengono aggiunti in modo esplicito dallo sviluppatore dell'applicazione, un'applicazione Katana risulta può utilizzare un numero inferiore di calcolo le risorse e di conseguenza, gestire un carico maggiore, rispetto ad altri tipi di server e altri Framework. Come i requisiti dell'applicazione richiedono altre funzionalità dall'infrastruttura sottostante, quelli possono essere aggiunti alla pipeline OWIN, ma che deve essere una decisione esplicita da parte dello sviluppatore dell'applicazione. Inoltre, sostituibilità dei componenti di livello inferiore significa che appena diventano disponibili, nuovi server ad alte prestazioni possono essere introdotti con facilità per migliorare le prestazioni delle applicazioni OWIN senza interrompere le applicazioni.

## <a name="getting-started-with-katana-components"></a>Introduzione a componenti Katana

Quando è stato introdotto per primo, un aspetto del [Node.js](http://nodejs.org/) framework con immediatamente attenzione era la semplicità con cui uno stato possibile creare ed eseguire un server Web. Se sono state racchiuse obiettivi Katana luce del [Node.js](http://nodejs.org/), uno potrebbe riportarli pronunciando che Katana offre molti dei vantaggi di [Node.js](http://nodejs.org/) (e altri framework simile) senza imporre agli sviluppatori di generare tutti gli elementi che e lo sa sullo sviluppo di applicazioni Web ASP.NET. Per questa istruzione abbia valore true, Guida introduttiva a progetto Katana deve essere ugualmente semplice per [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Creazione di "Hello World!"

Una differenza rilevante tra lo sviluppo di JavaScript e .NET è il presenza o assenza di un compilatore. Di conseguenza, il punto di partenza per un server Katana semplice è un progetto di Visual Studio. Tuttavia, è possibile iniziare con più minime dei tipi di progetto: l'applicazione Web ASP.NET vuota.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Successivamente, verrà installato il [systemweb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) pacchetto NuGet nel progetto. Questo pacchetto fornisce un server OWIN che viene eseguito nella pipeline delle richieste ASP.NET. Sono disponibili nel [raccolta NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) e può essere installato utilizzando la finestra di dialogo Gestione pacchetto Visual Studio o la console di gestione di pacchetto con il comando seguente:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

L'installazione di `Microsoft.Owin.Host.SystemWeb` pacchetto installerà alcuni pacchetti aggiuntivi come dipendenze. Uno di tali dipendenze è `Microsoft.Owin`, una libreria di diversi tipi di supporto e i metodi per lo sviluppo di applicazioni OWIN. È possibile utilizzare questi tipi per scrivere rapidamente il server di "hello world" seguente.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

È possibile eseguire il server Web molto semplice con Visual Studio **F5** comando e include il supporto completo per il debug.

## <a name="switching-hosts"></a>Cambio di host

Per impostazione predefinita, il precedente esempio di "hello world" viene eseguito nella pipeline delle richieste ASP.NET, che utilizza System. Web nel contesto di IIS. Questo può autonomamente add convenienza consente di trarre vantaggio dalla flessibilità e componibilità di una pipeline OWIN con le funzionalità di gestione e di scadenza complessiva di IIS. Tuttavia, potrebbe essere casi in cui i vantaggi offerti da IIS non sono necessari e il desiderio è per un host di dimensioni inferiore e più semplice. Cosa è necessario, quindi, per eseguire il server Web semplice di fuori di IIS e System. Web?

Per illustrare lo scopo di portabilità, lo spostamento da un host del server Web a una riga di comando richiede semplicemente l'aggiunta di nuove dipendenze di server e l'host cartella di output del progetto e quindi avviare l'host. In questo esempio è ospitare il server Web in un host Katana denominato `OwinHost.exe` e verrà utilizzato il server basato su Katana HttpListener. Analogamente ad altri componenti Katana questi viene acquisiti da NuGet usando il comando seguente:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Dalla riga di comando, è possibile quindi passare alla cartella radice del progetto ed eseguire semplicemente il `OwinHost.exe` (che è stato installato nella cartella strumenti del rispettivo pacchetto NuGet). Per impostazione predefinita, `OwinHost.exe` è configurato per cercare il server basato su HttpListener e pertanto è necessaria alcuna configurazione aggiuntiva. Spostarsi in un Web browser per `http://localhost:5000/` viene illustrata l'applicazione in esecuzione tramite la console.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architettura Katana

 L'architettura dei componenti Katana divide un'applicazione in quattro livelli logici, come illustrato di seguito: *host, server, il middleware,* e *applicazione*. L'architettura dei componenti è inserito in modo che le implementazioni di questi livelli possono essere facilmente sostituite, in molti casi, senza richiedere la ricompilazione dell'applicazione.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 L'host è responsabile per:

- Gestione del processo sottostante.
- Orchestrazione di flusso di lavoro che comporta la selezione di un server e la creazione di una pipeline OWIN tramite le richieste verranno gestite.

  Attualmente, sono disponibili 3 opzioni di hosting primarie per le applicazioni basate su Katana:  
  
**Accoppiato**: usando i tipi di HttpModule e gestore HTTP standard, le pipeline OWIN eseguibili in IIS come parte di un flusso di richiesta ASP.NET. Supporto per l'hosting ASP.NET è abilitato per l'installazione del pacchetto Microsoft.AspNet.Host.SystemWeb NuGet in un progetto di applicazione Web. Inoltre, poiché IIS funge da host e un server, la distinzione/host server OWIN viene unita secondo di questo pacchetto NuGet, vale a dire che se si utilizza l'host SystemWeb, uno sviluppatore non può sostituire un'implementazione di un server alternativo.  
  
**Host personalizzato**: Katana la suite di componenti consente allo sviluppatore la possibilità di ospitare applicazioni nel proprio processo personalizzato, se un'applicazione console del servizio di Windows, e così via. Questa funzionalità è simile alla funzionalità self-host fornite dall'API Web. Nell'esempio seguente viene illustrato un host personalizzato del codice API Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Il programma di installazione indipendente per un'applicazione Katana è simile:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Una differenza fondamentale tra gli esempi di API Web e Katana host indipendente è che il codice di configurazione Web API non è presente nell'esempio di host indipendente Katana. Per permettere la portabilità sia componibilità, Katana separa il codice che avvia il server dal codice che consente di configurare la pipeline di elaborazione delle richieste. Il codice che configura l'API Web, quindi è contenuto nella classe di avvio, è inoltre specificato come parametro di tipo in WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

La classe di avvio verrà descritta più dettagliatamente più avanti nell'articolo. Tuttavia, il codice necessario per avviare un Katana indipendente processo Cerca sorprendentemente simile al codice che si stia utilizzando oggi nelle applicazioni di servizio indipendente di ASP.NET Web API.

**OwinHost.exe**: anche alcuni verrà effettuato il tentativo di scrivere un processo personalizzato per eseguire applicazioni Katana Web, molti preferiscono semplicemente avviare un eseguibile preesistente che è possibile avviare un server ed eseguire l'applicazione. Per questo scenario, la suite di componente Katana include `OwinHost.exe`. Quando eseguito dall'interno directory radice del progetto, questo eseguibile avvia un server (utilizza il server di HttpListener per impostazione predefinita) e utilizzare le convenzioni per trovare ed eseguire la classe di avvio dell'utente. Per un controllo più granulare, il file eseguibile offre un numero di parametri della riga di comando aggiuntive.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Server

 Mentre l'host è responsabile per l'avvio e il processo entro il quale viene eseguita l'applicazione, la responsabilità del server di gestione è per aprire un socket di rete, l'ascolto delle richieste e inviarli tramite la pipeline di componenti OWIN specificati dall'utente (come si è già notato, questa pipeline è specificata nella classe di avvio dello sviluppatore dell'applicazione). Attualmente, il progetto Katana include due implementazioni di server: 

- **Systemweb**: come indicato in precedenza, IIS interagisce con la pipeline ASP.NET funge da un host e un server. Pertanto, quando si sceglie questa opzione di hosting, IIS gestisce i problemi a livello di host, ad esempio l'attivazione del processo sia ascolta le richieste HTTP. Per le applicazioni Web ASP.NET, invia quindi le richieste nella pipeline ASP.NET. L'host Katana SystemWeb registra un HttpModule ASP.NET e un gestore HTTP per intercettare le richieste che passano attraverso la pipeline HTTP e inviarli tramite la pipeline OWIN specificato dall'utente.
- **HttpListener**: indica il nome, questo server Katana utilizza classe HttpListener di .NET Framework per aprire un socket e inviare le richieste in una pipeline OWIN specificato developer. Questa è attualmente la selezione di server predefinito per l'API di host indipendente Katana e OwinHost.exe.

## <a name="middlewareframework"></a>Middleware/framework

 Come accennato in precedenza, quando il server accetta una richiesta da un client, è responsabile per passare attraverso una pipeline dei componenti OWIN, che sono specificate dal codice di avvio per gli sviluppatori. Questi componenti della pipeline sono noti come middleware.  
 A un livello molto semplice, un componente del middleware OWIN deve semplicemente implementare il delegato dell'applicazione OWIN, in modo che sia possibile chiamata.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Tuttavia, per semplificare lo sviluppo e la composizione dei componenti middleware, Katana supporta un numero limitato di tipi di helper e le convenzioni per i componenti middleware. La più comune di questi è il `OwinMiddleware` classe. Un componente middleware personalizzato creato utilizzando questa classe è simile al seguente: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Questa classe deriva da `OwinMiddleware`, implementa un costruttore che accetta un'istanza di middleware successivo nella pipeline come uno degli argomenti e la passa quindi al costruttore di base. Specificati argomenti aggiuntivi utilizzati per configurare il middleware vengono dichiarati come parametri del costruttore dopo il parametro middleware successivo.   
  
In fase di esecuzione, viene eseguito il middleware tramite sottoposto a override `Invoke` metodo. Questo metodo accetta un singolo argomento di tipo `OwinContext`. L'oggetto di contesto è fornito per il `Microsoft.Owin` pacchetto NuGet descritto in precedenza e fornisce accesso fortemente tipizzato per il dizionario di richiesta, risposta e l'ambiente, con alcuni tipi di supporto aggiuntivi.   
  
La classe middleware può essere facilmente aggiunte alla pipeline OWIN nel codice di avvio dell'applicazione come indicato di seguito:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Poiché l'infrastruttura Katana crea semplicemente una pipeline di componenti middleware OWIN, e i componenti sufficiente supportare il delegato dell'applicazione per partecipare alla pipeline, componenti middleware possono variare essere semplici logger da intero Framework come ASP.NET, API Web, o [SignalR](../../../signalr/index.md). Ad esempio, l'aggiunta di API Web ASP.NET alla pipeline OWIN precedente richiede aggiungendo il seguente codice di avvio:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

L'infrastruttura Katana compilerà la pipeline di componenti middleware in base all'ordine in cui sono stati aggiunti all'oggetto IAppBuilder nel metodo di configurazione. In questo esempio, LoggerMiddleware possibile gestire tutte le richieste che passano attraverso la pipeline, indipendentemente dalla gestione infine tali richieste. In questo modo potenti scenari in cui un componente del middleware (ad esempio, un componente di autenticazione) può elaborare le richieste per una pipeline che include più componenti e altri Framework (ad esempio, ASP.NET Web API SignalR e un server di file statici).
 
## <a name="applications"></a>Applicazioni

Come illustrato negli esempi precedenti, OWIN e il progetto Katana dovrebbe non essere considerati come un nuovo modello di programmazione dell'applicazione, ma piuttosto come un'astrazione per separare i modelli di programmazione di applicazioni e altri Framework dal server e infrastruttura di hosting. Ad esempio, durante la compilazione di applicazioni Web API, il framework per sviluppatori continuerà a utilizzare il framework di ASP.NET Web API, indipendentemente dal fatto l'applicazione viene eseguita in una pipeline OWIN utilizzando i componenti dal progetto Katana. Di un'unica posizione in cui sarà visibile allo sviluppatore di applicazioni correlate OWIN codice sarà il codice di avvio dell'applicazione, in cui lo sviluppatore compone la pipeline OWIN. Nel codice di avvio, lo sviluppatore registrerà una serie di istruzioni UseXx, in genere uno per ogni componente del middleware che elaborerà le richieste in ingresso. Questa esperienza avrà lo stesso effetto di registrazione di moduli HTTP nel mondo System. Web corrente. In genere, un middleware framework più grande, ad esempio API Web ASP.NET o [SignalR](../../../signalr/index.md) verrà registrato alla fine della pipeline. I componenti di middleware a montaggio incrociato, ad esempio quelle per l'autenticazione o la memorizzazione nella cache, in genere registrati verso l'inizio della pipeline in modo che essi elaborerà le richieste per tutti i Framework e i componenti registrati in un secondo momento nella pipeline. La separazione dei componenti middleware da altra e dai componenti dell'infrastruttura sottostante consente i componenti evoluzione diverse velocità assicurando che il sistema rimane stabile.

## <a name="components--nuget-packages"></a>Componenti, ovvero pacchetti NuGet

Come molti le librerie e Framework, i componenti del progetto Katana vengono distribuiti come un set di pacchetti NuGet. Per la prossima versione 2.0, il grafico delle dipendenze del pacchetto Katana è simile al seguente. Fare clic sull'immagine per ingrandirla.

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Quasi tutti i pacchetti nel progetto Katana dipendono, direttamente o indirettamente, il pacchetto Owin. Ricordare che questo è il pacchetto che contiene l'interfaccia IAppBuilder, che fornisce un'implementazione concreta della sequenza di avvio dell'applicazione descritta nella sezione 4 della specifica OWIN. Inoltre, molti dei pacchetti dipendono italiano, che fornisce un set di tipi di supporto per l'utilizzo di richieste e risposte HTTP. Il resto del pacchetto può essere classificato come host pacchetti infrastrutture (server o host) o middleware. Pacchetti e le dipendenze esterne al progetto Katana vengono visualizzate in arancione.

L'infrastruttura di hosting per Katana 2.0 include il SystemWeb e i server basati su HttpListener, OwinHost sia il pacchetto per l'esecuzione di applicazioni OWIN utilizzando OwinHost.exe, il pacchetto di owin per l'hosting automatico delle applicazioni OWIN in un host personalizzato (ad esempio applicazione console, il servizio di Windows e così via)

Per Katana 2.0, i componenti middleware sono principalmente incentrati sui diversi metodi di autenticazione. Viene fornito un componente di altro middleware per la diagnostica, che abilita il supporto per una pagina di inizio e di errore. Man mano che aumenta OWIN nell'astrazione di hosting di fatto, l'ecosistema di componenti middleware, sia quelle sviluppate da Microsoft e di terze parti, aumenterà anche numero.

## <a name="conclusion"></a>Conclusione

 Dall'inizio, l'obiettivo del progetto Katana non è stato per creare e quindi imporre agli sviluppatori di informazioni su un altro framework di Web. Piuttosto, l'obiettivo è stata un'astrazione per fornire agli sviluppatori di applicazioni Web .NET più scelta in precedenza è stato possibile creare. Suddividendo i livelli logici di un tipico stack di applicazioni Web in un set di componenti sostituibili, il progetto Katana consente ai componenti in tutto lo stack per migliorare in qualsiasi velocità ha senso per tali componenti. Tramite la compilazione di tutti i componenti per l'astrazione OWIN semplice, Katana consente Framework e le applicazioni basate su essi per garantire la portabilità tra un'ampia gamma di host e server differenti. Inserisce lo sviluppatore nel controllo dello stack, Katana assicura che lo sviluppatore è incaricato la scelta finale su come leggere o ricco di funzionalità deve essere proprio stack di Web.  
  

## <a name="for-more-information-about-katana"></a>Per ulteriori informazioni su Katana

- Il progetto Katana su GitHub: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Video: [OWIN per ASP.NET - progetto Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), da Howard Dierking.

## <a name="acknowledgements"></a>Riconoscimenti

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick è un writer di programmazione senior per Microsoft con particolare attenzione ad Azure e MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
