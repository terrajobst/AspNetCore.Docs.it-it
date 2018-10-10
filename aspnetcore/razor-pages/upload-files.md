---
title: Caricare file in una pagina Razor in ASP.NET Core
author: guardrex
description: Informazioni su come caricare i file in una pagina Razor.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/11/2018
uid: razor-pages/upload-files
ms.openlocfilehash: 92e72869967b6e3202c97b92e341ea22adc69651
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912501"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="88de7-103">Caricare file in una pagina Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88de7-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="88de7-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="88de7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="88de7-105">Questo argomento si basa il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span><span class="sxs-lookup"><span data-stu-id="88de7-105">This topic builds upon the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span></span>

<span data-ttu-id="88de7-106">Questo argomento illustra come usare l'associazione di modelli semplice per caricare i file, che funziona bene per il caricamento di file di piccole dimensioni.</span><span class="sxs-lookup"><span data-stu-id="88de7-106">This topic shows how to use simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="88de7-107">Per informazioni sulla trasmissione di file di grandi dimensioni, vedere [Caricamento di file di grandi dimensioni con il flusso](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="88de7-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="88de7-108">Nella procedura descritta di seguito viene aggiunta una funzionalità di caricamento di file di pianificazione di film all'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="88de7-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="88de7-109">Ogni pianificazione di film è rappresentata da una classe `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="88de7-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="88de7-110">La classe include due versioni della pianificazione.</span><span class="sxs-lookup"><span data-stu-id="88de7-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="88de7-111">La versione `PublicSchedule` viene messa a disposizione dei clienti.</span><span class="sxs-lookup"><span data-stu-id="88de7-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="88de7-112">L'altra versione, `PrivateSchedule`, viene usata per i dipendenti della società.</span><span class="sxs-lookup"><span data-stu-id="88de7-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="88de7-113">Le versioni vengono caricate come file separati.</span><span class="sxs-lookup"><span data-stu-id="88de7-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="88de7-114">Nell'esercitazione viene illustrato come eseguire il caricamento di due file da una pagina con un solo invio al server.</span><span class="sxs-lookup"><span data-stu-id="88de7-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="88de7-115">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="88de7-115">Security considerations</span></span>

<span data-ttu-id="88de7-116">Prestare attenzione quando si offre agli utenti la possibilità di caricare file in un server.</span><span class="sxs-lookup"><span data-stu-id="88de7-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="88de7-117">Gli utenti malintenzionati possono eseguire attacchi [Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) e di altro tipo su un sistema.</span><span class="sxs-lookup"><span data-stu-id="88de7-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="88de7-118">Alcuni passaggi di sicurezza che riducono le probabilità di riuscita degli attacchi sono:</span><span class="sxs-lookup"><span data-stu-id="88de7-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="88de7-119">Caricare i file in un'area di caricamento dedicata nel sistema, in modo da poter imporre più facilmente misure di sicurezza per il contenuto caricato.</span><span class="sxs-lookup"><span data-stu-id="88de7-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="88de7-120">Quando si permettono caricamenti di file, assicurarsi che le autorizzazioni di esecuzione siano disabilitate nella posizione di caricamento.</span><span class="sxs-lookup"><span data-stu-id="88de7-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="88de7-121">Usare nomi di file sicuri determinati dall'app e non dall'input dell'utente o non usare il nome del file caricato.</span><span class="sxs-lookup"><span data-stu-id="88de7-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="88de7-122">Consentire solo un set specifico di estensioni di file approvate.</span><span class="sxs-lookup"><span data-stu-id="88de7-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="88de7-123">Verificare che vengano eseguiti controlli sul lato client nel server.</span><span class="sxs-lookup"><span data-stu-id="88de7-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="88de7-124">I controlli sul lato client sono facili da aggirare.</span><span class="sxs-lookup"><span data-stu-id="88de7-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="88de7-125">Controllare le dimensioni del caricamento e impedire caricamenti di dimensioni maggiori del previsto.</span><span class="sxs-lookup"><span data-stu-id="88de7-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="88de7-126">Eseguire un rilevatore di virus/malware sul contenuto caricato.</span><span class="sxs-lookup"><span data-stu-id="88de7-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="88de7-127">Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:</span><span class="sxs-lookup"><span data-stu-id="88de7-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="88de7-128">Prendere il controllo totale di un sistema.</span><span class="sxs-lookup"><span data-stu-id="88de7-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="88de7-129">Sovraccaricare un sistema causandone il malfunzionamento completo.</span><span class="sxs-lookup"><span data-stu-id="88de7-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="88de7-130">Compromettere i dati di sistemi o utenti.</span><span class="sxs-lookup"><span data-stu-id="88de7-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="88de7-131">Applicare graffiti a un'interfaccia pubblica (defacing).</span><span class="sxs-lookup"><span data-stu-id="88de7-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="88de7-132">Aggiungere una classe FileUpload</span><span class="sxs-lookup"><span data-stu-id="88de7-132">Add a FileUpload class</span></span>

