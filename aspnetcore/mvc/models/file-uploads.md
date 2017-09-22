---
title: Caricamento di file in ASP.NET Core
author: ardalis
description: Come utilizzare l'associazione del modello e lo streaming per caricare i file in ASP.NET MVC di base.
keywords: ASP.NET Core, il caricamento di file del modello di associazione, IFormFile, streaming
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: ebc98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: e8608a46d6688df8da6c665a25b6f4db5f480461
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="file-uploads-in-aspnet-core"></a>Caricamento di file in ASP.NET Core

Da [Steve Smith](https://ardalis.com/)

Azioni di ASP.NET MVC supportano il caricamento di uno o più file utilizzando un modello semplice di associazione per i file più piccoli o di flusso per i file di dimensioni maggiori.

[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Il caricamento di file di piccole dimensioni con l'associazione di modelli

Per caricare file di piccole dimensioni, è possibile utilizzare un modulo HTML più parte o creare una richiesta POST con JavaScript. Un modulo di esempio utilizzando Razor, che supporta più i file caricati, è illustrato di seguito:

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

Per supportare il caricamento di file, è necessario specificare moduli HTML un `enctype` di `multipart/form-data`. Il `files` elemento input illustrato in precedenza supporta il caricamento di più file. Omettere il `multiple` attributo per questo elemento di input per consentire solo un singolo file da caricare. Il codice precedente viene eseguito il rendering in un browser come:

![Modulo di caricamento file](file-uploads/_static/upload-form.png)

I singoli file caricati nel server sono accessibili tramite [associazione di modelli](xref:mvc/models/model-binding) utilizzando il [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interfaccia. `IFormFile`presenta la struttura seguente:

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
> Non si basano su o considerare attendibile il `FileName` proprietà senza convalida. Il `FileName` proprietà deve essere utilizzata solo per scopi di visualizzazione.

Durante il caricamento di file utilizzando l'associazione di modelli e `IFormFile` interfaccia, il metodo di azione può accettare un singolo `IFormFile` o `IEnumerable<IFormFile>` (o `List<IFormFile>`) che rappresenta i diversi file. Nell'esempio seguente esegue il ciclo di uno o più file caricati, li salva nel file system locale e restituisce il numero totale e le dimensioni dei file caricati.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

I file caricati utilizzando il `IFormFile` tecnica vengono memorizzati nel buffer in memoria o su disco nel server web prima di essere elaborato. All'interno del metodo di azione, il `IFormFile` contenuto è accessibile come flusso. Oltre ai file system locale, file possono essere trasmesso a [archiviazione Blob di Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) o [Entity Framework](https://docs.microsoft.com/ef/core/index).

Per archiviare i dati di file binario in un database tramite Entity Framework, definire una proprietà di tipo `byte[]` nell'entità:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Specificare una proprietà viewmodel di tipo `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile`può essere utilizzato direttamente come un parametro di metodo di azione o una proprietà viewmodel, come illustrato in precedenza.

Copia il `IFormFile` in un flusso e salvarlo nella matrice di byte:

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
> Prestare attenzione quando si archiviano dati binari in database relazionali, come può influire negativamente sulle prestazioni.

## <a name="uploading-large-files-with-streaming"></a>Caricamento di file di grandi dimensioni con il flusso

Se le dimensioni o la frequenza di caricamento file provoca problemi di risorse per l'app, si consideri il caricamento del file di flusso, anziché memorizzarlo nella sua interezza, come illustrato in precedenza l'approccio di associazione del modello. Quando si utilizza `IFormFile` e associazione del modello è una quantità soluzione più semplice, streaming richiede un numero di passaggi per implementare correttamente.

> [!NOTE]
> Qualsiasi singolo file memorizzato nel buffer superiore a 64KB verrà spostati dalla memoria in un file temporaneo su disco nel server. Le risorse (disco, RAM) utilizzate dal caricamento di file dipendono dal numero e dimensione di caricamento di file simultanee. Il flusso non è tanto sulle prestazioni, è sulla scala. Se si tenta di caricamento di un numero eccessivo di buffer, del sito verrà arrestato in modo anomalo quando si esaurisce lo spazio di memoria o disco.

Nell'esempio seguente viene illustrato l'utilizzo di JavaScript/angolare per lo streaming a un'azione del controller. Token non riproducibili del file viene generato utilizzando un attributo di filtro personalizzato e passato nelle intestazioni HTTP anziché nel corpo della richiesta. Poiché il metodo di azione elabora i dati caricati direttamente, l'associazione di modelli è disabilitata per un altro filtro. All'interno dell'azione, il contenuto del form viene letti utilizzando un `MultipartReader`, che legge ogni singolo `MultipartSection`, l'elaborazione del file o di archiviare il contenuto come appropriato. Dopo tutte le sezioni sono stati letti, l'azione esegue il proprio associazione del modello.

L'azione iniziale carica il form e consente di salvare un token non riproducibili in un cookie (tramite il `GenerateAntiforgeryTokenCookieForAjax` attributo):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

L'attributo utilizza predefinito di ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) il supporto per impostare un cookie con un token di richiesta:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angolare passa automaticamente un token non riproducibili in un'intestazione di richiesta denominata `X-XSRF-TOKEN`. L'app ASP.NET MVC di base è configurato per fare riferimento a questa intestazione nella sua configurazione *Startup.cs*:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

Il `DisableFormValueModelBinding` attributo, illustrato di seguito, viene utilizzato per l'associazione di modelli per disabilitare il `Upload` metodo di azione.

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Poiché l'associazione del modello è disattivato, la `Upload` metodo di azione non accetta parametri. Interagisce direttamente con il `Request` proprietà `ControllerBase`. Oggetto `MultipartReader` viene utilizzato per leggere ogni sezione. Il file viene salvato con un nome di file GUID e i dati di chiave/valore vengono archiviati un `KeyValueAccumulator`. Una volta tutte le sezioni sono stati letti, il contenuto del `KeyValueAccumulator` vengono utilizzati per associare i dati del form a un tipo di modello.

L'intero `Upload` metodo è illustrato di seguito:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

Ecco alcuni problemi comuni riscontrati quando si lavora con il caricamento di file e le relative soluzioni possibili.

### <a name="unexpected-not-found-error-with-iis"></a>Errore imprevisto non trovato con IIS

L'errore seguente indica il caricamento del file supera il server configurato `maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

L'impostazione predefinita è `30000000`, ovvero circa 28,6 MB. Il valore può essere personalizzato modificando *Web. config*:

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

Questa impostazione si applica solo a IIS. Il comportamento non si verifica per impostazione predefinita, quando si ospita sul Kestrel. Per ulteriori informazioni, vedere [limiti richiesta \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>Eccezione di riferimento null con IFormFile

Se il controller è accettazione caricati i file usando `IFormFile` ma trovare che il valore è sempre null, verificare che il form HTML consiste nello specificare un `enctype` valore `multipart/form-data`. Se questo attributo non è impostato sul `<form>` elemento, il caricamento del file non verrà eseguito e qualsiasi associazione `IFormFile` argomenti è null.
