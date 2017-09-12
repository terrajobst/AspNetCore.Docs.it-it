---
title: Panoramica di viste
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 3b33c13f2385d3b07ba9b6f0bc0fd560abc3735c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a>Il rendering di HTML con le viste in ASP.NET MVC di base

Da [Steve Smith](https://ardalis.com/)

Controller MVC Core ASP.NET può restituire risultati formattati tramite *viste*.

## <a name="what-are-views"></a>Quali sono le visualizzazioni?

Nel modello Model-View-Controller (MVC), il *vista* incapsula i dettagli di presentazione di interazione dell'utente con l'app. Le visualizzazioni sono modelli HTML con codice incorporato che generano contenuto da inviare al client. Visualizzazioni utilizzano [sintassi Razor](razor.md), che consente l'interazione con codice HTML con quantità minima di codice o conferimento del codice.

Le visualizzazioni ASP.NET MVC di base sono *. cshtml* file archiviati per impostazione predefinita in un *viste* cartella all'interno dell'applicazione. In genere, ogni controller disporrà cartella corrispondente, in cui sono viste per le azioni del controller specifico.

![Cartella Views in Esplora soluzioni](overview/_static/views_solution_explorer.png)

Oltre alle viste di azione specifici, [visualizzazioni parziali](partial.md), [layout e altri file di visualizzazione speciale](layout.md) può essere utilizzato per ridurre la ripetizione e consentire di riutilizzare all'interno delle visualizzazioni dell'app.

## <a name="benefits-of-using-views"></a>Vantaggi dell'utilizzo di viste

Forniscono visualizzazioni [separazione delle problematiche](http://deviq.com/separation-of-concerns/) all'interno di un'applicazione MVC, che incapsula markup livello di interfaccia utente separatamente dalla logica di business. ASP.NET MVC visualizzazioni utilizzano [sintassi Razor](razor.md) per cambiare HTML markup e server logica sul lato semplice. Aspetti comuni, ricorrenti dell'interfaccia utente dell'applicazione possono essere riutilizzati facilmente tra viste utilizzando [layout e direttive condivise](layout.md) o [visualizzazioni parziali](partial.md).

## <a name="creating-a-view"></a>Creazione di una vista

Le viste che sono specifiche di un controller vengono create nel *viste / [ControllerName]* cartella. Le viste che vengono condivisi tra i controller vengono inserite nel */visualizzazioni/Shared* cartella. Nome del file di visualizzazione come l'azione del controller associato e aggiungere il *. cshtml* estensione di file. Ad esempio, per creare una vista per la *su* azione sul *Home* controller, è necessario creare il *About.cshtml* file nel  * /visualizzazioni/Home*cartella.

Un file di vista di esempio (*About.cshtml*):

[!code-html[Principale](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* codice è indicato il `@` simbolo. C# istruzioni vengono eseguite all'interno di blocchi di codice disattivare le parentesi graffe Razor (`{` `}`), ad esempio l'assegnazione di "About" per il `ViewData["Title"]` elemento illustrato in precedenza. Razor può essere utilizzato per visualizzare i valori all'interno di HTML facendo riferimento il valore con il `@` symbol, come illustrato nel `<h2>` e `<h3>` elementi sopra riportato.

Questa vista è incentrata sulla solo la parte dell'output di cui è responsabile. Il resto del layout della pagina e altri aspetti comuni della visualizzazione, vengono specificati in un' posizione. Altre informazioni, vedere [layout e la logica di visualizzazione condivisa](layout.md).

## <a name="how-do-controllers-specify-views"></a>Come viste specificare controller?

Le visualizzazioni vengono in genere restituite dalle azioni come un `ViewResult`. Metodo di azione, è possibile creare e restituire un `ViewResult` direttamente, ma in genere se il controller eredita da `Controller`, si utilizzerà semplicemente il `View` metodo di supporto, come in questo esempio viene illustrato:

*HomeController. cs*

[!code-csharp[Principale](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Il `View` metodo di supporto contiene diversi overload per rendere più semplice la restituzione di visualizzazioni per gli sviluppatori di app. È possibile specificare facoltativamente una visualizzazione da restituire, nonché un oggetto modello da passare alla visualizzazione.

Quando questa azione viene restituito, il *About.cshtml* visualizzazione indicata sopra viene eseguito il rendering:

![Informazioni sulla pagina](overview/_static/about-page.png)

### <a name="view-discovery"></a>Individuazione di visualizzazione

Quando un'azione restituisce una visualizzazione, un processo denominato *individuazione visualizzazione* avviene. Questo processo determina quali file di visualizzazione verrà utilizzato. A meno che non viene specificato un file di visualizzazione, il runtime cercherà una visualizzazione specifica del controller, quindi cerca innanzitutto il nome di visualizzazione corrispondente nel *Shared* cartella.

Quando un'azione restituisce il `View` metodo, ad esempio `return View();`, il nome dell'azione viene utilizzato come nome della vista. Ad esempio, se è stato chiamato da un metodo di azione denominato "Index", sarebbe equivalente al passaggio di un nome di vista di "Index". Nome di una vista può essere passato al metodo in modo esplicito (`return View("SomeView");`). In entrambi i casi, visualizzare l'individuazione Cerca un file di visualizzazione corrispondente in:

   1. Viste /\<ControllerName > /\<ViewName >. cshtml

   2. Viste/Shared/\<ViewName >. cshtml

>[!TIP]
> Si consiglia di seguenti la convenzione di restituire semplicemente `View()` dalle azioni quando possibile, poiché comporta più flessibile e più semplice per il refactoring del codice.

Invece di un nome di visualizzazione, è possibile specificare un percorso di file di visualizzazione. Se si utilizza un percorso assoluto a partire dalla radice dell'applicazione (facoltativamente inizia con "/" o "~ /"), il *. cshtml* estensione deve essere specificata come parte del percorso del file (ad esempio, `return View("Views/Home/About.cshtml");`). In alternativa, è possibile utilizzare un percorso relativo dalla directory del controller specifico all'interno di *viste* directory per specificare le viste in directory diverse (ad esempio, `return View("../Manage/Index");` all'interno del `HomeController`). Analogamente, è possibile scorrere la directory di specifici controller corrente (ad esempio, `return View("./About");`). Si noti che i percorsi relativi non usano il *. cshtml* estensione. Come accennato in precedenza, seguire la procedura consigliata di organizzare la struttura dei file per le viste in modo da riflettere le relazioni tra i controller, azioni e visualizzazioni per maggiore chiarezza e la manutenibilità.

> [!NOTE]
> [Le visualizzazioni parziali](partial.md) e [visualizzare componenti](view-components.md) utilizzare meccanismi di individuazione simile (ma non identica).

> [!NOTE]
> È possibile personalizzare la convenzione predefinita relative in cui le visualizzazioni si trovano all'interno dell'app usando un oggetto personalizzato `IViewLocationExpander`.

>[!TIP]
> Nomi di visualizzazione possono essere tra maiuscole e minuscole in base al file system sottostante. Per garantire la compatibilità tra i sistemi operativi, è sempre maiuscole/minuscole tra controller e i nomi di azione e le cartelle di visualizzazione associata e i nomi di file.

## <a name="passing-data-to-views"></a>Passaggio di dati a viste

È possibile passare dati alle viste utilizzando diversi meccanismi. L'approccio più efficace consiste nello specificare un *modello* tipo nella visualizzazione (conosciuta come un *viewmodel*, per distinguerlo dai tipi di modello di dominio aziendale) e quindi passare un'istanza di questo tipo per la visualizzazione dall'azione. È consigliabile che utilizzare un modello o modello di visualizzazione per passare dati a una visualizzazione. In questo modo la visualizzazione di sfruttare il controllo dei tipi sicuri. È possibile specificare un modello per una visualizzazione utilizzando il `@model` direttiva:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

Una volta che un modello è stato specificato per una vista, l'istanza inviato per la visualizzazione è possibile accedere in modo fortemente tipizzato tramite `@Model` come illustrato in precedenza. Per fornire un'istanza del tipo di modello per la visualizzazione, il controller passa come parametro:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
public IActionResult Contact()
   {
       ViewData["Message"] = "Your contact page.";

       var viewModel = new Address()
       {
           Name = "Microsoft",
           Street = "One Microsoft Way",
           City = "Redmond",
           State = "WA",
           PostalCode = "98052-6399"
       };
       return View(viewModel);
   }
   ```

Non sono previste restrizioni per i tipi che possono essere forniti per una vista come modello. È consigliabile passare oggetti poco (Plain Old CLR Object) di visualizzazione di modelli con pochi o nessun comportamento, in modo che la logica di business può essere incapsulata in un' posizione nell'app. Un esempio di questo approccio è la *indirizzo* viewmodel utilizzato nell'esempio precedente:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
namespace WebApplication1.ViewModels
   {
       public class Address
       {
           public string Name { get; set; }
           public string Street { get; set; }
           public string City { get; set; }
           public string State { get; set; }
           public string PostalCode { get; set; }
       }
   }
   ```

> [!NOTE]
> Non impedisce di utilizzare le stesse classi come tipi di modello di business e i tipi di modello di visualizzazione. Tuttavia, mantenendoli separato consente le visualizzazioni variare in modo indipendente dal modello di dominio o persistenza e può offrire alcuni vantaggi di sicurezza nonché (per i modelli che invieranno agli utenti per l'app mediante [associazione del modello](../models/model-binding.md)).

### <a name="loosely-typed-data"></a>Dati non tipizzati

Oltre a visualizzazioni fortemente tipizzate, tutte le visualizzazioni hanno accesso a una raccolta di dati non tipizzato. È possibile fare riferimento a questa raccolta stessa tramite la `ViewData` o `ViewBag` proprietà sul controller e visualizzazioni. Il `ViewBag` proprietà è un wrapper `ViewData` che fornisce una visualizzazione dinamica in tale raccolta. Non è una raccolta separata.

`ViewData`un oggetto dizionario avviene tramite `string` chiavi. È possibile archiviare e recuperare gli oggetti in essa contenuti e dovrai cast a un tipo specifico quando si estrae li. È possibile utilizzare `ViewData` per passare i dati da un controller per le visualizzazioni, nonché all'interno di viste (e le visualizzazioni parziali e layout). Dati di tipo stringa possono essere archiviati e utilizzati direttamente, senza la necessità di un cast.

Impostare alcuni valori per `ViewData` un'azione:

```csharp
public IActionResult SomeAction()
   {
       ViewData["Greeting"] = "Hello";
       ViewData["Address"]  = new Address()
       {
           Name = "Steve",
           Street = "123 Main St",
           City = "Hudson",
           State = "OH",
           PostalCode = "44236"
       };

       return View();
   }
   ```

Utilizzare i dati in una vista:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

Il `ViewBag` oggetti fornisce l'accesso dinamico per gli oggetti archiviati in `ViewData`. Può essere più semplice da utilizzare in quanto non richiede il cast. Nell'esempio stesso come in precedenza, mediante `ViewBag` anziché un oggetto fortemente tipizzato `address` istanza nella visualizzazione:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> Poiché entrambi si riferiscono allo stesso sottostante `ViewData` insieme, è possibile combinare e trovare una corrispondenza tra `ViewData` e `ViewBag` durante la lettura e scrittura di valori, se pratico.

### <a name="dynamic-views"></a>Visualizzazioni dinamiche

Viste non dichiarano un tipo di modello, ma hanno un'istanza del modello vengano passata possono fare riferimento l'istanza in modo dinamico. Ad esempio, se un'istanza di `Address` viene passato a una vista che non si dichiara un `@model`, la visualizzazione sarà comunque in grado di fare riferimento alle proprietà dell'istanza in modo dinamico, come illustrato:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

Questa funzionalità può offrire una certa flessibilità, ma non offre alcun protezione compilazione IntelliSense. Se la proprietà non esiste, la pagina avrà esito negativo in fase di esecuzione.

## <a name="more-view-features"></a>Altre funzionalità di visualizzazione

[Gli helper di tag](tag-helpers/intro.md) rendono più facile aggiungere un comportamento lato server per i tag HTML esistenti, evitando la necessità di utilizzare codice personalizzato o helper all'interno di viste. Gli helper di tag vengono applicati come attributi agli elementi HTML, che vengono ignorati dall'editor che non ha familiarità con essi, che consente di visualizzare i tag essere modificato e il rendering in una varietà di strumenti. Gli helper di tag di SQL e in particolare è possibile effettuare [utilizzo dei moduli](working-with-forms.md) molto più semplice.

Generazione di codice HTML personalizzato può essere ottenuta con molti helper HTML incorporati, e una logica più complessa dell'interfaccia utente (potenzialmente con i propri requisiti di dati) può essere incapsulata [Visualizza i componenti](view-components.md). Visualizza i componenti forniscono la stessa separazione delle problematiche che offrono controller e visualizzazioni e consente di eliminare la necessità di azioni e le viste per gestire i dati utilizzati dagli elementi dell'interfaccia utente comuni.

Come molti altri aspetti di ASP.NET Core, viste supportano [inserimento di dipendenze](../../fundamentals/dependency-injection.md), consentendo di servizi da [inserite nelle viste](dependency-injection.md).
