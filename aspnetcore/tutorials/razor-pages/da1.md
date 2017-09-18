---
title: Aggiornamento delle pagine generate
author: rick-anderson
description: Aggiornamento delle pagine generate con una migliore visualizzazione.
keywords: ASP.NET Core, pagine Razor
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 39b65f8af8304fabc6cf8d9a27992043f1e381a0
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="updating-the-generated-pages"></a>Aggiornamento delle pagine generate

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale. Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere **Release Date** (due parole).

![App per i film aperta in Chrome con i dati sui film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aggiornare il codice generato

Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate nel codice seguente:

[!code-csharp[Principale](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Fare clic con il pulsante destro del mouse su una riga rossa ondulata > **Azioni rapide e refactoring**.

  ![Menu di scelta rapida con **> Azioni rapide e refactoring**.](da1/qa.png)


Selezionare `using System.ComponentModel.DataAnnotations;`.

  ![using System.ComponentModel.DataAnnotations all'inizio dell'elenco](da1/da.png)

  Visual Studio aggiunge `using System.ComponentModel.DataAnnotations;`.

Gli attributi [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) verranno esaminati nell'esercitazione successiva. L'attributo [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) specifica il testo da visualizzare per il nome di un campo, in questo caso "Release Date" anziché "ReleaseDate". L'attributo [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) specifica il tipo di dati (Date) e quindi non vengono visualizzate le informazioni sull'ora archiviate nel campo.

Accedere a Pages/Movies e passare il mouse su un collegamento **Edit** (Modifica) per visualizzare l'URL di destinazione.

![Finestra del browser con il passaggio del mouse sul collegamento Edit (Modifica) e un URL di collegamento di http://localhost:1234/Movies/Edit/5](da1/edit7.png)

I collegamenti **Edit** (Modifica), **Details** (Dettagli) e **Delete** (Elimina) vengono generati dall'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) nel file *Pages/Movies/Index.cshtml*.

[!code-cshtml[Principale](razor-pages-start\snapshot_sample\RazorPagesMovie\Pages\Movie\Index.cshtml?highlight=16-18&range=32-)]

Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor. Nel codice precedente `AnchorTagHelper` genera in modo dinamico il valore di attributo `href` HTML dalla pagina Razor (la route è relativa), `asp-page` e l'ID di route (`asp-route-id`). Per altre informazioni, vedere [Generazione di URL per le pagine](xref:mvc/razor-pages/index#url-generation-for-pages).

Usare **Visualizza origine** dal browser preferito per esaminare il codice generato. Di seguito è riportata una parte del codice HTML generato:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>

```

I collegamenti generati dinamicamente passano l'ID del film con una stringa di query, ad esempio `http://localhost:5000/Movies/Details?id=2`. 

Aggiornare le pagine Razor Edit (Modifica), Details (Dettagli) e Delete (Elimina) in modo da usare il modello di route "{id: int}". Modificare la direttiva page per ognuna di queste pagine in `@page "{id:int}"`. Eseguire l'app e quindi visualizzare l'origine. Il codice HTML generato aggiunge l'ID alla parte di percorso dell'URL:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Una richiesta alla pagina con il modello di route "{id: int}" che **non** include l'intero restituirà un errore HTTP 404 (Non trovato). Ad esempio, `http://localhost:5000/Movies/Details` restituirà un errore 404. Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a>Aggiornare la gestione delle eccezioni di concorrenza

Aggiornare il metodo `OnPostAsync` nel file *Pages/Movies/Edit.cshtml.cs*. Il codice evidenziato seguente illustra le modifiche:

[!code-csharp[Principale](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet1&highlight=17-24)]

Il codice precedente rileva le eccezioni di concorrenza solo quando il primo client concorrente elimina il film e il secondo client concorrente invia le modifiche al film.

Per testare il blocco `catch`:

* Impostare un punto di interruzione su `catch (DbUpdateConcurrencyException)`
* Modificare un film.
* In un'altra finestra del browser, selezionare il collegamento **Delete** (Elimina) per lo stesso film e quindi eliminare il film.
* Nella finestra del browser precedente inviare le modifiche al film.

Il codice di produzione in genere rileva i conflitti di concorrenza quando due o più client hanno aggiornato contemporaneamente un record. Per altre informazioni, vedere [Gestione dei conflitti di concorrenza](xref:data/ef-mvc/concurrency).

### <a name="posting-and-binding-review"></a>Invio di post e analisi delle associazioni

Esaminare il file *Pages/Movies/Edit.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Edit.cshtml.cs?name=snippet2)]

Quando viene eseguita una richiesta HTTP GET alla pagina Movies/Edit (Film/Modifica), ad esempio `http://localhost:5000/Movies/Edit/2`:

* Il metodo `OnGetAsync` recupera il film dal database e restituisce il metodo `Page`. 
* Il metodo `Page` esegue il rendering della pagina Razor *Pages/Movies/Edit.cshtml*. Il file *Pages/Movies/Edit.cshtml* contiene la direttiva modello (`@model RazorPagesMovie.Pages.Movies.EditModel`) che rende il modello di film disponibile nella pagina.
* Il modulo Edit (Modifica) viene visualizzato con i valori dal film.

Quando viene inviata la pagina Movies/Edit (Film/Modifica):

* I valori del modulo nella pagina vengono associati alla proprietà `Movie`. L'attributo `[BindProperty]` abilita l'[associazione di modelli](xref:mvc/models/model-binding).

```csharp
[BindProperty]
public Movie Movie { get; set; }
```

* Se sono presenti errori nello stato del modello, ad esempio `ReleaseDate` non può essere convertito in una data, il modulo viene inviato nuovamente con i valori presentati.
* Se non sono presenti errori del modello, il film viene salvato.

I metodi HTTP GET nelle pagine Razor Index, Create e Delete seguono un criterio simile. Il metodo `OnPostAsync` HTTP POST nella pagina Razor Create segue un criterio simile al metodo `OnPostAsync` nella pagina Edit (Modifica) Razor .

La funzionalità di ricerca viene aggiunta nell'esercitazione successiva.

>[!div class="step-by-step"]
[Precedente: Utilizzo di SQL Server Local DB](xref:tutorials/razor-pages/sql)
[Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)
