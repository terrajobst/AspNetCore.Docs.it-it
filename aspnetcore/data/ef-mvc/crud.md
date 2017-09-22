---
title: Core di ASP.NET MVC con Entity Framework Core - CRUD - 2 di 10
author: tdykstra
description: 
keywords: ASP.NET Core, Entity Framework Core, CRUD, creare, leggere, aggiornare, eliminare
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 6e1cd570-40f1-4b24-8b6e-7d2d27758f18
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/crud
ms.openlocfilehash: 9fc2b4c126c4d109deb2125f0db70a355c04eb15
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>Creare, leggere, aggiornare ed eliminare - EF Core con l'esercitazione di base di ASP.NET MVC (2 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).

Nell'esercitazione precedente, creazione di un'applicazione MVC che archivia e Visualizza dati tramite Entity Framework sia LocalDB di SQL Server. In questa esercitazione, si sarà esaminare e personalizzare il CRUD (create, leggere, aggiornare ed eliminare) il codice che lo scaffolding di MVC il viene creata automaticamente nel controller e visualizzazioni.

> [!NOTE] 
> È pratica comune per implementare il modello di repository per creare un livello di astrazione tra il controller e il livello di accesso ai dati. Per mantenere queste esercitazioni semplice e sono specifici per illustrare come utilizzare Entity Framework stesso, non usano repository. Per informazioni sul repository con Entity Framework, vedere [dell'ultima esercitazione di questa serie](advanced.md).

In questa esercitazione, è possibile utilizzare le pagine web seguenti:

![Pagina dei dettagli per studenti](crud/_static/student-details.png)

![Pagina Crea Student](crud/_static/student-create.png)

![Pagina di modifica per studenti](crud/_static/student-edit.png)

![Pagina di eliminazione per studenti](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>Personalizzare la pagina dei dettagli

Il codice per la pagina di indice di studenti scaffolding tralasciato il `Enrollments` proprietà, in quanto tale proprietà contiene una raccolta. Nel **dettagli** pagina verranno visualizzati il contenuto della raccolta in una tabella HTML.

In *Controllers/StudentsController.cs*, il metodo di azione per i dettagli visualizzare utilizza il `SingleOrDefaultAsync` metodo per recuperare un singolo `Student` entità. Aggiungere il codice che chiama `Include`. `ThenInclude`, e `AsNoTracking` metodi, come illustrato nel seguente codice evidenziato.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

Il `Include` e `ThenInclude` metodi che il contesto di caricamento di `Student.Enrollments` proprietà di navigazione e all'interno di ogni registrazione di `Enrollment.Course` proprietà di navigazione.  Si apprenderà ulteriori informazioni su questi metodi di [la lettura dei dati correlati](read-related-data.md) esercitazione.

Il `AsNoTracking` metodo migliora le prestazioni negli scenari in cui le entità restituite non verranno aggiornate nel corso della durata del contesto corrente. Verranno fornite altre informazioni su `AsNoTracking` alla fine di questa esercitazione.

### <a name="route-data"></a>Dati sull'itinerario

Il valore della chiave che viene passato per il `Details` metodo proviene da *indirizzare dati*. Dati di route sono presenti il gestore di associazione del modello in un segmento dell'URL. Ad esempio, la route predefinita specifica segmenti controller, azione e id:

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

L'URL seguente, la route predefinita esegue il mapping istruttore come controller di indice dell'azione e 1 come id. Questi sono valori di dati di route.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

L'ultima parte dell'URL ("? courseID = 2021") è un valore di stringa di query. Lo strumento di associazione del modello verrà inoltre passare il valore di ID per il `Details` metodo `id` parametro se viene passato come un valore di stringa di query:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Nella pagina di indice, vengono creati gli URL di collegamento ipertestuale da istruzioni helper di tag nella visualizzazione Razor. Nel codice Razor seguente, il `id` parametro corrisponde la route predefinita, pertanto `id` viene aggiunto ai dati di route.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Verrà generato il seguente codice HTML quando `item.ID` è 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

Nel seguente codice Razor, `studentID` non corrisponde a un parametro di route predefinita, pertanto viene aggiunta come una stringa di query.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Verrà generato il seguente codice HTML quando `item.ID` è 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Per ulteriori informazioni sugli helper di tag, vedere [gli helper di Tag in ASP.NET Core](xref:mvc/views/tag-helpers/intro).

### <a name="add-enrollments-to-the-details-view"></a>Aggiungere le registrazioni per la visualizzazione dei dettagli

Aprire *Views/Students/Details.cshtml*. Ogni campo viene visualizzato utilizzando `DisplayNameFor` e `DisplayFor` helper, come illustrato nell'esempio seguente:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Dopo l'ultimo campo e immediatamente prima della chiusura `</dl>` tag, aggiungere il codice seguente per visualizzare un elenco delle registrazioni:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Se dopo avere incollato il codice, rientro del codice non è corretto, premere CTRL-K-D per correggere l'errore.

Questo codice consente di scorrere le entità di `Enrollments` proprietà di navigazione. Per ogni registrazione, viene visualizzato il titolo del corso e il livello. Il titolo del corso viene recuperato dall'entità che viene archiviato in corso la `Course` proprietà di navigazione di entità le registrazioni.

Eseguire l'app, selezionare il **studenti** scheda, quindi scegliere il **dettagli** collegamento per uno studente. Per gli studenti selezionato vedere l'elenco di corsi e livelli:

![Pagina dei dettagli per studenti](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Aggiornare la pagina di creazione

In *StudentsController.cs*, modificare HttpPost `Create` metodo aggiunta di un blocco try-catch e rimuovendo l'ID di `Bind` attributo.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Questo codice aggiunge l'entità di studenti creato dal gestore di associazione di modelli ASP.NET MVC per l'entità di studenti set e quindi Salva le modifiche apportate al database. (Strumento di associazione fa riferimento alle funzionalità di ASP.NET MVC che rende più semplice da utilizzare con i dati inviati da un form, converte i valori del form inseriti a tipi CLR e li passa al metodo di azione nei parametri di un raccoglitore di modelli. In questo caso, il gestore di associazione del modello crea un'istanza di un'entità Student mediante i valori delle proprietà dalla raccolta di Form.)

Si è rimosso `ID` dal `Bind` perché l'ID è il valore di chiave primaria che SQL Server verrà impostato automaticamente quando viene inserita la riga dell'attributo. Input dell'utente non viene impostato il valore ID.

Diverso dal `Bind` attributo, il blocco try-catch è l'unica modifica apportate al codice di scaffolding. Se un'eccezione che deriva da `DbUpdateException` è rilevata durante il salvataggio delle modifiche, viene visualizzato un messaggio di errore generico. `DbUpdateException`eccezioni in alcuni casi sono causate da un elemento esterno per l'applicazione, anziché un errore di programmazione, pertanto l'utente è consigliabile ripetere l'operazione. Anche se non è implementato in questo esempio, un'applicazione di qualità di produzione l'accesso utilizzando l'eccezione. Per ulteriori informazioni, vedere il **Log per informazioni dettagliate** sezione [monitoraggio e telemetria (compilazione reali le app Cloud mediante Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

Il `ValidateAntiForgeryToken` attributo consente di impedire attacchi di cross-site request forgery (CSRF). Il token viene inserito automaticamente nella visualizzazione per il [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) e viene incluso quando il form viene inviato dall'utente. Il token viene convalidato dal `ValidateAntiForgeryToken` attributo. Per ulteriori informazioni su CSRF, vedere [anti-richiesta falsificazione](../../security/anti-request-forgery.md).

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>Nota sulla sicurezza su overposting

Il `Bind` attributo che include il codice di supporto temporaneo nel `Create` metodo è un modo per proteggersi da overposting in scenari di creazione. Ad esempio, si supponga che l'entità Student includa un `Secret` proprietà che non si desidera questa pagina web da impostare.

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

Anche se non è un `Secret` campo della pagina web, un pirata informatico potrebbe utilizzare uno strumento come Fiddler o scrivere alcuni JavaScript, per registrare un `Secret` modulo valore. Senza il `Bind` attributo limitando i campi che il gestore di associazione del modello viene utilizzato durante la creazione di un'istanza di Student, lo strumento di associazione del modello preleverebbe che `Secret` formato valore e utilizzarlo per creare l'istanza di entità studente. Quindi indipendentemente dal valore specificato per hacker il `Secret` campo modulo verranno aggiornato nel database. La figura seguente mostra l'aggiunta di strumento Fiddler il `Secret` campo (con il valore "OverPost") per i valori del form inseriti.

![Campo segreto aggiunta Fiddler](crud/_static/fiddler.png)

Il valore "OverPost" viene quindi aggiunto correttamente al `Secret` proprietà dell'oggetto inserito di riga, anche se non si intendeva mai che la pagina web in grado di impostare tale proprietà.

È possibile impedire overposting negli scenari di modifica, lettura innanzitutto l'entità dal database e chiamando quindi `TryUpdateModel`, passando un elenco di proprietà consentiti esplicite. Si tratta del metodo utilizzato in queste esercitazioni.

Un modo alternativo per impedire overposting che è preferibile utilizzare molti sviluppatori è usare visualizzazione di modelli anziché le classi di entità con l'associazione di modelli. Includere solo le proprietà che si desidera aggiornare il modello di visualizzazione. Al termine il raccoglitore di modelli MVC, copiare le proprietà del modello di visualizzazione per l'istanza di entità, eventualmente utilizzando uno strumento come AutoMapper. Utilizzare `_context.Entry` nell'istanza di entità su cui impostare il relativo stato `Unchanged`, quindi impostare `Property("PropertyName").IsModified` su true in ogni proprietà di entità che è incluso nel modello di visualizzazione. Questo funzionamento del metodo sia in modifica e creare scenari.

### <a name="test-the-create-page"></a>La pagina di creazione di test

Il codice in *Views/Students/Create.cshtml* Usa `label`, `input`, e `span` (per i messaggi di convalida) gli helper per ogni campo di tag.

Eseguire l'app, selezionare il **studenti** scheda e fare clic su **Crea nuovo**.

Immettere i nomi e una data. Provare a immettere una data non valida se il browser consente di eseguire questa operazione. (Alcuni browser impongono l'utilizzo di un controllo selezione data). Quindi fare clic su **crea** per visualizzare il messaggio di errore.

![Errore di convalida Data](crud/_static/date-error.png)

Si tratta di convalida sul lato server che si ottiene per impostazione predefinita. in un'esercitazione successiva si noterà come aggiungere gli attributi che generano anche il codice per la convalida lato client. Il codice evidenziato di seguito viene illustrato il controllo di convalida nel modello di `Create` metodo.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Modificare la data in un valore valido e fare clic su **crea** per vedere il nuovo studente nel **indice** pagina.

## <a name="update-the-edit-page"></a>Aggiornare la pagina di modifica

In *StudentController.cs*, il HttpGet `Edit` metodo (quello senza il `HttpPost` attributo) utilizza il `SingleOrDefaultAsync` metodo per recuperare l'entità selezionata Student, come specificato nella `Details` (metodo). Non è necessario modificare questo metodo.

### <a name="recommended-httppost-edit-code-read-and-update"></a>Modifica HttpPost codice consigliato: lettura e aggiornamento

Sostituire il metodo di azione di modifica HttpPost con il codice seguente.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Queste modifiche implementano una procedura consigliata per evitare di overposting. Il scaffolder generato un `Bind` attributo e aggiungere l'entità creata dal gestore di associazione del modello dell'entità impostata con un `Modified` flag. Che codice non è consigliato per molti scenari, in quanto il `Bind` attributo Cancella tutti i dati esistenti nei campi non elencati nel `Include` parametro.

Il nuovo codice legge di entità esistente e chiama `TryUpdateModel` per aggiornare i campi dell'entità recuperati [basato sull'input dell'utente nei dati del form inseriti](xref:mvc/models/model-binding#how-model-binding-works). Rilevamento delle modifiche automatico set del Entity Framework di `Modified` flag per i campi che sono stati modificati dall'input del form. Quando il `SaveChanges` metodo viene chiamato, Entity Framework consente di creare istruzioni SQL per aggiornare la riga di database. I conflitti di concorrenza vengono ignorati e vengono aggiornate solo le colonne della tabella che sono state aggiornate dall'utente nel database. (Un'esercitazione successiva viene illustrato come gestire i conflitti di concorrenza).

Come procedura consigliata per evitare di overposting, i campi che si desidera essere aggiornabile per la **modifica** pagina sono inclusi nel `TryUpdateModel` parametri. (Una stringa vuota che precede l'elenco dei campi nell'elenco di parametri è per un prefisso da utilizzare con i nomi di campi form). Attualmente non esistono campi aggiuntivi che si vuole proteggere, ma l'elenco di campi che si desidera lo strumento di associazione per l'associazione assicura che se si aggiungono campi al modello di dati in futuro, che siano automaticamente protetti fino a quando non vengono aggiunti esplicitamente qui.

In seguito a queste modifiche, la firma del metodo di HttpPost `Edit` corrisponde al metodo di HttpGet `Edit` metodo; pertanto è stato rinominato il metodo `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Il codice modifica HttpPost alternativo: creazione e collegamento

Il codice di modifica di HttpPost consigliato assicura che solo le colonne modificate vengono aggiornati e mantiene i dati nelle proprietà che non si desidera includere per l'associazione di modelli. Tuttavia, l'approccio incentrato lettura richiede un database aggiuntivo leggere e può generare codice più complesso per gestire i conflitti di concorrenza. Un'alternativa consiste nel connettere un'entità creata dal gestore di associazione del modello per il contesto di Entity Framework e contrassegnarlo come modificata. (Non aggiorna il progetto con questo codice è illustrato solo per illustrare un approccio facoltativo.) 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

È possibile utilizzare questo approccio quando l'interfaccia utente della pagina web include tutti i campi dell'entità e aggiornarne uno di essi.

Il codice di scaffolding utilizza l'approccio di creare e collegare ma intercetta solo le `DbUpdateConcurrencyException` eccezioni e restituisce i codici di errore 404.  Nell'esempio illustrato genera un'eccezione di aggiornamento del database e visualizza un messaggio di errore.

### <a name="entity-states"></a>Stati di entità

Il contesto di database tiene traccia del fatto che le entità in memoria vengono sincronizzate con le relative righe corrispondenti nel database e questa informazione determina cosa accade quando si chiama il `SaveChanges` metodo. Ad esempio, quando si passa una nuova entità per il `Add` (metodo), che lo stato dell'entità è impostato su `Added`. Quando si chiama il `SaveChanges` (metodo), il contesto del database esegue un comando SQL INSERT.

Un'entità può essere in uno dei seguenti stati:

* `Added`. L'entità non esiste ancora nel database. Il `SaveChanges` metodo genera un'istruzione INSERT.

* `Unchanged`. Non deve essere eseguita con questa entità per il `SaveChanges` metodo. Quando si legga un'entità dal database, l'entità che inizia con questo stato.

* `Modified`. Sono stati modificati alcuni o tutti i valori di proprietà dell'entità. Il `SaveChanges` metodo genera un'istruzione UPDATE.

* `Deleted`. L'entità è stato contrassegnato per l'eliminazione. Il `SaveChanges` metodo genera un'istruzione DELETE.

* `Detached`. L'entità non viene rilevato dal contesto del database.

In un'applicazione desktop, le modifiche dello stato vengono in genere impostate automaticamente. Un'entità di leggere e modificare alcuni valori delle relative proprietà. In questo modo lo stato dell'entità viene convertito automaticamente in `Modified`. Quando si chiama `SaveChanges`, Entity Framework genera un'istruzione SQL UPDATE che aggiorna solo le proprietà effettive che è stato modificato.

In un'app web, il `DbContext` che legge inizialmente un'entità e consente di visualizzare i dati da modificare sono stati eliminati dopo il rendering di una pagina. Quando il HttpPost `Edit` viene chiamato il metodo di azione, viene effettuata una nuova richiesta web e si dispone di una nuova istanza di `DbContext`. Se si legge nuovamente l'entità in tale contesto di nuovo, consente di simulare l'elaborazione del desktop.

Tuttavia, se non si desidera eseguire questa operazione di lettura aggiuntivo, è necessario utilizzare l'oggetto entità creato dal gestore di associazione del modello.  Il modo più semplice per eseguire questa operazione è impostare lo stato dell'entità su Modified, come avviene nel codice alternativo HttpPost modifica illustrato in precedenza. Quando si chiama `SaveChanges`, Entity Framework Aggiorna tutte le colonne della riga di database, poiché il contesto non ha modo di conoscere le proprietà che è stato modificato.

Se si desidera evitare l'approccio incentrato lettura, ma si desidera anche l'istruzione SQL UPDATE per aggiornare solo i campi che l'utente ha effettivamente modificato, il codice è più complesso. È necessario salvare i valori originali in qualche modo (ad esempio utilizzando nascosti) in modo che siano disponibili quando HttpPost `Edit` metodo viene chiamato. È possibile creare un'entità di Student usando i valori originali, chiamata di `Attach` metodo con tale versione originale dell'entità, aggiornare i valori dell'entità per i nuovi valori e quindi chiamare `SaveChanges`.

### <a name="test-the-edit-page"></a>Pagina di modifica di test

Eseguire l'app, selezionare il **studenti** tab, quindi fare clic su un **modifica** collegamento ipertestuale.

![Pagina Modifica studenti](crud/_static/student-edit.png)

Modificare alcuni dei dati e fare clic su **salvare**. Il **indice** verrà visualizzata la pagina e verranno visualizzati i dati modificati.

## <a name="update-the-delete-page"></a>Aggiornare la pagina di eliminazione

In *StudentController.cs*, il codice del modello per il HttpGet `Delete` metodo utilizza il `SingleOrDefaultAsync` metodo per recuperare l'entità selezionata Student, come visto dei dettagli e modifica i metodi. Tuttavia, per implementare un personalizzata del messaggio di errore quando la chiamata a `SaveChanges` ha esito negativo, si aggiungeranno alcune funzionalità di questo metodo e la relativa visualizzazione corrispondente.

Come è stato illustrato per l'aggiornamento e creare operazioni, le operazioni di eliminazione richiedono due metodi di azione. Il metodo viene chiamato in risposta a una richiesta GET che consente di visualizzare una vista che consente all'utente di approvare o annullare l'operazione di eliminazione. Se l'utente approva, viene creata una richiesta POST. Quando ciò si verifica, HttpPost `Delete` metodo viene chiamato e quindi tale metodo esegue effettivamente l'operazione di eliminazione.

Si aggiungerà un blocco try-catch per HttpPost `Delete` metodo per gestire eventuali errori che potrebbero verificarsi quando il database viene aggiornato. Se si verifica un errore, il metodo HttpPost Delete chiama il metodo HttpGet Delete, passando un parametro che indica che si è verificato un errore. Il metodo HttpGet Delete viene quindi visualizzata nuovamente la pagina di conferma con il messaggio di errore, che consente all'utente la possibilità di annullare o ripetere l'operazione.

Sostituire il HttpGet `Delete` metodo di azione con il codice seguente, che gestisce la segnalazione errori.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Questo codice accetta un parametro facoltativo che indica se il metodo è stato chiamato dopo un errore di salvare le modifiche. Questo parametro è false quando la HttpGet `Delete` metodo viene chiamato senza un precedente errore. Quando viene chiamato da HttpPost `Delete` metodo in risposta a un errore di aggiornamento del database, il parametro è true e un messaggio di errore viene passato alla visualizzazione.

### <a name="the-read-first-approach-to-httppost-delete"></a>L'approccio di lettura-first per HttpPost Delete

Sostituire il HttpPost `Delete` metodo di azione (denominato `DeleteConfirmed`) con il codice seguente, che esegue l'operazione di eliminazione effettiva e rileva eventuali errori di aggiornamento del database.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

Questo codice viene recuperato l'entità selezionata, quindi chiama il `Remove` per impostare lo stato dell'entità su `Deleted`. Quando `SaveChanges` viene chiamato un SQL DELETE comando viene generato.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>L'approccio di creare e collegare alla HttpPost Delete

Se il miglioramento delle prestazioni in un'applicazione con volumi elevati è una priorità, è possibile evitare una query SQL non necessaria creando un'entità di Student usando solo il database primario della chiave di valore e quindi impostare lo stato dell'entità `Deleted`. Questo è tutto Entity Framework necessarie per eliminare l'entità. (Non inserire questo codice nel progetto, è solo per illustrare un'alternativa).

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Se l'entità dispone di dati correlati che devono essere eliminati anche, assicurarsi che tale eliminazione a catena è configurato nel database. Con questo approccio per l'eliminazione di entità, EF potrebbero non essere consapevoli esistono entità correlate da eliminare.

### <a name="update-the-delete-view"></a>Aggiornare la visualizzazione di eliminazione

In *Views/Student/Delete.cshtml*, aggiungere un messaggio di errore tra l'intestazione h2 e il titolo h3, come illustrato nell'esempio seguente:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Eseguire l'app, selezionare il **studenti** scheda e fare clic su un **eliminare** collegamento ipertestuale:

![Elimina pagina di conferma](crud/_static/student-delete.png)

Fare clic su **eliminare**. Verrà visualizzata la pagina di indice senza gli studenti eliminato. (Verrà visualizzato un esempio di codice in esecuzione nell'esercitazione di concorrenza di gestione degli errori.)

## <a name="closing-database-connections"></a>La chiusura delle connessioni di database

Per liberare le risorse che contiene una connessione al database, l'istanza del contesto deve essere eliminato presto quando al suo completamento. L'oggetto incorporato di ASP.NET Core [inserimento di dipendenze](../../fundamentals/dependency-injection.md) si occupa dell'attività per l'utente.

In *Startup.cs*, si chiama il [il metodo di estensione AddDbContext](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) per effettuare il provisioning di `DbContext` classe nel contenitore DI ASP.NET. Che metodo imposta la durata del servizio `Scoped` per impostazione predefinita. `Scoped`indica la durata dell'oggetto di contesto coincide con la durata richiesta web e `Dispose` metodo verrà chiamato automaticamente alla fine della richiesta web.

## <a name="handling-transactions"></a>Gestione delle transazioni

Per impostazione predefinita di Entity Framework implementa in modo implicito le transazioni. Negli scenari in cui si apportano modifiche a più righe o le tabelle e quindi chiamare `SaveChanges`, Entity Framework di verificare che tutte le modifiche apportate dal tutte esito automaticamente. Se alcune modifiche sono state completate prima e quindi si verifica un errore, tali modifiche vengono automaticamente il rollback. Per scenari in cui è necessario più controllare, ad esempio, se si desidera includere operazioni eseguite all'esterno di Entity Framework in una transazione, vedere [transazioni](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>Query di rilevamento di No

Quando un contesto di database recupera le righe della tabella e crea gli oggetti di entità che li rappresentano, per impostazione predefinita consente di determinare se le entità in memoria sono sincronizzate con i dati del database. I dati in memoria funge da cache e viene utilizzati quando si aggiorna un'entità. La memorizzazione nella cache non è spesso necessario in un'applicazione web perché contesto istanze sono in genere breve durata (un nuovo uno viene creato o eliminato per ogni richiesta) e il contesto che legge un'entità viene in genere eliminata prima di tale entità viene usato di nuovo.

È possibile disabilitare il rilevamento modifiche di oggetti entità nella memoria chiamando la `AsNoTracking` metodo. Scenari tipici in cui si desideri farlo che includono quanto segue:

* Nel corso della durata di contesto non è necessario aggiornare tutte le entità e non è necessario EF per [caricare automaticamente le proprietà di navigazione con entità recuperate dalla query separate](read-related-data.md). Spesso queste condizioni vengono soddisfatte nei metodi di azione di un controller HttpGet.

* Si esegue una query che recupera un volume elevato di dati e verrà aggiornata solo una piccola parte dei dati restituiti. Può essere più efficiente per disattivare il rilevamento per la query di grandi dimensioni ed eseguire una query in un secondo momento per alcune entità che dovranno essere aggiornati.

* Si desidera collegare un'entità per l'aggiornamento, ma in precedenza recuperare la stessa entità per uno scopo diverso. Poiché l'entità è già rilevato dal contesto di database, è possibile collegare l'entità che si desidera modificare. Un modo per gestire questa situazione consiste nel chiamare `AsNoTracking` nella query precedente.

Per ulteriori informazioni, vedere [rilevamento Visual Studio. No-rilevamento](https://docs.microsoft.com/ef/core/querying/tracking).

## <a name="summary"></a>Riepilogo

Ora è un set completo di pagine in cui eseguire semplici operazioni CRUD per le entità di studenti. Nella prossima esercitazione è possibile espandere le funzionalità del **indice** pagina mediante l'aggiunta di ordinamento, filtro e il paging.

>[!div class="step-by-step"]
[Precedente](intro.md)
[Successivo](sort-filter-page.md)  