<span data-ttu-id="88de7-133">Creare una pagina Razor per gestire una coppia di caricamenti di file.</span><span class="sxs-lookup"><span data-stu-id="88de7-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="88de7-134">Aggiungere una classe `FileUpload`, che è associata alla pagina per ottenere i dati di pianificazione.</span><span class="sxs-lookup"><span data-stu-id="88de7-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="88de7-135">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="88de7-135">Right click the *Models* folder.</span></span> <span data-ttu-id="88de7-136">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="88de7-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="88de7-137">Denominare la classe **FileUpload** e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="88de7-137">Name the class **FileUpload** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

<span data-ttu-id="88de7-138">La classe ha una proprietà per il titolo della pianificazione e una proprietà per ognuna delle due versioni della pianificazione.</span><span class="sxs-lookup"><span data-stu-id="88de7-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="88de7-139">Tutte e tre le proprietà sono obbligatorie e il titolo deve avere una lunghezza compresa fra 3 e 60 caratteri.</span><span class="sxs-lookup"><span data-stu-id="88de7-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="88de7-140">Aggiungere un metodo helper per caricare file</span><span class="sxs-lookup"><span data-stu-id="88de7-140">Add a helper method to upload files</span></span>

<span data-ttu-id="88de7-141">Per evitare la duplicazione del codice per l'elaborazione dei file di pianificazione caricati, è necessario per prima cosa aggiungere un metodo helper statico.</span><span class="sxs-lookup"><span data-stu-id="88de7-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="88de7-142">Creare una cartella *Utilità* nell'app e aggiungere un file *FileHelpers.cs* con il contenuto seguente.</span><span class="sxs-lookup"><span data-stu-id="88de7-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="88de7-143">Il metodo helper, `ProcessFormFile`, accetta [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) e restituisce una stringa contenente le dimensioni e il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="88de7-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="88de7-144">Il tipo di contenuto e la lunghezza vengono controllati.</span><span class="sxs-lookup"><span data-stu-id="88de7-144">The content type and length are checked.</span></span> <span data-ttu-id="88de7-145">Se il file non supera un controllo di convalida, viene aggiunto un errore a `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="88de7-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a><span data-ttu-id="88de7-146">Salvare il file su disco</span><span class="sxs-lookup"><span data-stu-id="88de7-146">Save the file to disk</span></span>

<span data-ttu-id="88de7-147">L'app di esempio salva i file caricati in campi di database.</span><span class="sxs-lookup"><span data-stu-id="88de7-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="88de7-148">Per salvare un file su disco, usare un [FileStream](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="88de7-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="88de7-149">L'esempio seguente copia un file gestito da `FileUpload.UploadPublicSchedule` in un `FileStream` in un metodo `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="88de7-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="88de7-150">`FileStream` scrive il file su disco in corrispondenza del `<PATH-AND-FILE-NAME>` specificato:</span><span class="sxs-lookup"><span data-stu-id="88de7-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

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

