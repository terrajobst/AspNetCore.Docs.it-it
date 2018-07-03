---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (include ad aprile 2011 Tools Update) ASP.NET MVC 3 è un framework per la creazione di applicazioni web scalabile, basata su standard usando il modello di progettazione consolidati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: ed3bf81943ac2ffa143cf169c87b3012269d9e77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375791"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(include ad aprile 2011 Tools Update)*
> 
> ASP.NET MVC 3 è un framework per compilare applicazioni scalabili, basati su standard web usando le potenzialità di ASP.NET e .NET Framework e modelli di progettazione consolidati.
> 
> Viene installato side-by-side con ASP.NET MVC 2, in modo da iniziare a usarlo subito!
> 
> Scaricare il [programma di installazione qui](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Funzionalità principali

- Sistema di Scaffolding integrato estendibile tramite NuGet
- Modelli di progetto abilitato 5 HTML
- Viste espressive, tra cui il nuovo motore di visualizzazione Razor
- Hook potenti con inserimento delle dipendenze e i filtri azione globali
- Supporto per Rich JavaScript con associazione JSON, convalida di jQuery e JavaScript non intrusivo
- *Leggere l'elenco completo di funzionalità [sotto](#overview)*

## <a name="top-links"></a>Collegamenti principali

What ' s New in ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 rilasciato](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC 3, WebMatrix, NuGet, IIS Express e Orchard rilasciato - la versione di gennaio Web di Microsoft nel contesto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [annuncio versione di ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Note sulla versione di ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Installazione e la Guida

- Installare ASP.NET MVC 3 usando il [installazione guidata piattaforma Web (scelta consigliata)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Installare ASP.NET MVC 3 usando il [programma di installazione eseguibile](https://go.microsoft.com/fwlink/?LinkID=208140)
- Installare [ASP.NET MVC 3 per Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lettura di [Introduzione all'esercitazione su ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Ottenere assistenza e discutere ASP.NET MVC 3 nel [forum](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Panoramica di ASP.NET MVC 3

ASP.NET MVC 3 si basa su ASP.NET MVC 1 e 2, aggiunta di eccezionali funzionalità che semplificano il codice e che consentono una maggiore estensibilità. In questo argomento viene fornita una panoramica di molte delle nuove funzionalità incluse in questa versione, organizzata nelle sezioni seguenti:

- [Lo Scaffolding con l'integrazione di MvcScaffold estendibile](#BM_MvcScaffolding)
- [Modelli di progetto abilitato 5 HTML](#BM_HTML5)
- [Il motore di visualizzazione Razor](#BM_TheRazorViewEngine)
- [Supporto per più motori di visualizzazione](#BM_Support_for_Multiple_View_Engines)
- [Miglioramenti di controller](#BM_Controller_Improvements)
- [JavaScript e Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Miglioramenti di convalida del modello](#BM_Model_Validation_Improvements)
- [Miglioramenti di inserimento delle dipendenze](#BM_Dependency_Injection_Improvements)
- [Altre nuove funzionalità](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Lo Scaffolding con l'integrazione di MvcScaffold estendibile

Il nuovo sistema di Scaffolding rende più semplice per prelevare e iniziare a usare in modo produttivo, se si è completamente nuova del Framework e automatizzare le attività comuni di sviluppo se si è esperti e conseguenze più familiari.

Questo è supportato da NuGet nuove *scaffolding* pacchetto chiamato **MvcScaffolding**. Il termine "Lo Scaffolding" viene usato da molte tecnologie di software per indicare "generare rapidamente una struttura di base del software che è possibile modificare e personalizzare". Il pacchetto di scaffolding che stiamo creando per ASP.NET MVC è molto utile in diversi scenari:

- **Se in fase di apprendimento ASP.NET MVC per la prima volta**, in quanto offre un modo rapido per ottenere alcune utili, codice funzionante, che è quindi possibile modificare e adattare in base alle esigenze. Evita così il drammatico esamina una pagina vuota e non che è chiaro dove iniziare!
- **Se si conosce bene ASP.NET MVC e sono ora esplorando alcune nuove tecnologie di componente aggiuntivo** , ad esempio un mapper relazionale a oggetti, un motore di visualizzazione, una libreria di test, e così via, poiché il creatore di tale tecnologia potrebbe aver creato anche un pacchetto di scaffolding per tale.
- **Se il lavoro implica la creazione di classi o i file di qualche tipo simili a ripetutamente**, poiché è possibile creare scaffolders personalizzato che restituiscono come output Fixture di test, gli script di distribuzione o qualsiasi altro componente è necessario. Chiunque nel team può usare il scaffolders personalizzati, troppo.

Altre funzionalità in MvcScaffolding includono:

- Supporto per i progetti c# e VB
- Il supporto per il Razor e ASPX della visualizzazione motori
- Supporta lo scaffolding in aree di ASP.NET MVC e utilizzando i layout/schemi di visualizzazione personalizzata
- È possibile personalizzare facilmente l'output modificando i modelli T4
- È possibile aggiungere scaffolders completamente nuova usando la logica personalizzata di PowerShell e i modelli T4 personalizzati. Questi (e i parametri personalizzati si) vengono visualizzati automaticamente nell'elenco di completamento tramite tasto tab della console.
- È possibile ottenere i pacchetti NuGet che contengono scaffolders aggiuntive per le tecnologie diverse (ad esempio, vi è un proof-of-concept uno per LINQ to SQL ora) e combinare e abbinare li insieme

ASP.NET MVC 3 Tools Update include eccezionale supporto di Visual Studio per questo sistema di scaffolding, ad esempio:

- Aggiungere che controller di finestra di dialogo supporta ora lo scaffolding automatico completo di Create, Read, Update e Delete azioni del controller e visualizzazioni corrispondenti. Per impostazione predefinita, questo esegue scaffolding delle codice di accesso ai dati tramite Entity Framework Code First.
- Aggiunge il supporto di finestra di dialogo Controller *esegue lo scaffolding estendibile* tramite NuGet pacchetti come *MvcScaffolding*. In questo modo l'inserimento di esegue lo scaffolding personalizzato nella finestra di dialogo che consentirebbe di creare esegue lo scaffolding per altre tecnologie di accesso ai dati, ad esempio NHibernate o persino JET con ODBCDirect se sta per maggiori informazioni.

Per ulteriori informazioni sullo Scaffolding di ASP.NET MVC 3, vedere le risorse seguenti:

- Steve Sanderson post-series, tra cui: 

    1. [Introduzione: Eseguire lo scaffolding del progetto ASP.NET MVC 3 con il pacchetto MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Utilizzo standard: casi d'uso tipici e opzioni](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relazioni uno-a-molti](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Le azioni di scaffolding e Unit test](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Si esegue l'override di modelli T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Questo post di: creazione scaffolders personalizzato](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Post di Scott Hanselman dalla sua sessione PDC 2010 [la creazione di un Blog con Microsoft "Senza nome pacchetto di Web amore"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Modelli di progetto 5 HTML

La finestra di dialogo Nuovo progetto include le versioni enable HTML 5 una casella di controllo di modelli di progetto. Questi modelli di sfruttare Modernizr 1.7 per fornire il supporto di compatibilità per CSS 3 e HTML5 nei browser di livello inferiore.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Il motore di visualizzazione Razor

ASP.NET MVC 3 viene fornito con un nuovo motore di visualizzazione denominato Razor che offre i vantaggi seguenti:

- Sintassi Razor è chiara e concisa e, che richiedono un numero minimo di sequenze di tasti.
- Razor è facile da imparare, in parte perché si basa su esistenti linguaggi come c# e Visual Basic.
- Visual Studio include IntelliSense e il codice colorazione della sintassi Razor.
- Le visualizzazioni Razor possono essere sottoposta a unit testata senza necessità di eseguire l'applicazione o avviare un server web.

Di seguito sono elencate alcune delle nuove funzionalità Razor:

- `@model` sintassi per specificare il tipo passato alla visualizzazione.
- `@* *@` sintassi di commento.
- La possibilità di specificare le impostazioni predefinite (ad esempio `layoutpage`) una volta per un intero sito.
- Il `Html.Raw` metodo per la visualizzazione di testo senza codifica HTML si.
- Supporto per la condivisione del codice tra più visualizzazioni (*\_viewstart* oppure  *\_viewstart.vbhtml* file).

Razor include anche nuovi helper HTML, come il seguente:

- `Chart`. Esegue il rendering di un grafico, che offre le stesse funzionalità come il controllo chart in ASP.NET 4.
- `WebGrid`. Esegue il rendering di una griglia dati, completa di funzionalità di impaginazione e ordinamento.
- `Crypto`. Usa gli algoritmi per creare correttamente l'hashing con salting e con hash delle password.
- `WebImage`. Esegue il rendering di un'immagine.
- `WebMail`. Invia un messaggio di posta elettronica.

Per ulteriori informazioni su Razor, vedere le risorse seguenti:

- [Post di blog Guthrie Introduzione a Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Introduzione a post di blog di Guthrie il @model (parola chiave)](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Introduzione a Razor layout il post di blog Guthrie](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor riferimento rapido sulle API](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Supporto per più motori di visualizzazione

Il **Aggiungi visualizzazione** finestra di dialogo in ASP.NET MVC 3 consente di scegliere il motore di visualizzazione che si desidera usare, e il **nuovo progetto** nella finestra di dialogo consente di specificare il motore di visualizzazione predefinito per un progetto. È possibile scegliere, ad esempio il motore di visualizzazione di Web Form (ASPX), Razor o un motore di visualizzazione open source [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), o [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Miglioramenti di controller

### <a name="global-action-filters"></a>Filtri azione globali

A volte si desidera eseguire la logica prima che venga eseguito un metodo di azione o dopo l'esecuzione di un metodo di azione. A tale scopo, ASP.NET MVC 2 fornito filtri dell'azione. I filtri azione sono attributi personalizzati che forniscono strumenti dichiarativi per aggiungere un comportamento pre-azione e post-azione ai metodi di azione di un controller specifico. Tuttavia, in alcuni casi si potrebbe voler specificare il comportamento di pre-azione o post-azione che si applica a tutti i metodi di azione. MVC 3 consente di specificare i filtri globali vengono aggiunte al `GlobalFilters` raccolta. Per altre informazioni sui filtri azione globali, vedere le risorse seguenti:

- [Blog di Guthrie all'anteprima 3 di MVC](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Applicazione di filtri in ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nuova proprietà "ViewBag"

Supporto per i controller MVC 2 un `ViewData` proprietà che consente di passare i dati a un modello di vista usando un dizionario con associazione tardiva API. In MVC 3, è anche possibile usare la sintassi un po' più semplice con il `ViewBag` proprietà allo stesso scopo. Ad esempio, invece di scrivere `ViewData["Message"]="text"`, è possibile scrivere `ViewBag.Message="text"`. Non è necessario definire tutte le classi fortemente tipizzate da utilizzare il `ViewBag` proprietà. Poiché si tratta di una proprietà dinamica, è possibile invece è sufficiente ottenere o impostare le proprietà e per risolverli in modo dinamico in fase di esecuzione. Internamente `ViewBag` delle proprietà vengono archiviate come coppie nome/valore nel `ViewData` dizionario. (Nota: nella maggior parte delle versioni non definitive di MVC 3, il `ViewBag` proprietà è denominata il `ViewModel` proprietà.)

### <a name="new-actionresult-types"></a>Nuovi tipi di "ActionResult"

Nell'esempio `ActionResult` tipi e metodi helper corrispondenti sono nuove o migliorate in MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Restituisce un codice di stato HTTP 404 al client.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Restituisce un reindirizzamento temporaneo (codice di stato HTTP 302) o un reindirizzamento permanente (codice di stato HTTP 301), a seconda di un parametro booleano. In combinazione con questa modifica, la [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) ora dispone di tre metodi per eseguire reindirizzamenti permanenti: `RedirectPermanent`, `RedirectToRoutePermanent`, e `RedirectToActionPermanent`. Questi metodi restituiscono un'istanza di `RedirectResult` con il `Permanent` impostata su `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Restituisce un codice di stato HTTP specificato dall'utente.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Miglioramenti di Ajax e JavaScript

Per impostazione predefinita, gli helper Ajax e convalida in MVC 3 usano un approccio JavaScript non intrusivo. JavaScript non intrusivo evita l'inserimento di JavaScript inline in HTML. Ciò rende il codice HTML più piccoli e meno risulti troppo affollato e rende più semplice sostituire o personalizzare le librerie JavaScript. Gli helper di convalida in MVC 3 usano anche il `jQueryValidate` plug-in per impostazione predefinita. Se si desidera che il comportamento di MVC 2, è possibile disabilitare JavaScript non intrusivo utilizzando un *Web. config* impostazione del file. Per altre informazioni sui miglioramenti apportati a JavaScript e Ajax, vedere le risorse seguenti:

- [Introduzione di base di JavaScript non intrusivo nel sito di Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Post di Brad Wilson Unobtrusive JavaScript](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Post di convalida JavaScript non intrusivo di Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Creazione di un'applicazione MVC 3 con Razor and Unobtrusive JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (esercitazione sul sito ASP.NET)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Convalida lato client abilitata per impostazione predefinita

Nelle versioni precedenti di MVC, è necessario chiamare in modo esplicito il `Html.EnableClientValidation` metodo da una vista per abilitare la convalida lato client. In MVC 3 questo non è più necessario perché la convalida lato client è abilitata per impostazione predefinita. (È possibile disabilitare questa usando un'impostazione di *Web. config* file.)

Affinché la convalida lato client lavorare, è comunque necessario fare riferimento il jQuery appropriato e le librerie di convalida di jQuery nel sito. È possibile ospitare le librerie nel proprio server o farvi riferimento da una rete di distribuzione di contenuti (CDN), ad esempio le reti CDN di Microsoft o Google.

### <a name="remote-validator"></a>Validator remoto

ASP.NET MVC 3 supporta le nuove [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) classe che consente di sfruttare la convalida di jQuery plug-del supporto tecnico di convalida remota. In questo modo la libreria di convalida lato client chiamare automaticamente un metodo personalizzato che definisce nel server per eseguire la logica di convalida che può essere eseguita solo sul lato server.

Nell'esempio seguente, il `Remote` attributo specifica che la convalida del client chiama un'azione denominata `UserNameAvailable` nel `UsersController` classe per convalidare il `UserName` campo.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

Nell'esempio seguente mostra il controller corrispondente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Per altre informazioni su come usare il `Remote` dell'attributo, vedere [procedura: implementare la convalida remota in ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) in MSDN library.

### <a name="json-binding-support"></a>Supporto dell'associazione JSON

ASP.NET MVC 3 include il supporto dell'associazione JSON incorporato che consente ai metodi di azione per la ricezione dei dati con codifica JSON e modello di associazione ai parametri del metodo di azione. Questa funzionalità è utile negli scenari che coinvolgono i modelli di client e il data binding. (Modelli di client consentono di formattare e visualizzare un singolo elemento dati o un set di elementi di dati con i modelli eseguiti nel client.) MVC 3 consente di connettere facilmente modelli di client con metodi di azione nel server che inviano e ricevono i dati JSON. Per altre informazioni sul supporto JSON per associazione, vedere la **JavaScript e AJAX miglioramenti** sezione [post di blog di MVC 3 Preview Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Miglioramenti di convalida del modello

### <a name="dataannotations-metadata-attributes"></a>Attributi dei metadati "DataAnnotations"

ASP.NET MVC 3 supporta `DataAnnotations` metadati attributi, ad esempio `DisplayAttribute`.

### <a name="validationattribute-class"></a>Classe "ValidationAttribute"

Il `ValidationAttribute` classe è stata migliorata in .NET Framework 4 per supportare un nuovo `IsValid` overload che fornisce ulteriori informazioni sul contesto di convalida corrente, ad esempio l'oggetto che viene convalidata. Questo consente scenari più avanzati in cui è possibile convalidare il valore corrente in un'altra proprietà del modello di base. Ad esempio, il nuovo `CompareAttribute` attributo consente di confrontare i valori delle due proprietà di un modello. Nell'esempio seguente, il `ComparePassword` proprietà deve corrispondere il `Password` campo per poter essere valida.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfacce di convalida

Il [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interfaccia consente di eseguire la convalida a livello di modello e consente di fornire la convalida dei messaggi di errore specifiche per lo stato del modello generale, o tra due proprietà all'interno del modello . MVC 3 ora recupera gli errori dal `IValidatableObject` durante l'associazione di modelli e automaticamente i flag di o evidenzia interessate campi all'interno di una vista utilizzando l'helper di form HTML predefiniti.

Il [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interfaccia consente a ASP.NET MVC individuare in fase di esecuzione se un servizio validator include il supporto per la convalida del client. Questa interfaccia è stata progettata in modo che può essere integrato con un'ampia gamma di Framework di convalida.

Per altre informazioni sulle interfacce di convalida, vedere la **miglioramenti del modello di convalida** sezione del [post di blog di MVC 3 Preview Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Si noti tuttavia che il riferimento a "IValidateObject" nel blog di deve essere "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Miglioramenti di inserimento delle dipendenze

ASP.NET MVC 3 fornisce un supporto migliore per l'applicazione di inserimento delle dipendenze e per l'integrazione con i contenitori di inserimento delle dipendenze o Inversion of Control (IOC). È stato aggiunto il supporto per l'inserimento delle dipendenze nelle aree seguenti:

- Controller (la registrazione e l'inserimento di controller factory, introducendo i controller).
- Viste (la registrazione e l'inserimento di motori di visualizzazione, l'inserimento di dipendenze in pagine di visualizzazione).
- Filtri di azione (individuazione e l'inserimento di filtri).
- Strumenti di associazione (la registrazione e l'inserimento).
- Provider di convalida di modelli (la registrazione e l'inserimento).
- Provider di metadati del modello (la registrazione e l'inserimento).
- Provider di valori (la registrazione e l'inserimento).

MVC 3 supporta la [localizzatore di servizi comune](https://github.com/unitycontainer/commonservicelocator) libreria e qualsiasi contenitore di inserimento delle dipendenze che supporta tale libreria `IServiceLocator` interfaccia. Supporta inoltre un nuovo `IDependencyResolver` interfaccia che rende più semplice integrare Framework di inserimento delle dipendenze.

Per altre informazioni sull'inserimento delle dipendenze in MVC 3, vedere le risorse seguenti:

- [Serie di Brad Wilson dei post di blog sul percorso del servizio](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Altre nuove funzionalità

### <a name="nuget-integration"></a>Integrazione di NuGet

ASP.NET MVC 3 installa automaticamente e consente a NuGet come parte della relativa configurazione. NuGet è Gestione pacchetti open source gratuita che rende più semplice trovare, installare e usare le librerie .NET e gli strumenti nei propri progetti. Funziona con tutti i tipi di progetto di Visual Studio (inclusi i Web Form ASP.NET e MVC ASP.NET).

NuGet consente agli sviluppatori che gestiscono i progetti open source (ad esempio progetti, ad esempio Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks ed Elmah) per le librerie del pacchetto e li registra in una raccolta online. È quindi facile per gli sviluppatori .NET che desiderano usare una di queste librerie per trovare il pacchetto e installarlo nei progetti a che cui stanno lavorando.

Con ASP.NET 3 Tools Update, modelli di progetto includono JavaScript librerie pacchetti NuGet preinstallati, in modo che siano aggiornabili tramite NuGet. Code First di Entity Framework viene pre-installato anche come pacchetto NuGet.

Per altre informazioni su NuGet, vedere la [documentazione di NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>La memorizzazione dell'Output parziale della pagina

ASP.NET MVC ha supportato la cache di output delle risposte pagina intera partire dalla versione 1. MVC 3 supporta anche la memorizzazione nella cache di output parziale della pagina, che consente di memorizzare facilmente le aree della cache o frammenti di una risposta. Per altre informazioni sulla memorizzazione nella cache, vedere la **parziale Page Output Caching** sezione del [post di blog Guthrie su MVC 3 versione finale candidata](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) e il **Output dell'azione figlio Caching** sezione il [note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Controllo granulare sulla convalida della richiesta

ASP.NET MVC è la convalida delle richieste predefinito che automaticamente consente di proteggersi contro attacchi intrusivi nel codice HTML e XSS. Tuttavia, talvolta si desidera disabilitare in modo esplicito la convalida richiesta, ad esempio se si desidera consentire agli utenti di pubblicare contenuto (ad esempio, nel post di blog o CMS contenuto) HTML. È ora possibile aggiungere un [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) attributo ai modelli o visualizzare i modelli per disabilitare la convalida delle richieste su una base per ogni proprietà durante l'associazione del modello. Per altre informazioni sulla convalida della richiesta, vedere le risorse seguenti:

- Il **JavaScript non intrusivo e convalida** sezione [post di blog Guthrie nella versione finale release candidate MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Extensible "nuovo progetto" nella finestra di dialogo

In ASP.NET MVC 3 è possibile aggiungere modelli di progetto, motori di visualizzazione, e unit test Framework di progetto per la **nuovo progetto** nella finestra di dialogo.

### <a name="template-scaffolding-improvements"></a>Miglioramenti ai modelli di Scaffolding

Modelli di scaffolding di ASP.NET MVC 3 sono preferibile che identificano le proprietà di chiave primaria per i modelli e gestirli in modo appropriato rispetto a versioni precedenti di MVC. (Ad esempio, i modelli di scaffolding verificare che la chiave primaria non lo scaffolding di un campo modulo modificabile.)

Per impostazione predefinita, la creazione e modifica delle strutture ora usano il `Html.EditorFor` helper anziché il `Html.TextBoxFor` helper. Ciò migliora il supporto per i metadati sul modello sotto forma di dati degli attributi di annotazione quando la **Aggiungi visualizzazione** una vista generata dalla finestra di dialogo.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nuovi overload per "Html.LabelFor" e "Html.LabelForModel"

Sono stati aggiunti nuovi overload di metodo per la `LabelFor` e `LabelForModel` metodi helper. I nuovi overload consentono di specificare o sostituire il testo dell'etichetta.

### <a name="sessionless-controller-support"></a>Supporto per il Controller senza sessione

In ASP.NET MVC 3 è possibile indicare se si desidera che una classe controller da utilizzare lo stato della sessione e in tal caso, se lo stato della sessione deve essere di lettura/scrittura o sola lettura. Per altre informazioni sul supporto per il controller di sessione, vedere [note sulla versione di MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nuova classe "AdditionalMetadataAttribute"

È possibile usare la [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) attributo per popolare il `ModelMetadata.AdditionalValues` dizionario per una proprietà del modello. Ad esempio, se un modello di visualizzazione dispone di una proprietà che deve essere visualizzata solo per gli amministratori, è possibile annotare tale proprietà come illustrato nell'esempio seguente:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Questi metadati viene resa disponibili per qualsiasi modello di visualizzazione o editor quando viene eseguito il rendering di un modello di visualizzazione del prodotto. È responsabilità dell'utente di interpretare le informazioni sui metadati.

### <a name="accountcontroller-improvements"></a>Miglioramenti di AccountController

AccountController nel modello di progetto Internet è stata notevolmente migliorata.

### <a name="new-intranet-project-template"></a>Nuovo modello di progetto Intranet

È incluso un nuovo modello di progetto Intranet che abilita l'autenticazione di Windows e rimuove il AccountController.
