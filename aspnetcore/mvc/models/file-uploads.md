---
title: Caricare file in ASP.NET Core
author: guardrex
description: Come usare l'associazione del modello e lo streaming per caricare i file in ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 04e7533aa190a4875d3f66e8665fec16abec48b3
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73462949"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="90423-103">Caricare file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90423-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="90423-104">Di [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)e [Rutger Storm](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="90423-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="90423-105">ASP.NET Core supporta il caricamento di uno o più file mediante l'associazione di modelli memorizzati nel buffer per i file più piccoli e il flusso non memorizzato nel buffer per file di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="90423-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="90423-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="90423-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="90423-107">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="90423-107">Security considerations</span></span>

<span data-ttu-id="90423-108">Prestare attenzione quando si fornisce agli utenti la possibilità di caricare file in un server.</span><span class="sxs-lookup"><span data-stu-id="90423-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="90423-109">Gli utenti malintenzionati possono tentare di:</span><span class="sxs-lookup"><span data-stu-id="90423-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="90423-110">Eseguire attacchi [di tipo Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="90423-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="90423-111">Caricare virus o malware.</span><span class="sxs-lookup"><span data-stu-id="90423-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="90423-112">Compromettere le reti e i server in altri modi.</span><span class="sxs-lookup"><span data-stu-id="90423-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="90423-113">I passaggi di sicurezza che riducono la probabilità di un attacco riuscito sono:</span><span class="sxs-lookup"><span data-stu-id="90423-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="90423-114">Caricare i file in un'area di caricamento file dedicata, preferibilmente in un'unità non di sistema.</span><span class="sxs-lookup"><span data-stu-id="90423-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="90423-115">Un percorso dedicato semplifica l'imposizione delle restrizioni di sicurezza sui file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="90423-116">Disabilitare le autorizzazioni di esecuzione per il percorso di caricamento del file.&dagger;</span><span class="sxs-lookup"><span data-stu-id="90423-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="90423-117">Non **rende permanente i** file caricati nella stessa struttura di directory dell'app.&dagger;</span><span class="sxs-lookup"><span data-stu-id="90423-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="90423-118">Usare un nome file sicuro determinato dall'app.</span><span class="sxs-lookup"><span data-stu-id="90423-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="90423-119">Non usare un nome file fornito dall'utente o il nome file non attendibile del file caricato.&dagger; HTML codifica il nome file non attendibile quando viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="90423-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="90423-120">Ad esempio, la registrazione del nome del file o la visualizzazione nell'interfaccia utente (Razor codifica automaticamente l'output).</span><span class="sxs-lookup"><span data-stu-id="90423-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="90423-121">Consenti solo le estensioni di file approvate per la specifica di progettazione dell'app.&dagger;</span><span class="sxs-lookup"><span data-stu-id="90423-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="90423-122">Verificare che i controlli lato client vengano eseguiti sul server.&dagger; controlli lato client sono facili da aggirare.</span><span class="sxs-lookup"><span data-stu-id="90423-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="90423-123">Controllare le dimensioni di un file caricato.</span><span class="sxs-lookup"><span data-stu-id="90423-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="90423-124">Impostare un limite di dimensioni massime per impedire caricamenti di grandi dimensioni.&dagger;</span><span class="sxs-lookup"><span data-stu-id="90423-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="90423-125">Quando i file non devono essere sovrascritti da un file caricato con lo stesso nome, controllare il nome del file sul database o sull'archiviazione fisica prima di caricare il file.</span><span class="sxs-lookup"><span data-stu-id="90423-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="90423-126">**Eseguire uno scanner virus/malware sul contenuto caricato prima che il file venga archiviato.**</span><span class="sxs-lookup"><span data-stu-id="90423-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="90423-127">&dagger;l'app di esempio illustra un approccio che soddisfa i criteri.</span><span class="sxs-lookup"><span data-stu-id="90423-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="90423-128">Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:</span><span class="sxs-lookup"><span data-stu-id="90423-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="90423-129">Ottenere il controllo completo di un sistema.</span><span class="sxs-lookup"><span data-stu-id="90423-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="90423-130">Sovraccaricare un sistema con il risultato che si verifica un arresto anomalo del sistema.</span><span class="sxs-lookup"><span data-stu-id="90423-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="90423-131">Compromettere i dati di sistemi o utenti.</span><span class="sxs-lookup"><span data-stu-id="90423-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="90423-132">Applicare i graffiti a un'interfaccia utente pubblica.</span><span class="sxs-lookup"><span data-stu-id="90423-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="90423-133">Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="90423-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="90423-134">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="90423-134">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="90423-135">Protezione di Azure: Verificare i controlli appropriati quando si accettano file dagli utenti</span><span class="sxs-lookup"><span data-stu-id="90423-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="90423-136">Per ulteriori informazioni sull'implementazione di misure di sicurezza, inclusi esempi dall'app di esempio, vedere la sezione [convalida](#validation) .</span><span class="sxs-lookup"><span data-stu-id="90423-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="90423-137">Scenari di archiviazione</span><span class="sxs-lookup"><span data-stu-id="90423-137">Storage scenarios</span></span>

<span data-ttu-id="90423-138">Le opzioni di archiviazione comuni per i file includono:</span><span class="sxs-lookup"><span data-stu-id="90423-138">Common storage options for files include:</span></span>

