---
title: Caricare file in una pagina Razor in ASP.NET Core
author: guardrex
description: Informazioni su come caricare file in una pagina Razor in ASP.NET Core usando la classe FileUpload.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 11/10/2018
ms.custom: mvc, seodec18
uid: razor-pages/upload-files
ms.openlocfilehash: a12653e5dc99ad731de63e151af6dbdbe38ffefc
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165222"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="37767-103">Caricare file in una pagina Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37767-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="37767-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="37767-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="37767-105">Questo argomento si basa il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span><span class="sxs-lookup"><span data-stu-id="37767-105">This topic builds upon the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span></span>

<span data-ttu-id="37767-106">Questo argomento illustra come usare l'associazione di modelli semplice per caricare i file, che funziona bene per il caricamento di file di piccole dimensioni.</span><span class="sxs-lookup"><span data-stu-id="37767-106">This topic shows how to use simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="37767-107">Per informazioni sulla trasmissione di file di grandi dimensioni, vedere [Caricamento di file di grandi dimensioni con il flusso](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="37767-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="37767-108">Nella procedura descritta di seguito viene aggiunta una funzionalità di caricamento di file di pianificazione di film all'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="37767-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="37767-109">Ogni pianificazione di film è rappresentata da una classe `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="37767-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="37767-110">La classe include due versioni della pianificazione.</span><span class="sxs-lookup"><span data-stu-id="37767-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="37767-111">La versione `PublicSchedule` viene messa a disposizione dei clienti.</span><span class="sxs-lookup"><span data-stu-id="37767-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="37767-112">L'altra versione, `PrivateSchedule`, viene usata per i dipendenti della società.</span><span class="sxs-lookup"><span data-stu-id="37767-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="37767-113">Le versioni vengono caricate come file separati.</span><span class="sxs-lookup"><span data-stu-id="37767-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="37767-114">Nell'esercitazione viene illustrato come eseguire il caricamento di due file da una pagina con un solo invio al server.</span><span class="sxs-lookup"><span data-stu-id="37767-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

<span data-ttu-id="37767-115">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37767-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="37767-116">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="37767-116">Security considerations</span></span>

<span data-ttu-id="37767-117">Prestare attenzione quando si offre agli utenti la possibilità di caricare file in un server.</span><span class="sxs-lookup"><span data-stu-id="37767-117">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="37767-118">Gli utenti malintenzionati possono eseguire attacchi [Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) e di altro tipo su un sistema.</span><span class="sxs-lookup"><span data-stu-id="37767-118">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="37767-119">Alcuni passaggi di sicurezza che riducono le probabilità di riuscita degli attacchi sono:</span><span class="sxs-lookup"><span data-stu-id="37767-119">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="37767-120">Caricare i file in un'area di caricamento dedicata nel sistema, in modo da poter imporre più facilmente misure di sicurezza per il contenuto caricato.</span><span class="sxs-lookup"><span data-stu-id="37767-120">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="37767-121">Quando si permettono caricamenti di file, assicurarsi che le autorizzazioni di esecuzione siano disabilitate nella posizione di caricamento.</span><span class="sxs-lookup"><span data-stu-id="37767-121">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="37767-122">Usare nomi di file sicuri determinati dall'app e non dall'input dell'utente o non usare il nome del file caricato.</span><span class="sxs-lookup"><span data-stu-id="37767-122">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="37767-123">Consentire solo un set specifico di estensioni di file approvate.</span><span class="sxs-lookup"><span data-stu-id="37767-123">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="37767-124">Verificare che vengano eseguiti controlli sul lato client nel server.</span><span class="sxs-lookup"><span data-stu-id="37767-124">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="37767-125">I controlli sul lato client sono facili da aggirare.</span><span class="sxs-lookup"><span data-stu-id="37767-125">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="37767-126">Controllare le dimensioni del caricamento e impedire caricamenti di dimensioni maggiori del previsto.</span><span class="sxs-lookup"><span data-stu-id="37767-126">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="37767-127">Eseguire un rilevatore di virus/malware sul contenuto caricato.</span><span class="sxs-lookup"><span data-stu-id="37767-127">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="37767-128">Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:</span><span class="sxs-lookup"><span data-stu-id="37767-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="37767-129">Prendere il controllo totale di un sistema.</span><span class="sxs-lookup"><span data-stu-id="37767-129">Completely takeover a system.</span></span>
> * <span data-ttu-id="37767-130">Sovraccaricare un sistema causandone il malfunzionamento completo.</span><span class="sxs-lookup"><span data-stu-id="37767-130">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="37767-131">Compromettere i dati di sistemi o utenti.</span><span class="sxs-lookup"><span data-stu-id="37767-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="37767-132">Applicare graffiti a un'interfaccia pubblica (defacing).</span><span class="sxs-lookup"><span data-stu-id="37767-132">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="37767-133">Aggiungere una classe FileUpload</span><span class="sxs-lookup"><span data-stu-id="37767-133">Add a FileUpload class</span></span>

