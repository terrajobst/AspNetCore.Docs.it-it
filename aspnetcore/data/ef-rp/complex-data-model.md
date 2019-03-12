---
title: Razor Pages con EF Core in ASP.NET Core - Modello di dati - 5 di 8
author: rick-anderson
description: In questa esercitazione si aggiungono altre entità e relazioni e si personalizza il modello di dati specificando regole di formattazione, convalida e mapping.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 56c2d783e8c75aad7487aa03309bb962f4354567
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346528"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="d96d9-103">Razor Pages con EF Core in ASP.NET Core - Modello di dati - 5 di 8</span><span class="sxs-lookup"><span data-stu-id="d96d9-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d96d9-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d96d9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="d96d9-105">Nelle esercitazioni precedenti è stato usato un modello di dati semplice costituito da tre entità.</span><span class="sxs-lookup"><span data-stu-id="d96d9-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="d96d9-106">In questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="d96d9-106">In this tutorial:</span></span>

* <span data-ttu-id="d96d9-107">Vengono aggiunte altre entità e relazioni.</span><span class="sxs-lookup"><span data-stu-id="d96d9-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="d96d9-108">Il modello di dati viene personalizzato specificando regole di formattazione, convalida e mapping del database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="d96d9-109">Le classi di entità per il modello di dati completato sono visualizzate nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Diagramma dell'entità](complex-data-model/_static/diagram.png)

<span data-ttu-id="d96d9-111">Se si verificano problemi che non si è in grado di risolvere, scaricare l'[app completa](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="d96d9-111">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="d96d9-112">Personalizzare il modello di dati usando gli attributi</span><span class="sxs-lookup"><span data-stu-id="d96d9-112">Customize the data model with attributes</span></span>

<span data-ttu-id="d96d9-113">In questa sezione il modello di dati viene personalizzato usando gli attributi.</span><span class="sxs-lookup"><span data-stu-id="d96d9-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="d96d9-114">Attributo DataType</span><span class="sxs-lookup"><span data-stu-id="d96d9-114">The DataType attribute</span></span>

<span data-ttu-id="d96d9-115">Attualmente le pagine Student (Studente) visualizzano l'ora associata alla data di iscrizione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="d96d9-116">In genere i campi data visualizzano solo la data e non l'ora.</span><span class="sxs-lookup"><span data-stu-id="d96d9-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="d96d9-117">Aggiornare *Models/Student.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="d96d9-118">L'attributo [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) indica un tipo di dati più specifico rispetto al tipo intrinseco del database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-118">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="d96d9-119">In questo caso deve essere visualizzata solo la data e non la data e l'ora.</span><span class="sxs-lookup"><span data-stu-id="d96d9-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="d96d9-120">L'enumerazione [DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) offre molti tipi di dati, ad esempio Date, Time, PhoneNumber, Currency, EmailAddress e così via. L'attributo `DataType` può anche consentire all'app di offrire automaticamente funzionalità specifiche del tipo.</span><span class="sxs-lookup"><span data-stu-id="d96d9-120">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="d96d9-121">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d96d9-121">For example:</span></span>

* <span data-ttu-id="d96d9-122">Il collegamento `mailto:` viene creato automaticamente per `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="d96d9-123">Il selettore data viene incluso per `DataType.Date` nella maggior parte dei browser.</span><span class="sxs-lookup"><span data-stu-id="d96d9-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="d96d9-124">L'attributo `DataType` genera attributi HTML 5 `data-` supportati dai browser HTML 5.</span><span class="sxs-lookup"><span data-stu-id="d96d9-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="d96d9-125">Gli attributi `DataType` non garantiscono la convalida.</span><span class="sxs-lookup"><span data-stu-id="d96d9-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="d96d9-126">`DataType.Date` non specifica il formato della data visualizzata.</span><span class="sxs-lookup"><span data-stu-id="d96d9-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="d96d9-127">Per impostazione predefinita il campo data viene visualizzato in base ai formati predefiniti per il valore [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) del server.</span><span class="sxs-lookup"><span data-stu-id="d96d9-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="d96d9-128">L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:</span><span class="sxs-lookup"><span data-stu-id="d96d9-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="d96d9-129">L'impostazione `ApplyFormatInEditMode` specifica che la formattazione deve essere applicata anche all'interfaccia utente di modifica.</span><span class="sxs-lookup"><span data-stu-id="d96d9-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="d96d9-130">Alcuni campi non devono usare `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="d96d9-131">Ad esempio il simbolo di valuta in genere non deve essere visualizzato in una casella di testo di modifica.</span><span class="sxs-lookup"><span data-stu-id="d96d9-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="d96d9-132">L'attributo `DisplayFormat` può essere usato da solo.</span><span class="sxs-lookup"><span data-stu-id="d96d9-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="d96d9-133">In genere l'uso dell'attributo `DataType` con l'attributo `DisplayFormat` è consigliato.</span><span class="sxs-lookup"><span data-stu-id="d96d9-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="d96d9-134">L'attributo `DataType` offre la semantica dei dati anziché specificarne il rendering in una schermata.</span><span class="sxs-lookup"><span data-stu-id="d96d9-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="d96d9-135">L'attributo `DataType` offre i vantaggi seguenti che non sono disponibili in `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="d96d9-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="d96d9-136">Il browser può abilitare le funzionalità HTML5.</span><span class="sxs-lookup"><span data-stu-id="d96d9-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="d96d9-137">Ad esempio può visualizzare un controllo di calendario, il simbolo della valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica, alcune istanze di convalida lato client e così via.</span><span class="sxs-lookup"><span data-stu-id="d96d9-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="d96d9-138">Per impostazione predefinita, il browser esegue il rendering dei dati usando il formato corretto in base alle impostazioni locali.</span><span class="sxs-lookup"><span data-stu-id="d96d9-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="d96d9-139">Per altre informazioni, vedere la [documentazione dell'helper tag \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="d96d9-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="d96d9-140">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d96d9-140">Run the app.</span></span> <span data-ttu-id="d96d9-141">Passare alla pagina Students Index (Indice studenti).</span><span class="sxs-lookup"><span data-stu-id="d96d9-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="d96d9-142">L'ora non viene più visualizzata.</span><span class="sxs-lookup"><span data-stu-id="d96d9-142">Times are no longer displayed.</span></span> <span data-ttu-id="d96d9-143">Ogni visualizzazione che usa il modello `Student` visualizza la data senza l'ora.</span><span class="sxs-lookup"><span data-stu-id="d96d9-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Pagina Students Index (Indice studenti) con date e senza ore](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="d96d9-145">Attributo StringLength</span><span class="sxs-lookup"><span data-stu-id="d96d9-145">The StringLength attribute</span></span>

