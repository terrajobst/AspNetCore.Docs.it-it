---
title: Pagine Razor con Entity Framework Core - modello di dati - 5 di 8
author: rick-anderson
description: "In questa esercitazione aggiungere altre entità e relazioni e personalizzare il modello di dati specificando le regole di mapping del database, convalida e formattazione."
keywords: Annotazioni dei dati ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c2761f79fa4836d29541526782969bb0fd47778b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a><span data-ttu-id="fb381-104">Creazione di un modello di dati complessi - Core EF esercitazione pagine Razor (5 di 8)</span><span class="sxs-lookup"><span data-stu-id="fb381-104">Creating a complex data model - EF Core with Razor Pages tutorial (5 of 8)</span></span>

<span data-ttu-id="fb381-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fb381-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="fb381-106">Nelle esercitazioni precedenti collaborato con un modello di dati di base che è stato composto da tre entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-106">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="fb381-107">In questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="fb381-107">In this tutorial:</span></span>

* <span data-ttu-id="fb381-108">Vengono aggiunte più entità e relazioni.</span><span class="sxs-lookup"><span data-stu-id="fb381-108">More entities and relationships are added.</span></span>
* <span data-ttu-id="fb381-109">Il modello di dati personalizzato specificando le regole di mapping del database, convalida e formattazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-109">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="fb381-110">Le classi di entità per il modello di dati completo è illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-110">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Diagramma di entità](complex-data-model/_static/diagram.png)

<span data-ttu-id="fb381-112">Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="fb381-112">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="fb381-113">Personalizzare il modello di dati con gli attributi</span><span class="sxs-lookup"><span data-stu-id="fb381-113">Customize the data model with attributes</span></span>

<span data-ttu-id="fb381-114">In questa sezione, il modello di dati è stato personalizzato utilizzando gli attributi.</span><span class="sxs-lookup"><span data-stu-id="fb381-114">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="fb381-115">L'attributo di tipo di dati</span><span class="sxs-lookup"><span data-stu-id="fb381-115">The DataType attribute</span></span>

