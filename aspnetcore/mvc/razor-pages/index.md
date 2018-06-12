---
title: Introduzione a Razor Pages in ASP.NET Core
author: Rick-Anderson
description: Informazioni su come Razor Pages in ASP.NET Core semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine rispetto a MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: c8611ef9218711410cde9e142202fa25c3e51862
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729458"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>Introduzione a Razor Pages in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)

Razor Pages è una nuova funzionalità di ASP.NET Core MVC che semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine.

Se si sta cercando un'esercitazione in cui si usa l'approccio Model-View-Controller, vedere [Introduzione ad ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).

Questo documento offre un'introduzione a Razor Pages. Non è un'esercitazione dettagliata. Se alcune sezioni risultano troppo avanzate, vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start). Per una panoramica di ASP.NET Core, vedere [Introduzione a ASP.NET Core](xref:index).

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a>Creazione di un progetto Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Per istruzioni dettagliate su come creare un progetto Razor Pages con Visual Studio, vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

Eseguire `dotnet new webapp` dalla riga di comando.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Eseguire `dotnet new razor` dalla riga di comando.

::: moniker-end

Aprire il file *CSPROJ* generato da Visual Studio per Mac.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

::: moniker range=">= aspnetcore-2.1"

Eseguire `dotnet new webapp` dalla riga di comando.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Eseguire `dotnet new razor` dalla riga di comando.

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli) 

::: moniker range=">= aspnetcore-2.1"

Eseguire `dotnet new webapp` dalla riga di comando.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Eseguire `dotnet new razor` dalla riga di comando.

::: moniker-end

---

## <a name="razor-pages"></a>Razor Pages

La funzionalità Razor Pages è abilitata in *Startup.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Si consideri una pagina di base: <a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Il codice precedente è molto simile a un file di visualizzazione Razor. Ciò che lo differenzia è la direttiva `@page`. `@page` trasforma il file in un'azione MVC, ovvero gestisce le richieste direttamente, senza passare attraverso un controller. `@page` deve essere la prima direttiva Razor in una pagina. `@page` influisce sul comportamento di altri costrutti Razor.

Nei due file seguenti viene visualizzata una pagina simile che usa una classe `PageModel`. Il file *Pages/Index2.cshtml*:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

Il modello di pagina *Pages/Index2.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Per convenzione, il file di classe `PageModel` ha lo stesso nome del file della pagina Razor con l'aggiunta di *.cs*. Ad esempio, la pagina Razor precedente è *Pages/Index2.cshtml*. Il file che contiene la classe `PageModel` è denominato *Pages/Index2.cshtml.cs*.

Le associazioni dei percorsi URL alle pagine sono determinate dalla posizione della pagina nel file system. Nella tabella seguente sono riportati alcuni percorsi di pagina Razor con gli URL corrispondenti:

