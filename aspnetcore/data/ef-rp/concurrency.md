---
title: Razor Pages con EF Core in ASP.NET Core - Concorrenza - 8 di 8
author: rick-anderson
description: Questa esercitazione descrive la gestione dei conflitti quando più utenti aggiornano la stessa entità contemporaneamente.
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/concurrency
ms.openlocfilehash: b6a8354bf438895f5188290013afefd883c4dd0a
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
<span data-ttu-id="6ce05-103">it-it/</span><span class="sxs-lookup"><span data-stu-id="6ce05-103">en-us/</span></span>

# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="6ce05-104">Razor Pages con EF Core in ASP.NET Core - Concorrenza - 8 di 8</span><span class="sxs-lookup"><span data-stu-id="6ce05-104">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="6ce05-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra) e [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="6ce05-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="6ce05-106">Questa esercitazione descrive la gestione dei conflitti quando più utenti aggiornano la stessa entità contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="6ce05-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="6ce05-107">Se si verificano problemi che non è possibile risolvere, scaricare l'[app completa per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="6ce05-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="6ce05-108">Conflitti di concorrenza</span><span class="sxs-lookup"><span data-stu-id="6ce05-108">Concurrency conflicts</span></span>

<span data-ttu-id="6ce05-109">Un conflitto di concorrenza si verifica quando:</span><span class="sxs-lookup"><span data-stu-id="6ce05-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="6ce05-110">Un utente passa alla pagina di modifica di un'entità.</span><span class="sxs-lookup"><span data-stu-id="6ce05-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="6ce05-111">Un altro utente aggiorna la stessa entità prima che la modifica del primo utente venga scritta nel database.</span><span class="sxs-lookup"><span data-stu-id="6ce05-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="6ce05-112">Se non è abilitato il rilevamento della concorrenza, quando si verificano aggiornamenti concorrenti:</span><span class="sxs-lookup"><span data-stu-id="6ce05-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="6ce05-113">L'ultimo aggiornamento è quello valido.</span><span class="sxs-lookup"><span data-stu-id="6ce05-113">The last update wins.</span></span> <span data-ttu-id="6ce05-114">In altri termini, nel database vengono salvati i valori dell'ultimo aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="6ce05-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="6ce05-115">I dati del primo aggiornamento vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="6ce05-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="6ce05-116">Concorrenza ottimistica</span><span class="sxs-lookup"><span data-stu-id="6ce05-116">Optimistic concurrency</span></span>

<span data-ttu-id="6ce05-117">La concorrenza ottimistica consente che si verifichino conflitti di concorrenza, quindi attiva le misure necessarie.</span><span class="sxs-lookup"><span data-stu-id="6ce05-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="6ce05-118">Ad esempio Jane visita la pagina Department Edit (Modifica - Reparto) e cambia il budget per il reparto English (Inglese) da $ 350.000,00 a $ 0,00.</span><span class="sxs-lookup"><span data-stu-id="6ce05-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Impostazione del budget su 0](concurrency/_static/change-budget.png)

<span data-ttu-id="6ce05-120">Prima che Jane faccia clic su **Salva** John visita la stessa pagina e cambia il valore del campo Start Date (Data inizio) da 9/1/2007 a 9/1/2013.</span><span class="sxs-lookup"><span data-stu-id="6ce05-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Impostazione della data di inizio su 2013](concurrency/_static/change-date.png)

<span data-ttu-id="6ce05-122">Jane fa clic su **Salva** per prima e vede la sua modifica quando il browser torna alla pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="6ce05-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Budget impostato su zero](concurrency/_static/budget-zero.png)