<span data-ttu-id="88de7-151">Il processo di lavoro deve disporre delle autorizzazioni di scrittura per il percorso specificato da `filePath`.</span><span class="sxs-lookup"><span data-stu-id="88de7-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="88de7-152">Il `filePath` *deve* includere il nome del file.</span><span class="sxs-lookup"><span data-stu-id="88de7-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="88de7-153">Se il nome file non è specificato, viene generata un'eccezione [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="88de7-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="88de7-154">Non mantenere mai i file caricati nello stessa struttura directory in cui si trova l'app.</span><span class="sxs-lookup"><span data-stu-id="88de7-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="88de7-155">Il codice di esempio non include alcuna protezione lato server contro il caricamento di file dannosi.</span><span class="sxs-lookup"><span data-stu-id="88de7-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="88de7-156">Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="88de7-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="88de7-157">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="88de7-157">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="88de7-158">Protezione di Azure: Verificare i controlli appropriati quando si accettano file dagli utenti</span><span class="sxs-lookup"><span data-stu-id="88de7-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="88de7-159">Salvare il file in Archiviazione BLOB di Azure</span><span class="sxs-lookup"><span data-stu-id="88de7-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="88de7-160">Per caricare il contenuto del file in Archiviazione Blob di Azure, vedere [Introduzione all'archivio BLOB di Azure con .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="88de7-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="88de7-161">Questo argomento dimostra come usare [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) per salvare un [FileStream](/dotnet/api/system.io.filestream) nell'archivio BLOB.</span><span class="sxs-lookup"><span data-stu-id="88de7-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="88de7-162">Aggiungere la classe Schedule</span><span class="sxs-lookup"><span data-stu-id="88de7-162">Add the Schedule class</span></span>

<span data-ttu-id="88de7-163">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="88de7-163">Right click the *Models* folder.</span></span> <span data-ttu-id="88de7-164">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="88de7-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="88de7-165">Denominare la classe **Schedule** e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="88de7-165">Name the class **Schedule** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

<span data-ttu-id="88de7-166">La classe usa gli attributi `Display` e `DisplayFormat`, che producono titoli descrittivi e la formattazione quando viene eseguito il rendering dei dati di pianificazione.</span><span class="sxs-lookup"><span data-stu-id="88de7-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a><span data-ttu-id="88de7-167">Aggiornare il database RazorPagesMovieContext</span><span class="sxs-lookup"><span data-stu-id="88de7-167">Update the RazorPagesMovieContext</span></span>

<span data-ttu-id="88de7-168">Specificare un `DbSet` in `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) per le pianificazioni:</span><span class="sxs-lookup"><span data-stu-id="88de7-168">Specify a `DbSet` in the `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a><span data-ttu-id="88de7-169">Aggiornare MovieContext</span><span class="sxs-lookup"><span data-stu-id="88de7-169">Update the MovieContext</span></span>

<span data-ttu-id="88de7-170">Specificare un `DbSet` in `MovieContext` (*Models/MovieContext.cs*) per le pianificazioni:</span><span class="sxs-lookup"><span data-stu-id="88de7-170">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="88de7-171">Aggiungere la tabella Schedule al database</span><span class="sxs-lookup"><span data-stu-id="88de7-171">Add the Schedule table to the database</span></span>

<span data-ttu-id="88de7-172">Aprire la Console di Gestione pacchetti: **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="88de7-172">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menu della Console di Gestione pacchetti](upload-files/_static/pmc.png)

<span data-ttu-id="88de7-174">Nella console di Gestione pacchetti eseguire i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="88de7-174">In the PMC, execute the following commands.</span></span> <span data-ttu-id="88de7-175">In questo modo viene aggiunta una tabella `Schedule` al database:</span><span class="sxs-lookup"><span data-stu-id="88de7-175">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="88de7-176">Aggiungere una pagina Razor di caricamento file</span><span class="sxs-lookup"><span data-stu-id="88de7-176">Add a file upload Razor Page</span></span>

<span data-ttu-id="88de7-177">Nella cartella *Pages* creare una cartella *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="88de7-177">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="88de7-178">Nella cartella *Schedules* creare una pagina denominata *Index.cshtml* per il caricamento di una pianificazione con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="88de7-178">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

