---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Che cosa è necessario eseguire in ASP.NET e quali operazioni eseguire invece | Documenti Microsoft
author: tfitzmac
description: In questo argomento vengono descritti i diversi errori comuni, assicurarsi di persone all'interno dei progetti web ASP.NET. Fornisce indicazioni per le operazioni da effettuare per evitare questi commo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 829f3a024bc15bec8b60b91193ba9bca37b78009
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034920"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Che cosa è necessario eseguire in ASP.NET e quali operazioni eseguire invece
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo argomento vengono descritti i diversi errori comuni, assicurarsi di persone all'interno dei progetti web ASP.NET. Fornisce indicazioni per le operazioni da effettuare per evitare questi errori comuni. È basato su un [presentazione](http://vimeo.com/68390507) da **Damian Edwards** norvegese conferenza di sviluppatori.


## <a name="disclaimer"></a>Dichiarazione di non responsabilità

In questo argomento non è come una guida completa per garantire che l'applicazione sia sicuro ed efficiente. È comunque necessario seguire le procedure consigliate per la sicurezza e delle prestazioni che non sono descritte in questo argomento. Si consiglia solo come evitare errori comuni relativi a processi e le classi .NET.

## <a name="overview"></a>Panoramica

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Conformità agli standard](#standards)

    - [Adattatori di controllo](#adapters)
    - [Proprietà di stile per controlli](#styleprop)
    - [Pagina e i callback di controllo](#callback)
    - [Rilevamento di funzionalità del browser](#browsercap)
- [Sicurezza](#security)

    - [Convalida della richiesta](#validation)
    - [Sessione e l'autenticazione basata su form senza cookie](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Attendibilità media](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Affidabilità e prestazioni](#performance)

    - [PreSendRequestHeaders e PreSendRequestContent](#presend)
    - [Eventi di pagina asincrona con Web Form](#asyncevents)
    - [Lavoro Fire-and-Forget](#fire)
    - [Il corpo entità della richiesta](#requestentity)
    - [Response. Redirect e Response](#redirect)
    - [EnableViewState e ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Richieste in esecuzione lunghe (> 110 secondi)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Conformità agli standard

<a id="adapters"></a>

### <a name="control-adapters"></a>Adattatori di controllo

Consiglio: Interrompere l'utilizzo di adattatori di controllo per il rendering adattivo e usare le query di supporto CSS e HTML conforme agli standard.

Gli adattatori di controlli sono stati introdotti in .NET 2.0 per il rendering di codice di presentazione che è stato personalizzato per gli ambienti e dispositivi diversi. A questo punto, il rendering adattivo può essere eseguito con CSS e HTML. Si deve interrompere l'utilizzo di adattatori di controllo e convertire tutti gli adapter esistenti in CSS e HTML.

Per ulteriori informazioni, vedere [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) e [procedura: aggiungere pagine Mobile Web Form di ASP.NET del / applicazione MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Proprietà di stile per controlli

Consiglio: Arrestare l'impostazione dei valori di stile nel markup del controllo e invece di impostare valori di formattazione nei fogli di stile CSS.

I controlli server Web contengono decine di proprietà che può essere utilizzata per impostare le proprietà di stile in linea. Ad esempio, la proprietà ForeColor imposta il colore del testo per un controllo. È possibile ottenere questo risultato stesso in modo più efficiente, tramite fogli di stile CSS. Fogli di stile consentono di centralizzare i valori di stile e di evitare di impostare questi valori in tutta l'applicazione.

L'esempio seguente mostra una classe CSS set testo rosso.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

Nell'esempio seguente viene illustrato come applicare in modo dinamico la classe CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Pagina e i callback di controllo

Consiglio: Interrompere l'utilizzo di callback di pagina e di controllo e quindi usare i seguenti: AJAX, UpdatePanel, metodi di azione MVC, API Web o SignalR.

Nelle versioni precedenti di ASP.NET, metodi di callback di pagina e il controllo abilitato, è possibile aggiornare parte della pagina web senza aggiornare un'intera pagina. È ora possibile eseguire aggiornamenti parziali della pagina tramite [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md). È consigliabile arrestare utilizzando metodi di callback, perché possono causare problemi con gli URL brevi e di routing. Per impostazione predefinita, i controlli non abilitare metodi di callback, ma se è abilitata questa funzionalità in un controllo, è consigliabile disabilitarla.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Rilevamento di funzionalità del browser

Consiglio: Interrompere l'uso di rilevamento di funzionalità browser statica e quindi usare rilevamento funzionalità dinamica.

Nelle versioni precedenti di ASP.NET, le funzionalità supportate per ogni browser vengono memorizzate in un file XML. Supporto delle funzionalità rilevamento tramite una ricerca statica non è l'approccio migliore. A questo punto, è possibile rilevare in modo dinamico un browser tutte le funzionalità supportate tramite un framework di rilevamento di funzionalità, ad esempio [Modernizr](http://modernizr.com/). Funzionalità rilevamento determina il supporto durante il tentativo di utilizzare un metodo o proprietà e quindi un controllo per verificare se il browser ha prodotto il risultato desiderato. Per impostazione predefinita, Modernizr è incluso nei modelli di applicazione Web.

<a id="security"></a>

## <a name="security"></a>Sicurezza

<a id="validation"></a>

### <a name="request-validation"></a>Convalida della richiesta

Consiglio: Convalidare l'input dell'utente e la codifica dell'output da utenti.

Convalida della richiesta è una funzionalità di ASP.NET che esamina ogni richiesta e interrompe la richiesta se viene trovata una minaccia. Non basarsi sulla convalida richiesta per la protezione dell'applicazione da attacchi di script tra siti. Al contrario, convalidare tutti gli input dagli utenti e codificare l'output. In alcuni casi, è possibile utilizzare espressioni regolari per convalidare l'input, ma nei casi più complessi, che è necessario convalidare input dell'utente usando le classi .NET che determinano se il valore corrisponde a valori consentiti.

Nell'esempio seguente viene illustrato come utilizzare un metodo statico nella classe Uri per determinare se l'Uri fornito da un utente è valido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Tuttavia, per verificare sufficientemente l'Uri, anche controllare per verificare che venga specificato `http` o `https`. L'esempio seguente usa i metodi di istanza per verificare che l'Uri sia valido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Prima di eseguire il rendering di input dell'utente in formato HTML o tra l'input dell'utente in una query SQL, codificare i valori per evitare che sia incluso codice dannoso.

È possibile HTML codifica il valore nel markup con la &lt;%: %&gt; sintassi, come illustrato di seguito.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

In alternativa, nella sintassi Razor, è possibile HTML codifica con @, come illustrato di seguito.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

L'esempio seguente mostra come HTML codifica un valore nel code-behind.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Per codificare in modo sicuro un valore per i comandi SQL, utilizzare i parametri del comando, ad esempio il [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Sessione e l'autenticazione basata su form senza cookie

Consiglio: Richiedono i cookie.

Il passaggio di informazioni di autenticazione nella stringa di query non è protetto. Quando l'applicazione include l'autenticazione, pertanto richiede i cookie. Se i cookie vengono archiviate informazioni riservate, è consigliabile richiedere SSL per il cookie.

Nell'esempio seguente viene illustrato come specificare il file Web. config che l'autenticazione basata su form richiede un cookie viene trasmesso tramite SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Consiglio: Mai impostata su false.

Per impostazione predefinita, EnbableViewStateMac è impostata su true. Anche se l'applicazione non utilizza lo stato di visualizzazione, non impostare EnableViewStateMac su false. Impostare questo valore su false verranno rendere l'applicazione vulnerabile agli script tra siti.

A partire da ASP.NET 4.5.2, il runtime applica **EnableViewStateMac = true**. Anche se si imposta su false, il runtime ignora questo valore e procede con il valore impostato su true. Per ulteriori informazioni, vedere [ASP.NET 4.5.2 ed EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Nell'esempio seguente viene illustrato come impostare EnableViewStateMac su true. Non è necessario impostare effettivamente questo valore su true perché è true per impostazione predefinita. Tuttavia, se si have impostato su false in qualsiasi pagina dell'applicazione, è necessario correggere subito questo valore.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Attendibilità media

Consiglio: Non dipendono attendibilità media (o qualsiasi altro livello di attendibilità) come limite di sicurezza.

Attendibilità parziale non protegge in modo adeguato l'applicazione e non deve essere utilizzata. In alternativa, usare l'attendibilità totale e isolare le applicazioni non attendibili nel pool di applicazioni separati. Inoltre, eseguire ogni pool di applicazioni con un'identità univoca. Per ulteriori informazioni, vedere [attendibilità parziale di ASP.NET non garantisce l'isolamento delle applicazioni](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Consiglio: Non disattivare le impostazioni di sicurezza &lt;appSettings&gt; elemento.

L'elemento appSettings contiene molti valori che sono necessari per gli aggiornamenti di sicurezza. Non si deve modificare o disabilitare questi valori. Se è necessario disabilitare questi valori quando si distribuisce un aggiornamento, immediatamente abilitare di nuovo dopo aver completato la distribuzione.

Per informazioni dettagliate, vedere [elemento appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Suggerimento: Usare [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) invece.

Il metodo UrlPathEncode è stato aggiunto a .NET Framework per risolvere un problema di compatibilità del browser molto specifiche. Non codificare in modo adeguato un URL e non impedisce l'applicazione di script tra siti. Non deve mai utilizzato nell'applicazione. Utilizzare invece [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

Nell'esempio seguente viene illustrato come passare un URL codificato come parametro di stringa di query per un controllo hyperlink.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Affidabilità e prestazioni

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders e PreSendRequestContent

Consiglio: Non utilizzare questi eventi con i moduli gestiti. Invece di scrivere un modulo nativo di IIS per eseguire l'operazione necessaria. Vedere [la creazione di moduli di codice nativo HTTP](https://msdn.microsoft.com/library/ms693629.aspx).

È possibile utilizzare il [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) e [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) gli eventi con i moduli nativi di IIS.
> [!WARNING]
> Non utilizzare `PreSendRequestHeaders` e `PreSendRequestContent` con i moduli gestiti che implementano `IHttpModule`. Impostazione di queste proprietà possono causare problemi durante le richieste asincrone. La combinazione di applicazione richiesto Routing (ARR) e WebSocket potrebbe portare a eccezioni di violazione di accesso che possono causare w3wp arresto anomalo. Ad esempio, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 in iiscore.dll ha causato un'eccezione di violazione di accesso (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Eventi di pagina asincrona con Web Form

Consiglio: In Web Form, evitare di scrivere async void metodi per gli eventi del ciclo di vita della pagina e quindi usare [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) per codice asincrono.

Se si contrassegna un evento di pagina con **async** e **void**, non è possibile determinare quando termina il codice asincrono. Utilizzare invece Page.RegisterAsyncTask per eseguire il codice asincrono in modo che consente di tenere traccia del relativo completamento.

Nell'esempio seguente un pulsante di fare clic su gestore contenente codice asincrono. Questo esempio è inclusa la lettura di un valore stringa in modo asincrono, che viene fornito solo come un esempio semplificato di un'attività asincrona e non come una procedura consigliata.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Se si usano le attività asincrone, impostare il framework di destinazione del runtime Http 4.5 nel file Web. config. Impostare il framework di destinazione 4.5 attiva nel contesto di sincronizzazione nuovo che è stato aggiunto in .NET 4.5. Questo valore è impostato per impostazione predefinita nei nuovi progetti in Visual Studio 2012, ma è non possibile impostare se si lavora con un progetto esistente.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Lavoro Fire-and-Forget

Suggerimento: Quando si gestisce una richiesta all'interno di ASP.NET, evitare di avvio lavoro fire-and-forget (tale chiamata al metodo QueueUserWorkItem o la creazione di un timer che chiama ripetutamente un delegato).

Se l'applicazione presenta lavoro fire-and-forget in esecuzione in ASP.NET, l'applicazione può ottenere sincronizzato. In qualsiasi momento, può essere eliminato il dominio dell'applicazione che si intende che il processo in corso potrebbe non corrispondere lo stato corrente dell'applicazione.

È consigliabile spostare questo tipo di lavoro all'esterno di ASP.NET. È possibile utilizzare un processi Web, servizio di Windows o un ruolo di lavoro in Azure per eseguire il lavoro in corso ed eseguire il codice da un altro processo.

Se è necessario eseguire questa operazione all'interno di ASP.NET, è possibile aggiungere il pacchetto Nuget chiamato [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) per eseguire il codice.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Il corpo entità della richiesta

Consiglio: Impedire la lettura di Request. Form o Request.InputStream prima dell'esecuzione del gestore eventi.

La prima è necessario leggere da Request. Form o Request.InputStream è il gestore evento eseguito. In MVC, il Controller è il gestore e l'Esegui evento quando viene eseguito il metodo di azione. In Web Form, la pagina è il gestore e l'Esegui evento quando viene generato l'evento Page. Init. Se in precedenza l'evento Esegui leggere il corpo entità della richiesta, interferire con l'elaborazione della richiesta.

Se è necessario leggere il corpo di entità di richiesta prima dell'evento di esecuzione, utilizzare uno [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Quando si utilizza GetBufferlessInputStream, ottenere il flusso non elaborato della richiesta e assumere la responsabilità per l'elaborazione della richiesta di intera. Dopo aver chiamato GetBufferlessInputStream, Request. Form e Request.InputStream non sono disponibili perché non sono stati compilati da ASP.NET. Quando si utilizza GetBufferedInputStream, si ottiene una copia del flusso della richiesta. Request. Form Request.InputStream rimangono comunque disponibili in un secondo momento nella richiesta poiché ASP.NET compila a altra copia.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response. Redirect e Response

Consiglio: Tenere presente le differenze nella modalità di gestione di thread dopo la chiamata [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

Il [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) metodo chiama il metodo Response. In un processo sincrono, chiamare Request.Redirect fa sì che il thread corrente viene interrotta immediatamente. Tuttavia, in un processo asincrono, chiamare Response. Redirect non interrompe il thread corrente, in modo continua l'esecuzione del codice per la richiesta. In un processo asincrono, è necessario restituire l'attività da parte del metodo per interrompere l'esecuzione di codice.

In un progetto MVC, è necessario non chiamare Response. Redirect. Al contrario, restituire un RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState e ViewStateMode

Suggerimento: Usare ViewStateMode anziché EnableViewState, per fornire un controllo granulare quali controlli utilizzare lo stato di visualizzazione.

Quando si imposta EnableViewState su false nella direttiva della pagina, lo stato di visualizzazione è disabilitato per tutti i controlli all'interno della pagina e non può essere abilitato. Se si desidera abilitare lo stato di visualizzazione solo per alcuni controlli della pagina, impostare ViewStateMode su disabilitato per la pagina.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Quindi, impostare ViewStateMode su abilitato, solo i controlli che devono effettivamente lo stato di visualizzazione.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Abilita lo stato di visualizzazione per solo i controlli che ne hanno necessità, è possibile ridurre le dimensioni dello stato di visualizzazione per le pagine web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Suggerimento: Usare provider Universal.

Nei modelli di progetto corrente, SqlMembershipProvider è stata sostituita da [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), che è disponibile come pacchetto NuGet. Se si utilizza SqlMembershipProvider in un progetto che è stato compilato con una versione precedente dei modelli, è necessario passare a Universal Providers. I provider Universal funziona con tutti i database che sono supportati da Entity Framework.

Per ulteriori informazioni, vedere [Introduzione a ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Le richieste a esecuzione prolungata (> 110 secondi)

Suggerimento: Usare [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [SignalR](../../../signalr/index.md) per i client connessi e utilizzare le operazioni dei / o asincrone.

Richieste a esecuzione prolungata possono causare risultati imprevisti e una riduzione delle prestazioni nell'applicazione web. L'impostazione di timeout predefinito per una richiesta è 110 secondi. Se si utilizza lo stato della sessione con una richiesta a esecuzione prolungata, ASP.NET verrà rilasciare il blocco sull'oggetto sessione dopo 110 secondi. Tuttavia, l'applicazione potrebbe essere all'interno di un'operazione sull'oggetto di sessione quando viene rilasciato il blocco e l'operazione potrebbe non essere completata correttamente. Se una seconda richiesta da parte dell'utente viene bloccata durante l'esecuzione della prima richiesta, la seconda richiesta può accedere all'oggetto di sessione in uno stato incoerente.

Se l'applicazione include operazioni dei / o di blocco (o sincrone), l'applicazione sarà non risponda.

Per migliorare le prestazioni, utilizzare le operazioni dei / o asincrone in .NET Framework. Inoltre, è possibile usare WebSocket o SignalR per la connessione client al server. Queste funzionalità sono progettate per gestire in modo efficiente le richieste a esecuzione prolungata.
