---
title: Razor Pages con EF Core in ASP.NET Core - CRUD - 2 di 8
author: rick-anderson
description: Illustra come creare, leggere, aggiornare ed eliminare con EF Core
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 17d48cae50745508a64a9fb8a153b7b891e64a23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278687"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a>Razor Pages con EF Core in ASP.NET Core - CRUD - 2 di 8

Di [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

In questa esercitazione viene esaminato e personalizzato il codice CRUD (Create, Read, Update, Delete) con scaffolding.

Nota: per ridurre la complessità e mantenere queste esercitazioni incentrate su EF Core, viene usato il codice EF Core nei modelli di pagina di Razor Pages. Alcuni sviluppatori usano un modello di servizio o di repository per creare un livello di astrazione tra l'interfaccia utente (Razor Pages) e il livello di accesso ai dati.

In questa esercitazione vengono modificate le pagine Razor Create, Edit, Delete e Details nella cartella *Student*.

Il codice con scaffolding usa il modello seguente per le pagine Create, Edit e Delete:

* Ottenere e visualizzare i dati richiesti con il metodo HTTP GET `OnGetAsync`.
* Salvare le modifiche apportate ai dati con il metodo HTTP POST `OnPostAsync`.

Le pagine Index e Details ottengono e visualizzano i dati richiesti con il metodo HTTP GET `OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>Sostituire SingleOrDefaultAsync con FirstOrDefaultAsync

Il codice generato usa [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) per recuperare l'entità richiesta. [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) è più efficace nel recupero di una sola entità:

* A meno che il codice non necessiti di verificare che non sia presente più di un'entità restituita dalla query. 
* `SingleOrDefaultAsync` recupera una maggior quantità di dati ed esegue operazioni non necessarie.
* `SingleOrDefaultAsync` genera un'eccezione se è presente più di un'entità che soddisfa la parte del filtro.
*  `FirstOrDefaultAsync` non genera alcun elemento se è presente più di un'entità che soddisfa la parte del filtro.

Sostituire globalmente `SingleOrDefaultAsync` con `FirstOrDefaultAsync`. `SingleOrDefaultAsync` viene usato in 5 posizioni:

* `OnGetAsync` nella pagina Details.
* `OnGetAsync` e `OnPostAsync` nelle pagine Edit e Delete.

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

Nella maggior parte del codice con scaffolding è possibile usare [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) al posto di `FirstOrDefaultAsync` o `SingleOrDefaultAsync`. 

`FindAsync`:

* Individua un'entità con la chiave primaria. Se il contesto rileva un'entità con la chiave primaria, l'entità viene restituita senza una richiesta al database.
* È semplice e conciso.
* È ottimizzato per la ricerca di una singola entità.
* Può offrire migliori prestazioni, ma raramente nei normali scenari Web.
* Usa in modo implicito [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) anziché [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Se si vuole usare Include per includere altre entità, l'uso di Find non è più appropriato. Ciò significa che potrebbe essere necessario abbandonare Find e passare a una query durante la creazione dell'app.

## <a name="customize-the-details-page"></a>Personalizzare la pagina Details

Passare alla pagina `Pages/Students`. I collegamenti **Edit**, **Details** e **Delete** vengono generati dall'[helper del tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) nel file *Pages/Students/Index.cshtml*.

[!code-cshtml[](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Selezionare un collegamento Details. L'URL è nel formato `http://localhost:5000/Students/Details?id=2`. L'ID studente viene passato tramite una stringa di query (`?id=2`).

Aggiornare le pagine Razor Edit, Details e Delete in modo da usare il modello di route `"{id:int}"`. Modificare la direttiva page per ognuna di queste pagine da `@page` a `@page "{id:int}"`.

Una richiesta alla pagina con il modello di route "{id: int}" che **non** include il valore di route intero restituisce un errore HTTP 404 (Non trovato). Ad esempio, `http://localhost:5000/Students/Details` restituisce un errore 404. Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:

 ```cshtml
@page "{id:int?}"
```

Eseguire l'app, fare clic su un collegamento Details e verificare che l'URL passi l'ID come dati della route (`http://localhost:5000/Students/Details/2`).

Non modificare globalmente `@page` in `@page "{id:int}"` per non interrompere i collegamenti alle pagine Home e Create.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Aggiungere dati correlati

Il codice con scaffolding della pagina Students Index non include la proprietà `Enrollments`. In questa sezione i contenuti della raccolta `Enrollments` sono visualizzati nella pagina Details.

Il metodo `OnGetAsync` del file *Pages/Students/Details.cshtml.cs* usa il metodo `FirstOrDefaultAsync` per recuperare una singola entità `Student`. Aggiungere il codice evidenziato seguente:

[!code-csharp[](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

I metodi `Include` e `ThenInclude` fanno in modo che il contesto carichi la proprietà di navigazione `Student.Enrollments` e la proprietà di navigazione `Enrollment.Course` all'interno di ogni registrazione. Questi metodi vengono esaminati in dettaglio nell'esercitazione sulla lettura dei dati correlati.

Il metodo `AsNoTracking` migliora le prestazioni negli scenari in cui le entità restituite non vengono aggiornate nel contesto corrente. `AsNoTracking` è descritto più avanti in questa esercitazione.

### <a name="display-related-enrollments-on-the-details-page"></a>Visualizzare le registrazioni correlate nella pagina Details

Aprire *Pages/Students/Details.cshtml*. Per visualizzare un elenco delle registrazioni, aggiungere il codice evidenziato seguente:

 <!--2do ricka. if doesn't change, remove dup --> [!code-cshtml[](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Se dopo aver incollato il codice il rientro è errato, premere CTRL-K-D per correggerlo.

Il codice precedente esegue il ciclo nelle entità nella proprietà di navigazione `Enrollments`. Per ogni registrazione, il codice visualizza il titolo del corso e il voto. Il titolo del corso viene recuperato dall'entità Course memorizzata nella proprietà di navigazione `Course` dell'entità Enrollments.

Eseguire l'app, selezionare la scheda **Students** e fare clic sul collegamento **Details** relativo a uno studente. Viene visualizzato l'elenco dei corsi e dei voti dello studente selezionato.

## <a name="update-the-create-page"></a>Aggiornare la pagina Create

Aggiornare il metodo `OnPostAsync` in *Pages/Students/Create.cshtml.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Esaminare il codice [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_):

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

Nel codice precedente `TryUpdateModelAsync<Student>` tenta di aggiornare l'oggetto `emptyStudent` usando i valori di modulo inviati dalla proprietà [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) in [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync` aggiorna solo le proprietà elencate (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

Nell'esempio precedente:

* Il secondo argomento (` "student", // Prefix`) è il prefisso usato per cercare i valori. Non viene fatta distinzione tra maiuscole e minuscole.
* I valori di modulo inviati sono convertiti nei tipi nel modello `Student` usando l'[associazione di modelli](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Overposting

L'uso di `TryUpdateModel` per l'aggiornamento dei campi con i valori inviati è una procedura di sicurezza consigliata poiché impedisce l'overposting. Ad esempio, si supponga che l'entità Student includa una proprietà `Secret` che la pagina Web non deve aggiornare o aggiungere:

[!code-csharp[](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Anche se l'app non include un campo `Secret` nella pagina Razor Create o Update, un hacker potrebbe impostare il valore `Secret` tramite overposting. Un hacker potrebbe usare uno strumento come Fiddler oppure scrivere codice JavaScript per inviare un valore di modulo `Secret`. Il codice originale non limita i campi usati dallo strumento di associazione di modelli durante la creazione di un'istanza di Student.

Qualsiasi valore specificato dall'hacker per il campo di modulo `Secret` viene aggiornato nel database. L'immagine seguente illustra lo strumento Fiddler che aggiunge il campo `Secret` (con il valore "OverPost") ai valori di modulo inviati.

![Fiddler aggiunge il campo Secret](../ef-mvc/crud/_static/fiddler.png)

Il valore "OverPost" è stato aggiunto alla proprietà `Secret` della riga inserita. La progettazione app non prevedeva in alcun modo che la proprietà `Secret` venisse impostata con la pagina Create.

<a name="vm"></a>
### <a name="view-model"></a>Modello di visualizzazione

Un modello di visualizzazione contiene in genere un subset delle proprietà incluse nel modello usato dall'applicazione. Il modello di applicazione è spesso chiamato modello di dominio. Il modello di dominio contiene in genere tutte le proprietà richieste dall'entità corrispondente nel database. Il modello di visualizzazione contiene solo le proprietà necessarie per il livello di interfaccia utente, ad esempio la pagina Create. Oltre al modello di visualizzazione, alcune app usano un modello di associazione o un modello di input per passare i dati dalla classe del modello di pagina di Razor Pages al browser e viceversa. Si consideri il modello di visualizzazione `Student` seguente:

[!code-csharp[](intro/samples/cu/Models/StudentVM.cs)]

I modelli di visualizzazione rappresentano un altro metodo per impedire l'overposting. Il modello di visualizzazione contiene solo le proprietà da visualizzare o aggiornare.

Il codice seguente usa il modello di visualizzazione `StudentVM` per creare un nuovo studente:

[!code-csharp[](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

Il metodo [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) imposta i valori di questo oggetto leggendo i valori di un altro oggetto [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues). `SetValues` usa la corrispondenza dei nomi di proprietà. Poiché il tipo di modello di visualizzazione non deve essere correlato al tipo di modello, è sufficiente che abbia proprietà corrispondenti.

Se si usa `StudentVM` è necessario che [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) venga aggiornato per l'uso di `StudentVM` anziché `Student`.

In Razor Pages la classe derivata `PageModel` è il modello di visualizzazione. 

## <a name="update-the-edit-page"></a>Aggiornare la pagina Edit

Aggiornare il modello di pagina per la pagina Edit. Le modifiche principali sono evidenziate:

[!code-csharp[](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Le modifiche al codice sono simili alla pagina Create con alcune eccezioni:

* `OnPostAsync` include un parametro `id` facoltativo.
* Lo studente corrente viene recuperato dal database senza creare uno studente vuoto.
* `FirstOrDefaultAsync` è stato sostituito con [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync` è una scelta ottimale quando si seleziona un'entità dalla chiave primaria. Per altre informazioni, vedere [FindAsync](#FindAsync).

### <a name="test-the-edit-and-create-pages"></a>Testare le pagine Edit e Create

Creare e modificare alcune entità studente.

## <a name="entity-states"></a>Stati di entità

Il contesto del database tiene traccia della sincronizzazione delle entità in memoria con le righe corrispondenti nel database. Le informazioni di sincronizzazione del contesto del database determinano le operazioni eseguite quando viene chiamato `SaveChanges`. Ad esempio, quando una nuova entità viene passata al metodo `Add`, lo stato dell'entità viene impostato su `Added`. Quando viene chiamato `SaveChanges`, il contesto del database genera un comando SQL INSERT.

Le entità possono essere in uno dei seguenti stati:

* `Added`: l'entità non esiste ancora nel database. Il metodo `SaveChanges` genera un'istruzione INSERT.

* `Unchanged`: non è necessario salvare alcuna modifica con questa entità. Un'entità ha questo stato quando viene letta dal database.

* `Modified`: sono stati modificati alcuni o tutti i valori di proprietà dell'entità. Il metodo `SaveChanges` genera un'istruzione UPDATE.

* `Deleted`: l'entità è stata contrassegnata per l'eliminazione. Il metodo `SaveChanges` genera un'istruzione DELETE.

* `Detached`: l'entità non viene registrata dal contesto del database.

In un'applicazione desktop le modifiche dello stato vengono in genere impostate automaticamente. Viene letta un'entità, vengono apportate le modifiche e lo stato dell'entità viene modificato automaticamente in `Modified`. La chiamata di `SaveChanges` genera un'istruzione SQL UPDATE che aggiorna solo le proprietà modificate.

In un'app Web il `DbContext` che legge un'entità e visualizza i dati viene eliminato dopo il rendering di una pagina. Quando viene chiamato il metodo `OnPostAsync` di una pagina, viene effettuata una nuova richiesta Web con una nuova istanza di `DbContext`. La rilettura dell'entità nel nuovo contesto simula l'elaborazione desktop.

## <a name="update-the-delete-page"></a>Aggiornare la pagina Delete

In questa sezione viene aggiunto un codice per implementare un messaggio di errore personalizzato quando la chiamata a `SaveChanges` ha esito negativo. Aggiungere una stringa per i possibili messaggi di errore:

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Sostituire il metodo `OnGetAsync` con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Il codice precedente contiene il parametro facoltativo `saveChangesError`. `saveChangesError` indica se il metodo è stato chiamato dopo un errore di eliminazione dell'oggetto Student. L'operazione di eliminazione potrebbe non riuscire a causa di problemi di rete temporanei. Gli errori di rete temporanei sono più probabili nel cloud. `saveChangesError` ha valore false quando `OnGetAsync` della pagina Delete viene chiamato dall'interfaccia utente. Quando `OnGetAsync` viene chiamato da `OnPostAsync` (perché l'operazione di eliminazione ha avuto esito negativo), il parametro `saveChangesError` ha valore true.

### <a name="the-delete-pages-onpostasync-method"></a>Metodo OnPostAsync delle pagine Delete

Sostituire `OnPostAsync` con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Il codice precedente recupera l'entità selezionata, quindi chiama il metodo `Remove` per impostare lo stato dell'entità su `Deleted`. Quando viene chiamato `SaveChanges`, viene generato un comando SQL DELETE. Se `Remove` ha esito negativo:

* Viene rilevata l'eccezione di database.
* Il metodo `OnGetAsync` delle pagine Delete viene chiamato con `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Aggiornare la pagina Razor Delete

Aggiungere il messaggio di errore evidenziato seguente alla pagina Razor Delete.

[!code-cshtml[](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Eseguire il test di Delete.

## <a name="common-errors"></a>Errori comuni

Student/Home o altri collegamenti non funzionano:

Verificare che la pagina Razor contenga la direttiva `@page` corretta. Ad esempio, la pagina Razor Student/Home **non** deve contenere un modello di route:

```cshtml
@page "{id:int}"
```

Ogni pagina Razor deve includere la direttiva `@page`.

> [!div class="step-by-step"]
> [Precedente](xref:data/ef-rp/intro)
> [Successivo](xref:data/ef-rp/sort-filter-page)
