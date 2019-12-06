---
title: Caricare file in ASP.NET Core
author: guardrex
description: Come usare l'associazione del modello e lo streaming per caricare i file in ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 20e58660185a3055e06e92d9136e80e2394a470d
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881063"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="05a3d-103">Caricare file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05a3d-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="05a3d-104">Di [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)e [Rutger Storm](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="05a3d-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="05a3d-105">ASP.NET Core supporta il caricamento di uno o più file mediante l'associazione di modelli memorizzati nel buffer per i file più piccoli e il flusso non memorizzato nel buffer per file di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="05a3d-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="05a3d-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="05a3d-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="05a3d-107">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="05a3d-107">Security considerations</span></span>

<span data-ttu-id="05a3d-108">Prestare attenzione quando si fornisce agli utenti la possibilità di caricare file in un server.</span><span class="sxs-lookup"><span data-stu-id="05a3d-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="05a3d-109">Gli utenti malintenzionati possono tentare di:</span><span class="sxs-lookup"><span data-stu-id="05a3d-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="05a3d-110">Eseguire attacchi [di tipo Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="05a3d-111">Caricare virus o malware.</span><span class="sxs-lookup"><span data-stu-id="05a3d-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="05a3d-112">Compromettere le reti e i server in altri modi.</span><span class="sxs-lookup"><span data-stu-id="05a3d-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="05a3d-113">I passaggi di sicurezza che riducono la probabilità di un attacco riuscito sono:</span><span class="sxs-lookup"><span data-stu-id="05a3d-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="05a3d-114">Caricare i file in un'area di caricamento file dedicata, preferibilmente in un'unità non di sistema.</span><span class="sxs-lookup"><span data-stu-id="05a3d-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="05a3d-115">Un percorso dedicato semplifica l'imposizione delle restrizioni di sicurezza sui file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="05a3d-116">Disabilitare le autorizzazioni di esecuzione per il percorso di caricamento del file.&dagger;</span><span class="sxs-lookup"><span data-stu-id="05a3d-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="05a3d-117">Non **rende permanente i** file caricati nella stessa struttura di directory dell'app.&dagger;</span><span class="sxs-lookup"><span data-stu-id="05a3d-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="05a3d-118">Usare un nome file sicuro determinato dall'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="05a3d-119">Non usare un nome file fornito dall'utente o il nome file non attendibile del file caricato.&dagger; HTML codifica il nome file non attendibile quando viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="05a3d-120">Ad esempio, la registrazione del nome del file o la visualizzazione nell'interfaccia utente (Razor codifica automaticamente l'output).</span><span class="sxs-lookup"><span data-stu-id="05a3d-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="05a3d-121">Consenti solo le estensioni di file approvate per la specifica di progettazione dell'app.&dagger;</span><span class="sxs-lookup"><span data-stu-id="05a3d-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="05a3d-122">Verificare che i controlli lato client vengano eseguiti sul server.&dagger; controlli lato client sono facili da aggirare.</span><span class="sxs-lookup"><span data-stu-id="05a3d-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="05a3d-123">Controllare le dimensioni di un file caricato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="05a3d-124">Impostare un limite di dimensioni massime per impedire caricamenti di grandi dimensioni.&dagger;</span><span class="sxs-lookup"><span data-stu-id="05a3d-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="05a3d-125">Quando i file non devono essere sovrascritti da un file caricato con lo stesso nome, controllare il nome del file sul database o sull'archiviazione fisica prima di caricare il file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="05a3d-126">**Eseguire uno scanner virus/malware sul contenuto caricato prima che il file venga archiviato.**</span><span class="sxs-lookup"><span data-stu-id="05a3d-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="05a3d-127">&dagger;l'app di esempio illustra un approccio che soddisfa i criteri.</span><span class="sxs-lookup"><span data-stu-id="05a3d-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="05a3d-128">Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:</span><span class="sxs-lookup"><span data-stu-id="05a3d-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="05a3d-129">Ottenere il controllo completo di un sistema.</span><span class="sxs-lookup"><span data-stu-id="05a3d-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="05a3d-130">Sovraccaricare un sistema con il risultato che si verifica un arresto anomalo del sistema.</span><span class="sxs-lookup"><span data-stu-id="05a3d-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="05a3d-131">Compromettere i dati di sistemi o utenti.</span><span class="sxs-lookup"><span data-stu-id="05a3d-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="05a3d-132">Applicare i graffiti a un'interfaccia utente pubblica.</span><span class="sxs-lookup"><span data-stu-id="05a3d-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="05a3d-133">Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="05a3d-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="05a3d-134">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="05a3d-134">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="05a3d-135">Protezione di Azure: Verificare i controlli appropriati quando si accettano file dagli utenti</span><span class="sxs-lookup"><span data-stu-id="05a3d-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="05a3d-136">Per ulteriori informazioni sull'implementazione di misure di sicurezza, inclusi esempi dall'app di esempio, vedere la sezione [convalida](#validation) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="05a3d-137">Scenari di archiviazione</span><span class="sxs-lookup"><span data-stu-id="05a3d-137">Storage scenarios</span></span>

<span data-ttu-id="05a3d-138">Le opzioni di archiviazione comuni per i file includono:</span><span class="sxs-lookup"><span data-stu-id="05a3d-138">Common storage options for files include:</span></span>

* <span data-ttu-id="05a3d-139">Database di</span><span class="sxs-lookup"><span data-stu-id="05a3d-139">Database</span></span>

  * <span data-ttu-id="05a3d-140">Per i caricamenti di file di piccole dimensioni, un database è spesso più veloce rispetto alle opzioni di archiviazione fisica (file system o condivisione di rete).</span><span class="sxs-lookup"><span data-stu-id="05a3d-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="05a3d-141">Un database è spesso più pratico delle opzioni di archiviazione fisica perché il recupero di un record di database per i dati utente può fornire simultaneamente il contenuto del file, ad esempio un'immagine avatar.</span><span class="sxs-lookup"><span data-stu-id="05a3d-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="05a3d-142">Un database è potenzialmente meno costoso rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="05a3d-143">Archiviazione fisica (file system o condivisione di rete)</span><span class="sxs-lookup"><span data-stu-id="05a3d-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="05a3d-144">Per i caricamenti di file di grandi dimensioni:</span><span class="sxs-lookup"><span data-stu-id="05a3d-144">For large file uploads:</span></span>
    * <span data-ttu-id="05a3d-145">I limiti del database possono limitare le dimensioni del caricamento.</span><span class="sxs-lookup"><span data-stu-id="05a3d-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="05a3d-146">L'archiviazione fisica è spesso meno economica rispetto all'archiviazione in un database.</span><span class="sxs-lookup"><span data-stu-id="05a3d-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="05a3d-147">L'archiviazione fisica è potenzialmente meno costosa rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="05a3d-148">Il processo dell'app deve avere le autorizzazioni di lettura e scrittura per il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="05a3d-149">**Non concedere mai l'autorizzazione Execute.**</span><span class="sxs-lookup"><span data-stu-id="05a3d-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="05a3d-150">Servizio di archiviazione dei dati (ad esempio, [archiviazione BLOB di Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="05a3d-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="05a3d-151">I servizi in genere offrono una maggiore scalabilità e resilienza rispetto a soluzioni locali che in genere sono soggette a singoli punti di errore.</span><span class="sxs-lookup"><span data-stu-id="05a3d-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="05a3d-152">I servizi sono un costo potenzialmente inferiore in scenari di infrastruttura di archiviazione di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="05a3d-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="05a3d-153">Per altre informazioni, vedere [Guida introduttiva: usare .NET per creare un BLOB nell'archivio oggetti](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="05a3d-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="05a3d-154">Nell'argomento viene illustrato <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, ma è possibile utilizzare <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> per salvare un <xref:System.IO.FileStream> nell'archiviazione BLOB quando si utilizza un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="05a3d-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="05a3d-155">Scenari di caricamento di file</span><span class="sxs-lookup"><span data-stu-id="05a3d-155">File upload scenarios</span></span>

<span data-ttu-id="05a3d-156">Due approcci generali per il caricamento di file sono il buffering e il flusso.</span><span class="sxs-lookup"><span data-stu-id="05a3d-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="05a3d-157">**Buffer**</span><span class="sxs-lookup"><span data-stu-id="05a3d-157">**Buffering**</span></span>

<span data-ttu-id="05a3d-158">L'intero file viene letto in un <xref:Microsoft.AspNetCore.Http.IFormFile>, ovvero una C# rappresentazione del file usato per elaborare o salvare il file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="05a3d-159">Le risorse (disco, memoria) utilizzate dai caricamenti di file dipendono dal numero e dalle dimensioni dei caricamenti di file simultanei.</span><span class="sxs-lookup"><span data-stu-id="05a3d-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="05a3d-160">Se un'app tenta di memorizzare nel buffer un numero eccessivo di caricamenti, il sito si arresta in modo anomalo quando esaurisce la memoria o lo spazio su disco.</span><span class="sxs-lookup"><span data-stu-id="05a3d-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="05a3d-161">Se le dimensioni o la frequenza dei caricamenti di file esauriscono le risorse dell'app, usare il flusso.</span><span class="sxs-lookup"><span data-stu-id="05a3d-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="05a3d-162">Ogni singolo file memorizzato nel buffer che supera 64 KB viene spostato dalla memoria in un file temporaneo su disco.</span><span class="sxs-lookup"><span data-stu-id="05a3d-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="05a3d-163">Il buffering di file di piccole dimensioni viene trattato nelle sezioni seguenti di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="05a3d-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="05a3d-164">Archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="05a3d-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="05a3d-165">Database</span><span class="sxs-lookup"><span data-stu-id="05a3d-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="05a3d-166">**Flusso**</span><span class="sxs-lookup"><span data-stu-id="05a3d-166">**Streaming**</span></span>

<span data-ttu-id="05a3d-167">Il file viene ricevuto da una richiesta multipart ed elaborato o salvato direttamente dall'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="05a3d-168">Il flusso non migliora significativamente le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="05a3d-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="05a3d-169">Il flusso riduce le richieste di memoria o di spazio su disco durante il caricamento dei file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="05a3d-170">Il flusso di file di grandi dimensioni è trattato nella sezione [caricare file di grandi dimensioni con flusso](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="05a3d-171">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer all'archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="05a3d-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="05a3d-172">Per caricare file di piccole dimensioni, usare un modulo multipart o creare una richiesta POST usando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="05a3d-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="05a3d-173">L'esempio seguente illustra l'uso di un modulo di Razor Pages per caricare un singolo file (*pages/BufferedSingleFileUploadPhysical. cshtml* nell'app di esempio):</span><span class="sxs-lookup"><span data-stu-id="05a3d-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="05a3d-174">L'esempio seguente è analogo all'esempio precedente, ad eccezione del fatto che:</span><span class="sxs-lookup"><span data-stu-id="05a3d-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="05a3d-175">Per inviare i dati del modulo, viene usato JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)).</span><span class="sxs-lookup"><span data-stu-id="05a3d-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="05a3d-176">Nessuna convalida.</span><span class="sxs-lookup"><span data-stu-id="05a3d-176">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="05a3d-177">Per eseguire il POST del form in JavaScript per i client che [non supportano l'API fetch](https://caniuse.com/#feat=fetch), usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="05a3d-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="05a3d-178">Usare un riempimento di recupero (ad esempio, [Window. fetch Refill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="05a3d-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="05a3d-179">Usare `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="05a3d-180">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-180">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="05a3d-181">Per supportare il caricamento di file, i moduli HTML devono specificare un tipo di codifica (`enctype`) di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="05a3d-182">Affinché un `files` elemento input supporti il caricamento di più file, fornire l'attributo `multiple` sull'elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="05a3d-183">È possibile accedere ai singoli file caricati nel server tramite l' [associazione di modelli](xref:mvc/models/model-binding) utilizzando <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="05a3d-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="05a3d-184">L'app di esempio illustra più caricamenti di file memorizzati nel buffer per scenari di archiviazione di database e fisici.</span><span class="sxs-lookup"><span data-stu-id="05a3d-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="05a3d-185">Non **utilizzare la** proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> diversa da per la visualizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="05a3d-186">Durante la visualizzazione o la registrazione, HTML codifica il nome del file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="05a3d-187">Un utente malintenzionato può fornire un nome file dannoso, inclusi percorsi completi o percorsi relativi.</span><span class="sxs-lookup"><span data-stu-id="05a3d-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="05a3d-188">Le applicazioni devono:</span><span class="sxs-lookup"><span data-stu-id="05a3d-188">Applications should:</span></span>
>
> * <span data-ttu-id="05a3d-189">Rimuovere il percorso dal nome file specificato dall'utente.</span><span class="sxs-lookup"><span data-stu-id="05a3d-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="05a3d-190">Salvare il nome file con codifica HTML, percorso-rimosso per l'interfaccia utente o la registrazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="05a3d-191">Genera un nuovo nome file casuale per l'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="05a3d-192">Il codice seguente rimuove il percorso dal nome del file:</span><span class="sxs-lookup"><span data-stu-id="05a3d-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="05a3d-193">Gli esempi forniti finora non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="05a3d-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="05a3d-194">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="05a3d-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="05a3d-195">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="05a3d-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="05a3d-196">Convalida</span><span class="sxs-lookup"><span data-stu-id="05a3d-196">Validation</span></span>](#validation)

<span data-ttu-id="05a3d-197">Quando si caricano file usando l'associazione di modelli e <xref:Microsoft.AspNetCore.Http.IFormFile>, il metodo di azione può accettare:</span><span class="sxs-lookup"><span data-stu-id="05a3d-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="05a3d-198">Singolo <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="05a3d-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="05a3d-199">Qualsiasi raccolta seguente che rappresenta diversi file:</span><span class="sxs-lookup"><span data-stu-id="05a3d-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="05a3d-200">[Elenca](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="05a3d-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="05a3d-201">L'associazione corrisponde ai file di form in base al nome.</span><span class="sxs-lookup"><span data-stu-id="05a3d-201">Binding matches form files by name.</span></span> <span data-ttu-id="05a3d-202">Ad esempio, il valore HTML `name` in `<input type="file" name="formFile">` deve corrispondere al C# parametro/proprietà associato (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="05a3d-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="05a3d-203">Per ulteriori informazioni, vedere la sezione [corrispondenza del nome dell'attributo del nome del parametro del metodo post](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="05a3d-204">L'esempio seguente consente di:</span><span class="sxs-lookup"><span data-stu-id="05a3d-204">The following example:</span></span>

* <span data-ttu-id="05a3d-205">Esegue il ciclo di uno o più file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="05a3d-206">USA [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per restituire un percorso completo per un file, incluso il nome del file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="05a3d-207">Salva i file nella file system locale usando un nome file generato dall'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="05a3d-208">Restituisce il numero totale e le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-208">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="05a3d-209">Utilizzare `Path.GetRandomFileName` per generare un nome file senza percorso.</span><span class="sxs-lookup"><span data-stu-id="05a3d-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="05a3d-210">Nell'esempio seguente il percorso viene ottenuto dalla configurazione:</span><span class="sxs-lookup"><span data-stu-id="05a3d-210">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="05a3d-211">Il percorso passato al <xref:System.IO.FileStream> *deve* includere il nome del file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="05a3d-212">Se il nome file non viene specificato, viene generata un'<xref:System.UnauthorizedAccessException> in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="05a3d-213">I file caricati con la tecnica <xref:Microsoft.AspNetCore.Http.IFormFile> vengono memorizzati nel buffer in memoria o su disco sul server prima dell'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="05a3d-214">All'interno del metodo di azione, il contenuto del <xref:Microsoft.AspNetCore.Http.IFormFile> è accessibile come <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="05a3d-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="05a3d-215">Oltre al file system locale, i file possono essere salvati in una condivisione di rete o in un servizio di archiviazione file, ad esempio [archiviazione BLOB di Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="05a3d-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="05a3d-216">Per un altro esempio che esegue il ciclo su più file per il caricamento e usa nomi di file sicuri, vedere *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="05a3d-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="05a3d-217">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) genera un'<xref:System.IO.IOException> se vengono creati più di 65.535 file senza eliminare i file temporanei precedenti.</span><span class="sxs-lookup"><span data-stu-id="05a3d-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="05a3d-218">Il limite di 65.535 file è un limite per server.</span><span class="sxs-lookup"><span data-stu-id="05a3d-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="05a3d-219">Per ulteriori informazioni su questo limite per il sistema operativo Windows, vedere la sezione Osservazioni negli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="05a3d-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="05a3d-220">GetTempFileNameA (funzione)</span><span class="sxs-lookup"><span data-stu-id="05a3d-220">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="05a3d-221">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer in un database</span><span class="sxs-lookup"><span data-stu-id="05a3d-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="05a3d-222">Per archiviare i dati dei file binari in un database utilizzando [Entity Framework](/ef/core/index), definire una proprietà di <xref:System.Byte> array nell'entità:</span><span class="sxs-lookup"><span data-stu-id="05a3d-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="05a3d-223">Specificare una proprietà del modello di pagina per la classe che include un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="05a3d-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="05a3d-224"><xref:Microsoft.AspNetCore.Http.IFormFile> possibile utilizzare direttamente come parametro del metodo di azione o come proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="05a3d-225">Nell'esempio precedente viene utilizzata una proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="05a3d-226">Il `FileUpload` viene utilizzato nel formato Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="05a3d-226">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="05a3d-227">Quando il modulo viene inviato al server, copiare il <xref:Microsoft.AspNetCore.Http.IFormFile> in un flusso e salvarlo come una matrice di byte nel database.</span><span class="sxs-lookup"><span data-stu-id="05a3d-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="05a3d-228">Nell'esempio seguente `_dbContext` archivia il contesto di database dell'app:</span><span class="sxs-lookup"><span data-stu-id="05a3d-228">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="05a3d-229">L'esempio precedente è simile a uno scenario illustrato nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="05a3d-230">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="05a3d-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="05a3d-231">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="05a3d-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="05a3d-232">Quando si archiviano dati binari all'interno di database relazionali, è necessario prestare attenzione, perché l'operazione può influire negativamente sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="05a3d-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="05a3d-233">Non basarsi o considerare attendibile la proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> senza convalida.</span><span class="sxs-lookup"><span data-stu-id="05a3d-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="05a3d-234">È consigliabile utilizzare la proprietà `FileName` solo a scopo di visualizzazione e solo dopo la codifica HTML.</span><span class="sxs-lookup"><span data-stu-id="05a3d-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="05a3d-235">Gli esempi forniti non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="05a3d-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="05a3d-236">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="05a3d-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="05a3d-237">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="05a3d-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="05a3d-238">Convalida</span><span class="sxs-lookup"><span data-stu-id="05a3d-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="05a3d-239">Caricare file di grandi dimensioni con flusso</span><span class="sxs-lookup"><span data-stu-id="05a3d-239">Upload large files with streaming</span></span>

<span data-ttu-id="05a3d-240">Nell'esempio seguente viene illustrato come utilizzare JavaScript per trasmettere un file a un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="05a3d-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="05a3d-241">Il token antifalsificazione del file viene generato usando un attributo di filtro personalizzato e passato alle intestazioni HTTP del client anziché nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="05a3d-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="05a3d-242">Poiché il metodo di azione elabora direttamente i dati caricati, l'associazione del modello di modulo viene disabilitata da un altro filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="05a3d-243">All'interno dell'azione, il contenuto del form viene letto tramite un `MultipartReader`, che legge ogni singola `MultipartSection`, elaborando il file o archiviandone il contenuto, come appropriato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="05a3d-244">Una volta lette le sezioni multiparte, l'azione esegue una propria associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="05a3d-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="05a3d-245">La risposta della pagina iniziale carica il modulo e salva un token antifalsificazione in un cookie (tramite l'attributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="05a3d-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="05a3d-246">L'attributo usa il [supporto antifalsificazione](xref:security/anti-request-forgery) predefinito di ASP.NET Core per impostare un cookie con un token di richiesta:</span><span class="sxs-lookup"><span data-stu-id="05a3d-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="05a3d-247">Il `DisableFormValueModelBindingAttribute` viene usato per disabilitare l'associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="05a3d-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="05a3d-248">Nell'app di esempio `GenerateAntiforgeryTokenCookieAttribute` e i `DisableFormValueModelBindingAttribute` vengono applicati come filtri ai modelli di applicazione della pagina di `/StreamedSingleFileUploadDb` e `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` usando le [convenzioni di Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="05a3d-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="05a3d-249">Poiché l'associazione di modelli non legge il modulo, i parametri associati al modulo non vengono associati (la query, la route e l'intestazione continuano a funzionare).</span><span class="sxs-lookup"><span data-stu-id="05a3d-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="05a3d-250">Il metodo di azione funziona direttamente con la proprietà `Request`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="05a3d-251">Per leggere ogni sezione, viene usato un `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="05a3d-252">I dati chiave/valore vengono archiviati in un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="05a3d-253">Una volta lette le sezioni multipart, il contenuto del `KeyValueAccumulator` viene utilizzato per associare i dati del modulo a un tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="05a3d-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="05a3d-254">Metodo di `StreamingController.UploadDatabase` completo per lo streaming in un database con EF Core:</span><span class="sxs-lookup"><span data-stu-id="05a3d-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="05a3d-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="05a3d-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="05a3d-256">Il metodo di `StreamingController.UploadPhysical` completo per lo streaming in una posizione fisica:</span><span class="sxs-lookup"><span data-stu-id="05a3d-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="05a3d-257">Nell'app di esempio, i controlli di convalida vengono gestiti da `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="05a3d-258">Validation</span><span class="sxs-lookup"><span data-stu-id="05a3d-258">Validation</span></span>

<span data-ttu-id="05a3d-259">La classe di `FileHelpers` dell'app di esempio illustra diversi controlli per i caricamenti di file <xref:Microsoft.AspNetCore.Http.IFormFile> memorizzati nel buffer e con flusso.</span><span class="sxs-lookup"><span data-stu-id="05a3d-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="05a3d-260">Per l'elaborazione <xref:Microsoft.AspNetCore.Http.IFormFile> caricamenti di file memorizzati nel buffer nell'app di esempio, vedere il metodo `ProcessFormFile` nel file *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="05a3d-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="05a3d-261">Per l'elaborazione di file trasmessi, vedere il metodo `ProcessStreamedFile` nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="05a3d-262">I metodi di elaborazione della convalida illustrati nell'app di esempio non analizzano il contenuto dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="05a3d-263">Nella maggior parte degli scenari di produzione, nel file viene usata un'API scanner di virus/malware prima di rendere disponibile il file per gli utenti o altri sistemi.</span><span class="sxs-lookup"><span data-stu-id="05a3d-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="05a3d-264">Sebbene l'esempio di argomento fornisca un esempio funzionante di tecniche di convalida, non implementare la classe `FileHelpers` in un'app di produzione a meno che non si:</span><span class="sxs-lookup"><span data-stu-id="05a3d-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="05a3d-265">Comprendere completamente l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="05a3d-266">Modificare l'implementazione in modo appropriato per l'ambiente e le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="05a3d-267">**Non implementare mai indiscriminatamente il codice di sicurezza in un'app senza soddisfare questi requisiti.**</span><span class="sxs-lookup"><span data-stu-id="05a3d-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="05a3d-268">Convalida contenuto</span><span class="sxs-lookup"><span data-stu-id="05a3d-268">Content validation</span></span>

<span data-ttu-id="05a3d-269">**Usare un'API di analisi di virus/malware di terze parti nel contenuto caricato.**</span><span class="sxs-lookup"><span data-stu-id="05a3d-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="05a3d-270">L'analisi dei file richiede le risorse del server in scenari con volumi elevati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="05a3d-271">Se le prestazioni di elaborazione delle richieste sono diminuite a causa dell'analisi dei file, provare a eseguire l'offload del lavoro di analisi in un [servizio in background](xref:fundamentals/host/hosted-services), possibilmente in un servizio in esecuzione in un server diverso dal server dell'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="05a3d-272">In genere, i file caricati vengono mantenuti in un'area in quarantena finché non vengono controllati dal programma antivirus in background.</span><span class="sxs-lookup"><span data-stu-id="05a3d-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="05a3d-273">Quando un file passa, il file viene spostato nel percorso di archiviazione file normale.</span><span class="sxs-lookup"><span data-stu-id="05a3d-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="05a3d-274">Questi passaggi vengono in genere eseguiti insieme a un record di database che indica lo stato di analisi di un file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="05a3d-275">Usando un approccio di questo tipo, l'app e il server app rimangono incentrati sulla risposta alle richieste.</span><span class="sxs-lookup"><span data-stu-id="05a3d-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="05a3d-276">Convalida dell'estensione di file</span><span class="sxs-lookup"><span data-stu-id="05a3d-276">File extension validation</span></span>

<span data-ttu-id="05a3d-277">L'estensione del file caricato deve essere verificata rispetto a un elenco di estensioni consentite.</span><span class="sxs-lookup"><span data-stu-id="05a3d-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="05a3d-278">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="05a3d-279">Convalida della firma del file</span><span class="sxs-lookup"><span data-stu-id="05a3d-279">File signature validation</span></span>

<span data-ttu-id="05a3d-280">La firma di un file è determinata dai primi byte all'inizio di un file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="05a3d-281">Questi byte possono essere usati per indicare se l'estensione corrisponde al contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="05a3d-282">L'app di esempio controlla le firme dei file per alcuni tipi di file comuni.</span><span class="sxs-lookup"><span data-stu-id="05a3d-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="05a3d-283">Nell'esempio seguente, la firma del file per un'immagine JPEG viene verificata rispetto al file:</span><span class="sxs-lookup"><span data-stu-id="05a3d-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="05a3d-284">Per ottenere ulteriori firme di file, vedere il [database delle firme file](https://www.filesignatures.net/) e le specifiche del file ufficiale.</span><span class="sxs-lookup"><span data-stu-id="05a3d-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="05a3d-285">Sicurezza del nome file</span><span class="sxs-lookup"><span data-stu-id="05a3d-285">File name security</span></span>

<span data-ttu-id="05a3d-286">Non usare mai un nome file fornito dal client per salvare un file nell'archiviazione fisica.</span><span class="sxs-lookup"><span data-stu-id="05a3d-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="05a3d-287">Creare un nome file sicuro per il file usando [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per creare un percorso completo (incluso il nome file) per l'archiviazione temporanea.</span><span class="sxs-lookup"><span data-stu-id="05a3d-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="05a3d-288">Razor HTML codifica automaticamente i valori delle proprietà per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="05a3d-289">Il codice seguente è sicuro da usare:</span><span class="sxs-lookup"><span data-stu-id="05a3d-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="05a3d-290">All'esterno di Razor, <xref:System.Net.WebUtility.HtmlEncode*> sempre il contenuto del nome file dalla richiesta di un utente.</span><span class="sxs-lookup"><span data-stu-id="05a3d-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="05a3d-291">Molte implementazioni devono includere un controllo per verificare che il file esista. in caso contrario, il file viene sovrascritto da un file con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="05a3d-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="05a3d-292">Fornire logica aggiuntiva per soddisfare le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="05a3d-293">Convalida dimensioni</span><span class="sxs-lookup"><span data-stu-id="05a3d-293">Size validation</span></span>

<span data-ttu-id="05a3d-294">Limitare le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="05a3d-295">Nell'app di esempio, le dimensioni del file sono limitate a 2 MB (indicate in byte).</span><span class="sxs-lookup"><span data-stu-id="05a3d-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="05a3d-296">Il limite viene fornito tramite la [configurazione](xref:fundamentals/configuration/index) dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="05a3d-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="05a3d-297">Il `FileSizeLimit` viene inserito nelle classi `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="05a3d-298">Quando le dimensioni di un file superano il limite, il file viene rifiutato:</span><span class="sxs-lookup"><span data-stu-id="05a3d-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="05a3d-299">Corrisponde al valore dell'attributo Name al nome del parametro del metodo POST</span><span class="sxs-lookup"><span data-stu-id="05a3d-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="05a3d-300">Nei moduli non Razor che INVIAno dati del modulo o usano direttamente `FormData` di JavaScript, il nome specificato nell'elemento o `FormData` del form deve corrispondere al nome del parametro nell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="05a3d-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="05a3d-301">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="05a3d-301">In the following example:</span></span>

* <span data-ttu-id="05a3d-302">Quando si usa un elemento `<input>`, l'attributo `name` viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="05a3d-303">Quando si usa `FormData` in JavaScript, il nome viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="05a3d-304">Usare un nome corrispondente per il parametro del C# metodo (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="05a3d-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="05a3d-305">Per un metodo gestore di pagina Razor Pages denominato `Upload`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="05a3d-306">Per un metodo di azione POST controller MVC:</span><span class="sxs-lookup"><span data-stu-id="05a3d-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="05a3d-307">Configurazione del server e dell'app</span><span class="sxs-lookup"><span data-stu-id="05a3d-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="05a3d-308">Limite di lunghezza del corpo multipart</span><span class="sxs-lookup"><span data-stu-id="05a3d-308">Multipart body length limit</span></span>

<span data-ttu-id="05a3d-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> imposta il limite per la lunghezza di ogni corpo multipart.</span><span class="sxs-lookup"><span data-stu-id="05a3d-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="05a3d-310">Le sezioni del modulo che superano questo limite generano una <xref:System.IO.InvalidDataException> durante l'analisi.</span><span class="sxs-lookup"><span data-stu-id="05a3d-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="05a3d-311">Il valore predefinito è 134.217.728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="05a3d-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="05a3d-312">Personalizzare il limite usando l'impostazione <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="05a3d-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> viene utilizzato per impostare il <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="05a3d-314">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    });
```

<span data-ttu-id="05a3d-315">In un'app Razor Pages o in un'app MVC, applicare il filtro al modello di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="05a3d-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="05a3d-316">Dimensioni massime del corpo della richiesta di gheppio</span><span class="sxs-lookup"><span data-stu-id="05a3d-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="05a3d-317">Per le app ospitate da gheppio, le dimensioni massime predefinite del corpo della richiesta sono pari a 30 milioni byte, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="05a3d-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="05a3d-318">Personalizzare il limite usando l'opzione del server [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) gheppio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="05a3d-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> viene usato per impostare [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="05a3d-320">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    });
```

<span data-ttu-id="05a3d-321">In un'app Razor Pages o in un'app MVC, applicare il filtro alla classe del gestore di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="05a3d-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="05a3d-322">Il `RequestSizeLimitAttribute` può essere applicato anche usando la direttiva Razor [`@attribute`](xref:mvc/views/razor#attribute) :</span><span class="sxs-lookup"><span data-stu-id="05a3d-322">The `RequestSizeLimitAttribute` can also be applied using the [`@attribute`](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="05a3d-323">Altri limiti del gheppio</span><span class="sxs-lookup"><span data-stu-id="05a3d-323">Other Kestrel limits</span></span>

<span data-ttu-id="05a3d-324">Per le app ospitate da gheppio possono essere applicati altri limiti di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="05a3d-325">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="05a3d-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="05a3d-326">Frequenza dei dati di richiesta e risposta</span><span class="sxs-lookup"><span data-stu-id="05a3d-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="05a3d-327">Limite lunghezza contenuto IIS</span><span class="sxs-lookup"><span data-stu-id="05a3d-327">IIS content length limit</span></span>

<span data-ttu-id="05a3d-328">Il limite di richieste predefinito (`maxAllowedContentLength`) è 30 milioni byte, che corrisponde a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="05a3d-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="05a3d-329">Personalizzare il limite nel file *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="05a3d-329">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="05a3d-330">Questa impostazione si applica solo a IIS.</span><span class="sxs-lookup"><span data-stu-id="05a3d-330">This setting only applies to IIS.</span></span> <span data-ttu-id="05a3d-331">Il comportamento non si verifica per impostazione predefinita con l'hosting in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="05a3d-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="05a3d-332">Per altre informazioni, vedere [limiti delle richieste \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="05a3d-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="05a3d-333">Le limitazioni del modulo ASP.NET Core o della presenza del modulo filtro richieste IIS possono limitare il caricamento a 2 o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="05a3d-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="05a3d-334">Per ulteriori informazioni, vedere [non è possibile caricare file di dimensioni superiori a 2 GB (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="05a3d-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="05a3d-335">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="05a3d-335">Troubleshoot</span></span>

<span data-ttu-id="05a3d-336">Di seguito sono trattati alcuni problemi comuni riscontrati durante il caricamento di file, con le soluzioni possibili corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="05a3d-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="05a3d-337">Errore non trovato quando viene distribuito in un server IIS</span><span class="sxs-lookup"><span data-stu-id="05a3d-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="05a3d-338">L'errore seguente indica che il file caricato supera la lunghezza del contenuto configurata del server:</span><span class="sxs-lookup"><span data-stu-id="05a3d-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="05a3d-339">Per ulteriori informazioni sull'aumento del limite, vedere la sezione [limite di lunghezza del contenuto IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="05a3d-340">Errore di connessione</span><span class="sxs-lookup"><span data-stu-id="05a3d-340">Connection failure</span></span>

<span data-ttu-id="05a3d-341">Un errore di connessione e una connessione al server di reimpostazione probabilmente indicano che il file caricato supera le dimensioni massime del corpo della richiesta del gheppio.</span><span class="sxs-lookup"><span data-stu-id="05a3d-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="05a3d-342">Per ulteriori informazioni, vedere la sezione relativa alle [dimensioni massime del corpo della richiesta di Gheppio](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="05a3d-343">I limiti di connessione del client gheppio possono richiedere anche modifiche.</span><span class="sxs-lookup"><span data-stu-id="05a3d-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="05a3d-344">Eccezione per riferimento Null con IFormFile</span><span class="sxs-lookup"><span data-stu-id="05a3d-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="05a3d-345">Se il controller accetta file caricati usando <xref:Microsoft.AspNetCore.Http.IFormFile> ma il valore è `null`, verificare che il formato HTML specifichi un valore `enctype` di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="05a3d-346">Se questo attributo non è impostato sull'elemento `<form>`, il caricamento del file non viene eseguito e gli argomenti <xref:Microsoft.AspNetCore.Http.IFormFile> associati sono `null`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="05a3d-347">Verificare inoltre che la [denominazione di caricamento nei dati del modulo corrisponda alla denominazione dell'app](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="05a3d-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="05a3d-348">Il flusso è troppo lungo</span><span class="sxs-lookup"><span data-stu-id="05a3d-348">Stream was too long</span></span>

<span data-ttu-id="05a3d-349">Gli esempi in questo argomento si basano su <xref:System.IO.MemoryStream> per conservare il contenuto del file caricato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-349">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="05a3d-350">Il limite delle dimensioni di un `MemoryStream` è `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-350">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="05a3d-351">Se lo scenario di caricamento dei file dell'app richiede un contenuto del file di dimensioni superiori a 50 MB, usare un approccio alternativo che non si basa su un singolo `MemoryStream` per contenere il contenuto di un file caricato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-351">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="05a3d-352">ASP.NET Core supporta il caricamento di uno o più file mediante l'associazione di modelli memorizzati nel buffer per i file più piccoli e il flusso non memorizzato nel buffer per file di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="05a3d-352">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="05a3d-353">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="05a3d-353">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="05a3d-354">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="05a3d-354">Security considerations</span></span>

<span data-ttu-id="05a3d-355">Prestare attenzione quando si fornisce agli utenti la possibilità di caricare file in un server.</span><span class="sxs-lookup"><span data-stu-id="05a3d-355">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="05a3d-356">Gli utenti malintenzionati possono tentare di:</span><span class="sxs-lookup"><span data-stu-id="05a3d-356">Attackers may attempt to:</span></span>

* <span data-ttu-id="05a3d-357">Eseguire attacchi [di tipo Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-357">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="05a3d-358">Caricare virus o malware.</span><span class="sxs-lookup"><span data-stu-id="05a3d-358">Upload viruses or malware.</span></span>
* <span data-ttu-id="05a3d-359">Compromettere le reti e i server in altri modi.</span><span class="sxs-lookup"><span data-stu-id="05a3d-359">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="05a3d-360">I passaggi di sicurezza che riducono la probabilità di un attacco riuscito sono:</span><span class="sxs-lookup"><span data-stu-id="05a3d-360">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="05a3d-361">Caricare i file in un'area di caricamento file dedicata, preferibilmente in un'unità non di sistema.</span><span class="sxs-lookup"><span data-stu-id="05a3d-361">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="05a3d-362">Un percorso dedicato semplifica l'imposizione delle restrizioni di sicurezza sui file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-362">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="05a3d-363">Disabilitare le autorizzazioni di esecuzione per il percorso di caricamento del file.&dagger;</span><span class="sxs-lookup"><span data-stu-id="05a3d-363">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="05a3d-364">Non **rende permanente i** file caricati nella stessa struttura di directory dell'app.&dagger;</span><span class="sxs-lookup"><span data-stu-id="05a3d-364">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="05a3d-365">Usare un nome file sicuro determinato dall'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-365">Use a safe file name determined by the app.</span></span> <span data-ttu-id="05a3d-366">Non usare un nome file fornito dall'utente o il nome file non attendibile del file caricato.&dagger; HTML codifica il nome file non attendibile quando viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-366">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="05a3d-367">Ad esempio, la registrazione del nome del file o la visualizzazione nell'interfaccia utente (Razor codifica automaticamente l'output).</span><span class="sxs-lookup"><span data-stu-id="05a3d-367">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="05a3d-368">Consenti solo le estensioni di file approvate per la specifica di progettazione dell'app.&dagger;</span><span class="sxs-lookup"><span data-stu-id="05a3d-368">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="05a3d-369">Verificare che i controlli lato client vengano eseguiti sul server.&dagger; controlli lato client sono facili da aggirare.</span><span class="sxs-lookup"><span data-stu-id="05a3d-369">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="05a3d-370">Controllare le dimensioni di un file caricato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-370">Check the size of an uploaded file.</span></span> <span data-ttu-id="05a3d-371">Impostare un limite di dimensioni massime per impedire caricamenti di grandi dimensioni.&dagger;</span><span class="sxs-lookup"><span data-stu-id="05a3d-371">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="05a3d-372">Quando i file non devono essere sovrascritti da un file caricato con lo stesso nome, controllare il nome del file sul database o sull'archiviazione fisica prima di caricare il file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-372">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="05a3d-373">**Eseguire uno scanner virus/malware sul contenuto caricato prima che il file venga archiviato.**</span><span class="sxs-lookup"><span data-stu-id="05a3d-373">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="05a3d-374">&dagger;l'app di esempio illustra un approccio che soddisfa i criteri.</span><span class="sxs-lookup"><span data-stu-id="05a3d-374">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="05a3d-375">Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:</span><span class="sxs-lookup"><span data-stu-id="05a3d-375">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="05a3d-376">Ottenere il controllo completo di un sistema.</span><span class="sxs-lookup"><span data-stu-id="05a3d-376">Completely gain control of a system.</span></span>
> * <span data-ttu-id="05a3d-377">Sovraccaricare un sistema con il risultato che si verifica un arresto anomalo del sistema.</span><span class="sxs-lookup"><span data-stu-id="05a3d-377">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="05a3d-378">Compromettere i dati di sistemi o utenti.</span><span class="sxs-lookup"><span data-stu-id="05a3d-378">Compromise user or system data.</span></span>
> * <span data-ttu-id="05a3d-379">Applicare i graffiti a un'interfaccia utente pubblica.</span><span class="sxs-lookup"><span data-stu-id="05a3d-379">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="05a3d-380">Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="05a3d-380">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="05a3d-381">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="05a3d-381">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="05a3d-382">Protezione di Azure: Verificare i controlli appropriati quando si accettano file dagli utenti</span><span class="sxs-lookup"><span data-stu-id="05a3d-382">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="05a3d-383">Per ulteriori informazioni sull'implementazione di misure di sicurezza, inclusi esempi dall'app di esempio, vedere la sezione [convalida](#validation) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-383">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="05a3d-384">Scenari di archiviazione</span><span class="sxs-lookup"><span data-stu-id="05a3d-384">Storage scenarios</span></span>

<span data-ttu-id="05a3d-385">Le opzioni di archiviazione comuni per i file includono:</span><span class="sxs-lookup"><span data-stu-id="05a3d-385">Common storage options for files include:</span></span>

* <span data-ttu-id="05a3d-386">Database di</span><span class="sxs-lookup"><span data-stu-id="05a3d-386">Database</span></span>

  * <span data-ttu-id="05a3d-387">Per i caricamenti di file di piccole dimensioni, un database è spesso più veloce rispetto alle opzioni di archiviazione fisica (file system o condivisione di rete).</span><span class="sxs-lookup"><span data-stu-id="05a3d-387">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="05a3d-388">Un database è spesso più pratico delle opzioni di archiviazione fisica perché il recupero di un record di database per i dati utente può fornire simultaneamente il contenuto del file, ad esempio un'immagine avatar.</span><span class="sxs-lookup"><span data-stu-id="05a3d-388">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="05a3d-389">Un database è potenzialmente meno costoso rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-389">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="05a3d-390">Archiviazione fisica (file system o condivisione di rete)</span><span class="sxs-lookup"><span data-stu-id="05a3d-390">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="05a3d-391">Per i caricamenti di file di grandi dimensioni:</span><span class="sxs-lookup"><span data-stu-id="05a3d-391">For large file uploads:</span></span>
    * <span data-ttu-id="05a3d-392">I limiti del database possono limitare le dimensioni del caricamento.</span><span class="sxs-lookup"><span data-stu-id="05a3d-392">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="05a3d-393">L'archiviazione fisica è spesso meno economica rispetto all'archiviazione in un database.</span><span class="sxs-lookup"><span data-stu-id="05a3d-393">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="05a3d-394">L'archiviazione fisica è potenzialmente meno costosa rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-394">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="05a3d-395">Il processo dell'app deve avere le autorizzazioni di lettura e scrittura per il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-395">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="05a3d-396">**Non concedere mai l'autorizzazione Execute.**</span><span class="sxs-lookup"><span data-stu-id="05a3d-396">**Never grant execute permission.**</span></span>

* <span data-ttu-id="05a3d-397">Servizio di archiviazione dei dati (ad esempio, [archiviazione BLOB di Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="05a3d-397">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="05a3d-398">I servizi in genere offrono una maggiore scalabilità e resilienza rispetto a soluzioni locali che in genere sono soggette a singoli punti di errore.</span><span class="sxs-lookup"><span data-stu-id="05a3d-398">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="05a3d-399">I servizi sono un costo potenzialmente inferiore in scenari di infrastruttura di archiviazione di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="05a3d-399">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="05a3d-400">Per altre informazioni, vedere [Guida introduttiva: usare .NET per creare un BLOB nell'archivio oggetti](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="05a3d-400">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="05a3d-401">Nell'argomento viene illustrato <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, ma è possibile utilizzare <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> per salvare un <xref:System.IO.FileStream> nell'archiviazione BLOB quando si utilizza un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="05a3d-401">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="05a3d-402">Scenari di caricamento di file</span><span class="sxs-lookup"><span data-stu-id="05a3d-402">File upload scenarios</span></span>

<span data-ttu-id="05a3d-403">Due approcci generali per il caricamento di file sono il buffering e il flusso.</span><span class="sxs-lookup"><span data-stu-id="05a3d-403">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="05a3d-404">**Buffer**</span><span class="sxs-lookup"><span data-stu-id="05a3d-404">**Buffering**</span></span>

<span data-ttu-id="05a3d-405">L'intero file viene letto in un <xref:Microsoft.AspNetCore.Http.IFormFile>, ovvero una C# rappresentazione del file usato per elaborare o salvare il file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-405">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="05a3d-406">Le risorse (disco, memoria) utilizzate dai caricamenti di file dipendono dal numero e dalle dimensioni dei caricamenti di file simultanei.</span><span class="sxs-lookup"><span data-stu-id="05a3d-406">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="05a3d-407">Se un'app tenta di memorizzare nel buffer un numero eccessivo di caricamenti, il sito si arresta in modo anomalo quando esaurisce la memoria o lo spazio su disco.</span><span class="sxs-lookup"><span data-stu-id="05a3d-407">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="05a3d-408">Se le dimensioni o la frequenza dei caricamenti di file esauriscono le risorse dell'app, usare il flusso.</span><span class="sxs-lookup"><span data-stu-id="05a3d-408">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="05a3d-409">Ogni singolo file memorizzato nel buffer che supera 64 KB viene spostato dalla memoria in un file temporaneo su disco.</span><span class="sxs-lookup"><span data-stu-id="05a3d-409">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="05a3d-410">Il buffering di file di piccole dimensioni viene trattato nelle sezioni seguenti di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="05a3d-410">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="05a3d-411">Archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="05a3d-411">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="05a3d-412">Database</span><span class="sxs-lookup"><span data-stu-id="05a3d-412">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="05a3d-413">**Flusso**</span><span class="sxs-lookup"><span data-stu-id="05a3d-413">**Streaming**</span></span>

<span data-ttu-id="05a3d-414">Il file viene ricevuto da una richiesta multipart ed elaborato o salvato direttamente dall'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-414">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="05a3d-415">Il flusso non migliora significativamente le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="05a3d-415">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="05a3d-416">Il flusso riduce le richieste di memoria o di spazio su disco durante il caricamento dei file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-416">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="05a3d-417">Il flusso di file di grandi dimensioni è trattato nella sezione [caricare file di grandi dimensioni con flusso](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-417">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="05a3d-418">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer all'archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="05a3d-418">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="05a3d-419">Per caricare file di piccole dimensioni, usare un modulo multipart o creare una richiesta POST usando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="05a3d-419">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="05a3d-420">L'esempio seguente illustra l'uso di un modulo di Razor Pages per caricare un singolo file (*pages/BufferedSingleFileUploadPhysical. cshtml* nell'app di esempio):</span><span class="sxs-lookup"><span data-stu-id="05a3d-420">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="05a3d-421">L'esempio seguente è analogo all'esempio precedente, ad eccezione del fatto che:</span><span class="sxs-lookup"><span data-stu-id="05a3d-421">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="05a3d-422">Per inviare i dati del modulo, viene usato JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)).</span><span class="sxs-lookup"><span data-stu-id="05a3d-422">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="05a3d-423">Nessuna convalida.</span><span class="sxs-lookup"><span data-stu-id="05a3d-423">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="05a3d-424">Per eseguire il POST del form in JavaScript per i client che [non supportano l'API fetch](https://caniuse.com/#feat=fetch), usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="05a3d-424">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="05a3d-425">Usare un riempimento di recupero (ad esempio, [Window. fetch Refill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="05a3d-425">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="05a3d-426">Usare `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-426">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="05a3d-427">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-427">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="05a3d-428">Per supportare il caricamento di file, i moduli HTML devono specificare un tipo di codifica (`enctype`) di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-428">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="05a3d-429">Affinché un `files` elemento input supporti il caricamento di più file, fornire l'attributo `multiple` sull'elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-429">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="05a3d-430">È possibile accedere ai singoli file caricati nel server tramite l' [associazione di modelli](xref:mvc/models/model-binding) utilizzando <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="05a3d-430">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="05a3d-431">L'app di esempio illustra più caricamenti di file memorizzati nel buffer per scenari di archiviazione di database e fisici.</span><span class="sxs-lookup"><span data-stu-id="05a3d-431">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="05a3d-432">Non **utilizzare la** proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> diversa da per la visualizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-432">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="05a3d-433">Durante la visualizzazione o la registrazione, HTML codifica il nome del file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-433">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="05a3d-434">Un utente malintenzionato può fornire un nome file dannoso, inclusi percorsi completi o percorsi relativi.</span><span class="sxs-lookup"><span data-stu-id="05a3d-434">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="05a3d-435">Le applicazioni devono:</span><span class="sxs-lookup"><span data-stu-id="05a3d-435">Applications should:</span></span>
>
> * <span data-ttu-id="05a3d-436">Rimuovere il percorso dal nome file specificato dall'utente.</span><span class="sxs-lookup"><span data-stu-id="05a3d-436">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="05a3d-437">Salvare il nome file con codifica HTML, percorso-rimosso per l'interfaccia utente o la registrazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-437">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="05a3d-438">Genera un nuovo nome file casuale per l'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-438">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="05a3d-439">Il codice seguente rimuove il percorso dal nome del file:</span><span class="sxs-lookup"><span data-stu-id="05a3d-439">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="05a3d-440">Gli esempi forniti finora non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="05a3d-440">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="05a3d-441">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="05a3d-441">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="05a3d-442">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="05a3d-442">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="05a3d-443">Convalida</span><span class="sxs-lookup"><span data-stu-id="05a3d-443">Validation</span></span>](#validation)

<span data-ttu-id="05a3d-444">Quando si caricano file usando l'associazione di modelli e <xref:Microsoft.AspNetCore.Http.IFormFile>, il metodo di azione può accettare:</span><span class="sxs-lookup"><span data-stu-id="05a3d-444">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="05a3d-445">Singolo <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="05a3d-445">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="05a3d-446">Qualsiasi raccolta seguente che rappresenta diversi file:</span><span class="sxs-lookup"><span data-stu-id="05a3d-446">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="05a3d-447">[Elenca](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="05a3d-447">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="05a3d-448">L'associazione corrisponde ai file di form in base al nome.</span><span class="sxs-lookup"><span data-stu-id="05a3d-448">Binding matches form files by name.</span></span> <span data-ttu-id="05a3d-449">Ad esempio, il valore HTML `name` in `<input type="file" name="formFile">` deve corrispondere al C# parametro/proprietà associato (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="05a3d-449">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="05a3d-450">Per ulteriori informazioni, vedere la sezione [corrispondenza del nome dell'attributo del nome del parametro del metodo post](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-450">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="05a3d-451">L'esempio seguente consente di:</span><span class="sxs-lookup"><span data-stu-id="05a3d-451">The following example:</span></span>

* <span data-ttu-id="05a3d-452">Esegue il ciclo di uno o più file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-452">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="05a3d-453">USA [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per restituire un percorso completo per un file, incluso il nome del file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-453">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="05a3d-454">Salva i file nella file system locale usando un nome file generato dall'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-454">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="05a3d-455">Restituisce il numero totale e le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-455">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="05a3d-456">Utilizzare `Path.GetRandomFileName` per generare un nome file senza percorso.</span><span class="sxs-lookup"><span data-stu-id="05a3d-456">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="05a3d-457">Nell'esempio seguente il percorso viene ottenuto dalla configurazione:</span><span class="sxs-lookup"><span data-stu-id="05a3d-457">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="05a3d-458">Il percorso passato al <xref:System.IO.FileStream> *deve* includere il nome del file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-458">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="05a3d-459">Se il nome file non viene specificato, viene generata un'<xref:System.UnauthorizedAccessException> in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-459">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="05a3d-460">I file caricati con la tecnica <xref:Microsoft.AspNetCore.Http.IFormFile> vengono memorizzati nel buffer in memoria o su disco sul server prima dell'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-460">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="05a3d-461">All'interno del metodo di azione, il contenuto del <xref:Microsoft.AspNetCore.Http.IFormFile> è accessibile come <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="05a3d-461">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="05a3d-462">Oltre al file system locale, i file possono essere salvati in una condivisione di rete o in un servizio di archiviazione file, ad esempio [archiviazione BLOB di Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="05a3d-462">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="05a3d-463">Per un altro esempio che esegue il ciclo su più file per il caricamento e usa nomi di file sicuri, vedere *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="05a3d-463">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="05a3d-464">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) genera un'<xref:System.IO.IOException> se vengono creati più di 65.535 file senza eliminare i file temporanei precedenti.</span><span class="sxs-lookup"><span data-stu-id="05a3d-464">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="05a3d-465">Il limite di 65.535 file è un limite per server.</span><span class="sxs-lookup"><span data-stu-id="05a3d-465">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="05a3d-466">Per ulteriori informazioni su questo limite per il sistema operativo Windows, vedere la sezione Osservazioni negli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="05a3d-466">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="05a3d-467">GetTempFileNameA (funzione)</span><span class="sxs-lookup"><span data-stu-id="05a3d-467">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="05a3d-468">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer in un database</span><span class="sxs-lookup"><span data-stu-id="05a3d-468">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="05a3d-469">Per archiviare i dati dei file binari in un database utilizzando [Entity Framework](/ef/core/index), definire una proprietà di <xref:System.Byte> array nell'entità:</span><span class="sxs-lookup"><span data-stu-id="05a3d-469">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="05a3d-470">Specificare una proprietà del modello di pagina per la classe che include un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="05a3d-470">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="05a3d-471"><xref:Microsoft.AspNetCore.Http.IFormFile> possibile utilizzare direttamente come parametro del metodo di azione o come proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-471"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="05a3d-472">Nell'esempio precedente viene utilizzata una proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-472">The prior example uses a bound model property.</span></span>

<span data-ttu-id="05a3d-473">Il `FileUpload` viene utilizzato nel formato Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="05a3d-473">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="05a3d-474">Quando il modulo viene inviato al server, copiare il <xref:Microsoft.AspNetCore.Http.IFormFile> in un flusso e salvarlo come una matrice di byte nel database.</span><span class="sxs-lookup"><span data-stu-id="05a3d-474">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="05a3d-475">Nell'esempio seguente `_dbContext` archivia il contesto di database dell'app:</span><span class="sxs-lookup"><span data-stu-id="05a3d-475">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="05a3d-476">L'esempio precedente è simile a uno scenario illustrato nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-476">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="05a3d-477">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="05a3d-477">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="05a3d-478">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="05a3d-478">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="05a3d-479">Quando si archiviano dati binari all'interno di database relazionali, è necessario prestare attenzione, perché l'operazione può influire negativamente sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="05a3d-479">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="05a3d-480">Non basarsi o considerare attendibile la proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> senza convalida.</span><span class="sxs-lookup"><span data-stu-id="05a3d-480">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="05a3d-481">È consigliabile utilizzare la proprietà `FileName` solo a scopo di visualizzazione e solo dopo la codifica HTML.</span><span class="sxs-lookup"><span data-stu-id="05a3d-481">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="05a3d-482">Gli esempi forniti non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="05a3d-482">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="05a3d-483">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="05a3d-483">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="05a3d-484">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="05a3d-484">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="05a3d-485">Convalida</span><span class="sxs-lookup"><span data-stu-id="05a3d-485">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="05a3d-486">Caricare file di grandi dimensioni con flusso</span><span class="sxs-lookup"><span data-stu-id="05a3d-486">Upload large files with streaming</span></span>

<span data-ttu-id="05a3d-487">Nell'esempio seguente viene illustrato come utilizzare JavaScript per trasmettere un file a un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="05a3d-487">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="05a3d-488">Il token antifalsificazione del file viene generato usando un attributo di filtro personalizzato e passato alle intestazioni HTTP del client anziché nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="05a3d-488">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="05a3d-489">Poiché il metodo di azione elabora direttamente i dati caricati, l'associazione del modello di modulo viene disabilitata da un altro filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-489">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="05a3d-490">All'interno dell'azione, il contenuto del form viene letto tramite un `MultipartReader`, che legge ogni singola `MultipartSection`, elaborando il file o archiviandone il contenuto, come appropriato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-490">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="05a3d-491">Una volta lette le sezioni multiparte, l'azione esegue una propria associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="05a3d-491">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="05a3d-492">La risposta della pagina iniziale carica il modulo e salva un token antifalsificazione in un cookie (tramite l'attributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="05a3d-492">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="05a3d-493">L'attributo usa il [supporto antifalsificazione](xref:security/anti-request-forgery) predefinito di ASP.NET Core per impostare un cookie con un token di richiesta:</span><span class="sxs-lookup"><span data-stu-id="05a3d-493">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="05a3d-494">Il `DisableFormValueModelBindingAttribute` viene usato per disabilitare l'associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="05a3d-494">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="05a3d-495">Nell'app di esempio `GenerateAntiforgeryTokenCookieAttribute` e i `DisableFormValueModelBindingAttribute` vengono applicati come filtri ai modelli di applicazione della pagina di `/StreamedSingleFileUploadDb` e `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` usando le [convenzioni di Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="05a3d-495">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="05a3d-496">Poiché l'associazione di modelli non legge il modulo, i parametri associati al modulo non vengono associati (la query, la route e l'intestazione continuano a funzionare).</span><span class="sxs-lookup"><span data-stu-id="05a3d-496">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="05a3d-497">Il metodo di azione funziona direttamente con la proprietà `Request`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-497">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="05a3d-498">Per leggere ogni sezione, viene usato un `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-498">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="05a3d-499">I dati chiave/valore vengono archiviati in un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-499">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="05a3d-500">Una volta lette le sezioni multipart, il contenuto del `KeyValueAccumulator` viene utilizzato per associare i dati del modulo a un tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="05a3d-500">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="05a3d-501">Metodo di `StreamingController.UploadDatabase` completo per lo streaming in un database con EF Core:</span><span class="sxs-lookup"><span data-stu-id="05a3d-501">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="05a3d-502">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="05a3d-502">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="05a3d-503">Il metodo di `StreamingController.UploadPhysical` completo per lo streaming in una posizione fisica:</span><span class="sxs-lookup"><span data-stu-id="05a3d-503">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="05a3d-504">Nell'app di esempio, i controlli di convalida vengono gestiti da `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-504">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="05a3d-505">Validation</span><span class="sxs-lookup"><span data-stu-id="05a3d-505">Validation</span></span>

<span data-ttu-id="05a3d-506">La classe di `FileHelpers` dell'app di esempio illustra diversi controlli per i caricamenti di file <xref:Microsoft.AspNetCore.Http.IFormFile> memorizzati nel buffer e con flusso.</span><span class="sxs-lookup"><span data-stu-id="05a3d-506">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="05a3d-507">Per l'elaborazione <xref:Microsoft.AspNetCore.Http.IFormFile> caricamenti di file memorizzati nel buffer nell'app di esempio, vedere il metodo `ProcessFormFile` nel file *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="05a3d-507">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="05a3d-508">Per l'elaborazione di file trasmessi, vedere il metodo `ProcessStreamedFile` nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-508">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="05a3d-509">I metodi di elaborazione della convalida illustrati nell'app di esempio non analizzano il contenuto dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-509">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="05a3d-510">Nella maggior parte degli scenari di produzione, nel file viene usata un'API scanner di virus/malware prima di rendere disponibile il file per gli utenti o altri sistemi.</span><span class="sxs-lookup"><span data-stu-id="05a3d-510">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="05a3d-511">Sebbene l'esempio di argomento fornisca un esempio funzionante di tecniche di convalida, non implementare la classe `FileHelpers` in un'app di produzione a meno che non si:</span><span class="sxs-lookup"><span data-stu-id="05a3d-511">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="05a3d-512">Comprendere completamente l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-512">Fully understand the implementation.</span></span>
> * <span data-ttu-id="05a3d-513">Modificare l'implementazione in modo appropriato per l'ambiente e le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-513">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="05a3d-514">**Non implementare mai indiscriminatamente il codice di sicurezza in un'app senza soddisfare questi requisiti.**</span><span class="sxs-lookup"><span data-stu-id="05a3d-514">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="05a3d-515">Convalida contenuto</span><span class="sxs-lookup"><span data-stu-id="05a3d-515">Content validation</span></span>

<span data-ttu-id="05a3d-516">**Usare un'API di analisi di virus/malware di terze parti nel contenuto caricato.**</span><span class="sxs-lookup"><span data-stu-id="05a3d-516">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="05a3d-517">L'analisi dei file richiede le risorse del server in scenari con volumi elevati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-517">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="05a3d-518">Se le prestazioni di elaborazione delle richieste sono diminuite a causa dell'analisi dei file, provare a eseguire l'offload del lavoro di analisi in un [servizio in background](xref:fundamentals/host/hosted-services), possibilmente in un servizio in esecuzione in un server diverso dal server dell'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-518">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="05a3d-519">In genere, i file caricati vengono mantenuti in un'area in quarantena finché non vengono controllati dal programma antivirus in background.</span><span class="sxs-lookup"><span data-stu-id="05a3d-519">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="05a3d-520">Quando un file passa, il file viene spostato nel percorso di archiviazione file normale.</span><span class="sxs-lookup"><span data-stu-id="05a3d-520">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="05a3d-521">Questi passaggi vengono in genere eseguiti insieme a un record di database che indica lo stato di analisi di un file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-521">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="05a3d-522">Usando un approccio di questo tipo, l'app e il server app rimangono incentrati sulla risposta alle richieste.</span><span class="sxs-lookup"><span data-stu-id="05a3d-522">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="05a3d-523">Convalida dell'estensione di file</span><span class="sxs-lookup"><span data-stu-id="05a3d-523">File extension validation</span></span>

<span data-ttu-id="05a3d-524">L'estensione del file caricato deve essere verificata rispetto a un elenco di estensioni consentite.</span><span class="sxs-lookup"><span data-stu-id="05a3d-524">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="05a3d-525">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-525">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="05a3d-526">Convalida della firma del file</span><span class="sxs-lookup"><span data-stu-id="05a3d-526">File signature validation</span></span>

<span data-ttu-id="05a3d-527">La firma di un file è determinata dai primi byte all'inizio di un file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-527">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="05a3d-528">Questi byte possono essere usati per indicare se l'estensione corrisponde al contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="05a3d-528">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="05a3d-529">L'app di esempio controlla le firme dei file per alcuni tipi di file comuni.</span><span class="sxs-lookup"><span data-stu-id="05a3d-529">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="05a3d-530">Nell'esempio seguente, la firma del file per un'immagine JPEG viene verificata rispetto al file:</span><span class="sxs-lookup"><span data-stu-id="05a3d-530">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="05a3d-531">Per ottenere ulteriori firme di file, vedere il [database delle firme file](https://www.filesignatures.net/) e le specifiche del file ufficiale.</span><span class="sxs-lookup"><span data-stu-id="05a3d-531">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="05a3d-532">Sicurezza del nome file</span><span class="sxs-lookup"><span data-stu-id="05a3d-532">File name security</span></span>

<span data-ttu-id="05a3d-533">Non usare mai un nome file fornito dal client per salvare un file nell'archiviazione fisica.</span><span class="sxs-lookup"><span data-stu-id="05a3d-533">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="05a3d-534">Creare un nome file sicuro per il file usando [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per creare un percorso completo (incluso il nome file) per l'archiviazione temporanea.</span><span class="sxs-lookup"><span data-stu-id="05a3d-534">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="05a3d-535">Razor HTML codifica automaticamente i valori delle proprietà per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-535">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="05a3d-536">Il codice seguente è sicuro da usare:</span><span class="sxs-lookup"><span data-stu-id="05a3d-536">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="05a3d-537">All'esterno di Razor, <xref:System.Net.WebUtility.HtmlEncode*> sempre il contenuto del nome file dalla richiesta di un utente.</span><span class="sxs-lookup"><span data-stu-id="05a3d-537">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="05a3d-538">Molte implementazioni devono includere un controllo per verificare che il file esista. in caso contrario, il file viene sovrascritto da un file con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="05a3d-538">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="05a3d-539">Fornire logica aggiuntiva per soddisfare le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="05a3d-539">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="05a3d-540">Convalida dimensioni</span><span class="sxs-lookup"><span data-stu-id="05a3d-540">Size validation</span></span>

<span data-ttu-id="05a3d-541">Limitare le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="05a3d-541">Limit the size of uploaded files.</span></span>

<span data-ttu-id="05a3d-542">Nell'app di esempio, le dimensioni del file sono limitate a 2 MB (indicate in byte).</span><span class="sxs-lookup"><span data-stu-id="05a3d-542">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="05a3d-543">Il limite viene fornito tramite la [configurazione](xref:fundamentals/configuration/index) dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="05a3d-543">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="05a3d-544">Il `FileSizeLimit` viene inserito nelle classi `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-544">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="05a3d-545">Quando le dimensioni di un file superano il limite, il file viene rifiutato:</span><span class="sxs-lookup"><span data-stu-id="05a3d-545">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="05a3d-546">Corrisponde al valore dell'attributo Name al nome del parametro del metodo POST</span><span class="sxs-lookup"><span data-stu-id="05a3d-546">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="05a3d-547">Nei moduli non Razor che INVIAno dati del modulo o usano direttamente `FormData` di JavaScript, il nome specificato nell'elemento o `FormData` del form deve corrispondere al nome del parametro nell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="05a3d-547">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="05a3d-548">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="05a3d-548">In the following example:</span></span>

* <span data-ttu-id="05a3d-549">Quando si usa un elemento `<input>`, l'attributo `name` viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-549">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="05a3d-550">Quando si usa `FormData` in JavaScript, il nome viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-550">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="05a3d-551">Usare un nome corrispondente per il parametro del C# metodo (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="05a3d-551">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="05a3d-552">Per un metodo gestore di pagina Razor Pages denominato `Upload`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-552">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="05a3d-553">Per un metodo di azione POST controller MVC:</span><span class="sxs-lookup"><span data-stu-id="05a3d-553">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="05a3d-554">Configurazione del server e dell'app</span><span class="sxs-lookup"><span data-stu-id="05a3d-554">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="05a3d-555">Limite di lunghezza del corpo multipart</span><span class="sxs-lookup"><span data-stu-id="05a3d-555">Multipart body length limit</span></span>

<span data-ttu-id="05a3d-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> imposta il limite per la lunghezza di ogni corpo multipart.</span><span class="sxs-lookup"><span data-stu-id="05a3d-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="05a3d-557">Le sezioni del modulo che superano questo limite generano una <xref:System.IO.InvalidDataException> durante l'analisi.</span><span class="sxs-lookup"><span data-stu-id="05a3d-557">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="05a3d-558">Il valore predefinito è 134.217.728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="05a3d-558">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="05a3d-559">Personalizzare il limite usando l'impostazione <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-559">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="05a3d-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> viene utilizzato per impostare il <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="05a3d-561">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-561">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="05a3d-562">In un'app Razor Pages o in un'app MVC, applicare il filtro al modello di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="05a3d-562">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="05a3d-563">Dimensioni massime del corpo della richiesta di gheppio</span><span class="sxs-lookup"><span data-stu-id="05a3d-563">Kestrel maximum request body size</span></span>

<span data-ttu-id="05a3d-564">Per le app ospitate da gheppio, le dimensioni massime predefinite del corpo della richiesta sono pari a 30 milioni byte, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="05a3d-564">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="05a3d-565">Personalizzare il limite usando l'opzione del server [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) gheppio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-565">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

<span data-ttu-id="05a3d-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> viene usato per impostare [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="05a3d-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="05a3d-567">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05a3d-567">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="05a3d-568">In un'app Razor Pages o in un'app MVC, applicare il filtro alla classe del gestore di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="05a3d-568">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="05a3d-569">Altri limiti del gheppio</span><span class="sxs-lookup"><span data-stu-id="05a3d-569">Other Kestrel limits</span></span>

<span data-ttu-id="05a3d-570">Per le app ospitate da gheppio possono essere applicati altri limiti di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="05a3d-570">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="05a3d-571">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="05a3d-571">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="05a3d-572">Frequenza dei dati di richiesta e risposta</span><span class="sxs-lookup"><span data-stu-id="05a3d-572">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="05a3d-573">Limite lunghezza contenuto IIS</span><span class="sxs-lookup"><span data-stu-id="05a3d-573">IIS content length limit</span></span>

<span data-ttu-id="05a3d-574">Il limite di richieste predefinito (`maxAllowedContentLength`) è 30 milioni byte, che corrisponde a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="05a3d-574">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="05a3d-575">Personalizzare il limite nel file *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="05a3d-575">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="05a3d-576">Questa impostazione si applica solo a IIS.</span><span class="sxs-lookup"><span data-stu-id="05a3d-576">This setting only applies to IIS.</span></span> <span data-ttu-id="05a3d-577">Il comportamento non si verifica per impostazione predefinita con l'hosting in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="05a3d-577">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="05a3d-578">Per altre informazioni, vedere [limiti delle richieste \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="05a3d-578">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="05a3d-579">Le limitazioni del modulo ASP.NET Core o della presenza del modulo filtro richieste IIS possono limitare il caricamento a 2 o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="05a3d-579">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="05a3d-580">Per ulteriori informazioni, vedere [non è possibile caricare file di dimensioni superiori a 2 GB (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="05a3d-580">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="05a3d-581">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="05a3d-581">Troubleshoot</span></span>

<span data-ttu-id="05a3d-582">Di seguito sono trattati alcuni problemi comuni riscontrati durante il caricamento di file, con le soluzioni possibili corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="05a3d-582">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="05a3d-583">Errore non trovato quando viene distribuito in un server IIS</span><span class="sxs-lookup"><span data-stu-id="05a3d-583">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="05a3d-584">L'errore seguente indica che il file caricato supera la lunghezza del contenuto configurata del server:</span><span class="sxs-lookup"><span data-stu-id="05a3d-584">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="05a3d-585">Per ulteriori informazioni sull'aumento del limite, vedere la sezione [limite di lunghezza del contenuto IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-585">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="05a3d-586">Errore di connessione</span><span class="sxs-lookup"><span data-stu-id="05a3d-586">Connection failure</span></span>

<span data-ttu-id="05a3d-587">Un errore di connessione e una connessione al server di reimpostazione probabilmente indicano che il file caricato supera le dimensioni massime del corpo della richiesta del gheppio.</span><span class="sxs-lookup"><span data-stu-id="05a3d-587">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="05a3d-588">Per ulteriori informazioni, vedere la sezione relativa alle [dimensioni massime del corpo della richiesta di Gheppio](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="05a3d-588">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="05a3d-589">I limiti di connessione del client gheppio possono richiedere anche modifiche.</span><span class="sxs-lookup"><span data-stu-id="05a3d-589">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="05a3d-590">Eccezione per riferimento Null con IFormFile</span><span class="sxs-lookup"><span data-stu-id="05a3d-590">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="05a3d-591">Se il controller accetta file caricati usando <xref:Microsoft.AspNetCore.Http.IFormFile> ma il valore è `null`, verificare che il formato HTML specifichi un valore `enctype` di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-591">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="05a3d-592">Se questo attributo non è impostato sull'elemento `<form>`, il caricamento del file non viene eseguito e gli argomenti <xref:Microsoft.AspNetCore.Http.IFormFile> associati sono `null`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-592">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="05a3d-593">Verificare inoltre che la [denominazione di caricamento nei dati del modulo corrisponda alla denominazione dell'app](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="05a3d-593">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="05a3d-594">Il flusso è troppo lungo</span><span class="sxs-lookup"><span data-stu-id="05a3d-594">Stream was too long</span></span>

<span data-ttu-id="05a3d-595">Gli esempi in questo argomento si basano su <xref:System.IO.MemoryStream> per conservare il contenuto del file caricato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-595">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="05a3d-596">Il limite delle dimensioni di un `MemoryStream` è `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="05a3d-596">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="05a3d-597">Se lo scenario di caricamento dei file dell'app richiede un contenuto del file di dimensioni superiori a 50 MB, usare un approccio alternativo che non si basa su un singolo `MemoryStream` per contenere il contenuto di un file caricato.</span><span class="sxs-lookup"><span data-stu-id="05a3d-597">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="05a3d-598">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="05a3d-598">Additional resources</span></span>

* [<span data-ttu-id="05a3d-599">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="05a3d-599">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [<span data-ttu-id="05a3d-600">Sicurezza di Azure: frame di sicurezza: convalida dell'input | Attenuazioni</span><span class="sxs-lookup"><span data-stu-id="05a3d-600">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="05a3d-601">Modelli di progettazione cloud di Azure: modello di chiave del posteggiatore</span><span class="sxs-lookup"><span data-stu-id="05a3d-601">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