<span data-ttu-id="6ce05-124">John fa clic su **Salva** in una pagina Edit (Modifica) che visualizza ancora un budget pari a $ 350.000,00.</span><span class="sxs-lookup"><span data-stu-id="6ce05-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="6ce05-125">Le operazioni successive dipendono da come si decide di gestire i conflitti di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="6ce05-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="6ce05-126">La concorrenza ottimistica include le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6ce05-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="6ce05-127">È possibile tenere traccia della proprietà che un utente ha modificato e aggiornare solo le colonne corrispondenti nel database.</span><span class="sxs-lookup"><span data-stu-id="6ce05-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="6ce05-128">Con questo scenario non si registra la perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="6ce05-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="6ce05-129">I due utenti hanno aggiornato proprietà diverse.</span><span class="sxs-lookup"><span data-stu-id="6ce05-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="6ce05-130">Quando un utente torna a visualizzare il reparto English (Inglese), visualizza sia le modifiche di Jane sia quelle di John.</span><span class="sxs-lookup"><span data-stu-id="6ce05-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="6ce05-131">Questo metodo di aggiornamento riduce il numero di conflitti che possono comportare la perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="6ce05-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="6ce05-132">Questo approccio: \* Non evita la perdita di dati se vengono apportate modifiche concorrenti alla stessa proprietà.</span><span class="sxs-lookup"><span data-stu-id="6ce05-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="6ce05-133">\* Risulta in genere poco pratico in un'app Web.</span><span class="sxs-lookup"><span data-stu-id="6ce05-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="6ce05-134">Richiede la manutenzione di un volume importante di codice statico per tenere traccia di tutti i valori recuperati e i nuovi valori.</span><span class="sxs-lookup"><span data-stu-id="6ce05-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="6ce05-135">La gestione di grandi quantità di codice statico può ridurre le prestazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6ce05-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="6ce05-136">\* Può rendere più complesse le app rispetto al rilevamento della concorrenza in un'entità.</span><span class="sxs-lookup"><span data-stu-id="6ce05-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="6ce05-137">È possibile consentire che la modifica di John sovrascriva la modifica di Jane.</span><span class="sxs-lookup"><span data-stu-id="6ce05-137">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="6ce05-138">Quando un utente torna a visualizzare il reparto English (Inglese), visualizza 9/1/2013 e il valore $ 350.000,00 recuperato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="6ce05-139">Questo scenario è detto *Priorità client* o *Last in Wins* (Priorità ultimo accesso).</span><span class="sxs-lookup"><span data-stu-id="6ce05-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="6ce05-140">Tutti i valori del client hanno la precedenza sui valori presenti nell'archivio dati. Se non si configura nessun codice per la gestione della concorrenza, la priorità client viene applicata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6ce05-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="6ce05-141">È possibile impedire che la modifica di John venga implementata nel database.</span><span class="sxs-lookup"><span data-stu-id="6ce05-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="6ce05-142">In genere l'app: \* Visualizza un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="6ce05-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="6ce05-143">\* Visualizza lo stato corrente dei dati.</span><span class="sxs-lookup"><span data-stu-id="6ce05-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="6ce05-144">\* Consente all'utente di riapplicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="6ce05-144">\* Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="6ce05-145">Questo scenario è detto *Store Wins* (Priorità archivio).</span><span class="sxs-lookup"><span data-stu-id="6ce05-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="6ce05-146">I valori dell'archivio dati hanno la precedenza sui valori inoltrati dal client. In questa esercitazione viene implementato lo scenario Store Wins (Priorità archivio).</span><span class="sxs-lookup"><span data-stu-id="6ce05-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="6ce05-147">Questo metodo garantisce che nessuna modifica venga sovrascritta senza che un utente riceva un avviso.</span><span class="sxs-lookup"><span data-stu-id="6ce05-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="6ce05-148">Gestione della concorrenza</span><span class="sxs-lookup"><span data-stu-id="6ce05-148">Handling concurrency</span></span> 

