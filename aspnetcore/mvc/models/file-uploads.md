---
title: Caricamento di file in ASP.NET Core
author: ardalis
description: Come usare l'associazione del modello e lo streaming per caricare i file in ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 07/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/file-uploads
ms.openlocfilehash: 314d585c7bf7f8c95f763babe6cdf93e514ff656
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="55a49-103">Caricamento di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55a49-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="55a49-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="55a49-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="55a49-105">Le azioni di ASP.NET MVC supportano il caricamento di uno o più file tramite un'associazione di modelli semplice per i file di dimensioni inferiori o tramite streaming per i file di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="55a49-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="55a49-106">Visualizzare o scaricare l'esempio da GitHub</span><span class="sxs-lookup"><span data-stu-id="55a49-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="55a49-107">Caricamento di file di piccole dimensioni tramite associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="55a49-107">Uploading small files with model binding</span></span>

<span data-ttu-id="55a49-108">Per caricare file di piccole dimensioni, è possibile usare un form HTML multiparte o costruire una richiesta POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="55a49-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="55a49-109">Di seguito è illustrato un form di esempio con Razor, che supporta più file caricati:</span><span class="sxs-lookup"><span data-stu-id="55a49-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="55a49-110">Per supportare il caricamento di file, i form HTML devono specificare un `enctype` corrispondente a `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="55a49-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="55a49-111">L'elemento input `files` illustrato in precedenza supporta il caricamento di più file.</span><span class="sxs-lookup"><span data-stu-id="55a49-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="55a49-112">Omettere l'attributo `multiple` dell'elemento di input per consentire il caricamento di un unico file.</span><span class="sxs-lookup"><span data-stu-id="55a49-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="55a49-113">Il rendering del markup sopra riportato corrisponde a:</span><span class="sxs-lookup"><span data-stu-id="55a49-113">The above markup renders in a browser as:</span></span>

![Form di caricamento di file](file-uploads/_static/upload-form.png)

<span data-ttu-id="55a49-115">I singoli file caricati nel server sono accessibili tramite [associazione di modelli](xref:mvc/models/model-binding) attraverso l'interfaccia [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile).</span><span class="sxs-lookup"><span data-stu-id="55a49-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="55a49-116">`IFormFile` ha la struttura seguente:</span><span class="sxs-lookup"><span data-stu-id="55a49-116">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="55a49-117">Non basarsi sulla proprietà `FileName` né considerarla attendibile senza convalida.</span><span class="sxs-lookup"><span data-stu-id="55a49-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="55a49-118">La proprietà `FileName` deve essere usata solo per scopi di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="55a49-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="55a49-119">Durante il caricamento di file tramite l'associazione di modelli e l'interfaccia `IFormFile`, il metodo di azione può accettare una singola `IFormFile` oppure una `IEnumerable<IFormFile>` (o una `List<IFormFile>`) che rappresenta diversi file.</span><span class="sxs-lookup"><span data-stu-id="55a49-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="55a49-120">L'esempio seguente esegue il ciclo attraverso uno o più file caricati, li salva nel file system locale e restituisce il numero totale e le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="55a49-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="55a49-121">Prima di essere elaborati, i file caricati tramite la tecnica `IFormFile` vengono memorizzati nel buffer in memoria o nel disco del server Web.</span><span class="sxs-lookup"><span data-stu-id="55a49-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="55a49-122">All'interno del metodo di azione, il contenuto di `IFormFile` è accessibile come flusso.</span><span class="sxs-lookup"><span data-stu-id="55a49-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="55a49-123">Oltre al file system locale, i file possono essere trasmessi ad [Archiviazione BLOB di Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) o a [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="55a49-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="55a49-124">Per archiviare i dati di file binari in un database tramite Entity Framework, definire una proprietà di tipo `byte[]` nell'entità:</span><span class="sxs-lookup"><span data-stu-id="55a49-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="55a49-125">Specificare una proprietà viewmodel di tipo `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="55a49-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="55a49-126">L'interfaccia `IFormFile` può essere usata direttamente come parametro di metodo di azione o come proprietà viewmodel, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="55a49-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="55a49-127">Copiare `IFormFile` in un flusso e salvare quest'ultimo nella matrice di byte:</span><span class="sxs-lookup"><span data-stu-id="55a49-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="55a49-128">Quando si archiviano dati binari all'interno di database relazionali, è necessario prestare attenzione, perché l'operazione può influire negativamente sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="55a49-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="55a49-129">Caricamento di file di grandi dimensioni tramite streaming</span><span class="sxs-lookup"><span data-stu-id="55a49-129">Uploading large files with streaming</span></span>

