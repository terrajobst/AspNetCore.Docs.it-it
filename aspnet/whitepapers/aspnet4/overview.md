---
uid: whitepapers/aspnet4/overview
title: In ASP.NET 4 e Cenni preliminari sullo sviluppo di Visual Studio 2010 Web | Documenti Microsoft
author: rick-anderson
description: Questo documento fornisce una panoramica delle numerose delle nuove funzionalità per ASP.NET che sono inclusi in.NET Framework 4 e in Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 6ce52c387ff835eda46bc1882b8b974889e2d4af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>Cenni preliminari sullo sviluppo di Visual Studio 2010 Web e di ASP.NET 4
====================
> Questo documento fornisce una panoramica delle numerose delle nuove funzionalità per ASP.NET che sono inclusi in.NET Framework 4 e in Visual Studio 2010.
> 
> [Scaricare il white paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Sommario**

**[Servizi principali](#0.2__Toc253429238 "_Toc253429238")**  
[File Web. config Refactoring](#0.2__Toc253429239 "_Toc253429239")  
[La memorizzazione nella cache di Output Extensible](#0.2__Toc253429240 "_Toc253429240")  
[Avvio automatico di applicazioni Web](#0.2__Toc253429241 "_Toc253429241")  
[Una pagina di reindirizzamento permanente](#0.2__Toc253429242 "_Toc253429242")  
[La compattazione lo stato della sessione](#0.2__Toc253429243 "_Toc253429243")  
[Espansione dell'intervallo di URL consentiti](#0.2__Toc253429244 "_Toc253429244")  
[Convalida delle richieste Extensible](#0.2__Toc253429245 "_Toc253429245")  
[La memorizzazione nella cache dell'oggetto e l'oggetto estensibilità memorizzazione nella cache](#0.2__Toc253429246 "_Toc253429246")  
[Extensible HTML, l'URL e codifica dell'intestazione HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitoraggio delle prestazioni per le singole applicazioni in un singolo processo di lavoro](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery inclusa con Web Form e MVC](#0.2__Toc253429251 "_Toc253429251")  
[Supporto di rete per il recapito di contenuto](#0.2__Toc253429252 "_Toc253429252")  
[Gli script esplicita ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Web Form](#0.2__Toc253429256 "_Toc253429256")**  
[Impostazione metatag con le proprietà di Page.MetaDescription e Page.MetaKeywords](#0.2__Toc253429257 "_Toc253429257")  
[L'abilitazione di stato di visualizzazione per i singoli controlli](#0.2__Toc253429258 "_Toc253429258")  
[Modifiche alle funzionalità del Browser](#0.2__Toc253429259 "_Toc253429259")  
[Routing in ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Impostazione degli ID Client](#0.2__Toc253429261 "_Toc253429261")  
[Impostazione della persistenza selezione di riga nei controlli di dati](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET Chart Control](#0.2__Toc253429263 "_Toc253429263")  
[Filtro dei dati con il controllo QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Le espressioni di codice codificata in formato HTML](#0.2__Toc253429265 "_Toc253429265")  
[Le modifiche del modello di progetto](#0.2__Toc253429266 "_Toc253429266")  
[Miglioramenti di CSS](#0.2__Toc253429267 "_Toc253429267")  
[Nascondere elementi intorno a campi nascosti div](#0.2__Toc253429268 "_Toc253429268")  
[Il rendering di una tabella esterna per i controlli basati su modelli](#0.2__Toc253429269 "_Toc253429269")  
[Miglioramenti al controllo ListView](#0.2__Toc253429270 "_Toc253429270")  
[Miglioramenti di controllo RadioButtonList e CheckBoxList](#0.2__Toc253429271 "_Toc253429271")  
[Miglioramenti apportati ai controlli menu](#0.2__Toc253429272 "_Toc253429272")  
[Procedura guidata e controlli CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Supporto di aree](#0.2__Toc253429275 "_Toc253429275")  
[Supporto della convalida attributo annotazione dei dati](#0.2__Toc253429276 "_Toc253429276")  
[Helper basati su modelli](#0.2__Toc253429277 "_Toc253429277")

**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**  
[Abilitazione di Dynamic Data per i progetti esistenti](#0.2__Toc253429279 "_Toc253429279")  
[La sintassi dichiarativa per il controllo DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[I modelli di entità](#0.2__Toc253429281 "_Toc253429281")  
[Nuovi modelli di campo per URL e indirizzi di posta elettronica](#0.2__Toc253429282 "_Toc253429282")  
[Creazione di collegamenti con il controllo DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Supporto per l'ereditarietà nel modello di dati](#0.2__Toc253429284 "_Toc253429284")  
[Supporto per le relazioni molti-a-molti (solo Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nuovi attributi di visualizzazione del controllo e il supporto delle enumerazioni](#0.2__Toc253429286 "_Toc253429286")  
[Supporto migliorato per i filtri](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web miglioramenti per lo sviluppo](#0.2__Toc253429288 "_Toc253429288")**  
[Migliorata compatibilità CSS](#0.2__Toc253429289 "_Toc253429289")  
[Frammenti di codice JavaScript e HTML](#0.2__Toc253429290 "_Toc253429290")  
[Miglioramenti di IntelliSense per JavaScript](#0.2__Toc253429291 "_Toc253429291")

**[Distribuzione di applicazioni con Visual Studio 2010 Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web imballaggio](#0.2__Toc253429293 "_Toc253429293")  
[Trasformazione Web. config](#0.2__Toc253429294 "_Toc253429294")  
[Distribuzione del database](#0.2__Toc253429295 "_Toc253429295")  
[Pubblicazione per le applicazioni Web con un clic](#0.2__Toc253429296 "_Toc253429296")  
[Resources](#0.2__Toc253429297 "_Toc253429297")

**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Servizi di base

In ASP.NET 4 introduce numerose funzionalità che migliorano i servizi ASP.NET di base, ad esempio la memorizzazione nella cache di output e l'archiviazione dello stato della sessione.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>File Web. config di Refactoring

Il `Web.config` file che contiene la configurazione per un'applicazione Web è aumentato notevolmente rispetto alle versioni precedenti di .NET Framework come sono state aggiunte nuove funzionalità, ad esempio Ajax, routing e integrazione con IIS 7. Questo ha reso difficile configurare o avviare nuove applicazioni Web senza uno strumento come Visual Studio. In the .NET Framework 4, sono stati spostati gli elementi di configurazione principali di `machine.config` file e le applicazioni ora ereditano tali impostazioni. In questo modo il `Web.config` file nelle applicazioni ASP.NET 4 può essere vuoto o contenere solo le righe seguenti che specificano per Visual Studio, la versione di framework è destinata l'applicazione:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>La memorizzazione nella cache di Output estendibile

Dal momento che è stato rilasciato ASP.NET 1.0, gli sviluppatori archiviare l'output generato di pagine, controlli e le risposte HTTP in memoria è abilitata la memorizzazione nella cache di output. Alle successive richieste Web, ASP.NET può essere utilizzato il contenuto più rapidamente recuperando l'output generato dalla memoria anziché rigenerarlo l'output da zero. Tuttavia, questo approccio presenta una limitazione, contenuto generato deve sempre essere archiviati in memoria e nei server in cui si verifica un traffico elevato, la memoria utilizzata per la memorizzazione nella cache di output può competere con richieste di memoria da altre parti di un'applicazione Web.

In ASP.NET 4 aggiunge un punto di estensibilità per la memorizzazione nella cache di output che consente di configurare uno o più provider di cache di output personalizzati. Provider di cache di output possono utilizzare qualsiasi meccanismo di archiviazione per mantenere il contenuto HTML. In questo modo è possibile creare provider di cache di output personalizzati per i meccanismi di persistenza diverse, che possono includere i dischi locali o remoti, archiviazione cloud e distribuito motori di cache.

Creare un provider di cache di output personalizzato come una classe che deriva dalla nuova *System.Web.Caching.OutputCacheProvider* tipo. È quindi possibile configurare il provider nel `Web.config` file utilizzando il nuovo *provider* sottosezione del *outputCache* elemento, come illustrato nell'esempio seguente:

[!code-xml[Main](overview/samples/sample2.xml)]

Per impostazione predefinita in ASP.NET 4, tutte le risposte HTTP, il rendering pagine e controlli utilizzano la cache di output in memoria, come illustrato nell'esempio precedente, in cui il *defaultProvider* attributo è impostato su AspNetInternalProvider. È possibile modificare il provider di cache di output predefinito utilizzato per un'applicazione Web specificando un nome di un provider diverso per *defaultProvider*.

Inoltre, è possibile selezionare un provider di cache di output diversi per ogni controllo e per ogni richiesta. È il modo più semplice per scegliere un provider di cache di output diverso per controlli utente Web diversi per eseguire in modo dichiarativo tramite la nuova *providerName* attributo in una direttiva di controllo, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Specificare un provider di cache di output diverso per una richiesta HTTP richiede un po' più lavoro. Anziché specificare in modo dichiarativo il provider, si esegue l'override di nuovo *GetOuputCacheProviderName* metodo il `Global.asax` file per specificare a livello di codice il provider da utilizzare per una richiesta specifica. Nell'esempio seguente viene illustrato come effettuare questa operazione.

[!code-csharp[Main](overview/samples/sample4.cs)]

Con l'aggiunta di estensibilità del provider della cache di output di ASP.NET 4, è ora possibile esercitare più aggressive e intelligente più strategie di memorizzazione nella cache di output per i siti Web. Ad esempio, è ora possibile memorizzare nella cache le pagine "Top 10" di un sito in memoria, durante la memorizzazione nella cache le pagine con un minore traffico su disco. In alternativa, è possibile memorizzare nella cache di ogni combinazione di variabili per una pagina visualizzata, ma utilizzare una cache distribuita in modo che il consumo di memoria viene trasferito dal server Web front-end.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Applicazioni Web di avvio automatico

Alcune applicazioni Web è necessario caricare grandi quantità di dati o eseguire l'inizializzazione costosa prima di servire la prima richiesta di elaborazione. Nelle versioni precedenti di ASP.NET, in queste situazioni era necessario ideare approcci personalizzati per "riattivazione" un'applicazione ASP.NET, quindi eseguire il codice di inizializzazione durante il *applicazione\_carico* metodo il `Global.asax` file.

Una nuova funzionalità di scalabilità denominata *avvio automatico* che direttamente gli indirizzi di questo scenario è disponibile quando ASP.NET 4 viene eseguito in IIS 7.5 in Windows Server 2008 R2. La funzionalità di avvio automatico offre un approccio controllato per l'avvio di un pool di applicazioni, l'inizializzazione di un'applicazione ASP.NET e l'accettazione delle richieste HTTP.

> [!NOTE] 
> 
> Modulo di riscaldamento di applicazioni IIS per IIS 7.5
> 
> Il team IIS ha rilasciato la versione beta test prima del modulo di riscaldamento di applicazione per IIS 7.5. In questo modo l'avvio di applicazioni anche più facile di quanto descritto in precedenza. Invece di scrivere codice personalizzato, specificare gli URL delle risorse da eseguire prima l'applicazione Web accetta le richieste dalla rete. Si verifica questo riscaldamento durante l'avvio del servizio IIS (se è stato configurato il pool di applicazioni IIS come *AlwaysRunning*) e quando un processo di lavoro IIS ricicla. Durante il riciclo, il processo di lavoro IIS precedenti continua a eseguire le richieste finché il processo di lavoro appena generato è riscaldato completamente, in modo che le applicazioni verificano non interruzioni o altri problemi a causa di unprimed cache. Si noti che questo modulo funziona con qualsiasi versione di ASP.NET, a partire dalla versione 2.0.
> 
> Per ulteriori informazioni, vedere [Application warm-up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) nel sito IIS.net Web. Per una procedura dettagliata viene illustrato come utilizzare la funzionalità di riscaldamento, vedere [introduzione con il modulo di riscaldamento di IIS 7.5 applicazione](https://www.iis.net/learn/manage) nel sito IIS.net Web.


Per utilizzare la funzionalità di avvio automatico, un amministratore di IIS imposta un pool di applicazioni in IIS 7.5 deve essere avviato automaticamente utilizzando la configurazione in seguito il `applicationHost.config` file:

[!code-xml[Main](overview/samples/sample5.xml)]

Poiché un singolo pool di applicazioni può contenere più applicazioni, specificare singole applicazioni deve essere avviato automaticamente utilizzando la configurazione in seguito il `applicationHost.config` file:

[!code-xml[Main](overview/samples/sample6.xml)]

Quando un server IIS 7.5 è avvio a freddo o un pool di applicazioni viene riciclato, IIS 7.5 utilizza le informazioni di `applicationHost.config` file per determinare che necessitano di applicazioni Web deve essere avviato automaticamente. Per ogni applicazione che è contrassegnato per l'avvio automatico, IIS 7.5 invia una richiesta ad ASP.NET 4 per avviare l'applicazione in uno stato durante il quale l'applicazione temporaneamente non accetta richieste HTTP. Quando è in questo stato, ASP.NET crea il tipo definito dal *serviceAutoStartProvider* attributo (come illustrato nell'esempio precedente) e chiama il relativo punto di ingresso pubblico.

Creare un tipo di avvio automatico gestito con il punto di ingresso mediante l'implementazione di *IProcessHostPreloadClient* interfaccia, come illustrato nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample7.cs)]

Dopo l'inizializzazione viene eseguito codice il *precaricamento* metodo e il metodo restituisce, l'applicazione ASP.NET è pronta a elaborare le richieste.

Con l'aggiunta di avvio automatico,5 IIS e ASP.NET 4, è ora disponibile un approccio per eseguire l'inizializzazione dell'applicazione costosa prima dell'elaborazione della richiesta HTTP prima ben definito. Ad esempio, è possibile utilizzare la nuova funzionalità di avvio automatico per inizializzare un'applicazione e quindi segnalare un bilanciamento del carico che l'applicazione è stato inizializzato e pronto ad accettare il traffico HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Reindirizzamento permanente di una pagina

È pratica comune nelle applicazioni Web per spostare pagine e contenuto intorno nel tempo, causando un accumulo di collegamenti non aggiornati nei motori di ricerca. In ASP.NET, gli sviluppatori hanno tradizionalmente gestito le richieste di URL precedente mediante il *Response. Redirect* metodo per inoltrare una richiesta al nuovo URL. Tuttavia, il *reindirizzare* metodo invia una risposta HTTP 302 trovato (reindirizzamento temporaneo), che comporta un HTTP aggiuntivo round trip quando gli utenti tentano di accedere gli URL precedenti.

Aggiunge un nuovo ASP.NET 4 *RedirectPermanent* metodo helper che rende più semplice da rilasciare HTTP 301 spostato in modo permanente le risposte, come nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample8.cs)]

Motori di ricerca e altri agenti utente che riconoscono i reindirizzamenti permanenti archivierà il nuovo URL che è associato al contenuto, che elimina l'apportate dal browser per i reindirizzamenti temporanei di round trip non necessario.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Riduzione dello stato della sessione

ASP.NET fornisce due opzioni predefinite per l'archiviazione dello stato della sessione in una Web farm: un provider di stato della sessione che richiama un server di stato della sessione out-of-process e un provider di stato della sessione che archivia i dati in un database di Microsoft SQL Server. Poiché entrambe le opzioni includono la memorizzazione delle informazioni di stato di fuori di processo di lavoro di un'applicazione Web, lo stato della sessione è stata serializzata prima che venga inviato in un archivio remoto. A seconda della quantità di informazioni consente di salvare uno sviluppatore nello stato sessione, le dimensioni dei dati serializzati possono raggiungere dimensioni piuttosto grandi.

In ASP.NET 4 introduce una nuova opzione di compressione per entrambi i tipi di provider di stato della sessione out-of-process. Quando il *compressionEnabled* illustrato nell'esempio seguente l'opzione di configurazione è impostata su *true*, ASP.NET verrà comprimere (e decomprimere) lo stato sessione serializzato con .NET Framework  *System.IO.Compression.GZipStream* classe.

[!code-xml[Main](overview/samples/sample9.xml)]

Con l'aggiunta del nuovo attributo per semplice di `Web.config` file, le applicazioni con cicli di CPU nel server Web di realizzare riduzioni sostanziale delle dimensioni dei dati dello stato sessione serializzati.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Espansione dell'intervallo di URL consentiti

In ASP.NET 4 introduce nuove opzioni per l'espansione dell'URL dell'applicazione. Le versioni precedenti di ASP.NET vincolato lunghezza dei percorsi URL a 260 caratteri, basati sul limite di percorso del file system NTFS. In ASP.NET 4, è possibile aumentare questo limite come appropriato per le applicazioni, (o diminuire) utilizza due nuovi *httpRuntime* attributi di configurazione. Nell'esempio seguente mostra questi nuovi attributi.

[!code-xml[Main](overview/samples/sample10.xml)]

Per consentire i percorsi superiori o inferiori (parte dell'URL che non include protocollo, nome del server e stringa di query), modificare il *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* attributo. Per consentire le stringhe di query superiori o inferiori, modificare il valore di *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* attributo.

In ASP.NET 4 consente inoltre di configurare i caratteri che vengono utilizzati dal controllo di caratteri di URL. Quando ASP.NET rileva un carattere non valido nella parte di percorso di un URL, rifiuta la richiesta e genera un errore HTTP 400. Nelle versioni precedenti di ASP.NET, i controlli di caratteri di URL sono limitati a un insieme fisso di caratteri. In ASP.NET 4, è possibile personalizzare il set di caratteri validi utilizzando il nuovo *requestPathInvalidChars* attributo del *httpRuntime* elemento di configurazione, come illustrato nell'esempio seguente:

[!code-xml[Main](overview/samples/sample11.xml)]

Per impostazione predefinita, il <em>requestPathInvalidChars</em> attributo definisce otto caratteri come non validi. (Nella stringa di cui è assegnata <em>requestPathInvalidChars</em> per impostazione predefinita<em>,</em>il minore di (&lt;), maggiore di (&gt;) e commerciale (&amp;) sono caratteri codifica, poiché il `Web.config` file è un file XML.) È possibile personalizzare il set di caratteri non validi in base alle esigenze.

> [!NOTE]
> Si noti che ASP.NET 4 rifiuta sempre i percorsi URL contenenti caratteri nell'intervallo ASCII da 0x00 a 0x1F, poiché sono caratteri dell'URL non validi come definito in RFC 2396 di IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). Nelle versioni di Windows Server che esegue IIS 6 o versioni successive, il driver di dispositivo del protocollo HTTP. sys Rifiuta automaticamente gli URL con questi caratteri.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Convalida delle richieste estensibile

Convalida della richiesta ASP.NET cerca i dati della richiesta HTTP in ingresso per le stringhe che vengono comunemente utilizzate in attacchi cross-site scripting (XSS). Se vengono rilevate eventuali stringhe XSS, la convalida delle richieste la stringa sospetta e restituisce un errore. La convalida delle richieste incorporata restituisce un errore solo quando trova le stringhe più comuni utilizzate negli attacchi XSS. Precedenti tentativi di rendere più aggressiva la convalida XSS ha restituito troppi falsi positivi. Tuttavia, i clienti potrebbe essere necessario controlli di convalida della richiesta che è molto più efficiente, o viceversa, potrebbe essere necessario intenzionalmente i XSS per pagine specifiche o per tipi specifici di richieste.

In ASP.NET 4, la funzionalità di convalida della richiesta è estensibile, in modo che è possibile utilizzare la logica di convalida delle richieste personalizzata. Per estendere la convalida della richiesta, si crea una classe che deriva dalla nuova *System.Web.Util.RequestValidator* tipo e configurare l'applicazione (nel *httpRuntime* sezione la `Web.config`file) per utilizzare il tipo personalizzato. Nell'esempio seguente viene illustrato come configurare una classe di convalida delle richieste personalizzata:

[!code-xml[Main](overview/samples/sample12.xml)]

Il nuovo *requestValidationType* attributo richiede una stringa di tipo identificatore di tipo .NET Framework standard che specifica la classe che fornisce la convalida delle richieste personalizzata. Per ogni richiesta, ASP.NET richiama il tipo personalizzato per l'elaborazione di tutti i dati della richiesta HTTP in ingresso. L'URL in ingresso, tutte le intestazioni HTTP (i cookie e intestazioni personalizzate) e il corpo dell'entità sono tutti disponibili per un controllo da una classe di convalida personalizzata richiesta simile a quello illustrato nell'esempio seguente:

[!code-csharp[Main](overview/samples/sample13.cs)]

Per i casi in cui non si desidera controllare una parte dei dati HTTP in ingresso, la classe di convalida delle richieste può eseguire il fallback per consentire l'esecuzione chiamando semplicemente la convalida della richiesta ASP.NET predefinito *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>La memorizzazione nella cache dell'oggetto e l'oggetto estensibilità di memorizzazione nella cache

Dopo la prima versione, in ASP.NET è inclusa una cache di oggetti in memoria potente (*Caching*). L'implementazione della cache è divenuta che è stata utilizzata nelle applicazioni non Web. Tuttavia, è difficile per un'applicazione Windows Form o WPF includere un riferimento a `System.Web.dll` solo per essere in grado di utilizzare la cache di oggetti ASP.NET.

Per rendere la memorizzazione nella cache disponibile per tutte le applicazioni, .NET Framework 4 introduce un nuovo assembly, un nuovo spazio dei nomi, alcuni tipi di base e una concreta la memorizzazione nella cache di implementazione. Il nuovo `System.Runtime.Caching.dll` assembly contiene una nuova API di memorizzazione nella cache nel *Caching* dello spazio dei nomi. Lo spazio dei nomi contiene due set di componenti di base delle classi:

- Tipi astratti che forniscono la base per la creazione di qualsiasi tipo di implementazione della cache personalizzata.
- Un'implementazione della cache di oggetti in memoria concreta (il *System.Runtime.Caching.MemoryCache* classe).

Il nuovo *MemoryCache* classe è modellata strettamente sulla cache di ASP.NET e la maggior parte della logica di motore di cache interna condivide con ASP.NET. Sebbene le API pubbliche di memorizzazione nella cache in *Caching* sono stati aggiornati per supportare lo sviluppo di cache personalizzate, se è stata usata ASP.NET *Cache* oggetto, si noterà concetti comuni nel nuove API.

Un'analisi approfondita del nuovo *MemoryCache* classe e l'API di base di supporto richiederebbe un intero documento. Tuttavia, nell'esempio seguente offre un'idea del funzionamento della nuova API di cache. L'esempio è stato scritto per un'applicazione Windows Forms, senza una dipendenza su `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Extensible HTML, URL e codifica dell'intestazione HTTP

In ASP.NET 4, è possibile creare una routine di codifica personalizzate per le attività di codifica di testo comuni seguenti:

- La codifica HTML.
- La codifica URL.
- Codifica dell'attributo HTML.
- Codifica delle intestazioni HTTP in uscita.

È possibile creare un codificatore personalizzato derivandolo dalla nuova *System.Web.Util.HttpEncoder* tipo e quindi la configurazione di ASP.NET per utilizzare il tipo personalizzato nel *httpRuntime* sezione la `Web.config` file, come Nell'esempio seguente:

[!code-xml[Main](overview/samples/sample15.xml)]

Dopo aver configurato un codificatore personalizzato, ASP.NET chiama automaticamente l'implementazione di codifica personalizzata ogni volta che pubblica i metodi di codifica di *System.Web.HttpUtility* o *System.Web.HttpServerUtility* le classi sono definite. Ciò consente a una parte di un team di sviluppo Web di creare un codificatore personalizzato che implementa una codifica dei caratteri aggressive, mentre il resto del team di sviluppo Web continua a utilizzare codifica le API pubbliche di ASP.NET. Configurando un codificatore personalizzato in a livello centrale le *httpRuntime* elemento, si è certi che tutte le chiamate di codifica di testo dalla codifica di API pubbliche di ASP.NET vengono indirizzate tramite il codificatore personalizzato.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitoraggio delle prestazioni per le singole applicazioni in un singolo processo di lavoro

Per aumentare il numero di siti Web che può essere ospitato in un singolo server, molti hoster eseguono più applicazioni ASP.NET in un singolo processo di lavoro. Tuttavia, se più applicazioni utilizzano un processo di lavoro condivisa singolo, è difficile per gli amministratori del server identificare una singola applicazione che si è verificati problemi.

ASP.NET 4 sfrutta la nuova funzionalità di monitoraggio delle risorse introdotta da CLR. Per abilitare questa funzionalità, è possibile aggiungere il seguente frammento di configurazione XML per il `aspnet.config` file di configurazione.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Si noti il `aspnet.config` trova nella directory di installazione di .NET Framework. Non è il `Web.config` file.


Quando il *appDomainResourceMonitoring* funzionalità è stata abilitata, due nuovi contatori delle prestazioni sono disponibili nella categoria performance "Applicazioni ASP.NET": *% tempo processore gestito* e  *Gestito memoria utilizzata*. Entrambi questi contatori delle prestazioni utilizzano la nuova funzionalità di gestione risorse di dominio dell'applicazione CLR per tenere traccia del tempo CPU stimato e utilizzo di memoria gestita delle singole applicazioni ASP.NET. Di conseguenza, con ASP.NET 4, gli amministratori hanno una visualizzazione più dettagliata nel consumo di risorse di singole applicazioni in esecuzione in un singolo processo di lavoro.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Multitargeting

È possibile creare un'applicazione che utilizza una specifica versione di .NET Framework. In ASP.NET 4, un nuovo attributo di *compilazione* elemento del `Web.config` file consente di utilizzare .NET Framework 4 e versioni successive. Se la destinazione in modo esplicito di .NET Framework 4 e, se si includono elementi facoltativi nel `Web.config` file, ad esempio le voci per *System. CodeDom*, questi elementi devono essere corretti per .NET Framework 4. (Se non in modo esplicito la destinazione è .NET Framework 4, il framework di destinazione viene derivato dalla mancanza di una voce di `Web.config` file.)

Nell'esempio seguente viene illustrato come utilizzare il *targetFramework* attributo la *compilazione* elemento di `Web.config` file.

[!code-xml[Main](overview/samples/sample17.xml)]

Si noti quanto segue sull'utilizzo di una versione specifica di .NET Framework:

- In un pool di applicazioni .NET Framework 4, il sistema di compilazione ASP.NET si presuppone di .NET Framework 4 come destinazione se il `Web.config` file non include il *targetFramework* attributo o se il `Web.config` file è mancante. (Potrebbe essere necessario apportare modifiche di codifica per l'applicazione per rendere l'esecuzione in .NET Framework 4.)
- Se si include il *targetFramework* attributo e se il *System. CodeDom* è definito l'elemento di `Web.config` file, questo file deve contenere le voci corrette per .NET Framework 4.
- Se si utilizza il *aspnet\_compilatore* comando per la precompilazione dell'applicazione (ad esempio un ambiente di compilazione), è necessario utilizzare la versione corretta del *aspnet\_compilatore* comando per il framework di destinazione. Utilizzare il compilatore fornito con .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) per compilare per .NET Framework 3.5 e versioni precedenti. Utilizzare il compilatore fornito con .NET Framework 4 per compilare le applicazioni create utilizzando tale framework o versioni successive.
- In fase di esecuzione, il compilatore utilizza assembly di framework più recente installata nel computer (e pertanto nella Global Assembly Cache). Se un aggiornamento viene eseguito in un secondo momento il framework (ad esempio ipotetica 4.1 è installata una versione), sarà possibile utilizzare le funzionalità della versione più recente del framework, anche se il *targetFramework* attributo destinato a una versione precedente (ad esempio 4.0). (Tuttavia, in fase di progettazione in Visual Studio 2010 o quando si utilizza il *aspnet\_compilatore* comando, utilizzando le funzionalità più recenti di framework causerà errori del compilatore).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery inclusi con Web Form e MVC

I modelli di Visual Studio per i controlli Web Form e MVC includono la libreria open source jQuery. Quando si crea un nuovo sito Web o un progetto, viene creata una cartella di script contenente i 3 file seguenti:

- jQuery-1.4.1. js il leggibile, minimizzata versione della libreria jQuery.
- jQuery-14.1.min.js: la versione della libreria jQuery minimizzata.
- jQuery-1.4.1-vsdoc.js: il file di documentazione di Intellisense per la libreria jQuery.

Includere la versione non minimizzata di jQuery quando si sviluppa un'applicazione. Includere la versione minimizzata di jQuery per applicazioni di produzione.

Ad esempio, la pagina Web Form seguente viene illustrato come è possibile utilizzare jQuery per modificare il colore di sfondo dei controlli casella di testo di ASP.NET su giallo quando hanno lo stato attivo.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Supporto di rete di distribuzione del contenuto

Il Microsoft Ajax rete CDN (Content Delivery) consente di aggiungere facilmente alle applicazioni Web ASP.NET Ajax e degli script jQuery. Ad esempio, iniziare a utilizzare la libreria jQuery aggiungendo semplicemente un `<script>` tag a una pagina che punta a Ajax.microsoft.com simile al seguente:

[!code-html[Main](overview/samples/sample19.html)]

È possibile sfruttare la rete CDN di Microsoft Ajax, è possibile migliorare notevolmente le prestazioni delle applicazioni Ajax. Il contenuto della rete CDN di Microsoft Ajax viene memorizzati nella cache nei server presenti in tutto il mondo. Inoltre, la rete CDN di Microsoft Ajax consente ai browser di riutilizzare i file JavaScript memorizzati nella cache per i siti Web che si trovano in domini diversi.

Microsoft Ajax Content Delivery Network supporta SSL (HTTPS), nel caso in cui sia necessario presentare una pagina web utilizzando Secure Sockets Layer.

Implementare un fallback quando la rete CDN non è disponibile. Testare il fallback.

Per ulteriori informazioni sulla rete CDN di Microsoft Ajax, visitare il seguente sito Web:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ScriptManager ASP.NET supporta la rete CDN di Microsoft Ajax. Semplicemente effettuando l'impostazione della una proprietà, la proprietà EnableCdn, è possibile recuperare tutti i file JavaScript framework ASP.NET dalla rete CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Dopo aver impostato la proprietà EnableCdn su true, il framework ASP.NET recupererà tutti i file JavaScript framework ASP.NET dalla rete CDN inclusi tutti i file JavaScript utilizzato per la convalida e l'UpdatePanel. Impostazione di questa uno proprietà può avere un impatto significativo sulle prestazioni dell'applicazione web.

È possibile impostare il percorso di rete CDN per i file JavaScript utilizzando l'attributo WebResource. La nuova proprietà CdnPath specifica il percorso della rete CDN utilizzata quando si imposta la proprietà EnableCdn su true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Script di ScriptManager esplicita

In passato, se è stato utilizzato il ScriptManger ASP.NET quindi è necessario caricare l'intera libreria Ajax di ASP.NET monolitico. È possibile sfruttare la nuova proprietà ScriptManager.AjaxFrameworkMode, è possibile controllare esattamente quali componenti della libreria ASP.NET Ajax vengono caricati e caricare solo i componenti della libreria Ajax di ASP.NET che è necessario.

La proprietà ScriptManager.AjaxFrameworkMode può essere impostata sui valori seguenti:

- Attivata: Specifica che il controllo ScriptManager include automaticamente il file di script MicrosoftAjax.js, ovvero un file di script combinato di ogni script di framework di base (comportamento legacy).
- Disattivata - Specifica che tutte le funzionalità di script Microsoft Ajax sono disabilitate e che il controllo ScriptManager non fa riferimento a tutti gli script automaticamente.
- Explicit - Specifica che verranno incluse in modo esplicito riferimenti a script al file di script singoli framework core che richiede la pagina e di includere riferimenti a tutte le dipendenze che richiede di ogni file di script.

Ad esempio, se si imposta la proprietà AjaxFrameworkMode sul valore Explicit è possibile specificare gli script particolari componente ASP.NET Ajax è necessario:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Form

Web Form è stata una funzionalità di base di ASP.NET dopo la versione di ASP.NET 1.0. Numerosi miglioramenti sono stati in quest'area per ASP.NET 4, inclusi i seguenti:

- La possibilità di impostare *meta* tag.
- Maggiore controllo sullo stato di visualizzazione.
- Modo semplice per lavorare con le funzionalità del browser.
- Supporto per il routing di ASP.NET con Web Form.
- Maggiore controllo sui ID generato.
- La possibilità di rendere persistenti le righe selezionate nei controlli di dati.
- Maggiore controllo sul codice HTML visualizzabile nel *FormView* e *ListView* controlli.
- Supporto per il filtro per i controlli origine dati.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>L'impostazione di tag Meta Page.MetaKeywords e le proprietà Page.MetaDescription

In ASP.NET 4 aggiunge due proprietà per il *pagina* (classe), *MetaKeywords* e *MetaDescription*. Queste due proprietà rappresentano corrispondente *meta* tag della pagina, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Queste due proprietà funzionano allo stesso modo modo in cui la pagina *titolo* proprietà. Le funzioni seguono queste regole:

1. Se sono presenti alcun *meta* tag il *head* elemento che corrisponde ai nomi delle proprietà (, ovvero nome = "parole chiave" per *Page.MetaKeywords* e il nome = "description" per  *Page.MetaDescription*, che significa che non sono state impostate queste proprietà), il *meta* tag verranno aggiunto alla pagina quando ne viene eseguito il rendering.
2. Se sono già presenti *meta* tag con questi nomi, queste proprietà fungono da get e set di metodi per il contenuto del tag esistenti.

È possibile impostare queste proprietà in fase di esecuzione, che consente di ottenere il contenuto da un database o altre origini, e che consente di impostare i tag in modo dinamico per descrivere le operazioni è una pagina particolare.

È inoltre possibile impostare il *parole chiave* e *descrizione* le proprietà di *@ Page* direttiva all'inizio del markup della pagina Web Form, come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Questo percorso sostituirà il *meta* tag già dichiarato nella pagina contenuto (se presente).

Il contenuto della descrizione *meta* tag vengono utilizzati per migliorare la ricerca di elencare le anteprime di Google. (Per informazioni dettagliate, vedere [migliorare i frammenti di codice con una trasformazione di descrizione meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) nel blog Google Webmaster centrale.) Google e ricerca di Windows Live non utilizzano il contenuto delle parole chiave per qualsiasi elemento, ma potrebbero altri motori di ricerca. Per ulteriori informazioni, vedere [Meta parole chiave consigli](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) nel sito Web di Guida del motore di ricerca.

Le nuove proprietà sono una funzione semplice, ma salvano dal requisito di aggiungerle manualmente o da codice personalizzato per creare il *meta* tag.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Attivazione dello stato di visualizzazione per i singoli controlli

Per impostazione predefinita, lo stato di visualizzazione è abilitato per la pagina, in modo che ogni controllo nella pagina Archivia lo stato di visualizzazione potenzialmente anche se non è necessaria per l'applicazione. Dati dello stato di visualizzazione sono incluso nel markup che una pagina viene generato l'errore e aumenta la quantità di tempo impiegato per l'invio di una pagina per il client e registrarlo nuovamente. L'archiviazione dello stato di visualizzazione più del necessario può causare un peggioramento delle prestazioni. Nelle versioni precedenti di ASP.NET, gli sviluppatori è stato possibile disabilitare lo stato di visualizzazione per i singoli controlli per ridurre le dimensioni di pagina, ma ha operazioni da eseguire in modo esplicito per singoli controlli. In ASP.NET 4, i controlli server Web includono un *ViewStateMode* proprietà che consente di disabilitare lo stato di visualizzazione per impostazione predefinita e quindi abilitarlo solo per i controlli che lo richiedono nella pagina.

Il *ViewStateMode* proprietà accetta un'enumerazione che dispone di tre valori: *abilitato*, *disabilitato*, e *eredita*. *Abilitata* consente di visualizzare lo stato per il controllo e per tutti i controlli figlio che vengono impostati su *eredita* o che non è nothing impostato. *Disabilitato* disattiva stato di visualizzazione, e *eredita* specifica che il controllo Usa il *ViewStateMode* impostazione dal controllo padre.

L'esempio seguente mostra come *ViewStateMode* proprietà funziona. Il markup e il codice per i controlli nella pagina seguente include i valori per il *ViewStateMode* proprietà:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Come si può notare, il codice disabilita lo stato di visualizzazione per il controllo PlaceHolder1. Il controllo di label1 figlio eredita il valore della proprietà (*eredita* è il valore predefinito per *ViewStateMode* per i controlli.) e quindi Salva nessuno stato di visualizzazione. Nel controllo PlaceHolder2 *ViewStateMode* è impostato su *abilitato*, in modo che questa proprietà viene ereditata label2 e Salva lo stato di visualizzazione. Quando la pagina viene caricata inizialmente, il *testo* proprietà sia *etichetta* controlli è impostato sulla stringa "[DynamicValue]".

L'effetto di queste impostazioni è che, quando la pagina viene caricata la prima volta, nel browser viene visualizzato il seguente output:

Disabilitato `: [DynamicValue]`

Abilitata:`[DynamicValue]`

Dopo un postback, tuttavia, viene visualizzato il seguente output:

Disabilitato `: [DeclaredValue]`

Abilitata:`[DynamicValue]`

Il controllo label1 (il cui *ViewStateMode* è impostato su *disabilitato*) non è mantenuto il valore su cui è stata impostata nel codice. Tuttavia, di controllare il label2 (il cui *ViewStateMode* è impostato su *abilitato*) ha mantenuto lo stato.

È inoltre possibile impostare *ViewStateMode* nel *@ Page* direttiva, come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample26.aspx)]

Il *pagina* classe è solo un altro controllo, funge da controllo padre per tutti gli altri controlli nella pagina. Il valore predefinito di *ViewStateMode* è *abilitato* per le istanze di *pagina*. Poiché per impostazione predefinita i controlli *eredita*, controlli erediteranno il *abilitato* valore della proprietà, a meno che non si imposta *ViewStateMode* a livello di pagina o controllo.

Il valore di *ViewStateMode* proprietà determina se lo stato di visualizzazione viene mantenuto solo se il *EnableViewState* è impostata su *true*. Se il *EnableViewState* è impostata su *false*, lo stato di visualizzazione non verrà mantenuto anche se *ViewStateMode* è impostato su *abilitato*.

È consigliabile utilizzare questa funzionalità con *ContentPlaceHolder* controlli nelle pagine master, in cui è possibile impostare *ViewStateMode* a *disabilitato* per lo schema di pagina e quindi abilitarlo singolarmente per *ContentPlaceHolder* i controlli che a loro volta contengono i controlli che richiedono lo stato di visualizzazione.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Modifiche alle funzionalità del Browser

ASP.NET determina le funzionalità del browser che un utente sta utilizzando per esplorare il sito tramite una funzionalità denominata *funzionalità del browser*. Funzionalità del browser sono rappresentate dal *HttpBrowserCapabilities* oggetto (esposti dal *Request* proprietà). Ad esempio, è possibile utilizzare il *HttpBrowserCapabilities* oggetto per determinare se il tipo e la versione del browser corrente supporta una particolare versione di JavaScript. In alternativa, utilizzare il *HttpBrowserCapabilities* oggetto per determinare se la richiesta proviene da un dispositivo mobile.

Il *HttpBrowserCapabilities* oggetto dipende da un set di file di definizione del browser. Questi file contengono informazioni sulle funzionalità del browser specifico. In ASP.NET 4, questi file di definizione del browser sono stati aggiornati per contenere informazioni sui browser introdotte di recente e i dispositivi, ad esempio Google Chrome, ricerca di smartphone BlackBerry di movimento e Apple iPhone.

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

#### <a name="using-browser-capabilities-providers"></a>Utilizzo di provider di funzionalità del Browser

In ASP.NET versione 3.5 Service Pack 1, è possibile definire le funzionalità di un browser con nei modi seguenti:

- A livello di computer, crea o aggiorna un `.browser` file XML nella cartella seguente:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Dopo aver definito una funzionalità del browser, eseguire il comando seguente da di Visual Studio Command Prompt per ricompilare l'assembly con funzionalità browser e aggiungerlo alla Global Assembly Cache:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Per una singola applicazione, si crea un `.browser` file dell'applicazione `App_Browsers` cartella.

Questi approcci è necessario modificare il file XML e per le modifiche a livello di computer, è necessario riavviare l'applicazione dopo aver eseguito aspnet\_regbrowsers.exe processo.

ASP.NET 4 include una funzionalità detta *i provider di funzionalità browser*. Come suggerisce il nome, questo consente di creare un provider che a sua volta consente di utilizzano il proprio codice per determinare le funzionalità del browser.

In pratica, gli sviluppatori spesso non definiscono le funzionalità del browser personalizzato. I file del browser sono difficile da aggiornare, il processo per aggiornarli è abbastanza complesso e la sintassi XML per `.browser` possibile complessi da utilizzare e definire i file. Cosa sarebbe semplificare questo processo è se ci fosse una comune sintassi di definizione del browser o un database che contiene le definizioni aggiornate del browser o anche un servizio Web per tale database. La nuova funzionalità di provider di funzionalità browser rende questi scenari possibili e per gli sviluppatori di terze parti.

Esistono due approcci principali per l'uso della nuova funzionalità di provider di funzionalità del browser ASP.NET 4: estendere le funzionalità di definizione di funzionalità del browser ASP.NET o completamente sostituirlo. Nelle sezioni seguenti descrivono prima procedura sostituire la funzionalità e come estenderlo.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Sostituisce la funzionalità di funzionalità del Browser ASP.NET

Per sostituire la funzionalità di definizione funzionalità ASP.NET browser completamente, seguire questi passaggi:

1. Creare una classe provider che deriva da *HttpCapabilitiesProvider* e che esegue l'override di *GetBrowserCapabilities* (metodo), come nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Il codice in questo esempio crea un nuovo *HttpBrowserCapabilities* oggetto, specificare solo la funzionalità denominata browser e l'impostazione di questa funzionalità per MyCustomBrowser.
2. Registrare il provider con l'applicazione. 

    Per utilizzare un provider con un'applicazione, è necessario aggiungere il *provider* attributo la *browserCaps* sezione la `Web.config` o `Machine.config` file. (È anche possibile definire gli attributi di provider in un *percorso* elemento per le directory specifiche nell'applicazione, ad esempio in una cartella per un dispositivo mobile specifico.) Nell'esempio seguente viene illustrato come impostare il *provider* attributo in un file di configurazione:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Un altro modo per registrare la nuova definizione di funzionalità browser è usare il codice, come illustrato nell'esempio seguente:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Questo codice deve essere eseguito *applicazione\_avviare* evento del `Global.asax` file. Qualsiasi modifica di *BrowserCapabilitiesProvider* classe deve verificarsi prima che qualsiasi codice nell'applicazione viene eseguita, per assicurarsi che la cache rimane in uno stato valido per l'oggetto risolto *HttpCapabilitiesBase* oggetto.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>La memorizzazione nella cache l'oggetto HttpBrowserCapabilities

Nell'esempio precedente è presente un problema, ovvero che il codice verrà eseguito ogni volta che viene richiamato il provider personalizzato per ottenere il *HttpBrowserCapabilities* oggetto. Ciò può verificarsi più volte durante ogni richiesta. Nell'esempio di codice per il provider non serve a molto. Tuttavia, se il codice nel provider personalizzato esegue il lavoro significativo in ordine per ottenere il *HttpBrowserCapabilities* dell'oggetto, questo può influire sulle prestazioni. Per evitare questa situazione, è possibile memorizzare nella cache il *HttpBrowserCapabilities* oggetto. Attenersi ai passaggi riportati di seguito.

1. Creare una classe che deriva da *HttpCapabilitiesProvider*, come quello nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Nell'esempio, il codice genera una chiave di cache chiamando un metodo BuildCacheKey personalizzato e ottiene l'intervallo di tempo da memorizzare nella cache chiamando un metodo GetCacheTime personalizzato. Il codice aggiunge quindi l'oggetto risolto *HttpBrowserCapabilities* oggetto alla cache. L'oggetto può essere recuperato dalla cache e riutilizzato in richieste successive che utilizzano il provider personalizzato.
2. Registrare il provider con l'applicazione, come descritto nella procedura precedente.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Estensione delle funzionalità di funzionalità del Browser ASP.NET

La precedente sezione viene descritto come creare un nuovo *HttpBrowserCapabilities* oggetto in ASP.NET 4. È anche possibile estendere la funzionalità di funzionalità del browser ASP.NET tramite l'aggiunta di nuove definizioni di funzionalità del browser a quelli che sono già in ASP.NET. È possibile farlo senza utilizzare le definizioni di browser XML. Nella seguente procedura come.

1. Creare una classe che deriva da *HttpCapabilitiesEvaluator* e che esegue l'override di *GetBrowserCapabilities* (metodo), come illustrato nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Questo codice Usa innanzitutto la funzionalità di funzionalità del browser ASP.NET per tentare di identificare il browser. Tuttavia, se non viene identificato browser sulla base delle informazioni definite nella richiesta di (ovvero, se il *Browser* proprietà del *HttpBrowserCapabilities* oggetto è la stringa "Unknown"), il codice chiama il provider personalizzato (MyBrowserCapabilitiesEvaluator) per identificare il browser.
2. Registrare il provider con l'applicazione, come descritto nell'esempio precedente.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Estensione delle funzionalità di funzionalità del Browser mediante l'aggiunta di nuove funzionalità per le definizioni di funzionalità esistenti

Oltre a creare un provider di definizione del browser personalizzato e la creazione dinamica di nuove definizioni del browser, è possibile estendere le definizioni esistenti del browser con funzionalità aggiuntive. Ciò consente di utilizzare una definizione che si desidera ma non dispone solo di alcune funzionalità. A tale scopo, attenersi alla procedura seguente.

1. Creare una classe che deriva da *HttpCapabilitiesEvaluator* e che esegue l'override di *GetBrowserCapabilities* (metodo), come illustrato nell'esempio seguente: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Nell'esempio di codice estende ASP.NET esistente *HttpCapabilitiesEvaluator* classe e ottiene il *HttpBrowserCapabilities* oggetto che corrisponde alla definizione della richiesta corrente con il codice seguente :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Il codice può quindi aggiungere o modificare una funzione per il browser. Esistono due modi per specificare una nuova funzionalità del browser:

    - Aggiungere una coppia chiave/valore per il *IDictionary* oggetto esposto dal *funzionalità* proprietà del *HttpCapabilitiesBase* oggetto. Nell'esempio precedente, il codice aggiunge una funzionalità denominata multitocco con un valore di *true*.
    - Impostare le proprietà esistenti del *HttpCapabilitiesBase* oggetto. Nell'esempio precedente, il codice imposta il *frame* proprietà *true*. Questa proprietà è semplicemente una funzione di accesso per il *IDictionary* oggetto esposto dal *funzionalità* proprietà. 

        > [!NOTE]
        > Questo modello si applica a qualsiasi proprietà di *HttpBrowserCapabilities*, tra cui gli adattatori di controllo.
2. Registrare il provider con l'applicazione, come descritto nella procedura precedente.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routing in ASP.NET 4

In ASP.NET 4 aggiunge il supporto incorporato per la distribuzione di Web Form. URL che non fanno riferimento a file fisici di richiesta di routing consente di che configurare un'applicazione ad accettare. In alternativa, è possibile utilizzare il routing per definire gli URL che sono significativi per gli utenti e che possono aiutare a con l'ottimizzazione motore di ricerca (SEO) per l'applicazione. L'URL per una pagina che visualizza le categorie di prodotto in un'applicazione esistente, ad esempio, potrebbe essere simile al seguente:

[!code-console[Main](overview/samples/sample36.cmd)]

Tramite il routing, è possibile configurare l'applicazione per accettare l'URL seguente per eseguire il rendering le stesse informazioni:

[!code-console[Main](overview/samples/sample37.cmd)]

Routing è stato disponibile a partire da ASP.NET 3.5 SP1. (Per un esempio di come utilizzare il routing ASP.NET 3.5 SP1, vedere il post [utilizzando Routing con WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "titolo di questa voce.") nel blog di Phil Haack.) Tuttavia, ASP.NET 4 include alcune funzionalità che rendono più semplice di utilizzare il routing, inclusi i seguenti:

- Il *PageRouteHandler* (classe), che è un semplice gestore HTTP utilizzato per definire le route. La classe passa i dati per la pagina che viene inoltrata la richiesta.
- Le nuove proprietà *HttpRequest.RequestContext* e *Page.RouteData* (ovvero un proxy per il *HttpRequest.RequestContext.RouteData* oggetto). Tali proprietà semplificano per accedere alle informazioni che viene passato dalla route.
- I seguente nuovo generatori di espressioni, che sono definiti in *System.Web.Compilation.RouteUrlExpressionBuilder* e *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, che fornisce un modo semplice per creare un URL che corrisponde a un URL di route all'interno di un controllo server ASP.NET.
- *RouteValue*, che fornisce un modo semplice per estrarre informazioni dal *RouteContext* oggetto.
- Il *RouteParameter* (classe), che rende più semplice passare i dati contenuti in un *RouteContext* oggetto a una query per un controllo origine dati (simile a [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routing per le pagine Web Form

Nell'esempio seguente viene illustrato come definire una route di Web Form utilizzando il nuovo *MapPageRoute* metodo il *Route* classe:

[!code-csharp[Main](overview/samples/sample38.cs)]

In ASP.NET 4 introduce le *MapPageRoute* metodo. Nell'esempio seguente è equivalente alla definizione SearchRoute illustrata nell'esempio precedente, ma usa il *PageRouteHandler* classe.

[!code-csharp[Main](overview/samples/sample39.cs)]

Il codice nell'esempio viene eseguito il mapping della route per una pagina fisica (nella route, prima di `~/search.aspx`). La prima definizione route specifica inoltre che il parametro denominato searchterm da estrarre dall'URL e passato alla pagina.

Il *MapPageRoute* metodo supporta l'overload del metodo seguente:

- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults)*
- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults, RouteValueDictionary constraints)*

Il *checkPhysicalUrlAccess* parametro specifica se la route deve verificare le autorizzazioni di sicurezza per la pagina fisica viene eseguito il routing (in questo caso, Search. aspx) e le autorizzazioni per l'URL in ingresso (in questo caso, eseguire la ricerca / {searchterm}). Se il valore di *checkPhysicalUrlAccess* è *false*, verranno verificate solo le autorizzazioni dell'URL in ingresso. Queste autorizzazioni sono definite nel `Web.config` file utilizzando le impostazioni, ad esempio le operazioni seguenti:

[!code-xml[Main](overview/samples/sample40.xml)]

Nella configurazione di esempio, accesso negato alla pagina fisica `search.aspx` per tutti gli utenti tranne quelli che appartengono al ruolo di amministratore. Quando il *checkPhysicalUrlAccess* parametro è impostato su *true* (ovvero il valore predefinito), solo gli utenti amministratori possono accedere /search/ l'URL {searchterm}, perché è la pagina fisica Search. aspx limitato agli utenti in tale ruolo. Se *checkPhysicalUrlAccess* è impostato su *false* e il sito è configurato come illustrato nell'esempio precedente, tutti gli utenti autenticati possono accedere /search/ l'URL {searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Lettura di informazioni di Routing in una pagina Web Form

Nel codice della pagina fisica Web Form, è possibile accedere alle informazioni di routing ha estratto dall'URL (o altre informazioni che è aggiunto un altro oggetto di *RouteData* oggetto) utilizzando due nuove proprietà:  *HttpRequest.RequestContext* e *Page.RouteData*. (*Page.RouteData* wraps *HttpRequest.RequestContext.RouteData*.) Nell'esempio seguente viene illustrato come utilizzare *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Il codice estrae il valore passato per il parametro searchterm, come definito nella route riportato in precedenza. Si consideri il seguente URL di richiesta:

[!code-console[Main](overview/samples/sample42.cmd)]

Quando viene effettuata la richiesta, la parola "scott" verrebbe eseguito nel `search.aspx` pagina.

#### <a name="accessing-routing-information-in-markup"></a>L'accesso alle informazioni di Routing nel Markup

Il metodo descritto nella sezione precedente viene illustrato come ottenere i dati della route nel codice in una pagina Web Form. È inoltre possibile utilizzare espressioni che consentono di accedere alle stesse informazioni nel markup. Generatori di espressioni sono un modo elegante e potente per lavorare con il codice dichiarativo. (Per ulteriori informazioni, vedere la voce [Express manualmente con personalizzato generatori di espressioni](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) sul blog di Phil Haack.)

In ASP.NET 4 include due nuovi generatori di espressioni per il routing di Web Form. Nell'esempio seguente viene illustrato come utilizzarle.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Nell'esempio di *RouteUrl* espressione viene utilizzata per definire un URL basato su un parametro di route. Questo evita di dover codificare l'URL completo nel markup e consente di modificare la struttura dell'URL in un secondo momento, senza richiedere modifiche per questo collegamento.

In base alla route definita in precedenza, questo codice genera l'URL seguente:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET funziona automaticamente le route corretta (ovvero, genera l'URL corretto) in base ai parametri di input. È anche possibile includere un nome di route nell'espressione, che consente di specificare una route da utilizzare.

Nell'esempio seguente viene illustrato come utilizzare il *RouteValue* espressione.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Quando la pagina che contiene il controllo viene eseguito, il valore "scott" viene visualizzato nell'etichetta.

Il *RouteValue* espressione rende più semplice per utilizzare dati di route nel markup, ed evita la necessità di lavorare con il più complesso Page.RouteData["x"] sintassi nel markup.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Utilizzando i dati di Route per i parametri di controllo origine dati

Il *RouteParameter* classe consente di specificare i dati della route come valore di parametro per le query in un controllo origine dati. Si [opera analogamente la](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) classe, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample46.aspx)]

In questo caso, verrà utilizzato il valore di searchterm di parametro di route per il @companyname parametro il <em>selezionare</em> istruzione.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Impostazione degli ID Client

Il nuovo *ClientIDMode* proprietà risolve un problema da molto tempo in ASP.NET, vale a dire come creano i controlli di *id* attributo per gli elementi che eseguono il rendering. Conoscere il *id* attributo per gli elementi di rendering è importante se l'applicazione include uno script di client che fa riferimento a questi elementi.

Il *id* attributo in formato HTML che viene eseguito il rendering dei controlli server Web viene generato sulla base di *ClientID* proprietà del controllo. Fino a quando ASP.NET 4, l'algoritmo per la generazione di *id* dall'attributo di *ClientID* proprietà è stata per concatenare il contenitore di denominazione (se presente), con l'ID e, nel caso di controlli ripetuti (come in controlli di dati), per aggiungere un prefisso e un numero sequenziale. Mentre questo è sempre garantito che gli ID dei controlli nella pagina siano univoci, l'algoritmo ha comportato ID che non erano stimabile e sono stati pertanto difficile da riferimento negli script client di controllo.

Il nuovo *ClientIDMode* proprietà consente di specificare il modo più precisamente l'ID client viene generato per i controlli. È possibile impostare il *ClientIDMode* proprietà per qualsiasi controllo, tra cui la pagina. Impostazioni possibili sono i seguenti:

- *AutoID* : questa opzione equivale all'algoritmo per la generazione *ClientID* i valori delle proprietà che è stato usato nelle versioni precedenti di ASP.NET.
- *Static* : Specifica che il *ClientID* valore sarà lo stesso ID senza concatenazione gli ID di denominazione dei contenitori padre. Può essere utile nei controlli utente Web. Poiché un controllo utente Web può trovarsi in diverse pagine e controlli contenitore diverso, può essere difficile la scrittura di script client per i controlli che utilizzano il *AutoID* algoritmo perché non è possibile prevedere i valori di ID sarà .
- *Stimabile* : questa opzione viene principalmente utilizzato nei controlli di dati che utilizzano modelli ripetuti. Concatena le proprietà ID di contenitori di denominazione del controllo, ma generato *ClientID* valori non contengono stringhe come "ctlxxx". Questa impostazione interagisce con il *ClientIDRowSuffix* proprietà del controllo. Impostare il *ClientIDRowSuffix* proprietà per il nome di un campo dati e il valore del campo utilizzato come suffisso per generato *ClientID* valore. In genere si usa la chiave primaria di un record di dati come il *ClientIDRowSuffix* valore.
- *Ereditare* : questa impostazione è il comportamento predefinito per i controlli, ovvero viene specificato che la generazione degli ID di un controllo corrisponde a quello del padre.

È possibile impostare il *ClientIDMode* proprietà a livello di pagina. Definisce il valore predefinito *ClientIDMode* valore per tutti i controlli nella pagina corrente.

Il valore predefinito *ClientIDMode* valore a livello di pagina è *AutoID*e il valore predefinito *ClientIDMode* valore a livello di controllo è *eredita*. Di conseguenza, se non si imposta questa proprietà in qualsiasi punto nel codice, tutti i controlli utilizzerà il *AutoID* algoritmo.

Impostare il valore a livello di pagina *@ Page* direttiva, come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample47.aspx)]

È inoltre possibile impostare il *ClientIDMode* valore nel file di configurazione a livello di computer (computer) o a livello di applicazione. Definisce il valore predefinito *ClientIDMode* impostazione per tutti i controlli in tutte le pagine dell'applicazione. Se si imposta il valore a livello di computer, che definisce il valore predefinito *ClientIDMode* impostazione per tutti i siti Web nel computer. Nell'esempio seguente il *ClientIDMode* impostazione nel file di configurazione:

[!code-xml[Main](overview/samples/sample48.xml)]

Come accennato in precedenza, il valore della *ClientID* proprietà viene derivata il contenitore di denominazione per padre un controllo. In alcuni scenari, ad esempio quando si utilizzano pagine master, i controlli possono finire con ID simili a quelli seguenti il rendering HTML:

[!code-html[Main](overview/samples/sample49.html)]

Anche se il *input* elemento mostrato nel codice (da un *TextBox* controllo) è solo due contenitori di denominazione nella pagina (nidificata *ContentPlaceholder* controlli), a causa della modalità di elaborazione delle pagine master, il risultato finale è un ID di controllo simile al seguente:

[!code-console[Main](overview/samples/sample50.cmd)]

Questo ID è garantito l'univocità nella pagina, ma è inutilmente tempo per la maggior parte dei casi. Si supponga per ridurre la lunghezza dell'ID di rendering e per avere maggiore controllo sulla modalità con cui l'ID viene generato. (Ad esempio, è necessario eliminare i prefissi "ctlxxx"). Il modo più semplice per ottenere questo risultato consiste nell'impostare il *ClientIDMode* proprietà come illustrato nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample51.aspx)]

In questo esempio, il *ClientIDMode* è impostata su *statico* per più esterna *NamingPanel* elemento e impostato su *stimabile* per il parametro *NamingControl* elemento. Queste impostazioni determinano il markup seguente (il resto della pagina e la pagina master si presuppone che sia lo stesso come nell'esempio precedente):

[!code-html[Main](overview/samples/sample52.html)]

Il *statico* impostazione abbia l'effetto di reimpostare la gerarchia di denominazione per tutti i controlli all'interno di più esterna *NamingPanel* elemento e di eliminare il *ContentPlaceHolder* e *MasterPage* gli ID l'ID generato. (Il *nome* attributo degli elementi viene eseguito il rendering non è interessato, pertanto il normale funzionamento ASP.NET viene mantenuto per gli eventi, lo stato di visualizzazione e così via.) È un effetto collaterale di reimpostare la gerarchia di denominazione che anche se si sposta il markup per il *NamingPanel* elementi a un altro *ContentPlaceholder* l'ID client viene eseguito il rendering di controllo, rimangono invariati.

> [!NOTE]
> Si noti che è responsabilità dell'utente per assicurarsi che gli ID di controllo di rendering sono univoci. Se non lo sono, è possibile interrompere qualsiasi funzionalità che richiedono un ID univoco per i singoli elementi HTML, ad esempio, il client *Document. getElementById* (funzione).


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Creazione di ID Client stimabile nei controlli con associazione a dati

Il *ClientID* valori generati per i controlli in un controllo elenco associato ai dati dall'algoritmo legacy possono essere lungo e non sono effettivamente prevedibili. Il *ClientIDMode* funzionalità consente di avere più controllo come questi ID vengono generati.

Il codice nell'esempio seguente include un *ListView* controllo:

[!code-aspx[Main](overview/samples/sample53.aspx)]

Nell'esempio precedente, il *ClientIDMode* e *RowClientIDRowSuffix* proprietà vengono impostate nel markup. Il *ClientIDRowSuffix* proprietà può essere utilizzata solo nei controlli associati a dati e il relativo comportamento varia a seconda di quale controllo in uso. Esistono le seguenti differenze:

- *GridView* controllo, è possibile specificare il nome di una o più colonne nell'origine dati, che vengono combinati in fase di esecuzione per creare il client ID. Ad esempio, se si imposta *RowClientIDRowSuffix* per "ProductName, ProductId", ID di controllo per gli elementi di rendering avranno un formato simile al seguente:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* controllo, è possibile specificare una singola colonna nell'origine dati che viene aggiunto per l'ID client. Ad esempio, se si imposta *ClientIDRowSuffix* per "ProductName", il controllo viene eseguito il rendering ID avrà un formato simile al seguente:

- [!code-console[Main](overview/samples/sample55.cmd)]

- In questo caso l'1 finale deriva dall'ID di prodotto dell'elemento di dati corrente.

- *Ripetitore* controllo, questo controllo non supporta il *ClientIDRowSuffix* proprietà. In un *Ripetitore* controllo, l'indice della riga corrente viene utilizzato. Quando si utilizza ClientIDMode = "Stimabile" con un *Ripetitore* controllare, i client vengono generati ID con il formato seguente:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Il valore 0 finale è l'indice della riga corrente.

Il *FormView* e *DetailsView* controlli non visualizzano più righe, in modo che non supportano il *ClientIDRowSuffix* proprietà.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Selezione di righe persistenti nei controlli dati

Il *GridView* e *ListView* controlli possono consentire agli utenti di selezionare una riga. Nelle versioni precedenti di ASP.NET, selezione basata sull'indice di riga nella pagina. Ad esempio, se si seleziona il terzo elemento nella pagina 1 e quindi passare alla pagina 2, è selezionato il terzo elemento nella pagina.

La selezione persistente è stata inizialmente supportata solo nei progetti di dati dinamica in .NET Framework 3.5 SP1. Quando questa funzionalità è abilitata, l'elemento attualmente selezionato è in base alla chiave di dati per l'elemento. Ciò significa che se si seleziona la terza riga nella pagina 1 e passare alla pagina 2, viene selezionato nulla nella pagina 2. Quando si torna alla pagina 1, la terza riga sia ancora selezionata. Selezione persistente è ora supportata per il *GridView* e *ListView* controlli in tutti i progetti utilizzando il *EnablePersistedSelection* proprietà, come illustrato di esempio:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Controllo ASP.NET Chart

ASP.NET *grafico* controllo espande le offerte di visualizzazione dei dati in .NET Framework. Utilizzo di *grafico* controllo, è possibile creare pagine ASP.NET che sono grafici intuitivi e visivamente accattivanti per eseguire complesse analisi statistica e finanziaria. ASP.NET *grafico* controllo è stato introdotto come componente aggiuntivo per la versione di .NET Framework versione 3.5 SP1 ed è parte della versione di .NET Framework 4.

Il controllo include le funzionalità seguenti:

- 35 tipi diversi di grafici.
- Un numero illimitato di aree del grafico, titoli, legende e annotazioni.
- Un'ampia gamma di impostazioni dell'aspetto di tutti gli elementi del grafico.
- Supporto per la maggior parte dei tipi di grafico 3D.
- Etichette dati intelligenti che è possono adattare automaticamente intorno punti dati.
- Le strisce, cambi di scala e la scala logaritmica.
- Oltre 50 formule statistiche e finanziarie per analisi e trasformazione dei dati.
- Associazione semplice e la manipolazione di dati del grafico.
- Supporto per formati di dati comuni, ad esempio data, ora e valuta.
- Supporto per l'interattività e personalizzazione basata sugli eventi, tra cui il client scegliere eventi di utilizzo di Ajax.
- Gestione dello stato.
- Flusso binario.

Le figure seguenti mostrano esempi di finanziari grafici generati dal controllo ASP.NET Chart.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Figura 2: Esempi di controllo grafico di ASP.NET

Per ulteriori esempi di utilizzo del controllo ASP.NET Chart, scaricare il codice di esempio nella [ambiente di esempio per i controlli Chart Microsoft](https://go.microsoft.com/fwlink/?LinkId=128300) pagina nel sito Web MSDN. È possibile trovare contenuto in altri esempi della community di [forum sui controlli Chart](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Aggiunta del controllo grafico di una pagina ASP.NET

Nell'esempio seguente viene illustrato come aggiungere un *grafico* controllo a una pagina ASP.NET utilizzando il markup. Nell'esempio di *grafico* controllo produce un istogramma per i punti dati statici.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Utilizzo dei grafici 3D

Il *grafico* controllo contiene un *ChartAreas* insieme, che può contenere *ChartArea* gli oggetti che definiscono le caratteristiche di aree del grafico. Ad esempio, per utilizzare 3D per un'area del grafico, utilizzare il *Area3DStyle* proprietà come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Nella figura seguente mostra un grafico 3D con quattro serie del *barra* tipo di grafico.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figura 3: grafico a barre 3D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Utilizzo di cambi di scala e scale logaritmiche

Cambi di scala e scale logaritmiche sono altri due modi per aggiungere complessità al grafico. Queste funzionalità sono specifiche per ogni asse in un'area grafico. Ad esempio, per utilizzare queste funzionalità sull'asse Y primario di un'area grafico, utilizzare il *AxisY.IsLogarithmic* e *ScaleBreakStyle* proprietà in un *ChartArea* oggetto. Il frammento di codice seguente viene illustrato come utilizzare i cambi di scala sull'asse Y primario.

[!code-aspx[Main](overview/samples/sample60.aspx)]

La figura seguente mostra l'asse Y con cambi di scala abilitati.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figura 4: Cambi di scala

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtro dei dati con il controllo QueryExtender

Un'attività molto comune per gli sviluppatori che creano pagine Web basate su dati consiste nel filtrare i dati. Ciò in genere è stata eseguita compilando *dove* controlli origine clausole nei dati. Questo approccio può essere complicato e in alcuni casi il *dove* sintassi non è possibile sfruttare la funzionalità completa del database sottostante.

Per rendere le operazioni di filtro, un nuovo *QueryExtender* controllo è stato aggiunto in ASP.NET 4. Questo controllo può essere aggiunto a *EntityDataSource* o *LinqDataSource* controlli per filtrare i dati restituiti da questi controlli. Poiché il *QueryExtender* controllo si basa su LINQ, il filtro viene applicato nel server di database prima che i dati vengono inviati alla pagina, che comporta operazioni molto efficiente.

Il *QueryExtender* controllo supporta un'ampia gamma di opzioni di filtro. Le sezioni seguenti vengono descritte queste opzioni e vengono forniti esempi di come utilizzarli.

#### <a name="search"></a>Cerca

Per l'opzione di ricerca, il *QueryExtender* controllo esegue una ricerca nei campi specificati. Nell'esempio seguente viene utilizzato il testo immesso nel controllo TextBoxSearch e si cerca il relativo contenuto nel `ProductName` e `Supplier.CompanyName` colonne nei dati restituiti dal *LinqDataSource* controllo.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Intervallo

L'opzione intervallo è simile all'opzione di ricerca, ma specifica una coppia di valori per definire l'intervallo. Nell'esempio seguente, il *QueryExtender* controllano le ricerche di `UnitPrice` colonna nei dati restituiti dal *LinqDataSource* controllo. L'intervallo è di lettura dai controlli TextBoxFrom e TextBoxTo nella pagina.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

L'opzione di espressione di proprietà consente di definire un confronto per un valore della proprietà. Se l'espressione restituisce *true*, vengono restituiti i dati che sono in corso l'analisi. Nell'esempio seguente, il *QueryExtender* controllo Filtra i dati in base ai dati nel `Discontinued` colonna sul valore dal controllo CheckBoxDiscontinued nella pagina.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>Espressione personalizzata

Infine, è possibile specificare un'espressione personalizzata da utilizzare con il *QueryExtender* controllo. Questa opzione consente di chiamare una funzione nella pagina che definisce la logica di filtro personalizzato. Nell'esempio seguente viene illustrato come specificare in modo dichiarativo un'espressione personalizzata nel *QueryExtender* controllo.

[!code-aspx[Main](overview/samples/sample64.aspx)]

Nell'esempio seguente viene illustrata la funzione personalizzata che viene richiamata dal *QueryExtender* controllo. In questo caso, anziché utilizzare una query di database che include un *dove* clausola, il codice Usa una query LINQ per filtrare i dati.

[!code-csharp[Main](overview/samples/sample65.cs)]

Questi esempi mostrano solo un'espressione utilizzata nel *QueryExtender* controllo alla volta. Tuttavia, è possibile includere più espressioni all'interno di *QueryExtender* controllo.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Le espressioni di codice codificata in formato HTML

Alcuni siti ASP.NET (in particolare con ASP.NET MVC) si basano sull'utilizzo di `<%` =  `expression %>` sintassi (spesso chiamata "un elevato numero di code") per scrivere un testo nella risposta. Quando si usano le espressioni di codice, è facile dimenticare di codifica HTML input di testo, se il testo fornito dall'utente, è possibile lasciare aperta pagine a un attacco XSS (Cross-Site Scripting).

In ASP.NET 4 introduce la nuova sintassi per le espressioni di codice seguente:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Questa sintassi utilizza la codifica HTML per impostazione predefinita durante la scrittura nella risposta. Questa nuova espressione viene convertita in modo efficace per le operazioni seguenti:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Ad esempio, &lt;%: richiesta ["UserInput"] %&gt; esegue la codifica HTML al valore di *richiesta ["UserInput"]*.

L'obiettivo di questa funzionalità è consentono di sostituire tutte le istanze di sintassi precedente con la nuova sintassi in modo che non è necessario decidere in ogni fase quello da utilizzare. Tuttavia, vi sono casi in cui il testo come output deve essere in formato HTML o è già stato codificato, nel qual caso può portare a doppia codifica.

Per i casi, ASP.NET 4 introduce una nuova interfaccia *IHtmlString*, insieme a un'implementazione concreta, *HtmlString*. Istanze di questi tipi consentono di indicare che il valore restituito è già correttamente codificato (o in caso contrario esaminato) per la visualizzazione in formato HTML e che pertanto il valore non deve essere codificata in formato HTML nuovamente. Le operazioni seguenti, ad esempio, non deve essere (e non) codificato in formato HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Metodi di supporto di ASP.NET MVC 2 sono state aggiornate per funzionare con questa nuova sintassi in modo che non siano doppie codifica, ma solo quando si esegue una versione di ASP.NET 4. Questa nuova sintassi non funziona quando si esegue un'applicazione mediante ASP.NET 3.5 SP1.

Tenere presente che questo non garantisce la protezione dagli attacchi XSS. Ad esempio, HTML che utilizza i valori di attributo che non sono inclusi tra virgolette può contenere l'input dell'utente che sono comunque soggetti. Si noti che l'output di controlli ASP.NET e gli helper di ASP.NET MVC include sempre i valori di attributo tra virgolette, che è l'approccio consigliato.

Analogamente, questa sintassi non esegue JavaScript codifica, ad esempio quando si crea una stringa JavaScript basata sull'input dell'utente.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Modifiche apportate ai modelli di progetto

Nelle versioni precedenti di ASP.NET, quando si utilizza Visual Studio per creare un nuovo progetto sito Web o un progetto di applicazione Web, i progetti risultanti contengono solo una pagina aspx, valore predefinito è `Web.config` file e `App_Data` cartella, come illustrato di seguito Figura:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio supporta anche un tipo di progetto di sito Web vuoto, che non contiene file affatto, come illustrato nella figura riportata di seguito:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Il risultato è che per i principianti, è molto ridotto di informazioni aggiuntive su come creare un'applicazione Web di produzione. Pertanto, ASP.NET 4 introduce tre nuovi modelli, uno per un progetto di applicazione Web vuoto e uno per un progetto di applicazione Web e sito Web.

#### <a name="empty-web-application-template"></a>Modello di applicazione Web vuota

Come suggerisce il nome, il modello di applicazione Web vuota è un progetto di applicazione Web ridotta. Si seleziona questo modello di progetto nella finestra di dialogo Nuovo progetto di Visual Studio, come illustrato nella figura riportata di seguito:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Fare clic per visualizzare l'immagine ingrandita](overview/_static/image8.png))

Quando si crea un'applicazione Web ASP.NET vuota, Visual Studio crea il layout della cartella seguente:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

È simile al layout del sito Web vuoto da versioni precedenti di ASP.NET, con un'eccezione. In Visual Studio 2010, progetti applicazione Web vuota e il sito Web vuoto contengono i seguenti elementi di minimo `Web.config` file che contiene informazioni utilizzate da Visual Studio per identificare il framework che il progetto è destinato a:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Senza questo *targetFramework* proprietà, impostazione predefinita di Visual Studio è destinato a .NET Framework 2.0 per mantenere la compatibilità durante l'apertura di applicazioni meno recenti.

#### <a name="web-application-and-web-site-project-templates"></a>Modelli di progetto di sito Web e dell'applicazione Web

Le altre due nuovi modelli di progetto forniti con Visual Studio 2010 contengono le modifiche principali. Nella figura seguente viene mostrato il layout di progetto creati quando si crea un nuovo progetto applicazione Web. (Il layout per un progetto di sito Web è praticamente identico).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Il progetto include un numero di file che non sono stati creati nelle versioni precedenti. Inoltre, il nuovo progetto applicazione Web è configurato con le funzionalità di appartenenza base, che consentono di iniziare rapidamente a protezione accesso alla nuova applicazione. A causa di questo tipo di inclusione, il `Web.config` file per il nuovo progetto include le voci che consentono di configurare l'appartenenza, ruoli e profili. Nell'esempio seguente il `Web.config` file per un nuovo progetto applicazione Web. (In questo caso, *roleManager* è disabilitato.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Fare clic per visualizzare l'immagine ingrandita](overview/_static/image14.png))

Il progetto contiene anche un secondo `Web.config` file nel `Account` directory. Il secondo file di configurazione consente di proteggere l'accesso alla pagina ChangePassword per non-gli utenti connessi. Nell'esempio seguente viene illustrato il contenuto del secondo `Web.config` file.

![](overview/_static/image15.png)

Inoltre, le pagine create per impostazione predefinita nei nuovi modelli di progetto contengono più contenuti che nelle versioni precedenti. Il progetto contiene una pagina master predefinita e il file CSS e la pagina predefinita (Default.aspx) è configurata per utilizzare la pagina master per impostazione predefinita. Il risultato è che quando si esegue l'applicazione Web o un sito Web per la prima volta, la pagina (home) predefinita è già funzionale. In realtà, è simile alla pagina predefinita che viene visualizzato automaticamente all'avvio di una nuova applicazione MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Fare clic per visualizzare l'immagine ingrandita](overview/_static/image18.png))

Lo scopo di queste modifiche ai modelli di progetto è di fornire istruzioni su come iniziare a creare una nuova applicazione Web. Con semanticamente corretto, strict XHTML 1.0 conforme e layout in cui viene specificato l'utilizzo di CSS, le pagine nei modelli rappresentano le procedure consigliate per la creazione di applicazioni Web ASP.NET 4. Le pagine predefinite hanno anche un layout a due colonne che è possibile personalizzare facilmente.

Si supponga, ad esempio, per una nuova applicazione Web si desidera modificare alcuni dei colori e inserire il logo della società al posto del logo dell'applicazione ASP.NET. A tale scopo, si crea una nuova directory in `Content` per archiviare l'immagine del logo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Per aggiungere l'immagine alla pagina, quindi aprire il `Site.Master` file, cercare il testo dell'applicazione ASP.NET in cui è definito e sostituirlo con un *immagine* elemento il cui *src* attributo è impostato per il nuovo logo immagine, come nell'esempio seguente:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Fare clic per visualizzare l'immagine ingrandita](overview/_static/image22.png))

È quindi possibile passare nel file Site.css e modificare le definizioni di classe CSS per modificare il colore di sfondo della pagina, oltre che dell'intestazione.

Il risultato di queste modifiche è che è possibile visualizzare una pagina iniziale personalizzata con il minimo sforzo:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Fare clic per visualizzare l'immagine ingrandita](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Miglioramenti di CSS

Una delle principali aree di lavoro in ASP.NET 4 è stato per il rendering di HTML che è conforme agli standard HTML più recente. Ciò include le modifiche a come controlli server Web ASP.NET utilizzano gli stili CSS.

#### <a name="compatibility-setting-for-rendering"></a>Impostazione di compatibilità per il Rendering

Per impostazione predefinita, quando un'applicazione Web o un sito Web destinato a .NET Framework 4, il *controlRenderingCompatibilityVersion* attributo del *pagine* viene impostato su "4.0". Questo elemento è definito nel livello di computer `Web.config` file e per impostazione predefinita, si applica a tutte le applicazioni ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

Il valore per *controlRenderingCompatibility* è una stringa, che consente di potenziali nuove definizioni di versione nelle versioni future. Nella versione corrente, i valori seguenti sono supportati per questa proprietà:

- "3.5". Questa impostazione indica markup e il rendering legacy. Markup sottoposto a rendering dai controlli è compatibile con le versioni precedenti al 100% e l'impostazione del *xhtmlConformance* proprietà viene applicata.
- "4.0". Se la proprietà dispone di questa impostazione, i controlli server ASP.NET Web eseguire le operazioni seguenti:
- Il *xhtmlConformance* proprietà viene sempre considerata come "Strict". Di conseguenza, i controlli il rendering del markup XHTML 1.0 Strict.
- La disabilitazione di controlli non di input non esegue il rendering di stili non validi.
- *div* attorno campi nascosti venga ora stile in modo non interferiscono con le regole CSS creati dall'utente.
- I controlli menu il rendering del markup semanticamente corretto e conforme alle linee guida di accessibilità.
- I controlli di convalida non vengono visualizzati gli stili in linea.
- I controlli che in precedenza eseguito il rendering del bordo = "0" (i controlli che derivano da ASP.NET *tabella* controllo e ASP.NET *immagine* controllo) non è più il rendering di questo attributo.

#### <a name="disabling-controls"></a>La disattivazione dei controlli

In ASP.NET 3.5 SP1 e versioni precedenti, il framework esegue il rendering di *disabilitato* attributo nel markup HTML per qualsiasi controllo il cui *abilitato* proprietà impostata su *false*. Tuttavia, in base alle specifiche HTML 4.01, solo *input* elementi devono avere l'attributo.

In ASP.NET 4, è possibile impostare il *controlRenderingCompatabilityVersion* proprietà su "3.5", come nell'esempio seguente:

[!code-xml[Main](overview/samples/sample70.xml)]

È possibile creare il markup per un *etichetta* controllo simile al seguente, che disabilita il controllo:

[!code-aspx[Main](overview/samples/sample71.aspx)]

Il *etichetta* controllo renderebbe il codice HTML seguente:

[!code-html[Main](overview/samples/sample72.html)]

In ASP.NET 4, è possibile impostare il *controlRenderingCompatabilityVersion* su "4.0". In tal caso, controlla solo il cui rendering viene eseguito *input* gli elementi verranno visualizzati un *disabilitato* attributo quando il controllo *abilitato* è impostata su *false* . I controlli che non eseguono il rendering HTML *input* gli elementi visualizzati invece un *classe* attributo che fa riferimento a una classe CSS che è possibile utilizzare per definire un aspetto disabilitato per il controllo. Ad esempio, il *etichetta* controllo illustrato nell'esempio precedente genera il markup seguente:

[!code-html[Main](overview/samples/sample73.html)]

Il valore predefinito per la classe che specificate per il controllo è "aspNetDisabled". Tuttavia, è possibile modificare questo valore predefinito impostando statica *DisabledCssClass* proprietà statiche della *WebControl* classe. Per gli sviluppatori di controllo, il comportamento da utilizzare per un controllo specifico può essere definito anche utilizzando la *SupportsDisabledAttribute* proprietà.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Nascondere elementi intorno campi nascosti div

ASP.NET 2.0 e versioni successive, eseguire il rendering campi nascosti specifici del sistema (ad esempio il *nascosto* elemento utilizzato per archiviare informazioni sullo stato di visualizzazione) all'interno di *div* elemento per garantire la conformità allo standard XHTML. Tuttavia, ciò può provocare un problema quando influisce su una regola CSS *div* elementi in una pagina. Ad esempio, sarà possibile ottenere una linea di un pixel visualizzato nella pagina intorno nascosto *div* elementi. In ASP.NET 4, *div* elementi che racchiudono i campi nascosti generati da ASP.NET aggiungere un riferimento alle classi CSS come nell'esempio seguente:

[!code-html[Main](overview/samples/sample74.html)]

È quindi possibile definire una classe CSS che si applica solo al *nascosto* gli elementi che vengono generati da ASP.NET, come nell'esempio seguente:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Il rendering di una tabella esterna per i controlli basati su modelli

Per impostazione predefinita, i seguenti controlli server Web ASP.NET che supportano i modelli vengono automaticamente eseguito il wrapping in una tabella esterna che viene utilizzata per applicare gli stili in linea:

- *FormView*
- *Account di accesso*
- *PasswordRecovery*
- *ChangePassword*
- *Wizard*
- *CreateUserWizard*

Una nuova proprietà denominata *RenderOuterTable* è stato aggiunto a questi controlli che consente la tabella esterna da rimuovere dal markup. Ad esempio, si consideri l'esempio seguente di un *FormView* controllo:

[!code-aspx[Main](overview/samples/sample76.aspx)]

In questo markup esegue il rendering alla pagina, che include una tabella HTML il seguente output:

[!code-html[Main](overview/samples/sample77.html)]

Per impedire che la tabella viene eseguito il rendering, è possibile impostare il *FormView* del controllo *RenderOuterTable* proprietà, come nell'esempio seguente:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Nell'esempio precedente viene eseguito il rendering il seguente output, senza il *tabella*, *tr*, e *td* elementi:

> Content


Questa funzionalità avanzata può rendere più semplice per il contenuto del controllo con CSS, stile poiché nessun tag imprevisto viene sottoposti a rendering dal controllo.

> [!NOTE]
> Si noti che questa modifica disabilita il supporto per la funzione di formattazione automatica nella finestra di progettazione di Visual Studio 2010, perché non è più un *tabella* elemento che può contenere attributi di stile vengono generati dall'opzione di formattazione automatica.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Miglioramenti al controllo ListView

Il *ListView* controllo è stato reso più facile da utilizzare in ASP.NET 4. La versione precedente del controllo è obbligatorio specificare un modello di layout che contiene un controllo server con un ID noto. Di seguito viene illustrato un esempio tipico di utilizzo di *ListView* controllo in ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

In ASP.NET 4, il *ListView* controllo non richiede un modello di layout. Il codice illustrato nell'esempio precedente può essere sostituito con il markup seguente:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Miglioramenti di controllo RadioButtonList e CheckBoxList

In ASP.NET 3.5, è possibile specificare layout per il *CheckBoxList* e *RadioButtonList* utilizzando le due impostazioni seguenti:

- *Flusso*. Il controllo esegue il rendering *span* elementi per il relativo contenuto.
- *Tabella*. Il controllo esegue il rendering di un *tabella* per contenere il relativo contenuto.

Nell'esempio seguente viene illustrato il markup per ognuno di questi controlli.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Per impostazione predefinita, i controlli di eseguire il rendering HTML simile al seguente:

[!code-html[Main](overview/samples/sample82.html)]

Poiché questi controlli contengono elenchi di elementi, per eseguire il rendering HTML semanticamente corretto, deve eseguire il rendering il relativo contenuto usando l'elenco HTML (*li*) elementi. Questo rende più semplice per gli utenti, leggere le pagine Web tramite le tecnologie e semplifica la stile CSS tramite i controlli.

In ASP.NET 4, il *CheckBoxList* e *RadioButtonList* controlli supportano i nuovi valori per il *RepeatLayout* proprietà:

- *OrderedList* : il contenuto viene visualizzato come *li* elementi all'interno di una *ol* elemento.
- *Layout UnorderedList* : il contenuto viene visualizzato come *li* elementi all'interno di una *ul* elemento.

Nell'esempio seguente viene illustrato come utilizzare i nuovi valori.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Il codice precedente genera il codice HTML seguente:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Nota Se si imposta *RepeatLayout* a *OrderedList* o *UnorderedList*, *RepeatDirection* proprietà non può più essere utilizzata e verrà genera un'eccezione in fase di esecuzione se è stata impostata la proprietà all'interno del markup o codice. La proprietà non avrebbe alcun valore perché è definito il layout visivo di questi controlli utilizzando invece CSS.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Miglioramenti del controllo menu

Prima di ASP.NET 4, il *Menu* una serie di tabelle HTML eseguito rendering del controllo. Questo reso più difficile applicare gli stili CSS di fuori di impostazione delle proprietà inline e non è conforme agli standard di accessibilità.

In ASP.NET 4, il controllo ora esegue il rendering HTML mediante semantic markup che include un elenco non ordinato e gli elementi dell'elenco. Nell'esempio seguente viene illustrato il markup in una pagina ASP.NET per il *Menu* controllo.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Quando si esegue il rendering della pagina, il controllo genera il codice HTML seguente (il *onclick* codice è stati omessi per maggiore chiarezza):

[!code-html[Main](overview/samples/sample86.html)]

Oltre ai miglioramenti per il rendering, navigazione da tastiera del menu è stata migliorata utilizzando Gestione dello stato attivo. Quando il *Menu* controllo riceve lo stato attivo, è possibile utilizzare i tasti di direzione per spostarsi di elementi. Il *Menu* controllo ora anche associa ruoli di applicazioni (ARIA) internet avanzate accessibile e gli attributi seguenti condi[ala il](http://www.w3.org/TR/wai-aria-practices/#menu "linee guida Menu ARIA")migliorato accessibilità.

In un blocco di stile nella parte superiore della pagina, anziché in linea con elementi HTML di cui è stato eseguito il rendering di rendering degli stili per il controllo menu. Se si desidera eseguire il controllo completo sugli stili per il controllo, è possibile impostare il nuovo *IncludeStyleBlock* proprietà *false*, nel qual caso il blocco di stile non viene generato. Un modo per utilizzare questa proprietà consiste nell'utilizzare la funzionalità di formattazione automatica nella finestra di progettazione di Visual Studio per impostare l'aspetto del menu. È quindi possibile eseguire la pagina, aprire l'origine della pagina e quindi copiare il blocco di stile viene eseguito il rendering in un file CSS esterno. In Visual Studio, annullare l'applicazione di stili e set *IncludeStyleBlock* a *false*. Il risultato è che l'aspetto del menu viene definito utilizzando gli stili in un foglio di stile esterno.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Procedura guidata e controlli CreateUserWizard

ASP.NET *guidata* e *CreateUserWizard* controlli supportano i modelli che consentono di definire il codice HTML che eseguono il rendering. (*CreateUserWizard* deriva da *guidata*.) Nell'esempio seguente viene illustrato il markup basato su un completamente modelli *CreateUserWizard* controllo:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Il controllo esegue il rendering HTML simile al seguente:

[!code-html[Main](overview/samples/sample88.html)]

In ASP.NET 3.5 SP1, sebbene sia possibile modificare il contenuto del modello, è comunque limitato l'output del controllo di *guidata* controllo. In ASP.NET 4, è possibile creare un *LayoutTemplate* modello e insert *segnaposto* controlli (mediante nomi riservati) per specificare la modalità di *controllo procedura guidata* per eseguire il rendering. Nell'esempio seguente viene illustrato questo:

[!code-aspx[Main](overview/samples/sample89.aspx)]

L'esempio contiene quanto segue denominata segnaposto di *LayoutTemplate* elemento:

- *headerPlaceholder* : in fase di esecuzione, questo viene sostituito dal contenuto del *HeaderTemplate* elemento.
- *sideBarPlaceholder* : in fase di esecuzione, questo viene sostituito dal contenuto del *SideBarTemplate* elemento.
- *wizardStepPlaceHolder* : in fase di esecuzione, questo viene sostituito dal contenuto del *WizardStepTemplate* elemento.
- *navigationPlaceholder* – in fase di esecuzione, che viene sostituita da alcun modello di navigazione che sono state definite.

Il markup nell'esempio che usa segnaposti di rendering viene eseguito il codice HTML seguente (senza contenuto effettivamente definito nei modelli):

[!code-html[Main](overview/samples/sample90.html)]

Il codice HTML solo che ora non definito dall'utente è un *span* elemento. (Che in futuro si prevede di versioni, anche il *span* elemento non vengono visualizzato.) A questo punto garantisce il controllo completo sulla praticamente tutto il contenuto generato dal *guidata* controllo.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC è stato introdotto come un framework di componente aggiuntivo in ASP.NET 3.5 SP1, marzo 2009. Visual Studio 2010 include ASP.NET MVC 2, che include funzionalità e nuove funzionalità.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Supporto di aree

Le aree consentono di gruppo controller e visualizzazioni in sezioni di un'applicazione di grandi dimensioni nel relativo isolamento dalle altre sezioni. Ogni area può essere implementata come un progetto ASP.NET MVC separato che è possibile farvi riferimento dall'applicazione principale. Questo consente di gestire la complessità, quando si compila un'applicazione di grandi dimensioni e rende più semplice per più team di collaborare in una singola applicazione.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Supporto della convalida l'annotazione dei dati attributo

*DataAnnotations* attributi consentono di associare la logica di convalida a un modello utilizzando gli attributi dei metadati. *DataAnnotations* gli attributi sono stati introdotti in ASP.NET Dynamic Data in ASP.NET 3.5 SP1. Questi attributi sono integrati nel gestore di associazione del modello predefinito e forniscono un mezzo guidati dai metadati per convalidare l'input dell'utente.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Helper basati su modelli

Helper basati su modelli consentono di associa automaticamente modifica e visualizzare i modelli con tipi di dati. Ad esempio, è possibile utilizzare un helper del modello per specificare che un elemento dell'interfaccia utente di selezione data automaticamente eseguito il rendering per un *System. DateTime* valore. È simile ai modelli di campo in ASP.NET Dynamic Data.

Il *Html.EditorFor* e *HTML. DisplayFor* metodi di supporto con un supporto predefinito per il rendering di dati standard tipi di oggetti anche complessi con più proprietà. Sono inoltre personalizzare il rendering in quanto consente di applicare gli attributi di annotazione dei dati ad esempio *DisplayName* e *ScaffoldColumn* per il *ViewModel* oggetto.

Spesso si desidera personalizzare l'output da ulteriormente gli helper dell'interfaccia utente e avere il controllo completo su ciò che viene generato. Il *Html.EditorFor* e *HTML. DisplayFor* metodi helper questo supportano l'utilizzo di un meccanismo di modello che consente di definire modelli esterni che è possono eseguire l'override e il rendering dell'output di controllo. I modelli possono essere visualizzati singolarmente per una classe.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data

Dati dinamici è stata introdotta nella versione di .NET Framework 3.5 SP1 metà del 2008. Questa funzionalità offre numerosi miglioramenti per la creazione di applicazioni guidate dai dati, inclusi i seguenti:

- Un'esperienza RAD per creare rapidamente un sito Web basato sui dati.
- Convalida automatica basata sui vincoli definiti nel modello di dati.
- La possibilità di modificare facilmente il markup generato per i campi di *GridView* e *DetailsView* controlli tramite i modelli di campo che fanno parte del progetto Dynamic Data.

> [!NOTE]
> Nota: per ulteriori informazioni, vedere il [documentazione Dynamic Data](https://msdn.microsoft.com/library/cc488545.aspx) in MSDN Library.


Per ASP.NET 4, Dynamic Data è stata migliorata per consentire agli sviluppatori di potenza ancora maggiore per creare rapidamente siti Web basati sui dati.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Abilitazione di Dynamic Data per i progetti esistenti

Funzionalità dinamiche di dati inclusi in .NET Framework 3.5 SP1 portato nuove funzionalità, ad esempio le operazioni seguenti:

- Modelli di campo – forniscono modelli basato sul tipo di dati per i controlli con associazione a dati. Modelli di campo forniscono un modo più semplice per personalizzare l'aspetto dei controlli di dati rispetto all'utilizzo dei campi modello per ogni campo.
- Convalida: Dynamic Data consente di utilizzare gli attributi su classi di dati per specificare la convalida per gli scenari comuni, come i campi obbligatori, il controllo degli intervalli, controllo dei tipi, criteri di ricerca tramite espressioni regolari e convalida personalizzata. La convalida viene applicata tramite i controlli di dati.

Tuttavia, queste funzionalità sono i seguenti requisiti:

- Il livello di accesso ai dati deve essere basata su Entity Framework o LINQ to SQL.
- Gli unici dati di origine controlli supportati per queste funzionalità sono state di *EntityDataSource* o *LinqDataSource* controlli.
- Le funzionalità necessarie di un progetto Web creato utilizzando i dati dinamici o i modelli di entità dati dinamiche per disporre di tutti i file necessari per supportare la funzionalità.

Un obiettivo principale di supporto dati dinamici in ASP.NET 4 è consentire la nuova funzionalità di Dynamic Data per qualsiasi applicazione ASP.NET. Nell'esempio seguente viene illustrato il markup per i controlli che possono sfruttare i vantaggi delle funzionalità dinamica dei dati in una pagina esistente.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Nel codice per la pagina, il codice seguente deve essere aggiunto per abilitare il supporto dati dinamici per questi controlli:

[!code-csharp[Main](overview/samples/sample92.cs)]

Quando il *GridView* controllo è in modalità di modifica, Dynamic Data automaticamente verifica che i dati immessi nel formato corretto. In caso contrario, viene visualizzato un messaggio di errore.

Questa funzionalità offre anche altri vantaggi, ad esempio la possibilità di specificare il valore predefinito i valori per la modalità di inserimento. Senza i dati dinamici, per implementare un valore predefinito per un campo, è necessario connettersi a un evento, individuare il controllo (utilizzando *FindControl*) e impostarne il valore. In ASP.NET 4, il *EnableDynamicData* supporta la chiamata di un secondo parametro che consente di passare i valori predefiniti per tutti i campi per l'oggetto, come illustrato in questo esempio:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Sintassi dichiarativa per il controllo DynamicDataManager

Il *DynamicDataManager* controllo è stato migliorato in modo che è possibile configurare in modo dichiarativo, come con la maggior parte dei controlli in ASP.NET, anziché solo nel codice. Il markup per il *DynamicDataManager* controllo è simile alla seguente:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Questo markup Abilita il comportamento di Dynamic Data per il controllo GridView1 cui fa riferimento il *trascinarne* sezione la *DynamicDataManager* controllo.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Modelli di entità

Modelli di entità offrono un nuovo modo per personalizzare il layout dei dati senza che sia necessario creare una pagina personalizzata. Pagina usare modelli di *FormView* controllo (anziché il *DetailsView* controllare, utilizzate nei modelli di pagina nelle versioni precedenti di Dynamic Data) e *DynamicEntity* controllo per il rendering di modelli di entità. In questo modo più preciso il markup che viene eseguito il rendering da Dynamic Data.

Nell'elenco seguente viene illustrato il nuovo layout di directory di progetto che contiene i modelli di entità:

[!code-console[Main](overview/samples/sample95.cmd)]

Il `EntityTemplate` directory contiene i modelli per la visualizzazione di oggetti del modello di dati. Per impostazione predefinita, gli oggetti vengono visualizzati mediante il `Default.ascx` modello, che fornisce codice simile al seguente il markup creato il *DetailsView* controllo utilizzato per i dati dinamici in ASP.NET 3.5 SP1. Nell'esempio seguente viene illustrato il markup per il `Default.ascx` controllo:

[!code-aspx[Main](overview/samples/sample96.aspx)]

I modelli predefiniti possono essere modificati per modificare l'aspetto dell'intero sito. Sono disponibili modelli per la visualizzazione, modifica e le operazioni di inserimento. Nuovi modelli possono essere aggiunti in base al nome dell'oggetto dati per modificare l'aspetto di un solo tipo di oggetto. Ad esempio, è possibile aggiungere il modello seguente:

[!code-console[Main](overview/samples/sample97.cmd)]

Il modello può contenere il markup seguente:

[!code-aspx[Main](overview/samples/sample98.aspx)]

I nuovi modelli di entità vengono visualizzati in una pagina utilizzando il nuovo *DynamicEntity* controllo. In fase di esecuzione, questo controllo viene sostituito con il contenuto del modello di entità. Il markup seguente mostra il *FormView* controllo il `Detail.aspx` modello che utilizza il modello di entità. Si noti il *DynamicEntity* elemento nel markup.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nuovi modelli di campo per URL e indirizzi di posta elettronica

In ASP.NET 4 vengono presentati due nuovi modelli di campo predefiniti, `EmailAddress.ascx` e `Url.ascx`. Questi modelli vengono utilizzati per i campi contrassegnati come *EmailAddress* o *Url* con il *DataType* attributo. Per *EmailAddress* oggetti, il campo viene visualizzato come collegamento ipertestuale che viene creato utilizzando il *mailto:* protocollo. Quando gli utenti fanno clic sul collegamento, apre il client di posta elettronica dell'utente e crea una struttura di un messaggio. Gli oggetti digitati come *Url* vengono visualizzati come collegamenti ipertestuali comuni.

Nell'esempio seguente viene illustrato come campi verrà contrassegnati.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Creazione di collegamenti con il controllo DynamicHyperLink

Dynamic Data utilizza la nuova funzionalità di routing che è stato aggiunto in .NET Framework 3.5 SP1 per controllare l'URL visualizzato agli utenti finali quando accedono al sito Web. Il nuovo *DynamicHyperLink* controllo semplifica la compilazione di collegamenti a pagine in un sito Dynamic Data. Nell'esempio seguente viene illustrato come utilizzare il *DynamicHyperLink* controllo:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Questo codice crea un collegamento che punta alla pagina di elenco per il `Products` tabella basata su route definite nel `Global.asax` file. Il controllo utilizza automaticamente il nome di tabella predefinito basato su pagina di dati dinamica.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Supporto per l'ereditarietà nel modello di dati

Entity Framework e LINQ to SQL supporta l'ereditarietà nei modelli di dati. Un esempio di questo potrebbe essere un database che include un `InsurancePolicy` tabella. Può inoltre contenere `CarPolicy` e `HousePolicy` le tabelle che presentano gli stessi campi `InsurancePolicy` e quindi aggiungere altri campi. Dati dinamici sono stati modificati per comprendere gli oggetti ereditati nel modello di dati e per supportare lo scaffolding per le tabelle ereditate.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Supporto per le relazioni molti-a-molti (solo per Entity Framework)

Entity Framework offre supporto avanzato per le relazioni molti-a-molti tra tabelle, implementato esponendo la relazione come una raccolta in un *entità* oggetto. Nuovo `ManyToMany.ascx` e `ManyToMany_Edit.ascx` sono stati aggiunti i modelli di campo per fornire il supporto per la visualizzazione e modifica di dati coinvolti in relazioni molti-a-molti.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nuovi attributi di visualizzazione del controllo e il supporto delle enumerazioni

Il *DisplayAttribute* è stato aggiunto per fornire maggiore controllo sulle modalità di visualizzazione dei campi. Il *DisplayName* attributo nelle versioni precedenti dei dati dinamici è possibile modificare il nome utilizzato come didascalia per un campo. Il nuovo *DisplayAttribute* classe consente di specificare ulteriori opzioni per la visualizzazione di un campo, ad esempio l'ordine in cui viene visualizzato un campo e se un campo verrà utilizzato come filtro. L'attributo fornisce anche il nome utilizzato per le etichette nel controllo indipendente un *GridView* controllare, il nome utilizzato un *DetailsView* controllano, il testo della Guida per il campo e il limite per il campo (se il campo accetta input di testo).

Il *EnumDataTypeAttribute* classe è stato aggiunto per consentire di eseguire il mapping campi per le enumerazioni. Quando si applica questo attributo a un campo, si specifica un tipo di enumerazione. Dynamic Data utilizza il nuovo `Enumeration.ascx` modello di campo per creare l'interfaccia utente per la visualizzazione e modifica dei valori di enumerazione. Il modello viene eseguito il mapping di valori del database per i nomi nell'enumerazione.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Supporto avanzato per i filtri

Dynamic Data 1.0 forniti con i filtri predefiniti per colonne booleane e le colonne di chiave esterna. I filtri non ha consentito di specificare se sono stati visualizzati o l'ordine in cui sono stati visualizzati. Il nuovo *DisplayAttribute* attributo indirizzi entrambi questi problemi consentendo di controllano se una colonna viene visualizzata come un filtro e in quale ordine verranno visualizzato.

Offrire un ulteriore miglioramento è che il supporto di filtro è stato reinstallato[scritto per utilizzare il nuovo](#0.2__QueryExtender "_QueryExtender") funzionalità di Web Form. Ciò consente di creare filtri senza conoscere il controllo origine dati utilizzata con i filtri. Con queste estensioni, i filtri sono inoltre stati trasformati in controlli modello, che consente di aggiungere nuovi. Infine, il *DisplayAttribute* classe indicato in precedenza consente il filtro predefinito essere sottoposto a override, nello stesso modo in cui *UIHint* consente il modello di campo predefinito per una colonna da sottoporre a override.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web miglioramenti per lo sviluppo

Lo sviluppo Web in Visual Studio 2010 è stato migliorato per maggiore compatibilità CSS, incrementare la produttività nuovo IntelliSense JavaScript dinamico e i frammenti di codice HTML e ASP.NET.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilità con CSS migliorata

La finestra di progettazione di Visual Web Developer in Visual Studio 2010 è stata aggiornata per migliorare la conformità agli standard CSS 2.1. La finestra di progettazione, meglio, consente di mantenere l'integrità dell'origine HTML ed è più affidabile rispetto alle versioni precedenti di Visual Studio. Dietro le quinte, architettura sono anche stati apportati miglioramenti che consentire una migliore futuri miglioramenti per il rendering, layout e i servizi.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Frammenti di codice JavaScript e HTML

Nell'editor HTML, IntelliSense viene completata automaticamente i nomi di tag. La funzionalità di frammenti di codice IntelliSense viene completata automaticamente tag intero e altro ancora. In Visual Studio 2010, sono supportati i frammenti di codice IntelliSense per JavaScript, insieme a c# e Visual Basic, che sono supportati nelle versioni precedenti di Visual Studio.

Visual Studio 2010 include più di 200 frammenti che consentono di completare automaticamente i tag HTML e ASP.NET comuni, inclusi attributi obbligatori (ad esempio runat = "server") e gli attributi comuni specifici di un tag (ad esempio *ID*,  *DataSourceID*, *ControlToValidate*, e *testo*).

È possibile scaricare ulteriori frammenti di codice, oppure è possibile scrivere frammenti che incapsulano i blocchi di markup utilizzati dal team per le attività comuni.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Miglioramenti di IntelliSense per JavaScript

In Visual Studio 2010, IntelliSense per JavaScript è stato riprogettato per fornire un'esperienza di modifica avanzata. IntelliSense riconosce ora gli oggetti generati dinamicamente da metodi quali *registerNamespace* e da tecniche analoghe utilizzate da altri framework JavaScript. Prestazioni sono state migliorate per l'analisi delle librerie di script di grandi dimensioni e la visualizzazione di IntelliSense con ritardo di elaborazione senza alcuna. Compatibilità è stata notevolmente migliorata per supportare quasi tutte le librerie di terze parti e per supportare diversi stili di codifica. I commenti della documentazione vengono ora analizzati durante la digitazione e viene utilizzata immediatamente da IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Distribuzione di applicazioni Web con Visual Studio 2010

Quando gli sviluppatori ASP.NET distribuiscono un'applicazione Web, individuano spesso che incontrano problemi come i seguenti:

- La distribuzione in un sito di hosting condiviso richiede tecnologie quali FTP, che può essere lento. Inoltre, è necessario eseguire manualmente l'attività, ad esempio l'esecuzione di script SQL per configurare un database ed è necessario modificare le impostazioni di IIS, ad esempio la configurazione di una cartella della directory virtuale come un'applicazione.
- In un ambiente aziendale, oltre a distribuire i file dell'applicazione Web, gli amministratori devono modificare spesso i file di configurazione di ASP.NET e le impostazioni di IIS. Gli amministratori di database devono eseguire una serie di script SQL per ottenere il database dell'applicazione in esecuzione. Tali installazioni laboriosa, spesso richiedere ore e deve essere documentato con attenzione.

Visual Studio 2010 include tecnologie che risolvere questi problemi e che consentono di distribuire facilmente applicazioni Web. Una di queste tecnologie è lo strumento di distribuzione Web (MsDeploy.exe) di IIS.

Funzionalità di distribuzione Web in Visual Studio 2010 le aree principali seguenti:

- Creazione di pacchetti Web
- Trasformazione Web. config
- Distribuzione del database
- Pubblicazione con un clic per le applicazioni Web

Le sezioni seguenti forniscono informazioni dettagliate su queste funzionalità.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Creazione di pacchetti Web

Visual Studio 2010 utilizza lo strumento MSDeploy per creare un file compresso (zip) per l'applicazione, che viene considerato un *pacchetto Web*. Il file del pacchetto contiene i metadati relativi all'applicazione e il contenuto seguente:

- Impostazioni di IIS, che include impostazioni pool di applicazioni, impostazioni di pagina di errore e così via.
- Il contenuto Web effettivo, che include pagine Web, controlli utente, contenuto statico (immagini e file HTML) e così via.
- Gli schemi di database di SQL Server e i dati.
- Certificati di sicurezza, componenti da installare nella Global Assembly Cache, le impostazioni del Registro di sistema e così via.

Un pacchetto Web può essere copiato in qualsiasi server e quindi installato manualmente tramite Gestione IIS. In alternativa, per la distribuzione automatica, il pacchetto può essere installato utilizzando i comandi della riga di comando o tramite API di distribuzione.

Visual Studio 2010 offre compilato nell'attività di MSBuild e destinazioni per creare pacchetti Web. Per ulteriori informazioni, vedere [Panoramica della distribuzione progetto applicazione Web ASP.NET](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) nel sito Web di MSDN e [10 + 20 motivi per cui è necessario creare un pacchetto Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) sul blog di Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Trasformazione Web. config

Per la distribuzione di applicazioni Web, Visual Studio 2010 introduce [trasformare i documenti XML (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), che è una funzionalità che consente di trasformare un `Web.config` file dalle impostazioni di sviluppo le impostazioni di produzione. Le impostazioni delle trasformazioni specificate nei file di trasformazione denominati `web.debug.config`, `web.release.config`e così via. (I nomi di questi file corrispondano le configurazioni di MSBuild). Un file di trasformazione include solo le modifiche che è necessario apportare a un `Web.config` file. Specificare le modifiche utilizzando la sintassi semplice.

Nell'esempio seguente viene mostrata una parte di un `web.release.config` file che può essere prodotti per la distribuzione della configurazione di rilascio. La parola chiave di sostituzione nell'esempio specifica che durante la distribuzione di *connectionString* nodo il `Web.config` file verrà sostituito con i valori sono elencati nell'esempio.

[!code-xml[Main](overview/samples/sample102.xml)]

Per ulteriori informazioni, vedere [sintassi di trasformazione Web. config per la distribuzione di progetto applicazione Web](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) MSDN <a id="0.2_a"> </a> sito Web e[distribuzione Web: trasformazione Web. config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)sul blog di Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Distribuzione del database

Un pacchetto di distribuzione di Visual Studio 2010 può includere le dipendenze nel database di SQL Server. Come parte della definizione del pacchetto, fornire la stringa di connessione per il database di origine. Quando si crea il pacchetto Web, Visual Studio 2010 consente di creare script SQL per lo schema del database e, facoltativamente, per i dati e li aggiunge al pacchetto. È inoltre possibile specificare gli script SQL personalizzati e la sequenza in cui deve essere eseguito nel server. In fase di distribuzione è fornire una stringa di connessione appropriato per il server di destinazione. il processo di distribuzione utilizza quindi la stringa di connessione per eseguire gli script che creano lo schema del database e aggiungono i dati.

Inoltre, tramite un solo clic pubblicazione, è possibile configurare la distribuzione per pubblicare direttamente il database quando l'applicazione viene pubblicato in un sito di hosting condiviso remoto. Per ulteriori informazioni, vedere [come: distribuire un Database con un progetto di applicazione Web](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) nel sito Web di MSDN e [distribuzione del Database con VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) sul blog di Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Pubblicazione con un clic per le applicazioni Web

Visual Studio 2010 consente inoltre di utilizzare il servizio Gestione remota di IIS per pubblicare un'applicazione Web a un server remoto. È possibile creare un profilo di pubblicazione per l'account di hosting o per i server di test o i server di gestione temporanea. Ogni profilo può salvare in modo sicuro le credenziali appropriate. È quindi possibile distribuire a qualsiasi destinazione server con un clic tramite il Web fare clic su uno degli strumenti di pubblicazione. Con Visual Studio 2010, è anche possibile pubblicare tramite la riga di comando di MSBuild. Ciò consente di configurare l'ambiente team build per includere la pubblicazione in un modello di integrazione continua.

Per ulteriori informazioni, vedere [procedura: distribuire un'applicazione con un clic pubblicazione dei progetti Web e distribuzione Web](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) nel sito Web MSDN e [Web 1-fare clic su pubblica con VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) sul blog di Vishal Joshi. Per visualizzare presentazioni video relative alla distribuzione di applicazioni Web in Visual Studio 2010, vedere [VS 2010 per Web Developer Preview](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) sul blog di Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Risorse

I siti Web seguenti forniscono ulteriori informazioni su ASP.NET 4 e Visual Studio 2010.

- [In ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) , consultare la documentazione ufficiale per ASP.NET 4 nel sito Web MSDN.
- [https://www.asp.net/](https://www.asp.net/) : ASP.NET il sito Web del team.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) e [ASP.NET Dynamic Data Content Map](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) -risorse Online sul sito del team ASP.NET e nella documentazione ufficiale per ASP.NET Dynamic Data.
- [https://www.asp.net/ajax/](../../ajax/index.md) : La risorsa Web principale per lo sviluppo di ASP.NET Ajax.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) Ovvero il blog del Team di Visual Web Developer, che include informazioni sulle funzionalità in Visual Studio 2010.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) , ovvero la risorsa Web principale per le versioni di anteprima di ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute nel presente documento rappresentano l'attuale opinione di Microsoft Corporation circa le problematiche discusse alla data della pubblicazione. Poiché Microsoft deve rispondere ai cambiamenti delle condizioni di mercato, il presente documento non deve essere interpretato quale un impegno da parte di Microsoft e Microsoft non può garantire l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è a esclusivo carico dell'utente. Senza limitare i diritti sanciti dal copyright, nessuna parte di questo documento può essere riprodotta, memorizzata o inserita in un sistema di ricerca o trasmessa in qualsiasi forma o mezzo (elettronico o meccanico, mediante fotocopia, registrazione o altro), per alcuno scopo, senza l'autorizzazione scritta di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto del presente documento. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna del presente documento non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non diversamente specificato, la società, organizzazioni, prodotti, nomi di dominio, indirizzi di posta elettronica, logo, persone, luoghi ed eventi citati in questo documento sono fittizio e nessuna associazione società, organizzazione, prodotto, nome di dominio, posta elettronica indirizzo, logo, persona, luogo o evento è intenzionale o può essere presupposta.

© 2009 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
