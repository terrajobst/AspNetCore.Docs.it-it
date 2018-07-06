---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: What ' s New in ASP.NET 4.5 e Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive le nuove funzionalità e miglioramenti che sono stati introdotti in ASP.NET 4.5. Vengono anche descritti i miglioramenti apportati per lo sviluppo web...
ms.author: aspnetcontent
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: cfe9b1a7f05b43d5eb638c8fa7cb581d1edac9d5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835813"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>What ' s New in ASP.NET 4.5 e Visual Studio 2012
====================
> Questo documento descrive le nuove funzionalità e miglioramenti che sono stati introdotti in ASP.NET 4.5. Vengono inoltre descritti i miglioramenti apportati per lo sviluppo web in Visual Studio 2012. Questo documento è stato pubblicato originariamente 29 febbraio 2012.


- [Framework e Runtime di ASP.NET Core](#_Toc318097372)

    - [In modo asincrono la lettura e scrittura di richieste e risposte HTTP](#_Toc318097373)
    - [Miglioramenti alla gestione delle HttpRequest](#_Toc318097374)
    - [In modo asincrono lo scaricamento di una risposta](#_Toc318097375)
    - [Supporto per *await* e *attività*-basato su moduli e gestori asincroni](#_Toc318097376)
    - [Moduli HTTP asincroni](#_Toc318097377)
    - [Gestori HTTP asincroni](#_Toc318097378)
    - [Nuove funzionalità di convalida richiesta ASP.NET](#_Toc318097379)
    - [Rinviata la convalida della richiesta ("lazy")](#_Toc318097380)
    - [Supporto per le richieste non convalidate](#_Toc318097381)
    - [Libreria AntiXSS](#_Toc318097382)
    - [Supporto del protocollo di WebSockets](#_Toc318097383)
    - [Creazione di bundle e minimizzazione](#_Toc318097384)
    - [Miglioramenti delle prestazioni per l'Hosting Web](#_Toc_perf)

        - [Fattori delle prestazioni chiave](#_Toc_perf_1)
        - [Requisiti per le nuove caratteristiche di prestazioni](#_Toc_perf_2)
        - [Condividere assembly con nome comune](#_Toc_perf_3)
        - [Tramite la compilazione JIT multicore per l'avvio più veloce](#_Toc_perf_4)
        - [Ottimizzazione di ottimizzazione per la memoria di garbage collection](#_Toc_perf_5)
        - [La prelettura per le applicazioni web](#_Toc_perf_6)
- [Web Form ASP.NET](#_Toc318097385)

    - [Controlli dati fortemente tipizzati](#_Toc318097386)
    - [Associazione di modelli](#_Toc318097387)

        - [Selezione dei dati](#_Toc318097388)
        - [Provider di valori](#_Toc318097389)
        - [Il filtro per i valori da un controllo](#_Toc318097390)
    - [Espressioni di associazione dati codificate in formato HTML](#_Toc318097391)
    - [Convalida discreta](#_Toc318097392)
    - [Aggiornamenti HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Progetto di condivisione tra Visual Studio 2010 e Visual Studio 2012 Release Candidate (compatibilità progetto)](#project-compatibility)
    - [Modifiche di configurazione nei modelli di ASP.NET 4.5 sito Web](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Supporto nativo di IIS 7 per il Routing di ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Editor HTML](#_Toc318097397)

        - [Smart Task](#_Toc318097398)
        - [Supporto WAI ARIA](#_Toc318097399)
        - [Nuovi frammenti di codice HTML5](#_Toc318097400)
        - [Estrai nel controllo utente](#_Toc318097401)
        - [IntelliSense per nugget di codice negli attributi](#_Toc318097402)
        - [Ridenominazione automatica di tag corrispondenti quando si rinomina tag di apertura o chiusura](#_Toc318097403)
        - [Generazione del gestore eventi](#_Toc318097404)
        - [Rientro automatico](#_Toc318097405)
        - [Completamento delle istruzioni con riduzione automatica](#_Toc318097406)
    - [Editor JavaScript](#_Toc318097407)

        - [Struttura del codice](#_Toc318097408)
        - [Corrispondenza parentesi graffe](#_Toc318097409)
        - [Vai a definizione](#_Toc318097410)
        - [Supporto di ECMAScript5](#_Toc318097411)
        - [IntelliSense DOM](#_Toc318097412)
        - [VSDOC firma overload](#_Toc318097413)
        - [Riferimenti impliciti](#_Toc318097414)
    - [Editor CSS](#_Toc318097415)

        - [Completamento delle istruzioni con riduzione automatica](#_Toc318097416)
        - [Rientro gerarchico.](#_Toc318097417)
        - [Supporto di HACK CSS](#_Toc318097418)
        - [Schemi specifici fornitore (- moz-, - webkit)](#_Toc318097419)
        - [Supporto di inserimento e rimozione di commenti](#_Toc318097420)
        - [Selezione colori](#_Toc318097421)
        - [Frammenti](#_Toc318097422)
        - [Aree personalizzate](#_Toc318097423)
    - [Controllo pagina](#_Toc318097424)
    - [la pubblicazione](#_Toc318097425)

        - [Profili di pubblicazione](#_Toc318097426)
        - [Merge e la precompilazione ASP.NET](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Dichiarazione di non responsabilità](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Framework e Runtime di ASP.NET Core

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>In modo asincrono la lettura e scrittura di richieste e risposte HTTP

ASP.NET 4 ha introdotto la possibilità di leggere un'entità di richiesta HTTP come un flusso usando il *HttpRequest.GetBufferlessInputStream* (metodo). Questo metodo fornito l'accesso tramite flusso per l'entità della richiesta. Tuttavia, eseguita in modo sincrono, che rimangano un thread per la durata di una richiesta.

ASP.NET 4.5 supporta la possibilità di leggere i flussi in modo asincrono in un'entità di richiesta HTTP e la possibilità di svuotare in modo asincrono. ASP.NET 4.5 offre anche la possibilità di un'entità di richiesta HTTP, che offre un'integrazione semplificata con i gestori HTTP a valle, ad esempio gestori di pagina. aspx e ASP.NET MVC controller doppio buffer.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Miglioramenti alla gestione delle HttpRequest

Il riferimento di Stream restituito da ASP.NET 4.5 dalla *HttpRequest.GetBufferlessInputStream* supporta metodi di lettura sincroni e asincroni. Il *Stream* oggetto restituito da *GetBufferlessInputStream* implementa ora sia BeginRead ed EndRead metodi. Asincrona *Stream* i metodi consentono di leggere in modo asincrono l'entità della richiesta in blocchi, mentre ASP.NET rilascia il thread corrente tra ogni iterazione di un ciclo di lettura asincrono.

ASP.NET 4.5 ha inoltre aggiunto un metodo complementare per la lettura di entità della richiesta in modo memorizzato nel buffer: *HttpRequest.GetBufferedInputStream*. Questo nuovo overload è paragonabile *GetBufferlessInputStream*, che supporta le letture sincrone e asincrone. Tuttavia, come, legge *GetBufferedInputStream* anche di copiare i byte di entità in buffer interni di ASP.NET in modo che a valle moduli e gestori possono comunque accedere l'entità della richiesta. Ad esempio, se alcuni upstream codice nella pipeline è già letto all'entità di richiesta mediante *GetBufferedInputStream*, è comunque possibile usare *HttpRequest.Form* o *HttpRequest.Files*. Ciò consente di eseguire l'elaborazione asincrona in una richiesta (ad esempio, il caricamento di un file di grandi dimensioni in un database di streaming), ma le pagine aspx ancora esecuzione e i controller ASP.NET MVC in un secondo momento.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>In modo asincrono lo scaricamento di una risposta

L'invio di risposte a un client HTTP può richiedere molto tempo quando il client è a portata di mano o ha una connessione a banda ridotta. In genere ASP.NET memorizza nel buffer i byte della risposta non appena vengono creati da un'applicazione. ASP.NET esegue quindi una singola operazione di invio dei buffer sospesi alla fine dell'elaborazione della richiesta.

Se la risposta memorizzata nel buffer è grande (ad esempio, un file di grandi dimensioni a un client di streaming), è necessario chiamare periodicamente *HttpResponse.Flush* per inviare l'output memorizzato nel buffer per il client e mantenere l'utilizzo di memoria sotto controllo. Tuttavia, perché *Flush* è una chiamata sincrona, in modo iterativo la chiamata *Flush* utilizzerà comunque un thread per la durata delle richieste potenzialmente di lunga.

ASP.NET 4.5 aggiunge il supporto per l'esecuzione Scarica in modo asincrono utilizzando il *BeginFlush* e *EndFlush* metodi del *HttpResponse* classe. Uso di questi metodi, è possibile creare moduli e gestori asincroni in modo incrementale di inviare dati a un client senza bloccare il thread del sistema operativo asincroni. Intermedi *BeginFlush* e *EndFlush* chiamate, ASP.NET rilascia il thread corrente. Questo riduce notevolmente il numero totale di thread attivi che sono necessari per supportare il download HTTP con esecuzione prolungata.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Supporto per *await* e *attività* -basato su moduli e gestori asincroni

.NET Framework 4 introdotto un concetto di programmazione asincrono detto una *attività*. Le attività sono rappresentate dal *Task* tipo e i tipi correlati nel *Tasks* dello spazio dei nomi. .NET Framework 4.5 si basa su questo con i miglioramenti del compilatore che agevolare l'uso con *attività* semplice gli oggetti. In .NET Framework 4.5, i compilatori supportano due nuove parole chiave: *await* e *async*. Il *await* parola chiave è una sintassi abbreviata per indicare che un frammento di codice deve attendere in modo asincrono in altre parti di codice. Il *async* parola chiave rappresenta un suggerimento che è possibile usare per contrassegnare i metodi come metodi asincroni basati su attività.

La combinazione delle *await*, *async*e il *attività* oggetto rende molto più semplice per la scrittura di codice asincrono in .NET 4.5. ASP.NET 4.5 supporta le semplificazioni con nuove API che consentono di scrivere i moduli HTTP asincroni e i gestori HTTP asincroni tramite i nuovi miglioramenti del compilatore.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Moduli HTTP asincroni

Si supponga di voler eseguire un lavoro asincrono all'interno di un metodo che restituisce un *attività* oggetto. L'esempio di codice seguente definisce un metodo asincrono che esegue una chiamata asincrona per scaricare la home page di Microsoft. Si noti l'uso del *async* parola chiave nella firma del metodo e il *await* le chiamate a *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Questo è tutto è necessario scrivere, .NET Framework consente di gestire automaticamente la rimozione dello stack di chiamate durante l'attesa del completamento del download, nonché il ripristino automatico dello stack di chiamate dopo il download viene completato.

Si supponga che si desidera usare questo metodo asincrono in un modulo di ASP.NET HTTP asincrono. ASP.NET 4.5 include un metodo helper (*EventHandlerTaskAsyncHelper*) e un nuovo tipo di delegato (*TaskEventHandler*) che è possibile usare per l'integrazione di metodi asincroni basati su attività con le versioni precedenti modello di programmazione asincrono esposta dalla pipeline HTTP ASP.NET. Questo esempio viene illustrato come:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Gestori HTTP asincroni

L'approccio tradizionale per la scrittura di gestori asincroni in ASP.NET consiste nell'implementare il *IHttpAsyncHandler* interfaccia. ASP.NET 4.5 è stato introdotto il *HttpTaskAsyncHandler* asincrona tipo di base che è possibile derivare da, rendendo molto più facile scrivere gestori asincroni.

Il *HttpTaskAsyncHandler* tipo è astratto e richiede di eseguire l'override di *ProcessRequestAsync* (metodo). Internamente ASP.NET si occupa dell'integrazione della firma restituita (un *Task* oggetto) di *ProcessRequestAsync* con il modello di programmazione asincrona precedenti utilizzato dalla pipeline di ASP.NET.

L'esempio seguente illustra come usare *Task* e *await* come parte dell'implementazione di un gestore HTTP asincrono:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nuove funzionalità di convalida richiesta ASP.NET

Per impostazione predefinita, ASP.NET esegue la convalida delle richieste, esamina le richieste per cercare il markup o script in campi, le intestazioni, cookie e così via. Se viene rilevato uno, ASP.NET genera un'eccezione. Questa funzione agisce come prima linea di difesa da potenziali attacchi di scripting intersito.

ASP.NET 4.5 rende facile da leggere in modo selettivo i dati della richiesta non convalidati. ASP.NET 4.5 si integra anche la diffusa libreria AntiXSS, che precedentemente era una libreria esterna.

Gli sviluppatori sono frequenti per la possibilità di disattivare in modo selettivo la convalida delle richieste per le proprie applicazioni. Ad esempio, se l'applicazione è un software di forum, è possibile consentire agli utenti di inviare commenti e post di forum in formato HTML, ma comunque assicurarsi che la convalida delle richieste controlla tutto il resto.

ASP.NET 4.5 introduce due funzionalità che rendono più semplice per usare in modo selettivo gli input non convalidati: rinviata la convalida della richiesta ("lazy") e l'accesso ai dati della richiesta non convalidati.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Rinviata la convalida della richiesta ("lazy")

In ASP.NET 4.5 per impostazione predefinita tutti i dati della richiesta è soggetta a convalida della richiesta. Tuttavia, è possibile configurare l'applicazione per rinviare la convalida delle richieste finché non si accede effettivamente i dati della richiesta. (Ciò è talvolta detta convalida delle richieste lazy, basata su termini, ad esempio il caricamento lazy per determinati scenari i dati.) È possibile configurare l'applicazione per usare la convalida posticipata nel file Web. config impostando il *requestValidationMode* attributo alla versione 4.5 nel *httpRUntime* elemento, come nell'esempio seguente:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Quando è impostata la modalità di convalida richiesta alla versione 4.5, la convalida delle richieste è attivata solo per un valore specifico della richiesta e solo quando il codice accede a tale valore. Ad esempio, se il codice ottiene il valore di Request.Form["forum\_post"], convalida delle richieste viene richiamata solo per tale elemento nella raccolta di form. Nessuno degli altri elementi nel *Form* raccolta vengono convalidati. Nelle versioni precedenti di ASP.NET, la convalida della richiesta è stata attivata per la raccolta di intera richiesta quando è stato eseguito qualsiasi elemento nella raccolta. Il nuovo comportamento rende più semplice per componenti diversi dell'applicazione esaminare diverse parti dei dati richiesta senza attivare la convalida delle richieste in altre parti.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Supporto per le richieste non convalidate

Convalida posticipata delle richieste da solo non risolve il problema di in modo selettivo ignorando la convalida della richiesta. La chiamata a Request.Form["forum\_post"] ancora trigger convalida della richiesta per quel valore specifico della richiesta. Potrebbero, tuttavia, si desidera accedere a questo campo senza attivare la convalida perché si vuole consentire il markup in quel campo.

A tale scopo, ASP.NET 4.5 supporta ora l'accesso non convalidato di richiedere i dati. ASP.NET 4.5 include una nuova *Unvalidated* nelle proprietà della raccolta di *HttpRequest* classe. Questa raccolta consente di accedere a tutti i valori comuni di dati della richiesta, ad esempio *Form*, *QueryString*, *cookie*, e *Url*.

Usando l'esempio forum, sia in grado di leggere i dati della richiesta non convalidati, è innanzitutto necessario configurare l'applicazione per usare la nuova modalità di convalida richiesta:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

È quindi possibile usare la *HttpRequest.Unvalidated* proprietà da leggere il valore di form non convalidati:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Security - *usare i dati della richiesta non convalidati con cautela.* ASP.NET 4.5 ha aggiunto le raccolte per renderne più semplice per accedere ai dati della richiesta non convalidati molto specifici e la proprietà della richiesta non convalidati. Tuttavia, è necessario eseguire ancora la convalida personalizzata sui dati richiesta non elaborata per garantire che il testo pericoloso non viene eseguito agli utenti.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Libreria AntiXSS

A causa la popolarità di Microsoft AntiXSS Library, ASP.NET 4.5 incorpora ora routine di codifica di base rispetto alla versione 4.0 di quella libreria.

La routine di codifica vengono implementate dal *AntiXssEncoder* tipo nella nuova *System.Web.Security.AntiXss* dello spazio dei nomi. È possibile usare la *AntiXssEncoder* tipo direttamente chiamando uno dei metodi di codifica statici che vengono implementati nel tipo. Tuttavia, l'approccio più semplice per usare la nuova routine anti-XSS consiste nel configurare un'applicazione ASP.NET di usare la *AntiXssEncoder* classe per impostazione predefinita. A tale scopo, aggiungere l'attributo seguente al file Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Quando la *encoderType* attributo è impostato per usare i *AntiXssEncoder* tipo, tutto l'output automaticamente la codifica in ASP.NET usa la nuova routine di codifica.

Queste sono le parti della libreria AntiXSS esterna che sono state incorporate in ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*, e *HtmlAttributeEncode*
- *XmlAttributeEncode* e *XmlEncode*
- *UrlEncode* e *UrlPathEncode* (nuovo)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Supporto del protocollo di WebSockets

Protocollo di WebSockets è un protocollo di rete basato su standard che definisce la modalità stabilire comunicazioni bidirezionali sicure e in tempo reale tra un client e un server tramite HTTP. Microsoft ha collaborato con corpi standard IETF sia W3C per definire il protocollo. Il protocollo di WebSockets è supportato da qualsiasi client (non solo browser), con Microsoft impegnati a investire notevoli risorse che supportano il protocollo di WebSockets sul client e sistemi operativi per dispositivi mobili.

Protocollo di WebSockets rende molto più semplice creare i trasferimenti di dati a esecuzione prolungata tra un client e un server. Ad esempio, la scrittura di un'applicazione di chat è molto più semplice, poiché è possibile stabilire una connessione a esecuzione prolungata true tra un client e un server. Non è necessario ricorrere a soluzioni alternative, ad esempio il polling periodico o polling prolungato HTTP per simulare il comportamento di un socket.

ASP.NET 4.5 e IIS 8 includono il supporto di WebSocket a basso livello, consentendo agli sviluppatori ASP.NET di usare le API gestite per la lettura e la scrittura di stringa sia i dati binari in un oggetto WebSocket in modo asincrono. Per ASP.NET 4.5, è disponibile una nuova *System.Web.WebSockets* dello spazio dei nomi che contiene i tipi per l'uso di protocollo di WebSockets.

Un client browser consente di stabilire una connessione WebSocket mediante la creazione di un modello DOM *WebSocket* oggetto che punta a un URL in un'applicazione ASP.NET, come nell'esempio seguente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

È possibile creare endpoint WebSocket in ASP.NET utilizzando qualsiasi tipo di modulo o un gestore. Nell'esempio precedente, è stato usato un file ashx, perché i file ashx sono un modo rapido per creare un gestore.

Secondo il protocollo di WebSockets, un'applicazione ASP.NET accetta una richiesta del client WebSocket, indicando che la richiesta deve essere aggiornata da una richiesta HTTP GET a una richiesta WebSocket. Di seguito è riportato un esempio:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

Il *AcceptWebSocketRequest* metodo accetta un delegato della funzione perché ASP.NET viene rimosso alla richiesta HTTP corrente e quindi trasferisce il controllo al delegato della funzione. Concettualmente questo approccio è simile alla modalità di utilizzo *oggetto*, in cui si definisce un delegato di avvio di thread in background viene eseguito.

Dopo aver ASP.NET e i client hanno completato un handshake WebSocket, ASP.NET richiama il delegato e dell'avvio dell'applicazione di WebSocket. Esempio di codice seguente illustra una semplice applicazione eco che usa il supporto di WebSocket predefinito in ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Il supporto in .NET 4.5 per il *await* (parola chiave) e operazioni asincrone basate su attività è una scelta naturale per la scrittura di applicazioni di WebSocket. L'esempio di codice indica che una richiesta WebSocket viene eseguito completamente in modo asincrono in ASP.NET. L'applicazione attende in modo asincrono un messaggio da inviare da un client tramite una chiamata *await socket. ReceiveAsync*. Analogamente, è possibile inviare un messaggio asincrono a un client tramite una chiamata *await socket. SendAsync*.

Nel browser, un'applicazione riceve i messaggi di WebSockets tramite un *onmessage* (funzione). Per inviare un messaggio da un browser, si chiama il *inviare* metodo per il *WebSocket* tipo DOM, come illustrato in questo esempio:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

In futuro, potrebbe essere rilascio di aggiornamenti di questa funzionalità che astratto da subito alcune di basso livello codifica che è richiesto in questa versione per WebSocket applicazioni.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Creazione di bundle e minimizzazione

Creazione di bundle consente di combinare singoli file JavaScript e CSS in bundle che può essere considerato come un singolo file. Minimizzazione riassume i file JavaScript e CSS rimuovendo spazi vuoti e altri caratteri che non sono necessarie. Queste funzionalità di lavoro con Web Form, ASP.NET MVC e pagine Web.

Aggregazioni vengono create utilizzando la classe Bundle o una delle relative classi figlio, ScriptBundle e StyleBundle. Dopo aver configurato un'istanza di un bundle, l'aggregazione viene resa disponibile per le richieste in ingresso, semplicemente aggiungerlo a un'istanza su BundleCollection globale. Nei modelli predefiniti, configurazione del bundle viene eseguita in un file BundleConfig. Questa configurazione predefinita consente di creare aggregazioni per tutti gli script di base e i file css usati dai modelli.

Bundle vengono fatto riferimento all'interno di viste utilizzando uno dei due metodi helper possibili. Per supportare il rendering di markup diversi per un bundle quando è in modalità debug e la modalità di rilascio, le classi ScriptBundle e StyleBundle hanno il metodo di supporto, eseguire il rendering. Quando è in modalità debug, Render genera markup per ogni risorsa nel bundle. Quando è in modalità di rilascio, Render genera un elemento di markup singolo per l'intero bundle. Attivazione/disattivazione tra debug e rilascio modalità può essere realizzata modificando l'attributo di debug dell'elemento di compilazione in Web. config come illustrato di seguito:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Inoltre, abilitazione o disabilitazione di ottimizzazione possono essere impostate direttamente tramite la proprietà BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Quando i file vengono aggregati, questi vengono innanzitutto ordinati alfabeticamente (la modalità di visualizzazione nel **Esplora soluzioni**). Sono quindi organizzati in modo che le librerie note e le estensioni personalizzate (ad esempio jQuery MooTools e illustra Dojo) vengono caricati per primi. Ad esempio, l'ordine finale per la creazione di bundle della cartella degli script, come illustrato in precedenza sarà:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. js

File CSS sono anche ordinati in ordine alfabetico e quindi riorganizzati in modo che reset.css e normalize.css precedere qualsiasi altro file. L'ordinamento finale della creazione di bundle della cartella Styles illustrata in precedenza sarà questo:

1. reset.css
2. Content.CSS
3. forms.css
4. globals.css
5. menu.CSS
6. Styles. CSS

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Miglioramenti delle prestazioni per l'Hosting Web

.NET Framework 4.5 e Windows 8 introduce funzionalità che consentono di conseguire un notevole miglioramento delle prestazioni per carichi di lavoro del server web. Sono inclusi una riduzione (fino a 35%) sia tempo di avvio e il footprint di memoria di web hosting di siti che utilizzano ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Fattori delle prestazioni chiave

In teoria, tutti i siti Web deve essere attivo e in memoria per garantire risposte rapide per la richiesta successiva, ogni volta che si tratta. Fattori che possono influire sulla velocità di risposta del sito includono:

- Il tempo che necessario per un sito per riavviare dopo che un pool di applicazioni viene riciclato. Questo è il tempo che necessario per avviare un processo del server web per il sito quando gli assembly di sito non sono più in memoria. (Gli assembly di piattaforma sono ancora in memoria, poiché vengono usati da altri siti.) Questa situazione è detta "a freddo sito, avvio a caldo framework" o semplicemente "a freddo sito avvio".
- Quantità di memoria occupa il sito. Condizioni per questo sono "utilizzo della memoria per ogni sito" o "working set non condiviso".

I nuovi miglioramenti delle prestazioni concentrano su entrambi i fattori.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Requisiti per le nuove caratteristiche di prestazioni

I requisiti per le nuove funzionalità possono essere suddivisi nelle categorie seguenti:

- Miglioramenti in esecuzione su .NET Framework 4.
- Miglioramenti che richiedono .NET Framework 4.5, ma possono eseguire in qualsiasi versione di Windows.
- Miglioramenti che sono disponibili solo con .NET Framework 4.5 in esecuzione in Windows 8.

Le prestazioni aumentano con ogni livello di miglioramento che è possibile abilitare.

Alcuni dei miglioramenti di .NET Framework 4.5 sfruttare i vantaggi più ampie funzionalità relative alle prestazioni che si applicano a nonché altri scenari.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Condividere assembly con nome comune

**Requisito**: .NET Framework 4 e Visual Studio 11 Developer Preview SDK

Spesso, diversi siti in un server di usare gli stessi assembly helper (ad esempio, gli assembly da un'applicazione starter kit o sample). Ogni sito ha una propria copia di tali assembly nella directory Bin. Anche se il codice oggetto per l'assembly è identico, che in assembly separati fisicamente, in modo che ogni assembly è necessario leggere separatamente durante l'avvio a freddo sito e mantenere separate in memoria.

La nuova funzionalità interning risolve questa procedura poco efficiente e consente di ridurre i requisiti di RAM e carico ora. Centralizzazione consente Windows mantenere un'unica copia di ciascun assembly nel file system, e singoli assembly nelle cartelle Bin del sito vengono sostituiti con i collegamenti simbolici alla singola copia. Se un singolo sito necessita di una versione dell'assembly distinta, il collegamento simbolico viene sostituito dalla nuova versione dell'assembly ed è interessato solo a tale sito.

Gli assembly mediante i collegamenti simbolici di condivisione richiede un nuovo strumento denominato aspnet\_intern.exe, che consente di creare e gestire l'archivio degli assembly centralizzato. Viene fornito come parte di Visual Studio 11 Developer Preview SDK. (Tuttavia, funzionerà in un sistema che ha solo l'installato .NET Framework 4, presupponendo di aver installato la versione più recente [aggiornare](https://support.microsoft.com/kb/2468871).)

Per assicurarsi che tutti gli assembly idonei sono stato centralizzati, si esegue aspnet\_intern.exe periodicamente (ad esempio, una volta a settimana come attività pianificata). Un utilizzo tipico consiste nel modo seguente:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Per visualizzare tutte le opzioni, eseguire lo strumento senza argomenti.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Tramite la compilazione JIT multicore per l'avvio più veloce

**Requisito**: .NET Framework 4.5

Per un avvio a freddo sito, non solo si dispongono di assembly per la lettura dal disco, ma il sito deve essere compilato tramite JIT. Per un sito complesso, si possono aggiungere ritardi significativi. Una tecnica per utilizzo generico di nuovo in .NET Framework 4.5 consente di ridurre questi ritardi distribuendo la compilazione JIT tra core di processori disponibili. Ciò avviene quanto e tempestivamente usando le informazioni raccolte durante la precedente Avvia del sito. Questa funzionalità implementata per il [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) (metodo).

In base ai core più la compilazione JIT è abilitata per impostazione predefinita in ASP.NET, in modo che non è necessario eseguire alcuna operazione per sfruttare i vantaggi di questa funzionalità. Se si desidera disabilitare questa funzionalità, apportare la seguente impostazione nel file Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Ottimizzazione di ottimizzazione per la memoria di garbage collection

**Requisito**: .NET Framework 4.5

Quando un sito è in esecuzione, l'uso dell'heap del garbage collector (GC) può essere un fattore significativo relativo consumo di memoria. Come qualsiasi garbage collector, il Garbage Collector di .NET Framework rende necessario compromesso tra il tempo di CPU (frequenza e la significatività delle raccolte) e il consumo di memoria (lo spazio aggiuntivo che viene usato per gli oggetti nuovi, liberati o liberare da dispositivo). Per le versioni precedenti, è disponibile materiale sussidiario su come configurare il Garbage Collector per raggiungere il giusto equilibrio (ad esempio, vedere [ASP.NET 2.0/3.5 configurazione condivisa di Hosting](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Per .NET Framework 4.5, invece di più impostazioni autonomo, un'impostazione di configurazione definita carico di lavoro è disponibile che consente a tutti i precedentemente consigliati delle impostazioni di Garbage Collection, nonché nuova ottimizzazione che offre prestazioni aggiuntive per ogni sito working set.

Per abilitare l'ottimizzazione della memoria di Garbage Collection, aggiungere l'impostazione seguente al file Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Se si ha familiarità con le indicazioni precedenti per le modifiche a ASPNET. config, si noti che questa impostazione sostituisce le impostazioni precedenti, ad esempio, non è necessario impostare gcServer gcConcurrent, e così via. Non è necessario rimuovere le impostazioni precedenti.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>La prelettura per le applicazioni web

**Requisito**: .NET Framework 4.5 in esecuzione in Windows 8

Per le diverse versioni, Windows ha incluso una tecnologia nota come le [hanno](http://en.wikipedia.org/wiki/Prefetcher) che riduce il costo di lettura dal disco di avvio dell'applicazione. Poiché l'avvio a freddo è un problema soprattutto per le applicazioni client, questa tecnologia non è stato incluso in Windows Server, che include solo i componenti essenziali per un server. Questa funzionalità è ora disponibile nella versione più recente di Windows Server, in cui è possibile ottimizzare il lancio di singoli siti Web.

Per Windows Server, non l'hanno è abilitato per impostazione predefinita. Per abilitare e configurare l'hanno per l'hosting web ad alta densità, eseguire il set di comandi seguente dalla riga di comando:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Quindi, per integrare l'hanno con le applicazioni ASP.NET, aggiungere quanto segue al file Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Web Form ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Controlli dati fortemente tipizzati

In ASP.NET 4.5, Web Form include alcuni miglioramenti per l'utilizzo di dati. Il primo miglioramento è controlli dati fortemente tipizzati. Per i controlli Web Form in versioni precedenti di ASP.NET, si visualizza un valore associato a dati mediante *Eval* e un'espressione di associazione dati:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Per l'associazione dati bidirezionale, si utilizza *associare*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

In fase di esecuzione, queste chiamate di usano la reflection per leggere il valore del membro specificato e quindi visualizzano il risultato nel markup. Questo approccio semplifica l'associazione dati con i dati arbitrari, unshaped.

Tuttavia, le espressioni di associazione dati simile al seguente non supportano funzionalità quali IntelliSense per i nomi dei membri, la navigazione (come Vai a definizione) o per questi nomi di controllo in fase di compilazione.

Per risolvere questo problema, ASP.NET 4.5 aggiunge la possibilità di dichiarare il tipo di dati dei dati che è associato un controllo. Eseguire questa operazione usando le nuove *ItemType* proprietà. Quando si imposta questa proprietà, sono disponibili due nuove variabili di tipizzati nell'ambito di espressioni di associazione dati: *articoli* e *BindItem*. Poiché le variabili sono fortemente tipizzate, otterrai i vantaggi dell'esperienza di sviluppo Visual Studio.


Per le espressioni di associazione dati bidirezionale, usare il *BindItem* variabile:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


La maggior parte dei controlli nel framework Web Form ASP.NET che supportano il data binding sono stati aggiornati per supportare le *ItemType* proprietà.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Associazione di modelli

Associazione di modelli estende l'associazione di dati nei controlli Web Form ASP.NET per lavorare con l'accesso ai dati incentrato sul codice. Lo strumento include concetti relativi ai *ObjectDataSource* controllo e dall'associazione di modelli in ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Selezione dei dati

Per configurare un controllo dati per utilizzare l'associazione di modelli per selezionare i dati, si imposta il controllo *SelectMethod* proprietà sul nome di un metodo nel codice della pagina. Il controllo dei dati chiama il metodo momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Non è necessario chiamare in modo esplicito il *DataBind* (metodo).

Nell'esempio seguente, il *GridView* controllo è configurato per usare un metodo denominato *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Si crea il *GetCategories* metodo nel codice della pagina. Per un'operazione di selezione semplice, il metodo non occorrono parametri e restituisce un *IEnumerable* oppure *IQueryable* oggetto. Se il nuovo *ItemType* è impostata (che abilita fortemente tipizzate espressioni di associazione dati, come illustrato nella sezione [controlli dati fortemente tipizzati](#_Toc318097386) in precedenza), le versioni di queste interfacce generiche deve essere restituita, ovvero *IEnumerable&lt;T&gt;*  oppure *IQueryable&lt;T&gt;*, con la *T* che corrisponde al tipo di parametro i *ItemType* proprietà (ad esempio, *IQueryable&lt;categoria&gt;*).

L'esempio seguente mostra il codice per un *GetCategories* (metodo). Questo esempio Usa il modello di Entity Framework Code First con il database di esempio Northwind. Il codice consente di verificare che la query restituisce i dettagli dei prodotti correlati per ogni categoria tramite la *inclusione* (metodo). (Ciò assicura che il *TemplateField* elemento nel markup viene visualizzato il conteggio dei prodotti in ogni categoria senza richiedere un' [n + 1 selezionare](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Quando si esegue la pagina, il *GridView* le chiamate di controllo la *GetCategories* metodo automaticamente ed esegue il rendering di dati restituiti usando i campi configurati:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Poiché il metodo select restituisce un *IQueryable* oggetti, il *GridView* controllo possibile manipolare ulteriormente la query prima dell'esecuzione. Ad esempio, il *GridView* controllo consente di aggiungere espressioni di query per l'ordinamento e paging ai restituito *IQueryable* oggetto prima che venga eseguito, in modo che tali operazioni vengono eseguite da sottostante Provider LINQ. In questo caso, Entity Framework garantisce che tali operazioni vengono eseguite nel database.

L'esempio seguente mostra le *GridView* controllo modificato per consentire l'ordinamento e paging:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Ora quando viene eseguita la pagina, il controllo può assicurarsi che viene visualizzata solo la pagina corrente dei dati e che viene ordinato in base alla colonna selezionata:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Per filtrare i dati restituiti, i parametri devono essere aggiunte al metodo select. Questi parametri verranno compilati dall'associazione modello in fase di esecuzione e di utilizzarli per modificare la query prima di restituire i dati.

Si supponga, ad esempio, che si vuole consentire agli utenti filtrare i prodotti immettendo una parola chiave nella stringa di query. È possibile aggiungere un parametro al metodo e aggiornare il codice per usare il valore del parametro:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Questo codice include un *in cui* espressione se viene fornito un valore per *parola chiave* e quindi restituisce i risultati della query.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Provider di valori

Nell'esempio precedente non era specifica sulla posizione in cui il valore per il *parola chiave* parametro era provenienti da. Per indicare queste informazioni, è possibile usare un attributo di parametro. Per questo esempio, è possibile usare la *QueryStringAttribute* classe che si trova nel *System.Web.ModelBinding* dello spazio dei nomi:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Ciò indica l'associazione di modelli per tentare di associare un valore dalla stringa di query per il *parola chiave* parametro in fase di esecuzione. (Questo potrebbe comportare l'esecuzione di conversione del tipo, anche se non è in questo caso.) Se non è possibile specificare un valore e il tipo è non nullable, viene generata un'eccezione.

Le origini dei valori per questi metodi sono dette provider di valori e gli attributi di parametro che indicano quali provider di valori da utilizzare vengono definiti gli attributi del provider di valore. Web Form includerà i provider di valori e gli attributi corrispondenti per tutte le origini tipiche dell'input utente in un'applicazione Web Form, ad esempio la stringa di query, cookie, i valori del form, controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo. È anche possibile scrivere provider di valori personalizzati.

Per impostazione predefinita, il nome del parametro viene utilizzato come chiave per trovare il valore della raccolta di provider di valore. Nell'esempio, il codice avrà un aspetto un valore di stringa di query denominato (parola chiave) (ad esempio ~ / default.aspx?keyword=chef). È possibile specificare una chiave personalizzata, passarlo come argomento all'attributo di parametro. Ad esempio, per usare il valore della variabile di stringa di query denominato q, si può eseguire questa operazione:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Se questo metodo nel codice della pagina, gli utenti possono filtrare i risultati passando una parola chiave usando la stringa di query:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Associazione di modelli consente di eseguire molte attività che sarebbe altrimenti necessario codificare manualmente: leggere il valore, verifica di un valore null, provare a convertirlo nel tipo appropriato, verifica se la conversione ha avuto esito positivo e infine, utilizzando il valore di query. Modello di associazione di risultati in molto meno codice e la possibilità di riusare le funzionalità in tutta l'applicazione.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Il filtro per i valori da un controllo

Si supponga che si desidera estendere l'esempio per consentire all'utente di scegliere un valore di filtro da un elenco di riepilogo a discesa. Aggiungere l'elenco di riepilogo a discesa seguente al markup e configurarlo per ottenerne i dati da un altro metodo usando il *SelectMethod* proprietà:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

In genere verrebbero aggiunte anche un *EmptyDataTemplate* elemento per il *GridView* controllare in modo che il controllo visualizzerà un messaggio se non vengono trovati alcuna corrispondenza prodotti:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Nel codice della pagina, aggiungere che il nuovo seleziona il metodo per l'elenco a discesa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Aggiornare infine il *GetProducts* selezionare il metodo per richiedere un nuovo parametro che contiene l'ID della categoria selezionata nell'elenco a discesa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

A questo punto quando si esegue la pagina, gli utenti possono selezionare una categoria dall'elenco a discesa elenco e il *GridView* controllo viene automaticamente associato nuovamente per visualizzare i dati filtrati. Questo è possibile perché l'associazione di modelli vengono rilevati i valori dei parametri per i metodi di selezionati e rileva se qualsiasi valore del parametro è stato modificato dopo un postback. In questo caso, l'associazione di modelli forza il controllo dati associati a cui associarsi nuovamente i dati.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Espressioni di associazione dati codificate in formato HTML

È possibile ora codifica HTML il risultato delle espressioni di associazione dati. Aggiungere i due punti (:) alla fine della &lt;% # prefisso che contrassegna l'espressione di associazione dati:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Convalida discreta

È ora possibile configurare i controlli validator incorporati per l'uso di JavaScript non intrusivo per la logica di convalida lato client. Ciò notevolmente riduce la quantità di JavaScript viene eseguito il rendering in linea nel markup della pagina e riduce le dimensioni complessive della pagina. È possibile configurare JavaScript non intrusivo per i controlli di convalida in uno dei modi seguenti:

- A livello globale aggiungendo l'impostazione seguente per il *&lt;appSettings&gt;* elemento nel file Web. config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- A livello globale impostando il metodo statico *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* proprietà *UnobtrusiveValidationMode.WebForms* (in genere nel *applicazione \_Avviare* metodo nel file Global. asax).
- Singolarmente per una pagina impostando la nuova *UnobtrusiveValidationMode* proprietà delle *pagina* classe *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Aggiornamenti HTML5

Alcuni sono stati apportati miglioramenti a Web Form controlli server per sfruttare i vantaggi delle nuove funzionalità di HTML5:

- Il *TextMode* proprietà delle *nella casella di testo* controllo è stato aggiornato per supportare i nuovi tipi di input HTML5, ad esempio *posta elettronica*, *datetime*, e così via.
- Il *FileUpload* controllare supporta ora i caricamenti di più file dal browser che supportano questa funzionalità HTML5.
- Ora il supporto durante la convalida HTML5 gli elementi di input di controlli di convalida.
- Nuovi elementi HTML5 con gli attributi che rappresentano un URL ora supportano runat = "server". Di conseguenza, è possibile usare le convenzioni di ASP.NET nei percorsi di URL, ad esempio il ~ operatore per rappresentare la radice dell'applicazione (ad esempio, &lt;video runat = "server" src="~/myVideo.wmv" /&gt;).
- Il *UpdatePanel* controllo è stato risolto per supportare i campi di input di registrazione HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta è ora incluso in Visual Studio 11 Beta. ASP.NET MVC è un framework per lo sviluppo di applicazioni Web altamente gestibili e testabili sulla sfruttando il modello Model-View-Controller (MVC). ASP.NET MVC 4 rende più semplice compilare applicazioni per il Web per dispositivi mobili e include l'API Web ASP.NET, che consente di compilare servizi HTTP che possono raggiungere qualsiasi dispositivo. Per altre informazioni, vedere la [note sulla versione di ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Pagine Web ASP.NET 2

Nuove funzionalità includono quanto segue:

- Modelli di sito nuovi e aggiornati.
- Aggiunta di lato server e la convalida lato client utilizzando la *convalida* helper.
- La possibilità di registrare gli script utilizza una gestione di asset.
- Abilitazione dell'account di accesso di Facebook e altri siti usando OAuth e OpenID.
- Aggiunta di mappe mediante la *viene eseguito il mapping* helper.
- Esecuzione di applicazioni Web Pages side-by-side.
- Il rendering di pagine per i dispositivi mobili.

Per altre informazioni su queste funzioni e gli esempi di codice a pagina intera, vedere [le funzionalità migliori in Web Pages 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Questa sezione vengono fornite informazioni sui miglioramenti per lo sviluppo web in Visual Web Developer 11 Beta e Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Progetto di condivisione tra Visual Studio 2010 e Visual Studio 2012 Release Candidate (compatibilità progetto)

Fino a Visual Studio 2012 Release Candidate, l'apertura di un progetto esistente in una versione più recente di Visual Studio avviata la conversione guidata. Ciò forzata un aggiornamento del contenuto (risorse) di una soluzione e progetto in nuovi formati che non sono compatibili con le versioni precedenti. Di conseguenza, dopo la conversione è Impossibile aprire il progetto nella versione precedente di Visual Studio.

Molti clienti hanno indicato che ciò non è stato l'approccio giusto. In Visual Studio 11 Beta, è ora supportata la condivisione di progetti e soluzioni con Visual Studio 2010 SP1. Ciò significa che se si apre un progetto 2010 in Visual Studio 2012 Release Candidate, sarà comunque in grado di aprire il progetto in Visual Studio 2010 SP1.

> [!NOTE]
> Alcuni tipi di progetti non possono essere condivisi tra Visual Studio 2010 SP1 e Visual Studio 2012 Release Candidate. Questi includono alcuni progetti precedenti (ad esempio ASP.NET MVC 2 progetti) o per scopi speciali (ad esempio, i progetti di installazione).

Quando si apre un progetto Web in Visual Studio 2010 SP1 per la prima volta in Visual Studio 11 Beta, le proprietà seguenti vengono aggiunti al file di progetto:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags UpgradeBackupLocation e OldToolsVersion vengono usati dal processo che consente di aggiornare il file di progetto. Non hanno alcun impatto sull'utilizzo del progetto in Visual Studio 2010.

VisualStudioVersion è una nuova proprietà usata da MSBuild 4.5 che indica la versione di Visual Studio per il progetto corrente. Poiché questa proprietà non esisteva in MSBuild 4.0 (la versione di MSBuild che Usa Visual Studio 2010 SP1), un valore predefinito è inserire nel file di progetto.

La proprietà VSToolsPath viene utilizzata per determinare il file con estensione targets corretto da importare dal percorso rappresentato dall'impostazione MSBuildExtensionsPath32.

Esistono anche alcune modifiche relative agli elementi di importazione. Queste modifiche sono necessarie per supportare la compatibilità tra entrambe le versioni di Visual Studio.

> [!NOTE]
> Se un progetto è condiviso tra Visual Studio 2010 SP1 e Visual Studio 11 Beta su due computer differenti e se il progetto include un database locale nell'App\_cartella dati, è necessario assicurarsi che sia la versione di SQL Server utilizzato dal database installato su entrambi i computer.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Modifiche di configurazione nei modelli di ASP.NET 4.5 sito Web

Sono state apportate le modifiche seguenti per il valore predefinito *Web. config* file per il sito che vengono create usando i modelli di sito Web in Visual Studio 2012 Release Candidate:

- Nel `<httpRuntime>` elemento, il `encoderType` attributo è ora impostato per impostazione predefinita per usare i tipi di AntiXSS che sono state aggiunte ad ASP.NET. Per informazioni dettagliate, vedere [libreria AntiXSS](#_Toc318097382).
- Anche nel `<httpRuntime>` elemento, il `requestValidationMode` attributo è impostato su "4.5". Ciò significa che per impostazione predefinita, la convalida della richiesta sia configurato per usare la convalida posticipata ("lazy"). Per informazioni dettagliate, vedere [nuove funzionalità di convalida richiesta ASP.NET](#_Toc318097379).
- Il `<modules>` elemento del `<system.webServer>` sezione non contiene un `runAllManagedModulesForAllRequests` attributo. (Valore predefinito è false). Ciò significa che se si usa una versione di IIS 7 che non è stato aggiornato a SP1, si potrebbero avere problemi con il routing in un nuovo sito. Per altre informazioni, vedere [supporto nativo di IIS 7 per il Routing ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Queste modifiche non interessano le applicazioni esistenti. Tuttavia, potrebbero rappresentare una differenza nel comportamento tra siti Web esistenti e nuovi siti Web creati per ASP.NET 4.5 con i nuovi modelli.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Supporto nativo di IIS 7 per il Routing di ASP.NET

Ciò non è una modifica ad ASP.NET di conseguenza, ma una modifica nei modelli per nuovi progetti di siti Web che possono interessare è se si usa una versione di IIS 7 che non è stato eseguito l'aggiornamento SP1 applicato.

In ASP.NET, è possibile aggiungere l'impostazione di configurazione seguente alle applicazioni per poter supportare il routing:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Quando **runAllManagedModulesForAllRequests** è true, un URL come `http://mysite/myapp/home` passa ad ASP.NET, anche se è presente alcun *aspx*, *MVC*, o un'estensione simile nel URL.

Esegue un aggiornamento per IIS 7 è stato effettuato il **runAllManagedModulesForAllRequests** impostazione inutili e supporta in modo nativo il routing di ASP.NET. (Per informazioni sull'aggiornamento, vedere l'articolo di supporto tecnico Microsoft [è disponibile che consente di determinati gestori di IIS 7.0 o IIS 7.5 di gestire le richieste il cui URL non terminano con un periodo di un aggiornamento](https://support.microsoft.com/kb/980368).)

Se il sito Web è in esecuzione in IIS 7 e se IIS è stato aggiornato, non è necessario impostare **runAllManagedModulesForAllRequests** su true. In effetti, impostandola su true non è consigliabile, perché aggiunge inutili overhead di elaborazione alla richiesta. Quando questa impostazione è true, tutte le richieste, incluse quelle relative *htm*, *jpg*, e altri file statici, anche passare attraverso la pipeline delle richieste ASP.NET.

Se si crea un nuovo sito Web ASP.NET 4.5 usando i modelli disponibili in Visual Studio 2012 RC, la configurazione per il sito Web non include il **runAllManagedModulesForAllRequests** impostazione. Ciò significa che, per impostazione predefinita, l'impostazione è false.

Se si esegue poi il sito Web in Windows 7 senza SP1 installato, IIS 7 non includono l'aggiornamento richiesto. Di conseguenza, il routing non funzionerà e verranno visualizzati errori. Se si dispone di un problema in cui il routing non funziona, è possibile eseguire il codice seguente:

- Aggiornare Windows 7 SP1, che verrà aggiunto l'aggiornamento a IIS 7.
- Installare l'aggiornamento descritto nell'articolo di supporto tecnico Microsoft elencato in precedenza.
- Impostare **runAllManagedModulesForAllRequests** su true nel file Web. config del sito Web. Si noti che verrà aggiunto un overhead per le richieste.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Smart Task

Nella visualizzazione progettazione, le proprietà complesse dei controlli server spesso sono associati finestre di dialogo e procedure guidate che semplificano la impostarle. Ad esempio, è possibile usare una finestra di dialogo speciali per aggiungere un'origine dati da un *Repeater* controllare o aggiungere colonne a un *GridView* controllo.

Questo tipo di Guida dell'interfaccia utente per le proprietà complesse, tuttavia, non è stata disponibile nella visualizzazione origine. Pertanto, Visual Studio 11 introduce le Smart Task per la visualizzazione origine. Le smart task sono sensibili al contesto tasti di scelta rapida per la funzionalità di uso comune negli editor del codice c# e Visual Basic.

Per i controlli Web Form ASP.NET, le Smart Task vengono visualizzati nel tag del server come una piccola icona quando il punto di inserimento si trova all'interno dell'elemento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

La Smart Task espansi quando si fa clic sull'icona o premere CTRL +. (punto), come in Editor del codice. Visualizza quindi i collegamenti che sono simili alle attività Smart in visualizzazione progettazione.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Ad esempio, la Smart Task nell'illustrazione precedente mostra le opzioni di attività di GridView. Se si sceglie di modificare le colonne, viene visualizzata la finestra di dialogo seguente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Compilare la finestra consente di impostare le stesse proprietà è possibile impostare nella visualizzazione progettazione. Quando si fa clic su OK, il markup per il controllo viene aggiornato con le nuove impostazioni:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Supporto WAI ARIA

La scrittura di siti Web accessibili sta diventando sempre più importante. Il [accessibilità WAI ARIA standard](http://www.w3.org/WAI/intro/aria) definisce come gli sviluppatori devono scrivere siti Web accessibili. Questo standard è completamente supportato in Visual Studio.

Ad esempio, il *ruolo* attributo ha ora supporto IntelliSense completo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Lo standard WAI ARIA introduce anche gli attributi che hanno il prefisso *aria -* che consentono di aggiungere la semantica per un documento HTML5. Visual Studio supporta anche completamente tali *aria -* attributi:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nuovi frammenti di codice HTML5

Per rendere più veloce e semplice scrivere markup HTML5 comunemente utilizzati, Visual Studio include un numero di frammenti di codice. Il frammento di video è un esempio:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Per richiamare il frammento di codice, premere Tab due volte quando l'elemento viene selezionato in IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Questa operazione produce un frammento di codice che è possibile personalizzare.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Estrai nel controllo utente

Nelle pagine web di grandi dimensioni, può essere una buona idea per spostare singole parti in controlli utente. Questo modulo di refactoring può aiutare ad aumentare la leggibilità della pagina e può semplificare la struttura della pagina.

Per semplificare questa operazione, quando si modificano le pagine Web Form nella visualizzazione di origine, è possibile a questo punto selezionare il testo in una pagina, pulsante destro del mouse e quindi scegliere Estrai nel controllo utente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense per nugget di codice negli attributi

Visual Studio ha sempre fornito IntelliSense per nugget di codice lato server in qualsiasi pagina o controllo. Ora Visual Studio include IntelliSense per nugget di codice in anche gli attributi HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Questo rende più semplice creare espressioni di associazione dati:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Ridenominazione automatica di tag corrispondenti quando si rinomina tag di apertura o chiusura

Se si rinomina un elemento HTML (ad esempio, si modifica un *div* tag da un *intestazione* tag), l'apertura o tag di chiusura corrispondente viene modificato anche in tempo reale.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Ciò consente di evitare l'errore in cui si dimentica di modificare un tag di chiusura o modificare quello errato.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generazione del gestore eventi

Visual Studio include ora le funzionalità nella vista di origine che consentono di scrivere i gestori eventi e li associano manualmente. Se si sta modificando un nome di evento nella visualizzazione origine, IntelliSense visualizza &lt;Crea nuovo evento&gt;, che verrà creato un gestore eventi nel codice della pagina che ha la firma a destra:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Per impostazione predefinita, il gestore dell'evento utilizzerà l'ID del controllo per il nome del metodo di gestione degli eventi:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Il gestore eventi risultante avrà un aspetto simile al seguente (in questo caso, in c#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Rientro automatico

Quando si preme INVIO all'interno di un elemento HTML vuoto, l'editor inserirà il punto di inserimento nella posizione corretta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Se si preme INVIO in questa posizione, il tag di chiusura viene spostato verso il basso e rientrato in base al tag di apertura. Il punto di inserimento viene inoltre applicato un rientro:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Completamento delle istruzioni con riduzione automatica

Elenco di IntelliSense in Visual Studio ora filtri basati su ciò che viene digitato in modo da visualizzare solo le opzioni necessarie:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense anche filtri basati sul case di titolo le singole parole nell'elenco di IntelliSense. Ad esempio, se si digita "dl", vengono visualizzati sia lista di distribuzione e asp: DataList:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Questa funzionalità rende più veloce per ottenere il completamento delle istruzioni per gli elementi noti.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript (editor)

L'editor JavaScript in Visual Studio 2012 Release Candidate è completamente nuova e notevolmente migliora l'esperienza di utilizzo di JavaScript in Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Struttura del codice

Aree della struttura vengono ora create automaticamente per tutte le funzioni, che consente di comprimere le parti del file che non sono pertinenti per l'elemento attivo corrente.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Corrispondenza parentesi graffe

Quando si posiziona il punto di inserimento di una parentesi o parentesi graffa di chiusura, l'editor evidenzia la corrispondente a uno.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Vai a definizione

Il comando Vai a definizione consente di passare all'origine per una funzione o variabile.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Supporto di ECMAScript5

L'editor supporta la nuova sintassi e le API in ECMAScript5, la versione più recente dello standard che descrive il linguaggio JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>IntelliSense DOM

IntelliSense per le API DOM è stata migliorata, con supporto per molte nuove API di HTML5, tra cui *querySelector*, archiviazione DOM, dati documenti tra domini, e *canvas*. DOM IntelliSense a questo punto è determinato da un singolo file JavaScript semplice, anziché da una definizione della libreria nativa. Questo rende facile da estendere o sostituire.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC firma overload

Commenti dettagliati di IntelliSense possono essere dichiarati per separato overload di funzioni JavaScript ora utilizzando i nuovi *&lt;firma&gt;* elemento, come illustrato in questo esempio:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Riferimenti impliciti

È ora possibile aggiungere i file JavaScript a un elenco centrale che verrà implicitamente incluso nell'elenco dei file che qualsiasi dato JavaScript file o un blocco riferimenti, vale a dire otterranno IntelliSense per il relativo contenuto. Ad esempio, è possibile aggiungere file jQuery per l'elenco centrale di file e si otterranno IntelliSense per le funzioni in qualsiasi blocco JavaScript del file, se è stato fatto riferimento in modo esplicito (con / / / &lt;riferimento /&gt;) o No.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS (editor)

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Completamento delle istruzioni con riduzione automatica

L'elenco di IntelliSense per i filtri di questo punto CSS basati sulle proprietà CSS e i valori supportati per lo schema selezionato.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense supporta inoltre le ricerche di case titolo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Rientro gerarchico

Editor CSS utilizza rientro della visualizzazione gerarchica delle regole, che offre una panoramica del modo in cui le regole CSS sono organizzate logicamente. Nell'esempio seguente, il #list un selettore CSS figlio dell'elenco e viene quindi applicato un rientro.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Nell'esempio seguente mostra l'ereditarietà più complesse:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Il rientro di una regola è determinato dalle regole relativo padre. Rientro gerarchico è abilitato per impostazione predefinita, ma è possibile disabilitare questa finestra di dialogo Opzioni (strumenti, le opzioni nella barra dei menu):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Supporto di HACK CSS

L'analisi di centinaia di file CSS reali Mostra HACK CSS sono molto comuni che Visual Studio supporta ora quelli più diffusi. Questo supporto include IntelliSense e la convalida dell'asterisco (\*) e un carattere di sottolineatura (\_) HACK di proprietà:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

HACK di selettore tipici sono supportate anche in modo che venga mantenuto rientro gerarchico anche quando vengono applicati. Un hacker selettore tipico utilizzato per destinazione Internet Explorer 7 consiste nell'anteporre un selettore con  *\*: primo figlio + html*. Mediante tale regola manterrà il rientro gerarchico:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schemi specifici fornitore (- moz-, - webkit)

CSS3 introduce molte proprietà che sono state implementate da browser diversi in momenti diversi. Ciò forzati in precedenza agli sviluppatori di codice per browser specifici usando la sintassi specifici del fornitore. Queste proprietà specifiche del browser sono ora inclusi in IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Supporto di inserimento e rimozione di commenti

È ora possibile commentare e rimuovere il commento regole CSS utilizzando gli stessi collegamenti che utilizzano nell'editor del codice (Ctrl + K, Ctrl + K, è possibile rimuovere il commento e C al commento).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Selezione colori

Nelle versioni precedenti di Visual Studio, IntelliSense per gli attributi relativi ai colori è costituita da un elenco di riepilogo a discesa di valori di colore con nome. Tale elenco è stato sostituito da una selezione colori complete.

Quando si immette un valore di colore, la selezione di colori viene visualizzata automaticamente e presenta un elenco di colori usati in precedenza, seguito da una tavolozza dei colori predefinita. È possibile selezionare un colore utilizzando il mouse o tastiera.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

L'elenco può essere espansa in una selezione colori completo. Il selettore consente di controllare il canale alfa convertendo automaticamente qualsiasi colore in RGBA quando si sposta il cursore:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Frammenti di codice

Frammenti di codice nell'editor CSS rendono più semplice e rapido per creare stili multibrowser. Molte proprietà di CSS3 che richiedono impostazioni specifiche del browser a questo punto sono state annullate in frammenti di codice.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Frammenti CSS supportano scenari avanzati (ad esempio, le query di supporto di CSS3) digitando il simbolo a (@), che mostra l'elenco di IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Quando si seleziona @media valore e premere Tab, nell'editor CSS viene inserito il frammento di codice seguente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Come con i frammenti di codice, è possibile creare frammenti CSS personalizzati.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Aree personalizzate

Denominate aree di codice, che sono già disponibili nell'editor del codice, sono ora disponibili per la modifica di CSS. Ciò consente di essere facilmente raggruppate blocchi stile correlati.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Quando un'area viene compressa viene visualizzato il nome dell'area:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Controllo pagina

Page Inspector è uno strumento che esegue il rendering di una pagina web (HTML, Web Form, ASP.NET MVC o Web Pages) in IDE di Visual Studio e ti permette di esaminare il codice sorgente sia l'output risultante. Per le pagine ASP.NET, controllo pagina consente di determinare quale codice lato server ha prodotto il markup HTML che viene eseguito il rendering nel browser.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Per altre informazioni sul controllo pagina, vedere le esercitazioni seguenti:

- Utilizzo di controllo pagina in [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Utilizzo di controllo pagina in [Web Forms ASP.NET](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Pubblicazione

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profili di pubblicazione

In Visual Studio 2010, le informazioni di pubblicazione per i progetti applicazione Web non viene archiviate nel controllo della versione e non sono progettate per la condivisione con altri utenti. In Visual Studio 2012 Release Candidate, il formato del profilo di pubblicazione è stato modificato. Sono già state un artefatto di team ed è ora possibile sfruttare dalle compilazioni basate su MSBuild. Informazioni sulla configurazione di compilazione è nella finestra di dialogo Pubblica in modo che è possibile passare facilmente le configurazioni di compilazione prima della pubblicazione.

Pubblicare i profili vengono archiviati nella cartella PublishProfiles. Il percorso della cartella dipende dalla quale linguaggio di programmazione in uso:

- C#: Properties\PublishProfiles
- Visual Basic: Project\PublishProfiles

Ogni profilo è un file di MSBuild. Durante la pubblicazione, questo file viene importato in file del progetto MSBuild. In Visual Studio 2010, se si desidera apportare modifiche al processo di pubblicazione o un pacchetto, è necessario inserire le personalizzazioni in un file denominato **ProjectName**. WPP. Questa funzionalità è ancora supportata, ma è ora possibile inserire le personalizzazioni nel profilo di pubblicazione stesso. In questo modo, le personalizzazioni verranno utilizzate solo per quel profilo.

È possibile anche sfruttare pubblicare profili da MSBuild. A tale scopo, usare il comando seguente quando si compila il progetto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Il valore project.csproj è il percorso del progetto e ProfileName è il nome del profilo da pubblicare. In alternativa, anziché passare il nome del profilo per il *PublishProfile* proprietà, è possibile passare il percorso completo per il profilo di pubblicazione.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Merge e la precompilazione ASP.NET

Per i progetti applicazione Web, Visual Studio 2012 Release Candidate aggiunge un'opzione nella pagina delle proprietà pubblicazione/creazione pacchetto Web che consente di precompilare e unire il contenuto del sito quando si pubblica o un pacchetto del progetto. Per visualizzare queste opzioni, fare clic sul progetto in Esplora soluzioni, scegliere Proprietà e quindi scegliere la pagina delle proprietà pubblicazione/creazione pacchetto Web. La figura seguente mostra il precompila l'applicazione prima di opzione di pubblicazione.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Quando questa opzione è selezionata, Visual Studio consente di precompilare l'applicazione ogni volta che pubblica o il pacchetto dell'applicazione web. Se si desidera controllare come viene precompilato il sito o come vengono uniti gli assembly, fare clic sul pulsante Avanzate per configurare queste opzioni.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Il server web predefinito per i progetti di test web in Visual Studio è ora IIS Express. Visual Studio Development Server è ancora un'opzione per server web locale durante lo sviluppo, ma IIS Express è ora il server consigliato. L'esperienza di utilizzo di IIS Express in Visual Studio 11 Beta è molto simile a quello in Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute nel presente documento rappresentano l'attuale opinione di Microsoft Corporation circa le problematiche discusse alla data della pubblicazione. Poiché Microsoft deve rispondere ai cambiamenti delle condizioni di mercato, il presente documento non deve essere interpretato quale un impegno da parte di Microsoft e Microsoft non può garantire l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è a esclusivo carico dell'utente. Senza limitare i diritti sanciti dal copyright, nessuna parte di questo documento può essere riprodotta, memorizzata o inserita in un sistema di ricerca o trasmessa in qualsiasi forma o mezzo (elettronico o meccanico, mediante fotocopia, registrazione o altro), per alcuno scopo, senza l'autorizzazione scritta di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto del presente documento. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna del presente documento non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non diversamente specificato, la società, organizzazioni, prodotti, nomi di dominio, indirizzi di posta elettronica, logo, persone, luoghi ed eventi citati in questo documento sono fittizio e nessuna associazione con qualsiasi società, organizzazione, prodotto, nome di dominio, indirizzo di posta elettronica indirizzo, logo, persona, luogo o evento è intenzionale o può essere presupposta.

© 2012 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
