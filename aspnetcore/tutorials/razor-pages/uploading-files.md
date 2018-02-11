---
title: Caricamento di file in una pagina Razor in ASP.NET Core
author: guardrex
description: Informazioni su come caricare i file in una pagina Razor.
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 4a2c6da6ed698d1a65ee51bd00a557e607f012da
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="0832b-103">Caricamento di file in una pagina Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0832b-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="0832b-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0832b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0832b-105">In questa sezione viene illustrato il caricamento di file con una pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="0832b-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="0832b-106">In questa esercitazione l'[app di esempio Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) per caricare i file usa l'associazione di modelli semplice che funziona correttamente per file di piccole dimensioni.</span><span class="sxs-lookup"><span data-stu-id="0832b-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="0832b-107">Per informazioni sulla trasmissione di file di grandi dimensioni, vedere [Caricamento di file di grandi dimensioni con il flusso](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="0832b-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="0832b-108">Nella procedura descritta di seguito viene aggiunta una funzionalità di caricamento di file di pianificazione di film all'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="0832b-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="0832b-109">Ogni pianificazione di film è rappresentata da una classe `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="0832b-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="0832b-110">La classe include due versioni della pianificazione.</span><span class="sxs-lookup"><span data-stu-id="0832b-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="0832b-111">La versione `PublicSchedule` viene messa a disposizione dei clienti.</span><span class="sxs-lookup"><span data-stu-id="0832b-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="0832b-112">L'altra versione, `PrivateSchedule`, viene usata per i dipendenti della società.</span><span class="sxs-lookup"><span data-stu-id="0832b-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="0832b-113">Le versioni vengono caricate come file separati.</span><span class="sxs-lookup"><span data-stu-id="0832b-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="0832b-114">Nell'esercitazione viene illustrato come eseguire il caricamento di due file da una pagina con un solo invio al server.</span><span class="sxs-lookup"><span data-stu-id="0832b-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="0832b-115">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="0832b-115">Security considerations</span></span>

<span data-ttu-id="0832b-116">Prestare attenzione quando si offre agli utenti la possibilità di caricare file in un server.</span><span class="sxs-lookup"><span data-stu-id="0832b-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="0832b-117">Gli utenti malintenzionati possono eseguire attacchi [Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) e di altro tipo su un sistema.</span><span class="sxs-lookup"><span data-stu-id="0832b-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="0832b-118">Alcuni passaggi di sicurezza che riducono le probabilità di riuscita degli attacchi sono:</span><span class="sxs-lookup"><span data-stu-id="0832b-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="0832b-119">Caricare i file in un'area di caricamento dedicata nel sistema, in modo da poter imporre più facilmente misure di sicurezza per il contenuto caricato.</span><span class="sxs-lookup"><span data-stu-id="0832b-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="0832b-120">Quando si permettono caricamenti di file, assicurarsi che le autorizzazioni di esecuzione siano disabilitate nella posizione di caricamento.</span><span class="sxs-lookup"><span data-stu-id="0832b-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="0832b-121">Usare nomi di file sicuri determinati dall'app e non dall'input dell'utente o non usare il nome del file caricato.</span><span class="sxs-lookup"><span data-stu-id="0832b-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="0832b-122">Consentire solo un set specifico di estensioni di file approvate.</span><span class="sxs-lookup"><span data-stu-id="0832b-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="0832b-123">Verificare che vengano eseguiti controlli sul lato client nel server.</span><span class="sxs-lookup"><span data-stu-id="0832b-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="0832b-124">I controlli sul lato client sono facili da aggirare.</span><span class="sxs-lookup"><span data-stu-id="0832b-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="0832b-125">Controllare le dimensioni del caricamento e impedire caricamenti di dimensioni maggiori del previsto.</span><span class="sxs-lookup"><span data-stu-id="0832b-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="0832b-126">Eseguire un rilevatore di virus/malware sul contenuto caricato.</span><span class="sxs-lookup"><span data-stu-id="0832b-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="0832b-127">Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:</span><span class="sxs-lookup"><span data-stu-id="0832b-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="0832b-128">Prendere il controllo totale di un sistema.</span><span class="sxs-lookup"><span data-stu-id="0832b-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="0832b-129">Sovraccaricare un sistema causandone il malfunzionamento completo.</span><span class="sxs-lookup"><span data-stu-id="0832b-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="0832b-130">Compromettere i dati di sistemi o utenti.</span><span class="sxs-lookup"><span data-stu-id="0832b-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="0832b-131">Applicare graffiti a un'interfaccia pubblica (defacing).</span><span class="sxs-lookup"><span data-stu-id="0832b-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="0832b-132">Aggiungere una classe FileUpload</span><span class="sxs-lookup"><span data-stu-id="0832b-132">Add a FileUpload class</span></span>

<span data-ttu-id="0832b-133">Creare una pagina Razor per gestire una coppia di caricamenti di file.</span><span class="sxs-lookup"><span data-stu-id="0832b-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="0832b-134">Aggiungere una classe `FileUpload`, che è associata alla pagina per ottenere i dati di pianificazione.</span><span class="sxs-lookup"><span data-stu-id="0832b-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="0832b-135">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="0832b-135">Right click the *Models* folder.</span></span> <span data-ttu-id="0832b-136">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="0832b-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="0832b-137">Denominare la classe **FileUpload** e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="0832b-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="0832b-138">La classe ha una proprietà per il titolo della pianificazione e una proprietà per ognuna delle due versioni della pianificazione.</span><span class="sxs-lookup"><span data-stu-id="0832b-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="0832b-139">Tutte e tre le proprietà sono obbligatorie e il titolo deve avere una lunghezza compresa fra 3 e 60 caratteri.</span><span class="sxs-lookup"><span data-stu-id="0832b-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="0832b-140">Aggiungere un metodo helper per caricare file</span><span class="sxs-lookup"><span data-stu-id="0832b-140">Add a helper method to upload files</span></span>

<span data-ttu-id="0832b-141">Per evitare la duplicazione del codice per l'elaborazione dei file di pianificazione caricati, è necessario per prima cosa aggiungere un metodo helper statico.</span><span class="sxs-lookup"><span data-stu-id="0832b-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="0832b-142">Creare una cartella *Utilità* nell'app e aggiungere un file *FileHelpers.cs* con il contenuto seguente.</span><span class="sxs-lookup"><span data-stu-id="0832b-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="0832b-143">Il metodo helper, `ProcessFormFile`, accetta [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) e restituisce una stringa contenente le dimensioni e il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="0832b-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="0832b-144">Il tipo di contenuto e la lunghezza vengono controllati.</span><span class="sxs-lookup"><span data-stu-id="0832b-144">The content type and length are checked.</span></span> <span data-ttu-id="0832b-145">Se il file non supera un controllo di convalida, viene aggiunto un errore a `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="0832b-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="0832b-146">Salvare il file su disco</span><span class="sxs-lookup"><span data-stu-id="0832b-146">Save the file to disk</span></span>

<span data-ttu-id="0832b-147">L'app di esempio salva il contenuto del file in un campo del database.</span><span class="sxs-lookup"><span data-stu-id="0832b-147">The sample app saves the file's content into a database field.</span></span> <span data-ttu-id="0832b-148">Per salvare il contenuto del file su disco, usare un [FileStream](/dotnet/api/system.io.filestream):</span><span class="sxs-lookup"><span data-stu-id="0832b-148">To save the file's content to disk, use a [FileStream](/dotnet/api/system.io.filestream):</span></span>

```csharp
using (var fileStream = new FileStream(filePath, FileMode.Create))
{
    await formFile.CopyToAsync(fileStream);
}
```

<span data-ttu-id="0832b-149">Il processo di lavoro deve disporre delle autorizzazioni di scrittura per il percorso specificato da `filePath`.</span><span class="sxs-lookup"><span data-stu-id="0832b-149">The worker process must have write permissions to the location specified by `filePath`.</span></span>

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="0832b-150">Salvare il file in Archiviazione BLOB di Azure</span><span class="sxs-lookup"><span data-stu-id="0832b-150">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="0832b-151">Per caricare il contenuto del file in Archiviazione Blob di Azure, vedere [Introduzione all'archivio BLOB di Azure con .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="0832b-151">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="0832b-152">Questo argomento dimostra come usare [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) per salvare un [FileStream](/dotnet/api/system.io.filestream) nell'archivio BLOB.</span><span class="sxs-lookup"><span data-stu-id="0832b-152">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="0832b-153">Aggiungere la classe Schedule</span><span class="sxs-lookup"><span data-stu-id="0832b-153">Add the Schedule class</span></span>

<span data-ttu-id="0832b-154">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="0832b-154">Right click the *Models* folder.</span></span> <span data-ttu-id="0832b-155">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="0832b-155">Select **Add** > **Class**.</span></span> <span data-ttu-id="0832b-156">Denominare la classe **Schedule** e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="0832b-156">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="0832b-157">La classe usa gli attributi `Display` e `DisplayFormat`, che producono titoli descrittivi e la formattazione quando viene eseguito il rendering dei dati di pianificazione.</span><span class="sxs-lookup"><span data-stu-id="0832b-157">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="0832b-158">Aggiornare MovieContext</span><span class="sxs-lookup"><span data-stu-id="0832b-158">Update the MovieContext</span></span>

<span data-ttu-id="0832b-159">Specificare un `DbSet` in `MovieContext` (*Models/MovieContext.cs*) per le pianificazioni:</span><span class="sxs-lookup"><span data-stu-id="0832b-159">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="0832b-160">Aggiungere la tabella Schedule al database</span><span class="sxs-lookup"><span data-stu-id="0832b-160">Add the Schedule table to the database</span></span>

<span data-ttu-id="0832b-161">Aprire la Console di Gestione pacchetti: **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="0832b-161">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="0832b-163">Nella console di Gestione pacchetti eseguire i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="0832b-163">In the PMC, execute the following commands.</span></span> <span data-ttu-id="0832b-164">In questo modo viene aggiunta una tabella `Schedule` al database:</span><span class="sxs-lookup"><span data-stu-id="0832b-164">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="0832b-165">Aggiungere una pagina Razor di caricamento file</span><span class="sxs-lookup"><span data-stu-id="0832b-165">Add a file upload Razor Page</span></span>

<span data-ttu-id="0832b-166">Nella cartella *Pages* creare una cartella *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="0832b-166">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="0832b-167">Nella cartella *Schedules* creare una pagina denominata *Index.cshtml* per il caricamento di una pianificazione con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="0832b-167">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="0832b-168">Ogni gruppo di moduli include un oggetto **\<label>** ovvero un'etichetta che visualizza il nome di ogni proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="0832b-168">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="0832b-169">Gli attributi `Display` del modello `FileUpload` specificano i valori di visualizzazione per le etichette.</span><span class="sxs-lookup"><span data-stu-id="0832b-169">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="0832b-170">Ad esempio, il nome visualizzato della proprietà `UploadPublicSchedule` è impostato con `[Display(Name="Public Schedule")]` e pertanto viene visualizzato "Public Schedule" nell'etichetta quando viene eseguito il rendering del modulo.</span><span class="sxs-lookup"><span data-stu-id="0832b-170">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="0832b-171">Ogni gruppo di moduli include una convalida **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="0832b-171">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="0832b-172">Se l'input dell'utente non soddisfa gli attributi della proprietà impostati nella classe `FileUpload` o se uno dei controlli della convalida file del metodo `ProcessFormFile` ha esito negativo, il modello non viene convalidato.</span><span class="sxs-lookup"><span data-stu-id="0832b-172">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="0832b-173">Quando il modello non viene convalidato, viene eseguito il rendering di un messaggio di convalida utile per l'utente.</span><span class="sxs-lookup"><span data-stu-id="0832b-173">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="0832b-174">Ad esempio, la proprietà `Title` viene annotata con `[Required]` e `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="0832b-174">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="0832b-175">Se l'utente non specifica un titolo, riceve un messaggio che indica che è necessario specificare un valore.</span><span class="sxs-lookup"><span data-stu-id="0832b-175">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="0832b-176">Se l'utente immette un valore inferiore a tre caratteri o superiore a sessanta, riceve un messaggio che indica che il valore ha una lunghezza non corretta.</span><span class="sxs-lookup"><span data-stu-id="0832b-176">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="0832b-177">Se viene specificato un file che non presenta alcun contenuto, viene visualizzato un messaggio che indica che il file è vuoto.</span><span class="sxs-lookup"><span data-stu-id="0832b-177">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="0832b-178">Aggiungere il modello di pagina</span><span class="sxs-lookup"><span data-stu-id="0832b-178">Add the page model</span></span>

<span data-ttu-id="0832b-179">Aggiungere il modello di pagina *Index.cshtml.cs* nella cartella *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="0832b-179">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="0832b-180">Il modello di pagina (`IndexModel` in *Index.cshtml.cs*) associa la classe `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="0832b-180">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0832b-181">Il modello usa anche un elenco delle pianificazioni, `IList<Schedule>`, per visualizzare le pianificazioni archiviate nel database nella pagina:</span><span class="sxs-lookup"><span data-stu-id="0832b-181">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="0832b-182">Quando la pagina viene caricata con `OnGetAsync`, la tabella `Schedules` viene popolata dal database e usata per generare una tabella HTML di pianificazioni caricate:</span><span class="sxs-lookup"><span data-stu-id="0832b-182">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="0832b-183">Quando il modulo viene pubblicato nel server, viene eseguito il controllo di `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="0832b-183">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="0832b-184">Se la convalida dà esito negativo, `Schedule` viene ricompilata, e il rendering della pagina genera uno o più messaggi di convalida che indicano il motivo per cui la convalida non è riuscita.</span><span class="sxs-lookup"><span data-stu-id="0832b-184">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="0832b-185">Se la convalida ha esisto positivo, le proprietà `FileUpload` vengono usate in *OnPostAsync* per completare il caricamento del file per le due versioni della pianificazione e per creare un nuovo oggetto `Schedule` per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="0832b-185">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="0832b-186">La pianificazione viene quindi salvata nel database:</span><span class="sxs-lookup"><span data-stu-id="0832b-186">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="0832b-187">Aggiungere un collegamento alla pagina Razor di caricamento file</span><span class="sxs-lookup"><span data-stu-id="0832b-187">Link the file upload Razor Page</span></span>

<span data-ttu-id="0832b-188">Aprire *_Layout.cshtml* e aggiungere un collegamento alla barra di navigazione per accedere alla pagina di caricamento dei file:</span><span class="sxs-lookup"><span data-stu-id="0832b-188">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="0832b-189">Aggiungere una pagina per confermare l'eliminazione della pianificazione</span><span class="sxs-lookup"><span data-stu-id="0832b-189">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="0832b-190">Quando l'utente fa clic per eliminare una pianificazione, viene offerta la possibilità di annullare l'operazione.</span><span class="sxs-lookup"><span data-stu-id="0832b-190">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="0832b-191">Aggiungere una pagina di conferma dell'eliminazione (*Delete.cshtml*) nella cartella *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="0832b-191">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="0832b-192">Il modello di pagina *Delete.cshtml.cs* carica una singola pianificazione identificata da `id` nei dati della route della richiesta.</span><span class="sxs-lookup"><span data-stu-id="0832b-192">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="0832b-193">Aggiungere il file *Delete.cshtml.cs* nella cartella *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="0832b-193">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="0832b-194">Il metodo `OnPostAsync` gestisce l'eliminazione della pianificazione tramite `id`:</span><span class="sxs-lookup"><span data-stu-id="0832b-194">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="0832b-195">Dopo aver completato l'eliminazione della pianificazione, `RedirectToPage` invia nuovamente l'utente alla pagina delle pianificazioni *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0832b-195">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="0832b-196">La pagina Razor Schedules operativa</span><span class="sxs-lookup"><span data-stu-id="0832b-196">The working Schedules Razor Page</span></span>

<span data-ttu-id="0832b-197">Quando la pagina viene caricata, il rendering delle etichette e degli input per il titolo della pianificazione, della pianificazione pubblica e della pianificazione privata viene eseguito con un pulsante di invio:</span><span class="sxs-lookup"><span data-stu-id="0832b-197">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![La pagina Razor Schedules come viene visualizzata al caricamento iniziale in assenza di errori di convalida e con i campi vuoti](uploading-files/_static/browser1.png)

<span data-ttu-id="0832b-199">Se si seleziona il pulsante **Carica** senza aver popolato i campi, vengono violati gli attributi `[Required]` nel modello.</span><span class="sxs-lookup"><span data-stu-id="0832b-199">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="0832b-200">`ModelState` non è valido.</span><span class="sxs-lookup"><span data-stu-id="0832b-200">The `ModelState` is invalid.</span></span> <span data-ttu-id="0832b-201">I messaggi di errore di convalida vengono visualizzati all'utente:</span><span class="sxs-lookup"><span data-stu-id="0832b-201">The validation error messages are displayed to the user:</span></span>

![I messaggi di errore di convalida vengono visualizzati accanto a ogni controllo input](uploading-files/_static/browser2.png)

<span data-ttu-id="0832b-203">Digitare due lettere nel campo **Titolo**.</span><span class="sxs-lookup"><span data-stu-id="0832b-203">Type two letters into the **Title** field.</span></span> <span data-ttu-id="0832b-204">Il messaggio di convalida cambia e indica che il titolo deve avere una lunghezza compresa tra 3 e 60 caratteri:</span><span class="sxs-lookup"><span data-stu-id="0832b-204">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Messaggio di convalida titolo modificato](uploading-files/_static/browser3.png)

<span data-ttu-id="0832b-206">Quando vengono caricate una o più pianificazioni, la sezione **Loaded Schedules** (Pianificazioni caricate) esegue il rendering delle pianificazioni caricate:</span><span class="sxs-lookup"><span data-stu-id="0832b-206">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabella delle pianificazioni caricate che include il titolo di ogni pianificazione, la data di caricamento con l'ora UTC, le dimensioni del file in versione pubblica e in versione privata](uploading-files/_static/browser4.png)

<span data-ttu-id="0832b-208">L'utente può scegliere il collegamento **Eliminazione** per accedere alla schermata di conferma dell'eliminazione, da dove è possibile confermare o annullare l'operazione.</span><span class="sxs-lookup"><span data-stu-id="0832b-208">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0832b-209">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="0832b-209">Troubleshooting</span></span>

<span data-ttu-id="0832b-210">Per risolvere i problemi relativi al caricamento di `IFormFile`, vedere [Caricamento di file in ASP.NET Core: Risoluzione dei problemi](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="0832b-210">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="0832b-211">Grazie aver completato questa introduzione alle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="0832b-211">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="0832b-212">I suggerimenti degli utenti sono importanti.</span><span class="sxs-lookup"><span data-stu-id="0832b-212">We appreciate feedback.</span></span> <span data-ttu-id="0832b-213">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) (Introduzione a MVC ed Entity Framework Core) è un ottimo completamento per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="0832b-213">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0832b-214">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0832b-214">Additional resources</span></span>

* [<span data-ttu-id="0832b-215">Caricamento di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0832b-215">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="0832b-216">IFormFile</span><span class="sxs-lookup"><span data-stu-id="0832b-216">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="0832b-217">Articolo Precedente: Convalida</span><span class="sxs-lookup"><span data-stu-id="0832b-217">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
