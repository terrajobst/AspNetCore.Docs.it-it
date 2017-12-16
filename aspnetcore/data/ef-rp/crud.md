---
title: Pagine Razor con Entity Framework Core - CRUD - 2 di 8
author: rick-anderson
description: Viene illustrato come creare, leggere, aggiornare ed eliminare con Entity Framework di base
keywords: ASP.NET Core, Entity Framework Core, CRUD, creare, leggere, aggiornare, eliminare
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 163bc35afed0bf1d9236935d5ce60e6975356594
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/15/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>Creare, leggere, aggiornare ed eliminare - Core EF con pagine Razor (2 di 8)

Da [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

In questa esercitazione, lo scaffolding CRUD (create, leggere, aggiornare ed eliminare) codice viene esaminato e personalizzati.

Nota: Per ridurre la complessità e mantenere queste esercitazioni con stato attivo in Entity Framework Core, il codice di Entity Framework Core viene utilizzato nei file di codice Razor pagine. Alcuni sviluppatori utilizzano un modello di repository o livello di servizio in per creare un livello di astrazione tra l'interfaccia utente (pagine Razor) e il livello di accesso ai dati.

In questa esercitazione, crea, modifica, eliminazione e pagine Razor dettagli di *Student* cartella vengono modificati.

Per le pagine di creazione, modifica ed eliminazione, il codice di scaffolding utilizza il modello seguente:

* Ottenere e visualizzare i dati richiesti con il metodo HTTP GET `OnGetAsync`.
* Salvare le modifiche ai dati con il metodo HTTP POST `OnPostAsync`.

Pagine dell'indice e i dettagli di ottenere e visualizzare i dati richiesti con il metodo HTTP GET`OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>Sostituire SingleOrDefaultAsync con FirstOrDefaultAsync

Il codice generato utilizza [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) per recuperare l'entità richiesta. [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) risulta più efficiente il recupero di un'entità:

* A meno che non è necessario verificare che non è più di un'entità restituita dalla query. 
* `SingleOrDefaultAsync`Recupera i dati più ed esegue operazioni non necessarie.
* `SingleOrDefaultAsync`genera un'eccezione se è presente più di un'entità che soddisfa la parte del filtro.
*  `FirstOrDefaultAsync`non genera eccezioni se è presente più di un'entità che soddisfa la parte del filtro.

Sostituire globalmente `SingleOrDefaultAsync` con `FirstOrDefaultAsync`. `SingleOrDefaultAsync`viene utilizzata nelle 5 posizioni:

* `OnGetAsync`Nella pagina dei dettagli.
* `OnGetAsync`e `OnPostAsync` nelle pagine di modifica e l'eliminazione.

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

La maggior parte del codice scaffolding, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) può essere utilizzato al posto di `FirstOrDefaultAsync` o `SingleOrDefaultAsync`. 

`FindAsync`:

