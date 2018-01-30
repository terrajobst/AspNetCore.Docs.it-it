---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: "Novità di ASP.NET 4.5 e Visual Studio 2012 | Documenti Microsoft"
author: rick-anderson
description: "Questo documento descrive le nuove funzionalità e i miglioramenti introdotti in ASP.NET 4.5. Vengono inoltre descritte apportati per lo sviluppo web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 16a2fa4b1b6617430703853543cbf29e18ba1103
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Novità di ASP.NET 4.5 e Visual Studio 2012
====================
> Questo documento descrive le nuove funzionalità e i miglioramenti introdotti in ASP.NET 4.5. Vengono inoltre descritte apportati per lo sviluppo web in Visual Studio 2012. Questo documento è stato pubblicato originariamente 29 febbraio 2012.


- [Framework e il Runtime di ASP.NET Core](#_Toc318097372)

    - [In modo asincrono la lettura e scrittura di richieste e risposte HTTP](#_Toc318097373)
    - [Miglioramenti alla gestione HttpRequest](#_Toc318097374)
    - [In modo asincrono lo scaricamento di una risposta](#_Toc318097375)
    - [Supporto per *await* e *attività*-gestori e i moduli asincrono basato su](#_Toc318097376)
    - [Moduli HTTP asincroni](#_Toc318097377)
    - [Gestori HTTP asincroni](#_Toc318097378)
    - [Nuove funzionalità di convalida richiesta ASP.NET](#_Toc318097379)
    - [Convalida della richiesta ("lazy) posticipata](#_Toc318097380)
    - [Supporto per le richieste non convalidate](#_Toc318097381)
    - [Libreria AntiXSS](#_Toc318097382)
    - [Supporto per il protocollo WebSocket](#_Toc318097383)
    - [Creazione di bundle e minimizzazione](#_Toc318097384)
    - [Miglioramenti delle prestazioni per l'Hosting Web](#_Toc_perf)

        - [Fattori di prestazioni chiave](#_Toc_perf_1)
        - [Requisiti per le nuove caratteristiche di prestazioni](#_Toc_perf_2)
        - [Condivisione di assembly comuni](#_Toc_perf_3)
        - [Tramite la compilazione JIT multicore per l'avvio veloce](#_Toc_perf_4)
        - [Ottimizzazione delle operazioni di garbage collection ottimizzare per la memoria](#_Toc_perf_5)
        - [La prelettura per le applicazioni web](#_Toc_perf_6)
- [Web Form ASP.NET](#_Toc318097385)

    - [Controlli dati fortemente tipizzati](#_Toc318097386)
    - [Associazione di modelli](#_Toc318097387)

        - [La selezione dei dati](#_Toc318097388)
        - [Provider di valori](#_Toc318097389)
        - [Applicando un filtro per i valori da un controllo](#_Toc318097390)
    - [Espressioni di associazione dati con codificata HTML](#_Toc318097391)
    - [Convalida non intrusiva](#_Toc318097392)
    - [Aggiornamenti di HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Progetto condivisione tra Visual Studio 2010 e Visual Studio 2012 Release Candidate (compatibilità del progetto)](#project-compatibility)
    - [Modifiche di configurazione di modelli di siti Web 4.5 di ASP.NET](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Supporto nativo di IIS 7 per il Routing di ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Editor HTML](#_Toc318097397)

        - [Smart tag](#_Toc318097398)
        - [Supporto WAI ARIA](#_Toc318097399)
        - [Nuovi frammenti di codice HTML5](#_Toc318097400)
        - [Estrai in controllo utente](#_Toc318097401)
        - [IntelliSense per parti di codice negli attributi](#_Toc318097402)
        - [Ridenominazione automatica delle corrispondenti tag quando si rinomina tag di apertura o chiusura](#_Toc318097403)
        - [Generazione di gestore eventi](#_Toc318097404)
        - [Rientro automatico](#_Toc318097405)
        - [Ridurre automaticamente il completamento delle istruzioni](#_Toc318097406)
    - [Editor JavaScript](#_Toc318097407)

        - [Struttura del codice](#_Toc318097408)
        - [Corrispondenza parentesi graffe](#_Toc318097409)
        - [Vai a definizione](#_Toc318097410)
        - [Supporto di ECMAScript5](#_Toc318097411)
        - [IntelliSense DOM](#_Toc318097412)
        - [Overload di firma VSDOC](#_Toc318097413)
        - [Riferimenti impliciti](#_Toc318097414)
    - [Editor CSS](#_Toc318097415)

        - [Ridurre automaticamente il completamento delle istruzioni](#_Toc318097416)
        - [Rientro gerarchico.](#_Toc318097417)
        - [Supporto delle modifiche alla CSS](#_Toc318097418)
        - [Schemi specifici del fornitore (- moz-, - webkit)](#_Toc318097419)
        - [Creazione di commenti e rimozione dei commenti sul supporto](#_Toc318097420)
        - [Selezione dei colori](#_Toc318097421)
        - [Frammenti](#_Toc318097422)
        - [Aree personalizzate](#_Toc318097423)
    - [Controllo pagina](#_Toc318097424)
    - [La pubblicazione](#_Toc318097425)

        - [Profili di pubblicazione](#_Toc318097426)
        - [Precompilazione ASP.NET e tipo merge](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Dichiarazione di non responsabilità](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Framework e il Runtime di ASP.NET Core

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>In modo asincrono la lettura e scrittura di richieste e risposte HTTP

ASP.NET 4 è stata introdotta la possibilità di leggere un'entità di richiesta HTTP come un flusso utilizzando il *Getbufferlessinputstream* metodo. Questo metodo è fornito un accesso tramite flusso per l'entità della richiesta. Tuttavia, eseguita in modo sincrono, cui collegata il thread per la durata di una richiesta.

ASP.NET 4.5 supporta la possibilità di leggere flussi in modo asincrono su un'entità di richiesta HTTP e la possibilità di scaricare in modo asincrono. ASP.NET 4.5 offre inoltre la possibilità di un'entità di richiesta HTTP, che fornisce un'integrazione più semplice con i gestori HTTP a valle, ad esempio gestori di pagina. aspx e ASP.NET MVC controller doppio buffer.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Miglioramenti alla gestione HttpRequest

Il riferimento al flusso restituito da ASP.NET 4.5 da *Getbufferlessinputstream* supporta metodi di lettura sincroni e asincroni. Il *flusso* oggetto restituito da *GetBufferlessInputStream* ora implementa il BeginRead ed EndRead metodi. Asincrona *flusso* metodi consentono di leggere in modo asincrono l'entità della richiesta in blocchi, mentre ASP.NET rilascia il thread corrente tra ogni iterazione di un ciclo di lettura asincrono.

ASP.NET 4.5 ha inoltre aggiunto un metodo correlato per la lettura di entità della richiesta in modo memorizzato nel buffer: *Getbufferedinputstream*. Questo nuovo overload funziona come *GetBufferlessInputStream*, che supporta le letture sincrone e asincrone. Tuttavia, durante la lettura, *GetBufferedInputStream* anche di copiare i byte di entità in buffer interni di ASP.NET in modo che i moduli a valle e gestori possono ancora accedere l'entità della richiesta. Ad esempio, se alcune a monte codice nella pipeline dispone già di lettura all'entità di richiesta mediante *GetBufferedInputStream*, è comunque possibile utilizzare *HttpRequest. Form* o *HttpRequest.Files*. Ciò consente di eseguire l'elaborazione asincrona in una richiesta (ad esempio, il caricamento di un file di grandi dimensioni a un database di streaming), ma le pagine aspx ancora esecuzione e controller MVC ASP.NET in un secondo momento.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>In modo asincrono lo scaricamento di una risposta

L'invio di risposte a un client HTTP può richiedere molto tempo quando il client è lontano o dispone di una connessione con larghezza di banda ridotta. In genere ASP.NET memorizza nel buffer i byte di risposta che vengono creati da un'applicazione. ASP.NET esegue quindi una singola operazione di invio dei buffer sospesi alla fine dell'elaborazione della richiesta.

Se la risposta memorizzata nel buffer è grande (ad esempio, un file di grandi dimensioni a un client di streaming), è necessario chiamare periodicamente *HttpResponse.Flush* per inviare l'output memorizzato nel buffer al client e mantenere l'utilizzo della memoria nel controllo. Tuttavia, perché *Flush* è una chiamata sincrona, la chiamata in modo iterativo *Flush* ancora utilizza un thread per la durata delle richieste potenzialmente lunga.

ASP.NET 4.5 aggiunge il supporto per l'esecuzione di scarica in modo asincrono utilizzando il *BeginFlush* e *EndFlush* metodi di *HttpResponse* classe. Utilizzo di questi metodi, è possibile creare moduli asincroni e i gestori asincroni in modo incrementale inviare dati a un client, senza bloccare il thread del sistema operativo. Tra *BeginFlush* e *EndFlush* chiamate, ASP.NET rilascia il thread corrente. Questo riduce notevolmente il numero totale di thread attivi che sono necessari per supportare il download HTTP con esecuzione prolungata.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Supporto per *await* e *attività* -gestori e i moduli asincrono basato su

.NET Framework 4 introdotto un concetto di programmazione asincrono detto un *attività*. Le attività sono rappresentate dal *attività* tipo e i tipi correlati nel *System.Threading.Tasks* dello spazio dei nomi. .NET Framework 4.5 si basa su questo con funzionalità avanzate del compilatore lavorare con *attività* semplice di oggetti. In .NET Framework 4.5, i compilatori supportano due nuove parole chiave: *await* e *async*. Il *await* la parola chiave è sintattico a sintassi abbreviata per indicare che una parte di codice è necessario attendere in modo asincrono in altre parti di codice. Il *async* parola chiave rappresenta un suggerimento che consente di contrassegnare i metodi come metodi asincroni basati su attività.

La combinazione di *await*, *async*e *attività* oggetto rende molto più semplice per la scrittura di codice asincrona in .NET 4.5. ASP.NET 4.5 supporta questi semplificazione con nuove API che consentono di scrivere moduli HTTP asincroni e i gestori HTTP asincroni utilizzando le nuove funzionalità del compilatore.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Moduli HTTP asincroni

Si supponga che si desidera eseguire un lavoro asincrono all'interno di un metodo che restituisce un *attività* oggetto. Esempio di codice seguente definisce un metodo asincrono che effettua una chiamata asincrona per scaricare la home page di Microsoft. Si noti l'uso del *async* parola chiave nella firma del metodo e *await* tutte le chiamate a *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Questo è tutto è necessario scrivere, .NET Framework consente di gestire automaticamente la rimozione dello stack di chiamate durante l'attesa del completamento del download, nonché di ripristino automaticamente lo stack di chiamate dopo il download è completato.

Si supponga ora che si desidera utilizzare questo metodo asincrono in un modulo HTTP ASP.NET asincrono. ASP.NET 4.5 include un metodo di supporto (*EventHandlerTaskAsyncHelper*) e un nuovo tipo di delegato (*TaskEventHandler*) che consente di integrare i metodi asincroni basati su attività con versioni precedenti modello di programmazione asincrona esposta dalla pipeline HTTP ASP.NET. Questo esempio viene illustrato come:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Gestori HTTP asincroni

L'approccio tradizionale per la scrittura di gestori asincroni in ASP.NET consiste nell'implementare il *IHttpAsyncHandler* interfaccia. ASP.NET 4.5 introduce il *HttpTaskAsyncHandler* asincrona tipo di base che è possibile derivare da, che rende molto più semplice scrivere gestori asincroni.

Il *HttpTaskAsyncHandler* tipo è astratto e richiede di eseguire l'override di *ProcessRequestAsync* metodo. Internamente occupa ASP.NET di integrazione della firma restituita (un *attività* oggetto) di *ProcessRequestAsync* con il modello di programmazione asincrono precedente usato dalla pipeline ASP.NET.

Nell'esempio seguente viene illustrato come utilizzare *attività* e *await* come parte dell'implementazione di un gestore HTTP asincrono:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nuove funzionalità di convalida richiesta ASP.NET

Per impostazione predefinita, ASP.NET esegue la convalida della richiesta, esamina le richieste per cercare di markup o uno script nei campi, le intestazioni, i cookie e così via. Se viene rilevato uno, ASP.NET genera un'eccezione. Questa funzione agisce come prima linea di difesa contro potenziali attacchi script tra siti.

ASP.NET 4.5 rende più facile da leggere in modo selettivo i dati della richiesta non convalidati. La libreria AntiXSS comune, che precedentemente era una libreria esterna si integra anche da ASP.NET 4.5.

Spesso gli sviluppatori hanno richiesto la possibilità di disattivare la convalida della richiesta per le applicazioni in modo selettivo. Ad esempio, se l'applicazione software forum, è consigliabile consentire agli utenti di inviare commenti e i post del forum in formato HTML, ma comunque assicurarsi che la convalida delle richieste controlla tutto il resto.

ASP.NET 4.5 introduce due funzionalità che rendono più facile da utilizzare in modo selettivo l'input non convalidati: posticipata la convalida della richiesta ("lazy") e l'accesso ai dati della richiesta non convalidati.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Convalida della richiesta ("lazy) posticipata

In ASP.NET 4.5 per impostazione predefinita tutti i dati della richiesta è soggetta a convalida richiesta. Tuttavia, è possibile configurare l'applicazione per rinviare la convalida della richiesta fino a quando non è effettivamente accedere ai dati di richiesta. (Questo è talvolta detta convalida della richiesta lazy, basata su termini quali il caricamento lazy per determinati scenari di dati.) È possibile configurare l'applicazione per utilizzare la convalida posticipata nel file Web. config mediante l'impostazione di *requestValidationMode* attributo 4.5 nel *httpRUntime* elemento, come nell'esempio seguente:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Quando la modalità di convalida richiesta è impostata su 4.5, la convalida delle richieste è attivata solo per un valore specifico di richiesta e solo quando il codice accede a tale valore. Ad esempio, se il codice ottiene il valore di Request.Form["forum\_post"], convalida della richiesta viene richiamata solo per tale elemento nella raccolta di form. Nessuno degli altri elementi di *modulo* raccolta vengono convalidati. Nelle versioni precedenti di ASP.NET, la convalida della richiesta è stata attivata per la raccolta dell'intera richiesta quando accede a un qualsiasi elemento nella raccolta. Il nuovo comportamento rende più semplice per componenti diversi dell'applicazione visualizzare diverse parti dei dati richiesta senza attivare la convalida richiesta altre parti.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Supporto per le richieste non convalidate

Convalida posticipata richiesta da solo non risolve il problema di in modo selettivo ignorando la convalida della richiesta. La chiamata a Request.Form["forum\_post"] ancora trigger convalida della richiesta per il valore di tale richiesta specifica. Potrebbe tuttavia si desidera accedere a questo campo senza attivare la convalida poiché si desidera consentire il markup in tale campo.

A tale scopo, ASP.NET 4.5 supporta ora l'accesso non convalidato di richiedere i dati. ASP.NET 4.5 include un nuovo *Unvalidated* nelle proprietà della raccolta di *HttpRequest* classe. Questa raccolta consente di accedere a tutti i valori comuni di dati della richiesta, ad esempio *modulo*, *QueryString*, *cookie*, e *Url*.

Utilizzando l'esempio di forum, per essere in grado di leggere i dati di richiesta non convalidati, è innanzitutto necessario configurare l'applicazione per utilizzare la nuova modalità di convalida richiesta:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

È quindi possibile utilizzare il *HttpRequest.Unvalidated* proprietà leggere il valore di form non convalidati:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Sicurezza - *utilizzare dati di richiesta non convalidati con cautela.* ASP.NET 4.5 ha aggiunto le raccolte per renderne più semplice per poter accedere ai dati di richiesta non convalidati molto specifico e la proprietà richiesta non convalidati. È comunque necessario eseguire la convalida personalizzata sui dati richiesta non elaborato per verificare che il testo pericoloso non viene eseguito agli utenti.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Libreria AntiXSS

A causa di diffusione di Microsoft AntiXSS Library, ASP.NET 4.5 incorpora una routine di codifica di base rispetto alla versione 4.0 di libreria.

Le routine di codifica implementate dal *AntiXssEncoder* tipo nel nuovo *System.Web.Security.AntiXss* dello spazio dei nomi. È possibile utilizzare il *AntiXssEncoder* tipo direttamente chiamando uno dei metodi statici codifica implementati nel tipo. Tuttavia, l'approccio più semplice per l'utilizzo di routine di anti-XSS nuovo è configurare un'applicazione ASP.NET di usare il *AntiXssEncoder* classe per impostazione predefinita. A tale scopo, aggiungere l'attributo seguente al file Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Quando il *encoderType* attributo è impostato per utilizzare il *AntiXssEncoder* tipo, tutto l'output automaticamente la codifica in ASP.NET viene utilizzata la nuova routine di codifica.

Queste sono le parti della libreria AntiXSS esterna che sono state incorporate in ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*, e *HtmlAttributeEncode*
- *XmlAttributeEncode* e *XmlEncode*
- *UrlEncode* e *UrlPathEncode* (nuovo)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Supporto per il protocollo WebSocket

Il protocollo WebSocket è un protocollo di rete basata su standard che definisce la modalità stabilire una comunicazione bidirezionale sicura e in tempo reale tra un client e un server su HTTP. Microsoft ha collaborato con corpi di standard IETF sia W3C per definire il protocollo. Il protocollo WebSocket è supportato da qualsiasi client (non solo browser), con Microsoft investire notevoli risorse di supporto protocollo WebSocket sia client che i sistemi operativi per dispositivi mobili.

Il protocollo WebSocket rende molto più semplice creare i trasferimenti di dati con esecuzione prolungata tra un client e un server. Ad esempio, la scrittura di un'applicazione di chat è molto più semplice, poiché è possibile stabilire una connessione con esecuzione prolungata true tra un client e un server. Non è necessario ricorrere a soluzioni alternative come polling periodico o polling prolungato HTTP per simulare il comportamento di un socket.

ASP.NET 4.5 e IIS 8 includono il supporto di WebSocket basso livello, consentendo agli sviluppatori ASP.NET di utilizzare le API gestite in modo asincrono lettura e scrittura sia di stringhe di dati binari in un oggetto WebSocket. Per ASP.NET 4.5, è disponibile un nuovo *System.Web.WebSockets* dello spazio dei nomi che contiene tipi per l'utilizzo con il protocollo WebSocket.

Un client browser stabilisce una connessione WebSocket creando un documento DOM *WebSocket* oggetto che punta a un URL in un'applicazione ASP.NET, come nell'esempio seguente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

È possibile creare endpoint WebSocket in ASP.NET utilizzando qualsiasi tipo di modulo o gestore. Nell'esempio precedente, è stato usato un file ashx,. ashx sono un modo rapido per creare un gestore.

Il protocollo WebSocket, in base a un'applicazione ASP.NET accetta una richiesta del client WebSocket indicando che la richiesta deve essere aggiornata da una richiesta GET HTTP a una richiesta WebSocket. Di seguito è riportato un esempio:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

Il *AcceptWebSocketRequest* metodo accetta un delegato della funzione poiché ASP.NET rimuove la richiesta HTTP corrente e quindi trasferisce il controllo per il delegato della funzione. Concettualmente questo approccio è simile a come si utilizza *System.Threading.Thread*, in cui si definisce un delegato di avvio di thread in background viene eseguito.

Dopo che il client e ASP.NET completata un handshake di WebSocket, ASP.NET chiama il delegato e dell'avvio dell'applicazione di WebSocket. Esempio di codice seguente illustra un'applicazione di echo semplice che utilizza il supporto di WebSocket incorporato in ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Il supporto in .NET 4.5 per il *await* (parola chiave) e alle operazioni asincrone basate su attività è una scelta naturale per la scrittura di applicazioni di WebSocket. L'esempio di codice indica che una richiesta WebSocket viene eseguito completamente in modo asincrono in ASP.NET. L'applicazione attende in modo asincrono un messaggio di essere inviato da un client chiamando *await socket. ReceiveAsync*. Analogamente, è possibile inviare un messaggio asincrono a un client chiamando *await socket. SendAsync*.

Nel browser, un'applicazione riceve messaggi WebSocket attraverso un *onmessage* (funzione). Per inviare un messaggio da un browser, si chiama il *inviare* metodo il *WebSocket* tipo DOM, come illustrato in questo esempio:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

In futuro, è possibile rilasciare gli aggiornamenti per questa funzionalità che astratta subito alcune la codifica basso livello che è necessaria in questa versione per WebSocket applicazioni.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Bundling and Minification

Creazione di bundle consente di combinare singoli file CSS e JavaScript in un bundle che può essere gestito come un singolo file. Minimizzazione condensa file CSS e JavaScript rimuovendo gli spazi vuoti e altri caratteri che non sono necessari. Queste funzioni con le pagine Web, ASP.NET MVC e Web Form.

Bundle vengono creati utilizzando la classe di aggregazione o una delle relative classi figlio, ScriptBundle e StyleBundle. Dopo aver configurato un'istanza di un pacchetto, il bundle di resi disponibile per le richieste in ingresso da semplicemente aggiunta a un'istanza su BundleCollection globale. Nei modelli predefiniti, configurazione di aggregazione viene eseguita in un file BundleConfig. Questa configurazione predefinita consente di creare aggregazioni per tutti i file css utilizzati dai modelli e gli script di base.

Bundle vengono fatto riferimento all'interno di viste utilizzando uno dei due metodi di supporto possibili. Per supportare il rendering di markup diversi per un bundle quando è in modalità debug e la modalità di rilascio, le classi di ScriptBundle e StyleBundle hanno il metodo di supporto, eseguire il rendering. In modalità di debug, Render genera il markup per ogni risorsa nel bundle. Quando è in modalità di rilascio, Render genera un elemento singolo di markup per l'intero bundle. Attivazione e disattivazione tra debug e rilascio modalità può essere eseguita modificando l'attributo di debug dell'elemento di compilazione in Web. config come illustrato di seguito:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Inoltre, è possibile impostare l'abilitazione o disabilitazione di ottimizzazione direttamente tramite la proprietà BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Quando sono inclusi i file, questi vengono innanzitutto ordinati alfabeticamente (il modo in cui vengono visualizzati in **Esplora**). Quindi sono organizzati in modo che le librerie e le estensioni personalizzate (ad esempio jQuery MooTools e Dojo) vengono caricate per primi. Ad esempio, l'ordine finale per l'aggiunta della cartella di script, come illustrato in precedenza sarà:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

File CSS sono anche ordinati in ordine alfabetico e quindi riorganizzati in modo che reset.css e normalize.css precedere qualsiasi altro file. L'ordinamento finale di aggregazione della cartella Styles illustrata in precedenza sarà questo:

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.CSS
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Miglioramenti delle prestazioni per l'Hosting Web

.NET Framework 4.5 e Windows 8 introdurre funzionalità che è possibile realizzare un notevole miglioramento delle prestazioni per carichi di lavoro del server web. Sono inclusi una riduzione (fino a 35%) sia di tempo di avvio e il footprint di memoria di servizi di hosting che usano ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Fattori di prestazioni chiave

In teoria, tutti i siti Web deve essere attivo e in memoria per garantire risposte rapide per la richiesta successiva, ogni volta che si tratta. Fattori che possono influire sulla velocità di risposta del sito includono:

- Il tempo che necessario per un sito per riavviare dopo un pool di applicazioni viene riciclato. Questo è il tempo che necessario per avviare un processo del server web per il sito quando gli assembly del sito non sono più in memoria. I gli assembly di piattaforma sono ancora in memoria, in quanto vengono utilizzati da altri siti. Questa situazione è detta "sito disattivo, avvio framework warm" o semplicemente "freddo avvio del sito."
- La quantità di memoria occupa il sito. Condizioni per questo sono "utilizzo della memoria per ogni sito" o "working set non condiviso".

I nuovi miglioramenti delle prestazioni concentrano su entrambi i fattori.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Requisiti per le nuove caratteristiche di prestazioni

I requisiti per le nuove funzionalità possono essere ripartiti queste categorie:

- Miglioramenti che eseguono .NET Framework 4.
- Miglioramenti che richiedono .NET Framework 4.5, ma possono eseguire in qualsiasi versione di Windows.
- Miglioramenti che sono disponibili solo con .NET Framework 4.5 è in esecuzione in Windows 8.

Prestazioni aumentano con ogni livello di miglioramento che è possibile abilitare.

Alcuni dei miglioramenti apportati a .NET Framework 4.5 sfruttare funzionalità più ampia che riguardano anche altri scenari.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Condivisione di assembly comuni

**Requisito**: .NET Framework 4 e Visual Studio 11 Developer Preview SDK

Spesso, diversi siti in un server usano gli stessi assembly helper (ad esempio, gli assembly da un'applicazione di starter kit o sample). Ogni sito ha una propria copia di questi assembly nella directory Bin. Anche se il codice oggetto per gli assembly identico, è assembly separate, pertanto ogni assembly deve essere letta separatamente durante l'avvio a freddo del sito e tenuti in memoria.

Le nuove funzionalità di centralizzazione risolve questa procedura poco efficiente e riducono i requisiti di RAM e carico tempo. Consente di centralizzazione Windows mantenere una singola copia di ciascun assembly nel file system e singoli assembly nella cartella Bin del sito vengono sostituiti con i collegamenti simbolici alla singola copia. Se un singolo sito richiede una versione dell'assembly distinta, il collegamento simbolico viene sostituito dalla nuova versione dell'assembly, ed è interessato solo da tale sito.

Condivisione di assembly utilizzando i collegamenti simbolici, è necessario un nuovo strumento denominato aspnet\_intern.exe, che consente di creare e gestire l'archivio di assembly centralizzato. Viene fornito come parte di Visual Studio 11 Developer Preview SDK. (Tuttavia, funzionerà in un sistema che dispone solo di installato .NET Framework 4, supponendo di aver installato la versione più recente [aggiornare](https://support.microsoft.com/kb/2468871).)

Per assicurarsi che tutti gli assembly idonei sono state centralizzati, si esegue aspnet\_intern.exe periodicamente (ad esempio, una volta alla settimana come attività pianificata). Un utilizzo tipico è la seguente:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Per visualizzare tutte le opzioni, eseguire lo strumento senza argomenti.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Tramite la compilazione JIT multicore per l'avvio veloce

**Requisito**: .NET Framework 4.5

Per l'avvio di un sito disattivo, non solo si gli assembly devono essere letti dal disco, ma il sito deve essere compilato tramite JIT. Per un sito complesso, si possono aggiungere ritardi significativi. Una tecnica generale di nuovo in .NET Framework 4.5 consente di ridurre questi ritardi suddividendo la compilazione JIT tra core di processori disponibili. Ciò è lo stesso e quanto prima possibile utilizzando le informazioni raccolte durante precedente Avvia del sito. Questa funzionalità implementata dal [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) metodo.

Utilizzo di più core la compilazione JIT è abilitata per impostazione predefinita in ASP.NET, pertanto non è necessario eseguire alcuna operazione per sfruttare i vantaggi di questa funzionalità. Se si desidera disabilitare questa funzionalità, è possibile apportare la seguente impostazione nel file Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Ottimizzazione delle operazioni di garbage collection ottimizzare per la memoria

**Requisito**: .NET Framework 4.5

Quando un sito è in esecuzione, l'utilizzo dell'heap GC (garbage collector) può essere un fattore determinante per l'utilizzo della memoria. Ad esempio garbage collector, GC .NET Framework rende compromessi tra il tempo di CPU (frequenza e il significato di raccolte) e il consumo di memoria (spazio aggiuntivo che viene utilizzato per gli oggetti nuovi, liberati o liberare grado). Per le versioni precedenti, vengono fornite indicazioni su come configurare il catalogo globale per ottenere il giusto equilibrio (ad esempio, vedere [ASP.NET 2.0/3.5 Hosting configurazione condivisa](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Per .NET Framework 4.5, anziché le impostazioni più autonomo, un'impostazione di configurazione definito al carico di lavoro è disponibile che consente a tutti i precedentemente consigliati delle impostazioni di Garbage Collection nonché nuova ottimizzazione che offre prestazioni aggiuntive per ogni sito working set.

Per abilitare l'ottimizzazione della memoria di catalogo globale, aggiungere l'impostazione seguente al file Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Se si ha familiarità con le istruzioni precedenti per le modifiche a ASPNET. config, si noti che questa impostazione sostituisce le impostazioni precedenti, ad esempio, non è necessario impostare gcServer gcConcurrent, e così via. Non è necessario rimuovere le impostazioni precedenti.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>La prelettura per le applicazioni web

**Requisito**: .NET Framework 4.5 è in esecuzione in Windows 8

Per le diverse versioni, Windows ha inserito la tecnologia di [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) che riduce il costo di lettura dal disco di avvio dell'applicazione. Poiché l'avvio a freddo costituisce un problema soprattutto per le applicazioni client, questa tecnologia non è stato incluso in Windows Server, che include solo i componenti essenziali per un server. Questa funzionalità è ora disponibile nella versione più recente di Windows Server, in cui è possibile ottimizzare il lancio di singoli siti Web.

Per Windows Server, il prefetcher non è abilitato per impostazione predefinita. Per abilitare e configurare il prefetcher per l'hosting web ad alta densità, eseguire il seguente set di comandi dalla riga di comando:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Quindi, per integrare il prefetcher con le applicazioni ASP.NET, aggiungere quanto segue al file Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Web Form ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Controlli dati fortemente tipizzati

In ASP.NET 4.5, Web Form include alcuni miglioramenti per l'utilizzo di dati. Il primo miglioramento è controlli dati fortemente tipizzati. Per i controlli Web Form nelle versioni precedenti di ASP.NET, visualizzare un valore associato a dati mediante *Eval* e un'espressione di associazione di dati:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Per i data binding bidirezionale, utilizzare *associare*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

In fase di esecuzione, queste chiamate di usare la reflection per leggere il valore del membro specificato e quindi visualizzare il risultato nel markup. Questo approccio semplifica l'associazione dati contro dati arbitrari, unshaped.

Tuttavia, le espressioni di associazione di dati simile al seguente non supportano funzionalità quali IntelliSense per i nomi dei membri, spostamento (ad esempio, Vai a definizione) o verifica di questi nomi in fase di compilazione.

Per risolvere questo problema, ASP.NET 4.5 aggiunge la possibilità di dichiarare il tipo di dati dei dati associato a un controllo. Eseguire questa operazione utilizzando il nuovo *ItemType* proprietà. Quando si imposta questa proprietà, due nuove variabili tipizzate sono disponibili nell'ambito di espressioni di associazione di dati: *elemento* e *BindItem*. Poiché le variabili sono fortemente tipizzate, si ottengono i vantaggi completi per lo sviluppo di Visual Studio.


Per le espressioni di data binding bidirezionale, utilizzare il *BindItem* variabile:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


La maggior parte dei controlli in framework Web Form ASP.NET che supportano l'associazione di dati sono stati aggiornati per supportare il *ItemType* proprietà.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Associazione di modelli

Associazione di modelli estende l'associazione di dati nei controlli Web Form ASP.NET per utilizzare l'accesso ai dati focalizzato sul codice. Concetti relativi a incorpora il *ObjectDataSource* controllo e dall'associazione di modelli in MVC ASP.NET.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>La selezione dei dati

Per configurare un controllo di dati per l'utilizzo di associazione di modelli per selezionare i dati, impostare il controllo *SelectMethod* proprietà sul nome di un metodo nel codice della pagina. Il controllo dati chiama il metodo nel momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Non è necessario chiamare in modo esplicito il *DataBind* metodo.

Nell'esempio seguente, il *GridView* controllo è configurato per utilizzare un metodo denominato *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Si crea il *GetCategories* metodo nel codice della pagina. Per un'operazione di selezione semplice, il metodo non occorrono parametri e deve restituire un *IEnumerable* o *IQueryable* oggetto. Se il nuovo *ItemType* impostata (che consente fortemente tipizzate espressioni di associazione dati, come illustrato nella sezione [controlli dati fortemente tipizzati](#_Toc318097386) in precedenza), le versioni di queste interfacce generiche deve essere restituito, ovvero *IEnumerable&lt;T&gt;*  o *IQueryable&lt;T&gt;*, con la *T* che corrisponde al tipo di parametro di *ItemType* proprietà (ad esempio, *IQueryable&lt;categoria&gt;*).

Nell'esempio seguente viene illustrato il codice per un *GetCategories* metodo. In questo esempio Usa il modello Entity Framework Code First con il database di esempio Northwind. Il codice verifica che la query restituisce i dettagli relativi ai prodotti correlati per ogni categoria per mezzo del *Include* metodo. (In questo modo il *TemplateField* elemento nel markup viene visualizzato il conteggio dei prodotti in ogni categoria senza un [n + 1 selezionare](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Quando la pagina viene eseguita, la *GridView* chiamate di controllo di *GetCategories* metodo automaticamente ed esegue il rendering di dati restituiti utilizzando i campi configurati:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Poiché il metodo select restituisce un *IQueryable* oggetto, il *GridView* controllo possibile manipolare ulteriormente la query prima dell'esecuzione. Ad esempio, il *GridView* controllo consente di aggiungere espressioni di query per l'ordinamento e paging restituito *IQueryable* in modo che tali operazioni vengono eseguite dal sottostante dell'oggetto prima che venga eseguito, Provider LINQ. In questo caso, Entity Framework garantisce che tali operazioni vengono eseguite nel database.

Nell'esempio seguente il *GridView* controllo modificato per consentire l'ordinamento e paging:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Ora quando la pagina viene eseguita, il controllo può assicurarsi che venga visualizzata solo la pagina corrente dei dati e che viene ordinato in base alla colonna selezionata:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Per filtrare i dati restituiti, i parametri devono essere aggiunti al metodo select. Questi parametri verranno popolati dall'associazione del modello in fase di esecuzione e di utilizzarli per modificare la query prima di restituire i dati.

Si supponga, ad esempio, che si desidera consentire agli utenti filtrare i prodotti immettendo una parola chiave nella stringa di query. È possibile aggiungere un parametro al metodo e aggiornare il codice per utilizzare il valore del parametro:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Questo codice include un *in* espressione se viene fornito un valore per *parola chiave* e quindi restituisce i risultati della query.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Provider di valori

Nell'esempio precedente non era specifica su dove il valore per il *parola chiave* parametro di stato provenienti da. Per indicare le informazioni, è possibile utilizzare un attributo di parametro. Per questo esempio, è possibile utilizzare il *QueryStringAttribute* nella classe di *System.Web.ModelBinding* dello spazio dei nomi:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Questo indica l'associazione del modello per tentare di associare un valore dalla stringa di query per il *parola chiave* parametro in fase di esecuzione. (Questo può comportare l'esecuzione di conversione del tipo, anche se ciò non accade in questo caso.) Se non è possibile specificare un valore e il tipo non nullable, viene generata un'eccezione.

Origini dei valori di questi metodi sono definite come provider di valori e gli attributi di parametro che indicano quali provider di valori da utilizzare vengono definiti gli attributi del provider di valore. Web Form includerà i provider di valori e attributi corrispondenti per tutte le origini tipiche dell'input utente in un'applicazione Web Form, ad esempio la stringa di query, cookie, i valori del form, controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo. È anche possibile scrivere provider di valori personalizzati.

Per impostazione predefinita, il nome del parametro viene utilizzato come chiave per trovare un valore nella raccolta di provider di valore. Nell'esempio, il codice viene visualizzato un valore di stringa di query denominato (parola chiave) (ad esempio, ~ / default.aspx?keyword=chef). È possibile specificare una chiave personalizzata, passarlo come argomento per l'attributo di parametro. Ad esempio, per utilizzare il valore della variabile di stringa di query denominato q, è possibile farlo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Se questo metodo è incluso il codice della pagina, gli utenti possono filtrare i risultati passando una parola chiave utilizzando la stringa di query:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Associazione del modello esegue molte delle attività che altrimenti dovrebbero essere codice manualmente: leggere il valore, verifica di un valore null, tentare di convertirlo nel tipo appropriato, verifica se la conversione è riuscita e, infine, utilizzando il valore di query. Modello di associazione di risultati molto meno codice e la capacità di riutilizzare la funzionalità in tutta l'applicazione.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Applicando un filtro per i valori da un controllo

Si supponga che si desidera estendere l'esempio per consentire all'utente di scegliere un valore di filtro da un elenco di riepilogo a discesa. Aggiungere l'elenco a discesa seguente al markup e configurarlo per ottenere i dati da un altro metodo usando il *SelectMethod* proprietà:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

In genere aggiungerà inoltre un *EmptyDataTemplate* elemento per il *GridView* controllare in modo che il controllo verrà visualizzato un messaggio se non vengono trovati alcuna corrispondenza prodotti:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Nel codice della pagina, aggiungere che il nuovo seleziona il metodo per l'elenco a discesa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Infine, aggiornare il *GetProducts* selezionare il metodo per richiedere un nuovo parametro che contiene l'ID della categoria selezionata dall'elenco a discesa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Ora quando viene eseguita la pagina, gli utenti possono selezionare una categoria dall'elenco a discesa e *GridView* controllo viene automaticamente associato nuovamente per visualizzare i dati filtrati. Questo è possibile perché l'associazione del modello tiene traccia i valori dei parametri per i metodi di selezione e rileva se qualsiasi valore del parametro è stato modificato dopo un postback. In questo caso, l'associazione di modelli forza il controllo dati associato a cui associarsi nuovamente i dati.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Espressioni di associazione dati con codificata HTML

È possibile ora codificare in formato HTML il risultato delle espressioni di associazione di dati. Aggiungere i due punti (:) alla fine del &lt;% # prefisso che contrassegna l'espressione di associazione di dati:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Convalida non intrusiva

È ora possibile configurare i controlli di convalida predefinite per l'utilizzo di JavaScript non intrusivo per la logica di convalida sul lato client. Questo notevolmente riduce la quantità di codice JavaScript eseguito il rendering inline nel markup della pagina e riduce le dimensioni complessive della pagina. È possibile configurare per i controlli di convalida di JavaScript non intrusivo in uno dei modi seguenti:

- A livello globale aggiungendo la seguente impostazione di  *&lt;appSettings&gt;*  elemento nel file Web. config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- A livello globale impostando statica *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* proprietà *UnobtrusiveValidationMode.WebForms* (in genere nel *applicazione \_Avviare* metodo nel file Global. asax).
- Singolarmente per una pagina impostando la nuova *UnobtrusiveValidationMode* proprietà del *pagina* classe *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Aggiornamenti di HTML5

Alcune sono stati apportati miglioramenti a Web Form controlli server per sfruttare i vantaggi delle nuove funzionalità di HTML5:

- Il *TextMode* proprietà del *TextBox* controllo è stato aggiornato per supportare i nuovi tipi di input HTML5 come *posta elettronica*, *datetime*, e così via.
- Il *FileUpload* controllare supporta ora i file più caricamenti dai browser che supportano questa funzionalità HTML5.
- I controlli di convalida ora il supporto convalida HTML5 gli elementi di input.
- Nuovi elementi HTML5 con attributi che rappresentano un URL ora supportano runat = "server". Di conseguenza, è possibile utilizzare le convenzioni ASP.NET nei percorsi URL, ad esempio il ~ (operatore) per rappresentare la radice dell'applicazione (ad esempio, &lt;video runat = "server" src="~/myVideo.wmv" /&gt;).
- Il *UpdatePanel* controllo è stato risolto per supportare i campi di input di registrazione HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta è ora incluso in Visual Studio 11 Beta. ASP.NET MVC è un framework per lo sviluppo di applicazioni Web altamente gestibili e testabili sulla sfruttando il modello Model-View-Controller (MVC). ASP.NET MVC 4 semplifica la compilazione di applicazioni per il Web per dispositivi mobili e include l'API Web ASP.NET, che consente di compilare servizi HTTP che possono raggiungere qualsiasi dispositivo. Per ulteriori informazioni, vedere il [note sulla versione di ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Pagine Web ASP.NET 2

Nuove funzionalità includono quanto segue:

- Modelli di sito nuovi e aggiornati.
- Aggiunta del server e la convalida lato client utilizzando il *convalida* helper.
- Consente di registrare gli script utilizzando un gestore di risorse.
- Abilitazione dell'account di accesso di Facebook e ad altri siti con OAuth e OpenID.
- Aggiunta di mappe mediante il *esegue il mapping* helper.
- Esecuzione di applicazioni Web Pages side-by-side.
- Rendering di pagine per i dispositivi mobili.

Per ulteriori informazioni su queste funzioni e gli esempi di codice pagina intera, vedere [la parte superiore funzionalità nella versione Beta 2 di pagine Web](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

In questa sezione vengono fornite informazioni sui miglioramenti introdotti per lo sviluppo web in Visual Web Developer 11 Beta e Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Progetto condivisione tra Visual Studio 2010 e Visual Studio 2012 Release Candidate (compatibilità del progetto)

Fino a Visual Studio 2012 Release Candidate, apertura di un progetto esistente in una versione più recente di Visual Studio avviata la procedura guidata di conversione. Questo forzata un aggiornamento del contenuto (risorse) di una soluzione e progetto nuovi formati che non sono compatibili con le versioni precedenti. Di conseguenza, dopo la conversione è Impossibile aprire il progetto nella versione precedente di Visual Studio.

Molti clienti hanno chiesto che questo non è l'approccio giusto. In Visual Studio 11 Beta, è ora supportato condivisione progetti e soluzioni con Visual Studio 2010 SP1. Ciò significa che se si apre un progetto 2010 in Visual Studio 2012 Release Candidate, sarà comunque in grado di aprire il progetto in Visual Studio 2010 SP1.

> [!NOTE]
> Alcuni tipi di progetti non possono essere condivise tra Visual Studio 2010 SP1 e Visual Studio 2012 Release Candidate. Includono alcuni progetti precedenti (ad esempio, i progetti ASP.NET MVC 2) o per scopi speciali (ad esempio progetti di installazione).

Quando si apre un progetto Web di Visual Studio 2010 SP1 per la prima volta in Visual Studio 11 Beta, le proprietà seguenti vengono aggiunti al file di progetto:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags UpgradeBackupLocation e OldToolsVersion vengono utilizzati dal processo che aggiorna il file di progetto. Non hanno alcun impatto sull'utilizzo con il progetto in Visual Studio 2010.

VisualStudioVersion è una nuova proprietà usata da MSBuild 4.5 che indica la versione di Visual Studio per il progetto corrente. Poiché questa proprietà non esiste in MSBuild 4.0 (la versione di MSBuild che viene utilizzato Visual Studio 2010 SP1), un valore predefinito viene inserita nel file di progetto.

La proprietà VSToolsPath viene utilizzata per determinare il file con estensione targets corretto da importare dal percorso rappresentato dall'impostazione MSBuildExtensionsPath32.

Esistono inoltre alcune modifiche relative agli elementi di importazione. Queste modifiche sono necessari per supportare la compatibilità tra versioni di Visual Studio.

> [!NOTE]
> Se un progetto è condiviso tra Visual Studio 2010 SP1 e Visual Studio 11 Beta su due computer differenti e se il progetto include un database locale nell'App\_cartella dati, è necessario assicurarsi che sia la versione di SQL Server utilizzato dal database installato in entrambi i computer.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Modifiche di configurazione di modelli di siti Web 4.5 di ASP.NET

Le seguenti modifiche apportate allo stato predefinito *Web. config* file per il sito che vengono creati utilizzando i modelli di siti Web in Visual Studio 2012 Release Candidate:

- Nel `<httpRuntime>` elemento, il `encoderType` attributo è ora impostato per impostazione predefinita per utilizzare i tipi di AntiXSS che sono state aggiunte di ASP.NET. Per informazioni dettagliate, vedere [libreria AntiXSS](#_Toc318097382).
- Anche nel `<httpRuntime>` elemento, il `requestValidationMode` attributo è impostato su "4.5". Ciò significa che per impostazione predefinita, la convalida delle richieste è configurata per utilizzare la convalida posticipata (lazy"). Per informazioni dettagliate, vedere [nuove funzionalità di convalida richiesta ASP.NET](#_Toc318097379).
- Il `<modules>` elemento il `<system.webServer>` sezione non contiene un `runAllManagedModulesForAllRequests` attributo. (Valore predefinito è false). Questo significa che se si utilizza una versione di IIS 7 che non è stato aggiornato a SP1, si potrebbero avere problemi con il routing in un nuovo sito. Per ulteriori informazioni, vedere [supporto nativo in IIS 7 per il Routing ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Queste modifiche non influiscono le applicazioni esistenti. Tuttavia, potrebbero rappresentare una differenza nel comportamento tra i siti Web esistenti e nuovi siti Web creati per ASP.NET 4.5 con i nuovi modelli.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Supporto nativo di IIS 7 per il Routing di ASP.NET

Questo non è una modifica ad ASP.NET di conseguenza, ma una modifica nei modelli per nuovi progetti di sito Web che possono verificarsi se si utilizza una versione di IIS 7 che non è stato applicato l'aggiornamento di SP1.

In ASP.NET, è possibile aggiungere la seguente impostazione di configurazione di applicazioni per supportare il routing di:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Quando **runAllManagedModulesForAllRequests** è true, un URL come `http://mysite/myapp/home` passa a ASP.NET, anche se non esiste alcun *aspx*, *MVC*, o estensione simili nel URL.

Esegue un aggiornamento che è stato eseguito in IIS 7 di **runAllManagedModulesForAllRequests** impostazione non necessari e supporta il routing in modo nativo di ASP.NET. (Per informazioni sull'aggiornamento, vedere l'articolo del supporto Microsoft [è disponibile che consente di determinati gestori di IIS 7.0 o IIS 7.5 di gestire le richieste il cui URL non terminano con un periodo di un aggiornamento](https://support.microsoft.com/kb/980368).)

Se il sito Web è in esecuzione in IIS 7 e se IIS è stato aggiornato, è necessario impostare **runAllManagedModulesForAllRequests** su true. In effetti, impostarlo su true è sconsigliato, perché consente di aggiungere inutili l'overhead di elaborazione alla richiesta. Quando questa impostazione è true, tutte le richieste, incluse quelle per *htm*, *jpg*, e altri file statici, anche passare attraverso la pipeline di richieste ASP.NET.

Se si crea un nuovo sito Web ASP.NET 4.5 utilizzando i modelli disponibili in Visual Studio 2012 RC, la configurazione per il sito Web non include il **runAllManagedModulesForAllRequests** impostazione. Ciò significa che, per impostazione predefinita, l'impostazione è false.

Se si esegue quindi il sito Web in Windows 7 senza SP1 installato, IIS 7 non includerà l'aggiornamento richiesto. Di conseguenza, il routing non funzionerà e verranno visualizzati errori. Se si dispone di un problema in cui il routing non funziona, è possibile eseguire il codice seguente:

- Aggiornare Windows 7 SP1, che verrà aggiunto l'aggiornamento a IIS 7.
- Installare l'aggiornamento descritto nell'articolo di supporto Microsoft elencata in precedenza.
- Impostare **runAllManagedModulesForAllRequests** su true nel file Web. config del sito Web specificato. Si noti che le richieste verranno aggiunti un certo overhead.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Smart tag

Nella visualizzazione progettazione, le proprietà complesse dei controlli server spesso sono associati a finestre di dialogo e procedure guidate per semplificare la nuova impostazione. Ad esempio, è possibile utilizzare una speciale di finestra di dialogo per aggiungere un'origine dati da un *Ripetitore* controllare o aggiungere colonne a un *GridView* controllo.

Questo tipo di Guida dell'interfaccia utente per le proprietà complesse, tuttavia, non è stato disponibile nella visualizzazione origine. Di conseguenza, Visual Studio 11 introduce Smart Task per visualizzazione origine. Smart tag sono collegamenti che riconoscano il contesto per le funzionalità di usate comune negli editor di c# e Visual Basic.

Per i controlli Web Form ASP.NET, smart tag vengono visualizzati nei tag server come una piccola icona quando il punto di inserimento è all'interno dell'elemento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Quando si fare clic sull'icona o premere CTRL +, si espande Smart Task. (punto), come in Editor del codice. Visualizza quindi i tasti di scelta rapida sono simili alle attività intelligente nella visualizzazione progettazione.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Ad esempio, le Smart Task nell'illustrazione precedente mostra le opzioni di attività di GridView. Se si sceglie di modificare le colonne, viene visualizzata la finestra di dialogo seguenti:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

La compilazione nella finestra consente di impostare le stesse proprietà è possibile impostare nella visualizzazione progettazione. Quando si fa clic su OK, il markup per il controllo viene aggiornato con le nuove impostazioni:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Supporto WAI ARIA

Scrittura di siti Web accessibili sta diventando sempre più importante. Il [standard di accessibilità WAI ARIA](http://www.w3.org/WAI/intro/aria) definisce come gli sviluppatori devono scrivere siti Web accessibili. Questo standard è ora supportato integralmente in Visual Studio.

Ad esempio, il *ruolo* presenta ora IntelliSense completo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

Lo standard WAI ARIA introduce inoltre gli attributi che sono preceduti *aria -* che consente di aggiungere la semantica di un documento di HTML5. Visual Studio supporta pienamente questi *aria -* attributi:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nuovi frammenti di codice HTML5

Per rendere più veloce e semplice scrivere tag HTML5 comunemente utilizzati, Visual Studio include un numero di frammenti di codice. Il frammento di video è un esempio:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Per richiamare il frammento di codice, premere Tab due volte quando l'elemento è selezionato in IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Ciò produce un frammento di codice che è possibile personalizzare.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Estrai in controllo utente

Nelle pagine web di grandi dimensioni, può essere opportuno spostare singoli elementi nei controlli utente. Il modulo di refactoring può consentire di aumentare la leggibilità della pagina e può semplificare la struttura della pagina.

Per semplificare questa operazione, quando si modificano le pagine Web Form in visualizzazione origine, è possibile a questo punto selezionare il testo in una pagina, pulsante destro del mouse e quindi scegliere Estrai nel controllo utente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense per parti di codice negli attributi

Visual Studio ha sempre fornito IntelliSense per individuare il codice lato server in qualsiasi pagina o controllo. Visual Studio include ora IntelliSense per parti di codice in anche gli attributi HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Questo rende più semplice creare espressioni di associazione di dati:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Ridenominazione automatica delle corrispondenti tag quando si rinomina tag di apertura o chiusura

Se si rinomina un elemento HTML (ad esempio, si modifica un *div* tag da un *intestazione* tag), l'apertura o tag di chiusura corrispondente viene modificato anche in tempo reale.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Ciò consente di evitare l'errore in cui si dimentica di modificare un tag di chiusura o modificare quella sbagliata.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generazione di gestore eventi

Visual Studio ora include nuove funzionalità nella vista di origine che consentono di scrivere gestori eventi e li associano manualmente. Se si sta modificando un nome di evento nella visualizzazione origine, IntelliSense visualizza &lt;creare un nuovo evento&gt;, che verrà creato un gestore eventi nel codice della pagina che dispone della firma corretta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Per impostazione predefinita, il gestore dell'evento utilizzerà l'ID del controllo per il nome del metodo di gestione degli eventi:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Il gestore dell'evento risultante sarà simile al seguente (in questo caso, in c#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Rientro automatico

Quando si preme INVIO all'interno di un elemento HTML vuoto, l'editor viene inserito il punto di inserimento nella posizione corretta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Se si preme INVIO in questa posizione, il tag di chiusura viene spostato verso il basso e rientrato per corrisponde al tag di apertura. Il punto di inserimento viene inoltre applicato un rientro:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Ridurre automaticamente il completamento delle istruzioni

Elenco di IntelliSense in Visual Studio ora filtri in base a quanto digitato in modo da visualizzare solo le opzioni:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense anche i filtri basati sull'iniziali maiuscole di singole parole nell'elenco di IntelliSense. Ad esempio, se si digita "dl", vengono visualizzati sia dl e asp: DataList:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Questa funzionalità rende più veloce per ottenere il completamento istruzioni per gli elementi noti.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript (editor)

L'editor JavaScript in Visual Studio 2012 Release Candidate è completamente nuova e migliora notevolmente l'esperienza di utilizzo di JavaScript in Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Struttura del codice

Le aree della struttura vengono create automaticamente per tutte le funzioni, che consente di comprimere le parti del file che non sono pertinenti per l'elemento attivo corrente.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Corrispondenza parentesi graffe

Quando si posiziona il punto di inserimento di una parentesi o parentesi graffa di chiusura, l'editor evidenzia la corrispondente a uno.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Vai a definizione

Il comando Vai a definizione consente di passare all'origine per una funzione o variabile.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Supporto di ECMAScript5

L'editor supporta le API e la nuova sintassi in ECMAScript5, la versione più recente dello standard che descrive il linguaggio JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>IntelliSense DOM

IntelliSense per le API DOM è stata migliorata, con supporto per molte nuove API di HTML5, tra cui *querySelector*, archiviazione DOM, messaggistica tra documenti e *area di disegno*. IntelliSense DOM è ora controllata da un singolo file JavaScript semplice, anziché da una definizione di raccolta del tipo nativo. In questo modo facile estendere o sostituire.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Overload di firma VSDOC

Commenti dettagliati di IntelliSense possono essere dichiarati per separato overload di funzioni JavaScript ora utilizzando il nuovo  *&lt;firma&gt;*  elemento, come illustrato in questo esempio:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Riferimenti impliciti

È ora possibile aggiungere file JavaScript in un elenco centrale in modo implicito incluse nell'elenco dei file che determinato JavaScript file o un blocco riferimenti, vale a dire si otterranno IntelliSense per il relativo contenuto. Ad esempio, è possibile aggiungere file jQuery all'elenco dei file centrale, e verrà visualizzato IntelliSense per le funzioni di jQuery in qualsiasi blocco JavaScript del file, se è stato fatto riferimento in modo esplicito (utilizzando / / / &lt;riferimento /&gt;) o non.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS (editor)

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Ridurre automaticamente il completamento delle istruzioni

Elenco di IntelliSense per i filtri di ora CSS basati sulle proprietà CSS e i valori supportati per lo schema selezionato.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense supporta inoltre ricerche case titolo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Rientro gerarchico

L'editor CSS utilizza rientro visualizzazione gerarchica delle regole, che fornisce una panoramica di come le regole CSS vengono organizzate logicamente. Nell'esempio seguente, il #list un selettore CSS figlio dell'elenco e pertanto viene rientrato.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

L'esempio seguente mostra l'ereditarietà più complessa:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Il rientro di una regola è determinato dalle regole relativo padre. Rientro gerarchico è abilitato per impostazione predefinita, ma è possibile disabilitare la finestra di dialogo Opzioni (strumenti, opzioni dalla barra dei menu):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Supporto delle modifiche alla CSS

Analisi di centinaia di file CSS reali mostra che gli HACK CSS sono molto comuni e Visual Studio supporta ora quelle più diffuse. Questo supporto include IntelliSense e convalida della stella (\*) e caratteri di sottolineatura (\_) gli HACK proprietà:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Gli HACK selettore tipici sono supportati anche in modo che il rientro gerarchico viene mantenuto anche quando vengono applicati. Hack un selettore tipico utilizzato per Internet Explorer 7 destinazione consiste nell'anteporre un selettore con  *\*: primo figlio + html*. Utilizzando tale regola manterrà il rientro gerarchico:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schemi specifici del fornitore (- moz-, - webkit)

CSS3 introduce molte proprietà che sono state implementate dai diversi browser in momenti diversi. È stato in precedenza agli sviluppatori di codice per browser specifici utilizzando la sintassi specifici del fornitore. Queste proprietà specifiche del browser sono ora inclusi in IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Creazione di commenti e rimozione dei commenti sul supporto

È possibile inviare commenti e rimuovere le regole CSS utilizzando gli stessi collegamenti che utilizzano nell'editor di codice (Ctrl + K, Ctrl + K, è possibile rimuovere il commento e C al commento).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Selezione colori

Nelle versioni precedenti di Visual Studio, IntelliSense per gli attributi relativi ai colori è costituita da un elenco di riepilogo a discesa di valori di colore con nome. Tale elenco è stato sostituito da un controllo selezione colori completa.

Quando si immette un valore di colore, la selezione colori viene visualizzata automaticamente e visualizza un elenco di colori usati in precedenza, seguito da una tavolozza di colori predefinita. È possibile selezionare un colore utilizzando il mouse o tastiera.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

È possibile espandere l'elenco in un controllo selezione colori completo. Il selettore consente di controllare il canale alfa convertendo qualsiasi colore in RGBA automaticamente quando si sposta il cursore:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Frammenti di codice

Frammenti di codice nell'editor CSS rendono più semplice e rapido per creare stili tra browser. Molte proprietà CSS3 che richiedono impostazioni specifiche del browser a questo punto sono state annullate in frammenti di codice.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Frammenti di codice CSS supportare scenari avanzati (ad esempio query di CSS3 media) digitando il simbolo at (@), che mostra l'elenco di IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Quando si seleziona @media valore e premere Tab, l'editor CSS inserisce il frammento di codice seguente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Come con i frammenti di codice, è possibile creare frammenti personalizzati CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Aree personalizzate

Aree di codice, che sono già disponibili nell'editor di codice, denominato sono ora disponibili per la modifica di CSS. Ciò consente di essere facilmente raggruppate in blocchi di stile correlati.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Quando un'area è compresso viene visualizzato il nome dell'area:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Controllo pagina

Controllo pagina è uno strumento che esegue il rendering di una pagina web (HTML, Web Form, ASP.NET MVC o Web Pages) nell'IDE di Visual Studio e consente di esaminare il codice sorgente sia l'output risultante. Per le pagine ASP.NET, controllo pagina consente di determinare quale codice lato server ha prodotto il markup HTML che viene eseguito il rendering nel browser.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Per ulteriori informazioni su controllo pagina, vedere le esercitazioni seguenti:

- Con Page Inspector in [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Con Page Inspector in [Web Forms ASP.NET](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Pubblicazione

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profili di pubblicazione

In Visual Studio 2010, le informazioni di pubblicazione per i progetti di applicazione Web non sono archiviate nel controllo della versione e non sono progettate per la condivisione con altri utenti. In Visual Studio 2012 Release Candidate, il formato del profilo di pubblicazione è stato modificato. È in un elemento del team e ora è facile usare dalle compilazioni in base a MSBuild. Informazioni sulla configurazione di compilazione è nella finestra di dialogo Pubblica in modo che è possibile passare facilmente le configurazioni di compilazione prima della pubblicazione.

Pubblicare i profili vengono archiviati nella cartella PublishProfiles. Il percorso della cartella varia a seconda del linguaggio di programmazione in uso:

- In c#: Properties\PublishProfiles
- Visual Basic: Project\PublishProfiles

Ogni profilo è un file di MSBuild. Durante la pubblicazione, questo file viene importato nel file di progetto MSBuild. In Visual Studio 2010, se si desidera apportare modifiche al processo di pubblicazione o di pacchetto, è necessario inserire le personalizzazioni in un file denominato **ProjectName**. wpp.targets. Questa funzionalità è ancora supportata, ma è possibile inserire le personalizzazioni nel profilo di pubblicazione stessa. In questo modo, le personalizzazioni verranno utilizzate solo per il profilo.

È possibile anche usare profili da MSBuild di pubblicazione. A tale scopo, utilizzare il comando seguente, quando si compila il progetto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Il valore project.csproj è il percorso del progetto e ProfileName è il nome del profilo da pubblicare. In alternativa, anziché passare il nome del profilo per il *PublishProfile* proprietà, è possibile passare il percorso completo per il profilo di pubblicazione.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Precompilazione ASP.NET e tipo merge

Per i progetti di applicazione Web, Visual Studio 2012 Release Candidate aggiunge un'opzione nella pagina Proprietà pubblicazione/creazione pacchetto Web che consente di precompilare e unire il contenuto del sito quando si pubblica o un pacchetto del progetto. Per visualizzare queste opzioni, fare clic sul progetto in Esplora soluzioni, scegliere Proprietà e quindi scegliere la pagina delle proprietà pubblicazione/creazione pacchetto Web. Nella figura seguente viene illustrato il Precompile tale applicazione prima opzione di pubblicazione.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Quando questa opzione è selezionata, Visual Studio consente di precompilare l'applicazione ogni volta che pubblica o il pacchetto dell'applicazione web. Se si desidera controllare la modalità con cui il sito è precompilato o come assembly vengono uniti, fare clic sul pulsante per configurare le opzioni avanzate.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Il server web predefinito per i progetti di test web in Visual Studio è ora IIS Express. Visual Studio Development Server è ancora un'opzione per server web locale durante lo sviluppo, ma IIS Express è ora il server consigliato. L'esperienza di utilizzo di IIS Express di Visual Studio 11 Beta è molto simile a quello in Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute nel presente documento rappresentano l'attuale opinione di Microsoft Corporation circa le problematiche discusse alla data della pubblicazione. Poiché Microsoft deve rispondere ai cambiamenti delle condizioni di mercato, il presente documento non deve essere interpretato quale un impegno da parte di Microsoft e Microsoft non può garantire l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è a esclusivo carico dell'utente. Senza limitare i diritti sanciti dal copyright, nessuna parte di questo documento può essere riprodotta, memorizzata o inserita in un sistema di ricerca o trasmessa in qualsiasi forma o mezzo (elettronico o meccanico, mediante fotocopia, registrazione o altro), per alcuno scopo, senza l'autorizzazione scritta di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto del presente documento. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna del presente documento non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non diversamente specificato, la società, organizzazioni, prodotti, nomi di dominio, indirizzi di posta elettronica, logo, persone, luoghi ed eventi citati in questo documento sono fittizio e nessuna associazione società, organizzazione, prodotto, nome di dominio, posta elettronica indirizzo, logo, persona, luogo o evento è intenzionale o può essere presupposta.

© 2012 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
