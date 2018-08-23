---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Che cosa è necessario eseguire operazioni in ASP.NET e come procedere invece | Microsoft Docs
author: tfitzmac
description: Questo argomento descrive i diversi errori comuni che gli utenti apportano all'interno dei progetti web ASP.NET. Fornisce indicazioni per cosa è necessario fare per evitare questi comu...
ms.author: riande
ms.date: 05/08/2014
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 7f2366ef8cb258a08c3cb96407f605864e5ec9a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832154"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Che cosa è necessario eseguire operazioni in ASP.NET e come procedere invece
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo argomento descrive i diversi errori comuni che gli utenti apportano all'interno dei progetti web ASP.NET. Fornisce indicazioni per cosa è necessario fare per evitare questi errori comuni. Si basa su un [presentation](http://vimeo.com/68390507) dal **Damian Edwards** presso Norwegian Developers Conference.


## <a name="disclaimer"></a>Dichiarazione di non responsabilità

In questo argomento non è come una guida completa per garantire che l'applicazione è sicura ed efficiente. È comunque necessario seguire le procedure consigliate per la sicurezza e prestazioni non indicati in questo argomento. Suggerisce solo come evitare gli errori più comuni relativi a processi e le classi .NET.

## <a name="overview"></a>Panoramica

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Conformità agli standard](#standards)

    - [Adattatori di controllo](#adapters)
    - [Proprietà di stile per controlli](#styleprop)
    - [Pagina e i callback di controllo](#callback)
    - [Rilevamento di funzionalità del browser](#browsercap)
- [Sicurezza](#security)

    - [Convalida delle richieste](#validation)
    - [Sessione e autenticazione basata su form senza cookie](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Livello di attendibilità medio](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Affidabilità e prestazioni](#performance)

    - [PreSendRequestHeaders e PreSendRequestContent](#presend)
    - [Eventi di pagina asincrona con i Web Form](#asyncevents)
    - [Lavoro Fire-and-Forget](#fire)
    - [Corpo entità richiedente](#requestentity)
    - [Response. Redirect e Response](#redirect)
    - [EnableViewState e ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Richieste di lunga durata esecuzione (> 110 secondi)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Conformità agli standard

<a id="adapters"></a>

### <a name="control-adapters"></a>Adattatori di controllo

Raccomandazione: Interrompere l'uso di adattatori di controllo per il rendering adattivo e usare invece le query supporti CSS e HTML conforme agli standard.

Gli adattatori di controlli sono stati introdotti in .NET 2.0 per il rendering di codice di presentazione che è stato personalizzato per gli ambienti e dispositivi diversi. A questo punto, il rendering adattivo può essere eseguito con HTML e CSS. È necessario interrompere l'uso di adattatori di controllo e convertire tutti gli adapter esistenti CSS e HTML.

Per altre informazioni, vedere [query sui supporti](http://www.w3.org/TR/css3-mediaqueries/) e [procedura: aggiungere pagine per dispositivi mobili per il Web Form ASP.NET / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Proprietà di stile per controlli

Raccomandazione: Arrestare l'impostazione dei valori di stile nel markup del controllo e invece impostare valori di formattazione in fogli di stile CSS.

I controlli server Web contengono decine di proprietà che può essere usata per impostare le proprietà di stile in linea. Ad esempio, la proprietà ForeColor imposta il colore del testo per un controllo. È possibile ottenere questo risultato stesso in modo più efficiente tramite fogli di stile CSS. Fogli di stile consentono di centralizzare i valori di stile e non impostare questi valori in tutta l'applicazione.

Nell'esempio seguente mostra una classe CSS il testo di set e impostato su rosso.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

L'esempio seguente viene illustrato come applicare in modo dinamico la classe CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Pagina e i callback di controllo

Raccomandazione: Interrompere l'uso di callback di pagina e controllo e usare invece le seguenti operazioni: AJAX, UpdatePanel, i metodi di azione MVC, API Web o SignalR.

Nelle versioni precedenti di ASP.NET, i metodi di callback di pagina e il controllo consentono di aggiornare una parte della pagina web senza l'aggiornamento di un'intera pagina. È ora possibile eseguire aggiornamenti parziali della pagina tramite [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md). È consigliabile arrestare usando i metodi di callback perché possono causare problemi con gli URL brevi e routing. Per impostazione predefinita, i controlli non abilitare i metodi di callback, ma se è abilitata questa funzionalità in un controllo, è consigliabile disabilitarla.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Rilevamento di funzionalità del browser

Raccomandazione: Interrompere l'uso di rilevamento di funzionalità del browser statico e usare invece il rilevamento di funzionalità dinamiche.

Nelle versioni precedenti di ASP.NET, le funzionalità supportate per ogni browser sono state archiviate in un file XML. Supporto della funzionalità rilevamento tramite una ricerca statico non è l'approccio migliore. A questo punto, è possibile rilevare in modo dinamico un browser le funzionalità supportate usando un framework di rilevamento di funzionalità, ad esempio [Modernizr](http://modernizr.com/). Rilevamento delle funzionalità determina il supporto tenta di utilizzare un metodo o proprietà e quindi un controllo per verificare se il browser ha prodotto il risultato desiderato. Per impostazione predefinita, Modernizr è incluso nei modelli di applicazione Web.

<a id="security"></a>

## <a name="security"></a>Sicurezza

<a id="validation"></a>

### <a name="request-validation"></a>Convalida delle richieste

Raccomandazione: Convalida dell'input utente e la codifica dell'output dagli utenti.

Convalida della richiesta è una funzionalità di ASP.NET che esamina ogni richiesta e interrompe la richiesta se viene rilevata una minaccia percepita. Non basarsi sul convalida delle richieste per la protezione dell'applicazione da attacchi di scripting intersito. Al contrario, convalidare tutti gli input dagli utenti e codificare l'output. In alcuni casi, è possibile usare espressioni regolari per convalidare l'input, ma nei casi più complessi, che è necessario convalidare i valori consentiti di input dell'utente tramite le classi .NET che determinano se il valore corrisponde.

Nell'esempio seguente viene illustrato come usare un metodo statico nella classe Uri per determinare se l'Uri fornito da un utente è valido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Tuttavia, per sufficientemente verificare l'Uri, dovrebbe anche controllare per verificare che venga specificato `http` o `https`. Nell'esempio seguente usa i metodi di istanza per verificare che l'Uri sia valido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Prima di eseguire il rendering di input dell'utente in formato HTML o inclusi input dell'utente in una query SQL, codificare i valori per assicurarsi che non è incluso codice dannoso.

Si può HTML codifica il valore nel markup con la &lt;%: %&gt; sintassi, come illustrato di seguito.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

O, nella sintassi Razor, si può HTML codificare con @, come illustrato di seguito.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

L'esempio seguente mostra come HTML codifica un valore nel code-behind.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Per codificare in modo sicuro un valore per i comandi SQL, usare i parametri di comando, ad esempio la [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Sessione e autenticazione basata su form senza cookie

Raccomandazione: Richiede i cookie.

Il passaggio di informazioni di autenticazione nella stringa di query non è protetto. Pertanto, richiede i cookie quando l'applicazione include l'autenticazione. Se i cookie vengono archiviate le informazioni riservate, è consigliabile richiedere SSL per il cookie.

Nell'esempio seguente viene illustrato come specificare il file Web. config che un cookie che viene trasmesso tramite SSL richiede l'autenticazione basata su form.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Raccomandazione: Mai impostata su false.

Per impostazione predefinita, EnbableViewStateMac è impostato su true. Anche se l'applicazione non usa lo stato di visualizzazione, non impostare EnableViewStateMac su false. Impostando questo valore su false verrà rendere l'applicazione vulnerabile a XSS.

A partire da ASP.NET 4.5.2, il runtime applica **EnableViewStateMac = true**. Anche se si impostato su false, il runtime ignora questo valore e procede con il valore impostato su true. Per altre informazioni, vedere [ASP.NET 4.5.2 ed EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Nell'esempio seguente viene illustrato come impostare EnableViewStateMac su true. Non devi effettivamente impostare questo valore su true perché è true per impostazione predefinita. Tuttavia, se si hanno impostata su false per qualsiasi pagina nell'applicazione, è necessario correggere subito questo valore.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Livello di attendibilità medio

Raccomandazione: Non dipendono attendibilità media (o qualsiasi altro livello di attendibilità) come limite di sicurezza.

Attendibilità parziale non protegge in modo adeguato l'applicazione e non deve essere utilizzata. In alternativa, usare l'attendibilità e isolare le applicazioni non attendibili nel pool di applicazioni separati. Inoltre, eseguire ogni pool di applicazioni in un'identità univoca. Per altre informazioni, vedere [ASP.NET parzialmente attendibile non garantisce l'isolamento applicazione](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Raccomandazione: Non disattivare le impostazioni di sicurezza &lt;appSettings&gt; elemento.

L'elemento appSettings contiene molti valori necessari per gli aggiornamenti della sicurezza. Non devi modificare o disabilitare tali valori. Se è necessario disabilitare questi valori quando si distribuisce un aggiornamento, immediatamente abilitare di nuovo dopo il completamento della distribuzione.

Per informazioni dettagliate, vedere [elemento appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Consiglio: Usare [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) invece.

Il metodo UrlPathEncode è stato aggiunto a .NET Framework per risolvere un problema di compatibilità browser molto specifico. Può non codificare in modo adeguato un URL e non protegge l'applicazione di scripting intersito. Non deve mai utilizzato nell'applicazione. Usare invece [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

Nell'esempio seguente viene illustrato come passare un URL codificato come parametro di stringa di query per un controllo collegamento ipertestuale.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Affidabilità e prestazioni

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders e PreSendRequestContent

Raccomandazione: Non usare questi eventi con moduli gestiti. Invece di scrivere un modulo IIS nativo per eseguire l'operazione necessaria. Visualizzare [creazione di moduli HTTP di codice nativo](https://msdn.microsoft.com/library/ms693629.aspx).

È possibile usare la [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) e [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) gli eventi con i moduli nativi IIS.
> [!WARNING]
> Non utilizzare `PreSendRequestHeaders` e `PreSendRequestContent` con i moduli gestiti che implementano `IHttpModule`. Impostazione di queste proprietà possono causare problemi con le richieste asincrone. La combinazione di applicazioni ha richiesto il Routing (ARR) e websockets potrebbe causare eccezioni di violazione di accesso che possono causare w3wp arresto anomalo del sistema. Ad esempio, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 in iiscore.dll ha generato un'eccezione di violazione di accesso (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Eventi di pagina asincrona con i Web Form

Raccomandazione: In Web Form, evitare di scrivere async void metodi per gli eventi del ciclo di vita di pagina e usare invece [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) per il codice asincrono.

Quando si contrassegna un evento di pagina con **async** e **void**, non è possibile determinare quando ha terminato il codice asincrono. Usare invece Page.RegisterAsyncTask per eseguire il codice asincrono in modo che consente di tenere traccia del relativo completamento.

L'esempio seguente mostra un pulsante di fare clic sul gestore che contiene il codice asincrono. Questo esempio include la lettura di un valore stringa in modo asincrono, che viene fornito solo come un esempio semplificato di un'attività asincrona e non come una procedura consigliata.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Se si usa le attività asincrone, impostare il framework di destinazione di runtime Http alla versione 4.5 nel file Web. config. Impostazione del framework di destinazione a 4.5 turns nel nuovo contesto di sincronizzazione che è stata aggiunta in .NET 4.5. Questo valore è impostato per impostazione predefinita nei nuovi progetti in Visual Studio 2012, ma è non possibile impostare se si lavora con un progetto esistente.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Lavoro Fire-and-Forget

Raccomandazione: Quando si gestisce una richiesta all'interno di ASP.NET, evitare l'avvio di lavoro fire-and-forget (tale chiamata al metodo ThreadPool. QueueUserWorkItem o la creazione di un timer che chiama ripetutamente un delegato).

Se l'applicazione presenta il lavoro fire-and-forget eseguito all'interno di ASP.NET, l'applicazione può ottenere sincronizzato. In qualsiasi momento, può essere eliminato il dominio dell'applicazione che si intende che il processo in corso potrebbe non corrispondere lo stato corrente dell'applicazione.

È consigliabile spostare questo tipo di lavoro all'esterno di ASP.NET. È possibile utilizzare un processo Web, servizio di Windows o un ruolo di lavoro in Azure per eseguire il lavoro in corso ed eseguire il codice da un altro processo.

Se è necessario eseguire questa operazione all'interno di ASP.NET, è possibile aggiungere il pacchetto Nuget chiamato [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) per eseguire il codice.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Corpo entità richiedente

Raccomandazione: Evitare la lettura di Request. Form o Request.InputStream prima del gestore evento eseguito.

Non appena possibile è consigliabile leggere da Request. Form o Request.InputStream è durante il gestore evento eseguito. In MVC, il Controller è il gestore e l'evento execute è quando viene eseguito il metodo di azione. In Web Form, la pagina è il gestore ed evento execute è quando viene generato l'evento Page. Init. Se si legge il corpo entità della richiesta precedente a evento execute, interferire con l'elaborazione della richiesta.

Se è necessario leggere il corpo di entità di richiesta prima dell'evento execute, utilizzare [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) oppure [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Quando si usa GetBufferlessInputStream, ottenere il flusso non elaborato dalla richiesta e si assume alcuna responsabilità per l'elaborazione della richiesta intera. Dopo aver chiamato GetBufferlessInputStream, Request. Form e Request.InputStream non sono disponibili perché non sono stati compilati da ASP.NET. Quando si usa GetBufferedInputStream, ottenere una copia del flusso dalla richiesta. Request. Form Request.InputStream rimangono comunque disponibili in un secondo momento nella richiesta poiché ASP.NET consente di popolare a altra copia.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response. Redirect e Response

Raccomandazione: Essere consapevoli delle differenze nella modalità di gestione di thread dopo la chiamata [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

Il [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) metodo chiama il metodo Response. In un processo sincrono, chiamare Request.Redirect fa sì che il thread corrente viene interrotta immediatamente. Tuttavia, in un processo asincrono, la chiamata a Response. Redirect non interrompe il thread corrente, quindi continua l'esecuzione di codice per la richiesta. In un processo asincrono, è necessario restituire l'attività dal metodo che deve interrompere l'esecuzione del codice.

In un progetto MVC, è necessario chiamare non Response. Redirect. Al contrario, restituisce un RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState e ViewStateMode

Consiglio: Usare ViewStateMode, anziché EnableViewState, per fornire un controllo granulare su quali controlli utilizzano lo stato di visualizzazione.

Quando si imposta EnableViewState su false nella direttiva di pagina, lo stato di visualizzazione è disabilitato per tutti i controlli all'interno della pagina e non può essere abilitato. Se si vuole attivare lo stato di visualizzazione per solo determinati controlli nella pagina, impostato su disabilitato ViewStateMode per la pagina.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Quindi, impostare ViewStateMode su abilitato, solo i controlli che devono effettivamente lo stato di visualizzazione.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Abilitando lo stato di visualizzazione per solo i controlli che ne hanno necessità, è possibile ridurre le dimensioni dello stato di visualizzazione per le pagine web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Consiglio: Usare i provider universali.

Nei modelli di progetto corrente, SqlMembershipProvider è stato sostituito da [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), disponibile come pacchetto NuGet. Se si usa SqlMembershipProvider in un progetto che è stato compilato con una versione precedente dei modelli, è necessario passare a Universal Providers. I provider universali funziona con tutti i database che sono supportati da Entity Framework.

Per altre informazioni, vedere [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Le richieste a esecuzione prolungata (> 110 secondi)

Consiglio: Usare [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) oppure [SignalR](../../../signalr/index.md) per i client connessi e utilizzare le operazioni dei / o asincrone.

Le richieste a esecuzione prolungata possono causare risultati imprevedibili e una riduzione delle prestazioni nell'applicazione web. L'impostazione di timeout predefinito per una richiesta è 110 secondi. Se si usa lo stato della sessione con una richiesta a esecuzione prolungata, ASP.NET rilascerà il blocco sull'oggetto sessione dopo 110 secondi. Tuttavia, l'applicazione potrebbe essere un'operazione sull'oggetto sessione in corso quando viene rilasciato il blocco e l'operazione potrebbe non essere completata correttamente. Se una seconda richiesta da parte dell'utente viene bloccata durante la prima richiesta è in esecuzione, la seconda richiesta può accedere all'oggetto di sessione in uno stato incoerente.

Se l'applicazione include operazioni dei / o di blocco (o sincrone), l'applicazione sarà non risponda.

Per migliorare le prestazioni, usare le operazioni dei / o asincrone in .NET Framework. Inoltre, è possibile usare WebSocket o SignalR per la connessione client al server. Queste funzionalità sono progettate per gestire in modo efficiente le richieste a esecuzione prolungata.
