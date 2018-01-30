---
title: Pagine Razor con Entity Framework Core, 8 - concorrenza - 8
author: rick-anderson
description: "In questa esercitazione viene illustrato come gestire i conflitti quando più utenti aggiornano la stessa entità nello stesso momento."
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/concurrency
ms.openlocfilehash: 1c6cdefa1410839606711d7460a8f4d0f1d6c72b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
en-us /

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>Gestione dei conflitti di concorrenza - Core EF con pagine Razor (8 di 8)

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), e [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

In questa esercitazione viene illustrato come gestire i conflitti quando più utenti di aggiornare un'entità contemporaneamente (allo stesso tempo). Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Si verifica un conflitto di concorrenza quando:

* Un utente passa alla pagina di modifica per un'entità.
* Un altro utente aggiorna la stessa entità prima di modifica del primo utente viene scritto il database.

Se non è abilitato il rilevamento di concorrenza, quando si verificano aggiornamenti simultanei:

* Ultimo aggiornamento wins. Vale a dire gli ultimi valori di aggiornamento vengono salvati al database.
* La prima degli aggiornamenti correnti andranno perse.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

Concorrenza ottimistica consente i conflitti di concorrenza consente di eseguire e quindi reagisce nel modo appropriato quando si verificano. Ad esempio, Jane visita la pagina di modifica del reparto e il budget per il reparto inglese 350,000.00 $ diventa $0,00.

![La modifica di budget per 0](concurrency/_static/change-budget.png)

Prima di Jane seleziona **salvare**, John visita la pagina stessa e il campo Data di inizio da 1/9/2007 a 1/9/2013.

![Modifica la data di inizio a 2013](concurrency/_static/change-date.png)

Jane seleziona **salvare** prima e vede cambia quando il browser viene visualizzata la pagina di indice.

![Budget modificato a zero](concurrency/_static/budget-zero.png)

Seleziona John **salvare** in una pagina di modifica che mostra ancora un budget di $350,000.00. Le operazioni successive è determinata dalla modalità di gestione i conflitti di concorrenza.

Concorrenza ottimistica include le opzioni seguenti:

* È possibile tenere traccia di quali proprietà di un utente ha modificato e aggiornare solo le colonne corrispondenti nel database.

 Nello scenario, non dati andrebbero persi. Diverse proprietà sono state aggiornate con i due utenti. La volta successiva che un utente che accede al dipartimento in lingua inglese, visualizzeranno le modifiche di Jane sia di John. Questo metodo di aggiornamento può ridurre il numero di conflitti che potrebbero comportare la perdita di dati. Questo approccio: * non è possibile evitare la perdita di dati se vengono apportate modifiche competizione per la stessa proprietà.
        * È in genere non è pratico in un'app web. È necessario mantenere lo stato del significativo per tenere traccia di recupero di tutti i valori e i nuovi valori. Gestione di grandi quantità di stato può influiscono sulle prestazioni dell'applicazione.
        * È possibile aumentare la complessità delle app rispetto al rilevamento di concorrenza in un'entità.

* È possibile consentire la modifica di John sovrascrivere la modifica di Jane.

 La volta successiva che un utente che accede al reparto in lingua inglese, essi visualizzeranno il 1/9/2013 e il valore di $350,000.00 recuperato. Questo approccio è definito un *prevalenza del Client* o *ultimo in Wins* scenario. (Tutti i valori dal client hanno la precedenza su ciò che si trova nell'archivio dati). Se non si configura questo codice per la gestione della concorrenza, prevalenza del Client viene eseguita automaticamente.

* È possibile impedire la modifica di John vengano aggiornati nel database. In genere, l'app sarebbe: * visualizzare un messaggio di errore.
        * Indica lo stato corrente dei dati.
        * Consente all'utente riapplicare le modifiche.

 Si tratta di un *archivio Wins* scenario. (I valori dell'archivio dati hanno la precedenza sui valori inviati dal client). Implementare lo scenario archivio Wins in questa esercitazione. Questo metodo assicura che modifiche non vengono sovrascritti senza che l'utente viene generato un avviso.

## <a name="handling-concurrency"></a>La gestione della concorrenza 

Quando una proprietà è configurata come un [token di concorrenza](https://docs.microsoft.com/ef/core/modeling/concurrency):

* Core EF verifica che proprietà non è stata modificata dopo che è stata recuperata. Il controllo viene eseguito quando [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) o [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) viene chiamato.
* Se la proprietà è stata modificata dopo che è stato recuperato, un [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) viene generata un'eccezione. 

Il modello di database e dati deve essere configurato per supportare la generazione di `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Il rilevamento dei conflitti di concorrenza su una proprietà

È possibile rilevare i conflitti di concorrenza a livello di proprietà con il [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attributo. L'attributo può essere applicato a più proprietà del modello. Per ulteriori informazioni, vedere [dati annotazioni-ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).

Il `[ConcurrencyCheck]` attributo non viene utilizzato in questa esercitazione.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Il rilevamento dei conflitti di concorrenza su una riga

Per rilevare i conflitti di concorrenza, un [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) rilevamento colonna viene aggiunto al modello.  `rowversion` :

* È SQL Server specifico. Altri database potrebbero non fornire una funzionalità simile.
* Viene utilizzato per determinare che un'entità non è stata modificata perché è stato recuperato dal database. 

Il database genera una sequenza `rowversion` numero che sia stato incrementato ogni volta che la riga viene aggiornato. In un `Update` o `Delete` comando, il `Where` clausola include il valore recuperato di `rowversion`. Se la riga aggiornata è stata modificata:

 * `rowversion`non corrisponde al valore di recupero.
 * Il `Update` o `Delete` comandi non trovano una riga perché la `Where` clausola include il recupero `rowversion`.
 * Oggetto `DbUpdateConcurrencyException` viene generata un'eccezione.

In Entity Framework Core, quando non le righe sono state aggiornate mediante un `Update` o `Delete` comando, viene generata un'eccezione di concorrenza.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Aggiungere una proprietà di rilevamento all'entità reparto

In *Models/Department.cs*, aggiungere una proprietà di rilevamento denominata RowVersion:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

Il [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attributo specifica che questa colonna viene inclusa nel `Where` clausola di `Update` e `Delete` comandi. L'attributo viene chiamato `Timestamp` perché le versioni precedenti di SQL Server utilizzato un database SQL `timestamp` il tipo di dati prima di SQL `rowversion` tipo sostituito.

L'API fluent è inoltre possibile specificare la proprietà di rilevamento:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Il codice seguente viene mostrata una parte di T-SQL generato da componenti di base di Entity Framework quando viene aggiornato il nome di reparto:

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Precedente evidenziata codice mostra il `WHERE` clausola contenente `RowVersion`. Se il database `RowVersion` non sia uguale al `RowVersion` parametro (`@p2`), non vengono aggiornate righe.

Il codice evidenziato di seguito viene illustrato il T-SQL che consente di verificare esattamente una riga è stata aggiornata:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) restituisce il numero di righe interessate dall'ultima istruzione. In nessun righe vengono aggiornate, EF Core genera una `DbUpdateConcurrencyException`.

È possibile visualizzare che il nucleo di EF T-SQL genera l'errore nella finestra di output di Visual Studio.

### <a name="update-the-db"></a>Aggiornare il database

Aggiunta di `RowVersion` Modifica proprietà modello di database, che richiede la migrazione.

Compilare il progetto. Nella finestra di comando, immettere quanto segue:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

I comandi precedenti:

* Aggiunge il *migrazioni / {ora stamp}_RowVersion.cs* file di migrazione.
* Gli aggiornamenti di *Migrations/SchoolContextModelSnapshot.cs* file. L'aggiornamento aggiunge il codice evidenziato di seguito per il `BuildModel` metodo:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* Esegue le migrazioni per aggiornare il database.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Lo scaffolding il modello di reparto

* Uscire da Visual Studio.
* Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
* Eseguire il comando seguente:

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

Il comando precedente delle strutture di `Department` modello. Aprire il progetto in Visual Studio.

Compilare il progetto. La compilazione genera errori simile al seguente:

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Una modifica globale `_context.Department` a `_context.Departments` (ovvero, aggiungere una "s" `Department`). 7 occorrenze sono disponibili e aggiornate.

### <a name="update-the-departments-index-page"></a>Aggiornare la pagina di indice reparti

Il motore di scaffolding creato un `RowVersion` colonna per la pagina di indice, ma tale campo non deve essere visualizzata. In questa esercitazione, l'ultimo byte del `RowVersion` viene visualizzato per comprendere la concorrenza. L'ultimo byte non è garantita l'univocità. Non visualizzare una vera app `RowVersion` o l'ultimo byte della `RowVersion`.

Aggiornare la pagina di indice:

* Sostituire l'indice con reparti.
* Sostituire il markup che contiene `RowVersion` con l'ultimo byte della `RowVersion`.
* Sostituire FirstMidName con nome completo.

Il markup seguente mostra la pagina aggiornata:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Aggiornare il modello di pagina di modifica

Aggiornamento *pages\departments\edit.cshtml.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Per rilevare un problema di concorrenza, il [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) viene aggiornato con il `rowVersion` valore dall'entità è stato recuperato. Core EF genera un comando SQL UPDATE con una clausola WHERE contenente originale `RowVersion` valore. Se nessuna riga è interessata dal comando di aggiornamento (righe di non avere originale `RowVersion` valore), un `DbUpdateConcurrencyException` viene generata un'eccezione.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

Nel codice precedente, `Department.RowVersion` è il valore quando l'entità è stata recuperata. `OriginalValue`è il valore nel database quando `FirstOrDefaultAsync` in questo metodo è stato chiamato.

Il codice seguente ottiene i valori del client (i valori a questo metodo di registrazione) e i valori DB:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Il codice seguente aggiunge un messaggio di errore personalizzati per i valori diversi da cosa è stata registrata per ogni colonna con DB `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Seguenti evidenziato set di codice il `RowVersion` valore per il nuovo valore recuperato dal database. La volta successiva che l'utente fa clic **salvare**, solo gli errori di concorrenza che si verificano dopo l'ultima visualizzazione della pagina di modifica verrà rilevata.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

Il `ModelState.Remove` istruzione è necessaria perché `ModelState` è il vecchio `RowVersion` valore. Nella pagina Razor il `ModelState` valore per un campo ha la precedenza sui valori di proprietà del modello quando sono presenti entrambe.

## <a name="update-the-edit-page"></a>Aggiornare la pagina di modifica

Aggiornamento *Pages/Departments/Edit.cshtml* con il markup seguente:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Il codice precedente:

* Gli aggiornamenti di `page` direttiva da `@page` a `@page "{id:int}"`.
* Aggiunge una versione di riga nascosta. `RowVersion`è necessario aggiungere in modo postback associa il valore.
* Visualizza l'ultimo byte della `RowVersion` a scopo di debug.
* Sostituisce `ViewData` con fortemente tipizzata `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Conflitti di concorrenza di test con la pagina di modifica

Aprire due istanze del browser di modifica su del reparto in lingua inglese:

* Eseguire l'app e selezionare i reparti.
* Fare doppio clic su di **modifica** collegamento ipertestuale per il reparto in lingua inglese e selezionare **aperto in una nuova scheda**.
* Nella prima scheda, fare clic su di **modifica** collegamento ipertestuale per il reparto in lingua inglese.

Le schede del due browser visualizzano le stesse informazioni.

Modificare il nome nella prima scheda del browser e fare clic su **salvare**.

![Reparto Modifica pagina 1 dopo la modifica](concurrency/_static/edit-after-change-1.png)

Il browser viene mostrata la pagina di indice con il valore modificato e aggiornato rowVersion indicatore. Si noti l'indicatore rowVersion aggiornato, viene visualizzato con il secondo postback in altra scheda.

Modificare un campo diverso nella seconda scheda del browser.

![Reparto Modifica pagina 2 dopo la modifica](concurrency/_static/edit-after-change-2.png)

Fare clic su **Salva**. Vengono visualizzati messaggi di errore per tutti i campi che non corrispondono ai valori di database:

![Messaggio di errore di pagina Modifica reparto](concurrency/_static/edit-error.png)

Questa finestra del browser non si desidera modificare il nome di campo. Copiare e incollare il valore corrente (lingue) nel campo nome. Scheda. La convalida lato client rimuove il messaggio di errore.

![Messaggio di errore di pagina Modifica reparto](concurrency/_static/cv.png)

Fare clic su **salvare** nuovamente. Il valore che immesso nella seconda scheda browser viene salvato. Visualizzare i valori salvati nella pagina di indice.

## <a name="update-the-delete-page"></a>Aggiornare la pagina di eliminazione

Aggiornare il modello di pagina di Delete con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

La pagina di eliminazione rileva i conflitti di concorrenza quando l'entità è stato modificato dopo che è stata recuperata. `Department.RowVersion`è la versione di riga quando l'entità è stata recuperata. Quando EF Core viene creato il comando SQL DELETE, include una clausola WHERE con `RowVersion`. Se i risultati del comando SQL DELETE in zero righe interessate:

* Il `RowVersion` in SQL DELETE il comando non corrisponde a `RowVersion` nel database.
* Viene generata un'eccezione di DbUpdateConcurrencyException.
* `OnGetAsync`viene chiamato con il `concurrencyError`.

### <a name="update-the-delete-page"></a>Aggiornare la pagina di eliminazione

Aggiornamento *Pages/Departments/Delete.cshtml* con il codice seguente:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


Il markup precedente apporta le modifiche seguenti:

* Gli aggiornamenti di `page` direttiva da `@page` a `@page "{id:int}"`.
* Aggiunge un messaggio di errore.
* Sostituisce FirstMidName con nome completo nel **amministratore** campo.
* Le modifiche `RowVersion` per visualizzare l'ultimo byte.
* Aggiunge una versione di riga nascosta. `RowVersion`è necessario aggiungere in modo postback associa il valore.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Conflitti di concorrenza di test con la pagina di eliminazione

Creare un test di reparto.

Aprire due istanze del browser di eliminazione sul reparto di test:

* Eseguire l'app e selezionare i reparti.
* Fare doppio clic su di **eliminare** collegamento ipertestuale per il reparto di test e selezionare **aperto in una nuova scheda**.
* Fare clic su di **modifica** collegamento ipertestuale per il reparto di test.

Le schede del due browser visualizzano le stesse informazioni.

Modificare il budget nella prima scheda del browser e fare clic su **salvare**.

Il browser viene mostrata la pagina di indice con il valore modificato e aggiornato rowVersion indicatore. Si noti l'indicatore rowVersion aggiornato, viene visualizzato con il secondo postback in altra scheda.

Eliminare il reparto di test dalla seconda scheda. Un errore di concorrenza viene visualizzato con i valori correnti dal database. Fare clic su **eliminare** Elimina l'entità, a meno che non `RowVersion` è stata updated.department è stato eliminato.

Vedere [ereditarietà](xref:data/ef-mvc/inheritance) su come ereditare un modello di dati.

### <a name="additional-resources"></a>Risorse aggiuntive

* [Token di concorrenza in EF Core](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [La gestione della concorrenza in EF Core](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[Precedente](xref:data/ef-rp/update-related-data)
