---
title: Caricare file in ASP.NET Core
author: guardrex
description: Come usare l'associazione del modello e lo streaming per caricare i file in ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/02/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: de8bfee22e39dfc5a6ed254cf0555887891d4590
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179302"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="151aa-103">Caricare file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="151aa-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="151aa-104">Di [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)e [Rutger Storm](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="151aa-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="151aa-105">ASP.NET Core supporta il caricamento di uno o più file mediante l'associazione di modelli memorizzati nel buffer per i file più piccoli e il flusso non memorizzato nel buffer per file di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="151aa-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="151aa-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="151aa-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="151aa-107">Considerazioni relative alla sicurezza</span><span class="sxs-lookup"><span data-stu-id="151aa-107">Security considerations</span></span>

<span data-ttu-id="151aa-108">Prestare attenzione quando si fornisce agli utenti la possibilità di caricare file in un server.</span><span class="sxs-lookup"><span data-stu-id="151aa-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="151aa-109">Gli utenti malintenzionati possono tentare di:</span><span class="sxs-lookup"><span data-stu-id="151aa-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="151aa-110">Eseguire attacchi [di tipo Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="151aa-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="151aa-111">Caricare virus o malware.</span><span class="sxs-lookup"><span data-stu-id="151aa-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="151aa-112">Compromettere le reti e i server in altri modi.</span><span class="sxs-lookup"><span data-stu-id="151aa-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="151aa-113">I passaggi di sicurezza che riducono la probabilità di un attacco riuscito sono:</span><span class="sxs-lookup"><span data-stu-id="151aa-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="151aa-114">Caricare i file in un'area dedicata di caricamento di file nel sistema, preferibilmente in un'unità non di sistema.</span><span class="sxs-lookup"><span data-stu-id="151aa-114">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="151aa-115">L'uso di un percorso dedicato rende più semplice l'imposizione di restrizioni di sicurezza sui file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-115">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="151aa-116">Disabilitare le autorizzazioni di esecuzione per il percorso di caricamento del file. &dagger;</span><span class="sxs-lookup"><span data-stu-id="151aa-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="151aa-117">Non salvare mai i file caricati nella stessa struttura di directory dell'app. &dagger;</span><span class="sxs-lookup"><span data-stu-id="151aa-117">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="151aa-118">Usare un nome file sicuro determinato dall'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="151aa-119">Non usare un nome file fornito dall'utente o il nome file non attendibile del file caricato. &dagger; Per visualizzare un nome di file non attendibile in un'interfaccia utente o in un messaggio di registrazione, codificare in HTML il valore.</span><span class="sxs-lookup"><span data-stu-id="151aa-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="151aa-120">Consenti solo un set specifico di estensioni di file approvate. &dagger;</span><span class="sxs-lookup"><span data-stu-id="151aa-120">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="151aa-121">Controllare la firma del formato di file per impedire a un utente di caricare un file mascherato. &dagger; Ad esempio, non consentire a un utente di caricare un file *exe* con estensione *txt* .</span><span class="sxs-lookup"><span data-stu-id="151aa-121">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="151aa-122">Verificare che anche i controlli lato client vengano eseguiti sul server. &dagger; I controlli sul lato client sono facili da aggirare.</span><span class="sxs-lookup"><span data-stu-id="151aa-122">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="151aa-123">Controllare le dimensioni di un file caricato e impedire i caricamenti maggiori del previsto. &dagger;</span><span class="sxs-lookup"><span data-stu-id="151aa-123">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="151aa-124">Quando i file non devono essere sovrascritti da un file caricato con lo stesso nome, controllare il nome del file sul database o sull'archiviazione fisica prima di caricare il file.</span><span class="sxs-lookup"><span data-stu-id="151aa-124">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="151aa-125">**Eseguire uno scanner virus/malware sul contenuto caricato prima che il file venga archiviato.**</span><span class="sxs-lookup"><span data-stu-id="151aa-125">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="151aa-126">l'app di esempio &dagger;The illustra un approccio che soddisfa i criteri.</span><span class="sxs-lookup"><span data-stu-id="151aa-126">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-127">Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:</span><span class="sxs-lookup"><span data-stu-id="151aa-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="151aa-128">Ottenere il controllo completo di un sistema.</span><span class="sxs-lookup"><span data-stu-id="151aa-128">Completely gain control of a system.</span></span>
> * <span data-ttu-id="151aa-129">Sovraccaricare un sistema con il risultato che si verifica un arresto anomalo del sistema.</span><span class="sxs-lookup"><span data-stu-id="151aa-129">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="151aa-130">Compromettere i dati di sistemi o utenti.</span><span class="sxs-lookup"><span data-stu-id="151aa-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="151aa-131">Applicare i graffiti a un'interfaccia utente pubblica.</span><span class="sxs-lookup"><span data-stu-id="151aa-131">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="151aa-132">Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="151aa-132">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="151aa-133">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="151aa-133">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="151aa-134">Sicurezza [Azure: Verificare che siano presenti i controlli appropriati quando si accettano file dagli utenti @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="151aa-134">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="151aa-135">Per ulteriori informazioni sull'implementazione di misure di sicurezza, inclusi esempi dall'app di esempio, vedere la sezione [convalida](#validation) .</span><span class="sxs-lookup"><span data-stu-id="151aa-135">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="151aa-136">Scenari di archiviazione</span><span class="sxs-lookup"><span data-stu-id="151aa-136">Storage scenarios</span></span>

<span data-ttu-id="151aa-137">Le opzioni di archiviazione comuni per i file includono:</span><span class="sxs-lookup"><span data-stu-id="151aa-137">Common storage options for files include:</span></span>

* <span data-ttu-id="151aa-138">Database</span><span class="sxs-lookup"><span data-stu-id="151aa-138">Database</span></span>

  * <span data-ttu-id="151aa-139">Per i caricamenti di file di piccole dimensioni, un database è spesso più veloce rispetto alle opzioni di archiviazione fisica (file system o condivisione di rete).</span><span class="sxs-lookup"><span data-stu-id="151aa-139">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="151aa-140">Un database è spesso più pratico delle opzioni di archiviazione fisica perché il recupero di un record di database per i dati utente può fornire simultaneamente il contenuto del file, ad esempio un'immagine avatar.</span><span class="sxs-lookup"><span data-stu-id="151aa-140">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="151aa-141">Un database è potenzialmente meno costoso rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="151aa-141">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="151aa-142">Archiviazione fisica (file system o condivisione di rete)</span><span class="sxs-lookup"><span data-stu-id="151aa-142">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="151aa-143">Per i caricamenti di file di grandi dimensioni:</span><span class="sxs-lookup"><span data-stu-id="151aa-143">For large file uploads:</span></span>
    * <span data-ttu-id="151aa-144">I limiti del database possono limitare le dimensioni del caricamento.</span><span class="sxs-lookup"><span data-stu-id="151aa-144">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="151aa-145">L'archiviazione fisica è spesso meno economica rispetto all'archiviazione in un database.</span><span class="sxs-lookup"><span data-stu-id="151aa-145">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="151aa-146">L'archiviazione fisica è potenzialmente meno costosa rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="151aa-146">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="151aa-147">Il processo dell'app deve avere le autorizzazioni di lettura e scrittura per il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="151aa-147">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="151aa-148">**Non concedere mai l'autorizzazione Execute.**</span><span class="sxs-lookup"><span data-stu-id="151aa-148">**Never grant execute permission.**</span></span>

* <span data-ttu-id="151aa-149">Servizio di archiviazione dei dati (ad esempio, [archiviazione BLOB di Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="151aa-149">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="151aa-150">I servizi in genere offrono una maggiore scalabilità e resilienza rispetto a soluzioni locali che in genere sono soggette a singoli punti di errore.</span><span class="sxs-lookup"><span data-stu-id="151aa-150">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="151aa-151">I servizi sono un costo potenzialmente inferiore in scenari di infrastruttura di archiviazione di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="151aa-151">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="151aa-152">Per altre informazioni, vedere [Avvio rapido: Usare .NET per creare un BLOB nell'archivio oggetti @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="151aa-152">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="151aa-153">Nell'argomento viene illustrato <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, ma è possibile utilizzare <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> per salvare un <xref:System.IO.FileStream> nell'archivio BLOB quando si utilizza un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="151aa-153">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="151aa-154">Scenari di caricamento di file</span><span class="sxs-lookup"><span data-stu-id="151aa-154">File upload scenarios</span></span>

<span data-ttu-id="151aa-155">Due approcci generali per il caricamento di file sono il buffering e il flusso.</span><span class="sxs-lookup"><span data-stu-id="151aa-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="151aa-156">**Buffer**</span><span class="sxs-lookup"><span data-stu-id="151aa-156">**Buffering**</span></span>

<span data-ttu-id="151aa-157">L'intero file viene letto in un <xref:Microsoft.AspNetCore.Http.IFormFile>, ovvero una C# rappresentazione del file usato per elaborare o salvare il file.</span><span class="sxs-lookup"><span data-stu-id="151aa-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="151aa-158">Le risorse (disco, memoria) utilizzate dai caricamenti di file dipendono dal numero e dalle dimensioni dei caricamenti di file simultanei.</span><span class="sxs-lookup"><span data-stu-id="151aa-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="151aa-159">Se un'app tenta di memorizzare nel buffer un numero eccessivo di caricamenti, il sito si arresta in modo anomalo quando esaurisce la memoria o lo spazio su disco.</span><span class="sxs-lookup"><span data-stu-id="151aa-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="151aa-160">Se le dimensioni o la frequenza dei caricamenti di file esauriscono le risorse dell'app, usare il flusso.</span><span class="sxs-lookup"><span data-stu-id="151aa-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="151aa-161">Ogni singolo file memorizzato nel buffer che supera 64 KB viene spostato dalla memoria in un file temporaneo su disco.</span><span class="sxs-lookup"><span data-stu-id="151aa-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="151aa-162">Il buffering di file di piccole dimensioni viene trattato nelle sezioni seguenti di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="151aa-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="151aa-163">Archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="151aa-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="151aa-164">Database</span><span class="sxs-lookup"><span data-stu-id="151aa-164">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="151aa-165">**Flusso**</span><span class="sxs-lookup"><span data-stu-id="151aa-165">**Streaming**</span></span>

<span data-ttu-id="151aa-166">Il file viene ricevuto da una richiesta multipart ed elaborato o salvato direttamente dall'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="151aa-167">Il flusso non migliora significativamente le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="151aa-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="151aa-168">Il flusso riduce le richieste di memoria o di spazio su disco durante il caricamento dei file.</span><span class="sxs-lookup"><span data-stu-id="151aa-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="151aa-169">Il flusso di file di grandi dimensioni è trattato nella sezione [caricare file di grandi dimensioni con flusso](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="151aa-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="151aa-170">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer all'archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="151aa-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="151aa-171">Per caricare file di piccole dimensioni, usare un modulo multipart o creare una richiesta POST usando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="151aa-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="151aa-172">L'esempio seguente illustra l'uso di un modulo di Razor Pages per caricare un singolo file (*pages/BufferedSingleFileUploadPhysical. cshtml* nell'app di esempio):</span><span class="sxs-lookup"><span data-stu-id="151aa-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="151aa-173">L'esempio seguente è analogo all'esempio precedente, ad eccezione del fatto che:</span><span class="sxs-lookup"><span data-stu-id="151aa-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="151aa-174">Per inviare i dati del modulo, viene usato JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)).</span><span class="sxs-lookup"><span data-stu-id="151aa-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="151aa-175">Nessuna convalida.</span><span class="sxs-lookup"><span data-stu-id="151aa-175">There's no validation.</span></span>

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

<span data-ttu-id="151aa-176">Per eseguire il POST del form in JavaScript per i client che [non supportano l'API fetch](https://caniuse.com/#feat=fetch), usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="151aa-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="151aa-177">Usare un riempimento di recupero (ad esempio, [Window. fetch Refill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="151aa-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="151aa-178">Usare `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="151aa-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="151aa-179">Esempio:</span><span class="sxs-lookup"><span data-stu-id="151aa-179">For example:</span></span>

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

<span data-ttu-id="151aa-180">Per supportare il caricamento di file, i moduli HTML devono specificare un tipo di codifica (`enctype`) di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="151aa-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="151aa-181">Per un elemento di input `files` per supportare il caricamento di più file, fornire l'attributo `multiple` nell'elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="151aa-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="151aa-182">Per accedere ai singoli file caricati nel server, è possibile usare l' [associazione di modelli](xref:mvc/models/model-binding) con <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="151aa-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="151aa-183">L'app di esempio illustra più caricamenti di file memorizzati nel buffer per scenari di archiviazione di database e fisici.</span><span class="sxs-lookup"><span data-stu-id="151aa-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-184">Non utilizzare o considerare attendibile la proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> senza convalida.</span><span class="sxs-lookup"><span data-stu-id="151aa-184">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="151aa-185">La proprietà `FileName` deve essere utilizzata solo a scopo di visualizzazione e solo dopo la codifica HTML del valore.</span><span class="sxs-lookup"><span data-stu-id="151aa-185">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="151aa-186">Gli esempi forniti finora non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="151aa-186">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="151aa-187">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="151aa-187">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="151aa-188">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="151aa-188">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="151aa-189">Convalida</span><span class="sxs-lookup"><span data-stu-id="151aa-189">Validation</span></span>](#validation)

<span data-ttu-id="151aa-190">Quando si caricano file usando l'associazione di modelli e <xref:Microsoft.AspNetCore.Http.IFormFile>, il metodo di azione può accettare:</span><span class="sxs-lookup"><span data-stu-id="151aa-190">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="151aa-191">Singolo <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="151aa-191">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="151aa-192">Qualsiasi raccolta seguente che rappresenta diversi file:</span><span class="sxs-lookup"><span data-stu-id="151aa-192">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="151aa-193">[Elenco](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3</span><span class="sxs-lookup"><span data-stu-id="151aa-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="151aa-194">L'associazione corrisponde ai file di form in base al nome.</span><span class="sxs-lookup"><span data-stu-id="151aa-194">Binding matches form files by name.</span></span> <span data-ttu-id="151aa-195">Ad esempio, il valore HTML `name` in `<input type="file" name="formFile">` deve corrispondere al C# parametro o alla proprietà associata (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="151aa-195">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="151aa-196">Per ulteriori informazioni, vedere la sezione [corrispondenza del nome dell'attributo del nome del parametro del metodo post](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="151aa-196">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="151aa-197">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="151aa-197">The following example:</span></span>

* <span data-ttu-id="151aa-198">Esegue il ciclo di uno o più file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-198">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="151aa-199">USA [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per restituire un percorso completo per un file, incluso il nome del file.</span><span class="sxs-lookup"><span data-stu-id="151aa-199">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="151aa-200">Salva i file nella file system locale usando un nome file generato dall'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-200">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="151aa-201">Restituisce il numero totale e le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-201">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="151aa-202">Utilizzare `Path.GetRandomFileName` per generare un nome file senza percorso.</span><span class="sxs-lookup"><span data-stu-id="151aa-202">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="151aa-203">Nell'esempio seguente il percorso viene ottenuto dalla configurazione:</span><span class="sxs-lookup"><span data-stu-id="151aa-203">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="151aa-204">Il percorso passato al <xref:System.IO.FileStream> *deve* includere il nome del file.</span><span class="sxs-lookup"><span data-stu-id="151aa-204">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="151aa-205">Se il nome file non viene specificato, viene generata un'<xref:System.UnauthorizedAccessException> in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="151aa-205">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="151aa-206">I file caricati con la tecnica <xref:Microsoft.AspNetCore.Http.IFormFile> vengono memorizzati nel buffer in memoria o su disco sul server prima dell'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="151aa-206">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="151aa-207">All'interno del metodo di azione, il contenuto <xref:Microsoft.AspNetCore.Http.IFormFile> è accessibile come <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="151aa-207">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="151aa-208">Oltre al file system locale, i file possono essere salvati in una condivisione di rete o in un servizio di archiviazione file, ad esempio [archiviazione BLOB di Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="151aa-208">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="151aa-209">Per un altro esempio che esegue il ciclo su più file per il caricamento e usa nomi di file sicuri, vedere *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="151aa-209">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-210">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) genera un'<xref:System.IO.IOException> se vengono creati più di 65.535 file senza eliminare i file temporanei precedenti.</span><span class="sxs-lookup"><span data-stu-id="151aa-210">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="151aa-211">Il limite di 65.535 file è un limite per server.</span><span class="sxs-lookup"><span data-stu-id="151aa-211">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="151aa-212">Per ulteriori informazioni su questo limite per il sistema operativo Windows, vedere la sezione Osservazioni negli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="151aa-212">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="151aa-213">GetTempFileNameA (funzione)</span><span class="sxs-lookup"><span data-stu-id="151aa-213">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="151aa-214">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer in un database</span><span class="sxs-lookup"><span data-stu-id="151aa-214">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="151aa-215">Per archiviare i dati dei file binari in un database utilizzando [Entity Framework](/ef/core/index), definire una proprietà della matrice <xref:System.Byte> nell'entità:</span><span class="sxs-lookup"><span data-stu-id="151aa-215">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="151aa-216">Specificare una proprietà del modello di pagina per la classe che include un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="151aa-216">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="151aa-217"><xref:Microsoft.AspNetCore.Http.IFormFile> può essere usato direttamente come parametro del metodo di azione o come proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="151aa-217"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="151aa-218">Nell'esempio precedente viene utilizzata una proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="151aa-218">The prior example uses a bound model property.</span></span>

<span data-ttu-id="151aa-219">Il `FileUpload` viene utilizzato nel formato Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="151aa-219">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="151aa-220">Quando il modulo viene inviato al server, copiare il <xref:Microsoft.AspNetCore.Http.IFormFile> in un flusso e salvarlo come una matrice di byte nel database.</span><span class="sxs-lookup"><span data-stu-id="151aa-220">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="151aa-221">Nell'esempio seguente `_dbContext` archivia il contesto di database dell'app:</span><span class="sxs-lookup"><span data-stu-id="151aa-221">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="151aa-222">L'esempio precedente è simile a uno scenario illustrato nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="151aa-222">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="151aa-223">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="151aa-223">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="151aa-224">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="151aa-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-225">Quando si archiviano dati binari all'interno di database relazionali, è necessario prestare attenzione, perché l'operazione può influire negativamente sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="151aa-225">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="151aa-226">Non utilizzare o considerare attendibile la proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> senza convalida.</span><span class="sxs-lookup"><span data-stu-id="151aa-226">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="151aa-227">La proprietà `FileName` deve essere utilizzata solo a scopo di visualizzazione e solo dopo la codifica HTML.</span><span class="sxs-lookup"><span data-stu-id="151aa-227">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="151aa-228">Gli esempi forniti non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="151aa-228">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="151aa-229">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="151aa-229">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="151aa-230">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="151aa-230">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="151aa-231">Convalida</span><span class="sxs-lookup"><span data-stu-id="151aa-231">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="151aa-232">Caricare file di grandi dimensioni con flusso</span><span class="sxs-lookup"><span data-stu-id="151aa-232">Upload large files with streaming</span></span>

<span data-ttu-id="151aa-233">Nell'esempio seguente viene illustrato come utilizzare JavaScript per trasmettere un file a un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="151aa-233">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="151aa-234">Il token antifalsificazione del file viene generato usando un attributo di filtro personalizzato e passato alle intestazioni HTTP del client anziché nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="151aa-234">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="151aa-235">Poiché il metodo di azione elabora direttamente i dati caricati, l'associazione del modello di modulo viene disabilitata da un altro filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="151aa-235">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="151aa-236">All'interno dell'azione, il contenuto del form viene letto tramite un `MultipartReader`, che legge ogni singola `MultipartSection`, elaborando il file o archiviandone il contenuto, come appropriato.</span><span class="sxs-lookup"><span data-stu-id="151aa-236">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="151aa-237">Una volta lette le sezioni multiparte, l'azione esegue una propria associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="151aa-237">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="151aa-238">La risposta della pagina iniziale carica il modulo e salva un token antifalsificazione in un cookie (tramite l'attributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="151aa-238">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="151aa-239">L'attributo usa il [supporto antifalsificazione](xref:security/anti-request-forgery) predefinito di ASP.NET Core per impostare un cookie con un token di richiesta:</span><span class="sxs-lookup"><span data-stu-id="151aa-239">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="151aa-240">Il `DisableFormValueModelBindingAttribute` viene usato per disabilitare l'associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="151aa-240">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="151aa-241">Nell'app di esempio, `GenerateAntiforgeryTokenCookieAttribute` e `DisableFormValueModelBindingAttribute` vengono applicati come filtri ai modelli di applicazione della pagina di `/StreamedSingleFileUploadDb` e `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` usando le [convenzioni di Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="151aa-241">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="151aa-242">Poiché l'associazione di modelli non legge il modulo, i parametri associati al modulo non vengono associati (la query, la route e l'intestazione continuano a funzionare).</span><span class="sxs-lookup"><span data-stu-id="151aa-242">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="151aa-243">Il metodo di azione funziona direttamente con la proprietà `Request`.</span><span class="sxs-lookup"><span data-stu-id="151aa-243">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="151aa-244">Per leggere ogni sezione, viene usato un `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="151aa-244">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="151aa-245">I dati chiave/valore vengono archiviati in un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="151aa-245">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="151aa-246">Dopo la lettura delle sezioni multiparte, il contenuto del `KeyValueAccumulator` viene utilizzato per associare i dati del modulo a un tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="151aa-246">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="151aa-247">Il metodo completo `StreamingController.UploadDatabase` per lo streaming in un database con EF Core:</span><span class="sxs-lookup"><span data-stu-id="151aa-247">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="151aa-248">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="151aa-248">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="151aa-249">Il metodo completo `StreamingController.UploadPhysical` per lo streaming in un percorso fisico:</span><span class="sxs-lookup"><span data-stu-id="151aa-249">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="151aa-250">Nell'app di esempio, i controlli di convalida vengono gestiti da `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="151aa-250">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="151aa-251">Convalida</span><span class="sxs-lookup"><span data-stu-id="151aa-251">Validation</span></span>

<span data-ttu-id="151aa-252">La classe `FileHelpers` dell'app di esempio illustra alcuni controlli per i caricamenti di file <xref:Microsoft.AspNetCore.Http.IFormFile> memorizzati nel buffer.</span><span class="sxs-lookup"><span data-stu-id="151aa-252">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="151aa-253">Per l'elaborazione di caricamenti di file memorizzati nel buffer <xref:Microsoft.AspNetCore.Http.IFormFile> nell'app di esempio, vedere il metodo `ProcessFormFile` nel file *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="151aa-253">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="151aa-254">Per l'elaborazione di file trasmessi, vedere il metodo `ProcessStreamedFile` nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="151aa-254">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-255">I metodi di elaborazione della convalida illustrati nell'app di esempio non analizzano il contenuto dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-255">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="151aa-256">Nella maggior parte degli scenari di produzione, nel file viene usata un'API scanner di virus/malware prima di rendere disponibile il file per gli utenti o altri sistemi.</span><span class="sxs-lookup"><span data-stu-id="151aa-256">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="151aa-257">Sebbene l'esempio di argomento fornisca un esempio funzionante di tecniche di convalida, non implementare la classe `FileHelpers` in un'app di produzione a meno che non si:</span><span class="sxs-lookup"><span data-stu-id="151aa-257">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="151aa-258">Comprendere completamente l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="151aa-258">Fully understand the implementation.</span></span>
> * <span data-ttu-id="151aa-259">Modificare l'implementazione in modo appropriato per l'ambiente e le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-259">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="151aa-260">**Non implementare mai indiscriminatamente il codice di sicurezza in un'app senza soddisfare questi requisiti.**</span><span class="sxs-lookup"><span data-stu-id="151aa-260">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="151aa-261">Convalida del contenuto</span><span class="sxs-lookup"><span data-stu-id="151aa-261">Content validation</span></span>

<span data-ttu-id="151aa-262">**Usare un'API di analisi di virus/malware di terze parti nel contenuto caricato.**</span><span class="sxs-lookup"><span data-stu-id="151aa-262">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="151aa-263">L'analisi dei file richiede le risorse del server in scenari con volumi elevati.</span><span class="sxs-lookup"><span data-stu-id="151aa-263">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="151aa-264">Se le prestazioni di elaborazione delle richieste sono diminuite a causa dell'analisi dei file, provare a eseguire l'offload del lavoro di analisi in un [servizio in background](xref:fundamentals/host/hosted-services), possibilmente in un servizio in esecuzione in un server diverso dal server dell'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-264">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="151aa-265">In genere, i file caricati vengono mantenuti in un'area in quarantena finché non vengono controllati dal programma antivirus in background.</span><span class="sxs-lookup"><span data-stu-id="151aa-265">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="151aa-266">Quando un file passa, il file viene spostato nel percorso di archiviazione file normale.</span><span class="sxs-lookup"><span data-stu-id="151aa-266">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="151aa-267">Questi passaggi vengono in genere eseguiti insieme a un record di database che indica lo stato di analisi di un file.</span><span class="sxs-lookup"><span data-stu-id="151aa-267">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="151aa-268">Usando un approccio di questo tipo, l'app e il server app rimangono incentrati sulla risposta alle richieste.</span><span class="sxs-lookup"><span data-stu-id="151aa-268">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="151aa-269">Convalida dell'estensione di file</span><span class="sxs-lookup"><span data-stu-id="151aa-269">File extension validation</span></span>

<span data-ttu-id="151aa-270">L'estensione del file caricato deve essere verificata rispetto a un elenco di estensioni consentite.</span><span class="sxs-lookup"><span data-stu-id="151aa-270">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="151aa-271">Esempio:</span><span class="sxs-lookup"><span data-stu-id="151aa-271">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="151aa-272">Convalida della firma del file</span><span class="sxs-lookup"><span data-stu-id="151aa-272">File signature validation</span></span>

<span data-ttu-id="151aa-273">La firma di un file è determinata dai primi byte all'inizio di un file.</span><span class="sxs-lookup"><span data-stu-id="151aa-273">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="151aa-274">Questi byte possono essere usati per indicare se l'estensione corrisponde al contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="151aa-274">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="151aa-275">L'app di esempio controlla le firme dei file per alcuni tipi di file comuni.</span><span class="sxs-lookup"><span data-stu-id="151aa-275">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="151aa-276">Nell'esempio seguente, la firma del file per un'immagine JPEG viene verificata rispetto al file:</span><span class="sxs-lookup"><span data-stu-id="151aa-276">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="151aa-277">Per ottenere ulteriori firme di file, vedere il [database delle firme file](https://www.filesignatures.net/) e le specifiche del file ufficiale.</span><span class="sxs-lookup"><span data-stu-id="151aa-277">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="151aa-278">Sicurezza del nome file</span><span class="sxs-lookup"><span data-stu-id="151aa-278">File name security</span></span>

<span data-ttu-id="151aa-279">Non usare mai un nome file fornito dal client per salvare un file nell'archiviazione fisica.</span><span class="sxs-lookup"><span data-stu-id="151aa-279">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="151aa-280">Creare un nome file sicuro per il file usando [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per creare un percorso completo (incluso il nome file) per l'archiviazione temporanea.</span><span class="sxs-lookup"><span data-stu-id="151aa-280">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="151aa-281">Razor HTML codifica automaticamente i valori delle proprietà per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="151aa-281">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="151aa-282">Il codice seguente è sicuro da usare:</span><span class="sxs-lookup"><span data-stu-id="151aa-282">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="151aa-283">All'esterno di Razor, è sempre @no__t il contenuto del nome file da una richiesta dell'utente.</span><span class="sxs-lookup"><span data-stu-id="151aa-283">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="151aa-284">Molte implementazioni devono includere un controllo per verificare che il file esista. in caso contrario, il file viene sovrascritto da un file con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="151aa-284">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="151aa-285">Fornire logica aggiuntiva per soddisfare le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-285">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="151aa-286">Convalida dimensioni</span><span class="sxs-lookup"><span data-stu-id="151aa-286">Size validation</span></span>

<span data-ttu-id="151aa-287">Limitare le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-287">Limit the size of uploaded files.</span></span>

<span data-ttu-id="151aa-288">Nell'app di esempio, le dimensioni del file sono limitate a 2 MB (indicate in byte).</span><span class="sxs-lookup"><span data-stu-id="151aa-288">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="151aa-289">Il limite viene fornito tramite la [configurazione](xref:fundamentals/configuration/index) dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="151aa-289">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="151aa-290">Il `FileSizeLimit` viene inserito in classi `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="151aa-290">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="151aa-291">Quando le dimensioni di un file superano il limite, il file viene rifiutato:</span><span class="sxs-lookup"><span data-stu-id="151aa-291">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="151aa-292">Corrisponde al valore dell'attributo Name al nome del parametro del metodo POST</span><span class="sxs-lookup"><span data-stu-id="151aa-292">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="151aa-293">Nei moduli non Razor che INVIAno dati del modulo o usano direttamente `FormData` di JavaScript, il nome specificato nell'elemento del form o `FormData` deve corrispondere al nome del parametro nell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="151aa-293">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="151aa-294">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="151aa-294">In the following example:</span></span>

* <span data-ttu-id="151aa-295">Quando si usa un elemento `<input>`, l'attributo `name` viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="151aa-295">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="151aa-296">Quando si usa `FormData` in JavaScript, il nome viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="151aa-296">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="151aa-297">Usare un nome corrispondente per il parametro del C# metodo (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="151aa-297">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="151aa-298">Per un metodo del gestore di pagina Razor Pages denominato `Upload`:</span><span class="sxs-lookup"><span data-stu-id="151aa-298">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="151aa-299">Per un metodo di azione POST controller MVC:</span><span class="sxs-lookup"><span data-stu-id="151aa-299">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="151aa-300">Configurazione del server e dell'app</span><span class="sxs-lookup"><span data-stu-id="151aa-300">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="151aa-301">Limite di lunghezza del corpo multipart</span><span class="sxs-lookup"><span data-stu-id="151aa-301">Multipart body length limit</span></span>

<span data-ttu-id="151aa-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> imposta il limite per la lunghezza di ogni corpo multipart.</span><span class="sxs-lookup"><span data-stu-id="151aa-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="151aa-303">Le sezioni del modulo che superano questo limite generano un'<xref:System.IO.InvalidDataException> quando vengono analizzate.</span><span class="sxs-lookup"><span data-stu-id="151aa-303">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="151aa-304">Il valore predefinito è 134.217.728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="151aa-304">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="151aa-305">Personalizzare il limite usando l'impostazione <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="151aa-305">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="151aa-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> viene usato per impostare il <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="151aa-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="151aa-307">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="151aa-307">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="151aa-308">In un'app Razor Pages o in un'app MVC, applicare il filtro al modello di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="151aa-308">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="151aa-309">Dimensioni massime del corpo della richiesta di gheppio</span><span class="sxs-lookup"><span data-stu-id="151aa-309">Kestrel maximum request body size</span></span>

<span data-ttu-id="151aa-310">Per le app ospitate da gheppio, le dimensioni massime predefinite del corpo della richiesta sono pari a 30 milioni byte, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="151aa-310">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="151aa-311">Personalizzare il limite usando l'opzione del server [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) gheppio:</span><span class="sxs-lookup"><span data-stu-id="151aa-311">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="151aa-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> viene usato per impostare [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="151aa-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="151aa-313">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="151aa-313">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="151aa-314">In un'app Razor Pages o in un'app MVC, applicare il filtro alla classe del gestore di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="151aa-314">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="151aa-315">Il `RequestSizeLimitAttribute` può essere applicato anche usando la direttiva Razor [@attribute](xref:mvc/views/razor#attribute) :</span><span class="sxs-lookup"><span data-stu-id="151aa-315">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="151aa-316">Altri limiti del gheppio</span><span class="sxs-lookup"><span data-stu-id="151aa-316">Other Kestrel limits</span></span>

<span data-ttu-id="151aa-317">Per le app ospitate da gheppio possono essere applicati altri limiti di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="151aa-317">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="151aa-318">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="151aa-318">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="151aa-319">Frequenza dei dati di richiesta e risposta</span><span class="sxs-lookup"><span data-stu-id="151aa-319">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="151aa-320">Limite lunghezza contenuto IIS</span><span class="sxs-lookup"><span data-stu-id="151aa-320">IIS content length limit</span></span>

<span data-ttu-id="151aa-321">Il limite di richieste predefinito (`maxAllowedContentLength`) è 30 milioni byte, che corrisponde a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="151aa-321">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="151aa-322">Personalizzare il limite nel file *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="151aa-322">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="151aa-323">Questa impostazione si applica solo a IIS.</span><span class="sxs-lookup"><span data-stu-id="151aa-323">This setting only applies to IIS.</span></span> <span data-ttu-id="151aa-324">Il comportamento non si verifica per impostazione predefinita con l'hosting in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="151aa-324">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="151aa-325">Per altre informazioni, vedere [limiti della richiesta \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="151aa-325">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="151aa-326">Le limitazioni del modulo ASP.NET Core o della presenza del modulo filtro richieste IIS possono limitare il caricamento a 2 o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="151aa-326">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="151aa-327">Per ulteriori informazioni, vedere [non è possibile caricare file di dimensioni superiori a 2 GB (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="151aa-327">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="151aa-328">Risolvere problemi</span><span class="sxs-lookup"><span data-stu-id="151aa-328">Troubleshoot</span></span>

<span data-ttu-id="151aa-329">Di seguito sono trattati alcuni problemi comuni riscontrati durante il caricamento di file, con le soluzioni possibili corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="151aa-329">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="151aa-330">Errore non trovato quando viene distribuito in un server IIS</span><span class="sxs-lookup"><span data-stu-id="151aa-330">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="151aa-331">L'errore seguente indica che il file caricato supera la lunghezza del contenuto configurata del server:</span><span class="sxs-lookup"><span data-stu-id="151aa-331">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="151aa-332">Per ulteriori informazioni sull'aumento del limite, vedere la sezione [limite di lunghezza del contenuto IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="151aa-332">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="151aa-333">Errore di connessione</span><span class="sxs-lookup"><span data-stu-id="151aa-333">Connection failure</span></span>

<span data-ttu-id="151aa-334">Un errore di connessione e una connessione al server di reimpostazione probabilmente indicano che il file caricato supera le dimensioni massime del corpo della richiesta del gheppio.</span><span class="sxs-lookup"><span data-stu-id="151aa-334">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="151aa-335">Per ulteriori informazioni, vedere la sezione relativa alle [dimensioni massime del corpo della richiesta di Gheppio](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="151aa-335">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="151aa-336">I limiti di connessione del client gheppio possono richiedere anche modifiche.</span><span class="sxs-lookup"><span data-stu-id="151aa-336">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="151aa-337">Eccezione per riferimento Null con IFormFile</span><span class="sxs-lookup"><span data-stu-id="151aa-337">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="151aa-338">Se il controller accetta i file caricati usando <xref:Microsoft.AspNetCore.Http.IFormFile>, ma il valore è `null`, verificare che il form HTML specifichi un valore di `enctype` di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="151aa-338">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="151aa-339">Se questo attributo non è impostato sull'elemento `<form>`, il caricamento del file non viene eseguito e gli argomenti <xref:Microsoft.AspNetCore.Http.IFormFile> associati sono `null`.</span><span class="sxs-lookup"><span data-stu-id="151aa-339">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="151aa-340">Verificare inoltre che la [denominazione di caricamento nei dati del modulo corrisponda alla denominazione dell'app](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="151aa-340">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="151aa-341">ASP.NET Core supporta il caricamento di uno o più file mediante l'associazione di modelli memorizzati nel buffer per i file più piccoli e il flusso non memorizzato nel buffer per file di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="151aa-341">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="151aa-342">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="151aa-342">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="151aa-343">Considerazioni relative alla sicurezza</span><span class="sxs-lookup"><span data-stu-id="151aa-343">Security considerations</span></span>

<span data-ttu-id="151aa-344">Prestare attenzione quando si fornisce agli utenti la possibilità di caricare file in un server.</span><span class="sxs-lookup"><span data-stu-id="151aa-344">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="151aa-345">Gli utenti malintenzionati possono tentare di:</span><span class="sxs-lookup"><span data-stu-id="151aa-345">Attackers may attempt to:</span></span>

* <span data-ttu-id="151aa-346">Eseguire attacchi [di tipo Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="151aa-346">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="151aa-347">Caricare virus o malware.</span><span class="sxs-lookup"><span data-stu-id="151aa-347">Upload viruses or malware.</span></span>
* <span data-ttu-id="151aa-348">Compromettere le reti e i server in altri modi.</span><span class="sxs-lookup"><span data-stu-id="151aa-348">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="151aa-349">I passaggi di sicurezza che riducono la probabilità di un attacco riuscito sono:</span><span class="sxs-lookup"><span data-stu-id="151aa-349">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="151aa-350">Caricare i file in un'area dedicata di caricamento di file nel sistema, preferibilmente in un'unità non di sistema.</span><span class="sxs-lookup"><span data-stu-id="151aa-350">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="151aa-351">L'uso di un percorso dedicato rende più semplice l'imposizione di restrizioni di sicurezza sui file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-351">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="151aa-352">Disabilitare le autorizzazioni di esecuzione per il percorso di caricamento del file. &dagger;</span><span class="sxs-lookup"><span data-stu-id="151aa-352">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="151aa-353">Non salvare mai i file caricati nella stessa struttura di directory dell'app. &dagger;</span><span class="sxs-lookup"><span data-stu-id="151aa-353">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="151aa-354">Usare un nome file sicuro determinato dall'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-354">Use a safe file name determined by the app.</span></span> <span data-ttu-id="151aa-355">Non usare un nome file fornito dall'utente o il nome file non attendibile del file caricato. &dagger; Per visualizzare un nome di file non attendibile in un'interfaccia utente o in un messaggio di registrazione, codificare in HTML il valore.</span><span class="sxs-lookup"><span data-stu-id="151aa-355">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="151aa-356">Consenti solo un set specifico di estensioni di file approvate. &dagger;</span><span class="sxs-lookup"><span data-stu-id="151aa-356">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="151aa-357">Controllare la firma del formato di file per impedire a un utente di caricare un file mascherato. &dagger; Ad esempio, non consentire a un utente di caricare un file *exe* con estensione *txt* .</span><span class="sxs-lookup"><span data-stu-id="151aa-357">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="151aa-358">Verificare che anche i controlli lato client vengano eseguiti sul server. &dagger; I controlli sul lato client sono facili da aggirare.</span><span class="sxs-lookup"><span data-stu-id="151aa-358">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="151aa-359">Controllare le dimensioni di un file caricato e impedire i caricamenti maggiori del previsto. &dagger;</span><span class="sxs-lookup"><span data-stu-id="151aa-359">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="151aa-360">Quando i file non devono essere sovrascritti da un file caricato con lo stesso nome, controllare il nome del file sul database o sull'archiviazione fisica prima di caricare il file.</span><span class="sxs-lookup"><span data-stu-id="151aa-360">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="151aa-361">**Eseguire uno scanner virus/malware sul contenuto caricato prima che il file venga archiviato.**</span><span class="sxs-lookup"><span data-stu-id="151aa-361">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="151aa-362">l'app di esempio &dagger;The illustra un approccio che soddisfa i criteri.</span><span class="sxs-lookup"><span data-stu-id="151aa-362">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-363">Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:</span><span class="sxs-lookup"><span data-stu-id="151aa-363">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="151aa-364">Ottenere il controllo completo di un sistema.</span><span class="sxs-lookup"><span data-stu-id="151aa-364">Completely gain control of a system.</span></span>
> * <span data-ttu-id="151aa-365">Sovraccaricare un sistema con il risultato che si verifica un arresto anomalo del sistema.</span><span class="sxs-lookup"><span data-stu-id="151aa-365">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="151aa-366">Compromettere i dati di sistemi o utenti.</span><span class="sxs-lookup"><span data-stu-id="151aa-366">Compromise user or system data.</span></span>
> * <span data-ttu-id="151aa-367">Applicare i graffiti a un'interfaccia utente pubblica.</span><span class="sxs-lookup"><span data-stu-id="151aa-367">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="151aa-368">Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="151aa-368">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="151aa-369">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="151aa-369">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="151aa-370">Sicurezza [Azure: Verificare che siano presenti i controlli appropriati quando si accettano file dagli utenti @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="151aa-370">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="151aa-371">Per ulteriori informazioni sull'implementazione di misure di sicurezza, inclusi esempi dall'app di esempio, vedere la sezione [convalida](#validation) .</span><span class="sxs-lookup"><span data-stu-id="151aa-371">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="151aa-372">Scenari di archiviazione</span><span class="sxs-lookup"><span data-stu-id="151aa-372">Storage scenarios</span></span>

<span data-ttu-id="151aa-373">Le opzioni di archiviazione comuni per i file includono:</span><span class="sxs-lookup"><span data-stu-id="151aa-373">Common storage options for files include:</span></span>

* <span data-ttu-id="151aa-374">Database</span><span class="sxs-lookup"><span data-stu-id="151aa-374">Database</span></span>

  * <span data-ttu-id="151aa-375">Per i caricamenti di file di piccole dimensioni, un database è spesso più veloce rispetto alle opzioni di archiviazione fisica (file system o condivisione di rete).</span><span class="sxs-lookup"><span data-stu-id="151aa-375">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="151aa-376">Un database è spesso più pratico delle opzioni di archiviazione fisica perché il recupero di un record di database per i dati utente può fornire simultaneamente il contenuto del file, ad esempio un'immagine avatar.</span><span class="sxs-lookup"><span data-stu-id="151aa-376">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="151aa-377">Un database è potenzialmente meno costoso rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="151aa-377">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="151aa-378">Archiviazione fisica (file system o condivisione di rete)</span><span class="sxs-lookup"><span data-stu-id="151aa-378">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="151aa-379">Per i caricamenti di file di grandi dimensioni:</span><span class="sxs-lookup"><span data-stu-id="151aa-379">For large file uploads:</span></span>
    * <span data-ttu-id="151aa-380">I limiti del database possono limitare le dimensioni del caricamento.</span><span class="sxs-lookup"><span data-stu-id="151aa-380">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="151aa-381">L'archiviazione fisica è spesso meno economica rispetto all'archiviazione in un database.</span><span class="sxs-lookup"><span data-stu-id="151aa-381">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="151aa-382">L'archiviazione fisica è potenzialmente meno costosa rispetto all'utilizzo di un servizio di archiviazione dati.</span><span class="sxs-lookup"><span data-stu-id="151aa-382">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="151aa-383">Il processo dell'app deve avere le autorizzazioni di lettura e scrittura per il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="151aa-383">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="151aa-384">**Non concedere mai l'autorizzazione Execute.**</span><span class="sxs-lookup"><span data-stu-id="151aa-384">**Never grant execute permission.**</span></span>

* <span data-ttu-id="151aa-385">Servizio di archiviazione dei dati (ad esempio, [archiviazione BLOB di Azure](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="151aa-385">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="151aa-386">I servizi in genere offrono una maggiore scalabilità e resilienza rispetto a soluzioni locali che in genere sono soggette a singoli punti di errore.</span><span class="sxs-lookup"><span data-stu-id="151aa-386">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="151aa-387">I servizi sono un costo potenzialmente inferiore in scenari di infrastruttura di archiviazione di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="151aa-387">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="151aa-388">Per altre informazioni, vedere [Avvio rapido: Usare .NET per creare un BLOB nell'archivio oggetti @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="151aa-388">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="151aa-389">Nell'argomento viene illustrato <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, ma è possibile utilizzare <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> per salvare un <xref:System.IO.FileStream> nell'archivio BLOB quando si utilizza un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="151aa-389">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="151aa-390">Scenari di caricamento di file</span><span class="sxs-lookup"><span data-stu-id="151aa-390">File upload scenarios</span></span>

<span data-ttu-id="151aa-391">Due approcci generali per il caricamento di file sono il buffering e il flusso.</span><span class="sxs-lookup"><span data-stu-id="151aa-391">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="151aa-392">**Buffer**</span><span class="sxs-lookup"><span data-stu-id="151aa-392">**Buffering**</span></span>

<span data-ttu-id="151aa-393">L'intero file viene letto in un <xref:Microsoft.AspNetCore.Http.IFormFile>, ovvero una C# rappresentazione del file usato per elaborare o salvare il file.</span><span class="sxs-lookup"><span data-stu-id="151aa-393">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="151aa-394">Le risorse (disco, memoria) utilizzate dai caricamenti di file dipendono dal numero e dalle dimensioni dei caricamenti di file simultanei.</span><span class="sxs-lookup"><span data-stu-id="151aa-394">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="151aa-395">Se un'app tenta di memorizzare nel buffer un numero eccessivo di caricamenti, il sito si arresta in modo anomalo quando esaurisce la memoria o lo spazio su disco.</span><span class="sxs-lookup"><span data-stu-id="151aa-395">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="151aa-396">Se le dimensioni o la frequenza dei caricamenti di file esauriscono le risorse dell'app, usare il flusso.</span><span class="sxs-lookup"><span data-stu-id="151aa-396">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="151aa-397">Ogni singolo file memorizzato nel buffer che supera 64 KB viene spostato dalla memoria in un file temporaneo su disco.</span><span class="sxs-lookup"><span data-stu-id="151aa-397">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="151aa-398">Il buffering di file di piccole dimensioni viene trattato nelle sezioni seguenti di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="151aa-398">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="151aa-399">Archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="151aa-399">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="151aa-400">Database</span><span class="sxs-lookup"><span data-stu-id="151aa-400">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="151aa-401">**Flusso**</span><span class="sxs-lookup"><span data-stu-id="151aa-401">**Streaming**</span></span>

<span data-ttu-id="151aa-402">Il file viene ricevuto da una richiesta multipart ed elaborato o salvato direttamente dall'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-402">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="151aa-403">Il flusso non migliora significativamente le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="151aa-403">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="151aa-404">Il flusso riduce le richieste di memoria o di spazio su disco durante il caricamento dei file.</span><span class="sxs-lookup"><span data-stu-id="151aa-404">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="151aa-405">Il flusso di file di grandi dimensioni è trattato nella sezione [caricare file di grandi dimensioni con flusso](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="151aa-405">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="151aa-406">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer all'archiviazione fisica</span><span class="sxs-lookup"><span data-stu-id="151aa-406">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="151aa-407">Per caricare file di piccole dimensioni, usare un modulo multipart o creare una richiesta POST usando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="151aa-407">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="151aa-408">L'esempio seguente illustra l'uso di un modulo di Razor Pages per caricare un singolo file (*pages/BufferedSingleFileUploadPhysical. cshtml* nell'app di esempio):</span><span class="sxs-lookup"><span data-stu-id="151aa-408">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="151aa-409">L'esempio seguente è analogo all'esempio precedente, ad eccezione del fatto che:</span><span class="sxs-lookup"><span data-stu-id="151aa-409">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="151aa-410">Per inviare i dati del modulo, viene usato JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)).</span><span class="sxs-lookup"><span data-stu-id="151aa-410">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="151aa-411">Nessuna convalida.</span><span class="sxs-lookup"><span data-stu-id="151aa-411">There's no validation.</span></span>

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

<span data-ttu-id="151aa-412">Per eseguire il POST del form in JavaScript per i client che [non supportano l'API fetch](https://caniuse.com/#feat=fetch), usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="151aa-412">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="151aa-413">Usare un riempimento di recupero (ad esempio, [Window. fetch Refill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="151aa-413">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="151aa-414">Usare `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="151aa-414">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="151aa-415">Esempio:</span><span class="sxs-lookup"><span data-stu-id="151aa-415">For example:</span></span>

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

<span data-ttu-id="151aa-416">Per supportare il caricamento di file, i moduli HTML devono specificare un tipo di codifica (`enctype`) di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="151aa-416">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="151aa-417">Per un elemento di input `files` per supportare il caricamento di più file, fornire l'attributo `multiple` nell'elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="151aa-417">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="151aa-418">Per accedere ai singoli file caricati nel server, è possibile usare l' [associazione di modelli](xref:mvc/models/model-binding) con <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="151aa-418">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="151aa-419">L'app di esempio illustra più caricamenti di file memorizzati nel buffer per scenari di archiviazione di database e fisici.</span><span class="sxs-lookup"><span data-stu-id="151aa-419">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-420">Non utilizzare o considerare attendibile la proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> senza convalida.</span><span class="sxs-lookup"><span data-stu-id="151aa-420">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="151aa-421">La proprietà `FileName` deve essere utilizzata solo a scopo di visualizzazione e solo dopo la codifica HTML del valore.</span><span class="sxs-lookup"><span data-stu-id="151aa-421">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="151aa-422">Gli esempi forniti finora non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="151aa-422">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="151aa-423">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="151aa-423">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="151aa-424">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="151aa-424">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="151aa-425">Convalida</span><span class="sxs-lookup"><span data-stu-id="151aa-425">Validation</span></span>](#validation)

<span data-ttu-id="151aa-426">Quando si caricano file usando l'associazione di modelli e <xref:Microsoft.AspNetCore.Http.IFormFile>, il metodo di azione può accettare:</span><span class="sxs-lookup"><span data-stu-id="151aa-426">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="151aa-427">Singolo <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="151aa-427">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="151aa-428">Qualsiasi raccolta seguente che rappresenta diversi file:</span><span class="sxs-lookup"><span data-stu-id="151aa-428">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="151aa-429">[Elenco](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3</span><span class="sxs-lookup"><span data-stu-id="151aa-429">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="151aa-430">L'associazione corrisponde ai file di form in base al nome.</span><span class="sxs-lookup"><span data-stu-id="151aa-430">Binding matches form files by name.</span></span> <span data-ttu-id="151aa-431">Ad esempio, il valore HTML `name` in `<input type="file" name="formFile">` deve corrispondere al C# parametro o alla proprietà associata (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="151aa-431">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="151aa-432">Per ulteriori informazioni, vedere la sezione [corrispondenza del nome dell'attributo del nome del parametro del metodo post](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="151aa-432">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="151aa-433">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="151aa-433">The following example:</span></span>

* <span data-ttu-id="151aa-434">Esegue il ciclo di uno o più file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-434">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="151aa-435">USA [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per restituire un percorso completo per un file, incluso il nome del file.</span><span class="sxs-lookup"><span data-stu-id="151aa-435">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="151aa-436">Salva i file nella file system locale usando un nome file generato dall'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-436">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="151aa-437">Restituisce il numero totale e le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-437">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="151aa-438">Utilizzare `Path.GetRandomFileName` per generare un nome file senza percorso.</span><span class="sxs-lookup"><span data-stu-id="151aa-438">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="151aa-439">Nell'esempio seguente il percorso viene ottenuto dalla configurazione:</span><span class="sxs-lookup"><span data-stu-id="151aa-439">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="151aa-440">Il percorso passato al <xref:System.IO.FileStream> *deve* includere il nome del file.</span><span class="sxs-lookup"><span data-stu-id="151aa-440">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="151aa-441">Se il nome file non viene specificato, viene generata un'<xref:System.UnauthorizedAccessException> in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="151aa-441">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="151aa-442">I file caricati con la tecnica <xref:Microsoft.AspNetCore.Http.IFormFile> vengono memorizzati nel buffer in memoria o su disco sul server prima dell'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="151aa-442">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="151aa-443">All'interno del metodo di azione, il contenuto <xref:Microsoft.AspNetCore.Http.IFormFile> è accessibile come <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="151aa-443">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="151aa-444">Oltre al file system locale, i file possono essere salvati in una condivisione di rete o in un servizio di archiviazione file, ad esempio [archiviazione BLOB di Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="151aa-444">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="151aa-445">Per un altro esempio che esegue il ciclo su più file per il caricamento e usa nomi di file sicuri, vedere *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="151aa-445">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-446">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) genera un'<xref:System.IO.IOException> se vengono creati più di 65.535 file senza eliminare i file temporanei precedenti.</span><span class="sxs-lookup"><span data-stu-id="151aa-446">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="151aa-447">Il limite di 65.535 file è un limite per server.</span><span class="sxs-lookup"><span data-stu-id="151aa-447">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="151aa-448">Per ulteriori informazioni su questo limite per il sistema operativo Windows, vedere la sezione Osservazioni negli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="151aa-448">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="151aa-449">GetTempFileNameA (funzione)</span><span class="sxs-lookup"><span data-stu-id="151aa-449">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="151aa-450">Caricare file di piccole dimensioni con associazione di modelli memorizzati nel buffer in un database</span><span class="sxs-lookup"><span data-stu-id="151aa-450">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="151aa-451">Per archiviare i dati dei file binari in un database utilizzando [Entity Framework](/ef/core/index), definire una proprietà della matrice <xref:System.Byte> nell'entità:</span><span class="sxs-lookup"><span data-stu-id="151aa-451">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="151aa-452">Specificare una proprietà del modello di pagina per la classe che include un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="151aa-452">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="151aa-453"><xref:Microsoft.AspNetCore.Http.IFormFile> può essere usato direttamente come parametro del metodo di azione o come proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="151aa-453"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="151aa-454">Nell'esempio precedente viene utilizzata una proprietà del modello associato.</span><span class="sxs-lookup"><span data-stu-id="151aa-454">The prior example uses a bound model property.</span></span>

<span data-ttu-id="151aa-455">Il `FileUpload` viene utilizzato nel formato Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="151aa-455">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="151aa-456">Quando il modulo viene inviato al server, copiare il <xref:Microsoft.AspNetCore.Http.IFormFile> in un flusso e salvarlo come una matrice di byte nel database.</span><span class="sxs-lookup"><span data-stu-id="151aa-456">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="151aa-457">Nell'esempio seguente `_dbContext` archivia il contesto di database dell'app:</span><span class="sxs-lookup"><span data-stu-id="151aa-457">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="151aa-458">L'esempio precedente è simile a uno scenario illustrato nell'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="151aa-458">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="151aa-459">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="151aa-459">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="151aa-460">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="151aa-460">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-461">Quando si archiviano dati binari all'interno di database relazionali, è necessario prestare attenzione, perché l'operazione può influire negativamente sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="151aa-461">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="151aa-462">Non utilizzare o considerare attendibile la proprietà `FileName` di <xref:Microsoft.AspNetCore.Http.IFormFile> senza convalida.</span><span class="sxs-lookup"><span data-stu-id="151aa-462">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="151aa-463">La proprietà `FileName` deve essere utilizzata solo a scopo di visualizzazione e solo dopo la codifica HTML.</span><span class="sxs-lookup"><span data-stu-id="151aa-463">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="151aa-464">Gli esempi forniti non prendono in considerazione le considerazioni sulla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="151aa-464">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="151aa-465">Le informazioni aggiuntive sono fornite nelle sezioni seguenti e nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="151aa-465">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="151aa-466">Considerazioni sulla sicurezza</span><span class="sxs-lookup"><span data-stu-id="151aa-466">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="151aa-467">Convalida</span><span class="sxs-lookup"><span data-stu-id="151aa-467">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="151aa-468">Caricare file di grandi dimensioni con flusso</span><span class="sxs-lookup"><span data-stu-id="151aa-468">Upload large files with streaming</span></span>

<span data-ttu-id="151aa-469">Nell'esempio seguente viene illustrato come utilizzare JavaScript per trasmettere un file a un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="151aa-469">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="151aa-470">Il token antifalsificazione del file viene generato usando un attributo di filtro personalizzato e passato alle intestazioni HTTP del client anziché nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="151aa-470">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="151aa-471">Poiché il metodo di azione elabora direttamente i dati caricati, l'associazione del modello di modulo viene disabilitata da un altro filtro personalizzato.</span><span class="sxs-lookup"><span data-stu-id="151aa-471">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="151aa-472">All'interno dell'azione, il contenuto del form viene letto tramite un `MultipartReader`, che legge ogni singola `MultipartSection`, elaborando il file o archiviandone il contenuto, come appropriato.</span><span class="sxs-lookup"><span data-stu-id="151aa-472">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="151aa-473">Una volta lette le sezioni multiparte, l'azione esegue una propria associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="151aa-473">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="151aa-474">La risposta della pagina iniziale carica il modulo e salva un token antifalsificazione in un cookie (tramite l'attributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="151aa-474">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="151aa-475">L'attributo usa il [supporto antifalsificazione](xref:security/anti-request-forgery) predefinito di ASP.NET Core per impostare un cookie con un token di richiesta:</span><span class="sxs-lookup"><span data-stu-id="151aa-475">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="151aa-476">Il `DisableFormValueModelBindingAttribute` viene usato per disabilitare l'associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="151aa-476">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="151aa-477">Nell'app di esempio, `GenerateAntiforgeryTokenCookieAttribute` e `DisableFormValueModelBindingAttribute` vengono applicati come filtri ai modelli di applicazione della pagina di `/StreamedSingleFileUploadDb` e `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` usando le [convenzioni di Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="151aa-477">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="151aa-478">Poiché l'associazione di modelli non legge il modulo, i parametri associati al modulo non vengono associati (la query, la route e l'intestazione continuano a funzionare).</span><span class="sxs-lookup"><span data-stu-id="151aa-478">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="151aa-479">Il metodo di azione funziona direttamente con la proprietà `Request`.</span><span class="sxs-lookup"><span data-stu-id="151aa-479">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="151aa-480">Per leggere ogni sezione, viene usato un `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="151aa-480">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="151aa-481">I dati chiave/valore vengono archiviati in un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="151aa-481">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="151aa-482">Dopo la lettura delle sezioni multiparte, il contenuto del `KeyValueAccumulator` viene utilizzato per associare i dati del modulo a un tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="151aa-482">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="151aa-483">Il metodo completo `StreamingController.UploadDatabase` per lo streaming in un database con EF Core:</span><span class="sxs-lookup"><span data-stu-id="151aa-483">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="151aa-484">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="151aa-484">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="151aa-485">Il metodo completo `StreamingController.UploadPhysical` per lo streaming in un percorso fisico:</span><span class="sxs-lookup"><span data-stu-id="151aa-485">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="151aa-486">Nell'app di esempio, i controlli di convalida vengono gestiti da `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="151aa-486">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="151aa-487">Convalida</span><span class="sxs-lookup"><span data-stu-id="151aa-487">Validation</span></span>

<span data-ttu-id="151aa-488">La classe `FileHelpers` dell'app di esempio illustra alcuni controlli per i caricamenti di file <xref:Microsoft.AspNetCore.Http.IFormFile> memorizzati nel buffer.</span><span class="sxs-lookup"><span data-stu-id="151aa-488">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="151aa-489">Per l'elaborazione di caricamenti di file memorizzati nel buffer <xref:Microsoft.AspNetCore.Http.IFormFile> nell'app di esempio, vedere il metodo `ProcessFormFile` nel file *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="151aa-489">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="151aa-490">Per l'elaborazione di file trasmessi, vedere il metodo `ProcessStreamedFile` nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="151aa-490">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="151aa-491">I metodi di elaborazione della convalida illustrati nell'app di esempio non analizzano il contenuto dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-491">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="151aa-492">Nella maggior parte degli scenari di produzione, nel file viene usata un'API scanner di virus/malware prima di rendere disponibile il file per gli utenti o altri sistemi.</span><span class="sxs-lookup"><span data-stu-id="151aa-492">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="151aa-493">Sebbene l'esempio di argomento fornisca un esempio funzionante di tecniche di convalida, non implementare la classe `FileHelpers` in un'app di produzione a meno che non si:</span><span class="sxs-lookup"><span data-stu-id="151aa-493">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="151aa-494">Comprendere completamente l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="151aa-494">Fully understand the implementation.</span></span>
> * <span data-ttu-id="151aa-495">Modificare l'implementazione in modo appropriato per l'ambiente e le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-495">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="151aa-496">**Non implementare mai indiscriminatamente il codice di sicurezza in un'app senza soddisfare questi requisiti.**</span><span class="sxs-lookup"><span data-stu-id="151aa-496">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="151aa-497">Convalida del contenuto</span><span class="sxs-lookup"><span data-stu-id="151aa-497">Content validation</span></span>

<span data-ttu-id="151aa-498">**Usare un'API di analisi di virus/malware di terze parti nel contenuto caricato.**</span><span class="sxs-lookup"><span data-stu-id="151aa-498">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="151aa-499">L'analisi dei file richiede le risorse del server in scenari con volumi elevati.</span><span class="sxs-lookup"><span data-stu-id="151aa-499">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="151aa-500">Se le prestazioni di elaborazione delle richieste sono diminuite a causa dell'analisi dei file, provare a eseguire l'offload del lavoro di analisi in un [servizio in background](xref:fundamentals/host/hosted-services), possibilmente in un servizio in esecuzione in un server diverso dal server dell'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-500">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="151aa-501">In genere, i file caricati vengono mantenuti in un'area in quarantena finché non vengono controllati dal programma antivirus in background.</span><span class="sxs-lookup"><span data-stu-id="151aa-501">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="151aa-502">Quando un file passa, il file viene spostato nel percorso di archiviazione file normale.</span><span class="sxs-lookup"><span data-stu-id="151aa-502">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="151aa-503">Questi passaggi vengono in genere eseguiti insieme a un record di database che indica lo stato di analisi di un file.</span><span class="sxs-lookup"><span data-stu-id="151aa-503">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="151aa-504">Usando un approccio di questo tipo, l'app e il server app rimangono incentrati sulla risposta alle richieste.</span><span class="sxs-lookup"><span data-stu-id="151aa-504">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="151aa-505">Convalida dell'estensione di file</span><span class="sxs-lookup"><span data-stu-id="151aa-505">File extension validation</span></span>

<span data-ttu-id="151aa-506">L'estensione del file caricato deve essere verificata rispetto a un elenco di estensioni consentite.</span><span class="sxs-lookup"><span data-stu-id="151aa-506">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="151aa-507">Esempio:</span><span class="sxs-lookup"><span data-stu-id="151aa-507">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="151aa-508">Convalida della firma del file</span><span class="sxs-lookup"><span data-stu-id="151aa-508">File signature validation</span></span>

<span data-ttu-id="151aa-509">La firma di un file è determinata dai primi byte all'inizio di un file.</span><span class="sxs-lookup"><span data-stu-id="151aa-509">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="151aa-510">Questi byte possono essere usati per indicare se l'estensione corrisponde al contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="151aa-510">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="151aa-511">L'app di esempio controlla le firme dei file per alcuni tipi di file comuni.</span><span class="sxs-lookup"><span data-stu-id="151aa-511">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="151aa-512">Nell'esempio seguente, la firma del file per un'immagine JPEG viene verificata rispetto al file:</span><span class="sxs-lookup"><span data-stu-id="151aa-512">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="151aa-513">Per ottenere ulteriori firme di file, vedere il [database delle firme file](https://www.filesignatures.net/) e le specifiche del file ufficiale.</span><span class="sxs-lookup"><span data-stu-id="151aa-513">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="151aa-514">Sicurezza del nome file</span><span class="sxs-lookup"><span data-stu-id="151aa-514">File name security</span></span>

<span data-ttu-id="151aa-515">Non usare mai un nome file fornito dal client per salvare un file nell'archiviazione fisica.</span><span class="sxs-lookup"><span data-stu-id="151aa-515">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="151aa-516">Creare un nome file sicuro per il file usando [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) per creare un percorso completo (incluso il nome file) per l'archiviazione temporanea.</span><span class="sxs-lookup"><span data-stu-id="151aa-516">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="151aa-517">Razor HTML codifica automaticamente i valori delle proprietà per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="151aa-517">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="151aa-518">Il codice seguente è sicuro da usare:</span><span class="sxs-lookup"><span data-stu-id="151aa-518">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="151aa-519">All'esterno di Razor, è sempre @no__t il contenuto del nome file da una richiesta dell'utente.</span><span class="sxs-lookup"><span data-stu-id="151aa-519">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="151aa-520">Molte implementazioni devono includere un controllo per verificare che il file esista. in caso contrario, il file viene sovrascritto da un file con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="151aa-520">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="151aa-521">Fornire logica aggiuntiva per soddisfare le specifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="151aa-521">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="151aa-522">Convalida dimensioni</span><span class="sxs-lookup"><span data-stu-id="151aa-522">Size validation</span></span>

<span data-ttu-id="151aa-523">Limitare le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="151aa-523">Limit the size of uploaded files.</span></span>

<span data-ttu-id="151aa-524">Nell'app di esempio, le dimensioni del file sono limitate a 2 MB (indicate in byte).</span><span class="sxs-lookup"><span data-stu-id="151aa-524">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="151aa-525">Il limite viene fornito tramite la [configurazione](xref:fundamentals/configuration/index) dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="151aa-525">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="151aa-526">Il `FileSizeLimit` viene inserito in classi `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="151aa-526">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="151aa-527">Quando le dimensioni di un file superano il limite, il file viene rifiutato:</span><span class="sxs-lookup"><span data-stu-id="151aa-527">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="151aa-528">Corrisponde al valore dell'attributo Name al nome del parametro del metodo POST</span><span class="sxs-lookup"><span data-stu-id="151aa-528">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="151aa-529">Nei moduli non Razor che INVIAno dati del modulo o usano direttamente `FormData` di JavaScript, il nome specificato nell'elemento del form o `FormData` deve corrispondere al nome del parametro nell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="151aa-529">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="151aa-530">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="151aa-530">In the following example:</span></span>

* <span data-ttu-id="151aa-531">Quando si usa un elemento `<input>`, l'attributo `name` viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="151aa-531">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="151aa-532">Quando si usa `FormData` in JavaScript, il nome viene impostato sul valore `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="151aa-532">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="151aa-533">Usare un nome corrispondente per il parametro del C# metodo (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="151aa-533">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="151aa-534">Per un metodo del gestore di pagina Razor Pages denominato `Upload`:</span><span class="sxs-lookup"><span data-stu-id="151aa-534">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="151aa-535">Per un metodo di azione POST controller MVC:</span><span class="sxs-lookup"><span data-stu-id="151aa-535">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="151aa-536">Configurazione del server e dell'app</span><span class="sxs-lookup"><span data-stu-id="151aa-536">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="151aa-537">Limite di lunghezza del corpo multipart</span><span class="sxs-lookup"><span data-stu-id="151aa-537">Multipart body length limit</span></span>

<span data-ttu-id="151aa-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> imposta il limite per la lunghezza di ogni corpo multipart.</span><span class="sxs-lookup"><span data-stu-id="151aa-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="151aa-539">Le sezioni del modulo che superano questo limite generano un'<xref:System.IO.InvalidDataException> quando vengono analizzate.</span><span class="sxs-lookup"><span data-stu-id="151aa-539">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="151aa-540">Il valore predefinito è 134.217.728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="151aa-540">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="151aa-541">Personalizzare il limite usando l'impostazione <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="151aa-541">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="151aa-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> viene usato per impostare il <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="151aa-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="151aa-543">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="151aa-543">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="151aa-544">In un'app Razor Pages o in un'app MVC, applicare il filtro al modello di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="151aa-544">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="151aa-545">Dimensioni massime del corpo della richiesta di gheppio</span><span class="sxs-lookup"><span data-stu-id="151aa-545">Kestrel maximum request body size</span></span>

<span data-ttu-id="151aa-546">Per le app ospitate da gheppio, le dimensioni massime predefinite del corpo della richiesta sono pari a 30 milioni byte, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="151aa-546">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="151aa-547">Personalizzare il limite usando l'opzione del server [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) gheppio:</span><span class="sxs-lookup"><span data-stu-id="151aa-547">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="151aa-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> viene usato per impostare [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) per una singola pagina o un'azione.</span><span class="sxs-lookup"><span data-stu-id="151aa-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="151aa-549">In un'app Razor Pages applicare il filtro con una [convenzione](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="151aa-549">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="151aa-550">In un'app Razor Pages o in un'app MVC, applicare il filtro alla classe del gestore di pagina o al metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="151aa-550">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="151aa-551">Altri limiti del gheppio</span><span class="sxs-lookup"><span data-stu-id="151aa-551">Other Kestrel limits</span></span>

<span data-ttu-id="151aa-552">Per le app ospitate da gheppio possono essere applicati altri limiti di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="151aa-552">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="151aa-553">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="151aa-553">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="151aa-554">Frequenza dei dati di richiesta e risposta</span><span class="sxs-lookup"><span data-stu-id="151aa-554">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="151aa-555">Limite lunghezza contenuto IIS</span><span class="sxs-lookup"><span data-stu-id="151aa-555">IIS content length limit</span></span>

<span data-ttu-id="151aa-556">Il limite di richieste predefinito (`maxAllowedContentLength`) è 30 milioni byte, che corrisponde a circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="151aa-556">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="151aa-557">Personalizzare il limite nel file *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="151aa-557">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="151aa-558">Questa impostazione si applica solo a IIS.</span><span class="sxs-lookup"><span data-stu-id="151aa-558">This setting only applies to IIS.</span></span> <span data-ttu-id="151aa-559">Il comportamento non si verifica per impostazione predefinita con l'hosting in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="151aa-559">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="151aa-560">Per altre informazioni, vedere [limiti della richiesta \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="151aa-560">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="151aa-561">Le limitazioni del modulo ASP.NET Core o della presenza del modulo filtro richieste IIS possono limitare il caricamento a 2 o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="151aa-561">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="151aa-562">Per ulteriori informazioni, vedere [non è possibile caricare file di dimensioni superiori a 2 GB (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="151aa-562">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="151aa-563">Risolvere problemi</span><span class="sxs-lookup"><span data-stu-id="151aa-563">Troubleshoot</span></span>

<span data-ttu-id="151aa-564">Di seguito sono trattati alcuni problemi comuni riscontrati durante il caricamento di file, con le soluzioni possibili corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="151aa-564">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="151aa-565">Errore non trovato quando viene distribuito in un server IIS</span><span class="sxs-lookup"><span data-stu-id="151aa-565">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="151aa-566">L'errore seguente indica che il file caricato supera la lunghezza del contenuto configurata del server:</span><span class="sxs-lookup"><span data-stu-id="151aa-566">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="151aa-567">Per ulteriori informazioni sull'aumento del limite, vedere la sezione [limite di lunghezza del contenuto IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="151aa-567">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="151aa-568">Errore di connessione</span><span class="sxs-lookup"><span data-stu-id="151aa-568">Connection failure</span></span>

<span data-ttu-id="151aa-569">Un errore di connessione e una connessione al server di reimpostazione probabilmente indicano che il file caricato supera le dimensioni massime del corpo della richiesta del gheppio.</span><span class="sxs-lookup"><span data-stu-id="151aa-569">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="151aa-570">Per ulteriori informazioni, vedere la sezione relativa alle [dimensioni massime del corpo della richiesta di Gheppio](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="151aa-570">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="151aa-571">I limiti di connessione del client gheppio possono richiedere anche modifiche.</span><span class="sxs-lookup"><span data-stu-id="151aa-571">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="151aa-572">Eccezione per riferimento Null con IFormFile</span><span class="sxs-lookup"><span data-stu-id="151aa-572">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="151aa-573">Se il controller accetta i file caricati usando <xref:Microsoft.AspNetCore.Http.IFormFile>, ma il valore è `null`, verificare che il form HTML specifichi un valore di `enctype` di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="151aa-573">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="151aa-574">Se questo attributo non è impostato sull'elemento `<form>`, il caricamento del file non viene eseguito e gli argomenti <xref:Microsoft.AspNetCore.Http.IFormFile> associati sono `null`.</span><span class="sxs-lookup"><span data-stu-id="151aa-574">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="151aa-575">Verificare inoltre che la [denominazione di caricamento nei dati del modulo corrisponda alla denominazione dell'app](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="151aa-575">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="151aa-576">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="151aa-576">Additional resources</span></span>

* [<span data-ttu-id="151aa-577">Caricamento di file senza restrizioni</span><span class="sxs-lookup"><span data-stu-id="151aa-577">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* <span data-ttu-id="151aa-578">Sicurezza [Azure: Frame di sicurezza: Convalida dell'input | Mitigazioni @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="151aa-578">[Azure Security: Security Frame: Input Validation | Mitigations](/azure/security/azure-security-threat-modeling-tool-input-validation)</span></span>
* <span data-ttu-id="151aa-579">Modelli di progettazione cloud [Azure: Modello di chiave del posteggiatore @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="151aa-579">[Azure Cloud Design Patterns: Valet Key pattern](/azure/architecture/patterns/valet-key)</span></span>