<span data-ttu-id="fb381-116">Attualmente, le pagine di student Visualizza l'ora della data di registrazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-116">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="fb381-117">I campi Data visualizzano in genere, solo la data e non l'ora.</span><span class="sxs-lookup"><span data-stu-id="fb381-117">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="fb381-118">Aggiornamento *Models/Student.cs* con il seguente codice evidenziato:</span><span class="sxs-lookup"><span data-stu-id="fb381-118">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="fb381-119">Il [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attributo specifica un tipo di dati che è più specifico di tipo intrinseco del database.</span><span class="sxs-lookup"><span data-stu-id="fb381-119">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="fb381-120">In questo caso che solo la data deve essere visualizzata, non la data e ora.</span><span class="sxs-lookup"><span data-stu-id="fb381-120">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="fb381-121">Il [enumerazione DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) fornisce per molti tipi di dati, ad esempio data, ora, numero di telefono, valuta, EmailAddress, e così via. Il `DataType` attributo può anche consentire all'app di fornire automaticamente le funzionalità specifiche del tipo.</span><span class="sxs-lookup"><span data-stu-id="fb381-121">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="fb381-122">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="fb381-122">For example:</span></span>

* <span data-ttu-id="fb381-123">Il `mailto:` collegamento viene creato automaticamente per `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="fb381-123">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="fb381-124">Il selettore data viene fornito per `DataType.Date` nella maggior parte dei browser.</span><span class="sxs-lookup"><span data-stu-id="fb381-124">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="fb381-125">Il `DataType` attributo genera HTML 5 `data-` attributi (si pronuncia dati dash) che utilizzano browser HTML 5.</span><span class="sxs-lookup"><span data-stu-id="fb381-125">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="fb381-126">Il `DataType` attributi non forniscono la convalida.</span><span class="sxs-lookup"><span data-stu-id="fb381-126">The `DataType` attributes do not provide validation.</span></span>

<span data-ttu-id="fb381-127">`DataType.Date` non specifica il formato della data visualizzata.</span><span class="sxs-lookup"><span data-stu-id="fb381-127">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="fb381-128">Per impostazione predefinita, viene visualizzato il campo Data in base ai formati predefiniti in base al server [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="fb381-128">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="fb381-129">L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:</span><span class="sxs-lookup"><span data-stu-id="fb381-129">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="fb381-130">Il `ApplyFormatInEditMode` impostazione specifica che la formattazione deve essere applicata anche all'interfaccia di modifica.</span><span class="sxs-lookup"><span data-stu-id="fb381-130">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="fb381-131">Alcuni campi non devono utilizzare `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="fb381-131">Some fields should not use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="fb381-132">Ad esempio, il simbolo di valuta in genere non deve essere visualizzato in una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="fb381-132">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="fb381-133">Il `DisplayFormat` attributo può essere utilizzato da solo.</span><span class="sxs-lookup"><span data-stu-id="fb381-133">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="fb381-134">È in genere consigliabile utilizzare il `DataType` attributo con il `DisplayFormat` attributo.</span><span class="sxs-lookup"><span data-stu-id="fb381-134">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="fb381-135">Il `DataType` attributo fornisce la semantica dei dati anziché come eseguirne il rendering in una schermata.</span><span class="sxs-lookup"><span data-stu-id="fb381-135">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="fb381-136">Il `DataType` attributo offre i vantaggi seguenti che non sono disponibili in `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="fb381-136">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="fb381-137">Il browser è possibile abilitare le funzionalità di HTML5.</span><span class="sxs-lookup"><span data-stu-id="fb381-137">The browser can enable HTML5 features.</span></span> <span data-ttu-id="fb381-138">Ad esempio, Mostra un controllo di calendario, il simbolo di valuta delle impostazioni locali appropriata, i collegamenti di posta elettronica, la convalida dell'input sul lato client e così via.</span><span class="sxs-lookup"><span data-stu-id="fb381-138">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="fb381-139">Per impostazione predefinita, il browser esegue il rendering di dati utilizzando il formato corretto in base alle impostazioni locali.</span><span class="sxs-lookup"><span data-stu-id="fb381-139">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="fb381-140">Per ulteriori informazioni, vedere il [ \<input > documentazione di supporto di Tag](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="fb381-140">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="fb381-141">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="fb381-141">Run the app.</span></span> <span data-ttu-id="fb381-142">Passare alla pagina di indice di studenti.</span><span class="sxs-lookup"><span data-stu-id="fb381-142">Navigate to the Students Index page.</span></span> <span data-ttu-id="fb381-143">Volte in cui non sono più visualizzati.</span><span class="sxs-lookup"><span data-stu-id="fb381-143">Times are no longer displayed.</span></span> <span data-ttu-id="fb381-144">Ogni vista che utilizza il `Student` modello consente di visualizzare la data senza ora.</span><span class="sxs-lookup"><span data-stu-id="fb381-144">Every view that uses the `Student` model displays the date without time.</span></span>

![Pagina di indice di studenti con date senza tempi di](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="fb381-146">L'attributo StringLength</span><span class="sxs-lookup"><span data-stu-id="fb381-146">The StringLength attribute</span></span>

<span data-ttu-id="fb381-147">Regole di convalida dei dati e i messaggi di errore di convalida possono essere specificati con gli attributi.</span><span class="sxs-lookup"><span data-stu-id="fb381-147">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="fb381-148">Il [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attributo specifica la lunghezza minima e massima dei caratteri consentiti in un campo dati.</span><span class="sxs-lookup"><span data-stu-id="fb381-148">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="fb381-149">Il `StringLength` attributo fornisce anche la convalida lato client e lato server.</span><span class="sxs-lookup"><span data-stu-id="fb381-149">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="fb381-150">Il valore minimo non ha alcun impatto sullo schema di database.</span><span class="sxs-lookup"><span data-stu-id="fb381-150">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="fb381-151">Aggiornamento di `Student` modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-151">Update the `Student` model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="fb381-152">Il codice precedente limita i nomi a non più di 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="fb381-152">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="fb381-153">Il `StringLength` non impedisca a un utente di immettere lo spazio vuoto per un nome di attributo.</span><span class="sxs-lookup"><span data-stu-id="fb381-153">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="fb381-154">Il [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attributo viene utilizzato per applicare le restrizioni per l'input.</span><span class="sxs-lookup"><span data-stu-id="fb381-154">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="fb381-155">Ad esempio, il codice seguente richiede il primo carattere da maiuscolo e i caratteri rimanenti alfabetico:</span><span class="sxs-lookup"><span data-stu-id="fb381-155">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="fb381-156">Eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="fb381-156">Run the app:</span></span>

* <span data-ttu-id="fb381-157">Passare alla pagina di studenti.</span><span class="sxs-lookup"><span data-stu-id="fb381-157">Navigate to the Students page.</span></span>
* <span data-ttu-id="fb381-158">Selezionare **Crea nuovo**, immettere un nome più di 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="fb381-158">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="fb381-159">Selezionare **crea**, la convalida lato client verrà visualizzato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="fb381-159">Select **Create**, client-side validation shows an error message.</span></span>

![Gli studenti indice pagina la visualizzazione di errori di lunghezza stringa](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="fb381-161">In **Esplora oggetti di SQL Server** (sillaba SSOX), aprire Progettazione tabelle Student facendo doppio clic il **Student** tabella.</span><span class="sxs-lookup"><span data-stu-id="fb381-161">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabella di studenti in sillaba SSOX prima migrazioni](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="fb381-163">L'immagine precedente mostra lo schema per il `Student` tabella.</span><span class="sxs-lookup"><span data-stu-id="fb381-163">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="fb381-164">I campi nome hanno tipo `nvarchar(MAX)` migrazioni non è stato eseguito nel database.</span><span class="sxs-lookup"><span data-stu-id="fb381-164">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="fb381-165">Quando le migrazioni vengono eseguite più avanti in questa esercitazione, i campi nome diventano `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="fb381-165">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="fb381-166">L'attributo di colonna</span><span class="sxs-lookup"><span data-stu-id="fb381-166">The Column attribute</span></span>

<span data-ttu-id="fb381-167">Gli attributi è possono controllare le modalità di mapping di classi e proprietà per il database.</span><span class="sxs-lookup"><span data-stu-id="fb381-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="fb381-168">In questa sezione, il `Column` attributo viene utilizzato per il mapping del nome del `FirstMidName` proprietà su "FirstName" nel database.</span><span class="sxs-lookup"><span data-stu-id="fb381-168">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="fb381-169">Quando viene creato il database, i nomi delle proprietà nel modello vengono utilizzati per i nomi di colonna (tranne quando la `Column` attributo viene utilizzato).</span><span class="sxs-lookup"><span data-stu-id="fb381-169">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="fb381-170">Il `Student` modello utilizza `FirstMidName` per il nome del primo campo perché il campo può contenere anche un cognome.</span><span class="sxs-lookup"><span data-stu-id="fb381-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="fb381-171">Aggiornamento di *Student.cs* file con il codice evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="fb381-171">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="fb381-172">Con la modifica precedente, `Student.FirstMidName` nell'app esegue il mapping al `FirstName` colonna il `Student` tabella.</span><span class="sxs-lookup"><span data-stu-id="fb381-172">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="fb381-173">L'aggiunta del `Column` attributo modifica il supporto del modello di `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="fb381-173">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="fb381-174">Il supporto del modello di `SchoolContext` non corrisponde più al database.</span><span class="sxs-lookup"><span data-stu-id="fb381-174">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="fb381-175">Se si esegue l'app prima di applicare le migrazioni, viene generata l'eccezione seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-175">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="fb381-176">Per aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="fb381-176">To update the DB:</span></span>

* <span data-ttu-id="fb381-177">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="fb381-177">Build the project.</span></span>
* <span data-ttu-id="fb381-178">Aprire una finestra di comando nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="fb381-178">Open a command window in the project folder.</span></span> <span data-ttu-id="fb381-179">Immettere i comandi seguenti per creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="fb381-179">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="fb381-180">Il `dotnet ef migrations add ColumnFirstName` comando genera il messaggio di avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-180">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="fb381-181">L'avviso viene generato perché i campi nome sono ora limitate a 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="fb381-181">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="fb381-182">Se un nome nel database di più di 50 caratteri, il 51 fino all'ultimo carattere andrebbero persi.</span><span class="sxs-lookup"><span data-stu-id="fb381-182">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="fb381-183">Testare l'app.</span><span class="sxs-lookup"><span data-stu-id="fb381-183">Test the app.</span></span>

<span data-ttu-id="fb381-184">Aprire la tabella di studenti in sillaba SSOX:</span><span class="sxs-lookup"><span data-stu-id="fb381-184">Open the Student table in SSOX:</span></span>

![Tabella di studenti in sillaba SSOX dopo le migrazioni](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="fb381-186">Prima dell'applicazione di migrazione, le colonne nome sono di tipo [nvarchar (max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="fb381-186">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="fb381-187">Le colonne nome sono ora `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="fb381-187">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="fb381-188">Il nome della colonna è stato modificato da `FirstMidName` a `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="fb381-188">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="fb381-189">Nella sezione seguente, compilazione dell'applicazione in alcune fasi genera errori del compilatore.</span><span class="sxs-lookup"><span data-stu-id="fb381-189">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="fb381-190">Le istruzioni specificano quando deve compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-190">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="fb381-191">Aggiornamento dell'entità per studenti</span><span class="sxs-lookup"><span data-stu-id="fb381-191">Student entity update</span></span>

![Entità Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="fb381-193">Aggiornamento *Models/Student.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-193">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="fb381-194">L'attributo obbligatorio</span><span class="sxs-lookup"><span data-stu-id="fb381-194">The Required attribute</span></span>

<span data-ttu-id="fb381-195">Il `Required` attributo rende i campi obbligatori di nome proprietà.</span><span class="sxs-lookup"><span data-stu-id="fb381-195">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="fb381-196">Il `Required` attributo non è necessaria per i tipi non nullable, ad esempio i tipi di valore (`DateTime`, `int`, `double`, ecc.).</span><span class="sxs-lookup"><span data-stu-id="fb381-196">The `Required` attribute is not needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="fb381-197">Tipi che non possono essere null vengono considerati automaticamente come i campi obbligatori.</span><span class="sxs-lookup"><span data-stu-id="fb381-197">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="fb381-198">Il `Required` attributo potrebbe essere sostituito con un parametro di lunghezza minima nel `StringLength` attributo:</span><span class="sxs-lookup"><span data-stu-id="fb381-198">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="fb381-199">L'attributo di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="fb381-199">The Display attribute</span></span>

<span data-ttu-id="fb381-200">Il `Display` attributo specifica che la didascalia per le caselle di testo deve essere "Nome", "Last Name", "Nome" e "Data di registrazione".</span><span class="sxs-lookup"><span data-stu-id="fb381-200">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="fb381-201">Le didascalie predefinita sono Nessuno spazio dividendo le parole, ad esempio "Cognome".</span><span class="sxs-lookup"><span data-stu-id="fb381-201">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="fb381-202">La proprietà FullName calcolato</span><span class="sxs-lookup"><span data-stu-id="fb381-202">The FullName calculated property</span></span>

<span data-ttu-id="fb381-203">`FullName`è una proprietà calcolata che restituisce un valore che viene creato concatenando due altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="fb381-203">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="fb381-204">`FullName`non è possibile impostare, che contiene solo una funzione di accesso get.</span><span class="sxs-lookup"><span data-stu-id="fb381-204">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="fb381-205">Non `FullName` colonna viene creata nel database.</span><span class="sxs-lookup"><span data-stu-id="fb381-205">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="fb381-206">Creare l'entità Instructor</span><span class="sxs-lookup"><span data-stu-id="fb381-206">Create the Instructor Entity</span></span>

![Entità Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="fb381-208">Creare *Models/Instructor.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-208">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="fb381-209">Si noti che molte proprietà sono la stessa nella `Student` e `Instructor` entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-209">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="fb381-210">Nell'esercitazione di ereditarietà di implementazione più avanti in questa serie, viene effettuato il refactoring di questo codice per eliminare la ridondanza.</span><span class="sxs-lookup"><span data-stu-id="fb381-210">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="fb381-211">Più attributi possono essere su una riga.</span><span class="sxs-lookup"><span data-stu-id="fb381-211">Multiple attributes can be on one line.</span></span> <span data-ttu-id="fb381-212">Il `HireDate` attributi può essere scritto come segue:</span><span class="sxs-lookup"><span data-stu-id="fb381-212">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="fb381-213">Le proprietà di navigazione CourseAssignments e OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="fb381-213">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="fb381-214">Il `CourseAssignments` e `OfficeAssignment` sono proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-214">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="fb381-215">Un docente può indicare un numero qualsiasi di corsi, pertanto `CourseAssignments` è definito come una raccolta.</span><span class="sxs-lookup"><span data-stu-id="fb381-215">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="fb381-216">Se una proprietà di navigazione contiene più entità:</span><span class="sxs-lookup"><span data-stu-id="fb381-216">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="fb381-217">Deve essere un tipo di elenco in cui le voci possono essere aggiunte, eliminate e aggiornate.</span><span class="sxs-lookup"><span data-stu-id="fb381-217">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="fb381-218">Tipi di proprietà di navigazione includono:</span><span class="sxs-lookup"><span data-stu-id="fb381-218">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="fb381-219">Se `ICollection<T>` è specificato, EF Core viene creato un `HashSet<T>` insieme per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="fb381-219">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="fb381-220">Il `CourseAssignment` entità è illustrato nella sezione sulle relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="fb381-220">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="fb381-221">Stato che un docente può avere al massimo un ufficio di regole di business di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="fb381-221">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="fb381-222">Il `OfficeAssignment` proprietà contiene un singolo `OfficeAssignment` entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-222">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="fb381-223">`OfficeAssignment`è null se non è assegnato.</span><span class="sxs-lookup"><span data-stu-id="fb381-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="fb381-224">Creare l'entità OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="fb381-224">Create the OfficeAssignment entity</span></span>

![Entità OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="fb381-226">Creare *Models/OfficeAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="fb381-227">L'attributo chiave</span><span class="sxs-lookup"><span data-stu-id="fb381-227">The Key attribute</span></span>

<span data-ttu-id="fb381-228">Il `[Key]` attributo viene utilizzato per identificare una proprietà come chiave primaria (PK) quando il nome della proprietà non è diverso da classnameID o ID.</span><span class="sxs-lookup"><span data-stu-id="fb381-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="fb381-229">È una relazione uno-a-zero-o-uno tra il `Instructor` e `OfficeAssignment` entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="fb381-230">L'assegnazione dell'ufficio esiste solo in relazione a istruttore a che viene assegnato.</span><span class="sxs-lookup"><span data-stu-id="fb381-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="fb381-231">Il `OfficeAssignment` PK è la chiave esterna (FK) per il `Instructor` entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="fb381-232">Core EF non riconosce automaticamente `InstructorID` come la chiave pubblica di `OfficeAssignment` perché:</span><span class="sxs-lookup"><span data-stu-id="fb381-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="fb381-233">`InstructorID`non rispetta la convenzione di denominazione ID o classnameID.</span><span class="sxs-lookup"><span data-stu-id="fb381-233">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="fb381-234">Pertanto, il `Key` attributo viene utilizzato per identificare `InstructorID` come la chiave pubblica:</span><span class="sxs-lookup"><span data-stu-id="fb381-234">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="fb381-235">Per impostazione predefinita, Core EF considera la chiave non generati dal database perché la colonna è la relazione di identificazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-235">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="fb381-236">La proprietà di navigazione Instructor</span><span class="sxs-lookup"><span data-stu-id="fb381-236">The Instructor navigation property</span></span>

<span data-ttu-id="fb381-237">Il `OfficeAssignment` proprietà di navigazione per il `Instructor` entità è nullable perché:</span><span class="sxs-lookup"><span data-stu-id="fb381-237">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="fb381-238">I tipi di riferimento (ad esempio le classi sono ammette valori null).</span><span class="sxs-lookup"><span data-stu-id="fb381-238">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="fb381-239">Un docente potrebbe non disporre di un'assegnazione di office.</span><span class="sxs-lookup"><span data-stu-id="fb381-239">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="fb381-240">Il `OfficeAssignment` entità dispone di un non-nullable `Instructor` proprietà di navigazione perché:</span><span class="sxs-lookup"><span data-stu-id="fb381-240">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="fb381-241">`InstructorID`sono non nullable.</span><span class="sxs-lookup"><span data-stu-id="fb381-241">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="fb381-242">L'assegnazione dell'ufficio non può esistere senza un docente.</span><span class="sxs-lookup"><span data-stu-id="fb381-242">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="fb381-243">Quando un `Instructor` entità è correlata una `OfficeAssignment` entità, ogni entità dispone di un riferimento a altro nella relativa proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-243">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="fb381-244">Il `[Required]` attributo può essere applicato per la `Instructor` proprietà di navigazione:</span><span class="sxs-lookup"><span data-stu-id="fb381-244">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="fb381-245">Il codice precedente specifica che deve essere un insegnante correlati.</span><span class="sxs-lookup"><span data-stu-id="fb381-245">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="fb381-246">Il codice precedente non è necessario perché il `InstructorID` chiave esterna (che è anche la chiave pubblica) è di tipo non nullable.</span><span class="sxs-lookup"><span data-stu-id="fb381-246">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="fb381-247">Modificare l'entità Course</span><span class="sxs-lookup"><span data-stu-id="fb381-247">Modify the Course Entity</span></span>

![Entità Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="fb381-249">Aggiornamento *Models/Course.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-249">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="fb381-250">Il `Course` entità dispone di una proprietà di chiave esterna (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="fb381-250">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="fb381-251">`DepartmentID`punta all'oggetto correlato `Department` entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-251">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="fb381-252">Il `Course` entità dispone di un `Department` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-252">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="fb381-253">Core EF non richiede una proprietà di chiave esterna per un modello di dati quando il modello dispone di una proprietà di navigazione per un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="fb381-253">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="fb381-254">Componenti di base di Entity Framework crea automaticamente chiavi esterne nel database ovunque siano necessarie.</span><span class="sxs-lookup"><span data-stu-id="fb381-254">EF Core automatically creates FKs in the database wherever they are needed.</span></span> <span data-ttu-id="fb381-255">Crea Core EF [nascondere le proprietà](https://docs.microsoft.com/ef/core/modeling/shadow-properties) per chiavi esterne creati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="fb381-255">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="fb381-256">Con la chiave esterna nel modello di dati possono effettuare aggiornamenti più semplice ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="fb381-256">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="fb381-257">Si consideri ad esempio un modello in cui la proprietà di chiave esterna `DepartmentID` è *non* inclusi.</span><span class="sxs-lookup"><span data-stu-id="fb381-257">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="fb381-258">Quando un'entità course viene recuperata da modificare:</span><span class="sxs-lookup"><span data-stu-id="fb381-258">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="fb381-259">Il `Department` entità è null se non viene caricato in modo non esplicito.</span><span class="sxs-lookup"><span data-stu-id="fb381-259">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="fb381-260">Per aggiornare l'entità course, il `Department` entità deve prima essere recuperato.</span><span class="sxs-lookup"><span data-stu-id="fb381-260">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="fb381-261">Quando la proprietà di chiave esterna `DepartmentID` viene incluso nel modello di dati, non è necessario recuperare il `Department` entità prima dell'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="fb381-261">When the FK property `DepartmentID` is included in the data model, there is no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="fb381-262">L'attributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="fb381-262">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="fb381-263">Il `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attributo specifica che la chiave pubblica viene fornita dall'applicazione anziché generato dal database.</span><span class="sxs-lookup"><span data-stu-id="fb381-263">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="fb381-264">Per impostazione predefinita, Core EF si presuppone che i valori di chiave primaria vengono generati dal database.</span><span class="sxs-lookup"><span data-stu-id="fb381-264">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="fb381-265">DB generato PK valori è in genere l'approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="fb381-265">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="fb381-266">Per `Course` entità, l'utente specifica il PK.</span><span class="sxs-lookup"><span data-stu-id="fb381-266">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="fb381-267">Ad esempio, un numero di corso, ad esempio una serie di 1000 per il reparto di matematica, una serie di 2000 per il reparto in lingua inglese.</span><span class="sxs-lookup"><span data-stu-id="fb381-267">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="fb381-268">Il `DatabaseGenerated` attributo può essere utilizzato anche per generare i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="fb381-268">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="fb381-269">Ad esempio, il database è possibile generare automaticamente un campo della data per registrare la data è stata creata o aggiornata una riga.</span><span class="sxs-lookup"><span data-stu-id="fb381-269">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="fb381-270">Per ulteriori informazioni, vedere [generato proprietà](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="fb381-270">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="fb381-271">Proprietà di chiave e la navigazione esterna</span><span class="sxs-lookup"><span data-stu-id="fb381-271">Foreign key and navigation properties</span></span>

<span data-ttu-id="fb381-272">Le proprietà di chiave esterna (FK) e una proprietà di navigazione nel `Course` entità riflettere le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fb381-272">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="fb381-273">Un corso viene assegnato a un reparto, pertanto non c'è un `DepartmentID` FK e un `Department` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-273">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="fb381-274">Un corso può avere qualsiasi numero di studenti, pertanto la `Enrollments` proprietà di navigazione è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="fb381-274">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="fb381-275">Un corso può dedicare da istruttori più, pertanto la `CourseAssignments` proprietà di navigazione è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="fb381-275">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="fb381-276">`CourseAssignment`viene spiegato [in un secondo momento](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="fb381-276">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="fb381-277">Creare l'entità di reparto</span><span class="sxs-lookup"><span data-stu-id="fb381-277">Create the Department entity</span></span>

![Entità reparto](complex-data-model/_static/department-entity.png)

<span data-ttu-id="fb381-279">Creare *Models/Department.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-279">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="fb381-280">L'attributo di colonna</span><span class="sxs-lookup"><span data-stu-id="fb381-280">The Column attribute</span></span>

<span data-ttu-id="fb381-281">In precedenza il `Column` attributo è stato usato per modificare il mapping di nomi di colonna.</span><span class="sxs-lookup"><span data-stu-id="fb381-281">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="fb381-282">Nel codice per il `Department` entità, il `Column` attributo viene utilizzato per modificare i mapping dei tipi di dati SQL.</span><span class="sxs-lookup"><span data-stu-id="fb381-282">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="fb381-283">Il `Budget` colonna viene definita utilizzando il tipo money di SQL Server nel database:</span><span class="sxs-lookup"><span data-stu-id="fb381-283">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="fb381-284">Mapping della colonna non è in genere necessario.</span><span class="sxs-lookup"><span data-stu-id="fb381-284">Column mapping is generally not required.</span></span> <span data-ttu-id="fb381-285">Core EF sceglie in genere il tipo di dati di SQL Server appropriato in base al tipo CLR per la proprietà.</span><span class="sxs-lookup"><span data-stu-id="fb381-285">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="fb381-286">CLR `decimal` mapping a un Server SQL del tipo `decimal` tipo.</span><span class="sxs-lookup"><span data-stu-id="fb381-286">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="fb381-287">`Budget`è per la valuta, e il tipo di dati money è più adatto per la valuta.</span><span class="sxs-lookup"><span data-stu-id="fb381-287">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="fb381-288">Proprietà di chiave e la navigazione esterna</span><span class="sxs-lookup"><span data-stu-id="fb381-288">Foreign key and navigation properties</span></span>

<span data-ttu-id="fb381-289">Le proprietà di chiave esterna e navigazione rifletteranno le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fb381-289">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="fb381-290">Un reparto può o non dispone di un amministratore.</span><span class="sxs-lookup"><span data-stu-id="fb381-290">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="fb381-291">Un amministratore è sempre un docente.</span><span class="sxs-lookup"><span data-stu-id="fb381-291">An administrator is always an instructor.</span></span> <span data-ttu-id="fb381-292">Pertanto il `InstructorID` proprietà è inclusa come la chiave esterna per il `Instructor` entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-292">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="fb381-293">La proprietà di navigazione è denominata `Administrator` ma contiene un `Instructor` entità:</span><span class="sxs-lookup"><span data-stu-id="fb381-293">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="fb381-294">Il punto interrogativo (?) nel codice precedente specifica che la proprietà è nullable.</span><span class="sxs-lookup"><span data-stu-id="fb381-294">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="fb381-295">Un reparto può avere molti corsi, pertanto non c'è una proprietà di navigazione corsi:</span><span class="sxs-lookup"><span data-stu-id="fb381-295">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="fb381-296">Nota: Per convenzione, Core EF Abilita eliminazione a catena per chiavi esterne non nullable e per le relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="fb381-296">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="fb381-297">Eliminazione a catena può comportare regole delete cascade circolare.</span><span class="sxs-lookup"><span data-stu-id="fb381-297">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="fb381-298">Circolare eliminazione a catena cause regole un'eccezione quando viene aggiunta una migrazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-298">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="fb381-299">Ad esempio, se il `Department.InstructorID` proprietà non è stata definita come nullable:</span><span class="sxs-lookup"><span data-stu-id="fb381-299">For example, if the `Department.InstructorID` property was not defined as nullable:</span></span>

* <span data-ttu-id="fb381-300">Componenti di base di Entity Framework consente di configurare una regola di delete cascade per eliminare istruttore quando viene eliminato il reparto.</span><span class="sxs-lookup"><span data-stu-id="fb381-300">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="fb381-301">L'eliminazione istruttore quando viene eliminato il reparto non è il comportamento previsto.</span><span class="sxs-lookup"><span data-stu-id="fb381-301">Deleting the instructor when the department is deleted is not the intended behavior.</span></span>

<span data-ttu-id="fb381-302">Se le regole di business è necessario il `InstructorID` proprietà essere non nullable, utilizzare la seguente istruzione di Microsoft Office fluent API:</span><span class="sxs-lookup"><span data-stu-id="fb381-302">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="fb381-303">Il codice precedente Disabilita eliminazione a catena della relazione insegnante di reparto.</span><span class="sxs-lookup"><span data-stu-id="fb381-303">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="fb381-304">Aggiornare l'entità di registrazione</span><span class="sxs-lookup"><span data-stu-id="fb381-304">Update the Enrollment entity</span></span>

<span data-ttu-id="fb381-305">È un record di registrazione per un una corso impiegato da uno studente.</span><span class="sxs-lookup"><span data-stu-id="fb381-305">An enrollment record is for a one course taken by one student.</span></span>

![Entità di registrazione](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="fb381-307">Aggiornamento *Models/Enrollment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-307">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="fb381-308">Proprietà di chiave e la navigazione esterna</span><span class="sxs-lookup"><span data-stu-id="fb381-308">Foreign key and navigation properties</span></span>

<span data-ttu-id="fb381-309">La proprietà di chiave esterna e le proprietà di navigazione rifletteranno le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fb381-309">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="fb381-310">È un record di registrazione per un corso, pertanto non c'è un `CourseID` proprietà di chiave esterna e un `Course` proprietà di navigazione:</span><span class="sxs-lookup"><span data-stu-id="fb381-310">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="fb381-311">È un record di registrazione per uno studente, pertanto non c'è un `StudentID` proprietà di chiave esterna e un `Student` proprietà di navigazione:</span><span class="sxs-lookup"><span data-stu-id="fb381-311">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="fb381-312">Relazioni molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="fb381-312">Many-to-Many Relationships</span></span>

<span data-ttu-id="fb381-313">È una relazione molti-a-molti tra la `Student` e `Course` entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-313">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="fb381-314">Il `Enrollment` entità funziona come una tabella di join molti-a-molti *con payload* nel database.</span><span class="sxs-lookup"><span data-stu-id="fb381-314">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="fb381-315">"Con payload" significa che il `Enrollment` tabella contiene dati aggiuntivi oltre a chiavi esterne per le tabelle unite in join (in questo caso, la chiave pubblica e `Grade`).</span><span class="sxs-lookup"><span data-stu-id="fb381-315">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="fb381-316">Nella figura seguente viene illustrato l'aspetto queste relazioni in un diagramma di entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-316">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="fb381-317">(Questo diagramma è stato generato utilizzando EF Power Tools di Entity Framework 6. x.</span><span class="sxs-lookup"><span data-stu-id="fb381-317">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="fb381-318">Creazione del diagramma non fa parte dell'esercitazione.)</span><span class="sxs-lookup"><span data-stu-id="fb381-318">Creating the diagram isn't part of the tutorial.)</span></span>

![Corso di studenti molti-a-molti relazione](complex-data-model/_static/student-course.png)

<span data-ttu-id="fb381-320">Ogni linea della relazione dispone di un 1 al fine di uno e un asterisco (*) a altra, per indicare una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="fb381-320">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="fb381-321">Se il `Enrollment` tabella non include informazioni di livello, sufficiente contenere le due chiavi esterne (`CourseID` e `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="fb381-321">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="fb381-322">Una tabella di join molti-a-molti senza payload viene talvolta definita una tabella di join pura (PJT).</span><span class="sxs-lookup"><span data-stu-id="fb381-322">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="fb381-323">Il `Instructor` e `Course` le entità dispongono di una relazione molti-a-molti con una tabella di join pure.</span><span class="sxs-lookup"><span data-stu-id="fb381-323">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="fb381-324">Nota: Tabelle di join implicito per le relazioni molti-a-molti, ma Core EF non supporta 6. x EF.</span><span class="sxs-lookup"><span data-stu-id="fb381-324">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="fb381-325">Per ulteriori informazioni, vedere [molti-a-molti relazioni in Entity Framework Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="fb381-325">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="fb381-326">L'entità CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="fb381-326">The CourseAssignment entity</span></span>

![Entità CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="fb381-328">Creare *Models/CourseAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-328">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="fb381-329">Per corsi di Instructor</span><span class="sxs-lookup"><span data-stu-id="fb381-329">Instructor-to-Courses</span></span>

![Per corsi di istruttore m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="fb381-331">La relazione molti-a-molti per istruttore-corsi:</span><span class="sxs-lookup"><span data-stu-id="fb381-331">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="fb381-332">È necessaria una tabella di join che deve essere rappresentata da un set di entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-332">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="fb381-333">È una tabella di join pure (tabella senza payload).</span><span class="sxs-lookup"><span data-stu-id="fb381-333">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="fb381-334">È comune per assegnare un nome di un'entità di join `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="fb381-334">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="fb381-335">Ad esempio, la tabella di join istruttore-a-corsi usando questo modello è `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="fb381-335">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="fb381-336">Tuttavia, è consigliabile usare un nome che descrive la relazione.</span><span class="sxs-lookup"><span data-stu-id="fb381-336">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="fb381-337">I modelli di dati inizia semplice e aumento delle dimensioni.</span><span class="sxs-lookup"><span data-stu-id="fb381-337">Data models start out simple and grow.</span></span> <span data-ttu-id="fb381-338">Join non-payload (PJTs) spesso sviluppare per includere i payload.</span><span class="sxs-lookup"><span data-stu-id="fb381-338">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="fb381-339">A partire dal nome di un'entità descrittivo, il nome non deve modificare la tabella di join cambia.</span><span class="sxs-lookup"><span data-stu-id="fb381-339">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="fb381-340">Idealmente, l'entità di join avrebbe il nome del relativo naturale (possibilmente singola parola) nel dominio aziendale.</span><span class="sxs-lookup"><span data-stu-id="fb381-340">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="fb381-341">Documentazione e i clienti, ad esempio, potrebbero essere collegati con un'entità di join denominata classificazioni.</span><span class="sxs-lookup"><span data-stu-id="fb381-341">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="fb381-342">Per la relazione molti-a-molti per istruttore-corsi, `CourseAssignment` è preferibile `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="fb381-342">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="fb381-343">Chiave composta</span><span class="sxs-lookup"><span data-stu-id="fb381-343">Composite key</span></span>

<span data-ttu-id="fb381-344">Chiavi esterne non sono nullable.</span><span class="sxs-lookup"><span data-stu-id="fb381-344">FKs are not nullable.</span></span> <span data-ttu-id="fb381-345">Le due chiavi esterne in `CourseAssignment` (`InstructorID` e `CourseID`) insieme identificare in modo univoco ogni riga del `CourseAssignment` tabella.</span><span class="sxs-lookup"><span data-stu-id="fb381-345">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="fb381-346">`CourseAssignment`non richiede un PK dedicato.</span><span class="sxs-lookup"><span data-stu-id="fb381-346">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="fb381-347">Il `InstructorID` e `CourseID` proprietà funzionano come una chiave composta</span><span class="sxs-lookup"><span data-stu-id="fb381-347">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="fb381-348">È l'unico modo per specificare composito usata a EF Core con il *API fluent*.</span><span class="sxs-lookup"><span data-stu-id="fb381-348">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="fb381-349">La sezione successiva illustra come configurare il PK composito.</span><span class="sxs-lookup"><span data-stu-id="fb381-349">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="fb381-350">Chiave composta garantisce:</span><span class="sxs-lookup"><span data-stu-id="fb381-350">The composite key ensures:</span></span>

* <span data-ttu-id="fb381-351">Per un corso sono consentite più righe.</span><span class="sxs-lookup"><span data-stu-id="fb381-351">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="fb381-352">Per un insegnante sono consentite più righe.</span><span class="sxs-lookup"><span data-stu-id="fb381-352">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="fb381-353">Più righe per lo stesso instructor e corso non è consentita.</span><span class="sxs-lookup"><span data-stu-id="fb381-353">Multiple rows for the same instructor and course is not allowed.</span></span>

<span data-ttu-id="fb381-354">Il `Enrollment` entità join definisce il proprio PK, i duplicati di questo tipo sono possibili.</span><span class="sxs-lookup"><span data-stu-id="fb381-354">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="fb381-355">Per evitare tali duplicati:</span><span class="sxs-lookup"><span data-stu-id="fb381-355">To prevent such duplicates:</span></span>

* <span data-ttu-id="fb381-356">Aggiungere un indice univoco per i campi di chiave esterna, o</span><span class="sxs-lookup"><span data-stu-id="fb381-356">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="fb381-357">Configurare `Enrollment` con una chiave primaria composta simile a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="fb381-357">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="fb381-358">Per ulteriori informazioni, vedere [indici](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="fb381-358">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="fb381-359">Aggiornare il contesto DB</span><span class="sxs-lookup"><span data-stu-id="fb381-359">Update the DB context</span></span>

<span data-ttu-id="fb381-360">Aggiungere il codice evidenziato di seguito per *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="fb381-360">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="fb381-361">Il codice precedente aggiunge le nuove entità e configura il `CourseAssignment` PK. composito dell'entità</span><span class="sxs-lookup"><span data-stu-id="fb381-361">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="fb381-362">In alternativa Fluent API per gli attributi</span><span class="sxs-lookup"><span data-stu-id="fb381-362">Fluent API alternative to attributes</span></span>

<span data-ttu-id="fb381-363">Il `OnModelCreating` metodo nel passaggio precedente di codice viene utilizzato il *fluent API* per configurare il comportamento principale di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fb381-363">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="fb381-364">L'API viene denominata "Microsoft Office fluent" perché viene usata spesso mettendo una serie di chiamate al metodo in una singola istruzione.</span><span class="sxs-lookup"><span data-stu-id="fb381-364">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="fb381-365">Il [seguente codice](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) è riportato un esempio di Microsoft Office fluent API:</span><span class="sxs-lookup"><span data-stu-id="fb381-365">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="fb381-366">In questa esercitazione, l'API fluent viene utilizzato solo per il mapping di database che non può essere eseguito con gli attributi.</span><span class="sxs-lookup"><span data-stu-id="fb381-366">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="fb381-367">Tuttavia, l'API fluent è possibile specificare la maggior parte delle formattazioni, convalida e regole di mapping che possono essere eseguite con gli attributi.</span><span class="sxs-lookup"><span data-stu-id="fb381-367">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="fb381-368">Alcuni attributi, ad esempio `MinimumLength` non può essere applicata con l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="fb381-368">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="fb381-369">`MinimumLength`non modificare lo schema, si applica solo una regola di convalida della lunghezza minima.</span><span class="sxs-lookup"><span data-stu-id="fb381-369">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="fb381-370">Alcuni sviluppatori preferiscono utilizzare l'API fluent esclusivamente in modo che mantengano le classi di entità "pulito".</span><span class="sxs-lookup"><span data-stu-id="fb381-370">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="fb381-371">È possibile combinare gli attributi e l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="fb381-371">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="fb381-372">Esistono alcune configurazioni che possono essere eseguite solo con l'API fluent (specificando una chiave primaria composta).</span><span class="sxs-lookup"><span data-stu-id="fb381-372">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="fb381-373">Esistono alcune configurazioni che possono essere eseguite solo con gli attributi (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="fb381-373">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="fb381-374">La procedura consigliata per l'utilizzo di Microsoft Office fluent API o attributi:</span><span class="sxs-lookup"><span data-stu-id="fb381-374">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="fb381-375">Scegliere uno di questi due approcci.</span><span class="sxs-lookup"><span data-stu-id="fb381-375">Choose one of these two approaches.</span></span>
* <span data-ttu-id="fb381-376">Utilizzare l'approccio scelto in modo coerente il più possibile.</span><span class="sxs-lookup"><span data-stu-id="fb381-376">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="fb381-377">Alcuni degli attributi utilizzati in questa esercitazione vengono utilizzati per:</span><span class="sxs-lookup"><span data-stu-id="fb381-377">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="fb381-378">Convalida solo (ad esempio, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="fb381-378">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="fb381-379">Configurazione di base EF solo (ad esempio, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="fb381-379">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="fb381-380">Configurazione di convalida e di Core di Entity Framework (ad esempio, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="fb381-380">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="fb381-381">Per ulteriori informazioni sugli attributi e Microsoft Office fluent API, vedere [metodi di configurazione](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="fb381-381">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="fb381-382">Entità diagramma che illustra le relazioni</span><span class="sxs-lookup"><span data-stu-id="fb381-382">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="fb381-383">Nella figura seguente viene illustrato il diagramma creati Power Tools di Entity Framework per il modello School completato.</span><span class="sxs-lookup"><span data-stu-id="fb381-383">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagramma di entità](complex-data-model/_static/diagram.png)

<span data-ttu-id="fb381-385">Il diagramma precedente mostra:</span><span class="sxs-lookup"><span data-stu-id="fb381-385">The preceding diagram shows:</span></span>

* <span data-ttu-id="fb381-386">Più righe di relazione uno-a-molti (da 1 a \*).</span><span class="sxs-lookup"><span data-stu-id="fb381-386">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="fb381-387">La linea della relazione uno-a-zero-o-uno (da 1 a 0..1) tra il `Instructor` e `OfficeAssignment` entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-387">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="fb381-388">La riga zero-o-relazione uno-a-molti (0..1 a *) tra il `Instructor` e `Department` entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-388">The zero-or-one-to-many relationship line (0..1 to *) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="fb381-389">Valore di inizializzazione del database con dati di Test</span><span class="sxs-lookup"><span data-stu-id="fb381-389">Seed the DB with Test Data</span></span>

<span data-ttu-id="fb381-390">Aggiornare il codice in *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="fb381-390">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="fb381-391">Il codice precedente fornisce i dati di inizializzazione per le nuove entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-391">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="fb381-392">La maggior parte di questo codice crea nuovi oggetti entità e carica i dati di esempio.</span><span class="sxs-lookup"><span data-stu-id="fb381-392">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="fb381-393">I dati di esempio viene utilizzati per il test.</span><span class="sxs-lookup"><span data-stu-id="fb381-393">The sample data is used for testing.</span></span> <span data-ttu-id="fb381-394">Il codice precedente crea le relazioni molti-a-molti seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-394">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="fb381-395">Nota: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) supporterà [seeding dei dati](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="fb381-395">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="fb381-396">Aggiungere una migrazione</span><span class="sxs-lookup"><span data-stu-id="fb381-396">Add a migration</span></span>

<span data-ttu-id="fb381-397">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="fb381-397">Build the project.</span></span> <span data-ttu-id="fb381-398">Aprire una finestra di comando nella cartella del progetto e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-398">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="fb381-399">Il comando precedente viene visualizzato un avviso sulla possibile perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="fb381-399">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="fb381-400">Se il `database update` comando viene eseguito, viene generato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-400">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="fb381-401">Quando le migrazioni vengono eseguite con i dati esistenti, potrebbero essere presenti vincoli di chiave esterna che non vengono soddisfatte con i dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="fb381-401">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="fb381-402">Per questa esercitazione, viene creato un nuovo database, pertanto non vi sono violazioni dei vincoli di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="fb381-402">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="fb381-403">Vedere [correzione vincoli di chiave esterna con dati legacy](#fk) per istruzioni su come correggere le violazioni di chiave esterna nel database corrente.</span><span class="sxs-lookup"><span data-stu-id="fb381-403">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="fb381-404">Modificare la stringa di connessione e aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="fb381-404">Change the connection string and update the DB</span></span>

<span data-ttu-id="fb381-405">Il codice aggiornato `DbInitializer` aggiunge dati di inizializzazione per le nuove entità.</span><span class="sxs-lookup"><span data-stu-id="fb381-405">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="fb381-406">Per forzare il Core di Entity Framework per creare un nuovo database vuoto:</span><span class="sxs-lookup"><span data-stu-id="fb381-406">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="fb381-407">Modificare il nome di stringa di connessione di database in *appSettings. JSON* a ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="fb381-407">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="fb381-408">Il nuovo nome deve essere un nome che non è stato utilizzato nel computer.</span><span class="sxs-lookup"><span data-stu-id="fb381-408">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="fb381-409">In alternativa, eliminare il database utilizzando:</span><span class="sxs-lookup"><span data-stu-id="fb381-409">Alternatively, delete the DB using:</span></span>

    * <span data-ttu-id="fb381-410">**Esplora oggetti di SQL Server** (sillaba SSOX).</span><span class="sxs-lookup"><span data-stu-id="fb381-410">**SQL Server Object Explorer** (SSOX).</span></span>
    * <span data-ttu-id="fb381-411">Il `database drop` comando CLI:</span><span class="sxs-lookup"><span data-stu-id="fb381-411">The `database drop` CLI command:</span></span>

   ```console
   dotnet ef database drop
   ```

<span data-ttu-id="fb381-412">Eseguire `database update` nella finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="fb381-412">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="fb381-413">Il comando precedente viene eseguito tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="fb381-413">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="fb381-414">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="fb381-414">Run the app.</span></span> <span data-ttu-id="fb381-415">Esegue l'app viene eseguita la `DbInitializer.Initialize` metodo.</span><span class="sxs-lookup"><span data-stu-id="fb381-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="fb381-416">Il `DbInitializer.Initialize` consente di popolare il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="fb381-416">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="fb381-417">Aprire il database in sillaba SSOX:</span><span class="sxs-lookup"><span data-stu-id="fb381-417">Open the DB in SSOX:</span></span>

* <span data-ttu-id="fb381-418">Espandere il **tabelle** nodo.</span><span class="sxs-lookup"><span data-stu-id="fb381-418">Expand the **Tables** node.</span></span> <span data-ttu-id="fb381-419">Vengono visualizzate le tabelle create.</span><span class="sxs-lookup"><span data-stu-id="fb381-419">The created tables are displayed.</span></span>
* <span data-ttu-id="fb381-420">Se sillaba SSOX è stato aperto in precedenza, fare clic su di **aggiornamento** pulsante.</span><span class="sxs-lookup"><span data-stu-id="fb381-420">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Tabelle in sillaba SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="fb381-422">Esaminare il **CourseAssignment** tabella:</span><span class="sxs-lookup"><span data-stu-id="fb381-422">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="fb381-423">Fare doppio clic su di **CourseAssignment** tabella e selezionare **Visualizza dati**.</span><span class="sxs-lookup"><span data-stu-id="fb381-423">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="fb381-424">Verificare il **CourseAssignment** tabella contiene dati.</span><span class="sxs-lookup"><span data-stu-id="fb381-424">Verify the **CourseAssignment** table contains data.</span></span>

![Dati CourseAssignment sillaba SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="fb381-426">Correzione di vincoli di chiave esterna con dati legacy</span><span class="sxs-lookup"><span data-stu-id="fb381-426">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="fb381-427">In questa sezione è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="fb381-427">This section is optional.</span></span>

<span data-ttu-id="fb381-428">Quando le migrazioni vengono eseguite con i dati esistenti, potrebbero essere presenti vincoli di chiave esterna che non vengono soddisfatte con i dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="fb381-428">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="fb381-429">Con dati di produzione, è necessario eseguire passaggi per la migrazione dei dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="fb381-429">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="fb381-430">In questa sezione viene fornito un esempio di correzione di violazioni dei vincoli di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="fb381-430">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="fb381-431">Non apportare queste modifiche al codice senza un backup.</span><span class="sxs-lookup"><span data-stu-id="fb381-431">Don't make these code changes without a backup.</span></span> <span data-ttu-id="fb381-432">Non apportare queste modifiche di codice se è completata la sezione precedente e il database aggiornato.</span><span class="sxs-lookup"><span data-stu-id="fb381-432">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="fb381-433">Il *{timestamp}_ComplexDataModel.cs* file contiene il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fb381-433">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="fb381-434">Il codice precedente aggiunge un non-nullable `DepartmentID` chiave esterna per il `Course` tabella.</span><span class="sxs-lookup"><span data-stu-id="fb381-434">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="fb381-435">Il database dall'esercitazione precedente contiene righe di `Course`, in modo che la tabella non può essere aggiornata le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="fb381-435">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="fb381-436">Per rendere il `ComplexDataModel` lavoro di migrazione con i dati esistenti:</span><span class="sxs-lookup"><span data-stu-id="fb381-436">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="fb381-437">Modificare il codice per fornire la nuova colonna (`DepartmentID`) un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="fb381-437">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="fb381-438">Creare un reparto FALSO denominato "Temp" come il reparto predefinito.</span><span class="sxs-lookup"><span data-stu-id="fb381-438">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="fb381-439">Correggere i vincoli di chiave esterni</span><span class="sxs-lookup"><span data-stu-id="fb381-439">Fix the foreign key constraints</span></span>

<span data-ttu-id="fb381-440">Aggiornamento di `ComplexDataModel` classi `Up` metodo:</span><span class="sxs-lookup"><span data-stu-id="fb381-440">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="fb381-441">Aprire il *{timestamp}_ComplexDataModel.cs* file.</span><span class="sxs-lookup"><span data-stu-id="fb381-441">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="fb381-442">Impostare come commento la riga di codice che aggiunge il `DepartmentID` colonna per il `Course` tabella.</span><span class="sxs-lookup"><span data-stu-id="fb381-442">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="fb381-443">Aggiungere il codice evidenziato di seguito.</span><span class="sxs-lookup"><span data-stu-id="fb381-443">Add the following highlighted code.</span></span> <span data-ttu-id="fb381-444">Il nuovo codice viene inserito dopo il `.CreateTable( name: "Department"` blocco:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="fb381-444">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="fb381-445">Con le modifiche precedenti, esistente `Course` righe saranno correlate al reparto "Temp" dopo il `ComplexDataModel` `Up` esecuzione del metodo.</span><span class="sxs-lookup"><span data-stu-id="fb381-445">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="fb381-446">Un'app di produzione è necessario:</span><span class="sxs-lookup"><span data-stu-id="fb381-446">A production app would:</span></span>

* <span data-ttu-id="fb381-447">Includere codice o script per aggiungere `Department` righe e le relative `Course` righe al nuovo `Department` righe.</span><span class="sxs-lookup"><span data-stu-id="fb381-447">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="fb381-448">Non utilizzare il reparto "Temp" o il valore predefinito per `Course.DepartmentID `.</span><span class="sxs-lookup"><span data-stu-id="fb381-448">Would not use the "Temp" department or the default value for `Course.DepartmentID `.</span></span>

<span data-ttu-id="fb381-449">L'esercitazione successiva illustra i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="fb381-449">The next tutorial covers related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fb381-450">[Precedente](xref:data/ef-rp/migrations)
[Successivo](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="fb381-450">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>