<span data-ttu-id="6ce05-149">Quando una proprietà è configurata come [token di concorrenza](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="6ce05-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="6ce05-150">EF Core verifica che la proprietà non sia stata modificata dopo il suo recupero.</span><span class="sxs-lookup"><span data-stu-id="6ce05-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="6ce05-151">La verifica viene eseguita quando si chiama [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) o [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="6ce05-151">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="6ce05-152">Se la proprietà è stata modificata dopo il recupero, viene generata un'eccezione [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="6ce05-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="6ce05-153">Il database e il modello di dati devono essere configurati per supportare la generazione di `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="6ce05-154">Rilevamento dei conflitti di concorrenza per una proprietà</span><span class="sxs-lookup"><span data-stu-id="6ce05-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="6ce05-155">È possibile rilevare i conflitti di concorrenza a livello delle proprietà con l'attributo [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="6ce05-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="6ce05-156">L'attributo può essere applicato a più proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="6ce05-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="6ce05-157">Per altre informazioni, vedere [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations) (Annotazioni dei dati - ConcurrencyCheck).</span><span class="sxs-lookup"><span data-stu-id="6ce05-157">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="6ce05-158">L'attributo `[ConcurrencyCheck]` non viene usato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="6ce05-158">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="6ce05-159">Rilevamento dei conflitti di concorrenza per una riga</span><span class="sxs-lookup"><span data-stu-id="6ce05-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="6ce05-160">Per rilevare i conflitti di concorrenza si aggiunge al modello una colonna di rilevamento [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="6ce05-160">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="6ce05-161">`rowversion`:</span><span class="sxs-lookup"><span data-stu-id="6ce05-161">`rowversion` :</span></span>

* <span data-ttu-id="6ce05-162">È specifica per SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6ce05-162">Is SQL Server specific.</span></span> <span data-ttu-id="6ce05-163">È possibile che altri database non dispongano di una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="6ce05-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="6ce05-164">Viene usata per determinare che un'entità non è stata modificata dopo il suo recupero dal database.</span><span class="sxs-lookup"><span data-stu-id="6ce05-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="6ce05-165">Il database genera un numero `rowversion` sequenziale che viene incrementato ogni volta che la riga viene aggiornata.</span><span class="sxs-lookup"><span data-stu-id="6ce05-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="6ce05-166">In un comando `Update` o `Delete` la clausola `Where` include il valore recuperato di `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="6ce05-167">Se la riga che viene aggiornata è stata modificata:</span><span class="sxs-lookup"><span data-stu-id="6ce05-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="6ce05-168">`rowversion` non corrisponde al valore recuperato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="6ce05-169">I comandi `Update` o `Delete` non trovano una riga perché la clausola `Where` include il valore `rowversion` recuperato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="6ce05-170">Viene generata un'eccezione `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="6ce05-171">In EF Core quando nessuna riga è stata aggiornata da un comando `Update` o `Delete` viene generata un'eccezione di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="6ce05-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="6ce05-172">Aggiungere una proprietà di rilevamento all'entità Department</span><span class="sxs-lookup"><span data-stu-id="6ce05-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="6ce05-173">In *Models/Department.cs* aggiungere una proprietà di rilevamento denominata RowVersion:</span><span class="sxs-lookup"><span data-stu-id="6ce05-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="6ce05-174">L'attributo [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) specifica che questa colonna è inclusa nella clausola `Where` dei comandi `Update` e `Delete`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-174">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="6ce05-175">L'attributo viene chiamato `Timestamp` perché le versioni precedenti di SQL Server usavano un tipo di dati SQL `timestamp` prima che questo fosse sostituito dal tipo SQL `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="6ce05-176">L'API Fluent può anche specificare la proprietà di rilevamento:</span><span class="sxs-lookup"><span data-stu-id="6ce05-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="6ce05-177">Il codice seguente visualizza un frammento di T-SQL generato da EF Core quando viene aggiornato il nome di Department (Reparto):</span><span class="sxs-lookup"><span data-stu-id="6ce05-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="6ce05-178">Il codice evidenziato precedente visualizza la clausola `WHERE` contenente `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="6ce05-179">Se nel database `RowVersion` non è uguale al parametro `RowVersion` (`@p2`) non viene aggiornata nessuna riga.</span><span class="sxs-lookup"><span data-stu-id="6ce05-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="6ce05-180">Il codice evidenziato seguente visualizza la notazione T-SQL che verifica che è stata aggiornata esattamente una riga:</span><span class="sxs-lookup"><span data-stu-id="6ce05-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="6ce05-181">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) restituisce il numero delle righe interessate dall'ultima istruzione.</span><span class="sxs-lookup"><span data-stu-id="6ce05-181">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="6ce05-182">Se non viene aggiornata nessuna riga, EF Core genera `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="6ce05-183">Il codice T-SQL generato da EF Core è visibile nella finestra di output di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ce05-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="6ce05-184">Aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="6ce05-184">Update the DB</span></span>

<span data-ttu-id="6ce05-185">L'aggiunta della proprietà `RowVersion` cambia il modello di database e ciò richiede una migrazione.</span><span class="sxs-lookup"><span data-stu-id="6ce05-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="6ce05-186">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="6ce05-186">Build the project.</span></span> <span data-ttu-id="6ce05-187">Digitare quanto segue in una finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="6ce05-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="6ce05-188">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="6ce05-188">The preceding commands:</span></span>

* <span data-ttu-id="6ce05-189">Aggiungono il file di migrazione *Migrations/{timestamp}_RowVersion.cs*.</span><span class="sxs-lookup"><span data-stu-id="6ce05-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="6ce05-190">Aggiornano il file *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="6ce05-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="6ce05-191">L'aggiornamento aggiunge al metodo `BuildModel` il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="6ce05-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="6ce05-192">Eseguono migrations per aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="6ce05-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="6ce05-193">Scaffolding del modello Departments (Reparti)</span><span class="sxs-lookup"><span data-stu-id="6ce05-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="6ce05-194">Uscire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ce05-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="6ce05-195">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="6ce05-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6ce05-196">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="6ce05-196">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

<span data-ttu-id="6ce05-197">Il comando precedente esegue lo scaffolding del modello `Department`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="6ce05-198">Aprire il progetto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ce05-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="6ce05-199">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="6ce05-199">Build the project.</span></span> <span data-ttu-id="6ce05-200">La compilazione genera errori simili al seguente:</span><span class="sxs-lookup"><span data-stu-id="6ce05-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="6ce05-201">Convertire a livello globale `_context.Department` in `_context.Departments` (ovvero aggiungere una "s" a `Department`).</span><span class="sxs-lookup"><span data-stu-id="6ce05-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="6ce05-202">Vengono trovate e aggiornate sette occorrenze.</span><span class="sxs-lookup"><span data-stu-id="6ce05-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="6ce05-203">Aggiornare la pagina Departments Index (Indice reparti)</span><span class="sxs-lookup"><span data-stu-id="6ce05-203">Update the Departments Index page</span></span>

<span data-ttu-id="6ce05-204">Il motore di scaffolding crea una colonna `RowVersion` per la pagina Index, ma questo campo non deve essere visualizzato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="6ce05-205">In questa esercitazione, l'ultimo byte di `RowVersion` viene visualizzato per facilitare la comprensione della concorrenza.</span><span class="sxs-lookup"><span data-stu-id="6ce05-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="6ce05-206">L'univocità dell'ultimo byte non è garantita.</span><span class="sxs-lookup"><span data-stu-id="6ce05-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="6ce05-207">Un'app reale non visualizza `RowVersion` o l'ultimo byte di `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="6ce05-208">Aggiornare la pagina Index:</span><span class="sxs-lookup"><span data-stu-id="6ce05-208">Update the Index page:</span></span>

* <span data-ttu-id="6ce05-209">Sostituire Index con Departments.</span><span class="sxs-lookup"><span data-stu-id="6ce05-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="6ce05-210">Sostituire il markup che contiene `RowVersion` con l'ultimo byte di `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="6ce05-211">Sostituire FirstMidName con FullName.</span><span class="sxs-lookup"><span data-stu-id="6ce05-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="6ce05-212">Il markup seguente visualizza la pagina aggiornata:</span><span class="sxs-lookup"><span data-stu-id="6ce05-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="6ce05-213">Aggiornare il modello di pagina Edit</span><span class="sxs-lookup"><span data-stu-id="6ce05-213">Update the Edit page model</span></span>

<span data-ttu-id="6ce05-214">Aggiornare *pages\departments\edit.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6ce05-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="6ce05-215">Per rilevare un problema di concorrenza, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) viene aggiornato con il valore `rowVersion` dall'entità dalla quale stato recuperato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-215">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="6ce05-216">EF Core genera un comando SQL UPDATE con una clausola WHERE contenente il valore `RowVersion` originale.</span><span class="sxs-lookup"><span data-stu-id="6ce05-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="6ce05-217">Se il comando UPDATE non ha effetto su nessuna riga (nessuna riga ha il valore originale `RowVersion`), viene generata un'eccezione `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="6ce05-218">Nel codice precedente, `Department.RowVersion` è il valore al momento del recupero dell'entità.</span><span class="sxs-lookup"><span data-stu-id="6ce05-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="6ce05-219">`OriginalValue` è il valore presente nel database quando in questo metodo è stato chiamato `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="6ce05-220">Il codice seguente ottiene i valori del client (i valori inseriti in questo metodo) e i valori del database:</span><span class="sxs-lookup"><span data-stu-id="6ce05-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="6ce05-221">Il codice seguente aggiunge un messaggio di errore personalizzato per ogni colonna che ha valori del database diversi da quelli inseriti in `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="6ce05-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="6ce05-222">Il codice evidenziato seguente imposta il valore `RowVersion` sul nuovo valore recuperato dal database.</span><span class="sxs-lookup"><span data-stu-id="6ce05-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="6ce05-223">Quando l'utente fa di nuovo clic su **Salva** vengono rilevati solo gli errori di concorrenza che si verificano dopo l'ultima visualizzazione della pagina Edit (Modifica).</span><span class="sxs-lookup"><span data-stu-id="6ce05-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="6ce05-224">L'istruzione `ModelState.Remove` è necessaria perché `ModelState` presenta il valore obsoleto `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="6ce05-225">Nella pagina Razor il valore `ModelState` di un campo ha la precedenza sui valori di proprietà del modello quando entrambi gli elementi sono presenti.</span><span class="sxs-lookup"><span data-stu-id="6ce05-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="6ce05-226">Aggiornare la pagina Edit (Modifica)</span><span class="sxs-lookup"><span data-stu-id="6ce05-226">Update the Edit page</span></span>

<span data-ttu-id="6ce05-227">Aggiornare *Pages/Departments/Edit.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="6ce05-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="6ce05-228">Il markup precedente:</span><span class="sxs-lookup"><span data-stu-id="6ce05-228">The preceding markup:</span></span>

* <span data-ttu-id="6ce05-229">Aggiorna la direttiva `page` da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="6ce05-230">Aggiunge una versione di riga nascosta.</span><span class="sxs-lookup"><span data-stu-id="6ce05-230">Adds a hidden row version.</span></span> <span data-ttu-id="6ce05-231">L'aggiunta di `RowVersion` è necessaria per far sì che il postback associ il valore.</span><span class="sxs-lookup"><span data-stu-id="6ce05-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="6ce05-232">Visualizza l'ultimo byte di `RowVersion` a scopo di debug.</span><span class="sxs-lookup"><span data-stu-id="6ce05-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="6ce05-233">Sostituisce `ViewData` con l'elemento `InstructorNameSL` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="6ce05-234">Eseguire il test dei conflitti di concorrenza con la pagina Edit (Modifica)</span><span class="sxs-lookup"><span data-stu-id="6ce05-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="6ce05-235">Aprire due istanze di browser con la pagina Edit (Modifica) e il reparto English (Inglese):</span><span class="sxs-lookup"><span data-stu-id="6ce05-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="6ce05-236">Eseguire l'app e selezionare Departments (Reparti).</span><span class="sxs-lookup"><span data-stu-id="6ce05-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="6ce05-237">Fare clic con il pulsante destro del mouse sul collegamento ipertestuale **Edit** (Modifica) per il reparto English (Inglese) e selezionare **Apri in una nuova scheda**.</span><span class="sxs-lookup"><span data-stu-id="6ce05-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="6ce05-238">Nella prima scheda fare clic sul collegamento ipertestuale **Edit** (Modifica) per il reparto English (Inglese).</span><span class="sxs-lookup"><span data-stu-id="6ce05-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="6ce05-239">Le due schede del browser visualizzano le stesse informazioni.</span><span class="sxs-lookup"><span data-stu-id="6ce05-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="6ce05-240">Modificare il nome nella prima scheda del browser e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="6ce05-240">Change the name in the first browser tab and click **Save**.</span></span>

![Pagina Department Edit (Modifica - Reparto) 1 dopo la modifica](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="6ce05-242">Il browser visualizza la pagina Index con il valore modificato e l'indicatore rowVersion aggiornato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="6ce05-243">Si noti l'indicatore rowVersion aggiornato, che è visualizzato sul secondo postback nell'altra scheda.</span><span class="sxs-lookup"><span data-stu-id="6ce05-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="6ce05-244">Modificare un altro campo nella seconda scheda del browser.</span><span class="sxs-lookup"><span data-stu-id="6ce05-244">Change a different field in the second browser tab.</span></span>

![Pagina Department Edit (Modifica - Reparto) 2 dopo la modifica](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="6ce05-246">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="6ce05-246">Click **Save**.</span></span> <span data-ttu-id="6ce05-247">Vengono visualizzati messaggi di errore per tutti i campi che non corrispondono ai valori del database:</span><span class="sxs-lookup"><span data-stu-id="6ce05-247">You see error messages for all fields that don't match the DB values:</span></span>

![Messaggio di errore della pagina Department Edit (Modifica - Reparto)](concurrency/_static/edit-error.png)

<span data-ttu-id="6ce05-249">Questa finestra del browser non prevedeva la modifica del campo Name (Nome).</span><span class="sxs-lookup"><span data-stu-id="6ce05-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="6ce05-250">Copiare e incollare il valore corrente Languages (Lingue) nel campo Name (Nome).</span><span class="sxs-lookup"><span data-stu-id="6ce05-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="6ce05-251">Uscire dalla scheda tramite tabulazione. La convalida lato client rimuove il messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="6ce05-251">Tab out. Client-side validation removes the error message.</span></span>

![Messaggio di errore della pagina Department Edit (Modifica - Reparto)](concurrency/_static/cv.png)

<span data-ttu-id="6ce05-253">Fare di nuovo clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="6ce05-253">Click **Save** again.</span></span> <span data-ttu-id="6ce05-254">Il valore immesso nella seconda scheda del browser viene salvato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="6ce05-255">I valori salvati vengono visualizzati nella pagina Index.</span><span class="sxs-lookup"><span data-stu-id="6ce05-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="6ce05-256">Aggiornare la pagina Delete (Elimina)</span><span class="sxs-lookup"><span data-stu-id="6ce05-256">Update the Delete page</span></span>

<span data-ttu-id="6ce05-257">Aggiornare il modello di pagina Delete (Elimina) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6ce05-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="6ce05-258">La pagina Delete (Elimina) rileva i conflitti di concorrenza quando l'entità è stata modificata dopo il recupero.</span><span class="sxs-lookup"><span data-stu-id="6ce05-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="6ce05-259">`Department.RowVersion` è la versione di riga quando l'entità è stata recuperata.</span><span class="sxs-lookup"><span data-stu-id="6ce05-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="6ce05-260">Quando EF Core crea il comando SQL DELETE include una clausola WHERE con `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="6ce05-261">Se il comando SQL DELETE non ha effetto su nessuna riga:</span><span class="sxs-lookup"><span data-stu-id="6ce05-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="6ce05-262">`RowVersion` nel comando SQL DELETE non corrisponde a `RowVersion` nel database.</span><span class="sxs-lookup"><span data-stu-id="6ce05-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="6ce05-263">Viene generata un'eccezione DbUpdateConcurrencyException.</span><span class="sxs-lookup"><span data-stu-id="6ce05-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="6ce05-264">`OnGetAsync` viene chiamata con `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="6ce05-265">Aggiornare la pagina Delete (Elimina)</span><span class="sxs-lookup"><span data-stu-id="6ce05-265">Update the Delete page</span></span>

<span data-ttu-id="6ce05-266">Aggiornare *Pages/Departments/Delete.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6ce05-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="6ce05-267">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="6ce05-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="6ce05-268">Aggiorna la direttiva `page` da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="6ce05-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="6ce05-269">Aggiunge un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="6ce05-269">Adds an error message.</span></span>
* <span data-ttu-id="6ce05-270">Sostituisce FirstMidName con FullName nel campo **Administrator** (Amministratore).</span><span class="sxs-lookup"><span data-stu-id="6ce05-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="6ce05-271">Modifica `RowVersion` per visualizzare l'ultimo byte.</span><span class="sxs-lookup"><span data-stu-id="6ce05-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="6ce05-272">Aggiunge una versione di riga nascosta.</span><span class="sxs-lookup"><span data-stu-id="6ce05-272">Adds a hidden row version.</span></span> <span data-ttu-id="6ce05-273">L'aggiunta di `RowVersion` è necessaria per far sì che il postback associ il valore.</span><span class="sxs-lookup"><span data-stu-id="6ce05-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="6ce05-274">Eseguire il test dei conflitti di concorrenza con la pagina Delete (Elimina)</span><span class="sxs-lookup"><span data-stu-id="6ce05-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="6ce05-275">Creare un reparto di test.</span><span class="sxs-lookup"><span data-stu-id="6ce05-275">Create a test department.</span></span>

<span data-ttu-id="6ce05-276">Aprire due istanze del browser con la pagina Delete (Elimina):</span><span class="sxs-lookup"><span data-stu-id="6ce05-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="6ce05-277">Eseguire l'app e selezionare Departments (Reparti).</span><span class="sxs-lookup"><span data-stu-id="6ce05-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="6ce05-278">Fare clic con il pulsante destro del mouse sul collegamento ipertestuale **Delete** (Elimina) per il reparto di test e selezionare **Apri in una nuova scheda**.</span><span class="sxs-lookup"><span data-stu-id="6ce05-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="6ce05-279">Fare clic sul collegamento ipertestuale **Edit**  (Modifica) per il reparto di test.</span><span class="sxs-lookup"><span data-stu-id="6ce05-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="6ce05-280">Le due schede del browser visualizzano le stesse informazioni.</span><span class="sxs-lookup"><span data-stu-id="6ce05-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="6ce05-281">Modificare il budget nella prima scheda del browser e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="6ce05-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="6ce05-282">Il browser visualizza la pagina Index con il valore modificato e l'indicatore rowVersion aggiornato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="6ce05-283">Si noti l'indicatore rowVersion aggiornato, che è visualizzato sul secondo postback nell'altra scheda.</span><span class="sxs-lookup"><span data-stu-id="6ce05-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="6ce05-284">Eliminare il reparto di test dalla seconda scheda. Viene visualizzato un errore di concorrenza con i valori correnti del database.</span><span class="sxs-lookup"><span data-stu-id="6ce05-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="6ce05-285">Se si fa clic su **Delete** (Elimina) l'entità viene eliminata, salvo se l'elemento `RowVersion` è stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="6ce05-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="6ce05-286">Per informazioni su come ereditare un modello di dati, vedere [Ereditarietà](xref:data/ef-mvc/inheritance).</span><span class="sxs-lookup"><span data-stu-id="6ce05-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="6ce05-287">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6ce05-287">Additional resources</span></span>

* <span data-ttu-id="6ce05-288">[Concurrency Tokens in EF Core](/ef/core/modeling/concurrency) (Token di concorrenza in EF Core)</span><span class="sxs-lookup"><span data-stu-id="6ce05-288">[Concurrency Tokens in EF Core](/ef/core/modeling/concurrency)</span></span>
* [<span data-ttu-id="6ce05-289">Gestione della concorrenza in EF Core</span><span class="sxs-lookup"><span data-stu-id="6ce05-289">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [<span data-ttu-id="6ce05-290">Precedente</span><span class="sxs-lookup"><span data-stu-id="6ce05-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