| Percorso e nome file               | URL corrispondente |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` o `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` o `/Store/Index` |

Note:

* Il runtime cerca i file delle pagine Razor nella cartella *Pages* per impostazione predefinita.
* `Index` è la pagina predefinita quando un URL non include una pagina.

## <a name="writing-a-basic-form"></a>Scrittura di un form di base

Razor Pages semplifica l'implementazione dei modelli normalmente usati con i Web browser durante la creazione di un'app. L'[associazione di modelli](xref:mvc/models/model-binding), gli [helper tag](xref:mvc/views/tag-helpers/intro) e gli helper HTML *funzionano* tutti con le proprietà definite in una classe di pagina Razor. Si consideri una pagina che implementa un form "contact us" di base per il modello `Contact`:

Per gli esempi riportati in questo documento, `DbContext` viene inizializzato nel file [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Il modello di dati:

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

Il contesto del database:

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

Il file di visualizzazione *Pages/Create.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

Il modello di pagina *Pages/Create.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Per convenzione, la classe `PageModel` è denominata `<PageName>Model` e si trova nello stesso spazio dei nomi della pagina.

La classe `PageModel` consente la separazione della logica di una pagina dalla relativa presentazione. Definisce i gestori di pagina per le richieste inviate alla pagina e i dati usati per il rendering della pagina. Questa separazione consente di gestire le dipendenze della pagina tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) e di eseguire [unit test](xref:test/razor-pages-tests) delle pagine.

La pagina contiene un oggetto `OnPostAsync`, ovvero un *metodo gestore* che viene eseguito per le richieste `POST`, quando un utente invia il form. È possibile aggiungere metodi gestore per qualsiasi verbo HTTP. I gestori più comuni sono:

* `OnGet` per inizializzare lo stato necessario per la pagina. Esempio di [OnGet](#OnGet).
* `OnPost` per gestire gli invii di form.

Il suffisso `Async` nel nome è facoltativo, ma viene spesso usato per convenzione per le funzioni asincrone. Il codice `OnPostAsync` nell'esempio precedente è simile a ciò che in genere si scrive in un controller. Il codice precedente è tipico di Razor Pages. La maggior parte delle primitive di MVC, ad esempio l'[associazione di modelli](xref:mvc/models/model-binding), la [convalida](xref:mvc/models/validation) e i risultati dell'azione vengono condivise.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

Il metodo `OnPostAsync` precedente:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

Il flusso di base di `OnPostAsync`:

Verificare se sono presenti errori di convalida.

*  Se non sono presenti errori, salvare i dati e reindirizzare.
*  Se sono presenti errori, visualizzare di nuovo la pagina con i messaggi di convalida. La convalida lato client è identica alle applicazioni ASP.NET Core MVC tradizionali. In molti casi gli errori di convalida vengono rilevati nel client e mai inviati al server.

Quando i dati vengono immessi correttamente, il metodo gestore `OnPostAsync` chiama il metodo helper `RedirectToPage` per restituire un'istanza di `RedirectToPageResult`. `RedirectToPage` è un nuovo risultato dell'azione, simile a `RedirectToAction` o `RedirectToRoute`, ma personalizzato per le pagine. Nell'esempio precedente viene reindirizzato alla pagina di indice radice (`/Index`). Il metodo `RedirectToPage` è descritto in dettaglio nella sezione [Generazione di URL per le pagine](#url_gen).

Quando il form inviato contiene errori di convalida (che vengono passati al server), il metodo gestore `OnPostAsync` chiama il metodo helper `Page`. `Page` restituisce un'istanza di `PageResult`. La restituzione di `Page` è simile al modo in cui le azioni nel controller restituiscono `View`. `PageResult` è il tipo restituito <!-- Review  --> predefinito per un metodo gestore. Un metodo gestore che restituisce `void` esegue il rendering della pagina.

La proprietà `Customer` usa l'attributo `[BindProperty]` optare per consentire l'associazione di modelli.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Per impostazione predefinita Razor Pages associa le proprietà solo ai verbi non GET. Il binding alle proprietà può ridurre la quantità di codice da scrivere. Riduce il codice usando la stessa proprietà per il eseguire il rendering dei campi del form (`<input asp-for="Customer.Name" />`) e accettare l'input.

> [!NOTE]
> Per motivi di sicurezza, è necessario acconsentire esplicitamente all'associazione dei dati della richiesta GET alle proprietà del modello di pagina. Verificare l'input dell'utente prima di eseguirne il mapping alle proprietà. Acconsentire esplicitamente a questo comportamento è utile quando si lavora in scenari basati su valori route o stringa di query.
>
> Per associare una proprietà nelle richieste GET, impostare la proprietà `SupportsGet` dell'attributo `[BindProperty]` su `true`: `[BindProperty(SupportsGet = true)]`

La home page (*Index.cshtml*):

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

Il file code-behind *Index.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

Il file *Index.cshtml* contiene il markup seguente per creare un collegamento di modifica per ogni contatto:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) ha usato l'attributo `asp-route-{value}` per generare un collegamento alla pagina di modifica. Il collegamento contiene i dati della route con l'ID contatto. Ad esempio `http://localhost:5000/Edit/1`.

Il file *Pages/Edit.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

La prima riga contiene la direttiva `@page "{id:int}"`. Il vincolo di routing `"{id:int}"` indica alla pagina di accettare le richieste inviate alla pagina che contengono i dati della route `int`. Se una richiesta inviata alla pagina non contiene dati della route che possono essere convertiti in un oggetto `int`, il runtime restituisce un errore HTTP 404 (non trovato). Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:

 ```cshtml
@page "{id:int?}"
```

Il file *Pages/Edit.cshtml.cs*:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

Il file *Index.cshtml* contiene anche il markup per creare un pulsante di eliminazione per ogni contatto cliente:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Quando viene eseguito il rendering del pulsante di eliminazione in HTML, il relativo `formaction` include i parametri per:

* L'ID di contatto cliente dall'attributo `asp-route-id`.
* L'`handler` specificato dall'attributo `asp-page-handler`.