* <span data-ttu-id="90423-139">Database</span><span class="sxs-lookup"><span data-stu-id="90423-139">Database</span></span>

  * <span data-ttu-id="90423-140">Per i caricamenti di file di piccole dimensioni, un database è spesso più veloce rispetto alle opzioni di archiviazione fisica (file system o condivisione di rete).</span><span class="sxs-lookup"><span data-stu-id="90423-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="90423-141">Un database è spesso più pratico delle opzioni di archiviazione fisica perché il recupero di un record di database per i dati utente può fornire simultaneamente il contenuto del file, ad esempio un'immagine avatar.</span><span class="sxs-lookup"><span data-stu-id="90423-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="90423-142">Un database è potenzialmente meno costoso rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="90423-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="90423-143">Archiviazione fisica (file system o condivisione di rete)</span><span class="sxs-lookup"><span data-stu-id="90423-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="90423-144">Per i caricamenti di file di grandi dimensioni:</span><span class="sxs-lookup"><span data-stu-id="90423-144">For large file uploads:</span></span>
    * <span data-ttu-id="90423-145">I limiti del database possono limitare le dimensioni del caricamento.</span><span class="sxs-lookup"><span data-stu-id="90423-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="90423-146">L'archiviazione fisica è spesso meno economica rispetto all'archiviazione in un database.</span><span class="sxs-lookup"><span data-stu-id="90423-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="90423-147">L'archiviazione fisica è potenzialmente meno costosa rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="90423-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="90423-148">Il processo dell'app deve avere le autorizzazioni di lettura e scrittura per il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="90423-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="90423-149">**Non concedere mai l'autorizzazione Execute.**</span><span class="sxs-lookup"><span data-stu-id="90423-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="90423-150">Servizio di archiviazione dei dati (ad esempio, [archiviazione BLOB di Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="90423-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="90423-151">I servizi in genere offrono una maggiore scalabilità e resilienza rispetto a soluzioni locali che in genere sono soggette a singoli punti di errore.</span><span class="sxs-lookup"><span data-stu-id="90423-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="90423-152">I servizi sono un costo potenzialmente inferiore in scenari di infrastruttura di archiviazione di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="90423-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="90423-153">Per altre informazioni, vedere [Guida introduttiva: usare .NET per creare un BLOB nell'archivio oggetti](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="90423-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="90423-154">Nell'argomento viene illustrato <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, ma è possibile utilizzare <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> per salvare un <xref:System.IO.FileStream> nell'archiviazione BLOB quando si utilizza un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="90423-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="90423-155">Scenari di caricamento di file</span><span class="sxs-lookup"><span data-stu-id="90423-155">File upload scenarios</span></span>

<span data-ttu-id="90423-156">Due approcci generali per il caricamento di file sono il buffering e il flusso.</span><span class="sxs-lookup"><span data-stu-id="90423-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="90423-157">**Buffer**</span><span class="sxs-lookup"><span data-stu-id="90423-157">**Buffering**</span></span>

<span data-ttu-id="90423-158">L'intero file viene letto in un <xref:Microsoft.AspNetCore.Http.IFormFile>, ovvero una C# rappresentazione del file usato per elaborare o salvare il file.</span><span class="sxs-lookup"><span data-stu-id="90423-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="90423-159">Le risorse (disco, memoria) utilizzate dai caricamenti di file dipendono dal numero e dalle dimensioni dei caricamenti di file simultanei.</span><span class="sxs-lookup"><span data-stu-id="90423-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="90423-160">Se un'app tenta di memorizzare nel buffer un numero eccessivo di caricamenti, il sito si arresta in modo anomalo quando esaurisce la memoria o lo spazio su disco.</span><span class="sxs-lookup"><span data-stu-id="90423-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="90423-161">Se le dimensioni o la frequenza dei caricamenti di file esauriscono le risorse dell'app, usare il flusso.</span><span class="sxs-lookup"><span data-stu-id="90423-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="90423-162">Ogni singolo file memorizzato nel buffer che supera 64 KB viene spostato dalla memoria in un file temporaneo su disco.</span><span class="sxs-lookup"><span data-stu-id="90423-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="90423-163">Il buffering di file di piccole dimensioni viene trattato nelle sezioni seguenti di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="90423-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="90423-164">Archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="90423-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="90423-165">Database</span><span class="sxs-lookup"><span data-stu-id="90423-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="90423-166">**Flusso**</span><span class="sxs-lookup"><span data-stu-id="90423-166">**Streaming**</span></span>

<span data-ttu-id="90423-167">Il file viene ricevuto da una richiesta multipart ed elaborato o salvato direttamente dall'app.</span><span class="sxs-lookup"><span data-stu-id="90423-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="90423-168">Il flusso non migliora significativamente le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="90423-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="90423-169">Il flusso riduce le richieste di memoria o di spazio su disco durante il caricamento dei file.</span><span class="sxs-lookup"><span data-stu-id="90423-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="90423-170">Il flusso di file di grandi dimensioni è trattato nella sezione [caricare file di grandi dimensioni con flusso](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="90423-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="90423-171">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer all'archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="90423-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="90423-172">Per caricare file di piccole dimensioni, usare un modulo multipart o creare una richiesta POST usando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="90423-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="90423-173">L'esempio seguente illustra l'uso di un modulo di Razor Pages per caricare un singolo file (*pages/BufferedSingleFileUploadPhysical. cshtml* nell'app di esempio):</span><span class="sxs-lookup"><span data-stu-id="90423-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="90423-174">L'esempio seguente è analogo all'esempio precedente, ad eccezione del fatto che:</span><span class="sxs-lookup"><span data-stu-id="90423-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="90423-175">Per inviare i dati del modulo, viene usato JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)).</span><span class="sxs-lookup"><span data-stu-id="90423-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="90423-176">Nessuna convalida.</span><span class="sxs-lookup"><span data-stu-id="90423-176">There's no validation.</span></span>

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

<span data-ttu-id="90423-177">Per eseguire il POST del form in JavaScript per i client che [non supportano l'API fetch](https://caniuse.com/#feat=fetch), usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="90423-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="90423-178">Usare un riempimento di recupero (ad esempio, [Window. fetch Refill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="90423-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="90423-179">Usare `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="90423-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="90423-180">Esempio:</span><span class="sxs-lookup"><span data-stu-id="90423-180">For example:</span></span>

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

<span data-ttu-id="90423-181">Per supportare il caricamento di file, i moduli HTML devono specificare un tipo di codifica (`enctype`) di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="90423-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="90423-182">Affinché un `files` elemento input supporti il caricamento di più file, fornire l'attributo `multiple` sull'elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="90423-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="90423-183">È possibile accedere ai singoli file caricati nel server tramite l' [associazione di modelli](xref:mvc/models/model-binding) utilizzando <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="90423-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="90423-184">L'app di esempio illustra più caricamenti di file memorizzati nel buffer per scenari di archiviazione di database e fisici.</span><span class="sxs-lookup"><span data-stu-id="90423-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="90423-185">Non **utilizzare la** proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> diversa da per la visualizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="90423-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="90423-186">Durante la visualizzazione o la registrazione, HTML codifica il nome del file.</span><span class="sxs-lookup"><span data-stu-id="90423-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="90423-187">Un utente malintenzionato può fornire un nome file dannoso, inclusi percorsi completi o percorsi relativi.</span><span class="sxs-lookup"><span data-stu-id="90423-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="90423-188">Le applicazioni devono:</span><span class="sxs-lookup"><span data-stu-id="90423-188">Applications should:</span></span>
>
> * <span data-ttu-id="90423-189">Rimuovere il percorso dal nome file specificato dall'utente.</span><span class="sxs-lookup"><span data-stu-id="90423-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="90423-190">Salvare il nome file con codifica HTML, percorso-rimosso per l'interfaccia utente o la registrazione.</span><span class="sxs-lookup"><span data-stu-id="90423-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="90423-191">Genera un nuovo nome file casuale per l'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="90423-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="90423-192">Il codice seguente rimuove il percorso dal nome del file:</span><span class="sxs-lookup"><span data-stu-id="90423-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="90423-193">Gli esempi forniti finora non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="90423-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="90423-194">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="90423-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="90423-195">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="90423-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="90423-196">Convalida</span><span class="sxs-lookup"><span data-stu-id="90423-196">Validation</span></span>](#validation)

<span data-ttu-id="90423-197">Quando si caricano file usando l'associazione di modelli e <xref:Microsoft.AspNetCore.Http.IFormFile>, il metodo di azione può accettare:</span><span class="sxs-lookup"><span data-stu-id="90423-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="90423-198">Singolo <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="90423-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="90423-199">Qualsiasi raccolta seguente che rappresenta diversi file:</span><span class="sxs-lookup"><span data-stu-id="90423-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="90423-200">[Elenca](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="90423-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="90423-201">L'associazione corrisponde ai file di form in base al nome.</span><span class="sxs-lookup"><span data-stu-id="90423-201">Binding matches form files by name.</span></span> <span data-ttu-id="90423-202">Ad esempio, il valore HTML `name` in `<input type="file" name="formFile">` deve corrispondere al C# parametro/proprietà associato (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="90423-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="90423-203">Per ulteriori informazioni, vedere la sezione [corrispondenza del nome dell'attributo del nome del parametro del metodo post](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="90423-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="90423-204">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="90423-204">The following example:</span></span>

* <span data-ttu-id="90423-205">Esegue il ciclo di uno o più file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="90423-206">USA [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per restituire un percorso completo per un file, incluso il nome del file.</span><span class="sxs-lookup"><span data-stu-id="90423-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="90423-207">Salva i file nella file system locale usando un nome file generato dall'app.</span><span class="sxs-lookup"><span data-stu-id="90423-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="90423-208">Restituisce il numero totale e le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-208">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="90423-209">Utilizzare `Path.GetRandomFileName` per generare un nome file senza percorso.</span><span class="sxs-lookup"><span data-stu-id="90423-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="90423-210">Nell'esempio seguente il percorso viene ottenuto dalla configurazione:</span><span class="sxs-lookup"><span data-stu-id="90423-210">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="90423-211">Il percorso passato al <xref:System.IO.FileStream> *deve* includere il nome del file.</span><span class="sxs-lookup"><span data-stu-id="90423-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="90423-212">Se il nome file non viene specificato, viene generata un'<xref:System.UnauthorizedAccessException> in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="90423-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="90423-213">I file caricati con la tecnica <xref:Microsoft.AspNetCore.Http.IFormFile> vengono memorizzati nel buffer in memoria o su disco sul server prima dell'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="90423-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="90423-214">All'interno del metodo di azione, il contenuto del <xref:Microsoft.AspNetCore.Http.IFormFile> è accessibile come <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="90423-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="90423-215">Oltre al file system locale, i file possono essere salvati in una condivisione di rete o in un servizio di archiviazione file, ad esempio [archiviazione BLOB di Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="90423-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="90423-216">Per un altro esempio che esegue il ciclo su più file per il caricamento e usa nomi di file sicuri, vedere *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="90423-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="90423-217">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) genera un'<xref:System.IO.IOException> se vengono creati più di 65.535 file senza eliminare i file temporanei precedenti.</span><span class="sxs-lookup"><span data-stu-id="90423-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="90423-218">Il limite di 65.535 file è un limite per server.</span><span class="sxs-lookup"><span data-stu-id="90423-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="90423-219">Per ulteriori informazioni su questo limite per il sistema operativo Windows, vedere la sezione Osservazioni negli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="90423-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="90423-220">GetTempFileNameA (funzione)</span><span class="sxs-lookup"><span data-stu-id="90423-220">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="90423-221">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer in un database</span><span class="sxs-lookup"><span data-stu-id="90423-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="90423-222">Per archiviare i dati dei file binari in un database utilizzando [Entity Framework](/ef/core/index), definire una proprietà di <xref:System.Byte> array nell'entità:</span><span class="sxs-lookup"><span data-stu-id="90423-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="90423-223">Specificare una proprietà del modello di pagina per la classe che include un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="90423-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="90423-224"><xref:Microsoft.AspNetCore.Http.IFormFile> possibile utilizzare direttamente come parametro del metodo di azione o come proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="90423-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="90423-225">Nell'esempio precedente viene utilizzata una proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="90423-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="90423-226">Il `FileUpload` viene utilizzato nel formato Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="90423-226">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="90423-227">Quando il modulo viene inviato al server, copiare il <xref:Microsoft.AspNetCore.Http.IFormFile> in un flusso e salvarlo come una matrice di byte nel database.</span><span class="sxs-lookup"><span data-stu-id="90423-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="90423-228">Nell'esempio seguente `_dbContext` archivia il contesto di database dell'app:</span><span class="sxs-lookup"><span data-stu-id="90423-228">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="90423-229">L'esempio precedente è simile a uno scenario illustrato nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="90423-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="90423-230">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="90423-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="90423-231">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="90423-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="90423-232">Quando si archiviano dati binari all'interno di database relazionali, è necessario prestare attenzione, perché l'operazione può influire negativamente sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="90423-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="90423-233">Non basarsi o considerare attendibile la proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> senza convalida.</span><span class="sxs-lookup"><span data-stu-id="90423-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="90423-234">È consigliabile utilizzare la proprietà `FileName` solo a scopo di visualizzazione e solo dopo la codifica HTML.</span><span class="sxs-lookup"><span data-stu-id="90423-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="90423-235">Gli esempi forniti non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="90423-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="90423-236">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="90423-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="90423-237">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="90423-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="90423-238">Convalida</span><span class="sxs-lookup"><span data-stu-id="90423-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="90423-239">Caricare file di grandi dimensioni con flusso</span><span class="sxs-lookup"><span data-stu-id="90423-239">Upload large files with streaming</span></span>

<span data-ttu-id="90423-240">Nell'esempio seguente viene illustrato come utilizzare JavaScript per trasmettere un file a un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="90423-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="90423-241">Il token antifalsificazione del file viene generato usando un attributo di filtro personalizzato e passato alle intestazioni HTTP del client anziché nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="90423-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="90423-242">Poiché il metodo di azione elabora direttamente i dati caricati, l'associazione del modello di modulo viene disabilitata da un altro filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="90423-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="90423-243">All'interno dell'azione, il contenuto del form viene letto tramite un `MultipartReader`, che legge ogni singola `MultipartSection`, elaborando il file o archiviandone il contenuto, come appropriato.</span><span class="sxs-lookup"><span data-stu-id="90423-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="90423-244">Una volta lette le sezioni multiparte, l'azione esegue una propria associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="90423-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="90423-245">La risposta della pagina iniziale carica il modulo e salva un token antifalsificazione in un cookie (tramite l'attributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="90423-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="90423-246">L'attributo usa il [supporto antifalsificazione](xref:security/anti-request-forgery) predefinito di ASP.NET Core per impostare un cookie con un token di richiesta:</span><span class="sxs-lookup"><span data-stu-id="90423-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="90423-247">Il `DisableFormValueModelBindingAttribute` viene usato per disabilitare l'associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="90423-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="90423-248">Nell'app di esempio `GenerateAntiforgeryTokenCookieAttribute` e i `DisableFormValueModelBindingAttribute` vengono applicati come filtri ai modelli di applicazione della pagina di `/StreamedSingleFileUploadDb` e `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` usando le [convenzioni di Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="90423-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="90423-249">Poiché l'associazione di modelli non legge il modulo, i parametri associati al modulo non vengono associati (la query, la route e l'intestazione continuano a funzionare).</span><span class="sxs-lookup"><span data-stu-id="90423-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="90423-250">Il metodo di azione funziona direttamente con la proprietà `Request`.</span><span class="sxs-lookup"><span data-stu-id="90423-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="90423-251">Per leggere ogni sezione, viene usato un `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="90423-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="90423-252">I dati chiave/valore vengono archiviati in un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="90423-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="90423-253">Una volta lette le sezioni multipart, il contenuto del `KeyValueAccumulator` viene utilizzato per associare i dati del modulo a un tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="90423-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="90423-254">Metodo di `StreamingController.UploadDatabase` completo per lo streaming in un database con EF Core:</span><span class="sxs-lookup"><span data-stu-id="90423-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="90423-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="90423-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="90423-256">Il metodo di `StreamingController.UploadPhysical` completo per lo streaming in una posizione fisica:</span><span class="sxs-lookup"><span data-stu-id="90423-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="90423-257">Nell'app di esempio, i controlli di convalida vengono gestiti da `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="90423-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="90423-258">Convalida</span><span class="sxs-lookup"><span data-stu-id="90423-258">Validation</span></span>

<span data-ttu-id="90423-259">La classe di `FileHelpers` dell'app di esempio illustra diversi controlli per i caricamenti di file <xref:Microsoft.AspNetCore.Http.IFormFile> memorizzati nel buffer e con flusso.</span><span class="sxs-lookup"><span data-stu-id="90423-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="90423-260">Per l'elaborazione <xref:Microsoft.AspNetCore.Http.IFormFile> caricamenti di file memorizzati nel buffer nell'app di esempio, vedere il metodo `ProcessFormFile` nel file *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="90423-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="90423-261">Per l'elaborazione di file trasmessi, vedere il metodo `ProcessStreamedFile` nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="90423-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="90423-262">I metodi di elaborazione della convalida illustrati nell'app di esempio non analizzano il contenuto dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="90423-263">Nella maggior parte degli scenari di produzione, nel file viene usata un'API scanner di virus/malware prima di rendere disponibile il file per gli utenti o altri sistemi.</span><span class="sxs-lookup"><span data-stu-id="90423-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="90423-264">Sebbene l'esempio di argomento fornisca un esempio funzionante di tecniche di convalida, non implementare la classe `FileHelpers` in un'app di produzione a meno che non si:</span><span class="sxs-lookup"><span data-stu-id="90423-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="90423-265">Comprendere completamente l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="90423-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="90423-266">Modificare l'implementazione in modo appropriato per l'ambiente e le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="90423-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="90423-267">**Non implementare mai indiscriminatamente il codice di sicurezza in un'app senza soddisfare questi requisiti.**</span><span class="sxs-lookup"><span data-stu-id="90423-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="90423-268">Convalida del contenuto</span><span class="sxs-lookup"><span data-stu-id="90423-268">Content validation</span></span>

<span data-ttu-id="90423-269">**Usare un'API di analisi di virus/malware di terze parti nel contenuto caricato.**</span><span class="sxs-lookup"><span data-stu-id="90423-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="90423-270">L'analisi dei file richiede le risorse del server in scenari con volumi elevati.</span><span class="sxs-lookup"><span data-stu-id="90423-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="90423-271">Se le prestazioni di elaborazione delle richieste sono diminuite a causa dell'analisi dei file, provare a eseguire l'offload del lavoro di analisi in un [servizio in background](xref:fundamentals/host/hosted-services), possibilmente in un servizio in esecuzione in un server diverso dal server dell'app.</span><span class="sxs-lookup"><span data-stu-id="90423-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="90423-272">In genere, i file caricati vengono mantenuti in un'area in quarantena finché non vengono controllati dal programma antivirus in background.</span><span class="sxs-lookup"><span data-stu-id="90423-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="90423-273">Quando un file passa, il file viene spostato nel percorso di archiviazione file normale.</span><span class="sxs-lookup"><span data-stu-id="90423-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="90423-274">Questi passaggi vengono in genere eseguiti insieme a un record di database che indica lo stato di analisi di un file.</span><span class="sxs-lookup"><span data-stu-id="90423-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="90423-275">Usando un approccio di questo tipo, l'app e il server app rimangono incentrati sulla risposta alle richieste.</span><span class="sxs-lookup"><span data-stu-id="90423-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="90423-276">Convalida dell'estensione di file</span><span class="sxs-lookup"><span data-stu-id="90423-276">File extension validation</span></span>

<span data-ttu-id="90423-277">L'estensione del file caricato deve essere verificata rispetto a un elenco di estensioni consentite.</span><span class="sxs-lookup"><span data-stu-id="90423-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="90423-278">Esempio:</span><span class="sxs-lookup"><span data-stu-id="90423-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="90423-279">Convalida della firma del file</span><span class="sxs-lookup"><span data-stu-id="90423-279">File signature validation</span></span>

<span data-ttu-id="90423-280">La firma di un file è determinata dai primi byte all'inizio di un file.</span><span class="sxs-lookup"><span data-stu-id="90423-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="90423-281">Questi byte possono essere usati per indicare se l'estensione corrisponde al contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="90423-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="90423-282">L'app di esempio controlla le firme dei file per alcuni tipi di file comuni.</span><span class="sxs-lookup"><span data-stu-id="90423-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="90423-283">Nell'esempio seguente, la firma del file per un'immagine JPEG viene verificata rispetto al file:</span><span class="sxs-lookup"><span data-stu-id="90423-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="90423-284">Per ottenere ulteriori firme di file, vedere il [database delle firme file](https://www.filesignatures.net/) e le specifiche del file ufficiale.</span><span class="sxs-lookup"><span data-stu-id="90423-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="90423-285">Sicurezza del nome file</span><span class="sxs-lookup"><span data-stu-id="90423-285">File name security</span></span>

<span data-ttu-id="90423-286">Non usare mai un nome file fornito dal client per salvare un file nell'archiviazione fisica.</span><span class="sxs-lookup"><span data-stu-id="90423-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="90423-287">Creare un nome file sicuro per il file usando [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per creare un percorso completo (incluso il nome file) per l'archiviazione temporanea.</span><span class="sxs-lookup"><span data-stu-id="90423-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="90423-288">Razor HTML codifica automaticamente i valori delle proprietà per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="90423-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="90423-289">Il codice seguente è sicuro da usare:</span><span class="sxs-lookup"><span data-stu-id="90423-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="90423-290">All'esterno di Razor, <xref:System.Net.WebUtility.HtmlEncode*> sempre il contenuto del nome file dalla richiesta di un utente.</span><span class="sxs-lookup"><span data-stu-id="90423-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="90423-291">Molte implementazioni devono includere un controllo per verificare che il file esista. in caso contrario, il file viene sovrascritto da un file con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="90423-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="90423-292">Fornire logica aggiuntiva per soddisfare le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="90423-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="90423-293">Convalida dimensioni</span><span class="sxs-lookup"><span data-stu-id="90423-293">Size validation</span></span>

<span data-ttu-id="90423-294">Limitare le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="90423-295">Nell'app di esempio, le dimensioni del file sono limitate a 2 MB (indicate in byte).</span><span class="sxs-lookup"><span data-stu-id="90423-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="90423-296">Il limite viene fornito tramite la [configurazione](xref:fundamentals/configuration/index) dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="90423-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="90423-297">Il `FileSizeLimit` viene inserito nelle classi `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="90423-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="90423-298">Quando le dimensioni di un file superano il limite, il file viene rifiutato:</span><span class="sxs-lookup"><span data-stu-id="90423-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="90423-299">Corrisponde al valore dell'attributo Name al nome del parametro del metodo POST</span><span class="sxs-lookup"><span data-stu-id="90423-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="90423-300">Nei moduli non Razor che INVIAno dati del modulo o usano direttamente `FormData` di JavaScript, il nome specificato nell'elemento o `FormData` del form deve corrispondere al nome del parametro nell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="90423-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="90423-301">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="90423-301">In the following example:</span></span>

* <span data-ttu-id="90423-302">Quando si usa un elemento `<input>`, l'attributo `name` viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="90423-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="90423-303">Quando si usa `FormData` in JavaScript, il nome viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="90423-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="90423-304">Usare un nome corrispondente per il parametro del C# metodo (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="90423-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="90423-305">Per un metodo gestore di pagina Razor Pages denominato `Upload`:</span><span class="sxs-lookup"><span data-stu-id="90423-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="90423-306">Per un metodo di azione POST controller MVC:</span><span class="sxs-lookup"><span data-stu-id="90423-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="90423-307">Configurazione del server e dell'app</span><span class="sxs-lookup"><span data-stu-id="90423-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="90423-308">Limite di lunghezza del corpo multipart</span><span class="sxs-lookup"><span data-stu-id="90423-308">Multipart body length limit</span></span>

<span data-ttu-id="90423-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> imposta il limite per la lunghezza di ogni corpo multipart.</span><span class="sxs-lookup"><span data-stu-id="90423-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="90423-310">Le sezioni del modulo che superano questo limite generano una <xref:System.IO.InvalidDataException> durante l'analisi.</span><span class="sxs-lookup"><span data-stu-id="90423-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="90423-311">Il valore predefinito è 134.217.728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="90423-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="90423-312">Personalizzare il limite usando l'impostazione <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="90423-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="90423-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> viene utilizzato per impostare il <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="90423-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="90423-314">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="90423-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="90423-315">In un'app Razor Pages o in un'app MVC, applicare il filtro al modello di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="90423-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="90423-316">Dimensioni massime del corpo della richiesta di gheppio</span><span class="sxs-lookup"><span data-stu-id="90423-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="90423-317">Per le app ospitate da gheppio, le dimensioni massime predefinite del corpo della richiesta sono pari a 30 milioni byte, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="90423-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="90423-318">Personalizzare il limite usando l'opzione del server [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) gheppio:</span><span class="sxs-lookup"><span data-stu-id="90423-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="90423-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> viene usato per impostare [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="90423-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="90423-320">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="90423-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="90423-321">In un'app Razor Pages o in un'app MVC, applicare il filtro alla classe del gestore di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="90423-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="90423-322">Il `RequestSizeLimitAttribute` può essere applicato anche usando la direttiva Razor [@attribute](xref:mvc/views/razor#attribute) :</span><span class="sxs-lookup"><span data-stu-id="90423-322">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="90423-323">Altri limiti del gheppio</span><span class="sxs-lookup"><span data-stu-id="90423-323">Other Kestrel limits</span></span>

<span data-ttu-id="90423-324">Per le app ospitate da gheppio possono essere applicati altri limiti di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="90423-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="90423-325">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="90423-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="90423-326">Frequenza dei dati di richiesta e risposta</span><span class="sxs-lookup"><span data-stu-id="90423-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="90423-327">Limite lunghezza contenuto IIS</span><span class="sxs-lookup"><span data-stu-id="90423-327">IIS content length limit</span></span>

<span data-ttu-id="90423-328">Il limite di richieste predefinito (`maxAllowedContentLength`) è 30 milioni byte, che corrisponde a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="90423-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="90423-329">Personalizzare il limite nel file *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="90423-329">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="90423-330">Questa impostazione si applica solo a IIS.</span><span class="sxs-lookup"><span data-stu-id="90423-330">This setting only applies to IIS.</span></span> <span data-ttu-id="90423-331">Il comportamento non si verifica per impostazione predefinita con l'hosting in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="90423-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="90423-332">Per altre informazioni, vedere [limiti delle richieste \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="90423-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="90423-333">Le limitazioni del modulo ASP.NET Core o della presenza del modulo filtro richieste IIS possono limitare il caricamento a 2 o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="90423-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="90423-334">Per ulteriori informazioni, vedere [non è possibile caricare file di dimensioni superiori a 2 GB (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="90423-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="90423-335">Risolvere i problemi</span><span class="sxs-lookup"><span data-stu-id="90423-335">Troubleshoot</span></span>

<span data-ttu-id="90423-336">Di seguito sono trattati alcuni problemi comuni riscontrati durante il caricamento di file, con le soluzioni possibili corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="90423-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="90423-337">Errore non trovato quando viene distribuito in un server IIS</span><span class="sxs-lookup"><span data-stu-id="90423-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="90423-338">L'errore seguente indica che il file caricato supera la lunghezza del contenuto configurata del server:</span><span class="sxs-lookup"><span data-stu-id="90423-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="90423-339">Per ulteriori informazioni sull'aumento del limite, vedere la sezione [limite di lunghezza del contenuto IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="90423-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="90423-340">Errore di connessione</span><span class="sxs-lookup"><span data-stu-id="90423-340">Connection failure</span></span>

<span data-ttu-id="90423-341">Un errore di connessione e una connessione al server di reimpostazione probabilmente indicano che il file caricato supera le dimensioni massime del corpo della richiesta del gheppio.</span><span class="sxs-lookup"><span data-stu-id="90423-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="90423-342">Per ulteriori informazioni, vedere la sezione relativa alle [dimensioni massime del corpo della richiesta di Gheppio](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="90423-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="90423-343">I limiti di connessione del client gheppio possono richiedere anche modifiche.</span><span class="sxs-lookup"><span data-stu-id="90423-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="90423-344">Eccezione per riferimento Null con IFormFile</span><span class="sxs-lookup"><span data-stu-id="90423-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="90423-345">Se il controller accetta file caricati usando <xref:Microsoft.AspNetCore.Http.IFormFile> ma il valore è `null`, verificare che il formato HTML specifichi un valore `enctype` di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="90423-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="90423-346">Se questo attributo non è impostato sull'elemento `<form>`, il caricamento del file non viene eseguito e gli argomenti <xref:Microsoft.AspNetCore.Http.IFormFile> associati sono `null`.</span><span class="sxs-lookup"><span data-stu-id="90423-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="90423-347">Verificare inoltre che la [denominazione di caricamento nei dati del modulo corrisponda alla denominazione dell'app](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="90423-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="90423-348">ASP.NET Core supporta il caricamento di uno o più file mediante l'associazione di modelli memorizzati nel buffer per i file più piccoli e il flusso non memorizzato nel buffer per file di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="90423-348">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="90423-349">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="90423-349">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="90423-350">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="90423-350">Security considerations</span></span>

<span data-ttu-id="90423-351">Prestare attenzione quando si fornisce agli utenti la possibilità di caricare file in un server.</span><span class="sxs-lookup"><span data-stu-id="90423-351">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="90423-352">Gli utenti malintenzionati possono tentare di:</span><span class="sxs-lookup"><span data-stu-id="90423-352">Attackers may attempt to:</span></span>

* <span data-ttu-id="90423-353">Eseguire attacchi [di tipo Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="90423-353">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="90423-354">Caricare virus o malware.</span><span class="sxs-lookup"><span data-stu-id="90423-354">Upload viruses or malware.</span></span>
* <span data-ttu-id="90423-355">Compromettere le reti e i server in altri modi.</span><span class="sxs-lookup"><span data-stu-id="90423-355">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="90423-356">I passaggi di sicurezza che riducono la probabilità di un attacco riuscito sono:</span><span class="sxs-lookup"><span data-stu-id="90423-356">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="90423-357">Caricare i file in un'area di caricamento file dedicata, preferibilmente in un'unità non di sistema.</span><span class="sxs-lookup"><span data-stu-id="90423-357">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="90423-358">Un percorso dedicato semplifica l'imposizione delle restrizioni di sicurezza sui file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-358">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="90423-359">Disabilitare le autorizzazioni di esecuzione per il percorso di caricamento del file.&dagger;</span><span class="sxs-lookup"><span data-stu-id="90423-359">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="90423-360">Non **rende permanente i** file caricati nella stessa struttura di directory dell'app.&dagger;</span><span class="sxs-lookup"><span data-stu-id="90423-360">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="90423-361">Usare un nome file sicuro determinato dall'app.</span><span class="sxs-lookup"><span data-stu-id="90423-361">Use a safe file name determined by the app.</span></span> <span data-ttu-id="90423-362">Non usare un nome file fornito dall'utente o il nome file non attendibile del file caricato.&dagger; HTML codifica il nome file non attendibile quando viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="90423-362">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="90423-363">Ad esempio, la registrazione del nome del file o la visualizzazione nell'interfaccia utente (Razor codifica automaticamente l'output).</span><span class="sxs-lookup"><span data-stu-id="90423-363">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="90423-364">Consenti solo le estensioni di file approvate per la specifica di progettazione dell'app.&dagger;</span><span class="sxs-lookup"><span data-stu-id="90423-364">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="90423-365">Verificare che i controlli lato client vengano eseguiti sul server.&dagger; controlli lato client sono facili da aggirare.</span><span class="sxs-lookup"><span data-stu-id="90423-365">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="90423-366">Controllare le dimensioni di un file caricato.</span><span class="sxs-lookup"><span data-stu-id="90423-366">Check the size of an uploaded file.</span></span> <span data-ttu-id="90423-367">Impostare un limite di dimensioni massime per impedire caricamenti di grandi dimensioni.&dagger;</span><span class="sxs-lookup"><span data-stu-id="90423-367">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="90423-368">Quando i file non devono essere sovrascritti da un file caricato con lo stesso nome, controllare il nome del file sul database o sull'archiviazione fisica prima di caricare il file.</span><span class="sxs-lookup"><span data-stu-id="90423-368">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="90423-369">**Eseguire uno scanner virus/malware sul contenuto caricato prima che il file venga archiviato.**</span><span class="sxs-lookup"><span data-stu-id="90423-369">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="90423-370">&dagger;l'app di esempio illustra un approccio che soddisfa i criteri.</span><span class="sxs-lookup"><span data-stu-id="90423-370">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="90423-371">Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:</span><span class="sxs-lookup"><span data-stu-id="90423-371">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="90423-372">Ottenere il controllo completo di un sistema.</span><span class="sxs-lookup"><span data-stu-id="90423-372">Completely gain control of a system.</span></span>
> * <span data-ttu-id="90423-373">Sovraccaricare un sistema con il risultato che si verifica un arresto anomalo del sistema.</span><span class="sxs-lookup"><span data-stu-id="90423-373">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="90423-374">Compromettere i dati di sistemi o utenti.</span><span class="sxs-lookup"><span data-stu-id="90423-374">Compromise user or system data.</span></span>
> * <span data-ttu-id="90423-375">Applicare i graffiti a un'interfaccia utente pubblica.</span><span class="sxs-lookup"><span data-stu-id="90423-375">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="90423-376">Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="90423-376">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="90423-377">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="90423-377">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="90423-378">Protezione di Azure: Verificare i controlli appropriati quando si accettano file dagli utenti</span><span class="sxs-lookup"><span data-stu-id="90423-378">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="90423-379">Per ulteriori informazioni sull'implementazione di misure di sicurezza, inclusi esempi dall'app di esempio, vedere la sezione [convalida](#validation) .</span><span class="sxs-lookup"><span data-stu-id="90423-379">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="90423-380">Scenari di archiviazione</span><span class="sxs-lookup"><span data-stu-id="90423-380">Storage scenarios</span></span>

<span data-ttu-id="90423-381">Le opzioni di archiviazione comuni per i file includono:</span><span class="sxs-lookup"><span data-stu-id="90423-381">Common storage options for files include:</span></span>

* <span data-ttu-id="90423-382">Database</span><span class="sxs-lookup"><span data-stu-id="90423-382">Database</span></span>

  * <span data-ttu-id="90423-383">Per i caricamenti di file di piccole dimensioni, un database è spesso più veloce rispetto alle opzioni di archiviazione fisica (file system o condivisione di rete).</span><span class="sxs-lookup"><span data-stu-id="90423-383">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="90423-384">Un database è spesso più pratico delle opzioni di archiviazione fisica perché il recupero di un record di database per i dati utente può fornire simultaneamente il contenuto del file, ad esempio un'immagine avatar.</span><span class="sxs-lookup"><span data-stu-id="90423-384">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="90423-385">Un database è potenzialmente meno costoso rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="90423-385">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="90423-386">Archiviazione fisica (file system o condivisione di rete)</span><span class="sxs-lookup"><span data-stu-id="90423-386">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="90423-387">Per i caricamenti di file di grandi dimensioni:</span><span class="sxs-lookup"><span data-stu-id="90423-387">For large file uploads:</span></span>
    * <span data-ttu-id="90423-388">I limiti del database possono limitare le dimensioni del caricamento.</span><span class="sxs-lookup"><span data-stu-id="90423-388">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="90423-389">L'archiviazione fisica è spesso meno economica rispetto all'archiviazione in un database.</span><span class="sxs-lookup"><span data-stu-id="90423-389">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="90423-390">L'archiviazione fisica è potenzialmente meno costosa rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="90423-390">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="90423-391">Il processo dell'app deve avere le autorizzazioni di lettura e scrittura per il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="90423-391">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="90423-392">**Non concedere mai l'autorizzazione Execute.**</span><span class="sxs-lookup"><span data-stu-id="90423-392">**Never grant execute permission.**</span></span>

* <span data-ttu-id="90423-393">Servizio di archiviazione dei dati (ad esempio, [archiviazione BLOB di Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="90423-393">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="90423-394">I servizi in genere offrono una maggiore scalabilità e resilienza rispetto a soluzioni locali che in genere sono soggette a singoli punti di errore.</span><span class="sxs-lookup"><span data-stu-id="90423-394">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="90423-395">I servizi sono un costo potenzialmente inferiore in scenari di infrastruttura di archiviazione di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="90423-395">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="90423-396">Per altre informazioni, vedere [Guida introduttiva: usare .NET per creare un BLOB nell'archivio oggetti](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="90423-396">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="90423-397">Nell'argomento viene illustrato <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, ma è possibile utilizzare <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> per salvare un <xref:System.IO.FileStream> nell'archiviazione BLOB quando si utilizza un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="90423-397">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="90423-398">Scenari di caricamento di file</span><span class="sxs-lookup"><span data-stu-id="90423-398">File upload scenarios</span></span>

<span data-ttu-id="90423-399">Due approcci generali per il caricamento di file sono il buffering e il flusso.</span><span class="sxs-lookup"><span data-stu-id="90423-399">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="90423-400">**Buffer**</span><span class="sxs-lookup"><span data-stu-id="90423-400">**Buffering**</span></span>

<span data-ttu-id="90423-401">L'intero file viene letto in un <xref:Microsoft.AspNetCore.Http.IFormFile>, ovvero una C# rappresentazione del file usato per elaborare o salvare il file.</span><span class="sxs-lookup"><span data-stu-id="90423-401">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="90423-402">Le risorse (disco, memoria) utilizzate dai caricamenti di file dipendono dal numero e dalle dimensioni dei caricamenti di file simultanei.</span><span class="sxs-lookup"><span data-stu-id="90423-402">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="90423-403">Se un'app tenta di memorizzare nel buffer un numero eccessivo di caricamenti, il sito si arresta in modo anomalo quando esaurisce la memoria o lo spazio su disco.</span><span class="sxs-lookup"><span data-stu-id="90423-403">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="90423-404">Se le dimensioni o la frequenza dei caricamenti di file esauriscono le risorse dell'app, usare il flusso.</span><span class="sxs-lookup"><span data-stu-id="90423-404">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="90423-405">Ogni singolo file memorizzato nel buffer che supera 64 KB viene spostato dalla memoria in un file temporaneo su disco.</span><span class="sxs-lookup"><span data-stu-id="90423-405">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="90423-406">Il buffering di file di piccole dimensioni viene trattato nelle sezioni seguenti di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="90423-406">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="90423-407">Archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="90423-407">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="90423-408">Database</span><span class="sxs-lookup"><span data-stu-id="90423-408">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="90423-409">**Flusso**</span><span class="sxs-lookup"><span data-stu-id="90423-409">**Streaming**</span></span>

<span data-ttu-id="90423-410">Il file viene ricevuto da una richiesta multipart ed elaborato o salvato direttamente dall'app.</span><span class="sxs-lookup"><span data-stu-id="90423-410">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="90423-411">Il flusso non migliora significativamente le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="90423-411">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="90423-412">Il flusso riduce le richieste di memoria o di spazio su disco durante il caricamento dei file.</span><span class="sxs-lookup"><span data-stu-id="90423-412">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="90423-413">Il flusso di file di grandi dimensioni è trattato nella sezione [caricare file di grandi dimensioni con flusso](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="90423-413">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="90423-414">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer all'archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="90423-414">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="90423-415">Per caricare file di piccole dimensioni, usare un modulo multipart o creare una richiesta POST usando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="90423-415">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="90423-416">L'esempio seguente illustra l'uso di un modulo di Razor Pages per caricare un singolo file (*pages/BufferedSingleFileUploadPhysical. cshtml* nell'app di esempio):</span><span class="sxs-lookup"><span data-stu-id="90423-416">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="90423-417">L'esempio seguente è analogo all'esempio precedente, ad eccezione del fatto che:</span><span class="sxs-lookup"><span data-stu-id="90423-417">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="90423-418">Per inviare i dati del modulo, viene usato JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)).</span><span class="sxs-lookup"><span data-stu-id="90423-418">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="90423-419">Nessuna convalida.</span><span class="sxs-lookup"><span data-stu-id="90423-419">There's no validation.</span></span>

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

<span data-ttu-id="90423-420">Per eseguire il POST del form in JavaScript per i client che [non supportano l'API fetch](https://caniuse.com/#feat=fetch), usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="90423-420">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="90423-421">Usare un riempimento di recupero (ad esempio, [Window. fetch Refill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="90423-421">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="90423-422">Usare `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="90423-422">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="90423-423">Esempio:</span><span class="sxs-lookup"><span data-stu-id="90423-423">For example:</span></span>

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

<span data-ttu-id="90423-424">Per supportare il caricamento di file, i moduli HTML devono specificare un tipo di codifica (`enctype`) di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="90423-424">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="90423-425">Affinché un `files` elemento input supporti il caricamento di più file, fornire l'attributo `multiple` sull'elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="90423-425">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="90423-426">È possibile accedere ai singoli file caricati nel server tramite l' [associazione di modelli](xref:mvc/models/model-binding) utilizzando <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="90423-426">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="90423-427">L'app di esempio illustra più caricamenti di file memorizzati nel buffer per scenari di archiviazione di database e fisici.</span><span class="sxs-lookup"><span data-stu-id="90423-427">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="90423-428">Non **utilizzare la** proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> diversa da per la visualizzazione e la registrazione.</span><span class="sxs-lookup"><span data-stu-id="90423-428">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="90423-429">Durante la visualizzazione o la registrazione, HTML codifica il nome del file.</span><span class="sxs-lookup"><span data-stu-id="90423-429">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="90423-430">Un utente malintenzionato può fornire un nome file dannoso, inclusi percorsi completi o percorsi relativi.</span><span class="sxs-lookup"><span data-stu-id="90423-430">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="90423-431">Le applicazioni devono:</span><span class="sxs-lookup"><span data-stu-id="90423-431">Applications should:</span></span>
>
> * <span data-ttu-id="90423-432">Rimuovere il percorso dal nome file specificato dall'utente.</span><span class="sxs-lookup"><span data-stu-id="90423-432">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="90423-433">Salvare il nome file con codifica HTML, percorso-rimosso per l'interfaccia utente o la registrazione.</span><span class="sxs-lookup"><span data-stu-id="90423-433">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="90423-434">Genera un nuovo nome file casuale per l'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="90423-434">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="90423-435">Il codice seguente rimuove il percorso dal nome del file:</span><span class="sxs-lookup"><span data-stu-id="90423-435">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="90423-436">Gli esempi forniti finora non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="90423-436">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="90423-437">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="90423-437">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="90423-438">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="90423-438">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="90423-439">Convalida</span><span class="sxs-lookup"><span data-stu-id="90423-439">Validation</span></span>](#validation)

<span data-ttu-id="90423-440">Quando si caricano file usando l'associazione di modelli e <xref:Microsoft.AspNetCore.Http.IFormFile>, il metodo di azione può accettare:</span><span class="sxs-lookup"><span data-stu-id="90423-440">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="90423-441">Singolo <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="90423-441">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="90423-442">Qualsiasi raccolta seguente che rappresenta diversi file:</span><span class="sxs-lookup"><span data-stu-id="90423-442">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="90423-443">[Elenca](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="90423-443">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="90423-444">L'associazione corrisponde ai file di form in base al nome.</span><span class="sxs-lookup"><span data-stu-id="90423-444">Binding matches form files by name.</span></span> <span data-ttu-id="90423-445">Ad esempio, il valore HTML `name` in `<input type="file" name="formFile">` deve corrispondere al C# parametro/proprietà associato (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="90423-445">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="90423-446">Per ulteriori informazioni, vedere la sezione [corrispondenza del nome dell'attributo del nome del parametro del metodo post](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="90423-446">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="90423-447">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="90423-447">The following example:</span></span>

* <span data-ttu-id="90423-448">Esegue il ciclo di uno o più file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-448">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="90423-449">USA [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per restituire un percorso completo per un file, incluso il nome del file.</span><span class="sxs-lookup"><span data-stu-id="90423-449">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="90423-450">Salva i file nella file system locale usando un nome file generato dall'app.</span><span class="sxs-lookup"><span data-stu-id="90423-450">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="90423-451">Restituisce il numero totale e le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-451">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="90423-452">Utilizzare `Path.GetRandomFileName` per generare un nome file senza percorso.</span><span class="sxs-lookup"><span data-stu-id="90423-452">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="90423-453">Nell'esempio seguente il percorso viene ottenuto dalla configurazione:</span><span class="sxs-lookup"><span data-stu-id="90423-453">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="90423-454">Il percorso passato al <xref:System.IO.FileStream> *deve* includere il nome del file.</span><span class="sxs-lookup"><span data-stu-id="90423-454">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="90423-455">Se il nome file non viene specificato, viene generata un'<xref:System.UnauthorizedAccessException> in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="90423-455">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="90423-456">I file caricati con la tecnica <xref:Microsoft.AspNetCore.Http.IFormFile> vengono memorizzati nel buffer in memoria o su disco sul server prima dell'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="90423-456">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="90423-457">All'interno del metodo di azione, il contenuto del <xref:Microsoft.AspNetCore.Http.IFormFile> è accessibile come <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="90423-457">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="90423-458">Oltre al file system locale, i file possono essere salvati in una condivisione di rete o in un servizio di archiviazione file, ad esempio [archiviazione BLOB di Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="90423-458">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="90423-459">Per un altro esempio che esegue il ciclo su più file per il caricamento e usa nomi di file sicuri, vedere *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="90423-459">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="90423-460">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) genera un'<xref:System.IO.IOException> se vengono creati più di 65.535 file senza eliminare i file temporanei precedenti.</span><span class="sxs-lookup"><span data-stu-id="90423-460">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="90423-461">Il limite di 65.535 file è un limite per server.</span><span class="sxs-lookup"><span data-stu-id="90423-461">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="90423-462">Per ulteriori informazioni su questo limite per il sistema operativo Windows, vedere la sezione Osservazioni negli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="90423-462">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="90423-463">GetTempFileNameA (funzione)</span><span class="sxs-lookup"><span data-stu-id="90423-463">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="90423-464">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer in un database</span><span class="sxs-lookup"><span data-stu-id="90423-464">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="90423-465">Per archiviare i dati dei file binari in un database utilizzando [Entity Framework](/ef/core/index), definire una proprietà di <xref:System.Byte> array nell'entità:</span><span class="sxs-lookup"><span data-stu-id="90423-465">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="90423-466">Specificare una proprietà del modello di pagina per la classe che include un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="90423-466">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="90423-467"><xref:Microsoft.AspNetCore.Http.IFormFile> possibile utilizzare direttamente come parametro del metodo di azione o come proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="90423-467"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="90423-468">Nell'esempio precedente viene utilizzata una proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="90423-468">The prior example uses a bound model property.</span></span>

<span data-ttu-id="90423-469">Il `FileUpload` viene utilizzato nel formato Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="90423-469">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="90423-470">Quando il modulo viene inviato al server, copiare il <xref:Microsoft.AspNetCore.Http.IFormFile> in un flusso e salvarlo come una matrice di byte nel database.</span><span class="sxs-lookup"><span data-stu-id="90423-470">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="90423-471">Nell'esempio seguente `_dbContext` archivia il contesto di database dell'app:</span><span class="sxs-lookup"><span data-stu-id="90423-471">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="90423-472">L'esempio precedente è simile a uno scenario illustrato nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="90423-472">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="90423-473">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="90423-473">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="90423-474">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="90423-474">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="90423-475">Quando si archiviano dati binari all'interno di database relazionali, è necessario prestare attenzione, perché l'operazione può influire negativamente sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="90423-475">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="90423-476">Non basarsi o considerare attendibile la proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> senza convalida.</span><span class="sxs-lookup"><span data-stu-id="90423-476">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="90423-477">È consigliabile utilizzare la proprietà `FileName` solo a scopo di visualizzazione e solo dopo la codifica HTML.</span><span class="sxs-lookup"><span data-stu-id="90423-477">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="90423-478">Gli esempi forniti non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="90423-478">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="90423-479">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="90423-479">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="90423-480">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="90423-480">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="90423-481">Convalida</span><span class="sxs-lookup"><span data-stu-id="90423-481">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="90423-482">Caricare file di grandi dimensioni con flusso</span><span class="sxs-lookup"><span data-stu-id="90423-482">Upload large files with streaming</span></span>

<span data-ttu-id="90423-483">Nell'esempio seguente viene illustrato come utilizzare JavaScript per trasmettere un file a un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="90423-483">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="90423-484">Il token antifalsificazione del file viene generato usando un attributo di filtro personalizzato e passato alle intestazioni HTTP del client anziché nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="90423-484">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="90423-485">Poiché il metodo di azione elabora direttamente i dati caricati, l'associazione del modello di modulo viene disabilitata da un altro filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="90423-485">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="90423-486">All'interno dell'azione, il contenuto del form viene letto tramite un `MultipartReader`, che legge ogni singola `MultipartSection`, elaborando il file o archiviandone il contenuto, come appropriato.</span><span class="sxs-lookup"><span data-stu-id="90423-486">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="90423-487">Una volta lette le sezioni multiparte, l'azione esegue una propria associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="90423-487">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="90423-488">La risposta della pagina iniziale carica il modulo e salva un token antifalsificazione in un cookie (tramite l'attributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="90423-488">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="90423-489">L'attributo usa il [supporto antifalsificazione](xref:security/anti-request-forgery) predefinito di ASP.NET Core per impostare un cookie con un token di richiesta:</span><span class="sxs-lookup"><span data-stu-id="90423-489">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="90423-490">Il `DisableFormValueModelBindingAttribute` viene usato per disabilitare l'associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="90423-490">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="90423-491">Nell'app di esempio `GenerateAntiforgeryTokenCookieAttribute` e i `DisableFormValueModelBindingAttribute` vengono applicati come filtri ai modelli di applicazione della pagina di `/StreamedSingleFileUploadDb` e `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` usando le [convenzioni di Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="90423-491">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="90423-492">Poiché l'associazione di modelli non legge il modulo, i parametri associati al modulo non vengono associati (la query, la route e l'intestazione continuano a funzionare).</span><span class="sxs-lookup"><span data-stu-id="90423-492">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="90423-493">Il metodo di azione funziona direttamente con la proprietà `Request`.</span><span class="sxs-lookup"><span data-stu-id="90423-493">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="90423-494">Per leggere ogni sezione, viene usato un `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="90423-494">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="90423-495">I dati chiave/valore vengono archiviati in un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="90423-495">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="90423-496">Una volta lette le sezioni multipart, il contenuto del `KeyValueAccumulator` viene utilizzato per associare i dati del modulo a un tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="90423-496">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="90423-497">Metodo di `StreamingController.UploadDatabase` completo per lo streaming in un database con EF Core:</span><span class="sxs-lookup"><span data-stu-id="90423-497">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="90423-498">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="90423-498">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="90423-499">Il metodo di `StreamingController.UploadPhysical` completo per lo streaming in una posizione fisica:</span><span class="sxs-lookup"><span data-stu-id="90423-499">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="90423-500">Nell'app di esempio, i controlli di convalida vengono gestiti da `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="90423-500">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="90423-501">Convalida</span><span class="sxs-lookup"><span data-stu-id="90423-501">Validation</span></span>

<span data-ttu-id="90423-502">La classe di `FileHelpers` dell'app di esempio illustra diversi controlli per i caricamenti di file <xref:Microsoft.AspNetCore.Http.IFormFile> memorizzati nel buffer e con flusso.</span><span class="sxs-lookup"><span data-stu-id="90423-502">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="90423-503">Per l'elaborazione <xref:Microsoft.AspNetCore.Http.IFormFile> caricamenti di file memorizzati nel buffer nell'app di esempio, vedere il metodo `ProcessFormFile` nel file *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="90423-503">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="90423-504">Per l'elaborazione di file trasmessi, vedere il metodo `ProcessStreamedFile` nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="90423-504">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="90423-505">I metodi di elaborazione della convalida illustrati nell'app di esempio non analizzano il contenuto dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-505">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="90423-506">Nella maggior parte degli scenari di produzione, nel file viene usata un'API scanner di virus/malware prima di rendere disponibile il file per gli utenti o altri sistemi.</span><span class="sxs-lookup"><span data-stu-id="90423-506">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="90423-507">Sebbene l'esempio di argomento fornisca un esempio funzionante di tecniche di convalida, non implementare la classe `FileHelpers` in un'app di produzione a meno che non si:</span><span class="sxs-lookup"><span data-stu-id="90423-507">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="90423-508">Comprendere completamente l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="90423-508">Fully understand the implementation.</span></span>
> * <span data-ttu-id="90423-509">Modificare l'implementazione in modo appropriato per l'ambiente e le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="90423-509">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="90423-510">**Non implementare mai indiscriminatamente il codice di sicurezza in un'app senza soddisfare questi requisiti.**</span><span class="sxs-lookup"><span data-stu-id="90423-510">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="90423-511">Convalida del contenuto</span><span class="sxs-lookup"><span data-stu-id="90423-511">Content validation</span></span>

<span data-ttu-id="90423-512">**Usare un'API di analisi di virus/malware di terze parti nel contenuto caricato.**</span><span class="sxs-lookup"><span data-stu-id="90423-512">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="90423-513">L'analisi dei file richiede le risorse del server in scenari con volumi elevati.</span><span class="sxs-lookup"><span data-stu-id="90423-513">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="90423-514">Se le prestazioni di elaborazione delle richieste sono diminuite a causa dell'analisi dei file, provare a eseguire l'offload del lavoro di analisi in un [servizio in background](xref:fundamentals/host/hosted-services), possibilmente in un servizio in esecuzione in un server diverso dal server dell'app.</span><span class="sxs-lookup"><span data-stu-id="90423-514">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="90423-515">In genere, i file caricati vengono mantenuti in un'area in quarantena finché non vengono controllati dal programma antivirus in background.</span><span class="sxs-lookup"><span data-stu-id="90423-515">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="90423-516">Quando un file passa, il file viene spostato nel percorso di archiviazione file normale.</span><span class="sxs-lookup"><span data-stu-id="90423-516">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="90423-517">Questi passaggi vengono in genere eseguiti insieme a un record di database che indica lo stato di analisi di un file.</span><span class="sxs-lookup"><span data-stu-id="90423-517">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="90423-518">Usando un approccio di questo tipo, l'app e il server app rimangono incentrati sulla risposta alle richieste.</span><span class="sxs-lookup"><span data-stu-id="90423-518">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="90423-519">Convalida dell'estensione di file</span><span class="sxs-lookup"><span data-stu-id="90423-519">File extension validation</span></span>

<span data-ttu-id="90423-520">L'estensione del file caricato deve essere verificata rispetto a un elenco di estensioni consentite.</span><span class="sxs-lookup"><span data-stu-id="90423-520">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="90423-521">Esempio:</span><span class="sxs-lookup"><span data-stu-id="90423-521">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="90423-522">Convalida della firma del file</span><span class="sxs-lookup"><span data-stu-id="90423-522">File signature validation</span></span>

<span data-ttu-id="90423-523">La firma di un file è determinata dai primi byte all'inizio di un file.</span><span class="sxs-lookup"><span data-stu-id="90423-523">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="90423-524">Questi byte possono essere usati per indicare se l'estensione corrisponde al contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="90423-524">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="90423-525">L'app di esempio controlla le firme dei file per alcuni tipi di file comuni.</span><span class="sxs-lookup"><span data-stu-id="90423-525">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="90423-526">Nell'esempio seguente, la firma del file per un'immagine JPEG viene verificata rispetto al file:</span><span class="sxs-lookup"><span data-stu-id="90423-526">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="90423-527">Per ottenere ulteriori firme di file, vedere il [database delle firme file](https://www.filesignatures.net/) e le specifiche del file ufficiale.</span><span class="sxs-lookup"><span data-stu-id="90423-527">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="90423-528">Sicurezza del nome file</span><span class="sxs-lookup"><span data-stu-id="90423-528">File name security</span></span>

<span data-ttu-id="90423-529">Non usare mai un nome file fornito dal client per salvare un file nell'archiviazione fisica.</span><span class="sxs-lookup"><span data-stu-id="90423-529">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="90423-530">Creare un nome file sicuro per il file usando [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per creare un percorso completo (incluso il nome file) per l'archiviazione temporanea.</span><span class="sxs-lookup"><span data-stu-id="90423-530">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="90423-531">Razor HTML codifica automaticamente i valori delle proprietà per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="90423-531">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="90423-532">Il codice seguente è sicuro da usare:</span><span class="sxs-lookup"><span data-stu-id="90423-532">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="90423-533">All'esterno di Razor, <xref:System.Net.WebUtility.HtmlEncode*> sempre il contenuto del nome file dalla richiesta di un utente.</span><span class="sxs-lookup"><span data-stu-id="90423-533">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="90423-534">Molte implementazioni devono includere un controllo per verificare che il file esista. in caso contrario, il file viene sovrascritto da un file con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="90423-534">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="90423-535">Fornire logica aggiuntiva per soddisfare le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="90423-535">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="90423-536">Convalida dimensioni</span><span class="sxs-lookup"><span data-stu-id="90423-536">Size validation</span></span>

<span data-ttu-id="90423-537">Limitare le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="90423-537">Limit the size of uploaded files.</span></span>

<span data-ttu-id="90423-538">Nell'app di esempio, le dimensioni del file sono limitate a 2 MB (indicate in byte).</span><span class="sxs-lookup"><span data-stu-id="90423-538">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="90423-539">Il limite viene fornito tramite la [configurazione](xref:fundamentals/configuration/index) dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="90423-539">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="90423-540">Il `FileSizeLimit` viene inserito nelle classi `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="90423-540">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="90423-541">Quando le dimensioni di un file superano il limite, il file viene rifiutato:</span><span class="sxs-lookup"><span data-stu-id="90423-541">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="90423-542">Corrisponde al valore dell'attributo Name al nome del parametro del metodo POST</span><span class="sxs-lookup"><span data-stu-id="90423-542">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="90423-543">Nei moduli non Razor che INVIAno dati del modulo o usano direttamente `FormData` di JavaScript, il nome specificato nell'elemento o `FormData` del form deve corrispondere al nome del parametro nell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="90423-543">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="90423-544">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="90423-544">In the following example:</span></span>

* <span data-ttu-id="90423-545">Quando si usa un elemento `<input>`, l'attributo `name` viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="90423-545">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="90423-546">Quando si usa `FormData` in JavaScript, il nome viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="90423-546">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="90423-547">Usare un nome corrispondente per il parametro del C# metodo (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="90423-547">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="90423-548">Per un metodo gestore di pagina Razor Pages denominato `Upload`:</span><span class="sxs-lookup"><span data-stu-id="90423-548">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="90423-549">Per un metodo di azione POST controller MVC:</span><span class="sxs-lookup"><span data-stu-id="90423-549">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="90423-550">Configurazione del server e dell'app</span><span class="sxs-lookup"><span data-stu-id="90423-550">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="90423-551">Limite di lunghezza del corpo multipart</span><span class="sxs-lookup"><span data-stu-id="90423-551">Multipart body length limit</span></span>

<span data-ttu-id="90423-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> imposta il limite per la lunghezza di ogni corpo multipart.</span><span class="sxs-lookup"><span data-stu-id="90423-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="90423-553">Le sezioni del modulo che superano questo limite generano una <xref:System.IO.InvalidDataException> durante l'analisi.</span><span class="sxs-lookup"><span data-stu-id="90423-553">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="90423-554">Il valore predefinito è 134.217.728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="90423-554">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="90423-555">Personalizzare il limite usando l'impostazione <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="90423-555">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="90423-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> viene utilizzato per impostare il <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="90423-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="90423-557">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="90423-557">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="90423-558">In un'app Razor Pages o in un'app MVC, applicare il filtro al modello di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="90423-558">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="90423-559">Dimensioni massime del corpo della richiesta di gheppio</span><span class="sxs-lookup"><span data-stu-id="90423-559">Kestrel maximum request body size</span></span>

<span data-ttu-id="90423-560">Per le app ospitate da gheppio, le dimensioni massime predefinite del corpo della richiesta sono pari a 30 milioni byte, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="90423-560">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="90423-561">Personalizzare il limite usando l'opzione del server [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) gheppio:</span><span class="sxs-lookup"><span data-stu-id="90423-561">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="90423-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> viene usato per impostare [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="90423-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="90423-563">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="90423-563">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="90423-564">In un'app Razor Pages o in un'app MVC, applicare il filtro alla classe del gestore di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="90423-564">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="90423-565">Altri limiti del gheppio</span><span class="sxs-lookup"><span data-stu-id="90423-565">Other Kestrel limits</span></span>

<span data-ttu-id="90423-566">Per le app ospitate da gheppio possono essere applicati altri limiti di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="90423-566">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="90423-567">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="90423-567">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="90423-568">Frequenza dei dati di richiesta e risposta</span><span class="sxs-lookup"><span data-stu-id="90423-568">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="90423-569">Limite lunghezza contenuto IIS</span><span class="sxs-lookup"><span data-stu-id="90423-569">IIS content length limit</span></span>

<span data-ttu-id="90423-570">Il limite di richieste predefinito (`maxAllowedContentLength`) è 30 milioni byte, che corrisponde a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="90423-570">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="90423-571">Personalizzare il limite nel file *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="90423-571">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="90423-572">Questa impostazione si applica solo a IIS.</span><span class="sxs-lookup"><span data-stu-id="90423-572">This setting only applies to IIS.</span></span> <span data-ttu-id="90423-573">Il comportamento non si verifica per impostazione predefinita con l'hosting in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="90423-573">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="90423-574">Per altre informazioni, vedere [limiti delle richieste \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="90423-574">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="90423-575">Le limitazioni del modulo ASP.NET Core o della presenza del modulo filtro richieste IIS possono limitare il caricamento a 2 o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="90423-575">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="90423-576">Per ulteriori informazioni, vedere [non è possibile caricare file di dimensioni superiori a 2 GB (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="90423-576">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="90423-577">Risolvere i problemi</span><span class="sxs-lookup"><span data-stu-id="90423-577">Troubleshoot</span></span>

<span data-ttu-id="90423-578">Di seguito sono trattati alcuni problemi comuni riscontrati durante il caricamento di file, con le soluzioni possibili corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="90423-578">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="90423-579">Errore non trovato quando viene distribuito in un server IIS</span><span class="sxs-lookup"><span data-stu-id="90423-579">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="90423-580">L'errore seguente indica che il file caricato supera la lunghezza del contenuto configurata del server:</span><span class="sxs-lookup"><span data-stu-id="90423-580">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="90423-581">Per ulteriori informazioni sull'aumento del limite, vedere la sezione [limite di lunghezza del contenuto IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="90423-581">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="90423-582">Errore di connessione</span><span class="sxs-lookup"><span data-stu-id="90423-582">Connection failure</span></span>

<span data-ttu-id="90423-583">Un errore di connessione e una connessione al server di reimpostazione probabilmente indicano che il file caricato supera le dimensioni massime del corpo della richiesta del gheppio.</span><span class="sxs-lookup"><span data-stu-id="90423-583">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="90423-584">Per ulteriori informazioni, vedere la sezione relativa alle [dimensioni massime del corpo della richiesta di Gheppio](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="90423-584">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="90423-585">I limiti di connessione del client gheppio possono richiedere anche modifiche.</span><span class="sxs-lookup"><span data-stu-id="90423-585">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="90423-586">Eccezione per riferimento Null con IFormFile</span><span class="sxs-lookup"><span data-stu-id="90423-586">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="90423-587">Se il controller accetta file caricati usando <xref:Microsoft.AspNetCore.Http.IFormFile> ma il valore è `null`, verificare che il formato HTML specifichi un valore `enctype` di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="90423-587">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="90423-588">Se questo attributo non è impostato sull'elemento `<form>`, il caricamento del file non viene eseguito e gli argomenti <xref:Microsoft.AspNetCore.Http.IFormFile> associati sono `null`.</span><span class="sxs-lookup"><span data-stu-id="90423-588">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="90423-589">Verificare inoltre che la [denominazione di caricamento nei dati del modulo corrisponda alla denominazione dell'app](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="90423-589">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="90423-590">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="90423-590">Additional resources</span></span>

* [<span data-ttu-id="90423-591">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="90423-591">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [<span data-ttu-id="90423-592">Sicurezza di Azure: frame di sicurezza: convalida dell'input | Attenuazioni</span><span class="sxs-lookup"><span data-stu-id="90423-592">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="90423-593">Modelli di progettazione cloud di Azure: modello di chiave del posteggiatore</span><span class="sxs-lookup"><span data-stu-id="90423-593">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
