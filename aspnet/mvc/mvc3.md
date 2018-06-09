---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (aprile 2011 include strumenti di aggiornamento) ASP.NET MVC 3 è un framework per la creazione di applicazioni web scalabili e basate su standard usando il modello di struttura consolidati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: c7eee987b28a5d7f8b40fe89a7bf7517ec06646f
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034737"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(aprile 2011 include strumenti di aggiornamento)*
> 
> ASP.NET MVC 3 è un framework per la compilazione di applicazioni web scalabili e basate su standard utilizzando schemi progettuali ben definiti e le potenzialità di ASP.NET e .NET Framework.
> 
> L'installazione side-by-side con ASP.NET MVC 2, in modo da iniziare a usarlo subito!
> 
> Scaricare il [installer qui](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Funzionalità principali

- Sistema integrato di Scaffolding estensibile tramite NuGet
- Modelli di progetto abilitato 5 HTML
- Viste espressive, tra cui il nuovo motore di visualizzazione Razor
- Di accedere con l'inserimento di dipendenze e i filtri azione globali
- Supporto di Rich JavaScript con JavaScript non intrusivo, convalida jQuery e associazione di JSON
- *Leggere l'elenco completo delle funzionalità [sotto](#overview)*

## <a name="top-links"></a>Collegamenti superiore

Novità di ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 rilasciato](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3, WebMatrix, NuGet, IIS Express e Orchard rilasciati per volta, la versione di gennaio Web di Microsoft nel contesto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [annuncio versione di ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Note sulla versione di ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installazione e la Guida

- Installa ASP.NET MVC 3 utilizzando il [installazione guidata piattaforma Web (scelta consigliata)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installa ASP.NET MVC 3 utilizzando il [programma di installazione eseguibile](https://go.microsoft.com/fwlink/?LinkID=208140)
- Installare [ASP.NET MVC 3 per anteprima sviluppatori Visual Studio 11](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lettura di [Intro to ASP.NET MVC 3 esercitazione](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Visualizzare la Guida e discutere ASP.NET MVC 3 nel [forum](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Panoramica di ASP.NET MVC 3

ASP.NET MVC 3 si basa su ASP.NET MVC 1 e 2, l'aggiunta di caratteristiche che semplificano il codice e consentire l'estensibilità più approfondita. In questo argomento viene fornita una panoramica di molte delle nuove funzionalità incluse in questa versione, organizzata nelle sezioni seguenti:

- [Scaffolding estendibile con l'integrazione MvcScaffold](#BM_MvcScaffolding)
- [Modelli di progetto abilitato 5 HTML](#BM_HTML5)
- [Il motore di visualizzazione Razor](#BM_TheRazorViewEngine)
- [Supporto per più motori di visualizzazione](#BM_Support_for_Multiple_View_Engines)
- [Miglioramenti di controller](#BM_Controller_Improvements)
- [JavaScript e Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Miglioramenti di convalida del modello](#BM_Model_Validation_Improvements)
- [Miglioramenti di inserimento di dipendenza](#BM_Dependency_Injection_Improvements)
- [Altre nuove funzionalità](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Scaffolding estendibile con l'integrazione MvcScaffold

Il nuovo sistema di Scaffolding rende più semplice per prelevare e iniziare a usare produttivo, se non si conosce completamente, il framework e automatizzare le comuni attività di sviluppo se si è esperti e conosce già cosa si sta eseguendo.

Questo è supportato da NuGet nuovo *scaffolding* pacchetto chiamato **MvcScaffolding**. Il termine "Scaffolding" viene utilizzato da numerose tecnologie di software per indicare "rapidamente la generazione di una struttura di base del software che è possibile modificare e personalizzare". Il pacchetto di scaffolding che si sta creando per ASP.NET MVC è molto utile in scenari diversi:

- **Se l'apprendimento ASP.NET MVC per la prima volta**, poiché offre un modo rapido per ottenere alcuni utili, codice funzionante, che è quindi possibile modificare e adattare in base alle esigenze. Evita il drammatico della ricerca in una pagina vuota e non è chiaro da dove iniziare!
- **Se si conosce bene ASP.NET MVC e sono ora esplorazione nuove tecnologie del componente aggiuntivo** , ad esempio un mapping relazionale a oggetti, un motore di visualizzazione, una libreria di test, e così via, perché il creatore di tale tecnologia può avere anche creato un pacchetto che lo scaffolding per tale.
- **Se il lavoro prevede la creazione di classi o i file di qualche tipo simili a ripetutamente**, in quanto è possibile creare scaffolders personalizzati degli strumenti di test, gli script di distribuzione o qualsiasi altro tipo di richiesta di output. Tutti i membri del team può utilizzare il scaffolders personalizzato, troppo.

Altre funzionalità in MvcScaffolding includono:

- Supporto per progetti c# e Visual Basic
- Il supporto per il Razor e ASPX visualizzare motori
- Supporta lo scaffolding in aree di ASP.NET MVC e utilizzando i layout/schemi di visualizzazione personalizzata
- È possibile personalizzare facilmente l'output modificando i modelli T4
- È possibile aggiungere scaffolders completamente nuovo utilizzando la logica personalizzata di PowerShell e i modelli T4 personalizzati. Questi (ed eventuali parametri personalizzati è stato concesso li) vengono visualizzati automaticamente nell'elenco di completamento tramite tab della console.
- È possibile ottenere i pacchetti NuGet contenente scaffolders aggiuntive per le tecnologie diverse (ad esempio, è disponibile un di prova uno per LINQ to SQL ora) e combinare e associarli insieme

ASP.NET MVC 3 Tools Update include il supporto di Visual Studio ottimale per il sistema lo scaffolding, ad esempio:

- Aggiungere la che finestra di dialogo Controller supporta ora lo scaffolding completamente automatico di Create, Read, Update e Delete azioni del controller e visualizzazioni corrispondenti. Per impostazione predefinita, questo strutture codice di accesso ai dati tramite Entity Framework Code First.
- Aggiunge il supporto di finestra di dialogo Controller *delle strutture extensible* tramite NuGet pacchetti come *MvcScaffolding*. In questo modo l'inserimento di strutture personalizzate nella finestra di dialogo che consente di creare strutture per altre tecnologie di accesso ai dati, ad esempio NHibernate o anche JET con ODBCDirect se sta inclinata di così!

Per ulteriori informazioni su Scaffolding di ASP.NET MVC 3, vedere le risorse seguenti:

- Steve Sanderson post-series, tra cui: 

    1. [Introduzione: Eseguire lo scaffolding di progetto ASP.NET MVC 3 con il pacchetto MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Utilizzo standard: casi di utilizzo tipici e opzioni](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relazioni uno-a-molti](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Le azioni che lo scaffolding e Unit test](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Si esegue l'override i modelli T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Questo post: creazione di scaffolders personalizzato](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Post di Scott Hanselman dalla sua sessione PDC 2010 [la creazione di un Blog con Microsoft "Senza nome pacchetto di Web piace"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Modelli di progetto 5 HTML

La finestra di dialogo Nuovo progetto include le versioni enable HTML 5 una casella di controllo di modelli di progetto. Questi modelli sfruttano Modernizr 1.7 per fornire il supporto di compatibilità per 5 HTML e CSS 3 nei browser di livello inferiore.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Il motore di visualizzazione Razor

ASP.NET MVC 3 viene fornito con un nuovo motore di visualizzazione denominato Razor che offre i vantaggi seguenti:

- La sintassi Razor è pulito e conciso, che richiedono un numero minimo di sequenze di tasti.
- Razor è facile da imparare, in parte perché si basa su esistenti linguaggi come c# e Visual Basic.
- Visual Studio include la colorazione IntelliSense e il codice per la sintassi Razor.
- Visualizzazioni Razor possono essere sottoposta a unit testata senza necessità di eseguire l'applicazione oppure avviare un server web.

Alcune nuove funzionalità Razor includono:

- `@model` sintassi per specificare il tipo passato alla visualizzazione.
- `@* *@` sintassi di commento.
- La possibilità di specificare le impostazioni predefinite (ad esempio `layoutpage`) una volta per un intero sito.
- Il `Html.Raw` metodo per la visualizzazione di testo senza codifica HTML è.
- Supporto per la condivisione del codice tra più visualizzazioni (*\_viewstart.cshtml* o  *\_viewstart.vbhtml* file).

Razor include anche nuovi helper HTML, ad esempio le operazioni seguenti:

- `Chart`. Esegue il rendering di un grafico, che offre le stesse funzionalità come il controllo chart in ASP.NET 4.
- `WebGrid`. Esegue il rendering di una griglia di dati, con funzionalità di ordinamento e paging.
- `Crypto`. Utilizza algoritmi per creare correttamente hash salt e hash delle password.
- `WebImage`. Esegue il rendering di un'immagine.
- `WebMail`. Invia un messaggio di posta elettronica.

Per ulteriori informazioni su Razor, vedere le risorse seguenti:

- [Post di blog Guthrie introduzione Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Post di blog Guthrie introdurre il @model (parola chiave)](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Introduzione a layout di Razor il post di blog Guthrie](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Riferimento API rapida Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Supporto per più motori di visualizzazione

Il **Aggiungi visualizzazione** la finestra di dialogo in ASP.NET MVC 3 consente di scegliere il motore di visualizzazione che si desidera utilizzare, e **nuovo progetto** la finestra di dialogo consente di specificare il motore di visualizzazione predefinito per un progetto. È possibile scegliere, ad esempio il motore di visualizzazione Web Form (ASPX), Razor o un motore di visualizzazione open source [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), o [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Miglioramenti di controller

### <a name="global-action-filters"></a>Filtri azione globali

Talvolta si desidera eseguire la logica prima dell'esecuzione di un metodo di azione o dopo l'esecuzione di un metodo di azione. A tale scopo, ASP.NET MVC 2 fornito filtri dell'azione. Filtri azione sono attributi personalizzati che forniscono strumenti dichiarativi per aggiungere un comportamento pre-azione e post-azione ai metodi di azione di un controller specifico. Tuttavia, in alcuni casi è consigliabile specificare il comportamento di pre- azione di o post-azione che si applica a tutti i metodi di azione. MVC 3 consente di specificare i filtri globali vengono aggiunte al `GlobalFilters` insieme. Per ulteriori informazioni sui filtri azione globali, vedere le risorse seguenti:

- [Relativo Guthrie sull'anteprima in MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Applicazione di filtri in ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nuova proprietà "ViewBag"

Supporto di controller MVC 2 un `ViewData` proprietà che consente di passare dati a un modello di visualizzazione tramite un dizionario ad associazione tardiva API. In MVC 3, è inoltre possibile utilizzare un po' più semplice sintassi con il `ViewBag` proprietà allo stesso scopo. Ad esempio, invece di scrittura `ViewData["Message"]="text"`, è possibile scrivere `ViewBag.Message="text"`. Non è necessario definire tutte le classi fortemente tipizzate per utilizzare il `ViewBag` proprietà. Perché è una proprietà dinamica, è invece possibile solo ottenere o impostare le proprietà e per risolverli in modo dinamico in fase di esecuzione. Internamente, `ViewBag` proprietà sono archiviate come coppie nome/valore di `ViewData` dizionario. (Nota: nella maggior parte delle versioni non definitive di MVC 3, il `ViewBag` proprietà è denominata la `ViewModel` proprietà.)

### <a name="new-actionresult-types"></a>Nuovi tipi di "ActionResult"

Le operazioni seguenti `ActionResult` tipi e metodi di supporto corrispondente sono nuove o migliorate in MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Restituisce un codice di stato HTTP 404 al client.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Restituisce un reindirizzamento temporaneo (codice di stato HTTP 302) o un reindirizzamento permanente (codice di stato HTTP 301), a seconda di un parametro booleano. In combinazione con questa modifica, il [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) dispone ora di tre metodi per l'esecuzione di reindirizzamenti permanenti: `RedirectPermanent`, `RedirectToRoutePermanent`, e `RedirectToActionPermanent`. Questi metodi restituiscono un'istanza di `RedirectResult` con il `Permanent` proprietà impostata su `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Restituisce un codice di stato HTTP specificato dall'utente.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Miglioramenti di Ajax e JavaScript

Per impostazione predefinita, gli helper di convalida e Ajax in MVC 3 usano un approccio di JavaScript non intrusivo. JavaScript non intrusivo evita di inserire inline JavaScript in HTML. Questo rende il codice HTML, inferiori e meno lineare e rende più semplice per lo swapping o personalizzare le librerie JavaScript. Gli helper di convalida in MVC 3 usano anche la `jQueryValidate` plug-in per impostazione predefinita. Se si desidera il comportamento di MVC 2, è possibile disabilitare JavaScript non intrusivo usando un *Web. config* impostazione del file. Per ulteriori informazioni sui miglioramenti di JavaScript e Ajax, vedere le risorse seguenti:

- [Introduzione di base per JavaScript non intrusivo nel sito di Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Post di blog di Brad Wilson non intrusivo JavaScript](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Post di blog di Brad Wilson JavaScript non intrusivo convalida](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Creazione di un'applicazione di MVC 3 con Razor and Unobtrusive JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (esercitazione sul sito ASP.NET)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Convalida lato client abilitata per impostazione predefinita

Nelle versioni precedenti di MVC, è necessario chiamare in modo esplicito il `Html.EnableClientValidation` metodo da una vista per abilitare la convalida lato client. In MVC 3 questo non è più necessario perché la convalida lato client è abilitata per impostazione predefinita. (È possibile disabilitare questa utilizzando un'impostazione di *Web. config* file.)

Affinché la convalida lato client funzionare, è necessario fare riferimento appropriato jQuery e jQuery librerie di convalida nel sito. È possibile ospitare tali librerie nel proprio server o fare riferimento a esse da una rete di distribuzione di contenuti (CDN) come CDN di Microsoft o Google.

### <a name="remote-validator"></a>Convalida remota

ASP.NET MVC 3 supporta il nuovo [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) classe che consente di sfruttare la convalida jQuery plug-del supporto di convalida remota. In questo modo la libreria di convalida lato client chiamare automaticamente un metodo personalizzato che definisce sul server per eseguire la logica di convalida che può essere eseguita solo sul lato server.

Nell'esempio seguente, il `Remote` attributo specifica che la convalida del client verrà richiamata un'azione denominata `UserNameAvailable` sul `UsersController` classe per convalidare il `UserName` campo.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Nell'esempio seguente viene illustrato il corrispondente controller.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Per ulteriori informazioni sull'utilizzo di `Remote` attributo, vedere [procedura: implementare la convalida remota in ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) in MSDN library.

### <a name="json-binding-support"></a>Supporto di associazione di JSON

ASP.NET MVC 3 include il supporto di associazione JSON incorporato che consente di metodi di azione per la ricezione dei dati con codifica JSON e modello di associazione ai parametri del metodo di azione. Questa funzionalità è utile in scenari che implicano client modelli e l'associazione di dati. (I modelli di client consentono di formattare e visualizzare un singolo elemento di dati o un set di elementi di dati utilizzando modelli eseguite sul client). MVC 3 consente di connettersi facilmente modelli client con i metodi di azione nel server che inviano e ricevono i dati JSON. Per ulteriori informazioni sul supporto di associazione JSON, vedere il **JavaScript e AJAX miglioramenti** sezione [post di blog di anteprima di MVC 3 di Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Miglioramenti di convalida del modello

### <a name="dataannotations-metadata-attributes"></a>Attributi dei metadati "DataAnnotations"

ASP.NET MVC 3 supporta `DataAnnotations` attributi di metadati quali `DisplayAttribute`.

### <a name="validationattribute-class"></a>Classe "ValidationAttribute"

Il `ValidationAttribute` classe è stata migliorata in .NET Framework 4 per supportare un nuovo `IsValid` overload che fornisce ulteriori informazioni sul contesto di convalida corrente, ad esempio l'oggetto da convalidare. In questo modo più dettagliati scenari in cui è possibile convalidare il valore corrente in base a un'altra proprietà del modello. Ad esempio, il nuovo `CompareAttribute` attributo consente di confrontare i valori delle due proprietà di un modello. Nell'esempio seguente, il `ComparePassword` proprietà deve corrispondere il `Password` campo per essere valide.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfacce di convalida

Il [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interfaccia consente di eseguire la convalida a livello di modello e consente di fornire la convalida dei messaggi di errore specifiche per lo stato del modello generale o tra due proprietà all'interno del modello . MVC 3 ora recupera gli errori di `IValidatableObject` durante l'associazione di modelli e automaticamente i flag di o evidenzia interessate campi all'interno di una vista utilizzando l'helper per form HTML incorporato.

Il [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interfaccia consente a ASP.NET MVC di individuare in fase di esecuzione se un validator dispone del supporto per la convalida del client. Questa interfaccia è stata progettata in modo che può essere integrata con un'ampia gamma di Framework di convalida.

Per ulteriori informazioni sulle interfacce di convalida, vedere il **miglioramenti di convalida del modello** sezione [post di blog di Scott Guthrie anteprima di MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Si noti tuttavia che il riferimento a "IValidateObject" nel blog di deve essere "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Miglioramenti di inserimento di dipendenza

ASP.NET MVC 3 fornisce un supporto migliore per l'applicazione Dependency Injection (DI) e per l'integrazione con i contenitori di inserimento di dipendenze o Inversion of Control (IOC). È stato aggiunto il supporto per DI nelle aree seguenti:

- Controller (registrazione e inserendo controller factory, inserendo i controller).
- Viste (registrazione e l'inserimento di motori di visualizzazione, l'inserimento di dipendenze in pagine di visualizzazione).
- Filtri dell'azione (individuazione e inserire i filtri).
- Raccoglitori di modelli (registrazione e inserimento).
- Provider di convalida di modelli (registrazione e inserimento).
- Provider di metadati di modello (registrazione e inserimento).
- Provider di valori (registrazione e inserimento).

MVC 3 supporta la [localizzatore di servizi comune](https://github.com/unitycontainer/commonservicelocator) libreria e qualsiasi contenitore che supporta tale libreria `IServiceLocator` interfaccia. Supporta inoltre un nuovo `IDependencyResolver` interfaccia che rende più semplice per l'integrazione DI Framework.

Per ulteriori informazioni DI MVC 3, vedere le risorse seguenti:

- [Serie di Brad Wilson di post di blog sul percorso del servizio](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Altre nuove funzionalità

### <a name="nuget-integration"></a>Integrazione di NuGet

ASP.NET MVC 3 automaticamente installa e Abilita NuGet come parte della relativa configurazione. NuGet è una gestione pacchetti open source gratuito che rende più semplice trovare, installare e utilizzare strumenti e librerie .NET nei progetti. Funziona con tutti i tipi di progetto di Visual Studio (incluso ASP.NET MVC e Web Form ASP.NET).

NuGet consente agli sviluppatori che gestiscono Apri origine progetti (ad esempio, ad esempio Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks ed Elmah) per le librerie del pacchetto e li registra in una raccolta in linea. È facile per gli sviluppatori .NET che desidera utilizzare una di queste librerie per trovare il pacchetto e installarlo nei progetti a che cui stanno lavorando.

Con ASP.NET 3 Tools Update, modelli di progetto includono JavaScript librerie pacchetti NuGet preinstallati, in modo che siano aggiornabili tramite NuGet. Code First di Entity Framework viene pre-installato anche come pacchetto NuGet.

Per altre informazioni su NuGet, vedere la [documentazione di NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>La memorizzazione nella cache di Output a pagina parziale

ASP.NET MVC è supportata la cache di output di risposta pagina intera fin dalla versione 1. MVC 3 supporta inoltre la memorizzazione nella cache di output a pagina parziale, che consente di memorizzare facilmente le aree della cache o frammenti di una risposta. Per ulteriori informazioni sulla memorizzazione nella cache, vedere il **nella cache di Output della pagina parziale** sezione [post di blog di Scott Guthrie su MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) e **figlio azione la cache di Output** sezione la [note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Controllo granulare convalida della richiesta

ASP.NET MVC è la convalida delle richieste predefinito che automaticamente aiuta a proteggersi da attacchi intrusivi nel codice HTML e CSS. Tuttavia, talvolta si desidera esplicitamente disattiva la convalida della richiesta, ad esempio se si desidera consentire agli utenti di registrare contenuto (ad esempio in post di blog o contenuto CMS) HTML. È ora possibile aggiungere un [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) attributo ai modelli o visualizzare i modelli per disabilitare la convalida della richiesta su una base per ogni proprietà durante l'associazione del modello. Per ulteriori informazioni sulla convalida richiesta, vedere le risorse seguenti:

- Il **convalida e JavaScript non intrusivo** sezione [post di blog di Scott Guthrie su MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Extensible "nuovo progetto" finestra di dialogo

In ASP.NET MVC 3 è possibile aggiungere modelli di progetto, i motori di visualizzazione e i framework di progetto di unit test di **nuovo progetto** la finestra di dialogo.

### <a name="template-scaffolding-improvements"></a>Miglioramenti ai modelli di Scaffolding

Modelli di scaffolding di ASP.NET MVC 3 presentano le prestazioni migliori per identificare le proprietà di chiave primaria su modelli e gestirli in modo appropriato rispetto alle versioni precedenti di MVC. (Ad esempio, i modelli di scaffolding ora assicurarsi che la chiave primaria non è scaffolding come un campo modulo modificabile.)

Per impostazione predefinita, la creazione e modifica delle strutture utilizzano ora il `Html.EditorFor` helper anziché il `Html.TextBoxFor` helper. Ciò migliora il supporto per i metadati sul modello sotto forma di dati quando gli attributi di annotazione di **Aggiungi visualizzazione** la finestra di dialogo Genera una visualizzazione.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nuovi overload per "Html.LabelFor" e "Html.LabelForModel"

Sono stati aggiunti nuovi overload di metodo per il `LabelFor` e `LabelForModel` metodi di supporto. I nuovi overload consentono di specificare o sostituire il testo dell'etichetta.

### <a name="sessionless-controller-support"></a>Supporto per il Controller di sessione

In ASP.NET MVC 3 è possibile indicare se si desidera che una classe controller da utilizzare lo stato della sessione e in tal caso, se lo stato della sessione deve essere di lettura/scrittura o sola lettura. Per ulteriori informazioni sul supporto di controller di sessione, vedere [note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nuova classe "AdditionalMetadataAttribute"

È possibile utilizzare il [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) attributi per popolare il `ModelMetadata.AdditionalValues` dizionario per una proprietà del modello. Se, ad esempio, un modello di visualizzazione dispone di una proprietà che deve essere visualizzata solo a un amministratore, è possibile annotare tale proprietà come illustrato nell'esempio seguente:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Questi metadati viene resa disponibili a qualsiasi modello di visualizzazione o editor quando viene eseguito il rendering di un modello di visualizzazione del prodotto. È responsabilità dell'utente di interpretare le informazioni sui metadati.

### <a name="accountcontroller-improvements"></a>Miglioramenti di AccountController

AccountController nel modello di progetto Internet è stata notevolmente migliorata.

### <a name="new-intranet-project-template"></a>Nuovo modello di progetto Intranet

È incluso un nuovo modello di progetto Intranet che consente l'autenticazione di Windows e rimuove il AccountController.