<span data-ttu-id="d96d9-146">È possibile specificare regole di convalida dei dati e messaggi di errore di convalida usando gli attributi.</span><span class="sxs-lookup"><span data-stu-id="d96d9-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="d96d9-147">L'attributo [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) specifica il numero minimo e massimo di caratteri consentiti in un campo dati.</span><span class="sxs-lookup"><span data-stu-id="d96d9-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="d96d9-148">L'attributo `StringLength` offre anche la convalida lato client e lato server.</span><span class="sxs-lookup"><span data-stu-id="d96d9-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="d96d9-149">Il valore minimo non ha alcun effetto sullo schema del database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="d96d9-150">Aggiornare il modello `Student` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="d96d9-151">Il codice precedente limita i nomi a un massimo di 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="d96d9-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="d96d9-152">L'attributo `StringLength` non impedisce a un utente di immettere spazi vuoti per un nome.</span><span class="sxs-lookup"><span data-stu-id="d96d9-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="d96d9-153">L'attributo [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) viene usato per applicare restrizioni all'input.</span><span class="sxs-lookup"><span data-stu-id="d96d9-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="d96d9-154">Ad esempio il codice seguente richiede che il primo carattere sia maiuscolo e i caratteri rimanenti siano caratteri alfabetici:</span><span class="sxs-lookup"><span data-stu-id="d96d9-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="d96d9-155">Eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="d96d9-155">Run the app:</span></span>

* <span data-ttu-id="d96d9-156">Passare alla pagina Student (Studente).</span><span class="sxs-lookup"><span data-stu-id="d96d9-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="d96d9-157">Selezionare **Crea nuovo** e immettere un nome di lunghezza superiore a 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="d96d9-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="d96d9-158">Quando si fa clic su **Crea** la convalida lato client visualizza un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="d96d9-158">Select **Create**, client-side validation shows an error message.</span></span>

![Pagina Students Index (Indice studenti) con errori di lunghezza stringa](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="d96d9-160">In **Esplora oggetti di SQL Server** (SSOX) aprire il designer della tabella **Student** (Studente) facendo doppio clic sulla tabella.</span><span class="sxs-lookup"><span data-stu-id="d96d9-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabella Student (Studenti) in SSOX prima delle migrazioni](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="d96d9-162">L'immagine precedente visualizza lo schema per la tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="d96d9-163">I campi nome hanno il tipo `nvarchar(MAX)` perché migrations non è stato eseguito nel database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="d96d9-164">Quando le istruzioni migrations verranno eseguite, più avanti in questa esercitazione, i campi nome diventeranno `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="d96d9-165">Attributo Column</span><span class="sxs-lookup"><span data-stu-id="d96d9-165">The Column attribute</span></span>

<span data-ttu-id="d96d9-166">Gli attributi possono controllare il mapping delle classi e delle proprietà nel database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="d96d9-167">In questa sezione l'attributo `Column` viene usato per il mapping del nome della proprietà `FirstMidName` su "FirstName" nel database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="d96d9-168">Quando viene creato il database, i nomi delle proprietà nel modello vengono usati per i nomi di colonna (tranne quando viene usato l'attributo `Column`).</span><span class="sxs-lookup"><span data-stu-id="d96d9-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="d96d9-169">Il modello `Student` usa il nome `FirstMidName` per il campo first-name (Nome) perché il campo potrebbe contenere anche un secondo nome.</span><span class="sxs-lookup"><span data-stu-id="d96d9-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="d96d9-170">Aggiornare il file *Student.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="d96d9-171">Con la modifica precedente, `Student.FirstMidName` nell'app esegue il mapping alla colonna `FirstName` della tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="d96d9-172">L'aggiunta dell'attributo `Column` modifica il modello che supporta `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="d96d9-173">Il modello che supporta `SchoolContext` non corrisponde più al database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="d96d9-174">Se l'app viene eseguita prima di applicare migrations, viene generata l'eccezione seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="d96d9-175">Per aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="d96d9-175">To update the DB:</span></span>

* <span data-ttu-id="d96d9-176">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="d96d9-176">Build the project.</span></span>
* <span data-ttu-id="d96d9-177">Aprire una finestra di comando nella cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="d96d9-177">Open a command window in the project folder.</span></span> <span data-ttu-id="d96d9-178">Immettere i comandi seguenti per creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="d96d9-178">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d96d9-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d96d9-179">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d96d9-180">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d96d9-180">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

------

<span data-ttu-id="d96d9-181">Il comando `migrations add ColumnFirstName` genera il messaggio di avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-181">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="d96d9-182">L'avviso viene generato perché i campi nome ora sono limitati a 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="d96d9-182">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="d96d9-183">Se un nome nel database ha più di 50 caratteri, i caratteri dal 51 all'ultimo andranno perduti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-183">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="d96d9-184">Eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="d96d9-184">Test the app.</span></span>

<span data-ttu-id="d96d9-185">Aprire la tabella Student (Studente) in SSOX:</span><span class="sxs-lookup"><span data-stu-id="d96d9-185">Open the Student table in SSOX:</span></span>

