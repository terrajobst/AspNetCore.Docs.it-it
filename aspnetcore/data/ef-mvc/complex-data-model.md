---
title: Core di ASP.NET MVC con Entity Framework Core - modello di dati - 5 di 10
author: tdykstra
description: "In questa esercitazione aggiungere altre entità e relazioni e personalizzare il modello di dati specificando le regole di mapping del database, convalida e formattazione."
keywords: Annotazioni dei dati ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 0dd63913-a041-48b6-96a4-3aeaedbdf5d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 7d216bc07d0a8d739f0cecbc5b571b6144c13e61
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a><span data-ttu-id="0a81c-104">Creazione di un modello di dati complessi - EF Core con l'esercitazione di base di ASP.NET MVC (5 di 10)</span><span class="sxs-lookup"><span data-stu-id="0a81c-104">Creating a complex data model - EF Core with ASP.NET Core MVC tutorial (5 of 10)</span></span>

<span data-ttu-id="0a81c-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0a81c-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0a81c-106">L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a81c-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="0a81c-107">Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="0a81c-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="0a81c-108">Nelle esercitazioni precedenti, si è lavorato con un modello di dati semplici che è stato composto da tre entità.</span><span class="sxs-lookup"><span data-stu-id="0a81c-108">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="0a81c-109">In questa esercitazione si aggiungeranno ulteriori entità e relazioni e verrà personalizzato il modello di dati specificando le regole di mapping del database, convalida e formattazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-109">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="0a81c-110">Al termine dell'operazione, le classi di entità verranno costituiscono il modello di dati completato che è illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="0a81c-110">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Diagramma di entità](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="0a81c-112">Personalizzare il modello di dati utilizzando gli attributi</span><span class="sxs-lookup"><span data-stu-id="0a81c-112">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="0a81c-113">In questa sezione si noterà come personalizzare il modello di dati utilizzando gli attributi che specificano la formattazione, convalida e regole di mapping del database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-113">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="0a81c-114">Quindi in diversi nelle sezioni seguenti che verrà creato il modello di dati completo dell'istituto di istruzione aggiungendo attributi alle classi è già stato creato e la creazione di nuove classi per i tipi di entità rimanenti nel modello.</span><span class="sxs-lookup"><span data-stu-id="0a81c-114">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="0a81c-115">L'attributo di tipo di dati</span><span class="sxs-lookup"><span data-stu-id="0a81c-115">The DataType attribute</span></span>

<span data-ttu-id="0a81c-116">Per le date di registrazione studenti, tutte le pagine web attualmente Visualizza il tempo insieme alla data, anche se tutto è rilevante per questo campo è la data.</span><span class="sxs-lookup"><span data-stu-id="0a81c-116">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="0a81c-117">Utilizzando gli attributi di annotazione dei dati, è possibile apportare una modifica che in ogni visualizzazione che mostra i dati per correggere il formato di visualizzazione del codice.</span><span class="sxs-lookup"><span data-stu-id="0a81c-117">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="0a81c-118">Per vedere un esempio di come eseguire l'operazione, verrà aggiunto un attributo di `EnrollmentDate` proprietà la `Student` classe.</span><span class="sxs-lookup"><span data-stu-id="0a81c-118">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="0a81c-119">In *Models/Student.cs*, aggiungere un `using` istruzione per il `System.ComponentModel.DataAnnotations` dello spazio dei nomi e aggiungere `DataType` e `DisplayFormat` gli attributi di `EnrollmentDate` proprietà, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0a81c-119">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