<span data-ttu-id="37767-134">Creare una pagina Razor per gestire una coppia di caricamenti di file.</span><span class="sxs-lookup"><span data-stu-id="37767-134">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="37767-135">Aggiungere una classe `FileUpload`, che è associata alla pagina per ottenere i dati di pianificazione.</span><span class="sxs-lookup"><span data-stu-id="37767-135">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="37767-136">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="37767-136">Right click the *Models* folder.</span></span> <span data-ttu-id="37767-137">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="37767-137">Select **Add** > **Class**.</span></span> <span data-ttu-id="37767-138">Denominare la classe **FileUpload** e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="37767-138">Name the class **FileUpload** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

<span data-ttu-id="37767-139">La classe ha una proprietà per il titolo della pianificazione e una proprietà per ognuna delle due versioni della pianificazione.</span><span class="sxs-lookup"><span data-stu-id="37767-139">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="37767-140">Tutte e tre le proprietà sono obbligatorie e il titolo deve avere una lunghezza compresa fra 3 e 60 caratteri.</span><span class="sxs-lookup"><span data-stu-id="37767-140">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="37767-141">Aggiungere un metodo helper per caricare file</span><span class="sxs-lookup"><span data-stu-id="37767-141">Add a helper method to upload files</span></span>

<span data-ttu-id="37767-142">Per evitare la duplicazione del codice per l'elaborazione dei file di pianificazione caricati, è necessario per prima cosa aggiungere un metodo helper statico.</span><span class="sxs-lookup"><span data-stu-id="37767-142">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="37767-143">Creare una cartella *Utilità* nell'app e aggiungere un file *FileHelpers.cs* con il contenuto seguente.</span><span class="sxs-lookup"><span data-stu-id="37767-143">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="37767-144">Il metodo helper, `ProcessFormFile`, accetta [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) e restituisce una stringa contenente le dimensioni e il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="37767-144">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="37767-145">Il tipo di contenuto e la lunghezza vengono controllati.</span><span class="sxs-lookup"><span data-stu-id="37767-145">The content type and length are checked.</span></span> <span data-ttu-id="37767-146">Se il file non supera un controllo di convalida, viene aggiunto un errore a `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="37767-146">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a><span data-ttu-id="37767-147">Salvare il file su disco</span><span class="sxs-lookup"><span data-stu-id="37767-147">Save the file to disk</span></span>

<span data-ttu-id="37767-148">L'app di esempio salva i file caricati in campi di database.</span><span class="sxs-lookup"><span data-stu-id="37767-148">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="37767-149">Per salvare un file su disco, usare un [FileStream](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="37767-149">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="37767-150">L'esempio seguente copia un file gestito da `FileUpload.UploadPublicSchedule` in un `FileStream` in un metodo `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="37767-150">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="37767-151">`FileStream` scrive il file su disco in corrispondenza del `<PATH-AND-FILE-NAME>` specificato:</span><span class="sxs-lookup"><span data-stu-id="37767-151">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="37767-152">Il processo di lavoro deve disporre delle autorizzazioni di scrittura per il percorso specificato da `filePath`.</span><span class="sxs-lookup"><span data-stu-id="37767-152">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="37767-153">Il `filePath` *deve* includere il nome del file.</span><span class="sxs-lookup"><span data-stu-id="37767-153">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="37767-154">Se il nome file non è specificato, viene generata un'eccezione [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="37767-154">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="37767-155">Non mantenere mai i file caricati nello stessa struttura directory in cui si trova l'app.</span><span class="sxs-lookup"><span data-stu-id="37767-155">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="37767-156">Il codice di esempio non include alcuna protezione lato server contro il caricamento di file dannosi.</span><span class="sxs-lookup"><span data-stu-id="37767-156">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="37767-157">Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="37767-157">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="37767-158">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="37767-158">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="37767-159">Sicurezza di Azure: Verificare la presenza dei controlli appropriati quando si accettano file dagli utenti</span><span class="sxs-lookup"><span data-stu-id="37767-159">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="37767-160">Salvare il file in Archiviazione BLOB di Azure</span><span class="sxs-lookup"><span data-stu-id="37767-160">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="37767-161">Per caricare il contenuto del file in Archiviazione Blob di Azure, vedere [Introduzione all'archivio BLOB di Azure con .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="37767-161">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="37767-162">Questo argomento dimostra come usare [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) per salvare un [FileStream](/dotnet/api/system.io.filestream) nell'archivio BLOB.</span><span class="sxs-lookup"><span data-stu-id="37767-162">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="37767-163">Aggiungere la classe Schedule</span><span class="sxs-lookup"><span data-stu-id="37767-163">Add the Schedule class</span></span>

<span data-ttu-id="37767-164">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="37767-164">Right click the *Models* folder.</span></span> <span data-ttu-id="37767-165">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="37767-165">Select **Add** > **Class**.</span></span> <span data-ttu-id="37767-166">Denominare la classe **Schedule** e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="37767-166">Name the class **Schedule** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

<span data-ttu-id="37767-167">La classe usa gli attributi `Display` e `DisplayFormat`, che producono titoli descrittivi e la formattazione quando viene eseguito il rendering dei dati di pianificazione.</span><span class="sxs-lookup"><span data-stu-id="37767-167">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a><span data-ttu-id="37767-168">Aggiornare il database RazorPagesMovieContext</span><span class="sxs-lookup"><span data-stu-id="37767-168">Update the RazorPagesMovieContext</span></span>

<span data-ttu-id="37767-169">Specificare un `DbSet` in `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) per le pianificazioni:</span><span class="sxs-lookup"><span data-stu-id="37767-169">Specify a `DbSet` in the `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a><span data-ttu-id="37767-170">Aggiornare MovieContext</span><span class="sxs-lookup"><span data-stu-id="37767-170">Update the MovieContext</span></span>

<span data-ttu-id="37767-171">Specificare un `DbSet` in `MovieContext` (*Models/MovieContext.cs*) per le pianificazioni:</span><span class="sxs-lookup"><span data-stu-id="37767-171">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="37767-172">Aggiungere la tabella Schedule al database</span><span class="sxs-lookup"><span data-stu-id="37767-172">Add the Schedule table to the database</span></span>

<span data-ttu-id="37767-173">Aprire la Console di gestione pacchetti (PMC): **Gli strumenti** > **Gestione pacchetti NuGet** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="37767-173">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menu della Console di Gestione pacchetti](upload-files/_static/pmc.png)