<span data-ttu-id="88de7-179">Ogni gruppo di moduli include un oggetto **\<label>** ovvero un'etichetta che visualizza il nome di ogni proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="88de7-179">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="88de7-180">Gli attributi `Display` del modello `FileUpload` specificano i valori di visualizzazione per le etichette.</span><span class="sxs-lookup"><span data-stu-id="88de7-180">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="88de7-181">Ad esempio, il nome visualizzato della proprietà `UploadPublicSchedule` è impostato con `[Display(Name="Public Schedule")]` e pertanto viene visualizzato "Public Schedule" nell'etichetta quando viene eseguito il rendering del modulo.</span><span class="sxs-lookup"><span data-stu-id="88de7-181">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="88de7-182">Ogni gruppo di moduli include una convalida **\<span>**.</span><span class="sxs-lookup"><span data-stu-id="88de7-182">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="88de7-183">Se l'input dell'utente non soddisfa gli attributi della proprietà impostati nella classe `FileUpload` o se uno dei controlli della convalida file del metodo `ProcessFormFile` ha esito negativo, il modello non viene convalidato.</span><span class="sxs-lookup"><span data-stu-id="88de7-183">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="88de7-184">Quando il modello non viene convalidato, viene eseguito il rendering di un messaggio di convalida utile per l'utente.</span><span class="sxs-lookup"><span data-stu-id="88de7-184">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="88de7-185">Ad esempio, la proprietà `Title` viene annotata con `[Required]` e `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="88de7-185">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="88de7-186">Se l'utente non specifica un titolo, riceve un messaggio che indica che è necessario specificare un valore.</span><span class="sxs-lookup"><span data-stu-id="88de7-186">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="88de7-187">Se l'utente immette un valore inferiore a tre caratteri o superiore a sessanta, riceve un messaggio che indica che il valore ha una lunghezza non corretta.</span><span class="sxs-lookup"><span data-stu-id="88de7-187">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="88de7-188">Se viene specificato un file che non presenta alcun contenuto, viene visualizzato un messaggio che indica che il file è vuoto.</span><span class="sxs-lookup"><span data-stu-id="88de7-188">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="88de7-189">Aggiungere il modello di pagina</span><span class="sxs-lookup"><span data-stu-id="88de7-189">Add the page model</span></span>

<span data-ttu-id="88de7-190">Aggiungere il modello di pagina *Index.cshtml.cs* nella cartella *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="88de7-190">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

<span data-ttu-id="88de7-191">Il modello di pagina (`IndexModel` in *Index.cshtml.cs*) associa la classe `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="88de7-191">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="88de7-192">Il modello usa anche un elenco delle pianificazioni, `IList<Schedule>`, per visualizzare le pianificazioni archiviate nel database nella pagina:</span><span class="sxs-lookup"><span data-stu-id="88de7-192">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="88de7-193">Quando la pagina viene caricata con `OnGetAsync`, la tabella `Schedules` viene popolata dal database e usata per generare una tabella HTML di pianificazioni caricate:</span><span class="sxs-lookup"><span data-stu-id="88de7-193">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="88de7-194">Quando il modulo viene pubblicato nel server, viene eseguito il controllo di `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="88de7-194">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="88de7-195">Se la convalida dà esito negativo, `Schedule` viene ricompilata, e il rendering della pagina genera uno o più messaggi di convalida che indicano il motivo per cui la convalida non è riuscita.</span><span class="sxs-lookup"><span data-stu-id="88de7-195">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="88de7-196">Se la convalida ha esisto positivo, le proprietà `FileUpload` vengono usate in *OnPostAsync* per completare il caricamento del file per le due versioni della pianificazione e per creare un nuovo oggetto `Schedule` per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="88de7-196">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="88de7-197">La pianificazione viene quindi salvata nel database:</span><span class="sxs-lookup"><span data-stu-id="88de7-197">The schedule is then saved to the database:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="88de7-198">Aggiungere un collegamento alla pagina Razor di caricamento file</span><span class="sxs-lookup"><span data-stu-id="88de7-198">Link the file upload Razor Page</span></span>

<span data-ttu-id="88de7-199">Aprire *Pages/Shared/_Layout.cshtml* e aggiungere un collegamento alla barra di navigazione per accedere alla pagina Pianificazioni:</span><span class="sxs-lookup"><span data-stu-id="88de7-199">Open *Pages/Shared/_Layout.cshtml* and add a link to the navigation bar to reach the Schedules page:</span></span>

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

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="88de7-200">Aggiungere una pagina per confermare l'eliminazione della pianificazione</span><span class="sxs-lookup"><span data-stu-id="88de7-200">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="88de7-201">Quando l'utente fa clic per eliminare una pianificazione, viene offerta la possibilità di annullare l'operazione.</span><span class="sxs-lookup"><span data-stu-id="88de7-201">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="88de7-202">Aggiungere una pagina di conferma dell'eliminazione (*Delete.cshtml*) nella cartella *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="88de7-202">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

