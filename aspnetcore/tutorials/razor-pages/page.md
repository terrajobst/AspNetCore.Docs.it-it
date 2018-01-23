---
title: Pagine Razor create in ASP.NET Core tramite scaffolding
author: rick-anderson
description: Illustra le pagine Razor generate tramite scaffolding.
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: ad2a2b48beb31dddcfd78a8aab79ac58ccda28f3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Pagine Razor create in ASP.NET Core tramite scaffolding

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa esercitazione vengono esaminate le pagine Razor create tramite scaffolding nell'argomento dell'esercitazione precedente [Aggiunta di un modello](xref:tutorials/razor-pages/model#scaffold-the-movie-model). 

[Visualizzare o scaricare](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) l'esempio.

## <a name="the-create-delete-details-and-edit-pages"></a>Pagine di creazione, eliminazione, dettagli e modifica.

Esaminare il modello di pagina *Pages/Movies/Index.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Le pagine Razor vengono derivate da `PageModel`. Per convenzione, la classe derivata `PageModel` viene denominata `<PageName>Model`. Il costruttore usa l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) per aggiungere `MovieContext` alla pagina. Tutte le pagine create tramite scaffolding seguono questo schema. Vedere [Codice asincrono](xref:data/ef-rp/intro#asynchronous-code) per altre informazioni sulla programmazione asincrona con Entity Framework.

Quando per la pagina viene eseguita una richiesta, il metodo `OnGetAsync` restituisce un elenco di filmati alla pagina Razor. Nella pagina Razor viene chiamato `OnGetAsync`o `OnGet` per inizializzare lo stato della pagina. In questo caso, `OnGetAsync` ottiene un elenco di filmati e li visualizza. 

Quando `OnGet` restituisce `void` o `OnGetAsync` restituisce`Task`, non viene usato alcun metodo restituito. Quando il tipo restituito è `IActionResult` o `Task<IActionResult>`, è necessario specificare un'istruzione return. Ad esempio, il metodo *Pages/Movies/Create.cshtml.cs* `OnPostAsync`:

<!-- TODO - replace with snippet
[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```
Esaminare la pagina Razor *Pages/Movies/Index.cshtml*:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Con Razor è possibile passare da HTML a C# o scegliere un markup specifico per Razor. Quando il simbolo `@` è seguito da una [parola chiave riservata Razor ](xref:mvc/views/razor#razor-reserved-keywords), la transizione avviene in un markup specifico per Razor, altrimenti in C#.

La direttiva Razor `@page` trasforma il file in un'azione MVC &mdash; in modo che possa gestire le richieste. `@page` deve essere la prima direttiva Razor in una pagina. `@page` è un esempio di transizione nel markup specifico per Razor. Vedere [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintassi Razor) per altre informazioni.

Esaminare l'espressione lambda usata nell'helper HTML seguente:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

L'helper HTML `DisplayNameFor` controlla la proprietà `Title` a cui fa riferimento nell'espressione lambda per determinare il nome visualizzato. L'espressione lambda viene controllata anziché valutata. Non sussiste pertanto violazione di accesso quando `model`, `model.Movie`, o `model.Movie[0]` sono `null` o vuoti. Quando invece l'espressione lambda viene valutata (ad esempio con `@Html.DisplayFor(modelItem => item.Title)`), vengono valutati i valori proprietà del modello.

<a name="md"></a>
### <a name="the-model-directive"></a>La direttiva @model

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

La direttiva `@model` specifica il tipo di modello passato alla pagina Razor. Nell'esempio precedente la riga `@model` rende disponibile la classe derivata `PageModel` alla pagina Razor. Il modello viene usato negli [helper HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` e `@Html.DisplayName` nella pagina.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData e layout

Esaminare il codice seguente:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

Il codice evidenziato sopra è un esempio di transizione Razor in C#. Tra i caratteri `{` e `}` è racchiuso un blocco di codice C#.

La classe di base `PageModel` ha una proprietà del dizionario `ViewData` che può essere usata per aggiungere i dati da passare a una visualizzazione. Gli oggetti vengono aggiunti al dizionario `ViewData` usando uno schema chiave/valore. Nell'esempio precedente la proprietà "Title" viene aggiunta al dizionario `ViewData`. La proprietà "Title" viene usata nel file *Pages/_Layout.cshtml*. Il markup seguente illustra le prime righe del file *Pages/_Layout.cshtml*.

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

La riga `@*Markup removed for brevity.*@` è un commento Razor. A differenza dei commenti HTML (`<!-- -->`), i commenti Razor non vengono inviati al client.

Eseguire l'app e i collegamenti di test nel progetto (**Home**, **Informazioni**, **Contatto**, **Crea**, **Modifica** ed **Elimina**). In ogni pagina viene impostato il titolo che è possibile visualizzare nella scheda del browser. Quando una pagina viene specificata come segnalibro, il titolo viene usato per il segnalibro. *Pages/Index.cshtml* e *Pages/Movies/Index.cshtml* hanno attualmente lo stesso titolo, ma è possibile modificarli in modo che i valori siano diversi.

La proprietà `Layout` viene impostata nel file *Pages/_ViewStart.cshtml*:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

Il markup precedente imposta il file di layout su *Pages/_Layout.cshtml* per tutti i file Razor contenuti nella cartella *Pagine*. Vedere [Layout](xref:mvc/razor-pages/index#layout) per altre informazioni.

### <a name="update-the-layout"></a>Aggiornare il layout

Modificare l'elemento `<title>` nel file *Pages/_Layout.cshtml* per usare una stringa più corta.

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Trovare l'elemento di ancoraggio seguente nel file *Pages/_Layout.cshtml*.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
Sostituire l'elemento precedente con il markup seguente.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

L'elemento di ancoraggio precedente è un [helper tag](xref:mvc/views/tag-helpers/intro). In questo caso, si tratta dell'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). L'attributo e il valore dell'helper tag `asp-page="/Movies/Index"` creano un collegamento alla pagina Razor `/Movies/Index`.

Salvare le modifiche e testare l'app selezionando il collegamento **RpMovie**. Visualizzare il file [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) in GitHub.

### <a name="the-create-code-behind-page"></a>Pagina di creazione code-behind

Esaminare il file code-behind *Pages/Movies/Create.cshtml.cs*:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

Il metodo `OnGet` inizializza lo stato necessario per la pagina. La pagina di creazione non possiede uno stato da inizializzare. Il metodo `Page` crea un oggetto `PageResult` che esegue il rendering della pagina *Create.cshtml*.

La proprietà `Movie` usa l'attributo `[BindProperty]` per consentire l'[associazione di modelli](xref:mvc/models/model-binding). Dopo che il modulo di creazione registra i valori, il runtime di ASP.NET Core associa i valori registrati al modello `Movie`.

Il metodo `OnPostAsync` viene eseguito quando la pagina inserisce i dati del modulo:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Se il modello contiene errori, il modulo viene nuovamente visualizzato insieme ai dati del modulo inviati. La maggior parte degli errori del modello sono riscontrabili sul lato client prima che il modulo sia inviato. La registrazione di un valore per il campo data non convertibile in una data è un esempio di errore del modello. Durante l'esercitazione saranno poi trattate nel dettaglio la convalida lato client e la convalida del modello.

Se non sono presenti errori nel modello, i dati vengono salvati e il browser viene reindirizzato alla pagina di indice.

### <a name="the-create-razor-page"></a>Pagina di creazione di Razor

Esaminare il file della pagina Razor *Pages/Movies/Create.cshtml*:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

Per visualizzare il tag `<form method="post">`, Visual Studio usa un tipo di carattere distintivo specifico per gli helper tag. L'elemento `<form method="post">` è un [helper tag del modulo](xref:mvc/views/working-with-forms#the-form-tag-helper). L'helper tag del modulo include automaticamente un [token antifalsificazione](xref:security/anti-request-forgery).

![Visualizzazione di VS17 della pagina Create.cshtml](page/_static/th.png)

Il motore di scaffolding crea un markup Razor per ogni campo nel modello (tranne l'ID) simile al seguente:

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

Gli [helper tag di convalida](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` e ` <span asp-validation-for`) visualizzano gli errori di convalida. Il tema della convalida viene trattato in modo più dettagliato successivamente in questa serie.

L'[helper tag di etichetta](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera la didascalia dell'etichetta e l'attributo `for` per la proprietà `Title`.

L'[helper tag di input](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) usa gli attributi [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produce gli attributi HTML necessari per la convalida jQuery sul lato client.

L'esercitazione successiva illustra SQL Server LocalDB e il seeding del database.

>[!div class="step-by-step"]
[Previous: Adding a model](xref:tutorials/razor-pages/model) (Precedente: Aggiunta di un modello)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql) (Successivo: SQL Server LocalDB)
