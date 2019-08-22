---
title: Razor Pages con EF Core in ASP.NET Core - Modello di dati - 5 di 8
author: tdykstra
description: In questa esercitazione si aggiungono altre entità e relazioni e si personalizza il modello di dati specificando regole di formattazione, convalida e mapping.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 8a1c0759453b02f4ce1c45471a8f93da626f8261
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583290"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="dc79e-103">Razor Pages con EF Core in ASP.NET Core - Modello di dati - 5 di 8</span><span class="sxs-lookup"><span data-stu-id="dc79e-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="dc79e-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc79e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="dc79e-105">Nelle esercitazioni precedenti è stato usato un modello di dati semplice costituito da tre entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="dc79e-106">In questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="dc79e-106">In this tutorial:</span></span>

* <span data-ttu-id="dc79e-107">Vengono aggiunte altre entità e relazioni.</span><span class="sxs-lookup"><span data-stu-id="dc79e-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="dc79e-108">Il modello di dati viene personalizzato specificando regole di formattazione, convalida e mapping del database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="dc79e-109">Il modello di dati completato è illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-109">The completed data model is shown in the following illustration:</span></span>

![Diagramma dell'entità](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a><span data-ttu-id="dc79e-111">Entità Student (Studente)</span><span class="sxs-lookup"><span data-stu-id="dc79e-111">The Student entity</span></span>

![Entità Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="dc79e-113">Sostituire il codice in *Models/Student.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-113">Replace the code in *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

<span data-ttu-id="dc79e-114">Il codice precedente aggiunge una proprietà `FullName` e aggiunge gli attributi seguenti alle proprietà esistenti:</span><span class="sxs-lookup"><span data-stu-id="dc79e-114">The preceding code adds a `FullName` property and adds the following attributes to existing properties:</span></span>

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="dc79e-115">Proprietà calcolata FullName</span><span class="sxs-lookup"><span data-stu-id="dc79e-115">The FullName calculated property</span></span>

<span data-ttu-id="dc79e-116">`FullName` è una proprietà calcolata che restituisce un valore creato concatenando altre due proprietà.</span><span class="sxs-lookup"><span data-stu-id="dc79e-116">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="dc79e-117">`FullName` non può essere impostata, quindi include solo una funzione di accesso get.</span><span class="sxs-lookup"><span data-stu-id="dc79e-117">`FullName` can't be set, so it has only a get accessor.</span></span> <span data-ttu-id="dc79e-118">Nel database non viene creata una colonna `FullName`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-118">No `FullName` column is created in the database.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="dc79e-119">Attributo DataType</span><span class="sxs-lookup"><span data-stu-id="dc79e-119">The DataType attribute</span></span>

```csharp
[DataType(DataType.Date)]
```

<span data-ttu-id="dc79e-120">Per le date di iscrizione degli studenti, tutte le pagine visualizzano attualmente l'ora del giorno insieme alla data, anche se è pertinente solo la data.</span><span class="sxs-lookup"><span data-stu-id="dc79e-120">For student enrollment dates, all of the pages currently display the time of day along with the date, although only the date is relevant.</span></span> <span data-ttu-id="dc79e-121">Mediante gli attributi di annotazione dei dati è possibile modificare il codice per correggere il formato di visualizzazione in tutte le pagine che visualizzano i dati.</span><span class="sxs-lookup"><span data-stu-id="dc79e-121">By using data annotation attributes, you can make one code change that will fix the display format in every page that shows the data.</span></span> 

<span data-ttu-id="dc79e-122">L'attributo [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) indica un tipo di dati più specifico rispetto al tipo intrinseco del database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-122">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="dc79e-123">In questo caso deve essere visualizzata solo la data e non la data e l'ora.</span><span class="sxs-lookup"><span data-stu-id="dc79e-123">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="dc79e-124">L'enumerazione [DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) offre molti tipi di dati, ad esempio Date, Time, PhoneNumber, Currency, EmailAddress e così via. L'attributo `DataType` può anche consentire all'app di offrire automaticamente funzionalità specifiche del tipo.</span><span class="sxs-lookup"><span data-stu-id="dc79e-124">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="dc79e-125">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dc79e-125">For example:</span></span>

* <span data-ttu-id="dc79e-126">Il collegamento `mailto:` viene creato automaticamente per `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-126">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="dc79e-127">Il selettore data viene incluso per `DataType.Date` nella maggior parte dei browser.</span><span class="sxs-lookup"><span data-stu-id="dc79e-127">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="dc79e-128">L'attributo `DataType` genera attributi HTML 5 `data-`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-128">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes.</span></span> <span data-ttu-id="dc79e-129">Gli attributi `DataType` non garantiscono la convalida.</span><span class="sxs-lookup"><span data-stu-id="dc79e-129">The `DataType` attributes don't provide validation.</span></span>

### <a name="the-displayformat-attribute"></a><span data-ttu-id="dc79e-130">Attributo DisplayFormat</span><span class="sxs-lookup"><span data-stu-id="dc79e-130">The DisplayFormat attribute</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="dc79e-131">`DataType.Date` non specifica il formato della data visualizzata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-131">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="dc79e-132">Per impostazione predefinita il campo data viene visualizzato in base ai formati predefiniti per il valore [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) del server.</span><span class="sxs-lookup"><span data-stu-id="dc79e-132">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="dc79e-133">L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data.</span><span class="sxs-lookup"><span data-stu-id="dc79e-133">The `DisplayFormat` attribute is used to explicitly specify the date format.</span></span> <span data-ttu-id="dc79e-134">L'impostazione `ApplyFormatInEditMode` specifica che la formattazione deve essere applicata anche all'interfaccia utente di modifica.</span><span class="sxs-lookup"><span data-stu-id="dc79e-134">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="dc79e-135">Alcuni campi non devono usare `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-135">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="dc79e-136">Ad esempio il simbolo di valuta in genere non deve essere visualizzato in una casella di testo di modifica.</span><span class="sxs-lookup"><span data-stu-id="dc79e-136">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="dc79e-137">L'attributo `DisplayFormat` può essere usato da solo.</span><span class="sxs-lookup"><span data-stu-id="dc79e-137">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="dc79e-138">In genere l'uso dell'attributo `DataType` con l'attributo `DisplayFormat` è consigliato.</span><span class="sxs-lookup"><span data-stu-id="dc79e-138">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="dc79e-139">L'attributo `DataType` offre la semantica dei dati anziché specificarne il rendering in una schermata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-139">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="dc79e-140">L'attributo `DataType` offre i vantaggi seguenti che non sono disponibili in `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-140">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="dc79e-141">Il browser può abilitare le funzionalità HTML5.</span><span class="sxs-lookup"><span data-stu-id="dc79e-141">The browser can enable HTML5 features.</span></span> <span data-ttu-id="dc79e-142">Ad esempio può visualizzare un controllo di calendario, il simbolo della valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica e la convalida dell'input sul lato client.</span><span class="sxs-lookup"><span data-stu-id="dc79e-142">For example, show a calendar control, the locale-appropriate currency symbol, email links, and client-side input validation.</span></span>
* <span data-ttu-id="dc79e-143">Per impostazione predefinita, il browser esegue il rendering dei dati usando il formato corretto in base alle impostazioni locali.</span><span class="sxs-lookup"><span data-stu-id="dc79e-143">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="dc79e-144">Per altre informazioni, vedere la [documentazione dell'helper tag \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="dc79e-144">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

### <a name="the-stringlength-attribute"></a><span data-ttu-id="dc79e-145">Attributo StringLength</span><span class="sxs-lookup"><span data-stu-id="dc79e-145">The StringLength attribute</span></span>

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

<span data-ttu-id="dc79e-146">È possibile specificare regole di convalida dei dati e messaggi di errore di convalida usando gli attributi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="dc79e-147">L'attributo [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) specifica il numero minimo e massimo di caratteri consentiti in un campo dati.</span><span class="sxs-lookup"><span data-stu-id="dc79e-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="dc79e-148">Il codice precedente limita i nomi a un massimo di 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="dc79e-148">The code shown limits names to no more than 50 characters.</span></span> <span data-ttu-id="dc79e-149">Un esempio che imposta la lunghezza minima della stringa è disponibile [di seguito](#the-required-attribute).</span><span class="sxs-lookup"><span data-stu-id="dc79e-149">An example that sets the minimum string length is shown [later](#the-required-attribute).</span></span>

<span data-ttu-id="dc79e-150">L'attributo `StringLength` offre anche la convalida lato client e lato server.</span><span class="sxs-lookup"><span data-stu-id="dc79e-150">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="dc79e-151">Il valore minimo non ha alcun effetto sullo schema del database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-151">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="dc79e-152">L'attributo `StringLength` non impedisce a un utente di immettere spazi vuoti per un nome.</span><span class="sxs-lookup"><span data-stu-id="dc79e-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="dc79e-153">L'attributo [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) può essere usato per applicare restrizioni all'input.</span><span class="sxs-lookup"><span data-stu-id="dc79e-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute can be used to apply restrictions to the input.</span></span> <span data-ttu-id="dc79e-154">Ad esempio il codice seguente richiede che il primo carattere sia maiuscolo e i caratteri rimanenti siano caratteri alfabetici:</span><span class="sxs-lookup"><span data-stu-id="dc79e-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc79e-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc79e-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc79e-156">In **Esplora oggetti di SQL Server** (SSOX) aprire il designer della tabella **Student** (Studente) facendo doppio clic sulla tabella.</span><span class="sxs-lookup"><span data-stu-id="dc79e-156">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabella Student (Studenti) in SSOX prima delle migrazioni](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="dc79e-158">L'immagine precedente visualizza lo schema per la tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-158">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="dc79e-159">I campi del nome sono di tipo `nvarchar(MAX)`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-159">The name fields have type `nvarchar(MAX)`.</span></span> <span data-ttu-id="dc79e-160">Quando una migrazione viene creata e applicata più avanti in questa esercitazione, i campi del nome diventano `nvarchar(50)` come risultato degli attributi di lunghezza della stringa.</span><span class="sxs-lookup"><span data-stu-id="dc79e-160">When a migration is created and applied later in this tutorial, the name fields become `nvarchar(50)` as a result of the string length attributes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc79e-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc79e-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dc79e-162">Nello strumento SQLite esaminare le definizioni di colonna per la tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-162">In your SQLite tool, examine the column definitions for the `Student` table.</span></span> <span data-ttu-id="dc79e-163">I campi del nome sono di tipo `Text`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-163">The name fields have type `Text`.</span></span> <span data-ttu-id="dc79e-164">Si noti che il campo del nome è chiamato `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-164">Notice that the first name field is called `FirstMidName`.</span></span> <span data-ttu-id="dc79e-165">Nella sezione successiva il nome di tale colonna viene modificato in `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-165">In the next section, you change the name of that column to `FirstName`.</span></span>

---

### <a name="the-column-attribute"></a><span data-ttu-id="dc79e-166">Attributo Column</span><span class="sxs-lookup"><span data-stu-id="dc79e-166">The Column attribute</span></span>

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

<span data-ttu-id="dc79e-167">Gli attributi possono controllare il mapping delle classi e delle proprietà nel database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="dc79e-168">Nel modello `Student` l'attributo `Column` viene usato per il mapping del nome della proprietà `FirstMidName` su "FirstName" nel database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-168">In the `Student` model, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the database.</span></span>

<span data-ttu-id="dc79e-169">Quando viene creato il database, i nomi delle proprietà nel modello vengono usati per i nomi di colonna (tranne quando viene usato l'attributo `Column`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-169">When the database is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span> <span data-ttu-id="dc79e-170">Il modello `Student` usa il nome `FirstMidName` per il campo first-name (Nome) perché il campo potrebbe contenere anche un secondo nome.</span><span class="sxs-lookup"><span data-stu-id="dc79e-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="dc79e-171">Con l'attributo `[Column]`, per `Student.FirstMidName` nel modello di dati viene eseguito il mapping alla colonna `FirstName` della tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-171">With the `[Column]` attribute, `Student.FirstMidName` in the data model maps to the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="dc79e-172">L'aggiunta dell'attributo `Column` modifica il modello che supporta `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="dc79e-173">Il modello che supporta `SchoolContext` non corrisponde più al database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="dc79e-174">Questa discrepanza verrà risolta aggiungendo una migrazione più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-174">That discrepancy will be resolved by adding a migration later in this tutorial.</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="dc79e-175">Attributo Required</span><span class="sxs-lookup"><span data-stu-id="dc79e-175">The Required attribute</span></span>

```csharp
[Required]
```

<span data-ttu-id="dc79e-176">L'attributo `Required` rende obbligatori i campi delle proprietà del nome.</span><span class="sxs-lookup"><span data-stu-id="dc79e-176">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="dc79e-177">L'attributo `Required` non è necessario per i tipi che non ammettono valori Null come i tipi valore (ad esempio, `DateTime`, `int` e `double`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-177">The `Required` attribute isn't needed for non-nullable types such as value types (for example, `DateTime`, `int`, and `double`).</span></span> <span data-ttu-id="dc79e-178">I tipi che non possono essere null vengono considerati automaticamente come campi obbligatori.</span><span class="sxs-lookup"><span data-stu-id="dc79e-178">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="dc79e-179">L'attributo `Required` può essere sostituito con un parametro di lunghezza minima nell'attributo `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-179">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="dc79e-180">Attributo Display</span><span class="sxs-lookup"><span data-stu-id="dc79e-180">The Display attribute</span></span>

```csharp
[Display(Name = "Last Name")]
```

<span data-ttu-id="dc79e-181">L'attributo `Display` specifica che la didascalia per le caselle di testo deve essere "First Name" (Nome), "Last Name" (Cognome), "Full Name" (Nome e cognome) ed "Enrollment Date" (Data di iscrizione).</span><span class="sxs-lookup"><span data-stu-id="dc79e-181">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="dc79e-182">Nelle didascalie predefinite le parole non erano divise da nessuno spazio, ad esempio "LastName".</span><span class="sxs-lookup"><span data-stu-id="dc79e-182">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="create-a-migration"></a><span data-ttu-id="dc79e-183">Creare una migrazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-183">Create a migration</span></span>

<span data-ttu-id="dc79e-184">Eseguire l'app e passare alla pagina Students (Studenti).</span><span class="sxs-lookup"><span data-stu-id="dc79e-184">Run the app and go to the Students page.</span></span> <span data-ttu-id="dc79e-185">Viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-185">An exception is thrown.</span></span> <span data-ttu-id="dc79e-186">Con l'attributo `[Column]` EF si aspetta di trovare una colonna denominata `FirstName`, ma il nome della colonna nel database è ancora `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-186">The `[Column]` attribute causes EF to expect to find a column named `FirstName`, but the column name in the database is still `FirstMidName`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc79e-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc79e-187">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc79e-188">Il messaggio di errore è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-188">The error message is similar to the following example:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

* <span data-ttu-id="dc79e-189">Nella console di Gestione pacchetti immettere i comandi seguenti per creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="dc79e-189">In the PMC, enter the following commands to create a new migration and update the database:</span></span>

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  <span data-ttu-id="dc79e-190">Il primo di questi comandi genera il messaggio di avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-190">The first of these commands generates the following warning message:</span></span>

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  <span data-ttu-id="dc79e-191">L'avviso viene generato perché i campi nome ora sono limitati a 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="dc79e-191">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="dc79e-192">Se un nome nel database ha più di 50 caratteri, i caratteri dal 51 all'ultimo andranno perduti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-192">If a name in the database had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="dc79e-193">Aprire la tabella Student (Studente) in SSOX:</span><span class="sxs-lookup"><span data-stu-id="dc79e-193">Open the Student table in SSOX:</span></span>

  ![Tabella Students (Studenti) in SSOX dopo le migrazioni](complex-data-model/_static/ssox-after-migration.png)

  <span data-ttu-id="dc79e-195">Prima dell'applicazione della migrazione, le colonne del nome erano di tipo [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="dc79e-195">Before the migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="dc79e-196">Ora le colonne del nome sono di tipo `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-196">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="dc79e-197">Il nome della colonna è cambiato da `FirstMidName` a `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-197">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc79e-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc79e-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dc79e-199">Il messaggio di errore è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-199">The error message is similar to the following example:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* <span data-ttu-id="dc79e-200">Aprire una finestra di comando nella cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="dc79e-200">Open a command window in the project folder.</span></span> <span data-ttu-id="dc79e-201">Immettere i comandi seguenti per creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="dc79e-201">Enter the following commands to create a new migration and update the database:</span></span>

  ```console
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  <span data-ttu-id="dc79e-202">Il comando database update visualizza un errore simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-202">The database update command displays an error like the following example:</span></span>

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

<span data-ttu-id="dc79e-203">Per questa esercitazione, il modo per superare questo errore consiste nell'eliminare e creare nuovamente la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="dc79e-203">For this tutorial, the way to get past this error is to delete and re-create the initial migration.</span></span> <span data-ttu-id="dc79e-204">Per altre informazioni, vedere la nota di avviso per SQLite all'inizio dell'[esercitazione sulle migrazioni](xref:data/ef-rp/migrations).</span><span class="sxs-lookup"><span data-stu-id="dc79e-204">For more information, see the SQLite warning note at the top of the [migrations tutorial](xref:data/ef-rp/migrations).</span></span>

* <span data-ttu-id="dc79e-205">Eliminare la cartella *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="dc79e-205">Delete the *Migrations* folder.</span></span>
* <span data-ttu-id="dc79e-206">Eseguire i comandi seguenti per eliminare il database, creare una nuova migrazione iniziale e applicare la migrazione:</span><span class="sxs-lookup"><span data-stu-id="dc79e-206">Run the following commands to drop the database, create a new initial migration, and apply the migration:</span></span>

  ```console
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* <span data-ttu-id="dc79e-207">Esaminare la tabella Student nello strumento SQLite.</span><span class="sxs-lookup"><span data-stu-id="dc79e-207">Examine the Student table in your SQLite tool.</span></span> <span data-ttu-id="dc79e-208">La colonna FirstMidName è ora FirstName.</span><span class="sxs-lookup"><span data-stu-id="dc79e-208">The column that was FirstMidName is now FirstName.</span></span>

---

* <span data-ttu-id="dc79e-209">Eseguire l'app e passare alla pagina Students (Studenti).</span><span class="sxs-lookup"><span data-stu-id="dc79e-209">Run the app and go to the Students page.</span></span>
* <span data-ttu-id="dc79e-210">Si noti che l'ora non viene inclusa nell'input o visualizzata insieme alla data.</span><span class="sxs-lookup"><span data-stu-id="dc79e-210">Notice that times are not input or displayed along with dates.</span></span>
* <span data-ttu-id="dc79e-211">Selezionare **Create New** (Crea nuovo) e provare immettere un nome di lunghezza superiore a 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="dc79e-211">Select **Create New**, and try to enter a name longer than 50 characters.</span></span>

> [!Note]
> <span data-ttu-id="dc79e-212">Nelle sezioni seguenti la compilazione dell'app genera errori del compilatore in alcune fasi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-212">In the following sections, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="dc79e-213">Le istruzioni specificano quando compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-213">The instructions specify when to build the app.</span></span>

## <a name="the-instructor-entity"></a><span data-ttu-id="dc79e-214">Entità Instructor</span><span class="sxs-lookup"><span data-stu-id="dc79e-214">The Instructor Entity</span></span>

![Entità Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="dc79e-216">Creare *Models/Instructor.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-216">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

<span data-ttu-id="dc79e-217">Un'unica riga può ospitare più attributi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-217">Multiple attributes can be on one line.</span></span> <span data-ttu-id="dc79e-218">Gli attributi `HireDate` possono essere scritti come segue:</span><span class="sxs-lookup"><span data-stu-id="dc79e-218">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a><span data-ttu-id="dc79e-219">Proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-219">Navigation properties</span></span>

<span data-ttu-id="dc79e-220">Le proprietà `CourseAssignments` e `OfficeAssignment` sono proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-220">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="dc79e-221">Un insegnante può tenere un numero qualsiasi di corsi, pertanto `CourseAssignments` è definita come raccolta.</span><span class="sxs-lookup"><span data-stu-id="dc79e-221">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="dc79e-222">Un insegnante può avere al massimo un ufficio, quindi la proprietà `OfficeAssignment` contiene una singola entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-222">An instructor can have at most one office, so the `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="dc79e-223">`OfficeAssignment` è null se non è assegnato nessun ufficio.</span><span class="sxs-lookup"><span data-stu-id="dc79e-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a><span data-ttu-id="dc79e-224">Entità OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="dc79e-224">The OfficeAssignment entity</span></span>

![Entità OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="dc79e-226">Creare *Models/OfficeAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="dc79e-227">Attributo Key</span><span class="sxs-lookup"><span data-stu-id="dc79e-227">The Key attribute</span></span>

<span data-ttu-id="dc79e-228">L'attributo `[Key]` viene usato per identificare una proprietà come chiave primaria (PK, Primary Key) quando il nome della proprietà è diverso da classnameID o ID.</span><span class="sxs-lookup"><span data-stu-id="dc79e-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="dc79e-229">È una relazione uno-a-zero-o-uno tra le entità `Instructor` e `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="dc79e-230">L'assegnazione di un ufficio esiste solo in relazione all'insegnante al quale viene assegnato l'ufficio.</span><span class="sxs-lookup"><span data-stu-id="dc79e-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="dc79e-231">La chiave primaria `OfficeAssignment` è anche la chiave esterna (FK, Foreign Key) per l'entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span>

<span data-ttu-id="dc79e-232">EF Core non è in grado di riconoscere automaticamente `InstructorID` come chiave primaria di `OfficeAssignment` perché `InstructorID` non segue la convenzione di denominazione ID o classnameID.</span><span class="sxs-lookup"><span data-stu-id="dc79e-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because `InstructorID` doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="dc79e-233">Di conseguenza l'attributo `Key` viene usato per identificare l'entità `InstructorID` come chiave primaria:</span><span class="sxs-lookup"><span data-stu-id="dc79e-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="dc79e-234">Per impostazione predefinita EF Core considera la chiave come non generata dal database, perché la colonna è destinata a una relazione di identificazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="dc79e-235">Proprietà di navigazione Instructor</span><span class="sxs-lookup"><span data-stu-id="dc79e-235">The Instructor navigation property</span></span>

<span data-ttu-id="dc79e-236">La proprietà di navigazione `Instructor.OfficeAssignment` può essere Null perché potrebbe non essere presente una riga `OfficeAssignment` per un determinato insegnante.</span><span class="sxs-lookup"><span data-stu-id="dc79e-236">The `Instructor.OfficeAssignment` navigation property can be null because there might not be an `OfficeAssignment` row for a given instructor.</span></span> <span data-ttu-id="dc79e-237">Un insegnante potrebbe non avere un ufficio assegnato.</span><span class="sxs-lookup"><span data-stu-id="dc79e-237">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="dc79e-238">La proprietà di navigazione `OfficeAssignment.Instructor` avrà sempre un'entità Instructor perché il tipo `InstructorID` della chiave esterna è `int`, ovvero un tipo valore che non ammette valori Null.</span><span class="sxs-lookup"><span data-stu-id="dc79e-238">The `OfficeAssignment.Instructor` navigation property will always have an instructor entity because the foreign key `InstructorID` type is `int`, a non-nullable value type.</span></span> <span data-ttu-id="dc79e-239">Un'assegnazione di ufficio non può esistere senza un insegnante.</span><span class="sxs-lookup"><span data-stu-id="dc79e-239">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="dc79e-240">Quando un'entità `Instructor` dispone di un'entità `OfficeAssignment` correlata, ogni entità include un riferimento all'altra entità nella relativa proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-240">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="dc79e-241">Entità Course</span><span class="sxs-lookup"><span data-stu-id="dc79e-241">The Course Entity</span></span>

![Entità Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="dc79e-243">Aggiornare *Models/Course.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-243">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="dc79e-244">L'entità `Course` dispone di una proprietà chiave esterna (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-244">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="dc79e-245">`DepartmentID` fa riferimento all'entità `Department` correlata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-245">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="dc79e-246">L'entità `Course` dispone di una proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-246">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="dc79e-247">EF Core non richiede una proprietà chiave esterna per un modello di dati quando il modello include una proprietà di navigazione per un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-247">EF Core doesn't require a foreign key property for a data model when the model has a navigation property for a related entity.</span></span> <span data-ttu-id="dc79e-248">EF Core crea automaticamente le chiavi esterne nel database quando sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="dc79e-248">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="dc79e-249">EF Core crea [proprietà nascoste](/ef/core/modeling/shadow-properties) per le chiavi esterne create automaticamente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-249">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="dc79e-250">Includere la chiave esterna in modo esplicito nel modello di dati, tuttavia, può rendere più semplici ed efficienti gli aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-250">However, explicitly including the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="dc79e-251">Si consideri ad esempio un modello in cui la proprietà chiave esterna `DepartmentID` *non* è inclusa.</span><span class="sxs-lookup"><span data-stu-id="dc79e-251">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="dc79e-252">Quando un'entità Course viene recuperata per la modifica:</span><span class="sxs-lookup"><span data-stu-id="dc79e-252">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="dc79e-253">La proprietà `Department` è Null se non viene caricata in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="dc79e-253">The `Department` property is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="dc79e-254">Per aggiornare l'entità Course, è in primo luogo necessario recuperare l'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-254">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="dc79e-255">Quando la proprietà chiave esterna `DepartmentID` è inclusa nel modello di dati, non è necessario recuperare l'entità `Department` prima di un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="dc79e-255">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="dc79e-256">Attributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="dc79e-256">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="dc79e-257">L'attributo `[DatabaseGenerated(DatabaseGeneratedOption.None)]` indica che la chiave primaria viene resa disponibile dall'applicazione anziché essere generata dal database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-257">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="dc79e-258">Per impostazione predefinita, EF Core presuppone che i valori di chiave primaria vengano generati dal database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-258">By default, EF Core assumes that PK values are generated by the database.</span></span> <span data-ttu-id="dc79e-259">La generazione nel database è in genere l'approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="dc79e-259">Database-generated is generally the best approach.</span></span> <span data-ttu-id="dc79e-260">Per le entità `Course` la chiave primaria viene specificata dall'utente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-260">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="dc79e-261">Un esempio può essere un numero di corso, quale la serie 1000 per il reparto di matematica o la serie 2000 per il reparto di lingua inglese.</span><span class="sxs-lookup"><span data-stu-id="dc79e-261">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="dc79e-262">L'attributo `DatabaseGenerated` può essere usato anche per generare valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-262">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="dc79e-263">Ad esempio, il database può generare automaticamente un campo data per registrare la data di creazione o aggiornamento di una riga.</span><span class="sxs-lookup"><span data-stu-id="dc79e-263">For example, the database can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="dc79e-264">Per altre informazioni, vedere [Generated Properties](/ef/core/modeling/generated-properties) (Proprietà generate).</span><span class="sxs-lookup"><span data-stu-id="dc79e-264">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="dc79e-265">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="dc79e-266">Le proprietà chiave esterna (FK) e le proprietà di navigazione nell'entità `Course` riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc79e-266">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="dc79e-267">Un corso viene assegnato a un solo reparto, pertanto è presente una chiave esterna `DepartmentID` e una proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-267">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="dc79e-268">Un corso può avere un numero qualsiasi di studenti iscritti, pertanto la proprietà di navigazione `Enrollments` è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="dc79e-268">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="dc79e-269">Un corso può essere impartito da più insegnanti, pertanto la proprietà di navigazione `CourseAssignments` è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="dc79e-269">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="dc79e-270">`CourseAssignment` viene illustrato [più avanti](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="dc79e-270">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="the-department-entity"></a><span data-ttu-id="dc79e-271">Entità Department</span><span class="sxs-lookup"><span data-stu-id="dc79e-271">The Department entity</span></span>

![Entità Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="dc79e-273">Creare *Models/Department.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-273">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="dc79e-274">Attributo Column</span><span class="sxs-lookup"><span data-stu-id="dc79e-274">The Column attribute</span></span>

<span data-ttu-id="dc79e-275">In precedenza l'attributo `Column` è stato usato per modificare il mapping del nome di colonna.</span><span class="sxs-lookup"><span data-stu-id="dc79e-275">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="dc79e-276">Nel codice dell'entità `Department` l'attributo `Column` viene usato per modificare il mapping dei tipi di dati SQL.</span><span class="sxs-lookup"><span data-stu-id="dc79e-276">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="dc79e-277">La colonna `Budget` viene definita usando il tipo SQL Server money nel database:</span><span class="sxs-lookup"><span data-stu-id="dc79e-277">The `Budget` column is defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="dc79e-278">In genere il mapping di colonne non è necessario.</span><span class="sxs-lookup"><span data-stu-id="dc79e-278">Column mapping is generally not required.</span></span> <span data-ttu-id="dc79e-279">EF Core sceglie il tipo di dati di SQL Server appropriato in base al tipo CLR della proprietà.</span><span class="sxs-lookup"><span data-stu-id="dc79e-279">EF Core chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="dc79e-280">Il tipo CLR `decimal` esegue il mapping a un tipo SQL Server `decimal`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-280">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="dc79e-281">`Budget` è associato alla valuta e il tipo di dati money è più adatto per la valuta.</span><span class="sxs-lookup"><span data-stu-id="dc79e-281">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="dc79e-282">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-282">Foreign key and navigation properties</span></span>

<span data-ttu-id="dc79e-283">Le proprietà chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc79e-283">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="dc79e-284">Un reparto può avere o non avere un amministratore.</span><span class="sxs-lookup"><span data-stu-id="dc79e-284">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="dc79e-285">Un amministratore è sempre un insegnante.</span><span class="sxs-lookup"><span data-stu-id="dc79e-285">An administrator is always an instructor.</span></span> <span data-ttu-id="dc79e-286">Di conseguenza la proprietà `InstructorID` è inclusa come chiave esterna per l'entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-286">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="dc79e-287">La proprietà di navigazione è denominata `Administrator` ma contiene un'entità `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-287">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="dc79e-288">Il punto interrogativo (?) nel codice precedente specifica che la proprietà è nullable.</span><span class="sxs-lookup"><span data-stu-id="dc79e-288">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="dc79e-289">Un reparto può avere molti corsi, pertanto è disponibile una proprietà di navigazione Courses:</span><span class="sxs-lookup"><span data-stu-id="dc79e-289">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="dc79e-290">per convenzione, EF Core abilita l'eliminazione a catena per le chiavi esterne non nullable e per le relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-290">By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="dc79e-291">Questo comportamento predefinito può generare regole di eliminazione a catena circolari.</span><span class="sxs-lookup"><span data-stu-id="dc79e-291">This default behavior can result in circular cascade delete rules.</span></span> <span data-ttu-id="dc79e-292">Quando viene aggiunta una migrazione, le regole di eliminazione a catena circolari causano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-292">Circular cascade delete rules cause an exception when a migration is added.</span></span>

<span data-ttu-id="dc79e-293">Ad esempio, se la definizione della proprietà `Department.InstructorID` specifica che non ammette valori Null, EF Core configurerebbe una regola di eliminazione a catena.</span><span class="sxs-lookup"><span data-stu-id="dc79e-293">For example, if the `Department.InstructorID` property was defined as non-nullable, EF Core would configure a cascade delete rule.</span></span> <span data-ttu-id="dc79e-294">In tal caso, il dipartimento verrebbe eliminato in seguito all'eliminazione dell'insegnante assegnato come amministratore.</span><span class="sxs-lookup"><span data-stu-id="dc79e-294">In that case, the department would be deleted when the instructor assigned as its administrator is deleted.</span></span> <span data-ttu-id="dc79e-295">In questo scenario, una regola Restrict potrebbe essere più sensata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-295">In this scenario, a restrict rule would make more sense.</span></span> <span data-ttu-id="dc79e-296">L'API Fluent seguente imposta una regola Restrict e disabilita l'eliminazione a catena.</span><span class="sxs-lookup"><span data-stu-id="dc79e-296">The following fluent API would set a restrict rule and disable cascade delete.</span></span>

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a><span data-ttu-id="dc79e-297">Entità Enrollment (Iscrizione)</span><span class="sxs-lookup"><span data-stu-id="dc79e-297">The Enrollment entity</span></span>

<span data-ttu-id="dc79e-298">Un record di iscrizione è relativo a un solo corso frequentato da un solo studente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-298">An enrollment record is for one course taken by one student.</span></span>

![Entità Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="dc79e-300">Aggiornare *Models/Enrollment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-300">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="dc79e-301">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-301">Foreign key and navigation properties</span></span>

<span data-ttu-id="dc79e-302">Le proprietà chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc79e-302">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="dc79e-303">Un record di iscrizione è relativo a un solo corso, pertanto sono presenti una proprietà chiave esterna `CourseID` e una proprietà di navigazione `Course`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-303">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="dc79e-304">Un record di iscrizione è relativo a un solo studente, pertanto sono presenti una proprietà chiave esterna `StudentID` e una proprietà di navigazione `Student`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-304">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="dc79e-305">Relazioni molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="dc79e-305">Many-to-Many Relationships</span></span>

<span data-ttu-id="dc79e-306">Esiste una relazione molti-a-molti tra le entità `Student` e `Course`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-306">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="dc79e-307">L'entità `Enrollment` funziona come una tabella di join molti-a-molti *con payload* nel database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-307">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="dc79e-308">"Con payload" significa che la tabella `Enrollment` contiene dati aggiuntivi oltre alle chiavi esterne delle tabelle di join (in questo caso la chiave primaria e `Grade`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-308">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="dc79e-309">La figura seguente illustra l'aspetto di queste relazioni in un diagramma di entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-309">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="dc79e-310">Il diagramma è stato generato con [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) per EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="dc79e-310">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="dc79e-311">La creazione del diagramma non fa parte dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-311">Creating the diagram isn't part of the tutorial.)</span></span>

![Relazione molti-a-molti Student-Course (Studente-Corso)](complex-data-model/_static/student-course.png)

<span data-ttu-id="dc79e-313">Ogni riga della relazione inizia con un 1 e termina con un asterisco (\*), per indicare una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-313">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="dc79e-314">Se la tabella `Enrollment` non include informazioni sul livello, è sufficiente che contenga le due chiavi esterne `CourseID` e `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-314">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="dc79e-315">Una tabella di join molti-a-molti senza payload è anche detta tabella di join pura (PJT, Pure Join Table).</span><span class="sxs-lookup"><span data-stu-id="dc79e-315">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="dc79e-316">Le entità `Instructor` e `Course` hanno una relazione molti-a-molti con una tabella di join pura.</span><span class="sxs-lookup"><span data-stu-id="dc79e-316">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="dc79e-317">Nota: Le tabelle di join implicite per le relazioni molti-a-molti sono supportate in EF 6.x, ma non in EF Core.</span><span class="sxs-lookup"><span data-stu-id="dc79e-317">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="dc79e-318">Per altre informazioni, vedere [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (Relazioni molti-a-molti in EF Core 2.0).</span><span class="sxs-lookup"><span data-stu-id="dc79e-318">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="dc79e-319">Entità CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="dc79e-319">The CourseAssignment entity</span></span>

![Entità CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="dc79e-321">Creare *Models/CourseAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-321">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

<span data-ttu-id="dc79e-322">La relazione molti-a-molti tra insegnanti e corsi richiede una tabella di join e l'entità per tale tabella di join è CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="dc79e-322">The Instructor-to-Courses many-to-many relationship requires a join table, and the entity for that join table is CourseAssignment.</span></span>

![Relazione m:M tra insegnante e corsi](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="dc79e-324">È pratica comune assegnare a un'entità di join un nome `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-324">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="dc79e-325">Ad esempio la tabella di join istruttori-corsi che usa questa convenzione sarebbe `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-325">For example, the Instructor-to-Courses join table using this pattern would be `CourseInstructor`.</span></span> <span data-ttu-id="dc79e-326">È tuttavia consigliabile usare un nome che descrive la relazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-326">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="dc79e-327">I modelli di dati iniziano come strutture semplici, quindi le loro dimensioni aumentano.</span><span class="sxs-lookup"><span data-stu-id="dc79e-327">Data models start out simple and grow.</span></span> <span data-ttu-id="dc79e-328">Le tabelle di join senza payload (PJT) si evolvono spesso per includere il payload.</span><span class="sxs-lookup"><span data-stu-id="dc79e-328">Join tables without payload (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="dc79e-329">Se si assegna inizialmente un nome di entità descrittivo, non sarà necessario modificarlo quando la tabella di join cambia.</span><span class="sxs-lookup"><span data-stu-id="dc79e-329">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="dc79e-330">Idealmente l'entità di join dovrebbe avere il proprio nome naturale (se possibile composto da un'unica parola) nel dominio di business.</span><span class="sxs-lookup"><span data-stu-id="dc79e-330">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="dc79e-331">Ad esempio Books (Documentazione) e Customers (Clienti) potrebbero essere collegati mediante un'entità di join Ratings (Valutazioni).</span><span class="sxs-lookup"><span data-stu-id="dc79e-331">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="dc79e-332">Per la relazione molti-a-molti Instructor-to-Courses `CourseAssignment` è preferibile a `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-332">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="dc79e-333">Chiave composta</span><span class="sxs-lookup"><span data-stu-id="dc79e-333">Composite key</span></span>

<span data-ttu-id="dc79e-334">Le due chiavi esterne in `CourseAssignment` (`InstructorID` e `CourseID`) identificano insieme in modo univoco ogni riga della tabella `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-334">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="dc79e-335">`CourseAssignment` non richiede una chiave primaria dedicata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-335">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="dc79e-336">Le proprietà `InstructorID` e `CourseID` funzionano come una chiave primaria composta.</span><span class="sxs-lookup"><span data-stu-id="dc79e-336">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="dc79e-337">L'unico modo per specificare chiavi primarie composte in EF Core è l'*API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="dc79e-337">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="dc79e-338">La sezione successiva illustra come configurare la chiave primaria composta.</span><span class="sxs-lookup"><span data-stu-id="dc79e-338">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="dc79e-339">La chiave composta garantisce che:</span><span class="sxs-lookup"><span data-stu-id="dc79e-339">The composite key ensures that:</span></span>

* <span data-ttu-id="dc79e-340">Sono consentite più righe per un corso.</span><span class="sxs-lookup"><span data-stu-id="dc79e-340">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="dc79e-341">Sono consentite più righe per un insegnante.</span><span class="sxs-lookup"><span data-stu-id="dc79e-341">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="dc79e-342">Non sono consentite più righe per lo stesso insegnante e lo stesso corso.</span><span class="sxs-lookup"><span data-stu-id="dc79e-342">Multiple rows aren't allowed for the same instructor and course.</span></span>

<span data-ttu-id="dc79e-343">L'entità di join `Enrollment` definisce la propria chiave primaria, pertanto sono possibili i duplicati di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="dc79e-343">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="dc79e-344">Per evitare tali duplicati:</span><span class="sxs-lookup"><span data-stu-id="dc79e-344">To prevent such duplicates:</span></span>

* <span data-ttu-id="dc79e-345">Aggiungere un indice univoco ai campi chiave esterna oppure</span><span class="sxs-lookup"><span data-stu-id="dc79e-345">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="dc79e-346">Configurare `Enrollment` con una chiave primaria composta simile a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-346">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="dc79e-347">Per altre informazioni, vedere [Indexes](/ef/core/modeling/indexes) (Indici).</span><span class="sxs-lookup"><span data-stu-id="dc79e-347">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="dc79e-348">Aggiornare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="dc79e-348">Update the database context</span></span>

<span data-ttu-id="dc79e-349">Aggiornare *Data/SchoolContext.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-349">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

<span data-ttu-id="dc79e-350">Questo codice aggiunge le nuove entità e configura la chiave primaria composta dell'entità `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-350">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="dc79e-351">Alternativa API Fluent agli attributi</span><span class="sxs-lookup"><span data-stu-id="dc79e-351">Fluent API alternative to attributes</span></span>

<span data-ttu-id="dc79e-352">Il metodo `OnModelCreating` nel codice precedente usa l'*API Fluent* per configurare il comportamento di EF Core.</span><span class="sxs-lookup"><span data-stu-id="dc79e-352">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="dc79e-353">L'API è denominata "API Fluent" perché viene spesso usata unendo una serie di chiamate di metodi in un'unica istruzione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-353">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="dc79e-354">Il [codice seguente](/ef/core/modeling/#use-fluent-api-to-configure-a-model) è un esempio di API Fluent:</span><span class="sxs-lookup"><span data-stu-id="dc79e-354">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="dc79e-355">In questa esercitazione l'API Fluent viene usata solo per le operazioni di mapping del database che non possono essere eseguite con gli attributi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-355">In this tutorial, the fluent API is used only for database mapping that can't be done with attributes.</span></span> <span data-ttu-id="dc79e-356">Tuttavia l'API Fluent può specificare la maggior parte delle regole di formattazione, convalida e mapping specificabili tramite gli attributi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-356">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="dc79e-357">Alcuni attributi quali `MinimumLength` non possono essere applicati con l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="dc79e-357">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="dc79e-358">`MinimumLength` non modifica lo schema, ma si limita ad applicare una regola di convalida per la lunghezza minima.</span><span class="sxs-lookup"><span data-stu-id="dc79e-358">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="dc79e-359">Alcuni sviluppatori preferiscono usare solo l'API Fluent, per dare un aspetto "ordinato" alle classi di entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-359">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="dc79e-360">È possibile combinare gli attributi e l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="dc79e-360">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="dc79e-361">Alcune configurazioni possono essere eseguite solo con l'API Fluent (specificando una chiave primaria composta).</span><span class="sxs-lookup"><span data-stu-id="dc79e-361">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="dc79e-362">Altre configurazioni possono essere eseguite solo con gli attributi (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-362">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="dc79e-363">La procedura consigliata per l'uso dell'API Fluent o degli attributi è la seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-363">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="dc79e-364">Scegliere uno dei due approcci.</span><span class="sxs-lookup"><span data-stu-id="dc79e-364">Choose one of these two approaches.</span></span>
* <span data-ttu-id="dc79e-365">Usare l'approccio scelto con la massima coerenza possibile.</span><span class="sxs-lookup"><span data-stu-id="dc79e-365">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="dc79e-366">Alcuni degli attributi usati in questa esercitazione vengono usati per:</span><span class="sxs-lookup"><span data-stu-id="dc79e-366">Some of the attributes used in this tutorial are used for:</span></span>

* <span data-ttu-id="dc79e-367">Solo convalida (ad esempio `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-367">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="dc79e-368">Solo configurazione di EF Core (ad esempio `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-368">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="dc79e-369">Convalida e configurazione di EF Core (ad esempio `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-369">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="dc79e-370">Per altre informazioni sul confronto tra attributi e API Fluent, vedere [Metodi di configurazione](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="dc79e-370">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram"></a><span data-ttu-id="dc79e-371">Diagramma dell'entità</span><span class="sxs-lookup"><span data-stu-id="dc79e-371">Entity diagram</span></span>

<span data-ttu-id="dc79e-372">La figura seguente visualizza il diagramma creato da EF Power Tools per il modello School completato.</span><span class="sxs-lookup"><span data-stu-id="dc79e-372">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagramma dell'entità](complex-data-model/_static/diagram.png)

<span data-ttu-id="dc79e-374">Il diagramma precedente visualizza quanto segue:</span><span class="sxs-lookup"><span data-stu-id="dc79e-374">The preceding diagram shows:</span></span>

* <span data-ttu-id="dc79e-375">Più linee di relazione uno-a-molti (da 1 a \*).</span><span class="sxs-lookup"><span data-stu-id="dc79e-375">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="dc79e-376">La linea di relazione uno-a-zero-o-uno (da 1 a 0..1) tra le entità `Instructor` e `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-376">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="dc79e-377">La linea di relazione zero-o-uno-a-molti (da 0..1 a \*) tra le entità `Instructor` e `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-377">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="dc79e-378">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="dc79e-378">Seed the database</span></span>

<span data-ttu-id="dc79e-379">Aggiornare il codice in *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="dc79e-379">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

<span data-ttu-id="dc79e-380">Il codice precedente offre i dati di inizializzazione per le nuove entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-380">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="dc79e-381">La maggior parte di questo codice crea nuovi oggetti entità e carica dati di esempio.</span><span class="sxs-lookup"><span data-stu-id="dc79e-381">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="dc79e-382">I dati di esempio vengono usati per i test.</span><span class="sxs-lookup"><span data-stu-id="dc79e-382">The sample data is used for testing.</span></span> <span data-ttu-id="dc79e-383">Visualizzare `Enrollments` e `CourseAssignments` per alcuni esempi del modo in cui può essere impostato il valore di inizializzazione per le tabelle join molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-383">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="dc79e-384">Aggiungere una migrazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-384">Add a migration</span></span>

<span data-ttu-id="dc79e-385">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="dc79e-385">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc79e-386">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc79e-386">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc79e-387">Nella console di Gestione pacchetti eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-387">In PMC, run the following command.</span></span>

```powershell
Add-Migration ComplexDataModel
```

<span data-ttu-id="dc79e-388">Il comando precedente visualizza un avviso sulla possibile perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="dc79e-388">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="dc79e-389">Se viene eseguito il comando `database update`, viene generato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-389">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="dc79e-390">Nella prossima sezione viene descritto come procedere per questo errore.</span><span class="sxs-lookup"><span data-stu-id="dc79e-390">In the next section, you see what to do about this error.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc79e-391">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc79e-391">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dc79e-392">Se si aggiunge una migrazione e si esegue il comando `database update`, viene generato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-392">If you add a migration and run the `database update` command, the following error would be produced:</span></span>

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

<span data-ttu-id="dc79e-393">Nella sezione successiva si vedrà come evitare questo errore.</span><span class="sxs-lookup"><span data-stu-id="dc79e-393">In the next section, you see how to avoid this error.</span></span>

---

## <a name="apply-the-migration-or-drop-and-re-create"></a><span data-ttu-id="dc79e-394">Applicare la migrazione o eliminare e ricreare</span><span class="sxs-lookup"><span data-stu-id="dc79e-394">Apply the migration or drop and re-create</span></span>

<span data-ttu-id="dc79e-395">Ora che è disponibile un database esistente, è necessario preoccuparsi di come applicare eventuali modifiche.</span><span class="sxs-lookup"><span data-stu-id="dc79e-395">Now that you have an existing database, you need to think about how to apply changes to it.</span></span> <span data-ttu-id="dc79e-396">Questa esercitazione illustra due alternative:</span><span class="sxs-lookup"><span data-stu-id="dc79e-396">This tutorial shows two alternatives:</span></span>

* <span data-ttu-id="dc79e-397">[Eliminare e ricreare il database](#drop).</span><span class="sxs-lookup"><span data-stu-id="dc79e-397">[Drop and re-create the database](#drop).</span></span> <span data-ttu-id="dc79e-398">Scegliere questa sezione se si usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="dc79e-398">Choose this section if you're using SQLite.</span></span>
* <span data-ttu-id="dc79e-399">[Applicare la migrazione al database esistente](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="dc79e-399">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="dc79e-400">Le istruzioni riportate in questa sezione funzionano solo per SQL Server e **non per SQLite**.</span><span class="sxs-lookup"><span data-stu-id="dc79e-400">The instructions in this section work for SQL Server only, **not for SQLite**.</span></span> 

<span data-ttu-id="dc79e-401">Entrambe le scelte funzionano per SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dc79e-401">Either choice works for SQL Server.</span></span> <span data-ttu-id="dc79e-402">Anche se il metodo che prevede l'applicazione della migrazione è più complesso e richiede più tempo, si tratta dell'approccio consigliato per gli ambienti di produzione reali.</span><span class="sxs-lookup"><span data-stu-id="dc79e-402">While the apply-migration method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a><span data-ttu-id="dc79e-403">Eliminare e ricreare il database</span><span class="sxs-lookup"><span data-stu-id="dc79e-403">Drop and re-create the database</span></span>

<span data-ttu-id="dc79e-404">[Ignorare questa sezione](#apply-the-migration) se si usa SQL Server e si vuole adottare l'approccio con applicazione della migrazione nella sezione seguente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-404">[Skip this section](#apply-the-migration) if you're using SQL Server and want to do the apply-migration approach in the following section.</span></span>

<span data-ttu-id="dc79e-405">Per forzare la creazione di un nuovo database da parte di EF Core, rimuovere e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="dc79e-405">To force EF Core to create a new database, drop and update the database:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc79e-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc79e-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dc79e-407">Nella **console di Gestione pacchetti** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-407">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Drop-Database
  ```

* <span data-ttu-id="dc79e-408">Eliminare la cartella *Migrations* e quindi eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-408">Delete the *Migrations* folder, then run the following command:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc79e-409">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc79e-409">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dc79e-410">Aprire una finestra di comando e passare alla cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="dc79e-410">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="dc79e-411">La cartella del progetto contiene il file *ContosoUniversity.csproj*.</span><span class="sxs-lookup"><span data-stu-id="dc79e-411">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="dc79e-412">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-412">Run the following command:</span></span>

  ```console
  dotnet ef database drop --force
  ```

* <span data-ttu-id="dc79e-413">Eliminare la cartella *Migrations* e quindi eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-413">Delete the *Migrations* folder, then run the following command:</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

<span data-ttu-id="dc79e-414">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="dc79e-414">Run the app.</span></span> <span data-ttu-id="dc79e-415">Quando si esegue l'app viene eseguito il metodo `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="dc79e-416">`DbInitializer.Initialize` popola il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-416">The `DbInitializer.Initialize` populates the new database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc79e-417">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc79e-417">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc79e-418">Aprire il database in SSOX:</span><span class="sxs-lookup"><span data-stu-id="dc79e-418">Open the database in SSOX:</span></span>

* <span data-ttu-id="dc79e-419">Se SSOX è stato aperto in precedenza, fare clic sul pulsante **Aggiorna**.</span><span class="sxs-lookup"><span data-stu-id="dc79e-419">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="dc79e-420">Espandere il nodo **Tabelle**.</span><span class="sxs-lookup"><span data-stu-id="dc79e-420">Expand the **Tables** node.</span></span> <span data-ttu-id="dc79e-421">Vengono visualizzate le tabelle create.</span><span class="sxs-lookup"><span data-stu-id="dc79e-421">The created tables are displayed.</span></span>

  ![Tabelle in Esplora oggetti di SQL Server](complex-data-model/_static/ssox-tables.png)

* <span data-ttu-id="dc79e-423">Esaminare la tabella **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="dc79e-423">Examine the **CourseAssignment** table:</span></span>

  * <span data-ttu-id="dc79e-424">Fare clic con il pulsante destro del mouse sulla tabella **CourseAssignment** e selezionare **Visualizza dati**.</span><span class="sxs-lookup"><span data-stu-id="dc79e-424">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
  * <span data-ttu-id="dc79e-425">Verificare che la tabella **CourseAssignment** contenga dati.</span><span class="sxs-lookup"><span data-stu-id="dc79e-425">Verify the **CourseAssignment** table contains data.</span></span>

  ![Dati CourseAssignment in SSOX](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc79e-427">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc79e-427">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dc79e-428">Usare lo strumento SQLite per esaminare il database:</span><span class="sxs-lookup"><span data-stu-id="dc79e-428">Use your SQLite tool to examine the database:</span></span>

* <span data-ttu-id="dc79e-429">Nuove tabelle e colonne.</span><span class="sxs-lookup"><span data-stu-id="dc79e-429">New tables and columns.</span></span>
* <span data-ttu-id="dc79e-430">Dati definiti nelle tabelle, ad esempio la tabella **CourseAssignment**.</span><span class="sxs-lookup"><span data-stu-id="dc79e-430">Seeded data in tables, for example the **CourseAssignment** table.</span></span>

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a><span data-ttu-id="dc79e-431">Applicare la migrazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-431">Apply the migration</span></span>

<span data-ttu-id="dc79e-432">Questa sezione è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="dc79e-432">This section is optional.</span></span> <span data-ttu-id="dc79e-433">Questa procedura funziona solo per SQL Server Local DB e solo se è stata ignorata la sezione [Eliminare e ricreare il database](#drop) precedente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-433">These steps work only for SQL Server LocalDB and only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="dc79e-434">Quando le migrazioni vengono eseguite con dati esistenti, possono essere presenti vincoli di chiave esterna che non vengono soddisfatti con i dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="dc79e-435">Con i dati di produzione, è necessario eseguire passaggi per la migrazione dei dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="dc79e-436">Questa sezione visualizza un esempio di correzione delle violazioni dei vincoli di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="dc79e-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="dc79e-437">Non apportare queste modifiche al codice senza un backup.</span><span class="sxs-lookup"><span data-stu-id="dc79e-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="dc79e-438">Non apportare queste modifiche al codice se è stata completata la sezione precedente [Eliminare e ricreare il database](#drop).</span><span class="sxs-lookup"><span data-stu-id="dc79e-438">Don't make these code changes if you completed the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="dc79e-439">Il file *{timestamp}_ComplexDataModel.cs* contiene il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="dc79e-440">Il codice precedente aggiunge una chiave esterna non nullable `DepartmentID` alla tabella `Course`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="dc79e-441">Il database dell'esercitazione precedente contiene righe in `Course`, pertanto la tabella non può essere aggiornata mediante le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="dc79e-441">The database from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="dc79e-442">Per far sì che la migrazione `ComplexDataModel` funzioni con i dati esistenti:</span><span class="sxs-lookup"><span data-stu-id="dc79e-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="dc79e-443">Modificare il codice per assegnare un valore predefinito alla nuova colonna (`DepartmentID`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="dc79e-444">Creare un reparto fittizio denominato "Temp" che assume il ruolo di reparto predefinito.</span><span class="sxs-lookup"><span data-stu-id="dc79e-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="dc79e-445">Risolvere i vincoli della chiave esterna</span><span class="sxs-lookup"><span data-stu-id="dc79e-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="dc79e-446">Nella classe della migrazione `ComplexDataModel` aggiornare il metodo `Up`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-446">In the `ComplexDataModel` migration class, update the `Up` method:</span></span>

* <span data-ttu-id="dc79e-447">Aprire il file *{timestamp}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc79e-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="dc79e-448">Impostare come commento la riga di codice che aggiunge la colonna `DepartmentID` alla tabella `Course`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="dc79e-449">Aggiungere il codice evidenziato seguente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-449">Add the following highlighted code.</span></span> <span data-ttu-id="dc79e-450">Il nuovo codice viene inserito dopo il blocco `.CreateTable( name: "Department"`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

<span data-ttu-id="dc79e-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span><span class="sxs-lookup"><span data-stu-id="dc79e-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span></span>

<span data-ttu-id="dc79e-452">Con le modifiche precedenti, le righe `Course` esistenti saranno correlate al dipartimento "Temp" dopo l'esecuzione del metodo `ComplexDataModel.Up`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-452">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel.Up` method runs.</span></span>

<span data-ttu-id="dc79e-453">La modalità di gestione della situazione illustrata di seguito è semplificata per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-453">The way of handling the situation shown here is simplified for this tutorial.</span></span> <span data-ttu-id="dc79e-454">Un'app di produzione:</span><span class="sxs-lookup"><span data-stu-id="dc79e-454">A production app would:</span></span>

* <span data-ttu-id="dc79e-455">Includerà codice o script per l'aggiunta di righe `Department` e righe `Course` correlate alle nuove righe `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-455">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="dc79e-456">Non userà il reparto "Temp" o il valore predefinito per `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-456">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc79e-457">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc79e-457">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dc79e-458">Nella **console di Gestione pacchetti** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-458">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="dc79e-459">Dato che il metodo `DbInitializer.Initialize` è progettato per funzionare solo con un database vuoto, usare SSOX per eliminare tutte le righe nelle tabelle Student e Course.</span><span class="sxs-lookup"><span data-stu-id="dc79e-459">Because the `DbInitializer.Initialize` method is designed to work only with an empty database, use SSOX to delete all the rows in the Student and Course tables.</span></span> <span data-ttu-id="dc79e-460">(L'eliminazione a catena si occuperà della tabella Enrollment.)</span><span class="sxs-lookup"><span data-stu-id="dc79e-460">(Cascade delete will take care of the Enrollment table.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc79e-461">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc79e-461">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dc79e-462">Se si usa SQL Server Local DB con Visual Studio Code, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-462">If you're using SQL Server LocalDB with Visual Studio Code, run the following command:</span></span>

  ```console
  dotnet ef database update
  ```

---

<span data-ttu-id="dc79e-463">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="dc79e-463">Run the app.</span></span> <span data-ttu-id="dc79e-464">Quando si esegue l'app viene eseguito il metodo `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-464">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="dc79e-465">`DbInitializer.Initialize` popola il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-465">The `DbInitializer.Initialize` populates the new database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc79e-466">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="dc79e-466">Next steps</span></span>

<span data-ttu-id="dc79e-467">Nelle due esercitazioni successive viene illustrato come leggere e aggiornare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="dc79e-467">The next two tutorials show how to read and update related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dc79e-468">[Esercitazione precedente](xref:data/ef-rp/migrations)
> [Esercitazione successiva](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="dc79e-468">[Previous tutorial](xref:data/ef-rp/migrations)
[Next tutorial](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="dc79e-469">Nelle esercitazioni precedenti è stato usato un modello di dati semplice costituito da tre entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-469">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="dc79e-470">In questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="dc79e-470">In this tutorial:</span></span>

* <span data-ttu-id="dc79e-471">Vengono aggiunte altre entità e relazioni.</span><span class="sxs-lookup"><span data-stu-id="dc79e-471">More entities and relationships are added.</span></span>
* <span data-ttu-id="dc79e-472">Il modello di dati viene personalizzato specificando regole di formattazione, convalida e mapping del database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-472">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="dc79e-473">Le classi di entità per il modello di dati completato sono visualizzate nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-473">The entity classes for the completed data model are shown in the following illustration:</span></span>

![Diagramma dell'entità](complex-data-model/_static/diagram.png)

<span data-ttu-id="dc79e-475">Se si verificano problemi che non si è in grado di risolvere, scaricare l'[app completa](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="dc79e-475">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="dc79e-476">Personalizzare il modello di dati usando gli attributi</span><span class="sxs-lookup"><span data-stu-id="dc79e-476">Customize the data model with attributes</span></span>

<span data-ttu-id="dc79e-477">In questa sezione il modello di dati viene personalizzato usando gli attributi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-477">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="dc79e-478">Attributo DataType</span><span class="sxs-lookup"><span data-stu-id="dc79e-478">The DataType attribute</span></span>

<span data-ttu-id="dc79e-479">Attualmente le pagine Student (Studente) visualizzano l'ora associata alla data di iscrizione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-479">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="dc79e-480">In genere i campi data visualizzano solo la data e non l'ora.</span><span class="sxs-lookup"><span data-stu-id="dc79e-480">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="dc79e-481">Aggiornare *Models/Student.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-481">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="dc79e-482">L'attributo [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) indica un tipo di dati più specifico rispetto al tipo intrinseco del database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-482">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="dc79e-483">In questo caso deve essere visualizzata solo la data e non la data e l'ora.</span><span class="sxs-lookup"><span data-stu-id="dc79e-483">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="dc79e-484">L'enumerazione [DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) offre molti tipi di dati, ad esempio Date, Time, PhoneNumber, Currency, EmailAddress e così via. L'attributo `DataType` può anche consentire all'app di offrire automaticamente funzionalità specifiche del tipo.</span><span class="sxs-lookup"><span data-stu-id="dc79e-484">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="dc79e-485">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dc79e-485">For example:</span></span>

* <span data-ttu-id="dc79e-486">Il collegamento `mailto:` viene creato automaticamente per `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-486">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="dc79e-487">Il selettore data viene incluso per `DataType.Date` nella maggior parte dei browser.</span><span class="sxs-lookup"><span data-stu-id="dc79e-487">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="dc79e-488">L'attributo `DataType` genera attributi HTML 5 `data-` supportati dai browser HTML 5.</span><span class="sxs-lookup"><span data-stu-id="dc79e-488">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="dc79e-489">Gli attributi `DataType` non garantiscono la convalida.</span><span class="sxs-lookup"><span data-stu-id="dc79e-489">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="dc79e-490">`DataType.Date` non specifica il formato della data visualizzata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-490">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="dc79e-491">Per impostazione predefinita il campo data viene visualizzato in base ai formati predefiniti per il valore [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) del server.</span><span class="sxs-lookup"><span data-stu-id="dc79e-491">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="dc79e-492">L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:</span><span class="sxs-lookup"><span data-stu-id="dc79e-492">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="dc79e-493">L'impostazione `ApplyFormatInEditMode` specifica che la formattazione deve essere applicata anche all'interfaccia utente di modifica.</span><span class="sxs-lookup"><span data-stu-id="dc79e-493">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="dc79e-494">Alcuni campi non devono usare `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-494">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="dc79e-495">Ad esempio il simbolo di valuta in genere non deve essere visualizzato in una casella di testo di modifica.</span><span class="sxs-lookup"><span data-stu-id="dc79e-495">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="dc79e-496">L'attributo `DisplayFormat` può essere usato da solo.</span><span class="sxs-lookup"><span data-stu-id="dc79e-496">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="dc79e-497">In genere l'uso dell'attributo `DataType` con l'attributo `DisplayFormat` è consigliato.</span><span class="sxs-lookup"><span data-stu-id="dc79e-497">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="dc79e-498">L'attributo `DataType` offre la semantica dei dati anziché specificarne il rendering in una schermata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-498">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="dc79e-499">L'attributo `DataType` offre i vantaggi seguenti che non sono disponibili in `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-499">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="dc79e-500">Il browser può abilitare le funzionalità HTML5.</span><span class="sxs-lookup"><span data-stu-id="dc79e-500">The browser can enable HTML5 features.</span></span> <span data-ttu-id="dc79e-501">Ad esempio può visualizzare un controllo di calendario, il simbolo della valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica, alcune istanze di convalida lato client e così via.</span><span class="sxs-lookup"><span data-stu-id="dc79e-501">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="dc79e-502">Per impostazione predefinita, il browser esegue il rendering dei dati usando il formato corretto in base alle impostazioni locali.</span><span class="sxs-lookup"><span data-stu-id="dc79e-502">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="dc79e-503">Per altre informazioni, vedere la [documentazione dell'helper tag \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="dc79e-503">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="dc79e-504">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="dc79e-504">Run the app.</span></span> <span data-ttu-id="dc79e-505">Passare alla pagina Students Index (Indice studenti).</span><span class="sxs-lookup"><span data-stu-id="dc79e-505">Navigate to the Students Index page.</span></span> <span data-ttu-id="dc79e-506">L'ora non viene più visualizzata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-506">Times are no longer displayed.</span></span> <span data-ttu-id="dc79e-507">Ogni visualizzazione che usa il modello `Student` visualizza la data senza l'ora.</span><span class="sxs-lookup"><span data-stu-id="dc79e-507">Every view that uses the `Student` model displays the date without time.</span></span>

![Pagina Students Index (Indice studenti) con date e senza ore](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="dc79e-509">Attributo StringLength</span><span class="sxs-lookup"><span data-stu-id="dc79e-509">The StringLength attribute</span></span>

<span data-ttu-id="dc79e-510">È possibile specificare regole di convalida dei dati e messaggi di errore di convalida usando gli attributi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-510">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="dc79e-511">L'attributo [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) specifica il numero minimo e massimo di caratteri consentiti in un campo dati.</span><span class="sxs-lookup"><span data-stu-id="dc79e-511">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="dc79e-512">L'attributo `StringLength` offre anche la convalida lato client e lato server.</span><span class="sxs-lookup"><span data-stu-id="dc79e-512">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="dc79e-513">Il valore minimo non ha alcun effetto sullo schema del database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-513">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="dc79e-514">Aggiornare il modello `Student` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-514">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="dc79e-515">Il codice precedente limita i nomi a un massimo di 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="dc79e-515">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="dc79e-516">L'attributo `StringLength` non impedisce a un utente di immettere spazi vuoti per un nome.</span><span class="sxs-lookup"><span data-stu-id="dc79e-516">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="dc79e-517">L'attributo [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) viene usato per applicare restrizioni all'input.</span><span class="sxs-lookup"><span data-stu-id="dc79e-517">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="dc79e-518">Ad esempio il codice seguente richiede che il primo carattere sia maiuscolo e i caratteri rimanenti siano caratteri alfabetici:</span><span class="sxs-lookup"><span data-stu-id="dc79e-518">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="dc79e-519">Eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="dc79e-519">Run the app:</span></span>

* <span data-ttu-id="dc79e-520">Passare alla pagina Student (Studente).</span><span class="sxs-lookup"><span data-stu-id="dc79e-520">Navigate to the Students page.</span></span>
* <span data-ttu-id="dc79e-521">Selezionare **Crea nuovo** e immettere un nome di lunghezza superiore a 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="dc79e-521">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="dc79e-522">Quando si fa clic su **Crea** la convalida lato client visualizza un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="dc79e-522">Select **Create**, client-side validation shows an error message.</span></span>

![Pagina Students Index (Indice studenti) con errori di lunghezza stringa](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="dc79e-524">In **Esplora oggetti di SQL Server** (SSOX) aprire il designer della tabella **Student** (Studente) facendo doppio clic sulla tabella.</span><span class="sxs-lookup"><span data-stu-id="dc79e-524">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabella Student (Studenti) in SSOX prima delle migrazioni](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="dc79e-526">L'immagine precedente visualizza lo schema per la tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-526">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="dc79e-527">I campi nome hanno il tipo `nvarchar(MAX)` perché migrations non è stato eseguito nel database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-527">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="dc79e-528">Quando le istruzioni migrations verranno eseguite, più avanti in questa esercitazione, i campi nome diventeranno `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-528">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="dc79e-529">Attributo Column</span><span class="sxs-lookup"><span data-stu-id="dc79e-529">The Column attribute</span></span>

<span data-ttu-id="dc79e-530">Gli attributi possono controllare il mapping delle classi e delle proprietà nel database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-530">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="dc79e-531">In questa sezione l'attributo `Column` viene usato per il mapping del nome della proprietà `FirstMidName` su "FirstName" nel database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-531">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="dc79e-532">Quando viene creato il database, i nomi delle proprietà nel modello vengono usati per i nomi di colonna (tranne quando viene usato l'attributo `Column`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-532">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="dc79e-533">Il modello `Student` usa il nome `FirstMidName` per il campo first-name (Nome) perché il campo potrebbe contenere anche un secondo nome.</span><span class="sxs-lookup"><span data-stu-id="dc79e-533">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="dc79e-534">Aggiornare il file *Student.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-534">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="dc79e-535">Con la modifica precedente, `Student.FirstMidName` nell'app esegue il mapping alla colonna `FirstName` della tabella `Student`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-535">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="dc79e-536">L'aggiunta dell'attributo `Column` modifica il modello che supporta `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-536">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="dc79e-537">Il modello che supporta `SchoolContext` non corrisponde più al database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-537">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="dc79e-538">Se l'app viene eseguita prima di applicare migrations, viene generata l'eccezione seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-538">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="dc79e-539">Per aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="dc79e-539">To update the DB:</span></span>

* <span data-ttu-id="dc79e-540">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="dc79e-540">Build the project.</span></span>
* <span data-ttu-id="dc79e-541">Aprire una finestra di comando nella cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="dc79e-541">Open a command window in the project folder.</span></span> <span data-ttu-id="dc79e-542">Immettere i comandi seguenti per creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="dc79e-542">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc79e-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc79e-543">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc79e-544">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc79e-544">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="dc79e-545">Il comando `migrations add ColumnFirstName` genera il messaggio di avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-545">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="dc79e-546">L'avviso viene generato perché i campi nome ora sono limitati a 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="dc79e-546">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="dc79e-547">Se un nome nel database ha più di 50 caratteri, i caratteri dal 51 all'ultimo andranno perduti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-547">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="dc79e-548">Eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="dc79e-548">Test the app.</span></span>

<span data-ttu-id="dc79e-549">Aprire la tabella Student (Studente) in SSOX:</span><span class="sxs-lookup"><span data-stu-id="dc79e-549">Open the Student table in SSOX:</span></span>

![Tabella Students (Studenti) in SSOX dopo le migrazioni](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="dc79e-551">Prima dell'applicazione della migrazione, le colonne del nome erano di tipo [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="dc79e-551">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="dc79e-552">Ora le colonne del nome sono di tipo `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-552">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="dc79e-553">Il nome della colonna è cambiato da `FirstMidName` a `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-553">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="dc79e-554">Nella sezione seguente la compilazione dell'applicazione genera errori del compilatore in alcune fasi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-554">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="dc79e-555">Le istruzioni specificano quando compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-555">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="dc79e-556">Aggiornamento dell'entità Student</span><span class="sxs-lookup"><span data-stu-id="dc79e-556">Student entity update</span></span>

![Entità Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="dc79e-558">Aggiornare *Models/Student.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-558">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="dc79e-559">Attributo Required</span><span class="sxs-lookup"><span data-stu-id="dc79e-559">The Required attribute</span></span>

<span data-ttu-id="dc79e-560">L'attributo `Required` rende obbligatori i campi delle proprietà del nome.</span><span class="sxs-lookup"><span data-stu-id="dc79e-560">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="dc79e-561">L'attributo `Required` non è necessario per i tipi non nullable, ad esempio per i tipi valore (`DateTime`, `int`, `double` e così via).</span><span class="sxs-lookup"><span data-stu-id="dc79e-561">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="dc79e-562">I tipi che non possono essere null vengono considerati automaticamente come campi obbligatori.</span><span class="sxs-lookup"><span data-stu-id="dc79e-562">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="dc79e-563">L'attributo `Required` può essere sostituito con un parametro di lunghezza minima nell'attributo `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-563">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="dc79e-564">Attributo Display</span><span class="sxs-lookup"><span data-stu-id="dc79e-564">The Display attribute</span></span>

<span data-ttu-id="dc79e-565">L'attributo `Display` specifica che la didascalia per le caselle di testo deve essere "First Name" (Nome), "Last Name" (Cognome), "Full Name" (Nome e cognome) ed "Enrollment Date" (Data di iscrizione).</span><span class="sxs-lookup"><span data-stu-id="dc79e-565">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="dc79e-566">Nelle didascalie predefinite le parole non erano divise da nessuno spazio, ad esempio "LastName".</span><span class="sxs-lookup"><span data-stu-id="dc79e-566">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="dc79e-567">Proprietà calcolata FullName</span><span class="sxs-lookup"><span data-stu-id="dc79e-567">The FullName calculated property</span></span>

<span data-ttu-id="dc79e-568">`FullName` è una proprietà calcolata che restituisce un valore creato concatenando altre due proprietà.</span><span class="sxs-lookup"><span data-stu-id="dc79e-568">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="dc79e-569">`FullName` non è impostabile e include solo una funzione di accesso get.</span><span class="sxs-lookup"><span data-stu-id="dc79e-569">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="dc79e-570">Nel database non viene creata una colonna `FullName`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-570">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="dc79e-571">Creare l'entità Instructor</span><span class="sxs-lookup"><span data-stu-id="dc79e-571">Create the Instructor Entity</span></span>

![Entità Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="dc79e-573">Creare *Models/Instructor.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-573">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="dc79e-574">Un'unica riga può ospitare più attributi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-574">Multiple attributes can be on one line.</span></span> <span data-ttu-id="dc79e-575">Gli attributi `HireDate` possono essere scritti come segue:</span><span class="sxs-lookup"><span data-stu-id="dc79e-575">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="dc79e-576">Proprietà di navigazione CourseAssignments e OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="dc79e-576">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="dc79e-577">Le proprietà `CourseAssignments` e `OfficeAssignment` sono proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-577">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="dc79e-578">Un insegnante può tenere un numero qualsiasi di corsi, pertanto `CourseAssignments` è definita come raccolta.</span><span class="sxs-lookup"><span data-stu-id="dc79e-578">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="dc79e-579">Se una proprietà di navigazione contiene più entità:</span><span class="sxs-lookup"><span data-stu-id="dc79e-579">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="dc79e-580">Deve essere un tipo di elenco in cui le voci possono essere aggiunte, eliminate e aggiornate.</span><span class="sxs-lookup"><span data-stu-id="dc79e-580">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="dc79e-581">I tipi di proprietà di navigazione includono:</span><span class="sxs-lookup"><span data-stu-id="dc79e-581">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="dc79e-582">Se è specificato `ICollection<T>`, per impostazione predefinita EF Core crea una raccolta `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-582">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="dc79e-583">L'entità `CourseAssignment` è illustrata nella sezione sulle relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-583">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="dc79e-584">Le regole business di Contoso University specificano che un insegnante non può avere più di un ufficio.</span><span class="sxs-lookup"><span data-stu-id="dc79e-584">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="dc79e-585">La proprietà `OfficeAssignment` contiene un'unica entità `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-585">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="dc79e-586">`OfficeAssignment` è null se non è assegnato nessun ufficio.</span><span class="sxs-lookup"><span data-stu-id="dc79e-586">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="dc79e-587">Creare l'entità OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="dc79e-587">Create the OfficeAssignment entity</span></span>

![Entità OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="dc79e-589">Creare *Models/OfficeAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-589">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="dc79e-590">Attributo Key</span><span class="sxs-lookup"><span data-stu-id="dc79e-590">The Key attribute</span></span>

<span data-ttu-id="dc79e-591">L'attributo `[Key]` viene usato per identificare una proprietà come chiave primaria (PK, Primary Key) quando il nome della proprietà è diverso da classnameID o ID.</span><span class="sxs-lookup"><span data-stu-id="dc79e-591">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="dc79e-592">È una relazione uno-a-zero-o-uno tra le entità `Instructor` e `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-592">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="dc79e-593">L'assegnazione di un ufficio esiste solo in relazione all'insegnante al quale viene assegnato l'ufficio.</span><span class="sxs-lookup"><span data-stu-id="dc79e-593">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="dc79e-594">La chiave primaria `OfficeAssignment` è anche la chiave esterna (FK, Foreign Key) per l'entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-594">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="dc79e-595">EF Core non riconosce automaticamente `InstructorID` come chiave primaria di `OfficeAssignment` perché:</span><span class="sxs-lookup"><span data-stu-id="dc79e-595">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="dc79e-596">`InstructorID` non segue la convenzione di denominazione ID o classnameID.</span><span class="sxs-lookup"><span data-stu-id="dc79e-596">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="dc79e-597">Di conseguenza l'attributo `Key` viene usato per identificare l'entità `InstructorID` come chiave primaria:</span><span class="sxs-lookup"><span data-stu-id="dc79e-597">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="dc79e-598">Per impostazione predefinita EF Core considera la chiave come non generata dal database, perché la colonna è destinata a una relazione di identificazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-598">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="dc79e-599">Proprietà di navigazione Instructor</span><span class="sxs-lookup"><span data-stu-id="dc79e-599">The Instructor navigation property</span></span>

<span data-ttu-id="dc79e-600">La proprietà di navigazione `OfficeAssignment` per l'entità `Instructor` è nullable perché:</span><span class="sxs-lookup"><span data-stu-id="dc79e-600">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="dc79e-601">I tipi di riferimento (ad esempio le classi) sono nullable.</span><span class="sxs-lookup"><span data-stu-id="dc79e-601">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="dc79e-602">Un insegnante potrebbe non avere un ufficio assegnato.</span><span class="sxs-lookup"><span data-stu-id="dc79e-602">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="dc79e-603">L'entità `OfficeAssignment` ha una proprietà di navigazione `Instructor` non nullable perché:</span><span class="sxs-lookup"><span data-stu-id="dc79e-603">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="dc79e-604">`InstructorID` è non nullable.</span><span class="sxs-lookup"><span data-stu-id="dc79e-604">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="dc79e-605">Un'assegnazione di ufficio non può esistere senza un insegnante.</span><span class="sxs-lookup"><span data-stu-id="dc79e-605">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="dc79e-606">Quando un'entità `Instructor` dispone di un'entità `OfficeAssignment` correlata, ogni entità include un riferimento all'altra entità nella relativa proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-606">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="dc79e-607">L'attributo `[Required]` può essere applicato alla proprietà di navigazione `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-607">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="dc79e-608">Il codice precedente specifica che deve essere presente un insegnante correlato.</span><span class="sxs-lookup"><span data-stu-id="dc79e-608">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="dc79e-609">Il codice precedente non è necessario perché la chiave esterna `InstructorID` (che è anche la chiave primaria) è non nullable.</span><span class="sxs-lookup"><span data-stu-id="dc79e-609">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="dc79e-610">Modificare l'entità Course</span><span class="sxs-lookup"><span data-stu-id="dc79e-610">Modify the Course Entity</span></span>

![Entità Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="dc79e-612">Aggiornare *Models/Course.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-612">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="dc79e-613">L'entità `Course` dispone di una proprietà chiave esterna (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-613">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="dc79e-614">`DepartmentID` fa riferimento all'entità `Department` correlata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-614">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="dc79e-615">L'entità `Course` dispone di una proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-615">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="dc79e-616">EF Core non richiede una proprietà chiave esterna per un modello di dati quando il modello dispone di una proprietà di navigazione per un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-616">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="dc79e-617">EF Core crea automaticamente le chiavi esterne nel database quando sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="dc79e-617">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="dc79e-618">EF Core crea [proprietà nascoste](/ef/core/modeling/shadow-properties) per le chiavi esterne create automaticamente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-618">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="dc79e-619">Il fatto di avere la chiave esterna nel modello di dati può rendere più semplici ed efficienti gli aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-619">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="dc79e-620">Si consideri ad esempio un modello in cui la proprietà chiave esterna `DepartmentID` *non* è inclusa.</span><span class="sxs-lookup"><span data-stu-id="dc79e-620">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="dc79e-621">Quando un'entità Course viene recuperata per la modifica:</span><span class="sxs-lookup"><span data-stu-id="dc79e-621">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="dc79e-622">L'entità `Department` è null se non viene caricata in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="dc79e-622">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="dc79e-623">Per aggiornare l'entità Course, è in primo luogo necessario recuperare l'entità `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-623">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="dc79e-624">Quando la proprietà chiave esterna `DepartmentID` è inclusa nel modello di dati, non è necessario recuperare l'entità `Department` prima di un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="dc79e-624">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="dc79e-625">Attributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="dc79e-625">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="dc79e-626">L'attributo `[DatabaseGenerated(DatabaseGeneratedOption.None)]` indica che la chiave primaria viene resa disponibile dall'applicazione anziché essere generata dal database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-626">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="dc79e-627">Per impostazione predefinita, Core EF presuppone che i valori di chiave primaria vengano generati dal database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-627">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="dc79e-628">La generazione dei valori di chiave primaria nel database è in genere l'approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="dc79e-628">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="dc79e-629">Per le entità `Course` la chiave primaria viene specificata dall'utente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-629">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="dc79e-630">Un esempio può essere un numero di corso, quale la serie 1000 per il reparto di matematica o la serie 2000 per il reparto di lingua inglese.</span><span class="sxs-lookup"><span data-stu-id="dc79e-630">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="dc79e-631">L'attributo `DatabaseGenerated` può essere usato anche per generare valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-631">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="dc79e-632">Ad esempio il database può generare automaticamente un campo data per registrare la data di creazione o aggiornamento di una riga.</span><span class="sxs-lookup"><span data-stu-id="dc79e-632">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="dc79e-633">Per altre informazioni, vedere [Generated Properties](/ef/core/modeling/generated-properties) (Proprietà generate).</span><span class="sxs-lookup"><span data-stu-id="dc79e-633">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="dc79e-634">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-634">Foreign key and navigation properties</span></span>

<span data-ttu-id="dc79e-635">Le proprietà chiave esterna (FK) e le proprietà di navigazione nell'entità `Course` riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc79e-635">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="dc79e-636">Un corso viene assegnato a un solo reparto, pertanto è presente una chiave esterna `DepartmentID` e una proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-636">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="dc79e-637">Un corso può avere un numero qualsiasi di studenti iscritti, pertanto la proprietà di navigazione `Enrollments` è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="dc79e-637">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="dc79e-638">Un corso può essere impartito da più insegnanti, pertanto la proprietà di navigazione `CourseAssignments` è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="dc79e-638">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="dc79e-639">`CourseAssignment` viene illustrato [più avanti](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="dc79e-639">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="dc79e-640">Creare l'entità Department</span><span class="sxs-lookup"><span data-stu-id="dc79e-640">Create the Department entity</span></span>

![Entità Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="dc79e-642">Creare *Models/Department.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-642">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="dc79e-643">Attributo Column</span><span class="sxs-lookup"><span data-stu-id="dc79e-643">The Column attribute</span></span>

<span data-ttu-id="dc79e-644">In precedenza l'attributo `Column` è stato usato per modificare il mapping del nome di colonna.</span><span class="sxs-lookup"><span data-stu-id="dc79e-644">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="dc79e-645">Nel codice dell'entità `Department` l'attributo `Column` viene usato per modificare il mapping dei tipi di dati SQL.</span><span class="sxs-lookup"><span data-stu-id="dc79e-645">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="dc79e-646">La colonna `Budget` viene definita usando il tipo SQL Server money nel database:</span><span class="sxs-lookup"><span data-stu-id="dc79e-646">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="dc79e-647">In genere il mapping di colonne non è necessario.</span><span class="sxs-lookup"><span data-stu-id="dc79e-647">Column mapping is generally not required.</span></span> <span data-ttu-id="dc79e-648">Generalmente EF Core sceglie il tipo di dati SQL Server appropriato in base al tipo CLR della proprietà.</span><span class="sxs-lookup"><span data-stu-id="dc79e-648">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="dc79e-649">Il tipo CLR `decimal` esegue il mapping a un tipo SQL Server `decimal`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-649">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="dc79e-650">`Budget` è associato alla valuta e il tipo di dati money è più adatto per la valuta.</span><span class="sxs-lookup"><span data-stu-id="dc79e-650">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="dc79e-651">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-651">Foreign key and navigation properties</span></span>

<span data-ttu-id="dc79e-652">Le proprietà chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc79e-652">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="dc79e-653">Un reparto può avere o non avere un amministratore.</span><span class="sxs-lookup"><span data-stu-id="dc79e-653">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="dc79e-654">Un amministratore è sempre un insegnante.</span><span class="sxs-lookup"><span data-stu-id="dc79e-654">An administrator is always an instructor.</span></span> <span data-ttu-id="dc79e-655">Di conseguenza la proprietà `InstructorID` è inclusa come chiave esterna per l'entità `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-655">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="dc79e-656">La proprietà di navigazione è denominata `Administrator` ma contiene un'entità `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-656">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="dc79e-657">Il punto interrogativo (?) nel codice precedente specifica che la proprietà è nullable.</span><span class="sxs-lookup"><span data-stu-id="dc79e-657">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="dc79e-658">Un reparto può avere molti corsi, pertanto è disponibile una proprietà di navigazione Courses:</span><span class="sxs-lookup"><span data-stu-id="dc79e-658">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="dc79e-659">Nota: per convenzione, EF Core abilita l'eliminazione a catena per le chiavi esterne non nullable e per le relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-659">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="dc79e-660">L'eliminazione a catena può generare regole di eliminazione a catena circolari.</span><span class="sxs-lookup"><span data-stu-id="dc79e-660">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="dc79e-661">Quando viene aggiunta una migrazione, le regole di eliminazione a catena circolari determinano un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-661">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="dc79e-662">Ad esempio, se la proprietà `Department.InstructorID` è stata definita come nullable:</span><span class="sxs-lookup"><span data-stu-id="dc79e-662">For example, if the `Department.InstructorID` property was defined as non-nullable:</span></span>

* <span data-ttu-id="dc79e-663">EF Core configura una regola di eliminazione a catena per eliminare il reparto quando viene eliminato l'insegnante.</span><span class="sxs-lookup"><span data-stu-id="dc79e-663">EF Core configures a cascade delete rule to delete the department when the instructor is deleted.</span></span>
* <span data-ttu-id="dc79e-664">L'eliminazione del reparto quando viene eliminato l'insegnante non è il comportamento previsto.</span><span class="sxs-lookup"><span data-stu-id="dc79e-664">Deleting the department when the instructor is deleted isn't the intended behavior.</span></span>
* <span data-ttu-id="dc79e-665">L'API Fluent seguente potrebbe impostare una regola restrict anziché cascade.</span><span class="sxs-lookup"><span data-stu-id="dc79e-665">The following fluent API would set a restrict rule instead of cascade.</span></span>

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

<span data-ttu-id="dc79e-666">Il codice precedente disabilita l'eliminazione a catena per la relazione reparto-insegnante.</span><span class="sxs-lookup"><span data-stu-id="dc79e-666">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="dc79e-667">Aggiornare l'entità Enrollment</span><span class="sxs-lookup"><span data-stu-id="dc79e-667">Update the Enrollment entity</span></span>

<span data-ttu-id="dc79e-668">Un record di iscrizione è relativo a un solo corso frequentato da un solo studente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-668">An enrollment record is for one course taken by one student.</span></span>

![Entità Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="dc79e-670">Aggiornare *Models/Enrollment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-670">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="dc79e-671">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-671">Foreign key and navigation properties</span></span>

<span data-ttu-id="dc79e-672">Le proprietà chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc79e-672">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="dc79e-673">Un record di iscrizione è relativo a un solo corso, pertanto sono presenti una proprietà chiave esterna `CourseID` e una proprietà di navigazione `Course`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-673">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="dc79e-674">Un record di iscrizione è relativo a un solo studente, pertanto sono presenti una proprietà chiave esterna `StudentID` e una proprietà di navigazione `Student`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-674">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="dc79e-675">Relazioni molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="dc79e-675">Many-to-Many Relationships</span></span>

<span data-ttu-id="dc79e-676">Esiste una relazione molti-a-molti tra le entità `Student` e `Course`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-676">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="dc79e-677">L'entità `Enrollment` funziona come una tabella di join molti-a-molti *con payload* nel database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-677">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="dc79e-678">"Con payload" significa che la tabella `Enrollment` contiene dati aggiuntivi oltre alle chiavi esterne delle tabelle di join (in questo caso la chiave primaria e `Grade`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-678">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="dc79e-679">La figura seguente illustra l'aspetto di queste relazioni in un diagramma di entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-679">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="dc79e-680">Il diagramma è stato generato con [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) per EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="dc79e-680">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="dc79e-681">La creazione del diagramma non fa parte dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-681">Creating the diagram isn't part of the tutorial.)</span></span>

![Relazione molti-a-molti Student-Course (Studente-Corso)](complex-data-model/_static/student-course.png)

<span data-ttu-id="dc79e-683">Ogni riga della relazione inizia con un 1 e termina con un asterisco (\*), per indicare una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-683">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="dc79e-684">Se la tabella `Enrollment` non include informazioni sul livello, è sufficiente che contenga le due chiavi esterne `CourseID` e `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-684">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="dc79e-685">Una tabella di join molti-a-molti senza payload è anche detta tabella di join pura (PJT, Pure Join Table).</span><span class="sxs-lookup"><span data-stu-id="dc79e-685">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="dc79e-686">Le entità `Instructor` e `Course` hanno una relazione molti-a-molti con una tabella di join pura.</span><span class="sxs-lookup"><span data-stu-id="dc79e-686">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="dc79e-687">Nota: Le tabelle di join implicite per le relazioni molti-a-molti sono supportate in EF 6.x, ma non in EF Core.</span><span class="sxs-lookup"><span data-stu-id="dc79e-687">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="dc79e-688">Per altre informazioni, vedere [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (Relazioni molti-a-molti in EF Core 2.0).</span><span class="sxs-lookup"><span data-stu-id="dc79e-688">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="dc79e-689">Entità CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="dc79e-689">The CourseAssignment entity</span></span>

![Entità CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="dc79e-691">Creare *Models/CourseAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-691">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="dc79e-692">Instructor-to-Courses</span><span class="sxs-lookup"><span data-stu-id="dc79e-692">Instructor-to-Courses</span></span>

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="dc79e-694">La relazione molti-a-molti Instructor-to-Courses (Insegnante-Corsi):</span><span class="sxs-lookup"><span data-stu-id="dc79e-694">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="dc79e-695">Richiede una tabella di join che deve essere rappresentata da un set di entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-695">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="dc79e-696">È una tabella di join pura (tabella senza payload).</span><span class="sxs-lookup"><span data-stu-id="dc79e-696">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="dc79e-697">È pratica comune assegnare a un'entità di join un nome `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-697">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="dc79e-698">Ad esempio la tabella di join Instructor-to-Courses che usa questa convenzione sarà `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-698">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="dc79e-699">È tuttavia consigliabile usare un nome che descrive la relazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-699">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="dc79e-700">I modelli di dati iniziano come strutture semplici, quindi le loro dimensioni aumentano.</span><span class="sxs-lookup"><span data-stu-id="dc79e-700">Data models start out simple and grow.</span></span> <span data-ttu-id="dc79e-701">In molti casi ai join senza payload vengono assegnati payload in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="dc79e-701">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="dc79e-702">Se si assegna inizialmente un nome di entità descrittivo, non sarà necessario modificarlo quando la tabella di join cambia.</span><span class="sxs-lookup"><span data-stu-id="dc79e-702">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="dc79e-703">Idealmente l'entità di join dovrebbe avere il proprio nome naturale (se possibile composto da un'unica parola) nel dominio di business.</span><span class="sxs-lookup"><span data-stu-id="dc79e-703">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="dc79e-704">Ad esempio Books (Documentazione) e Customers (Clienti) potrebbero essere collegati mediante un'entità di join Ratings (Valutazioni).</span><span class="sxs-lookup"><span data-stu-id="dc79e-704">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="dc79e-705">Per la relazione molti-a-molti Instructor-to-Courses `CourseAssignment` è preferibile a `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-705">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="dc79e-706">Chiave composta</span><span class="sxs-lookup"><span data-stu-id="dc79e-706">Composite key</span></span>

<span data-ttu-id="dc79e-707">Le chiavi esterne non sono nullable.</span><span class="sxs-lookup"><span data-stu-id="dc79e-707">FKs are not nullable.</span></span> <span data-ttu-id="dc79e-708">Le due chiavi esterne in `CourseAssignment` (`InstructorID` e `CourseID`) identificano insieme in modo univoco ogni riga della tabella `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-708">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="dc79e-709">`CourseAssignment` non richiede una chiave primaria dedicata.</span><span class="sxs-lookup"><span data-stu-id="dc79e-709">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="dc79e-710">Le proprietà `InstructorID` e `CourseID` funzionano come una chiave primaria composta.</span><span class="sxs-lookup"><span data-stu-id="dc79e-710">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="dc79e-711">L'unico modo per specificare chiavi primarie composte in EF Core è l'*API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="dc79e-711">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="dc79e-712">La sezione successiva illustra come configurare la chiave primaria composta.</span><span class="sxs-lookup"><span data-stu-id="dc79e-712">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="dc79e-713">La chiave composta garantisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="dc79e-713">The composite key ensures:</span></span>

* <span data-ttu-id="dc79e-714">Sono consentite più righe per un corso.</span><span class="sxs-lookup"><span data-stu-id="dc79e-714">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="dc79e-715">Sono consentite più righe per un insegnante.</span><span class="sxs-lookup"><span data-stu-id="dc79e-715">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="dc79e-716">Non sono consentite più righe per lo stesso insegnante e lo stesso corso.</span><span class="sxs-lookup"><span data-stu-id="dc79e-716">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="dc79e-717">L'entità di join `Enrollment` definisce la propria chiave primaria, pertanto sono possibili i duplicati di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="dc79e-717">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="dc79e-718">Per evitare tali duplicati:</span><span class="sxs-lookup"><span data-stu-id="dc79e-718">To prevent such duplicates:</span></span>

* <span data-ttu-id="dc79e-719">Aggiungere un indice univoco ai campi chiave esterna oppure</span><span class="sxs-lookup"><span data-stu-id="dc79e-719">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="dc79e-720">Configurare `Enrollment` con una chiave primaria composta simile a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-720">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="dc79e-721">Per altre informazioni, vedere [Indexes](/ef/core/modeling/indexes) (Indici).</span><span class="sxs-lookup"><span data-stu-id="dc79e-721">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="dc79e-722">Aggiornare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="dc79e-722">Update the DB context</span></span>

<span data-ttu-id="dc79e-723">Aggiungere il codice evidenziato seguente al file *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="dc79e-723">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="dc79e-724">Questo codice aggiunge le nuove entità e configura la chiave primaria composta dell'entità `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-724">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="dc79e-725">Alternativa API Fluent agli attributi</span><span class="sxs-lookup"><span data-stu-id="dc79e-725">Fluent API alternative to attributes</span></span>

<span data-ttu-id="dc79e-726">Il metodo `OnModelCreating` nel codice precedente usa l'*API Fluent* per configurare il comportamento di EF Core.</span><span class="sxs-lookup"><span data-stu-id="dc79e-726">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="dc79e-727">L'API è denominata "API Fluent" perché viene spesso usata unendo una serie di chiamate di metodi in un'unica istruzione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-727">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="dc79e-728">Il [codice seguente](/ef/core/modeling/#use-fluent-api-to-configure-a-model) è un esempio di API Fluent:</span><span class="sxs-lookup"><span data-stu-id="dc79e-728">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="dc79e-729">In questa esercitazione l'API Fluent viene usata solo per le operazioni di mapping del database che non possono essere eseguite con gli attributi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-729">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="dc79e-730">Tuttavia l'API Fluent può specificare la maggior parte delle regole di formattazione, convalida e mapping specificabili tramite gli attributi.</span><span class="sxs-lookup"><span data-stu-id="dc79e-730">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="dc79e-731">Alcuni attributi quali `MinimumLength` non possono essere applicati con l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="dc79e-731">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="dc79e-732">`MinimumLength` non modifica lo schema, ma si limita ad applicare una regola di convalida per la lunghezza minima.</span><span class="sxs-lookup"><span data-stu-id="dc79e-732">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="dc79e-733">Alcuni sviluppatori preferiscono usare solo l'API Fluent, per dare un aspetto "ordinato" alle classi di entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-733">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="dc79e-734">È possibile combinare gli attributi e l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="dc79e-734">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="dc79e-735">Alcune configurazioni possono essere eseguite solo con l'API Fluent (specificando una chiave primaria composta).</span><span class="sxs-lookup"><span data-stu-id="dc79e-735">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="dc79e-736">Altre configurazioni possono essere eseguite solo con gli attributi (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-736">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="dc79e-737">La procedura consigliata per l'uso dell'API Fluent o degli attributi è la seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-737">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="dc79e-738">Scegliere uno dei due approcci.</span><span class="sxs-lookup"><span data-stu-id="dc79e-738">Choose one of these two approaches.</span></span>
* <span data-ttu-id="dc79e-739">Usare l'approccio scelto con la massima coerenza possibile.</span><span class="sxs-lookup"><span data-stu-id="dc79e-739">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="dc79e-740">Alcuni attributi di questa esercitazione vengono usati per:</span><span class="sxs-lookup"><span data-stu-id="dc79e-740">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="dc79e-741">Solo convalida (ad esempio `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-741">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="dc79e-742">Solo configurazione di EF Core (ad esempio `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-742">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="dc79e-743">Convalida e configurazione di EF Core (ad esempio `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-743">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="dc79e-744">Per altre informazioni sul confronto tra attributi e API Fluent, vedere [Metodi di configurazione](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="dc79e-744">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="dc79e-745">Diagramma dell'entità che visualizza le relazioni</span><span class="sxs-lookup"><span data-stu-id="dc79e-745">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="dc79e-746">La figura seguente visualizza il diagramma creato da EF Power Tools per il modello School completato.</span><span class="sxs-lookup"><span data-stu-id="dc79e-746">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagramma dell'entità](complex-data-model/_static/diagram.png)

<span data-ttu-id="dc79e-748">Il diagramma precedente visualizza quanto segue:</span><span class="sxs-lookup"><span data-stu-id="dc79e-748">The preceding diagram shows:</span></span>

* <span data-ttu-id="dc79e-749">Più linee di relazione uno-a-molti (da 1 a \*).</span><span class="sxs-lookup"><span data-stu-id="dc79e-749">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="dc79e-750">La linea di relazione uno-a-zero-o-uno (da 1 a 0..1) tra le entità `Instructor` e `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-750">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="dc79e-751">La linea di relazione zero-o-uno-a-molti (da 0..1 a \*) tra le entità `Instructor` e `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-751">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="dc79e-752">Inizializzare il database con dati di test</span><span class="sxs-lookup"><span data-stu-id="dc79e-752">Seed the DB with Test Data</span></span>

<span data-ttu-id="dc79e-753">Aggiornare il codice in *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="dc79e-753">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="dc79e-754">Il codice precedente offre i dati di inizializzazione per le nuove entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-754">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="dc79e-755">La maggior parte di questo codice crea nuovi oggetti entità e carica dati di esempio.</span><span class="sxs-lookup"><span data-stu-id="dc79e-755">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="dc79e-756">I dati di esempio vengono usati per i test.</span><span class="sxs-lookup"><span data-stu-id="dc79e-756">The sample data is used for testing.</span></span> <span data-ttu-id="dc79e-757">Visualizzare `Enrollments` e `CourseAssignments` per alcuni esempi del modo in cui può essere impostato il valore di inizializzazione per le tabelle join molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-757">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="dc79e-758">Aggiungere una migrazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-758">Add a migration</span></span>

<span data-ttu-id="dc79e-759">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="dc79e-759">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc79e-760">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc79e-760">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc79e-761">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc79e-761">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="dc79e-762">Il comando precedente visualizza un avviso sulla possibile perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="dc79e-762">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="dc79e-763">Se viene eseguito il comando `database update`, viene generato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-763">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="dc79e-764">Applicare la migrazione</span><span class="sxs-lookup"><span data-stu-id="dc79e-764">Apply the migration</span></span>

<span data-ttu-id="dc79e-765">Ora che è disponibile un database esistente, è necessario preoccuparsi di come applicare eventuali modifiche future.</span><span class="sxs-lookup"><span data-stu-id="dc79e-765">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="dc79e-766">Questa esercitazione illustra due approcci:</span><span class="sxs-lookup"><span data-stu-id="dc79e-766">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="dc79e-767">Eliminare e ricreare il database</span><span class="sxs-lookup"><span data-stu-id="dc79e-767">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="dc79e-768">[Applicare la migrazione al database esistente](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="dc79e-768">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="dc79e-769">Anche se questo metodo è più complesso e richiede più tempo, è l'approccio consigliato per gli ambienti di produzione reali.</span><span class="sxs-lookup"><span data-stu-id="dc79e-769">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="dc79e-770">**Nota**: questa è una sezione facoltativa dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-770">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="dc79e-771">È possibile eseguire i passaggi di eliminazione e ricreazione e ignorare questa sezione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-771">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="dc79e-772">Se si vuole seguire la procedura descritta in questa sezione, non eseguire i passaggi di eliminazione e ricreazione.</span><span class="sxs-lookup"><span data-stu-id="dc79e-772">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="dc79e-773">Eliminare e ricreare il database</span><span class="sxs-lookup"><span data-stu-id="dc79e-773">Drop and re-create the database</span></span>

<span data-ttu-id="dc79e-774">Il codice aggiornato in `DbInitializer` aggiunge dati di inizializzazione per le nuove entità.</span><span class="sxs-lookup"><span data-stu-id="dc79e-774">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="dc79e-775">Per forzare la creazione di un nuovo database da parte di EF Core, rimuovere e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="dc79e-775">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc79e-776">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc79e-776">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc79e-777">Nella **console di Gestione pacchetti** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-777">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="dc79e-778">Eseguire `Get-Help about_EntityFrameworkCore` dalla console di Gestione pacchetti per ottenere informazioni.</span><span class="sxs-lookup"><span data-stu-id="dc79e-778">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc79e-779">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dc79e-779">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dc79e-780">Aprire una finestra di comando e passare alla cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="dc79e-780">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="dc79e-781">La cartella del progetto contiene il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc79e-781">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="dc79e-782">Digitare quanto segue nella finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="dc79e-782">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

---

<span data-ttu-id="dc79e-783">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="dc79e-783">Run the app.</span></span> <span data-ttu-id="dc79e-784">Quando si esegue l'app viene eseguito il metodo `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-784">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="dc79e-785">`DbInitializer.Initialize` popola il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="dc79e-785">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="dc79e-786">Aprire il database in SSOX:</span><span class="sxs-lookup"><span data-stu-id="dc79e-786">Open the DB in SSOX:</span></span>

* <span data-ttu-id="dc79e-787">Se SSOX è stato aperto in precedenza, fare clic sul pulsante **Aggiorna**.</span><span class="sxs-lookup"><span data-stu-id="dc79e-787">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="dc79e-788">Espandere il nodo **Tabelle**.</span><span class="sxs-lookup"><span data-stu-id="dc79e-788">Expand the **Tables** node.</span></span> <span data-ttu-id="dc79e-789">Vengono visualizzate le tabelle create.</span><span class="sxs-lookup"><span data-stu-id="dc79e-789">The created tables are displayed.</span></span>

![Tabelle in Esplora oggetti di SQL Server](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="dc79e-791">Esaminare la tabella **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="dc79e-791">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="dc79e-792">Fare clic con il pulsante destro del mouse sulla tabella **CourseAssignment** e selezionare **Visualizza dati**.</span><span class="sxs-lookup"><span data-stu-id="dc79e-792">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="dc79e-793">Verificare che la tabella **CourseAssignment** contenga dati.</span><span class="sxs-lookup"><span data-stu-id="dc79e-793">Verify the **CourseAssignment** table contains data.</span></span>

![Dati CourseAssignment in SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="dc79e-795">Applicare la migrazione al database esistente</span><span class="sxs-lookup"><span data-stu-id="dc79e-795">Apply the migration to the existing database</span></span>

<span data-ttu-id="dc79e-796">Questa sezione è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="dc79e-796">This section is optional.</span></span> <span data-ttu-id="dc79e-797">Questa procedura funziona solo se è stata ignorata la sezione [Eliminare e ricreare il database](#drop) precedente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-797">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="dc79e-798">Quando le migrazioni vengono eseguite con dati esistenti, possono essere presenti vincoli di chiave esterna che non vengono soddisfatti con i dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-798">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="dc79e-799">Con i dati di produzione, è necessario eseguire passaggi per la migrazione dei dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="dc79e-799">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="dc79e-800">Questa sezione visualizza un esempio di correzione delle violazioni dei vincoli di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="dc79e-800">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="dc79e-801">Non apportare queste modifiche al codice senza un backup.</span><span class="sxs-lookup"><span data-stu-id="dc79e-801">Don't make these code changes without a backup.</span></span> <span data-ttu-id="dc79e-802">Non apportare queste modifiche al codice se è stata completata la sezione precedente e il database è stato aggiornato.</span><span class="sxs-lookup"><span data-stu-id="dc79e-802">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="dc79e-803">Il file *{timestamp}_ComplexDataModel.cs* contiene il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="dc79e-803">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="dc79e-804">Il codice precedente aggiunge una chiave esterna non nullable `DepartmentID` alla tabella `Course`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-804">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="dc79e-805">Il database dell'esercitazione precedente contiene righe in `Course`, pertanto la tabella non può essere aggiornata mediante le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="dc79e-805">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="dc79e-806">Per far sì che la migrazione `ComplexDataModel` funzioni con i dati esistenti:</span><span class="sxs-lookup"><span data-stu-id="dc79e-806">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="dc79e-807">Modificare il codice per assegnare un valore predefinito alla nuova colonna (`DepartmentID`).</span><span class="sxs-lookup"><span data-stu-id="dc79e-807">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="dc79e-808">Creare un reparto fittizio denominato "Temp" che assume il ruolo di reparto predefinito.</span><span class="sxs-lookup"><span data-stu-id="dc79e-808">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="dc79e-809">Risolvere i vincoli della chiave esterna</span><span class="sxs-lookup"><span data-stu-id="dc79e-809">Fix the foreign key constraints</span></span>

<span data-ttu-id="dc79e-810">Aggiornare il metodo `Up` delle classi `ComplexDataModel`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-810">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="dc79e-811">Aprire il file *{timestamp}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc79e-811">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="dc79e-812">Impostare come commento la riga di codice che aggiunge la colonna `DepartmentID` alla tabella `Course`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-812">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="dc79e-813">Aggiungere il codice evidenziato seguente.</span><span class="sxs-lookup"><span data-stu-id="dc79e-813">Add the following highlighted code.</span></span> <span data-ttu-id="dc79e-814">Il nuovo codice viene inserito dopo il blocco `.CreateTable( name: "Department"`:</span><span class="sxs-lookup"><span data-stu-id="dc79e-814">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="dc79e-815">Con le modifiche precedenti, le righe `Course` esistenti saranno correlate al reparto "Temp" dopo l'esecuzione del metodo `ComplexDataModel` `Up`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-815">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="dc79e-816">Un'app di produzione:</span><span class="sxs-lookup"><span data-stu-id="dc79e-816">A production app would:</span></span>

* <span data-ttu-id="dc79e-817">Includerà codice o script per l'aggiunta di righe `Department` e righe `Course` correlate alle nuove righe `Department`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-817">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="dc79e-818">Non userà il reparto "Temp" o il valore predefinito per `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="dc79e-818">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="dc79e-819">L'esercitazione successiva illustra i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="dc79e-819">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc79e-820">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="dc79e-820">Additional resources</span></span>

* [<span data-ttu-id="dc79e-821">Versione YouTube dell'esercitazione (parte 1)</span><span class="sxs-lookup"><span data-stu-id="dc79e-821">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="dc79e-822">Versione YouTube dell'esercitazione (parte 2)</span><span class="sxs-lookup"><span data-stu-id="dc79e-822">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)



> [!div class="step-by-step"]
> <span data-ttu-id="dc79e-823">[Precedente](xref:data/ef-rp/migrations)
> [Successivo](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="dc79e-823">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end