![Tabella Students (Studenti) in SSOX dopo le migrazioni](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="d96d9-187">Prima dell'applicazione della migrazione, le colonne del nome erano di tipo [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="d96d9-187">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="d96d9-188">Ora le colonne del nome sono di tipo `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-188">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="d96d9-189">Il nome della colonna è cambiato da `FirstMidName` a `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-189">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="d96d9-190">Nella sezione seguente la compilazione dell'applicazione genera errori del compilatore in alcune fasi.</span><span class="sxs-lookup"><span data-stu-id="d96d9-190">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="d96d9-191">Le istruzioni specificano quando compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-191">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="d96d9-192">Aggiornamento dell'entità Student</span><span class="sxs-lookup"><span data-stu-id="d96d9-192">Student entity update</span></span>

![Entità Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="d96d9-194">Aggiornare *Models/Student.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-194">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="d96d9-195">Attributo Required</span><span class="sxs-lookup"><span data-stu-id="d96d9-195">The Required attribute</span></span>

<span data-ttu-id="d96d9-196">L'attributo `Required` rende obbligatori i campi delle proprietà del nome.</span><span class="sxs-lookup"><span data-stu-id="d96d9-196">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="d96d9-197">L'attributo `Required` non è necessario per i tipi non nullable, ad esempio per i tipi valore (`DateTime`, `int`, `double` e così via).</span><span class="sxs-lookup"><span data-stu-id="d96d9-197">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="d96d9-198">I tipi che non possono essere null vengono considerati automaticamente come campi obbligatori.</span><span class="sxs-lookup"><span data-stu-id="d96d9-198">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="d96d9-199">L'attributo `Required` può essere sostituito con un parametro di lunghezza minima nell'attributo `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="d96d9-199">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="d96d9-200">Attributo Display</span><span class="sxs-lookup"><span data-stu-id="d96d9-200">The Display attribute</span></span>

<span data-ttu-id="d96d9-201">L'attributo `Display` specifica che la didascalia per le caselle di testo deve essere "First Name" (Nome), "Last Name" (Cognome), "Full Name" (Nome e cognome) ed "Enrollment Date" (Data di iscrizione).</span><span class="sxs-lookup"><span data-stu-id="d96d9-201">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="d96d9-202">Nelle didascalie predefinite le parole non erano divise da nessuno spazio, ad esempio "LastName".</span><span class="sxs-lookup"><span data-stu-id="d96d9-202">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="d96d9-203">Proprietà calcolata FullName</span><span class="sxs-lookup"><span data-stu-id="d96d9-203">The FullName calculated property</span></span>

<span data-ttu-id="d96d9-204">`FullName` è una proprietà calcolata che restituisce un valore creato concatenando altre due proprietà.</span><span class="sxs-lookup"><span data-stu-id="d96d9-204">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="d96d9-205">`FullName` non è impostabile e include solo una funzione di accesso get.</span><span class="sxs-lookup"><span data-stu-id="d96d9-205">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="d96d9-206">Nel database non viene creata una colonna `FullName`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-206">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="d96d9-207">Creare l'entità Instructor</span><span class="sxs-lookup"><span data-stu-id="d96d9-207">Create the Instructor Entity</span></span>

![Entità Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="d96d9-209">Creare *Models/Instructor.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-209">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="d96d9-210">Un'unica riga può ospitare più attributi.</span><span class="sxs-lookup"><span data-stu-id="d96d9-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="d96d9-211">Gli attributi `HireDate` possono essere scritti come segue:</span><span class="sxs-lookup"><span data-stu-id="d96d9-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="d96d9-212">Proprietà di navigazione CourseAssignments e OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="d96d9-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="d96d9-213">Le proprietà `CourseAssignments` e `OfficeAssignment` sono proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="d96d9-214">Un insegnante può tenere un numero qualsiasi di corsi, pertanto `CourseAssignments` è definita come raccolta.</span><span class="sxs-lookup"><span data-stu-id="d96d9-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="d96d9-215">Se una proprietà di navigazione contiene più entità:</span><span class="sxs-lookup"><span data-stu-id="d96d9-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="d96d9-216">Deve essere un tipo di elenco in cui le voci possono essere aggiunte, eliminate e aggiornate.</span><span class="sxs-lookup"><span data-stu-id="d96d9-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="d96d9-217">I tipi di proprietà di navigazione includono:</span><span class="sxs-lookup"><span data-stu-id="d96d9-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="d96d9-218">Se è specificato `ICollection<T>`, per impostazione predefinita EF Core crea una raccolta `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="d96d9-219">L'entità `CourseAssignment` è illustrata nella sezione sulle relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="d96d9-220">Le regole business di Contoso University specificano che un insegnante non può avere più di un ufficio.</span><span class="sxs-lookup"><span data-stu-id="d96d9-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="d96d9-221">La proprietà `OfficeAssignment` contiene un'unica entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="d96d9-222">`OfficeAssignment` è null se non è assegnato nessun ufficio.</span><span class="sxs-lookup"><span data-stu-id="d96d9-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="d96d9-223">Creare l'entità OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="d96d9-223">Create the OfficeAssignment entity</span></span>

![Entità OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="d96d9-225">Creare *Models/OfficeAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="d96d9-226">Attributo Key</span><span class="sxs-lookup"><span data-stu-id="d96d9-226">The Key attribute</span></span>

<span data-ttu-id="d96d9-227">L'attributo `[Key]` viene usato per identificare una proprietà come chiave primaria (PK, Primary Key) quando il nome della proprietà è diverso da classnameID o ID.</span><span class="sxs-lookup"><span data-stu-id="d96d9-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="d96d9-228">È una relazione uno-a-zero-o-uno tra le entità `Instructor` e `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="d96d9-229">L'assegnazione di un ufficio esiste solo in relazione all'insegnante al quale viene assegnato l'ufficio.</span><span class="sxs-lookup"><span data-stu-id="d96d9-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="d96d9-230">La chiave primaria `OfficeAssignment` è anche la chiave esterna (FK, Foreign Key) per l'entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="d96d9-231">EF Core non riconosce automaticamente `InstructorID` come chiave primaria di `OfficeAssignment` perché:</span><span class="sxs-lookup"><span data-stu-id="d96d9-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="d96d9-232">`InstructorID` non segue la convenzione di denominazione ID o classnameID.</span><span class="sxs-lookup"><span data-stu-id="d96d9-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="d96d9-233">Di conseguenza l'attributo `Key` viene usato per identificare l'entità `InstructorID` come chiave primaria:</span><span class="sxs-lookup"><span data-stu-id="d96d9-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="d96d9-234">Per impostazione predefinita EF Core considera la chiave come non generata dal database, perché la colonna è destinata a una relazione di identificazione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="d96d9-235">Proprietà di navigazione Instructor</span><span class="sxs-lookup"><span data-stu-id="d96d9-235">The Instructor navigation property</span></span>

<span data-ttu-id="d96d9-236">La proprietà di navigazione `OfficeAssignment` per l'entità `Instructor` è nullable perché:</span><span class="sxs-lookup"><span data-stu-id="d96d9-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="d96d9-237">I tipi di riferimento (ad esempio le classi) sono nullable.</span><span class="sxs-lookup"><span data-stu-id="d96d9-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="d96d9-238">Un insegnante potrebbe non avere un ufficio assegnato.</span><span class="sxs-lookup"><span data-stu-id="d96d9-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="d96d9-239">L'entità `OfficeAssignment` ha una proprietà di navigazione `Instructor` non nullable perché:</span><span class="sxs-lookup"><span data-stu-id="d96d9-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="d96d9-240">`InstructorID` è non nullable.</span><span class="sxs-lookup"><span data-stu-id="d96d9-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="d96d9-241">Un'assegnazione di ufficio non può esistere senza un insegnante.</span><span class="sxs-lookup"><span data-stu-id="d96d9-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="d96d9-242">Quando un'entità `Instructor` dispone di un'entità `OfficeAssignment` correlata, ogni entità include un riferimento all'altra entità nella relativa proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="d96d9-243">L'attributo `[Required]` può essere applicato alla proprietà di navigazione `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="d96d9-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="d96d9-244">Il codice precedente specifica che deve essere presente un insegnante correlato.</span><span class="sxs-lookup"><span data-stu-id="d96d9-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="d96d9-245">Il codice precedente non è necessario perché la chiave esterna `InstructorID` (che è anche la chiave primaria) è non nullable.</span><span class="sxs-lookup"><span data-stu-id="d96d9-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="d96d9-246">Modificare l'entità Course</span><span class="sxs-lookup"><span data-stu-id="d96d9-246">Modify the Course Entity</span></span>

![Entità Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="d96d9-248">Aggiornare *Models/Course.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="d96d9-249">L'entità `Course` dispone di una proprietà chiave esterna (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="d96d9-250">`DepartmentID` fa riferimento all'entità `Department` correlata.</span><span class="sxs-lookup"><span data-stu-id="d96d9-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="d96d9-251">L'entità `Course` dispone di una proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="d96d9-252">EF Core non richiede una proprietà chiave esterna per un modello di dati quando il modello dispone di una proprietà di navigazione per un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="d96d9-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="d96d9-253">EF Core crea automaticamente le chiavi esterne nel database quando sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="d96d9-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="d96d9-254">EF Core crea [proprietà nascoste](/ef/core/modeling/shadow-properties) per le chiavi esterne create automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d96d9-254">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="d96d9-255">Il fatto di avere la chiave esterna nel modello di dati può rendere più semplici ed efficienti gli aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="d96d9-256">Si consideri ad esempio un modello in cui la proprietà chiave esterna `DepartmentID` *non* è inclusa.</span><span class="sxs-lookup"><span data-stu-id="d96d9-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="d96d9-257">Quando un'entità Course viene recuperata per la modifica:</span><span class="sxs-lookup"><span data-stu-id="d96d9-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="d96d9-258">L'entità `Department` è null se non viene caricata in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="d96d9-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="d96d9-259">Per aggiornare l'entità Course, è in primo luogo necessario recuperare l'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="d96d9-260">Quando la proprietà chiave esterna `DepartmentID` è inclusa nel modello di dati, non è necessario recuperare l'entità `Department` prima di un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="d96d9-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="d96d9-261">Attributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="d96d9-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="d96d9-262">L'attributo `[DatabaseGenerated(DatabaseGeneratedOption.None)]` indica che la chiave primaria viene resa disponibile dall'applicazione anziché essere generata dal database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="d96d9-263">Per impostazione predefinita, Core EF presuppone che i valori di chiave primaria vengano generati dal database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="d96d9-264">La generazione dei valori di chiave primaria nel database è in genere l'approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="d96d9-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="d96d9-265">Per le entità `Course` la chiave primaria viene specificata dall'utente.</span><span class="sxs-lookup"><span data-stu-id="d96d9-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="d96d9-266">Un esempio può essere un numero di corso, quale la serie 1000 per il reparto di matematica o la serie 2000 per il reparto di lingua inglese.</span><span class="sxs-lookup"><span data-stu-id="d96d9-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="d96d9-267">L'attributo `DatabaseGenerated` può essere usato anche per generare valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="d96d9-268">Ad esempio il database può generare automaticamente un campo data per registrare la data di creazione o aggiornamento di una riga.</span><span class="sxs-lookup"><span data-stu-id="d96d9-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="d96d9-269">Per altre informazioni, vedere [Generated Properties](/ef/core/modeling/generated-properties) (Proprietà generate).</span><span class="sxs-lookup"><span data-stu-id="d96d9-269">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="d96d9-270">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="d96d9-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="d96d9-271">Le proprietà chiave esterna (FK) e le proprietà di navigazione nell'entità `Course` riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d96d9-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="d96d9-272">Un corso viene assegnato a un solo reparto, pertanto è presente una chiave esterna `DepartmentID` e una proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="d96d9-273">Un corso può avere un numero qualsiasi di studenti iscritti, pertanto la proprietà di navigazione `Enrollments` è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="d96d9-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="d96d9-274">Un corso può essere impartito da più insegnanti, pertanto la proprietà di navigazione `CourseAssignments` è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="d96d9-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="d96d9-275">`CourseAssignment` viene illustrato [più avanti](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="d96d9-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="d96d9-276">Creare l'entità Department</span><span class="sxs-lookup"><span data-stu-id="d96d9-276">Create the Department entity</span></span>

![Entità Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="d96d9-278">Creare *Models/Department.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="d96d9-279">Attributo Column</span><span class="sxs-lookup"><span data-stu-id="d96d9-279">The Column attribute</span></span>

<span data-ttu-id="d96d9-280">In precedenza l'attributo `Column` è stato usato per modificare il mapping del nome di colonna.</span><span class="sxs-lookup"><span data-stu-id="d96d9-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="d96d9-281">Nel codice dell'entità `Department` l'attributo `Column` viene usato per modificare il mapping dei tipi di dati SQL.</span><span class="sxs-lookup"><span data-stu-id="d96d9-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="d96d9-282">La colonna `Budget` viene definita usando il tipo SQL Server money nel database:</span><span class="sxs-lookup"><span data-stu-id="d96d9-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="d96d9-283">In genere il mapping di colonne non è necessario.</span><span class="sxs-lookup"><span data-stu-id="d96d9-283">Column mapping is generally not required.</span></span> <span data-ttu-id="d96d9-284">Generalmente EF Core sceglie il tipo di dati SQL Server appropriato in base al tipo CLR della proprietà.</span><span class="sxs-lookup"><span data-stu-id="d96d9-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="d96d9-285">Il tipo CLR `decimal` esegue il mapping a un tipo SQL Server `decimal`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="d96d9-286">`Budget` è associato alla valuta e il tipo di dati money è più adatto per la valuta.</span><span class="sxs-lookup"><span data-stu-id="d96d9-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="d96d9-287">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="d96d9-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="d96d9-288">Le proprietà chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d96d9-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="d96d9-289">Un reparto può avere o non avere un amministratore.</span><span class="sxs-lookup"><span data-stu-id="d96d9-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="d96d9-290">Un amministratore è sempre un insegnante.</span><span class="sxs-lookup"><span data-stu-id="d96d9-290">An administrator is always an instructor.</span></span> <span data-ttu-id="d96d9-291">Di conseguenza la proprietà `InstructorID` è inclusa come chiave esterna per l'entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="d96d9-292">La proprietà di navigazione è denominata `Administrator` ma contiene un'entità `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="d96d9-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="d96d9-293">Il punto interrogativo (?) nel codice precedente specifica che la proprietà è nullable.</span><span class="sxs-lookup"><span data-stu-id="d96d9-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="d96d9-294">Un reparto può avere molti corsi, pertanto è disponibile una proprietà di navigazione Courses:</span><span class="sxs-lookup"><span data-stu-id="d96d9-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="d96d9-295">Nota: per convenzione, EF Core abilita l'eliminazione a catena per le chiavi esterne non nullable e per le relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="d96d9-296">L'eliminazione a catena può generare regole di eliminazione a catena circolari.</span><span class="sxs-lookup"><span data-stu-id="d96d9-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="d96d9-297">Quando viene aggiunta una migrazione, le regole di eliminazione a catena circolari determinano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="d96d9-298">Se ad esempio la proprietà `Department.InstructorID` non è stata definita come nullable:</span><span class="sxs-lookup"><span data-stu-id="d96d9-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="d96d9-299">EF Core configura una regola di eliminazione a catena per eliminare l'insegnante quando viene eliminato il reparto.</span><span class="sxs-lookup"><span data-stu-id="d96d9-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="d96d9-300">L'eliminazione dell'insegnante quando viene eliminato il reparto non è il comportamento atteso.</span><span class="sxs-lookup"><span data-stu-id="d96d9-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="d96d9-301">Se le regole business richiedevano che la proprietà `InstructorID` fosse non nullable, usare l'istruzione API Fluent seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="d96d9-302">Il codice precedente disabilita l'eliminazione a catena per la relazione reparto-insegnante.</span><span class="sxs-lookup"><span data-stu-id="d96d9-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="d96d9-303">Aggiornare l'entità Enrollment</span><span class="sxs-lookup"><span data-stu-id="d96d9-303">Update the Enrollment entity</span></span>

<span data-ttu-id="d96d9-304">Un record di iscrizione è relativo a un solo corso frequentato da un solo studente.</span><span class="sxs-lookup"><span data-stu-id="d96d9-304">An enrollment record is for one course taken by one student.</span></span>

![Entità Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="d96d9-306">Aggiornare *Models/Enrollment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="d96d9-307">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="d96d9-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="d96d9-308">Le proprietà chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d96d9-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="d96d9-309">Un record di iscrizione è relativo a un solo corso, pertanto sono presenti una proprietà chiave esterna `CourseID` e una proprietà di navigazione `Course`:</span><span class="sxs-lookup"><span data-stu-id="d96d9-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="d96d9-310">Un record di iscrizione è relativo a un solo studente, pertanto sono presenti una proprietà chiave esterna `StudentID` e una proprietà di navigazione `Student`:</span><span class="sxs-lookup"><span data-stu-id="d96d9-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="d96d9-311">Relazioni molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="d96d9-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="d96d9-312">Esiste una relazione molti-a-molti tra le entità `Student` e `Course`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="d96d9-313">L'entità `Enrollment` funziona come una tabella di join molti-a-molti *con payload* nel database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="d96d9-314">"Con payload" significa che la tabella `Enrollment` contiene dati aggiuntivi oltre alle chiavi esterne delle tabelle di join (in questo caso la chiave primaria e `Grade`).</span><span class="sxs-lookup"><span data-stu-id="d96d9-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="d96d9-315">La figura seguente illustra l'aspetto di queste relazioni in un diagramma di entità.</span><span class="sxs-lookup"><span data-stu-id="d96d9-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="d96d9-316">Il diagramma è stato generato con [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) per EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="d96d9-316">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="d96d9-317">La creazione del diagramma non fa parte dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Relazione molti-a-molti Student-Course (Studente-Corso)](complex-data-model/_static/student-course.png)

<span data-ttu-id="d96d9-319">Ogni riga della relazione inizia con un 1 e termina con un asterisco (\*), per indicare una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="d96d9-320">Se la tabella `Enrollment` non include informazioni sul livello, è sufficiente che contenga le due chiavi esterne `CourseID` e `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="d96d9-321">Una tabella di join molti-a-molti senza payload è anche detta tabella di join pura (PJT, Pure Join Table).</span><span class="sxs-lookup"><span data-stu-id="d96d9-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="d96d9-322">Le entità `Instructor` e `Course` hanno una relazione molti-a-molti con una tabella di join pura.</span><span class="sxs-lookup"><span data-stu-id="d96d9-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="d96d9-323">Nota: Le tabelle di join implicite per le relazioni molti-a-molti sono supportate in EF 6.x, ma non in EF Core.</span><span class="sxs-lookup"><span data-stu-id="d96d9-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="d96d9-324">Per altre informazioni, vedere [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (Relazioni molti-a-molti in EF Core 2.0).</span><span class="sxs-lookup"><span data-stu-id="d96d9-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="d96d9-325">Entità CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="d96d9-325">The CourseAssignment entity</span></span>

![Entità CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="d96d9-327">Creare *Models/CourseAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="d96d9-328">Instructor-to-Courses</span><span class="sxs-lookup"><span data-stu-id="d96d9-328">Instructor-to-Courses</span></span>

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="d96d9-330">La relazione molti-a-molti Instructor-to-Courses (Insegnante-Corsi):</span><span class="sxs-lookup"><span data-stu-id="d96d9-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="d96d9-331">Richiede una tabella di join che deve essere rappresentata da un set di entità.</span><span class="sxs-lookup"><span data-stu-id="d96d9-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="d96d9-332">È una tabella di join pura (tabella senza payload).</span><span class="sxs-lookup"><span data-stu-id="d96d9-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="d96d9-333">È pratica comune assegnare a un'entità di join un nome `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="d96d9-334">Ad esempio la tabella di join Instructor-to-Courses che usa questa convenzione sarà `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="d96d9-335">È tuttavia consigliabile usare un nome che descrive la relazione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="d96d9-336">I modelli di dati iniziano come strutture semplici, quindi le loro dimensioni aumentano.</span><span class="sxs-lookup"><span data-stu-id="d96d9-336">Data models start out simple and grow.</span></span> <span data-ttu-id="d96d9-337">In molti casi ai join senza payload vengono assegnati payload in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="d96d9-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="d96d9-338">Se si assegna inizialmente un nome di entità descrittivo, non sarà necessario modificarlo quando la tabella di join cambia.</span><span class="sxs-lookup"><span data-stu-id="d96d9-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="d96d9-339">Idealmente l'entità di join dovrebbe avere il proprio nome naturale (se possibile composto da un'unica parola) nel dominio di business.</span><span class="sxs-lookup"><span data-stu-id="d96d9-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="d96d9-340">Ad esempio Books (Documentazione) e Customers (Clienti) potrebbero essere collegati mediante un'entità di join Ratings (Valutazioni).</span><span class="sxs-lookup"><span data-stu-id="d96d9-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="d96d9-341">Per la relazione molti-a-molti Instructor-to-Courses `CourseAssignment` è preferibile a `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="d96d9-342">Chiave composta</span><span class="sxs-lookup"><span data-stu-id="d96d9-342">Composite key</span></span>

<span data-ttu-id="d96d9-343">Le chiavi esterne non sono nullable.</span><span class="sxs-lookup"><span data-stu-id="d96d9-343">FKs are not nullable.</span></span> <span data-ttu-id="d96d9-344">Le due chiavi esterne in `CourseAssignment` (`InstructorID` e `CourseID`) identificano insieme in modo univoco ogni riga della tabella `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="d96d9-345">`CourseAssignment` non richiede una chiave primaria dedicata.</span><span class="sxs-lookup"><span data-stu-id="d96d9-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="d96d9-346">Le proprietà `InstructorID` e `CourseID` funzionano come una chiave primaria composta.</span><span class="sxs-lookup"><span data-stu-id="d96d9-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="d96d9-347">L'unico modo per specificare chiavi primarie composte in EF Core è l'*API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="d96d9-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="d96d9-348">La sezione successiva illustra come configurare la chiave primaria composta.</span><span class="sxs-lookup"><span data-stu-id="d96d9-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="d96d9-349">La chiave composta garantisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="d96d9-349">The composite key ensures:</span></span>

* <span data-ttu-id="d96d9-350">Sono consentite più righe per un corso.</span><span class="sxs-lookup"><span data-stu-id="d96d9-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="d96d9-351">Sono consentite più righe per un insegnante.</span><span class="sxs-lookup"><span data-stu-id="d96d9-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="d96d9-352">Non sono consentite più righe per lo stesso insegnante e lo stesso corso.</span><span class="sxs-lookup"><span data-stu-id="d96d9-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="d96d9-353">L'entità di join `Enrollment` definisce la propria chiave primaria, pertanto sono possibili i duplicati di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="d96d9-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="d96d9-354">Per evitare tali duplicati:</span><span class="sxs-lookup"><span data-stu-id="d96d9-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="d96d9-355">Aggiungere un indice univoco ai campi chiave esterna oppure</span><span class="sxs-lookup"><span data-stu-id="d96d9-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="d96d9-356">Configurare `Enrollment` con una chiave primaria composta simile a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="d96d9-357">Per altre informazioni, vedere [Indexes](/ef/core/modeling/indexes) (Indici).</span><span class="sxs-lookup"><span data-stu-id="d96d9-357">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="d96d9-358">Aggiornare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="d96d9-358">Update the DB context</span></span>

<span data-ttu-id="d96d9-359">Aggiungere il codice evidenziato seguente al file *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="d96d9-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="d96d9-360">Questo codice aggiunge le nuove entità e configura la chiave primaria composta dell'entità `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="d96d9-361">Alternativa API Fluent agli attributi</span><span class="sxs-lookup"><span data-stu-id="d96d9-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="d96d9-362">Il metodo `OnModelCreating` nel codice precedente usa l'*API Fluent* per configurare il comportamento di EF Core.</span><span class="sxs-lookup"><span data-stu-id="d96d9-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="d96d9-363">L'API è denominata "API Fluent" perché viene spesso usata unendo una serie di chiamate di metodi in un'unica istruzione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="d96d9-364">Il [codice seguente](/ef/core/modeling/#methods-of-configuration) è un esempio di API Fluent:</span><span class="sxs-lookup"><span data-stu-id="d96d9-364">The [following code](/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="d96d9-365">In questa esercitazione l'API Fluent viene usata solo per le operazioni di mapping del database che non possono essere eseguite con gli attributi.</span><span class="sxs-lookup"><span data-stu-id="d96d9-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="d96d9-366">Tuttavia l'API Fluent può specificare la maggior parte delle regole di formattazione, convalida e mapping specificabili tramite gli attributi.</span><span class="sxs-lookup"><span data-stu-id="d96d9-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="d96d9-367">Alcuni attributi quali `MinimumLength` non possono essere applicati con l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="d96d9-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="d96d9-368">`MinimumLength` non modifica lo schema, ma si limita ad applicare una regola di convalida per la lunghezza minima.</span><span class="sxs-lookup"><span data-stu-id="d96d9-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="d96d9-369">Alcuni sviluppatori preferiscono usare solo l'API Fluent, per dare un aspetto "ordinato" alle classi di entità.</span><span class="sxs-lookup"><span data-stu-id="d96d9-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="d96d9-370">È possibile combinare gli attributi e l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="d96d9-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="d96d9-371">Alcune configurazioni possono essere eseguite solo con l'API Fluent (specificando una chiave primaria composta).</span><span class="sxs-lookup"><span data-stu-id="d96d9-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="d96d9-372">Altre configurazioni possono essere eseguite solo con gli attributi (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="d96d9-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="d96d9-373">La procedura consigliata per l'uso dell'API Fluent o degli attributi è la seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="d96d9-374">Scegliere uno dei due approcci.</span><span class="sxs-lookup"><span data-stu-id="d96d9-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="d96d9-375">Usare l'approccio scelto con la massima coerenza possibile.</span><span class="sxs-lookup"><span data-stu-id="d96d9-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="d96d9-376">Alcuni attributi di questa esercitazione vengono usati per:</span><span class="sxs-lookup"><span data-stu-id="d96d9-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="d96d9-377">Solo convalida (ad esempio `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="d96d9-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="d96d9-378">Solo configurazione di EF Core (ad esempio `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="d96d9-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="d96d9-379">Convalida e configurazione di EF Core (ad esempio `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="d96d9-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="d96d9-380">Per altre informazioni sul confronto tra attributi e API Fluent, vedere [Metodi di configurazione](/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="d96d9-380">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="d96d9-381">Diagramma dell'entità che visualizza le relazioni</span><span class="sxs-lookup"><span data-stu-id="d96d9-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="d96d9-382">La figura seguente visualizza il diagramma creato da EF Power Tools per il modello School completato.</span><span class="sxs-lookup"><span data-stu-id="d96d9-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagramma dell'entità](complex-data-model/_static/diagram.png)

<span data-ttu-id="d96d9-384">Il diagramma precedente visualizza quanto segue:</span><span class="sxs-lookup"><span data-stu-id="d96d9-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="d96d9-385">Più linee di relazione uno-a-molti (da 1 a \*).</span><span class="sxs-lookup"><span data-stu-id="d96d9-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="d96d9-386">La linea di relazione uno-a-zero-o-uno (da 1 a 0..1) tra le entità `Instructor` e `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="d96d9-387">La linea di relazione zero-o-uno-a-molti (da 0..1 a \*) tra le entità `Instructor` e `Department`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="d96d9-388">Inizializzare il database con dati di test</span><span class="sxs-lookup"><span data-stu-id="d96d9-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="d96d9-389">Aggiornare il codice in *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="d96d9-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="d96d9-390">Il codice precedente offre i dati di inizializzazione per le nuove entità.</span><span class="sxs-lookup"><span data-stu-id="d96d9-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="d96d9-391">La maggior parte di questo codice crea nuovi oggetti entità e carica dati di esempio.</span><span class="sxs-lookup"><span data-stu-id="d96d9-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="d96d9-392">I dati di esempio vengono usati per i test.</span><span class="sxs-lookup"><span data-stu-id="d96d9-392">The sample data is used for testing.</span></span> <span data-ttu-id="d96d9-393">Visualizzare `Enrollments` e `CourseAssignments` per alcuni esempi del modo in cui può essere impostato il valore di inizializzazione per le tabelle join molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-393">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="d96d9-394">Aggiungere una migrazione</span><span class="sxs-lookup"><span data-stu-id="d96d9-394">Add a migration</span></span>

<span data-ttu-id="d96d9-395">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="d96d9-395">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d96d9-396">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d96d9-396">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d96d9-397">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d96d9-397">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

------

<span data-ttu-id="d96d9-398">Il comando precedente visualizza un avviso sulla possibile perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="d96d9-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="d96d9-399">Se viene eseguito il comando `database update`, viene generato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="d96d9-400">Applicare la migrazione</span><span class="sxs-lookup"><span data-stu-id="d96d9-400">Apply the migration</span></span>

<span data-ttu-id="d96d9-401">Ora che è disponibile un database esistente, è necessario preoccuparsi di come applicare eventuali modifiche future.</span><span class="sxs-lookup"><span data-stu-id="d96d9-401">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="d96d9-402">Questa esercitazione illustra due approcci:</span><span class="sxs-lookup"><span data-stu-id="d96d9-402">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="d96d9-403">Eliminare e ricreare il database</span><span class="sxs-lookup"><span data-stu-id="d96d9-403">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="d96d9-404">[Applicare la migrazione al database esistente](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="d96d9-404">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="d96d9-405">Anche se questo metodo è più complesso e richiede più tempo, è l'approccio consigliato per gli ambienti di produzione reali.</span><span class="sxs-lookup"><span data-stu-id="d96d9-405">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="d96d9-406">**Nota**: questa è una sezione facoltativa dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-406">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="d96d9-407">È possibile eseguire i passaggi di eliminazione e ricreazione e ignorare questa sezione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-407">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="d96d9-408">Se si vuole seguire la procedura descritta in questa sezione, non eseguire i passaggi di eliminazione e ricreazione.</span><span class="sxs-lookup"><span data-stu-id="d96d9-408">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="d96d9-409">Eliminare e ricreare il database</span><span class="sxs-lookup"><span data-stu-id="d96d9-409">Drop and re-create the database</span></span>

<span data-ttu-id="d96d9-410">Il codice aggiornato in `DbInitializer` aggiunge dati di inizializzazione per le nuove entità.</span><span class="sxs-lookup"><span data-stu-id="d96d9-410">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="d96d9-411">Per forzare la creazione di un nuovo database da parte di EF Core, rimuovere e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="d96d9-411">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d96d9-412">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d96d9-412">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d96d9-413">Nella **console di Gestione pacchetti** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-413">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="d96d9-414">Eseguire `Get-Help about_EntityFrameworkCore` dalla console di Gestione pacchetti per ottenere informazioni.</span><span class="sxs-lookup"><span data-stu-id="d96d9-414">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d96d9-415">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="d96d9-415">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d96d9-416">Aprire una finestra di comando e passare alla cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="d96d9-416">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="d96d9-417">La cartella del progetto contiene il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d96d9-417">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="d96d9-418">Digitare quanto segue nella finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="d96d9-418">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

------

<span data-ttu-id="d96d9-419">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="d96d9-419">Run the app.</span></span> <span data-ttu-id="d96d9-420">Quando si esegue l'app viene eseguito il metodo `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-420">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="d96d9-421">`DbInitializer.Initialize` popola il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="d96d9-421">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="d96d9-422">Aprire il database in SSOX:</span><span class="sxs-lookup"><span data-stu-id="d96d9-422">Open the DB in SSOX:</span></span>

* <span data-ttu-id="d96d9-423">Se SSOX è stato aperto in precedenza, fare clic sul pulsante **Aggiorna**.</span><span class="sxs-lookup"><span data-stu-id="d96d9-423">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="d96d9-424">Espandere il nodo **Tabelle**.</span><span class="sxs-lookup"><span data-stu-id="d96d9-424">Expand the **Tables** node.</span></span> <span data-ttu-id="d96d9-425">Vengono visualizzate le tabelle create.</span><span class="sxs-lookup"><span data-stu-id="d96d9-425">The created tables are displayed.</span></span>

![Tabelle in Esplora oggetti di SQL Server](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="d96d9-427">Esaminare la tabella **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="d96d9-427">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="d96d9-428">Fare clic con il pulsante destro del mouse sulla tabella **CourseAssignment** e selezionare **Visualizza dati**.</span><span class="sxs-lookup"><span data-stu-id="d96d9-428">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="d96d9-429">Verificare che la tabella **CourseAssignment** contenga dati.</span><span class="sxs-lookup"><span data-stu-id="d96d9-429">Verify the **CourseAssignment** table contains data.</span></span>

![Dati CourseAssignment in SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="d96d9-431">Applicare la migrazione al database esistente</span><span class="sxs-lookup"><span data-stu-id="d96d9-431">Apply the migration to the existing database</span></span>

<span data-ttu-id="d96d9-432">Questa sezione è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="d96d9-432">This section is optional.</span></span> <span data-ttu-id="d96d9-433">Questa procedura funziona solo se è stata ignorata la sezione [Eliminare e ricreare il database](#drop) precedente.</span><span class="sxs-lookup"><span data-stu-id="d96d9-433">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="d96d9-434">Quando le migrazioni vengono eseguite con dati esistenti, possono essere presenti vincoli di chiave esterna che non vengono soddisfatti con i dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="d96d9-435">Con i dati di produzione, è necessario eseguire passaggi per la migrazione dei dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="d96d9-436">Questa sezione visualizza un esempio di correzione delle violazioni dei vincoli di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="d96d9-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="d96d9-437">Non apportare queste modifiche al codice senza un backup.</span><span class="sxs-lookup"><span data-stu-id="d96d9-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="d96d9-438">Non apportare queste modifiche al codice se è stata completata la sezione precedente e il database è stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="d96d9-438">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="d96d9-439">Il file *{timestamp}_ComplexDataModel.cs* contiene il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d96d9-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="d96d9-440">Il codice precedente aggiunge una chiave esterna non nullable `DepartmentID` alla tabella `Course`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="d96d9-441">Il database dell'esercitazione precedente contiene righe in `Course`, pertanto la tabella non può essere aggiornata mediante le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="d96d9-441">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="d96d9-442">Per far sì che la migrazione `ComplexDataModel` funzioni con i dati esistenti:</span><span class="sxs-lookup"><span data-stu-id="d96d9-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="d96d9-443">Modificare il codice per assegnare un valore predefinito alla nuova colonna (`DepartmentID`).</span><span class="sxs-lookup"><span data-stu-id="d96d9-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="d96d9-444">Creare un reparto fittizio denominato "Temp" che assume il ruolo di reparto predefinito.</span><span class="sxs-lookup"><span data-stu-id="d96d9-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="d96d9-445">Risolvere i vincoli della chiave esterna</span><span class="sxs-lookup"><span data-stu-id="d96d9-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="d96d9-446">Aggiornare il metodo `Up` delle classi `ComplexDataModel`:</span><span class="sxs-lookup"><span data-stu-id="d96d9-446">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="d96d9-447">Aprire il file *{timestamp}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="d96d9-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="d96d9-448">Impostare come commento la riga di codice che aggiunge la colonna `DepartmentID` alla tabella `Course`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="d96d9-449">Aggiungere il codice evidenziato seguente.</span><span class="sxs-lookup"><span data-stu-id="d96d9-449">Add the following highlighted code.</span></span> <span data-ttu-id="d96d9-450">Il nuovo codice viene inserito dopo il blocco `.CreateTable( name: "Department"`:</span><span class="sxs-lookup"><span data-stu-id="d96d9-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="d96d9-451">Con le modifiche precedenti, le righe `Course` esistenti saranno correlate al reparto "Temp" dopo l'esecuzione del metodo `ComplexDataModel` `Up`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-451">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="d96d9-452">Un'app di produzione:</span><span class="sxs-lookup"><span data-stu-id="d96d9-452">A production app would:</span></span>

* <span data-ttu-id="d96d9-453">Includerà codice o script per l'aggiunta di righe `Department` e righe `Course` correlate alle nuove righe `Department`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-453">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="d96d9-454">Non userà il reparto "Temp" o il valore predefinito per `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="d96d9-454">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="d96d9-455">L'esercitazione successiva illustra i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="d96d9-455">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d96d9-456">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d96d9-456">Additional resources</span></span>

* [<span data-ttu-id="d96d9-457">Versione YouTube dell'esercitazione (parte 1)</span><span class="sxs-lookup"><span data-stu-id="d96d9-457">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="d96d9-458">Versione YouTube dell'esercitazione (parte 2)</span><span class="sxs-lookup"><span data-stu-id="d96d9-458">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="d96d9-459">[Precedente](xref:data/ef-rp/migrations)
> [Successivo](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="d96d9-459">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
