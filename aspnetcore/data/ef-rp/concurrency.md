---
title: Razor Pages con EF Core in ASP.NET Core - Concorrenza - 8 di 8
author: rick-anderson
description: Questa esercitazione descrive la gestione dei conflitti quando più utenti aggiornano la stessa entità contemporaneamente.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/concurrency
ms.openlocfilehash: cd06cb1056e1c856214d2440533aad5789907107
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207342"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>Razor Pages con EF Core in ASP.NET Core - Concorrenza - 8 di 8

Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra) e [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Questa esercitazione descrive la gestione dei conflitti quando più utenti aggiornano la stessa entità contemporaneamente. Se si verificano problemi che non si è in grado di risolvere, [scaricare o visualizzare l'app completa](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). [Istruzioni per il download](xref:index#how-to-download-a-sample).

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Un conflitto di concorrenza si verifica quando:

* Un utente passa alla pagina di modifica di un'entità.
* Un altro utente aggiorna la stessa entità prima che la modifica del primo utente venga scritta nel database.

Se non è abilitato il rilevamento della concorrenza, quando si verificano aggiornamenti concorrenti:

* L'ultimo aggiornamento è quello valido. In altri termini, nel database vengono salvati i valori dell'ultimo aggiornamento.
* I dati del primo aggiornamento vengono ignorati.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

La concorrenza ottimistica consente che si verifichino conflitti di concorrenza, quindi attiva le misure necessarie. Ad esempio Jane visita la pagina Department Edit (Modifica - Reparto) e cambia il budget per il reparto English (Inglese) da $ 350.000,00 a $ 0,00.

![Impostazione del budget su 0](concurrency/_static/change-budget.png)

Prima che Jane faccia clic su **Salva** John visita la stessa pagina e cambia il valore del campo Start Date (Data inizio) da 9/1/2007 a 9/1/2013.

![Impostazione della data di inizio su 2013](concurrency/_static/change-date.png)

Jane fa clic su **Salva** per prima e vede la sua modifica quando il browser torna alla pagina di indice.

![Budget impostato su zero](concurrency/_static/budget-zero.png)

John fa clic su **Salva** in una pagina Edit (Modifica) che visualizza ancora un budget pari a $ 350.000,00. Le operazioni successive dipendono da come si decide di gestire i conflitti di concorrenza.

La concorrenza ottimistica include le opzioni seguenti:

* È possibile tenere traccia della proprietà che un utente ha modificato e aggiornare solo le colonne corrispondenti nel database.

  Con questo scenario non si registra la perdita di dati. I due utenti hanno aggiornato proprietà diverse. Quando un utente torna a visualizzare il reparto English (Inglese), visualizza sia le modifiche di Jane sia quelle di John. Questo metodo di aggiornamento riduce il numero di conflitti che possono comportare la perdita di dati. Questo approccio:
 
  * Non evita la perdita di dati se vengono apportate modifiche concorrenti alla stessa proprietà.
  * Risulta in genere poco pratico in un'app Web. Richiede la manutenzione di un volume importante di codice statico per tenere traccia di tutti i valori recuperati e i nuovi valori. La gestione di grandi quantità di codice statico può ridurre le prestazioni dell'applicazione.
  * Può rendere più complesse le app rispetto al rilevamento della concorrenza in un'entità.

* È possibile consentire che la modifica di John sovrascriva la modifica di Jane.

  Quando un utente torna a visualizzare il reparto English (Inglese), visualizza 9/1/2013 e il valore $ 350.000,00 recuperato. Questo scenario è detto *Priorità client* o *Last in Wins* (Priorità ultimo accesso). Tutti i valori del client hanno la precedenza sui valori presenti nell'archivio dati. Se non si configura nessun codice per la gestione della concorrenza, la priorità client viene applicata automaticamente.

* È possibile impedire che la modifica di John venga implementata nel database. In genere, l'app:

  * Visualizza un messaggio di errore.
  * Visualizza lo stato corrente dei dati.
  * Consente all'utente di riapplicare le modifiche.

  Questo scenario è detto *Store Wins* (Priorità archivio). I valori dell'archivio dati hanno la precedenza sui valori inoltrati dal client. In questa esercitazione viene implementato lo scenario Store Wins (Priorità archivio). Questo metodo garantisce che nessuna modifica venga sovrascritta senza che un utente riceva un avviso.

## <a name="handling-concurrency"></a>Gestione della concorrenza 

Quando una proprietà è configurata come [token di concorrenza](/ef/core/modeling/concurrency):

* EF Core verifica che la proprietà non sia stata modificata dopo il suo recupero. La verifica viene eseguita quando si chiama [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) o [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_).
* Se la proprietà è stata modificata dopo il recupero, viene generata un'eccezione [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0). 

Il database e il modello di dati devono essere configurati per supportare la generazione di `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Rilevamento dei conflitti di concorrenza per una proprietà

È possibile rilevare i conflitti di concorrenza a livello delle proprietà con l'attributo [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0). L'attributo può essere applicato a più proprietà del modello. Per altre informazioni, vedere [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations) (Annotazioni dei dati - ConcurrencyCheck).

L'attributo `[ConcurrencyCheck]` non viene usato in questa esercitazione.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Rilevamento dei conflitti di concorrenza per una riga

Per rilevare i conflitti di concorrenza si aggiunge al modello una colonna di rilevamento [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).  `rowversion`:

* È specifica per SQL Server. È possibile che altri database non dispongano di una funzionalità simile.
* Viene usata per determinare che un'entità non è stata modificata dopo il suo recupero dal database. 

Il database genera un numero `rowversion` sequenziale che viene incrementato ogni volta che la riga viene aggiornata. In un comando `Update` o `Delete` la clausola `Where` include il valore recuperato di `rowversion`. Se la riga che viene aggiornata è stata modificata:

 * `rowversion` non corrisponde al valore recuperato.
 * I comandi `Update` o `Delete` non trovano una riga perché la clausola `Where` include il valore `rowversion` recuperato.
 * Viene generata un'eccezione `DbUpdateConcurrencyException`.

In EF Core quando nessuna riga è stata aggiornata da un comando `Update` o `Delete` viene generata un'eccezione di concorrenza.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Aggiungere una proprietà di rilevamento all'entità Department

In *Models/Department.cs* aggiungere una proprietà di rilevamento denominata RowVersion:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

L'attributo [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) specifica che questa colonna è inclusa nella clausola `Where` dei comandi `Update` e `Delete`. L'attributo viene chiamato `Timestamp` perché le versioni precedenti di SQL Server usavano un tipo di dati SQL `timestamp` prima che questo fosse sostituito dal tipo SQL `rowversion`.

L'API Fluent può anche specificare la proprietà di rilevamento:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Il codice seguente visualizza un frammento di T-SQL generato da EF Core quando viene aggiornato il nome di Department (Reparto):

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Il codice evidenziato precedente visualizza la clausola `WHERE` contenente `RowVersion`. Se nel database `RowVersion` non è uguale al parametro `RowVersion` (`@p2`) non viene aggiornata nessuna riga.

Il codice evidenziato seguente visualizza la notazione T-SQL che verifica che è stata aggiornata esattamente una riga:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) restituisce il numero delle righe interessate dall'ultima istruzione. Se non viene aggiornata nessuna riga, EF Core genera `DbUpdateConcurrencyException`.

Il codice T-SQL generato da EF Core è visibile nella finestra di output di Visual Studio.

### <a name="update-the-db"></a>Aggiornare il database

L'aggiunta della proprietà `RowVersion` cambia il modello di database e ciò richiede una migrazione.

Compilare il progetto. Digitare quanto segue in una finestra di comando:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

I comandi precedenti:

* Aggiungono il file di migrazione *Migrations/{timestamp}_RowVersion.cs*.
* Aggiornano il file *Migrations/SchoolContextModelSnapshot.cs*. L'aggiornamento aggiunge al metodo `BuildModel` il codice evidenziato seguente:

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* Eseguono migrations per aggiornare il database.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Scaffolding del modello Departments (Reparti)

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Seguire le istruzioni in [Eseguire lo scaffolding del modello Student (Studente)](xref:data/ef-rp/intro#scaffold-the-student-model) e usare `Department` per la classe modello.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

 Eseguire il comando seguente:

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

------

Il comando precedente esegue lo scaffolding del modello `Department`. Aprire il progetto in Visual Studio.

Compilare il progetto.

### <a name="update-the-departments-index-page"></a>Aggiornare la pagina Departments Index (Indice reparti)

Il motore di scaffolding crea una colonna `RowVersion` per la pagina Index, ma questo campo non deve essere visualizzato. In questa esercitazione, l'ultimo byte di `RowVersion` viene visualizzato per facilitare la comprensione della concorrenza. L'univocità dell'ultimo byte non è garantita. Un'app reale non visualizza `RowVersion` o l'ultimo byte di `RowVersion`.

Aggiornare la pagina Index:

* Sostituire Index con Departments.
* Sostituire il markup che contiene `RowVersion` con l'ultimo byte di `RowVersion`.
* Sostituire FirstMidName con FullName.

Il markup seguente visualizza la pagina aggiornata:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Aggiornare il modello di pagina Edit

Aggiornare *pages\departments\edit.cshtml.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Per rilevare un problema di concorrenza, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) viene aggiornato con il valore `rowVersion` dall'entità dalla quale stato recuperato. EF Core genera un comando SQL UPDATE con una clausola WHERE contenente il valore `RowVersion` originale. Se il comando UPDATE non ha effetto su nessuna riga (nessuna riga ha il valore originale `RowVersion`), viene generata un'eccezione `DbUpdateConcurrencyException`.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

Nel codice precedente, `Department.RowVersion` è il valore al momento del recupero dell'entità. `OriginalValue` è il valore presente nel database quando in questo metodo è stato chiamato `FirstOrDefaultAsync`.

Il codice seguente ottiene i valori del client (i valori inseriti in questo metodo) e i valori del database:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Il codice seguente aggiunge un messaggio di errore personalizzato per ogni colonna che ha valori del database diversi da quelli inseriti in `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Il codice evidenziato seguente imposta il valore `RowVersion` sul nuovo valore recuperato dal database. Quando l'utente fa di nuovo clic su **Salva** vengono rilevati solo gli errori di concorrenza che si verificano dopo l'ultima visualizzazione della pagina Edit (Modifica).

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

L'istruzione `ModelState.Remove` è necessaria perché `ModelState` presenta il valore obsoleto `RowVersion`. Nella pagina Razor il valore `ModelState` di un campo ha la precedenza sui valori di proprietà del modello quando entrambi gli elementi sono presenti.

## <a name="update-the-edit-page"></a>Aggiornare la pagina Edit (Modifica)

Aggiornare *Pages/Departments/Edit.cshtml* con il markup seguente:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Il markup precedente:

* Aggiorna la direttiva `page` da `@page` a `@page "{id:int}"`.
* Aggiunge una versione di riga nascosta. L'aggiunta di `RowVersion` è necessaria per far sì che il postback associ il valore.
* Visualizza l'ultimo byte di `RowVersion` a scopo di debug.
* Sostituisce `ViewData` con l'elemento `InstructorNameSL` fortemente tipizzato.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Eseguire il test dei conflitti di concorrenza con la pagina Edit (Modifica)

Aprire due istanze di browser con la pagina Edit (Modifica) e il reparto English (Inglese):

* Eseguire l'app e selezionare Departments (Reparti).
* Fare clic con il pulsante destro del mouse sul collegamento ipertestuale **Edit** (Modifica) per il reparto English (Inglese) e selezionare **Apri in una nuova scheda**.
* Nella prima scheda fare clic sul collegamento ipertestuale **Edit** (Modifica) per il reparto English (Inglese).

Le due schede del browser visualizzano le stesse informazioni.

Modificare il nome nella prima scheda del browser e fare clic su **Salva**.

![Pagina Department Edit (Modifica - Reparto) 1 dopo la modifica](concurrency/_static/edit-after-change-1.png)

Il browser visualizza la pagina Index con il valore modificato e l'indicatore rowVersion aggiornato. Si noti l'indicatore rowVersion aggiornato, che è visualizzato sul secondo postback nell'altra scheda.

Modificare un altro campo nella seconda scheda del browser.

![Pagina Department Edit (Modifica - Reparto) 2 dopo la modifica](concurrency/_static/edit-after-change-2.png)

Fare clic su **Salva**. Vengono visualizzati messaggi di errore per tutti i campi che non corrispondono ai valori del database:

![Messaggio di errore della pagina Department Edit (Modifica - Reparto)](concurrency/_static/edit-error.png)

Questa finestra del browser non prevedeva la modifica del campo Name (Nome). Copiare e incollare il valore corrente Languages (Lingue) nel campo Name (Nome). Uscire dalla scheda tramite tabulazione. La convalida lato client rimuove il messaggio di errore.

![Messaggio di errore della pagina Department Edit (Modifica - Reparto)](concurrency/_static/cv.png)

Fare di nuovo clic su **Salva**. Il valore immesso nella seconda scheda del browser viene salvato. I valori salvati vengono visualizzati nella pagina Index.

## <a name="update-the-delete-page"></a>Aggiornare la pagina Delete (Elimina)

Aggiornare il modello di pagina Delete (Elimina) con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

La pagina Delete (Elimina) rileva i conflitti di concorrenza quando l'entità è stata modificata dopo il recupero. `Department.RowVersion` è la versione di riga quando l'entità è stata recuperata. Quando EF Core crea il comando SQL DELETE include una clausola WHERE con `RowVersion`. Se il comando SQL DELETE non ha effetto su nessuna riga:

* `RowVersion` nel comando SQL DELETE non corrisponde a `RowVersion` nel database.
* Viene generata un'eccezione DbUpdateConcurrencyException.
* `OnGetAsync` viene chiamata con `concurrencyError`.

### <a name="update-the-delete-page"></a>Aggiornare la pagina Delete (Elimina)

Aggiornare *Pages/Departments/Delete.cshtml* con il codice seguente:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


Il markup precedente apporta le modifiche seguenti:

* Aggiorna la direttiva `page` da `@page` a `@page "{id:int}"`.
* Aggiunge un messaggio di errore.
* Sostituisce FirstMidName con FullName nel campo **Administrator** (Amministratore).
* Modifica `RowVersion` per visualizzare l'ultimo byte.
* Aggiunge una versione di riga nascosta. L'aggiunta di `RowVersion` è necessaria per far sì che il postback associ il valore.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Eseguire il test dei conflitti di concorrenza con la pagina Delete (Elimina)

Creare un reparto di test.

Aprire due istanze del browser con la pagina Delete (Elimina):

* Eseguire l'app e selezionare Departments (Reparti).
* Fare clic con il pulsante destro del mouse sul collegamento ipertestuale **Delete** (Elimina) per il reparto di test e selezionare **Apri in una nuova scheda**.
* Fare clic sul collegamento ipertestuale **Edit**  (Modifica) per il reparto di test.

Le due schede del browser visualizzano le stesse informazioni.

Modificare il budget nella prima scheda del browser e fare clic su **Salva**.

Il browser visualizza la pagina Index con il valore modificato e l'indicatore rowVersion aggiornato. Si noti l'indicatore rowVersion aggiornato, che è visualizzato sul secondo postback nell'altra scheda.

Eliminare il reparto di test dalla seconda scheda. Viene visualizzato un errore di concorrenza con i valori correnti del database. Se si fa clic su **Delete** (Elimina) l'entità viene eliminata, salvo se l'elemento `RowVersion` è stato aggiornato.

Per informazioni su come ereditare un modello di dati, vedere [Ereditarietà](xref:data/ef-mvc/inheritance).

### <a name="additional-resources"></a>Risorse aggiuntive

* [Concurrency Tokens in EF Core](/ef/core/modeling/concurrency) (Token di concorrenza in EF Core)
* [Gestione della concorrenza in EF Core](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [Precedente](xref:data/ef-rp/update-related-data)