<span data-ttu-id="55a49-130">Se le dimensioni o la frequenza del caricamento dei file causa problemi di risorse per l'app, è consigliabile caricare i file tramite streaming, anziché memorizzarli interamente nel buffer, come avviene con l'approccio di associazione di modelli illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="55a49-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="55a49-131">L'uso di `IFormFile` e dell'associazione di modelli rappresenta una soluzione più semplice, mentre l'implementazione corretta dello streaming richiede un certo numero di passaggi.</span><span class="sxs-lookup"><span data-stu-id="55a49-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="55a49-132">Ogni file memorizzato nel buffer superiore a 64 KB viene spostato dalla RAM in un file temporaneo in un disco nel server.</span><span class="sxs-lookup"><span data-stu-id="55a49-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="55a49-133">Le risorse (disco, RAM) usate dall'operazione di caricamento di file dipendono dal numero e dalle dimensioni dei file caricati simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="55a49-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="55a49-134">Con lo streaming, il problema non è rappresentato dalle prestazioni ma dalla scalabilità.</span><span class="sxs-lookup"><span data-stu-id="55a49-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="55a49-135">Se si tenta di caricare nel buffer un numero eccessivo di file, quando la memoria o lo spazio su disco si esaurisce il sito si arresterà in modo anomalo.</span><span class="sxs-lookup"><span data-stu-id="55a49-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="55a49-136">L'esempio seguente illustra l'uso di JavaScript/Angular per eseguire lo streaming in un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="55a49-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="55a49-137">Il token anti falsificazione del file viene generato tramite un attributo di filtro personalizzato e passato in intestazioni HTTP anziché nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="55a49-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="55a49-138">Poiché il metodo di azione elabora i dati caricati direttamente, l'associazione di modelli viene disabilitata da un altro filtro.</span><span class="sxs-lookup"><span data-stu-id="55a49-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="55a49-139">All'interno dell'azione, il contenuto del form viene letto tramite un `MultipartReader`, che legge ogni singola `MultipartSection`, elaborando il file o archiviandone il contenuto, come appropriato.</span><span class="sxs-lookup"><span data-stu-id="55a49-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="55a49-140">Dopo che tutte le sezioni sono state lette, l'azione esegue la propria associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="55a49-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="55a49-141">L'azione iniziale carica il form e salva un token anti falsificazione in un cookie tramite l'attributo `GenerateAntiforgeryTokenCookieForAjax`:</span><span class="sxs-lookup"><span data-stu-id="55a49-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="55a49-142">L'attributo usa il supporto [anti falsificazione](xref:security/anti-request-forgery) incorporato di ASP.NET Core per impostare un cookie con un token della richiesta:</span><span class="sxs-lookup"><span data-stu-id="55a49-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="55a49-143">Angular passa automaticamente un token anti falsificazione in un'intestazione di richiesta denominata `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="55a49-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="55a49-144">Nel file di configurazione *Startup.cs* l'app ASP.NET Core MVC è configurata per fare riferimento a questa intestazione:</span><span class="sxs-lookup"><span data-stu-id="55a49-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="55a49-145">L'attributo `DisableFormValueModelBinding`, illustrato di seguito, viene usato per disabilitare l'associazione di modelli per il metodo di azione `Upload`.</span><span class="sxs-lookup"><span data-stu-id="55a49-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="55a49-146">Poiché l'associazione di modelli è disabilitata, il metodo di azione `Upload` non accetta parametri</span><span class="sxs-lookup"><span data-stu-id="55a49-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="55a49-147">ma usa direttamente la proprietà `Request` di `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="55a49-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="55a49-148">Per leggere ogni sezione, viene usato un `MultipartReader`.</span><span class="sxs-lookup"><span data-stu-id="55a49-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="55a49-149">Il file viene salvato con un nome di file GUID e i dati di chiave/valore vengono archiviati in un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="55a49-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="55a49-150">Dopo che tutte le sezioni sono state lette, il contenuto del `KeyValueAccumulator` viene usato per associare i dati del form a un tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="55a49-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="55a49-151">Il metodo `Upload` è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="55a49-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="55a49-152">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="55a49-152">Troubleshooting</span></span>

<span data-ttu-id="55a49-153">Di seguito sono trattati alcuni problemi comuni riscontrati durante il caricamento di file, con le soluzioni possibili corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="55a49-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="55a49-154">Errore imprevisto Non trovato con IIS</span><span class="sxs-lookup"><span data-stu-id="55a49-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="55a49-155">L'errore seguente indica che il caricamento di file supera il valore di `maxAllowedContentLength` configurato per il server:</span><span class="sxs-lookup"><span data-stu-id="55a49-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="55a49-156">L'impostazione predefinita è `30000000`, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="55a49-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="55a49-157">Il valore può essere personalizzato modificando *web.config*:</span><span class="sxs-lookup"><span data-stu-id="55a49-157">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="55a49-158">Questa impostazione si applica solo a IIS.</span><span class="sxs-lookup"><span data-stu-id="55a49-158">This setting only applies to IIS.</span></span> <span data-ttu-id="55a49-159">Il comportamento non si verifica per impostazione predefinita con l'hosting in Kestrel.</span><span class="sxs-lookup"><span data-stu-id="55a49-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="55a49-160">Per altre informazioni, vedere [Request Limits \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/) (Limiti delle richieste <requestLimits>).</span><span class="sxs-lookup"><span data-stu-id="55a49-160">For more information, see [Request Limits \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="55a49-161">Eccezione per riferimento Null con IFormFile</span><span class="sxs-lookup"><span data-stu-id="55a49-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="55a49-162">Se il controller accetta file caricati tramite `IFormFile` ma si rileva che il valore è sempre Null, verificare che il form HTML specifichi un valore `enctype` corrispondente a `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="55a49-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="55a49-163">Se questo attributo non è impostato per l'elemento `<form>`, il caricamento di file non si verifica e qualsiasi argomento `IFormFile` associato è Null.</span><span class="sxs-lookup"><span data-stu-id="55a49-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