<span data-ttu-id="37767-175">Nella console di Gestione pacchetti eseguire i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="37767-175">In the PMC, execute the following commands.</span></span> <span data-ttu-id="37767-176">In questo modo viene aggiunta una tabella `Schedule` al database:</span><span class="sxs-lookup"><span data-stu-id="37767-176">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="37767-177">Aggiungere una pagina Razor di caricamento file</span><span class="sxs-lookup"><span data-stu-id="37767-177">Add a file upload Razor Page</span></span>

<span data-ttu-id="37767-178">Nella cartella *Pages* creare una cartella *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="37767-178">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="37767-179">Nella cartella *Schedules* creare una pagina denominata *Index.cshtml* per il caricamento di una pianificazione con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="37767-179">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

<span data-ttu-id="37767-180">Ogni gruppo di moduli include un oggetto **\<label>** ovvero un'etichetta che visualizza il nome di ogni proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="37767-180">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="37767-181">Gli attributi `Display` del modello `FileUpload` specificano i valori di visualizzazione per le etichette.</span><span class="sxs-lookup"><span data-stu-id="37767-181">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="37767-182">Ad esempio, il nome visualizzato della proprietà `UploadPublicSchedule` è impostato con `[Display(Name="Public Schedule")]` e pertanto viene visualizzato "Public Schedule" nell'etichetta quando viene eseguito il rendering del modulo.</span><span class="sxs-lookup"><span data-stu-id="37767-182">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="37767-183">Ogni gruppo di moduli include una convalida **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="37767-183">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="37767-184">Se l'input dell'utente non soddisfa gli attributi della proprietà impostati nella classe `FileUpload` o se uno dei controlli della convalida file del metodo `ProcessFormFile` ha esito negativo, il modello non viene convalidato.</span><span class="sxs-lookup"><span data-stu-id="37767-184">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="37767-185">Quando il modello non viene convalidato, viene eseguito il rendering di un messaggio di convalida utile per l'utente.</span><span class="sxs-lookup"><span data-stu-id="37767-185">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="37767-186">Ad esempio, la proprietà `Title` viene annotata con `[Required]` e `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="37767-186">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="37767-187">Se l'utente non specifica un titolo, riceve un messaggio che indica che è necessario specificare un valore.</span><span class="sxs-lookup"><span data-stu-id="37767-187">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="37767-188">Se l'utente immette un valore inferiore a tre caratteri o superiore a sessanta, riceve un messaggio che indica che il valore ha una lunghezza non corretta.</span><span class="sxs-lookup"><span data-stu-id="37767-188">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="37767-189">Se viene specificato un file che non presenta alcun contenuto, viene visualizzato un messaggio che indica che il file è vuoto.</span><span class="sxs-lookup"><span data-stu-id="37767-189">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="37767-190">Aggiungere il modello di pagina</span><span class="sxs-lookup"><span data-stu-id="37767-190">Add the page model</span></span>

