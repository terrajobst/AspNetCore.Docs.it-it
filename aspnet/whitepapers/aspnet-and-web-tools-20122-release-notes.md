---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: Web ASP.NET e strumenti 2012.2 note sulla versione | Documenti Microsoft
author: rick-anderson
description: Note sulla versione per ASP.NET e Web strumenti 2012.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: 52559a47f86e572f873d4eaaab50e87eb51722fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "28047232"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET e strumenti Web 2012.2 note sulla versione
====================
> Questo documento descrive la versione di ASP.NET e Web strumenti 2012.2. È un aggiornamento Web gli strumenti di Visual Studio e ASP.NET.


- [Note sull'installazione](#_Installation)
- [Documentazione](#_Documentation)
- [Supporto](#_Support)
- [Requisiti software](#_Software_Requirements)
- [Nuove funzionalità di ASP.NET e strumenti Web 2012.2](#_New_Features_in)

    - [Strumenti](#_Tooling)
    - [Pubblicazione sul Web](#_Web_Publishing)
    - [Modelli di MVC ASP.NET](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [URL brevi di ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problemi noti e modifiche di rilievo](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Note sull'installazione

ASP.NET e 2012.2 strumenti Web per Visual Studio 2012 può essere installato utilizzando [installazione guidata piattaforma Web](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Si tratta di un aggiornamento di Visual Studio 2012 o Visual Studio Express 2012 per Web, è necessario. Se non è installato Visual Studio, Visual Studio Express 2012 per Web verrà installato.

È possibile anche installare ASP.NET e Web strumenti 2012.2 manualmente. È necessario disporre di Visual Studio 2012 o Visual Studio Express 2012 per Web installato. Utilizzare quindi le istruzioni seguenti: 

1. Scaricare [ASP.NET e Web Framework 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) programma di installazione dall'area download.
2. Quando richiesto fare clic su Esegui. È anche possibile salvare il file per eseguirlo in un secondo momento.
3. Verificare la versione di Visual Studio verrà effettuato l'aggiornamento. È possibile farlo tramite l'avvio di Visual Studio che si desidera aggiornare. Scegliere la voce di menu della Guida.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Se è visualizzata la voce di menu &quot;su Microsoft Visual Studio 2012 per Web&quot; quindi scaricare [Web Developer Tools 2012.2 - Visual Studio Express 2012 per Web](https://go.microsoft.com/fwlink/?LinkID=282228). In caso contrario scaricare [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Quando richiesto fare clic su Esegui. È anche possibile salvare il file per eseguirlo in un secondo momento.

> [!NOTE]
> Versione ASP.NET e Web strumenti 2012.2 non include SQL Server Data Tools. SQL Server e database SQL di Windows Azure fornisce un set più completo del database, gli strumenti inclusi lo sviluppo di progetti di backup offline, confronto schema e funzionalità di distribuzione di database avanzato. Per ulteriori informazioni o per installare SQL Server Data Tools visitare [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET e Web strumenti 2012.2 sono disponibili dal sito web ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Supporto

ASP.NET Web strumenti 2012.2 ufficialmente rilasciato e supportato. È possibile utilizzare il canale di supporto normale. È anche possibile pubblicare domande nei forum di ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), in cui i membri della community ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisiti software

ASP.NET e Web strumenti 2012.2 richiede Visual Studio 2012 o Visual Studio Express 2012 per Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nuove funzionalità di ASP.NET e strumenti Web 2012.2

Questa sezione vengono descritte le funzionalità che sono state introdotte nella versione di ASP.NET e Web strumenti 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Strumenti

- Controllo pagina 

    - Supporta il mapping di selezione JavaScript che consente il controllo pagina eseguire il mapping di elementi che sono stati aggiunti dinamicamente alla pagina al codice JavaScript corrispondente.
    - La possibilità di visualizzare gli aggiornamenti CSS in tempo reale.
    - Per altre informazioni, vedere [sincronizzazione automatica CSS e JavaScript selezione Mapping in controllo pagina](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Supporta l'evidenziazione della sintassi per CoffeeScript, Mustache, i manubri delle e JsRender.
    - L'editor HTML fornisce Intellisense per le associazioni di ritaglio.
    - MINORE di modifica e il compilatore supporta per abilitare la compilazione dinamica CSS utilizzando minore.
    - Incolla JSON come una classe .NET. Tramite questo comando Incolla speciale per incollare JSON in un linguaggio c# o i file di codice Visual Basic.NET e Visual Studio genera automaticamente le classi .NET dedotte da JSON.
- Supporto dell'emulatore di dispositivi mobili aggiunge hook di estensibilità in modo che gli emulatori di terze parti possono essere installati come un progetto VSIX. Gli emulatori installati verranno visualizzati nell'elenco a discesa F5, in modo che gli sviluppatori possono visualizzare in anteprima i siti Web su una vasta gamma di dispositivi mobili. Altre informazioni su questa funzionalità in post sul blog di Scott Hanselman [la nuova BrowserStack integrazione con Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Pubblicazione sul Web

- Progetti di sito Web è ora dispongono la stessa esperienza pubblicazione progetti applicazione Web inclusa la pubblicazione di siti Web di Windows Azure.
- Pubblicazione selettiva &#8211; per uno o più file è possibile eseguire le azioni seguenti (dopo la pubblicazione in un endpoint di distribuzione Web): 

    - Pubblicare i file selezionati.
    - Vedere la differenza tra un file locale e un file remoto.
    - Aggiornare il file locale con il file remoto o aggiornare il file remoto con il file locale.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Modelli di MVC ASP.NET

- Il nuovo modello applicazione Facebook consente la scrittura di facilmente applicazioni Canvas per Facebook. In alcuni semplici passaggi, è possibile creare un'applicazione Facebook che ottiene i dati da un utente connesso e si integra con gli amici. Il modello include una nuova libreria per la gestione dei plumbing coinvolti nella creazione di app per Facebook, ad esempio autenticazione, autorizzazioni, accesso ai dati di Facebook e altro ancora. Per ulteriori informazioni sull'utilizzo del modello applicazione Facebook, vedere [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Un nuovo modello MVC per applicazioni a pagina singola consente agli sviluppatori di compilare applicazioni web sul lato client interattive mediante HTML 5, CSS 3 e Knockout comuni e le librerie JavaScript jQuery, nella parte superiore di ASP.NET Web API. Il modello include un'applicazione di elenco "todo" che illustra le procedure comuni per la creazione di un'applicazione JavaScript HTML5 che utilizza un'API server RESTful. È possibile leggere informazioni, vedere [ https://www.asp.net/single-page-application ](../single-page-application/index.md).
- È ora possibile creare un progetto VSIX che consente di aggiungere nuovi modelli alla finestra di dialogo Nuovo progetto MVC ASP.NET. Altre informazioni, vedere: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Pacchetto FixedDisplayModes &#8211; modelli di progetto MVC sono stati aggiornati per includere il nuovo pacchetto NuGet 'FixedDisplayModes', che contiene una soluzione alternativa per un bug in MVC 4. Per ulteriori informazioni sulla correzione contenuta nel pacchetto, vedere questo post di blog ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) dal team di MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

ASP.NET Web API è stata migliorata con numerose nuove funzionalità:

- ASP.NET Web API OData
- Traccia API Web ASP.NET
- Pagina della Guida ASP.NET Web API

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData offre la flessibilità che necessaria per compilare endpoint OData con logica di business completo su qualsiasi origine dati. Con ASP.NET Web API OData è controllare la quantità di semantica OData che si desidera esporre. ASP.NET Web API OData è incluso con i modelli di progetto ASP.NET MVC 4 ed è disponibile anche in NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData supporta attualmente le caratteristiche seguenti:

- Abilitare la semantica di query OData applicando l'attributo [Queryable].
- Convalidare le query OData e limitare il set di opzioni di query supportate, operatori e funzioni facilmente.
- Parametro associato a ODataQueryOptions direttamente per ottenere una rappresentazione dell'albero sintattico astratto della query che possono essere convalidate e applicata a un oggetto IQueryable o IEnumerable.
- Abilita paging basato su servizio e generazione di collegamenti nella pagina successiva, specificando i limiti di risultati all'attributo [Queryable].
- Richiedere un conteggio del numero totale di risorse corrispondente usando $inlinecount inline.
- Controllare la propagazione null.
- Qualsiasi o tutti gli operatori in $filter.
- Derivare un modello entity data model per convenzione o personalizzare in modo esplicito un modello in modo analogo a Entity Framework Code-First.
- Entità di esporre imposta mediante derivazione da EntitySetController.
- Convenzioni di semplice e personalizzabile, per esporre le proprietà di navigazione, la modifica dei collegamenti e implementazione di azioni OData.
- Semplificato routing tramite il metodo di estensione MapODataRoute.
- Supporto per il controllo delle versioni che espongono più modelli EDM.
- Esporre $metadata e documento di servizio in modo è possibile generare client (.NET, Windows Phone, Windows Store e così via) per l'API Web.
- Supporto per i formati OData Atom, JSON e JSON dettagliati.
- Creare, aggiornare, parzialmente aggiornamento (PATCH) ed eliminare le entità.
- Una query e modificare le relazioni tra entità.
- Creare collegamenti di relazione che fino ai percorsi di rete.
- Tipi complessi.
- Ereditarietà dei tipi di entità.
- Proprietà della raccolta.
- Enumerazioni.
- Azioni OData.
- Basa stessa base di WCF Data Services, vale a dire ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Per ulteriori informazioni su ASP.NET Web API OData, vedere [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Traccia API Web ASP.NET

Traccia di ASP.NET Web API si integra i dati di traccia dal web API con analisi .NET. È ora abilitata per impostazione predefinita nel modello di progetto API Web. Analisi dei dati per il web API viene inviata alla finestra di Output e vengono rese disponibili tramite IntelliTrace. ASP.NET Web API Tracing consente alle informazioni di traccia quando è ospitato in Windows Azure mediante l'integrazione con l'API di Web [diagnostica Windows Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). È anche possibile installare e abilitare la traccia di ASP.NET Web API in qualsiasi applicazione che utilizza il pacchetto NuGet di traccia ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Per ulteriori informazioni sulla configurazione e l'utilizzo di ASP.NET Web API Tracing vedere [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Pagina della Guida ASP.NET Web API

La pagina della Guida di ASP.NET Web API è ora inclusa per impostazione predefinita nel modello di progetto API Web. La pagina della Guida di ASP.NET Web API genera automaticamente documentazione relativa a web API tra gli endpoint HTTP, i metodi HTTP supportati, i parametri e i payload di messaggi di richiesta e risposta di esempio. Documentazione viene automaticamente effettuato il pull dai commenti nel codice. È anche possibile aggiungere la pagina della Guida di ASP.NET Web API per qualsiasi applicazione che utilizza il pacchetto NuGet di ASP.NET Web API Guida Page ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Per ulteriori informazioni sulla configurazione e personalizzazione vedere la pagina della Guida ASP.NET Web API [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR è più semplice per aggiungere funzionalità web in tempo reale per l'applicazione ASP.NET, uso WebSocket se disponibile e automaticamente eseguire il fallback su altre tecniche quando non è.

Per ulteriori informazioni sull'utilizzo di ASP.NET SignalR vedere [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>URL brevi di ASP.NET

FriendlyURLs ASP.NET è molto semplice per gli sviluppatori di web form generare pulitura ricerca URL (senza l'estensione aspx). Non richiede a nessuna configurazione minima e può essere utilizzato con le applicazioni ASP.NET v 4.0 esistenti. La funzionalità FriendlyURLs rende anche più facile per gli sviluppatori aggiungere il supporto per dispositivi mobili per le applicazioni, supportando il passaggio tra le visualizzazioni di desktop e mobile.

Per ulteriori informazioni sull'installazione e utilizzo di URL brevi di ASP.NET, vedere [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo nella versione di ASP.NET e Web strumenti 2012.2.

### <a name="installation-issues"></a>Problemi di installazione

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Ordine di installazione di Visual Studio 2012

L'installazione di un aggiuntive SKU di Visual Studio 2012 dopo l'installazione di ASP.NET e Web strumenti 2012.2 richiederanno un'operazione di ripristino. Si consideri la seguente sequenza:

1. Installare Visual Studio Express 2012 per Web
2. Installare ASP.NET e strumenti Web 2012.2
3. Installare Visual Studio 2012 Professional, Premium o Ultimate

Passaggio 2 comporta solo l'installazione degli aggiornamenti per Express per Web. Per verificare che lo SKU aggiuntivo installato durante il passaggio 3 includa l'aggiornamento sarà necessario ripristinare ASP.NET e 2012.2 strumenti Web per installare gli aggiornamenti per lo SKU ultimo installato. Questo vale anche se le SKU nel passaggio 1 e 3 sono invertiti.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>L'installazione di Microsoft ASP.NET e Web strumenti 2012.2 quando viene aperto Visual Studio

Se Visual Studio è aperto durante l'installazione di Microsoft ASP.NET e Web strumenti 2012.2, Visual Studio potrebbe terminare con uno stato non valido. Si consiglia agli utenti di chiudere tutte le istanze di Visual Studio prima di procedere con l'installazione.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Annulla l'installazione ASP.NET e Web strumenti 2012.2 nel corso di installazione

Annullamento di ASP.NET e Web strumenti 2012.2 il programma di installazione nel corso di installazione lascerà Visual Studio in uno stato non valido. Per risolvere questo problema, eseguire la procedura seguente: 

- Vai a Installazione applicazioni.
- Disinstallare Microsoft ASP.NET e 2012.2 strumenti Web, se presente.
- Reinstallare Microsoft ASP.NET e strumenti Web 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Dopo la disinstallazione di ASP.NET e 2012.2 strumenti Web di ASP.NET MVC 4 sono presenti modelli e modelli di siti Web Razor v2

Disinstallazione di ASP.NET e Web strumenti 2012.2 disinstallerà anche tutti i modelli di siti Web Razor v2 e di ASP.NET MVC 4 da Visual Studio 2012.

La soluzione alternativa consiste nel ripristinare l'installazione di Visual Studio 2012 per reinstallare ASP.NET MVC 4 e modelli di siti Web Razor v2.

### <a name="tooling-issues"></a>Gli strumenti di problemi

#### <a name="nuget-error-reported-during-project-creation"></a>Errore di NuGet segnalati durante la creazione del progetto

Dopo l'installazione di ASP.NET e Web strumenti 2012.2 venga visualizzato il seguente errore durante la creazione di un progetto MVC 4

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

ASP.NET e Web strumenti 2012.2 viene fornito NuGet 2.1 e aggiornerà l'estensione in Visual Studio 2012. In alcuni casi, il programma di installazione VSIX avrà esito negativo aggiornare correttamente il progetto VSIX. La procedura seguente consente di risolvere il problema:

1. Avviare Visual Studio 2012 come amministratore
2. Passare a strumenti -&gt;estensioni e aggiornamenti e la disinstallazione di NuGet.
3. Chiudere Visual Studio.
4. Passare alla cartella di installazione di ASP.NET e 2012.2 strumenti Web:

    1. Per Visual Studio 2012: **Programmi\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Per Visual Studio Express 2012 per Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 per Web**
5. Fare doppio clic su NuGet.Tools.vsix reinstallare NuGet

### <a name="web-api-issues"></a>Problemi di Web API

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>L'analisi dei problemi relativi a $filter e valori letterali DateTime

Il parser URI OData non riesce analizzare correttamente i valori letterali datetime parziale. Ad esempio, $filter = datetime di inizio eq'2012-12-31T12:00' non può essere analizzata correttamente. Una soluzione alternativa consiste nell'usare il $filter completo letterale, = datetime di inizio eq'2012-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData non supporta i nomi di proprietà tra maiuscole e minuscole.

OData non supporta i nomi delle proprietà di distinzione tra maiuscole e le query OData e il percorso odata. Gli elementi di lavoro, vedere:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Se gli utenti hanno maiuscole/minuscole sul lato server e client javascript, viene probabilmente verrà rilevato il problema. Questo problema è da progettazione nel protocollo odata. Tuttavia, molti utenti segnala il problema. Per risolvere questo problema, gli utenti devono correggere ai case nell'URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>OData predefinito convenzioni di routing non supporta POST o PUT sulla proprietà di navigazione.

OData predefinito convenzioni di routing non supporta POST o PUT sulla proprietà di navigazione. Vedere l'elemento di lavoro [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Assenza di questa convenzione di uso comune in convenzioni predefinite.

Per risolvere questo problema, gli utenti devono estendere nuova convenzione di routing per supportarla.

### <a name="facebook-template-issues"></a>Problemi nel modello Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Modello applicazione Facebook funziona solo con .NET 4.5

Nell'elenco a discesa di framework nella finestra di dialogo Nuovo progetto per visualizzare il modello applicazione Facebook in ASP.NET MVC 4, è necessario selezionare .NET 4.5.

#### <a name="real-time-update-controller"></a>Controller di aggiornamento in tempo reale

Il modello applicazione Facebook consente facilmente di utente di creare un Controller API Web per gestire gli aggiornamenti in tempo reale da Facebook. Se il computer di sviluppo è dietro un NAT, il Controller potrebbe non funzionare senza un'ulteriore configurazione di rete. Per ulteriori informazioni, vedere di seguito: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Query stringa valori in conflitto con i parametri di OAuth di Facebook

I campi seguenti sono in conflitto con la chiamata della finestra di dialogo di OAuth di Facebook eseguire il backup di URL. Non aggiungere i valori di stringa di query con i nomi seguenti: codice di errore, errore\_descrizione, l'errore\_motivo.

#### <a name="using-page-inspector-with-facebook-template"></a>Con controllo pagina modello Facebook

È possibile utilizzare la funzionalità di controllo pagina in Visual Studio 2012 durante il debug dell'applicazione Facebook. Controllo pagina attualmente non supporta IFRAME.

### <a name="single-page-application-template-issues"></a>Single Page Application Template problemi

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Con aggiornamento 1.9/Knockout 2.2.1, durante l'esecuzione di progetto MVC SPA predefinito, nuova modifica elemento di attività immettere JQuery evento dello stato attivo non viene gestito correttamente.

Con JQuery 1.9/Knockout 2.2.1 dell'aggiornamento, quando si esegue il progetto MVC SPA predefinito, nuova modifica elemento di attività immettere non più lo stato attivo alla casella di modifica elemento todo nuovo dopo aver immesso il nuovo elemento di attività all'elenco attività.

Per fare riferimento soluzione alternativa [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)e apportare correzione simile al codice di esempio seguente:

File todo.model.js  
 funzione todolist(data), aggiungere seguenti:  
 **self.isSelected = ko.observable(false);**

funzione todoList.prototype.addTodo, aggiungere il seguente testo blacked:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

File cshtml, aggiungere il seguente testo blacked:  
 &lt;form data-bind=&quot;submit: addTodo&quot;&gt;  
 &lt;input class=&quot;addTodo&quot; type=&quot;text&quot; data-bind=&quot;value: newTodoTitle, placeholder: 'Type here to add', blurOnEnter: true, **hasfocus: isSelected**, event: { blur: addTodo }&quot; /&gt;  
 &lt;/form&gt;