<span data-ttu-id="88de7-203">Il modello di pagina *Delete.cshtml.cs* carica una singola pianificazione identificata da `id` nei dati della route della richiesta.</span><span class="sxs-lookup"><span data-stu-id="88de7-203">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="88de7-204">Aggiungere il file *Delete.cshtml.cs* nella cartella *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="88de7-204">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

<span data-ttu-id="88de7-205">Il metodo `OnPostAsync` gestisce l'eliminazione della pianificazione tramite `id`:</span><span class="sxs-lookup"><span data-stu-id="88de7-205">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

<span data-ttu-id="88de7-206">Dopo aver completato l'eliminazione della pianificazione, `RedirectToPage` invia nuovamente l'utente alla pagina delle pianificazioni *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="88de7-206">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="88de7-207">La pagina Razor Schedules operativa</span><span class="sxs-lookup"><span data-stu-id="88de7-207">The working Schedules Razor Page</span></span>

<span data-ttu-id="88de7-208">Quando la pagina viene caricata, il rendering delle etichette e degli input per il titolo della pianificazione, della pianificazione pubblica e della pianificazione privata viene eseguito con un pulsante di invio:</span><span class="sxs-lookup"><span data-stu-id="88de7-208">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![La pagina Razor Schedules come viene visualizzata al caricamento iniziale in assenza di errori di convalida e con i campi vuoti](upload-files/_static/browser1.png)

<span data-ttu-id="88de7-210">Se si seleziona il pulsante **Carica** senza aver popolato i campi, vengono violati gli attributi `[Required]` nel modello.</span><span class="sxs-lookup"><span data-stu-id="88de7-210">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="88de7-211">`ModelState` non è valido.</span><span class="sxs-lookup"><span data-stu-id="88de7-211">The `ModelState` is invalid.</span></span> <span data-ttu-id="88de7-212">I messaggi di errore di convalida vengono visualizzati all'utente:</span><span class="sxs-lookup"><span data-stu-id="88de7-212">The validation error messages are displayed to the user:</span></span>

![I messaggi di errore di convalida vengono visualizzati accanto a ogni controllo input](upload-files/_static/browser2.png)

<span data-ttu-id="88de7-214">Digitare due lettere nel campo **Titolo**.</span><span class="sxs-lookup"><span data-stu-id="88de7-214">Type two letters into the **Title** field.</span></span> <span data-ttu-id="88de7-215">Il messaggio di convalida cambia e indica che il titolo deve avere una lunghezza compresa tra 3 e 60 caratteri:</span><span class="sxs-lookup"><span data-stu-id="88de7-215">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Messaggio di convalida titolo modificato](upload-files/_static/browser3.png)

<span data-ttu-id="88de7-217">Quando vengono caricate una o più pianificazioni, la sezione **Loaded Schedules** (Pianificazioni caricate) esegue il rendering delle pianificazioni caricate:</span><span class="sxs-lookup"><span data-stu-id="88de7-217">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabella delle pianificazioni caricate che include il titolo di ogni pianificazione, la data di caricamento con l'ora UTC, le dimensioni del file in versione pubblica e in versione privata](upload-files/_static/browser4.png)

<span data-ttu-id="88de7-219">L'utente può scegliere il collegamento **Eliminazione** per accedere alla schermata di conferma dell'eliminazione, da dove è possibile confermare o annullare l'operazione.</span><span class="sxs-lookup"><span data-stu-id="88de7-219">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="88de7-220">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="88de7-220">Troubleshooting</span></span>

<span data-ttu-id="88de7-221">Per risolvere i problemi relativi con `IFormFile` caricamento, vedere [caricamenti di File in ASP.NET Core: risoluzione dei problemi](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="88de7-221">For troubleshooting information with `IFormFile` uploading, see [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>