<span data-ttu-id="37767-191">Aggiungere il modello di pagina *Index.cshtml.cs* nella cartella *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="37767-191">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

<span data-ttu-id="37767-192">Il modello di pagina (`IndexModel` in *Index.cshtml.cs*) associa la classe `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="37767-192">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="37767-193">Il modello usa anche un elenco delle pianificazioni, `IList<Schedule>`, per visualizzare le pianificazioni archiviate nel database nella pagina:</span><span class="sxs-lookup"><span data-stu-id="37767-193">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="37767-194">Quando la pagina viene caricata con `OnGetAsync`, la tabella `Schedules` viene popolata dal database e usata per generare una tabella HTML di pianificazioni caricate:</span><span class="sxs-lookup"><span data-stu-id="37767-194">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="37767-195">Quando il modulo viene pubblicato nel server, viene eseguito il controllo di `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="37767-195">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="37767-196">Se la convalida dà esito negativo, `Schedule` viene ricompilata, e il rendering della pagina genera uno o più messaggi di convalida che indicano il motivo per cui la convalida non è riuscita.</span><span class="sxs-lookup"><span data-stu-id="37767-196">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="37767-197">Se la convalida ha esisto positivo, le proprietà `FileUpload` vengono usate in *OnPostAsync* per completare il caricamento del file per le due versioni della pianificazione e per creare un nuovo oggetto `Schedule` per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="37767-197">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="37767-198">La pianificazione viene quindi salvata nel database:</span><span class="sxs-lookup"><span data-stu-id="37767-198">The schedule is then saved to the database:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="37767-199">Aggiungere un collegamento alla pagina Razor di caricamento file</span><span class="sxs-lookup"><span data-stu-id="37767-199">Link the file upload Razor Page</span></span>

<span data-ttu-id="37767-200">Aprire *Pages/Shared/_Layout.cshtml* e aggiungere un collegamento alla barra di navigazione per accedere alla pagina Pianificazioni:</span><span class="sxs-lookup"><span data-stu-id="37767-200">Open *Pages/Shared/_Layout.cshtml* and add a link to the navigation bar to reach the Schedules page:</span></span>

