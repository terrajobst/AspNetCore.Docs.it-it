---
title: ASP.NET Core MVC con EF Core - Modello di dati - 5 di 10
author: rick-anderson
description: In questa esercitazione si aggiungono altre entità e relazioni e si personalizza il modello di dati specificando regole di formattazione, convalida e mapping.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 1d3c69c8c658b5ca2f0253b790b0dc75d44d3064
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093114"
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a><span data-ttu-id="4cbdb-103">ASP.NET Core MVC con EF Core - Modello di dati - 5 di 10</span><span class="sxs-lookup"><span data-stu-id="4cbdb-103">ASP.NET Core MVC with EF Core - Data Model - 5 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4cbdb-104">Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4cbdb-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4cbdb-105">L'applicazione Web di esempio Contoso University illustra come creare applicazioni Web ASP.NET Core MVC con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="4cbdb-106">Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="4cbdb-107">Nelle esercitazioni precedenti è stato usato un modello di dati semplice costituito da tre entità.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-107">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="4cbdb-108">In questa esercitazione si aggiungeranno altre entità e relazioni e si personalizzerà il modello di dati specificando regole di formattazione, convalida e mapping del database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-108">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="4cbdb-109">Al termine dell'operazione le classi di entità verranno incluse nel modello di dati completato, illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-109">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Diagramma dell'entità](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="4cbdb-111">Personalizzare il modello di dati usando gli attributi</span><span class="sxs-lookup"><span data-stu-id="4cbdb-111">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="4cbdb-112">In questa sezione si apprenderà come personalizzare il modello di dati usando attributi che specificano regole di formattazione, convalida e mapping del database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-112">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="4cbdb-113">Quindi nelle sezioni seguenti si creerà il modello di dati School (Istituto scolastico) completo, aggiungendo attributi alle classi già create e creando nuove classi per i tipi di entità rimanenti nel modello.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-113">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="4cbdb-114">Attributo DataType</span><span class="sxs-lookup"><span data-stu-id="4cbdb-114">The DataType attribute</span></span>

<span data-ttu-id="4cbdb-115">Per le date di iscrizione degli studenti, tutte le pagine Web attualmente visualizzano l'ora oltre alla data, anche se l'unico elemento rilevante di questo campo è la data.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-115">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="4cbdb-116">Mediante gli attributi di annotazione dei dati è possibile modificare il codice per correggere il formato di visualizzazione in tutte le visualizzazioni che visualizzano i dati.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-116">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="4cbdb-117">Per un esempio di come eseguire questa operazione si aggiunge un attributo alla proprietà `EnrollmentDate` nella classe `Student`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-117">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="4cbdb-118">In *Models/Student.cs* aggiungere un'istruzione `using` per lo spazio dei nomi `System.ComponentModel.DataAnnotations` e aggiungere gli attributi `DataType` e `DisplayFormat` alla proprietà `EnrollmentDate` come indicato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-118">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="4cbdb-119">L'attributo `DataType` viene usato per specificare un tipo di dati che è più specifico del tipo intrinseco del database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-119">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="4cbdb-120">In questo caso si vuole tenere traccia solo della data e non di data e ora.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-120">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="4cbdb-121">L'enumerazione `DataType` include molti tipi di dati, ad esempio Date, Time, PhoneNumber, Currency, EmailAddress e così via.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-121">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="4cbdb-122">L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-122">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="4cbdb-123">Ad esempio, è possibile creare un collegamento `mailto:` per `DataType.EmailAddress` e fornire un selettore data per `DataType.Date` nei browser che supportano HTML5.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-123">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="4cbdb-124">L'attributo `DataType` produce attributi HTML5 `data-` supportati dai browser HTML5.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="4cbdb-125">Gli attributi `DataType` non garantiscono alcuna convalida.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-125">The `DataType` attributes don't provide any validation.</span></span>