Di seguito è riportato un esempio di pulsante di eliminazione di cui è stato eseguito il rendering con un ID di contatto cliente `1`:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Quando il pulsante è selezionato, viene inviata una richiesta `POST` di modulo al server. Per convenzione, il nome del metodo del gestore viene selezionato in base al valore del parametro `handler` in base allo schema `OnPost[handler]Async`.

Poiché in questo esempio l'`handler` è `delete`, il metodo `OnPostDeleteAsync` viene usato per elaborare la richiesta `POST`. Se `asp-page-handler` viene impostato su un valore diverso, come `remove`, viene selezionato un metodo gestore della pagina con il nome `OnPostRemoveAsync`.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

Il metodo `OnPostDeleteAsync`:

* Accetta l'`id` dalla stringa di query.
* Interroga il database in merito al contatto del cliente con `FindAsync`.
* Se viene trovato il contatto cliente, viene rimosso dall'elenco dei contatti del cliente. Il database viene aggiornato.
* Chiama `RedirectToPage` per reindirizzare alla pagina di indice radice (`/Index`).

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a>Contrassegnare le proprietà della pagina con Required

Le proprietà di `PageModel` possono essere decorate con l'attributo con [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute):

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

Per altre informazioni, vedere [Convalida del modello](xref:mvc/models/validation).

## <a name="manage-head-requests-with-the-onget-handler"></a>Gestire le richieste HEAD con il gestore OnGet

In genere, per le richieste HEAD viene creato e chiamato un gestore HEAD:

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

In assenza di un gestore HEAD (`OnHead`) definito, Razor Pages ricorre alla chiamata del gestore di pagine GET (`OnGet`) come fallback in ASP.NET Core 2.1 o versioni successive. È possibile acconsentire esplicitamente a questo comportamento con il [metodo SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) in `Startup.Configure` per ASP.NET Core 2.1-2.x:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

`SetCompatibilityVersion` imposta effettivamente l'opzione di Razor Pages `AllowMappingHeadRequestsToGetHandler` su `true`.

Anziché acconsentire esplicitamente a tutti i comportamenti 2.1 con `SetCompatibilityVersion`, è possibile accettare comportamenti specifici. Il codice seguente consente di accettare le richieste HEAD di mapping nel gestore GET.


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF e Razor Pages

Non è necessario scrivere codice per la [convalida antifalsificazione](xref:security/anti-request-forgery). La generazione e la convalida del token antifalsificazione sono automaticamente incluse in Razor Pages.

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Uso di layout, righe parzialmente eseguite, modelli e helper tag con Razor Pages

Le pagine funzionano con tutte le funzionalità del motore di visualizzazione Razor. I layout, le righe parzialmente eseguite, i modelli, gli helper tag, *_ViewStart.cshtml* e *_ViewImports.cshtml* funzionano esattamente come nelle normali visualizzazioni Razor.

La pagina verrà ora riorganizzata in modo da usufruire di alcune di queste funzionalità.