```cshtml
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-page="/Index">Home</a></li>
        <li><a asp-page="/Schedules/Index">Schedules</a></li>
        <li><a asp-page="/About">About</a></li>
        <li><a asp-page="/Contact">Contact</a></li>
    </ul>
</div>
```

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="37767-201">Aggiungere una pagina per confermare l'eliminazione della pianificazione</span><span class="sxs-lookup"><span data-stu-id="37767-201">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="37767-202">Quando l'utente fa clic per eliminare una pianificazione, viene offerta la possibilità di annullare l'operazione.</span><span class="sxs-lookup"><span data-stu-id="37767-202">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="37767-203">Aggiungere una pagina di conferma dell'eliminazione (*Delete.cshtml*) nella cartella *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="37767-203">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

<span data-ttu-id="37767-204">Il modello di pagina *Delete.cshtml.cs* carica una singola pianificazione identificata da `id` nei dati della route della richiesta.</span><span class="sxs-lookup"><span data-stu-id="37767-204">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="37767-205">Aggiungere il file *Delete.cshtml.cs* nella cartella *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="37767-205">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

<span data-ttu-id="37767-206">Il metodo `OnPostAsync` gestisce l'eliminazione della pianificazione tramite `id`:</span><span class="sxs-lookup"><span data-stu-id="37767-206">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

<span data-ttu-id="37767-207">Dopo aver completato l'eliminazione della pianificazione, `RedirectToPage` invia nuovamente l'utente alla pagina delle pianificazioni *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="37767-207">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="37767-208">La pagina Razor Schedules operativa</span><span class="sxs-lookup"><span data-stu-id="37767-208">The working Schedules Razor Page</span></span>

<span data-ttu-id="37767-209">Quando la pagina viene caricata, il rendering delle etichette e degli input per il titolo della pianificazione, della pianificazione pubblica e della pianificazione privata viene eseguito con un pulsante di invio:</span><span class="sxs-lookup"><span data-stu-id="37767-209">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![La pagina Razor Schedules come viene visualizzata al caricamento iniziale in assenza di errori di convalida e con i campi vuoti](upload-files/_static/browser1.png)

<span data-ttu-id="37767-211">Se si seleziona il pulsante **Carica** senza aver popolato i campi, vengono violati gli attributi `[Required]` nel modello.</span><span class="sxs-lookup"><span data-stu-id="37767-211">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="37767-212">`ModelState` non è valido.</span><span class="sxs-lookup"><span data-stu-id="37767-212">The `ModelState` is invalid.</span></span> <span data-ttu-id="37767-213">I messaggi di errore di convalida vengono visualizzati all'utente:</span><span class="sxs-lookup"><span data-stu-id="37767-213">The validation error messages are displayed to the user:</span></span>

![I messaggi di errore di convalida vengono visualizzati accanto a ogni controllo input](upload-files/_static/browser2.png)

<span data-ttu-id="37767-215">Digitare due lettere nel campo **Titolo**.</span><span class="sxs-lookup"><span data-stu-id="37767-215">Type two letters into the **Title** field.</span></span> <span data-ttu-id="37767-216">Il messaggio di convalida cambia e indica che il titolo deve avere una lunghezza compresa tra 3 e 60 caratteri:</span><span class="sxs-lookup"><span data-stu-id="37767-216">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Messaggio di convalida titolo modificato](upload-files/_static/browser3.png)

<span data-ttu-id="37767-218">Quando vengono caricate una o più pianificazioni, la sezione **Loaded Schedules** (Pianificazioni caricate) esegue il rendering delle pianificazioni caricate:</span><span class="sxs-lookup"><span data-stu-id="37767-218">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabella delle pianificazioni caricate che include il titolo di ogni pianificazione, la data di caricamento con l'ora UTC, le dimensioni del file in versione pubblica e in versione privata](upload-files/_static/browser4.png)

<span data-ttu-id="37767-220">L'utente può scegliere il collegamento **Eliminazione** per accedere alla schermata di conferma dell'eliminazione, da dove è possibile confermare o annullare l'operazione.</span><span class="sxs-lookup"><span data-stu-id="37767-220">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="37767-221">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="37767-221">Troubleshooting</span></span>

<span data-ttu-id="37767-222">Per risolvere i problemi relativi con `IFormFile` caricamento, vedere [caricamenti di File in ASP.NET Core: Risoluzione dei problemi](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="37767-222">For troubleshooting information with `IFormFile` uploading, see [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>