<span data-ttu-id="0a81c-120">[!code-csharp[Principale](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-120">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]</span></span>

<span data-ttu-id="0a81c-121">Il `DataType` attributo viene utilizzato per specificare un tipo di dati che è più specifico di tipo intrinseco del database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-121">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="0a81c-122">In questo caso si vuole inserire solo tenere traccia delle date, non la data e ora.</span><span class="sxs-lookup"><span data-stu-id="0a81c-122">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="0a81c-123">Il `DataType` enumerazione fornisce per molti tipi di dati, ad esempio Date, Time, PhoneNumber, valuta, EmailAddress e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="0a81c-123">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="0a81c-124">Il `DataType` attributo può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo.</span><span class="sxs-lookup"><span data-stu-id="0a81c-124">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="0a81c-125">Ad esempio, un `mailto:` possibile creare un collegamento per `DataType.EmailAddress`, e un selettore data può essere fornito per `DataType.Date` nei browser che supporta HTML5.</span><span class="sxs-lookup"><span data-stu-id="0a81c-125">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="0a81c-126">Il `DataType` attributo genera HTML 5 `data-` attributi (si pronuncia dati dash) in grado di comprendere i browser HTML 5.</span><span class="sxs-lookup"><span data-stu-id="0a81c-126">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="0a81c-127">Il `DataType` attributi non forniscono alcuna convalida.</span><span class="sxs-lookup"><span data-stu-id="0a81c-127">The `DataType` attributes do not provide any validation.</span></span>

<span data-ttu-id="0a81c-128">`DataType.Date`Specifica il formato della data che viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="0a81c-128">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="0a81c-129">Per impostazione predefinita, viene visualizzato il campo dei dati in base ai formati predefiniti in base a CultureInfo del server.</span><span class="sxs-lookup"><span data-stu-id="0a81c-129">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="0a81c-130">Il `DisplayFormat` attributo viene utilizzato per specificare in modo esplicito il formato di data:</span><span class="sxs-lookup"><span data-stu-id="0a81c-130">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="0a81c-131">Il `ApplyFormatInEditMode` impostazione specifica che la formattazione deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica.</span><span class="sxs-lookup"><span data-stu-id="0a81c-131">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="0a81c-132">(Non è possibile che per alcuni campi, ad esempio, per i valori di valuta, occorre evitare il simbolo di valuta nella casella di testo per la modifica.)</span><span class="sxs-lookup"><span data-stu-id="0a81c-132">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="0a81c-133">È possibile utilizzare il `DisplayFormat` attributo da se stesso, ma in genere è consigliabile utilizzare il `DataType` anche l'attributo.</span><span class="sxs-lookup"><span data-stu-id="0a81c-133">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="0a81c-134">Il `DataType` attributo fornisce la semantica dei dati anziché come eseguirne il rendering in una schermata e fornisce i seguenti vantaggi che non si ottengono con `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="0a81c-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="0a81c-135">Il browser è possibile abilitare le funzionalità di HTML5 (ad esempio per visualizzare un controllo di calendario, il simbolo di valuta delle impostazioni locali appropriata, i collegamenti di posta elettronica, alcuni client-side input convalida e così via).</span><span class="sxs-lookup"><span data-stu-id="0a81c-135">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="0a81c-136">Per impostazione predefinita, il browser verrà eseguito il rendering di dati utilizzando il formato corretto in base alle impostazioni locali del sistema.</span><span class="sxs-lookup"><span data-stu-id="0a81c-136">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="0a81c-137">Per ulteriori informazioni, vedere il [ \<input > tag di documentazione di supporto](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0a81c-137">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="0a81c-138">Eseguire nuovamente la pagina di indice di studenti e notare che volte non vengono più visualizzate per le date di registrazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-138">Run the Students Index page again and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="0a81c-139">Lo stesso sarà true per qualsiasi vista che utilizza il modello di studenti.</span><span class="sxs-lookup"><span data-stu-id="0a81c-139">The same will be true for any view that uses the Student model.</span></span>

![Pagina di indice di studenti con date senza tempi di](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="0a81c-141">L'attributo StringLength</span><span class="sxs-lookup"><span data-stu-id="0a81c-141">The StringLength attribute</span></span>

<span data-ttu-id="0a81c-142">È inoltre possibile specificare regole di convalida dei dati e i messaggi di errore di convalida utilizzando gli attributi.</span><span class="sxs-lookup"><span data-stu-id="0a81c-142">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="0a81c-143">Il `StringLength` attributo imposta la lunghezza massima del database e fornisce lato client e lato server della convalida di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0a81c-143">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="0a81c-144">È inoltre possibile specificare la lunghezza minima della stringa in questo attributo, ma il valore minimo non ha alcun impatto sullo schema di database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-144">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="0a81c-145">Si supponga che si desidera garantire che gli utenti non immettere più di 50 caratteri per un nome.</span><span class="sxs-lookup"><span data-stu-id="0a81c-145">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="0a81c-146">Per aggiungere questa limitazione, è necessario `StringLength` gli attributi di `LastName` e `FirstMidName` proprietà, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="0a81c-146">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

<span data-ttu-id="0a81c-147">[!code-csharp[Principale](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-147">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]</span></span>

<span data-ttu-id="0a81c-148">Il `StringLength` attributo non impedisce a un utente di immettere lo spazio vuoto per un nome.</span><span class="sxs-lookup"><span data-stu-id="0a81c-148">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="0a81c-149">È possibile utilizzare il `RegularExpression` attributo per applicare restrizioni per l'input.</span><span class="sxs-lookup"><span data-stu-id="0a81c-149">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="0a81c-150">Ad esempio, il codice seguente richiede il primo carattere da maiuscolo e i caratteri rimanenti alfabetico:</span><span class="sxs-lookup"><span data-stu-id="0a81c-150">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="0a81c-151">Il `MaxLength` attributo fornisce funzionalità simili a quelle di `StringLength` attributo ma non è disponibile sul lato client convalida.</span><span class="sxs-lookup"><span data-stu-id="0a81c-151">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="0a81c-152">Il modello di database è stata modificata in modo che richiede una modifica nello schema del database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-152">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="0a81c-153">Utilizzare le migrazioni per aggiornare lo schema senza perdere i dati che possono essere aggiunti al database tramite l'interfaccia utente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-153">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="0a81c-154">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="0a81c-154">Save your changes and build the project.</span></span> <span data-ttu-id="0a81c-155">Quindi aprire la finestra di comando nella cartella del progetto e immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="0a81c-155">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="0a81c-156">Il `migrations add` comando genera un avviso che potrebbe verificarsi una perdita di dati, in quanto la modifica rende la lunghezza massima più breve per due colonne.</span><span class="sxs-lookup"><span data-stu-id="0a81c-156">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="0a81c-157">Le migrazioni crea un file denominato * \<timeStamp > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="0a81c-157">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="0a81c-158">Questo file contiene codice di `Up` metodo che aggiornerà il database in base al modello di dati corrente.</span><span class="sxs-lookup"><span data-stu-id="0a81c-158">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="0a81c-159">Il `database update` comando è stato eseguito il codice.</span><span class="sxs-lookup"><span data-stu-id="0a81c-159">The `database update` command ran that code.</span></span>

<span data-ttu-id="0a81c-160">Il timestamp come preceduto al nome del file migrazioni utilizzato da Entity Framework per ordinare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="0a81c-160">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="0a81c-161">È possibile creare più migrazioni prima di eseguire il comando update-database, e quindi tutte le migrazioni sono applicate nell'ordine in cui sono stati creati.</span><span class="sxs-lookup"><span data-stu-id="0a81c-161">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="0a81c-162">Eseguire la pagina di creazione e immettere il nome di più di 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="0a81c-162">Run the Create page, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="0a81c-163">Quando si fa clic su Crea, la convalida lato client viene illustrato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="0a81c-163">When you click Create, client side validation shows an error message.</span></span>

![Gli studenti indice pagina la visualizzazione di errori di lunghezza stringa](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="0a81c-165">L'attributo di colonna</span><span class="sxs-lookup"><span data-stu-id="0a81c-165">The Column attribute</span></span>

<span data-ttu-id="0a81c-166">Inoltre, è possibile utilizzare gli attributi per controllare il mapping tra le classi e le proprietà per il database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-166">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="0a81c-167">Si supponga che sia stato usato il nome `FirstMidName` per il nome del primo campo perché il campo può contenere anche un cognome.</span><span class="sxs-lookup"><span data-stu-id="0a81c-167">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="0a81c-168">Ma si desidera che la colonna di database sia denominato `FirstName`, perché gli utenti che scrittura di query ad hoc sul database sono abituati a tale nome.</span><span class="sxs-lookup"><span data-stu-id="0a81c-168">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="0a81c-169">Per rendere questo mapping, è possibile utilizzare il `Column` attributo.</span><span class="sxs-lookup"><span data-stu-id="0a81c-169">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="0a81c-170">Il `Column` attributo specifica che quando viene creato il database, la colonna del `Student` tabella in cui viene eseguito il mapping per il `FirstMidName` proprietà verrà denominata `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="0a81c-170">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="0a81c-171">In altre parole, quando il codice fa riferimento a `Student.FirstMidName`, i dati provengono dal o aggiornati nel `FirstName` colonna del `Student` tabella.</span><span class="sxs-lookup"><span data-stu-id="0a81c-171">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="0a81c-172">Se non si specificano nomi di colonna, essi sono assegnato lo stesso nome come nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="0a81c-172">If you don't specify column names, they are given the same name as the property name.</span></span>

<span data-ttu-id="0a81c-173">Nel *Student.cs* file, aggiungere un `using` istruzione per `System.ComponentModel.DataAnnotations.Schema` e aggiungere l'attributo del nome di colonna per il `FirstMidName` proprietà, come illustrato nel codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="0a81c-173">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

<span data-ttu-id="0a81c-174">[!code-csharp[Principale](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-174">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]</span></span>

<span data-ttu-id="0a81c-175">L'aggiunta del `Column` attributo modifica il supporto del modello di `SchoolContext`, in modo che non corrisponda al database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-175">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="0a81c-176">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="0a81c-176">Save your changes and build the project.</span></span> <span data-ttu-id="0a81c-177">Quindi aprire la finestra di comando nella cartella del progetto e immettere i comandi seguenti per creare un'altra migrazione:</span><span class="sxs-lookup"><span data-stu-id="0a81c-177">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="0a81c-178">In **Esplora oggetti di SQL Server**, aprire Progettazione tabelle Student facendo doppio clic il **Student** tabella.</span><span class="sxs-lookup"><span data-stu-id="0a81c-178">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabella di studenti in sillaba SSOX dopo le migrazioni](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="0a81c-180">Prima dell'applicazione le prime due migrazioni, le colonne nome sono di tipo nvarchar (max).</span><span class="sxs-lookup"><span data-stu-id="0a81c-180">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="0a81c-181">Sono ora nvarchar (50) e il nome della colonna è stato modificato da FirstMidName per FirstName.</span><span class="sxs-lookup"><span data-stu-id="0a81c-181">They are now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="0a81c-182">Se si tenta di compilare prima del completamento della creazione di tutte le classi di entità nelle sezioni seguenti, si potrebbero verificare errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-182">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="0a81c-183">Modifiche finali per l'entità di Student</span><span class="sxs-lookup"><span data-stu-id="0a81c-183">Final changes to the Student entity</span></span>

![Entità Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="0a81c-185">In *Models/Student.cs*, sostituire il codice aggiunto in precedenza con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="0a81c-185">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="0a81c-186">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="0a81c-186">The changes are highlighted.</span></span>

<span data-ttu-id="0a81c-187">[!code-csharp[Principale](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-187">[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="0a81c-188">L'attributo obbligatorio</span><span class="sxs-lookup"><span data-stu-id="0a81c-188">The Required attribute</span></span>

<span data-ttu-id="0a81c-189">Il `Required` attributo rende i campi obbligatori di nome proprietà.</span><span class="sxs-lookup"><span data-stu-id="0a81c-189">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="0a81c-190">Il `Required` attributo non è necessaria per i tipi non nullable, ad esempio i tipi di valore (DateTime, int, double, float, ecc.).</span><span class="sxs-lookup"><span data-stu-id="0a81c-190">The `Required` attribute is not needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="0a81c-191">Tipi che non possono essere null vengono considerati automaticamente come i campi obbligatori.</span><span class="sxs-lookup"><span data-stu-id="0a81c-191">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="0a81c-192">È possibile rimuovere il `Required` attributo e sostituirlo con un parametro di lunghezza minima per il `StringLength` attributo:</span><span class="sxs-lookup"><span data-stu-id="0a81c-192">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="0a81c-193">L'attributo di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="0a81c-193">The Display attribute</span></span>

<span data-ttu-id="0a81c-194">Il `Display` attributo specifica che la didascalia per le caselle di testo deve essere "Nome", "Last Name", "Nome" e "Data di registrazione" anziché il nome della proprietà in ogni istanza (che non dispone di spazio dividendo le parole).</span><span class="sxs-lookup"><span data-stu-id="0a81c-194">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="0a81c-195">La proprietà FullName calcolato</span><span class="sxs-lookup"><span data-stu-id="0a81c-195">The FullName calculated property</span></span>

<span data-ttu-id="0a81c-196">`FullName`è una proprietà calcolata che restituisce un valore che viene creato concatenando due altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="0a81c-196">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="0a81c-197">Di conseguenza ha solo una funzione di accesso get e nessun `FullName` colonna verrà generata nel database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-197">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="0a81c-198">Creare l'entità Instructor</span><span class="sxs-lookup"><span data-stu-id="0a81c-198">Create the Instructor Entity</span></span>

![Entità Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="0a81c-200">Creare *Models/Instructor.cs*, sostituendo il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0a81c-200">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

<span data-ttu-id="0a81c-201">[!code-csharp[Principale](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-201">[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]</span></span>

<span data-ttu-id="0a81c-202">Si noti che molte proprietà sono uguali nelle entità Student e Instructor.</span><span class="sxs-lookup"><span data-stu-id="0a81c-202">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="0a81c-203">Nel [implementazione ereditarietà](inheritance.md) esercitazione più avanti in questa serie, sarà il refactoring questo codice per eliminare la ridondanza.</span><span class="sxs-lookup"><span data-stu-id="0a81c-203">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="0a81c-204">È possibile inserire più attributi su una riga, pertanto è possibile anche scrivere la `HireDate` attributi come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0a81c-204">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="0a81c-205">Le proprietà di navigazione CourseAssignments e OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="0a81c-205">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="0a81c-206">Il `CourseAssignments` e `OfficeAssignment` sono proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-206">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="0a81c-207">Un docente può indicare un numero qualsiasi di corsi, pertanto `CourseAssignments` è definito come una raccolta.</span><span class="sxs-lookup"><span data-stu-id="0a81c-207">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="0a81c-208">Se una proprietà di navigazione può contenere più entità, il tipo deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate.</span><span class="sxs-lookup"><span data-stu-id="0a81c-208">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="0a81c-209">È possibile specificare `ICollection<T>` o un tipo, ad esempio `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="0a81c-209">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="0a81c-210">Se si specifica `ICollection<T>`, EF crea un `HashSet<T>` insieme per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="0a81c-210">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="0a81c-211">Il motivo per cui sono `CourseAssignment` entità vengono illustrati di seguito nella sezione sulle relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="0a81c-211">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="0a81c-212">Regole di business Contoso University stato che un docente può solo avere al massimo un ufficio, pertanto la `OfficeAssignment` proprietà contiene una singola entità OfficeAssignment (che può essere null se non viene assegnato).</span><span class="sxs-lookup"><span data-stu-id="0a81c-212">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="0a81c-213">Creare l'entità OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="0a81c-213">Create the OfficeAssignment entity</span></span>

![Entità OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="0a81c-215">Creare *Models/OfficeAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0a81c-215">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

<span data-ttu-id="0a81c-216">[!code-csharp[Principale](intro/samples/cu/Models/OfficeAssignment.cs)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-216">[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]</span></span>

### <a name="the-key-attribute"></a><span data-ttu-id="0a81c-217">L'attributo chiave</span><span class="sxs-lookup"><span data-stu-id="0a81c-217">The Key attribute</span></span>

<span data-ttu-id="0a81c-218">È una relazione uno-a-zero-o-uno tra istruttore e le entità OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="0a81c-218">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="0a81c-219">L'assegnazione dell'ufficio esiste solo in relazione a istruttore a che viene assegnato, e pertanto la chiave primaria è anche la chiave esterna per l'entità Instructor.</span><span class="sxs-lookup"><span data-stu-id="0a81c-219">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="0a81c-220">Ma Entity Framework non riconoscono automaticamente InstructorID come chiave primaria dell'entità perché il nome non rispetta la convenzione di denominazione ID o classnameID.</span><span class="sxs-lookup"><span data-stu-id="0a81c-220">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="0a81c-221">Pertanto, il `Key` attributo viene utilizzato per identificare come la chiave:</span><span class="sxs-lookup"><span data-stu-id="0a81c-221">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="0a81c-222">È inoltre possibile utilizzare il `Key` attributo se l'entità dispone di chiave primaria ma si desidera attribuire un nome di proprietà diverso da classnameID o ID.</span><span class="sxs-lookup"><span data-stu-id="0a81c-222">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="0a81c-223">Per impostazione predefinita, la chiave EF considera come non generati dal database perché la colonna è la relazione di identificazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-223">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="0a81c-224">La proprietà di navigazione Instructor</span><span class="sxs-lookup"><span data-stu-id="0a81c-224">The Instructor navigation property</span></span>

<span data-ttu-id="0a81c-225">Entità Instructor è nullable `OfficeAssignment` proprietà di navigazione (perché un docente potrebbe non disporre di un'assegnazione di office), e l'entità OfficeAssignment ha non-nullable `Instructor` proprietà di navigazione (perché non è un'assegnazione di office esistere senza un docente - `InstructorID` non ammette valori null).</span><span class="sxs-lookup"><span data-stu-id="0a81c-225">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="0a81c-226">Quando un'entità Instructor dispone di un'entità correlata di OfficeAssignment, ogni entità saranno necessario un riferimento a altro nella relativa proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-226">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="0a81c-227">È possibile inserire un `[Required]` attributo sulla proprietà di navigazione istruttore per specificare che deve essere presente un insegnante correlati, ma non è necessario eseguire questa operazione perché il `InstructorID` chiave esterna (che è anche la chiave per questa tabella) è di tipo non nullable.</span><span class="sxs-lookup"><span data-stu-id="0a81c-227">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="0a81c-228">Modificare l'entità Course</span><span class="sxs-lookup"><span data-stu-id="0a81c-228">Modify the Course Entity</span></span>

![Entità Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="0a81c-230">In *Models/Course.cs*, sostituire il codice aggiunto in precedenza con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="0a81c-230">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="0a81c-231">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="0a81c-231">The changes are highlighted.</span></span>

<span data-ttu-id="0a81c-232">[!code-csharp[Principale](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-232">[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]</span></span>

<span data-ttu-id="0a81c-233">L'entità corso ha una proprietà di chiave esterna `DepartmentID` che fa riferimento a entità reparto correlata che ha un `Department` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-233">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="0a81c-234">Entity Framework non è necessario aggiungere una proprietà di chiave esterna per il modello di dati quando si dispone di una proprietà di navigazione per un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="0a81c-234">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="0a81c-235">EF crea chiavi esterne nel database, ovunque siano necessarie e crea automaticamente [nascondere le proprietà](https://docs.microsoft.com/ef/core/modeling/shadow-properties) per loro.</span><span class="sxs-lookup"><span data-stu-id="0a81c-235">EF automatically creates foreign keys in the database wherever they are needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="0a81c-236">Ma con la chiave esterna nel modello di dati è possibile eseguire gli aggiornamenti più semplice ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="0a81c-236">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="0a81c-237">Ad esempio, quando si recupera un'entità del corso di modifica, l'entità Department è null se non si caricarlo, pertanto, quando si aggiorna l'entità course, è necessario prima recuperare l'entità Department.</span><span class="sxs-lookup"><span data-stu-id="0a81c-237">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="0a81c-238">Quando la proprietà di chiave esterna `DepartmentID` viene incluso nel modello di dati, non è necessario recuperare l'entità Department prima di aggiornare.</span><span class="sxs-lookup"><span data-stu-id="0a81c-238">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="0a81c-239">L'attributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="0a81c-239">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="0a81c-240">Il `DatabaseGenerated` attributo con il `None` parametro il `CourseID` proprietà specifica che i valori di chiave primaria vengono forniti dall'utente anziché generati dal database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-240">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="0a81c-241">Per impostazione predefinita, Entity Framework si presuppone che i valori di chiave primaria vengono generati dal database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-241">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="0a81c-242">Che è quello desiderato nella maggior parte degli scenari.</span><span class="sxs-lookup"><span data-stu-id="0a81c-242">That's what you want in most scenarios.</span></span> <span data-ttu-id="0a81c-243">Tuttavia, per le entità Course, verrà usare un numero di corso specificato dall'utente, ad esempio una serie di 1000 per reparto, una serie di 2000 per un altro reparto e così via.</span><span class="sxs-lookup"><span data-stu-id="0a81c-243">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="0a81c-244">Il `DatabaseGenerated` attributo può essere utilizzato anche per generare i valori predefiniti, come nel caso di colonne di database utilizzate per registrare la data è stata creata o aggiornata una riga.</span><span class="sxs-lookup"><span data-stu-id="0a81c-244">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="0a81c-245">Per ulteriori informazioni, vedere [generato proprietà](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="0a81c-245">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="0a81c-246">Proprietà di chiave e la navigazione esterna</span><span class="sxs-lookup"><span data-stu-id="0a81c-246">Foreign key and navigation properties</span></span>

<span data-ttu-id="0a81c-247">La proprietà di chiave esterna e le proprietà di navigazione nell'entità Course rifletteranno le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0a81c-247">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="0a81c-248">Un corso viene assegnato a un reparto, pertanto non c'è un `DepartmentID` chiave esterna e un `Department` proprietà di navigazione per i motivi indicati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="0a81c-248">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="0a81c-249">Un corso può avere qualsiasi numero di studenti, pertanto la `Enrollments` proprietà di navigazione è una raccolta:</span><span class="sxs-lookup"><span data-stu-id="0a81c-249">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="0a81c-250">Un corso può dedicare da istruttori più, pertanto la `CourseAssignments` proprietà di navigazione è una raccolta (il tipo `CourseAssignment` è illustrato [in un secondo momento](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="0a81c-250">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="0a81c-251">Creare l'entità di reparto</span><span class="sxs-lookup"><span data-stu-id="0a81c-251">Create the Department entity</span></span>

![Entità reparto](complex-data-model/_static/department-entity.png)


<span data-ttu-id="0a81c-253">Creare *Models/Department.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0a81c-253">Create *Models/Department.cs* with the following code:</span></span>

<span data-ttu-id="0a81c-254">[!code-csharp[Principale](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-254">[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="0a81c-255">L'attributo di colonna</span><span class="sxs-lookup"><span data-stu-id="0a81c-255">The Column attribute</span></span>

<span data-ttu-id="0a81c-256">In precedenza è stato utilizzato il `Column` attributo per modificare il mapping di nomi di colonna.</span><span class="sxs-lookup"><span data-stu-id="0a81c-256">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="0a81c-257">Nel codice per l'entità Department, il `Column` dell'attributo utilizzato per modificare SQL mapping dei tipi in modo che la colonna verrà definita utilizzando il tipo money di SQL Server nel database:</span><span class="sxs-lookup"><span data-stu-id="0a81c-257">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="0a81c-258">Mapping della colonna in genere non è necessario perché il tipo di dati di SQL Server appropriato in base al tipo CLR definito per la proprietà di sceglie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0a81c-258">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="0a81c-259">CLR `decimal` mapping a un Server SQL del tipo `decimal` tipo.</span><span class="sxs-lookup"><span data-stu-id="0a81c-259">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="0a81c-260">Ma in questo caso si sa che la colonna sarà possibile che contenga gli importi in valuta e il tipo di dati money è più appropriato a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="0a81c-260">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="0a81c-261">Proprietà di chiave e la navigazione esterna</span><span class="sxs-lookup"><span data-stu-id="0a81c-261">Foreign key and navigation properties</span></span>

<span data-ttu-id="0a81c-262">Le proprietà di navigazione e di chiave esterne rifletteranno le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0a81c-262">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="0a81c-263">Un reparto può non avere o un amministratore e un amministratore è sempre un docente.</span><span class="sxs-lookup"><span data-stu-id="0a81c-263">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="0a81c-264">Pertanto il `InstructorID` proprietà è inclusa come chiave esterna per l'entità Instructor e viene aggiunto un punto interrogativo dopo il `int` digitare designazione per contrassegnare la proprietà come nullable.</span><span class="sxs-lookup"><span data-stu-id="0a81c-264">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="0a81c-265">La proprietà di navigazione denominata `Administrator` ma contiene un'entità Instructor:</span><span class="sxs-lookup"><span data-stu-id="0a81c-265">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="0a81c-266">Un reparto può avere molti corsi, pertanto non c'è una proprietà di navigazione corsi:</span><span class="sxs-lookup"><span data-stu-id="0a81c-266">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="0a81c-267">Per convenzione, Entity Framework consente l'eliminazione a catena per chiavi esterne non nullable e per le relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="0a81c-267">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="0a81c-268">Ciò può comportare le regole delete cascade circolare, verrà generata un'eccezione quando si tenta di aggiungere una migrazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-268">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="0a81c-269">Ad esempio, se la proprietà Department.InstructorID non è stato definito come nullable, EF sarebbe configurare una regola di delete cascade per eliminare istruttore quando si elimina il reparto, ovvero non è auspicabile che si verificano.</span><span class="sxs-lookup"><span data-stu-id="0a81c-269">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which is not what you want to have happen.</span></span> <span data-ttu-id="0a81c-270">Se le regole di business è necessario il `InstructorID` proprietà sia non nullable, è necessario utilizzare la seguente istruzione di Microsoft Office fluent API per disattivare eliminazione a catena della relazione:</span><span class="sxs-lookup"><span data-stu-id="0a81c-270">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="0a81c-271">Modificare l'entità di registrazione</span><span class="sxs-lookup"><span data-stu-id="0a81c-271">Modify the Enrollment entity</span></span>

![Entità di registrazione](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="0a81c-273">In *Models/Enrollment.cs*, sostituire il codice aggiunto in precedenza con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0a81c-273">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

<span data-ttu-id="0a81c-274">[!code-csharp[Principale](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-274">[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="0a81c-275">Proprietà di chiave e la navigazione esterna</span><span class="sxs-lookup"><span data-stu-id="0a81c-275">Foreign key and navigation properties</span></span>

<span data-ttu-id="0a81c-276">La proprietà di chiave esterna e le proprietà di navigazione rifletteranno le relazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0a81c-276">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="0a81c-277">È un record di registrazione per un singolo corso, pertanto non c'è un `CourseID` proprietà di chiave esterna e un `Course` proprietà di navigazione:</span><span class="sxs-lookup"><span data-stu-id="0a81c-277">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="0a81c-278">È un record di registrazione per un singolo studente, pertanto non c'è un `StudentID` proprietà di chiave esterna e un `Student` proprietà di navigazione:</span><span class="sxs-lookup"><span data-stu-id="0a81c-278">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="0a81c-279">Relazioni molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="0a81c-279">Many-to-Many Relationships</span></span>

<span data-ttu-id="0a81c-280">Vi è una relazione molti-a-molti tra le entità Student e Course e l'entità di registrazione funziona come una tabella di join molti-a-molti *con payload* nel database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-280">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="0a81c-281">"Con payload" significa che la tabella di registrazione contiene dati aggiuntivi oltre a chiavi esterne per le tabelle unite in join (in questo caso, una chiave primaria e una proprietà di livello).</span><span class="sxs-lookup"><span data-stu-id="0a81c-281">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="0a81c-282">Nella figura seguente viene illustrato l'aspetto queste relazioni in un diagramma di entità.</span><span class="sxs-lookup"><span data-stu-id="0a81c-282">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="0a81c-283">(Questo diagramma è stato generato utilizzando gli strumenti di Entity Framework Power per Entity Framework 6. x; si crea il diagramma non fa parte dell'esercitazione, appena viene utilizzato come un'illustrazione.)</span><span class="sxs-lookup"><span data-stu-id="0a81c-283">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Corso di studenti molti-a-molti relazione](complex-data-model/_static/student-course.png)

<span data-ttu-id="0a81c-285">Ogni linea della relazione dispone di un 1 al fine di uno e un asterisco (*) a altra, per indicare una relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="0a81c-285">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="0a81c-286">Se la tabella di registrazione non include informazioni di livello, sarebbe sufficiente contenere le due chiavi esterne CourseID e StudentID.</span><span class="sxs-lookup"><span data-stu-id="0a81c-286">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="0a81c-287">In tal caso, sarebbe una tabella di join molti-a-molti senza payload (o una tabella di join pure) del database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-287">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="0a81c-288">Le entità Instructor e corso dispongono di questo tipo di relazione molti-a-molti e il passaggio successivo consiste nel creare una classe di entità per fungere da una tabella di join senza payload.</span><span class="sxs-lookup"><span data-stu-id="0a81c-288">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="0a81c-289">(Supporta Entity Framework 6. x tabelle join implicito per le relazioni molti-a-molti, ma Core EF non.</span><span class="sxs-lookup"><span data-stu-id="0a81c-289">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="0a81c-290">Per ulteriori informazioni, vedere il [discussione nel repository GitHub Core EF](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="0a81c-290">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="0a81c-291">L'entità CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="0a81c-291">The CourseAssignment entity</span></span>

![Entità CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="0a81c-293">Creare *Models/CourseAssignment.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0a81c-293">Create *Models/CourseAssignment.cs* with the following code:</span></span>

<span data-ttu-id="0a81c-294">[!code-csharp[Principale](intro/samples/cu/Models/CourseAssignment.cs)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-294">[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]</span></span>

### <a name="join-entity-names"></a><span data-ttu-id="0a81c-295">Aggiungere i nomi delle entità</span><span class="sxs-lookup"><span data-stu-id="0a81c-295">Join entity names</span></span>

<span data-ttu-id="0a81c-296">È necessaria una tabella di join del database per la relazione molti-a-molti per istruttore-corsi e deve essere rappresentato da un set di entità.</span><span class="sxs-lookup"><span data-stu-id="0a81c-296">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="0a81c-297">È comune per assegnare un nome di un'entità di join `EntityName1EntityName2`, che in questo caso sarebbe `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="0a81c-297">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="0a81c-298">Tuttavia, è consigliabile scegliere un nome che descrive la relazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-298">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="0a81c-299">I modelli di dati inizia semplice e aumento delle dimensioni, con join no payload spesso recupero payload più tardi.</span><span class="sxs-lookup"><span data-stu-id="0a81c-299">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="0a81c-300">Se si inizia con un nome descrittivo di entità, non è necessario modificare il nome in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="0a81c-300">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="0a81c-301">Idealmente, l'entità di join avrebbe il nome del relativo naturale (possibilmente singola parola) nel dominio aziendale.</span><span class="sxs-lookup"><span data-stu-id="0a81c-301">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="0a81c-302">Documentazione e i clienti, ad esempio, potrebbero essere collegati tramite classificazioni.</span><span class="sxs-lookup"><span data-stu-id="0a81c-302">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="0a81c-303">Per questa relazione, `CourseAssignment` è una soluzione migliore rispetto alla `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="0a81c-303">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="0a81c-304">Chiave composta</span><span class="sxs-lookup"><span data-stu-id="0a81c-304">Composite key</span></span>

<span data-ttu-id="0a81c-305">Poiché le chiavi esterne non sono nullable e insieme in modo univoco ogni riga della tabella, non è necessario per una chiave primaria separata.</span><span class="sxs-lookup"><span data-stu-id="0a81c-305">Since the foreign keys are not nullable and together uniquely identify each row of the table, there is no need for a separate primary key.</span></span> <span data-ttu-id="0a81c-306">Il *InstructorID* e *CourseID* proprietà dovrebbero funzionare come una chiave primaria composta.</span><span class="sxs-lookup"><span data-stu-id="0a81c-306">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="0a81c-307">È l'unico modo per identificare le chiavi primarie composte per Entity Framework utilizzando la *API fluent* (non può essere eseguita con gli attributi).</span><span class="sxs-lookup"><span data-stu-id="0a81c-307">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="0a81c-308">Si noterà come configurare la chiave primaria composta nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="0a81c-308">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="0a81c-309">Chiave composta assicura che possono essere presenti più righe per un corso e più righe per un insegnante, non può avere più righe per la stessa instructor e corso.</span><span class="sxs-lookup"><span data-stu-id="0a81c-309">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="0a81c-310">Il `Enrollment` join entità definisce la propria chiave primaria, sono possibili i duplicati di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="0a81c-310">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="0a81c-311">Per evitare tali duplicati, è possibile aggiungere un indice univoco per i campi di chiave esterni o configurare `Enrollment` con una chiave primaria composta simile a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0a81c-311">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="0a81c-312">Per ulteriori informazioni, vedere [indici](https://docs.efproject.net/en/latest/modeling/indexes.html).</span><span class="sxs-lookup"><span data-stu-id="0a81c-312">For more information, see [Indexes](https://docs.efproject.net/en/latest/modeling/indexes.html).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="0a81c-313">Aggiornare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="0a81c-313">Update the database context</span></span>

<span data-ttu-id="0a81c-314">Aggiungere il codice evidenziato di seguito per il *Data/SchoolContext.cs* file:</span><span class="sxs-lookup"><span data-stu-id="0a81c-314">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

<span data-ttu-id="0a81c-315">[!code-csharp[Principale](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-315">[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]</span></span>

<span data-ttu-id="0a81c-316">Questo codice aggiunge le nuove entità e configura chiave primaria composta CourseAssignment dell'entità.</span><span class="sxs-lookup"><span data-stu-id="0a81c-316">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="0a81c-317">In alternativa Fluent API per gli attributi</span><span class="sxs-lookup"><span data-stu-id="0a81c-317">Fluent API alternative to attributes</span></span>

<span data-ttu-id="0a81c-318">Il codice nel `OnModelCreating` metodo il `DbContext` utilizzato dalla classe il *fluent API* per configurare il comportamento di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0a81c-318">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="0a81c-319">L'API viene denominata "Microsoft Office fluent" perché viene usata spesso mettendo una serie di chiamate di metodo insieme in una singola istruzione, come in questo esempio dal [documentazione principale di EF](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="0a81c-319">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="0a81c-320">In questa esercitazione, si utilizza l'API fluent solo per mapping del database che si può fare con gli attributi.</span><span class="sxs-lookup"><span data-stu-id="0a81c-320">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="0a81c-321">Tuttavia, è possibile utilizzare anche l'API fluent per specificare la maggior parte della formattazione, convalida e regole di mapping che è possibile eseguire utilizzando gli attributi.</span><span class="sxs-lookup"><span data-stu-id="0a81c-321">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="0a81c-322">Alcuni attributi, ad esempio `MinimumLength` non può essere applicata con l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="0a81c-322">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="0a81c-323">Come accennato in precedenza, `MinimumLength` non modifica lo schema, si applica solo una regola di convalida sul lato client e server.</span><span class="sxs-lookup"><span data-stu-id="0a81c-323">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="0a81c-324">Alcuni sviluppatori preferiscono utilizzare l'API fluent esclusivamente in modo che mantengano le classi di entità "pulito".</span><span class="sxs-lookup"><span data-stu-id="0a81c-324">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="0a81c-325">È possibile combinare gli attributi e l'API fluent se si desidera, e sono disponibili alcune personalizzazioni che è possono solo tramite l'API fluent, ma in genere di è consigliabile scegliere uno di questi due approcci e usare in che modo coerente il più possibile.</span><span class="sxs-lookup"><span data-stu-id="0a81c-325">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="0a81c-326">Se si utilizzano entrambi, si noti che ogni volta che si verifica un conflitto, Microsoft Office Fluent API esegue l'override di attributi.</span><span class="sxs-lookup"><span data-stu-id="0a81c-326">If you do use both, note that wherever there is a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="0a81c-327">Per ulteriori informazioni sugli attributi e Microsoft Office fluent API, vedere [metodi di configurazione](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="0a81c-327">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="0a81c-328">Entità diagramma che illustra le relazioni</span><span class="sxs-lookup"><span data-stu-id="0a81c-328">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="0a81c-329">Nella figura seguente viene illustrato il diagramma che strumenti di Entity Framework Power creare per il modello School completato.</span><span class="sxs-lookup"><span data-stu-id="0a81c-329">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Diagramma di entità](complex-data-model/_static/diagram.png)

<span data-ttu-id="0a81c-331">Oltre alle linee di relazione uno-a-molti (da 1 a \*), sarà possibile visualizzare la linea della relazione uno-a-zero-o-uno (da 1 a 0..1) tra le entità Instructor e OfficeAssignment e la riga zero-o-relazione uno-a-molti (0..1 a *) tra il Entità Instructor e Department.</span><span class="sxs-lookup"><span data-stu-id="0a81c-331">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="0a81c-332">Valore di inizializzazione del Database con dati di Test</span><span class="sxs-lookup"><span data-stu-id="0a81c-332">Seed the Database with Test Data</span></span>

<span data-ttu-id="0a81c-333">Sostituire il codice di *Data/DbInitializer.cs* file con il codice seguente per fornire i dati iniziali per le nuove entità creata.</span><span class="sxs-lookup"><span data-stu-id="0a81c-333">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

<span data-ttu-id="0a81c-334">[!code-csharp[Principale](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-334">[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]</span></span>

<span data-ttu-id="0a81c-335">Come osservato nella prima esercitazione, la maggior parte di questo codice crea nuovi oggetti entità e carica i dati di esempio nella proprietà come necessario per i test.</span><span class="sxs-lookup"><span data-stu-id="0a81c-335">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="0a81c-336">Si noti la modalità di gestione delle relazioni molti-a-molti: il codice crea relazioni tramite la creazione di entità di `Enrollments` e `CourseAssignment` aggiungere set di entità.</span><span class="sxs-lookup"><span data-stu-id="0a81c-336">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="0a81c-337">Aggiungere una migrazione</span><span class="sxs-lookup"><span data-stu-id="0a81c-337">Add a migration</span></span>

<span data-ttu-id="0a81c-338">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="0a81c-338">Save your changes and build the project.</span></span> <span data-ttu-id="0a81c-339">Aprire la finestra di comando nella cartella del progetto, quindi immettere il `migrations add` comando (non eseguire il comando update-database ancora):</span><span class="sxs-lookup"><span data-stu-id="0a81c-339">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="0a81c-340">Viene visualizzato un avviso sulla possibile perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="0a81c-340">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="0a81c-341">Se si è tentato di eseguire il `database update` comando a questo punto (non ancora), si otterrebbe il seguente errore:</span><span class="sxs-lookup"><span data-stu-id="0a81c-341">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="0a81c-342">L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "FK_dbo. Course_dbo. Department_DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="0a81c-342">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="0a81c-343">Si è verificato il conflitto in una tabella del database "ContosoUniversity", "dbo. Reparto", colonna 'DepartmentID'.</span><span class="sxs-lookup"><span data-stu-id="0a81c-343">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="0a81c-344">A volte quando si esegue con dati esistenti, è necessario inserire dati stub nel database per soddisfare i vincoli di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="0a81c-344">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="0a81c-345">Il codice generato nei componenti di `Up` metodo aggiunge una chiave esterna a DepartmentID non ammette valori null nella tabella Course.</span><span class="sxs-lookup"><span data-stu-id="0a81c-345">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="0a81c-346">Se sono già presenti righe nella tabella Course quando viene eseguito il codice, il `AddColumn` operazione non riesce perché SQL Server non riconosce il valore da inserire nella colonna che non può essere null.</span><span class="sxs-lookup"><span data-stu-id="0a81c-346">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="0a81c-347">Per questa esercitazione viene eseguita la migrazione in un nuovo database, ma in un'applicazione di produzione è necessario effettuare la migrazione di gestire i dati esistenti, pertanto le indicazioni riportate di seguito viene illustrato un esempio di come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-347">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="0a81c-348">Per rendere questa migrazione lavorare con i dati esistenti, che è necessario modificare il codice per fornire la nuova colonna un valore predefinito e creare un reparto stipendi denominata "Temp" come il reparto predefinito.</span><span class="sxs-lookup"><span data-stu-id="0a81c-348">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="0a81c-349">Di conseguenza, esistente righe corso saranno tutti correlati al reparto "Temp" dopo il `Up` esecuzione del metodo.</span><span class="sxs-lookup"><span data-stu-id="0a81c-349">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="0a81c-350">Aprire il *{timestamp}_ComplexDataModel.cs* file.</span><span class="sxs-lookup"><span data-stu-id="0a81c-350">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="0a81c-351">Impostare come commento la riga di codice che aggiunge la colonna DepartmentID è presente nella tabella Course.</span><span class="sxs-lookup"><span data-stu-id="0a81c-351">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  <span data-ttu-id="0a81c-352">[!code-csharp[Principale](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-352">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]</span></span>

* <span data-ttu-id="0a81c-353">Dopo il codice che crea la tabella di reparto, aggiungere il codice evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0a81c-353">Add the following highlighted code after the code that creates the Department table:</span></span>

  <span data-ttu-id="0a81c-354">[!code-csharp[Principale](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="0a81c-354">[!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="0a81c-355">In un'applicazione di produzione, è necessario scrivere codice o script per aggiungere righe di reparto e correlare righe corso per le nuove righe di reparto.</span><span class="sxs-lookup"><span data-stu-id="0a81c-355">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="0a81c-356">È quindi non è più necessario il reparto "Temp" o il valore predefinito nella colonna Course.DepartmentID.</span><span class="sxs-lookup"><span data-stu-id="0a81c-356">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="0a81c-357">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="0a81c-357">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="0a81c-358">Modificare la stringa di connessione e aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="0a81c-358">Change the connection string and update the database</span></span>

<span data-ttu-id="0a81c-359">Ora è nuovo codice `DbInitializer` classe che aggiunge i dati iniziali per le nuove entità a un database vuoto.</span><span class="sxs-lookup"><span data-stu-id="0a81c-359">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="0a81c-360">Per creare un nuovo database vuoto EF, modificare il nome del database nella stringa di connessione in *appSettings. JSON* ContosoUniversity3 o un altro nome che non è stato utilizzato nel computer in uso.</span><span class="sxs-lookup"><span data-stu-id="0a81c-360">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="0a81c-361">Salvare le modifiche apportate ai *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="0a81c-361">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="0a81c-362">Come alternativa alla modifica del nome di database, è possibile eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-362">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="0a81c-363">Utilizzare **Esplora oggetti di SQL Server** (sillaba SSOX) o `database drop` comando CLI:</span><span class="sxs-lookup"><span data-stu-id="0a81c-363">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="0a81c-364">Dopo aver modificato il nome del database o eliminare il database, eseguire il `database update` comando nella finestra di comando per eseguire la migrazione.</span><span class="sxs-lookup"><span data-stu-id="0a81c-364">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="0a81c-365">Eseguire l'app affinché i `DbInitializer.Initialize` metodo per eseguire e popolare il nuovo database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-365">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="0a81c-366">Aprire il database in sillaba SSOX come fatto in precedenza ed espandere il **tabelle** nodo per visualizzare tutte le tabelle che sono state create.</span><span class="sxs-lookup"><span data-stu-id="0a81c-366">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="0a81c-367">(Se è ancora aperto sillaba SSOX dal momento precedente, fare clic sul pulsante Aggiorna).</span><span class="sxs-lookup"><span data-stu-id="0a81c-367">(If you still have SSOX open from the earlier time, click the Refresh button.)</span></span>

![Tabelle in sillaba SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="0a81c-369">Eseguire l'applicazione per attivare l'inizializzatore di codice che esegue il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="0a81c-369">Run the application to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="0a81c-370">Fare doppio clic su di **CourseAssignment** tabella e selezionare **Visualizza dati** per verificare che disponga di dati in essa contenuti.</span><span class="sxs-lookup"><span data-stu-id="0a81c-370">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![Dati CourseAssignment sillaba SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="0a81c-372">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="0a81c-372">Summary</span></span>

<span data-ttu-id="0a81c-373">È ora disponibile un modello di dati più complesso e il database corrispondente.</span><span class="sxs-lookup"><span data-stu-id="0a81c-373">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="0a81c-374">Nell'esercitazione seguente, si apprenderà ulteriori informazioni su come accedere ai dati correlati.</span><span class="sxs-lookup"><span data-stu-id="0a81c-374">In the following tutorial, you'll learn more about how to access related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0a81c-375">[Precedente](migrations.md)
[Successivo](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="0a81c-375">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  
