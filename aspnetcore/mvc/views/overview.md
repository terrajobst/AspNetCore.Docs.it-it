---
title: Visualizzazioni in ASP.NET MVC di base
author: ardalis
description: Informazioni su come viste di gestiscono la presentazione dei dati dell'app e l'interazione dell'utente in ASP.NET MVC di base.
keywords: ASP.NET Core, visualizzare, MVC, razor, viewmodel, viewdata, viewbag
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 4530d2f500dd887bf649a753283fb3e4af995322
ms.sourcegitcommit: c2f6c593d81fbd90e6ddd672fe0a5636d06b615a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2017
---
# <a name="views-in-aspnet-core-mvc"></a>Visualizzazioni in ASP.NET MVC di base

Da [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)

Nel **M**enti -**V**ISTA -**C**modello ontroller (MVC), il *vista* gestisce l'interazione utente e presentazione dei dati dell'applicazione. Una vista è un modello HTML con incorporato [markup Razor](xref:mvc/views/razor). Markup Razor è codice che interagisce con il markup HTML per produrre una pagina Web che viene inviata al client.

In ASP.NET MVC di base, le visualizzazioni sono *. cshtml* file che utilizzano il [linguaggio di programmazione c#](/dotnet/csharp/) nel codice Razor. In genere, visualizzare i file sono raggruppati in cartelle denominate per ognuna dell'app [controller](xref:mvc/controllers/actions). Le cartelle vengono archiviate in un *viste* cartella principale dell'app:

![Esplora soluzioni di Visual Studio nella cartella viste è aperta con la cartella Home aprire per visualizzare i file cshtml About.cshtml e Contact.cshtml](overview/_static/views_solution_explorer.png)

Il *Home* controller è rappresentato da un *Home* cartella all'interno di *viste* cartella. Il *Home* cartella contiene le viste per il *su*, *contatto*, e *indice* pagine Web (Home Page). Quando un utente richiede uno di questi tre pagine Web, le azioni del controller nel *Home* controller determinare quale delle tre visualizzazioni viene utilizzato per compilare e restituire una pagina Web all'utente.

Utilizzare [layout](xref:mvc/views/layout) per fornire le sezioni di pagina Web coerente e ridurre ripetizioni nel codice. Layout contengono spesso l'intestazione, gli elementi di navigazione e i menu e il piè di pagina. L'intestazione e piè di pagina contengono in genere codice boilerplate per molti elementi di metadati e i collegamenti alle risorse di script e stile. Layout consentono di evitare questo markup boilerplate nelle visualizzazioni.

[Le visualizzazioni parziali](xref:mvc/views/partial) ridurre la duplicazione del codice mediante la gestione di parti riutilizzabili di viste. Ad esempio, una visualizzazione parziale è utile per una biografia autore in un sito Web del blog che viene visualizzato in diverse visualizzazioni. Una biografia autore è contenuto visualizzazione comune e non richiede codice da eseguire per produrre il contenuto per la pagina Web. Contenuto biografia autore è disponibile per la visualizzazione per l'associazione di modelli da solo, pertanto l'utilizzo di una visualizzazione parziale per questo tipo di contenuto è ideale.

[Visualizzare i componenti](xref:mvc/views/view-components) sono simili a parziale viste che consentono di ridurre il codice ripetitivo, ma appropriati per visualizzare il contenuto che richiede il codice per l'esecuzione nel server per eseguire il rendering della pagina Web. Visualizza i componenti sono utili quando il contenuto sottoposto a rendering richiede interazione con il database, ad esempio per un sito Web carrello degli acquisti. Visualizza i componenti non sono limitati per l'associazione del modello per produrre l'output di pagina Web.

## <a name="benefits-of-using-views"></a>Vantaggi dell'utilizzo di viste

Le viste consentono di definire un [ **S**eparation **o**f **C**oncerns (SoC) progettazione](http://deviq.com/separation-of-concerns/) all'interno di un'applicazione MVC separando il markup dell'interfaccia utente altre parti dell'app. Dopo la progettazione SoC rende l'app modulare, che offre diversi vantaggi:

* L'app è più facile da gestire perché è meglio organizzato. Le visualizzazioni vengono in genere raggruppate per funzionalità delle app. Questo rende più semplice trovare viste correlate, quando si utilizza una funzionalità.
* Le parti dell'app sono loosely coupled. È possibile compilare e aggiornare le visualizzazioni dell'app separatamente dai componenti di accesso dati e logica di business. È possibile modificare le visualizzazioni dell'app senza dover necessariamente aggiornare altre parti dell'app.
* Risulta più semplice testare le parti dell'interfaccia utente dell'app, perché le visualizzazioni sono unità distinte.
* A causa di una migliore organizzazione, è meno probabile che sarà accidentalmente ripetizione sezioni dell'interfaccia utente.

## <a name="creating-a-view"></a>Creazione di una vista

Le viste che sono specifiche di un controller vengono create nel *viste / [ControllerName]* cartella. Le viste che vengono condivisi tra i controller vengono inserite nel *Views/Shared* cartella. Per creare una vista, aggiungere un nuovo file e assegnargli lo stesso nome dell'azione del controller associata con il *. cshtml* estensione di file. Per creare una visualizzazione corrispondente con il *su* azione nel *Home* controller, creare un *About.cshtml* file nel *Views/Home*cartella:

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* markup inizia con il `@` simbolo. Eseguire istruzioni c# inserendo in c# il codice all'interno [blocchi di codice Razor](xref:mvc/views/razor#razor-code-blocks) disattivare le parentesi graffe (`{ ... }`). Ad esempio, vedere l'assegnazione di "About" per `ViewData["Title"]` illustrato in precedenza. È possibile visualizzare i valori in HTML facendo riferimento il valore con il `@` simbolo. Visualizzare il contenuto del `<h2>` e `<h3>` elementi sopra riportato.

Il contenuto della visualizzazione illustrato in precedenza è solo una parte dell'intera pagina Web che viene eseguito il rendering all'utente. Il resto del layout della pagina e altri aspetti comuni della visualizzazione sono specificati in altri file di visualizzazione. Per ulteriori informazioni, vedere il [argomento Layout](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Il modo in cui i controller specifica viste

Le visualizzazioni vengono in genere restituite dalle azioni come un [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), che è un tipo di [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult). Metodo di azione, è possibile creare e restituire un `ViewResult` direttamente, ma che non è in genere eseguito. Poiché la maggior parte dei controller eredita [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), è sufficiente utilizzare il `View` metodo helper per restituire il `ViewResult`:

*HomeController. cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Quando questa azione viene restituito, il *About.cshtml* visualizzazione indicata nell'ultima sezione viene visualizzato come la seguente pagina Web:

![Sulla pagina sottoposta a rendering nel browser Edge](overview/_static/about-page.png)

Il `View` metodo helper dispone di diversi overload. È possibile specificare:

* Una vista esplicita da restituire:

  ```csharp
  return View("Orders");
  ```
* A [modello](xref:mvc/models/model-binding) da passare alla vista:

  ```csharp
  return View(Orders);
  ```
* Una vista e un modello:

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>Individuazione di visualizzazione

Quando un'azione restituisce una visualizzazione, un processo denominato *individuazione visualizzazione* avviene. Questo processo determina quale file di visualizzazione in base al nome di visualizzazione. 

Il comportamento predefinito del `View` metodo (`return View();`) deve restituire una visualizzazione con lo stesso nome del metodo di azione da cui viene chiamato. Ad esempio, il *su* `ActionResult` nome del metodo del controller a cui viene utilizzato per cercare un file di visualizzazione denominato *About.cshtml*. In primo luogo, il runtime cerca *viste / [ControllerName]* cartella per la visualizzazione. Se non esiste una visualizzazione corrispondente non viene trovato, viene cercato il *Shared* cartella per la visualizzazione.

Non è importante se si restituisce in modo implicito il `ViewResult` con `return View();` o passare in modo esplicito il nome della visualizzazione per il `View` metodo con `return View("<ViewName>");`. In entrambi i casi, visualizzare l'individuazione Cerca un file di visualizzazione corrispondente nell'ordine indicato:

   1. *Viste /\[ControllerName]\[ViewName]. cshtml*
   1. *Viste/Shared/\[ViewName]. cshtml*

Invece di un nome di visualizzazione, è possibile specificare un percorso di file di visualizzazione. Se si utilizza un percorso assoluto a partire dalla radice dell'applicazione (facoltativamente inizia con "/" o "~ /"), il *. cshtml* estensione deve essere specificata:

```csharp
return View("Views/Home/About.cshtml");
```

È inoltre possibile utilizzare un percorso relativo per specificare le viste in directory diverse senza la *. cshtml* estensione. All'interno di `HomeController`, è possibile restituire il *indice* visualizzazione del *Gestisci* viste con un percorso relativo:

```csharp
return View("../Manage/Index");
```

Analogamente, è possibile indicare la directory di specifici controller corrente con il ". /" prefisso:

```csharp
return View("./About");
```

[Le visualizzazioni parziali](xref:mvc/views/partial) e [visualizzare componenti](xref:mvc/views/view-components) utilizzare meccanismi di individuazione simile (ma non identica).

È possibile personalizzare la convenzione predefinita per la modalità viste si trovano all'interno dell'app usando un oggetto personalizzato [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).

Individuazione di visualizzazione si basa sull'individuazione di visualizzare i file in base al nome di file. Se il file system sottostante è tra maiuscole e minuscole, visualizzare i nomi sono probabilmente maiuscole/minuscole. Per garantire la compatibilità tra i sistemi operativi, maiuscole/minuscole tra controller e i nomi di azione e le cartelle di visualizzazione associata e i nomi di file. Se si verifica un errore che non è possibile trovare un file di visualizzazione mentre si lavora con un sistema di file tra maiuscole e minuscole, verificare che le maiuscole e minuscole corrispondano tra il file di visualizzazione richiesta e il nome del file di visualizzazione.

Seguire la procedura consigliata di organizzare la struttura dei file per le visualizzazioni in modo da riflettere le relazioni tra i controller, azioni e visualizzazioni per maggiore chiarezza e la manutenibilità.

## <a name="passing-data-to-views"></a>Passaggio di dati a viste

È possibile passare dati alle viste utilizzando diversi approcci. L'approccio più efficace consiste nello specificare un [modello](xref:mvc/models/model-binding) tipo nella visualizzazione. Questo modello è noto come un *viewmodel*. Passare un'istanza del tipo viewmodel alla visualizzazione dall'azione.

Utilizzando un viewmodel di passare dati a una vista consente di sfruttare *sicuro* controllo dei tipi. *Tipizzazione forte* (o *fortemente tipizzato*) significa che ogni variabile e costante dispone di un tipo definito in modo esplicito (ad esempio, `string`, `int`, o `DateTime`). In fase di compilazione, viene verificata la validità dei tipi utilizzati in una vista.

[Visual Studio](https://www.visualstudio.com/vs/) e [codice di Visual Studio](https://code.visualstudio.com/) sono elencati i membri di classe fortemente tipizzata utilizzando la funzione [IntelliSense](/visualstudio/ide/using-intellisense). Quando si desidera visualizzare le proprietà di un elemento viewmodel, digitare il nome della variabile per l'elemento viewmodel seguito da un punto (`.`). Ciò consente di scrivere codice più rapidamente con meno errori.

Specificare un modello utilizzando il `@model` direttiva. Utilizzare il modello con `@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Per fornire al modello per la visualizzazione, il controller passa come parametro:

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

Non sono previste restrizioni per i tipi di modello che è possibile fornire una visualizzazione. Si consiglia di utilizzare **P**Incolla **O**ld **C**LR **O**ViewModel oggetto (POCO) con pochi o nessun comportamento (metodi) definita. In genere, le classi viewmodel sono archiviate nel *modelli* cartella o un oggetto separato *ViewModel* cartella nella radice dell'applicazione. Il *indirizzo* viewmodel utilizzato nell'esempio precedente è un elemento viewmodel POCO archiviati in un file denominato *Address.cs*:

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
> Non impedisce di utilizzare le stesse classi per i tipi di viewmodel e i tipi di modello di business. L'utilizzo di modelli separati consente, tuttavia, le visualizzazioni variare in modo indipendente dai dati di logica di business e parti di accesso dell'app. La separazione dei modelli e ViewModel offre vantaggi di sicurezza se i modelli usano [associazione del modello](xref:mvc/models/model-binding) e [convalida](xref:mvc/models/validation) per i dati inviati all'app dall'utente.


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a>Dati con tipizzazione debole (ViewData e ViewBag)

Nota: `ViewBag` non è disponibile nelle pagine Razor.

Oltre a visualizzazioni fortemente tipizzate, le visualizzazioni di avere a disposizione un *con tipizzazione debole* (chiamato anche *fortemente tipizzato*) insieme di dati. A differenza dei tipi sicuri, *tipi deboli* (o *perdere tipi*) significa che non dichiarare in modo esplicito il tipo di dati in uso. È possibile utilizzare la raccolta dei dati con tipizzazione debole per il passaggio di piccole quantità di dati da e verso i controller e visualizzazioni.

| Passaggio di dati tra un...                        | Esempio                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Controller e una visualizzazione                             | Popolamento di un elenco a discesa con i dati.                                          |
| Visualizzazione e un [visualizzazione layout](xref:mvc/views/layout)   | L'impostazione di  **\<titolo >** contenuto dell'elemento in visualizzazione layout di finestra da un file di visualizzazione.  |
| [Visualizzazione parziale](xref:mvc/views/partial) e una visualizzazione | Un widget che visualizza i dati in base a una pagina Web che l'utente ha richiesto.      |

È possibile fare riferimento a questa raccolta tramite il `ViewData` o `ViewBag` proprietà sul controller e visualizzazioni. Il `ViewData` proprietà è un dizionario di oggetti con tipizzazione debole. Il `ViewBag` proprietà è un wrapper `ViewData` che fornisce le proprietà dinamiche per sottostante `ViewData` insieme.

`ViewData`e `ViewBag` vengono risolti in modo dinamico in fase di esecuzione. Poiché non offrono controllo dei tipi in fase di compilazione, entrambi sono in genere più errori rispetto all'utilizzo di un elemento viewmodel. Per questo motivo, alcuni sviluppatori preferiscono utilizzare minima o mai `ViewData` e `ViewBag`.


<a name="VD"></a>

**ViewData**

`ViewData`è un [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) si accede tramite `string` chiavi. Dati di tipo stringa possono essere archiviati e usati direttamente, senza la necessità di un cast, ma è necessario eseguire il cast di altri `ViewData` valori a specifici tipi di oggetto quando li estraggono. È possibile utilizzare `ViewData` per passare i dati dal controller alle visualizzazioni e all'interno di visualizzazioni, tra cui [visualizzazioni parziali](xref:mvc/views/partial) e [layout](xref:mvc/views/layout).

Di seguito è riportato un esempio che imposta valori per un messaggio di benvenuto e un indirizzo utilizzando `ViewData` un'azione:

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

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

**ViewBag**

Nota: `ViewBag` non è disponibile nelle pagine Razor.

`ViewBag`è un [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) oggetto che fornisce l'accesso dinamico per gli oggetti archiviati in `ViewData`. `ViewBag`può essere più semplice da utilizzare in quanto non richiede il cast. Nell'esempio seguente viene illustrato come utilizzare `ViewBag` con lo stesso risultato utilizzando `ViewData` sopra:

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
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

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**Utilizzano ViewData e ViewBag simultaneamente**

Nota: `ViewBag` non è disponibile nelle pagine Razor.

Poiché `ViewData` e `ViewBag` fare riferimento alla stessa sottostante `ViewData` insieme, è possibile utilizzare sia `ViewData` e `ViewBag` e combinare e associare tra loro durante la lettura e scrittura di valori.

Impostare il titolo tramite `ViewBag` e la descrizione usando `ViewData` nella parte superiore di un *About.cshtml* Vista:

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Leggere le proprietà ma inverte l'utilizzo di `ViewData` e `ViewBag`. Nel *layout. cshtml* file, ottenere il titolo tramite `ViewData` e ottenere la descrizione utilizzando `ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Tenere presente che le stringhe non richiedono un cast per `ViewData`. È possibile utilizzare `@ViewData["Title"]` senza eseguire il cast.

Utilizzo di entrambe `ViewData` e `ViewBag` in lavori stesso, tempo fa, combinazione e la corrispondenza di lettura e scrittura delle proprietà. Il markup seguente viene eseguito il rendering:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**Riepilogo delle differenze tra ViewData e ViewBag**

 `ViewBag`non è disponibile nelle pagine Razor.

* `ViewData`
  * Deriva da [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), pertanto dispone di proprietà di dizionario che può essere utile, ad esempio `ContainsKey`, `Add`, `Remove`, e `Clear`.
  * Le chiavi nel dizionario sono stringhe, pertanto gli spazi vuoti sono consentito. Esempio: `ViewData["Some Key With Whitespace"]`
  * Qualsiasi tipo diverso da un `string` deve essere impostato nella visualizzazione per utilizzare `ViewData`.
* `ViewBag`
  * Deriva da [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), quindi ne consente la creazione delle proprietà dinamiche utilizzando la notazione del punto (`@ViewBag.SomeKey = <value or object>`), ed non è necessario alcun cast. La sintassi di `ViewBag` rende più rapido aggiungere al controller e visualizzazioni.
  * Più semplice verificare la presenza di valori null. Esempio: `@ViewBag.Person?.Name`

**Quando utilizzare ViewData o ViewBag**

Entrambi `ViewData` e `ViewBag` sono ugualmente validi approcci per il passaggio di piccole quantità di dati tra i controller e visualizzazioni. La scelta di quello da utilizzare è basata sulla preferenza. È possibile combinare e associare `ViewData` e `ViewBag` oggetti, tuttavia, il codice è più facile da leggere e gestire con un approccio utilizzato in modo coerente. Entrambi gli approcci sono risolti in modo dinamico in fase di esecuzione e quindi soggetto a causa di errori di runtime. Alcuni team di sviluppo per evitare il problema.

### <a name="dynamic-views"></a>Visualizzazioni dinamiche

Tipo di viste che non dichiarano un modello utilizzando `@model` ma che dispongono di un'istanza del modello passata a essi (ad esempio, `return View(Address);`) possono fare riferimento le proprietà dell'istanza in modo dinamico:

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Questa funzionalità offre flessibilità ma non offre protezione compilazione o IntelliSense. Se la proprietà non esiste, la generazione di pagine Web non riesce in fase di esecuzione.

## <a name="more-view-features"></a>Altre funzionalità di visualizzazione

[Gli helper di tag](xref:mvc/views/tag-helpers/intro) rendono più facile aggiungere un comportamento lato server per i tag HTML esistenti. Gli helper di Tag consente di evitare la necessità di scrivere codice personalizzato o helper all'interno delle visualizzazioni. Gli helper di tag vengono applicati come attributi agli elementi HTML e vengono ignorati per gli editor che non è possibile elaborarli. Ciò consente di modificare ed eseguire il rendering di markup di visualizzazione in un'ampia gamma di strumenti.

Generazione di codice HTML personalizzato può essere ottenuta con molti helper HTML incorporato. Una logica più complessa dell'interfaccia utente può essere gestita da [Visualizza i componenti](xref:mvc/views/view-components). Visualizza i componenti forniscono la stessa SoC tale controller e visualizzazioni offrono. È possibile eliminare la necessità di azioni e visualizzazioni che gestiscono i dati utilizzati da elementi dell'interfaccia utente comune.

Come molti altri aspetti di ASP.NET Core, viste supportano [inserimento di dipendenze](xref:fundamentals/dependency-injection), consentendo di servizi da [inserite nelle viste](xref:mvc/views/dependency-injection).