* Consente di individuare un'entità con la chiave primaria (PK). Se un'entità con la chiave pubblica è stata rilevata dal contesto, viene restituito senza una richiesta al database.
* È semplice e conciso.
* È ottimizzato per cercare una singola entità.
* Possono avere i vantaggi di prestazioni in alcune situazioni, ma vengono raramente entrano in gioco per gli scenari web normale.
* Utilizza in modo implicito [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) anziché [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Se si desidera includere altre entità, allora trova non è più appropriato. Ciò significa che potrebbe essere necessario abbandonare Find e spostare in una query durante l'avanzamento app.

## <a name="customize-the-details-page"></a>Personalizzare la pagina dei dettagli

Passare a `Pages/Students` pagina. Il **modifica**, **dettagli**, e **eliminare** collegamenti vengono generati dal [Helper di Tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) nel *pagine e gli studenti / Cshtml* file.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Selezionare un collegamento di dettagli. L'URL è nel formato `http://localhost:5000/Students/Details?id=2`. L'ID dello studente viene passato utilizzando una stringa di query (`?id=2`).

Aggiornare la modifica, dettagli ed Elimina pagine Razor da utilizzare il `"{id:int}"` modello di route. Modificare la direttiva page per ognuna di queste pagine da `@page` a `@page "{id:int}"`.

Una richiesta per la pagina con il modello di route "{id: int}" che esegue **non** includono un numero intero route valore restituisce HTTP 404 (non trovato) l'errore. Ad esempio, `http://localhost:5000/Students/Details` restituisce un errore 404. Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:

 ```cshtml
@page "{id:int?}"
```

Eseguire l'app, fare clic su un collegamento di dettagli e verificare l'URL è passando l'ID come dati della route (`http://localhost:5000/Students/Details/2`).

Non modificare globalmente `@page` a `@page "{id:int}"`, in questo modo viene interrotta i collegamenti per la home page e creare pagine.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Aggiungere i dati correlati

Il codice per la pagina di indice di studenti scaffolding non include il `Enrollments` proprietà. In questa sezione, il contenuto del `Enrollments` raccolta viene visualizzata nella pagina dei dettagli.

Il `OnGetAsync` metodo *Pages/Students/Details.cshtml.cs* utilizza il `FirstOrDefaultAsync` metodo per recuperare un singolo `Student` entità. Aggiungere il codice evidenziato di seguito:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

Il `Include` e `ThenInclude` metodi che il contesto di caricamento di `Student.Enrollments` proprietà di navigazione e all'interno di ogni registrazione di `Enrollment.Course` proprietà di navigazione. Questi metodi sono examinied in dettaglio in questa esercitazione i dati relativi alla lettura.

Il `AsNoTracking` metodo migliora le prestazioni negli scenari quando le entità restituite non vengono aggiornati nel contesto corrente. `AsNoTracking`viene illustrata più avanti in questa esercitazione.

### <a name="display-related-enrollments-on-the-details-page"></a>Visualizzare le registrazioni correlate nella pagina dei dettagli

Aprire *Pages/Students/Details.cshtml*. Aggiungere il codice evidenziato di seguito per visualizzare un elenco delle registrazioni:

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Se dopo avere incollato il codice, rientro del codice non è corretto, premere CTRL-K-D per correggere l'errore.

Il codice precedente consente di scorrere le entità nel `Enrollments` proprietà di navigazione. Per ogni registrazione, viene visualizzato il titolo del corso e il livello. Il titolo del corso viene recuperato dall'entità che viene archiviato in corso la `Course` proprietà di navigazione di entità le registrazioni.

Eseguire l'app, selezionare il **studenti** scheda, quindi scegliere il **dettagli** collegamento per uno studente. Viene visualizzato l'elenco di corsi e livelli per gli studenti selezionato.

## <a name="update-the-create-page"></a>Aggiornare la pagina di creazione

Aggiornamento di `OnPostAsync` metodo *Pages/Students/Create.cshtml.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Esaminare il [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) codice:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

Nel codice precedente, `TryUpdateModelAsync<Student>` tenta di aggiornare il `emptyStudent` utilizzando i valori del form inseriti dal [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) proprietà il [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync`Aggiorna solo le proprietà elencate (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

Nell'esempio precedente:

* Il secondo argomento (` "student", // Prefix`) è il prefisso utilizzato per cercare i valori. Non è tra maiuscole e minuscole.
* I valori del form inseriti vengono convertiti in tipi nel `Student` del modello utilizzando [associazione del modello](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Overposting

Utilizzando `TryUpdateModel` per aggiornare i campi con valori pubblicati è una protezione ottimale, perché impedisce overposting. Ad esempio, si supponga che l'entità Student includa un `Secret` proprietà di questa pagina web non devono aggiornare o aggiungere:

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Anche se l'app non dispone di un `Secret` campo pagina Razor, un pirata informatico create/update è stato possibile impostare il `Secret` valore overposting. Un pirata informatico potrebbe utilizzare uno strumento come Fiddler o scrivere alcuni JavaScript, per registrare un `Secret` modulo valore. Il codice originale non si limita i campi che il gestore di associazione del modello viene utilizzato durante la creazione di un'istanza di studenti.

Valore che il pirata informatico specificato per il `Secret` campo del form viene aggiornato nel database. La figura seguente mostra l'aggiunta di strumento Fiddler il `Secret` campo (con il valore "OverPost") per i valori del form inseriti.

![Campo segreto aggiunta Fiddler](../ef-mvc/crud/_static/fiddler.png)

Il valore "OverPost" viene aggiunto al `Secret` proprietà della riga inserita. La finestra di progettazione di app non è destinata la `Secret` proprietà da impostare con la pagina di creazione.

<a name="vm"></a>
### <a name="view-model"></a>Modello di visualizzazione

In genere, un modello di visualizzazione contiene un subset delle proprietà incluse nel modello utilizzato dall'applicazione. Il modello di applicazione è spesso definito il modello di dominio. Il modello di dominio in genere contiene tutte le proprietà necessarie per l'entità corrispondente nel database. Il modello di visualizzazione contiene solo le proprietà necessarie per il livello di interfaccia utente (ad esempio, la pagina Crea). Oltre al modello di visualizzazione, alcune applicazioni di utilizzano un modello di associazione o il modello di input per passare dati tra la classe code-behind pagine Razor e il browser. Tenere presente quanto segue `Student` modello di visualizzazione:

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

Visualizzazione di modelli fornisce un modo alternativo per impedire overposting. Il modello di visualizzazione contiene solo le proprietà per visualizzare (visualizzazione) o l'aggiornamento.

Il codice seguente usa il `StudentVM` modello di visualizzazione per creare un nuovo studente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

Il [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metodo imposta i valori di questo oggetto tramite la lettura di valori da un altro [i PropertyValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) oggetto. `SetValues`Usa la corrispondenza dei nomi di proprietà. Il tipo di modello di visualizzazione non deve essere correlato al tipo di modello, ma è sufficiente disporre le proprietà corrispondenti.

Utilizzando `StudentVM` richiede [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) aggiornate per l'utilizzo `StudentVM` anziché `Student`.

Nelle pagine Razor, la `PageModel` classe derivata è il modello di visualizzazione. 

## <a name="update-the-edit-page"></a>Aggiornare la pagina di modifica

Aggiornare il file code-behind pagina Modifica:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Le modifiche al codice sono simili alla pagina Crea con alcune eccezioni:

* `OnPostAsync`è facoltativo `id` parametro.
* Gli studenti correnti vengono recuperati dal database, invece di creare un studente vuoto.
* `FirstOrDefaultAsync`è stato sostituito con [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync`è una scelta ottimale quando si seleziona un'entità dalla chiave primaria. Vedere [FindAsync](#FindAsync) per ulteriori informazioni.

### <a name="test-the-edit-and-create-pages"></a>La modifica di test e creare pagine

Creare e modificare alcune entità studente.

## <a name="entity-states"></a>Stati di entità

Il contesto DB tiene traccia delle entità in memoria siano sincronizzate con le relative righe corrispondenti nel database di. Le informazioni di sincronizzazione di contesto DB determinano cosa accade quando `SaveChanges` viene chiamato. Ad esempio, quando una nuova entità viene passata al `Add` (metodo), che lo stato dell'entità è impostato su `Added`. Quando `SaveChanges` viene chiamato, il database contesto esegue un comando SQL INSERT.

Un'entità può essere in uno dei seguenti stati:

* `Added`: L'entità non esiste ancora nel database. Il `SaveChanges` metodo genera un'istruzione INSERT.

* `Unchanged`: Nessuna modifica desidera essere salvati con questa entità. Un'entità ha questo stato quando viene letto dal database.

* `Modified`: Sono stati modificati alcuni o tutti i valori di proprietà dell'entità. Il `SaveChanges` metodo genera un'istruzione UPDATE.

* `Deleted`: L'entità è stato contrassegnato per l'eliminazione. Il `SaveChanges` metodo genera un'istruzione DELETE.

* `Detached`: L'entità non viene rilevato dal contesto DB.

In un'applicazione desktop, le modifiche dello stato vengono in genere impostate automaticamente. Un'entità è di lettura, vengono apportate modifiche e lo stato dell'entità viene convertito automaticamente in `Modified`. La chiamata `SaveChanges` genera un'istruzione SQL UPDATE che aggiorna solo le proprietà modificate.

In un'app web, il `DbContext` che legge un'entità e consente di visualizzare i dati sono stati eliminati dopo il rendering di una pagina. Quando una pagina `OnPostAsync` metodo viene chiamato, viene effettuata una richiesta web nuovo e con una nuova istanza di `DbContext`. Rilettura delle entità in tale contesto nuovo simula l'elaborazione del desktop.

## <a name="update-the-delete-page"></a>Aggiornare la pagina di eliminazione

In questa sezione viene aggiunto codice per implementare un errore personalizzato messaggio quando la chiamata a `SaveChanges` ha esito negativo. Aggiungere una stringa contenente i messaggi di errore possile:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Sostituire il metodo `OnGetAsync` con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Il codice precedente contiene il parametro facoltativo `saveChangesError`. `saveChangesError`indica se il metodo è stato chiamato dopo un errore di eliminazione dell'oggetto studente. L'operazione di eliminazione potrebbe non riuscire a causa di temporanei problemi di rete. Gli errori di rete temporanei sono più probabile che nel cloud. `saveChangesError`è false quando la pagina di eliminazione `OnGetAsync` viene chiamato dall'interfaccia utente. Quando `OnGetAsync` viene chiamato da `OnPostAsync` (perché l'operazione di eliminazione non riuscita), il `saveChangesError` parametro è true.

### <a name="the-delete-pages-onpostasync-method"></a>Metodo Delete pagine OnPostAsync

Sostituire il `OnPostAsync` con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Il codice precedente recupera l'entità selezionata, quindi chiama il `Remove` per impostare lo stato dell'entità su `Deleted`. Quando `SaveChanges` viene chiamato un SQL DELETE comando viene generato. Se `Remove` ha esito negativo:

* Viene rilevata l'eccezione di database.
* Le pagine di eliminazione `OnGetAsync` metodo viene chiamato con `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Aggiornare la pagina di eliminazione Razor

Aggiungere il seguente messaggio di errore evidenziato eliminare Razor.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Eliminazione di test.

## <a name="common-errors"></a>Errori comuni

Non funzionano studente/Home o altri collegamenti:

Verificare la pagina Razor contiene corrette `@page` direttiva. Ad esempio, la pagina di Razor studente/iniziale deve **non** contiene un modello di route:

```cshtml
@page "{id:int}"
```

Ogni pagina Razor deve includere il `@page` direttiva.

>[!div class="step-by-step"]
[Precedente](xref:data/ef-rp/intro)
[Successivo](xref:data/ef-rp/sort-filter-page)