<span data-ttu-id="4cbdb-126">`DataType.Date` non specifica il formato della data visualizzata.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="4cbdb-127">Per impostazione predefinita il campo dati viene visualizzato in base ai formati predefiniti derivanti dal valore CultureInfo del server.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-127">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="4cbdb-128">L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="4cbdb-129">L'impostazione `ApplyFormatInEditMode` specifica che la formattazione deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="4cbdb-130">Per determinati campi questa scelta non è consigliabile. Ad esempio per i valori di valuta può non risultare opportuno che il simbolo di valuta sia incluso nella casella di testo per la modifica.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-130">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="4cbdb-131">È possibile usare l'attributo `DisplayFormat` da solo, ma in genere è consigliabile usare anche l'attributo `DataType`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-131">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="4cbdb-132">L'attributo `DataType` esprime la semantica dei dati anziché la modalità di rendering dei dati in una schermata e offre i seguenti vantaggi che non si ottengono con `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-132">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="4cbdb-133">Il browser può abilitare le funzionalità HTML5, ad esempio per visualizzare un controllo di calendario, il simbolo della valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica, alcune istanze di convalida lato client e così via.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-133">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="4cbdb-134">Per impostazione predefinita, il browser eseguirà il rendering dei dati usando il formato corretto in base alle impostazioni locali del sistema.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-134">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="4cbdb-135">Per altre informazioni, vedere la [documentazione dell'helper tag \<input>](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-135">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="4cbdb-136">Eseguire l'app, passare alla pagina Students Index (Indice studenti) e verificare che l'ora non viene più visualizzata nelle date di iscrizione.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-136">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="4cbdb-137">Lo stesso vale per tutte le viste che usano il modello Student (Studente).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-137">The same will be true for any view that uses the Student model.</span></span>

![Pagina Students Index (Indice studenti) con date e senza ore](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="4cbdb-139">Attributo StringLength</span><span class="sxs-lookup"><span data-stu-id="4cbdb-139">The StringLength attribute</span></span>

<span data-ttu-id="4cbdb-140">È anche possibile specificare regole di convalida dei dati e messaggi di errore di convalida mediante gli attributi.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-140">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="4cbdb-141">L'attributo `StringLength` imposta la lunghezza massima nel database e offre la convalida lato client e lato server per ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-141">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="4cbdb-142">È anche possibile specificare la lunghezza minima della stringa in questo attributo, ma il valore minimo non ha alcun effetto sullo schema del database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-142">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="4cbdb-143">Ad esempio si supponga di voler limitare a 50 il numero massimo di caratteri che gli utenti possono immettere per un nome.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-143">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="4cbdb-144">Per aggiungere questa limitazione aggiungere attributi `StringLength` alle proprietà `LastName` e `FirstMidName`, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-144">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="4cbdb-145">L'attributo `StringLength` non impedisce a un utente di immettere spazi vuoti per un nome.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-145">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="4cbdb-146">È possibile usare l'attributo `RegularExpression` per applicare restrizioni all'input.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-146">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="4cbdb-147">Ad esempio il codice seguente richiede che il primo carattere sia maiuscolo e i caratteri rimanenti siano caratteri alfabetici:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-147">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="4cbdb-148">L'attributo `MaxLength` offre funzionalità simili a quelle dell'attributo `StringLength` ma non offre la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-148">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="4cbdb-149">La modifica apportata al modello di database ora richiede un aggiornamento dello schema del database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-149">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="4cbdb-150">Si userà migrations per aggiornare lo schema senza perdere i dati eventualmente aggiunti al database tramite l'interfaccia utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-150">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="4cbdb-151">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-151">Save your changes and build the project.</span></span> <span data-ttu-id="4cbdb-152">Quindi aprire una finestra dei comandi nella cartella di progetto ed eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-152">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="4cbdb-153">Il comando `migrations add` segnala che può verificarsi la perdita di dati, perché la modifica riduce la lunghezza massima per due colonne.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-153">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="4cbdb-154">L'istruzione migrations crea un file con nome *\<timestamp>_MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-154">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="4cbdb-155">Il metodo `Up` di questo file contiene codice che aggiorna il database per adattarlo al modello di dati corrente.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-155">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="4cbdb-156">Il comando `database update` ha eseguito tale codice.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-156">The `database update` command ran that code.</span></span>

<span data-ttu-id="4cbdb-157">Il timestamp che precede il nome del file delle migrazioni viene usato da Entity Framework per ordinare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-157">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="4cbdb-158">È possibile creare più migrazioni prima di eseguire il comando update-database, dopodiché tutte le migrazioni vengono applicate nell'ordine in cui sono state create.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-158">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="4cbdb-159">Eseguire l'app, selezionare la scheda **Students** (Studenti), fare clic su **Crea nuovo** e immettere un nome con più di 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-159">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="4cbdb-160">Quando si fa clic su **Crea** la convalida lato client visualizza un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-160">When you click **Create**, client side validation shows an error message.</span></span>

![Pagina Students Index (Indice studenti) con errori di lunghezza stringa](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="4cbdb-162">Attributo Column</span><span class="sxs-lookup"><span data-stu-id="4cbdb-162">The Column attribute</span></span>

<span data-ttu-id="4cbdb-163">È possibile usare gli attributi anche per controllare il mapping delle classi e delle proprietà nel database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-163">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="4cbdb-164">Si supponga di aver usato il nome `FirstMidName` per il campo first-name (Nome) perché il campo potrebbe contenere anche un secondo nome.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-164">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="4cbdb-165">Tuttavia si vuole che la colonna di database sia denominata `FirstName`, perché gli utenti che scrivono query ad hoc per il database sono abituati a tale nome.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-165">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="4cbdb-166">Per eseguire questo mapping è possibile usare l'attributo `Column`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-166">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="4cbdb-167">L'attributo `Column` specifica che quando viene creato il database, la colonna della tabella `Student` mappata sulla proprietà `FirstMidName` verrà denominata `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-167">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="4cbdb-168">In altri termini, quando il codice fa riferimento a `Student.FirstMidName` i dati provengono dalla colonna `FirstName` della tabella `Student` o vengono aggiornati in tale colonna.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-168">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="4cbdb-169">Se non si specificano nomi di colonna, le colonne avranno lo stesso nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-169">If you don't specify column names, they're given the same name as the property name.</span></span>

<span data-ttu-id="4cbdb-170">Nel file *Student.cs* aggiungere un'istruzione `using` per `System.ComponentModel.DataAnnotations.Schema` e aggiungere l'attributo del nome colonna alla proprietà `FirstMidName`, come illustrato nel codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-170">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="4cbdb-171">L'aggiunta dell'attributo `Column` modifica il modello che supporta `SchoolContext` e che pertanto non corrisponderà al database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-171">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="4cbdb-172">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-172">Save your changes and build the project.</span></span> <span data-ttu-id="4cbdb-173">Quindi aprire una finestra dei comandi nella cartella di progetto ed eseguire i comandi seguenti per creare un'altra migrazione:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-173">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="4cbdb-174">In **Esplora oggetti di SQL Server** aprire il designer della tabella **Student** (Studente) facendo doppio clic sulla tabella.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-174">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabella Students (Studenti) in SSOX dopo le migrazioni](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="4cbdb-176">Prima dell'applicazione delle prime due migrazioni, le colonne del nome erano di tipo nvarchar(MAX).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-176">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="4cbdb-177">Ora sono di tipo nvarchar(50) e il nome colonna è cambiato da FirstMidName a FirstName.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-177">They're now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="4cbdb-178">Se si prova a compilare prima di aver creato tutte le classi di entità delle sezioni seguenti, possono verificarsi errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-178">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="4cbdb-179">Modifiche finali all'entità Student</span><span class="sxs-lookup"><span data-stu-id="4cbdb-179">Final changes to the Student entity</span></span>

![Entità Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="4cbdb-181">In *Models/Student.cs* sostituire il codice aggiunto in precedenza con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-181">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="4cbdb-182">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-182">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="4cbdb-183">Attributo Required</span><span class="sxs-lookup"><span data-stu-id="4cbdb-183">The Required attribute</span></span>

<span data-ttu-id="4cbdb-184">L'attributo `Required` rende obbligatori i campi delle proprietà del nome.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-184">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="4cbdb-185">L'attributo `Required` non è necessario per i tipi non nullable quali i tipi del valore (DateTime, int, double, float e così via).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-185">The `Required` attribute isn't needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="4cbdb-186">I tipi che non possono essere null vengono considerati automaticamente come campi obbligatori.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-186">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="4cbdb-187">È possibile rimuovere l'attributo `Required` e sostituirlo con un parametro di lunghezza minima per l'attributo `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-187">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="4cbdb-188">Attributo Display</span><span class="sxs-lookup"><span data-stu-id="4cbdb-188">The Display attribute</span></span>

<span data-ttu-id="4cbdb-189">L'attributo `Display` specifica che la didascalia delle caselle di testo deve essere "First Name" (Nome), "Last Name" (Cognome), "Full Name" (Nome e cognome) ed "Enrollment Date" (Data di iscrizione) anziché il nome della proprietà (senza spazi tra le parole) in ogni istanza.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-189">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="4cbdb-190">Proprietà calcolata FullName</span><span class="sxs-lookup"><span data-stu-id="4cbdb-190">The FullName calculated property</span></span>

<span data-ttu-id="4cbdb-191">`FullName` è una proprietà calcolata che restituisce un valore creato concatenando altre due proprietà.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-191">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="4cbdb-192">Di conseguenza ha una sola funzione di accesso get e nel database non viene generata nessuna colonna `FullName`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-192">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="4cbdb-193">Creare l'entità Instructor</span><span class="sxs-lookup"><span data-stu-id="4cbdb-193">Create the Instructor Entity</span></span>

![Entità Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="4cbdb-195">Creare *Models/Instructor.cs* sostituendo il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-195">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="4cbdb-196">Si noti che molte proprietà sono uguali nelle entità Student e Instructor.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-196">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="4cbdb-197">Nell'esercitazione [Implementing Inheritance](inheritance.md) (Implementazione dell'ereditarietà) più avanti in questa serie si effettuerà il refactoring di questo codice per eliminare la ridondanza.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-197">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="4cbdb-198">È possibile posizionare più attributi su una riga, pertanto è anche possibile scrivere gli attributi `HireDate` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-198">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="4cbdb-199">Proprietà di navigazione CourseAssignments e OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="4cbdb-199">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="4cbdb-200">Le proprietà `CourseAssignments` e `OfficeAssignment` sono proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-200">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="4cbdb-201">Un insegnante può tenere un numero qualsiasi di corsi, pertanto `CourseAssignments` è definita come raccolta.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-201">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="4cbdb-202">Se una proprietà di navigazione può contenere più entità, il tipo della proprietà deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-202">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="4cbdb-203">È possibile specificare `ICollection<T>` o un tipo come `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-203">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="4cbdb-204">Se si specifica `ICollection<T>`, per impostazione predefinita EF crea una raccolta `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-204">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="4cbdb-205">Il motivo per cui queste sono entità `CourseAssignment` viene illustrato più avanti nella sezione relativa alle relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-205">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="4cbdb-206">Le regole business di Contoso University specificano che un insegnante non può avere più di un ufficio, pertanto la proprietà `OfficeAssignment` contiene un'unica entità OfficeAssignment (che può essere null se non è assegnato alcun ufficio).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-206">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="4cbdb-207">Creare l'entità OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="4cbdb-207">Create the OfficeAssignment entity</span></span>

![Entità OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="4cbdb-209">Creare *Models/OfficeAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-209">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="4cbdb-210">Attributo Key</span><span class="sxs-lookup"><span data-stu-id="4cbdb-210">The Key attribute</span></span>

<span data-ttu-id="4cbdb-211">Esiste una relazione uno-a-zero-o-uno tra le entità Instructor e OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-211">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="4cbdb-212">L'assegnazione dell'ufficio esiste solo in relazione all'insegnante al quale viene assegnato l'ufficio, pertanto la chiave primaria dell'assegnazione è anche la chiave esterna per l'entità Instructor.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-212">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="4cbdb-213">Tuttavia Entity Framework non riconosce automaticamente InstructorID come chiave primaria di questa entità, perché il nome non rispetta la convenzione di denominazione ID o classnameID.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-213">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="4cbdb-214">Per identificare l'entità come chiave viene usato l'attributo `Key`:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-214">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="4cbdb-215">È anche possibile usare l'attributo `Key` se l'entità dispone di una chiave primaria propria ma si vuole assegnare alla proprietà un nome diverso da classnameID o ID.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-215">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="4cbdb-216">Per impostazione predefinita, EF considera la chiave come non generata dal database, perché la colonna è destinata a una relazione di identificazione.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-216">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="4cbdb-217">Proprietà di navigazione Instructor</span><span class="sxs-lookup"><span data-stu-id="4cbdb-217">The Instructor navigation property</span></span>

<span data-ttu-id="4cbdb-218">L'entità Instructor dispone di una proprietà di navigazione `OfficeAssignment` nullable (perché un docente potrebbe non avere un ufficio assegnato) e l'entità OfficeAssignment dispone di una proprietà di navigazione `Instructor` non nullable (perché un'assegnazione di ufficio non può esistere senza un insegnante: `InstructorID` è non nullable).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-218">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="4cbdb-219">Quando un'entità Instructor dispone di un'entità OfficeAssignment correlata, ogni entità include un riferimento all'altra entità nella relativa proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-219">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="4cbdb-220">È possibile inserire un attributo `[Required]` nella proprietà di navigazione Instructor per specificare che deve essere presente un insegnante correlato, ma questa operazione non è necessaria perché la chiave esterna `InstructorID` (che è anche la chiave per questa tabella) è di tipo non nullable.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-220">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="4cbdb-221">Modificare l'entità Course</span><span class="sxs-lookup"><span data-stu-id="4cbdb-221">Modify the Course Entity</span></span>

![Entità Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="4cbdb-223">In *Models/Course.cs* sostituire il codice aggiunto in precedenza con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-223">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="4cbdb-224">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-224">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="4cbdb-225">L'entità Course ha una proprietà di chiave esterna `DepartmentID` che fa riferimento all'entità Department correlata e include una proprietà di navigazione `Department`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-225">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="4cbdb-226">In Entity Framework non è necessario aggiungere una proprietà di chiave esterna al modello di dati se è disponibile una proprietà di navigazione per un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-226">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="4cbdb-227">EF crea automaticamente chiavi esterne nel database ogni volta che sono necessarie e crea [proprietà nascoste](https://docs.microsoft.com/ef/core/modeling/shadow-properties) per tali chiavi.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-227">EF automatically creates foreign keys in the database wherever they're needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="4cbdb-228">Tuttavia il fatto di avere la chiave esterna nel modello di dati può rendere più semplici ed efficienti gli aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-228">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="4cbdb-229">Quando ad esempio si recupera un'entità Course per la modifica, l'entità Department è null se non viene caricata, pertanto prima di aggiornare l'entità Course è necessario recuperare l'entità Department.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-229">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="4cbdb-230">Quando la proprietà di chiave esterna `DepartmentID` viene inclusa nel modello di dati, non è necessario recuperare l'entità Department prima di eseguire l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-230">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="4cbdb-231">Attributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="4cbdb-231">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="4cbdb-232">L'attributo `DatabaseGenerated` con il parametro `None` per la proprietà `CourseID` indica che i valori di chiave primaria vengono specificati dall'utente anziché essere generati dal database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-232">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="4cbdb-233">Per impostazione predefinita, Entity Framework presuppone che i valori di chiave primaria vengano generati dal database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-233">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="4cbdb-234">Questa è la condizione ottimale nella maggior parte degli scenari.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-234">That's what you want in most scenarios.</span></span> <span data-ttu-id="4cbdb-235">Tuttavia per le entità Course si userà un numero di corso specificato dall'utente, ad esempio il numero di serie 1000 per un reparto, il numero di serie 2000 per un altro reparto e così via.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-235">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="4cbdb-236">Anche l'attributo `DatabaseGenerated` può essere usato anche per generare valori predefiniti, come nel caso delle colonne di database usate per registrare la data di creazione o aggiornamento di una riga.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-236">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="4cbdb-237">Per altre informazioni, vedere [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties) (Proprietà generate).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-237">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="4cbdb-238">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="4cbdb-238">Foreign key and navigation properties</span></span>

<span data-ttu-id="4cbdb-239">La proprietà di chiave esterna e le proprietà di navigazione nell'entità Course riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-239">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="4cbdb-240">Un corso viene assegnato a un solo reparto, pertanto sono presenti una chiave esterna `DepartmentID` e una proprietà di navigazione `Department` per i motivi indicati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-240">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="4cbdb-241">Un corso può avere un numero qualsiasi di studenti iscritti, pertanto la proprietà di navigazione `Enrollments` è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-241">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="4cbdb-242">Un corso può essere impartito da più insegnanti, pertanto la proprietà di navigazione `CourseAssignments` è una raccolta (il tipo `CourseAssignment` è illustrato [più avanti](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="4cbdb-242">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="4cbdb-243">Creare l'entità Department</span><span class="sxs-lookup"><span data-stu-id="4cbdb-243">Create the Department entity</span></span>

![Entità Department](complex-data-model/_static/department-entity.png)


<span data-ttu-id="4cbdb-245">Creare *Models/Department.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-245">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="4cbdb-246">Attributo Column</span><span class="sxs-lookup"><span data-stu-id="4cbdb-246">The Column attribute</span></span>

<span data-ttu-id="4cbdb-247">In precedenza l'attributo `Column` è stato usato per modificare il mapping del nome di colonna.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-247">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="4cbdb-248">Nel codice dell'entità Department l'attributo `Column` viene usato per modificare il mapping del tipo di dati SQL e per far sì che la colonna venga definita usando il tipo SQL Server money nel database:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-248">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="4cbdb-249">In genere il mapping della colonna non è necessario, perché Entity Framework sceglie il tipo di dati SQL Server appropriato in base al tipo CLR definito per la proprietà.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-249">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="4cbdb-250">Il tipo CLR `decimal` esegue il mapping a un tipo SQL Server `decimal`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-250">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="4cbdb-251">In questo caso tuttavia si sa che la colonna includerà importi in valuta, pertanto il tipo di dati money è più appropriato.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-251">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="4cbdb-252">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="4cbdb-252">Foreign key and navigation properties</span></span>

<span data-ttu-id="4cbdb-253">Le proprietà di chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-253">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="4cbdb-254">Un reparto può avere o meno un amministratore e un amministratore è sempre un insegnante.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-254">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="4cbdb-255">Di conseguenza la proprietà `InstructorID` è inclusa come chiave esterna per l'entità Instructor e dopo l'indicazione del tipo `int` viene aggiunto un punto interrogativo, per contrassegnare la proprietà come nullable.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-255">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="4cbdb-256">La proprietà di navigazione è denominata `Administrator` ma contiene un'entità Instructor:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-256">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="4cbdb-257">Un reparto può avere molti corsi, pertanto è disponibile una proprietà di navigazione Courses:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-257">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="4cbdb-258">Per convenzione, Entity Framework consente l'eliminazione a catena per le chiavi esterne non nullable e per le relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-258">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="4cbdb-259">Ciò può determinare regole di eliminazione a catena circolari, che generano un'eccezione quando si prova ad aggiungere una migrazione.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-259">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="4cbdb-260">Se ad esempio la proprietà Department.InstructorID non è stata definita come nullable, EF configura una regola di eliminazione a catena per eliminare l'insegnante quando si elimina il reparto, un risultato indesiderato.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-260">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which isn't what you want to have happen.</span></span> <span data-ttu-id="4cbdb-261">Se le regole business richiedono che la proprietà `InstructorID` sia non nullable, è necessario usare la seguente istruzione API Fluent per disattivare l'eliminazione a catena nella relazione:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-261">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="4cbdb-262">Modificare l'entità Enrollment</span><span class="sxs-lookup"><span data-stu-id="4cbdb-262">Modify the Enrollment entity</span></span>

![Entità Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="4cbdb-264">In *Models/Enrollment.cs* sostituire il codice aggiunto in precedenza con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-264">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="4cbdb-265">Proprietà chiave esterna e di navigazione</span><span class="sxs-lookup"><span data-stu-id="4cbdb-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="4cbdb-266">Le proprietà di chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-266">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="4cbdb-267">Un record di iscrizione è relativo a un singolo corso, pertanto sono presenti una proprietà di chiave esterna `CourseID` e una proprietà di navigazione `Course`:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-267">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="4cbdb-268">Un record di iscrizione è relativo a un singolo studente, pertanto sono presenti una proprietà di chiave esterna `StudentID` e una proprietà di navigazione `Student`:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-268">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="4cbdb-269">Relazioni molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="4cbdb-269">Many-to-Many Relationships</span></span>

<span data-ttu-id="4cbdb-270">È presente una relazione molti-a-molti tra le entità Student e Course e l'entità Enrollment funziona come una tabella di join molti-a-molti *con payload* nel database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-270">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="4cbdb-271">"Con payload" significa che la tabella Enrollment contiene dati aggiuntivi oltre alle chiavi esterne delle tabelle di join (in questo caso una chiave primaria e una proprietà Grade).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-271">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="4cbdb-272">La figura seguente illustra l'aspetto di queste relazioni in un diagramma di entità.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-272">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="4cbdb-273">Il diagramma è stato generato con Entity Framework Power Tools per EF 6.x. La creazione del diagramma non fa parte dell'esercitazione e il diagramma viene visualizzato solo come esempio.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-273">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Relazione molti-a-molti Student-Course (Studente-Corso)](complex-data-model/_static/student-course.png)

<span data-ttu-id="4cbdb-275">Ogni riga della relazione inizia con un 1 e termina con un asterisco (\*), per indicare una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-275">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="4cbdb-276">Se la tabella Enrollment (Iscrizione) non include informazioni sul livello è sufficiente che contenga le due chiavi esterne CourseID e StudentID.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-276">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="4cbdb-277">In questo caso sarebbe una tabella di join molti-a-molti senza payload (o tabella di join pura) del database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-277">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="4cbdb-278">Le entità Instructor e Course dispongono di questo tipo di relazione molti-a-molti e il passaggio successivo consiste nel creare una classe di entità che faccia da tabella di join senza payload.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-278">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="4cbdb-279">Le tabelle di join implicite per le relazioni molti-a-molti sono supportate in EF 6.x, ma non in EF Core.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-279">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="4cbdb-280">Per altre informazioni, vedere l'[approfondimento nel repository EF Core di GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-280">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="4cbdb-281">Entità CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="4cbdb-281">The CourseAssignment entity</span></span>

![Entità CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="4cbdb-283">Creare *Models/CourseAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-283">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="4cbdb-284">Nomi delle entità di join</span><span class="sxs-lookup"><span data-stu-id="4cbdb-284">Join entity names</span></span>

<span data-ttu-id="4cbdb-285">È necessaria una tabella di join del database per la relazione molti-a-molti Instructor-Courses e tale relazione deve essere rappresentata da un set di entità.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-285">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="4cbdb-286">È pratica comune assegnare a un'entità di join il nome `EntityName1EntityName2`, che in questo caso sarebbe `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-286">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="4cbdb-287">È tuttavia consigliabile scegliere un nome che descrive la relazione.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-287">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="4cbdb-288">I modelli di dati sono inizialmente semplici, quindi crescono e diventano più complessi. In molti casi ai join senza payload vengono assegnati payload in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-288">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="4cbdb-289">Se si inizia con un nome di entità descrittivo, non sarà necessario modificarlo successivamente.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-289">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="4cbdb-290">Idealmente l'entità di join dovrebbe avere il proprio nome naturale (se possibile composto da un'unica parola) nel dominio di business.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-290">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="4cbdb-291">Ad esempio Books (Documentazione) e Customers (Clienti) potrebbero essere collegati mediante Ratings (Valutazioni).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-291">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="4cbdb-292">Per questa relazione `CourseAssignment` è una soluzione migliore rispetto a `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-292">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="4cbdb-293">Chiave composta</span><span class="sxs-lookup"><span data-stu-id="4cbdb-293">Composite key</span></span>

<span data-ttu-id="4cbdb-294">Dato che le chiavi esterne non sono nullable e insieme identificano in modo univoco ogni riga della tabella, non è necessario avere una chiave primaria separata.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-294">Since the foreign keys are not nullable and together uniquely identify each row of the table, there's no need for a separate primary key.</span></span> <span data-ttu-id="4cbdb-295">Le proprietà *InstructorID* e *CourseID* funzionano come una chiave primaria composta.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-295">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="4cbdb-296">L'unico modo per identificare le chiavi primarie composte in Entity Framework è l'uso di *API Fluent* (l'operazione non può essere eseguita con gli attributi).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-296">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="4cbdb-297">Nella sezione successiva si vedrà come configurare la chiave primaria composta.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-297">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="4cbdb-298">La chiave composta garantisce che anche se è possibile avere più righe per un corso e più righe per un insegnante, non è possibile avere più righe per lo stesso insegnante e lo stesso corso.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-298">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="4cbdb-299">L'entità di join `Enrollment` definisce la propria chiave primaria, pertanto sono possibili i duplicati di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-299">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="4cbdb-300">Per evitare tali duplicati è possibile aggiungere un indice univoco ai campi chiave esterna o configurare `Enrollment` con una chiave primaria composta simile a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-300">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="4cbdb-301">Per altre informazioni, vedere [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes) (Indici).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-301">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="4cbdb-302">Aggiornare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="4cbdb-302">Update the database context</span></span>

<span data-ttu-id="4cbdb-303">Aggiungere il codice evidenziato di seguito al file *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-303">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="4cbdb-304">Questo codice aggiunge le nuove entità e configura la chiave primaria composta dell'entità CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-304">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="4cbdb-305">Alternativa API Fluent agli attributi</span><span class="sxs-lookup"><span data-stu-id="4cbdb-305">Fluent API alternative to attributes</span></span>

<span data-ttu-id="4cbdb-306">Il codice nel metodo `OnModelCreating` della classe `DbContext` usa l'*API Fluent* per configurare il comportamento di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-306">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="4cbdb-307">L'API è denominata "API Fluent" perché viene spesso usata unendo una serie di chiamate di metodi in un'unica istruzione, come in questo esempio tratto dalla [documentazione di EF Core](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="4cbdb-307">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="4cbdb-308">In questa esercitazione si usa l'API Fluent solo per operazioni di mapping del database non eseguibili con gli attributi.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-308">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="4cbdb-309">È tuttavia possibile usare l'API Fluent anche per specificare la maggior parte delle regole di formattazione, convalida e mapping specificabili tramite gli attributi.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-309">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="4cbdb-310">Alcuni attributi quali `MinimumLength` non possono essere applicati con l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-310">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="4cbdb-311">Come accennato in precedenza, `MinimumLength` non modifica lo schema, ma si limita ad applicare una regola di convalida lato client e lato server.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-311">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="4cbdb-312">Alcuni sviluppatori preferiscono usare solo l'API Fluent, per dare un aspetto "ordinato" alle classi di entità.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-312">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="4cbdb-313">È possibile usare un misto di attributi e API Fluent e alcune personalizzazioni sono eseguibili solo mediante l'API Fluent, ma in generale è consigliabile scegliere uno dei due approcci e usarlo in modo il più possibile coerente.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-313">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="4cbdb-314">Se si usano entrambi gli approcci, tenere presente che ogni volta che si verifica un conflitto l'API Fluent esegue l'override degli attributi.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-314">If you do use both, note that wherever there's a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="4cbdb-315">Per altre informazioni sul confronto tra attributi e API Fluent, vedere [Metodi di configurazione](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="4cbdb-315">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="4cbdb-316">Diagramma dell'entità che visualizza le relazioni</span><span class="sxs-lookup"><span data-stu-id="4cbdb-316">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="4cbdb-317">La figura seguente visualizza il diagramma creato da Entity Framework Power Tools per il modello School completato.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-317">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Diagramma dell'entità](complex-data-model/_static/diagram.png)

<span data-ttu-id="4cbdb-319">Oltre alle linee delle relazioni uno-a-molti (da 1 a \*) è possibile visualizzare la linea della relazione uno-a-zero-o-uno (da 1 a 0..1) tra le entità Instructor e OfficeAssignment e la linea della relazione zero-o-uno-a-molti (da 0..1 a *) tra le entità Instructor e Department.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-319">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="4cbdb-320">Assegnare valori di inizializzazione al database con dati di test</span><span class="sxs-lookup"><span data-stu-id="4cbdb-320">Seed the Database with Test Data</span></span>

<span data-ttu-id="4cbdb-321">Sostituire il codice nel file *Data/DbInitializer.cs* con il codice seguente per includere dati di inizializzazione per le nuove entità create.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-321">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="4cbdb-322">Come si è visto nella prima esercitazione, la maggior parte di questo codice crea semplicemente nuovi oggetti entità e carica i dati di esempio nelle proprietà in base alle esigenze di test.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-322">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="4cbdb-323">Osservare la modalità di gestione delle relazioni molti-a-molti: il codice crea relazioni tramite la creazione di entità nei set di entità di join `Enrollments` e `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-323">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="4cbdb-324">Aggiungere una migrazione</span><span class="sxs-lookup"><span data-stu-id="4cbdb-324">Add a migration</span></span>

<span data-ttu-id="4cbdb-325">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-325">Save your changes and build the project.</span></span> <span data-ttu-id="4cbdb-326">Quindi aprire la finestra di comando nella cartella del progetto e immettere il comando `migrations add` (non eseguire ancora il comando update-database):</span><span class="sxs-lookup"><span data-stu-id="4cbdb-326">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="4cbdb-327">Viene visualizzato un avviso sulla possibile perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-327">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="4cbdb-328">Se si prova a eseguire il comando `database update` in questa fase (evitare di farlo), si ottiene il seguente errore:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-328">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="4cbdb-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="4cbdb-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="4cbdb-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID' (L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "FK_dbo.Course_dbo.Department_DepartmentID". Il conflitto si è verificato nella colonna 'DepartmentID' della tabella "dbo.Department"del database "ContosoUniversity").</span><span class="sxs-lookup"><span data-stu-id="4cbdb-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="4cbdb-331">In determinati casi, quando si eseguono migrazioni con dati esistenti è necessario inserire dati stub nel database per soddisfare i vincoli di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-331">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="4cbdb-332">Il codice generato nel metodo `Up` aggiunge una chiave esterna DepartmentID alla tabella Course.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-332">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="4cbdb-333">Se sono già presenti righe nella tabella Course quando viene eseguito il codice, l'operazione `AddColumn` non riesce perché SQL Server non determina il valore da inserire nella colonna, che non può essere null.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-333">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="4cbdb-334">In questa esercitazione si esegue la migrazione in un nuovo database, ma in un'applicazione di produzione è necessario che la migrazione gestisca i dati esistenti. Le istruzioni che seguono visualizzano un esempio di esecuzione di questa operazione.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-334">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="4cbdb-335">Per fare in modo che questa migrazione funzioni con i dati esistenti, è necessario modificare il codice per dare alla nuova colonna un valore predefinito e creare un reparto stub denominato "Temp" che svolga la funzione di reparto predefinito.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-335">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="4cbdb-336">Di conseguenza, dopo l'esecuzione del metodo `Up` tutte le righe Course esistenti saranno correlate al reparto "Temp".</span><span class="sxs-lookup"><span data-stu-id="4cbdb-336">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="4cbdb-337">Aprire il file *{timestamp}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-337">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>

* <span data-ttu-id="4cbdb-338">Impostare come commento la riga di codice che aggiunge la colonna DepartmentID alla tabella Course.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-338">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="4cbdb-339">Aggiungere il codice evidenziato seguente dopo il codice che crea la tabella Department:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-339">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="4cbdb-340">In un'applicazione di produzione è necessario creare codice o script per aggiungere righe Department e associare le righe Course alle nuove righe Department.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-340">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="4cbdb-341">In tal modo il reparto "Temp" o il valore predefinito nella colonna Course.DepartmentID non saranno più necessari.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-341">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="4cbdb-342">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-342">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="4cbdb-343">Modificare la stringa di connessione e aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="4cbdb-343">Change the connection string and update the database</span></span>

<span data-ttu-id="4cbdb-344">Ora la classe `DbInitializer` include nuovo codice che aggiunge dati di inizializzazione per le nuove entità a un database vuoto.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-344">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="4cbdb-345">Per fare in modo che EF crei un nuovo database vuoto, cambiare il nome del database nella stringa di connessione *appsettings.json* digitando ContosoUniversity3 o un altro nome non ancora adottato nel computer in uso.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-345">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="4cbdb-346">Salvare le modifiche apportate ad *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-346">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="4cbdb-347">In alternativa alla modifica del nome del database, è possibile eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-347">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="4cbdb-348">Usare **Esplora oggetti di SQL Server** (SSOX) o il comando CLI `database drop`:</span><span class="sxs-lookup"><span data-stu-id="4cbdb-348">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="4cbdb-349">Dopo aver modificato il nome del database o eliminato il database, eseguire il comando `database update` nella finestra di comando per eseguire le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-349">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="4cbdb-350">Eseguire l'app per far sì che il metodo `DbInitializer.Initialize` venga eseguito e popoli il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-350">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="4cbdb-351">Aprire il database in SSOX come in precedenza, quindi espandere il nodo **Tabelle** per visualizzare tutte le tabelle che sono state create.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-351">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="4cbdb-352">Se SSOX è ancora aperto dall'operazione precedente, fare clic sul pulsante **Aggiorna**.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-352">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![Tabelle in SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="4cbdb-354">Eseguire l'app per attivare il codice inizializzatore che esegue l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-354">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="4cbdb-355">Fare clic con il pulsante destro del mouse sulla tabella **CourseAssignment** e selezionare **Visualizza dati** per verificare che la tabella contenga dati.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-355">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![Dati CourseAssignment in SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="4cbdb-357">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="4cbdb-357">Summary</span></span>

<span data-ttu-id="4cbdb-358">Ora sono presenti un modello di dati più complesso e il database corrispondente.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-358">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="4cbdb-359">Nell'esercitazione seguente si apprenderanno tecniche per l'accesso ai dati correlati.</span><span class="sxs-lookup"><span data-stu-id="4cbdb-359">In the following tutorial, you'll learn more about how to access related data.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="4cbdb-360">[Precedente](migrations.md)
> [Successivo](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="4cbdb-360">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>