Aggiungere una [pagina di layout](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

Il [Layout](xref:mvc/views/layout):

* Controlla il layout di ogni pagina, a meno che la pagina non accetti il layout.
* Importa le strutture HTML, ad esempio JavaScript e i fogli di stile.

Vedere l'articolo sulla [pagina Layout](xref:mvc/views/layout) per altre informazioni.

La proprietà [Layout](xref:mvc/views/layout#specifying-a-layout) proprietà viene impostata in *Pages/_ViewStart.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

Il layout è nella cartella *Pages* (Pagine). Le pagine cercano altre visualizzazioni (layout, modelli, righe parzialmente eseguite) in modo gerarchico, partendo dalla stessa cartella della pagina corrente. Un layout nella cartella *Pages* può essere usato da qualsiasi pagina Razor della cartella *Pages*.

Si consiglia di **non** inserire il file di layout nella cartella *Views/Shared*. *Views/Shared* è un modello destinato alle visualizzazioni MVC. Le pagine Razor devono basarsi sulla gerarchia di cartelle, non su convenzioni di percorso.

La ricerca delle visualizzazioni da una pagina Razor include la cartella *Pages*. Il layout, i modelli e le righe parzialmente eseguite in uso con i controller MVC e le visualizzazioni Razor standard *funzionano solo*.

Aggiungere un file *Pages/_ViewImports.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` viene spiegato in seguito nell'esercitazione. La direttiva `@addTagHelper` inserisce gli [helper tag predefiniti](xref:mvc/views/tag-helpers/builtin-th/Index) in tutte le pagine presenti nella cartella *Pages*.

<a name="namespace"></a>

Quando la direttiva `@namespace` viene usata in modo esplicito in una pagina:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

La direttiva imposta lo spazio dei nomi per la pagina. La direttiva `@model` non deve necessariamente includere lo spazio dei nomi.

Se la direttiva `@namespace` è contenuta in *_ViewImports.cshtml*, lo spazio dei nomi specificato indica il prefisso per lo spazio dei nomi generato nella pagina che importa la direttiva `@namespace`. Il resto dello spazio dei nomi generato (la parte del suffisso) è il percorso relativo separato dal punto tra la cartella contenente *_Viewimports.cshtml* e la cartella contenente la pagina.

Ad esempio, il file code-behind *Pages/Customers/Edit.cshtml.cs* imposta in modo esplicito lo spazio dei nomi:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

Il file *Pages/_ViewImports.cshtml* file imposta il seguente spazio dei nomi:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

Lo spazio dei nomi generato per la pagina Razor *Pages/Customers/Edit.cshtml* corrisponde al file code-behind. La direttiva `@namespace` è stata progettata in modo che le classi C# aggiunte a un progetto e il codice generato dalle pagine *funzionino* senza dover aggiungere una direttiva `@using` per il file code-behind.

`@namespace` *Funziona anche con le normali visualizzazioni Razor.*

Il file di visualizzazione *Pages/Create.cshtml* originale:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Il file di visualizzazione *Pages/Create.cshtml* aggiornato:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

Il [progetto iniziale per Razor Pages](#rpvs17) contiene il file *Pages/_ValidationScriptsPartial.cshtml*, che esegue la convalida sul lato client.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Generazione di URL per le pagine

La pagina `Create`, riportata in precedenza, usa `RedirectToPage`:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

L'applicazione ha la struttura di file o cartella seguente:

* */Pages*

  * *Index.cshtml*
  * */Customers*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

Le pagine *Pages/Customers/Create.cshtml* e *Pages/Customers/Edit.cshtml* reindirizzano a *Pages/Index.cshtml* dopo l'esecuzione. La stringa `/Index` fa parte dell'URI di accesso alla pagina precedente. La stringa `/Index` può essere usata per generare gli URI alla pagina *Pages/Index.cshtml*. Ad esempio:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Il nome della pagina è il percorso alla pagina dalla cartella radice */Pages*, inclusa una barra iniziale `/`, ad esempio `/Index`. Gli esempi di generazione di URL precedenti offrono opzioni e caratteristiche funzionali avanzate rispetto agli URL hardcoded. La generazione di URL usa il [routing](xref:mvc/controllers/routing) ed è in grado di generare e codificare i parametri in base al modo in cui la route è definita nel percorso di destinazione.

La generazione di URL per le pagine supporta i nomi relativi. La tabella seguente indica quale pagina di indice viene selezionata con diversi parametri `RedirectToPage` da *Pages/Customers/Create.cshtml*:

| RedirectToPage(x)| Pagina |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Pages/Index* |
| RedirectToPage("./Index"); | *Pages/Customers/Index* |
| RedirectToPage("../Index") | *Pages/Index* |
| RedirectToPage("Index")  | *Pages/Customers/Index* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")`, e `RedirectToPage("../Index")` sono <em>nomi relativi</em>. Il parametro `RedirectToPage` è <em>combinato</em> con il percorso della pagina corrente per calcolare il nome della pagina di destinazione.  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

Il collegamento dei nomi relativi è utile quando si compilano siti con una struttura complessa. Se si usano i nomi relativi per il collegamento tra le pagine in una cartella, è possibile rinominare tale cartella. Tutti i collegamenti continuano a funzionare perché non includono il nome della cartella.

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a>Attributo viewData

È possibile passare i dati a una pagina tramite [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute). I valori delle proprietà nei controller o nei modelli Razor Page decorate con `[ViewData]` vengono archiviati e caricati da [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).

Nell'esempio seguente `AboutModel` contiene una proprietà `Title` decorata con `[ViewData]`. La proprietà `Title` è impostata sul titolo della pagina About (Informazioni):

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

Nella pagina About (Informazioni) accedere alla proprietà `Title` come proprietà del modello:

```cshtml
<h1>@Model.Title</h1>
```

Nel layout il titolo viene letto dal dizionario ViewData:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core espone la proprietà [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) per un [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller). Questa proprietà archivia i dati finché non viene letta. I metodi `Keep` e `Peek` possono essere usati per esaminare i dati senza eliminazione. `TempData` è utile per il reindirizzamento quando i dati sono necessari per più di una singola richiesta.

L'attributo `[TempData]` è nuovo in ASP.NET Core 2.0 ed è supportato per controller e pagine.

Il codice seguente imposta il valore di `Message` usando `TempData`:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

Il markup seguente nel file *Pages/Customers/Index.cshtml* visualizza il valore di `Message` usando `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

Il modello di pagina *Pages/Customers/Index.cshtml.cs* applica l'attributo `[TempData]` alla proprietà `Message`.

```cs
[TempData]
public string Message { get; set; }
```

Per altre informazioni, vedere [TempData](xref:fundamentals/app-state#tempdata).

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a>Più gestori per pagina

La pagina seguente genera markup per due gestori di pagina usando l'helper tag `asp-page-handler`:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

Il form nell'esempio precedente contiene due pulsanti di invio, ognuno dei quali usa `FormActionTagHelper` per l'invio a un URL diverso. L'attributo `asp-page-handler` è correlato a `asp-page`. `asp-page-handler` genera gli URL che indirizzano a ognuno dei metodi gestore definiti da una pagina. `asp-page` non è specificato poiché l'esempio è collegato alla pagina corrente.

Il modello di pagina:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

Il codice precedente usa *metodi gestore denominati*. I metodi gestore denominati vengono creati usando il testo che nel nome segue `On<HTTP Verb>` e precede `Async` (se presente). Nell'esempio precedente i metodi della pagina sono OnPost**JoinList**Async e OnPost**JoinListUC**Async. Rimuovendo *OnPost* e *Async*, i nomi dei gestori sono `JoinList` e `JoinListUC`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Usando il codice precedente, il percorso URL che indirizza a `OnPostJoinListAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. Il percorso URL che indirizza a `OnPostJoinListUCAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.



## <a name="customizing-routing"></a>Personalizzazione del routing

Se si vuole omettere la stringa di query `?handler=JoinList` dall'URL, è possibile modificare la route in modo da inserire il nome del gestore nella parte di percorso dell'URL. È possibile personalizzare la route aggiungendo un modello di route racchiuso tra virgolette doppie dopo la direttiva `@page`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

La route precedente inserisce il nome del gestore nel percorso URL anziché nella stringa di query. `?` che segue `handler` indica che il parametro di route è facoltativo.

È possibile usare `@page` per aggiungere altri segmenti e parametri alla route di una pagina. Tutti gli elementi presenti vengono **aggiunti** alla route predefinita della pagina. L'uso di un percorso assoluto o virtuale per modificare la route della pagina, ad esempio `"~/Some/Other/Path"`, non è supportato.

## <a name="configuration-and-settings"></a>Configurazione e impostazioni

Per configurare le opzioni avanzate, usare il metodo di estensione `AddRazorPagesOptions` nel generatore MVC:

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Attualmente è possibile usare `RazorPagesOptions` per impostare la directory radice per le pagine o aggiungere convenzioni di modello applicativo per le pagine. In questo modo si potrà garantire una maggiore estendibilità in futuro.

Per la precompilazione delle visualizzazioni, vedere l'articolo sulla [compilazione delle visualizzazioni Razor](xref:mvc/views/view-compilation).

[Scaricare o visualizzare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).

Vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start), che si basa su questa introduzione.

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Specificare che Razor Pages si trova nella radice del contenuto

Per impostazione predefinita, la directory radice di Razor Pages è */Pages*. Aggiungere [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) per specificare che Razor Pages è nella radice del contenuto ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) dell'app:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Specificare che Razor Pages è in una directory radice personalizzata

Aggiungere [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) per specificare che Razor Pages si trova in una directory radice personalizzata nell'app (fornire un percorso relativo):

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a>Vedere anche

* [Introduzione a ASP.NET Core](xref:index)
* [Sintassi Razor](xref:mvc/views/razor)
* [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Convenzioni di autorizzazione di Razor Pages](xref:security/authorization/razor-pages-authorization)
* [Route personalizzata di Razor Pages e provider di modelli di pagina](xref:mvc/razor-pages/razor-pages-conventions)
* [Unit test per Razor Pages](xref:test/razor-pages-tests)
