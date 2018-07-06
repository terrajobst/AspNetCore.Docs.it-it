---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: ASP.NET e Web Tools 2012.2 note sulla versione | Microsoft Docs
author: rick-anderson
description: Note sulla versione di ASP.NET and Web Tools 2012.2.
ms.author: aspnetcontent
ms.date: 02/14/2013
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: 687eca7d1a7368338bb16c5de24fbe99e9ba500d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806407"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET and Web Tools 2012.2 note sulla versione
====================
> Questo documento descrive la versione di ASP.NET and Web Tools 2012.2. È un aggiornamento per gli strumenti di Visual Studio Web e ASP.NET.


- [Note sull'installazione](#_Installation)
- [Documentazione](#_Documentation)
- [Supporto](#_Support)
- [Requisiti software](#_Software_Requirements)
- [Nuove funzionalità di ASP.NET and Web Tools 2012.2](#_New_Features_in)

    - [Strumenti](#_Tooling)
    - [Pubblicazione sul Web](#_Web_Publishing)
    - [Modelli ASP.NET MVC](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [URL brevi di ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problemi noti e modifiche di rilievo](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Note sull'installazione

ASP.NET and Web Tools 2012.2 per Visual Studio 2012 può essere installato usando [installazione guidata piattaforma Web](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Si tratta di un aggiornamento di Visual Studio 2012 o Visual Studio Express 2012 per Web, che è obbligatorio. Se non è installato Visual Studio, verrà installato Visual Studio Express 2012 per Web.

È possibile anche installare ASP.NET and Web Tools 2012.2 manualmente. È necessario disporre di Visual Studio 2012 o Visual Studio Express 2012 per Web installati. Usare quindi le istruzioni seguenti: 

1. Scaricare [ASP.NET e Framework 2012.2 Web](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) programma di installazione dall'area Download Microsoft.
2. Quando richiesto scegliere Esegui. È anche possibile salvare il file per l'esecuzione in un secondo momento.
3. Verificare la versione di Visual Studio verrà aggiornata. Questo scopo, è possibile l'avvio di Visual Studio che si desidera aggiornare. Scegliere la voce di menu della Guida.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Se è visualizzata la voce di menu &quot;su Microsoft Visual Studio 2012 per Web&quot; quindi scaricare [Web Developer Tools 2012.2 - Visual Studio Express 2012 per Web](https://go.microsoft.com/fwlink/?LinkID=282228). In caso contrario, scaricare [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Quando richiesto scegliere Esegui. È anche possibile salvare il file per l'esecuzione in un secondo momento.

> [!NOTE]
> Versione ASP.NET and Web Tools 2012.2 non include SQL Server Data Tools. SQL Server e database SQL di Windows Azure offre un set più ampio del database, strumenti, tra cui lo sviluppo basato su progetto offline, confronto schema e le funzionalità di distribuzione avanzato del database. Per altre informazioni o per installare SQL Server Data Tools, visitare [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET and Web Tools 2012.2 sono disponibili dal sito web ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Supporto

ASP.NET and Web Tools 2012.2 è ufficialmente rilasciate e supportate. È possibile usare il canale di supporto. È anche possibile pubblicare domande nei forum di ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), in cui i membri della community di ASP.NET sono spesso in grado di fornire supporto informale.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisiti software

Di ASP.NET and Web Tools 2012.2 richiede Visual Studio 2012 o Visual Studio Express 2012 per Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nuove funzionalità di ASP.NET and Web Tools 2012.2

Questa sezione descrive le funzionalità che sono state introdotte nella versione di ASP.NET and Web Tools 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Strumenti

- Controllo pagina 

    - Supporta il mapping di selezione di JavaScript che consente il controllo pagina eseguire il mapping di elementi che sono stati aggiunti in modo dinamico alla pagina di tornare al codice JavaScript corrispondente.
    - La possibilità di visualizzare gli aggiornamenti CSS in tempo reale.
    - Per altre informazioni, leggere [sincronizzazione automatica CSS e JavaScript il Mapping di selezione in controllo pagina](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Supporta l'evidenziazione della sintassi per CoffeeScript, Mustache, Handlebars e JsRender.
    - Editor HTML include Intellisense per le associazioni Knockout.
    - MINORE di modifica e il compilatore supportano per abilitare la compilazione di CSS dinamica con minore.
    - Incolla JSON come una classe .NET. Tramite questo comando Incolla speciale per incollare JSON in un linguaggio c# o file di codice Visual Basic.NET e Visual Studio genera automaticamente le classi .NET dedotte da JSON.
- Supporto dell'emulatore per dispositivi mobili aggiunge hook di estensibilità in modo che gli emulatori di terze parti possono essere installati come un'estensione VSIX. Gli emulatori installati verranno visualizzato nell'elenco a discesa F5, in modo che gli sviluppatori possono visualizzare in anteprima i siti Web in un'ampia gamma di dispositivi mobili. Altre informazioni su questa funzionalità nel post sul blog di Scott Hanselman [la nuova integrazione BrowserStack con Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Pubblicazione sul Web

- Progetti di siti Web è ora dispongono la stessa esperienza di pubblicazione come i progetti applicazione Web, inclusa la pubblicazione di siti Web di Azure.
- Pubblicazione selettiva &#8211; per uno o più file è possibile eseguire le azioni seguenti (dopo la pubblicazione in un endpoint di distribuzione Web): 

    - Pubblicare i file selezionati.
    - Notare la differenza tra un file locale e un file remoto.
    - Aggiornare il file locale con il file remoto o aggiornare il file remoto con il file locale.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Modelli ASP.NET MVC

- Il nuovo modello applicazione Facebook semplifica la scrittura di applicazioni Canvas per Facebook. In pochi semplici passaggi, è possibile creare un'applicazione Facebook che ottiene dati da un utente connesso e si integra con gli amici. Il modello include una nuova libreria per la gestione dei plumbing coinvolti nella creazione di app per Facebook, tra cui l'autenticazione, autorizzazioni, l'accesso ai dati di Facebook e altro ancora. Per altre informazioni sull'utilizzo del modello applicazione Facebook, vedere [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Un nuovo modello MVC per applicazioni a pagina singola consente agli sviluppatori di compilare applicazioni web interattive sul lato client, mediante HTML 5, CSS 3 e Knockout più diffusi e le librerie JavaScript di jQuery, nella parte superiore di API Web ASP.NET. Il modello include un'applicazione di elenco "todo" che illustra le procedure comuni per la creazione di un'applicazione JavaScript HTML5 che usa un'API server RESTful. È possibile leggere altre informazioni [ https://www.asp.net/single-page-application ](../single-page-application/index.md).
- È ora possibile creare un progetto VSIX che consente di aggiungere nuovi modelli alla finestra di dialogo Nuovo progetto ASP.NET MVC. Altre informazioni, vedere: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Pacchetto FixedDisplayModes &#8211; modelli di progetti MVC sono stati aggiornati per includere il nuovo pacchetto NuGet 'FixedDisplayModes', che contiene una soluzione alternativa per un bug in MVC 4. Per altre informazioni della correzione contenuti nel pacchetto, fare riferimento a questo post di blog ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) dal team di MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>API Web ASP.NET

API Web ASP.NET è stata migliorata con numerose nuove funzionalità:

- ASP.NET Web API OData
- Traccia API Web ASP.NET
- Pagina della Guida ASP.NET Web API

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData ti offre la flessibilità che ti servono per creare gli endpoint OData con una logica di business avanzata su qualsiasi origine dati. Con ASP.NET Web API OData è controllare la quantità di semantica di OData che si desidera esporre. ASP.NET Web API OData è incluso con i modelli di progetto ASP.NET MVC 4 e sono disponibile anche da NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData supporta attualmente le funzionalità seguenti:

- Abilitare la semantica di query OData applicando l'attributo [Queryable].
- Convalidare le query OData e limitare il set di opzioni di query supportate, operatori e funzioni con facilità.
- Parametro associato a ODataQueryOptions direttamente per ottenere una rappresentazione dell'albero sintattico astratto della query che possono essere convalidate e applicate a un oggetto IQueryable o IEnumerable.
- Abilitare il paging basato sul servizio e generazione del collegamento pagina successiva specificando limiti relativi risultato all'attributo [Queryable].
- Richiedere un conteggio del numero totale delle risorse corrispondenti usando $inlinecount impostato come inline.
- Controllare la gestione della propagazione null.
- Qualsiasi o tutti gli operatori in $filter.
- In grado di dedurre un entity data model per convenzione o in modo esplicito personalizzare un modello in modo analogo a Entity Framework Code First.
- Set di entità di esporre mediante la derivazione da EntitySetController.
- Convenzioni di semplici e personalizzabili per esporre le proprietà di navigazione, la modifica dei collegamenti e implementare le azioni OData.
- Semplificata di routing usando il metodo di estensione MapODataRoute.
- Supporto per il controllo delle versioni tramite l'esposizione di più modelli EDM.
- Esporre $metadata e documento di servizio in modo che è possibile generare client (.NET, Windows Phone, Windows Store e così via) per l'API Web.
- Supporto per i formati OData Atom, JSON e JSON dettagliati.
- Creare, aggiornare, parzialmente aggiornamento (PATCH) ed eliminare entità.
- Query e modificare le relazioni tra entità.
- Creare collegamenti di relazione che collegare un massimo di route.
- Tipi complessi.
- Ereditarietà dei tipi di entità.
- Proprietà della raccolta.
- Enumerazioni.
- Azioni OData.
- Compilato in base stessa base di WCF Data Services, vale a dire ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Per altre informazioni su ASP.NET Web API OData, vedere [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Traccia API Web ASP.NET

Traccia API Web ASP.NET si integra i dati di traccia alle API web con la traccia di .NET. Si è ora abilitato per impostazione predefinita nel modello di progetto API Web. Analisi dei dati per il web API viene inviata alla finestra di Output e vengono rese disponibili tramite IntelliTrace. La traccia di ASP.NET Web API consente all'utente di informazioni di traccia relative all'API Web quando è ospitato in Windows Azure tramite l'integrazione con [diagnostica di Microsoft Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). È anche possibile installare e abilitare la traccia di ASP.NET Web API in qualsiasi applicazione che usa il pacchetto NuGet di traccia ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Per altre informazioni sulla configurazione e sull'uso di traccia di ASP.NET Web API, vedere [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Pagina della Guida ASP.NET Web API

La pagina della Guida di ASP.NET Web API è ora incluso per impostazione predefinita nel modello di progetto API Web. La pagina della Guida di ASP.NET Web API genera automaticamente la documentazione per le API, compresi gli endpoint HTTP, i metodi HTTP supportati, i parametri e i payload di messaggi di richiesta e risposta di esempio web. Documentazione di viene automaticamente effettuato il pull dai commenti nel codice. È anche possibile aggiungere la pagina della Guida di ASP.NET Web API a qualsiasi applicazione con il pacchetto NuGet di ASP.NET Web API utili Page ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Per altre informazioni sulla configurazione e personalizzazione vedere la pagina della Guida ASP.NET Web API [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR rende più semplice per aggiungere funzionalità web in tempo reale per l'applicazione ASP.NET, usando WebSocket, se disponibile e automaticamente eseguire il fallback su altre tecniche quando non lo è.

Per altre informazioni sull'uso di ASP.NET SignalR, vedere [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>URL brevi di ASP.NET

FriendlyURLs ASP.NET è molto semplice per gli sviluppatori di web form generare più chiaro alla ricerca degli URL (senza l'estensione aspx). Non richiede a nessuna configurazione minima e può essere utilizzato con le applicazioni ASP.NET v4.0 esistenti. La funzionalità FriendlyURLs rende anche più semplice per gli sviluppatori di aggiungere il supporto per dispositivi mobili alle applicazioni, supportando il passaggio tra le visualizzazioni per dispositivi mobili e desktop.

Per altre informazioni sull'installazione e utilizzo di URL brevi di ASP.NET, vedere [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo nella versione di ASP.NET and Web Tools 2012.2.

### <a name="installation-issues"></a>Problemi di installazione

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Installazioni non in ordine di Visual Studio 2012

Installazione di un aggiuntivi dello SKU di Visual Studio 2012 dopo l'installazione di ASP.NET and Web Tools 2012.2 richiederanno un'operazione di ripristino. Si consideri la seguente sequenza:

1. Installare Visual Studio Express 2012 per Web
2. Installare ASP.NET and Web Tools 2012.2
3. Installare Visual Studio 2012 Professional, Premium o Ultimate

Passaggio 2 produrrebbe solo installazione degli aggiornamenti per le edizioni Express per Web. Per assicurarsi che lo SKU aggiuntivo installato durante il passaggio 3 include l'aggiornamento si sarà necessario ripristinare il ASP.NET and Web Tools 2012.2 per installare gli aggiornamenti per lo SKU ultimo installato. Questo vale anche se le SKU nel passaggio 1 e 3 vengono invertiti.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Installazione di Microsoft ASP.NET and Web Tools 2012.2 quando Visual Studio è aperto

Se Visual Studio è aperto durante l'installazione di Microsoft ASP.NET and Web Tools 2012.2, Visual Studio possono trovarsi in uno stato non valido. Si consiglia agli utenti di chiudere tutte le istanze di Visual Studio prima di procedere con l'installazione.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Annullamento dell'installazione ASP.NET and Web Tools 2012.2 al centro installazione

Annullamento di ASP.NET and Web Tools 2012.2 il programma di installazione al centro installazione lascerà Visual Studio in uno stato non valido. Per risolvere questo problema, eseguire la procedura seguente: 

- Vai a Installazione applicazioni.
- Disinstallare Microsoft ASP.NET and Web Tools 2012.2, se presente.
- Reinstallare Microsoft ASP.NET and Web Tools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Dopo la disinstallazione di ASP.NET and Web Tools 2012.2 ASP.NET MVC 4 modelli e i modelli di sito Web di Razor v2 non sono presenti

Disinstallazione di ASP.NET and Web Tools 2012.2 tutti ASP.NET MVC 4 e modelli di siti Web di Razor v2 verrà anche disinstallare da Visual Studio 2012.

La soluzione alternativa consiste nel ripristinare l'installazione di Visual Studio 2012 per reinstallare ASP.NET MVC 4 e modelli di siti Web di Razor v2.

### <a name="tooling-issues"></a>Problemi relativi agli strumenti

#### <a name="nuget-error-reported-during-project-creation"></a>Errore di NuGet segnalati durante la creazione del progetto

Dopo l'installazione di ASP.NET and Web Tools 2012.2 venga visualizzato l'errore seguente quando si crea un progetto MVC 4

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

Di ASP.NET and Web Tools 2012.2 fornito NuGet 2.1 e aggiornerà l'estensione in Visual Studio 2012. In alcuni casi, il programma di installazione VSIX avrà esito negativo aggiornare correttamente il progetto VSIX. La procedura seguente consentirà di risolvere questo problema:

1. Avviare Visual Studio 2012 come amministratore
2. Passare a strumenti -&gt;estensioni e aggiornamenti e disinstallare NuGet.
3. Chiudere Visual Studio.
4. Passare alla cartella di installazione di ASP.NET and Web Tools 2012.2:

    1. Per Visual Studio 2012: **Programmi\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Per Visual Studio Express 2012 per Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 per Web**
5. Fare doppio clic su NuGet.Tools.vsix reinstallare NuGet

### <a name="web-api-issues"></a>Problemi di API Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>I problemi in $filter e valori letterali di data/ora di analisi

Il parser URI OData non riesce analizzare correttamente i valori letterali datetime parziale. Ad esempio, $filter = valore datetime di inizio eq'2012-12-31T12:00' non può essere analizzata correttamente. Soluzione alternativa consiste nell'usare il $filter completo valore letterale, valore datetime di inizio eq'2012 =-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData non supporta i nomi di proprietà tra maiuscole e minuscole.

OData non supporta i nomi delle proprietà di distinzione tra maiuscole e le query OData e il percorso odata. Gli elementi di lavoro, vedere:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Se gli utenti dispongono di maiuscole e minuscole diverse in javascript client e lato server, viene probabilmente verrà rilevato il problema. Questo problema è per impostazione predefinita nel protocollo odata. Tuttavia, molti utenti segnala il problema. Per risolvere questo problema, gli utenti devono risolvere i casi nell'URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>OData predefinito le convenzioni di routing non supporta POST o PUT sulla proprietà di navigazione.

OData predefinito le convenzioni di routing non supporta POST o PUT sulla proprietà di navigazione. Vedere workitem [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Assenza di questa convenzione di uso comune in convenzioni predefinite.

Per risolvere questo problema, gli utenti devono estendere nuova convenzione di routing per supportarlo.

### <a name="facebook-template-issues"></a>Problemi del modello Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Modello applicazione Facebook funziona solo con .NET 4.5

.NET 4.5 è necessario selezionare dall'elenco a discesa framework nella finestra di dialogo Nuovo progetto per visualizzare il modello applicazione Facebook in ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Controller di aggiornamenti in tempo reale

Il modello applicazione Facebook consente facilmente di utente di creare un Controller API Web per gestire gli aggiornamenti in tempo reale provenienti da Facebook. Se il computer di sviluppo è dietro un NAT, il Controller potrebbe non funzionare senza ulteriore configurazione di rete. Per informazioni dettagliate, vedere qui: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Query valori stringa siano in conflitto con i parametri OAuth di Facebook

I campi seguenti sono in conflitto con la chiamata della finestra di dialogo di OAuth di Facebook eseguire il URL. Non aggiungere i propri valori di stringa di query con i nomi seguenti: codice di errore, errore\_la descrizione, errore\_motivo.

#### <a name="using-page-inspector-with-facebook-template"></a>Utilizzo di controllo pagina con il modello di Facebook

È possibile usare la funzionalità di controllo pagina in Visual Studio 2012 durante il debug dell'applicazione Facebook. Controllo pagina non supporta attualmente gli IFRAME.

### <a name="single-page-application-template-issues"></a>Problemi del modello applicazione singola pagina

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Con JQuery update 1.9/Knockout 2.2.1, durante l'esecuzione di progetto di applicazione a singola pagina MVC predefinita, nuova modifica elemento todo immettere evento dello stato attivo non viene gestito correttamente.

Con JQuery 1.9/Knockout 2.2.1 l'aggiornamento, quando si esegue il progetto di applicazione a singola pagina MVC predefinita, nuova modifica elemento todo immettere non più lo stato attivo alla casella di modifica nuovo elemento todo dopo aver immesso il nuovo elemento todo all'elenco attività.

Per riferimento soluzione alternativa [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)e apportare correzione simile al codice di esempio seguente:

File todo.model.js  
 funzione todolist(data), aggiungere seguenti:  
 **self.isSelected = ko.observable(false);**

funzione todoList.prototype.addTodo, aggiungere il testo blacked seguente:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

File index. cshtml, aggiungere il testo blacked seguente:  
 &lt;form data-bind=&quot;submit: addTodo&quot;&gt;  
 &lt;classe di input =&quot;addTodo&quot; tipo =&quot;testo&quot; data-bind =&quot;valore: newTodoTitle, segnaposto: 'Type aggiungere altra qui', blurOnEnter: true, **hasfocus: isSelected**, evento: {blur: addTodo}&quot; /&gt;  
 &lt;/form&gt;
