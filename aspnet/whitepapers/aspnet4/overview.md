---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 e panoramica di sviluppo Web di Visual Studio 2010 | Microsoft Docs
author: rick-anderson
description: Questo documento fornisce una panoramica di molte delle nuove funzionalità per ASP.NET che sono inclusi in.NET Framework 4 e in Visual Studio 2010.
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 5c7aa95b18bc0a97f42cc981476c110830286fa5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829043"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>Panoramica sullo sviluppo di Visual Studio 2010 Web e ASP.NET 4
====================
> Questo documento fornisce una panoramica di molte delle nuove funzionalità per ASP.NET che sono inclusi in.NET Framework 4 e in Visual Studio 2010.
> 
> [Scarica questo white paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Sommario**

**[Servizi di base](#0.2__Toc253429238 "_Toc253429238")**  
[File Web. config Refactoring](#0.2__Toc253429239 "_Toc253429239")  
[La memorizzazione nella cache di Output estensibile](#0.2__Toc253429240 "_Toc253429240")  
[Avvio automatico Web Applications](#0.2__Toc253429241 "_Toc253429241")  
[Reindirizzamento permanente di una pagina](#0.2__Toc253429242 "_Toc253429242")  
[Lo stato della sessione di compattazione](#0.2__Toc253429243 "_Toc253429243")  
[Espandendo l'intervallo di URL consentiti](#0.2__Toc253429244 "_Toc253429244")  
[Convalida delle richieste estendibile](#0.2__Toc253429245 "_Toc253429245")  
[La memorizzazione nella cache dell'oggetto e l'oggetto estendibilità di memorizzazione nella cache](#0.2__Toc253429246 "_Toc253429246")  
[HTML estendibile, URL e codifica delle intestazioni HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitoraggio delle prestazioni per le singole applicazioni in un singolo processo di lavoro](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery è incluso con Web Form e MVC](#0.2__Toc253429251 "_Toc253429251")  
[Supporto di rete di distribuzione del contenuto](#0.2__Toc253429252 "_Toc253429252")  
[Gli script esplicito di ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Web Form](#0.2__Toc253429256 "_Toc253429256")**  
[Impostazione metatag con le proprietà di Page.MetaDescription e Page.MetaKeywords](#0.2__Toc253429257 "_Toc253429257")  
[L'abilitazione dello stato di visualizzazione per i singoli controlli](#0.2__Toc253429258 "_Toc253429258")  
[Modifiche alle funzionalità del Browser](#0.2__Toc253429259 "_Toc253429259")  
[Routing in ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Impostazione degli ID Client](#0.2__Toc253429261 "_Toc253429261")  
[Selezione di righe persistente nei controlli dati](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET Chart Control](#0.2__Toc253429263 "_Toc253429263")  
[Filtro dei dati con il controllo QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Le espressioni di codice codificata in formato HTML](#0.2__Toc253429265 "_Toc253429265")  
[Le modifiche al modello di progetto](#0.2__Toc253429266 "_Toc253429266")  
[Miglioramenti CSS](#0.2__Toc253429267 "_Toc253429267")  
[Nascondere elementi intorno a campi nascosti div](#0.2__Toc253429268 "_Toc253429268")  
[Il rendering di una tabella esterna per i controlli basati su modelli](#0.2__Toc253429269 "_Toc253429269")  
[Miglioramenti al controllo ListView](#0.2__Toc253429270 "_Toc253429270")  
[Miglioramenti di controllo RadioButtonList e CheckBoxList](#0.2__Toc253429271 "_Toc253429271")  
[Miglioramenti al controllo menu](#0.2__Toc253429272 "_Toc253429272")  
[Procedura guidata e controlli CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Supporto di aree](#0.2__Toc253429275 "_Toc253429275")  
[Il supporto della convalida di annotazione dei dati attributo](#0.2__Toc253429276 "_Toc253429276")  
[Helper basati su modelli](#0.2__Toc253429277 "_Toc253429277")

**[I dati dinamici](#0.2__Toc253429278 "_Toc253429278")**  
[Abilitazione di Dynamic Data per i progetti esistenti](#0.2__Toc253429279 "_Toc253429279")  
[Sintassi dichiarativa per il controllo DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[I modelli di entità](#0.2__Toc253429281 "_Toc253429281")  
[Nuovi modelli di campo per URL e indirizzi di posta elettronica](#0.2__Toc253429282 "_Toc253429282")  
[Creazione di collegamenti con il controllo DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Supporto dell'ereditarietà nel modello di dati delle](#0.2__Toc253429284 "_Toc253429284")  
[Supporto per le relazioni molti-a-molti (solo per Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nuovi attributi di visualizzazione del controllo e il supporto delle enumerazioni](#0.2__Toc253429286 "_Toc253429286")  
[Supporto migliorato per i filtri](#0.2__Toc253429287 "_Toc253429287")

**[Miglioramenti di sviluppo Web di Visual Studio 2010](#0.2__Toc253429288 "_Toc253429288")**  
[Compatibilità CSS migliorata](#0.2__Toc253429289 "_Toc253429289")  
[Frammenti di codice JavaScript e HTML](#0.2__Toc253429290 "_Toc253429290")  
[Miglioramenti di IntelliSense per JavaScript](#0.2__Toc253429291 "_Toc253429291")

**[Distribuzione di applicazioni con Visual Studio 2010 Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web Packaging](#0.2__Toc253429293 "_Toc253429293")  
[Trasformazione Web. config](#0.2__Toc253429294 "_Toc253429294")  
[Distribuzione del database](#0.2__Toc253429295 "_Toc253429295")  
[Pubblicazione di applicazioni Web con un clic](#0.2__Toc253429296 "_Toc253429296")  
[Le risorse](#0.2__Toc253429297 "_Toc253429297")

**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Servizi di base

ASP.NET 4 introduce numerose funzionalità che consentono di migliorare i servizi ASP.NET core, ad esempio la memorizzazione nella cache di output e l'archiviazione dello stato della sessione.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>File Web. config di Refactoring

Il `Web.config` file che contiene la configurazione per un'applicazione Web è aumentato notevolmente rispetto di alcune versioni precedenti di .NET Framework come sono state aggiunte nuove funzionalità, ad esempio Ajax, routing e l'integrazione con IIS 7. Questo ha reso più difficile configurare o avviare nuove applicazioni Web senza uno strumento come Visual Studio. In the .NET Framework 4, sono stati spostati gli elementi di configurazione principali di `machine.config` file e applicazioni ora ereditano queste impostazioni. In questo modo il `Web.config` file nelle applicazioni ASP.NET 4 può essere vuoto o contenere solo le righe seguenti, che specificano per Visual Studio versione del framework è destinata all'applicazione:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>La memorizzazione nella cache di Output estensibile

Dal momento in cui è stato rilasciato ASP.NET 1.0, la memorizzazione nella cache di output ha consentito agli sviluppatori di archiviare l'output generato da pagine, controlli e le risposte HTTP in memoria. Nelle successive richieste Web, ASP.NET può gestire il contenuto più rapidamente recuperando l'output generato dalla memoria invece di rigenerare l'output da zero. Tuttavia, questo approccio presenta una limitazione: contenuto generato deve sempre essere archiviati in memoria e nei server in cui si verifica un traffico elevato, la memoria utilizzata per la memorizzazione nella cache di output può competere con richieste di memoria da altre parti di un'applicazione Web.

ASP.NET 4 consente di aggiungere un punto di estendibilità per la memorizzazione nella cache di output che consente di configurare uno o più provider di cache di output personalizzati. Provider di cache di output è possibile usare qualsiasi meccanismo di archiviazione per mantenere il contenuto HTML. In questo modo è possibile creare provider di cache di output personalizzati per i meccanismi di persistenza diverse, che possono includere i dischi locali o remoti, archiviazione cloud e motori di cache distribuiti.

Si crea un provider di cache di output personalizzato come una classe che deriva dalla nuova *System.Web.Caching.OutputCacheProvider* tipo. È quindi possibile configurare il provider nel `Web.config` file con la nuova *provider* sottosezione del *outputCache* elemento, come illustrato nell'esempio seguente:

[!code-xml[Main](overview/samples/sample2.xml)]

Per impostazione predefinita in ASP.NET 4, tutte le risposte HTTP, viene eseguito il rendering delle pagine e controlli di usano la cache di output in memoria, come illustrato nell'esempio precedente, in cui il *defaultProvider* attributo è impostato su AspNetInternalProvider. È possibile modificare il provider di cache di output predefinito utilizzato per un'applicazione Web specificando un nome del provider diversa *defaultProvider*.

Inoltre, è possibile selezionare provider di cache di output diversi per ogni controllo e per ogni richiesta. Il modo più semplice per scegliere un provider di cache di output diversi per diversi controlli utente Web deve eseguire in modo dichiarativo tramite la nuova *providerName* attributo in una direttiva di controllo, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Specifica di un provider di cache di output diversa per una richiesta HTTP richiede un po' più di lavoro. Anziché specificare in modo dichiarativo il provider, si esegue l'override di nuovo *GetOuputCacheProviderName* metodo nella `Global.asax` file per specificare a livello di programmazione quali provider da utilizzare per una richiesta specifica. Nell'esempio seguente viene illustrato come effettuare questa operazione.

[!code-csharp[Main](overview/samples/sample4.cs)]

Con l'aggiunta di estendibilità del provider di cache di output per ASP.NET 4, è ora possibile eseguire più aggressive e più intelligenti strategie di memorizzazione nella cache di output per i siti Web. Ad esempio, è ora possibile memorizzare nella cache le pagine di "Top 10" di un sito in memoria, durante la memorizzazione nella cache le pagine con un minore traffico su disco. In alternativa, è possibile memorizzare nella cache ogni diversa combinazione di una pagina sottoposta a rendering, ma usare una cache distribuita in modo che il consumo di memoria viene scaricato dai server Web front-end.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Applicazioni Web di avvio automatico

Alcune applicazioni Web è necessario caricare grandi quantità di dati o eseguire l'inizializzazione costosa elaborazione prima di inviarla alla prima richiesta. Nelle versioni precedenti di ASP.NET, questi casi era necessario ideare approcci personalizzati per "riattivazione" un'applicazione ASP.NET e quindi eseguire il codice di inizializzazione durante la *Application\_carico* metodo nella `Global.asax` file.

Una nuova funzionalità di scalabilità denominata *avvio automatico* che direttamente gli indirizzi di questo scenario è disponibile se ASP.NET 4 viene eseguito in IIS 7.5 su Windows Server 2008 R2. La funzionalità avvio automatico offre un approccio controllato per l'avvio di un pool di applicazioni, l'inizializzazione di un'applicazione ASP.NET e quindi accettare le richieste HTTP.

> [!NOTE] 
> 
> Modulo IIS Application warm-up per IIS 7.5
> 
> Il team IIS ha rilasciato la versione beta test prima del modulo Application warm-up per IIS 7.5. In questo modo le applicazioni ancora più semplice di quanto descritto in precedenza di riscaldamento. Invece di scrivere codice personalizzato, specificare gli URL delle risorse da eseguire prima l'applicazione Web accetta le richieste dalla rete. Il warm-up si verifica durante l'avvio del servizio IIS (se è stato configurato il pool di applicazioni IIS come *AlwaysRunning*) e quando un processo di lavoro IIS ricicla. Durante il riciclo, il processo di lavoro IIS precedente continua a eseguire le richieste finché il processo di lavoro appena generato viene completamente preparato, in modo che le applicazioni presentare alcun rischio di interruzioni o altri problemi dovuti alle cache unprimed. Si noti che questo modulo funziona con qualsiasi versione di ASP.NET, a partire dalla versione 2.0.
> 
> Per altre informazioni, vedere [Application warm-up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) sul sito IIS.net Web. Per una procedura dettagliata che illustra come usare la funzionalità di riscaldamento, vedere [Guida introduttiva con il modulo di IIS 7.5 Application warm-up](https://www.iis.net/learn/manage) sul sito IIS.net Web.


Per usare la funzionalità avvio automatico, un amministratore IIS imposta un pool di applicazioni in IIS 7.5 per essere avviato automaticamente con la configurazione seguente nel `applicationHost.config` file:

[!code-xml[Main](overview/samples/sample5.xml)]

Poiché un singolo pool di applicazioni può contenere più applicazioni, si specificano alle singole applicazioni di essere avviato automaticamente con la configurazione seguente nel `applicationHost.config` file:

[!code-xml[Main](overview/samples/sample6.xml)]

Quando un server IIS 7.5 è avviati a freddo o un pool di applicazioni viene riciclato, IIS 7.5 utilizza le informazioni contenute nel `applicationHost.config` file per determinare che necessitano di applicazioni Web per essere avviato automaticamente. Per ogni applicazione che è contrassegnato per l'avvio automatico, IIS 7.5 invia una richiesta ad ASP.NET 4 per avviare l'applicazione in uno stato durante i quali l'applicazione temporaneamente non accetta richieste HTTP. Quando è in questo stato, ASP.NET crea un'istanza di tipo definito per il *serviceAutoStartProvider* attributo (come illustrato nell'esempio precedente) e la chiamata al suo punto di ingresso pubblici.

Si crea un tipo di avvio automatico gestito con punto di ingresso implementando il *IProcessHostPreloadClient* interfaccia, come illustrato nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample7.cs)]

PO inicializaci il codice viene eseguito nel *Preload* metodo e il metodo termina, l'applicazione ASP.NET è pronto per elaborare le richieste.

Con l'aggiunta di avvio automatico per,5 IIS e ASP.NET 4, è ora disponibile un approccio ben definito per l'esecuzione di inizializzazione dell'applicazione costosa prima dell'elaborazione della prima richiesta HTTP. Ad esempio, è possibile usare la nuova funzionalità di avvio automatico per inizializzare un'applicazione e quindi segnalare un bilanciamento del carico che l'applicazione è stato inizializzato e pronto per accettare il traffico HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Reindirizzamento permanente di una pagina

È pratica comune nelle applicazioni Web per spostare le pagine e altro contenuto intorno nel corso del tempo, causando un accumulo di collegamenti non aggiornati nei motori di ricerca. In ASP.NET, gli sviluppatori hanno tradizionalmente gestito richieste per gli URL precedenti usando il *Response. Redirect* metodo inoltrare una richiesta al nuovo URL. Tuttavia, il *reindirizzare* metodo genera una risposta HTTP 302-trovato (reindirizzamento temporaneo), con conseguente round trip aggiuntivo HTTP quando gli utenti provano ad accedere gli URL precedenti.

Aggiunge un nuovo ASP.NET 4 *RedirectPermanent* metodo helper che rende più semplice da rilasciare HTTP 301 spostato in modo permanente le risposte, come nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample8.cs)]

I motori di ricerca e altri agenti utente che riconoscono i reindirizzamenti permanenti archivia il nuovo URL che è associato il contenuto, che consente di eliminare il round trip non necessari effettuate dal browser per i reindirizzamenti temporanei.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Riduzione dello stato della sessione

ASP.NET fornisce due opzioni predefinite per l'archiviazione dello stato della sessione in una Web farm: un provider dello stato della sessione che richiama un server dello stato della sessione out-of-process e un provider dello stato della sessione che archivia i dati in un database Microsoft SQL Server. Poiché entrambe le opzioni includono la memorizzazione delle informazioni sullo stato all'esterno di processo di lavoro di un'applicazione Web, lo stato della sessione deve essere serializzato prima che venga inviato in un archivio remoto. A seconda della quantità di informazioni consente di salvare uno sviluppatore nello stato sessione, le dimensioni dei dati serializzati possono diventare molto esteso.

ASP.NET 4 introduce una nuova opzione di compressione per entrambi i tipi di provider dello stato della sessione out-of-process. Quando la *compressionEnabled* illustrato nell'esempio seguente l'opzione di configurazione è impostato su *true*, ASP.NET comprimerà (e decomprimere) lo stato della sessione serializzato con .NET Framework  *System.IO.Compression.GZipStream* classe.

[!code-xml[Main](overview/samples/sample9.xml)]

Con la semplice aggiunta del nuovo attributo per il `Web.config` file, le applicazioni con cicli di CPU nei server Web può realizzare sostanziale riduzioni delle dimensioni dei dati serializzati dello stato della sessione.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Espandendo l'intervallo di URL consentiti

ASP.NET 4 introduce nuove opzioni per l'espansione dell'URL dell'applicazione. Le versioni precedenti di ASP.NET vincolata lunghezze di percorso URL a 260 caratteri, sulla base del limite di percorso del file system NTFS. In ASP.NET 4, è possibile aumentare questo limite come appropriato per le applicazioni, (o diminuire) utilizzando due nuove *httpRuntime* attributi di configurazione. Nell'esempio seguente mostra questi nuovi attributi.

[!code-xml[Main](overview/samples/sample10.xml)]

Per consentire più brevi o più percorsi (parte dell'URL che non include protocol, nome del server e stringa di query), modificare il *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* attributo. Per consentire le stringhe di query superiori o inferiori, modificare il valore della *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* attributo.

ASP.NET 4 consente inoltre di configurare i caratteri che vengono utilizzati dal controllo di carattere di URL. Quando ASP.NET rileva un carattere non valido nella parte di percorso di un URL, rifiuta la richiesta e genera un errore HTTP 400. Nelle versioni precedenti di ASP.NET, i controlli sui caratteri URL erano limitati a un set fisso di caratteri. In ASP.NET 4, è possibile personalizzare il set di caratteri validi usando le nuove *requestPathInvalidChars* attributo il *httpRuntime* elemento di configurazione, come illustrato nell'esempio seguente:

[!code-xml[Main](overview/samples/sample11.xml)]

Per impostazione predefinita, il <em>requestPathInvalidChars</em> attributo definisce otto caratteri come non validi. (Nella stringa di cui è assegnata <em>requestPathInvalidChars</em> per impostazione predefinita<em>,</em>il minore di (&lt;), maggiore di (&gt;) ed e commerciale (&amp;) sono caratteri codifica, in quanto il `Web.config` file è un file XML.) È possibile personalizzare il set di caratteri non validi in base alle esigenze.

> [!NOTE]
> Si noti che ASP.NET 4 sempre rifiuta i percorsi URL che contengono caratteri compresi nell'intervallo ASCII da 0x00 a 0x1F, perché questi sono i caratteri URL non validi come definito in RFC 2396 di IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). Nelle versioni di Windows Server che eseguono IIS 6 o versioni successive, il driver di dispositivo del protocollo HTTP. sys Rifiuta automaticamente gli URL con questi caratteri.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Convalida delle richieste estendibile

Convalida delle richieste ASP.NET esegue la ricerca di dati della richiesta HTTP in ingresso per le stringhe che vengono comunemente usati negli attacchi di cross-site scripting (XSS). Se vengono rilevate potenziali stringhe XSS, la convalida delle richieste la stringa sospetta e restituisce un errore. La convalida delle richieste predefinito restituisce un errore solo quando vengono rilevati i più comuni stringhe usate in attacchi XSS. Tentativi precedenti per rendere più aggressiva la convalida di XSS ha restituito troppi falsi positivi. Tuttavia, i clienti potrebbe essere necessario controlli di convalida delle richieste che è più aggressiva, o viceversa, potrebbe essere necessario intenzionalmente i XSS per pagine specifiche o per tipi specifici di richieste.

In ASP.NET 4, la funzionalità di convalida della richiesta è estensibile, in modo che è possibile usare per la logica di convalida delle richieste personalizzata. Per estendere la convalida delle richieste, si crea una classe che deriva dalla nuova *System.Web.Util.RequestValidator* tipo e si configura l'applicazione (nelle *httpRuntime* sezione la `Web.config`file) per utilizzare il tipo personalizzato. Nell'esempio seguente viene illustrato come configurare una classe di convalida delle richieste personalizzata:

[!code-xml[Main](overview/samples/sample12.xml)]

Il nuovo *requestValidationType* attributo richiede una stringa di tipo identificatore di tipo .NET Framework standard che specifica la classe che fornisce la convalida delle richieste personalizzata. Per ogni richiesta, ASP.NET richiama il tipo personalizzato per elaborare ogni blocco di dati di richiesta HTTP in ingresso. L'URL in ingresso, tutte le intestazioni HTTP (cookie e intestazioni personalizzate) e il corpo dell'entità sono tutti disponibili per l'ispezione da una classe di convalida personalizzata richiesta simile a quello illustrato nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample13.cs)]

Per i casi in cui non si desidera controllare una parte dei dati HTTP in ingresso, la classe di convalida delle richieste può eseguire il fallback per consentire la convalida delle richieste ASP.NET predefinita chiamando semplicemente *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>La memorizzazione nella cache dell'oggetto e l'oggetto estendibilità di memorizzazione nella cache

Sin dal primo rilascio, ASP.NET ha incluso una cache di oggetti in memoria potenti (*Caching*). L'implementazione della cache è stata così popolare che è stato utilizzato nelle applicazioni non Web. Tuttavia, è difficile a includere un riferimento a un'applicazione Windows Forms o WPF `System.Web.dll` solo per essere in grado di usare la cache degli oggetti ASP.NET.

Per rendere la memorizzazione nella cache disponibile per tutte le applicazioni, .NET Framework 4 introduce un nuovo assembly, un nuovo spazio dei nomi, alcuni tipi di base e un concreto implementazione di memorizzazione nella cache. Il nuovo `System.Runtime.Caching.dll` assembly contiene una nuova API di memorizzazione nella cache nel *Caching* dello spazio dei nomi. Lo spazio dei nomi contiene due set di base di classi:

- Tipi astratti che forniscono le basi per la creazione di qualsiasi tipo di implementazione della cache personalizzata.
- Un'implementazione della cache oggetti in memoria concreto (le *System.Runtime.Caching.MemoryCache* classe).

Il nuovo *MemoryCache* classe viene modellata strettamente sulla cache di ASP.NET e gran parte della logica di gestione della cache interna condivide con ASP.NET. Anche se le API pubbliche di memorizzazione nella cache nel *Caching* sono state aggiornate per supportare lo sviluppo della cache personalizzate, se si usa ASP.NET *Cache* oggetto, si troveranno familiari concetti nel nuove API.

Un'analisi approfondita del nuovo *MemoryCache* classe e che supportano le API di base richiederebbe un intero documento. Tuttavia, l'esempio seguente offre un'idea di come funziona la nuova API di cache. L'esempio è stato scritto per un'applicazione Windows Forms, senza alcuna dipendenza su `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>HTML estendibile, URL e codifica delle intestazioni HTTP

In ASP.NET 4, è possibile creare le routine di codifica personalizzate per le attività di codifica di testo comuni seguenti:

- La codifica HTML.
- Codifica URL.
- Codifica dell'attributo HTML.
- Codifica delle intestazioni HTTP in uscita.

È possibile creare un codificatore personalizzato mediante derivazione dalla nuova *System.Web.Util.HttpEncoder* tipo e quindi la configurazione di ASP.NET per utilizzare il tipo personalizzato nel *httpRuntime* sezione la `Web.config` file, come illustrato nell'esempio seguente:

[!code-xml[Main](overview/samples/sample15.xml)]

Dopo aver configurato un codificatore personalizzato, ASP.NET richiama automaticamente l'implementazione di codifica personalizzata ogni volta che pubblica metodi di codifica il *System.Web.HttpUtility* o *System.Web.HttpServerUtility* classi sono definite. In tal modo una parte di un team di sviluppo Web di creare un codificatore personalizzato che implementa una codifica dei caratteri aggressivi, mentre il resto del team di sviluppo Web continua a usare la codifica le API pubbliche di ASP.NET. Configurando in modo centralizzato un codificatore personalizzato nel *httpRuntime* elemento, si ha la garanzia che tutte le chiamate di codifica di testo dalla codifica le API pubbliche di ASP.NET vengono indirizzate attraverso il codificatore personalizzato.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitoraggio delle prestazioni per le singole applicazioni in un singolo processo di lavoro

Per aumentare il numero di siti Web che può essere ospitato in un singolo server, molti hoster eseguire più applicazioni ASP.NET in un singolo processo di lavoro. Tuttavia, se più applicazioni utilizzano un processo di lavoro condiviso singolo, è difficile per gli amministratori del server identificare una singola applicazione che si è verificati problemi.

ASP.NET 4 si avvale delle nuove funzionalità di monitoraggio delle risorse introdotte da CLR. Per abilitare questa funzionalità, è possibile aggiungere il seguente frammento di configurazione XML per il `aspnet.config` file di configurazione.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Si noti il `aspnet.config` file si trova nella directory in cui è installato .NET Framework. Non è il `Web.config` file.


Quando la *appDomainResourceMonitoring* funzionalità è stata abilitata, due nuovi contatori delle prestazioni sono disponibili nella categoria performance "Le applicazioni ASP.NET": *% tempo processore gestito* e *Gestione della memoria usata*. Entrambi questi contatori delle prestazioni consente di tenere traccia del tempo di CPU stimato e utilizzo della memoria gestita delle singole applicazioni ASP.NET la nuova funzionalità di gestione risorse di dominio dell'applicazione CLR. Con ASP.NET 4, di conseguenza, gli amministratori hanno ora una vista più granulare sull'utilizzo di risorse di singole applicazioni in esecuzione in un singolo processo di lavoro.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Multitargeting

È possibile creare un'applicazione destinata a una versione specifica di .NET Framework. In ASP.NET 4, un nuovo attributo nella *compilation* elemento del `Web.config` file consente di usare .NET Framework 4 e versioni successive. Se si definisce in modo esplicito come destinazione .NET Framework 4 e, se si includono elementi facoltativi nel `Web.config` file, ad esempio le voci relative *System. CodeDom*, questi elementi devono essere corretti per .NET Framework 4. (Se non esplicitamente destinazione è .NET Framework 4, il framework di destinazione viene dedotto dalla mancanza di una voce nel `Web.config` file.)

L'esempio seguente illustra l'uso del *targetFramework* attributo il *compilazione* elemento del `Web.config` file.

[!code-xml[Main](overview/samples/sample17.xml)]

Si noti quanto segue come destinazione una versione specifica di .NET Framework:

- In un pool di applicazioni .NET Framework 4, il sistema di compilazione ASP.NET presuppone che il componente .NET Framework 4 come destinazione se il `Web.config` file non include le *targetFramework* attributo oppure se il `Web.config` manca il file. (Si potrebbe essere necessario apportare modifiche di scrittura di codice per l'applicazione per eseguirla in .NET Framework 4.)
- Se si include il *targetFramework* attributo e se il *System. CodeDom* elemento è definito nel `Web.config` file, questo file deve contenere le voci corrette per .NET Framework 4.
- Se si usa la *aspnet\_compilatore* comando per precompilare l'applicazione (ad esempio un ambiente di compilazione), è necessario usare la versione corretta del *aspnet\_compilatore* comando per il framework di destinazione. Usare il compilatore fornito con .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) per la compilazione per .NET Framework 3.5 e versioni precedenti. Usare il compilatore fornito con .NET Framework 4 per compilare le applicazioni create tramite tale framework o versioni successive.
- In fase di esecuzione, il compilatore Usa gli assembly di framework più recenti installati nel computer (e pertanto nella Global Assembly Cache). Se un aggiornamento viene eseguito in un secondo momento il framework (ad esempio, un'ipotetica versione 4.1 è installata), sarà possibile utilizzare le funzionalità della versione più recente del framework, anche se il *targetFramework* attributo ha come destinazione una versione precedente (ad esempio 4.0). (Tuttavia, in fase di progettazione in Visual Studio 2010 o quando si usa la *aspnet\_compilatore* comando, usando le funzionalità più recenti di framework causerà errori del compilatore).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery è incluso con Web Form e MVC

I modelli di Visual Studio per i controlli Web Form e MVC includono la libreria jQuery open source. Quando si crea un nuovo sito Web o un progetto, viene creata una cartella di script che contiene i 3 file seguenti:

- jQuery-1.4.1. js di leggibile dall'utente, minimizzata versione della libreria jQuery.
- jQuery-14.1.min.js: la versione della libreria jQuery minimizzata.
- jQuery-1.4.1-vsdoc. js.-il file di documentazione di Intellisense per la libreria jQuery.

Includere la versione di jQuery non minimizzata durante lo sviluppo di un'applicazione. Includere la versione minimizzata di jQuery per applicazioni di produzione.

Ad esempio, la pagina Web Form seguente viene illustrato come è possibile usare jQuery per modificare il colore di sfondo dei controlli TextBox ASP.NET su giallo quando hanno lo stato attivo.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Supporto della rete CDN

Il Content Delivery Network (rete CDN Microsoft Ajax) consente di aggiungere facilmente ASP.NET Ajax e jQuery script alle applicazioni Web. Ad esempio, è possibile iniziare a usare la libreria jQuery semplicemente aggiungendo un `<script>` tag a una pagina che punta a Ajax.microsoft.com simile al seguente:

[!code-html[Main](overview/samples/sample19.html)]

Grazie all'uso della rete CDN Microsoft Ajax, è possibile migliorare significativamente le prestazioni delle applicazioni Ajax. Il contenuto della rete CDN Microsoft Ajax viene memorizzati nella cache nei server di tutto il mondo. Rete CDN Microsoft Ajax consente inoltre ai browser di riutilizzare i file JavaScript memorizzati nella cache per i siti Web che si trovano in domini diversi.

Microsoft Ajax Content Delivery Network supporta SSL (HTTPS) nel caso in cui è necessario presentare una pagina web utilizzando Secure Sockets Layer.

Implementare un fallback quando non è disponibile la rete CDN. Testare il fallback.

Per altre informazioni sulla rete CDN Microsoft Ajax, visitare il sito Web seguente:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ScriptManager ASP.NET supporta la rete CDN Microsoft Ajax. È sufficiente per l'impostazione di una proprietà, la proprietà EnableCdn, è possibile recuperare tutti i file ASP.NET framework JavaScript dalla rete CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Dopo aver impostato la proprietà EnableCdn per il valore true, il framework ASP.NET recupererà tutti i file ASP.NET framework JavaScript dalla rete CDN tra tutti i file JavaScript usati per la convalida e UpdatePanel. Impostazione di questa uno proprietà può avere un impatto significativo sulle prestazioni dell'applicazione web.

È possibile impostare il percorso della rete CDN per i file JavaScript usando l'attributo WebResource. La nuova proprietà CdnPath specifica il percorso della rete CDN utilizzata quando si imposta la proprietà EnableCdn su true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Script esplicito di ScriptManager

In passato, se è stato usato il ScriptManger ASP.NET quindi erano necessari per caricare l'intera libreria Ajax di ASP.NET monolitica. Grazie all'uso della nuova proprietà ScriptManager.AjaxFrameworkMode, è possibile controllare esattamente quali componenti della libreria ASP.NET Ajax sono caricati e caricare solo i componenti di ASP.NET Ajax Library che è necessario.

La proprietà ScriptManager.AjaxFrameworkMode può essere impostata sui valori seguenti:

- Abilitato: Specifica che il controllo ScriptManager include automaticamente il file di script MicrosoftAjax. js, che è un file di script combinato di ogni script del framework principale (comportamento legacy).
- Disabilitata: Specifica che tutte le funzionalità di script Microsoft Ajax sono disabilitate e che il controllo ScriptManager non fa riferimento a tutti gli script automaticamente.
- Esplicita: Specifica che verranno esplicitamente inclusi riferimenti a script al file di script principale singoli framework richiesto dalla pagina e che si includerà i riferimenti alle dipendenze richiesti da ogni file di script.

Ad esempio, se si imposta la proprietà AjaxFrameworkMode sul valore Explicit è possibile specificare gli script particolari componente ASP.NET Ajax è necessario:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Form

Web Form è stata una funzionalità fondamentale in ASP.NET a partire da ASP.NET 1.0. Molti miglioramenti sono stati in quest'area per ASP.NET 4, incluse le seguenti:

- Possibilità di impostare *meta* tag.
- Maggiore controllo sulla stato di visualizzazione.
- Modi più semplici per lavorare con le funzionalità del browser.
- Supporto per il routing con Web Form ASP.NET.
- Maggiore controllo sui ID generato.
- La possibilità di rendere persistenti le righe selezionate nei controlli dati.
- Maggiore controllo sul codice HTML sottoposto a rendering nel *FormView* e *ListView* controlli.
- Supporto per il filtro per i controlli origine dati.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Impostazione metatag con le proprietà di Page.MetaDescription e Page.MetaKeywords

ASP.NET 4 aggiunge due proprietà per il *pagina* (classe), *MetaKeywords* e *MetaDescription*. Queste due proprietà rappresentano corrispondente *meta* tag nella pagina, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Queste due proprietà funzionano allo stesso modo in cui la pagina *titolo* proprietà. Seguono queste regole:

1. Se sono presenti alcun *meta* tag nel *head* elemento che corrisponde ai nomi delle proprietà (vale a dire, assegnare un nome = "keywords" per *Page.MetaKeywords* e il nome = "description" per  *Page.MetaDescription*, che significa che non sono state impostate queste proprietà), il *meta* tag verranno aggiunto alla pagina quando ne viene eseguito il rendering.
2. Se esistono già *meta* tag con questi nomi, queste proprietà agiscono da ottenere e impostare i metodi per il contenuto dei tag esistenti.

È possibile impostare queste proprietà in fase di esecuzione, che consente di ottenere il contenuto da un database o un'altra origine, e che consente di impostare i tag in modo dinamico di descrivere ciò che è per una pagina particolare.

È anche possibile impostare il *parole chiave* e *descrizione* le proprietà nel *@ Page* direttiva all'inizio del markup della pagina Web Form, come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Sostituirà il *meta* tag già dichiarato nella pagina contenuto (se presente).

Il contenuto della descrizione *meta* tag vengono usati per migliorare la ricerca elenca le anteprime di Google. (Per informazioni dettagliate, vedere [migliorare frammenti di codice con un makeover descrizione meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) sul blog di Google Webmaster centrale.) Google e Windows Live Search non usano il contenuto delle parole chiave per qualsiasi elemento, ma potrebbe essere altri motori di ricerca. Per altre informazioni, vedere [consigli di parole chiave Meta](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) sul sito Web di Guida del motore di ricerca.

Queste nuove proprietà sono una funzionalità semplice, ma quanto ridurre dal requisito di aggiungerle manualmente o da codice personalizzato per creare il *meta* tag.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>L'abilitazione dello stato di visualizzazione per i singoli controlli

Per impostazione predefinita, lo stato di visualizzazione è abilitato per la pagina, in modo che ogni controllo nella pagina potenzialmente archivia lo stato di visualizzazione anche se non è necessaria per l'applicazione. Dati sullo stato di visualizzazione sono incluso nel markup che una pagina che genera l'errore e aumenta la quantità di tempo che necessario per inviare una pagina al client e registrarlo nuovamente. L'archiviazione dello stato di visualizzazione più del necessario può causare un peggioramento delle prestazioni. Nelle versioni precedenti di ASP.NET, gli sviluppatori è stato possibile disabilitare lo stato di visualizzazione per i singoli controlli per ridurre le dimensioni della pagina, ma era necessario eseguire in modo esplicito per i singoli controlli. In ASP.NET 4, i controlli server Web includono una *ViewStateMode* proprietà che consente di disabilitare lo stato di visualizzazione per impostazione predefinita e quindi abilitarlo solo per i controlli che lo richiedono nella pagina.

Il *ViewStateMode* proprietà accetta un'enumerazione che dispone di tre valori: *Enabled*, *disabilitato*, e *eredita*. *Abilitata* consente di visualizzare lo stato per il controllo e per tutti i controlli figlio che sono impostati su *eredita* o che have impostato niente. *Disabled* Disabilita visualizzazione stato, e *eredita* specifica che il controllo Usa il *ViewStateMode* impostazione dal controllo padre.

L'esempio seguente illustra come la *ViewStateMode* proprietà funziona. Il markup e codice per i controlli nella pagina seguente include i valori per il *ViewStateMode* proprietà:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Come può notare, il codice disabilita lo stato di visualizzazione per il controllo PlaceHolder1. Il controllo di label1 figlio eredita il valore della proprietà (*Inherit* è il valore predefinito per *ViewStateMode* per i controlli.) e quindi Salva nessuno stato di visualizzazione. Nel controllo PlaceHolder2 *ViewStateMode* è impostata su *abilitato*, pertanto questa proprietà viene ereditata label2 e Salva lo stato di visualizzazione. Quando la pagina viene caricata, il *testo* proprietà di entrambi *etichetta* controlli viene impostato sulla stringa "[DynamicValue]".

L'effetto di queste impostazioni è che quando la pagina viene caricata la prima volta, l'output seguente viene visualizzata nel browser:

Disabilitato `: [DynamicValue]`

Abilitato:`[DynamicValue]`

Dopo un postback, tuttavia, viene visualizzato l'output seguente:

Disabilitato `: [DeclaredValue]`

Abilitato:`[DynamicValue]`

Il controllo label1 (il cui *ViewStateMode* è impostato su *disabilitato*) non ha mantenuto il valore su cui è stata impostata nel codice. Tuttavia, controllare il label2 (il cui *ViewStateMode* valore è impostato su *abilitato*) ha mantenuto lo stato.

È anche possibile impostare *ViewStateMode* nel *@ Page* direttiva, come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample26.aspx)]

Il *pagina* classe è semplicemente un altro controllo, agisce come il controllo padre per tutti gli altri controlli nella pagina. Il valore predefinito *ViewStateMode* viene *Enabled* per le istanze di *pagina*. Poiché per impostazione predefinita i controlli *eredita*, controlli erediterà il *Enabled* valore della proprietà solo se si impostano *ViewStateMode* a livello di pagina o controllo.

Il valore della *ViewStateMode* proprietà determina se lo stato di visualizzazione viene mantenuto solo se il *EnableViewState* viene impostata su *true*. Se il *EnableViewState* è impostata su *false*, lo stato di visualizzazione non verrà mantenuto anche se *ViewStateMode* è impostata su *abilitato*.

Un utilizzo corretto per questa funzionalità è con *ContentPlaceHolder* controlli nelle pagine master, in cui è possibile impostare *ViewStateMode* al *disabilitato* per il servizio master di pagina e quindi abilitarlo per singolo *ContentPlaceHolder* controlli che a loro volta contengono controlli che richiedono lo stato di visualizzazione.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Modifiche alle funzionalità del Browser

ASP.NET determina le funzionalità del browser che un utente sta usando per esplorare il sito utilizzando una funzionalità denominata *funzionalità del browser*. Funzionalità del browser sono rappresentate dal *HttpBrowserCapabilities* oggetto (esposti dalle *Request* proprietà). Ad esempio, è possibile usare la *HttpBrowserCapabilities* oggetto per determinare se il tipo e la versione del browser corrente supporta una particolare versione di JavaScript. In alternativa, è possibile usare la *HttpBrowserCapabilities* oggetto per determinare se la richiesta ha avuto origine da un dispositivo mobile.

Il *HttpBrowserCapabilities* oggetto è determinato da un set di file di definizione del browser. Questi file contengono informazioni sulle funzionalità del browser specifico. In ASP.NET 4, questi file di definizione del browser sono stati aggiornati per contenere informazioni sui browser introdotte di recente e i dispositivi, ad esempio Google Chrome, ricerca in movimento BlackBerry Smartphone e Apple iPhone.

L'elenco seguente mostra il nuovo browser di file di definizione:

- *blackberry.browser*
- *chrome.browser*
- *Default.browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Uso di provider di funzionalità del Browser

In ASP.NET versione 3.5 Service Pack 1, è possibile definire le funzionalità di un browser con nei modi seguenti:

- A livello di computer, si creano o aggiornano una `.browser` file XML nella cartella seguente:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Dopo aver definito la funzionalità del browser, eseguire il comando seguente da Visual Studio Prompt dei comandi per ricompilare l'assembly di funzionalità del browser e aggiungerlo alla Global Assembly Cache:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Per una singola applicazione, si crea una `.browser` file dell'applicazione `App_Browsers` cartella.

Questi approcci è necessario modificare i file XML e per le modifiche a livello di computer, è necessario riavviare l'applicazione dopo aver eseguito aspnet\_regbrowsers.exe processo.

ASP.NET 4 include una funzionalità detta *provider di funzionalità browser*. Come suggerisce il nome, questo consente di creare un provider che a sua volta ti permette di usano il proprio codice per determinare le funzionalità del browser.

In pratica, gli sviluppatori spesso non definiscono le funzionalità del browser personalizzato. Browser file sono difficile da aggiornare, il processo per l'aggiornamento li è piuttosto complicata e la sintassi XML per `.browser` file possono essere complessi da usare e definire. Ciò che potrebbe rendere molto più semplice questo processo è se esistesse una sintassi di definizione browser comune o un database che contiene le definizioni aggiornate del browser, o anche un servizio Web per un database di questo tipo. La nuova funzionalità di provider di funzionalità browser rende questi scenari possibili e per gli sviluppatori di terze parti.

Esistono due approcci principali per l'uso la nuova funzionalità di provider di funzionalità browser ASP.NET 4: estendere le funzionalità del browser ASP.NET le funzionalità di definizione o sostituirne totalmente lo. Prima di tutto, le sezioni seguenti descrivono come sostituire le funzionalità e quindi descritto come estenderla.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Sostituzione della funzionalità di funzionalità del Browser di ASP.NET

Per sostituire completamente la funzionalità di definizione funzionalità ASP.NET browser, seguire questa procedura:

1. Creare una classe di provider che deriva da *HttpCapabilitiesProvider* e che esegue l'override di *GetBrowserCapabilities* (metodo), come nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Il codice in questo esempio crea un nuovo *HttpBrowserCapabilities* oggetto, che specifica solo la funzionalità denominata browser e l'impostazione di questa funzionalità per MyCustomBrowser.
2. Registrare il provider con l'applicazione. 

    Per usare un provider con un'applicazione, è necessario aggiungere il *provider* attributo il *browserCaps* sezione la `Web.config` o `Machine.config` file. (È anche possibile definire gli attributi di provider in un *posizione* (elemento) per le directory specifiche nell'applicazione, ad esempio in una cartella per un dispositivo mobile specifico.) Nell'esempio seguente viene illustrato come impostare il *provider* attributo in un file di configurazione:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Un altro modo per registrare la nuova definizione di funzionalità del browser è usare il codice, come illustrato nell'esempio seguente:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Questo codice deve essere eseguito *Application\_avviare* evento del `Global.asax` file. Qualsiasi modifica per il *BrowserCapabilitiesProvider* classe deve verificarsi prima dell'esecuzione di qualsiasi codice dell'applicazione, per assicurarsi che la cache rimane in uno stato valido per l'oggetto risolto *HttpCapabilitiesBase* oggetti.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>La memorizzazione nella cache l'oggetto HttpBrowserCapabilities

Nell'esempio precedente è presente un problema, che indica che il codice viene eseguito ogni volta che il provider personalizzato viene richiamato per ottenere il *HttpBrowserCapabilities* oggetto. Ciò può verificarsi più volte durante ogni richiesta. Nell'esempio, il codice per il provider non esegue gran parte. Tuttavia, se il codice nel provider personalizzato viene eseguito un lavoro notevole in ordine per ottenere il *HttpBrowserCapabilities* dell'oggetto, questo può influire sulle prestazioni. Per evitare che ciò accada, è possibile memorizzare nella cache le *HttpBrowserCapabilities* oggetto. Attenersi ai passaggi riportati di seguito.

1. Creare una classe che deriva da *HttpCapabilitiesProvider*, come quello nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Nell'esempio, il codice genera una chiave di cache chiamando un metodo BuildCacheKey personalizzato e ottiene la lunghezza di tempo da memorizzare nella cache chiamando un metodo GetCacheTime personalizzato. Il codice aggiunge quindi l'oggetto risolto *HttpBrowserCapabilities* oggetto alla cache. L'oggetto può essere recuperato dalla cache e riutilizzato a richieste successive che utilizzano il provider personalizzato.
2. Registrare il provider con l'applicazione come descritto nella procedura precedente.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Estendere le funzionalità di funzionalità del Browser di ASP.NET

La sezione precedente descrive come creare una nuova *HttpBrowserCapabilities* oggetto in ASP.NET 4. È anche possibile estendere la funzionalità di funzionalità del browser ASP.NET mediante l'aggiunta di nuove definizioni di funzionalità del browser a quelle già presenti in ASP.NET. È possibile farlo senza l'utilizzo di definizioni del browser XML. La procedura seguente viene illustrato come.

1. Creare una classe che deriva da *HttpCapabilitiesEvaluator* e che esegue l'override di *GetBrowserCapabilities* metodo, come illustrato nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Prima di tutto, questo codice Usa la funzionalità di funzionalità del browser ASP.NET per tentare di identificare il browser. Tuttavia, se non viene individuato alcun browser sulla base delle informazioni definite nella richiesta (vale a dire, se il *Browser* proprietà delle *HttpBrowserCapabilities* oggetto è la stringa "Unknown"), il codice chiama il provider personalizzato (MyBrowserCapabilitiesEvaluator) per identificare il browser.
2. Registrare il provider con l'applicazione come descritto nell'esempio precedente.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Estendere le funzionalità di funzionalità del Browser mediante l'aggiunta di nuove funzionalità per le definizioni di funzionalità esistenti

Oltre alla creazione di un provider di definizione browser personalizzato e per la creazione dinamica di nuove definizioni del browser, è possibile estendere le definizioni esistenti del browser con funzionalità aggiuntive. Ciò consente di usare una definizione è vicino al risultato desiderato, ma non include solo alcune funzionalità. A tale scopo, attenersi alla procedura seguente.

1. Creare una classe che deriva da *HttpCapabilitiesEvaluator* e che esegue l'override di *GetBrowserCapabilities* metodo, come illustrato nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Il codice di esempio estende l'ASP.NET esistenti *HttpCapabilitiesEvaluator* classe e ottiene le *HttpBrowserCapabilities* oggetto che corrisponde alla definizione della richiesta corrente con il codice seguente :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Il codice può quindi aggiungere o modificare una funzione per questo browser. Esistono due modi per specificare una nuova funzionalità del browser:

    - Aggiungere una coppia chiave/valore per il *IDictionary* oggetto esposto dalle *funzionalità* proprietà del *HttpCapabilitiesBase* oggetto. Nell'esempio precedente, il codice aggiunge una funzionalità denominata MultiTouch con un valore *true*.
    - Impostare le proprietà esistenti del *HttpCapabilitiesBase* oggetto. Nell'esempio precedente, il codice imposta la *frame* proprietà *true*. Questa proprietà è semplicemente una funzione di accesso per il *IDictionary* esposto dall'oggetto il *funzionalità* proprietà. 

        > [!NOTE]
        > Tenere presente questo modello si applica a qualsiasi proprietà del *HttpBrowserCapabilities*, tra cui gli adattatori di controllo.
2. Registrare il provider con l'applicazione come descritto nella procedura precedente.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routing in ASP.NET 4

ASP.NET 4 aggiunge il supporto incorporato per l'uso di routing con Web Form. Routing consente di che configurare un'applicazione di accettare richiesta URL non sono mappati a file fisici. In alternativa, è possibile utilizzare il routing per definire gli URL che sono significativi per gli utenti e che può aiutare con l'ottimizzazione motore di ricerca (SEO) per l'applicazione. Ad esempio, l'URL per una pagina che visualizza le categorie di prodotti in un'applicazione esistente potrebbe essere simile al seguente:

[!code-console[Main](overview/samples/sample36.cmd)]

Tramite il routing, è possibile configurare l'applicazione di accettare l'URL seguente per eseguire il rendering le stesse informazioni:

[!code-console[Main](overview/samples/sample37.cmd)]

Il routing è stata disponibile a partire da ASP.NET 3.5 SP1. (Per un esempio di come usare il routing in ASP.NET 3.5 SP1, vedere l'intervento [tramite Routing con Web Form](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "titolo di questa voce.") nel blog di Phil Haack.) Tuttavia, ASP.NET 4 include alcune funzionalità che rendono più semplice usare il routing, inclusi i seguenti:

- Il *PageRouteHandler* (classe), che è un semplice gestore HTTP che usa quando si definiscono le route. La classe passa i dati alla pagina che viene inoltrata la richiesta.
- Le nuove proprietà *HttpRequest.RequestContext* e *Page.RouteData* (ovvero un proxy per il *HttpRequest.RequestContext.RouteData* oggetto). Queste proprietà rendono più semplice per accedere alle informazioni che viene passato dalla route.
- I seguenti nuovi generatori di espressioni, definite in *System.Web.Compilation.RouteUrlExpressionBuilder* e *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, che fornisce un modo semplice per creare un URL che corrisponde a un URL di route all'interno di un controllo server ASP.NET.
- *RouteValue*, che fornisce un modo semplice per estrarre informazioni dal *RouteContext* oggetto.
- Il *RouteParameter* classe, che rende più semplice passare i dati contenuti in un *RouteContext* oggetto a una query per un controllo origine dati (simile a [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Il routing per pagine Web Form

Nell'esempio seguente viene illustrato come definire una route di Web Form con la nuova *MapPageRoute* metodo per il *Route* classe:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 è stato introdotto il *MapPageRoute* (metodo). Nell'esempio seguente è equivalente alla definizione SearchRoute illustrata nell'esempio precedente, ma usa il *PageRouteHandler* classe.

[!code-csharp[Main](overview/samples/sample39.cs)]

Il codice nell'esempio viene eseguito il mapping della route a una pagina fisica (nella route prima, a `~/search.aspx`). La prima definizione di route specifica anche che il parametro denominato searchterm da estrarre dall'URL e passato alla pagina.

Il *MapPageRoute* metodo supporta l'overload del metodo seguente:

- *MapPageRoute (routeName stringa, stringa routeUrl, physicalFile string, bool checkPhysicalUrlAccess)*
- *MapPageRoute (routeName stringa, stringa routeUrl, physicalFile stringa, bool checkPhysicalUrlAccess, RouteValueDictionary valori predefiniti)*
- *MapPageRoute (routeName stringa, stringa routeUrl, physicalFile stringa, bool checkPhysicalUrlAccess, RouteValueDictionary le impostazioni predefinite, i vincoli RouteValueDictionary)*

Il *checkPhysicalUrlAccess* parametro specifica se la route deve verificare le autorizzazioni di sicurezza per la pagina fisica viene eseguito il routing (in questo caso, Search. aspx) e le autorizzazioni per l'URL in ingresso (in questo caso, eseguire la ricerca / {searchterm}). Se il valore di *checkPhysicalUrlAccess* viene *false*, disporrà solo delle autorizzazioni dell'URL in ingresso. Queste autorizzazioni sono definite nel `Web.config` file utilizzando le impostazioni come il seguente:

[!code-xml[Main](overview/samples/sample40.xml)]

Nella configurazione di esempio, accedere alla pagina fisica `search.aspx` per tutti gli utenti, tranne quelli che fanno parte del ruolo di amministratore. Quando la *checkPhysicalUrlAccess* parametro è impostato su *true* (ovvero il valore predefinito), solo gli utenti amministratori possono accedere /search/ l'URL {searchterm}, perché è la pagina fisica Search. aspx limitato agli utenti di tale ruolo. Se *checkPhysicalUrlAccess* è impostata su *false* e il sito è configurato come illustrato nell'esempio precedente, tutti gli utenti autenticati possono accedere /search/ l'URL {searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Leggere le informazioni sul Routing in una pagina Web Form

Nel codice della pagina fisica Web Form, è possibile accedere alle informazioni di routing ha estratto dall'URL (o altre informazioni che ha aggiunto un altro oggetto per il *RouteData* oggetto) utilizzando due nuove proprietà:  *HttpRequest.RequestContext* e *Page.RouteData*. (*Page.RouteData* esegue il wrapping *HttpRequest.RequestContext.RouteData*.) Nell'esempio seguente viene illustrato come utilizzare *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Il codice estrae il valore passato per il parametro searchterm, come definito nella route riportato in precedenza. Prendere in considerazione l'URL della richiesta seguente:

[!code-console[Main](overview/samples/sample42.cmd)]

Quando viene effettuata questa richiesta, la parola "scott" sarebbero mostrati nel `search.aspx` pagina.

#### <a name="accessing-routing-information-in-markup"></a>L'accesso a informazioni di Routing nel Markup

Il metodo descritto nella sezione precedente viene illustrato come ottenere i dati della route nel codice in una pagina Web Form. È anche possibile usare le espressioni nel markup che consentono di accedere alle stesse informazioni. Generatori di espressioni rappresentano un modo elegante e potente per lavorare con il codice dichiarativo. (Per altre informazioni, vedere l'intervento [Express autonomamente con Custom generatori di espressioni](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) sul blog di Phil Haack.)

ASP.NET 4 sono disponibili due nuovi generatori di espressioni per il routing dei Web Form. Nell'esempio seguente viene illustrato come usarle.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Nell'esempio, il *RouteUrl* espressione viene usata per definire un URL basato su un parametro di route. Questo evita di dover come hardcoded l'URL completo nel markup e consente di modificare la struttura di URL in un secondo momento senza richiedere qualsiasi modifica apportata a questo collegamento.

In base alla route definita in precedenza, questo codice genera l'URL seguente:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET funziona automaticamente la route corretta (vale a dire genera l'URL corretto) in base ai parametri di input. È anche possibile includere un nome di route nell'espressione, che consente di specificare una route da usare.

Nell'esempio seguente viene illustrato come utilizzare il *RouteValue* espressione.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Quando la pagina che contiene questo controllo viene eseguito, il valore "scott" viene visualizzato nell'etichetta.

Il *RouteValue* espressione rende più semplice usare i dati della route nel markup, ed evita la necessità di lavorare con le più complesse Page.RouteData["x"] sintassi nel markup.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Utilizzando i dati di Route per i parametri di controllo origine dati

Il *RouteParameter* classe consente di specificare i dati della route come valore di parametro per le query in un controllo origine dati. Si [opera analogamente il](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) classe, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample46.aspx)]

In questo caso, verrà utilizzato il valore di searchterm di parametro di route per il @companyname parametro il <em>selezionare</em> istruzione.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Impostazione degli ID Client

Il nuovo *ClientIDMode* proprietà corregge un problema di lunga durato in ASP.NET, vale a dire come controlli creano il *id* attributo per gli elementi che eseguono il rendering. Conoscendo i *id* attributo per gli elementi di rendering è importante se l'applicazione include uno script client che fa riferimento a questi elementi.

Il *id* attributo in formato HTML che viene eseguito il rendering dei controlli server Web viene generato in base il *ClientID* proprietà del controllo. Fino a 4 di ASP.NET, l'algoritmo per la generazione di *id* dell'attributo dalle *ClientID* proprietà è stata per concatenare il contenitore di denominazione (se presente) con l'ID e nel caso di controlli ripetuti (come in controlli dati), per aggiungere un prefisso e un numero sequenziale. Mentre questo è sempre garantito che gli ID dei controlli nella pagina siano univoci, l'algoritmo ha comportato ID che non erano prevedibile e pertanto non è semplice riferimento nello script client.

Il nuovo *ClientIDMode* proprietà consente di specificare in modo più preciso come ID client viene generato per i controlli. È possibile impostare il *ClientIDMode* proprietà per un controllo, inclusa per la pagina. Impostazioni possibili sono i seguenti:

- *AutoID* – ciò equivale all'algoritmo per la generazione *ClientID* i valori delle proprietà che è stata usata nelle versioni precedenti di ASP.NET.
- *Statica* : Specifica che il *ClientID* valore sarà lo stesso ID senza concatenando l'ID dell'elemento padre di denominazione dei contenitori. Ciò può essere utile nei controlli utente Web. Poiché un controllo utente Web può trovarsi in diverse pagine e controlli contenitore diverso, può essere difficile scrivere lo script client per i controlli che utilizzano le *AutoID* algoritmo perché non è possibile prevedere quali saranno i valori di ID .
- *Stimabile* : questa opzione è principalmente per l'utilizzo nei controlli dati che utilizzano modelli ripetuti. Concatena le proprietà ID dei contenitori di denominazione del controllo, ma a generati *ClientID* valori non contengono stringhe come "ctlxxx". Questa impostazione funziona in combinazione con il *ClientIDRowSuffix* proprietà del controllo. Impostare il *ClientIDRowSuffix* proprietà per il nome di un campo dati e il valore del campo utilizzato come suffisso per generato *ClientID* valore. In genere si utilizza la chiave primaria di un record di dati come le *ClientIDRowSuffix* valore.
- *Ereditare* : questa impostazione è il comportamento predefinito per i controlli, specifica che la generazione degli ID di un controllo è lo stesso come relativo elemento padre.

È possibile impostare il *ClientIDMode* proprietà a livello di pagina. Questa impostazione definisce il valore predefinito *ClientIDMode* valore per tutti i controlli nella pagina corrente.

Il valore predefinito *ClientIDMode* valore a livello di pagina viene *AutoID*e il valore predefinito *ClientIDMode* valore a livello di controllo è *eredita*. Di conseguenza, se non si imposta questa proprietà in qualsiasi punto nel codice, tutti i controlli per impostazione predefinita il *AutoID* algoritmo.

Si imposta il valore a livello di pagina *@ Page* direttiva, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample47.aspx)]

È anche possibile impostare il *ClientIDMode* valore nel file di configurazione, a livello di computer (computer) o a livello di applicazione. Questa impostazione definisce il valore predefinito *ClientIDMode* impostazione per tutti i controlli in tutte le pagine nell'applicazione. Se si imposta il valore a livello di computer, definisce il valore predefinito *ClientIDMode* impostazione per tutti i siti Web in tale computer. L'esempio seguente mostra le *ClientIDMode* impostazione nel file di configurazione:

[!code-xml[Main](overview/samples/sample48.xml)]

Come indicato in precedenza, il valore della *ClientID* proprietà viene derivata dal contenitore di denominazione per padre un controllo. In alcuni scenari, ad esempio quando si utilizza le pagine master, i controlli finisce con l'ID, ad esempio quelli di seguito viene eseguito il rendering HTML:

[!code-html[Main](overview/samples/sample49.html)]

Anche se il *input* elemento mostrato nel markup (da un *nella casella di testo* controllo) è solo due contenitori di denominazione profonda nella pagina (annidata *ContentPlaceholder* controlli), a causa della modalità di elaborazione delle pagine master, il risultato finale è un ID di controllo come segue:

[!code-console[Main](overview/samples/sample50.cmd)]

Questo ID è garantito l'univocità nella pagina, ma è inutilmente tempo per la maggior parte degli scopi. Si supponga che si desidera ridurre la lunghezza dell'ID sottoposto a rendering e per avere maggiore controllo sul modo in cui viene generato l'ID. (Ad esempio, si desidera eliminare i prefissi "ctlxxx".) Il modo più semplice per ottenere questo risultato consiste nell'impostare il *ClientIDMode* proprietà come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample51.aspx)]

In questo esempio, il *ClientIDMode* è impostata su *statici* per più esterna *NamingPanel* elemento e impostato su *stimabile* per interna *NamingControl* elemento. Queste impostazioni hanno come risultato nel markup seguente (il resto della pagina e pagina master si presuppone che sia lo stesso come nell'esempio precedente):

[!code-html[Main](overview/samples/sample52.html)]

Il *statici* impostazione ha l'effetto di reimpostare la gerarchia di denominazione per tutti i controlli all'interno di più esterna *NamingPanel* elemento e di eliminare il *ContentPlaceHolder* e *MasterPage* gli ID dall'ID generato. (Il *nome* attributo degli elementi viene eseguito il rendering non è interessato, in modo che la normale funzionalità ASP.NET viene mantenuta per gli eventi, lo stato di visualizzazione e così via.) È un effetto collaterale di reimpostare la gerarchia di denominazione che anche se si sposta il markup per il *NamingPanel* elementi in un altro *ContentPlaceholder* (controllo), l'ID client viene eseguito il rendering rimangono invariati.

> [!NOTE]
> Si noti che è responsabilità dell'utente per assicurarsi che gli ID di controllo sottoposto a rendering siano univoci. Se non lo sono le funzionalità che richiede gli ID univoci per i singoli elementi HTML, ad esempio, il client può causare interruzioni *Document. getElementById* (funzione).


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Creazione di ID Client prevedibili nei controlli con associazione a dati

Il *ClientID* valori generati per i controlli in un controllo elenco associato ai dati dall'algoritmo legacy possono essere lunghe e non sono molto prevedibili. Il *ClientIDMode* funzionalità consente di avere un maggior controllo sul modo in cui questi ID vengono generati.

Il markup nell'esempio seguente include un' *ListView* controllo:

[!code-aspx[Main](overview/samples/sample53.aspx)]

Nell'esempio precedente, il *ClientIDMode* e *RowClientIDRowSuffix* proprietà vengono impostate nel markup. Il *ClientIDRowSuffix* proprietà può essere utilizzata solo nei controlli con associazione a dati e il relativo comportamento è diverso a seconda di quale controllo in uso. Esistono le seguenti differenze:

- *GridView* controllo, è possibile specificare il nome di una o più colonne nell'origine dati, che vengono combinati in fase di esecuzione per creare il client ID. Ad esempio, se si imposta *RowClientIDRowSuffix* per "ProductName, ProductId", ID di controllo per elementi visualizzabili avrà un formato simile al seguente:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* controllo, è possibile specificare una singola colonna nell'origine dati che viene aggiunto all'ID client. Ad esempio, se si imposta *ClientIDRowSuffix* per "ProductName", gli ID di controllo viene eseguito il rendering avrà un formato simile al seguente:

- [!code-console[Main](overview/samples/sample55.cmd)]

- In questo caso il 1 finale è derivato dall'ID del prodotto dell'elemento di dati corrente.

- *Repeater* controllo, questo controllo non supporta la *ClientIDRowSuffix* proprietà. In un *Repeater* controllo, l'indice della riga corrente viene utilizzato. Quando si usa ClientIDMode = "Stimabile" con un *Repeater* controllare, client ID vengono generati che hanno il formato seguente:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Il valore 0 finali è l'indice della riga corrente.

Il *FormView* e *DetailsView* controlli non visualizzano più righe, in modo che non supportano il *ClientIDRowSuffix* proprietà.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Selezione di righe persistente nei controlli dati

Il *GridView* e *ListView* controlli possono consentire agli utenti di selezionare una riga. Nelle versioni precedenti di ASP.NET, selezione basata su indice di riga nella pagina. Ad esempio, se si seleziona il terzo elemento nella pagina 1 e quindi passare alla pagina 2, il terzo elemento in tale pagina è selezionato.

Selezione persistente è stato inizialmente supportata solo nei progetti di dati dinamici in .NET Framework 3.5 SP1. Quando questa funzionalità è abilitata, l'elemento attualmente selezionato è basato sulla chiave dei dati per l'elemento. Ciò significa che se si seleziona la terza riga nella pagina 1 e passare alla pagina 2, viene selezionato nulla nella pagina 2. Quando si torna alla pagina 1, la terza riga sia ancora selezionata. Selezione persistente è ora supportata per il *GridView* e *ListView* controlli in tutti i progetti usando il *EnablePersistedSelection* proprietà, come illustrato di esempio seguente:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Controllo ASP.NET Chart

In ASP.NET *grafico* controllo si espande le offerte di visualizzazione dei dati in .NET Framework. Usando il *grafico* (controllo), è possibile creare pagine ASP.NET che sono grafici intuitivi e visivamente accattivanti per complessa analisi statistiche e finanziarie. In ASP.NET *grafico* controllo è stato introdotto come componente aggiuntivo per la versione di .NET Framework versione 3.5 SP1 ed è parte della versione di .NET Framework 4.

Il controllo include le funzionalità seguenti:

- 35 tipi diversi di grafici.
- Un numero illimitato di aree del grafico, titoli, legende e annotazioni.
- Un'ampia gamma di impostazioni dell'aspetto di tutti gli elementi del grafico.
- Supporto 3D per la maggior parte dei tipi di grafico.
- Etichette dati intelligente che possono adattarsi automaticamente intorno ai punti dati.
- Le strisce, cambi di scala e la scala logaritmica.
- Oltre 50 formule statistiche e finanziarie per analisi e trasformazione dei dati.
- Associazione semplice e la manipolazione di dati del grafico.
- Supporto per formati di dati comuni, ad esempio date, ore e valuta.
- Supporto per l'interattività e personalizzazione basate su eventi, che include client fare clic su eventi tramite Ajax.
- Gestione dello stato.
- Flusso binario.

Le figure seguenti mostrano esempi di finanziari grafici generati dal controllo ASP.NET Chart.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Nella figura 2: Esempi di controlli ASP.NET grafico

Per altri esempi di come usare il controllo ASP.NET Chart, scaricare il codice di esempio nel [ambiente di esempio per Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) pagina sul sito Web MSDN. È possibile trovare altri esempi della community il sito Web di [forum sui controlli Chart](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Aggiunta del controllo grafico a una pagina ASP.NET

Nell'esempio seguente viene illustrato come aggiungere un *grafico* controllo a una pagina ASP.NET utilizzando il markup. Nell'esempio, il *grafico* controllo produce un istogramma a colonne per i punti dati statici.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Tramite i grafici 3D

Il *grafico* controllo contiene un *ChartAreas* raccolta, che può contenere *ChartArea* gli oggetti che definiscono le caratteristiche di aree del grafico. Per usare 3D per un'area grafico, ad esempio, è consigliabile usare la *Area3DStyle* proprietà come illustrato di seguito:

[!code-aspx[Main](overview/samples/sample59.aspx)]

La figura seguente mostra un grafico 3D con quattro serie del *barra* tipo di grafico.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figura 3: grafico a barre 3D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Utilizzo di cambi di scala e scale logaritmiche

Cambi di scala e scale logaritmiche sono altri due modi per aggiungere complessità al grafico. Queste funzionalità sono specifiche per ogni asse in un'area grafico. Per usare queste funzionalità sull'asse Y primario di un'area grafico, ad esempio, è consigliabile usare la *AxisY.IsLogarithmic* e *ScaleBreakStyle* le proprietà in un *ChartArea* oggetto. Il frammento di codice seguente viene illustrato come utilizzare i cambi di scala sull'asse Y primario.

[!code-aspx[Main](overview/samples/sample60.aspx)]

La figura seguente mostra l'asse Y con cambi di scala abilitati.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figura 4: Cambi di scala

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtro dei dati con il controllo QueryExtender

Un'attività molto comune per gli sviluppatori che creano pagine Web basate sui dati è di filtrare i dati. Ciò in genere è stata eseguita creando *in cui* clausole nei dati di origine i controlli. Questo approccio può essere complicato e in alcuni casi il *in cui* sintassi non è possibile sfruttare la funzionalità completa del database sottostante.

Per rendere le operazioni di filtro, una nuova *QueryExtender* controllo è stato aggiunto in ASP.NET 4. Questo controllo può essere aggiunto a *EntityDataSource* oppure *LinqDataSource* controlli per filtrare i dati restituiti da questi controlli. Poiché il *QueryExtender* controllo si basa su LINQ, il filtro viene applicato nel server di database prima che i dati vengono inviati alla pagina, con conseguente operazioni molto efficiente.

Il *QueryExtender* controllo supporta un'ampia gamma di opzioni di filtro. Le sezioni seguenti descrivono queste opzioni e vengono forniti esempi di come usarli.

#### <a name="search"></a>Cerca

Per l'opzione di ricerca, il *QueryExtender* controllo esegue una ricerca nei campi specificati. Nell'esempio seguente, il controllo Usa il testo immesso nel controllo TextBoxSearch e cerca il relativo contenuto nel `ProductName` e `Supplier.CompanyName` alle colonne dei dati restituiti dalle *LinqDataSource* controllo.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Intervallo

L'opzione intervallo è simile all'opzione di ricerca, ma specifica una coppia di valori per definire l'intervallo. Nell'esempio seguente, il *QueryExtender* controllano le ricerche di `UnitPrice` colonna nei dati restituiti dai *LinqDataSource* controllo. L'intervallo viene letto dai controlli TextBoxFrom e TextBoxTo nella pagina.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

L'opzione di espressione di proprietà consente di definire un confronto al valore della proprietà. Se l'espressione viene valutata *true*, vengono restituiti i dati che sono in corso l'analisi. Nell'esempio seguente, il *QueryExtender* controllo consente di filtrare i dati in base ai dati nel `Discontinued` colonna sul valore dal controllo CheckBoxDiscontinued nella pagina.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>Espressione personalizzata

Infine, è possibile specificare un'espressione personalizzata da usare con il *QueryExtender* controllo. Questa opzione consente di chiamare una funzione nella pagina che definisce la logica di filtro personalizzato. Nell'esempio seguente viene illustrato come specificare in modo dichiarativo un'espressione personalizzata nel *QueryExtender* controllo.

[!code-aspx[Main](overview/samples/sample64.aspx)]

L'esempio seguente mostra la funzione personalizzata che viene richiamata per la *QueryExtender* controllo. In questo caso, invece di usare una query di database che include un' *in cui* clausola, il codice Usa una query LINQ per filtrare i dati.

[!code-csharp[Main](overview/samples/sample65.cs)]

Questi esempi illustrano solo un'espressione usata nel *QueryExtender* controllo alla volta. Tuttavia, è possibile includere più espressioni all'interno di *QueryExtender* controllo.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Le espressioni di codice codificata in formato HTML

Alcuni siti ASP.NET (in particolare con ASP.NET MVC) si basano su usando `<%` =  `expression %>` sintassi (spesso chiamata "nugget di codice") per scrivere un testo nella risposta. Quando si usano le espressioni di codice, è facile dimenticare di codifica HTML input il testo, se il testo proviene dall'utente, può lasciare aperte le pagine di un attacco XSS (Cross-Site Scripting).

ASP.NET 4 introduce la nuova sintassi per espressioni di codice seguente:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Questa sintassi Usa la codifica HTML per impostazione predefinita durante la scrittura nella risposta. Questa nuova espressione viene convertita in modo efficace in quanto segue:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Ad esempio, &lt;%: richiesta ["UserInput"] %&gt; esegue la codifica HTML del valore di *richiesta ["UserInput"]*.

L'obiettivo di questa funzionalità è consentono di sostituire tutte le istanze della sintassi precedente con la nuova sintassi in modo che non è necessario decidere in ogni passaggio, quello da usare. Tuttavia, vi sono casi in cui il testo in fase di output deve essere in formato HTML o è già stato codificato, nel qual caso può portare a doppia codifica.

In questi casi, ASP.NET 4 introduce una nuova interfaccia *IHtmlString*, insieme a un'implementazione concreta *HtmlString*. Istanze di questi tipi consentono di indicare che il valore restituito è codificato già correttamente (o in caso contrario esaminare) per la visualizzazione in formato HTML e che pertanto il valore non deve essere codificata in formato HTML nuovamente. Ad esempio, quanto segue non deve essere (e non) codificato in formato HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Metodi di supporto di ASP.NET MVC 2 sono stati aggiornati per funzionare con questa nuova sintassi in modo che non siano double codificato, ma solo quando si esegue ASP.NET 4. Questa nuova sintassi non funziona quando si esegue un'applicazione tramite ASP.NET 3.5 SP1.

Tenere presente che questo non garantisce la protezione da attacchi XSS. HTML che utilizza i valori di attributo che non sono racchiusi tra virgolette, ad esempio, può contenere input dell'utente sono comunque soggetti. Si noti che l'output di controlli ASP.NET e gli helper di ASP.NET MVC include sempre i valori di attributo tra virgolette, che è l'approccio consigliato.

Questa sintassi in modo analogo, non esegue la codifica JavaScript, ad esempio quando si crea una stringa di JavaScript in base all'input utente.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Modifiche al modello progetto

Nelle versioni precedenti di ASP.NET, quando si usa Visual Studio per creare un nuovo progetto di sito Web o un progetto di applicazione Web, i progetti risultanti contengono solo una pagina default. aspx, valore predefinito `Web.config` file e il `App_Data` cartella, come illustrato di seguito Figura:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio supporta anche un tipo di progetto sito Web vuoto che non contiene alcun file affatto, come illustrato nella figura seguente:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Il risultato è che per i principianti, è opportuno molto documentarsi su come compilare un'applicazione Web di produzione. Pertanto, ASP.NET 4 introduce tre nuovi modelli, uno per un progetto di applicazione Web vuoto e una per un progetto di applicazione Web e sito Web.

#### <a name="empty-web-application-template"></a>Modello di applicazione Web vuota

Come suggerisce il nome, il modello di applicazione Web vuota è un progetto di applicazione Web ridotta. Si seleziona questo modello di progetto dalla finestra di dialogo Nuovo progetto di Visual Studio, come illustrato nella figura seguente:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](overview/_static/image8.png))

Quando si crea un'applicazione Web ASP.NET vuota, Visual Studio crea il layout cartella seguente:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

È simile al layout del sito Web vuoto da versioni precedenti di ASP.NET, con un'eccezione. In Visual Studio 2010, progetti applicazione Web vuota e il sito Web vuoto contengono quanto segue minimo `Web.config` file che contiene informazioni usate da Visual Studio per identificare il framework che è destinato il progetto:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Senza questa *targetFramework* proprietà, Visual Studio il valore predefinito è destinato a .NET Framework 2.0 per mantenere la compatibilità durante l'apertura delle applicazioni meno recenti.

#### <a name="web-application-and-web-site-project-templates"></a>Modelli di progetto sito Web e l'applicazione Web

Altri due nuovi modelli di progetto forniti con Visual Studio 2010 contengono modifiche rilevanti. La figura seguente illustra il layout di progetto che viene creato quando si crea un nuovo progetto di applicazione Web. (Il layout per un progetto sito Web è praticamente identico).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Il progetto include un numero di file che non sono stati creati nelle versioni precedenti. Inoltre, il nuovo progetto di applicazione Web è configurato con le funzionalità di appartenenza base, che consente di iniziare rapidamente la protezione accesso alla nuova applicazione. A causa di questa inclusione, il `Web.config` file per il nuovo progetto include le voci che consentono di configurare l'appartenenza, ruoli e profili. L'esempio seguente illustra il `Web.config` file per un nuovo progetto di applicazione Web. (In questo caso *roleManager* è disabilitata.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](overview/_static/image14.png))

Il progetto contiene anche una seconda `Web.config` del file nei `Account` directory. Il secondo file di configurazione fornisce un modo per proteggere l'accesso alla pagina ChangePassword per non-gli utenti connessi. L'esempio seguente mostra il contenuto del secondo `Web.config` file.

![](overview/_static/image15.png)

Le pagine create per impostazione predefinita nei nuovi modelli di progetto contengono anche più contenuti che nelle versioni precedenti. Il progetto contiene una pagina master predefinita e file CSS e la pagina predefinita (default. aspx) è configurata per utilizzare la pagina master per impostazione predefinita. Il risultato è che quando si esegue l'applicazione Web o un sito Web per la prima volta, l'impostazione predefinita (home) pagina è già funzionale. In effetti, è simile alla pagina predefinita che viene visualizzato quando si avvia una nuova applicazione MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](overview/_static/image18.png))

Lo scopo di queste modifiche ai modelli di progetto è fornire indicazioni su come iniziare a creare una nuova applicazione Web. Con markup di 1.0 conformi XHTML strict, semanticamente corrette e layout in cui viene specificato usando lo stile CSS, le pagine nei modelli rappresentano le procedure consigliate per la creazione di applicazioni Web ASP.NET 4. Le pagine predefinite hanno anche un layout a due colonne che è possibile personalizzare con facilità.

Si supponga, ad esempio, per una nuova applicazione Web si desidera modificare alcuni dei colori e inserire il logo della società al posto del logo dell'applicazione ASP.NET. A tale scopo, si crea una nuova directory in `Content` per archiviare l'immagine del logo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Per aggiungere l'immagine della pagina, quindi aprire il `Site.Master` file, trovare in cui è definito, il testo di My ASP.NET Application e sostituirla con un' *immagine* elemento la cui *src* attributo è impostato per il nuovo logo immagine, come nell'esempio seguente:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](overview/_static/image22.png))

È quindi possibile analizzare il file Site CSS e modificare le definizioni di classe CSS per modificare il colore di sfondo della pagina, nonché che dell'intestazione.

Il risultato di queste modifiche è che è possibile visualizzare una pagina iniziale personalizzata con il minimo sforzo:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Miglioramenti CSS

Una delle principali aree di lavoro in ASP.NET 4 è stata per il rendering HTML che è conforme agli standard HTML più recente. Ciò include modifiche al modo in cui i controlli server Web ASP.NET utilizzano gli stili CSS.

#### <a name="compatibility-setting-for-rendering"></a>Impostazione di compatibilità per il Rendering

Per impostazione predefinita, quando un'applicazione Web o un sito Web è destinata a .NET Framework 4, il *controlRenderingCompatibilityVersion* attributo il *pagine* elemento è impostato su "4.0". Questo elemento è definito nel livello di computer `Web.config` file e per impostazione predefinita viene applicata a tutte le applicazioni ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

Il valore per *controlRenderingCompatibility* è una stringa, che consente a potenziali nuove definizioni di versione nelle versioni future. Nella versione corrente, i valori seguenti sono supportati per questa proprietà:

- "3.5". Questa impostazione indica markup e rendering legacy. Markup sottoposto a rendering dai controlli è compatibile con le versioni precedenti al 100% e l'impostazione delle *xhtmlConformance* va alla proprietà.
- "4.0". Se la proprietà dispone di questa impostazione, i controlli server Web ASP.NET eseguire le operazioni seguenti:
- Il *xhtmlConformance* proprietà viene sempre considerata come "Strict". Di conseguenza, i controlli il rendering del markup XHTML 1.0 Strict.
- Disabilitare i controlli non di input non è più esegue il rendering degli stili non validi.
- *div* nascosti tra gli elementi a questo punto hanno ora uno stile in modo che non interferiscono con le regole CSS creati dall'utente.
- I controlli menu il rendering del markup che è semanticamente corretto e conforme alle linee guida di accessibilità.
- I controlli di convalida non vengono visualizzati gli stili in linea.
- Controlla che in precedenza viene eseguito il rendering del bordo = "0" (i controlli che derivano da ASP.NET *tabella* controllo e ASP.NET *immagine* controllo) non è più il rendering di questo attributo.

#### <a name="disabling-controls"></a>La disabilitazione dei controlli

In ASP.NET 3.5 SP1 e versioni precedenti, il framework esegue il rendering di *disabilitati* attributo nel markup HTML per qualsiasi controllo il cui *Enabled* proprietà impostata su *false*. Tuttavia, in base alle specifiche HTML 4.01, solo *input* elementi devono avere questo attributo.

In ASP.NET 4, è possibile impostare il *controlRenderingCompatabilityVersion* proprietà su "3.5", come nell'esempio seguente:

[!code-xml[Main](overview/samples/sample70.xml)]

È possibile creare markup per un *etichetta* controllo simile alla seguente, che disabilita il controllo:

[!code-aspx[Main](overview/samples/sample71.aspx)]

Il *etichetta* controllo visualizzerebbe il codice HTML seguente:

[!code-html[Main](overview/samples/sample72.html)]

In ASP.NET 4, è possibile impostare il *controlRenderingCompatabilityVersion* su "4.0". In tal caso, controlla solo il cui rendering viene eseguito *input* verranno eseguito il rendering di elementi di un *disabilitato* attributo quando il controllo *abilitato* è impostata su *false* . I controlli che non eseguono il rendering HTML *input* elementi invece il rendering di un *classe* attributo che fa riferimento a una classe CSS che è possibile usare per definire un aspetto disabilitato per il controllo. Ad esempio, il *etichetta* controllo, illustrato nell'esempio precedente genera il markup seguente:

[!code-html[Main](overview/samples/sample73.html)]

Il valore predefinito per la classe specificata per questo controllo è "aspNetDisabled". Tuttavia, è possibile modificare questo valore predefinito, impostare il metodo statico *DisabledCssClass* proprietà statica della *WebControl* classe. Per gli sviluppatori di controllo, il comportamento da utilizzare per un controllo specifico può anche essere definito usando le *SupportsDisabledAttribute* proprietà.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Se si nasconde div elementi intorno a campi nascosti

ASP.NET 2.0 e versioni successive, eseguire il rendering campi nascosti specifici del sistema (ad esempio la *nascosto* elemento usato per archiviare le informazioni sullo stato di visualizzazione) all'interno *div* elemento per garantire la conformità allo standard XHTML. Tuttavia, ciò può provocare un problema quando si verifica una regola CSS *div* elementi in una pagina. Ad esempio, può comportare una linea di un pixel visualizzati nella pagina di circa nascosto *div* elementi. In ASP.NET 4 *div* gli elementi che racchiudono i campi nascosti generati da ASP.NET aggiungere un riferimento alla classe CSS come nell'esempio seguente:

[!code-html[Main](overview/samples/sample74.html)]

È possibile definire una classe CSS che si applica solo al *nascosto* gli elementi che vengono generati da ASP.NET, come nell'esempio seguente:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Il rendering di una tabella esterna per i controlli basati su modelli

Per impostazione predefinita, i seguenti controlli server Web ASP.NET che supportano i modelli vengono eseguito il wrapping in una tabella esterna che viene usata per applicare stili in linea:

- *FormView*
- *Account di accesso*
- *PasswordRecovery*
- *ChangePassword*
- *Procedura guidata*
- *CreateUserWizard*

Una nuova proprietà denominata *RenderOuterTable* è stato aggiunto a questi controlli che consente la tabella esterna da rimuovere dal markup. Ad esempio, si consideri l'esempio seguente di una *FormView* controllo:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Questo markup viene eseguito il rendering l'output seguente alla pagina, che include una tabella HTML:

[!code-html[Main](overview/samples/sample77.html)]

Per evitare che la tabella sottoposta a rendering, è possibile impostare il *FormView* del controllo *RenderOuterTable* proprietà, come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Nell'esempio precedente viene eseguito il rendering il seguente output, senza la *tabella*, *tr*, e *td* elementi:

> Content


Questa funzionalità avanzata può rendere più semplice per applicare uno stile il contenuto del controllo con CSS, perché non viene eseguito il rendering di nessun tag imprevisto dal controllo.

> [!NOTE]
> Si noti che questa modifica disabilita il supporto per la funzione di formattazione automatica nella finestra di progettazione di Visual Studio 2010, perché non è più disponibile un' *tabella* elemento che può contenere gli attributi di stile vengono generati dall'opzione di formattazione automatica.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Miglioramenti al controllo ListView

Il *ListView* controllo è diventato più facile da usare in ASP.NET 4. La versione precedente del controllo è obbligatorio specificare un modello di layout che conteneva un controllo server con un ID noto. Il markup seguente illustra un tipico esempio di come usare il *ListView* controllo in ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

In ASP.NET 4 il *ListView* controllo non richiede un modello di layout. Il markup illustrato nell'esempio precedente può essere sostituito con il markup seguente:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Miglioramenti di controllo RadioButtonList e CheckBoxList

In ASP.NET 3.5, è possibile specificare layout per la *CheckBoxList* e *RadioButtonList* utilizzando le due impostazioni seguenti:

- *Flusso*. Il rendering del controllo *span* elementi per il relativo contenuto.
- *Tabella*. Il rendering del controllo una *tabella* elemento per il relativo contenuto.

Nell'esempio seguente viene illustrato il markup per ognuno di questi controlli.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Per impostazione predefinita, i controlli di rendering HTML simile al seguente:

[!code-html[Main](overview/samples/sample82.html)]

Poiché questi controlli contengono elenchi di elementi, eseguire il rendering HTML semanticamente corretto, deve eseguire il rendering il contenuto usando l'elenco HTML (*li*) elementi. Questo rende più semplice per gli utenti che hanno letto le pagine Web tramite le tecnologie e rende più semplice per applicare uno stile CSS tramite i controlli.

In ASP.NET 4 il *CheckBoxList* e *RadioButtonList* controlli supportano i seguenti nuovi valori per il *RepeatLayout* proprietà:

- *OrderedList* – il contenuto viene visualizzato come *li* elementi all'interno di un *ol* elemento.
- *UnorderedList* – il contenuto viene visualizzato come *li* elementi all'interno di una *ul* elemento.

Nell'esempio seguente viene illustrato come usare i nuovi valori.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Il markup precedente genera il codice HTML seguente:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Nota Se si imposta *RepeatLayout* al *OrderedList* oppure *UnorderedList*, il *RepeatDirection* proprietà non può più essere utilizzata e verrà generare un'eccezione in fase di esecuzione se la proprietà è stata impostata all'interno del markup o codice. La proprietà potrebbe non hanno alcun valore perché il layout visivo di questi controlli viene definito utilizzando invece CSS.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Miglioramenti al controllo menu

Prima di ASP.NET 4, il *Menu* controllo viene eseguito il rendering di una serie di tabelle HTML. Questo rendeva difficile applicare stili CSS di fuori di impostazione delle proprietà inline e non è conforme agli standard di accessibilità.

In ASP.NET 4, il controllo a questo punto viene eseguito il rendering HTML utilizzando markup semantico che include un elenco non ordinato e gli elementi dell'elenco. Nell'esempio seguente viene illustrato il markup in una pagina ASP.NET per la *Menu* controllo.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Quando si esegue il rendering della pagina, il controllo genera il codice HTML seguente (il *onclick* codice è stati omessi per maggiore chiarezza):

[!code-html[Main](overview/samples/sample86.html)]

Oltre ai miglioramenti per il rendering, navigazione da tastiera del menu di scelta è stata migliorata con gestione dello stato attivo. Quando la *Menu* controllo riceve lo stato attivo, è possibile usare i tasti di direzione per spostarsi di elementi. Il *dal Menu* controllo ora anche collega accessibile rich internet applications (ARIA) i ruoli e gli attributi seguente[ala il](http://www.w3.org/TR/wai-aria-practices/#menu "linee guida ARIA Menu")migliorato accessibilità.

Stili per il controllo menu vengono visualizzati in un blocco di stile nella parte superiore della pagina, invece che in linea con gli elementi HTML sottoposto a rendering. Se si vuole assumere il controllo completo tramite l'applicazione di stili per il controllo, è possibile impostare il nuovo *IncludeStyleBlock* proprietà *false*, nel qual caso il blocco di stile non viene generato. Un modo per utilizzare questa proprietà è usare la funzionalità di formattazione automatica nella finestra di progettazione di Visual Studio per impostare l'aspetto del menu. È quindi possibile eseguire la pagina, aprire l'origine della pagina e quindi copiare il blocco di stile sottoposto a rendering in un file CSS esterno. In Visual Studio, annullare l'applicazione di stili e impostare *IncludeStyleBlock* al *false*. Il risultato è che l'aspetto del menu viene definito usando gli stili in un foglio di stile esterno.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Procedura guidata e controlli CreateUserWizard

In ASP.NET *Wizard* e *CreateUserWizard* controlli supportano i modelli che consentono di definire il codice HTML che eseguono il rendering. (*CreateUserWizard* deriva da *guidata*.) Nell'esempio seguente viene illustrato il markup per basato su un completamente template *CreateUserWizard* controllo:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Il controllo viene eseguito il rendering HTML simile al seguente:

[!code-html[Main](overview/samples/sample88.html)]

In ASP.NET 3.5 SP1, sebbene sia possibile modificare il contenuto del modello, è comunque limitato controllo sull'output del *guidata* controllo. In ASP.NET 4, è possibile creare un *LayoutTemplate* modello e inserimento *segnaposto* controlli (mediante nomi riservati) per specificare come vuole la *controllo Wizard* per eseguire il rendering. L'esempio seguente illustra questo:

[!code-aspx[Main](overview/samples/sample89.aspx)]

L'esempio contiene i componenti seguenti denominati segnaposto tra il *LayoutTemplate* elemento:

- *headerPlaceholder* : in fase di esecuzione, questo viene sostituito dal contenuto del *HeaderTemplate* elemento.
- *sideBarPlaceholder* : in fase di esecuzione, questo viene sostituito dal contenuto del *SideBarTemplate* elemento.
- *wizardStepPlaceHolder* : in fase di esecuzione, questo viene sostituito dal contenuto del *WizardStepTemplate* elemento.
- *navigationPlaceholder* : in fase di esecuzione, che viene sostituita da alcun modello di navigazione che sono state definite.

Il markup nell'esempio che usa segnaposti esegue il rendering del codice HTML seguente (senza il contenuto effettivamente definito nei modelli di):

[!code-html[Main](overview/samples/sample90.html)]

Il codice HTML solo che ora non definito dall'utente è un *span* elemento. (Che in futuro si prevedono di versioni, anche il *span* elemento non vengono visualizzato.) Si ottengono così controllo completo sulla praticamente tutto il contenuto che viene generato per il *guidata* controllo.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC è stato introdotto come un framework del componente aggiuntivo in ASP.NET 3.5 SP1 nel mese di marzo 2009. Visual Studio 2010 include ASP.NET MVC 2, che include nuove funzionalità.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Supporto di aree

Le aree consentono di gruppo controller e visualizzazioni in sezioni di un'applicazione di grandi dimensioni in isolamento relativo da altre sezioni. Ogni area può essere implementato come un progetto ASP.NET MVC separato che è possibile fare riferimento dall'applicazione principale. Ciò consente di gestire la complessità quando si compila un'applicazione di grandi dimensioni e rende più semplice per più team di collaborare in una singola applicazione.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Supporto della convalida di attributo di annotazione dei dati

*DataAnnotations* attributi consentono di associare la logica di convalida a un modello utilizzando gli attributi dei metadati. *DataAnnotations* gli attributi sono state introdotte in ASP.NET Dynamic Data in ASP.NET 3.5 SP1. Questi attributi sono stati integrati nel gestore di associazione del modello predefinito e forniscono un mezzo basata sui metadati per convalidare l'input dell'utente.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Helper basati su modelli

Helper basati su modelli consentono di associa automaticamente modifica e visualizzare i modelli con tipi di dati. Ad esempio, è possibile utilizzare un helper del modello per specificare che un elemento dell'interfaccia utente di selezione data viene eseguito automaticamente il rendering per un *System. DateTime* valore. È simile ai modelli di campo in ASP.NET Dynamic Data.

Il *Html.EditorFor* e *HTML. DisplayFor* metodi helper includono supporto incorporato per il rendering dei dati standard i tipi di oggetti anche complessi con più proprietà. Sono inoltre personalizzare il rendering, consentendo di applicare gli attributi di annotazione dei dati, ad esempio *DisplayName* e *ScaffoldColumn* per il *ViewModel* oggetto.

Spesso si desidera personalizzare l'output da ulteriormente gli helper dell'interfaccia utente e avere il controllo completo su ciò che viene generato. Il *Html.EditorFor* e *HTML. DisplayFor* metodi helper ciò supportano l'utilizzo di un meccanismo di creazione di modelli che consente di definire i modelli esterni che possono eseguire l'override e il controllo dell'output sottoposto a rendering. I modelli possono essere sottoposto a rendering singolarmente per una classe.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data

I dati dinamici è stata introdotta nella versione di .NET Framework 3.5 SP1 in metà del 2008. Questa funzionalità offre numerosi miglioramenti per la creazione di applicazioni guidate dai dati, incluse le seguenti:

- Un'esperienza RAD per creare rapidamente un sito Web basato sui dati.
- Convalida automatica che si basa sui vincoli definiti nel modello di dati.
- La possibilità di modificare facilmente il markup generato per i campi di *GridView* e *DetailsView* controlli con i modelli di campo che fanno parte del progetto di Dynamic Data.

> [!NOTE]
> Nota: per altre informazioni, vedere la [documentazione di Dynamic Data](https://msdn.microsoft.com/library/cc488545.aspx) in MSDN Library.


Per ASP.NET 4, i dati dinamici è stato migliorato per consentire agli sviluppatori di eseguire operazioni più avanzate consente di creare rapidamente siti Web basati sui dati.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Abilitazione di Dynamic Data per i progetti esistenti

Le funzionalità dinamiche dei dati inclusi in .NET Framework 3.5 SP1 introdotte nuove funzionalità come il seguente:

- I modelli di campo: questi forniscono modelli basato sul tipo di dati per i controlli con associazione a dati. I modelli di campo forniscono un modo più semplice per personalizzare l'aspetto dei controlli di dati rispetto all'uso dei campi modello per ogni campo.
- Convalida: Dynamic Data consente di usare gli attributi su classi di dati per specificare la convalida per gli scenari comuni, ad esempio i campi obbligatori, il controllo degli intervalli, il controllo dei tipi, criteri di ricerca tramite espressioni regolari e la convalida personalizzata. La convalida viene applicata per i controlli dei dati.

Tuttavia, queste funzioni presentano i requisiti seguenti:

- Il livello di accesso ai dati doveva essere basato su Entity Framework o LINQ to SQL.
- Gli unici dati di origine controlli supportati per queste funzionalità erano il *EntityDataSource* oppure *LinqDataSource* controlli.
- Le funzionalità necessarie a un progetto Web creato utilizzando i modelli di entità dinamiche dei dati o dei dati dinamici per disporre di tutti i file che erano necessarie per supportare la funzionalità.

Un obiettivo principale del supporto per i dati dinamici in ASP.NET 4 consiste nell'abilitare la nuova funzionalità di Dynamic Data per qualsiasi applicazione ASP.NET. Nell'esempio seguente viene illustrato il markup per i controlli che possono sfruttare i vantaggi delle funzionalità dinamica dei dati in una pagina esistente.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Nel codice della pagina, deve essere aggiunto il codice seguente per abilitare il supporto di Dynamic Data per questi controlli:

[!code-csharp[Main](overview/samples/sample92.cs)]

Quando la *GridView* controllo è in modalità di modifica, Dynamic Data verifica automaticamente che i dati immessi siano nel formato corretto. In caso contrario, viene visualizzato un messaggio di errore.

Questa funzionalità offre anche altri vantaggi, ad esempio la possibilità di specificare il valore predefinito i valori per la modalità di inserimento. Senza i dati dinamici, per implementare un valore predefinito per un campo, è necessario collegare a un evento, individuare il controllo del codice (utilizzando *FindControl*) e impostarne il valore. In ASP.NET 4 il *EnableDynamicData* chiamata supporta un secondo parametro che consente di passare i valori predefiniti per tutti i campi nell'oggetto, come illustrato in questo esempio:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Sintassi dichiarativa per il controllo DynamicDataManager

Il *DynamicDataManager* controllo è stato migliorato in modo che è possibile configurarlo in modo dichiarativo, come con la maggior parte dei controlli in ASP.NET, anziché solo nel codice. Il markup per il *DynamicDataManager* controllo è simile al seguente:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Questo markup consente al comportamento di Dynamic Data per il controllo GridView1 a cui fa riferimento il *trascinarne* sezione il *DynamicDataManager* controllo.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Modelli di entità

I modelli di entità offrono un nuovo modo per personalizzare il layout dei dati senza che sia necessario creare una pagina personalizzata. Usare i modelli di pagina il *FormView* controllo (anziché il *DetailsView* controllare, usati nei modelli di pagina nelle versioni precedenti di Dynamic Data) e il *DynamicEntity* controllo per il rendering di modelli di entità. Ciò offre maggiore controllo sul markup che viene eseguito il rendering da Dynamic Data.

L'elenco seguente mostra il nuovo layout di directory di progetto che contiene i modelli di entità:

[!code-console[Main](overview/samples/sample95.cmd)]

Il `EntityTemplate` directory contiene modelli per informazioni su come visualizzare gli oggetti del modello di dati. Per impostazione predefinita, gli oggetti vengono sottoposti a rendering utilizzando il `Default.ascx` modello, che fornisce il markup che somiglia proprio ad il markup creato dal *DetailsView* controllo utilizzato per i dati dinamici in ASP.NET 3.5 SP1. Nell'esempio seguente viene illustrato il markup per il `Default.ascx` controllo:

[!code-aspx[Main](overview/samples/sample96.aspx)]

I modelli predefiniti possono essere modificati per modificare l'aspetto dell'intero sito. Sono disponibili modelli per la visualizzazione, modifica e le operazioni di inserimento. Nuovi modelli possono essere aggiunti in base al nome dell'oggetto dati per modificare l'aspetto di un solo tipo di oggetto. Ad esempio, è possibile aggiungere il modello seguente:

[!code-console[Main](overview/samples/sample97.cmd)]

Il modello può contenere il markup seguente:

[!code-aspx[Main](overview/samples/sample98.aspx)]

I nuovi modelli di entità vengono visualizzati in una pagina con la nuova *DynamicEntity* controllo. In fase di esecuzione, questo controllo viene sostituito con il contenuto del modello di entità. Il markup seguente mostra le *FormView* controllare nel `Detail.aspx` modello di pagina che utilizza il modello di entità. Si noti che il *DynamicEntity* elemento nel markup.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nuovi modelli di campo per URL e indirizzi di posta elettronica

ASP.NET 4 introduce due nuovi modelli di campo predefinito, `EmailAddress.ascx` e `Url.ascx`. Questi modelli vengono usati per i campi contrassegnati come *EmailAddress* oppure *Url* con il *DataType* attributo. Per la *EmailAddress* oggetti, il campo viene visualizzato come collegamento ipertestuale che viene creato usando la *mailto:* protocollo. Quando gli utenti fanno clic sul collegamento, viene aperto il client di posta elettronica dell'utente e crea una struttura di un messaggio. Gli oggetti tipizzati come *Url* vengono visualizzati come collegamenti ipertestuali comuni.

Nell'esempio seguente viene illustrato come i campi potrebbero essere contrassegnati.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Creazione di collegamenti con il controllo DynamicHyperLink

Dynamic Data utilizza la nuova funzionalità di routing che è stata aggiunta in .NET Framework 3.5 SP1 per controllare l'URL visualizzato agli utenti finali quando accedono al sito Web. Il nuovo *DynamicHyperLink* controllo semplifica la compilazione di collegamenti alle pagine in un sito Dynamic Data. Nell'esempio seguente viene illustrato come utilizzare il *DynamicHyperLink* controllo:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Questo codice crea un collegamento che punta alla pagina dell'elenco per il `Products` tabella basata su route definite nel `Global.asax` file. Il controllo utilizza automaticamente il nome di tabella predefinito basato su pagina di dati dinamica.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Supporto per l'ereditarietà nel modello di dati

Entity Framework e LINQ to SQL supporta l'ereditarietà nei modelli di dati. Un esempio di questo potrebbe essere un database che include un `InsurancePolicy` tabella. È anche possibile che contenga `CarPolicy` e `HousePolicy` le tabelle che includono gli stessi campi degli `InsurancePolicy` e quindi aggiungere altri campi. Dati dinamica sono stati modificati per comprendere gli oggetti ereditati nel modello di dati e per supportare lo scaffolding per le tabelle ereditate.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Supporto per le relazioni molti-a-molti (solo per Entity Framework)

Entity Framework offre supporto avanzato per le relazioni molti-a-molti tra le tabelle, che viene implementato esponendo la relazione come una raccolta in un' *entità* oggetto. Nuove `ManyToMany.ascx` e `ManyToMany_Edit.ascx` i modelli di campo sono stati aggiunti per fornire supporto per la visualizzazione e modifica di dati coinvolti in relazioni molti-a-molti.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nuovi attributi di visualizzazione del controllo e il supporto delle enumerazioni

Il *DisplayAttribute* è stato aggiunto per offrirti maggiore controllo sulle modalità di visualizzazione dei campi. Il *DisplayName* attributo nelle versioni precedenti dei dati dinamici è possibile modificare il nome utilizzato come didascalia per un campo. Il nuovo *DisplayAttribute* classe consente di specificare più opzioni per la visualizzazione di un campo, ad esempio l'ordine in cui viene visualizzato un campo e se verrà utilizzato un campo come filtro. L'attributo offre anche un controllo indipendente del nome usato per le etichette in un *GridView* controllare, il nome usato in un *DetailsView* determinano, il testo della Guida per il campo, e la filigrana utilizzati per il campo (se il campo accetta input di testo).

Il *EnumDataTypeAttribute* classe è stato aggiunto per consentire di eseguire il mapping campi per le enumerazioni. Quando si applica questo attributo a un campo, si specifica un tipo di enumerazione. Dynamic Data utilizza il nuovo `Enumeration.ascx` modello di campo per la creazione dell'interfaccia utente per visualizzare e modificare i valori di enumerazione. Il modello viene eseguito il mapping di valori del database per i nomi nell'enumerazione.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Supporto avanzato per i filtri

Dynamic Data 1.0 forniti con i filtri predefiniti per colonne booleane e le colonne di chiave esterna. I filtri non ha consentito di specificare se fossero visualizzati o l'ordine in cui sono stati visualizzati. Il nuovo *DisplayAttribute* attributo indirizzi entrambi questi problemi consentendo di controllano se una colonna viene visualizzata come filtro e in quale ordine che verrà visualizzato.

Offrire un ulteriore miglioramento è che il supporto di filtro è stato reinstallato[scritto per utilizzare il nuovo](#0.2__QueryExtender "_QueryExtender") funzionalità di Web Form. Ciò consente di creare filtri senza richiedere la conoscenza del controllo origine dati che verranno usati con i filtri. Insieme a queste estensioni, i filtri sono anche stati attivati in controlli del modello, pertanto è possibile aggiungerne di nuovi. Infine, il *DisplayAttribute* classe indicato in precedenza consente il filtro predefinito eseguire l'override, nello stesso modo in cui *UIHint* consente al modello di campo predefinite per una colonna da sottoporre a override.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Miglioramenti di sviluppo Web di Visual Studio 2010

Sviluppo Web in Visual Studio 2010 è stato migliorato per la compatibilità CSS maggiore, un aumento della produttività mediante i frammenti di markup HTML e ASP.NET e nuovo IntelliSense JavaScript dinamica.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilità CSS migliorato

La finestra di progettazione di Visual Web Developer in Visual Studio 2010 è stata aggiornata per migliorare la conformità agli standard CSS 2.1. La finestra di progettazione consente di mantenere l'integrità di origine HTML meglio e più robusto nelle versioni precedenti di Visual Studio. Dietro le quinte, dell'architettura sono anche stati apportati miglioramenti che consentono una migliore futuri miglioramenti nella gestibilità, layout e rendering.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Frammenti di codice JavaScript e HTML

Nell'editor HTML, IntelliSense e completa automaticamente i nomi di tag. La funzionalità frammenti di codice IntelliSense e completa automaticamente intero tag e così via. In Visual Studio 2010, sono supportati frammenti di codice IntelliSense per JavaScript, insieme a c# e Visual Basic, che sono supportati nelle versioni precedenti di Visual Studio.

Visual Studio 2010 include frammenti di oltre 200 che consentono di completare automaticamente i tag HTML e ASP.NET comuni, inclusi gli attributi obbligatori (ad esempio runat = "server") e gli attributi comuni specifici di un tag (ad esempio *ID*,  *Elemento DataSourceID*, *ControlToValidate*, e *testo*).

È possibile scaricare ulteriori frammenti di codice, oppure è possibile scrivere frammenti di codice personalizzati che incapsulano i blocchi di markup che utente o il team utilizza per attività comuni.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Miglioramenti di IntelliSense per JavaScript

In Visual Studio 2010, IntelliSense per JavaScript è stato riprogettato per fornire un'esperienza di modifica ancora più dettagliata. IntelliSense ora riconosce gli oggetti generati dinamicamente dai metodi, ad esempio *registerNamespace* e tecniche simili usate da altri framework JavaScript. Prestazioni sono state migliorate per analizzare librerie di script di grandi dimensioni e la visualizzazione di IntelliSense con poco o nessun ritardo di elaborazione. Compatibilità è stata notevolmente migliorata per supportare quasi tutte le librerie di terze parti e supportare diversi stili di codifica. I commenti della documentazione vengono ora analizzati durante la digitazione e immediatamente vengono sfruttate da IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Distribuzione di applicazioni Web con Visual Studio 2010

Quando gli sviluppatori ASP.NET distribuisce un'applicazione Web, sono spesso disponibili che rilevano i problemi seguenti:

- La distribuzione in un sito di hosting condiviso richiede tecnologie quali FTP, che può essere lenta. Inoltre, è necessario eseguire manualmente le attività quali l'esecuzione di script SQL per configurare un database ed è necessario modificare le impostazioni di IIS, ad esempio la configurazione di una cartella della directory virtuale come un'applicazione.
- In un ambiente aziendale, oltre a distribuire i file dell'applicazione Web, gli amministratori devono modificare frequentemente il file di configurazione di ASP.NET e le impostazioni di IIS. Gli amministratori di database devono eseguire una serie di script SQL per ottenere il database dell'applicazione in esecuzione. Queste installazioni sono laboriosa, spesso richiedere ore e deve essere documentato con cura.

Visual Studio 2010 include tecnologie che risolvere questi problemi, che consentono di distribuire facilmente applicazioni Web. Una di queste tecnologie è lo strumento di distribuzione Web IIS (MsDeploy.exe).

Funzionalità di distribuzione Web in Visual Studio 2010 include le aree principali seguenti:

- Creazione di pacchetti Web
- Trasformazione Web. config
- Distribuzione del database
- Pubblicazione con un clic per le applicazioni Web

Le sezioni seguenti riportano informazioni dettagliate su queste funzionalità.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Creazione di pacchetti Web

Visual Studio 2010 Usa lo strumento MSDeploy per creare un file compresso (zip) per l'applicazione, che viene considerata una *pacchetto Web*. Il file del pacchetto contiene i metadati relativi all'applicazione e il contenuto seguente:

- Impostazioni di IIS, che include impostazioni pool di applicazioni, impostazioni di pagina di errore e così via.
- Il contenuto Web effettivo, che include le pagine Web, controlli utente, contenuto statico (immagini e file HTML) e così via.
- Gli schemi di database di SQL Server e i dati.
- Certificati di sicurezza, dei componenti da installare nella Global Assembly Cache, le impostazioni del Registro di sistema e così via.

Un pacchetto Web può essere copiato in qualsiasi server e quindi installare manualmente tramite Gestione IIS. In alternativa, per la distribuzione automatica, il pacchetto può essere installato usando i comandi della riga di comando o tramite API di distribuzione.

Visual Studio 2010 offre creata nell'attività di MSBuild e destinazioni per creare pacchetti di Web. Per altre informazioni, vedere [Panoramica della distribuzione progetto applicazione Web ASP.NET](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) sul sito Web MSDN e [10 + 20 motivi per cui è necessario creare un pacchetto Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) sul blog di Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Trasformazione Web. config

Per la distribuzione di applicazioni Web, Visual Studio 2010 viene introdotta [trasformazione XDT (XML Document)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), che è una funzionalità che consente di trasformare un `Web.config` file dalle impostazioni di sviluppo per le impostazioni di produzione. Le impostazioni di trasformazione sono specificate nel file di trasformazione denominati `web.debug.config`, `web.release.config`e così via. (I nomi di questi file corrispondono le configurazioni di MSBuild). Un file di trasformazione include solo le modifiche che è necessario apportare a un distribuito `Web.config` file. Si specificano le modifiche usando la sintassi semplice.

Nell'esempio seguente illustra una parte di un `web.release.config` file che può essere prodotti per la distribuzione della configurazione di rilascio. La parola chiave Replace nell'esempio specifica che durante la distribuzione di *connectionString* nodo il `Web.config` file verrà sostituito con i valori sono elencati nell'esempio.

[!code-xml[Main](overview/samples/sample102.xml)]

Per altre informazioni, vedere [sintassi di trasformazione Web. config per la distribuzione del progetto dell'applicazione Web](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) in MSDN <a id="0.2_a"> </a> sito Web e[distribuzione Web: trasformazione Web. config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)nel blog di Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Distribuzione del database

Un pacchetto di distribuzione di Visual Studio 2010 può includere le dipendenze nei database di SQL Server. Come parte della definizione del pacchetto, è fornire la stringa di connessione per il database di origine. Quando si crea il pacchetto Web, Visual Studio 2010 consente di creare script SQL per lo schema del database e, facoltativamente, per i dati e li aggiunge al pacchetto. È anche possibile specificare gli script SQL personalizzati e la sequenza in cui deve essere eseguito nel server. In fase di distribuzione è fornire una stringa di connessione appropriato per il server di destinazione; il processo di distribuzione Usa quindi questa stringa di connessione per eseguire gli script di creare lo schema del database e aggiungono i dati.

Inoltre, con un solo clic pubblica, è possibile configurare la distribuzione per pubblicare direttamente il database quando l'applicazione viene pubblicata in un sito di hosting condiviso remoto. Per altre informazioni, vedere [procedura: distribuire un Database con un progetto di applicazione Web](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) sul sito Web MSDN e [distribuzione del Database con Visual Studio 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) sul blog di Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Pubblicazione con un clic per le applicazioni Web

Visual Studio 2010 consente inoltre di usare il servizio Gestione remota di IIS per pubblicare un'applicazione Web a un server remoto. È possibile creare un profilo di pubblicazione per l'account di hosting o per i server di test o i server di gestione temporanea. Ogni profilo può salvare in modo sicuro le credenziali appropriate. È quindi possibile distribuire a qualsiasi destinazione server con un solo clic tramite il Web con un clic sulla barra degli strumenti di pubblicazione. Con Visual Studio 2010, è possibile pubblicare anche tramite la riga di comando di MSBuild. Ciò consente di configurare l'ambiente di compilazione team in modo da includere la pubblicazione in un modello di integrazione continua.

Per altre informazioni, vedere [procedura: distribuire un Web Application Project tramite un solo clic la pubblicazione e distribuzione Web](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) sul sito Web MSDN e [Web 1-fare clic su pubblica con Visual Studio 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) sul blog di Vishal Joshi. Per visualizzare presentazioni video relative alla distribuzione di applicazioni Web in Visual Studio 2010, vedere [Visual Studio 2010 per Web Developer Preview](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) sul blog di Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Risorse

I siti Web seguenti forniscono informazioni aggiuntive su ASP.NET 4 e Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) , consultare la documentazione ufficiale per ASP.NET 4 nel sito Web MSDN.
- [https://www.asp.net/](https://www.asp.net/) : ASP.NET il sito Web del team.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) e [ASP.NET Dynamic Data Content Map](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) , alle risorse Online sul sito del team ASP.NET e nella documentazione ufficiale per ASP.NET Dynamic Data.
- [https://www.asp.net/ajax/](../../ajax/index.md) Ovvero la risorsa Web principale per lo sviluppo di ASP.NET Ajax.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) Ovvero il blog del Team di Visual Web Developer, che include informazioni sulle funzionalità di Visual Studio 2010.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) , ovvero la risorsa Web principale per le versioni di anteprima di ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute nel presente documento rappresentano l'attuale opinione di Microsoft Corporation circa le problematiche discusse alla data della pubblicazione. Poiché Microsoft deve rispondere ai cambiamenti delle condizioni di mercato, il presente documento non deve essere interpretato quale un impegno da parte di Microsoft e Microsoft non può garantire l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è a esclusivo carico dell'utente. Senza limitare i diritti sanciti dal copyright, nessuna parte di questo documento può essere riprodotta, memorizzata o inserita in un sistema di ricerca o trasmessa in qualsiasi forma o mezzo (elettronico o meccanico, mediante fotocopia, registrazione o altro), per alcuno scopo, senza l'autorizzazione scritta di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto del presente documento. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna del presente documento non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non diversamente specificato, la società, organizzazioni, prodotti, nomi di dominio, indirizzi di posta elettronica, logo, persone, luoghi ed eventi citati in questo documento sono fittizio e nessuna associazione con qualsiasi società, organizzazione, prodotto, nome di dominio, indirizzo di posta elettronica indirizzo, logo, persona, luogo o evento è intenzionale o può essere presupposta.

© 2009 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
